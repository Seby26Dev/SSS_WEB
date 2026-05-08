# The last challage it's called "Welcome"
## On the main "Welcome" page the description informs us that the flag is split into 4 parts hidden across the website

<img width="1334" height="717" alt="image" src="https://github.com/user-attachments/assets/ec313189-4f6e-4a34-a413-80fc31b1036d" />


### The first part of the flag is located in the page source by inspecting the source page we can find a link to static/css/main.css

<img width="1064" height="521" alt="image" src="https://github.com/user-attachments/assets/95337cb4-080a-4110-b6ef-07239f935459" />

### When we access static/css/main.css we find a comment containing the first part of the flag. Notice the FFF{ prefix which suggests the text is obfuscated using a ROT cipher
```
/* The first part of the flag is: "FFF{rirel_" */

"FFF{rirel_
```

<img width="1184" height="629" alt="image" src="https://github.com/user-attachments/assets/32a586f4-ce8f-47c4-b2c2-f221e9a4ea5a" />

### The second part of the flag is hidden directly in the HTML page. White text on a white background 
<img width="1426" height="613" alt="image" src="https://github.com/user-attachments/assets/66cc4f1d-1bf8-4e6e-9617-ffaf4e926fd3" />

```
<p id="hidden" style="margin-left: 5px; color: white;">svyr_</p>

"FFF{rirel_svyr_
```
### For the third part we need to go back to the page source and will be located in hidden.js file

<img width="1125" height="522" alt="image" src="https://github.com/user-attachments/assets/70071e2b-6951-478e-a33a-717438aa6be0" />

### Navigating to this `JavaScript` file reveals the next piece of the flag in console.log()

<img width="1254" height="387" alt="image" src="https://github.com/user-attachments/assets/7b2bea7a-6372-4eef-8c5f-817269621e79" />

```
adventure. The third part of the flag is: "unf_".

"FFF{rirel_svyr_unf_
```
### The final part was hidden inside an "invisible" PNG image loaded on the page. By opening the image directly we can see some text


<img width="1193" height="529" alt="image" src="https://github.com/user-attachments/assets/5615ddd0-13a5-4040-b473-6a10e4701079" />


<img width="1627" height="944" alt="image" src="https://github.com/user-attachments/assets/f62ca09b-584a-4d6b-a168-88fca7cf019a" />

### To extract the text quickly i used Gemini to give me the text in raw text
```
Sometimes you have to look very closely. Everywhere.

Meanwhile, here is the last part of the flag:
"srryvatf}"


"FFF{rirel_svyr_unf_srryvatf}

```

<img width="1414" height="858" alt="image" src="https://github.com/user-attachments/assets/c4b6a8c8-5eae-4751-afc2-5bc184f5e976" />


### For the flag i use CyberChef with ROT13 



<img width="1389" height="698" alt="image" src="https://github.com/user-attachments/assets/f2fb30c4-3f53-4d1c-b687-4cd3c42a73d1" />



