# Simple Queries

Summary: This is a continuation to the MySQL project. I have already installed MySQL, created a schema, and imported a data set. These are some simple queries.

### Queries

SELECT * FROM statistics_lm3nitro;
Make sure to double-click the scheme and that it is in bold to ensure that it is selected before running the query.

![Pasted image 20240504130423](https://github.com/lm3nitro/Projects/assets/55665256/c669a29f-3f0c-4c22-95d3-9c0808f12970)

4:55 time

Query 1

Name, country, and net worth

SELECT personName,country,finalWorth FROM statistics_lm3nitro;

![Pasted image 20240504140323](https://github.com/lm3nitro/Projects/assets/55665256/3e277739-5150-4345-bf9a-c08c5f782714)


Query 2

See only countries, without duplicates

SELECT DISTINCT country FROM statistics_lm3nitro;

![Pasted image 20240504140607](https://github.com/lm3nitro/Projects/assets/55665256/75f9fbef-f882-4141-a8dc-55f03ac7d7ae)


Query 3
Number of Distinct countries using the function COUNT
SELECT COUNT(DISTINCT country) FROM statistics_lm3nitro;

![Pasted image 20240504140837](https://github.com/lm3nitro/Projects/assets/55665256/fbaf358f-66fd-42c0-9d3b-8deb7f9bd1e3)

Query 4
See the amount of people on the list

SELECT COUNT(DISTINCT personName) FROM statistics_lm3nitro;

![Pasted image 20240504141223](https://github.com/lm3nitro/Projects/assets/55665256/7e9e13b8-5e47-4c80-ac21-cebc1e17318e)

Query 5

Average net worth from the list using the AVG function

SELECT AVG(finalWorth) FROM statistics_lm3nitro;


![Pasted image 20240504141449](https://github.com/lm3nitro/Projects/assets/55665256/6be56c42-14b1-402a-af8a-302b3ea31816)


Query 6

Find all where the country is France

SELECT * FROM statistics_lm3nitro
WHERE country = 'france';


![Pasted image 20240504142652](https://github.com/lm3nitro/Projects/assets/55665256/60d4245d-3bd8-49c5-973f-f5636fe8819f)

Query 7

Find all where the country is france and NOT the city of Paris:

SELECT * FROM statistics_lm3nitro
WHERE country = 'france' AND city <> 'paris';

![Pasted image 20240504143010](https://github.com/lm3nitro/Projects/assets/55665256/e261c5d3-2c96-44ea-aa87-fd6ecd4a2ddb)


Query 8

See all from countries of France and spain

SELECT * FROM statistics_lm3nitro
WHERE country = 'france' or country = 'spain';


![Pasted image 20240504143310](https://github.com/lm3nitro/Projects/assets/55665256/dbace42c-c67c-48ba-bf93-d18705ef294f)


Query 9

See all from Spain, France, and Italy. Please note the last line is commented out

SELECT * FROM statistics_lm3nitro
WHERE country IN('france','spain','italy');
#WHERE country = 'france' or country = 'spain';


![Pasted image 20240504143817](https://github.com/lm3nitro/Projects/assets/55665256/0e489656-737e-441d-b62a-0a0bdd10f38a)


Query 10 

Count all the self made billionaires from the list from france, spain, and italy:

SELECT * FROM statistics_lm3nitro
WHERE country IN('france','spain','italy');

SELECT COUNT(personname) FROM statistics_lm3nitro
WHERE selfmade ='true';

![Pasted image 20240504145131](https://github.com/lm3nitro/Projects/assets/55665256/a161c2b1-67fb-426c-bde0-9713d5fbd65c)



Query 11

Count the number of people based on industry type and group the total by industry:

SELECT Industries, COUNT(personName) FROM statistics_lm3nitro
GROUP BY industries;


![Pasted image 20240504145450](https://github.com/lm3nitro/Projects/assets/55665256/79094849-51ce-4dc9-9b74-41d4dfcab596)


Query 12
List amount of people under each industry in descending order

select industries, count(personname) from statistics_lm3nitro
group by industries
order by count(personname) desc;


![Pasted image 20240504190914](https://github.com/lm3nitro/Projects/assets/55665256/5313d7bf-2eb9-44fa-be87-e6a28cf5ee82)



Query 13

Rename column in table output:

select industries, count(personname) as quantity_of_rich_people from statistics_lm3nitro
group by industries
order by count(personname) desc;

![Pasted image 20240504191321](https://github.com/lm3nitro/Projects/assets/55665256/c7e06543-de72-4280-aece-30130066af80)

Query 14

Limit output to 5

select industries, count(personname) as quantity_of_rich_people from statistics_lm3nitro
group by industries
order by count(personname) desc
limit 5;

![Pasted image 20240504191529](https://github.com/lm3nitro/Projects/assets/55665256/154ebf33-d697-44c6-a7e6-e184304d552b)



Query 15

See how many people were born in each month in ascending order

![Pasted image 20240504192425](https://github.com/lm3nitro/Projects/assets/55665256/9fe2c159-0280-4438-823a-862c43457736)


To save a query

![Pasted image 20240504192636](https://github.com/lm3nitro/Projects/assets/55665256/8119d6d3-a616-4265-8382-23e5a9569b9b)


Export to Excel

![Pasted image 20240504192907](https://github.com/lm3nitro/Projects/assets/55665256/6ca76c10-584f-4c97-a672-b337f5c8f26d)



