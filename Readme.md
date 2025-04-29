# dbops-project
-- создаём основную базу, с которой будем работать
CREATE DATABASE store;

-- добавляем отдельную роль для выполнения миграций
CREATE ROLE migrations LOGIN PASSWORD '!123';

-- разрешаем обращаться к объектам схемы public
GRANT USAGE ON SCHEMA public TO migrations;

-- предоставляем полный доступ к уже существующим таблицам в этой схеме
GRANT ALL ON ALL TABLES IN SCHEMA public TO migrations;

-- настраиваем автоматическую передачу прав на таблицы, которые будут создаваться в будущем
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO migrations;
