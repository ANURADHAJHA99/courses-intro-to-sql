---
title: Filtering rows
description: >-
  This chapter builds on the first by teaching you how to filter tables for rows 
  satisfying some criteria of interest. You'll learn how to use basic comparison 
  operators, combine multiple criteria, match patterns in text, and much more.

--- type:PlainMultipleChoiceExercise lang:sql xp:50 key:bfc80ff2e5
## Filtering results

Congrats on finishing the first chapter! You now know how to select columns and perform basic counts. This chapter will focus on filtering your results.

In SQL, the `WHERE` keyword allows you to filter based on both text and numeric values in a table. There are a few different comparison operators you can use:

- `=` equal
- `<>` not equal
- `<` less than
- `>` greater than
- `<=` less than or equal to
- `>=` greater than or equal to

For example, you can filter text records such as `title`. The following code returns all films with the title `'Metropolis'`:

```
SELECT title
FROM films
WHERE title = 'Metropolis';
```

Note that the `WHERE` clause always comes after the `FROM` statement!

<hr>
What does the following query return?

```
SELECT title
FROM films
WHERE release_year > 2000;
```

*** =instructions
- Films released before the year 2000
- Films released after the year 2000
- Films released after the year 2001
- Films released in 2000

*** =hint
If you're stuck, refer to the list of comparison operators above!

*** =sct
```{python}
success_msg = "Correct!"
msg2 = "Incorrect. `>` means *strictly* greater than and *not* equal to."

Ex().test_mc(2, [msg2, success_msg, msg2, msg2])
```

--- type:BulletExercise lang:sql xp:100 key:b90db25f34
## Simple filtering of numeric values

As you learned in the previous exercise, the `WHERE` clause can be be used to filter numeric records, such as years or ages.

For example, the following query selects all details for films with a budget over ten thousand dollars:

```
SELECT *
FROM films
WHERE budget > 10000;
```

Now it's your turn to use the `WHERE` clause to filter numeric values!

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])

```

*** =sample_code
```{sql}

```

*** =type1: NormalExercise
*** =key1: 8a4615ada8
*** =xp1: 20

*** =instructions1
Get all details for all films released in 2016.

*** =solution1
```{sql}
SELECT *
FROM films
WHERE release_year = 2016;
```
*** =sct1
```{python}
sel = check_node('SelectStmt')

