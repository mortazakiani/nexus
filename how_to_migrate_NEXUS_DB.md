# nexus_repo_update
how  to  upgrade  nexus repository   from 3.70.x to 3.71.x with sql  migrate
# how to update step by  step 
  - before start : The migrator utility requires three times the disk space (minimum 10 GB) as your $data-dir/db directory.
  - before start : You must have at least 16GB RAM available.
  - first  step is  upgarde  to 3.70.X</br>
  > [!IMPORTANT]
  > Provision an external PostgreSQL database (external mode only); ensure that it uses UTF8 encoding</br>
  - secend step Obtain the Database Migrator utility, which you can download from (https://help.sonatype.com/en/download.html)</br>
  - third  ins Ensure you are using OpenJDK 8 or 11</br>
  > [!IMPORTANT]
  > If you are migrating from OrientDB, you must migrate your database while using Java 8 or 11. You can upgrade to Java 17 after you are off of OrientDB.</br>
 - fourth Perform a full backup using normal backup procedures this not  include backup  vm or anything  else </br>
 > [!NOTE]
 > how  to get  backup: https://help.sonatype.com/en/backup-and-restore.html</br>
 - fifth Shut down Nexus Repository</br>
 - sixth Run the migrator utility from $data-dir/db </br>
 > [!CAUTION]
 > data-dir <ins> means  where  you install  nexus</ins>  e.g home/admin/nexus/nexus-data  in this  example $data-dir is nexus-data</br>
 ## if you  migrate to  H2 follow  this steps </br>
 - Using your system console, change the directory to$data-dir/db.</br>
 - If migrating from OrientDB, run the following command:</br>
 ```
java -Xmx16G -Xms16G -XX:+UseG1GC -XX:MaxDirectMemorySize=28672M \ 
-jar nexus-db-migrator-*.jar \ 
--migration_type=h2
```
- If migrating from PostgreSQL
```
java -jar nexus-db-migrator-*.jar \ 
--migration_type=postgres_to_h2 \ 
--db_url="jdbc:postgresql://<database URL>:<port>/nexus?user=postgresUser&password=secretPassword&currentSchema=nexus"
```
- --db_url="jdbc:postgresql://localhost:5432/nexus?user=postgresUser&password=secretPassword&currentSchema=nexus" – This is the URL to your Postgres database
  - nexus – database name
  - user=postgresUser – database user
  - password=secretPassword – database user password
  - currentSchema=nexus – example database schema (optional).
> [!NOTE]
> - Note that you must enter 2 dashes before the <ins>**migration_type**</ins> parameter.
> - You must wrap the <ins>**db_url**</ins> parameter value with double quotes and enter two dashes before the <ins>**db_url**</ins> parameter.
- Edit the $data-dir/etc/nexus.properties file and add the following line:
```
nexus.datastore.enabled=true
```
- If you are migrating to H2 from PostgreSQL, you must remove or rename the nexus-store.properties file you used for PostgreSQL </br> so that Nexus Repository will not continue to try and boot to that properties file.
- lunch  nexus and wait for fully  loading the datas.
- **ENJOY**


  
