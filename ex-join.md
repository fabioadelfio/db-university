1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql
SELECT 
	`degrees`.`id` AS `degree_id`,
    `degrees`.`name` AS `degree_name`,
    `students`.`id` AS `student_id`,
    `students`.`name` AS `student_name`,
    `students`.`surname` AS `student_surname`,
    `students`.`date_of_birth`,
    `students`.`fiscal_code`,
    `students`.`enrolment_date`,
    `students`.`registration_number`,
    `students`.`email`
FROM `degrees`

INNER JOIN `students`
ON `degrees`.`id` = `students`.`degree_id`

WHERE `degrees`.`name` = "Corso di Laurea in Economia";
```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze

```sql
SELECT 
	`departments`.`id` AS `department_id`,
    `departments`.`name` AS `department_name`,
    `degrees`.`id` AS `degree_id`,
    `degrees`.`name` AS `degree_name`,
    `degrees`.`level` AS `degree_level`,
    `degrees`.`address` AS `degree_address`,
    `degrees`.`email`,
    `degrees`.`website`
FROM `departments`

INNER JOIN `degrees`
ON `departments`.`id` = `degrees`.`department_id`

WHERE `departments`.`name` = "Dipartimento di Neuroscienze" AND `degrees`.`level` = "magistrale";
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```sql
SELECT 
	`teachers`.`id` AS `teacher_id`,
    `teachers`.`name` AS `teacher_name`,
    `teachers`.`surname` AS `teacher_surname`,
    `courses`.`id` AS `course_id`,
    `courses`.`name` AS `course_name`,
    `courses`.`description` AS `course_description`,
    `courses`.`period`,
    `courses`.`year`,
    `courses`.`cfu`,
    `courses`.`website`
FROM `teachers`

INNER JOIN `course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`

INNER JOIN `courses`
ON `course_teacher`.`course_id` = `courses`.`id`

WHERE `teachers`.`id` = "44";
```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome

```sql
SELECT 
	`students`.`id` AS `student_id`,
    `students`.`surname` AS `student_surname`,
    `students`.`name` AS `student_name`,
    `degrees`.`id`,
    `degrees`.`department_id`,
    `degrees`.`name` AS `degree_name`,
    `degrees`.`level`,
    `degrees`.`address`,
    `degrees`.`email`,
    `degrees`.`website`,
    `departments`.`name` AS `department_name`
FROM `departments`

INNER JOIN `degrees`
ON `departments`.`id` = `degrees`.`department_id`

INNER JOIN `students`
ON `degrees`.`id` = `students`.`degree_id`

ORDER BY `students`.`surname` ASC ,`students`.`name` ASC;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql
SELECT 
	`degrees`.`id` AS `degree_id`,
    `degrees`.`name` AS `degree_name`,
    `degrees`.`level` AS `degree_level`,
    `courses`.`id` AS `course_id`,
    `courses`.`name` AS `course_name`,
    `courses`.`description` AS `course_description`,
    `courses`.`period` AS `course_period`,
    `courses`.`year` AS `course_year`,
    `courses`.`cfu` AS `course_cfu`,
    `teachers`.`id` AS `teacher_id`,
    `teachers`.`name` AS `teacher_name`,
    `teachers`.`surname` AS `teacher_surname`,
    `teachers`.`phone` AS `teacher_phone`,
    `teachers`.`email` AS `teacher_email`,
    `teachers`.`office_address` AS `teacher_office_address`,
    `teachers`.`office_number` AS `teacher_office_number`
FROM `degrees`

INNER JOIN `courses`
ON `degrees`.`id` = `courses`.`degree_id`

INNER JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`

INNER JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`;
```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)

```sql
SELECT 
	`teachers`.`id` AS `teacher_id`,
    `teachers`.`name` AS `teacher_name`,
    `teachers`.`surname` AS `teacher_surname`,
    `departments`.`id` AS `department_id`,
    `departments`.`name` AS `department_name`
FROM `departments`

INNER JOIN `degrees`
ON `departments`.`id` = `degrees`.`department_id`

INNER JOIN `courses`
ON `degrees`.`id` = `courses`.`degree_id`

INNER JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`

INNER JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`

WHERE `departments`.`name` = "Dipartimento di Matematica";
```

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18

```sql
SELECT 
	`students`.`id` AS `student_id`,
    `students`.`name` AS `student_name`,
    `students`.`surname` AS `student_surname`,
    COUNT(*) AS `exam_attempts`,
    MAX(`exam_student`.`vote`) AS `max_vote`
    
FROM `students`

INNER JOIN `exam_student`
ON `students`.`id` = `exam_student`.`student_id`

INNER JOIN `exams`
ON `exam_student`.`exam_id` = `exams`.`id`

WHERE `exam_student`.`vote` >= 18

GROUP BY student_id
ORDER BY student_id;
```
