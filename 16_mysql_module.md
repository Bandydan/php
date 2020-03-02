# MySQL module

### 1. Создать таблицу со следующими данными:

```
+----+--------+----------------+------------------+--------+
| id | name   | pwd            | email            | gender |
+----+--------+----------------+------------------+--------+
|  1 | Vasya  | 21341234qwfsdf | mmm@mmail.com    | m      |
|  2 | Alex   | 21341234       | mmm@gmail.com    | m      |
|  3 | Alexey | qq21341234Q    | alexey@gmail.com | m      |
|  4 | Helen  | MarryMeeee     | hell@gmail.com   | f      |
|  5 | Jenny  | SmakeMyb       | eachup@gmail.com | f      |
|  6 | Lora   | burn23         | tpicks@gmail.com | f      |
+----+--------+----------------+------------------+--------+
```

### 2. Отобразить данные приведенного ниже вида, обратить внимание на `he` и `she`:

```
+-----------------------------------------------+
| info                                          |
+-----------------------------------------------+
| This is Vasya, he has email mmm@mmail.com     |
| This is Alex, he has email mmm@gmail.com      |
| This is Alexey, he has email alexey@gmail.com |
| This is Helen, she has email hell@gmail.com   |
| This is Jenny, she has email eachup@gmail.com |
| This is Lora, she has email tpicks@gmail.com  |
+-----------------------------------------------+
```

### 3. Отобразить данные приведенного ниже вида:

```
+---------------------+
| Gender information: |
+---------------------+
| We have 3 boys!     |
| We have 3 girls!    |
+---------------------+
```


### 4. Создать и заполнить структуру данных для хранения словарей и слов из них:

```sql
CREATE TABLE dicts(
	id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
	title VARCHAR(255) NOT NULL DEFAULT "",
	updated TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE TABLE words(
	id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
	word VARCHAR(255) NOT NULL DEFAULT "",
	type VARCHAR(100) NOT NULL DEFAULT 'w',
	dict_id INT NOT NULL DEFAULT 1,
	updated TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

```

```sql
INSERT INTO dicts
	(title)
VALUES
	('animals'),
	('school'),
	('nature'),
	('human'),
	('SF');

INSERT INTO words
	(word, dict_id)
VALUES
	('turtle', 1),
	('pig', 1),
	('dog', 1),
	('cat', 1),
	('lizard', 1),
	('cow', 1),
	('rabbit', 1),
	('frog', 1),
	('headgehog', 1),
	('goat', 1);

INSERT INTO words
	(word, dict_id)
VALUES
	('desk', 2),
	('book', 2),
	('chalk', 2),
	('pen', 2),
	('pencil', 2),
	('copybook', 2),
	('lesson', 2),
	('teacher', 2),
	('pupils', 2),
	('school', 2);

INSERT INTO words
	(word, dict_id)
VALUES
	('ray', 3),
	('thunder', 3),
	('sun', 3),
	('field', 3),
	('hill', 3),
	('mountain', 3),
	('river', 3),
	('forest', 3),
	('grass', 3),
	('rain', 3);

INSERT INTO words
	(word, dict_id)
VALUES
	('hair', 4),
	('nail', 4),
	('finger', 4),
	('eye', 4),
	('tooth', 4),
	('knee', 4),
	('elbow', 4),
	('leg', 4),
	('arm', 4),
	('head', 4);

INSERT INTO words
	(word, dict_id)
VALUES
	('engine', 5),
	('steel', 5),
	('power', 5),
	('nuclear', 5),
	('shotgun', 5),
	('laser', 5),
	('flight', 5),
	('energy', 5),
	('Moon', 5),
	('splace', 5);

```




### 5. Получите результат:
```
+---------+-------+
| title   | words |
+---------+-------+
| animals |    10 |
| school  |    10 |
| nature  |    10 |
| human   |    10 |
| SF      |    10 |
+---------+-------+
5 rows in set (0.01 sec)
```
