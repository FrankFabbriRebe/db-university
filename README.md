# Esercizio DB University - 2

**1. Selezionare tutti gli studenti nati nel 1990 (160)**
```sql
SELECT * 
FROM students 
  WHERE year(date_of_birth) = 1990;
```

**2.Selezionare tutti i corsi che valgono più di 10 crediti (479)**
```sql
SELECT * 
FROM courses 
  WHERE cfu > 10;
```

**3.Selezionare tutti gli studenti che hanno più di 30 anni**
```sql
SELECT * 
FROM students 
  WHERE YEAR(CURDATE()) - YEAR(date_of_birth) > 30 
  OR (YEAR(CURDATE()) - YEAR(date_of_birth) = 30 
    AND CURDATE() > CONCAT(YEAR(date_of_birth), '-', MONTH(date_of_birth), '-', DAY(date_of_birth)));
```

**4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)**
```sql
SELECT * 
FROM courses 
  WHERE period like "I semestre" 
    AND year = 1;
```

**5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)**
```sql
SELECT * 
FROM exams 
  WHERE date = '2020-06-20' 
  AND hour >= '14:00:00';
```

**6. Selezionare tutti i corsi di laurea magistrale (38)**
```sql
SELECT * 
FROM degrees 
  WHERE level = "magistrale";
```

**7. Da quanti dipartimenti è composta l'università? (12)**
```sql
SELECT COUNT(*) 
FROM departments;
```

**8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)**
```sql
SELECT * 
FROM teachers 
  WHERE phone is null;
```



# Esercizio DB University - 3

## GROUP BY

**1. Contare quanti iscritti ci sono stati ogni anno**
```sql
SELECT enrolment_date, COUNT(id) "numero_iscritti"
FROM students
  GROUP BY enrolment_date;
```

**2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio**
```sql
SELECT office_address, COUNT(*) "numero_insegnanti"
FROM teachers
  GROUP BY office_address;
```

**3. Calcolare la media dei voti di ogni appello d'esame (dell'esame vogliamo solo l'id)**
```sql
SELECT exam_id, FLOOR(AVG(exam_student.vote)) "media_voti"
FROM exam_student
  GROUP BY exam_id;
```

**4. Contare quanti corsi di laurea ci sono per ogni dipartimento**
```sql
SELECT department_id, COUNT(id) "degrees"
FROM degrees
  GROUP BY department_id;
```

## JOIN

**1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia**
```sql
SELECT students.name "nome_studente", degrees.name "degrees"
FROM degrees
    JOIN students
    ON degrees.id = students.degree_id
WHERE degrees.name LIKE 'Corso di Laurea in Economia';
```

**2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze**
```sql
SELECT degrees.name "degrees"
FROM departments
    JOIN degrees
    ON departments.id = degrees.department_id
WHERE degrees.level LIKE "magistrale" AND departments.name LIKE "Dipartimento di Neuroscienze";
```

**3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)**
```sql
SELECT name
FROM courses
  JOIN course_teacher
  ON course_teacher.course_id = courses.id
WHERE course_teacher.teacher_id = 44;
```

**4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome**
```sql
SELECT students.surname "cognome_studente", students.name "nome_studente", degrees.name "nome_corso_laurea", departments.name "nome_dipartimento"
FROM students
    JOIN degrees
    ON students.degree_id = degrees.id
    JOIN departments
    ON degrees.department_id = departments.id
 ORDER BY students.surname;
```

**5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti**
```sql
SELECT degrees.name "degrees", courses.name "courses", teachers.name "teachers_name", teachers.surname "teacher_surname"
FROM degrees
    JOIN courses
    ON degrees.id = courses.degree_id
    JOIN course_teacher
    ON courses.id = course_teacher.course_id
     JOIN teachers
    ON course_teacher.teacher_id = teachers.id;
```

**6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)**
```sql
SELECT teachers.name, teachers.surname
FROM teachers
JOIN course_teacher
    ON teachers.id = course_teacher.teacher_id
  JOIN courses
    ON course_teacher.course_id = courses.id
  JOIN degrees
    ON courses.degree_id = degrees.id
  JOIN departments
    ON degrees.department_id = departments.id
WHERE departments.name LIKE 'Dipartimento di Matematica';
```
