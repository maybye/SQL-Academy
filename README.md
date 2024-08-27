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

44. Найдите максимальный возраст (количество лет) среди обучающихся 10 классов на сегодняшний день. Для получения текущих даты и времени используйте функцию NOW(). ★
<details><summary>Решение</summary>

  ```
SELECT MAX(TIMESTAMPDIFF(year, birthday, NOW())) as max_year
FROM Student
    JOIN Student_in_class
    ON Student.id = Student_in_class.student
    JOIN Class
    ON Student_in_class.class = Class.id
WHERE name LIKE "10%"
  ```
</details>

45. Какие кабинеты чаще всего использовались для проведения занятий? Выведите те, которые использовались максимальное количество раз. ★
<details><summary>Решение</summary>

  ```
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING COUNT(*) = 
        (SELECT COUNT(classroom) as t
        FROM Schedule
        GROUP BY classroom
        ORDER BY t DESC
        LIMIT 1)
  ```
</details>

46. В каких классах введет занятия преподаватель "Krauze" ? ★
<details><summary>Решение</summary>

  ```
SELECT DISTINCT name
FROM Teacher
    JOIN Schedule
    ON Schedule.teacher = Teacher.id
    JOIN Class
    ON Schedule.class = Class.id
WHERE last_name = 'Krauze'
  ```
</details> 

47. Сколько занятий провел Krauze 30 августа 2019 г.? ★
<details><summary>Решение</summary>

  ```
SELECT COUNT(date) as count
FROM Teacher
    JOIN Schedule
    ON Schedule.teacher = Teacher.id
WHERE DATE_FORMAT(date, '%d-%m-%Y') = '30-08-2019' and last_name = "Krauze" 
  ```
</details>

48. Выведите заполненность классов в порядке убывания ★
<details><summary>Решение</summary>

  ```
SELECT COUNT(student) as count, name
FROM Student_in_class
    JOIN Class
    ON Student_in_class.class = Class.id
GROUP BY name
ORDER BY count DESC
  ```
</details>

49. Какой процент обучающихся учится в "10 A" классе? Выведите ответ в диапазоне от 0 до 100 с округлением до четырёх знаков после запятой, например, 96.0201 ★
<details><summary>Решение</summary>

  ```
SELECT COUNT(*) / (SELECT COUNT(*) FROM Student_in_class) * 100 as percent
FROM Student_in_class
    JOIN Class
    ON Class.id = Student_in_class.class
WHERE name = "10 A"
  ```
</details>
<details><summary>Решение 2</summary>

  ```
WITH t AS   
    (SELECT COUNT(student) as st
    FROM Student_in_class
        JOIN Class
        ON Student_in_class.class = Class.id
    WHERE name = "10 A")

SELECT ROUND( ((SELECT st FROM t) / COUNT(id) * 100 ), 4) as percent
FROM Student_in_class
  ```
</details>

50. Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону. ★
<details><summary>Решение</summary>

  ```
SELECT COUNT(student) as count, name
SELECT FLOOR(COUNT(*) / (SELECT COUNT(*) FROM Student)* 100 ) as percent
FROM Student
WHERE DATE_FORMAT(birthday, "%Y") = '2000'
  ```
</details>

51. Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods). ★
<details><summary>Решение</summary>

  ```
INSERT INTO Goods
SET good_id =
    (SELECT COUNT(*) + 1
    FROM Goods as g),
    
    good_name = 'Cheese',
    type = (SELECT good_type_id
        FROM GoodTypes
        WHERE good_type_name = 'food')
  ```
</details>

52. Добавьте в список типов товаров (GoodTypes) новый тип "auto". ★
<details><summary>Решение</summary>

  ```
INSERT INTO GoodTypes
SET good_type_id = (
		SELECT COUNT(*) + 1
		FROM GoodTypes AS g
	),
	good_type_name = 'auto'
  ```
</details>

53. Измените имя "Andie Quincey" на новое "Andie Anthony".
<details><summary>Решение</summary>

  ```
UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey';
  ```
</details>

54. Удалить всех членов семьи с фамилией "Quincey". ★
<details><summary>Решение</summary>

  ```
DELETE 
FROM FamilyMembers
WHERE member_name LIKE "%Quincey%"
  ```
</details>

