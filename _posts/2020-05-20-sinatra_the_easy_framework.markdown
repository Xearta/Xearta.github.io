---
layout: post
title:      "Sinatra, The Easy Framework"
date:       2020-05-21 03:03:47 +0000
permalink:  sinatra_the_easy_framework
---

Module two was certainally a rollercoaster stuck on hills! It felt like I was constantly going up and down when going through the lessons and labs. Starting out going downhill with SQL, then that struggle with ORMs, the breeze of ActiveRecords, the association concepts, and then finally finishing off with a great framework, Sinatra. 

I was fortunate enough to have a cohort lead that showed me a gem called [`corneal` ](https://github.com/thebrianemory/corneal) which allowed me to create the entire file structure of my Sinatra app with just one command: `corneal new APP-NAME`. This helped my anxiety compared to my CLI project when I didn't have any idea where to start. Next, I came up with an application idea. I created a Sinatra app that would allow a `User` (Physician) to create appointments for its patients.

### The Process
I found the [[BONUS] Sinatra Project Planning Resources](https://learn.co/tracks/online-software-engineering-structured/sinatra/sinatra-project-mode/bonus-sinatra-project-planning-resources) very helpful when trying to figure out the process of starting out. I copied the steps into a new `NOTES.md` file in my project directory, and started to plan out my projects' tables.

```
User:
has_many :appointments
has_many :patients, :through => :appointments

Patient:
has_many :appointments
has_many :users, :through => :appointments

Appointment:
belongs_to :user
belongs_to :patient
```

I used `corneal` to create my migrations, built my models, ran `rake db:migrate` and then ran `shotgun` to make sure everything was there. Then I started to work on my controllers. I started with my regular `ApplicationController` which uses helper methods to deal with logging in/out and sessions. 

I then stubbed out my 7 restful routes for my `AppointmentsController` and started working on them one at a time. 

Restful Routes: 
`Index, New, Create, Show, Edit, Update, Destroy`

Building a route, then the display....and, Voila! My application worked from for creating appointments but then I had to create my `PatientController`. Luckily, it's really easy to copy and paste and just change a few words around. Thus, the application worked! Well, kind of...

### The Struggles

I was able to sucessfully create my application according to the requirements fairly easily. However, I decided I wanted to stretch myself to do some more things. *(I will still be working on this to make it even better!)* I wanted to sort my appointments by date, display appointments for you and others, depreciate old appointments, increase the ease of use, and even style it better. That's where my stuggles started. Onwards, to Google.

Google has helped me with this project *(and some of my other labs)* tremendously! Googling is a **skill** that, I am proud to say, I am pretty decent at. Let's see how I got some of these things to work:

Sorting appointments by date: (`appointments/index.erb`)
```
<% @appointments = @appointments.sort_by { |appt| appt.appointment_date } %>
```

Displaying appointments for just the current user and everyone: (`appointments/index.erb`). I had to check for each appointment if you are the correct user for "your" appointments.
```
<% @appointments.each do |appointment| %>
  <% if current_user.id == appointment.user_id %>
	...some display code...
	<% end %>
```

Depreciate old appointments: (`appointments/index.erb` & `stylesheets/main.css`). I have my appointments in a table so I had to adjust the `<tr>` class depending on if the appointment was *old* or current. Then I did some css to the old appointment displays.

```
<% if appointment.appointment_date < DateTime.now %>
      <%= "<tr class='passed'>" %>
    <% else %>
        <%= "<tr>" %>
    <%end%>
		
.passed {
  color: #e8e8e8;
  opacity: 60%;
  text-decoration: line-through;
}
```
	
I also just increased the usage of the application by adding `<a>` tags (links) all around. I didn't like the feel of constantly having to hit the back button or typing into the URL.
	
### Thoughts
This project has pushed me more than I originally thought when I read the requirements. Because of my nature, I decided to create some 'stretch goals' before I even started my project. Some of them, I didn't achieve, (required some JS) but, for the most part, I did complete them. I am proud that I was able to complete them and I will constantly work on this application to further improve it's abilities.
	
My app: [Sinatra Appointment Tracker](https://github.com/Xearta/sinatra-appointment-tracker)
