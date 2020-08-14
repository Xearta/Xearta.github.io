---
layout: post
title:      "Flatiron School's Effect On Me"
date:       2020-08-14 12:03:19 -0400
permalink:  flatiron_schools_effect_on_me
---

## Introduction
---
Coming to the conclusion of my journey at Flatiron School, I decided I wanted my last blog post to be about my journey as a student at Flatiron. I wanted to go through my previous projects and blog posts, see how I grew as a software developer, and decide if Flatiron was worth it for me. Each section will talk about the projects and at the bottom there will be links to each of the projects GitHub repos.

I want to preface by saying that I had some knowledge with programming through previous experience in Java, and JavaScript.

## Ruby
---
The first module was basic **Ruby**. With my prior knowledge of programming concepts, I was able to breeze through this section but, I did learn some things along the way. The biggest takeaway from this module for me was the ability to scrape web sites for data. I knew it was possible but, I didn't know that there were `gem`s like `nokogiri` to make it **very** simple to do.

For the final project of this section, I was tasked to create a CLI (Command Line Interface) that would scrape a website and present the user with some information. Here are some snip-its from my project.


```
#=> scraper.rb

# Accept a specified date and genre
# Scrape the specified page based on the date and genre given
def self.get_page(date,genre)
    doc = Nokogiri::HTML(open("https://www.nytimes.com/books/best-sellers/
		#{date[2]}/#{date[0]}/#{date[1]}/combined-print-and-e-book-#{genre}/"))
end

# Use the scraped page from #get_page
def self.get_book_list(date,genre)
    results = self.get_page(date,genre).css("ol.css-12yzwg4 li.css-13y32ub")
end
```
## Sinatra
---
This is really when I got my feet wet! The Sinatra section covers SQL, ORMs, ActiveRecord, and Sinatra. SQL is a database language that is *insanely* easy to learn enough to get by. It can be difficult to learn more advanced features  but, I didn't need any advanced features for any of my projects. ORMs (Object Relational Mapping) and ActiveRecord go hand-in-hand. ActiveRecord is Ruby's version of an ORM and makes it very easy to deal with ORMs in Ruby.

Learning Sinatra was very insightful in understanding how web sites work. I learned about the seven RESTful routes, MVCs, nested forms, sessions and cookies, user authentication, and secure passwords. This section had a lot of big takeaways but, the highlight would be user authentication and secure passwords. Using a gem like `bcrypt`, I was able to hash the passwords of the users and not store them into my database as plain text.

For the final project in the Sinatra section, I created an appointment tracker for doctors to track their appointments with patients. Here is a snip-it from my project where I make the user login to be able to see anything

```
#=> appointments_controller.rb

get '/appointments' do
    redirect_if_not_logged_in
    @appointments = Appointment.all
    erb :'appointments/index'
end
```
```
#=> application_controller.rb

def redirect_if_not_logged_in
    if !logged_in?
        redirect "/login?error=You have to be logged in to do that"
    end
end

def logged_in?
    !!session[:user_id]
end
```

## Rails
---
Ruby on Rails is just another Ruby framework like Sinatra. However, it is **much** larger and more comprehensive. The good thing is, it wasn't very hard at all to transition from Sinatra to Rails. In fact, Rails feels so much easier to use. I learned how to do CRUD actions in Rails, validations, nested forms, layouts and partials, routing, nested routing and 3rd party authentication. My biggest takeaway from Rails would be the validations for the models and the 3rd party authentication.

For my final project in Rails, I re-created my appointment tracker. It may seem redundent but, with the extra features that Rails offers, my project in Rails was able to be on another level compared to the Sinatra version. Here is a snip-it of some of the validations.

```
#=> user.rb (Doctor)
validates :username, :presence => true,
                     :uniqueness => true
										 
#=> patient.rb
validates :name, :presence => true,
                 :uniqueness => true,
                 :format => { with: /\D/, message: "%{value} is not valid. Must only contain letters." }
validates :age, :presence => true
```


