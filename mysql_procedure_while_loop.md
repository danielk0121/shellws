### mysql while loop

```
DELIMITER $$
DROP PROCEDURE IF EXISTS `proc_tmp_20191114`$$
CREATE PROCEDURE proc_tmp_20191114()
BEGIN
	DECLARE _tdate INT DEFAULT 0 ;
	DECLARE _sdate INT DEFAULT 20191010 ;
	DECLARE _edate INT DEFAULT 20191024 ;
	
	SET _tdate = _sdate ;	
	loop1: while(_tdate <= _edate)
	DO
		CALL proc_insert_rpstatdd(_tdate) ;
-- 		SELECT CONCAT(_sdate,' ', _edate,' ',_tdate) msg;
		SET _tdate = DATE_FORMAT(DATE_ADD(_tdate, INTERVAL 1 DAY),'%Y%m%d') ;		
	END while loop1;
	
END $$
delimiter ;

call proc_tmp_20191114() ;
```
