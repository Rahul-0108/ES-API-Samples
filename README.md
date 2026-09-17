# ES-API Sample Apps

Copyright © Bentley Systems, Incorporated. All rights reserved.

A repository for sample applications which utilise aspects of Bentley's ES-API. This README details processes which are the same among the subprojects.
* [ES-API Projects](/EsApiProjectsSampleApp/) - Demonstrates how to create, query, and delete a Project using the Enterprise Systems Project API, also dealing with billing country endpoints and data center endpoint usage.
* [ES-API WorkArea](/EsApiProjectsSampleApp/) - ProjectWise Web Connections API for mapping Work Area Connections from Project Wise Design Integration to iTwin Projects.
* [ES-API 4D Schedules Service](/EsApi4DScheduleServiceSampleApp/) - Demonstrates an example workflow of acquiring a Resource Status History item and changing the Date value within said item using the 4D Schedules External API.
* [ES-API 4D Schedules SPA](/EsApi4DScheduleSPASampleApp/) - Demonstrates an example workflow of acquiring a Resource Status History item and changing the Date value within said item using the 4D Schedules External API.

## Prerequisites

* [Git](https://git-scm.com/)
* (For non-web applications) [.NET 6.0](https://dotnet.microsoft.com/download/dotnet/6.0/)
* (For web applications) [NodeJS and NPM](https://nodejs.org/en/download)
* Optionally an IDE like Visual Studio 2022 or [Visual Studio Code](https://code.visualstudio.com/). It is also possible to use command line.

## How to acquire a token

Valid access token with scope 'enterprise' is required to access API endpoints. For more technical information about tokens and authentication see: https://developer.bentley.com/apis/overview/authorization.

In order to run this sample app or if you want to develop your own application you'll have to register a client in https://developer.bentley.com/esregister.

### API Client registration steps

 1. If you're not logged in, you'll get redirected to a login page once you go to https://developer.bentley.com/esregister. If you don't have one already, create an account and start the trial. Then go back to api client registration page.
 2. Fill in application name. This is a display name, client id will be generated automatically.
 3. Check the api client details:
    1. Make sure `Enterprise` is checked under *API associations*
    2. Make sure `enterprise` scope is added under *Allowed scopes*
    3. For web apps, make sure `Allow Offline Access` is checked.
 4. Select an appropriate application type. If you want to run the sample app select `Web App` type. If you don't know which type to choose for a user-facing application check out https://developer.bentley.com/apis/overview/authorization.
 5. Fill in redirect url. This is the url to your application which authentication service will come back to once user is logged in.
 6. Click `Save`
 7. Make sure to copy client secret and close the dialog.
 8. A page should appear with created api client. In order to get tokens you'll also need the client id that should be shown in this window.
 9. You should be able to authenticate now by using client id and secret with the appropriate flow.

### Commands to get the token via Web App flow


#### Login using the /authorize endpoint

```text
Open the following URL in a browser:

https://ims.bentley.com/connect/authorize?response_type=code&client_id=YOUR_CLIENT_ID&redirect_uri=YOUR_REDIRECT_URI&scope=enterprise&state=YOUR_STATE

For example if the redirect url is http://localhost:5001/callback:

https://ims.bentley.com/connect/authorize?response_type=code&client_id=YOUR_CLIENT_ID&redirect_uri=http%3A%2F%2Flocalhost%3A5001%2Fcallback&scope=enterprise&state=12345

After successful login, Bentley IMS redirects to the configured redirect URI:

https://localhost:5001/callback?code=AUTHORIZATION_CODE&state=12345

Copy the code value from the callback URL. The authorization code is short-lived, so use it promptly to request the access token.

Note: The redirect_uri must exactly match one of the redirect URIs configured for the Web App.
```

#### Bash

```sh
curl --request POST ^
  --url "https://ims.bentley.com/connect/token" ^
  --header "content-type: application/x-www-form-urlencoded" ^
  --data-urlencode "grant_type=authorization_code" ^
  --data-urlencode "code=YOUR_AUTHORIZATION_CODE" ^
  --data-urlencode "client_id=YOUR_CLIENT_ID" ^
  --data-urlencode "client_secret=YOUR_CLIENT_SECRET" ^
  --data-urlencode "redirect_uri=YOUR_REDIRECT_URI"

Note: Do not encode redirect_uri
```

#### Powershell

```pwsh
(Invoke-WebRequest -Method 'Post' `
   -Uri 'https://ims.bentley.com/connect/token' `
   -Headers @{
      'content-type' = 'application/x-www-form-urlencoded'
   } `
   -Body @{
      grant_type='authorization_code'
      code='YOUR_AUTHORIZATION_CODE'
      client_id='YOUR_CLIENT_ID'
      client_secret='YOUR_CLIENT_SECRET'
      redirect_uri='YOUR_REDIRECT_URI'
   }).Content

Note: Do not encode redirect_uri
```
