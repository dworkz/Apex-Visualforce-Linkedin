# Apex-Visualforce-Linkedin
Simple template LinkedIn integration with Salesforce. 
Apply and sing in functionality.

Every integration that I was doing was bringing me a lot of pain, cause I simply couldn’t understand how to get an access to API, how to do this correctly and so on. So every time I was googling for some super simple class or description to start with.
   So, yesterday I decided to check newly published LinkedIn API. My idea was to create a very simple skeleton: a VF page and a class. In this case a very newbie developer could start with that skeleton and create more functionality based on it.

Today, LinkedIn offers 4 possibilities of using an API:
•	Sign in with LinkedIn
•	Apply with LinkedIn
•	Share on LinkedIn
•	Manage Company Pages

LinkedIn API description

   In this article I will cover Sign In and Apply functionality, maybe afterwards I will compose an article on other two options, we’ll see.
Basically, these two methods are very similar, the only difference is that Sign In has a very limited number of fields available, but Apply is intended more for a company to have an apply for a job form, so there could be problems with using it for different purposes.

Fileds for Sign In
Fields for Apply
So, let’s get our hands dirty!

First of all we have to register an application:
1.      Go to https://www.linkedin.com/secure/developer and press “Add New Application”
2.      Fill in the form, no rocket science here.
3.      I selected all checkboxes in the default scope section, we can change this later, or just add a scope parameter while authorizing.
4.      In OAuth 2.0 Redirect URLs section I added a link to the page that will act like a form, in my case it is https://vprok-developer-edition.na15.force.com/MyCommunity/LinkedIntestPage . I added it into the community to make it public, so everyone could share his data without sf account.

   After the application is registered there is one last thing we should do before writing the apex.
Go to setup -> Remote site settings and add two links:
•	https://www.linkedin.com
•	https://api.linkedin.com

Time for some Apex now! We’re going to need only one simple page and one class that will act as a controller.
Create a page with a name “LinkedIntestPage” and a class with a name “LinkedInControllerTest”.
Code for the page is very simple, no fancy things at all!

<apex:page controller="LinkedInControllerTest" showHeader="false">  
<apex:form>  
    <apex:pageblock>  
        <apex:commandbutton action="{!LinkedInAuth}" rendered="{!showButton}" value="LinkedIn Authentication">  </apex:commandbutton>
    </apex:pageblock>  
</apex:form>  
<apex:outputtext rendered="{!showGreet}">Thank you, {!firstName}</apex:outputtext>
</apex:page>

There are some variables in the controller that you should take care of (link to the manual):

private static final String stateKey = '12345';
This is a code that you use to avoid CSRF attacks. You can use any code you want.

private static final String apiKey = ' 78r5gw0tg3ah81'; // client_id
This your application key(Consumer Key / API Key), in my case it is 78r5gw0tg3ah81

private static final String redirectUri = 'https://vprok-developer-edition.na15.force.com/MyCommunity/LinkedIntestPage';
Redirect URI is basically the page user will be redirected after accepting the data share. The same page that was described in “OAuth 2.0 Redirect URLs” when you were registering the app.

private static final String secret = ' bMlzOyYP1tpCkjPT'; // client_secret


Basically, the code just creates a new page reference with the link containing all the necessary information , than the user is redirected to this link and if the user presses "Allow", the browser is redirected back to the starting page, but the code parameter is appended as a parameter.

If the code and state parameters are alright - GetAccessToken() method fires and after the access token is acquired the CreateGreetMessage() method fires , which basically makes a call to LinkedIn API to get some user data. In my case it just takes FirstName to create a message on the page. So after whole process the user will get something like "Thank you, {!FirstName}"

That's all! This template could be used to make further enhancements, for example creating a contact after authorization or whatever you will need.
