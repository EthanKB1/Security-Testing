**Ethan Bannister,**
**BAN22597236,**
**Security Testing - Portfolio 1,**
**Due date - 19/02/2024**


# Portfolio 1 - Security Testing

## Requirement 3 - Exploit a Cross-Site Scripting vulnerability

### What is Cross-site Scripting?

Cross-site scripting is a type of security vulnerability typically found in web applications. XSS attacks enable attackers to inject client-side scripts into web pages viewed by other users. A cross-site scripting vulnerability may be used by attackers to bypass controls.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/065b7254-384f-47a2-b938-8591b913ebbc)

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

SQL injection is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other users, or any other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application content or behaviour.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/8057fd48-498c-4201-8e24-64b7dba24fc5)


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

Now to get the admin email you'd need to change the 'email' to ```admin' or 1=1 --```, once this has been done you can send the request and you will then be presented with a token and the email of the admin for juice shop

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

Broken access control is a type of vulnerability that allows unauthorised users to gain access to sensitive data or systems. This can happen when controls such as authentication and authorisation are not properly implemented or when there are weaknesses in the way these controls are enforced.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/f598875d-4ad7-4967-b64e-7efabe7582cf)

### Attempt 1

I'm going to view someone else basket other than mine. To do this I am going to sign into the admin account and head over to "Your Basket"

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/2fcf35d2-3c32-45de-90a6-d03bc976f352)

Now for me to view the other basket I would need to open the inspect panel and from there I would need to navigate over to the storage tab

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c38437f9-a6f0-4da7-ada2-d323f576bb4f)

Once on the storage tab, I would need to navigate over to "Session Storage" where the HTTP address would appear, once on it there will be 2 items in a table which are ```bid``` and ```ItemTotal```. To view someone else's basket you'd need to change the ```bid``` value to a different number.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/922ccd04-1216-41d1-8030-bd9e7a0cc50b)

Here is the bid number changed and the view of the other basket:

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/bb8ed938-9d5a-4d5c-a597-91418e61f416)

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/1db97ca4-5f02-4bc8-b04c-eb2e7ad94d4c)

I have come across something when you change the ```bid``` value to be more than 5 nothing will show

### Attempt 2

In this attempt, I am going to try and access the administration page that the juice shop has. I have done some research into this and for me to view the page I would need to be signed in as the admin if I was signed into any other account I wouldn't be allowed to access the page.

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

### What is Broken authentication ?

Broken authentication is a web vulnerability that allows attackers to bypass authentication controls, steal someone's identity to log in, or break other security mechanisms.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/22e17bca-92dd-4dfd-9ff3-207eda1c5f6c)


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

### Attempt 3

In this attempt, I am going to try and change the bender password without using SQL injection or forget password. For this attempt what I would need to do is sign into the benders account how I have done this using his email which is bender@juice-sh.op but I would need to add an apostrophe and 2 dashes so it will look like this ```bender@juice-sh.op'--``` and for the password, I can just put ```123```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/0944b296-e2f7-4251-b3c5-ad3a5f7e093e)

Now that I am logged into this account I would need to navigate over to "change password", to get to change password I would need to click on account -> privacy & security -> change password

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/48d4c09f-4fd3-4e50-9757-b3a8a07b9a10)

Now that I am on this page I can fill in the information required. Whilst I'm on this page I have BurpSuite running in the background and is looking out for any requests made by me. The information put in was ```1234```, ```123456``` and ```123456```, The reason for the last 2 passwords being longer than the first is because the password must be 5-20 characters long so ```1234```won't be accepted. Once all of the information has been filled in I can now press ```change```.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/ba37aaa4-4bed-4cb4-88da-60ad9f32aee8)

Once I pressed to change it said "Current password does not match", this is fine and now we can make our way over to BurpSuite. Now that I am on BurpSuite and on HTTP history I can now see that a request for password change has been made, and I can see that the passwords I used are there in the RAW file.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/dcf7d1c0-07e8-4c4a-a3e7-64e6f248c5d2)

The next step for me is to send the request to the repeater and I can do this by pressing ```CRTL + R```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/2800f2f1-64d8-42fd-8ea5-1eb17585b427)

Once in the repeater, I can see that the current password is not correct. Now the next step I need to do is to get rid of the current password which is shown below

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/4296e607-bd57-44b9-944c-5e61b7d8076c)

New version:

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/d25c8382-8258-4b99-aa5b-2f031ba0eb5a)

Now that I have gotten rid of the current password I can send the changes I made over to the response page and now I can see benders email and his password, the reason for his password looking like that is because has been turned into a hash.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/486c067f-c367-4992-8164-1edbb921fa46)

As part of this challenge and to complete it I would need to change his password to ```slurmCl4ssic```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/a4e37e8a-f4b0-44ed-a634-6914798483bc)

