---
layout: post
title: Scraping French Girls
tags: []
tumblr_url: http://jelm.be/post/85634190060/scraping-french-girls
comments: True
---
It’s not what it sounds like.

![French Girls](/images/2014-05-13-scraping-french-girls-1.png)

[French Girls](https://www.frenchgirlsapp.com/) is an iPhone app where strangers anonymously draw portraits based on “selfies” others have posted. Some of the drawings are hilarious, some of them disturbing, some of them extremely racist, and very few are actually really good. But that wasn’t what I was interested in.

<!--more-->

> **TLDR**: How to dissect the backend api of an iOS app, scrape the data and make an endless refreshing selfie wall with face detection.

> **UPDATE 19 May 2014**: It looks like the French Girls App started to use gzip compression in their API communication. I’ll update the code if I find the time, or you can just remove the ‘Accept-Encoding’ header on the requests.

The app gives you a continuous stream of selfies and drawings posted by users. I was more interested in the back-end of this app and in particular how the app itself communicates with it. There’s no login or registration involved when using the app, so it would be interesting to see how they keep track of their users.

First step is to install the app and have a look at the requests it makes and the traffic it generates. Wireshark seems like a good option here, but after some Googling I came out at [Charles](http://www.charlesproxy.com/) (another, more command line way, would be to use [mitmproxy](http://mitmproxy.org/)). This is an HTTP proxy that you can use to sniff web traffic. It can also sign it’s own SSL certificates, so it’s perfect to peek into the HTTPS calls the app makes. Charles is nagware, but you can use it for 30 mins at a time, which is enough for our purpose.

![French Girls](/images/2014-05-13-scraping-french-girls-2.png)

After setting up Charles somewhat like [this](http://blog.cloudfour.com/using-charles-proxy-to-examine-ios-apps/). We can start inspecting the traffic.

![French Girls](/images/2014-05-13-scraping-french-girls-3.png)

The first thing you notice is the heavy tracking in the app, with every move or touch you do in the app, there are at least 5 requests send to different trackers. I’m not sure if this is common in most (iOS) apps, but I’m sure they had their reasons to include this many.

At first glance it seems they’re using an [Amazonaws](http://en.wikipedia.org/wiki/Amazon_Web_Services) store to store their data (the pictures) and they’re also making calls to `api-1-1.frenchgirlsapp.com`. After adding these two domains to the SSL certificates option of the Charles proxy, we can have a closer look at the actual calls being made.

![French Girls](/images/2014-05-13-scraping-french-girls-4.png)

If we look at the GET request, being made during a simple refresh of the picture feed, to `api-1-1.frenchgirlsapp.com`, the path looks like this:  `https://api-1-1.frenchgirlsapp.com/feed/recent/1399820113508/24`. The ‘feed’ and ‘recent’ are pretty self explanatory, the long number looks like [unix time](http://en.wikipedia.org/wiki/Unix_time). If we look at the JSON response, things become a little clearer.

![French Girls](/images/2014-05-13-scraping-french-girls-5.png)

We get 24 objects back from the server that each hold the info for a specific picture and it’s corresponding drawings. There is also a ‘last_evaluated_date’ send, which holds the same date as the last drawing of the last picture in the response. Now it starts to make more sense why there was a unix timestamp send with the request. To make the [infinite scroll](http://en.wiktionary.org/wiki/infinite_scroll) in the app work, the first request includes the current timestamp, and the amount of pictures to return (24). The server sends the pictures back and the timestamp of the last picture. When the user reaches the bottom of the list, a new request is sent, but this time containing the timestamp of the last picture currently being displayed instead of the current time. This way the server can answer with all the next pictures in the list / in time.

![French Girls](/images/2014-05-13-scraping-french-girls-6.png)

If we take a look at the data that the server returns for  a specific picture, we see that every picture has a creation date, a last drawing date, a device_id, a key and a url. The url seems to be the url that is being used in the next request the app makes (the ones to get the picture data). So it uses the api server to get the structure of the data used in the app and the amazonaws servers to store all its pictures. If we copy this url pointing to the AmazonAWS and paste it in a browser [`https://frenchgirlsapp.s3.amazonaws.com/photo/6A68AC62-DE23-4497-95DC-1C5ACE4A9C0C/1399818173447.png`](https://frenchgirlsapp.s3.amazonaws.com/photo/6A68AC62-DE23-4497-95DC-1C5ACE4A9C0C/1399818173447.png), we indeed find the same selfie that was just loaded in the app.

![French Girls](/images/2014-05-13-scraping-french-girls-7.png)

The folder structure on the amazonaws server seems to be `/photo/device_id/timestamp.png`. We now have an answer to how the French Girls app keeps track of it’s users, just via a unique device_id.

![French Girls](/images/2014-05-13-scraping-french-girls-8.png)

This id seems to be pushed to the api server every time the app is started with a post request to `/push/register` which holds the id and a push_token.

It seems that with all this information we could easily make a web version of the French Girls app. I need to train my [node.js](http://nodejs.org/) skills, so let’s try to make a simple request to the api-server with node and see if we can extract the selfie locations from the answer. 


But this seems to give us a 401.

    Notice: Undefined index: Key in /var/app/current/index.php on line 20
	{
	  “error”: {
	    “code”: 401,
	    “message”: “Unauthorized: Please Authorize your API access.”
	  }
	}

Damn, we need a key. Let’s have another look at the request made by the app. The headers conveniently include a ‘Key’ value, let’s try to include that in our request, and let’s add all the other header values too. The more we look like a request from the actual app, the more stealth we are.


 That was easy, this gives us the exact same json output as we saw in Charles. Now we just need to extract the correct urls and serve them as html to a user who connects to our node.js server.





In this code-snipet we created a webserver on port 8888 that serves our ‘index’ variable. The variable gets filled up by the for loop in the request function. This way we build up a very crude and totally non-comform html page while itterating through the json response of the api server. When we visit [http://127.0.0.1:8888/](http://127.0.0.1:8888/):

![French Girls](/images/2014-05-13-scraping-french-girls-9.png)

A Selfies Wall! And you can see the drawings when you mouse-over.
Right now we make a single request for 24 selfies (same amount as the app makes), and then our server serves the links to those pictures (stored on the French Girls amazonaws) in a html form to the requesting browser. Lets make our node script make multiple requests to the api server so we can serve an up to date html file to our users. We’re basically [scraping](http://en.wikipedia.org/wiki/Web_scraping) the French Girls api server. We can use the very convenient [node-wsscraper](https://github.com/davej/node-wsscraper) for this.


We’re using the html ‘refresh’ tag to make the browser refresh the webpage at the same rate that we’re updating our index by requesting the new links from the api server.

Yay! We made an **auto reloading web page of endless selfies!**

So how can we make this even better? Yup, **face detection**. We could implement our face detection ourselves via [opencv](http://opencv.org/), but let’s take the easy way and use another api for that. [Skybiometry](http://skybiometry.com/) is one of the many face detection / recognition providers that also has a free plan. The free plan lets you make 100 calls per hour and 5000 per day, plenty for our little demo. The Skybiometry api is pretty simple, you send them the link to the photo in which you want to detect faces, and they send you a json string back with the detected faces and plenty more info, like where the nose is, if they’re smiling and so on. Have a look in their [docs](http://skybiometry.com/Documentation) for all the details. Since we already have the urls of the pictures we want to check it’s only a matter of sending our scraped url to the Skybiometry api and parsing the json response. There are many ways to then mark the face on our served pictures, but since I’m lazy, let’s take the easy way (the right way would probably be with CSS). Let’s make a transparent png image which marks the right location of the faces based on the json data and overlay this image over the original one with html.


To draw the overlay png we’re using [node-canvas](https://github.com/Automattic/node-canvas) and to serve the saved png files we’re using [node-static](https://github.com/cloudhead/node-static). In the code you can see that our web server now checks if a file is requested, if so it serves the file if it’s available in the ‘/public’ folder (don’t forget to create it). If no file is requested the server still serves what’s stored in our ‘index’ variable. We now also have a ‘makeOverlay’ function, which sends a request to the Biometry api, parses the result and draws up a png if a face has been found and then saves the png in the /public folder. Don’t forget to paste in your Biometry key and secret at the top of the code (you get them after registering). The amount of selfies requested is changed to 4 in this example since it takes a while to detect faces of a large amount of selfies (we would already have loaded a new set of selfies before the result of the old set comes in). Note that in none of these examples we actually download any of the pictures coming from the French Girls app.

![French Girls](/images/2014-05-13-scraping-french-girls-10.png)

This was a great exercise to better understand how iOS and mobile apps work and communicate with their back-end server. This example also proves that node.js is a pretty quick and easy way to develop web applications. Don't take any of the code as an example of good coding practices, most of the code is just thrown together from existing examples.

Note that the developers (and users) of the French Girls app probably won't like it if you use their data (selfies and drawings) in your own web application, so always check with them first before you use this for anything. I'm also not responsible for what you do with this example, obviously.

If you got this far you're probably wondering: ‘So how do I get all the selfies (or drawings) from one specific user?’. Good question! The French Girls app also has a tab where you can see your own selfies, drawings and likes. If we inspect the traffic when we reload this section of the app we don't see requests to the frenchGirls api server but to `dynamodb.us-east-1.amazonaws.com/`. Dynamodb is the NoSQL database service of Amazon coupled to the Amazon storage the app uses to store all its data (selfies). If you look at the content of the request you see that the app requests all entries from a specific device_id (your own). If we could replay this request with a different device_id, we could request the location of all the pictures of a specific user. But luckily for the users of the app, making a request to dynamodb needs to be authenticated by a signature. This signature is based on several different parameters (more info [here](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/MakingHTTPRequests.html)), of which one is a secret access key. We would need this access key to make our own requests. So the individual user data is pretty safe ( … for now, if you feel adventurous have a look [here](http://www.gironda.org/2013/03/03/digging-in-the-vineyard-part-2.html)).

Another way to collect pictures of one specific user would of course be to scrape every picture that appears in the app’s feed and then group them per device_id. But that requires some storage and an actual incentive.

I hope this post was mildly informative, if you have any questions or remarks, don't hesitate to comment or contact me.
