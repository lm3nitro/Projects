# Queries

Please note: This is a continuation of *Import Dabase.md*.

### Scope: 
This is a continuation to the MySQL project. I have already installed MySQL, created a schema, and imported a data set. These are some simple queries. I will be using the dataset that was previously imported in *Import Database.md*. This is a dataset is a list of billionaires and their statistics (age, name, birthday. networth, etc.) The following queries are a simple set of queries using a mix of functions to see the data we have represented in different ways.

Before starting to create queries, we will need to ensure that we have selected our schema. You can do this by double-clicking the scheme and ensureing that it is in **bold** before running the query. As you can see in the image below, our schema is in **bold** and we can select everything in our dataset in a simple query to ensure everything is running:

```
SELECT * FROM statistics_lm3nitro;
```
![Pasted image 20240504130423](https://github.com/lm3nitro/Projects/assets/55665256/c669a29f-3f0c-4c22-95d3-9c0808f12970)


#### Query #1

This query will obtain the name, country, and net worth fields:

```
SELECT personName,country,finalWorth FROM statistics_lm3nitro;
```
![Pasted image 20240504140323](https://github.com/lm3nitro/Projects/assets/55665256/3e277739-5150-4345-bf9a-c08c5f782714)


#### Query #2

This will let us see the list of countries where the billioneres reside and will remove the duplicates:

```
SELECT DISTINCT country FROM statistics_lm3nitro;
```
![Pasted image 20240504140607](https://github.com/lm3nitro/Projects/assets/55665256/75f9fbef-f882-4141-a8dc-55f03ac7d7ae)


#### Query #3

To see the amount of distinct countries, we can use the function **COUNT**:

```
SELECT COUNT(DISTINCT country) FROM statistics_lm3nitro;
```

![Pasted image 20240504140837](https://github.com/lm3nitro/Projects/assets/55665256/fbaf358f-66fd-42c0-9d3b-8deb7f9bd1e3)

#### Query #4

To see the amount of people on the list:

```
SELECT COUNT(DISTINCT personName) FROM statistics_lm3nitro;
```
![Pasted image 20240504141223](https://github.com/lm3nitro/Projects/assets/55665256/7e9e13b8-5e47-4c80-ac21-cebc1e17318e)

#### Query #5

If we want to see the average net worth from the list, we can use the "AVG" function:

```
SELECT AVG(finalWorth) FROM statistics_lm3nitro;
```
![Pasted image 20240504141449](https://github.com/lm3nitro/Projects/assets/55665256/6be56c42-14b1-402a-af8a-302b3ea31816)

#### Query #6

To list all the billionaires that are from France:

```
SELECT * FROM statistics_lm3nitro
WHERE country = 'france';
```

![Pasted image 20240504142652](https://github.com/lm3nitro/Projects/assets/55665256/60d4245d-3bd8-49c5-973f-f5636fe8819f)

#### Query #7

Here we will do the same as above, but we will also exclude the city of Paris:

```
SELECT * FROM statistics_lm3nitro
WHERE country = 'france' AND city <> 'paris';
```
![Pasted image 20240504143010](https://github.com/lm3nitro/Projects/assets/55665256/e261c5d3-2c96-44ea-aa87-fd6ecd4a2ddb)

#### Query #8

See billionaires from both France and Spain:

```
SELECT * FROM statistics_lm3nitro
WHERE country = 'france' or country = 'spain';
```

![Pasted image 20240504143310](https://github.com/lm3nitro/Projects/assets/55665256/dbace42c-c67c-48ba-bf93-d18705ef294f)

#### Query #9

This is another way to select multiple countries. Here I am filtering for Spain, France, and Italy. Please note the last line is commented out so that it is not included in the query:

```
SELECT * FROM statistics_lm3nitro
WHERE country IN('france','spain','italy');
#WHERE country = 'france' or country = 'spain';
```

![Pasted image 20240504143817](https://github.com/lm3nitro/Projects/assets/55665256/0e489656-737e-441d-b62a-0a0bdd10f38a)


#### Query #10 

This query lets us count all the billionaires that are not only from France, Spain, and Italy, but that are also self made:

```
SELECT * FROM statistics_lm3nitro
WHERE country IN('france','spain','italy');

SELECT COUNT(personname) FROM statistics_lm3nitro
WHERE selfmade ='true';
```

![Pasted image 20240504145131](https://github.com/lm3nitro/Projects/assets/55665256/a161c2b1-67fb-426c-bde0-9713d5fbd65c)


#### Query #11

We can also count the number of people based on industry type and *group* the total by their industry:

```
SELECT Industries, COUNT(personName) FROM statistics_lm3nitro
GROUP BY industries;
```

![Pasted image 20240504145450](https://github.com/lm3nitro/Projects/assets/55665256/79094849-51ce-4dc9-9b74-41d4dfcab596)


#### Query #12

We can also use the query above and sort it in descending order:

```
select industries, count(personname) from statistics_lm3nitro
group by industries
order by count(personname) desc;
```

![Pasted image 20240504190914](https://github.com/lm3nitro/Projects/assets/55665256/5313d7bf-2eb9-44fa-be87-e6a28cf5ee82)


#### Query #13

Its also possible to rename the column in table output using the *as* command:

```
select industries, count(personname) as quantity_of_rich_people from statistics_lm3nitro
group by industries
order by count(personname) desc;
```

![Pasted image 20240504191321](https://github.com/lm3nitro/Projects/assets/55665256/c7e06543-de72-4280-aece-30130066af80)

#### Query #14

If we are only interested in seeing the top 5, we can use the *Limit* command in conjunction with the *desc* command:

```
select industries, count(personname) as quantity_of_rich_people from statistics_lm3nitro
group by industries
order by count(personname) desc
limit 5;
```

![Pasted image 20240504191529](https://github.com/lm3nitro/Projects/assets/55665256/154ebf33-d697-44c6-a7e6-e184304d552b)


#### Query #15

This is similar to the descending order, only it is the ascending order. Here we can see how many billionaires were born in each month in ascending order:

```
select birthMonth, count(personname) as Quantity from statistics_lm3nitro
group by birthMonth
order by birthMonth asc;
```

![Pasted image 20240504192425](https://github.com/lm3nitro/Projects/assets/55665256/9fe2c159-0280-4438-823a-862c43457736)


#### Save a query

If you have created a long and complex query and want to save it, you can do so by clicking the save icon, which will open a dialog box sop that you can name the query and save it, as shown below. This is also helpful if there are queries that you run frequently and don't want to keep re-typing it:

![Pasted image 20240504192636](https://github.com/lm3nitro/Projects/assets/55665256/8119d6d3-a616-4265-8382-23e5a9569b9b)


#### Export

You can also export the results of a query in different formats. In the example below I chose to export it in xlsx:

![Pasted image 20240504192907](https://github.com/lm3nitro/Projects/assets/55665256/6ca76c10-584f-4c97-a672-b337f5c8f26d)


###Summary:

These queries are simple but show how different functions can be used to filter, group, and sort the data we have and have it represented the way we want. Overall, MySQL is important as it allows us to access, manage, and manipulate databases. The queries presented above were ran on the actual MySQL server, but this is not normally the case. These servers normally have a DBA assigned to them, and users are only given certain rights, permissions, and least privledge access to the data. We can see an example of this remote access continued in the next part titled *Connecting Remotely.md*. 
