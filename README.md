# Решение заданий из тренажера SQL Academy

★ - задачи повышенного уровня сложности согласно SQL Academy

1. Вывести имена всех людей, которые есть в базе данных авиакомпаний

<details><summary>Решение</summary>

  ```
SELECT name
FROM Passenger
  ```
</details>

2. Вывести названия всеx авиакомпаний 
<details><summary>Решение</summary>

  ```
SELECT name
FROM Company
  ```
</details>

3. Вывести все рейсы, совершенные из Москвы 
<details><summary>Решение</summary>

  ```
SELECT *
FROM Trip
WHERE town_from = 'Moscow'
  ```
</details>

4. Вывести имена людей, которые заканчиваются на "man"
<details><summary>Решение</summary>

  ```
SELECT name
FROM Passenger
WHERE name LIKE "%man"
  ```
</details>

5. Вывести количество рейсов, совершенных на TU-134
<details><summary>Решение</summary>

  ```
SELECT COUNT(id) as count
FROM Trip
WHERE plane = 'TU-134'
  ```
</details>

6. Какие компании совершали перелеты на Boeing
<details><summary>Решение</summary>

  ```
SELECT DISTINCT  name
FROM Company
    JOIN Trip
    ON Company.id = Trip.company
WHERE plane = 'Boeing'
  ```
</details>

7. Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)
<details><summary>Решение</summary>

  ```
SELECT DISTINCT plane
FROM Trip
WHERE town_to = 'Moscow'
  ```
</details>

8. В какие города можно улететь из Парижа (Paris) и сколько времени это займёт? ★
<details><summary>Решение</summary>

  ```
SELECT town_to, 
TIMEDIFF (time_in, time_out) as flight_time 
FROM Trip
WHERE town_from ='Paris'
  ```
</details>

9. Какие компании организуют перелеты из Владивостока (Vladivostok)?
<details><summary>Решение</summary>

  ```
SELECT name
FROM Company
    JOIN Trip
    ON Company.id = Trip.company
WHERE town_from = 'Vladivostok'
  ```
</details>

10. Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г. ★
<details><summary>Решение</summary>

  ```
SELECT *
FROM Trip
WHERE DATE_FORMAT(time_out, '%H') BETWEEN 10 and 13
	OR DATE_FORMAT(time_out, '%T') = '14:00:00'
	AND DATE_FORMAT(time_out, '%d') = 1
	AND DATE_FORMAT(time_out, '%b') = 'Jan'
	AND DATE_FORMAT(time_out, '%Y') = 1900
  ```
</details>

11. Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени. ★
<details><summary>Решение</summary>

  ```
SELECT name
FROM Passenger
WHERE LENGTH(name) = (
		SELECT (MAX(LENGTH(name)))
		FROM Passenger
	)
  ```
</details>

12. Вывести id и количество пассажиров для всех прошедших полётов
<details><summary>Решение</summary>

  ```
SELECT trip, COUNT(passenger) as count
FROM Pass_in_trip 
GROUP BY trip
    
  ```
</details>

13. Вывести имена людей, у которых есть полный тёзка среди пассажиров ★
<details><summary>Решение</summary>

  ```
SELECT name
FROM Passenger
GROUP BY name
HAVING COUNT(name) > 1
    
  ```
</details>

14. В какие города летал Bruce Willis
<details><summary>Решение</summary>

  ```
SELECT town_to
FROM Trip
JOIN Pass_in_trip
ON trip.id = Pass_in_trip.trip
JOIN Passenger
ON Passenger.id = Pass_in_trip.passenger
WHERE name = 'Bruce Willis'
    
  ```
</details>

15. Выведите дату и время прилёта пассажира Стив Мартин (Steve Martin) в Лондон (London)
<details><summary>Решение</summary>

  ```
SELECT time_in
FROM Trip
JOIN Pass_in_trip
ON trip.id = Pass_in_trip.trip
JOIN Passenger
ON Passenger.id = Pass_in_trip.passenger
WHERE name = 'Steve Martin' and town_to = 'London'
    
  ```
