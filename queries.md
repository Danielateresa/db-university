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
SELECT `students`.`*` FROM `students` JOIN `degrees` ON `students`.`degree_id`= `degrees`.`id` WHERE `degrees`.`name`= 'Corso di Laurea in Economia';
```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```sql
SELECT `degrees`.`name`,`degrees`.`level` FROM `degrees` JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' AND `degrees`.`level` = 'magistrale';
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sql
SELECT `courses`.`id`,`courses`.`name`,`courses`.`period`,`courses`.`description`,`courses`.`year`,`courses`.`cfu`,`courses`.`website`, `teachers`.`id`,`teachers`.`name`,`teachers`.`surname`
FROM `course_teacher`
JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
JOIN `courses`
ON `course_teacher`.`course_id`= `courses`.`id`
WHERE `teachers`.`id` = 44;
```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```sql
SELECT `students`.`id`,`students`.`surname`,`students`.`name`, `degrees`.`id`,`degrees`.`id`,`degrees`.`name`,`degrees`.`level`,`degrees`.`id`,`degrees`.`address`,`degrees`.`id`,`degrees`.`email`,`degrees`.`website`,`degrees`.`department_id` FROM `students` JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id` ORDER BY `students`.`surname`, `students`.`name`;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti