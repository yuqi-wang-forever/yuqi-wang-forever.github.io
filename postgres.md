```bash
mydb=# select now();
              now
-------------------------------
 2023-05-12 16:51:58.520862+00
(1 row)

mydb=# select now()::DATE;
    now
------------
 2023-05-12
(1 row)

mydb=# select now()::time;
       now
-----------------
 16:53:00.979864
(1 row)

mydb=# select now()::time with time zone;
        now
--------------------
 16:54:18.255087+00
(1 row)

mydb=# select now()::timestamp;
            now
----------------------------
 2023-05-12 16:54:43.112131
(1 row)

mydb=# select now()::timestamp with time zone;
              now
-------------------------------
 2023-05-12 16:55:04.390665+00
(1 row)
```

防止除零操作返回错误，返回null
```sql
select caolesce(10/nullif(0,0))
```
```bash
select nullif(0,0) #返回null
select nullif(2,9) #返回2 第一个参数
```
使用**interval**配合 + -运算符更改时间
```bash
mydb=# select (now() + interval '8 hours')::date;
    date
------------
 2023-05-13
(1 row)
mydb=# select now()::date + interval '1 year';
      ?column?
---------------------
 2024-05-12 00:00:00
(1 row)

mydb=# select now()::date + interval '1 hour';
      ?column?
---------------------
 2023-05-12 01:00:00
(1 row)

mydb=# select now()::date + interval '8 hours';
      ?column?
---------------------
 2023-05-12 08:00:00
(1 row)
```
从now()中提取日期信息
```bash
mydb=# select extract(year from now());
 extract
---------
    2023
(1 row)

mydb=# select extract(month from now());
 extract
---------
       5
(1 row)

mydb=# select extract(day from now());
 extract
---------
      13
(1 row)

mydb=# select extract(hour from now());
 extract
---------
       1
(1 row)

mydb=# select extract(secend from now());
ERROR:  unit "secend" not recognized for type timestamp with time zone
mydb=# select extract(down from now());
ERROR:  unit "down" not recognized for type timestamp with time zone
mydb=# select extract(dow from now()); #星期几
 extract
---------
       6
(1 row)
mydb=# select extract(dow from now() + interval'1 day'); #0-6代表 日-六
 extract
---------
       0
(1 row)

```
age()查询出年龄
```bash
mydb=# select first_name,last_name,gender,country_of_birth,date_of_birth,age(now(),date_of_birth) age from person limit
5 offset 0;
 first_name |  last_name  | gender | country_of_birth | date_of_birth |                   age
------------+-------------+--------+------------------+---------------+-----------------------------------------
 Alecia     | Di Domenico | Female | China            | 1961-03-28    | 62 years 1 mon 16 days 01:38:52.803322
 Kara-lynn  | Lyne        | Female | Portugal         | 2009-09-09    | 13 years 8 mons 4 days 01:38:52.803322
 Torey      | McClancy    | Female | Indonesia        | 1978-05-04    | 45 years 9 days 01:38:52.803322
 Pia        | Dummett     | Female | Slovenia         | 1967-12-21    | 55 years 4 mons 23 days 01:38:52.803322
 Vivi       | Gammel      | Female | Ukraine          | 1985-10-31    | 37 years 6 mons 13 days 01:38:52.803322
(5 rows)

mydb=# select first_name,last_name,gender,country_of_birth,date_of_birth,extract(year from age(now(),date_of_birth)) age
 from person limit 5 offset 0;
 first_name |  last_name  | gender | country_of_birth | date_of_birth | age
------------+-------------+--------+------------------+---------------+-----
 Alecia     | Di Domenico | Female | China            | 1961-03-28    |  62
 Kara-lynn  | Lyne        | Female | Portugal         | 2009-09-09    |  13
 Torey      | McClancy    | Female | Indonesia        | 1978-05-04    |  45
 Pia        | Dummett     | Female | Slovenia         | 1967-12-21    |  55
 Vivi       | Gammel      | Female | Ukraine          | 1985-10-31    |  37
(5 rows)

mydb=#
```

