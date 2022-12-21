# Module 2: User Authentication and Registration with Amazon Cognito User Pools

In this module you'll create an Amazon Cognito user pool to manage your users' accounts. You'll deploy pages that enable customers to register as a new user, verify their email address, and sign into the site.

## Architecture Overview

When users visit our website they will first register a new user account. For the purposes of this project we'll only require them to provide an email address and password to register. However, you can configure Amazon Cognito to require additional attributes in your own applications.

After users submit their registration, Amazon Cognito will send a confirmation email with a verification code to the address they provided. To confirm their account, users will return to your site and enter their email address and the verification code they received. You can also confirm user accounts using the Amazon Cognito console if you want to use fake email addresses for testing.

After users have a confirmed account (either using the email verification process or a manual confirmation through the console), they will be able to sign in. When users sign in, they enter their username (or email) and password. A JavaScript function then communicates with Amazon Cognito, authenticates using the Secure Remote Password protocol (SRP), and receives back a set of JSON Web Tokens (JWT). The JWTs contain claims about the identity of the user and will be used in the next module to authenticate against the RESTful API you build with Amazon API Gateway.

![image](https://user-images.githubusercontent.com/115881685/208940815-6fe2e016-3f87-4d96-be8f-6d7cb466b2d3.png)

### Implementation Instructions

❗ Ensure you've completed the Static Web hosting step before beginning the workshop.

#### 1. Create an Amazon Cognito User Pool

##### Background

Amazon Cognito provides two different mechanisms for authenticating users. You can use Cognito User Pools to add sign-up and sign-in functionality to your application or use Cognito Identity Pools to authenticate users through social identity providers such as Facebook, Twitter, or Amazon, with SAML identity solutions, or by using your own identity system. For this module you'll use a user pool as the backend for the provided registration and sign-in pages.

Use the Amazon Cognito console to create a new user pool using the default settings. Once your pool is created, note the Pool Id. You'll use this value in a later section.

###### ✅ Step-by-step directions

1. Go to the Amazon Cognito Console

2. Choose Manage your User Pools.

3. Choose Create a User Pool

4. Provide a name for your user pool such as WildRydes, then select Review Defaults

![image](https://user-images.githubusercontent.com/115881685/208941850-64b2572c-7eed-4772-8ca1-ef1e5d1a7e60.png)

5. On the review page, click Create pool.

6. Note the Pool Id on the Pool details page of your newly created user pool.

###### 2. Add an App Client to Your User Pool

From the Amazon Cognito console select your user pool and then select the App clients section. Add a new app and make sure the Generate client secret option is deselected. Client secrets aren't supported with the JavaScript SDK. If you do create an app with a generated secret, delete it and create a new one with the correct configuration.

###### ✅ Step-by-step directions

1. From the Pool Details page for your user pool, select App clients from the General settings section in the left navigation bar.

2. Choose Add an app client.

3. Give the app client a name such as WildRydesWebApp.

4. Uncheck the Generate client secret option. Client secrets aren't supported for use with browser-based applications.

5. Choose Create app client.

![image](https://user-images.githubusercontent.com/115881685/208942547-c209d873-f6b6-4bed-a50b-63293a0216fc.png)

6. Note the App client id for the newly created application.

##### 3. Update the config.js File in Your Website

The /js/config.js file: ([https://github.com/georgeonalo/Serverless-Web-Application-1_StaticWebHosting-website-js-config.js-]), contains settings for the user pool ID, app client ID and Region. Update this file with the settings from the user pool and app you created in the previous steps and commit the file back to your git repository.



