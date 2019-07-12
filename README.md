# Cashport SDK - Typescript
A Typescript SDK to integrate CashPort in your apps. https://cashport.io

# Installing
Include the dependency in your project:
```javascript
npm install cashport-js --save
```

# Getting Started
This section contains all the steps required to integrate *Cashport* in your app.

## 1. Get the API Credentials.

Go to [https://cashport.io/developers](https://cashport.io/developers.html) and apply for the *API Credentials*. 

In the meanwhile you can use this `API_ID` to start developing.
 
`L77MZzEO72ZZSrRg58ysiGvveqFe51rK9lMDXKILD6YJf4lNibacSUx0xr979duv`

> Remember this are just temporal credentials.

## 2. Create an Authorization Request

First of all, you have to include an html element where Cashport will build the custom components:

```html
<div id="cashport-container"></div>
```

Some basic imports and initializations: 

```javascript
const {
    AuthorizationRequest,
    Cashport,
    GrantedAuthorization,
    PaymentRequestFactory,
    PersonalInfoPermission,
    SignTransactionRequestBuilder
} = require('cashport-js');

const appId = 'your-app-id';
let cashport = new Cashport();
```

Then you can enable the Cashport login:

```javascript
let cashport = new Cashport();
let permissions = [PersonalInfoPermission.HANDLE, PersonalInfoPermission.FIRST_NAME, PersonalInfoPermission.LAST_NAME, PersonalInfoPermission.EMAIL];
let authRequest = new AuthorizationRequest(permissions, appId);
let uri = cashport.getAuthorizationURI(authRequest, {
    onDeny: () => {
     // request denied
    },
    onSuccess: (authorization) => {
     // request authorized
    }
});

console.log(uri); // This uri you can QRCode it or redirect user to it in order to login using the Handcash wallet app (if installed)
```

You will get the *Granted Authorization* in the callback.

## 3. Create a Sign Transaction Request
Once the authorization is granted you have all what you need to perform automatic payment requests, in addition to the personal information.

Let's see how to tip to a $handle.

This is the code you need to create a *Sign Transaction Request* and handle the response. 

```javascript
let request = SignTransactionRequestBuilder.useApiId(appId)
        .withCredentials(senderHandle, authToken, channelId)
        .addPayment(PaymentRequestFactory.create().getPayToAddress("aeku", 10000))
        .build();
cashport.sendSignTransactionRequest(request, {
        onStart: () => {
            // Sending request...
        },
        onEnd: () => {
            // The request is done.
        },
        onSuccess: (requestId, transactionId) => {
            // Well done!
        },
        onAuthorizedFundsLimitReached: (requestId) => {
            // Authorized funds limit reached";
        },
        onDeviceNotAvailable: () => {
            // Device not available. The user should check his device connection.
        },
        onInsufficientWalletFunds: (requestId) => {
            // Insufficient funds.
        },
        onTokenExpired: (requestId) => {
            // Token expired. Time to login again.
        },
        onNotAuthorized: (requestId) => {
            // Not authorized, what are you doing?
        },
        onInternalWalletError: (requestId) => {
            // Internal wallet error :(
        },
        onBadRequest: (message, errorCode) => {
            // Bad request. WTF are your doing!?
        },
        onAPICallError: (message) => {
            // API Call error. We did something wrong :$
        }
    });
}
```

**✅ Congrats, you have completed your first authorized transaction!**

# Demo

Try the demo at [try.cashport.io](https://try.cashport.io)

# Next
- Build your first app and start to disrupt your industry! 
- More docs coming soon!

```python
#BringTheOasis
```

This code is based on Handcash Cashport-typescript "cashport-sdk" library. If you appreciate this, please consider a donation to $aeku
