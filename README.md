# OAuth2.0_AzureAD

This is a one of the usages widh AzureAD's OAuth2.0 instead of using Auth0.<br>
See also https://github.com/developer-onizuka/istioAuth0.

# 1. App registration in AureAD


<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_1.png" width="480">
<br>


# 2. Define Redirect URI
A redirect URI, or reply URL, is the location where the authorization server sends the user once the app has been successfully authorized and granted an authorization code or access token.<br>

**(1) Select Authentication**<br>

<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_2.png" width="480">
<br>

**(2) Push add platform button**<br>

<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_3.png" width="480">
<br>

**(3) Set the Redirect URI**<br>

<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_4.png" width="480">
<br>

# 3. API permission
Applications are authorized to call APIs when they are granted permissions by users/admins as part of the consent process. <br>

**(1) Push add a permission button**<br>
<br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_5.png" width="480">
<br>

**(2) Select Azure Service Management**<br>
<br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_6.png" width="480">
<br>

**(3) Check user_impersonation and add permission**<br>
<br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_7.png" width="480">
<br>

**(4) Copy URI**<br>
<br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_8.png" width="480">
<br>

# 4. Get Access Token from AzureAD

**(1) Create the following string from your TenantID, ClientID and URI which you copyed above.**
```
https://login.microsoftonline.com/<Your TenantID>/oauth2/v2.0/authorize?client_id=<Your ClientID>&response_type=token&redirect_uri=https%3A%2F%2Fonprem.example.com&state=12345&scope=https%3A%2F%2Fmanagement.azure.com%2Fuser_impersonation
```

**(2) Access with the string**<br>
If you access get succeed then, you are brought to a distinct URL like below.<br>
The URL contains the access token of OAuth2.0. Retrive it and keep it as secret.<br><br>
<img src="https://github.com/developer-onizuka/OAuth2.0_AzureAD/blob/main/OAuth2.0_AzureAD_9.png" width="720">
<br>

# 5. Setup istio IngressGateway

```
$ git clone https://github.com/developer-onizuka/OAuth2.0_AzureAD
$ vi azureAD-jwt-onprem.yaml
 You should edit some lines instead of using <yourTenantID> and also edit the email which you use in AzureAD as the authentication.

$ kubectl apply -f azureAD-jwt-onprem.yaml
$ kubectl apply -f nginx-onprem-azureAD.yaml
```

# 6. Access to the Redirect URL
You can access the redirect URL which you specified in the previous step, but you can refer to the steps below:

https://github.com/developer-onizuka/istioAuth0#7-access-with-bearer-token
