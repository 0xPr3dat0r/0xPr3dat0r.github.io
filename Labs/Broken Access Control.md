Access control is a way an application decide  what the user can do 
* Authentication - Confirms the users identity 
* Session Management - Tracks the users activities and identifies the request that are being made by the user
* Access Control - Checks if the action that the users is about to perform is permitted 
Poorly access control can pose a huge security risk.

**Vertical Privilege Escalation -: This is when the hacker or users gains access to a functionality that they are not supposed to access. For example if a non administrative user gain access to the admin page and can delete users accounts is also known as Vertical Privilege Escalation.
Vertical privilege escalation is when the hacker moves from a low level user and then to a high level user.**

**Unprotected Functionality:-  This type access control vulnerability is when the hacker or a user can request for a particular page this is only intended for the admin by typing it into the url, Hackers can also bruteforce webpages of the website and for this to occur there has to be some form of vertical privilege escalation. For example a website host a sensitive functionality like this `https://website.com/` and then the hackers can just admin to the url and it actually gives them the admin page `https://website.com/admin`**

**Lab : - Unprotected admin functionality**
![[Pasted image 20250909142211.png]]

**Step 1: I started by intercepting the website traffic**
![[Pasted image 20250912185020.png]]

**Step 2 : I look at all the functionality in the web page**
![[Pasted image 20250912185138.png]]
I noticed the My Account Button 
**Step 3: Got redirected to the login page**
![[Pasted image 20250912185402.png]]
Then i sent the request to Burp Repeater 
![[Pasted image 20250912185540.png]]

**Step 4 : Switched the login header to admin so as to get the admin panel**
![[Pasted image 20250912185800.png]]
![[Pasted image 20250912185830.png]] Got a 404 response, Tried  admin-panel and possible name for the admin webpage 
**Step 5 : Boom Got a 200 response code**
![[Pasted image 20250912190425.png]]
![[Pasted image 20250912190624.png]]
So as you can see an unauthorized user got access to the admin panel and the unauthorized user can delete users  and we get a congratulations that we have solved the Lab
![[Pasted image 20250912190958.png]]


**Unprotected admin functionality:-  Some applications hide sensitive features (like admin panels) behind obscure URLs, a method called _security by obscurity_. However, this is ineffective because URLs can still leak. For example, an admin panel URL hidden in JavaScript may still be visible to all users in the source code, even if the link only shows up for admins.

**Lab :  Unprotected admin functionality with unpredictable URL**
![[Pasted image 20250923004917.png]]

**Step 1: I intercepted the request using burp proxy and i went straight to login page.**
![[Pasted image 20250923005037.png]]
![[Pasted image 20250923005151.png]]
**Step 2:  Tried to login wasn't meant to login !**
![[Pasted image 20250923005323.png]]
![[Pasted image 20250923005536.png]]
**Step 3: Went back to the home page to see if there was any clue and i clicked on the first product that i saw and sent the request to Burp Repeater !**
![[Pasted image 20250923005846.png]]
**Step 4: Noticed something about the JavaScript code, There had dropped the URL for the admin  page in the code**
![[Pasted image 20250923010020.png]]
**Step 5: Changed the request to the URL code that was for the admin page Boom I got a 200 response code .**
![[Pasted image 20250923010228.png]]
**Step 6: I could delete the users !**
![[Pasted image 20250923010423.png]]

**Parameter-based access control methods:- Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location. This could be:
- **A hidden field.
- **A cookie
- **A preset query string parameter.
The application makes access control decisions based on the submitted value. For example:
`https://insecure-website.com/login/home.jsp?admin=true https://insecure-website.com/login/home.jsp?role=1`
This approach is insecure because a user can modify the value and access functionality they're not authorized to, such as administrative functions.**

**Lab: User role controlled by request parameter**
![[Pasted image 20250923012144.png]]
**Step 1: I intercepted the request of the login page**
![[Pasted image 20250923012550.png]]
**Step2: First thing i noticed is that admin is set to False!**
![[Pasted image 20250923012646.png]]
**Step3 : Sent the request to Burp Repeater and changed the parameter to True**
![[Pasted image 20250923012843.png]]
**Step 4: Got a 302 response code and i followed the redirect and i got a 200 response code but i could not get access to the admin portal or page.**
![[Pasted image 20250923012926.png]]
**Step 5: I logged in again and i intercepted the request  again and i saw the admin parameter false and i changed it to true and forwarded the request.**
![[Pasted image 20250923145829.png]]
**Step 6: I clicked on the admin panel and i intercepted the request and changed the parameter to true**
![[Pasted image 20250923150052.png]]
![[Pasted image 20250923150123.png]]
**Step 7: Forwarded the request and changed the request again and changed the cookie parameter**
![[Pasted image 20250923150400.png]]
and i was able to delete the users account and Boom
![[Pasted image 20250923150536.png]]

**Horizontal  Privilege Escalation:-  This occurs to when the users is able to gain resources belonging to another user, Instead of their own for example. Horizontal privilege escalation attacks may use similar types of exploit methods to vertical privilege escalation. For example, a user might access their own account page using the following URL:
`https://insecure-website.com/myaccount?id=123`
If an attacker modifies the `id` parameter value to that of another user, they might gain access to another user's account page, and the associated data and functions. In some applications, the exploitable parameter does not have a predictable value. For example, instead of an incrementing number, an application might use globally unique identifiers (GUIDs) to identify users. This may prevent an attacker from guessing or predicting another user's identifier. However, the GUIDs belonging to other users might be disclosed elsewhere in the application where users are referenced, such as user messages or reviews.**

**Lab:  User ID controlled by request parameter, with unpredictable user IDs**
![[Pasted image 20250923153817.png]]
**Step 1: As you all know start by intercepting the request and when i logged in i was given an API key**
![[Pasted image 20250923154403.png]]
**Step 2: I logged out to see if there could be any clue**
![[Pasted image 20250923155759.png]]
No there was no clue 
**Step 3: So the thing i have been looking through the articles to see if the user carlos had made a comment on one of them.**
![[Pasted image 20250923161700.png]]
and i have been searching for quite some time now but it struck my mind what if carlos had written an article will the website expose is GUID. 
![[Pasted image 20250923161816.png]]
This article was written by the administrator.
**Step 4: I finally found an article that the user carlos wrote!**
![[Pasted image 20250923162147.png]]
**Step 5: I tried interacting with the article that carlos wrote to see if the website will show his GUID**
![[Pasted image 20250923162805.png]]
After made a comment on the post got nothing. Then i clicked on the users name carlos and i got his  id 
![[Pasted image 20250923162940.png]]
**Step 6: I logged out so i could switch the id  and i clicked on my account**
![[Pasted image 20250923163709.png]]
and i saw account id and i changed the id to carlos own id and Boom i was able to access carlos account and get his own API key  `io8KpnnXtpJE6hm4yJvdrIu7n3H1dqpc`

## Prevention 
- Never rely on obfuscation alone for access control.
- Unless a resource is intended to be publicly accessible, deny access by default.
- Wherever possible, use a single application-wide mechanism for enforcing access controls.
- At the code level, make it mandatory for developers to declare the access that is allowed for each resource, and deny access by default.
- Thoroughly audit and test access controls to ensure they work as designed.