## JavaScript with Rails
---
This is when it got really exciting! I knew a bit of JavaScript prior to starting at Flatiron which did come in handy. However, prior knowledge is definetly not necessary! I was surprised that I was able to easily switch into a JavaScript mindset after learning Ruby for the previous three months. In the JavaScript module I learned how to Manipulate the DOM, JS event handling, scope, hoisting, fetch, inheritence, and Rails as an API. The biggest takeaway by far was using `fetch` to communicate with a server.

For my project in the JavaScript and Rails section, I created a Trail Master web site. Trail Master uses Rails as an API for the backend. It allows users to view, create, edit, and delete trails with an asynchronous `fetch` call. Here is a snip-it from my project where I use a `fetch` `GET` request to get all of the trails from the server.

```
//=> index.js
fetch('http://localhost:3000/trails')
    .then(resp => resp.json())
    .then(trails => {
      Trail.all = [];
        new Trail(trail);
      });

//=> trail.js
 constructor(trail) {
    this.id = trail.id;
    this.name = trail.name;
    this.start_location = trail.start_location;
    this.end_location = trail.end_location;
    this.distance = trail.distance;
    this.difficulty = trail.difficulty;
    this.completion_time = trail.completion_time;
    this.elevation_gain = trail.elevation_gain;
    this.image_url = trail.image_url;
    this.completed = trail.completed;
    this.comments = trail.comments;
    Trail.all.push(this);
  }
```

## React and Redux
---
This module should really be split into two seperate modules. I don't think that I had enough time to fully grasp React and Redux within the module time (three weeks). Here is a list of what is covered in each section.

* **React**: NPM, JSX, Components, Props, State, Events in React, Form Handling, Lifecycle Methods, fetch in React, Types of Components, React Router.

* **Redux**: Redux Flow, Store, Reducers, Dispatch, MapStateToProps, MapDispatchToProps, Combine Reducers, Thunk.

This is an **incredible** about of information to learn in just three weeks. Unfortunately, I didn't retain a lot of the information in the labs and had to do a lot of research when it came time to do my project. Also, **React Hooks** and **Context API** were not covered in the curriculum. I believe if React and Redux were split into two seperate modules, you could more easily fit in the materials and explain them in greater details.

The biggest takeaway for me in this section would be learning **JSX** and the **Redux Store**. JSX makes it really easy to work with HTML elements and JavaScript all in one. The Redux Store is a global state for your application in React making it easy to access the state of your application in different components.

For my React and Redux project, I created a full E-commerce web site called Widget Store. Widget Store has a similar layout to Amazon with full user authentication, a shopping cart, checkout, ordering, *fake paying*, and *fake delivering*.
Here is a snip-it from my project where I acess all of the projects from the backend.

```
//=> HomeContainer.js

useEffect(() => {
    dispatch(fetchProducts());
}, [dispatch]);

//=> productActions.js
export const fetchProducts = () => dispatch => {
  fetch(`/products`)
    .then(res => res.json())
    .then(products => dispatch({ type: 'FETCH_PRODUCTS', payload: products }));
};

//=> productReducers.js
export const productListReducer = ( state = { products: [] }, action) => {
  switch (action.type) {
    case 'FETCH_PRODUCTS':
      return {
        products: action.payload,
        filteredProducts: action.payload,
      };
  };
};
```

## Conclusion
---
In conclusion, I would say that Flatiron School was definitely worth it for me! I learned so much about web development that I had no clue about. I felt that the first three modules were explained and structured very well. However, the last two were a bit all over the place. I wish the JavaScript module was designed with more example labs and I wish that the React and Redux module was split into two seperate modules. I feel that the last module has a lot of information in a short amount of time.

My future plans are the continue with web development and continue to build upon the skills I learned. I want to continue to work on React, learn new frameworks and even learn mobile development *(Maybe...React Native)*

## Links
---
* [Ruby Book List CLI](https://github.com/xearta/book_list_cli)
* [Sinatra Appointment Tracker](https://github.com/Xearta/sinatra-appointment-tracker)
* [Rails Appointment Tracker](https://github.com/Xearta/rails-appointment-tracker)
* [JS & Ruby -Trail Master](https://github.com/Xearta/trail-master)
* [React-Redux Widget Store Frontend](https://github.com/Xearta/react-ecommerce-frontend)
* [React-Redux Widget Store Backend](https://github.com/Xearta/react-ecommerce-backend)

