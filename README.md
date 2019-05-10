# MOVIE

1. Write a query to display movie names and number of times that movie is issued to customers.
   Incase movies are never issued to customers display number of times as 0. Display the details in sorted order based on
number of times (in descending order)  and then by movie name (in ascending order). The Alias name for the number of movies 
issued is ISSUE_COUNT.

A) SELECT m.MOVIE_NAME,count(ISSUE_ID) ISSUE_COUNT FROM
   movies_master m LEFT JOIN customer_issue_details c ON m.MOVIE_ID=c.MOVIE_ID
   GROUP BY m.movie_name
   ORDER BY ISSUE_COUNT DESC,MOVIE_NAME;
   
 --------------------------------------------------------------------------------------------------------------------------------------
 
 2. Write a query to display id,name,age,contact no of customers whose age is greater than 25 and and who have registered in 
 the year 2012.   Display contact no in the below format +91-XXX-XXX-XXXX example +91-987-678-3434 and use the alias name as 
 "CONTACT_ISD". If the contact no is null then display as 'N/A'  Sort all the records in ascending order based on age and then by name.

A) SELECT CUSTOMER_ID,CUSTOMER_NAME,AGE,ifnull(
concat('+91-',substring(contact_no,1,3),'-',
substring(contact_no,4,3),'-',substring(contact_no,7)),'N/A') CONTACT_ISD
FROM customer_master WHERE age>25 and year(date_of_registration)='2012'
ORDER BY age,CUSTOMER_NAME;

---------------------------------------------------------------------------------------------------------------------------------------

3.Write a query to display the movie category and number of movies in that category.
Display records based on number of movies from higher to lower order  and then by movie category in 
ascending order.Hint: Use NO_OF_MOVIES as alias name for number of movies.

A) SELECT MOVIE_CATEGORY,count(MOVIE_ID) NO_OF_MOVIES FROM
movies_master GROUP BY MOVIE_CATEGORY
ORDER BY NO_OF_MOVIES DESC,MOVIE_CATEGORY;

-------------------------------------------------------------------------------------------------------------------------------------------

4.Write a query to display the number of customers having card with description “Gold card”. <br/>
   Hint: Use CUSTOMER_COUNT as alias name for number of customers

A) SELECT count(c.customer_id) CUSTOMER_COUNT FROM 
library_card_master l JOIN customer_card_details c ON l.CARD_ID=c.CARD_ID
WHERE description='Gold';

-----------------------------------------------------------------------------------------------------------------------------

5.Write a query to display the customer id, customer name, year of registration,library card id, card issue date of all the customers who hold library card. Display the records sorted by customer name in descending order. Use REGISTERED_YEAR as alias name for year of registration.

A) SELECT c.customer_id,c.customer_name,
year(c.DATE_OF_REGISTRATION) REGISTERED_YEAR,cd.card_id,cd.issue_date FROM 
customer_master c JOIN customer_card_details cd ON c.customer_id=cd.customer_id
ORDER BY CUSTOMER_NAME DESC;

   
   ---------------------------------------------------------------------------------------------------------------------------------
   
   6.Write a query to display issue id, customer id, customer name for the customers who have paid fine and whose name starts with
   'R'. Fine is calculated based on return date and actual date of return. If the date of actual return is after date of return
   then fine need to be paid by the customer order by customer name.
   
A) SELECT ci.issue_id,ci.CUSTOMER_ID,c.CUSTOMER_NAME FROM
   customer_master c JOIN customer_issue_details ci ON c.customer_id=ci.customer_id
   WHERE customer_name LIKE 'R%' and ci.actual_date_return>ci.return_date
   ORDER BY customer_name;
   
   ------------------------------------------------------------------------------------------------------------------------------------
   
   7. Write a query to display customer id, customer name, card id, card description and card amount in dollars of
   customers who have taken movie on the same day the library card is registered. For Example Assume John registered a
   library card on 12th Jan 2013 and he took a movie on 12th Jan 2013 then display his details. AMOUNT_DOLLAR = amount/52.42 and 
   round it to zero decimal places and display as $Amount. Example Assume 500 is the amount then dollar value will be $10.
   Hint: Use AMOUNT_DOLLAR as alias name for amount in dollar. Display the records in ascending order based on customer name.

A) SELECT c.CUSTOMER_ID,c.CUSTOMER_NAME,l.card_id,l.DESCRIPTION,
concat('$',round(amount/52.42)) AMOUNT_DOLLAR FROM
customer_master c JOIN customer_issue_details ci ON c.customer_id=ci.customer_id
JOIN customer_card_details cc ON cc.customer_id=c.customer_id
JOIN library_card_master l ON cc.card_id=l.card_id
WHERE c.DATE_OF_REGISTRATION=ci.issue_date
ORDER BY customer_name;

----------------------------------------------------------------------------------------------------------------------------------------

