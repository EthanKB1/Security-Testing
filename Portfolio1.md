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

In this attempt, I will try and perform a server-side XSS protection attack. A server-side attack aims to bypass the security measures implemented on the server side to prevent or mitigate cross-site scripting vulnerabilities. This targets weaknesses in the server-side processing of user input or output encoding.

For this attack, I'm creating a new account on JuiceShop

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/510550b2-7537-4f94-9594-2b194a1e9f4e)

And I will need to have Burp Suite Interceptor turned on

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/53a9d09d-9f5f-4310-a86d-002d67357c3c)

I have now intercepted the Burpsuite login, bringing me to this page.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/d99848c1-de98-407e-b8b8-ee49b56e0976)

From this point, I would need to change the "email" to ```<iframe src=\"javascript:alert('xss')\">```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/88f8dba1-968d-4841-943f-f4ba61062c40)

once that is done I would then need to send my request and this will get sent to the response tab. This is what gets displayed

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/dfccd464-6a50-4db9-bb68-3233d475a45a)

*note - Login credentials may be different as originally this attempt was a failure but now it has been resolved

## Failed attempts

This line of code ```<img src="javascript:alert('XSS')">``` did not output anything. This is the result I got:

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/64b29098-acc6-455d-8e76-a5c097f2341e)

I later then tried this ```<img src="https://www.wfla.com/wp-content/uploads/sites/71/2023/05/GettyImages-1389862392.jpg?w=2560&h=1440&crop=1">``` but this turned out to not work out as well as this is a regular image tag and not an XSS payload.

what this line of code is supposed to ```<img src="javascript:alert('XSS')">``` is to execute a JavaScript alert when the image tag is rendered on the page.

## Requirement 4 - Exploit a SQL Injection vulnerability

### What are SQL injection attacks?


### Attempt 1

For this attack, I signed in as an admin using this sign ```admin' or 1=1;-```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/58cff92d-ee83-4a78-8d28-07f71b75d00b)
![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/953b3519-6204-4805-8523-b85931a0b282)

Another way of making this attack is through BurpSuite, How you'd make this injection attack is first to try and log into the admin account on JuiceShop by inputting ```admin``` and ```admin``` for both user and password, when this is done you'd get an error saying 'Invalid email or password'

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/cc20336f-007e-4470-9d4b-2e1b1da6f9f7)

If you go onto burpsuite and go on proxy > http history you'd then be presented with all the requests which have been made. If you scroll down you'll see a request for user login called ```/rest/user/login```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/8bb24b8c-73f5-4d34-bf88-54a56e6d2e0a)

When you click on the request it will show what you tried to log in as

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/e183c22b-f49b-4f54-8e25-f540f049ef6a)

Now the next step is to send this to the Repeater in Burpsuite, once this is in the repeater you will be presented with this information

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c115495a-baa4-48d7-81d4-33c33b2a4eb1)

Now in order to get the admin email you'd need to change the 'email' to ```admin' or 1=1 --```, once this has been done you can send the request and you will then be presented with a token and the email of the admin for juice shop

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/6620ac9b-69e5-4add-bdb1-022ba686719c)


### Attempt 2

In this attempt, I will log in as one of the accounts on Juiceshop. Upon further research, I have come across the "LoginBenderChallenge". To find this email for Bender you'd need to look at the reviews of each product that the shop has until you come across the Bender email.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/158e6172-3ec0-4046-8bdf-f10aeb353790)

So in this case the email was found under the "Banana Juice" listing. The next step is to try and log into the account and to do this you'd need to have BurpSuite open. Now you would need to try and sign into an account in this case I used ```j``` for the email and ```.``` for the password

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/1b056b58-b2ae-43c8-96d8-eb0b61ce1f2e)

Now the next step is to open Burpsuite and navigate to the Proxy tab then Intercept, here you would need to turn Intercept on and then you'd be able to attempt to login as shown above. Once you have attempted to log in you will get an error but in BurpSuite you will be presented with this information

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c08d0c7d-244d-4dfc-bcbd-b1080fda98e1)

Now once you have this information all you'd need to do is change the "email" to ```bender@juice-sh.op'--```, once this is done you can then forward the request you have intercepted.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/1da36f0e-37ee-418e-af28-97afb4162b36)

