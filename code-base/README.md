## Caching User Profiles:

- Use Redis to cache user profiles to reduce database queries.
- Example code:

```js
const redis = require("redis");
const client = redis.createClient();

// Function to get a user's profile from Redis cache or database
const getUserProfile = async (userId) => {
  const cachedProfile = await client.get(`user:${userId}`);
  if (cachedProfile) {
    return JSON.parse(cachedProfile);
  } else {
    const userProfile = await fetchUserProfileFromDatabase(userId);
    await client.set(`user:${userId}`, JSON.stringify(userProfile));
    return userProfile;
  }
};
```

## Real-Time Notifications:

- Use Redis Pub-Sub for real-time notifications.
- Example code:

```js
const redis = require("redis");
const publisher = redis.createClient();
const subscriber = redis.createClient();

// Publish a notification
const sendNotification = (userId, message) => {
  const notification = { userId, message };
  publisher.publish("notifications", JSON.stringify(notification));
};

// Subscribe to notifications
subscriber.subscribe("notifications");
subscriber.on("message", (channel, message) => {
  const notification = JSON.parse(message);
  // Handle the notification on the frontend
  console.log("Received notification:", notification);
});
```

## Session Management:

- Store user sessions in Redis for fast access.
- Example code:

```js
const express = require("express");
const session = require("express-session");
const redis = require("redis");
const connectRedis = require("connect-redis");

const RedisStore = connectRedis(session);
const client = redis.createClient();

const app = express();
app.use(
  session({
    store: new RedisStore({ client }),
    secret: "your_secret_key",
    resave: false,
    saveUninitialized: false,
  })
);
```

## Rate Limiting:

- Implement rate limiting using Redis for API endpoints.
- Example code:

```js
const redis = require("redis");
const client = redis.createClient();
const { promisify } = require("util");

// Set a rate limit for an IP address
const setRateLimit = async (ip, limit, interval) => {
  const key = `rate-limit:${ip}`;
  await client.set(key, limit, "EX", interval);
};

// Check if the rate limit is exceeded
const isRateLimitExceeded = async (ip) => {
  const key = `rate-limit:${ip}`;
  const decr = promisify(client.decr).bind(client);
  const currentCount = await decr(key);
  if (currentCount < 0) {
    return true;
  }
  return false;
};
```

## Caching User Profiles:

- Use Redis to cache user profiles to reduce database queries.
- Example code:

```js

```
