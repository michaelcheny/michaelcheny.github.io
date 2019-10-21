---
layout: post
title:      "My Rails Project - Real Life RPG"
date:       2019-10-21 03:13:07 -0400
permalink:  my_rails_project_-_real_life_rpg
---



This rails project was probably the most intense project we've had so far. The entire process felt like a wild rollercoaster ride with many ups and downs but most important, it created a great learning experience. For this rails project, we had to build a complete, functional, restful, web application using the tools we've learned in the past few months. One of the hardest parts of this projust was the brainstorming process since I couldn't commit to an idea for the first couple of days, so I started stressing because time was ticking down. I started off by building the basic requirements, like users, sessions, omniauth, and the views, then the idea of building something related to role playing games popped into my mind. 

I took inspiration from some classic games I've played while growing up, like Final Fantasy, Runescape, and Chrono Trigger. I thought it would be fun and creative to give a task/goals list some gaming elements to encourage users to get stuff done and have fun doing it. The goal of this project was to model my app like an RPG, where the user has a list of skills which they could level up, users can visit the quest bulletin to pick up quests which grants experience points, users can enter a boss battle to gain something special, and users can check out the highscores list to see how they rank up to other users. 

The initial set up of the app was straight forward with the use of [rails generators](https://guides.rubyonrails.org/command_line.html), and was much quicker than doing it by hand like we did in the Sinatra project. The entire Rails framework made everything a breeze in terms of organization and following the MVC structure. The main struggle I had was trying to decide on how to associate all my models together. In the end, I decided on:
```

ActiveRecord::Schema.define(version: 2019_10_20_220856) do

  create_table "quest_skills", force: :cascade do |t|
    t.integer "points"
    t.integer "quest_id"
    t.integer "skill_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["quest_id"], name: "index_quest_skills_on_quest_id"
    t.index ["skill_id"], name: "index_quest_skills_on_skill_id"
  end

  create_table "quests", force: :cascade do |t|
    t.string "name"
    t.string "description"
    t.integer "difficulty_level"
    t.integer "level_requirement"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  create_table "skills", force: :cascade do |t|
    t.string "name"
    t.string "description"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  create_table "task_skills", force: :cascade do |t|
    t.integer "points"
    t.integer "task_id"
    t.integer "skill_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["skill_id"], name: "index_task_skills_on_skill_id"
    t.index ["task_id"], name: "index_task_skills_on_task_id"
  end

  create_table "tasks", force: :cascade do |t|
    t.string "name"
    t.string "description"
    t.integer "difficulty_level"
    t.integer "priority_level"
    t.integer "user_id"
    t.boolean "completed", default: false
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["user_id"], name: "index_tasks_on_user_id"
  end

  create_table "user_quests", force: :cascade do |t|
    t.boolean "completed", default: false
    t.integer "user_id"
    t.integer "quest_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["quest_id"], name: "index_user_quests_on_quest_id"
    t.index ["user_id"], name: "index_user_quests_on_user_id"
  end

  create_table "user_skills", force: :cascade do |t|
    t.integer "level", default: 1
    t.integer "experience_pts", default: 0
    t.integer "skill_id"
    t.integer "user_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["skill_id"], name: "index_user_skills_on_skill_id"
    t.index ["user_id"], name: "index_user_skills_on_user_id"
  end

  create_table "users", force: :cascade do |t|
    t.string "username"
    t.string "email"
    t.string "password_digest"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.string "google_token"
    t.string "google_refresh_token"
    t.boolean "master", default: false
  end

end
```

I let the join tables hold the experience gained per task/quest and the status of completed. This allowed all the users to have skills that belongs to themselves, and have their own quests, and tasks. 
I took advantage of the helpers folder a lot throughout the app, by rendering different views for the users based on different conditions. Some examples that come to mind was that if a user is under level 50, they are not able to create a new Quest, they must complete tasks, and visit the Quest bulletin board to accept quests made by "Master" players. Once a user reaches level 50, they now unlock the boss battle, which they can then defeat the boss and earn the status of "Master", thus allowing them to create quests. The boss battle part was fairy simple to set up:

```
  # Shows link to boss battle
  def boss_battle_link
    if current_user.total_level >= 50
      render 'layouts/boss_battle_link'
    end
  end
```

The user then had to "defeat" the boss by answering a question, if they got it right, they now gain the status of "Master".

```
response = boss_battle_params[:input].downcase
    if answer == response
      current_user.update(master: true)
      flash[:success] = "Congrats! You've slain the boss and obtained the status of Master!"
      redirect_to dashboard_path
    else
      flash[:error] = "The Jellybean got what he wanted. Please try again."
      redirect_to bossbattle_path
    end
```

And yes, the boss is an evil jelly bean.

Overall, I had more fun struggling and grinding through this project than the first two projects combined. It made me feel like a real programmer, being able to design my own application from the ground up, and choosing how I wanted the app to flow. 