Now that you have forwarded the request you can turn intercept off and go back to the website. Now that you are back on the website you will notice that you are in the Bender account 

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/3c34211b-7f45-4c63-83ad-c24fbdcfc41a)

## Failed Attempt

### Attempt 3

In this attempt, I am going to try and get the admin password. From an earlier attempt, I was able to get the admin email but I was not able to get the password. Now what I am going to do is try and log in as the admin with the email I obtained but the password is just going to be ```fff```. Now what I need to do is open Burpsuite and turn Intercept on

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/9d663952-27eb-4527-ac81-04e7cefa00dc)
![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/0039d3e5-ceb6-42e5-b5af-012fbd4dd08a)

Now the information it intercepted can be viewed and you can see the email and password used. Now the next step is to highlight all of the information and send it to the intruder

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c533931d-7c43-4be2-a579-e8612b116806)

Once the request has been sent to the intruder what needs to be done now is first select ```Clear``` and then highlight the password and select ```add```. Before I move on to the next step make sure that the "Choose attack type" is set to "Sniper"

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/56a0ab66-8f31-400b-b9bb-ea865a8cf567)

Now that the password has been highlighted and the attack type is set to sniper I can now move to the "Payloads" section where I will now see this screen

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/850ce2d1-de2e-4ead-8ca5-c20e6c791174)

The next thing to do is go under payload settings and select load. This is the screen that comes up

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/a7cb13c0-a838-424f-bc2b-37748785e890)

For this bit, I need to try and find the words list folder. to find this folder you need to go to ```usr/share/wordlists/```

I won't be able to continue with this attack as I appear to be missing a folder called "seclists" which is under "wordlist".

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c4df5247-a710-4dbf-8b9d-7a3b941a7196)

## Requirement 5 - Broken Access Control vulnerability

### What are Broken access control attacks?


### Attempt 1

I'm going to view someone else basket other than mine. In order to do this I am going to sign into the admin account and head over to "Your Basket"

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/2fcf35d2-3c32-45de-90a6-d03bc976f352)

Now in order for me to view the other basket I would need to open the inspect panel and from there I would need to navigate over to the storage tab

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c38437f9-a6f0-4da7-ada2-d323f576bb4f)

Once on the storage tab, I would need to navigate over to "Session Storage" where the HTTP address would appear, once on it there will be 2 items in a table which are ```bid``` and ```ItemTotal```. In order to view someone else's basket you'd need to change the ```bid``` value to a different number.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/922ccd04-1216-41d1-8030-bd9e7a0cc50b)

Here is the bid number changed and the view of the other basket:

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/bb8ed938-9d5a-4d5c-a597-91418e61f416)

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/1db97ca4-5f02-4bc8-b04c-eb2e7ad94d4c)

I have come across something when you change the ```bid``` value to be more than 5 nothing will show

### Attempt 2

In this attempt, I am going to try and access the administration page that the juice shop has. I have done some research into this and in order for me to view the page I would need to be signed in as the admin if I was signed into any other account I wouldn't be allowed to access the page.

First of all for me to find the administration page I would need to go into the inspect panel and navigate my way to "Debugger" -> "Main.js". once you have the "Main'js" open you can then press ```CTRL + F``` and type in admin, Now you'd need to find the line of code which looks like ```path: 'administration'```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/283f4338-12bb-4f7d-9946-19935a30211c)

Once that has been found the next step is to type in the administration into the URL as shown below

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/95a49047-cc98-4a65-ac1b-17962f1501dd)

Now that you've pressed enter you will taken to the administration page where you can view the registered users and the feedback which have been given

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/ea28e13d-b7eb-4a03-93ae-bd62729044c4)

### Attempt 3

This is a quick attempt as this ties into Attempt 2 where we now have access to the administration page. For this attempt, I have the power to delete a review and as part of a challenge, I must delete a 5-star review

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/2805290e-54ac-4b4b-89f0-365cbb85902a)

## Requirement 6 - Exploit an Authentication Bypass vulnerability

### What is Broken authentication

### Attempt 1

In this attempt, I'm going to try and bypass the CAPTCHA and submit 10 or more customer feedbacks within 20 seconds. I will demonstrate what I have done to complete this challenge. First of all, on JuiceShop they have a section where a customer can submit some feedback on their website. To complete this attack you'd need to fill out the customer feedback form. Below is a picture of the form which needs to be filled out.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/4894fae2-5e90-41d5-b944-a55f130a356d)

