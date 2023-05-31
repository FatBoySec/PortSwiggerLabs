### How to detect SQL injection vulnerabilities

SQL injection can be detected manually by using a systematic set of tests against every entry point in the application. This typically involves:

* Submitting the single quote character ```'``` and looking for errors or other anomalies.

* Submitting some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and looking for systematic differences in the resulting application responses.

* Submitting Boolean conditions such as ```OR 1=1``` and ```OR 1=2```, and looking for differences in the application's responses.

* Submitting payloads designed to trigger time delays when executed within a SQL query, and looking for differences in the time taken to respond.

* Submitting OAST payloads designed to trigger an out-of-band network interaction when executed within a SQL query, and monitoring for any resulting interactions.
