---
title: "Show Facebook page posts and images in a custom layout on Android"
date: 2021-06-15T21:00:00-04:00
categories:
  - tools
tags:
  - android
---

Suppose you have an Android application where it is desired to see posts of a certain facebook page. Or it coud be just any interface, like a webpage or iOS app. Having the possibility to observe the page feed user might get to touch with important news, updates, information etc.
The most straightforward and simplest approach is just to implement some kind of web view (a window to some other page) with link to the page. While this is not very convenient, it also looks terrible.

Here I am going to demontrate the steps to undertake in order to achieve a design similar to the following picture. Do not worry about the language (its Czech).

![alt text][feed1] | ![alt text][feed2]

We`ve taken the page`s content and displayed it in a design of the particular app. 

There are basically two major aspects when creating such service:
1. Using the Facebook Graph API to obtain the content (platform independent)
2. Processing the API response (covered for Android)

**Note:** Make sure you have admin rights to the facebook page which feed you want to access.

## Using the Facebook Graph API to obtain the page`s content

This section is mostly about how to formulate a proper HTTP calls in order to obtain a so-called *long-lived page access token*. Using the token the page`s content can be accessed for the scope of several months. This stack overflow thread, [facebook: permanent Page Access Token?](https://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token?noredirect=1&lq=1), offers a nice summarization of the process.

### 1. Create a Facebook graph app

* Go to the Graph explorer tool - [https://developers.facebook.com/tools/explorer/](https://developers.facebook.com/tools/explorer/)
* Create a new app. Go to *My Apps* then *Create app* and link the newly created app with the existing facebook page. **Note:** This new page is just a API interface to your page. 

### 2. Get user short-lived access token

* In the Graph explorer tool, select the created application and generate *User Access Token*. In the following pop-up choose the permissions *pages_show_list*, *pages_read_engagement* and *manage_pages*. 
* Grant access by logging into your Facebook account. 
* Store the short-lived user access token which appears after clicking *Get access token*.

## 3. Get user long-lived access token

While the short-lived token is valid only in the scope of minutes, long-lived lasts about an hour or so. To obtain it we need *app_id* and *app_secret* keys. These can be found in the setting page of the Graph app. Once you retrieve them, make a HTTPS GET call in the following format:

```
https://graph.facebook.com/v10.0/oauth/access_token?  
    grant_type=fb_exchange_token&          
    client_id={app-id}&
    client_secret={app-secret}&
    fb_exchange_token={your-access-token}
```

Insert the requested values and you should receive a response like this:

```
{
  "access_token":"{long-lived-user-access-token}",
  "token_type": "bearer",
  "expires_in": 5183944 //The number of seconds until the token expires
}
```

Store the long-lived user access token.

## 4. Get a user ID

With the long-lived user access token make a GET request to

```
https://graph.facebook.com/v2.10/me?access_token={long_lived_access_token}
```

In the response, store the *id* field. That is you user ID.

## 5. Get a page long-lived access token

Now we are able to retrieve the token that grants us content of the page with expiration in the scope of months. Make the following call:

```
https://graph.facebook.com/v10.0/{user-id}/accounts?
  access_token={long-lived-user-access-token}
```

Under the field *access_token* is the page long-lived access token. Store it and test it in the [access token debugger](https://developers.facebook.com/tools/debug/accesstoken). Under *expires* it shoud say "never" and *data_expiry* 3 months. 

[feed1]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/fb_page/feed1.png?raw=true
[feed2]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/fb_page/feed2.png?raw=true

