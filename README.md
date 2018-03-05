
Denisse Velásquez

call_list=# CREATE DATABASE pintagram;
CREATE DATABASE
call_list=# \c pintagram;
psql (10.1, server 10.3)
You are now connected to database "pintagram" as user "prueba1".


pintagram=# CREATE TABLE users ( id_user SERIAL PRIMARY KEY, first_name varchar(20) NOT NULL, last_name varchar(20) NOT NULL, email varchar(50) NOT NULL);
CREATE TABLE
pintagram=# \d users
                                        Table "public.users"
   Column   |         Type          | Collation | Nullable |                Default                 
------------+-----------------------+-----------+----------+----------------------------------------
 id_user    | integer               |           | not null | nextval('users_id_user_seq'::regclass)
 first_name | character varying(20) |           | not null | 
 last_name  | character varying(20) |           | not null | 
 email      | character varying(50) |           | not null | 
Indexes:
    "users_pkey" PRIMARY KEY, btree (id_user)


pintagram=# CREATE TABLE tags ( id_tag SERIAL PRIMARY KEY, name_tag varchar(20) NOT NULL, desc_tag varchar(50));
CREATE TABLE
pintagram=# \d tags
                                      Table "public.tags"
  Column  |         Type          | Collation | Nullable |               Default                
----------+-----------------------+-----------+----------+--------------------------------------
 id_tag   | integer               |           | not null | nextval('tags_id_tag_seq'::regclass)
 name_tag | character varying(20) |           | not null | 
 desc_tag | character varying(50) |           |          | 
Indexes:
    "tags_pkey" PRIMARY KEY, btree (id_tag)
    


pintagram=# CREATE TABLE images ( id_image SERIAL PRIMARY KEY, desc_image VARCHAR(50), data_image BYTEA, date_up DATE, user_id INTEGER REFERENCES users(id_user));
CREATE TABLE
pintagram=# \d images
                                        Table "public.images"
   Column   |         Type          | Collation | Nullable |                 Default                  
------------+-----------------------+-----------+----------+------------------------------------------
 id_image   | integer               |           | not null | nextval('images_id_image_seq'::regclass)
 desc_image | character varying(50) |           |          | 
 data_image | bytea                 |           |          | 
 date_up    | date                  |           |          | 
 user_id    | integer               |           |          | 
Indexes:
    "images_pkey" PRIMARY KEY, btree (id_image)
Foreign-key constraints:
    "images_user_id_fkey" FOREIGN KEY (user_id) REFERENCES users(id_user)


pintagram=# CREATE TABLE likes ( id_user INTEGER REFERENCES users(id_user),id_image INTEGER REFERENCES images(id_image), PRIMARY KEY (id_user, id_image));
CREATE TABLE
pintagram=# \d likes
                Table "public.likes"
  Column  |  Type   | Collation | Nullable | Default 
----------+---------+-----------+----------+---------
 id_user  | integer |           | not null | 
 id_image | integer |           | not null | 
Indexes:
    "likes_pkey" PRIMARY KEY, btree (id_user, id_image)
Foreign-key constraints:
    "likes_id_image_fkey" FOREIGN KEY (id_image) REFERENCES images(id_image)
    "likes_id_user_fkey" FOREIGN KEY (id_user) REFERENCES users(id_user)



pintagram=# CREATE TABLE imgtag ( id_tag INTEGER REFERENCES tags(id_tag),id_image INTEGER REFERENCES images(id_image), PRIMARY KEY (id_tag, id_image));
CREATE TABLE
pintagram=# \d imgtag
                Table "public.imgtag"
  Column  |  Type   | Collation | Nullable | Default 
----------+---------+-----------+----------+---------
 id_tag   | integer |           | not null | 
 id_image | integer |           | not null | 
Indexes:
    "imgtag_pkey" PRIMARY KEY, btree (id_tag, id_image)
Foreign-key constraints:
    "imgtag_id_image_fkey" FOREIGN KEY (id_image) REFERENCES images(id_image)
    "imgtag_id_tag_fkey" FOREIGN KEY (id_tag) REFERENCES tags(id_tag)


pintagram=# \dt
         List of relations
 Schema |  Name  | Type  |  Owner  
--------+--------+-------+---------
 public | images | table | prueba1
 public | imgtag | table | prueba1
 public | likes  | table | prueba1
 public | tags   | table | prueba1
 public | users  | table | prueba1
(5 rows)








CREATE TABLE
call_list=# \dt
        List of relations
 Schema | Name  | Type  |  Owner  
--------+-------+-------+---------
 public | users | table | prueba1
(1 row)


3) Ingresar un usuario, llamado Carlos (el resto de los datos deben inventarlos).
4) Ingresar un usuario, llamada Laura (el resto de los datos deben inventarlos).

call_list=# INSERT INTO users (first_name, email) VALUES ('Carlos', 'carlos@desafio.uk'), ('Laura','laura@desafio.uk');
INSERT 0 2
call_list=# select * from users;
 id | first_name |       email       
----+------------+-------------------
  1 | Carlos     | carlos@desafio.uk
  2 | Laura      | laura@desafio.uk
(2 rows)


5) Crear una tabla llamada calls con los campos id (clave primaria), phone, date, user_id (foreign key relacionado a users).


call_list=# CREATE TABLE calls ( id SERIAL PRIMARY KEY, phone integer, date date, user_id integer references users(id));
ERROR:  there is no unique constraint matching given keys for referenced table "users"
call_list=# alter table users add primary key (id);
ALTER TABLE
call_list=# CREATE TABLE calls ( id SERIAL PRIMARY KEY, phone integer, date date, user_id integer references users(id));
CREATE TABLE
call_list=# \dt
        List of relations
 Schema | Name  | Type  |  Owner  
