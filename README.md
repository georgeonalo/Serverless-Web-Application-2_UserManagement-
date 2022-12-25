# Module 2: User Authentication and Registration with Amazon Cognito User Pools

In this module you'll create an Amazon Cognito user pool to manage your users' accounts. You'll deploy pages that enable customers to register as a new user, verify their email address, and sign into the site.

## Architecture Overview

When users visit our website they will first register a new user account. For the purposes of this project we'll only require them to provide an email address and password to register. However, you can configure Amazon Cognito to require additional attributes in your own applications.

After users submit their registration, Amazon Cognito will send a confirmation email with a verification code to the address they provided. To confirm their account, users will return to your site and enter their email address and the verification code they received. You can also confirm user accounts using the Amazon Cognito console if you want to use fake email addresses for testing.

After users have a confirmed account (either using the email verification process or a manual confirmation through the console), they will be able to sign in. When users sign in, they enter their username (or email) and password. A JavaScript function then communicates with Amazon Cognito, authenticates using the Secure Remote Password protocol (SRP), and receives back a set of JSON Web Tokens (JWT). The JWTs contain claims about the identity of the user and will be used in the next module to authenticate against the RESTful API you build with Amazon API Gateway.

![image](https://user-images.githubusercontent.com/115881685/208940815-6fe2e016-3f87-4d96-be8f-6d7cb466b2d3.png)

### Implementation Instructions

❗ Ensure you've completed the [Static Web hosting](https://github.com/georgeonalo/Serverless-Web-Application-1_StaticWebHosting-) step before beginning the workshop.

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

##### ✅ Step-by-step directions

1. On your Cloud9 development environment open js/config.js

2. Update the cognito section with the correct values for the user pool and app you just created. You can find the value for userPoolId on the Pool details page of the Amazon Cognito console after you select the user pool that you created.

![image](https://user-images.githubusercontent.com/115881685/208946003-6dfa5bbf-4b4b-4318-8ddd-e20648095e2b.png)

You can find the value for userPoolClientId by selecting App clients from the left navigation bar. Use the value from the App client id field for the app you created in the previous section.

![image](https://user-images.githubusercontent.com/115881685/208946116-d9ba9436-63a9-40be-ac34-d64f595270bf.png)

The value for region should be the AWS Region code where you created your user pool. E.g. us-east-1 for the N. Virginia Region, or us-west-2 for the Oregon Region. If you're not sure which code to use, you can look at the Pool ARN value on the Pool details page. The Region code is the part of the ARN immediately after arn:aws:cognito-idp:.

The updated config.js file should look like this. Note that the actual values for your file will be different:

window._config = {

    cognito: {
    
        userPoolId: 'us-west-2_uXboG5pAb', // e.g. us-east-2_uXboG5pAb
        
        userPoolClientId: '25ddkmj4v6hfsfvruhpfi7n4hv', // e.g. 25ddkmj4v6hfsfvruhpfi7n4hv
        
        region: 'us-west-2' // e.g. us-east-2
        
    },
    
    api: {
    
        invokeUrl: '' // e.g. https://rc7nyt4tql.execute-api.us-west-2.amazonaws.com/prod,
        
    }
    
};

3. Save the modified file making sure the filename is still config.js.

4. Commit the changes to your git repository:

$ git add js/config.js 

$ git commit -m "configure cognito"

$ git push

...

Counting objects: 4, done.

Compressing objects: 100% (4/4), done.

Writing objects: 100% (4/4), 415 bytes | 415.00 KiB/s, done.

Total 4 (delta 3), reused 0 (delta 0)

To https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site

   7668ed4..683e884  master -> master
   
   Amplify Console should pick up the changes and begin building and deploying your web application.
   
Note: Instead of having you write the browser-side code for managing the registration, verification, and sign in flows, we provide a working implementation in the assets you deployed in the first module. The cognito-auth.js file contains the code that handles UI events and invokes the appropriate Amazon Cognito Identity SDK methods. For more information about the SDK, see the project page on GitHub.

##### Implementation Validation

##### ✅ Step-by-step directions


1. Visit register.html under your website domain, or choose the Giddy Up! button on the homepage of your site.

2. Complete the registration form and choose Let's Ryde. You can use your own email or enter a fake email. Make sure to choose a password that contains at least one upper-case letter, a number, and a special character. Don't forget the password you entered for later. You should see an alert that confirms that your user has been created.

3. Confirm your new user using one of the two following methods.

4. If you used an email address you control, you can complete the account verification process by visiting /verify.html under your website domain and entering the verification code that is emailed to you. Please note, the verification email may end up in your spam folder. For real deployments we recommend configuring your user pool to use Amazon Simple Email Service to send emails from a domain you own.

5. If you used a dummy email address, you must confirm the user manually through the Cognito console.

  i. From the AWS console, click Services then select Cognito under Security, Identity & Compliance.
  ii. Choose Manage your User Pools
  iii. Select the WildRydes user pool and click Users and groups in the left navigation bar.
  iv. You should see a user corresponding to the email address that you submitted through the registration page. Choose that username to view the user detail page.
  v. Choose Confirm user to finalize the account creation process.
  
6. After confirming the new user using either the /verify.html page or the Cognito console, visit /signin.html and log in using the email address and password you entered during the registration step.

7. If successful you should be redirected to /ride.html. You should see a notification that the API is not configured.

![image](https://user-images.githubusercontent.com/115881685/208949428-95b0b25d-cf60-4fb6-a4a6-028117ac0148.png)

##### ⭐ Recap

🔑 Amazon Cognito provides two different capabilities for managing users, federated identities and user pools. Amazon Cognito user pools can handle almost every aspect about managing users, their login credentials, handling password resets, multifactor authentication and much more!

🔧 In this module you've used user pools to create a completely hosted and managed user management system that will allow us to authenticate your users and manage their user information. From there you've updated the website to use the user pool and utlized the AWS SDKs to provide a signin form on the site.

##### Next

✅ After you have successfully logged into your web application, you can proceed to the next module, [Serverless Backed](https://github.com/georgeonalo/Serverless-Web-Application-3_ServerlessBackend-)
   
   



