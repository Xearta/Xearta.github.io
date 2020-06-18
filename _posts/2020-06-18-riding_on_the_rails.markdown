---
layout: post
title:      "Riding On The Rails"
date:       2020-06-18 20:17:15 +0000
permalink:  riding_on_the_rails
---

### The Climb
After struggling with parts of Sinatra, I was very eager to learn a framework that made the syntax much easier and I wanted to be a better learner.

Introduce: **Ruby On Rails**. 

Going through the labs, I would take spend extra time to take notes of the things that I learned. I would then put these notes into an application called [Notion](http://notion.so). In which, this [guide](https://drive.google.com/file/d/1dn2QEl-SNKfuU8MTb0SwQtjLf7cGSWmI/view?usp=sharing) was born. I made the guide available to my slack channel at the beginning of project week to see if I could help out in anyway.


This guide made my transition into Rails much, much easier!

### The Ride
Starting out my idea phase of my application, I decided on something I felt was do-able but I could add some stretch goals. So, I created an **Appointment Tracker App**. The `User` would be a Doctor. The app can create patients and appointments for those patients.
###### Relationships
```
User/Physician:
- has_many :appointments
- has_many :patients, :through => :appointments

Patient:
- has_many :appointments
- has_many :users, :through => :appointments

Appointment:
- belongs_to :user
- belongs_to :patient
```

##### User Authentication
After I had my ideas setup and they met the requirements, I used [Devise](https://github.com/heartcombo/devise) to create my users and registration views. **Devise** makes it very easy to generate a user authentication system with very little effort. Then, you hook **OmniAuth** into Devise for 3rd party authentication as well.

*I used my own guide that I created to do this which made it very easy to do!*


##### MVC
Following a login system, I had to make my app relationships come alive. I used the rails **generators** to create my models, views, and controllers. The resource generators will generator model, controller, views, and migrations!
```
rails g resource patient        name age:integer
rails g resource appointment    user_id:integer patient_id:integer appointment_date:datetime
```

##### Routing
After setting up my MVC, I looked at my `routes.rb`. As part of the requirements and restful conventions, I ensured that I had a nested route that made sense. You should be able to make an appointment based on a patient. `/patients/:id/appointments/:id`
```
resources :patients do
    resources :appointments
end
```

###### Strong Params
After my routing, I started on my Strong Params to make sure my attributes were the only things that would be altered. Strong Params make it so attributes cannot be altered unless they are permitted. It makes it so you cannot alter your `balance` on your bank account, for example. I used Strong Params in my both of my controllers but below is the example of my `patient` controller which also has nested attributes for an `appointment`
```
private
def patient_params
    params.require(:patient).permit(:name, :age, :search,
                                        appointments_attributes:
                                        [:appointment_date])
end
```

###### Nested Forms
Now we need a way to input all of this data! Forms in general are amazing in Rails but **Nested Forms** are even cooler! With a nested form and correct Strong Params, you can get many different objects with just one form. Also, you can use the `.build` method to create as many objects/forms as you need. Below, is the nested form that I used for making an `appointment` inside the `#new patient` form.

```
<%= f.fields_for :appointments do |af| %>
        <p><%= af.label :appointment_date, "Appointment Date/Time:"%>
        <%= af.datetime_field :appointment_date, min: Date.today %></p>
<% end %>
```

###### Validations
But, we don't want just any data, do we? Well, no. In comes **ActiveRecord Validations**! Validations are incredibly short macros that protect your forms/databases from *bad* data. There are many types of validations but, I only used a few that I felt were helpful. Below, are the validations I used for the `Patient` model to ensure I would get good data.
```
validates :name, :presence => true,
                     :uniqueness => true,
                     :format => { with: /\D/, message: "%{value} is not valid. Must only contain letters." }
validates :age, :presence => true
```


###### Scope Methods
The final *challenging* bit of the project was a scope method. The hardest part was deciding on how I could use a scope method effectively. I decided that I would create a scope method based on **expired** appointments. (Being after the current time). This was useful a few times in my application when I would check for **Upcoming Appointments** or even crossing out appointments that were old.



    scope :expired, -> { where("appointment_date < ?", (Time.now - 14400)) }
*I had to subtract from the Time.now due to the way that SQLite deals with DateTime*

### The Descent
This project took a bit longer than my Sinatra project *(Which I expected)*. It was also a bit more challenging. However, I enjoyed every bit of it. I even gave myself a few stretch goals to work on and I plan to update the project even more afterwards! I feel that this project really taught me what is possible with **Ruby**! 

Looking forward, I plan to continue to work on this project and create more small Rails apps to keep my knowledge fresh and up-to-date as we progress through the curriculum. I even created a project *'tracker'* to keep track of when I create projects and what frameworks or languages I use. This tracker will ensure that I rotate my languages/frameworks to work on my worst ones.

**Links:**
- [My project](https://github.com/Xearta/rails-appointment-tracker)
- [Notion | What I use for the guide and project tracker](https://notion.so)
- [Devise](https://github.com/heartcombo/devise)
