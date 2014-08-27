# Analytics Wrapper

This component is a wrapper around different analytics APIs
So far we support the following:
- Mixpanel
- Localytics
- Flurry

An extra API called "analog" is there for debugging purpose if you wanna see what you are supposed to log.


## Register API

You'll need to register the APIs you wanna use to send logs to them

```javascript
var analytics = require('analytics');
analytics.register(require('mixpanel'), 'your_token', config);
```

## Register static data
You can register static data like userId.
In the worse case, if the API doesn't support it, it will be added to the data you wanna track.

```javascript
analytics.freeze(userId, { age: 21, sex: 'm' });
```

## Track events
Once all your APIs are register you can send an even simply by doing the following
```javascript
analytics.send('player.levelUp', { level: 5 });
```

## Track view change
Some APIs support specifically view changes.
If not it will log as a regular events.
```javascript
analytics.screen('viewName');
```

## Note
The API od Mixpanel has been modified to follow our need.
Saying that, normal usage should work as the original version.