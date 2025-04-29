
-- создаём основную базу, с которой будем работать
```sql
CREATE DATABASE store;
```

-- добавляем отдельную роль для выполнения миграций
```sql
CREATE ROLE migrations LOGIN PASSWORD '!123';
```

-- разрешаем обращаться к объектам схемы public
```sql
GRANT USAGE ON SCHEMA public TO migrations;
```

-- разрешаем создавать объекты в схеме public (нужно для работы Flyway)
```sql
GRANT CREATE ON SCHEMA public TO migrations;
```

-- предоставляем полный доступ к уже существующим таблицам в этой схеме
```sql
GRANT ALL ON ALL TABLES IN SCHEMA public TO migrations;
```

-- настраиваем автоматическую передачу прав на таблицы, которые будут создаваться в будущем
```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO migrations;
```

-- Количество заказанных сосисок по дням за последние 7 суток
SELECT
  o.date_created,                
  SUM(op.quantity) AS total_sold
FROM orders AS o
JOIN order_product AS op
  ON o.id = op.order_id
WHERE
  o.status = 'shipped'
  AND o.date_created > NOW() - INTERVAL '7 DAY'
GROUP BY
  o.date_created
ORDER BY
  o.date_created;