8.Write a query to display the customer id, customer name,contact number and address of customers
who have taken movies from library without library card and whose address ends with 'Nagar'.
Display customer name in upper case. Hint: Use CUSTOMER_NAME as alias name for customer name. Display the details sorted in ascending
order based on customer name.

A) SELECT CUSTOMER_ID,upper(CUSTOMER_NAME) CUSTOMER_NAME,contact_no,contact_add FROM
   customer_master WHERE contact_add LIKE '%Nagar' and 
customer_id NOT IN (SELECT customer_id FROM customer_card_details)
and customer_id IN (SELECT customer_id FROM customer_issue_details)
ORDER BY CUSTOMER_NAME;

---------------------------------------------------------------------------------------------------------------------------------------

9. Write a query to display the movie id, movie name,release year,director name of movies acted by the leadactor1 who 
acted maximum number of movies .Display the records sorted in ascending order based on movie name.
SELECT movie_id,movie_name,release_date,director FROM movies_master 

A) SELECT movie_id,movie_name,release_date,director FROM movies_master WHERE lead_role_1 IN(SELECT lead_role_1 FROM 
(SELECT lead_role_1,count(movie_id)ct FROM movies_master 
GROUP BY lead_role_1)t WHERE t.ct>=ALL(SELECT count(movie_id) 
FROM movies_master GROUP BY lead_role_1)) ORDER BY movie_name;

-------------------------------------------------------------------------------------------------------------------------------------

10.Write a query to display the customer name and number of movies issued to that customer sorted 
by customer name in ascending order.  If a customer has not  been issued with any movie then display 0. <br>Hint: Use MOVIE_COUNT 
as alias name for number of movies issued.

A) SELECT c.customer_name,count(ci.movie_id) MOVIE_COUNT FROM
customer_master c LEFT JOIN customer_issue_details ci ON c.customer_id=ci.customer_id
GROUP BY c.customer_id ORDER BY c.customer_name;

----------------------------------------------------------------------------------------------------------------------------------------

11.Write a query to display serial number,issue id, customer id, customer name, movie id and movie name 
of all the videos that are issued and display in ascending order based on serial number.Serial number can be generated 
from the issue id , that is last two characters of issue id is the serial number. For Example Assume the issue id is I00005
then the serial number is 05 Hint: Alias name for serial number is 'SERIAL_NO'

A) SELECT substring(ci.issue_id,-2) SERIAL_NO,ci.issue_id,c.customer_id,c.customer_name,
m.movie_id,m.movie_name FROM customer_master c JOIN customer_issue_details ci
ON c.customer_id=ci.customer_id JOIN movies_master m ON m.movie_id=ci.movie_id
ORDER BY SERIAL_NO;

-----------------------------------------------------------------------------------------------------------------------------------------
12.Write a query to display the issue id,issue date, customer id, customer name and contact number for videos that are issued
in the year 2013.Display the records in decending order based on issue date of the video.

A) SELECT ci.issue_id,ci.issue_date,c.customer_id,c.customer_name,c.contact_no FROM
customer_master c JOIN customer_issue_details ci ON c.customer_id=ci.customer_id
and year(ci.issue_date)='2013' ORDER BY ci.issue_date DESC;

------------------------------------------------------------------------------------------------------------------------------------

13.Write a query to display movie id ,movie name and actor names of movies which are not issued to any customers. 
<br> Actors Name to be displayed in the below format.LEAD_ACTOR_ONE space ambersant space LEAD_ACTOR_TWO.Example: 
Assume lead actor one's name is "Jack Tomson" and Lead actor two's name is "Maria" then Actors name will be "Jack Tomsom 
& Maria"Hint:Use ACTORS as alias name for actors name. <br> Display the records in ascending order based on movie name.

A) SELECT movie_id,movie_name,concat(lead_role_1,' & ',lead_role_2) ACTOR FROM movies_master
   WHERE movie_id NOT IN (SELECT movie_id FROM customer_issue_details) ORDER BY movie_name;
   
-------------------------------------------------------------------------------------------------------------------------------------

14.Write a query to display the director's name, movie name and lead_actor_name1 of all the movies directed by the director 
who directed more than one movie. Display the directors name in capital letters. Use DIRECTOR_NAME as alias name for director 
name column Display the records sorted in ascending order based on director_name and then by movie_name in descending order.

A) SELECT upper(director) DIRECTOR_NAME,movie_name,lead_role_1 FROM movies_master
GROUP BY director HAVING count(movie_id)>1 ORDER BY director,movie_name DESC;

--------------------------------------------------------------------------------------------------------------------------------------

15.Write a query to display number of customers who have registered in the library in the year 2012
and who have given/provided contact number. <br> Hint:Use NO_OF_CUSTOMERS as alias name for number of customers.

A) SELECT count(customer_id) NO_OF_CUSTOMER FROM customer_master
WHERE contact_no is not null and year(date_of_registration)='2012';

---------------------------------------------------------------------------------------------------------------------------------------
16.Write a query to display the customer's name, contact number,library card id and library card description of all 
the customers irrespective of customers holding a library card. If customer contact number is not available then display 
his address. Display the records sorted in ascending order based on customer name.  Hint: Use CONTACT_DETAILS as alias name for 
customer contact.

