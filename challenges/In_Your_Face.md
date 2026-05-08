# The third challenge it's called In_Your_Face
## Vulnerability : Information Disclosure ( CWE-615 ) 



## The challenge description tells us to get the flag from "in your face." When we visit the web page we only see a simple text saying "Face off!"

<img width="929" height="184" alt="image" src="https://github.com/user-attachments/assets/7e456951-6b2d-4ad3-a276-28438456baf0" />


### By looking at the raw `HTML` returned by the server, i spotted a hidden `HTML` comment left by the developer:

### Ctrl + U 

```HTML
<!doctype html>
<html>
  <head>
    <title>My page</title>
    <!-- U1NTe2NhZ2VfdHJhdm9sdGF9 -->
  </head>
  <body>
    <p>Face off!<p>
  </body>
</html>
```

### The hidden string `U1NTe2NhZ2VfdHJhdm9sdGF9` contains uppercase letters, lowercase letters, and numbers, which is the standard format for base64 encoding

## To retrieve the flag i decoded the string using the terminal :

```
echo "U1NTe2NhZ2VfdHJhdm9sdGF9" | base64 -d ; jq .
```

<img width="472" height="58" alt="image" src="https://github.com/user-attachments/assets/645be20a-5aed-451c-a81d-733676703b84" />






