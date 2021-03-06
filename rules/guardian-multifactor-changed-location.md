---
gallery: true
short_description: Trigger multifactor authentication when location changed
categories:
- multifactor
- guardian
---

## Multifactor when country changes

This rule is used to trigger multifactor authentication when the current country does not match the country of the previous session

```js
function (user, context, callback) {

  user.app_metadata = user.app_metadata || {}; 
  
  if (user.app_metadata.last_location !== context.request.geoip.country_code) {
    context.multifactor = { 
      provider: 'guardian',
     
      // optional, defaults to true. Set to false to force Guardian authentication every time. 
      // See https://auth0.com/docs/multifactor-authentication/custom#change-the-frequency-of-authentication-requests for details
      allowRememberBrowser: false
    };  
  }
  
  //Set the location context for next time
  user.app_metadata.last_location = context.request.geoip.country_code;
  auth0.users.updateAppMetadata(user.user_id, user.app_metadata)
    .then( function() {
      callback(null, user, context);
    })
    .catch( function(err) {
      callback(err);
    }); 
}
```
