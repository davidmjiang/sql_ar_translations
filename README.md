##SQL to AR

1.
```sql
SELECT *
FROM
  users;
```

```
  User.all
```

2.
```sql
SELECT *
FROM
  users
WHERE
  user.id = 1;
```

```
  User.find(1)
```

3.
```sql
SELECT *
FROM
  posts
ORDER BY
  created_at DESC
LIMIT 1;
```

```
Post.all.order(created_at: desc).limit(1)
```

4.
```sql
SELECT *
FROM
  users
JOIN
  posts
ON
  posts.author_id = users.id
WHERE
  posts.created_at >= '2014-08-31 00:00:00';
```

```
User.joins("JOIN posts ON posts.author_id = users.id").where("posts.created_at >= '2014-08-31 00:00:00'")
```

5.
```sql
SELECT
  count(*)
FROM
  users
GROUP BY
  favorite_color
HAVING
  count(*) > 1;
```

```
User.select("count(*)").group(:favorite_color).having("count(*) > 1")
```

* The most recently updated user
```
  User.order(updated_at: desc).first
```
* The oldest user (by age)
  ```
  User.order( age: desc ).first
  ```

* all the users
```
  User.all
```

* all posts sorted in descending order by date created
```
Post.order(created_at: desc)
```

##AR to SQL
1. Post.all
```
SELECT * FROM posts

2. Post.first
```
SELECT * FROM posts LIMIT 1
```


3. Post.last
```
  SELECT * FROM post ORDER BY id DESC LIMIT 1
```
4. Post.where(:id => 4)
```
SELECT * FROM posts
WHERE id = 4
```

5. Post.find(4)
```
SELECT * FROM posts
WHERE id = 4
```

6. User.count
```
SELECT COUNT(*)
FROM posts
```

7. Post.select(:name).where(:created_at > 3.days.ago).order(:created_at)
```
SELECT name
FROM posts
WHERE (GETDATE() - created_at) > 3
ORDER BY created_at
```

8. Post.select("COUNT(*)").group(:category_id)
```
SELECT COUNT(*)
FROM posts
GROUP BY category_id
```

9. All posts created before 2014
```
SELECT *
FROM posts
WHERE created_at < 2014
```

10. A list of all (unique) first names for authors who have written at least 3 posts
```
SELECT DISTINCT users.first_name
FROM
  users
JOIN
  posts
ON
  posts.author_id = users.id
GROUP BY author_id
HAVING COUNT(*) > 3
```

11. The posts with titles that start with the word "The"
```
SELECT *
FROM posts
WHERE title LIKE 'The%'
```

12. Posts with IDs of 3,5,7, and 9
```
SELECT *
FROM posts
WHERE id IN (3, 5, 7, 9)
```

##Custom Queries
1. Which author has written the most posts?
SQL
```
SELECT users.first_name, COUNT(*) AS count
FROM
  users
JOIN
  posts
ON
  posts.author_id = users.id
GROUP BY author_id
HAVING count = MAX(count)
```
AR
```
Post.joins(JOIN users ON posts.author_id = users.id).select("users.first_name, COUNT(*) AS count").group(:author_id).having("count = MAX(count)")
```