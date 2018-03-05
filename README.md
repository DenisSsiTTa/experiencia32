
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






