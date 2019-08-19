---
layout: post
title:      "Sinatra Project - The Process of Creating a Calorie Tracking App"
date:       2019-08-18 10:48:15 -0400
permalink:  sinatra_project_-_the_process_of_creating_a_calorie_tracking_app
---


Notes for myself.
1. Sinatra and this project (why i choose my idea)
2. database and sql
2. Setting things up and routing (typos and typos)
3. validations and showing error codes
4. Debugging and learning a lot
5. reading the documentation before stack overflow (insert that funny meme)
5. Building good habits through this project (git commits non stop, good commit messages, etc)
6. BRB


It's been a while since our very first project, which was on Object Oreintated Ruby. Time has come and now we're onto our second project, this one was on Sinatra, a very powerful open sourced web application library that uses the rack web server interface. Upon starting this project, I already felt a lot more confident in my abilities to pull off a project within the timeframe. Planning for this Sinatra Project wasn't as difficult as the first one, we had to create a web application using the CRUD (create, read, update, and delete) fundamentals utilizing a MVC (model view controller). The main purpose of this app was to be able to track something important to me, so that was a no brainer, I immediately decided on building a meal tracking app to supplement with fitness and weight management. This project felt special to me in a way because food and fitness are my passions and I also can't hide the fact that I've put on a good 15 pounds of blubber in the past year or so. I can potentially use this app I've created to manage my eating habits and lifestyle, and continue building on it as I advance further into the curriculum, which is like killing multiple birds with one stone.

Some of the major concepts we had to utilize in our Sinatra project were SQL and databases. We had to manage a collection of data, that supported storage and manipulation of data. My app was able to check those boxes by having the ability to create accounts as a user, adding meals, and being able to modify them using the CRUD fundamentals. Since Sinatra doesn't supprt databases by itself, we had to use ActiveRecord to do all the magical, behind the scenes stuff to get our app linked up with our databases. ActiveRecord does a lot of the heavy lifting for us, like setting up relationships, and taking care of migrations, and database management. I was able to get my app set up very quickly with a few commands. I called `rake db:create_migration NAME=create_users` to quicky set up my users' table, and `rake db:create_migration NAME=create_meals` for my meals table. This allowed me to quickly jump into adding information to my tables, so I can then say `rake db:migrate` and `rake db:seed` to begin building on my app.

I thought the most difficult part of this project was getting everything set up in a neat and restful manner. The project requirements were to have our app using RESTful routes, and be very fluid. I had a hard time deciding on the names of the routes, especially for my `User` model, it was a battle between `/user`, `/home`, `/main`, and `/index/`, I felt like I spent more time than I should've there. Once I've gotten over the naming roadblock, things felt a lot smoother, until I ran into my first major bug, that was based on a darn typo. I thought I would've learned by my mistakes from all the labs we had to do prior to the project, but nope, I left a few typo errors get the best of me for a few hours. I've learned that routes and forms in a ActiveRecord/Sinatra App is very picky, and you cannot have typos, otherwise the entire app will stop working and make you want to rip your hair out.

After carefully going over my code a few times and ironing out all the typos, it was time to get down to business, and everything was flowing like fresh milk out of a jug. Another requirement for the project was validations for user login attributes, and good user authentication throughout the app. For this part, I resorted to the ActiveRecord documentation, as well as my cohort lead, and various study groups for guidance. Validations on trigger on variations of `create`, `save`, and `update` methods, so I had to strategically place them in my controllers. 
```
 ## validation for account creation
  validates :name, length: { minimum: 2 }, presence: true
  validates :username, presence: true, uniqueness: true
  validates :password_confirmation, presence: true, on: :create
  validates :password, presence: true, length: { in: 6..20 }, confirmation: true, unless: ->(u){ u.password.blank? }
```
The block above was placed in my User class, for validating the creation of user accounts. It made sure users weren't creating accounts with a single letter for a name, and that their username has to be unique, meaning there cannot be multiple users with the username of `gamergurl19`. The password was also validated by making sure the sure can't create an account with a blank password, and that the password has to be entered twice, to avoid typos. I also set the length requirement to be between 6 to 20 letters long. The `unless: ->(u){ u.password.blank? }` is a lambda placed there to prevent errors on updating the user later on, and without that, we would get a `password must be 6-20 length` error every time we updated our user without a password.

You can't have a successful project without debugging the crap out of it. I'm 100% positive that I've spent twice as much time debugging my app, than actually adding features. The process was frustrating, but at the same time, it made me a much better programmer. I quickly fixed some bad programming patterns that I discovered while debugging some errors, like not having enough pseudo-code, or not spacing out each seperation of concerns. There were some times where I've spent amost a full day debugging, but the knowledge gained out of it was certainly worth it. The struggle of getting stuck improved not only my programming skills, but also improved my googling and stackoverflow reading skills as well. 

I had so many encounters where I updated a part of my controller/view, and certain parts of my app would stop working. I got that darn error page engraved into my memory already and I'm pretty sure I've had a weird dream about it recently. `Sinatra does not know this ditty` (wakes up drenched cold sweat). Usually when this error appeared, it was because an method used in my view was being called on something nil. I would just backtrace it into my controller and fix it. One thing I've learned towards the end of my project was to embrace the error page. I discovered that it is possible to click and expand on the errors, and it would usually lead right to the cause. This made debugging much easier towards the final few days, and progress started ramping up.

My app was quickly coming together. I had my RESTful routes all wired up correctly, user input and forms were being validated, and CRUD was working as expected. Users are able to sign in, edit their profile, create and edit meals, and see what other users are adding. All the routes and actions are protected by user authentication, so users can't just edit and hack another user... unless their password was somehow easily guessed. 

Overall, in the past week and a half of working on this project, I've learned a lot and had a lot of fun. The Sinatra project felt much rewarding than the CLI project, since it was more interactive and more visually pleasing to work with. Some tips that I've gathered throughout the process are to have the application side-by-side with text editor, and test after every little update. Always have a mental image of where you are currently at in the controller while navigating the application, this helps debugging tremendously. Also, error messages are not the enemy, treat them as an ally that you secretly hate and good things will happen. I am looking forward to continue building upon this app and improving it as I advance in the curriculum. :D


