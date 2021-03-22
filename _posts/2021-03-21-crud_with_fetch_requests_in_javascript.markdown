---
layout: post
title:      "CRUD with Fetch Requests in JavaScript"
date:       2021-03-22 00:31:52 +0000
permalink:  crud_with_fetch_requests_in_javascript
---


**TL;DR

Having been accustomed to the 'convention over configuration' nature of object-oriented programming in Ruby, when I encountered JavaScript I felt like a goldfish might feel getting dropped into the ocean. With all of the freedom of JavaScript, it felt difficult to know where to begin writing my JavaScript project. Thankfully, with a basic understanding of object-orientation in Ruby, an idea of the purpose and workflow of my app, and a broad overview of the possibilities of JavaScript, I began. 

**DOMAIN MODEL

I decided to try my hand at an application that would randomly generate previews of musical scores with links to the full pieces. My intent is to introduce musicians, specifically organists in this version, to works of music they make like to play and learn that may be previously unknown to them. Many musicians make use of a website called [IMSLP](https://imslp.org/wiki/Main_Page) which makes over 177,000 scores available for free. Since many classical musicians may not have access to significant music score libraries, they often find music through IMSLP. It's typical for them to search pages according to composers they know or filter by instrument, but what about discovering hidden gems? Without access to a library to browse through, it may not be possible or may be overwhelming. Enter **Repertoire Builder**. Repertoire Builder is an application I built to help organists discover music they may have not known how to search for by randomizing a search for them. They then can like and comment music they're interested in and ultimately use the application to help them compile recital/concert programs. 

**FRONTEND & BACKEND

Since JavaScript lets us have access to data more quickly than a pure Ruby application, we have the ability to use the clarity of structure of object-orientation and RESTful routes in the backend while enhancing the user experience in the frontend. To communicate between the frontend and backend, JavaScript uses fetch requests. 

**FETCH REQUESTS & CRUD

**** The R(ead) of CRUD****
So, how do we get the information from the Ruby backend to the screen? Introducing the fetch request:
* From your frontend main JavaScript file, often called index.js, call on the backend API with: *
```fetch('baseURL/objects')```
or in my case
```fetch('http://localhost:3000/pieces')```
* Handle the promise, i.e. use:*
```.then(response => response.json())```
This `.then` method allows the frontend to continue processing the code below the `.then` while the data from the backend comes in. This way, the user can see things rendered to the page while waiting for all of the backend data to get to the frontend. The, using the `=>` arrow function, the response gets translated from a string to JSON format with `.json`. 
* Do something with the data, i.e. use*
for each 
``` .then(data => {
            data.forEach(piece => {
                new Piece(piece)
            });
            this.renderPieces();
        })```
This iterates over the data returned (The data doesn't have to be called 'data'. It can be called anything, by the way, like in my domain, `pieces`, for example.). It instantiates frontend objects (called 'instances' in Ruby). Then, with that new array of pieces it creates, it calls the function 'renderPieces()'.
* Include a `.catch`.*
'There's always a catch!' Well, let's hope so! I mean, include a `.catch` method, which can render an error in the browser if there's a problem with the fetch request, i.e. ```.catch(err => alert(err))``` This shows a pop-up message in the browser with the error.

So, now that I was able to retrieve data from the backend, I *just* had to render it to the page. (It was more complicated than I expected but, for our intents and purposes, let's focus on just the fetch requests!) I wanted to the user to be able to like the piece (basically a bookmarker for the piece, not a social media accumulative one-- but if I create a user model, that would be nice!) but that fetch request uses the patch method. Let's hold off on that and talk about the fetch for 'POST' first.


**** The C(reate) of CRUD****

I wanted the user to be able to make comments on the pieces delivered. So I created a function with a fetch "POST"  request. First I needed to add an event listener to the submit button of the comment form which looked like:
used:
```      document.querySelectorAll('.comment-form').forEach((textbox) => {  
            textbox.addEventListener('submit', (e) => {
                e.preventDefault();
                let piece_id = e.target.dataset.pieceId
                let text= e.target.text.value
                const data = {
                    text,
                    piece_id
                };
                submitComment(data);
                e.target.reset()
                })
        }) ```
				
This listens to the submit button of the comment form. At the click (event), this procedure prevents the page from reloading, i.e. `e.preventDefault();`, finds the id of the submit button's (target's) piece. Then it sets the text value of that piece by using the text and newly found piece_id. It sends that information (data) to the submitComment function and clears (`e.target.reset()`) the comment form. 
				
				
``` function submitComment(data) {
            fetch('http://localhost:3000/comments', {
                method: "POST",```
								This sends a post request to the backend at the comments endpoint. It uses data from the 
                headers: {
                    Accept: "application/json",
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(data)
            })
            .then((res) => res.json())
            .then((data) => {
                console.log(data)
                let newComment = new Comment(data);
                // debugger
                appendComment(newComment)
            })
            .catch((error) => {
                console.error("Error:", error)
            })
        }