分页
```bash
mydb=# select * from person limit 1;
 id | first_name |  last_name  |            email             | gender | date_of_birth | country_of_birth
----+------------+-------------+------------------------------+--------+---------------+------------------
  1 | Alecia     | Di Domenico | adidomenicorr@deviantart.com | Female | 1961-03-28    | China
(1 row)

mydb=# select * from person limit 2;
 id | first_name |  last_name  |            email             | gender | date_of_birth | country_of_birth
----+------------+-------------+------------------------------+--------+---------------+------------------
  1 | Alecia     | Di Domenico | adidomenicorr@deviantart.com | Female | 1961-03-28    | China
  2 | Kara-lynn  | Lyne        | klyne0@nih.gov               | Female | 2009-09-09    | Portugal
  
(2 rows)
mydb=# select * from person limit 5 offset 10;
 id | first_name |  last_name  |           email           | gender | date_of_birth | country_of_birth
----+------------+-------------+---------------------------+--------+---------------+------------------
 11 | Burl       | Krysztofiak |                           | Male   | 2019-08-21    | Oman
 12 | Cordell    | Mc Caghan   | cmccaghana@macromedia.com | Male   | 1968-01-18    | Poland
 13 | Serge      | Gwynn       | sgwynnb@eepurl.com        | Male   | 1961-01-13    | Philippines
 14 | Jaye       | Timblett    | jtimblettc@dedecms.com    | Male   | 2000-12-08    | Belarus
 15 | Dudley     | Caneo       |                           | Male   | 1979-03-27    | Yemen
(5 rows)
```

