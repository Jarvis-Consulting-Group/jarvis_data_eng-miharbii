# Introduction
This learning project is an excellent opportunity for individuals to develop their skills in SQL and relational database management systems (RDBMS) by solving various SQL queries. By working on this project, individuals can gain hands-on experience in using SQL to manipulate data and retrieve valuable insights from large datasets. This project is designed to help individuals understand the fundamentals of RDBMS, such as designing schemas, creating tables, and establishing relationships between different tables.

Moreover, individuals can learn how to use SQL to query and modify data, write complex subqueries, and aggregate data using functions like COUNT(), AVG(), and SUM(). The project also helps individuals understand the importance of data normalization, indexing, and other best practices for efficient data management.

By completing this learning project, individuals can improve their proficiency in SQL and RDBMS and gain practical skills that can be applied in various fields such as data analytics, business intelligence, and software engineering.
# SQL Quries

##### Table Setup (DDL)

###### write SQL DDL statements to create the following tables
![Untitled](./DDL_image.png)
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
```
insert into cd.facilities (
  facid, name, membercost, guestcost, 
  initialoutlay, monthlymaintenance
) 
values 
  (9, 'Spa', 20, 30, 100000, 800);
```
###### Questions 2: Insert calculated data into a table - Let's try adding the spa to the facilities table again. This time, though, we want to automatically generate the value for the next facid, rather than specifying it as a constant. Use the following values for everything else: Name: 'Spa', membercost: 20, guestcost: 30, initialoutlay: 100000, monthlymaintenance: 800.
```
insert into cd.facilities (
  facid, name, membercost, guestcost, 
  initialoutlay, monthlymaintenance
) 
select 
  (
    select 
      max(facid) 
    from 
      cd.facilities
  )+ 1, 
  'Spa', 
  20, 
  30, 
  100000, 
  800;
```
###### Questions 3: Update some existing data - We made a mistake when entering the data for the second tennis court. The initial outlay was 10000 rather than 8000: you need to alter the data to fix the error.
```
UPDATE 
  cd.facilities 
SET 
  initialoutlay = 10000 
WHERE 
  facid = 1;
```
###### Questions 4: Update a row based on the contents of another row - We want to alter the price of the second tennis court so that it costs 10% more than the first one. Try to do this without using constant values for the prices, so that we can reuse the statement if we want to.
```update 
  cd.facilities facs 
set 
  membercost = (
    select 
      membercost * 1.1 
    from 
      cd.facilities 
    where 
      facid = 0
  ), 
  guestcost = (
    select 
      guestcost * 1.1 
    from 
      cd.facilities 
    where 
      facid = 0
  ) 
where 
  facs.facid = 1;
 ```
###### Questions 5: Delete all bookings - As part of a clearout of our database, we want to delete all bookings from the cd.bookings table. How can we accomplish this?
```
delete from 
  cd.bookings;
```
###### Questions 6: Delete a member from the cd.members table - We want to remove member 37, who has never made a booking, from our database. How can we achieve that?
```
delete from 
  cd.members 
where 
  memid = 37;
```
##### Basics

###### Question 7: Control which rows are retrieved - part 2 - How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.
```
select 
  facid, 
  name, 
  membercost, 
  monthlymaintenance 
from 
  cd.facilities 
where 
  membercost > 0 
  and (
    membercost < monthlymaintenance / 50.0
  );
  ```
###### Question 8: Basic string searches - How can you produce a list of all facilities with the word 'Tennis' in their name?
```
select 
  * 
from 
  cd.facilities 
where 
  name like '%Tennis%';
  ```

###### Question 9: Matching against multiple possible values - How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.
```select 
  * 
from 
  cd.facilities 
where 
  facid in (1, 5);
  ```
###### Question 10: Working with dates - How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.
```
select 
  memid, 
  surname, 
  firstname, 
  joindate 
from 
  cd.members 
where 
  joindate >= '2012-09-01';
  ```
###### Question 11: Combining results from multiple queries - You, for some reason, want a combined list of all surnames and all facility names. Yes, this is a contrived example :-). Produce that list!
```
select 
  surname 
from 
  cd.members 
union 
select 
  name 
from 
  cd.facilities;
```
##### Joins

###### Question 12: Retrieve the start times of members' bookings - How can you produce a list of the start times for bookings by members named 'David Farrell'?
```
select 
  bks.starttime 
from 
  cd.bookings bks 
  inner join cd.members mems on mems.memid = bks.memid 
where 
  mems.firstname = 'David' 
  and mems.surname = 'Farrell';
  ```
###### Question 13: Work out the start times of bookings for tennis courts - How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time.
```
select 
  bks.starttime as start, 
  facs.name as name 
from 
  cd.facilities facs 
  inner join cd.bookings bks on facs.facid = bks.facid 
where 
  facs.name in (
    'Tennis Court 2', 'Tennis Court 1'
  ) 
  and bks.starttime >= '2012-09-21' 
  and bks.starttime < '2012-09-22' 
order by 
  bks.starttime;
  ```
###### Question 14: Produce a list of all members, along with their recommender - How can you output a list of all members, including the individual who recommended them (if any)? Ensure that results are ordered by (surname, firstname).
```
select 
  mems.firstname as memfname, 
  mems.surname as memsname, 
  recs.firstname as recfname, 
  recs.surname as recsname 
from 
  cd.members mems 
  left outer join cd.members recs on recs.memid = mems.recommendedby 
order by 
  memsname, 
  memfname;
  ```
