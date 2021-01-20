
# **Introduction**
#### Welcome to Digital Payments Wallet Sandbox
------------------------------------------------------------------------------------------

### A few words about Digital Payments Wallet 
Digital Payments Wallet is the multi-award winning wallet by National Bank of Greece that is used to:
- send money to your friends and make payments and purchases with your phone
- make money transfers between your friends
- pay businesses and professionals

### The API
This API provides a standard RESTful interface that enables a user to
- Perform money transfers to another member or merchant or even make donations by just  **registering as a Member**  to our service and verifying your mobile device as a trusted one. 
- Once the registration step is completed simply  **select the other member**  you wish to  **send money to or request money from or pay**  and pretty much that's it.
> Visit https://developer.nbg.gr/documentation/ibank-Pay-Sandbox-OAuth2-v17
>for the full API documentation
> 
### Real life Use Case Scenarios
**Use Case 1:** Given that you are out with your friends at a restaurant and the waiter doesn't have change for everyone, one friend pays the whole bill. Then he suggests that you can give your share via the myWallet App. 
All you have to do is download myWallet App, make a registration, discover your friends from your contact list and pay them.
In this scenario we will be using the API to register as a member and perform a P2P payment.

#### How do I make a registration?
First of all, we will create our sandbox by making an **HTTP POST** request to the following URL
>https://apis.nbg.gr/sandbox/ibankpay/oauth2/v1.7/sandbox

With a request body:
```json
 {
   "sandbox_id": "5DE11422-D7D5-4727-82FA-0C26F195C66C"
 }
``` 

**Note: Remember to store *sandbox_id* somewhere in your application, because you will need to provide it in as a header in each api call.**

Then, you have to create a sandbox user by making an **HTTP POST** request to the following URL
>https://apis.nbg.gr/sandbox/ibankpay/oauth2/v1.7/sandbox/{sandbox_id}/users

In order to register a member you have to make an **HTTP POST** request to the following URL
>https://apis.nbg.gr/sandbox/ibankpay/oauth2/v1.7/P2P/registerMember

To complete the registration you have to verify the device by making an **HTTP POST** request to the following URL
>https://apis.nbg.gr/sandbox/ibankpay/oauth2/v1.7/P2P/verifyDeviceRegistration
  
#### How do I send money?

Find from your list of contacts the members that use myWallet with an **HTTP POST** request to the following URL
https://apis.nbg.gr/sandbox/ibankpay/oauth2/v1.7/P2P/discoverMemberV2

Then invoke an **HTTP POST** request to the following URL to complete the payment
>https://apis.nbg.gr/sandbox/ibankpay/oauth2/v1.7/P2P/payV2

**Use Case 2:** A user owns a prepaid card or a payband (wearable) and wants to view the card balance and the list of card transactions through his mobile device. The API call sequence should be:
1.  _POST_  **/p2p/askAccountV3**  to check if wallet member already exists
2.  _POST_  **/p2p/registerAccount**  to create a new wallet member, if additional verification is required a verification code will be sent via SMS to register the mobile device (bypass if member already exists)
    
3.  _POST_  **/p2p/registerDevice**  to register the mobile device (bypass if member does not already exist)
    
4.  _POST_  **/p2p/verifyDeviceRegistration**  to complete the mobile device registration using the verification code (if additional verification was required)
    
5.  _POST_  **/p2p/askAccountV3**  to get the wallet member status
    
6.  _POST_  **/p2p/registerAccountFastLogin**  to set a PIN or fingerprint for the fast login feature

7.  Log in again so the token is updated.
8.  _POST_  **/p2p/getAccountsInfo**  to retrieve the card balance
    
9.  _POST_  **/p2p/getProductTransactions**  to retrieve the list of card transactions
    
10.  _POST_  **/p2p/addForeignAccount**  to add an additional prepaid card or payband (wearable), a verification code will be sent via SMS to the card owner
    
11. _POST_  **/p2p/addAccountVerification**  to complete the additional card/payband addition using the verification code
    
12.  _POST_  **/p2p/getAccountsInfo**  to retrieve the card balance for both cards
    
13.  _POST_  **/p2p/getProductTransactions**  to retrieve the list of card transactions for the additional card
----
**Use Case 3:** A user gives consent for an account, registers to wallet, loads an amount to it and makes a payment. The API call sequence should be: 

14.  _POST_  **/p2p/askMemberv2**  to check if wallet member already exists    

15.  _POST_  **/p2p/registerMember**  to create a new wallet member, a verification code will be sent via SMS to register the mobile device 

16. _POST_  **/p2p/verifyDeviceRegistration**  to complete the mobile device registration using the verification code

17.  _POST_  **/p2p/askMemberv2**  to get the wallet member status    

18.  _POST_  **/balance/load**  to load an amount to the wallet

19.  _POST_  **/balance/info**  to get the wallet balance info

20.  _POST_  **/p2b/machinePaymentRequestRegistrationV2**  to create a payment request as a merchant. **This call is invoked by the POS**

21.  _POST_  **/p2b/paymentrequestinfov3**  to get the payment info (should be pending)

22.  _POST_  **/p2p/acceptCreditV2**  to accept the payment

23.  _POST_  **/p2b/machinePaymentRequestInfo**  to complete the payment as a merchant. **This call is invoked by the POS**

24.  _POST_  **/p2b/paymentrequestinfov3**  to get the payment info (should be completed)

---

**Use Case 4:** A registered user sends money to a friend that is registered to a different bank via SEPA Credit Tranfer insant (SCT Inst). The API call sequence should be: 

1.  _POST_  **/p2p/askMemberv2**  to get the wallet member status    

2.  _POST_  **/p2p/payv4**  to get the payment's commission and the contact's info
3.  _POST_  **/p2p/payv4**  to execute the payment
4.  _POST_  **/p2p/remittanceQuery**  to ask for the payment's status (should be completed within 10 seconds). Recurring calls are required.

---
Created by **NBG**. 
See more at https://developer.nbg.gr/