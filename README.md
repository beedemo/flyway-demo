flyway-demo
===========

Project shows flyway maven plugin capabilities.
See flyway website for detailed info.

To create database resolve following maven properties:
    db.url - database url
    db.user - database user
    db.password - database password
    db.driver - oracle.jdbc.OracleDriver (for oracle)

Commands:
 - initial command
mvn clean compile flyway:migrate (for new database)

 - command to reset database
mvn clean compile flyway:clean flyway:migrate

For more commands consult plugin documentation.

