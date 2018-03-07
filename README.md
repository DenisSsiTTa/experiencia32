
Alumno: Denisse Velásquez Salazar
mail: denisse.velasquez@gmail.com


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



-- Se insertan usuarios
pintagram=# INSERT INTO users (first_name, last_name, email) VALUES ('Juanin Juan','Jarry','jjj@31minutos.cl');
INSERT 0 1
pintagram=# INSERT INTO users (first_name, last_name, email) VALUES ('Juan Carlos','Bodoque','jc@31minutos.cl');
INSERT 0 1
pintagram=# INSERT INTO users (first_name, last_name, email) VALUES ('Tulio','Triviño Tufillo','ttt@31minutos.cl');
INSERT 0 1
pintagram=# INSERT INTO users (first_name, last_name, email) VALUES ('Policarpo','Avendaño','pa@31minutos.cl');
INSERT 0 1
pintagram=# select * from users;
 id_user | first_name  |    last_name    |      email       
---------+-------------+-----------------+------------------
       1 | Juanin Juan | Jarry           | jjj@31minutos.cl
       2 | Juan Carlos | Bodoque         | jc@31minutos.cl
       3 | Tulio       | Triviño Tufillo | ttt@31minutos.cl
       4 | Policarpo   | Avendaño        | pa@31minutos.cl
(4 rows)


-- Ingresar 2 imágenes por usuario

pintagram=# INSERT INTO images (desc_image, data_image, date_up, user_id) VALUES ('Juanin Formal', pg_read_binary_file('lo_import/jjh1.jpeg')::bytea, CURRENT_DATE, 1);
INSERT 0 1
pintagram=# INSERT INTO images (desc_image, data_image, date_up, user_id) VALUES ('Juanin Informal', pg_read_binary_file('lo_import/jjh2.jpeg')::bytea, CURRENT_DATE, 1);
INSERT 0 1
pintagram=# INSERT INTO images (desc_image, data_image, date_up, user_id) VALUES ('Juan Carlos Formal', pg_read_binary_file('lo_import/jc1.jpeg')::bytea, CURRENT_DATE, 2);
INSERT 0 1
pintagram=# INSERT INTO images (desc_image, data_image, date_up, user_id) VALUES ('Juan Carlos informal', pg_read_binary_file('lo_import/jc2.png')::bytea, CURRENT_DATE, 2);
INSERT 0 1
pintagram=# INSERT INTO images (desc_image, data_image, date_up, user_id) VALUES ('Tulio Formal', pg_read_binary_file('lo_import/ttt1.jpeg')::bytea, CURRENT_DATE, 3);
INSERT 0 1
pintagram=# INSERT INTO images (desc_image, data_image, date_up, user_id) VALUES ('Tulio Informal', pg_read_binary_file('lo_import/ttt2.jpeg')::bytea, CURRENT_DATE, 3);
INSERT 0 1
pintagram=# INSERT INTO images (desc_image, data_image, date_up, user_id) VALUES ('Policarpo Formal', pg_read_binary_file('lo_import/pa1.jpeg')::bytea, CURRENT_DATE, 4);
INSERT 0 1
pintagram=# INSERT INTO images (desc_image, data_image, date_up, user_id) VALUES ('Policarpo Informal', pg_read_binary_file('lo_import/pa2.jpeg')::bytea, CURRENT_DATE, 4);
INSERT 0 1
pintagram=# select * from images;
pintagram=# 
pintagram=# select id_image, desc_image, date_up, user_id from images;
 id_image |      desc_image      |  date_up   | user_id 
----------+----------------------+------------+---------
       14 | Juanin Formal        | 2018-03-07 |       1
       15 | Juanin Informal      | 2018-03-07 |       1
       16 | Juan Carlos Formal   | 2018-03-07 |       2
       17 | Juan Carlos informal | 2018-03-07 |       2
       18 | Tulio Formal         | 2018-03-07 |       3
       19 | Tulio Informal       | 2018-03-07 |       3
       20 | Policarpo Formal     | 2018-03-07 |       4
       21 | Policarpo Informal   | 2018-03-07 |       4
(8 rows)


-- Ingresar 3 likes por cada imagen

pintagram=# INSERT INTO likes (id_user, id_image) values (1,14);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (2,14);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (3,14);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (1,15);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (2,15);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (3,15);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (1,16);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (2,16);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (3,16);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (1,17);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (2,17);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (3,17);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (1,18);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (2,18);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (3,18);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (2,19);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (3,19);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (4,19);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (2,20);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (3,20);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (4,20);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (2,21);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (3,21);
INSERT 0 1
pintagram=# INSERT INTO likes (id_user, id_image) values (4,21);
INSERT 0 1
pintagram=# select * from likes;
 id_user | id_image 
