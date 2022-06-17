# OAuth2.0_AzureAD

This is a one of the examples widh AzureAD's OAuth2.0 function instead of using Auth0. See also https://github.com/developer-onizuka/istioAuth0.
Consider my URL (https://onprem.example.com) which I am using as the redirect URI in this repository is a one of APIs which serves subscribers the json as a subscription.You could get some tokens to access some API servers such as https://openweathermap.org/ so that you can start your own SaaS businesses with some public APIs.

# 1. App registration in AureAD


<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_1.png" width="480">
<br>


# 2. Define Redirect URI
A redirect URI, or reply URL, is the location where the authorization server sends the user once the app has been successfully authorized and granted an authorization code or access token.<br>

**(1) Select Authentication**<br>

<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_2.png" width="480">
<br>

**(2) Push add a platform button**<br>
Select Web.<br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_3.png" width="480">
<br>

**(3) Set the Redirect URI**<br>
Please note I am using the Implicit Grant(RFC6749#4.2) of Auth2.0. You might use the Authorization Code Grant of cource(RFC6749#4.1).<br>
See also followings:<br>
> https://www.youtube.com/watch?v=PKPj_MmLq5E <br>
> https://datatracker.ietf.org/doc/html/rfc6749

<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_4.png" width="480">
<br>

# 3. Resource server registration
Register the resource server with Azure AD. In Azure AD, the resource server must also be registered as an "application".
<br><br><br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_15.png" width="640">
<br><br><br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_13.png" width="640">
<br><br><br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_14.png" width="480">
<br><br><br>

# 4. API permission
Applications are authorized to call APIs when they are granted permissions by users/admins as part of the consent process. <br>

**(1) Push add a permission button**<br>
<br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_5.png" width="480">
<br>

**(2) Select Azure Service Management**<br>
Select My API.<br>
<br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_11.png" width="320">
<br>

**(3) Check files.read and add permission**<br>
<br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_12.png" width="480">
<br>

# 5. Get Access Token from AzureAD

**(1) Create the following string from your TenantID, ClientID and URI which you copied above.**
```
https://login.microsoftonline.com/<Your TenantID>/oauth2/v2.0/authorize?client_id=<Your ClientID>&response_type=token&redirect_uri=https%3A%2F%2Fonprem.example.com&state=12345&scope=api%3A%2F%2FXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/file.read
```

**(2) Accept permission request**<br>
You can accept to allow apps to access your API or not thru this window.<br>
If you did it, the access token will be given.<br>

<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_10.png" width="240">
<br>

**(3) Access with the string**<br>
If you access get succeed then, you are brought to a distinct URL like below.<br>
The URL contains the access token of OAuth2.0. Retrive it and keep it as secret.<br><br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_9.png" width="720">
<br>

# 6. Setup istio IngressGateway

```
$ git clone https://github.com/developer-onizuka/OAuth2.0_AzureAD
$ vi azureAD-jwt-onprem.yaml
 You should edit some lines instead of using <yourTenantID> and also edit the email which you use in AzureAD as the authentication.

$ kubectl apply -f azureAD-jwt-onprem.yaml
$ kubectl apply -f nginx-onprem-azureAD.yaml
```

# 7. Access to the Redirect URL
You can access the redirect URL which you specified in the previous step, but you can refer to the steps below:

https://github.com/developer-onizuka/istioAuth0#7-access-with-bearer-token


# 8. Get a token by using C# (App code associated with API)
Consider my URL (https://onprem.example.com) is a one of APIs which serves subscribers the json as a subscription.<br>
You could get some tokens to access some API servers such as https://openweathermap.org/ so that you can start your own SaaS businesses.<br>

> https://github.com/developer-onizuka/What_is_AzureAD#get-a-token-by-using-c
