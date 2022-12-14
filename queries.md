Dopo aver creato un nuovo database nel vostro phpMyAdmin e aver importato lo schema (stesso allegato di stamattina), eseguite le query qui sotto.


1. Selezionare tutti gli studenti nati nel 1990 (160)
```sql
SELECT * FROM `students` WHERE year(`date_of_birth`)=1990;
```

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
```sql
SELECT* FROM `courses` WHERE `cfu`> '10';
```

3. Selezionare tutti gli studenti che hanno più di 30 anni
```sql
SELECT * FROM `students` WHERE TIMESTAMPDIFF(year,date_of_birth,NOW()) >30;
```

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
```sql
SELECT * FROM `courses` WHERE `period`= 'I semestre' AND `year`= '1';
```

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
```sql
SELECT * FROM `exams` WHERE `date`= '2020-06-20' AND `hour` > '14:00:00';
```

6. Selezionare tutti i corsi di laurea magistrale (38)
```sql
SELECT * FROM `degrees` WHERE `level` = 'magistrale';
```

7. Da quanti dipartimenti è composta l'università? (12)
```sql
SELECT COUNT(id) FROM `departments`;
```

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
```sql
SELECT * FROM `teachers` WHERE `phone` is NULL;
```

Group by:
1. Contare quanti iscritti ci sono stati ogni anno
```sql
SELECT COUNT(id) as `total_enrolment`, year(`enrolment_date`) FROM `students` GROUP BY year(`enrolment_date`);
```

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```sql
SELECT COUNT(id) as `teachers_with_same_office`, `office_address` FROM `teachers` GROUP BY `office_address`;
```

3. Calcolare la media dei voti di ogni appello d'esame
```sql
SELECT AVG(`vote`) as `average_vote`, `exam_id` FROM `exam_student` GROUP BY `exam_id`;
```

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
```sql
SELECT COUNT(id) as `degree_amount`, `department_id`, `name`
FROM `degrees`
GROUP BY `department_id`;
```

Join:
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```sql
SELECT `students`.`*` 
FROM `students` 
JOIN `degrees` 
ON `students`.`degree_id`= `degrees`.`id` 
WHERE `degrees`.`name`= 'Corso di Laurea in Economia';
```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```sql
SELECT `degrees`.`name`,`degrees`.`level` 
FROM `degrees` 
JOIN `departments` 
ON `degrees`.`department_id` = `departments`.`id` 
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' AND `degrees`.`level` = 'magistrale';
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sql
SELECT `courses`.`id` as `course_id`,`courses`.`name`  as `course_name`,`courses`.`period` as `course_period`,`courses`.`description` as `course_description`,`courses`.`year` as `course_year`,`courses`.`cfu`,`courses`.`website` as `course_website`, `teachers`.`id` as `teachers_id`,`teachers`.`name` as `teachers_name`,`teachers`.`surname` as `teachers_surname`
FROM `course_teacher`
JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
JOIN `courses`
ON `course_teacher`.`course_id`= `courses`.`id`
WHERE `teachers`.`id` = 44;
```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```sql
SELECT `students`.`id` as `student_id`,`students`.`surname` as `student_surname`,`students`.`name` as `student_name`, `degrees`.`id` as `degree_id`,`degrees`.`name` as `degree_name`,`degrees`.`level` as `degree_level`,`degrees`.`address` as `degree_address`,`degrees`.`email` as `degree_email`,`degrees`.`website` as `degree_website`,`degrees`.`department_id` as `department_id`, `departments`.`name` as `department_name`
FROM `students` 
JOIN `degrees` 
ON `students`.`degree_id` = `degrees`.`id` 
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id` 
ORDER BY `students`.`surname`, `students`.`name`;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```sql
SELECT `degrees`.`id` as `degree_id`,`degrees`.`name` as `degree_name`, `courses`.`id` as `course_id`,`courses`.`name` as `course_name`,`teachers`.`id` as `teacher_id`,`teachers`.`name` as `teacher_name`,`teachers`.`surname` as `teacher_surname` 
FROM `course_teacher` 
JOIN `teachers` 
ON `course_teacher`.`teacher_id` = `teachers`.`id` 
JOIN `courses` 
ON `course_teacher`.`course_id` = `courses`.`id` 
JOIN `degrees` 
ON `courses`.`degree_id` = `degrees`.`id`;
```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```sql
SELECT DISTINCT `teachers`.`id` as `teacher_id`, `teachers`.`name` as `teacher_name`, `teachers`.`surname` as `teacher_surname`, `departments`.`id` as `department_id`, `departments`.`name` as `department_name` 
FROM `course_teacher` 
JOIN `teachers` 
ON `course_teacher`.`teacher_id` = `teachers`.`id` 
JOIN `courses` 
ON `course_teacher`.`course_id` = `courses`.`id` 
JOIN `degrees` 
ON `courses`.`degree_id` = `degrees`.`id` 
JOIN `departments` 
ON `degrees`.`department_id` = `departments`.`id` 
WHERE `departments`.`name` = 'Dipartimento di Matematica';
```

7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami
NON ANCORA COMPLETATO
```sql
SELECT `students`.`id` as `student_id`,`students`.`surname` as `student_surname`,`students`.`name` as `student_name`, `exams`.`id` as `exam_id`, `exam_student`.`vote`
FROM `exam_student` 
JOIN `students`
ON `exam_student`.`student_id` = `students`.`id`
JOIN `exams`
ON `exam_student`.`exam_id` = `exams`.`id` 
WHERE `exam_student`.`vote` >= 18;
```