---
pagename: Client Credentials
keywords:
sitesection: Documents
categoryname: "Developer Tools"
documentname: LivePerson Functions
permalink: liveperson-functions-external-invocations-client-credentials.html
indicator: both
redirect_from:
  - function-as-a-service-external-invocations-client-credentials.html
---

This section describes how to call functions from outside of LivePerson's platform via the OAuth 2.0 grant type [Client Credentials](https://oauth.net/2/grant-types/client-credentials/). This is the preferred way to authorize for machine-to-machine communication.

Under the hood LivePerson Functions uses JSON Web Tokens (JWTs) to authorize requests ([here](https://jwt.io/introduction/) is a good introduction on JWTs).

### Step 1: Generate client_id & client_secret:

* Execute the **App Installation** process in order to generate a `client_id` and `client_secret` for the OAuth 2.0 flow. This can be done by your LivePerson account team, so you should contact them for more assistance (you can also see [this](https://developers.liveperson.com/guides-le-applications-installing.html) guide for more detailed information). You can use this JSON as a base template for requesting an App Installation :

```json
{
  "client_name": "My First App",
  "description": "This is my first App",
  "grant_types": [
    "client_credentials"
  ],
  "response_types": ["token"],
  "scope": "faas.lambda.invoke"
}
```

### Step 2: Retrieve domains of the Functions Invocation Gateway

The Functions Invocation Gateway is the main entry-point for invoking `lambdas` in Functions via its RESTful API. You can use the following hosts for external invocation (the base domain is: `faasGW`):

* **APAC**: sy.faasgw.liveperson.net

* **EMEA**: lo.faasgw.liveperson.net

* **US**:   va.faasgw.liveperson.net

However, instead of hardcoding the domain, it is recommended to use the [LivePerson Domain API](https://developers.liveperson.com/agent-domain-domain-api.html) to retrieve this information by providing the service name `faasGW` and retrieving the domain **dynamically**.

### Step 3: Retrieve the domain of the authorization server.

You'll also need to pass your `client_id` and `client_secret` to one of our authorization servers for authentication. Depending on your account, you can use the following servers (the base domain is: `sentinel`):

* **APAC:** sy.sentinel.liveperson.net

* **EMEA:** lo.sentinel.liveperson.net

* **US:**   va.sentinel.liveperson.net

However, instead of hardcoding the authorization server, it is recommended to use the [LivePerson Domain API](https://developers.liveperson.com/agent-domain-domain-api.html) to retrieve this information by providing the service name `sentinel` and retrieving the host address **dynamically**.

### Step 4: Get the **lambda UUID** from Functions

Use the Functions UI to retrieve the `lambda UUID` of the function that should be invoked. Do so by navigating to the **Deployments** section and opening the "Invoke your Function" screen by clicking "Invoke" on the function. The function's `UUID` is then displayed at the top of the page which opens, beneath your function's name. Note it down.

<img src="img/faas-invokeuuid.png" alt="LivePerson Functions Get UUID" style="width:100%;"/>

### Step 5: Generate OAuth 2.0 token

Together with the retrieved domain of the authorization server and the `client_id` + `client_secret`, we can now generate an access-token to authenticate against our Functions Invocation Gateway.

In the following example, [Postman](https://www.getpostman.com/) will be used to better illustrate how external applications can authenticate against the authorization server and execute an invocation. Feel free to import this ([Postman Collection](https://raw.githubusercontent.com/LivePersonInc/developers-community/master/assets/FaaS.postman_collection.json)) to execute the same steps within your local machine:

<img src="img/faas-postman.png" alt="Postman" style="width:100%;"/>


1. In your request-tab under your function click on **Authorization**

2. Select type OAuth 2.0

3. Click on **Get New Access Token**

4. In the appearing dialog fill out the form:

![](img/faas-token-client-credentials.png)

  1. **Token Name**: The name of the token.

  2. **Grant Type**: Select grant type **Client Credentials**.

  3. **Access Token URL**: The endpoint for the server, which exchanges the authorization code for an access token. For example:
  
      {% raw %}
      `https://{{domain}}/sentinel/api/account/{{accountId}}/app/token?v=2.0`
      {% endraw %}

  4. **Client ID**: the generated `client_id` you received from the [App Installation](https://developers.liveperson.com/guides-le-applications-installing.html) (provided by your LivePerson account team).

  5. **Client Secret**: the generated `client_secret` you received from the [App Installation](https://developers.liveperson.com/guides-le-applications-installing.html) (provided by your LivePerson account team).

  6. **Scope**: leave empty.

  7. **Client Authentication**: Select **body**.

  8. Click on **Request Token** to send the request and retrieve your token.

  9.  On the following screen you can see your generated **Access Token**. You'll use this token to perform the invocation. The access-token will be sent with every Functions invocation.

  10. If you click **Use Token**, the Access Token will be used for your Postman call.

### Step 6: Calling the invocation API directly from your systems

After executing the above steps you should have all data needed to execute calls against Functions from your own systems.

Retrieved data:

  * **AccountId** is available.

  * **LambdaUUID** is available.

  * **client_id + client_secret** is available after App-Installation.

  * **Authorization-Server + Functions - domain** is available.

Using curl you can remotely invoke the function as follows. Note that the `userId` is the only parameter that can be chosen arbitrarily, all other parameters are derived from LivePerson configurations. The payload has conform to expected json.
```curl
curl -X POST \
  'https://{{faasGW_domain}}/api/account/{{accountId}}/lambdas/{{lambdaUUID}}/invoke?v=1&userId={{userId}}' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -H 'Authorization: <YOUR_AUTH_TOKEN>' \
  -d '{
  "timestamp": 0,
  "headers": [],
  "payload": {}
  }'
  ```
  
While calling Functions with the access-token, you should also make sure to request a new token before it expires. You can find out if a token is expired by checking the `exp` field of the JWT. You can enter your access-token into the [JWT debugger](https://jwt.io/#debugger) in order to see how the JWT looks like.