--------+-------+-------+---------
 public | calls | table | prueba1
 public | users | table | prueba1
(2 rows)



6) Agregar a la tabla users el campo last_name.

call_list=# ALTER TABLE users ADD COLUMN last_name varchar(40);
ALTER TABLE
call_list=# select * from users;
 id | first_name |       email       | last_name 
----+------------+-------------------+-----------
  1 | Carlos     | carlos@desafio.uk | 
  2 | Laura      | laura@desafio.uk  | 
(2 rows)


7) Ingresar el apellido del usuario Carlos.
8) Ingresar el apellido del usuario Laura.

call_list=# UPDATE users SET last_name = 'Caszely' where first_name = 'Carlos';
UPDATE 1
call_list=# UPDATE users SET last_name = 'Bozzo' where first_name = 'Laura';
UPDATE 1
call_list=# select * from users;
 id | first_name |       email       | last_name 
----+------------+-------------------+-----------
  1 | Carlos     | carlos@desafio.uk | Caszely
  2 | Laura      | laura@desafio.uk  | Bozzo
(2 rows)



9) Ingresar 6 llamadas asociadas al usuario Laura.
10) Ingresar 4 llamadas asociadas al usuario Carlos.


call_list=# INSERT INTO calls (phone, date, user_id) VALUES (657577752, '2018-02-21 04:00:00',2), (657577752, '2017-12-25 00:00:00',2), (657577752, '2017-12-01 22:05:00',2), (657577752, '2018-03-05 11:05:00',2), (657577752, '2018-01-01 04:05:06',2), (657577752, '2018-01-08 10:07:00',2);
INSERT 0 6
call_list=# select * from calls
call_list-# ;
 id |   phone   |    date    | user_id 
----+-----------+------------+---------
  1 | 657577752 | 2018-02-21 |       2
  2 | 657577752 | 2017-12-25 |       2
  3 | 657577752 | 2017-12-01 |       2
  4 | 657577752 | 2018-03-05 |       2
  5 | 657577752 | 2018-01-01 |       2
  6 | 657577752 | 2018-01-08 |       2
(6 rows)

call_list=# INSERT INTO calls (phone, date, user_id) VALUES (94832811, '2018-02-02 04:00:00',1), (94832811, '2017-12-25 00:01:00',1), (94832811, '2017-12-26 22:05:00',1), (94832811, '2018-02-22 11:05:00',1);
INSERT 0 4
call_list=# select * from calls                                                                                             ;
 id |   phone   |    date    | user_id 
----+-----------+------------+---------
  1 | 657577752 | 2018-02-21 |       2
  2 | 657577752 | 2017-12-25 |       2
  3 | 657577752 | 2017-12-01 |       2
  4 | 657577752 | 2018-03-05 |       2
  5 | 657577752 | 2018-01-01 |       2
  6 | 657577752 | 2018-01-08 |       2
  7 |  94832811 | 2018-02-02 |       1
  8 |  94832811 | 2017-12-25 |       1
  9 |  94832811 | 2017-12-26 |       1
 10 |  94832811 | 2018-02-22 |       1
(10 rows)


11) Crear un nuevo usuario.

call_list=# INSERT INTO users (first_name, email, last_name) VALUES ('Juanin', 'Juanin@31minutos.cl', 'Juan Jarry')
call_list-# ;
INSERT 0 1
call_list=# select * from users;
 id | first_name |        email        | last_name  
----+------------+---------------------+------------
  1 | Carlos     | carlos@desafio.uk   | Caszely
  2 | Laura      | laura@desafio.uk    | Bozzo
  3 | Juanin     | Juanin@31minutos.cl | Juan Jarry
(3 rows)



12) Seleccionar la cantidad de llamados de cada uno de los usuarios (nombre de usuario y cantidad de llamadas).

call_list=# select "first_name" || ' ' || "last_name" as user, count(calls.id) from users LEFT JOIN calls ON (users.id = calls.user_id) group by first_name, last_name;
       user        | count 
-------------------+-------
 Juanin Juan Jarry |     0
 Carlos Caszely    |     4
 Laura Bozzo       |     6
(3 rows)


13) Seleccionar los llamados del usuario llamado Carlos ordenados por fecha en orden descendente.

call_list=# select "first_name" || ' ' || "last_name" as user, date from users LEFT JOIN calls ON (users.id = calls.user_id) where first_name = 'Carlos' order by date DESC;
      user      |    date    
----------------+------------
 Carlos Caszely | 2018-02-22
 Carlos Caszely | 2018-02-02
 Carlos Caszely | 2017-12-26
 Carlos Caszely | 2017-12-25
(4 rows)


*** Nuevos cambios solicitados por cliente.
"Necesito agregar a la base una tabla de auditoría que registre el motivo del borrado de una llamada y el usuario que lo efectuó."


call_list=# CREATE TABLE auditory (id_audit SERIAL PRIMARY KEY, message varchar(50), user_id integer REFERENCES users(id), call_id integer);
CREATE TABLE
call_list=# \d auditory
                                       Table "public.auditory"
  Column  |         Type          | Collation | Nullable |                  Default                   
----------+-----------------------+-----------+----------+--------------------------------------------
 id_audit | integer               |           | not null | nextval('auditory_id_audit_seq'::regclass)
 message  | character varying(50) |           |          | 
 user_id  | integer               |           |          | 
 call_id  | integer               |           |          | 
Indexes:
    "auditory_pkey" PRIMARY KEY, btree (id_audit)
Foreign-key constraints:
    "auditory_user_id_fkey" FOREIGN KEY (user_id) REFERENCES users(id)



A partir de la base anterior, agregar este requerimiento y modelar la base de datos (agregar print de pantalla [utilizar https://www.draw.io/]).