# Node.js-MySQL-Tutorial
- [Node.js & MySQL Tutorial](https://opentutorials.org/module/3560)

해당 수업을 통해 Node.js와 MySQL을 연동하여 사용하는 방법에 대해 실습한 내용입니다.

## 1. 실습 준비

-  [소스 코드](https://github.com/web-n/node.js-mysql/releases/tag/1) 다운로드

- VSC로 다운로드 폴더 열기

## 2. MySQL DB 셋팅

#### 1. MySQL 데이터베이스 서버 접속
```bash
mysql -u root -p
```

#### 2. opentutorials 데이터베이스를 생성하고 선택
```sql
CREATE DATABASE opentutorials;
USE opentutorials;
```

#### 3. 아래 코드를 붙여넣기 해서 TABLE 생성하고 INSERT 하기
```sql
--
-- Table structure for table `author`
--
 
 
CREATE TABLE `author` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  `profile` varchar(200) DEFAULT NULL,
  PRIMARY KEY (`id`)
);
 
--
-- Dumping data for table `author`
--
 
INSERT INTO `author` VALUES (1,'egoing','developer');
INSERT INTO `author` VALUES (2,'duru','database administrator');
INSERT INTO `author` VALUES (3,'taeho','data scientist, developer');
 
--
-- Table structure for table `topic`
--
 
CREATE TABLE `topic` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(30) NOT NULL,
  `description` text,
  `created` datetime NOT NULL,
  `author_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
);
 
--
-- Dumping data for table `topic`
--
 
INSERT INTO `topic` VALUES (1,'MySQL','MySQL is...','2018-01-01 12:10:11',1);
INSERT INTO `topic` VALUES (2,'Oracle','Oracle is ...','2018-01-03 13:01:10',1);
INSERT INTO `topic` VALUES (3,'SQL Server','SQL Server is ...','2018-01-20 11:01:10',2);
INSERT INTO `topic` VALUES (4,'PostgreSQL','PostgreSQL is ...','2018-01-23 01:03:03',3);
INSERT INTO `topic` VALUES (5,'MongoDB','MongoDB is ...','2018-01-30 12:31:03',1);
```

---

## 3. Node.js 셋팅

#### 1. MySQL 모듈 설치
```bash
npm install --save mysql
```

#### 2. nodejs/mysql.js 파일에 아래와 같이 MySQL 연결
```jsx
var mysql      = require('mysql');
// 비밀번호는 별도의 파일로 분리해서 버전관리에 포함시키지 않아야 합니다. 
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'root',
  password : '111111',
  database : 'opentutorials'
});
  
connection.connect();
  
connection.query('SELECT * FROM topic', function (error, results, fields) {
    if (error) {
        console.log(error);
    }
    console.log(results);
});
  
connection.end();
```

---

## 4. MySQL DB에서 데이터를 가져와서 웹페이지에 구현하기

#### 1. MySQL 모듈 가져오고 접속하기
```jsx
// main.js 파일에 아래 코드 추가

var mysql = require("mysql");
var db = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "111111",
  database: "opentutorials",
});
db.connect();
```

#### 2. db.query를 사용하여 데이터베이스의 'SELECT * FROM topic' 쿼리를 실행한다.
```jsx
//main.js 파일 수정

if (pathname === "/") {
    if (queryData.id === undefined) {
      /// 여기 안의 내용을 아래와 같이 수정 /// 
    }


    db.query(`SELECT * FROM topic`, function (error, topics) {
        var title = "Welcome";
        var description = "Hello, Node.js";
        var list = template.list(topics);
        var html = template.HTML(
          title,
          list,
          `<h2>${title}</h2>${description}`,
          `<a href="/create">create</a>`
        );
        response.writeHead(200);
        response.end(html);
      });
```

---

## 5. SQL 명령어 정리

- 데이터 베이스 생성
```sql
CREATE DATABASE 데이터베이스이름;
```

- 데이터베이스 선택
```sql
USE 데이터베이스이름;
```

- 테이블 생성
```sql
CREATE TABLE 테이블이름 (
  컬럼1 데이터타입,
  컬럼2 데이터타입,
  ...
);
```

- 테이블 조회
```sql
SHOW TABLES;
```

- 데이터 조회
```sql
SELECT * FROM 테이블이름;
```

- 데이터 삽입
```sql
INSERT INTO 테이블이름 (컬럼1, 컬럼2, ...) VALUES (값1, 값2, ...);
```

- 데이터 수정

```sql
UPDATE 테이블이름 SET 컬럼1 = 값1 WHERE 조건;
```

- 데이터 삭제:
```sql
DELETE FROM 테이블이름 WHERE 조건;
```

- 조건으로 데이터 조회
```sql
SELECT * FROM 테이블이름 WHERE 조건;
```

- 정렬하여 데이터 조회
```sql
SELECT * FROM 테이블이름 ORDER BY 컬럼 ASC/DESC;
```

- 그룹별로 데이터 조회
```sql
SELECT 컬럼, COUNT(*) FROM 테이블이름 GROUP BY 컬럼;
```
