---
layout: post
title:      "LFG Collab - Online Project Hub Built on React, Redux, and Rails"
date:       2020-03-23 00:36:08 +0000
permalink:  lfg_collab_-_online_project_hub_built_on_react_redux_and_rails
---


After few weeks of practicing React and Redux, it is time to put what I've learned to the test for this final project. This final project is an open-ended assignment where we can build anything we want as long as it incorporates what we've learned through out the curriculum. A few of the guidelines we had to follow were, writing our code in ES6 as much as possible, have at least 5 stateless components, 2 container components, and 3 routes. We also had to implement `react-router` with proper RESTful routing, `Redux` middleware like `thunk`, use of `async` actions to interact with our back end API built on Rails and have some styling. For my project, I decided to build an online project hub for users to find projects and collaborate with each other, called LFG Collab, think of it as a group Tinder for project collaboration. I choose the name LFG Collab because LFG stands for "Looking for group", and this was ideal because this app is for people looking for others to collaborate on projects with. 

LFG Collab uses Ruby on Rails as an API for the back end, and React with Redux for the front end. Setting up the back end was fairly straight forward, models include: `User`, `Project`, `Category`, `Comment`, `Reaction`, and `UserProject`. `Users` has many `projects` through `user_projects`, `Category` has many `projects`, `projects` has many `comments` and `reactions`. 

```ruby
ActiveRecord::Schema.define(version: 2020_03_06_194425) do

  # These are extensions that must be enabled in order to support this database
  enable_extension "plpgsql"

  create_table "categories", force: :cascade do |t|
    t.string "name"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  create_table "comments", force: :cascade do |t|
    t.string "content"
    t.bigint "user_id", null: false
    t.string "commentable_type"
    t.bigint "commentable_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["commentable_type", "commentable_id"], name: "index_comments_on_commentable_type_and_commentable_id"
    t.index ["user_id"], name: "index_comments_on_user_id"
  end

  create_table "projects", force: :cascade do |t|
    t.string "name"
    t.string "description"
    t.integer "owner_id"
    t.boolean "online"
    t.integer "team_size"
    t.bigint "category_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["category_id"], name: "index_projects_on_category_id"
  end

  create_table "reactions", force: :cascade do |t|
    t.integer "reaction_type"
    t.bigint "user_id", null: false
    t.string "reactable_type"
    t.bigint "reactable_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["reactable_type", "reactable_id"], name: "index_reactions_on_reactable_type_and_reactable_id"
    t.index ["user_id"], name: "index_reactions_on_user_id"
  end

  create_table "user_projects", force: :cascade do |t|
    t.bigint "user_id", null: false
    t.bigint "project_id", null: false
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["project_id"], name: "index_user_projects_on_project_id"
    t.index ["user_id"], name: "index_user_projects_on_user_id"
  end

  create_table "users", force: :cascade do |t|
    t.string "name"
    t.string "email"
    t.string "password_digest"
    t.string "city"
    t.string "state"
    t.string "country"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  add_foreign_key "comments", "users"
  add_foreign_key "reactions", "users"
  add_foreign_key "user_projects", "projects"
  add_foreign_key "user_projects", "users"
end
```

