Запрос SQL для выборки данных в соотношении "многие ко многим"

## Описание
База данных содержит продукты и категории. Одному продукту может соответствовать много категорий, в одной категории может быть много продуктов. Требуется SQL запрос для выбора всех пар «Имя продукта – Имя категории». Если у продукта нет категорий, то его имя все равно должно выводиться.

## Струкутра БД
Промежуточная таблица Realation предназначена для реализации отношения "многие ко многим"

Отношение между таблицами Realation и Product реализована как 1 к 0..* - такая связть позволяет хранить продукты не относящиеся ни к одной категории.

![Database diagram](https://i.ibb.co/4ZVPh4c/Untitled-Diagram-8.jpg)

## Решение

**1 Вариант**
Поиск связей начинается от имеющихся продуктов в таблице Product

    SELECT product_name, category_name
        FROM [SQL_Test].dbo.Product
    LEFT JOIN [SQL_Test].dbo.Relation
		ON  Product.product_id = Relation.product_id
	LEFT JOIN [SQL_Test].dbo.Category
		ON Relation.category_id = Category.category_id

**2 Вариант**
Поиск связей начинается от имеющих категорий в таблице Category и дополнительно включаются все продукты, которые не имеют связей с таблицей Relation

    SELECT category_name, product_name
        FROM [SQL_Test].dbo.Category
	INNER JOIN [SQL_Test].dbo.Relation
		ON Category.category_id = Relation.category_id
	RIGHT JOIN [SQL_Test].dbo.Product
		ON Relation.product_id = Product.product_id
