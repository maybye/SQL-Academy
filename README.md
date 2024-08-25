# Решение заданий из тренажера SQL Academy

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

8. В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?
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

10. Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.
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

11. Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.
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

13. Вывести имена людей, у которых есть полный тёзка среди пассажиров
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

16. Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.
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

17. Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили.
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

18. Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.
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

20. Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму
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

21. Определить товары, которые покупали более 1 раза
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

23. Найдите самый дорогой деликатес (delicacies) и выведите его цену
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

24. Определить кто и сколько потратил в июне 2005
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

25. Определить, какие товары не покупались в 2005 году
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
