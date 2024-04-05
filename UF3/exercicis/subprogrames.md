# Subprogrames i Procediments

### Exercici 1
Funció que canvia el format d'una data de AAAA-MM-DD a DD-MM-AAAA.

```mysql
DELIMITER //

DROP FUNCTION IF EXISTS spData //
CREATE FUNCTION spData (pData DATE) 
RETURNS CHAR(10)
DETERMINISTIC
BEGIN 
    DECLARE vRetorn CHAR(10);
    
    SET vRetorn = DATE_FORMAT(pData, '%d-%m-%y');
    
    RETURN vRetorn;
END //

SELECT spdata(curdate());
```
