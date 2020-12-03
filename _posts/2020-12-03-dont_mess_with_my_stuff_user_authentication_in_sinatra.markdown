---
layout: post
title:      "Don't mess with my stuff! User Authentication in Sinatra"
date:       2020-12-03 15:03:41 +0000
permalink:  dont_mess_with_my_stuff_user_authentication_in_sinatra
---

[selfish panda](https://tenor.com/view/dont-touch-panda-gif-8529679)

I have been working on building a web app to help folks request food from a foodbank. It wasn't until I was in my assesment interview that I ran into a few unfortunate blindspots. A user could alter another's request information! In my previous project which had a command line interface, none of the elements of logging in, sessions, or persistence factored in. This time around, I didn't take into account that a user could access other users' orders via the browser. 

So, a little background first: The app, called "Foodbank" has the domain model of one side of a foodbank; The user can make, edit, or delete a food request. (Further down the line, I would like to add donor and donations models.) So, my problem was that once a user had signed up and/ logged in, they could do anything they wanted with anyone's food requests! What to do?

This is where the helper methods `current_user` and `logged_in?` come to the rescue!
[two dogs to the rescue](https://tenor.com/view/dont-touch-panda-gif-8529679)
These handy helpers which live in the Application Controller are ready to jump in to work in the other controllers and in the views. 

The `logged_in?` method checks to see if the user's id is incorporated into the session hash, i.e. `def logged_in? !!session[:user_id]` We use the 'double-bang' operator, i.e. '!!' to create a double negative validation of our question, "Is the user's id not not in the session hash?" Some critical locations for including this method in my app are:
* in the create operation: Only allows the user to create a new food request if they're logged in.
* in the index view: Only allows the user to see food requests if they're logged in.

The `current_user` method searches the User model for the user instance which matches the session's user id, i.e. `def current_user User.find_by(:id => session[:user_id])`. This comes in handy for keeping users' food requests private and separate. When a user wants to see all of and only their food requests, ActiveRecord allows us to call `current_user.food_requests` to find only the food requests that belong to the current user. Moreover, to show details about a particular food request, we can use `current_user.food_requests.find_by(id: params["id"])`. This takes the dynamic part of the `'/food_requests/:id'` route to find the specific food request. It prevents the current user from looking at others' food requests by only searching in the current user's food requests. The edit and patch requests are handled in the same way. If the user tries to access an unauthorized page, they are redirected to a different page with an error message such as, "You must be logged in to edit this request."


