Password-less login
===================
Password-less login into Salesforce Communities 


Overview
--------
This project provides an example of password-less login into Salesforce Communities to enable your customers and partners to login using a OTP that is sent to them via SMS or Email.


This solution uses a custom Communities login visualforce page and an Apex controller that implement a single page interactive login process.. The Apex controller first renders a visualforce page the invites users to enter their phone number, and then renders a secondary visualforce page that prompts users to enter the One-Time-Password (OTP) code that is sent to their phone.

When users enter their phone number and hit ‘Next’ the login controller does the following:
- Lookup the user based on the entered phone number. If no user with a matching phone number is found an error message shows up. 
- Generates a OTP code and send it via SMS to the user phone
- Dynamically renders the secondary page that prompts the user to enter the OTP for validation.  

When the user types in the OTP the controller verifies if this OTP matches the one that was originally generated and sent to the user by the controller. If the two OTPs match the user identity is successfully verified and the controller invokes an OAuth JWT Bearer flow to get an API session (access token), and then exchange it with a UI session and log in the user into the Community.


Some things to keep in mind
---------------------------
- This example uses SMS OTP for identity verification. Similarly you can extend it to support other verification methods such as Email verification (i.e. OTP sent to the user email address), or send/receive iMessages. 

- Phone number should be unique. In this example the users phone numbers are stored in the User.communitynickname field that is a unique field. You can either use this field or create a custom unique field to store the user’s verified phone number. 

- Registration - this examples assumes that all the community users are already registered and their phone numbers are verified and stored in the system. In a real world scenario you may want to implement a self-service Signup process to allow onboarding new users. 

- User Recovery - consider adding a user recovery option to provide users the ability to login with alternative identity verification methods in case they forgot/change their phone number or lost their mobile device. For example, you may still want to require users to set a password or an email address during the initial registration. 



Instructions
------------
1. Integrate a 3rd party SMS delivery service such as Twilio or TeleSign

   Install the Twilio's unmanged package for Salesforce from the "Twilio Helper Library for Salesforce":
   https://www.twilio.com/docs/libraries/salesforce

2. Generate private and public key

   The private key is required to sign the JSON Web Token (JWT) requests.
   The certificate (public key) is required to verify the JSON Web Signature (JWS) and perform the API-based authentication. 
   
    - Go to Setup > Security Controls > Certificate and Key Management and generate a self-signed certificate.
    - Download the certificate to you desktop

3. Create a connected app
 
   Create a connected app to handle the OAuth JWT bearer flow

   - Go to Setup > Build > Create > Apps and create an OAuth connected app
   - Enter 'https://login.salesforce.com/services/oauth2/success' as the Callback URL
   - Select API scope
   - Upload the certificate from step #2

4. Create a Remote Site Setting

   To allow Communities to make outbound HTTP calls to itself there's a need to whitelist the Community URL. 

   - Go to Security > Security Controls and create a new Remote Site Setting with your Community URL

5. Create the Passwordless VF page

6. Create the LoginController apex class 
   
   Replace the following:
   - TWILIO_ACCOUNT_SID  - The Twilio Account SID
   - TWILIO_AUTH_TOKEN   - The Twilio Auth Token 
   - TWILIO_PHONE_NUMBER - The phone number that initiated the OTP message. This should be one of your Twilio phone numbers.

   - CONNECTED_APP_CLIENT_ID - The connected app client id 
 
7. In Communities, under Administration > Login & Registration, choose the Passwordless visualforce page as the login page. 

