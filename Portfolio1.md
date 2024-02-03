# Portfolio 1 - Security Testing

## Requirement 2 - OWASP Juice Shop Functionality testing

1. What are the features and functions of the application?
2. What are the inputs and outputs of the application?
3. What are the expected and unexpected behaviours of the application?

## Requirement 3 - Exploit a Cross-Site Scripting vulnerability

### Attempt 1
The first attempt at this was me inputting a piece of HTML code into the search bar of JuiceShop

The line of code was:
```<h1>Hello Im in</h1>```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/58708b3b-905a-44f7-9b38-0c64c6306bf2)

As you can see from the image provided once I entered my search it showed up in the URL and we can see that the search I made appears in the Search results of JuiceShop
However, if we inspect the webpage you can see that I have embedded my search into the webpage

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/fbcc66b1-0473-490b-a191-5b3f038a8966)

_Since this is a simple execution I did not come across any problems_

### Attempt 2

For this attempt at Cross-Site Scripting, I performed a DOM XSS attack that uses malicious code to modify the elements/environment in the client-side script.

The code I used for this attack was:
```<iframe src="javascript:alert('xss')">```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/07628db3-e565-492b-a617-ed539cf969d4)

What this attack does is that the payload was handled and improperly embedded into the page by the application frontend code without ever sending it to the server

This is the result you get from executing this attack

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/37dc8606-2a99-48c1-b49e-ed26cf68029e)

### Attempt 3

For this attempt, I entered an HTML iframe element which embeds a SoundCloud player widget into Juiceshop. This is done but implementing this line of code into the search bar

```<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/13f8f78a-b87f-4fe4-a9ff-a97735d20161)

The only issue I have right now is that my Kali cannot connect to the internet meaning that the SoundCloud widget won't be displayed on the webpage.

### Attempt 4

For this attempt, I performed a Reflected XSS attack. A Reflected XSS attack happens when an attacker injects browser executable code within a single HTTP response.

The steps which I took to perform this reflected XSS attack was that I created an account on Juiceshop and bought 2 items. I then went to the "Track Order" page 

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/6146f821-093e-40b1-88ab-175b276e35ea)

As you can see from the URL of the page there is an ID of "id=1aed-5b......" and this is vulnerable to these sorts of attacks

So what I did was I changed the ID after the equals sign to ```<iframe src="javascript:alert('xss')">```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/16579167-412f-4d46-b70d-7771962f0365)

After the attack was done this was the result of what I got

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/ec8a25e5-0ddd-4fbc-a19c-90a0a3dcf731)

_Problems with attack performed_

Upon further research, the expected output was supposed to be

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/0df281e7-9016-41a2-ad92-40f256d84470)


### Failed attempts

This line of code ```<img src="javascript:alert('XSS')">``` did not output anything. This is the result I got:

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/64b29098-acc6-455d-8e76-a5c097f2341e)

I later then tried this ```<img src="https://www.wfla.com/wp-content/uploads/sites/71/2023/05/GettyImages-1389862392.jpg?w=2560&h=1440&crop=1">``` but this turned out to not work out as well as this is a regular image tag and not an XSS payload.

what this line of code is supposed to ```<img src="javascript:alert('XSS')">``` is to execute a JavaScript alert when the image tag is rendered on the page.


_References used_

Anusha Ihalapathirana (2021) OWASP Juice Shop — XSS Tier 0 and XSS Tier 1 Challenge Solutions. Available at: https://medium.com/swlh/owasp-juice-shop-xss-tier-0-and-xss-tier-1-challenge-solutions-48d414e42d2a (accessed 3 February 2024).

Cross-Site Scripting (XSS):: Pwning OWASP Juice Shop (2024) Available at: https://pwning.owasp-juice.shop/companion-guide/latest/part2/xss.html (accessed 3 February 2024).




‌