Now if I send the request over to the response I will get the same information outputted back to me but the password hash has been changed due to me changing the password to ```slurmCl4ssic```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/b1779334-4be6-4fa7-874a-6208244c90a9)

Since this has been completed I can now go back to JuiceShop and I will have completed the challenge

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/a35fed00-a03a-4d9e-87a0-880aaf8757a6)


## Requirement 7 - Exploit an Improper input Validation vulnerability

### What is Improper Input Validation vulnerability?

Improper Input Validation is a security vulnerability that happens when an application does not properly validate or sanitise input data before processing it. This vulnerability can allow an attacker to inject malicious input into the application, which can be used to compromise the system or steal sensitive information 

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/5cd795bc-06c2-433f-84ed-e86a51d33e0d)


### Attempt 1

In this attempt, I am going to upload a file that has no .pdf or .zip extensions. First of all, what I need to do is file a complaint on JuiceShop

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c1f59acc-b784-4cd0-b192-4ddf144d79c3)

The next step is to add a file, What I did beforehand is I created a text document on my desktop and put "hello" in it, I then saved it as a pdf with the name ```doc1.pdf```, now that I have a file to upload I can click on upload and select the file I created.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/f8cb71e6-937f-437c-99d0-7ccb9d5de459)

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/b918489b-6fd4-4b19-a5b9-d1f19808db73)

Now the next step is to file the complaint. Once I pressed submit the complaint was filed and went over to BurpSuite where I was on the HTTP history tab, I will need to look for the request called ```/file-upload```.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/05ffb705-f5ea-42fe-9d2c-baf20050708e)

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/d1ea6a54-584b-4be0-b82e-973e6ba7bb6e)

Now that I have found the request I can send it to the repeater. Once in the repeater, this is the information I will be presented with

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/fffb9ee6-d599-4990-88f8-00c58955ed2b)

Now I can view the file's content, which in this case is just "hello". Now the next step is to delete the content of the file and replace it with some data from a PNG file which was on my desktop. 

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/6d79a572-6b0f-4e76-9a8c-247c417c6102)

This is the PNG file I am using:

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/dcaf6fca-7038-4d51-b4c6-8fc099cad207)

Now that the data has been deleted the next step is to get the data from the PNG file to do this I will need to open the image within Notepad and I will be presented with this information

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/25d250ab-b71e-4f26-b9ec-148e8b337d50)

Now that I have some of the data from the PNG file I will need to paste it into BurpSuite in the place where "hello" was, 

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/0e75ce11-718a-4162-9450-29858a20f10a)

The next step is to change the extensions from PDF to PNG

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/34d3d938-53c8-43ff-926b-e1d44102b784)

Updated version:

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/9bdd3f0c-2928-4ad2-97e5-f6a95bd15e5e)

Now that has been done I can send the request to the responses tab and once that is done I can now go back onto JuiceShop I have now completed that challenge

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/a2ce8e11-01ad-4021-a68a-600cdfdbe3ba)

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/70e96d93-9134-4722-95c3-adc163bf199c)

## Requirement 8 - Exploit a Sensitive Data Exposure vulnerability

### What is sensitive data exposure?

Sensitive Data exposure is a vulnerability that happens when a web application does not adequately protect sensitive information from being accessible to attackers. The information which it could include is credit card data, medical history, session tokens, or other authentication credentials.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c8090234-a0fb-48b2-877b-e7f8fe1295a3)

There a three types of Sensitive Data exposure they are Availability breach, Confidentiality breach, and Integrity breach=

### How to prevent it

There are different practices which can help prevent sensitive data exposure and they are assessing risks associated with data, minimise data surface area, storing passwords using salted hashing functions and leveraging MFA, and disabling autocomplete and caching

### Attempt 1

For this attempt, I am going to try and gain access to any access log of the server. For this vulnerability, I am going to be using ```ffuf``` which stands for "Fuzz Faster you Fool". The intended use for ffuf is for discovering elements and content within web applications, or web servers. The next step is to open the terminal within Kali and type in ```ffuf```, Once I have typed that in I was presented with parameters which give me a list of commands which I can use and these different commands have different functionalities

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/3fbcd190-6bad-4150-8184-38166f94faae)

The next step is to allocate a wordlist and a good wordlist on Kali is the dirb wordlist. So for me to use this wordlist I would need to input this command into the terminal ```ffuf -w /usr/share/wordlists/dirb/common.txt``` There is more to add to this command as we also need to add the URL and some parameters before entering the command. The next step is to add the URL of JuiceShop and include ```FUZZ``` at the end of the URL so, in this case, it's ```http://192.168.123.111:3000/FUZZ``` now when this is added together with the first command it should look like this ```ffuf -w /usr/share/wordlists/dirb/common.txt -u http://192.168.123.111:3000/FUZZ``` 

