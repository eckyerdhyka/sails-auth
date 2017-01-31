
``` shell
git clone https://github.com/multunus/sails-auth-example.git
cd sails-auth-example
npm install
sails lift

 
#### Steps

Step 1. Install the following npm modules:

``` shell
npm install sails-generate-auth --save
npm install passport-local --save
npm install passport --save
npm install bcryptjs --save
npm install validator --save
npm install passport-facebook --save
npm install passport-twitter --save
npm install passport-google-oauth --save
npm install passport-linkedin --save
```

`passport-local` module is for local authentication strategy. `passport`, `bcryptjs` and `validator` and dependencies for `passport` and `sails-generate-auth`

Step 2. Now, all you have to do to integrate passport is to run the following command in your application:

``` shell
sails generate auth
```

It automatically generates all the files needed by passport.

Step 3. Add the following routes to `config/routes.rb`

``` js
'get /login': 'AuthController.login',
'get /logout': 'AuthController.logout',
'get /register': 'AuthController.register',

'post /auth/local': 'AuthController.callback',
'post /auth/local/:action': 'AuthController.callback',

'get /auth/:provider': 'AuthController.provider',
'get /auth/:provider/callback': 'AuthController.callback',
'get /auth/:provider/:action': 'AuthController.callback',
```

Step 4. Next, change your `config/bootstrap.js` to load your Passport providers on startup by adding the following line:

``` js
sails.services.passport.loadStrategies();
```

You can see a detailed explanation of the steps until this in [sails-generate-auth](https://github.com/kasperisager/sails-generate-auth/) page.

Step 5. Modify `api/policies/sessionAuth.js` to set the policy for controller actions:

``` js
module.exports = function(req, res, next) {
  if(req.user) return next();
  res.redirect('/login');
};
```

Step 6. Now, make the following changes in `config/policies.js` to apply the `sessionAuth` policy to controllers:

``` js
module.exports.policies = {
  '*': ['passport', 'sessionAuth'],
  'auth': {
    '*': ['passport']
  }
}
``` 

Here you are applying `sessionAuth` policy for all the controller actions except those in `AuthController`. Because auth actions like login, logout and register need to be accessed without logging in.

Now, lift your Sails application and see things in action! 

---