55. Удалить компании, совершившие наименьшее количество рейсов.
<details><summary>Решение</summary>

  ```
DELETE 
FROM Company
WHERE id IN (
		SELECT company
		FROM Trip
		GROUP BY company
		HAVING COUNT(*) = (
				SELECT COUNT(id) AS count
				FROM Trip
				GROUP BY company
				ORDER BY count 
					LIMIT 1
			)
	)
  ```
</details>

56. Удалить все перелеты, совершенные из Москвы (Moscow).
<details><summary>Решение</summary>

  ```
DELETE
FROM trip
WHERE town_from = 'Moscow';
  ```
</details>

57. Перенести расписание всех занятий на 30 мин. вперед. ★
<details><summary>Решение</summary>

  ```
UPDATE Timepair
SET start_pair = ADDTIME (start_pair, '00:30:00'), 
end_pair = ADDTIME (end_pair, '00:30:00')
  ```
</details>

58. Добавить отзыв с рейтингом 5 на жилье, находящиеся по адресу "11218, Friel Place, New York", от имени "George Clooney"
<details><summary>Решение</summary>

  ```
INSERT INTO Reviews
SET id = (
		SELECT COUNT(*) + 1
		FROM Reviews as r
	),
	reservation_id = (
		SELECT Reservations.id
		FROM Reservations 
		    JOIN Users 
		    ON Reservations.user_id = Users.id
		    JOIN Rooms
		    ON Reservations.room_id = Rooms.id
		WHERE name = 'George Clooney' and 
		      address = '11218, Friel Place, New York'
	),
	rating = 5
  ```
</details>

59. Вывести пользователей,указавших Белорусский номер телефона ? Телефонный код Белоруссии +375. ★
<details><summary>Решение</summary>

  ```
SELECT *
FROM Users
WHERE phone_number LIKE "+375%"
  ```
</details>

60. Выведите идентификаторы преподавателей, которые хотя бы один раз за всё время преподавали в каждом из одиннадцатых классов.
<details><summary>Решение</summary>

  ```
SELECT DISTINCT teacher
FROM Schedule
    JOIN Class
    ON Class.id = Schedule.class
WHERE name LIKE "11%"
GROUP BY teacher
HAVING COUNT(DISTINCT name) = (SELECT COUNT(DISTINCT name) FROM Class WHERE name LIKE "11%")
  ```
</details>

61. Выведите список комнат, которые были зарезервированы хотя бы на одни сутки в 12-ую неделю 2020 года. В данной задаче в качестве одной недели примите период из семи дней, первый из которых начинается 1 января 2020 года. Например, первая неделя года — 1–7 января, а третья — 15–21 января. ★
<details><summary>Решение</summary>

  ```
SELECT Rooms.*
FROM Rooms
    JOIN Reservations
    ON Reservations.room_id = Rooms.id
WHERE WEEK(start_date, 1) = 12 AND YEAR(start_date) = 2020
  ```
</details>

62. Вывести в порядке убывания популярности доменные имена 2-го уровня, используемые пользователями для электронной почты. Полученный результат необходимо дополнительно отсортировать по возрастанию названий доменных имён. ★
<details><summary>Решение</summary>

  ```
SELECT SUBSTRING_INDEX(email, "@", -1) as domain, COUNT(email) as count
FROM Users
GROUP BY domain
ORDER BY count DESC, domain 
  ```
</details>

63. Выведите отсортированный список (по возрастанию) фамилий и имен студентов в виде Фамилия.И. ★
<details><summary>Решение</summary>

  ```
SELECT CONCAT(last_name, '.' ,LEFT(first_name, 1), '.') as name
FROM Student
ORDER BY name
  ```
</details>

64. Вывести количество бронирований по каждому месяцу каждого года, в которых было хотя бы 1 бронирование. Результат отсортируйте в порядке возрастания даты бронирования. ★
<details><summary>Решение</summary>

  ```
SELECT YEAR(start_date) as year, MONTH(start_date) as month, COUNT(id) as amount
FROM Reservations
GROUP BY year, month
ORDER BY year, month
  ```
</details>