Below is a screenshot of what the contents of the dirb wordlist are:

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/7b360234-4a5e-4ecd-bb57-0abae682b39a)

This is what it should look like in the terminal:

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/a09a8b51-7c66-4463-a49e-fa89cffdda77)

Now when you run the command this is what you will see

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/9c016fcd-26b5-49e7-af30-81d672419857)

This will keep going on and on until it's finished so I stopped the process as I noticed that the size of the folders is 1987. With ffuf it allows me to filter out content sizes using the parameters ```-fs``` and ```1987```, so now the full command would be ```ffuf -w /usr/share/wordlists/dirb/common.txt -u http://192.168.123.111:3000/FUZZ -fs 1987```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/8f9248a6-39da-4384-813b-58b69740c6fa)

Now when I run this command this is the output I get 

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/a22bc57f-e3a1-4630-aac7-207c9fbfe5e3)

Compared to before there are a lot fewer results showing up because we filtered out all the content with the size of 1987, The content which was found varies in content length and there are some interesting folders like "assets", "ftp" and so on. Now that I have some folders to go to I can put them at the end of the URL to see where it takes me.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/52d5a128-a802-4923-bf56-9b7125962df2)

When I redirected the page to assets I was redirected to a blank page, based on this I think that there is nothing to be shown within the assets folder

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/ca93a76d-09c8-4ab3-90c4-9fc7677e4526)

Since assets don't provide me with any information I am going to move on to ```ftp```, I am going to edit the URL get rid of assets and change it to FTP

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/f73e440b-27df-4ffa-bfee-dd58d07adec4)

When I searched for this page I came across some contents within the ftp folder

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/68e6a6d1-80ff-4081-8dcb-88b4a9562e3f)

Based on the files presented to me the ones which stand out the most are ```incident-support.kdbx``` and ```legal.md```. The reason for this is that I am trying to find the access logs so based on these files it could be the support team or the legal team. Now with this information, I can go back to ffuf and alter the command so instead of using FUZZ on the main webpage itself I am going to try and FUZZ the support folder to see if it exists and if there is any information within it. So the command should look like this ```ffuf -w /usr/share/wordlists/dirb/common.txt -u http://192.168.123.111:3000/support/FUZZ -fs 1987```

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/ff761b92-71cc-4f7f-8928-8177f355460b)

Once this has been completed there are 2 folders which I came across under the support folder.

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/c74f10a1-bf4e-4135-a836-59e89861bdb6)

What I am going to do now is search for ```logs``` in the URL and see where it brings me

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/99b90908-b364-4b5b-8342-f19beacfa2ac)

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/868a662d-8f9f-4c9e-891a-76e4c95ec899)

As I searched for logs within the support folder it took me to the access logs page which allows me to have access to all the log files. When I click on the files they will automatically download and I can access them this is what the files contain 

![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/016e7b55-d1c2-47a8-868d-2cd3589f70f6)

## Requirement 10 - Job search employability activity


![image](https://github.com/EthanKB1/Security-Testing/assets/157480256/a8c5988a-ab53-430e-8dd2-62bc36a85cc1)


## _References used_

Anusha Ihalapathirana (2021) OWASP Juice Shop — XSS Tier 0 and XSS Tier 1 Challenge Solutions. Available at: https://medium.com/swlh/owasp-juice-shop-xss-tier-0-and-xss-tier-1-challenge-solutions-48d414e42d2a (accessed 3 February 2024).

Cross-Site Scripting (XSS):: Pwning OWASP Juice Shop (2024) Available at: https://pwning.owasp-juice.shop/companion-guide/latest/part2/xss.html (accessed 3 February 2024).

Gao, W. (2020) Juice Shop 3.6 - Client Side XSS Protection - https://www.youtube.com/watch?v=9x7vAOgepic

‌Hamdan, M. (2020) Broken Authentication and SQL Injection - OWASP Juice Shop TryHackMe.

‌Web Security Tutorials (2020) OWASP Juice Shop Solution for CAPTCHA Bypass - https://www.youtube.com/watch?v=dlKWtRDKQQ0

‌thehackerish (2020) Broken Authentication and Session Management tutorial - thehackerish. Available at: https://thehackerish.com/broken-authentication-and-session-management-tutorial/ (accessed 17 February 2024).

Hacksplained (2020) ★★★★ Access Log (Sensitive Data Exposure) - https://www.youtube.com/watch?v=RBTfGk-ZwnY

What is Sensitive Data Exposure and How to Prevent it (2024) Available at: https://www.sentra.io/learn/sensitive-data-exposure (accessed 18 February 2024).

What is SQL Injection? Tutorial & Examples | Web Security Academy (2024) Available at: https://portswigger.net/web-security/sql-injection#SnippetTab (accessed 18 February 2024).

‌‌

‌


‌


