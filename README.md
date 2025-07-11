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
order by rn desc, hacker_id
;
</code></pre>
