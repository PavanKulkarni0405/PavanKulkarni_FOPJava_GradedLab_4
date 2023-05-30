INSERT INTO SUPPLIER VALUES(1,"Rajesh Retails","Delhi",'1234567890');
INSERT INTO SUPPLIER VALUES(2,"Appario Ltd.","Mumbai",'2589631470');
INSERT INTO SUPPLIER VALUES(3,"Knome products","Banglore",'9785462315');
INSERT INTO SUPPLIER VALUES(4,"Bansal Retails","Kochi",'8975463285');
INSERT INTO SUPPLIER VALUES(5,"Mittal Ltd.","Lucknow",'7898456532');
select * from supplier;

INSERT INTO CUSTOMER VALUES(1,"AAKASH",'9999999999',"DELHI",'M');
INSERT INTO CUSTOMER VALUES(2,"AMAN",'9785463215',"NOIDA",'M');
INSERT INTO CUSTOMER VALUES(3,"NEHA",'9999999999',"MUMBAI",'F');
INSERT INTO CUSTOMER VALUES(4,"MEGHA",'9994562399',"KOLKATA",'F');
INSERT INTO CUSTOMER VALUES(5,"PULKIT",'7895999999',"LUCKNOW",'M');
select * from customer;

INSERT INTO CATEGORY VALUES( 1,"BOOKS");
INSERT INTO CATEGORY VALUES(2,"GAMES");
INSERT INTO CATEGORY VALUES(3,"GROCERIES");
INSERT INTO CATEGORY VALUES (4,"ELECTRONICS");
INSERT INTO CATEGORY VALUES(5,"CLOTHES");
select * from category;

INSERT INTO PRODUCT VALUES(1,"GTA V","Windows 7 and above with i5 processor and 8GB RAM",2);
INSERT INTO PRODUCT VALUES(2,"TSHIRT","SIZE-L with Black, Blue and White variations",5);
INSERT INTO PRODUCT VALUES(3,"ROG LAPTOP","Windows 10 with 15inch screen, i7 processor, 1TB SSD",4);
INSERT INTO PRODUCT VALUES(4,"OATS","Highly Nutritious from Nestle",3);
INSERT INTO PRODUCT VALUES(5,"HARRY POTTER","Best Collection of all time by J.K Rowling",1);
INSERT INTO PRODUCT VALUES(6,"MILK","1L Toned MIlk",3);
INSERT INTO PRODUCT VALUES(7,"Boat EarPhones","1.5Meter long Dolby Atmos",4);
INSERT INTO PRODUCT VALUES(8,"Jeans","Stretchable Denim Jeans with various sizes and color",5);
INSERT INTO PRODUCT VALUES(9,"Project IGI","compatible with windows 7 and above",2);
INSERT INTO PRODUCT VALUES(10,"Hoodie","Black GUCCI for 13 yrs and above",5);
INSERT INTO PRODUCT VALUES(11,"Rich Dad Poor Dad","Written by RObert Kiyosaki",1);
INSERT INTO PRODUCT VALUES(12,"Train Your Brain","By Shireen Stephen",1);
select * from product;

INSERT INTO SUPPLIER_PRICING VALUES(1,1,2,1500);
INSERT INTO SUPPLIER_PRICING VALUES(2,3,5,30000);
INSERT INTO SUPPLIER_PRICING VALUES(3,5,1,3000);
INSERT INTO SUPPLIER_PRICING VALUES(4,2,3,2500);
INSERT INTO SUPPLIER_PRICING VALUES(5,4,1,1000);
INSERT INTO SUPPLIER_PRICING VALUES(6,12,2,780);
INSERT INTO SUPPLIER_PRICING VALUES(7,12,4,789);
INSERT INTO SUPPLIER_PRICING VALUES(8,3,1,31000);
INSERT INTO SUPPLIER_PRICING VALUES(9,1,5,1450);
INSERT INTO SUPPLIER_PRICING VALUES(10,4,2,999);
INSERT INTO SUPPLIER_PRICING VALUES(11,7,3,549);
INSERT INTO SUPPLIER_PRICING VALUES(12,7,4,529);
INSERT INTO SUPPLIER_PRICING VALUES(13,6,2,105);
INSERT INTO SUPPLIER_PRICING VALUES(14,6,1,99);
INSERT INTO SUPPLIER_PRICING VALUES(15,2,5,2999);
INSERT INTO SUPPLIER_PRICING VALUES(16,5,2,2999);
select * from supplier_pricing;

INSERT INTO ORDERS VALUES (101,1500,"2021-10-06",2,1);
INSERT INTO ORDERS VALUES(102,1000,"2021-10-12",3,5);
INSERT INTO ORDERS VALUES(103,30000,"2021-09-16",5,2);
INSERT INTO ORDERS VALUES(104,1500,"2021-10-05",1,1);
INSERT INTO ORDERS VALUES(105,3000,"2021-08-16",4,3);
INSERT INTO ORDERS VALUES(106,1450,"2021-08-18",1,9);
INSERT INTO ORDERS VALUES(107,789,"2021-09-01",3,7);
INSERT INTO ORDERS VALUES(108,780,"2021-09-07",5,6);
INSERT INTO ORDERS VALUES(109,3000,"2021-09-10",5,3);
INSERT INTO ORDERS VALUES(110,2500,"2021-09-10",2,4);
INSERT INTO ORDERS VALUES(111,1000,"2021-09-15",4,5);
INSERT INTO ORDERS VALUES(112,789,"2021-09-16",4,7);
INSERT INTO ORDERS VALUES(113,31000,"2021-09-16",1,8);
INSERT INTO ORDERS VALUES(114,1000,"2021-09-16",3,5);
INSERT INTO ORDERS VALUES(115,3000,"2021-09-16",5,3);
INSERT INTO ORDERS VALUES(116,99,"2021-09-17",2,14);