Before I click submit BurpSuite needs to be open and intercept to be turned on. Once that is on I can then press the submit button, once I pressed the submit button I went back onto BurpSuite and went over to the HTTP history tab under Proxy and then all the history appeared from when I pressed the submit button. Now what I need to do is to find the request from when the submit button was pressed. To find this easier I will need to look for ```/api/Feedbacks/```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/35084845-60b5-4ab4-824b-be2109e9da51)

Now that I have found it I can see the information I entered appear on the request tab, this is how I know that I am on the correct one. Now the next step is to send this request to the intruder. Once in the intruder tab, I would need to highlight the comment I made and then add special characters to it, ```§```.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/2daf582a-8929-444f-b4fc-6ac3f5dd4ab0)

Now that I've added the character to both ends of the comment I made I can then make my way over to the "Payloads" tab. In the "Payloads" tab there is an option called "Payload settings [Simple List]" In this section, I will need to add numbers from 1 to 11.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/9fe4dfb1-a3cb-46a4-b8fc-945f732d96d8)

Now that I've added the numbers in I can start the attack, when I press the ```Start attack``` button another window comes up and shows each payload being executed and it does this 11 times.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/77558ebf-ec80-4083-b59c-fb8b4f5d1a7b)

Now when you go back onto JuiceShop it shows that you have completed the challenge. Now to see if the attack worked I can sign into the admin account and go to the administration page and here I can see all the customer feedback including the ones I have done in the attack.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/2ee568a4-b0c1-40e7-8f11-c46b4df925ad)

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c60b2fd2-58b4-45eb-a0df-a1c12f9794b3)

### Attempt 2

In this attempt, I am going to try and access the scoreboard of JuiceShop. The Scoreboard in Juiceshop allows you to track all the challenges you've done. At first, the scoreboard couldn't be accessed from the menu on JuiceShop. In this attempt, I am going to try and access the scoreboard.

For me to access the scoreboard I need to open the inspect element on JuiceShop and it will bring me to the elements page.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/9631347a-2410-4224-b187-85546d7dbdbe)

Now I would need to navigate to the "Sources" tab. To reach this tab I would need to go to Debugger -> Sources. Now that I am on the Sources tab I would need to click on the file called ```main.js```, Once I have clicked on that file this should appear

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/82e90bf1-6048-4aae-ae7f-e9e84fcf2ef7)

The next step is to search the path of scoreboards, to do this I would need to do ```CTRL + F```, which will allow me to search for ```score-board```, Once this has been put into the search bar the I went down one on the arrow and it took me to the path for score-boards.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/332f15f7-e053-498b-8c3f-1634935ea272)

Now that I have found the path for the scoreboard I can now put this into the search bar of Firefox.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/005c3830-f1be-4d08-8af4-0fefee67575a)

Now when I search for this It will take me to the Scoreboards page and I can view what challenges I have done

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/787a9d82-b8ad-4be7-84ed-709f7ef3e54b)


## _References used_

Anusha Ihalapathirana (2021) OWASP Juice Shop — XSS Tier 0 and XSS Tier 1 Challenge Solutions. Available at: https://medium.com/swlh/owasp-juice-shop-xss-tier-0-and-xss-tier-1-challenge-solutions-48d414e42d2a (accessed 3 February 2024).

Cross-Site Scripting (XSS):: Pwning OWASP Juice Shop (2024) Available at: https://pwning.owasp-juice.shop/companion-guide/latest/part2/xss.html (accessed 3 February 2024).

Gao, W. (2020) Juice Shop 3.6 - Client Side XSS Protection - https://www.youtube.com/watch?v=9x7vAOgepic

‌Hamdan, M. (2020) Broken Authentication and SQL Injection - OWASP Juice Shop TryHackMe.

‌Web Security Tutorials (2020) OWASP Juice Shop Solution for CAPTCHA Bypass - https://www.youtube.com/watch?v=dlKWtRDKQQ0

‌thehackerish (2020) Broken Authentication and Session Management tutorial - thehackerish. Available at: https://thehackerish.com/broken-authentication-and-session-management-tutorial/ (accessed 17 February 2024).




‌