A) SELECT c.customer_name,ifnull(c.contact_no,c.contact_add) CONTACT_DETAILS,l.card_id,l.description FROM
customer_master c LEFT JOIN customer_card_details cc ON c.customer_id=cc.customer_id
LEFT JOIN library_card_master l ON l.card_id=cc.card_id
ORDER BY customer_name;

--------------------------------------------------------------------------------------------------------------------------------------

17. Write a query to display the customer id, customer name and number of times the same movie is issued to the same customers
who have taken same movie more than once. Display the records sorted by customer name in decending order  For Example: Assume customer
John has taken Titanic three times and customer Ram has taken Die hard only once then display the details of john.  
Hint: Use NO_OF_TIMES as alias name for number of times

A) SELECT ci.customer_id,c.customer_name,count(ci.movie_id) NO_OF_TIMES FROM
customer_issue_details ci JOIN customer_master c ON c.customer_id=ci.customer_id
GROUP BY ci.customer_id,ci.movie_id HAVING count(movie_id)>1
ORDER BY customer_name DESC;

-----------------------------------------------------------------------------------------------------------------------------------------

18.Write a query to display customer id, customer name,contact number, movie category and number of movies issued to each customer based
on movie category who has been issued with more than one movie in that category. Example: Display contact number as "+91-876-456-2345"
format.&nbsp; Hint:Use NO_OF_MOVIES as alias name for number of movies column. Hint:Use CONTACT_ISD as alias name for contact number.
Display the records sorted in ascending order based on customer name and then by movie category.

A) SELECT c.customer_id,c.customer_name,concat('+91-',substring(c.contact_no,1,3),'-',
substring(c.contact_no,4,3),'-',substring(c.contact_no,7)) CONTACT_ISD
,m.movie_category,count(cc.movie_id) NO_OF_MOVIES FROM customer_master c JOIN customer_issue_details cc
ON c.customer_id=cc.customer_id JOIN movies_master m ON m.movie_id=cc.movie_id
GROUP BY c.customer_id,m.movie_category HAVING count(cc.movie_id)>1
ORDER BY customer_name,movie_category;

-----------------------------------------------------------------------------------------------------------------------------------------

19.Write a query to display customer id and customer name of customers who has been issued with maximum number of movies 
and customer who has been issued with minimum no of movies. For example Assume customer John has been issued 5 movies, Ram 
has been issued 10 movies and Kumar has been issued 2 movies. The name and id of Ram should be displayed for issuing maximum movies 
and Kumar should be displayed for issuing minimum movies. Consider only the customers who have been issued with atleast 1 movie
Customer(s) who has/have been issued the maximum number of movies must be displayed first followed by the customer(s) who has/have 
been issued with the minimum number of movies. In case of multiple customers who have been displayed with the maximum or minimum number
of movies, display the records sorted in ascending order based on customer name.

A) SELECT cid.customer_id , customer_name FROM  customer_master cm JOIN customer_issue_details cidON cm.customer_id=cid.customer_id
GROUP BY customer_id , customer_name
HAVING count(movie_id)>=ALL(SELECT  count(movie_id)
FROM customer_issue_details
GROUP BY customer_id)
UNION
SELECT cid.customer_id , customer_name FROM
customer_master cm JOIN customer_issue_details cid
ON cm.customer_id=cid.customer_id
GROUP BY customer_id , customer_name
HAVING count(movie_id)<=ALL(SELECT  count(movie_id)
FROM customer_issue_details
GROUP BY customer_id) ORDER BY customer_name;

----------------------------------------------------------------------------------------------------------------------------------------

20.Write a query to display the customer id , customer name and number of times movies have been issued from Comedy 
category. Display only for customers who has taken more than once. Hint: Use NO_OF_TIMES as alias name  
Display the records in ascending order based on customer name.

A) SELECT c.customer_id,c.customer_name,count(m.movie_id) NO_OF_TIMES FROM
customer_master c JOIN customer_issue_details cc ON c.customer_id=cc.customer_id
JOIN movies_master m ON m.movie_id=cc.movie_id 
WHERE m.movie_category='Comedy'
GROUP BY c.customer_id HAVING count(m.movie_id)>1
ORDER BY customer_name;

--------------------------------------------------------------------------------------------------------------------------------------

21.Write a query to display customer id and total rent paid by the customers who are issued with the videos. Need not 
display the customers who has not taken / issued with any videos. Hint: Alias Name for total rent paid is TOTAL_COST. 
Display the records sorted in ascending order based on customer id

A) SELECT cid.customer_id, sum(m.rent_cost) TOTAL_COST FROM customer_issue_details cid  
JOIN movies_master mm ON cid.movie_id=mm.movie_id GROUP BY cid.customer_id order by customer_id;

------------------------------------------------------------------------------------------------------------------------------------



















