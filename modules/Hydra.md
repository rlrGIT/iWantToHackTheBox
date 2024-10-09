Hydra is a tool for login brute forcing. The program works by sending login requests of various types in parallel; part of the skill of using the tool is knowing what kind of request format to send to an application.
- Choosing between application specific http requests, or more generically between different HTTP verbs.
- If the URL changes, we know that the we are dealing with a GET form, rather than a POST form. POST requests typically do not change the URI.

### Usage
```
hydra <option> -U
```
- Specifies usage info for the option selected

### One of the distinguishing features about Hydra is the ability to program how it interprets HTTP requests, responses, and success and failures

For example, in the following command:
```shell-session
1) /login.php:

2) [user parameter]=^USER^&[password parameter]=^PASS^:
(parameters are required fields in a HTTP request, their names can vary)

4) [FAIL/SUCCESS]=[success/failed string]
```

1) Specifies the login index/URI
2) Specifies the user name and password values
	- ^USER^ and ^PASS^ represent variable values for HYDRA and \[user parameter\] represents the words required in the HTTP request specified by the API

1) A user provided string that distinguishes between failed and successful attempts based off of user provided details. Hydra will keep looking until a failure string is not found in the response, for example. You can pull this from text rendered after an invalid login, e.g. "Login failed for that username/password"

### It is best that the fail/success string is distinct enough to reduce false positives

- Inspecting the source and lookin for keywords in the HTML can be particularly useful

### Use a browser to figure out the parameters and format required for a successful HTTP request for Hydra

- Examine the network traffic to look at the individual requests going through the browser.
- Copy a request as a URL, this will give you the information you need to use it with cURL or other tools
- We can also use Burp Suite to monitor traffic more closely by using it as a proxy with a tool like FoxyProxy

### Use -u and -f (loop around users, not passwords and stop when a pair is found, respectively)

### Try to systematically increase password list complexity; start small and add details

### Look for open ports on the current machine with netstat


