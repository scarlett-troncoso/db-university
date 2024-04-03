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
