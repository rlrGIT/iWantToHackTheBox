### Started with nmap, identified two open ports
- ssh open on port 22
- http server on port 80

### Attempted password brute force in the background
- Hydra - tried default credentials
- Took too long, so stopped

### Identified Web App Running on port 80
- Not sure if this is how you're supposed to do it, but I used:
`curl -v <ip>:<port>`, and I got a `302 Found` pointing to `http://permx.htb`. To be able to examine this site, which won't be on DNS cause it's just hosted on this server, I connected the IP to the hostname:
```
sudo sh -c "echo '<ip> permx.htb' >> /etc/hosts"
```
This allowed me to access the site through the browser. I was not able to find any login pages or anything interesting, so I pivoted to fuzzing the site with `ffuf`.

- Directory fuzzing
- Page fuzzing/extension fuzzing
- Vhost fuzzing
### Identified Virtual Host (Many-to-one page mapping of IP)
I originally did not filter, and got a ton of results, so I narrowed it down by filtering with a regex. 
```
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://permx.htb:80/ -H "Host: FUZZ.permx.htb" -mr "admin"
```

Using this command, I identified the `lms` subdomain, which returned a `200 OK` code rather than `302`, and was also way larger in size:

```
lms                     [Status: 200, Size: 19347, Words: 4910, Lines: 353, Duration: 103ms]

admin                   [Status: 302, Size: 281, Words: 18, Lines: 10, Duration: 2036ms]
```

We can see the difference in status code, size, and words, so I added this `vhost` to `/etc/hosts` and accessed the site. It's a portal!

### Attempted to use Hydra with http-post-form login, found password but it doesn't work?

- I used inspect element to find the form info

```   
hydra -l admin -P /opt/useful/seclists/Passwords/Leaked-Databases/rockyou-50.txt -f lms.permx.htb -s 80 http-post-form "/index.php:username=^USER^&password=^PASS^:F=<form id='formLogin'"
```

- admin:12345 - not sure what the issue is here...
- I think I might understand what is happening, apparently Hydra cannot distinguish whether a login attempt is successful or not. From here I need to:
	- check to see if my hydra command is doing what I think it does
	- check to see if my domain target is correct 
	- check to see if Hydra understands what a failed login attempt looks like

### At this point I think you need to take a step back, and start systematically brute forcing logins

- Try checking to see if your failure string syntax is ok
- Default combos
- Start with username brute force, although I think `admin` is a username because there on the admin panel site, there is `admin@permx.htb` as a contact name.
- Next, try looking at brute forcing passwords

### Note: you are rushing to try stuff. Try being more systematic