</details>

16. Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет. ★
<details><summary>Решение</summary>

  ```
SELECT name, COUNT(trip) as count
FROM Passenger
	JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
GROUP BY name
HAVING COUNT(trip) >= 1
ORDER BY count DESC, name
    
  ```
</details>

17. Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили. ★
<details><summary>Решение</summary>

  ```
SELECT member_name,
	status, SUM(unit_price * amount) as costs
FROM FamilyMembers
	JOIN Payments ON FamilyMembers.member_id = Payments.family_member
WHERE DATE_FORMAT(date, "%Y") = 2005
GROUP BY member_name, status
HAVING costs IS NOT NULL
    
  ```
</details>

18. Выведите имя самого старшего человека. Если таких несколько, то выведите их всех. ★
<details><summary>Решение</summary>

  ```
SELECT member_name
FROM FamilyMembers
ORDER BY birthday ASC
LIMIT 1;
    
  ```
</details>

19. Определить, кто из членов семьи покупал картошку (potato)
<details><summary>Решение</summary>

  ```
SELECT DISTINCT  status
FROM FamilyMembers
    JOIN  Payments
    ON FamilyMembers.member_id = Payments.family_member
    JOIN Goods
    ON Payments.good = Goods.good_id
WHERE good_name = 'potato'
    
  ```
</details>

20. Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму. ★
<details><summary>Решение</summary>

  ```
SELECT member_name, status, SUM(amount*unit_price) as costs
FROM FamilyMembers 
    JOIN Payments 
    ON FamilyMembers.member_id = Payments.family_member
    JOIN Goods
    ON Payments.good = Goods.good_id
    JOIN GoodTypes
    ON Goods.type = GoodTypes.good_type_id
WHERE good_type_name = 'entertainment'
GROUP BY member_name, status
    
  ```
</details>

21. Определить товары, которые покупали более 1 раза. ★
<details><summary>Решение</summary>

  ```
SELECT good_name
FROM Goods
	JOIN Payments ON Payments.good = Goods.good_id
GROUP BY good_name
HAVING COUNT(payment_id) > 1
    
  ```
</details>

22. Найти имена всех матерей (mother)
<details><summary>Решение</summary>

  ```
SELECT member_name
FROM FamilyMembers
WHERE status = 'mother'
    
  ```
</details>

23. Найдите самый дорогой деликатес (delicacies) и выведите его цену. ★
<details><summary>Решение</summary>

  ```
SELECT good_name, unit_price
FROM Goods
    JOIN Payments
    ON Goods.good_id = Payments.good
    JOIN GoodTypes
    ON Goods.type = GoodTypes.good_type_id
WHERE good_type_name = 'delicacies'
ORDER BY unit_price DESC 
LIMIT 1
    
  ```
</details>

24. Определить кто и сколько потратил в июне 2005. ★
<details><summary>Решение</summary>

  ```
SELECT member_name, SUM(unit_price*amount) as costs
FROM FamilyMembers
    JOIN Payments
    ON FamilyMembers.member_id = Payments.family_member
WHERE DATE_FORMAT(date, "%m/%Y") = '06/2005'
GROUP BY member_name
    
  ```
</details>

25. Определить, какие товары не покупались в 2005 году. ★
<details><summary>Решение</summary>

  ```
SELECT good_name
FROM Goods g
WHERE NOT EXISTS (
    SELECT 1
    FROM Payments p
    WHERE p.good = g.good_id
      AND DATE_FORMAT(p.date, "%Y") = 2005
)
  ```
</details>

26. Определить группы товаров, которые не приобретались в 2005 году. ★
<details><summary>Решение</summary>

  ```
SELECT good_type_name
FROM  GoodTypes gt 
WHERE NOT EXISTS (
    SELECT 1
    FROM Goods g
    JOIN Payments p ON g.good_id = p.good
    WHERE g.type = gt.good_type_id
      AND DATE_FORMAT(p.date, "%Y") = 2005
)
GROUP BY good_type_id
  ```