---------+----------
       1 |       14
       2 |       14
       3 |       14
       1 |       15
       2 |       15
       3 |       15
       1 |       16
       2 |       16
       3 |       16
       1 |       17
       2 |       17
       3 |       17
       1 |       18
       2 |       18
       3 |       18
       2 |       19
       3 |       19
       4 |       19
       2 |       20
       3 |       20
       4 |       20
       2 |       21
       3 |       21
       4 |       21
(24 rows)


-- Ingresar 8 tags

pintagram=# INSERT INTO tags (name_tag, desc_tag) values ('formal', 'imagen formal');
INSERT 0 1
pintagram=# INSERT INTO tags (name_tag, desc_tag) values ('informal', 'imagen informal');
INSERT 0 1
pintagram=# INSERT INTO tags (name_tag, desc_tag) values ('no oficial', 'imagen no oficial');
INSERT 0 1
pintagram=# INSERT INTO tags (name_tag, desc_tag) values ('dibujo', 'dibujo de fans');
INSERT 0 1
pintagram=# INSERT INTO tags (name_tag, desc_tag) values ('love', '');
INSERT 0 1
pintagram=# INSERT INTO tags (name_tag, desc_tag) values ('nature', '');
INSERT 0 1
pintagram=# INSERT INTO tags (name_tag, desc_tag) values ('smile', '');
INSERT 0 1
pintagram=# INSERT INTO tags (name_tag, desc_tag) values ('happy', '');
INSERT 0 1
pintagram=# select * from tags;
 id_tag |  name_tag  |     desc_tag      
--------+------------+-------------------
      1 | formal     | imagen formal
      2 | informal   | imagen informal
      3 | no oficial | imagen no oficial
      4 | dibujo     | dibujo de fans
      5 | love       | 
      6 | nature     | 
      7 | smile      | 
      8 | happy      | 
(8 rows)

-- Ingresar 3 tags por imagen

pintagram=# INSERT INTO imgtag (id_tag, id_image) values (1,14);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (2,14);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (3,14);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (1,15);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (2,15);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (3,15);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (6,16);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (7,16);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (8,16);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (3,17);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (4,17);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (5,17);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (1,18);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (2,18);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (3,18);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (2,19);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (3,19);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (4,19);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (2,20);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (5,20);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (7,20);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (8,21);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (3,21);
INSERT 0 1
pintagram=# INSERT INTO imgtag (id_tag, id_image) values (4,21);
INSERT 0 1
pintagram=# select * from imgtag;
 id_tag | id_image 
--------+----------
      1 |       14
      2 |       14
      3 |       14
      1 |       15
      2 |       15
      3 |       15
      6 |       16
      7 |       16
      8 |       16
      3 |       17
      4 |       17
      5 |       17
      1 |       18
      2 |       18
      3 |       18
      2 |       19
      3 |       19
      4 |       19
      2 |       20
      5 |       20
      7 |       20
      8 |       21
      3 |       21
      4 |       21
(24 rows)


-- Crear una consulta que muestre el nombre de la imagen y la cantidad de likes que tiene esa imagen.

pintagram=# select desc_image, count(likes.id_user) from images LEFT JOIN likes ON (images.id_image = likes.id_image) group by desc_image;
      desc_image      | count 
----------------------+-------
 Juan Carlos Formal   |     3
 Tulio Informal       |     3
 Juan Carlos informal |     3
 Policarpo Formal     |     3
 Juanin Formal        |     3
 Tulio Formal         |     3
 Juanin Informal      |     3
 Policarpo Informal   |     3
(8 rows)

-- Crear una consulta que muestre el nombre del usuario y los nombres de las fotos que le pertenecen.

pintagram=# select "first_name" || ' ' || "last_name" as user, desc_image from users, images where users.id_user = images.user_id;
         user          |      desc_image      
-----------------------+----------------------
 Juanin Juan Jarry     | Juanin Formal
 Juanin Juan Jarry     | Juanin Informal
 Juan Carlos Bodoque   | Juan Carlos Formal
 Juan Carlos Bodoque   | Juan Carlos informal
 Tulio Triviño Tufillo | Tulio Formal
 Tulio Triviño Tufillo | Tulio Informal
 Policarpo Avendaño    | Policarpo Formal
 Policarpo Avendaño    | Policarpo Informal
(8 rows)


-- Crear una consulta que muestre el nombre del tag y la cantidad de imagenes asociadas a ese tag.

pintagram=# select name_tag, count(imgtag.id_image) from tags LEFT JOIN imgtag ON (tags.id_tag = imgtag.id_tag) group by name_tag;
  name_tag  | count 
------------+-------
 happy      |     2
 smile      |     2
 no oficial |     6
 dibujo     |     3
 formal     |     3
 nature     |     1
 love       |     2
 informal   |     5
(8 rows)
















