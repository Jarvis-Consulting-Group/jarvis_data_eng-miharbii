# Introduction
This learning project is an excellent opportunity for individuals to develop their skills in SQL and relational database management systems (RDBMS) by solving various SQL queries. By working on this project, individuals can gain hands-on experience in using SQL to manipulate data and retrieve valuable insights from large datasets. This project is designed to help individuals understand the fundamentals of RDBMS, such as designing schemas, creating tables, and establishing relationships between different tables.

Moreover, individuals can learn how to use SQL to query and modify data, write complex subqueries, and aggregate data using functions like COUNT(), AVG(), and SUM(). The project also helps individuals understand the importance of data normalization, indexing, and other best practices for efficient data management.

By completing this learning project, individuals can improve their proficiency in SQL and RDBMS and gain practical skills that can be applied in various fields such as data analytics, business intelligence, and software engineering.
# SQL Quries

##### Table Setup (DDL)

###### write SQL DDL statements to create the following tables
```
CREATE TABLE cd.facilities
    (
       facid integer NOT NULL, 
       name character varying(100) NOT NULL, 
       membercost numeric NOT NULL, 
       guestcost numeric NOT NULL, 
       initialoutlay numeric NOT NULL, 
       monthlymaintenance numeric NOT NULL, 
       CONSTRAINT facilities_pk PRIMARY KEY (facid)
    );
```
```
CREATE TABLE cd.members
    (
       memid integer NOT NULL, 
       surname character varying(200) NOT NULL, 
       firstname character varying(200) NOT NULL, 
       address character varying(300) NOT NULL, 
       zipcode integer NOT NULL, 
       telephone character varying(20) NOT NULL, 
       recommendedby integer,
       joindate timestamp NOT NULL,
       CONSTRAINT members_pk PRIMARY KEY (memid),
       CONSTRAINT fk_members_recommendedby FOREIGN KEY (recommendedby)
            REFERENCES cd.members(memid) ON DELETE SET NULL
    );
```
```
CREATE TABLE cd.bookings
    (
       bookid integer NOT NULL, 
       facid integer NOT NULL, 
       memid integer NOT NULL, 
       starttime timestamp NOT NULL,
       slots integer NOT NULL,
       CONSTRAINT bookings_pk PRIMARY KEY (bookid),
       CONSTRAINT fk_bookings_facid FOREIGN KEY (facid) REFERENCES cd.facilities(facid),
       CONSTRAINT fk_bookings_memid FOREIGN KEY (memid) REFERENCES cd.members(memid)
    );
```
##### Modifying Data

###### Question 1: Insert some data into a table - The club is adding a new facility - a spa. We need to add it into the facilities table. Use the following values: facid: 9, Name: 'Spa', membercost: 20, guestcost: 30, initialoutlay: 100000, monthlymaintenance: 800.

###### Questions 2: Insert calculated data into a table - Let's try adding the spa to the facilities table again. This time, though, we want to automatically generate the value for the next facid, rather than specifying it as a constant. Use the following values for everything else: Name: 'Spa', membercost: 20, guestcost: 30, initialoutlay: 100000, monthlymaintenance: 800.

###### Questions 3: Update some existing data - We made a mistake when entering the data for the second tennis court. The initial outlay was 10000 rather than 8000: you need to alter the data to fix the error.

###### Questions 4: Update a row based on the contents of another row - We want to alter the price of the second tennis court so that it costs 10% more than the first one. Try to do this without using constant values for the prices, so that we can reuse the statement if we want to.

###### Questions 5: Delete all bookings - As part of a clearout of our database, we want to delete all bookings from the cd.bookings table. How can we accomplish this?

###### Questions 6: Delete a member from the cd.members table - We want to remove member 37, who has never made a booking, from our database. How can we achieve that?

##### Basics

###### Question 7: Control which rows are retrieved - part 2 - How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.

###### Question 8: Basic string searches - How can you produce a list of all facilities with the word 'Tennis' in their name?

###### Question 9: Matching against multiple possible values - How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.

###### Question 10: Working with dates - How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.

###### Question 11: Combining results from multiple queries - You, for some reason, want a combined list of all surnames and all facility names. Yes, this is a contrived example :-). Produce that list!

##### Joins

###### Question 12: Retrieve the start times of members' bookings - How can you produce a list of the start times for bookings by members named 'David Farrell'?

###### Question 13: Work out the start times of bookings for tennis courts - How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time.

###### Question 14: Produce a list of all members, along with their recommender - How can you output a list of all members, including the individual who recommended them (if any)? Ensure that results are ordered by (surname, firstname).

###### Question 15: Produce a list of all members who have recommended another member - How can you output a list of all members who have recommended another member? Ensure that there are no duplicates in the list, and that results are ordered by (surname, firstname).

###### Question 16: Produce a list of all members, along with their recommender, using no joins. - How can you output a list of all members, including the individual who recommended them (if any), without using any joins? Ensure that there are no duplicates in the list, and that each firstname + surname pairing is formatted as a column and ordered.


