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