65. Необходимо вывести рейтинг для комнат, которые хоть раз арендовали, как среднее значение рейтинга отзывов округленное до целого вниз. ★
<details><summary>Решение</summary>

  ```
SELECT room_id, FLOOR(AVG(rating)) as rating
FROM Reservations
    JOIN Reviews
    ON Reviews.reservation_id = Reservations.id
GROUP BY room_id
-- ORDER BY rating
  ```
</details>

66. Вывести список комнат со всеми удобствами (наличие ТВ, интернета, кухни и кондиционера), а также общее количество дней и сумму за все дни аренды каждой из таких комнат. ★
<details><summary>Решение</summary>

  ```
SELECT home_type, address, 
    IFNULL(SUM(total/Reservations.price), 0) as days,
    IFNULL(SUM(total), 0) as total_fee
FROM Rooms
    LEFT JOIN Reservations
    ON Reservations.room_id = Rooms.id
WHERE has_air_con = 1 AND has_tv = 1 AND has_kitchen = 1 AND has_internet = 1
GROUP BY home_type, address
  ```
</details>

67. Вывести время отлета и время прилета для каждого перелета в формате "ЧЧ:ММ, ДД.ММ - ЧЧ:ММ, ДД.ММ", где часы и минуты с ведущим нулем, а день и месяц без. ★
<details><summary>Решение</summary>

  ```
SELECT CONCAT(
		DATE_FORMAT(time_out, '%H:%i, %e.%c'),
		' - ',
		DATE_FORMAT(time_in, '%H:%i, %e.%c')
	) as flight_time
FROM Trip
  ```
</details>

68. Для каждой комнаты, которую снимали как минимум 1 раз, найдите имя человека, снимавшего ее последний раз, и дату, когда он выехал.
<details><summary>Решение</summary>

  ```
SELECT room_id,
	name,
	end_date
FROM Reservations
	JOIN Users ON Reservations.user_id = Users.id
WHERE end_date IN (
		SELECT MAX(end_date)
		FROM Reservations
		GROUP BY room_id
	)
  ```
</details>

69. Вывести идентификаторы всех владельцев комнат, что размещены на сервисе бронирования жилья и сумму, которую они заработали
<details><summary>Решение</summary>

  ```
SELECT owner_id, IFNULL(SUM(total), 0) as total_earn
FROM Rooms
    LEFT JOIN Reservations
    ON Reservations.room_id = Rooms.id 
GROUP BY owner_id

  ```
</details>

70. Необходимо категоризовать жилье на economy, comfort, premium по цене соответственно <= 100, 100 < цена < 200, >= 200. В качестве результата вывести таблицу с названием категории и количеством жилья, попадающего в данную категорию ★
<details><summary>Решение</summary>

  ```
SELECT CASE
    WHEN price  >= 200 THEN 'premium'
    WHEN price  > 100 THEN 'comfort'
    ELSE 'economy'
    END AS category,
    COUNT(id) as count
FROM Rooms
GROUP BY category

  ```
</details>

71. Найдите какой процент пользователей, зарегистрированных на сервисе бронирования, хоть раз арендовали или сдавали в аренду жилье. Результат округлите до сотых.
<details><summary>Решение</summary>

  ```
SELECT 
    Round((COUNT(DISTINCT id)/(SELECT COUNT(id) FROM Users) * 100), 2) as percent
FROM Users
WHERE id in (SELECT DISTINCT user_id
FROM Reservations) OR 
    id in (SELECT DISTINCT owner_id
FROM Reservations
    LEFT JOIN Rooms
    ON Rooms.id = Reservations.room_id)

  ```
</details>

72. Выведите среднюю цену бронирования за сутки для каждой из комнат, которую бронировали хотя бы один раз. Среднюю цену необходимо округлить до целого значения вверх. ★
<details><summary>Решение</summary>

  ```
SELECT room_id, CEILING(AVG(price)) as avg_price 
FROM Reservations
GROUP BY room_id

  ```
</details>

73. Выведите id тех комнат, которые арендовали нечетное количество раз ★
<details><summary>Решение</summary>

  ```
SELECT room_id, COUNT(*) as count
FROM Reservations
GROUP BY room_id
HAVING COUNT(*) % 2 != 0

  ```
</details>

74. Выведите идентификатор и признак наличия интернета в помещении. Если интернет в сдаваемом жилье присутствует, то выведите «YES», иначе «NO».
<details><summary>Решение</summary>

  ```
SELECT id, 
        IF(has_internet = 1, 'YES', 'NO') as has_internet
FROM ROOMS

  ```
