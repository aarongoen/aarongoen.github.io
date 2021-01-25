---
layout: post
title:      "RESTful Routes"
date:       2021-01-25 04:58:29 +0000
permalink:  restful_routes
---


So your mother may ask, "So when I log in to a website, how does the internet know what information to give me?" I would do my best to say, "Well, one part of the picture has to do with something called RESTful Routes."

Basically, when you go online to shop at Amazon you may type something in a little box. The little box is part of a form that gets sent through the internet after you hit return or click submit. Your browser, like Chrome, sends a message; in this case, it's a called a POST request. When you fill out a form and send it, the server, which has a list of routes, looks at the information that was on your URL and the info you sent, and plugs it in to a logic machine called a controller. 

The controller decides what to do with the information; Whatever the response is, be it to render a new page or redirect you to another, or redirect you right back to where you are, routes act as the network between these actions. 

For example, you just bought a new computer, and you're logging in to your Amazon account for the first time on it. Amazon doesn't know it's you until your type in your username and password. When you go to the site and it presents you that form, it does that because your computer makes a GET request to Amazon to ask it to render the form. When you fill it out, you make a POST request back to Amazon. Amazon receives the information and checks to see if you are who you say you are. This is called authentication. Then, it will redirect you to a landing page where you can shop for items. 

Whenever you click on a button, a link, or logout, you're sending a POST request. The server receives that information, performs some logic on it, and sends you a response based on your request. If you forgot your password or have to update it, that's called a PATCH or PUT request. You may be changing your address, a PATCH request, or resetting your password, which may be considered either. In any case, routes help keep the internet traffic flowing in a predictable manner. 



