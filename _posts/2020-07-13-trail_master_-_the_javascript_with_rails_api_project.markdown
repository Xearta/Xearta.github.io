---
layout: post
title:      "Trail Master - The JavaScript With Rails API Project"
date:       2020-07-13 17:27:33 +0000
permalink:  trail_master_-_the_javascript_with_rails_api_project
---

## The Inspiration
---
**Trail Master** is a **SPA** (Single Page Application) that allows users to create, view, update, and delete hiking trails. The user can mark trails as completed, sort and filter trails, and even comment on the trails! The application utilizes Vanilla JavaScript, HTML, and CSS for the frontend and Ruby on Rails for the backend API.

The inspiration for Trail Master came from my love of the outdoors and nature. I love being able to explore the outdoors and I wanted a way for people to be able to view different trails, mark them as completed, and even comment on them for others to see. 

## Creating The API
---
The first step in this endeavour is to create a backend to host all of the trail data. Well, Ruby on Rails makes it incredibly easy to do so with just one command.

`rails new trail-master-backend --api --database=postgresql`

![](https://media.giphy.com/media/M33UV4NDvkTHa/giphy.gif)

This created a backend with Postgres as the database. I chose Postgres because, I want to be able to host my application on [Heroku](http://heroku.com) for a live demo. After Rails does its magic and creates the API, I used the Rails resource generators to create the Trail and Comment models and created some seed data. But, how do we see this information?

## Rendering The Trails
---
I had to figure out a way to get the data from the backend and display it on the frontend. Luckily, we we have `fetch` with JavaScript. I used `fetch` to send asynchronous AJAX requests to the backend to get all of the trail data, convert them to JSON, turn each trail into a JavaScript object, then render them on the page. *Whew! That's a mouthful!* All of this happens in **milliseconds**. 

1. I had to create a Trail `class` for JavaScript Object-Orientation
```
class Trail {
    constructor(id, name, ...) {
		this.id = id;
		this.name = name;
		...
		Trail.all.push(this);
    }
		 
    //Render Method
    render() {
		return `Rendered Text`
    }
		 
    //Class Method
    static listTrails() {
		Trail.all.forEach(trail => main.innerHTML += trail.render())
    }
}
```
2. I had to send a `GET` `fetch` request to the backend
3. I had to convert the response to JSON
4. I had to iterate through each JSON object and create JavaScript objects
```
fetch("http://localhost:3000/trails")
.then(resp => resp.json())
.then(trails => {

  trails.map(trail => {
	//Iterate over every trail and create a new Trail Object
	//Each trail object gets added to the Trail.all for listing
	
	new Trail(trail.id, trail.name, ...)
  }
}
```
5. I had to display them on the page
```
Trail.listTrails();
```

## Building a Filter System
#### Filter Buttons
---

After completing the requirements for the project, I decided to do a few stretch goals of getting filtering to work. I found a **library** for creating a filter system with animations online called [MixItUp](https://www.kunkalabs.com/mixitup/). After reading through the documentation, I figured this would be a great tool that I could utilize for filtering by **difficulty, distance, and elevation gain**. 

I created the `select` tag with the options and the filter button:
```
<select class="select-filter">
  <option value="difficulty">Difficulty</option>
  <option value="distance">Distance</option>
  <option value="elevation_gain">Elevation Gain</option>
</select>
						
<select class="type-filter">
  <option value="asc">Ascending</option>
  <option value="desc">Descending</option>
</select>

<button type="button" id="filter-btn">Filter</button>
```

I then added a click listener on the button to filter with the **MixItUp** tool. 
```
document.querySelector('#filter-btn').addEventListener('click', () => {
   const selectFilter = document.querySelector('.select-filter').value;
   const typeFilter = document.querySelector('.type-filter').value;

   mixer.sort(`${selectFilter}:${typeFilter}`);
})
```


I also did this with **completed** or **incomplete** to be able to filter the trails by completion status.

#### Search Filter
---
I wanted to have a search bar to be able to filter through the trails to find one that matches the searched name or location. Unfortunately, **MixItUp** doesn't have a search functionality. So, I had to create one from scratch. Not a problem, we have *Google*, after all.

HTML:
```
<div class="filterSearch">
  <input type="text" id="searchInput" placeholder="Search by name or location...">
</div>
```


JS:
```
const filterSearch = () => {
    let filterValue = document.querySelector('#searchInput').value.toUpperCase();
    let cards = document.querySelectorAll('.card');
    
    cards.forEach(card => {
            let name = card.querySelector('h4');
            let location = card.querySelector('.location');

        if (name.textContent.toUpperCase().indexOf(filterValue) > -1 || location.textContent.toUpperCase().indexOf(filterValue) > -1) {
            card.style.display = '';
        } else {
            card.style.display = 'none';
        }
    })
}
```

As you type in the search bar, it will filter away the cards that don't match the query by **name** OR **location**.


## Future Updates
---
The biggest future update that I would love to incorperate would be to have users. I would love to make it so that the completed trails and comments would be user bound. However, adding trails would be global allowing anyone to see all of the available trails. 

Currently, as it stands, the application is only useful locally because anyone can mark trails completed or incomplete. However, I would love to have the application hosted with individual user accounts.


## Links:
---
- [Trail Master Repo](https://github.com/xearta/trail-master)
- [Heroku](http://heroku.com)
- [MixItUp](https://www.kunkalabs.com/mixitup/)

