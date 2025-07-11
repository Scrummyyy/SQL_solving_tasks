# 🧠 SQL Practice with HackerRank

Це репозиторій з моїми рішеннями SQL-задач з платформи [HackerRank](https://www.hackerrank.com/domains/sql). Мета цього проєкту — вдосконалити навички роботи з SQL та продемонструвати здатність вирішувати реальні задачі, що базуються на запитах до реляційних баз даних.

## 🧩 Що всередині

- 📁 Категорії задач:
  - `Basic` – базові SELECT-запити
  - `Advanced` – вкладені запити, CASE, логіка
  - `Aggregation` – SUM, COUNT, AVG, HAVING
  - `Joins` – INNER JOIN, LEFT JOIN, SELF JOIN
  - `Subqueries` – вкладені підзапити
  - `OLAP` – аналітичні функції (OVER, RANK тощо)

- 📄 Формат:
  - Кожна задача зберігається у окремому `.sql` файлі.
  - Назви файлів відповідають назві задач на HackerRank.
  - У кожному файлі є:
    - Назва задачі
    - Посилання на задачу
    - Опис умови (коротко)
    - Моє рішення

<h4>Q1 Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.</h4>
<h4>💾 DataFrames Used:</h4>

- `Hackers(hacker_id, name)`
- `Difficulty(difficulty_level, score)`
- `Challenges(challenge_id, difficulty_level)`
- `Submissions(submission_id, hacker_id, challenge_id, score)`

<h3>🧪 Solution (Using <code>SQL</code>):</h3>

<pre><code class="language-sql">
with 
cte as(
select hacker_id, name, rn from(
    select a.*,
    count(a.hacker_id) rn
    from Hackers a
    left join Submissions b on a.hacker_id = b.hacker_id
    left join Challenges c on b.challenge_id = c.challenge_id
    left join Difficulty d on c.difficulty_level = d.difficulty_level
    where b.score = d.score
    group by  a.hacker_id, a.name
    having count(a.hacker_id) > 1
    )
)
select hacker_id, name from cte
order by rn desc, hacker_id;
</code></pre>
<h4>Q2 Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.</h4>
<h4>💾 DataFrames Used:</h4>

- `Company(company_code, founder)`
- `Employee(company_code, manager_code, senior_manager_code, lead_manager_code,  Employee_code)`

<h3>🧪 Solution (Using <code>SQL</code>):</h3>

<pre><code class="language-sql">
select 
a.company_code, a.founder,
count(distinct e.lead_manager_code) lead_manager_count, 
count(distinct e.Senior_Manager_code) Senior_Manager_count,
count(distinct e.Manager_code) manager_count,
count(distinct e.Employee_code) Employee_count
from Company a
left join Employee e on a.company_code = e.company_code
group by a.company_code, a.founder
order by a.company_code;
</code></pre>

<h4>Q3 . The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.</h4>
<h4>💾 DataFrames Used:</h4>

- `Students(id, name, marks)`
- `Grades(min_mark, max_mark, grade)`

<h3>🧪 Solution (Using <code>SQL</code>):</h3>

<pre><code class="language-sql">
with 
grades_marks as (
select a.id, a.name, a.marks, b.grade from students a
left join Grades b on a.marks between b.min_mark and b.max_mark
)
select case when a.grade >= 8 THEN a.name else null end names, 
a.grade, a.marks
from grades_marks a
order by a.grade desc, case when a.grade >= 8 THEN a.name else null end;
</code></pre>