没有主键primary key 导致两条一样的记录
```bash
mydb=# \d car;
                       Table "public.car"
 Column |         Type          | Collation | Nullable | Default
--------+-----------------------+-----------+----------+---------
 id     | integer               |           |          |
 make   | character varying(50) |           |          |
 model  | character varying(50) |           |          |
 price  | character varying(50) |           |          |

mydb=# select * from car where id = 1;
 id | make |  model   |   price
----+------+----------+------------
  1 | BMW  | 5 Series | $322515.36
  1 | BMW  | 5 Series | $322515.36
```
删除主键
```bash
mydb=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           | not null |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)

mydb=# alter table person drop constraint person_pkey;
ALTER TABLE
mydb=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           | not null |
```
添加主键 有多行重复时添加主键会失败 需删除 主键可设置多个列名 称为联合主键/复合主键
```bash
mydb=# alter table person add primary key(id,email)

mydb=# alter table person add primary key(id,email);
ERROR:  column "email" of relation "person" contains null values
mydb=# alter table person add primary key(id);
ALTER TABLE
mydb=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           | not null |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)

mydb=#

```
唯一索引
```bash
mydb=# delete from person where id=1001;
DELETE 1
mydb=# select email,count(*) from person group by email having count(*)>1;
 email | count
-------+-------
       |   330
(1 row)

mydb=#
mydb=# select email,count(*) from person group by email;
                email                | count
-------------------------------------+-------
 mrowany@nhs.uk                      |     1
 wnice7r@technorati.com              |     1
 lkramer4d@tiny.cc                   |     1
 ghayfield2z@live.com                |     1
                                     |   330
 drobeyh5@theguardian.com            |     1
 bstonehousejg@deliciousdays.com     |     1
 kzottoli39@theguardian.com          |     1
 rakast31@google.ca                  |     1
 thasell6r@google.com.hk             |     1
 dollander5@storify.com              |     1
 cmingardon4@hibu.com                |     1
```
给空数据填充字符串
```bash
mydb=# select coalesce(1);
 coalesce
----------
        1
(1 row)

mydb=# select coalesce(null,'','ops is null') as email;
 email
-------

(1 row)

mydb=# select coalesce(null,'ops is null') as email;
    email
-------------
 ops is null
(1 row)

mydb=# select coalesce(null,'ops is null') email;
    email
-------------
 ops is null
(1 row)

mydb=# select coalesce(null,'ops is null') email2;
   email2
-------------
 ops is null
(1 row)

```
唯一索引
```bash
                           ^
mydb=# alter table person add constraint person_email_index_unique unique(email);
ALTER TABLE
mydb=# d\ person
invalid command \
Try \? for help.
mydb-# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           | not null |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)
    "person_email_index_unique" UNIQUE CONSTRAINT, btree (email)

mydb=# alter table person drop constraint person_email_index_unique;
ALTER TABLE
mydb=# alter table person add unique(email);
ALTER TABLE
mydb=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           | not null |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)
    "person_email_key" UNIQUE CONSTRAINT, btree (email)
```
检查约束
```bash
mydb=# select distinct gender from person;
   gender
-------------
 Genderqueer
 Bigender
 Genderfluid
 Male
 Polygender
 Non-binary
 Female
 Agender
(8 rows)

mydb=# alter table person add constraint person_check_gender check ( gender='Male' or gender = 'Female' or gender = 'Agender' or gender = 'Bigender' or gender = 'Genderqueer' or gender='Genderfluid' or gender='Polygender' or gender = 'Non-binary');
ALTER TABLE
mydb=# \d person
                                         Table "public.person"
      Column      |          Type          | Collation | Nullable |              Default
------------------+------------------------+-----------+----------+------------------------------------
 id               | bigint                 |           | not null | nextval('person_id_seq'::regclass)
 first_name       | character varying(50)  |           | not null |
 last_name        | character varying(50)  |           | not null |
 email            | character varying(150) |           |          |
 gender           | character varying(50)  |           | not null |
 date_of_birth    | date                   |           | not null |
 country_of_birth | character varying(50)  |           | not null |
Indexes:
    "person_pkey" PRIMARY KEY, btree (id)
    "person_email_key" UNIQUE CONSTRAINT, btree (email)
Check constraints:
    "person_check_gender" CHECK (gender::text = 'Male'::text OR gender::text = 'Female'::text OR gender::text = 'Agender'::text OR gender::text = 'Bigender'::text OR gender::text = 'Genderqueer'::text OR gender::text = 'Genderfluid'::text OR gender::text = 'Polygender'::text OR gender::text = 'Non-binary'::text)

mydb=#
```
删除操作
```bash
mydb=# delete from person; #删除整张表
```
修改
```bash
update person set email='Jack@stargate.tv',gender='male' where name = 'Jack';
```
添加已经存在的时候 在sql语句后加on conflict (id) do nothing防止错误（只对唯一约束的列，主键有效）
```bash
mydb=# insert into person(id,first_name,last_name,email,gender,date_of_birth,country_of_birth) values (1,'Alecia','Di Do
menico','adidomenicorr@deviantart.com','Female','1961-03-28','China');
ERROR:  duplicate key value violates unique constraint "person_pkey"
DETAIL:  Key (id)=(1) already exists.
mydb=# insert into person(id,first_name,last_name,email,gender,date_of_birth,country_of_birth) values (1,'Alecia','Di Domenico','adidomenicorr@deviantart.com','Female','1961-03-28','China') on conflict(id) do nothing;
INSERT 0 0
mydb=# insert into person(id,first_name,last_name,email,gender,date_of_birth,country_of_birth) values (1,'Alecia','Di Domenico','adidomenicorr@deviantart.com','Female','1961-03-28','China') on conflict(email) do nothing;
INSERT 0 0
mydb=# insert into person(id,first_name,last_name,email,gender,date_of_birth,country_of_birth) values (1,'Alecia','Di Domenico','adidomenicorr@deviantart.com','Female','1961-03-28','China') on conflict(gender) do nothing;
ERROR:  there is no unique or exclusion constraint matching the ON CONFLICT specification
mydb=#
```
添加on conflict(id) do update ---
```bash
mydb=# insert into person(id,first_name,last_name,email,gender,date_of_birth,country_of_birth) values (1,'Alecia','Di Domenico','adidomenicorr@deviantart.com','Female','1961-03-28','China') on conflict(id) do update set email='alecia@gmail. com.uk',country_of_birth='UK';
INSERT 0 1
mydb=# select * from person where id = 1;
 id | first_name |  last_name  |        email        | gender | date_of_birth | country_of_birth
----+------------+-------------+---------------------+--------+---------------+------------------
  1 | Alecia     | Di Domenico | alecia@gmail.com.uk | Female | 1961-03-28    | UK
(1 row)
第二种方式
mydb=# insert into person(id,first_name,last_name,email,gender,date_of_birth,country_of_birth) values (1,'Alecia','Carter','adidomenicorr@deviantart.com.uk','Female','1961-03-28','China') on conflict(id) do update set email=excluded.email,
last_name = excluded.last_name;
INSERT 0 1
mydb=# select * from person where id = 1;
 id | first_name | last_name |              email              | gender | date_of_birth | country_of_birth
----+------------+-----------+---------------------------------+--------+---------------+------------------
  1 | Alecia     | Carter    | adidomenicorr@deviantart.com.uk | Female | 1961-03-28    | UK
(1 row)

mydb=#

```
**Join 只会对主键和外键同时存在的时候将记录查询出来**

```bash
select * from person_id_seq #查看序列信息
select nextval('person_id_seq'); #让序列的值加一
alter sequence person_id_seq restart with 10;#将序列设置为从10开始
```
**设置使用级联可以免除删除记录时的外键约束**
```bash
delete from person;会删除表中所有记录
delete from person where 字句

```
拓展
```txt

安装plv8 可以编写JS函数
```
UUID: Universally unique identifier
```bash
mydb=# create extension if not exists "uuid-ossp";
CREATE EXTENSION
# 使用uuid作为表主键
insert person(id,first_name,last_name,gender,email,date_of_birth,country_of_birth) values(uuid_generate_v4,--------);
```

```bash
select * from person join car on person.car_uid=car.car_uid; 
# 当两个表中的外键字段一样 可改为
select * from person (left/right/innner) join car using (car_uid)
```
