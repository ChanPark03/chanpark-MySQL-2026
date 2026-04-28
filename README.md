# chanpark-MySQL-2026

## MySQL studying repo

### 2026-04-28

show databases;
create database iot;
use iot;
create table student(
    id int,  
    name varchar(20)
);

create table apply(
    id int,
    stu_name varchar(20)
);  

show tables;

create database madang;
use madang;
show databases;

create table Book(
bookid int PRIMARY KEY,
bookname varchar(40),
publisher varchar(40),
price int
);

desc book;
drop table book; = table 삭제.

create table Customer(
custid int PRIMARY KEY,
name varchar(40),
address varchar(40),
phone varchar(40)
);

create table Orders(
orderid int PRIMARY KEY,
custid int,
bookid int,
saleprice int,
orderdate date,
FOREIGN KEY (custid) REFERENCES Customer(custid),
FOREIGN KEY (bookid) REFERENCES Book(bookid)
);

조회
select * from book;

insert into book values(1, '축구의 역사', '굿스포츠', 7000);
insert into book values(2, '축구 아는 여자', '나무수', 13000);
insert into book values(3, '축구의 이해', '대한미디어', 22000);
insert into book values(4, '골프 바이블', '대한미디어', 35000);
insert into book values(5, '피겨 교본', '굿스포츠', 8000);
insert into book values(6, '배구 단계별기술', '굿스포츠', 6000);
insert into book values(7, '야구의 추억', '이상미디어', 20000);
insert into book values(8, '야구를 부탁해', '이상미디어', 13000);
insert into book values(9, '올림픽 이야기', '삼성당', 7500);
insert into book values(10, 'Olympic Champions', 'Pearson', 13000);

insert into customer values(1,'박지성','영국 맨체스타','000-5000-0001');
insert into customer values(2, '김연아', '대한민국 서울', '000-6000-0001');
insert into customer values(3, '김연경', '대한민국 경기도', '000-7000-0001');
insert into customer values(4, '추신수', '미국 클리블랜드', '000-8000-0001');
insert into customer values(5, '박세리', '대한민국 대전', NULL);

insert into orders values(1, 1, 1, 6000, STR_TO_DATE('2024-07-01', '%Y-%m-%d'));
insert into orders values(2, 1, 3, 21000, STR_TO_DATE('2024-07-03', '%Y-%m-%d'));
insert into orders values(3, 2, 5, 8000, STR_TO_DATE('2024-07-03', '%Y-%m-%d'));
insert into orders values(4, 3, 6, 6000, STR_TO_DATE('2024-07-04', '%Y-%m-%d'));
insert into orders values(5, 4, 7, 20000, STR_TO_DATE('2024-07-05', '%Y-%m-%d'));
insert into orders values(6, 1, 2, 12000, STR_TO_DATE('2024-07-07', '%Y-%m-%d'));
insert into orders values(7, 4, 8, 13000, STR_TO_DATE('2024-07-07', '%Y-%m-%d'));
insert into orders values(8, 3, 10, 12000, STR_TO_DATE('2024-07-08', '%Y-%m-%d'));
insert into orders values(9, 2, 10, 7000, STR_TO_DATE('2024-07-09', '%Y-%m-%d'));
insert into orders values(10, 3, 8, 13000, STR_TO_DATE('2024-07-10', '%Y-%m-%d'));

select bookname, price, from Book;  

select DISTINCT publisher from Book;  distcint는 중복 제거.  

select * from book where price < 20000; price가 20000보다 작은 책 조회.

select * from book where price BETWEEN 10000 AND 20000; price가 10000과 20000 사이인 책 조회.  

select * from book where publisher IN ('굿스포츠', '대한미디어'); publisher가 굿스포츠 또는 대한미디어인 책 조회.  

select * from book where price is null; price가 null인 책 조회.  is not null은 null이 아닌 값 조회.

select * from book where publisher = '굿스포츠' or publisher = '대한미디어'; publisher가 굿스포츠 또는 대한미디어인 책 조회.  

select bookname, publisher from book where bookname like '축구의 역사'; like 와 in 의 차이점은 like는 패턴 매칭을 사용하여 문자열을 검색하는 반면, in은 특정 값 목록에서 일치하는 값을 검색합니다. like는 와일드카드 문자(%, _)를 사용하여 패턴을 정의할 수 있지만, in은 단순히 값 목록을 나열합니다.  

select bookname from book where bookid = 3; bookid가 3인 책의 이름 조회.  

도서를 이름순으로 정렬할때  
select * from book order by bookname; bookname을 기준으로 오름차순 정렬.  

가격순으로 검색하고 가격이 같으면 이름순으로 정렬할 때  
select * from book order by price, bookname;  

도서의 가격을 내림차순으로 검색하고 가격이 같다면 출판사를 오름차순으로 정렬할 때  
select * from book order by price desc, publisher asc;  

### 집계함수와 group by  

SUM, AVG, MAX, MIN, COUNT 등이 집계함수이다.  

select SUM(saleprice) from orders; 주문의 총 판매 가격 조회.  

select SUM(saleprice) AS 총매출 from orders;  

select SUM(saleprice) AS 총매출 from orders where custid = 2; 2번 고객이 주문한 도서의 총판매액 조회.  

고객이 주문한 도서의 총판매액, 평균값, 최저가, 최고가 조회.  
select SUM(saleprice) AS Total,  
AVG(saleprice) AS Average,  
MIN(saleprice) AS Minimum,  
MAX(saleprice) AS Maximum  
from orders;  

마당 서점의 도서 판매 건수 조회.  
select COUNT(*) from orders; (*)는 모든 컬럼을 의미한다.  

고객별로 주문한 도서의 총수량과 총판매액 조회;  
select custid, COUNT(*) AS 도서수량, SUM(saleprice) AS 총액 from orders group by custid;  

group by는 특정 컬럼을 기준으로 데이터를 그룹화하여 집계함수를 적용할 때 사용한다. group by 뒤에는 그룹화할 컬럼이 오며, 집계함수는 그룹화된 데이터에 대해 계산된다. group by를 사용하면 각 그룹별로 집계된 결과를 얻을 수 있다.

가격이 8000원 이상인 도서를 구매한 고객에 대해 고객별 주문 도서의 총수량을 조회하고 총수량이 2이상인 고객만 조회.  
select custid, COUNT(*) AS 도서수량  
from orders  
where saleprice >= 8000  
group by custid  
having COUNT(*) >= 2;  

having 절은 group by 절과 함께 사용  

### join 조인  

select *
from customer, orders; 이렇게 조건없이 작성하면 모든 경우의 수 가 나온다.  

select *
from customer, orders
where customer.custid = orders.custid; 조건을 추가한 쿼리. custid가 같은 경우에만 조인된다.  

select *
from customer, orders
where customer.custid = orders.custid
order by customer.custid; order by 를 사용해 고객별로 정렬.  

select name, saleprice
from customer, orders
where customer.custid = orders.custid; where 절을 사용하여 정렬.  

고객별로 주문한 모든 도서의 총판매액을 구하고, 고객별로 정렬.
select name, SUM(saleprice)
from customer, orders
where customer.custid = orders.custid
group by customer.name
order by customer.name;  

self join은 자기 자신과 조인하는 경우에 사용한다. 
select a.name AS 고객명, b.name AS 도서명
from orders orders
JOIN customer a ON orders.custid = a.custid
JOIN book b ON orders.bookid = b.bookid;  셀프 조인 예시. orders 테이블을 두 번 조인하여 고객명과 도서명을 조회한다. orders 테이블을 a와 b로 각각 조인하여 고객명과 도서명을 가져온다.   