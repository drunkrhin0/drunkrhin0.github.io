---
title: Learn How to Use the Twitter API (V1)
date: 2021-05-25 +0800
categories: [Resources]
tags: [twitter-thread, content, resources, twitter, API, postman, http, curl, bearer token, api key, secret key]
toc: true
---

# 1. Apply for a Developer Account

Apply for a developer account on the [Twitter Dev website](https://developer.twitter.com)

# 2. Create a New Project
1. Create a new project with the name of your choice and attach it to your app.
2. Take note of the following (you won't see them again):
   1. API key
   2. Secret key
   3. Bearer token

# 3. Make an HTTP Request

Change your username if desired and add in your bearer token:

```http
curl --request GET 'https://api.twitter.com/2/tweets/search/recent?query=from:drunkrhin0' --header 'Authorization: Bearer $BEARER_TOKEN'
```
# 4. Retrieve the ID of Retweeters

Want to retrieve IDs of people who retweeted something? The additional parameters here are to limit it to 100 and stringify them for ease of use

```http
curl --location --request GET 'https://api.twitter.com/1.1/statuses/retweeters/ids.json?id=1397012765173325825&count=100&stringify_ids=true' \
--header 'Authorization: Bearer YOURBEARERTOKENHERE' \
```
# 5. Convert IDs to Usernames

No one wants to read IDs though. Convert them to usernames.

```http
curl --location --request GET 'https://api.twitter.com/2/users?ids=210362293,721276873754472448,1350804242404167681&user.fields=username' \
--header 'Authorization: Bearer YOURBEARERTOKENHERE' \
```

# 6. Retrieve likes from a Tweet

```http
curl --location --request GET 'https://api.twitter.com/2/tweets/1397012765173325825/liking_users' \
--header 'Authorization: Bearer YOURBEARERTOKENHERE' \
```

# 7. Using Postman

cURL is great... but sometimes it can be a pain. You can also import all of these into [Postman](https://www.postman.com/) which provides you with greater flexibility and a nicer way to look at the screen.

![Postman Screenshot](https://pbs.twimg.com/media/E2N9YdgVEAAIUz2?format=jpg&name=large)