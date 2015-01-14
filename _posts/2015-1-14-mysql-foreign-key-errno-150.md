---
layout: post
title: Mysql Foreign Key Error with errno 150
---

When execute `ALTER TABLE ADD FOREIGN KEY` or create table with foreign keys, 
you would get MySQL Foreign Key Problem (errno: 150). It means that a foreign 
key constraint was not correctly formed.

To avoid this error, please check:

* The two tables must be ENGINE=InnoDB. (can be others: ENGINE=MyISAM works too)
* The two tables must have the same charset.
* The PK column(s) in the parent table and the FK column(s) must be the same data type. (if the PRIMARY Key in the Parent table is UNSIGNED, be sure to select UNSIGNED in the Child Table field)
* The PK column(s) in the parent table and the FK column(s), if they have a define collation type, must have the same collation type;
* If the PK of the parent table is more than one field, the order of the fields in the FK must be the same as the order in the PK 
* If there is data already in the foreign key table, the FK column value(s) must match values in the parent table PK columns.
* And the child table cannot be a temporary table.
