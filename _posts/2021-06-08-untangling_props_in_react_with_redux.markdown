---
layout: post
title:      "Untangling Props in React with Redux"
date:       2021-06-08 15:20:43 +0000
permalink:  untangling_props_in_react_with_redux
---


Trying to organize and keep track of state in React can get tricky, especially if you're passing down props to different children and grandchildren. One way to make this process easier is to use React to keep track of state globally.

I built an app which helps church music directors plan music according to liturgical day. So, for example, if the user would like to discover new choral music appropriate to the bible readings for the Sunday before Christmas (also known as Advent 4), they could find suggested pieces which quote or reference those bible readings for that day. The user can add new pieces to that list as well. Here's a simplified version of the schema: ![](https://drive.google.com/file/d/1zag8cy1PJk1ScqAS-9kGdSPkCsZ0U1RZ/view?usp=sharing)


With my components structured like so: [](https://drive.google.com/file/d/1EQRFx8iEu1pnJI2bk7BFta4HEivMLYiB/view?usp=sharing)

