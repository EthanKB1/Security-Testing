# Portfolio 1 - Security Testing

## Requirement 2 - OWASP Juice Shop Functionality testing

1. What are the features and functions of the application?
2. What are the inputs and outputs of the application?
3. What are the expected and unexpected behaviours of the application?

## Requirement 3 - Exploit a Cross-Site Scripting vulnerability

### Attempt 1
The first attempt at this was me inputting a piece of HTML code into the search bar of JuiceShop

The line of code was:
"<h1>Hello Im in</h1>"

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/58708b3b-905a-44f7-9b38-0c64c6306bf2)

As you can see from the image provided once I entered my search it showed up in the URL and we can see that the search I made appears in the Search results of JuiceShop
However, if we inspect the webpage you can see that I have embedded my search into the webpage

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/fbcc66b1-0473-490b-a191-5b3f038a8966)

_Since this is a simple execution I did not come across any problems_

### Attempt 2

For this attempt at Cross-Site Scripting, I performed a DOM XSS attack that uses malicious code to modify the elements/environment in the client-side script.

The code I used for this attack was:
"<iframe src="javascript:alert('xss')">"

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/07628db3-e565-492b-a617-ed539cf969d4)

What this attack does is that the payload was handled and improperly embedded into the page by the application frontend code without ever sending it to the server

This is the result you get from executing this attack

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/37dc8606-2a99-48c1-b49e-ed26cf68029e)

_References used_

Anusha Ihalapathirana (2021) OWASP Juice Shop — XSS Tier 0 and XSS Tier 1 Challenge Solutions. Available at: https://medium.com/swlh/owasp-juice-shop-xss-tier-0-and-xss-tier-1-challenge-solutions-48d414e42d2a (accessed 3 February 2024).

‌


