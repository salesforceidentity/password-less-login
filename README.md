Password-less login
===================
Password-less login into Salesforce Communities 


Overview
--------
This project provides an example of password-less login into Salesforce Communities to enable your customers and partners to login using a OTP that is sent to them via SMS or Email.





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
   - <YOUR ACCOUNT SID> - The Twilio Account SID
   - <YOUR AUTH TOKEN>  - The Twilio Auth Token 
   - <YOUR Twilio Phone Number> - The phone number that initiated the OTP message. This should be one of your Twilio phone numbers.

   - <CONNECTED APP CLIENT_ID> - The connected app client id 
 
7. In Communities, under Administration > Login & Registration, choose the Passwordless visualforce page as the login page. 

