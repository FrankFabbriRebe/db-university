# Esercizio query

**1. Selezionare tutti gli studenti nati nel 1990 (160)**
//
SELECT * 
FROM students 
  WHERE year(date_of_birth) = 1990;
//

**2.Selezionare tutti i corsi che valgono più di 10 crediti (479)**
SELECT * 
FROM courses 
  WHERE cfu > 10;

**3.Selezionare tutti gli studenti che hanno più di 30 anni**
SELECT * 
FROM students 
  WHERE YEAR(CURDATE()) - YEAR(date_of_birth) > 30 
  OR (YEAR(CURDATE()) - YEAR(date_of_birth) = 30 
    AND CURDATE() > CONCAT(YEAR(date_of_birth), '-', MONTH(date_of_birth), '-', DAY(date_of_birth))); 

**4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)**
SELECT * 
FROM courses 
  WHERE period like "I semestre" 
    AND year = 1;

**5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)**
SELECT * 
FROM exams 
  WHERE date = '2020-06-20' 
  AND hour >= '14:00:00';

**6. Selezionare tutti i corsi di laurea magistrale (38)**
SELECT * 
FROM degrees 
  WHERE level = "magistrale";

**7. Da quanti dipartimenti è composta l'università? (12)**
SELECT COUNT(*) 
FROM departments;

**8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)**
SELECT * 
FROM teachers 
  WHERE phone is null;
