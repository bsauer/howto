===
SQL
===

PostgreSQL Tips
===============

* sysdate in PostgreSQL

    ::
        
        select * from message_detail md where md.last_chg_datetime >= NOW() - '1 day'::INTERVAL;
        
Sysdate is not directly supported so remember the above.

Reference: http://stackoverflow.com/questions/1888544/how-to-select-records-from-last-24-hours-using-sql