INSERT INTO RATING VALUES(1,101,4);
INSERT INTO RATING VALUES(2,102,3);
INSERT INTO RATING VALUES(3,103,1);
INSERT INTO RATING VALUES(4,104,2);
INSERT INTO RATING VALUES(5,105,4);
INSERT INTO RATING VALUES(6,106,3);
INSERT INTO RATING VALUES(7,107,4);
INSERT INTO RATING VALUES(8,108,4);
INSERT INTO RATING VALUES(9,109,3);
INSERT INTO RATING VALUES(10,110,5);
INSERT INTO RATING VALUES(11,111,3);
INSERT INTO RATING VALUES(12,112,4);
INSERT INTO RATING VALUES(13,113,2);
INSERT INTO RATING VALUES(14,114,1);
INSERT INTO RATING VALUES(15,115,1);
INSERT INTO RATING VALUES(16,116,0);


4. Display the total number of customers based on gender who have placed individual orders of worth at least Rs.3000.
SELECT cus_gender, COUNT(DISTINCT customer.cus_id) as Total_Customers
FROM customer
JOIN orders ON customer.cus_id = orders.cus_id
WHERE orders.ORD_AMOUNT >= 3000
GROUP BY cus_gender;

5.Display all the orders along with product name ordered by a customer having Customer_Id=2
select cus_name, cus_city, o.ORD_AMOUNT, o.pricing_id, s.PRO_ID, p.PRO_NAME, p.PRO_DESC
   from customer inner join orders as o 
     on customer.cus_id=o.CUS_ID 
     inner join supplier_pricing as s
       on o.PRICING_ID = s.PRICING_ID
     inner join product as p
       on s.PRO_ID=p.PRO_ID
     and customer.cus_id=2;
     
 6.Display the Supplier details who can supply more than one product.
 select s.supp_name, count(p.PRO_NAME) as num_of_products from supplier as s inner join supplier_pricing as sp on s.SUPP_ID=sp.SUPP_ID
        inner join product as p on sp.PRO_ID=p.PRO_ID
        group by s.SUPP_NAME
        having num_of_products > 1;
        
 7.Find the least expensive product from each category and print the table with category id, name, product name and price of the product
 
 SELECT c.CAT_ID, c.CAT_NAME, p.PRO_NAME, sp.SUPP_PRICE
FROM Category c
INNER JOIN (
    SELECT p.CAT_ID, MIN(sp.SUPP_PRICE) AS min_price
    FROM Product p
    INNER JOIN Supplier_pricing sp ON p.PRO_ID = sp.PRO_ID
    GROUP BY p.CAT_ID
) AS sub ON c.CAT_ID = sub.CAT_ID
INNER JOIN Product p ON sub.CAT_ID = p.CAT_ID
INNER JOIN Supplier_pricing sp ON p.PRO_ID = sp.PRO_ID AND sub.min_price = sp.SUPP_PRICE;
 

8.Display the Id and Name of the Product ordered after “2021-10-05”
  select c.cus_name, o.ord_amount, o.ord_date, p.pro_name, p.PRO_DESC  from orders as o inner join supplier_pricing as sp 
         on o.PRICING_ID=sp.PRICING_ID
     inner join product as p on sp.pro_id=p.PRO_ID   
     inner join customer as c
       on o.CUS_ID=c.cus_id
    where o.ORD_DATE > "2021-09-01" ;

9.Display customer name and gender whose names start or end with character 'A'.
select customer.cus_name,customer.cus_gender from customer where customer.cus_name like 'A%' or customer.cus_name like '%A';     

10.Create a stored procedure to display supplier id, name, Rating(Average rating of all the products sold by every customer) and
Type_of_Service. For Type_of_Service, If rating =5, print “Excellent Service”,If rating >4 print “Good Service”, If rating >2 print “Average
Service” else print “Poor Service”. Note that there should be one rating per supplier

DELIMITER //
CREATE PROCEDURE DisplaySupplierRating()
BEGIN
  SELECT 
    s.SUPP_ID, 
    s.SUPP_NAME, 
    AVG(r.RAT_RATSTARTS) AS Rating,
    CASE
      WHEN AVG(r.RAT_RATSTARTS) = 5 THEN 'Excellent Service'
      WHEN AVG(r.RAT_RATSTARTS) > 4 THEN 'Good Service'
      WHEN AVG(r.RAT_RATSTARTS) > 2 THEN 'Average Service'
      ELSE 'Poor Service'
    END AS Type_of_Service
  FROM 
    Supplier AS s
    JOIN Supplier_pricing AS sp ON s.SUPP_ID = sp.SUPP_ID
    JOIN orders AS o ON sp.PRICING_ID = o.PRICING_ID
    JOIN Rating AS r ON o.ORD_ID = r.ORD_ID
  GROUP BY 
    s.SUPP_ID, 
    s.SUPP_NAME;
END //
DELIMITER ;



CALL DisplaySupplierRating();

