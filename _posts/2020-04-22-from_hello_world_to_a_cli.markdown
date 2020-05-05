---
layout: post
title:      "From 'Hello World' To A CLI"
date:       2020-04-22 15:14:15 -0400
permalink:  from_hello_world_to_a_cli
---


I came from having prior experience with other programming languages so I was able to smoothly move through the labs. This caused me to feel a bit over confident when it came time for the CLI project. I came upon the project page, read through the project requirements, opened a fresh IDE and stared blankly at it. A sudden realisation hit. I wasn't sure how to actually start my project. 

I struggled to even come up with an idea, let alone an entire application. After pacing around for awhile, I came up with my idea to scrape the New York Times Best Sellers lists and display the list. Then, the user would be able to get more information about the books on the list if they so desire. Great! Now, how do I make this work? Well, I found a '[Scraper Checker](http://https://repl.it/@TheGingertonic/ScraperChecker)' Repl.it that allows you to see if a website is actually scrapable. My web site worked so I was ready to get started!

While messing with my web site I was using, I figured out that the date in the URL changed as I changed the week. This means that I could make my application more dynamic for the user! Below is an example of the date part of my code.

*[CLI.rb] *This part asks the user for a date in a certain format and checks to see if the format is valid with the #valid_date? method (below)
```
message =  "Enter the week you want to see [2015+] (Format: MM-DD-YYYY)"
puts message
date = gets.chomp

# Check if the date is valid in the format we need (MM-DD-YYYY)
until valid_date?(date)
  puts "\nIncorrect date format! Please try again.\n" 
	puts message
  date = gets.chomp
end
# Convert date to the necessary format for scraper
date2 = date.split('-')
```
		
*[CLI.rb]* This part checks to see if the date is valid. This part took some time researching to figure out. ([StackOverflow](https://stackoverflow.com/questions/39368909/ruby-date-format-validation))
```
def valid_date?(string)
    return true if string == ''"

    !!(string.match(/\d{2}-\d{2}-\d{4}/) && Date.strptime(string, '%m-%d-%Y'))
  rescue ArgumentError
    false
  end
```
*[CLI.rb]* This part creates the book objects with the given date. (This also accepts a genre that my application works with too)
```
# Create the book objects (15 books)
BookList::Scraper.get_book_list(date2,genre).each do |b|
  BookList::Book.new_from_webpage(b)
end
```
*[scraper.rb] *This is from the scraper and uses Nokogiri to scrape the page. In the code, you can see the use of the date array that the application generates in the URL. As well, at the end it uses the genre that the application asks for later.
```
def self.get_page(date,genre)
    doc = Nokogiri::HTML(open("https://www.nytimes.com/books/best-sellers/#{date[2]}/#{date[0]}/#{date[1]}/combined-print-and-e-book-#{genre}/"))
end
```

I used Avi's tutorial on the learn page for help on setting up my application. Then I found a sample project on the learn page which was very similar to what I was actually doing. Since programmers are *lazy*, I would use a lot of inspiration from the code on that project for my application. As well, for some of my more advanced features (the date validator), I used my Googling skills to help with those. Thus, my application was formed.

I learned a lot from this project. I learned the importance of Git, the importance of looking this up, and the importance of learning from your mistakes. This project seemed very daunting at first but I was able to complete the project in just a few hours by stepping through each part of the project one at a time. I now feel that I am able to work on more projects with just a blank slate and that feels awesome!


*You can view the rest of my code at my [GitHub](https://github.com/Xearta/book_list_cli).*

