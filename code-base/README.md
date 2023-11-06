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

## Leaderboards:

- Use Redis sorted sets to create leaderboards for games or user rankings.
- Example code:

```js
// Add a user's score to the leaderboard
client.zadd("leaderboard", score, username);

// Get the top N users in the leaderboard
client.zrevrange("leaderboard", 0, N - 1, "WITHSCORES", (err, members) => {
  console.log("Leaderboard:", members);
});
```

## Caching HTML Fragments:

- Store HTML fragments in Redis to cache dynamically generated content.
- Example code:

```js
const cacheKey = "user-profile:1234";
const htmlFragment = generateUserProfileHTML(userId);

// Cache the HTML fragment for a set duration
client.setex(cacheKey, 3600, htmlFragment);

// Retrieve the cached HTML
client.get(cacheKey, (err, cachedHTML) => {
  if (cachedHTML) {
    // Use the cached HTML
    console.log("Cached HTML:", cachedHTML);
  } else {
    // Generate and cache the HTML if it's not found
  }
});
```

## Geospatial Indexing:

- Use Redis geospatial commands to store and query location data.
- Example code:

```js
// Add locations to Redis with latitude and longitude
client.geoadd("locations", [
  13.361389,
  38.115556,
  "Paris",
  15.087269,
  37.502669,
  "Rome",
]);

// Find locations within a specified radius
client.georadius("locations", 13.1, 37.6, 50, "km", (err, locations) => {
  console.log("Locations within 50 km of Paris:", locations);
});
```

## Task Queues with Redis Streams:

- Use Redis Streams to implement task queues for background processing.
- Example code:

```js
const taskStream = "tasks";

// Add a task to the stream
client.xadd(taskStream, "*", ["task", "processImage", "imageId:123"]);

// Retrieve and process tasks from the stream
client.xread("BLOCK", 0, "STREAMS", taskStream, "0", (err, streams) => {
  const tasks = streams[0][1];
  tasks.forEach((task) => {
    // Process the task
    console.log("Processing task:", task);
  });
});
```

## Message Queues:

- Use Redis Pub-Sub to create a publish-subscribe messaging system.
- Example code:

```js
// Publisher
client.publish("channel", "Hello, subscribers!");

// Subscriber
const subscriber = redis.createClient();
subscriber.subscribe("channel");
subscriber.on("message", (channel, message) => {
  console.log(`Received message on channel "${channel}": ${message}`);
});
```

## Caching User Profiles:

- Use Redis to cache user profiles to reduce database queries.
- Example code:

```js

```
