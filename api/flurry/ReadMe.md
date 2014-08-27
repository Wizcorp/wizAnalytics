#Flurry Analytics

Mobile Web SDK
README SDK version 1.0.0
Welcome to Flurry Analytics! This file contains:

1. Introduction
2. Basic Integration
3. Complete Feature List 4. FAQ

## 1. Introduction
This SDK allows you to track the usage and behavior of your website or web­based application for viewing in the Flurry
Analytics system. It is designed to be as easy as possible with a basic setup complete in under 5 minutes.
This archive should contain these files:
- *ProjectApiKey.txt* : This file contains the name of your project and your project's API key.
- *Analytics­README.pdf* : This file containing instructions on how to use Flurry Analytics. The library is hosted by
Flurry.
- *flurry.js* : The required library containing Flurry's collection and reporting code.

## 2. Basic Integration
Place this snippet at the top of every page you want to track with Flurry Analytics:
```javascript
var flurry = require('flurry');
flurry.init("YOUR_API_KEY")
```
You're done! That's all you need to do to begin receiving basic metric data from your site or web app.
￼￼￼
￼Some data automatically collected includes:
- OS
- UserAgent
- Referral URL
- Session Identification
- Session Length
- Session Paused Length
- Installation Time

## 3. Complete Feature List
__Note: to track user behavior, there must be an active session (see startSession).__

### Tracking Demographics
FlurryAgent.setUserId(userId);
Use setUserId to log the user's assigned ID or username in your system after identifying the user. UserId is a
non­ blank string of characters between 1 and 255 characters. Flurry recommends that you provide a UserId as with some
functionality planned in the future, Flurry Analytics will be able to provide you analytics across the different
platforms where you engage with your users.

### Tracking Location
FlurryAgent.setLocation(latitude, longitude, accuracy);
Use setLocation to set the current GPS location of the user. Flurry will keep only location information from the last
call to this method. Latitude and longitude are double values, whereas accuracy is a float value.

### Tracking User Behavior
FlurryAgent.logEvent(eventName);
Use logEvent to count the number of times certain events happen during a session of your application.
This can be useful for measuring how often users perform various actions, for example. Your application is currently
limited to counting occurrences for 300 different event ids. A session is limited to logging 1000 events and 100 unique
event names. Event names must be non­blank strings with a maximum length of 255 characters. If that limit is reached
and surpassed, some data might go unreported.
FlurryAgent.logEvent(eventName, eventParameters);
Use this version of logEvent to count the number of times certain events happen during a session of your application and
to pass dynamic parameters to be recorded with that event. Event parameters can be passed
in as an object, where the key and value are non­blank string literals with a maximum length of 255 characters.
For example, you could record that a user used your search box tool and also dynamically record which search terms the
user entered. A maximum of 10 event parameters per event is supported.
An example of an object to use with this method could be:
```javascript
var eventParameters = {};
eventParameters[‘search_term’] = “Flurry”;
eventParameters[‘matches_criteria’] = “false”; // all values must be strings
FlurryAgent.logEvent(eventName, eventParameters, true);
```
Use this version of logEvent to start timed event with event parameters. eventParameters can be set to an empty object
‘{}’)
￼
￼FlurryAgent.endTimedEvent(eventName);
Use endTimedEvent to end a timed event before the app exits, otherwise timed events automatically end when the app exits.
FlurryAgent.endTimedEvent(eventName, eventParameters);
You may provide event parameters when ending a timed event.

### Tracking a Session
```javascript
FlurryAgent.startSession(apiKey);
```
Use startSession to start a user session. The API key will be supplied to you in the archive and can be found in
‘ProjectApiKey.txt’. An initial request will be made to the flurry servers indicating that the session was started.
For most integrations this should be called on page load (see section 2).
If the user already has an active session with your application when startSession is called it will be unpaused and
resumed.
```javascript
FlurryAgent.endSession();
```
Use endSession to end the current session. This will attempt to make one last request to the flurry servers with any
data collected since the last request to the flurry servers was made, and end the session. The session is not resumable,
and a subsequent call to startSession would create a new session. This method is not necessary to include, and for most
integrations will not be used.
```javascript
FlurryAgent.pauseSession();
```
Use pauseSession to explicitly pause the current session. This will attempt to make a request to the flurry servers
with any data collected since the last request to the flurry servers was made. The paused session can be resumed by
calling startSession before the session has timed out (see Tracking Inactivity below). For applications that have
specific definitions of what an inactive user, this method can be useful for meeting those requirements.
For most integrations, like endSession above, it can be omitted.
```javascript
FlurryAgent.setRequestInterval(timeInSeconds);
```
Use setRequestInterval to adjust the interval the SDK checks to determine if there is new user activity to report.
The default, and minimum interval is 5 seconds. For instrumenting services that have low network availability, or have
custom requirements (such as data usage) a larger interval might be preferable.

### Assigning App Version
```javascript
FlurryAgent.setAppVersion(appVersion);
```
Use setAppVersion to define the version of the application you are tracking. Flurry can differentiate the usage
statistics based on this identifier. App Versions must be non­blank strings with a maximum length of 255 characters.

### Tracking Application Errors
```javascript
FlurryAgent.logError(errorName, errorMessage, lineNumber);
```
Use logError to log exceptions and/or errors that occur in your app. Flurry will report the first 10 errors that occur
in each session. The errorName and errorMessage parameters are non­blank strings with a maximum length of 255 characters.
Line number is an integer.

### Tracking Inactivity
```javascript
FlurryAgent.setSessionContinueSeconds(seconds);
```
Use setSessionContinueSeconds to set the amount of seconds needed to pass that will initiate an end of the current
session and create a new session when the user returns to the application. The application is considered to be in the
‘active’ state when the user has the browser window/tab in the foreground. The application is considered to be in the
‘pause’ state when the user has anything in the foreground other than the browser window/tab in which the application
is running, or if the session has been explicitly paused (see Tracking a Session). Seconds should be provided as an
integer greater than or equal to 0. The default duration is 5 minutes. If a session has been paused longer than that,
it will not be resumable. A call to startSession after a previous session has timed out will start a new session.


## 4. FAQ

### How large is the Flurry Analytics SDK?
The Flurry javascript library is about 17 KB.

### When does the Flurry Agent send data?
The Flurry Agent will send data to Flurry servers when the session starts and send session fragments periodically every
30 seconds as new session data is collected.

### How much data does the Agent send each session?
The total amount of data will vary depending on how much data the developer chooses to log, but an average case is 3 KB
per session.

### What data does the Agent send?
The data sent by the Flurry Agent includes time stamps, logged events, logged errors, and various browser specific
information. This is the same information that can be seen in the custom event logs in the Event Analytics section.
We do not collect personally identifiable information.

### How do we see the OS and Browser types in developerPortal?
To capture that information, you can detect your browser or OS and log them as events with parameters. For example:
```javascript
var env = {};
env[‘os’] = “iOS”; // determine the OS
env[‘browser’] = “Safari”; // determine the browser

FlurryAgent.logEvent(“environment”, env); // save the OS and Browser
```
-----------------------------------------------------------------------------------------------------------------------
You will be able to see the parameter distribution of OS and Browser for the “environment” event in the Events section
of your application’s Analytics.
Please let us know if you have any questions. If you need any help, just email support@flurry.com!
Cheers,
The Flurry Team http://www.flurry.com support@flurry.com
￼￼