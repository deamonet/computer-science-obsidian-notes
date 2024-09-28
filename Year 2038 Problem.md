183

[](https://stackoverflow.com/posts/2012620/timeline)

_I have marked this as a community wiki so feel free to edit at your leisure._

## What exactly is the Year 2038 problem?

"The year 2038 problem (also known as Unix Millennium Bug, Y2K38 by analogy to the Y2K problem) may cause some computer software to fail before or in the year 2038. The problem affects all software and systems that store system time as a signed 32-bit integer, and interpret this number as the number of seconds since 00:00:00 UTC on January 1, 1970."

---

## Why does it occur and what happens when it occurs?

Times beyond **03:14:07 UTC on Tuesday, 19 January 2038** will 'wrap around' and be stored internally as a negative number, which these systems will interpret as a time in December 13, 1901 rather than in 2038. This is due to the fact that the number of seconds since the UNIX epoch (January 1 1970 00:00:00 GMT) will have exceeded a computer's maximum value for a 32-bit signed integer.

---

## How do we solve it?

-   Use long data types (64 bits is sufficient)
-   For MySQL (or MariaDB), if you don't need the time information consider using the `DATE` column type. If you need higher accuracy, use `DATETIME` rather than `TIMESTAMP`. Beware that `DATETIME` columns do not store information about the timezone, so your application will have to know which timezone was used.
-   [Other Possible solutions described on Wikipedia](http://en.wikipedia.org/wiki/Year_2038_problem#Solutions)
-   Upgrade your Mysql to [8.0.28](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-28.html#mysqld-8-0-28-feature) or higher

---

## Are there any possible alternatives to using it, which do not pose a similar problem?

Try wherever possible to use large types for storing dates in databases: 64-bits is sufficient - a long long type in GNU C and POSIX/SuS, or `sprintf('%u'...)` in PHP or the BCmath extension.

---

## What are some potentially breaking use cases even though we're not yet in 2038?

So a MySQL [DATETIME](http://dev.mysql.com/doc/refman/5.6/en/datetime.html) has a range of 1000-9999, but TIMESTAMP only has a range of 1970-2038. If your system stores birthdates, future forward dates (e.g. 30 year mortgages), or similar, you're already going to run into this bug. Again, don't use TIMESTAMP if this is going to be a problem.

---

## What can we do to the existing applications that use TIMESTAMP, to avoid the so-called problem, when it really occurs?

Few PHP applications will still be around in 2038, though it's hard to foresee as the web hardly a legacy platform yet.

Here is a process for altering a database table column to convert `TIMESTAMP` to `DATETIME`. It starts with creating a temporary column:

```sql
# rename the old TIMESTAMP field
ALTER TABLE `myTable` CHANGE `myTimestamp` `temp_myTimestamp` int(11) NOT NULL;

# create a new DATETIME column of the same name as your old column
ALTER TABLE `myTable` ADD `myTimestamp` DATETIME NOT NULL;

# update all rows by populating your new DATETIME field
UPDATE `myTable` SET `myTimestamp` = FROM_UNIXTIME(temp_myTimestamp);

# remove the temporary column
ALTER TABLE `myTable` DROP `temp_myTimestamp`
```

---

**Resources**

-   [Year 2038 Problem (Wikipedia)](http://en.wikipedia.org/wiki/Year_2038_problem)
	-   [The Internet Will End in 30 Years](http://readwrite.com/2008/03/13/the_internet_will_end_in_30_years/)