# Subprogrames i Procediments

### Exercici 1 - Fes una funció anomenada spData, tal que donada una data en format MySQL ( AAAA-MM-DD ) ens retorni una cadena de caràcters en format DD-MM-AAAA


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

### Exercici 2 - Fes una funció anomenada spPotencia, tal que donada una base i un exponent, ens calculi la seva potència. Intenta no utilitzar la funció POW.

```mysql
DELIMITER //

DROP FUNCTION IF EXISTS spPotencia //
CREATE FUNCTION spPotencia(pBase INT, pExp INT)
RETURNS BIGINT
DETERMINISTIC
BEGIN
	DECLARE vRetorn BIGINT;
	
    IF pBase IS NOT NULL AND pExp IS NOT NULL THEN
		SET vRetorn = 1;
        WHILE pEXp > 0 DO
			SET vRetorn = vRetorn * pBase;
            SET pExp = pExp - 1;
		END WHILE;
	END IF;
    
    RETURN vRetorn;
END //

SELECT spPotencia(4, 10);
```

### Exercici 3 - Fes una funció anomenada spIncrement que donat un codi d’empleat i un % de increment, ens calculi el salari sumant aquest percentatge.

```mysql
DROP FUNCTION IF EXISTS spIncrement //
CREATE FUNCTION spIncrement (pEmpleatId INT, pIncrement FLOAT) RETURNS FLOAT
NOT DETERMINISTIC READS SQL DATA
BEGIN
	DECLARE vRetorn FLOAT;
    
    SELECT salari*(pIncrement+100)/100 INTO vRetorn;
    
    RETURN vRetorn;
END //

SELECT spIncrement(103,10);
```

### Exercici 4 - Fes una funció anomenada spPringat, tal que li passem un codi de departament, i ens torni el codi d’empleat que guanya menys d’aquell departament. 

```mysql
DELIMITER //
CREATE FUNCTION spPringat(departament_id INT)
RETURNS INT
DETERMINISTIC
BEGIN 
	DECLARE id_empleat INT;
    
    SELECT empleat_id INTO id_empleat
    FROM empleats
    WHERE departament_id = departament_id
    ORDER BY salari ASC
    LIMIT 1;
    
    RETURN id_empleat;
END //

SELECT spPringat(60);
```

### Exercici 6 - Fes una funció anomenada spCategoria, tal que donat un codi d’empleat, ens digui en quina categoria professional està. El criteri que volem seguir per determinar la categoria professional és en funció dels anys que porta treballant a l’empresa: - Entre 0 i 1 anys -> Auxiliar - Entre 2 i 10 anys -> Oficial de Segona - Entre 11 i 20 Anys -> Oficial de Primera - Més de 20 anys -> Que es jubili!

```mysql
DROP FUNCTION IF EXISTS spCategoria;
DELIMITER //
CREATE FUNCTION spCategoria(empleat_id INT)
RETURNS CHAR(2)
DETERMINISTIC
BEGIN
	DECLARE anys_treballats INT;
	DECLARE categoria VARCHAR(15);
    
    SELECT anys_treballats = DATESTAMPDIFF(YEAR, data_contractacio, GETDATE())
		INTO anys_treballats
	FROM empleats
    WHERE empleat_id = empleat_id;
    
    CASE 
		WHEN anys_treballats <= 1 THEN SET categoria = 'Auxiliar';
        WHEN anys_treballats >= 2 AND anys_treballats <= 10 THEN SET categoria = 'Oficial de Segona';
		WHEN anys_treballats >= 11 AND anys_treballats <= 20 THEN SET categoria = 'Oficial de Primera';
		ELSE SET categoria = 'Que es jubili!';
	END CASE;
    
    RETURN categoria;
END //
```

### Exercici 8 - Fes una funció anomenada spEdat, tal que donada una data per paràmetre ens retorni l'edat d'una persona. Les dates posteriors a la data d'avui han de retornar 0.

```mysql
DELIMITER //
DROP FUNCTION IF EXISTS spEdat //
CREATE FUNCTION spEdat(pData DATE)
RETURNS INT
DETERMINISTIC
	BEGIN
		DECLARE vEdat INT;
        
        SET vEdat = TIMESTAMPDIFF(YEAR, pData, curdate());
        
        IF (pData > curdate())
			THEN RETURN 0;
		END IF;
        
	RETURN vEdat;

END //

SELECT spEdat('2004-10-07')
```
