## 1. Selezionare tutti gli studenti nati nel 1990 (160)

----sql
SELECT \*
FROM `students`
WHERE
YEAR(`date_of_birth`)=1990;

---

## 2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

----sql
SELECT \*
FROM `courses`
WHERE `cfu` > 10;

---

## 3. Selezionare tutti gli studenti che hanno più di 30 anni

----sql
SELECT \*
FROM `students`
WHERE TIMESTAMPDIFF(YEAR, date_of_birth, CURDATE()) > 30;

↑↑↑// questa soluzione é giusta pero considera solo l'anno non i calcoli dei mesi

---

Altra opzione:
SELECT \*
FROM `students`
WHERE DATEDIFF(NOW(), `date_of_birth`) / 365.25 > 30;

↑↑↑// questa soluzione non é stata quella piú precisa

---

Altra opzione:
SELECT \*
FROM `students`
WHERE YEAR(NOW()) - YEAR(date_of_birth) > 30;

↑↑↑// questa soluzione non é tanto precisa

---

SELECT \*
FROM `students`
WHERE YEAR(date_of_birth) < 1994;

↑↑↑// questa soluzione non va bene perche il prossimo anno 2025 non funzionera piu, abbiamo bisogno un funzione che calcoli gli anni da sola e cosi funzioni tutti gli anni.

---

## 4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

----sql
SELECT \*
FROM `courses`
WHERE `period` = 'I semestre'
AND `year` = 1;

---

## 5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

----sql
SELECT \*
FROM `exams`
WHERE `date` = '2020-06-20'
AND `hour`  
LIKE '14%'

---

Altra opzione:
SELECT \*
FROM `exams`
WHERE `date` = '2020-06-20'
AND hour(`hour`) >= 14;

## 6. Selezionare tutti i corsi di laurea magistrale (38)

----sql
SELECT \*
FROM `degrees`
WHERE `name`
LIKE 'Corso di Laurea Magistrale%';

---

Altra opzione:

SELECT \*
FROM `degrees`
WHERE `level` = 'magistrale'

## 7. Da quanti dipartimenti è composta l'università? (12)

----sql
SELECT COUNT(\*)
FROM `departments`;

---

## 8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

----sql
SELECT \*
FROM `teachers`
WHERE `phone`
IS NULL;

---

# Query esercizio 28-03-24.

# Group by:

# 1. Contare quanti iscritti ci sono stati ogni anno

# 2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

# 3. Calcolare la media dei voti di ogni appello d'esame

# 4. Contare quanti corsi di laurea ci sono per ogni dipartimento

# Joins:

# 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

----sql
SELECT `students`.`id`, `students`.`name`, `students`.`surname`, `degrees`.`name` AS `degree_name`
FROM `students`
JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

# 2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

----sql
SELECT `degrees`. \*
FROM `degrees`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
WHERE `degrees`.`level` = 'Magistrale'
AND `departments`.`name` = 'Dipartimento di Neuroscienze';

# 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

----sql
SELECT `courses`.`id`, `courses`.`name` AS `course_name`, `teachers`.`name` AS `teacher_name`, `teachers`.`surname` AS `teacher_surname`, `teachers`.`id` AS `teacher_id`
FROM `courses`
JOIN `course_teacher`
ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE `teachers`.`name` = 'Fulvio'
AND `teachers`.`surname` = 'Amato';

# 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

----sql
SELECT `students`.`surname`, `students`.`name` AS `name_student`, `degrees`.`name` AS `degree_name`, `departments`.`name` AS `department`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
ORDER BY `students`.`surname`, `students`.`name` ASC;

# 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

----sql
SELECT `degrees`.`name` AS `degree`, `courses`.`name` AS `course`, `teachers`.`name` AS `teacher_name`, `teachers`.`surname`
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`;

# 6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

# 7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.
