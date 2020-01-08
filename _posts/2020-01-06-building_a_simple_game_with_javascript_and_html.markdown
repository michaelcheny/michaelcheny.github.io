---
layout: post
title:      "Building a Simple Game with Javascript and HTML"
date:       2020-01-06 07:08:21 -0500
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

I started off by building the backend with rails using the `--api` flag, `$ rails new backend_api --api`. This created a slimmed down directory without all the views and unnessary stuff. I gave the backend two models, Players and Games, Players has many Games, and Games belongs to Player. The Game has score and rating, while Player has a name. The backend process was fairly simple for this project, since it was more front-end heavy with JavaScript. The frontend was built with organization and modularity in mind, similar to Rails's MVC. Images, sounds, and fonts are stored inside `assets/` in their own folders, and `styles.css` is stored inside the `style/` folder. I stored all my JavaScript files within the `src/` directory, which has 3 sub directories. `src/adapters` stored all of my "adapters", which dealt with all the fetches that communicated with the backend, `src/components` stored all of my main JavaScript classes like `game.js`, `sounds.js`, `input.js`, and all the character classes, and `src/PageManagers` stores the class that renders the player's name and scores on the page. 


```
Javascript-Project/
  backend-api/
    app/
    (...other rails files and folders)
  gameFrontend/
	  assets/
		src/
    index.html
    style/
  LICENSE
  README.md
	```
	
The idea of Floppy Drop was to create a very simple game with a fun theme which required minimal input from the user. Once the page loads, the player is greeted with a screen asking for their name or initials. Initially, I designed it so it would ask for the player's name after the game ends, like a more traditional arcade game, but asking them prior to playing the game accomplished a few things - it allows me to store their name, so I can greet them by name on the next screen, and it allows me to save them as a `Player` on the backend, so they have the ability to play again without having to re-enter their name. The player will then press `enter` to enter the next menu screen, which shows them how to play. The player can now press `enter` or `click` on the game canvas to begin playing. The player controls the head left or right by using the `left` or `right` arrow keys, and they can press `s` to pause during the game. There are three types of objects that the player will come in contact with, a defender, a charge taker, and a referee. Contact with a defender object means that you flop and earn 100 points, contact with a charge taker results in losing 1 life (hard to pull off a flop against a person taking a charge), and contact with a referee results in +500 points, and an extra life. The referees help the most so I made their spawn rates very low compared to the other objects. 
```
spawnFallingObjects() {
    setInterval(() => {
      if (this.gameState === GAMESTATE.RUNNING) {
        const rand = Math.floor(Math.random() * 100);
        if (rand < 80) {
          const defence = new Defence(this);
          this.defenders.push(defence);
        }
        if (rand < 40) {
          const avoidCharge = new Charge(this);
          this.allCharge.push(avoidCharge);
        }
        if (rand < 2) { // every 500ms, there is less than a 2% chance of a referee spawning
          const ref = new Referee(this);
          this.refs.push(ref);
        }
      }
    }, 500);
  }
	```
	
	
	
	
	
	
	(explain html canvas and game class)
	
	(explain mutation observers)
	
	

