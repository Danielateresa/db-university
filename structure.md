Modellizzare la struttura di una tabella per memorizzare tutti i dati riguardanti una università:
sono presenti diversi **Dipartimenti** (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
ogni Dipartimento offre più **Corsi di Laurea** (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
ogni Corso di Laurea prevede diversi **Corsi** (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
ogni Corso può essere tenuto da diversi **Insegnanti**;
ogni Corso prevede più **appelli d'Esame**;
ogni **Studente** è iscritto ad un solo **Corso di Laurea**;
ogni Studente può iscriversi a più appelli di Esame;
per ogni appello d'Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente. Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.


Tables:

Dipartimenti:
- id  | PK  BIGINT  AI, UNIQUE, NOT NULL  INDEX
- nome | VARCHAR(60)  NOT NULL
- descrizione | TEXT  NULL

Studenti:
- id | PK | BIGINT  AI, UNIQUE, NOT NULL  INDEX
- nome  | VARCHAR(20)  NOT NULL
- cognome  | VARCHAR(20)  NOT NULL
- email  | VARCHAR(30)  NULL
- numero | VARCHAR(30)  NULL
- matricola | VARCHAR(10)  NOT NULL 
- corso_laurea_id | FK  BIGINT 

Corsi:
- id | PK | BIGINT  AI, UNIQUE, NOT NULL  INDEX
- ore | SMALLINT  NOT NULL
- docenti_id FK BIGINT NULL
- numero_aula VARCHAR(5) NULL
- nome_corso VARCHAR(30) NOT NULL
- programma TEXT NOT NULL
- modalità-frequenza VARCHAR(20) NULL

Corsi di Laurea:
- id | PK  BIGINT  AI, UNIQUE, NOT NULL  INDEX
- corso_id FK BIGINT
- programma TEXT NOT NULL
- durata VARCHAR(10) NULL
- data_inizio DARE NOT NULL
- sede VARCHAR(20) NOT NULL
- dipartimento_id KK BIGINT
- lingua  VARCHAR(20) NOT NULL


Insegnanti:
- id | PK  BIGINT  AI, UNIQUE, NOT NULL  INDEX
- corso_id FK BIGINT
- nome VARCHAR(20) NOT NULL
- cognome VARCHAR(20) NOT NULL


Appelli:
- id | PK | BIGINT | AI, UNIQUE, NOT NULL | INDEX
- nome VARCHAR(20) NOT NULL
- corso_id FK BIGINT
- data DATE NOT NULL
- numero_aula VARCHAR(5) NULL
- tipologia VARCHAR(20) NULL

Appelli_studente: (pivot)
- appello_id FK
- studente_id FK
- appello_status TINYINT NULL
- voto DECIMAL(3,1) NULL

Corsi_insegnanti: (pivot)
- insegnanti_id FK
- corso_id FK



Relazioni:
oneToMany => Dipartimenti & Corsi_di_laurea
oneToMany => Corso_di_laurea & Corsi
manyToMany => Corsi & Insegnanti (pivot)
oneToMany => Corso & Studente
oneToMany => Corso & Appelli 
manyToMany => Appelli & Studente (pivot)