star = sel.check_node('Star').has_equal_ast('Are you selecting all columns?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    star,
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type2: NormalExercise
*** =key2: 5e6e1c74c6
*** =xp2: 20

*** =instructions2
Get the number of films released before 2000.
*** =solution2
```{sql}
SELECT COUNT(*)
FROM films
WHERE release_year < 2000;
```
*** =sct2
```{python}
sel = check_node('SelectStmt')

count_call = sel.check_field('target_list', 0).has_equal_ast('Are you calling `COUNT` correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    count_call,
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type3: NormalExercise
*** =key3: d66f3d41b7
*** =xp3: 20

*** =instructions3
Get the title and release year of films released after 2000.
*** =solution3
```{sql}
SELECT title, release_year
FROM films
WHERE release_year > 2000;
```
*** =sct3
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    count_call,
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

--- type:BulletExercise lang:sql xp:100 key:b90db25f33
## Simple filtering of text

Remember, the `WHERE` clause can be also be used to filter text results, such as names or countries.

For example, this query gets the titles of all films which were filmed in China:

```
SELECT title
FROM films
WHERE country = 'China';
```

Now it's your turn to practice using `WHERE` with text values!

**Important: in PostgreSQL (the version of SQL we're using), you must use single quotes with `WHERE`.**

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films', 'people'])
```

*** =sample_code
```{sql}

```

*** =type1: NormalExercise
*** =key1: b645308dcd
*** =xp1: 20

*** =instructions1
Get all details for all French language films.
*** =solution1
```{sql}
SELECT *
FROM films
WHERE language = 'French';
```
*** =hint1
Remember, to get all details you can use `SELECT *`

*** =sct1
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Check your `WHERE` clause! Make sure to use single quotes.')

star = sel.check_node('Star').has_equal_ast('Are you selecting all the columns?')

Ex().test_correct(check_result(), [
    from_clause,
    where_clause,
    star,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type2: NormalExercise
*** =key2: 051f6fb8ec
*** =xp2: 20

*** =instructions2
Get the name and birth date of the person born on November 11th, 1974. Remember to use ISO date format (`'1974-11-11'`)!
*** =solution2
```{sql}
SELECT name, birthdate
FROM people
WHERE birthdate = '1974-11-11';
```

*** =hint2
Your `WHERE` clause should be `WHERE birthdate = '1974-11-11';`

*** =sct2
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Check your `WHERE` clause! Make sure to use single quotes.')
```

*** =type3: NormalExercise
*** =key3: 7b872a3af0
*** =xp3: 20

*** =instructions3
Get the number of Hindi language films.
*** =solution3
```{sql}
SELECT COUNT(*)
FROM films
WHERE language = 'Hindi';
```

*** =hint3
Remember, you can use `COUNT(*)` to count the number of rows.

*** =sct3
```{python}
sel = check_node('SelectStmt')

count_call = sel.check_field('target_list', 0).has_equal_ast('Are you calling `COUNT` correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Check your `WHERE` clause! Make sure to use single quotes.')

Ex().test_correct(check_result(), [
    count_call,
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type4: NormalExercise
*** =key4: 2c87504f11
*** =xp4: 20

*** =instructions4
Get all details for all films with an R certification.
*** =solution4
```{sql}
SELECT *
FROM films
WHERE certification = 'R';
```
*** =sct4
```{python}
sel = check_node('SelectStmt')

star = sel.check_node('Star').has_equal_ast('Are you selecting all columns?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Check your `WHERE` clause! Make sure to use single quotes.')

Ex().test_correct(check_result(), [
    star,
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```


--- type:BulletExercise lang:sql xp:100 key:5bda32d7c8
## WHERE AND

Often, you'll want to select data based on multiple conditions. You can build up your `WHERE` queries by combining multiple conditions with the `AND` keyword.

For example:

```
SELECT title
FROM films
WHERE release_year > 1994
AND release_year < 2000;
```

gives you the titles of films released between 1994 and 2000.

Note that you need to specify the column name separately for every `AND` condition, so the following would be invalid:

```
SELECT title
FROM films
WHERE release_year > 1994 AND < 2000;
```

You can add as many `AND` conditions as you need!

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])
```

*** =sample_code
```{sql}
```

*** =type1: NormalExercise
*** =key1: 7ccf93b215
*** =xp1: 20

*** =instructions1
Get the title and release year for all Spanish language films released before 2000.
*** =solution1
```{sql}
SELECT title, release_year
FROM films
WHERE release_year < 2000
AND language = 'Spanish';
```
*** =sct1
```{python}
sel = check_node('SelectStmt')

title = test_column('title', msg='Did you include the `title` column?')

release_year_col = test_column('release_year', msg='Did you include the `release_year` column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause')

release_year = where_clause.has_equal_ast(sql='release_year < 2000', start='expression', exact=False, msg='Did you check the `release_year`?')

language = where_clause.has_equal_ast(sql="language = 'Spanish'", start='expression', exact=False, msg='Did you check the `language`? Make sure to use single quotes.')

Ex().test_correct(check_result(), [
    release_year,
    language,
    from_clause,
    title,
    release_year_col,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type2: NormalExercise
*** =key2: e703c95e46
*** =xp2: 20

*** =instructions2
Get all details for Spanish language films released after 2000.
*** =solution2
```{sql}
SELECT *
FROM films
WHERE release_year > 2000
AND language = 'Spanish';
```
*** =sct2
```{python}
sel = check_node('SelectStmt')

star = sel.check_node('Star').has_equal_ast('Are you selecting all columns?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

release_year = where_clause.has_equal_ast(sql='release_year > 2000', start='expression', exact=False, msg='Did you check the `release_year` correctly?')

language = where_clause.has_equal_ast(sql="language = 'Spanish'", start='expression', exact=False, msg='Did you check the `language` correctly? Make sure to use single quotes.')

Ex().test_correct(check_result(), [
    star,
    release_year,
    language,
    where_clause,
    from_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type3: NormalExercise
*** =key3: 7f2ba5c82f
*** =xp3: 20

*** =instructions3
Get all details for Spanish language films released after 2000, but before 2010.
*** =solution3
```{sql}
SELECT *
FROM films
WHERE release_year > 2000
AND release_year < 2010
AND language = 'Spanish';
```
*** =sct3
```{python}
sel = check_node('SelectStmt')

star = sel.check_node('Star').has_equal_ast(msg='Are you selecing all columns?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

release_year1 = where_clause.has_equal_ast(sql='release_year > 2000', start='expression', exact=False, msg='Did you check the `release_year` correctly?')

release_year2 = where_clause.has_equal_ast(sql='release_year < 2010', start='expression', exact=False, msg='Did you check the `release_year` correctly?')

language = where_clause.has_equal_ast(sql="language = 'Spanish'", start='expression', exact=False, msg='Did you check the `language` correctly? Make sure to use single quotes.')



Ex().test_correct(check_result(), [
    release_year1, 
    release_year2,
    language,
    where_clause,
    from_clause,
    star,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

--- type:TabExercise lang:sql xp:100 key:ecc1838fc7
## WHERE AND OR

What if you want to select rows based on multiple conditions where some, but not necessarily all, of the conditions need to be met? For this, SQL has the `OR` clause.

For example, the following returns all films released in *either* 1994 or 2000:

```
SELECT title
FROM films
WHERE release_year = 1994
OR release_year = 2000;
```

Note that you need to specify the column for every `OR` condition, so the following is invalid:

```
SELECT title
FROM films
WHERE release_year = 1994 OR 2000;
```

When combining `AND` and `OR`, be sure to enclose the individual clauses in parentheses, like so:

```
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
AND (certification = 'PG' OR certification = 'R');
```

Otherwise, due to SQL's precedence rules, you may not get the results you're expecting!

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])
```

*** =sample_code
```{sql}
```

*** =type1: NormalExercise

*** =key1: 510b387baa
*** =xp1: 20

*** =instructions1
Get the title and release year for films released in the 90s.
*** =hint1
```{sql}
SELECT ___, ___
FROM ___
WHERE ___ >= 1990 AND ___ < 2000;
```
*** =solution1
```{sql}
SELECT title, release_year
FROM films
WHERE release_year >= 1990 AND release_year < 2000;
```
*** =sct1
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause')

where_one = sel.check_field('where_clause').check_field('left').has_equal_ast('Is the first part of your `WHERE` clause correct?')

where_two = sel.check_field('where_clause').check_field('right').has_equal_ast('Is the second part of your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    where_one,
    where_two,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type2: NormalExercise
*** =key2: 969ed73542
*** =xp2: 20

*** =instructions2
Get the title and release year of French or Spanish films released in the 90s.
*** =hint2
```{sql}
SELECT ___, ___
FROM ___
WHERE (___ >= 1990 AND ___ < 2000)
AND (___ = 'French' OR ___ = 'Spanish');
```
*** =solution2
```{sql}
SELECT title, release_year
FROM films
WHERE (release_year >= 1990 AND release_year < 2000)
AND (language = 'French' OR language = 'Spanish');
```
*** =sct2
```{python}
# TODO
```

*** =type3: NormalExercise
*** =key3: aee831c1d8
*** =xp3: 20

*** =instructions3
Get the title and release year of French or Spanish films released in the 90s that took in more than $20M at the box office.
*** =solution3
```{sql}
SELECT title, release_year
FROM films
WHERE (release_year >= 1990 AND release_year < 2000)
AND (language = 'French' OR language = 'Spanish')
AND gross > 20000000;
```
*** =sct3
```{python}
# TODO
```

--- type:PlainMultipleChoiceExercise lang:sql xp:50 key:a1827199e2
## BETWEEN

As you've learned, you can use the following query to get titles of all films released in and between 1994 and 2000:

```
SELECT title
FROM films
WHERE release_year >= 1994
AND release_year <= 2000;
```

Checking for ranges like this is very common, so in SQL the `BETWEEN` keyword provides a useful shorthand for filtering values within a specified range. This query is equivalent to the one above:

```
SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000;
```

It's important to remember that `BETWEEN` is _inclusive_, meaning the beginning and end values are included in the results!

<hr>
What does the `BETWEEN` keyword do?

*** =instructions
- Filter numeric values
- Filter text values
- Filter values in a specified list
- Filter values in a specified range

*** =hint
Think about looking for values **between** a beginning and end point.

*** =sct
```{python}
success_msg = 'Correct!'
numeric = 'Incorrect. `BETWEEN` does not just filter numeric values.'
text = 'Incorrect. `BETWEEN` does not just filter text values.'
lst = 'Incorrect!'

Ex().test_mc(4, [numeric, text, lst, success_msg])
```

--- type:TabExercise lang:sql xp:100 key:9c11f67712
## BETWEEN (2)

Similar to the `WHERE` clause, the `BETWEEN` clause can be used with multiple `AND` and `OR` operators, so you can build up your queries and make them even more powerful!

For example, suppose we have a table called `kids`. We can get the names of all kids between the ages of 2 and 12 from the United States:

```
SELECT name
FROM kids
WHERE age BETWEEN 2 AND 12
AND nationality = 'USA';
```

Take a go at using `BETWEEN` with `AND`!

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])
```

*** =sample_code
```{sql}

```

*** =type1: NormalExercise

*** =key1: 9252da136b
*** =xp1: 20

*** =instructions1
Get the title and release year of all films released between 1990 and 2000.
*** =solution1
```{sql}
SELECT title, release_year
FROM films
WHERE release_year BETWEEN 1990 AND 2000;
```
*** =sct1
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])

```

*** =type2: NormalExercise
*** =key2: e189875af9
*** =xp2: 20

*** =instructions2
Get the number of films released in the 90s.
*** =solution2
```{sql}
SELECT COUNT(*)
FROM films
WHERE release_year BETWEEN 1990 AND 2000;
```
*** =sct2
```{python}
sel = check_node('SelectStmt')

count_call = sel.check_field('target_list', 0)

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')


Ex().test_correct(check_result(), [
    count_call,
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type3: NormalExercise

*** =key3: d21a4bec02
*** =xp3: 20

*** =instructions3
Get the title and budget of all films made between 2000 and 2015 with budgets over $100 million.
*** =solution3
```{sql}
SELECT title, budget
FROM films
WHERE release_year
BETWEEN 2000 AND 2015
AND budget > 100000000;
```
*** =sct3
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

between = where_clause.check_node('BinaryExpr').has_equal_ast('Is your use of `BETWEEN` correct?')

Ex().test_correct(check_result(), [
    from_clause,
    between,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type4: NormalExercise

*** =key4: 9087bf33ac
*** =xp4: 20

*** =instructions4
Get the title and language of all films made between 1990 and 1995 or those in the Spanish language.
*** =solution4
```{sql}
SELECT title, language
FROM films
WHERE release_year
BETWEEN 1990 AND 1995
OR language = 'Spanish';
```
*** =sct4
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

between = where_clause.check_field('left').has_equal_ast('Is your use of `BETWEEN` correct?')

or_op = where_clause.check_field('right').has_equal_ast('Is your `OR` operator correct?')

Ex().test_correct(check_result(), [
    from_clause,
    between,
    or_op,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

--- type:BulletExercise lang:sql xp:100 key:4fc7e638f8
## WHERE IN

Well done!

As you've seen, `WHERE` is very useful for filtering results. However, if you want to filter based on many conditions, `WHERE` can get unwieldy. For example:

```
SELECT name
FROM kids
WHERE age = 2
OR age = 4
OR age = 6
OR age = 8
OR age = 10;
```

Enter the `IN` operator! The `IN` operator allows you to specify multiple values in a `WHERE` clause.

Basically, `IN` makes it easier and quicker to specify multiple `OR` conditions! Neat, right?

So, the above example would become simply:

```
SELECT name
FROM kids
WHERE age IN (2, 4, 6, 8, 10);
```

Try using the `IN` operator yourself!

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])
```

*** =sample_code
```{sql}

```

*** =type1: NormalExercise
*** =key1: dc7674d358
*** =xp1: 20

*** =instructions1
Get the title and release year of all films released in 1990 or released in 2000 that were longer than two hours. Remember, duration is in minutes!

*** =solution1
```{sql}
SELECT title, release_year
FROM films
WHERE release_year IN (1990, 2000)
AND duration > 120;
```

*** =hint1
The first part of your `WHERE` clause should be `WHERE release_year IN (1990, 2000)`

*** =sct1
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause')

in_op = where_clause.has_equal_ast(
        'Is your use of `IN` correct?',
        sql='release_year IN (1990, 2000)', start='binary_expression', exact = False)

duration = where_clause.check_node('BinaryExpr').has_equal_ast('Did you check `duration` correctly?')

Ex().test_correct(check_result(), [
    in_op,
    duration,
    from_clause,
    where_clause.has_equal_ast('Is your `WHERE` clause correct?'),
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type2: NormalExercise
*** =key2: 3a84c097d2
*** =xp2: 20

*** =instructions2
Get the title and language of all films which were in English, Spanish or French.
*** =solution2
```{sql}
SELECT title, language
FROM films
WHERE language IN ('English', 'Spanish', 'French');
```

*** =hint2
Your `WHERE` clause should be `WHERE language IN ('English', 'Spanish', 'French');`

*** =sct2
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type3: NormalExercise
*** =key3: 3c947b0d2d
*** =xp3: 20

*** =instructions3
Get the title and certification of all films with an NC-17 or R certification.
*** =solution3
```{sql}
SELECT title, certification
FROM films
WHERE certification IN ('NC-17', 'R');
```

*** =hint3
Your `WHERE` clause should be `WHERE certification IN ('NC-17', 'R');`

*** =sct3
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

--- type:PlainMultipleChoiceExercise lang:sql xp:50 key:5cf67b42b3
## Introduction to NULL and IS NULL

In SQL, `NULL` represents a missing or unknown value. You can check if a row has a column with the value `NULL` using the expression `IS NULL`. For example, to count the number of missing birth dates in the people table:

```
SELECT COUNT(*)
FROM people
WHERE birthdate IS NULL;
```

As you can see, `IS NULL` is useful when combined with `WHERE` to figure out what data you're missing.

Often, you will also want to filter out missing values so you only get results which are not `NULL`. To do this, you can use the `IS NOT NULL` operator.

For example:

```
SELECT name
FROM people
WHERE birthdate IS NOT NULL;
```

will give you the names of all the people whose birth date is _not_ missing in the `people` table.

<hr>
What does `NULL` represent?

*** =instructions
- A Corrupt entry
- A Missing value
- A Empty string
- A Invalid value

*** =hint
Remember, `NULL` represents values which are missing or unknown.

*** =sct
```{python}
corrupt = 'Incorrect. We can not be sure that a `NULL` value is actually corrupt.'
success_msg = 'Correct! `NULL` is used to represent unknown values.'
empty = 'Incorrect. An empty string is not the same as a `NULL` value.'
invalid = 'Incorrect!'

Ex().test_mc(2, [corrupt, success_msg, empty, invalid])
```

--- type:BulletExercise lang:sql xp:100 key:84411d78ab
## NULL and IS NULL

Now that you know what `NULL` is and what it's used for, let's get some practice!

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films', 'people'])
```

*** =sample_code
```{sql}

```

*** =type1: NormalExercise
*** =key1: 3c646ada87
*** =xp1: 20

*** =instructions1
Get the names of people who are still alive, i.e. whose death date is missing.
*** =solution1
```{sql}
SELECT name
FROM people
WHERE deathdate IS NULL;
```

*** =hint1
Your `WHERE` clause should be `WHERE deathdate IS NULL;`

*** =sct1
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Are you checking `IS NULL` in your `WHERE` clause?')

Ex().test_correct(check_result(), [
    where_clause,
    from_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type2: NormalExercise
*** =key2: 3c646ada88
*** =xp2: 20

*** =instructions2
Get the number of films which don't have a language associated with them.
*** =solution2
```{sql}
SELECT COUNT(title)
FROM films
WHERE language IS NULL;
```
*** =hint2
Your `WHERE` clause should be `WHERE language IS NULL;`

*** =sct2
```{python}
sel = check_node('SelectStmt')

count_call = sel.check_node('Call').has_equal_ast('Are you calling `COUNT` correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Are you checking `IS NULL` in your `WHERE` clause?')

Ex().test_correct(check_result(), [
    where_clause,
    count_call,
    from_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])

```

*** =type3: NormalExercise
*** =key3: 3c646ada89
*** =xp3: 20

*** =instructions3
Get the title of every film which doesn't have a budget associated with it.
*** =solution3
```{sql}
SELECT title
FROM films
WHERE budget IS NULL;
```
*** =sct3
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Are you checking `IS NULL` in your `WHERE` clause?')

Ex().test_correct(check_result(), [
    where_clause,
    from_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

--- type:BulletExercise lang:sql xp:100 key:84411d78ac
## LIKE and NOT LIKE

Good work!

As you have seen, the `WHERE` column can be used to filter text data. However, so far you've only been able to filter based on specific text string. In the real world, often you'll want to search for a specific pattern rather than a specific text string.

In SQL, the `LIKE` operator can be used in a `WHERE` clause to search for a specific pattern in a column.

In relation to the `LIKE` operator, a _wildcard_ is a placeholder for some other values. There are two wildcards you can use with `LIKE`:

* The `'%'` wildcard will match zero, one, or many characters in text.

For example, the following query:

```
SELECT name
FROM companies
WHERE name LIKE 'Data%';
```

matches companies like `'Data'`, `'DataC'` `'DataCamp'`, `'DataMind'`, and so on.

* The `'_'` wildcard will match a _single_ character.

For example, the following query:

```
SELECT name
FROM companies
WHERE name LIKE 'DataC_mp';
```

matches companies like `'DataCamp'`, `'DataComp'`, and so on:

You can also use the `NOT LIKE' operator to find records that **don't** match the pattern you specify.

Got it? Let's practice!

*** =pre_exercise_code
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['people'])
```

*** =sample_code
```{sql}

```

*** =type1: NormalExercise
*** =key1: 9e3c3ef68f
*** =xp1: 20

*** =instructions1
Get people from the `people` table whose names begin with 'B'.
*** =solution1
```{sql}
SELECT name
FROM people
WHERE name LIKE 'B%';
```

*** =hint1
Your `WHERE` clause should be `WHERE name LIKE 'B%';`

*** =sct1
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type2: NormalExercise
*** =key2: 606b667e1c
*** =xp2: 20

*** =instructions2
Get people whose names have 'r' as the second letter.
*** =solution2
```{sql}
SELECT name
FROM people
WHERE name LIKE '_r%';
```

*** =hint2
Your `WHERE` clause should be `WHERE name LIKE '_r%';`

*** =sct2
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

*** =type3: NormalExercise
*** =key3: 2e4f49a528
*** =xp3: 20

*** =instructions3
Get people whose names don't start with A.
*** =solution3
```{sql}
SELECT name
FROM people
WHERE name NOT LIKE 'A%';
```

*** =hint3
Your `WHERE` clause should be `WHERE name NOT LIKE 'A%';`

*** =sct3
```{python}
sel = check_node('SelectStmt')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    where_clause,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```
