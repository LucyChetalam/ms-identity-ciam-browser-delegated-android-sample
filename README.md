# Sign in users and call a protected web API in sample Android (Kotlin) mobile app

* [Overview](#overview)
* [Contents](#contents)
* [Prerequisites](#prerequisites)
* [Project setup](#project-setup)
* [Key concepts](#key-concepts)
* [Reporting problems](#reporting-problems)
* [Contributing](#contributing)

## Overview

This guide demonstrates how to configure a sample Android mobile application to sign in users, and call an ASP.NET Core web API using Microsoft Entra for customers.

## Contents

| File/folder | Description |
|-------------|-------------|
| `/app/src/main/res/raw/auth_config_ciam.json`       | Configuration file. |
| `.gitignore` | Define what to ignore at commit time. |
| `README.md` | This README file. |
| `LICENSE`   | The license for the sample. |

## Prerequisites

- <a href="https://developer.android.com/studio" target="_blank">Android Studio</a>.
- Microsoft Entra External ID for customers tenant. If you don't already have one, <a href="https://aka.ms/ciam-free-trial?wt.mc_id=ciamcustomertenantfreetrial_linkclick_content_cnl" target="_blank">sign up for a free trial</a>. 
- An API registration that exposes at least one scope (delegated permissions) and one app role (application permission) such as *ToDoList.Read*. If you haven't already, follow the instructions for [call an API in a sample Android mobile app](https://learn.microsoft.com/en-us/entra/external-id/customers/sample-native-authentication-android-sample-app-call-web-api) to have a functional protected ASP.NET Core web API. Make sure you complete the following steps:

    - Register a web API application
    - Configure API scopes
    - Configure app roles
    - Configure optional claims
    - Clone or download sample web API
    - Configure and run sample web API

## Project setup

To enable your application to authenicate users with Microsoft Entra, Microsoft Entra for customers must be made aware of the application you create. The following steps show you how to:

### Step 1: Register an application

Register your app in the Microsoft Entra admin center using the steps in [Register an application](https://learn.microsoft.com/en-us/entra/external-id/customers/sample-mobile-app-android-kotlin-sign-in-call-api#register-an-application).

### Step 2: Add a platform redirect URL

Add platform URL using the steps in [Add a platform redirect URL](https://learn.microsoft.com/en-us/entra/external-id/customers/sample-mobile-app-android-kotlin-sign-in-call-api#add-a-platform-redirect-url).

### Step 3: Enable public client flow

Enable public client flow using the steps in [Enable public client flow](https://learn.microsoft.com/en-us/entra/external-id/customers/sample-mobile-app-android-kotlin-sign-in-call-api#enable-public-client-flow).

### Step 4: Delegated permission to Microsoft Graph

Grant API permissions using the steps in [Delegated permission to Microsoft Graph](https://learn.microsoft.com/en-us/entra/external-id/customers/sample-mobile-app-android-kotlin-sign-in-call-api#delegated-permission-to-microsoft-graph)

### Step 5: Grant web API permissions to the Android sample app

Grant web API permissions to the Android sample app using the steps in [Grant web API permissions to the Android sample app](https://learn.microsoft.com/en-us/entra/external-id/customers/sample-mobile-app-android-kotlin-sign-in-call-api#grant-web-api-permissions-to-the-android-sample-app)

### Step 6: Clone sample Android mobile application

Clone the sample Android mobile application by following the steps outlined in [Clone sample Android mobile application](https://learn.microsoft.com/en-us/entra/external-id/customers/sample-mobile-app-android-kotlin-sign-in-call-api#clone-sample-android-mobile-application).

### Step 7: Run and test sample Android mobile application

Run and test the Android sample mobile application by following the steps in [Run and test sample Android mobile application](https://learn.microsoft.com/en-us/entra/external-id/customers/sample-mobile-app-android-kotlin-sign-in-call-api#run-and-test-the-sample-android-mobile-application).

## Key concepts

Open `app/src/main/res/raw/auth_config_ciam.json` file and you find the following json configurations:

```kotlin
{
  "client_id" : "Enter_the_Application_Id_Here",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "Enter_the_Redirect_Uri_Here",
  "account_mode" : "SINGLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "CIAM",
      "authority_url": "https://Enter_the_Tenant_Subdomain_Here.ciamlogin.com/Enter_the_Tenant_Subdomain_Here.onmicrosoft.com/"
    }
  ]
}
```

The JSON configuration file has:

- `Enter_the_Application_Id_Here` is replaced with the **Application (client) ID** of the app you registered during project setup.
- `Enter_the_Redirect_Uri_Here` is replaced with the value of *redirect_uri* in the Microsoft Authentication Library (MSAL) configuration file you downloaded earlier when you added the platform redirect URL.
- `Enter_the_Tenant_Subdomain_Here` is replaced with the Directory (tenant) subdomain. For example, if your tenant primary domain is `contoso.onmicrosoft.com`, use `contoso`. If you don't know your tenant subdomain, learn how to [read your tenant details](how-to-create-customer-tenant-portal.md#get-the-customer-tenant-details).

You use `app/src/main/res/raw/auth_config_ciam.json` file to set configuration options when you initialize the client app in the Microsoft Authentication Library (MSAL).

To create *PublicClientApplication* object, use the following code:

```kotlin
private suspend fun initClient(): ISingleAccountPublicClientApplication = withContext(Dispatchers.IO) {
    return@withContext PublicClientApplication.createSingleAccountPublicClientApplication(
        this@MainActivity,
        R.raw.auth_config_ciam
    )
}
```

In the `initClient()` method, create an MSAL instance so that we can perform authentication logic and interact with our tenant. The `app/src/main/res/raw/auth_config_ciam.json` file is passed as parameter.

## Reporting problems

* Search the [GitHub issues](https://github.com/Azure-Samples/ms-identity-ciam-browser-delegated-android-sample/issues) in the repository - your problem might already have been reported or have an answer.
* Nothing similar? [Open an issue](https://github.com/Azure-Samples/ms-identity-ciam-browser-delegated-android-sample/issues/new) that clearly explains the problem you're having running the sample app.

## Contributing

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information, see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
