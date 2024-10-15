### Injection Types

There are a number of different mechanisms that allow SQL injections to work - we can separate the into the following (BEUSTQ):  

- Boolean-based bind
- Error-based
- Union query-based
- Stacked queries
- Time-based bind
- Inline queries 

### Blind Injections

Blind injections can be used to gather metadata about the tables, such as DBMS information, names of rows and columns, and relational details. We initially may not know how data is structured in the database, these methods can help us determine information about how to craft a more revealing injection payload.
### Boolean-based (Blind injection)
- Utilize the behavior of the application, through fuzzy comparisons of the raw response content, HTTP codes, page titles, and filtered text (for example). 
- For example, if there is a specific entry in the database whose value we are trying to discover, we could write an injection payload to compare the characters of a value in a row or column byte by byte, until we get a "TRUE" response.

### Error Based
Sometimes, a database management system (DBMS) may have its errors forwarded by the server (bad). This can sometimes carry the results from requested queries - this information can be used in tandem with known misbehaviors in order to retrieve information. This kind ofattack is considered faster than all other types except UNION query based ones.

### Union Query Based
Union queries are dangerous because they generally allow extension/concatenation of results from both the original query and the UNION sub-query. This can be used to pull information from additional rows and columns, and in an ideal scenario dump the entire database.

### Stacked Queries
This is a way to inject statements after the original vulnerable query, which is allowed by the SQL parser's grammar.

### Time-based (Blind)
Similar to boolean type injections, but the difference in time between a queries is used to determine whether injections are successful or not (similar to crypto timing attacks). This is much slower than boolean based, since you are largely at the mercy of server response times.

### Inline Queries
Rare, because of the wya modern apps are written. Queries are "nested" or "inlined", which leads to more kinds of information extracted.

### Out-of-Band SQL Injection
Advanced, this type is used when all other types are unsupported or too slow. You essentially filter query results from DNS traffic. (Dunno how this works).