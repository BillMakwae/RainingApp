# RainingApp
A skeleton of a raining notification app

# UX Perspective
The first thing I want to think about is what is this app for? For a user, their goal is to be notified of incoming rain *before* it happens. To do this, theres a few things I would consider from a UX perspective:
- What is the time range that we would notify the user? Ideally we shouldn't disturb the user unnecessarily (i.e while they're sleeping)
- How precise do we want this notification to be? We could get the user's exact location at every given moment and ping them 1 hour in advance of incoming rain, but that would be battery draining and raises ethical issues regarding user data.
- The most important aspect of this app would be to get the notifcation part down. There's no sense in having a notifying app, if I need to enter the app in order to get the notification.

To answer the questions above, I would approach the problem in this way:
- Users would have the option to select the time range when they want the notification (i.e 7am to 10pm)
- Users would be able to set their location to a specfic city/coordinate, rather than track their location.
- Users would receive a push notification from a server if their location is expected to have rain within the next hour. (This gets around the issue of battery drainage and needless user tracking)

# Technical Perspective
Now that we know what our app is going to do, we can start thinking about its implementation. I personally haven't implemented an app with push notifications so I will document the steps that I would take.

1. For this app, the *real magic* will be happening in the backend. We need to set up a system for keeping track of specific users and their time/location preferences. My specific choice would be Firebase using their [Real-Time Database](https://firebase.google.com/docs/database)
2. We need to have a push notification system, luckily Firebase also provides this service as [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging)
3. Now that those systems are in place, how do we keep track of when the rain is coming? Since this is a relatively simple app that only cares about whether or not a place is raining, we can use [OpenWeatherMap's One Call Api](https://openweathermap.org/api), and feed the location of each user. Our app is relatively small (right now) so the free tier with 1000/api calls per day should suffice.
4. What language/framework will we be using to build the client app? I personally would build this in [Flutter](https://flutter.dev), since it would be relatively quick, and it interplays nicely with Firebase (lots of free code snippets from google). Also, it would be cross platform, and work with iOS as well as Android with only one codebase!

# Pros and Cons
Pros with this implementation:
- If a lot of users are in the same city, we can save money by using only one api call to check the weather. This saves us api calls and keeps us from going over our daily limit too quickly.
- Since we are only doing api calls from the server, we can reduce the total amount of api calls made ( as opposed to each user making their own api calls)
- Since we are only doing push notifications instead of tracking user location, we wont drain the user's battery much.
- Requires less code since you dont have to worry about tracking
- Cross Platform with only one codebase so it is easier to maintain.

Cons with this implementation:
- If the user is not connected to the internet, they might miss the notification. (The device doesn't save weather forecasts)
- Does not handle multiple cities/locations for each user. For people that commute, the weather in their home city may not be the same as the city that they attend school/work
- This only gives a warning an hour in advance (which we have relativly high certainty that it will rain though the Weather Api). While this means it may be more accurate, the user might not have enough time to prepare for the rain (i.e a commuter already left home by the time they get the notification)
- Since it is built in Flutter, newer devs unfamiliar with the frame work would take longer to implement features.
- Relatively low amount of API calls in the free tier of OpenWeather API

# Final Remarks / Improvements
This solution is relatively quick, requires little front end set up, and can be done with no cost (assuming the amount of users is small/located in the same city). It is cross platform and only requires one code base.

If this app was to be solely for iOS, we could use Apple's [WeatherKit](https://developer.apple.com/weatherkit/) instead of openweather api which has a higher api cap. But we would have to build the app in Swift, leaving our android friends behind unless we did a completly seperate implementation for them.


