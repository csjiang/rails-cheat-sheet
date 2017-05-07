# Rails cheat sheet 

### ROUTE
(in your browser, where you type in the URL)
- **Template:** `www.your-app.c9.io/controller/action`
    - **example 1:** `www.christine-todo-app.c9.io/list/index`
    - **example 2:** `www.christine-todo-app.c9.io/list/1`
    - **example 3:** `www.christine-todo-app.c9.io/list/6000`

(in your code, at `app/config/routes.rb`)
- **Template:** `get 'controller/something' (=> 'controller#action')` 
   - include the part in the parentheses only if the 'something' in the route (that you actually type in) and the action that you want to handle it have different names. 

    - **example 1:** `get 'list/index' (=> 'list#index')` 
        - we don't need the part with the arrow for this example, since the route matches exactly with the controller + action names! 

    - **example 2:** `get 'list/1' => 'list#first'`
    - **example 3:** `get 'list/:id' => 'list#show'` 
        - Anything in the route that you write with a colon (`:`) in front of it is a 'param', or parameter, and whatever you type in for it (in the browser) will get stored by the application in something called params. 
        - So in `list#show` (the show action of the list controller), you'll be able to access the value of the param 'id' by typing `params['id']`. 

### CONTROLLER & ACTION

(in your code, in the file `app/controllers/controller.rb`)
**Template:**
```
class Controller < ApplicationController
    def action
    	@instance_variable = 'something'
    end
end
```
**example 1:**
```
class ListController < ApplicationController
    def index
    	@my_list_title = 'Chores'
    end
end
```
**example 2:**
```
class ListController < ApplicationController
    def first
    	@id = '1'
    	@my_list_title = 'Homework'
    end
end
```
**example 3:**
```
class ListController < ApplicationController
    def show
    	@id = params['id']
    end
end
```

### VIEW
(in your code: inside the `app/views` folder, inside another folder with the same name as the controller, and in a file with the same name as the action - `app/views/controller/action.html.erb`)
`<h1><%= @instance_variable %>`

(in the browser, when you actually view the page at     `www.your-app.c9.io/controller/action`)
something

**example 1:**
(in your code, at `app/views/list/index.html.erb`)
`<h1><%= @my_list_title %></h1>`

(in the browser, at `your-app.c9.io/list/index`)
Chores

example 2: 
(in your code, at `app/views/list/index.html.erb`)
`<h1><%= @id %></h1>
<h1><%= @my_list_title %></h1>`

(in the browser, at `your-app.c9.io/list/index`)
1
Homework

example 3: 
(in your code, at `app/views/list/show.html.erb`)
`<h1><%= @id %></h1>`

(in the browser, at `your-app.c9.io/list/6000`)
6000

(in the browser, if you go to `your-app.c9.io/list/30498304`)
30498304

## Textual walkthrough of RCAV pattern
When you type in `your-app.c9.io/list/show` into your browser and hit 'enter', the browser makes a 'request' to the server that says, "HEY! CAN YOU GET ME `LIST/SHOW`?"

The first thing that happens on the server is that your router looks inside your `config/routes.rb` file to find out what to do. If there is a path that matches `get 'list/show'`, it looks for the `List` controller. You can imagine the router saying, "HEY! `LIST` CONTROLLER! LOOKS LIKE THIS ONE'S ON YOU! CAN YOU HANDLE THIS FOR ME?"

Then the `List` controller says, 'which action should I use?' and finds that the action requested = `show`. Then it goes to `controllers/list_controller.rb` and runs all the code inside of the `show` action. That means any calculations/storing of instance variables, etc. that happens inside of the `show` action is applied. 

Say we decide to store an instance variable, by writing `@fruit = 'banana'`. Any variables that are 'assigned' (if you think of a variable as a container that stores some value, 'assignment' is just putting something in that container, so here we put the string 'banana' inside the `@fruit` container) are then passed along to the same view that matches the controller and action in the original route. 

The view in `views/list/show.html.erb` can be thought of as a template. The extension 'erb' stands for 'embedded Ruby' and means you can 'embed' (or plug in) Ruby values (including variables!) by using Ruby code inside special tags (`<% %>` and `<%= %>`-- do you remember the difference?). 

The view file is connected to the controller and action with corresponding names (so `views/list/show.html.erb` corresponds to... the `List` controller and the `show` action). Through this special connection, whatever instance variables are created in the `show` action of the List controller are *passed along* to the view, which is located at `views/list/show.html.erb`. 

So what gets displayed? That's a trick question-- we're not at the actual display (or view) that you'd see in your browser yet. We're still on the server side, because your application isn't finished handling the request that came in ("HEY! CAN YOU GET ME `LIST/SHOW`?"). 

Before the server can send back the requested information, it needs to plug in the variables that were assigned when the `List` controller executed its `show` action. So all the Ruby that is embedded into the html.erb template is 'evaluated', and when those values have all been plugged in properly, the server is finally ready to send its response. The stuff it sends back is just a plain HTML file. No embedded Ruby-- that was already evaluated (+ the results were plugged in) on the server. 

So what you end up seeing in your browser, when you visit `your-app.c9.io/list/show`, is that final HTML file that is the result of all the stuff that was evaluated and plugged in on the server! 


