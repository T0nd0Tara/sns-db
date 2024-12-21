# sns-db
### Sql-NoSql Database

## The Problem
Sql is a GREAT database option. It is fast, doesn't take a lot of hardware space, allow transactions and
multiprocessing, etc...

All because it saves data as 2D tables.

But that's also my problem with it.
Most of the time when programming, we don't need a 2D table to work with - we want to work with a complex object

For example take a look at this struct

```cpp
struct Person {
    int id;
    std::string first_name;
    std::string last_name;

    int parent_id;
};
```

If we want to save this struct in the SQL database we'd just make a table called `Person` with each row having `id`,
`first_name`, `last_name` and `parent_id` columns (yea each person here has only one parent).

Pretty simple.
But look what happens when we make this struct a little bit more complex. Let's add for each person a vector for each
child it has:

```cpp
struct Person {
    int id;
    std::string first_name;
    std::string last_name;

    int parent_id;
    std::vector<int> children_ids;
};
```
We don't need to add another field as `children_ids` can be infered from `parent_id`. But now to fetch one `Person` from
our DB is a bit harder.

We have 2 options:
1. Aggregating the children into a list
2. Fetching the person's columns first and then creating another query to fetch it's children 

Both options are obviously bad... As the former can cause false data, and the latter is slow.

## The Solution
What if, the db would save everything inside tables, BUT, would send it in BSON/JSON/other non table like format?
(BSON/JSON will probably be too bloated for such a thing but we can create another format)

Well we'd obviously need another syntax to ask for queries from the DB, but what's the holdup?

It won't replace `NoSql` databases, it would still save up everything in tables, so a bunch on NULLs would still be
wastful

But it can maybe replace regular `SQL`...

