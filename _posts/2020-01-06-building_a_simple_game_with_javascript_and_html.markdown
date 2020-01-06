---
layout: post
title:      "Building a Simple Game with Javascript and HTML"
date:       2020-01-06 12:08:20 +0000
permalink:  building_a_simple_game_with_javascript_and_html
---



After countless hours of browsing programming memes, and putting in sweat and tears into my Javascript project for a while, I can finally understand why a lot of experienced developers say that Javascript is a weird language. It's one of those languages that doesn't like to throw many errors, because doing so may break the frontend or cause hard to find bugs. Compared to more beginner friendly languages like Ruby, learning to debug issues in Javascript is a skill of its own, and one that I had to develop for this project. 

![](https://i.redd.it/2ekr6czdct341.jpg) 

For our Javascript project, we were tasked with building a Single Page Application (SPA) using Javascript, HTML, and CSS for the frontend, and using Ruby on Rails as an API for our backend. Some of the requirements include: 

1) All interactions between the client (frontend) and the server (backend) must be handled Asyncronously (AJAX) in JSON format.

2) Must use Object Oreinted approach in building out the application (JavaScript classes).
 
3) The models in the backend must include at least one `has_many` relationship. `Player has_many Games` 

4) The application must have at least 3 AJAX calls, covering at least 2 out of 4 CRUD functions. Using `fetch`.

The brainstorming process for this project was easier than my previous projects. I knew that JavaScript was a very powerful language, and provided enough tools to develop simple browser games. I was inspired by a game called [Easy Money Snake](http://easymoneysnake.com/), which is a browser based "snake" game with a fun twist. The main character is Kevin Durant, a future NBA hall of famer, who was also the 2014 MVP and a 2 time world chamption, who many considers a "snake" for choosing to switch teams. The game inspired me to develop my own browser based game. I decided I was going to base my game on a very controversial basketball player, James Harden, who is known for his "acting" on the court. He's mostly known for being one of the best offensive players in the league, but his "flopping" and "referee baiting" is usually the topic of conversation. Another game I drew inspiration from was "Flappy Bird", a game where you control an object on the screen, and try to avoid obstacles moving on the screen. I would say Floppy Drop is a blend of elements taken from few games, including one that involved catching ice cream falling from the sky, that was popular over 10 years ago. 

I started off by building the backend with rails using the `--api` flag, `$ rails new my_api --api`. This created a slimmed down directory without all the views and unnessary stuff. I gave the backend two models, Players and Games, Players has many Games, and Games belongs to Player. The Game had score and rating, while Player had name. The frontend had the normal set up, with images and sound bites inside `assets/`. 
```
Javascript-Project/
  backend-api/
    app/
    (...other rails files and folders)
  gameFrontend/
	  assets/
    index.html
    style.css
    index.js
  README.md
	```
	
	(explain html canvas and game class)
	
	(explain mutation observers)
	
	

