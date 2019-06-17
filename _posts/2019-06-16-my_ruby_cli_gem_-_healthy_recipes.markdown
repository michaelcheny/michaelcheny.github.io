---
layout: post
title:      "My Ruby CLI Gem - Healthy Recipes"
date:       2019-06-16 20:45:22 -0400
permalink:  my_ruby_cli_gem_-_healthy_recipes
---


#### Reflection
Oh boy, this project was was both tough, and fun, but more towards the fun side! After a month of Ruby prework, and another 6 weeks of Ruby fundamentals, I've finally made it to the very first project. It's amazing how fast time went by while trying to learn how to program. It felt like just yesterday, I was reading the `Intro to Ruby` lesson on Learn, and learning about Matz, the creator of the Ruby language. After spending many hours working through labs, and reading lessons, I am finally able to put everything together and build my first application A Ruby CLI gem which scrapes the web using `Nokogiri`.


#### The hardest part of the project - Overcoming Fear
When I first read the project instructions and requirements, I panicked for a bit, thinking that it was going to mentally destroy me. Just the word, project, spooked me out a bit, and knowing theres 15 steps in the instructions didn't help much. But after few minutes of anxiety, I began to collect myself and starting breaking down the project, into smaller steps which made a world of difference! In a way, it was kind of like losing 30 pounds, you can't lose it all overnight, you have to take it step by step and do it the right way. That was the mentality I had while jumping into the project, take it step by step and do it right by planning everything out.

#### Brainstorming and flaking on my initial project
Before settling on my current project, I knew I wanted to do something related to health, fitness, or cooking. I've been training powerlifting and preaching the healthy lifestyle for the past 7 years, so this topic was inevitable for me. It was decided, I was going to scrape a food database website and display the food nutritional information for users to see. 
After finally deciding on `https://www.nutritionix.com/`, I started scraping the API, and getting my pagination set up for it, I thought that everything was going smoothly. Fast forward to 2 days later,**(insert everything is fine while burning meme)** I realized that I was only scraping the first level of the API. All the items I wanted to scrape were available on the first level and the second level just repeated the first level, this was not going to satisfy the multi-layer scrape requirement. Not only that, but it also took forever while loading up the app, since it was scraping **a lot** of foods. 
![](https://i.postimg.cc/wjYbFZY6/cropofmyfirstproject.png)![](https://i.postimg.cc/3Nvbmrp7/Oneeternitylater.jpg)

I even added a progress bar to prevent users from thinking my app froze. At the end of the day, I decided to put this project on hold, and transition over to a back-up project. I will come back to this after project week is over!

#### Healthy Recipes
Back to the drawing board - After another few hours of searching the web for 'healthy foods', I found `http://www.whfoods.com/recipestoc.php`. This was perfect since it was a site with over 500 recipes to scrape and it also promotes eating healthy! **(insert food loving meme)** 

#### The fun begins
This was where things got interesting, I was able to build my own application and have it work exactly the way I wanted! To kick off the building process, I drew up a road map using [pseudo code](https://www.wikihow.com/Write-Pseudocode)  to outline my application. To be honest, prior to the project, I haven't really taken full advantage of writing pseudo code, and I missed out because pseudo code is AMAZING! **(insert spongebob amazed face meme)** After outlining my application, I started with creating a working `environment.rb` file to make sure all the `.rb` files were properly communicating with one another. The next step was to start scraping `http://www.whfoods.com/recipestoc.php`.

#### Nokogiri and scraping
In order to scrape, `http://www.whfoods.com/recipestoc.php`, I had to use a few tools to help me out. Those tools are `open-uri`, `Nokogiri`, and my bestfriend, `pry`. During the scraping process, everything was looking good until I tried scraping and assigning the categories for each recipe. The site was a bit outdated, and the categories had no clickable link that brought me to the recipes within that category, so I had to play around with different ideas trying to figure out what was the best way to scrape the categories. There were 17 categories in total, 10 of them were non-vegan and the other 7 were "meat-less" categories. The 7 meat-less categories also shared the same names as the non-vegan categories so that added more trouble, and required extra steps. To tackle this, I had to collect the category names first, and add the "Vegan" keyword in front of the `11-17th` element: `category_name_container[10..16].each{|container| categories << "Vegan " + container.text}`. Next I had to assign recipes to those categories, and working with an outdated site made it tough. The only thing determining the different categories for the recipes was the CSS selector. ![](https://i.postimg.cc/zB5mJgRW/cssselectorstroublescraping.png)

For this, I had to use either if statements, or a case statement, I settled for the latter, and this worked surprisingly well. So then, I set the variables of  `name`, `category`, `url`, and `animal_friendly` to their corresponding scraped text, while I created the `Recipes` class to initialize with those variables: `Recipes.new(name, url, category, animal_friendly).

#### Following my pseudo code
```
## Greet the user
## Scrape the Recipe names, url, categories
## Launch the main menu
## Show all 17 categories
## Let user select a category by grabbing their input
## From user's selected category, list recipes from that category
## Grab their input again and list out the Recipe

### The flow of my application should look like this:
## greeting > main_category_selection_menu > recipe_selection_menu > display_recipe > go back to whichever menu
```
Once the initial scraping was completed, I went back to my pseudo code, and created a `CLI` class, this was the driver of my application, while the `RecipeScraper` and `Recipes` were the motor and chassis. The first thing I created here was a `def call` method, which got called on as soon as the user launches the application and to do this, I had to add `CLI.new.call` in my bin executable file:
```
#!/usr/bin/env ruby

require_relative "../lib/healthy_recipes.rb"

CLI.new.call
```
After this, I was able to start creating more methods, and thanks to writing pseudo code beforehand, I was never completely zoned out wondering what to do next. Next step was to create a `main_menu` class so user's will be able to choose a category and be shown a list of recipes to choose from. That process wasn't so bad, I created a `get_category_names` method in my `Recipes` class to `collect` all the names of the categories so I would be able to then itererate over them within in my `CLI` class with `each_with_index` and display each category. I did the same for the recipe menu, except this time I passed the user's input as an argument to a `get_recipe_info(input)` method to scrape the second and third level of the website for ingredients, instructions, serving size, and calories. The `display_recipe_info` method also accepted an argument of the recipe, and list out its information. ![](https://i.postimg.cc/0NgXW3gk/Recipedisplay.png)

#### Testing edge cases and control flow
I added a failsafe in my application so that whenever the user enters an invalid input, it would display an error message and show the options again.

TO BE CONTINUED





#### Tips I picked up from this project
- Google Chrome and Mozilla Firefox has a "Copy Selector" button to copy and paste the css selector.
![](https://i.postimg.cc/YCgyhS7x/chromecssselectorcopy.png)

- Give variables very meaningful names.
- Binding.pry is love.
- Always be mentally aware of where you're currently at in your application (know which methods go where and what they return).
- Using "explicit" self can help transition into other languages even though it's not needed in Ruby. 
- [Pseudo code](https://en.wikipedia.org/wiki/Pseudocode), [pesudo code](https://www.wikihow.com/Write-Pseudocode), [pseudo code](https://blog.codinghorror.com/pseudocode-or-code/)
- Run tests very often


### A video of my project
[![](http://img.youtube.com/vi/qmSgfbnx0DY/0.jpg)](http://www.youtube.com/watch?v=qmSgfbnx0DY "Healthy Recipes Ruby CLI Gem")