</details>

27. Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её. ★
<details><summary>Решение</summary>

  ```
SELECT good_type_name, SUM(amount*unit_price) as costs
FROM Goods
    JOIN Payments
    ON Goods.good_id = Payments.good
    JOIN GoodTypes
    ON GoodTypes.good_type_id = Goods.type
WHERE YEAR(date) = 2005
GROUP BY good_type_name

  ```
</details>

28. Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ?
<details><summary>Решение</summary>

  ```
SELECT COUNT(id) as count
FROM Trip
WHERE town_from = 'Rostov' AND town_to = 'Moscow'

  ```
</details>

29. Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134. ★
<details><summary>Решение</summary>

  ```
SELECT DISTINCT name
FROM Pass_in_trip pt
    JOIN Trip t
    ON pt.trip = t.id
    JOIN Passenger p
    ON p.id = pt.passenger
WHERE town_to = 'Moscow' and plane = 'TU-134'

  ```
</details>

30. Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности. ★
<details><summary>Решение</summary>

  ```
SELECT COUNT(passenger) as count, trip
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC

  ```
</details>

31. Вывести всех членов семьи с фамилией Quincey. ★
<details><summary>Решение</summary>

  ```
SELECT *
FROM FamilyMembers
WHERE member_name LIKE '%Quincey%'

  ```
</details>

32. Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону. ★
<details><summary>Решение</summary>

  ```
SELECT ROUND(AVG(TIMESTAMPDIFF(year, birthday, current_date )), 0) as age
FROM FamilyMembers

  ```
</details>

33. Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar). В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры. ★
<details><summary>Решение</summary>

  ```
SELECT AVG(unit_price) as cost
FROM Payments
    JOIN Goods
    ON Payments.good = Goods.good_id
WHERE good_name LIKE "%caviar"

  ```
</details>

34. Сколько всего 10-ых классов.
<details><summary>Решение</summary>

  ```
SELECT COUNT(id) as count
FROM Class
WHERE name LIKE "10%"
  ```
</details>

35. Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий? ★
<details><summary>Решение</summary>

  ```
SELECT COUNT(DISTINCT classroom) as count
FROM Schedule
WHERE DATE_FORMAT(date, "%d/%m/%Y") = '02/09/2019'
  ```
</details>

36. Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?
<details><summary>Решение</summary>

  ```
SELECT *
FROM Student
WHERE address LIKE "%ul. Pushkina%"
  ```
</details>

37. Сколько лет самому молодому обучающемуся ? ★
<details><summary>Решение</summary>

  ```
SELECT TIMESTAMPDIFF(year, birthday, current_date) as year
FROM Student
ORDER BY year 
LIMIT 1
  ```
</details>

38. Сколько Анн (Anna) учится в школе ?
<details><summary>Решение</summary>

  ```
SELECT COUNT(id) as count
FROM Student
WHERE first_name = 'Anna'
  ```
</details>

39. Сколько обучающихся в 10 B классе ?
<details><summary>Решение</summary>

  ```
SELECT COUNT(student) as count
FROM Student_in_class
    JOIN Class
    ON Class.id = Student_in_class.class
WHERE name LIKE "10 B"
  ```
</details>

40. Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.). Обратите внимание, что в базе данных есть несколько учителей с такими фамилией и инициалами. ★
<details><summary>Решение</summary>

  ```
SELECT name as subjects
FROM Schedule
    JOIN Teacher
    ON Teacher.id = Schedule.teacher
    JOIN Subject
    ON Subject.id = Schedule.subject
WHERE first_name LIKE "P%" AND middle_name LIKE "P%" AND last_name = 'Romashkin'
  ```
</details>

