# ALLODS-WEB-LIBRARY

## About
This repository is a rework of the web library of Allods dedicated to all server owners that want a web technology more newest and better than PHP.
We (@paulus-allods) have also reworks the API Core to change from Hessian Protocol to JSON Rest. This will allows an easiest use of the web library to control your game server.

What you'll be able to do ?

- Control Account : Create, Update, Sanctions, Collection Editions Management
- Control Game Server : define maxUsersOnShard limit

More incoming (BillingServer API, GameTool & LogServer and much more) !

## How to start ?
Start by moving the jar files in the `/jars` folder into your `gameServer/server_bin/jars` (make a backup of those files before just in case).
Start wanted server, and you can now use this web library to manipulate your game server.
Now create your `.env` file with servers data variables : 
(this web library save these data into process.env, using dotenv package.)
```dotenv
ACCOUNT_SERVER_HOST=127.0.0.1
ACCOUNT_SERVER_PORT=9337
BILLING_SERVER_HOST=127.0.0.1
BILLING_SERVER_PORT=9336
...
```

## API Collection
Here is the web API Collection of an Allods Game Server, with code examples to get those data with this web library (if not implemented, will shows request form data).

### Account

- **POST** - /createAccount
```javascript
let Account = new Account('Username');
Account.create({
  password,
  accessLevel,
  accountStatus
});
```
- **POST** - /accountDetails
```javascript
let Account = new Account('Username');
Account.get().then(accountData => accountData);
```
- **POST** - /accountStatus
```json
{
  "userName": "userName"
}
```
- **POST** - /accountInfo
```json
{
  "userName": "userName"
}
```
- **POST** - /checkPassword
```javascript
let Account = new Account('Username');
Account.checkPassword('password').then(result => result);
// {"status":"SUCCESS","reason":"OK"} or
// {"status":"FAILED","reason":"wrong password"}
```
- **PUT** - /changePassword
```javascript
let Account = new Account('Username');
Account.changePassword('newPassword').then(result => result);
```
- **PUT** - /accountStatus
```javascript
let Account = new Account('Username');
// Active or Inactive
Account.setStatus('Active').then(result => result);
```
- **PUT** - /baseAccessLevel
```javascript
let Account = new Account('Username');
// User, Master or Developer
Account.setBaseAccess('User').then(result => result);
```
- **PUT** - /currentAccessLevel
```javascript
let Account = new Account('Username');
// User, Master or Developer
Account.setCurrentAccess('User').then(result => result);
```

### Sanction

- **POST** - /sanctions
```javascript
let Account = new Account('Username');
Account.Sanction().get().then(sanctions => sanctions);
// {"status":"SUCCESS","reason":"OK","sanctions":[]}
```
- **PUT** - /sanctions
```javascript
let Account = new Account('Username');
Account.Sanction().set({
  type: 'Ban', // Ban, Silence, PlayerTradeBan, TotalTradeBan
  reason: 'Sanction Reason',
  gmName: 'Game Master Nickname',
  expireTime: 'timestamp'
}).then(result => {
  console.log(result);
  Account.Sanction().get().then(sanctions => sanctions);
});
// {"status":"SUCCESS","reason":"OK"}
// {"status":"SUCCESS","reason":"OK","sanctions":[{"login":"Username","sanctionType":"Ban","isOn":true,"expireTime":1586965256,"reason":"Sanction Reason","gmName":"Game Master Nickname"}]}
```
- **DELETE** - /sanctions
```javascript
// NOTE : this will not hard delete the row in the db. It will updates the sanction with the new values you set below and with the flag column at 0

let Account = new Account('Username');
Account.Sanction().delete({
  type: 'Ban', // Which sanction type to remove ?
  reason: 'Sanction Removal Reason', // Reason of the deletion of the sanction
  gmName: 'Game Master who removed sanction', // name of the GM who delete the sanction
}).then(result => {
  console.log(result);
  Account.Sanction().get().then(sanctions => sanctions);
});
// {"status":"SUCCESS","reason":"OK"}
// {"status":"SUCCESS","reason":"OK","sanctions":[{"login":"Username","sanctionType":"Ban","isOn":false,"expireTime":1586965256,"reason":"Sanction Reason","gmName":"Game Master Nickname"}]}
```

### Collection Editions

... soon