Since I wanted to allow users to log in and out, I had a few options while I was setting up the sessions. I initially started with [JSON Web Tokens](https://jwt.io/introduction/) using the [Devise JWT gem](https://github.com/waiting-for-dev/devise-jwt), but that gem had depencency issues and caused a lot of [errors](https://stackoverflow.com/questions/60301010/rails-post-deploy-to-heroku-unable-to-run-console-dbmigrate).  I decided to migrate the app over to using traditional cookies instead. The only downside of this was that it won't be compatable with mobile, which is not a big issue since this is meant to be a web application.

```ruby
class ApplicationController < ActionController::API
  include ActionController::Cookies
  include ActionController::RequestForgeryProtection

  before_action :set_csrf_cookie
  protect_from_forgery with: :exception


  private

  def set_csrf_cookie
    cookies['CSRF-TOKEN'] = form_authenticity_token
  end
end
```

This made it so any non `get` requests from the client required to be sent with a [cookie](https://en.wikipedia.org/wiki/HTTP_cookie), this acted like handshake for data passing from the front end to the back end and vice-versa. The cookie flow looked like this: `App mounts -> fetch cookie token -> save token to redux state -> map token to props using connect() ->  include token for certain fetch requests -> delete token if user logs out -> fetch new token for a new session`. In my front end, I had to include the cookie as a part of the header for the fetch actions. 

```javascript 
export const createComment = (projectId, token, content) => {
  return async dispatch => {
    try {
      const res = await fetch("http://localhost:3001/api/v1/comments", {
        method: "POST",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
          "X-CSRF-TOKEN": token
        },
        body: JSON.stringify({
          comment: {
            content
          },
          projectId
        }),
        credentials: "include"
      });
      if (!res.ok) {
        throw res;
      }
      const data = await res.json();
      dispatch({ type: ADD_COMMENT, payload: data });
    } catch (error) {
      console.log(error.message);
    }
  };
};
```

For the front end, I implemented a few additonal npm packages in addition to the required ones, like `"react-bootstrap"` to handle fluid styling, and `"moment"` to handle date formatting. I wanted this app to be straight forward for users so I kept the nav bar design to a minimum. The links on the bar changes whenever a user logs in, the `NavBar` component is connected to redux so it has access to the `user` store knowing when the user is authorized or not. 

```javascript
renderAuthLinks = () => {
    const { authenticated } = this.props;
    if (authenticated) {
      return (
        <>
          <Nav.Item>
            <Link to="/newproject">
              <li className="nav-routes">New Project</li>
            </Link>
          </Nav.Item>

          <Nav.Item>
            <Link to="/myprojects">
              <li className="nav-routes">My Projects</li>
            </Link>
          </Nav.Item>

          <Nav.Item>
            <Link to="/account">
              <li className="nav-routes">My Profile</li>
            </Link>
          </Nav.Item>

          <Nav.Item>
            <Link to="#">
              <li className="nav-routes" onClick={this.handleLogout}>
                Log Out
              </li>
            </Link>
          </Nav.Item>
        </>
      );
    } else {
      return (
        <>
          <Nav.Item>
            <Link to="/registration">
              <li className="nav-routes">Register</li>
            </Link>
          </Nav.Item>

          <Nav.Item>
            <Link to="#">
              <li className="nav-routes">
                <span onClick={() => this.setState({ showLogin: true })}>
                  Log In
                </span>
                <LogInForm
                  show={this.state.showLogin}
                  onHide={() => this.setState({ showLogin: false })}
                />
              </li>
            </Link>
          </Nav.Item>
        </>
      );
    }
  };
	```

Throughout the front end, I implemented authentication using tools from Redux and React Router. Whenever a user logs in successfully, a dispatch is triggered and the initial state of the `user` goes from 
```javascript 
state = {
    user: {},
    authenticated: false,
    loading: false
  },
	```
	to
	```javascript
	case LOG_IN:
      return {
        ...state,
        user: {
          id: action.payload.id,
          name: action.payload.name,
          email: action.payload.email,
          city: action.payload.city,
          state: action.payload.state,
          country: action.payload.country
        },
        authenticated: !!action.payload.authenticated,
        loading: false
      };
			```
			
Incorporating this with the `Redirect` method from `react-router` meant that users who were not authorized will be redirected to a different page. It would be pointless allowing a logged in user to visit the registration page since they're already registered. 
			
```javascript
  render() {
    const { authenticated } = this.props;

    if (authenticated) {
      return <Redirect to="/" />;
    }
		
		return ( ...registration form)
		
		}
```

The toughest part of this project in my opinion was design and organization of the front end. I tried to keep all the components as organized as possible and the reducers as easily accessiby as possible. When I first started building out the reducers, I had comments nested deep under projects because I figured since they comments belonged to a project, it would be logical to format the reducer that way. This turned out to be more complicated than giving `comments` it's own reducer. 




```javascript

//BEFORE

// case ADD_COMMENT:
    //   return {
    //     ...state,
    //     currentProject: {
    //       ...state.currentProject,
    //       comments: [...state.currentProject.comments, action.payload]
    //     }
    //   };

    // case UPDATE_COMMENT:
    //   const allComments = [...state.currentProject.comments];
    //   console.log(allComments);
    //   const idx = allComments.findIndex(comment => {
    //     console.log(comment);
    //     return comment.id === action.payload.id;
    //   });
    //   console.log(idx);
    //   console.log(state.currentProject.comments[idx]);

    //   return {
    //     ...state,
    //     currentProject: {
    //       ...state.currentProject,
    //       comments: [...state.currentProject.comments],
    //       ...state.currentProject.comments,
    //       [idx]: action.payload
    //     }
    //   };

    // case DELETE_COMMENT:
    //   const comments = state.currentProject.comments.filter(
    //     comment => comment.id !== action.payload
    //   );
    //   return {
    //     ...state,
    //     currentProject: {
    //       ...state.currentProject,
    //       comments: comments
    //     }
    //   }

// AFTER

		const commentsReducer = (state = [], action) => {
  switch (action.type) {
    case LOADING_COMMENTS:
      return {
        ...state,
        loading: true
      };

    case GET_COMMENTS:
      return action.payload;

    case UPDATE_COMMENT:
      return state.map((comment, index) => {
        if (comment.id === action.payload.id) {
          return action.payload;
        }
        return comment;
      });

    case ADD_COMMENT:
      return [...state, action.payload];

    case DELETE_COMMENT:
      const comments = state.filter(comment => comment.id !== action.payload);
      return comments;

    default:
      return state;
  }
}
```

I learned that a good rule of thumb to follow is to never nest too deep since that will make things harder in the long run when more objects are being added. 

At the end, I had a lot of fun building LFG Collab. The early stages were tough because the `jwt-devise` gem had dependency issues and it was tough trying to figure out why my migrations weren't running, but ever since I switched over to the more traditional cookies, things were straight forward. Troubleshooting React and Redux weren't that bad thanks to chrome extensions like `redux-dev-tools`. I am excited to continue adding new features to this project and learning more tools.
		