</details>

75. Выведите фамилию, имя и дату рождения студентов, кто был рожден в мае.
<details><summary>Решение</summary>

  ```
SELECT last_name, first_name, birthday
FROM Student
WHERE MONTH(birthday) = 5

  ```
</details>

76. Вывести имена всех пользователей сервиса бронирования жилья, а также два признака: является ли пользователь собственником какого-либо жилья (is_owner) и является ли пользователь арендатором (is_tenant). В случае наличия у пользователя признака необходимо вывести в соответствующее поле 1, иначе 0. ★
<details><summary>Решение</summary>

  ```
SELECT name,
    IF(id IN (SELECT user_id FROM Reservations), 1, 0) as is_tenant,
    IF(id IN (SELECT owner_id FROM Rooms), 1, 0) as is_owner
FROM Users

  ```
</details>

77. Создайте представление с именем "People", которое будет содержать список имен (first_name) и фамилий (last_name) всех студентов (Student) и преподавателей(Teacher) ★
<details><summary>Решение</summary>

  ```
CREATE VIEW People AS 
SELECT first_name, last_name
FROM Student
UNION
SELECT first_name, last_name
FROM Teacher

  ```
</details>

78. Выведите всех пользователей с электронной почтой в «hotmail.com» ★
<details><summary>Решение</summary>

  ```
SELECT *
FROM Users
WHERE email LIKE "%@hotmail.com"

  ```
</details>

79. Выведите поля id, home_type, price у всего жилья из таблицы Rooms. Если комната имеет телевизор и интернет одновременно, то в качестве цены в поле price выведите цену, применив скидку 10%. ★
<details><summary>Решение</summary>

  ```
SELECT CASE 
    WHEN has_internet = 1 AND has_tv = 1 THEN price * 0.9
    ELSE price
    END as price,
    id, home_type
FROM Rooms
  ```
</details>

80. Создайте представление «Verified_Users» с полями id, name и email, которое будет показывает только тех пользователей, у которых подтвержден адрес электронной почты. ★
<details><summary>Решение</summary>

  ```
CREATE VIEW Verified_Users AS 
SELECT id, name, email
FROM Users
WHERE email_verified_at IS NOT NULL
  ```
</details>

81 (93). Какой средний возраст клиентов, купивших Smartwatch (использовать наименование товара product.name) в 2024 году? ★
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

82 (94). Вывести имена покупателей, каждый из которых приобрёл Laptop и Monitor (использовать наименование товара product.name) в марте 2024 года? ★
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

83 (97). Посчитать количество работающих складов на текущую дату по каждому городу. Вывести только те города, у которых количество складов более 80. Данные на выходе - город, количество складов. ★
<details><summary>Решение</summary>

  ```
SELECT city, COUNT(warehouse_id) as warehouse_count
FROM Warehouses
WHERE date_close IS NULL 
GROUP BY city
HAVING COUNT(*) > 80
  ```
</details>

84 (99). Посчитай доход с женской аудитории (доход = сумма(price * items)). Обратите внимание, что в таблице женская аудитория имеет поле user_gender «female» или «f».
<details><summary>Решение</summary>

  ```
SELECT SUM(price*items) as income_from_female
FROM Purchases
WHERE user_gender LIKE 'f%'
  ```
</details>

85 (101).Выведи для каждого пользователя первое наименование, которое он заказал (первое по времени транзакции). ★
<details><summary>Решение</summary>

  ```
SELECT user_id, item
FROM Transactions
WHERE transaction_ts IN (SELECT MIN(transaction_ts)
FROM Transactions
GROUP BY user_id)
  ```
</details>

86 (103). Вывести список имён сотрудников, получающих большую заработную плату, чем у непосредственного руководителя.
<details><summary>Решение</summary>

  ```
SELECT employees.name
FROM Employee AS employees, Employee AS chieves
WHERE chieves.id = employees.chief_id AND employees.salary > chieves.salary;
  ```
</details>

87 (109). Выведите название страны, где находится город «Salzburg»
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

88 (111). Посчитайте население каждого региона. В качестве результата выведите название региона и его численность населения. ★
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
