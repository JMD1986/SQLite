#This Project is to get comfortable using SQLite commands to search through a database

##Knowing how to properly look through a database is essential if we intend to deal with large amount of info.

Here are the questions posed to my and how I answered them

####1) How many users are there?

The easiest way to do this is to just type

`
SELECT * FROM users;
`

That just dumps EVERYTHING from the users group. I would not suggest this for huge amounts of data. We will discuss that next.

####2) What are the 5 most expensive items?

If we type in

`
SELECT * FROM items ORDER BY price DESC LIMIT 5;
`

we will get the return

`id|title|category|description|price
25|Small Cotton Gloves|Automotive, Shoes & Beauty|Multi-layered modular service-desk|9984
83|Small Wooden Computer|Health|Re-engineered fault-tolerant adapter|9859
100|Awesome Granite Pants|Toys & Books|Upgradable 24/7 access|9790
40|Sleek Wooden Hat|Music & Baby|Quality-focused heuristic info-mediaries|9390
60|Ergonomic Steel Car|Books & Outdoors|Enterprise-wide secondary firmware|9341`

We went from high to low and limited the results by 5. By limiting the amount we can expect to be returned we can insure we dont get caught loading the entire dataset like we did in the first question. If we have GIANT tables we are wasting processing power by doing that.

edit: I dont know how SQL reads commands and don't know if this is stall basically doing

####3) Whats the cheapest book?

so if we enter the code

`
SELECT * FROM items WHERE category LIKE "book";
`

we won't get anything. If we type in

`
SELECT * FROM items WHERE category LIKE "books";
`

we will get 4 items. But if we type

`
SELECT * FROM items WHERE category LIKE "book%";
`

we get 6 items. Our dataset has some pretty ineffecient labeling and its interfering with our technique. If we had a whole lot of time we could write some code that could fix this lets just deal with the cards we've been dealt.


####4) Who lives at “6439 Zetta Hills, Willmouth, WY”? Do they have another address?

`SELECT * FROM addresses WHERE category LIKE '6439 Zetta Hills';`

this gives us

`
id|user_id|street|city|state|zip
43|40|6439 Zetta Hills|Willmouth|WY|15029
`

we now go look her up based on her used id

`SELECT * FROM users WHERE id LIKE 40;`

and we get

`
id|first_name|last_name|email
40|Corrine|Little|rubie_kovacek@grimes.net
`

####5) Correct Virginie Mitchell’s address to “New York, NY, 10108”.

First I found her ID

`
SELECT * FROM users WHERE first_name LIKE "Virginie";
`

this returns

`
id|first_name|last_name|email
39|Virginie|Mitchell|daisy.crist@altenwerthmonahan.biz
`

So I looked up her address using her user id.



then I changed her Address the following way

`
UPDATE addresses SET city = "New York" WHERE id = "41";

UPDATE addresses SET zip = "10108" WHERE id = "41";

UPDATE addresses SET state = "NY" WHERE id = 41";
`


####6) How much would it cost to buy one of each tool?

We ask for the sum of the prices in the categories that contain the word tool.

`
SELECT sum(price) FROM items WHERE category LIKE "tool%";
`

this gives us

`
sum(price)
24605
`


####7) How many total items did we sell?

We call the sum on the quantity of orders

`
SELECT sum(quantity) FROM orders;
`

this gives us

`
sum(quantity)
2125
`


####8) How much was spent on books?

This one is a bit more involved. We have to determine the id numbers of books. We did that previously with:

`
SELECT * FROM items WHERE category LIKE "book%";
`
this of course gave us

`
id|title|category|description|price
4|Fantastic Steel Chair|Books|Advanced attitude-oriented encryption|9246
21|Fantastic Rubber Shoes|Books|Reverse-engineered modular hierarchy|8904
60|Ergonomic Steel Car|Books & Outdoors|Enterprise-wide secondary firmware|9341
76|Ergonomic Granite Chair|Books|De-engineered bi-directional portal|1496
97|Incredible Granite Computer|Books, Toys & Tools|De-engineered national policy|2377
98|Practical Plastic Hat|Books|Implemented non-volatile model|3056
`

so now we know our user id numbers.

I honestly have no idea how to do this.

####9) Simulate buying an item by inserting a User for yourself and an Order for that User.

