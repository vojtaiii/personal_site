---
title: "Get Facebook page posts and images for use in a custom layout"
date: 2021-06-15T21:00:00-04:00
categories:
  - tools
tags:
  - android
  - facebook
---



Suppose you have an Android application where it is desired to see posts of a specific Facebook page. Or it could be just any interface, like a webpage or iOS app. Having the possibility to observe the page feed, the user might get to touch with important news, updates, information, etc.
The most straightforward approach is implementing some web view (a window to some other page) with a link to the page. While this is not very convenient, it also looks terrible.

Here I am going to demonstrate the steps to undertake to retrieve the page's content. Then you can use it to achieve a design similar to the following picture. Do not worry about the language (it's Czech).

![alt text][feed1] | ![alt text][feed2]

Here, I have taken the page's content and displayed it in a design of the particular app. 

**Note:** Make sure you have admin rights to the Facebook page, which feed you want to access.

## Using the Facebook Graph API to obtain the page's content

This section is mostly about formulating HTTP calls to obtain a so-called *long-lived page access token*. The token allows the page's content to be accessed for the scope of several months. This stack overflow thread, [Facebook: permanent Page Access Token?](https://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token?noredirect=1&lq=1), offers an excellent summarization of the process.

### 1. Create a Facebook graph app

* Go to the Graph explorer tool - [https://developers.facebook.com/tools/explorer/](https://developers.facebook.com/tools/explorer/)
* Create a new app. Go to *My Apps* then *Create app* and link the newly created app with the existing Facebook page. **Note:** This new page is just an API interface to your page. 

### 2. Get user short-lived access token

* In the Graph explorer tool, select the created application and generate *User Access Token*. In the following pop-up choose the permissions *pages_show_list*, *pages_read_engagement* and *manage_pages*. 
* Grant access by logging into your Facebook account. 
* Store the short-lived user access token, which appears after clicking *Get access token*.

### 3. Get user long-lived access token

While the short-lived token is valid only in the scope of minutes, long-lived lasts about an hour or so. To obtain it we need *app_id* and *app_secret* keys. These can be found on the setting page of the Graph app. Once you retrieve them, make an HTTPS GET call in the following format:

```
https://graph.facebook.com/v10.0/oauth/access_token?  
    grant_type=fb_exchange_token&          
    client_id={app-id}&
    client_secret={app-secret}&
    fb_exchange_token={your-access-token}
```

Insert the requested values, and you should receive a response like this:

```
{
  "access_token":"{long-lived-user-access-token}",
  "token_type": "bearer",
  "expires_in": 5183944 //The number of seconds until the token expires
}
```

Store the long-lived user access token.

### 4. Get a user ID

With the long-lived user access token, make a GET request to

```
https://graph.facebook.com/v10.0/
me?access_token={long_lived_access_token}
```

In the response, store the *id* field. That is your user ID.

### 5. Get a page long-lived access token

Now we can retrieve the token that grants us the page's content with expiration in the scope of months. Make the following call:

```
https://graph.facebook.com/v10.0/{user-id}/accounts?
  access_token={long-lived-user-access-token}
```

Under the field *access_token* of the response is the page long-lived access token. Store it and test it in the [access token debugger](https://developers.facebook.com/tools/debug/accesstoken). Under *expires*, it should say "never" and *data_expiry* 3 months. 

### 6. Get the page content

For the needs of this tutorial and its sample app, we need the following fields of each of the page's post:
* Date and time when it was created
* The text content (if present)
* The image content (if present)

To retrieve these properties, make the following request (use the page long-lived access token generated in the previous step):

```
https://graph.facebook.com/102046414836971/feed?
fields=created_time,message,full_picture,access_token
&access_token={long-lived page acces token}
```

You see, we specified the desired fields in the request `created_time`, `message`, and `full_picture`. The response then looks like following:

```
{
   "data": [
      {
         "created_time": "2021-06-06T11:08:06+0000",
         "message": "Hey-ho lets go",
         "full_picture": "https://url_link_to_the picture1",
         "id": "long_id_of_the_post1"
      },
      {
         "created_time": "2021-06-06T11:00:35+0000",
         "message": "Best punk bands",
         "full_picture": "https://url_link_to_the picture2",
         "id": "long_id_of_the_post2"
      },
      {
         "created_time": "2021-06-02T15:19:35+0000",
         "message": "Nevermind the bollocs!!!",
         "full_picture": "https://url_link_to_the picture2",
         "id": "long_id_of_the_post2"
      },
      .
      .
      .
```

By default, you should get the last 25 posts on the page's feed. If the picture or message is not present in the post, the corresponding field is missing out from the response. 

[feed1]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/fb_page/feed1.png?raw=true
[feed2]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/fb_page/feed2.png?raw=true