###### Question 15: Produce a list of all members who have recommended another member - How can you output a list of all members who have recommended another member? Ensure that there are no duplicates in the list, and that results are ordered by (surname, firstname).
```
select 
  distinct recs.firstname as firstname, 
  recs.surname as surname 
from 
  cd.members mems 
  inner join cd.members recs on recs.memid = mems.recommendedby 
order by 
  surname, 
  firstname;
  ```
###### Question 16: Produce a list of all members, along with their recommender, using no joins. - How can you output a list of all members, including the individual who recommended them (if any), without using any joins? Ensure that there are no duplicates in the list, and that each firstname + surname pairing is formatted as a column and ordered.
```
select 
  distinct mems.firstname || ' ' || mems.surname as member, 
  (
    select 
      recs.firstname || ' ' || recs.surname as recommender 
    from 
      cd.members recs 
    where 
      recs.memid = mems.recommendedby
  ) 
from 
  cd.members mems 
order by 
  member;
```
##### Aggregation

###### Question 17: Count the number of recommendations each member makes. - Produce a count of the number of recommendations each member has made. Order by member ID.
```
select 
  recommendedby, 
  count(*) 
from 
  cd.members 
where 
  recommendedby is not null 
group by 
  recommendedby 
order by 
  recommendedby;
  ```
###### Question 18: List the total slots booked per facility - Produce a list of the total number of slots booked per facility. For now, just produce an output table consisting of facility id and slots, sorted by facility id.
```
select 
  facid, 
  sum(slots) as "Total Slots" 
from 
  cd.bookings 
group by 
  facid 
order by 
  facid;
  ```
###### Question 19: List the total slots booked per facility in a given month - Produce a list of the total number of slots booked per facility in the month of September 2012. Produce an output table consisting of facility id and slots, sorted by the number of slots.
```
select 
  facid, 
  sum(slots) as "Total Slots" 
from 
  cd.bookings 
where 
  starttime >= '2012-09-01' 
  and starttime < '2012-10-01' 
group by 
  facid 
order by 
  sum(slots);
  ```
###### Question 20: List the total slots booked per facility per month - Produce a list of the total number of slots booked per facility per month in the year of 2012. Produce an output table consisting of facility id and slots, sorted by the id and month.
```
select 
  facid, 
  extract(
    month 
    from 
      starttime
  ) as month, 
  sum(slots) as "Total Slots" 
from 
  cd.bookings 
where 
  extract(
    year 
    from 
      starttime
  ) = 2012 
group by 
  facid, 
  month 
order by 
  facid, 
  month;
  ```
###### Question 21: Find the count of members who have made at least one booking - Find the total number of members (including guests) who have made at least one booking.
```
select 
  count(distinct memid) 
from 
  cd.bookings
```
###### Question 22: List each member's first booking after September 1st 2012 - Produce a list of each member name, id, and their first booking after September 1st 2012. Order by member ID.
```
select 
  mems.surname, 
  mems.firstname, 
  mems.memid, 
  min(bks.starttime) as starttime 
from 
  cd.bookings bks 
  inner join cd.members mems on mems.memid = bks.memid 
where 
  starttime >= '2012-09-01' 
group by 
  mems.surname, 
  mems.firstname, 
  mems.memid 
order by 
  mems.memid;
  ```
###### Question 23: Produce a list of member names, with each row containing the total member count - Produce a list of member names, with each row containing the total member count. Order by join date, and include guest members.
```
select 
  count(*) over(), 
  firstname, 
  surname 
from 
  cd.members 
order by 
  joindate
```
###### Question 24: Produce a numbered list of members - Produce a monotonically increasing numbered list of members (including guests), ordered by their date of joining. Remember that member IDs are not guaranteed to be sequential.
```
select 
  row_number() over(
    order by 
      joindate
  ), 
  firstname, 
  surname 
from 
  cd.members 
order by 
  joindate
  ```
###### Question 25: Output the facility id that has the highest number of slots booked, again - Output the facility id that has the highest number of slots booked. Ensure that in the event of a tie, all tieing results get output.
```
select 
  facid, 
  total 
from 
  (
    select 
      facid, 
      sum(slots) total, 
      rank() over (
        order by 
          sum(slots) desc
      ) rank 
    from 
      cd.bookings 
    group by 
      facid
  ) as ranked 
where 
  rank = 1
  ```
##### String

###### Question 26: Format the names of members - Output the names of all members, formatted as 'Surname, Firstname'
```
select 
  surname || ', ' || firstname as name 
from 
  cd.members
  ```
###### Question 27: Find telephone numbers with parentheses - You've noticed that the club's member table has telephone numbers with very inconsistent formatting. You'd like to find all the telephone numbers that contain parentheses, returning the member ID and telephone number sorted by member ID.
```
select 
  memid, 
  telephone 
from 
  cd.members 
where 
  telephone ~ '[()]';
  ```
###### Question 28: Count the number of members whose surname starts with each letter of the alphabet - You'd like to produce a count of how many members you have whose surname starts with each letter of the alphabet. Sort by the letter, and don't worry about printing out a letter if the count is 0.
```
select 
  substr (mems.surname, 1, 1) as letter, 
  count(*) as count 
from 
  cd.members mems 
group by 
  letter 
order by 
  letter
```


