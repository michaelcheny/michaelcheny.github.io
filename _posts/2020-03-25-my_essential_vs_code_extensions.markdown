---
layout: post
title:      "My Essential VS Code Extensions"
date:       2020-03-25 23:37:49 +0000
permalink:  my_essential_vs_code_extensions
---


Visual Studio Code may look overwhelming at first, but once you start to get familiar with it, you'll probably find a lot of handy features that can benefit your programming experience. There are countless numbers of extensions in the VSCode market place, anywhere from extensions to help you take notes, to debuggers for specific languages, or even IDE themes to spice up your environment. While the stock VSCode experience is great, it can be enhanced with a few extensions. A few of my favorite extensions in VSCode are ESLint, Prettier, ES 7 React/Redux/ GraphQL/React-Native snippets, Material Icon Theme, One Monokai theme, Ruby,  Ruby Solargraph, and VSCode Ruby.

ESLint and Prettier are tools to help keep your code consistent and clean. ESLint analyzes your code to quickly find problems and can automatically fix a lot of them. My favorite thing about it is that it is aware of syntax and tells you which line the error came from. 
```javascript
Parsing error: Unexpected token, expected ";"

  47 |   handleKeyPress(target) {
  48 |     if (target.key === "Enter") {
> 49 |       this.handleSubmit();i like turtles
     |                             ^
  50 |     }
  51 |   }
  52 |
```
It shows the error is on line 49.

Prettier is a code formatter that enforces a consistent style. Some people are die-hard semicolon fans and some aren't, but you'll see the occasional inconsistent people using semicolons in some parts of their codes, and not using it in other parts. Prettier helps with keeping everything consistent, and the best part about it is you can fully configure it to your liking. It defaults to formatting on command, but you can also configure it to format on save. 

Ruby, Ruby Solargraph, and VSCode Ruby are great extensions for the Ruby section of the curriculum, they all aid in Ruby language support and debugging. Ruby and Ruby Solargraph comes with the Ruby language server that has features like intellisense, code completetion, and inline documentation. The great thing about these is that it is also fully confirgurable so you can customize it to however you want. The VSCode Ruby has syntax highlighting and snippets that can make progamming in Ruby much more enjoyable. 

```ruby
thing.each do |item|
  puts item # item gets highlighted
end
```

ES7 React/Redux/GraphQL/ React-Native snippets is another handy extension that provides JavaScript and React/Redux snippets in ES7. It makes creating new components an absolute breeze, create the file, type `rce` and hit tab, and it builts out the boilerplate for you.

```javascript
import React, { Component } from 'react'

export class YourFile extends Component {
  render() {
    return (
      <div>
        
      </div>
    )
  }
}

export default YourFile
```

It has snippets for any type of component, `rfce` for functional components and `rafce` for functional components with the arrow function are just a few. My absolute favorite snippet it provides is `rcredux` which takes the pain out of building out Redux connected components.

```javascript
import React, { Component } from 'react'
import { connect } from 'react-redux'

export class NewProjectPage extends Component {
  render() {
    return (
      <div>
        
      </div>
    )
  }
}

const mapStateToProps = (state) => ({
  
})

const mapDispatchToProps = {
  
}

export default connect(mapStateToProps, mapDispatchToProps)(NewProjectPage)
```

These are just a few of the extensions available to choose from, there are thousands of them out there that do different things. If you spend a few minutes on the extension store, you may find something that can potentially change the way you code! You can choose from different themes to suit your mood on different times of the day, to different icon themes, or even fiery explosion effects on each key press. Have fun and give them all a try.
