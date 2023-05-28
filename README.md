# storeproceduremysql
store procedure mysql

insert data to table C from table A and B 

<img width="973" alt="image" src="https://github.com/dimashartono/storeproceduremysql/assets/45741186/a26e1027-c16d-4719-833f-083378fc02b7">


DELIMITER //

CREATE PROCEDURE InsertDataFromAB()

BEGIN

  INSERT INTO table_c (id, cardnumber, iss, acq, dest, status_a, status_iss, status_acq, status_dest)
  
  SELECT a.id, a.cardnumber, a.iss, a.acq, a.dest, a.status AS status_a, 
    CASE WHEN b.iss = b.source AND b.status = 1 THEN 1 ELSE 0 END AS status_iss,
    CASE WHEN b.acq = b.source AND b.status = 1 THEN 1 ELSE 0 END AS status_acq,
    CASE WHEN b.dest = b.source AND b.status = 1 THEN 1 ELSE 0 END AS status_dest
    
  FROM table_a a
  
  LEFT JOIN table_b b ON a.cardnumber = b.cardnumber 
  
  WHERE a.status = b.status;
  
END //

DELIMITER ;

to execute: 

CALL InsertDataFromAB();