41. Выясните, во сколько по расписанию начинается четвёртое занятие.
<details><summary>Решение</summary>

  ```
SELECT DISTINCT start_pair
FROM Schedule
    JOIN Timepair
    ON Schedule.number_pair = Timepair.id
WHERE number_pair = 4
  ```
</details>

42 (53). Измените имя "Andie Quincey" на новое "Andie Anthony".
<details><summary>Решение</summary>

  ```
UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey';
  ```
</details>

43 (56). Удалить все перелеты, совершенные из Москвы (Moscow).
<details><summary>Решение</summary>

  ```
DELETE
FROM trip
WHERE town_from = 'Moscow';
  ```
</details>

44 (74). Выведите идентификатор и признак наличия интернета в помещении. Если интернет в сдаваемом жилье присутствует, то выведите «YES», иначе «NO».
<details><summary>Решение</summary>

  ```
SELECT id, 
        IF(has_internet = 1, 'YES', 'NO') as has_internet
FROM ROOMS
  ```
</details>

45 (75). Выведите фамилию, имя и дату рождения студентов, кто был рожден в мае.
<details><summary>Решение</summary>

  ```
SELECT last_name, first_name, birthday
FROM Student
WHERE MONTH(birthday) = 5
  ```
</details>

46 (93). Какой средний возраст клиентов, купивших Smartwatch (использовать наименование товара product.name) в 2024 году? ★
<details><summary>Решение</summary>

  ```
WITH orders AS 
    (SELECT  customer_key 
    FROM Purchase
    WHERE product_key IN (SELECT product_key FROM Product WHERE name = 'Smartwatch')
        AND DATE_FORMAT(date, "%Y") = 2024)

SELECT AVG(age) as average_age
FROM Customer
WHERE customer_key IN (SELECT customer_key FROM orders)

  ```
</details>

47 (94). Вывести имена покупателей, каждый из которых приобрёл Laptop и Monitor (использовать наименование товара product.name) в марте 2024 года? ★
<details><summary>Решение</summary>

  ```
WITH t AS 
    (SELECT customer_key 
    FROM Purchase
    WHERE product_key IN (SELECT product_key FROM Product WHERE name = 'Laptop') 
        AND DATE_FORMAT(date, "%Y") = 2024 
        AND DATE_FORMAT(date, "%b") = 'Mar'),
    t2 AS
    (SELECT customer_key 
    FROM Purchase
    WHERE product_key IN (SELECT product_key FROM Product WHERE name = 'Monitor') 
        AND DATE_FORMAT(date, "%Y") = 2024 
        AND DATE_FORMAT(date, "%b") = 'Mar')
  ```
</details>

48 (99). Посчитай доход с женской аудитории (доход = сумма(price * items)). Обратите внимание, что в таблице женская аудитория имеет поле user_gender «female» или «f».
<details><summary>Решение</summary>

  ```
SELECT SUM(price*items) as income_from_female
FROM Purchases
WHERE user_gender LIKE 'f%'
  ```
</details>

49 (103). Вывести список имён сотрудников, получающих большую заработную плату, чем у непосредственного руководителя.
<details><summary>Решение</summary>

  ```
SELECT employees.name
FROM Employee AS employees, Employee AS chieves
WHERE chieves.id = employees.chief_id AND employees.salary > chieves.salary;
  ```
</details>

50 (109). Выведите название страны, где находится город «Salzburg»
<details><summary>Решение</summary>

  ```
SELECT Countries.name as country_name
FROM Countries
    JOIN Regions
    ON Countries.id = Regions.countryid
    JOIN Cities
    ON Regions.id = Cities.regionid
WHERE Cities.name = 'Salzburg'
  ```
</details>

51 (111). Посчитайте население каждого региона. В качестве результата выведите название региона и его численность населения. ★
<details><summary>Решение</summary>

  ```
SELECT Regions.name as region_name,
    SUM(population) as  total_population
FROM Cities
    JOIN Regions
    ON Cities.regionid = Regions.id
GROUP BY Regions.name
  ```
</details>
