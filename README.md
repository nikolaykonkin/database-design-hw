# Домашнее задание к занятию «Базы данных»

**Выполнил:** Николай Конкин

---

### Инструкция по выполнению домашнего задания

1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и вашу фамилию и имя
   - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
   - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
   - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
5. В личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы по выполнению заданий спрашивайте в разделе "Вопросы по заданию" в личном кабинете.

Желаем успехов в выполнении домашнего задания!

---

### Задание 1

Опишите не менее семи таблиц, из которых состоит база данных. Определите, какие данные хранятся в этих таблицах, и какой тип данных у столбцов, если данные хранятся в PostgreSQL. Начертите схему полученной модели данных.

На основе предоставленного файла Excel с отчетом, я выделил следующие сущности для построения базы данных:

#### Таблицы базы данных:

1. **Employees** (Сотрудники) — хранит информацию о сотрудниках.

| Столбец | Тип данных | Описание |
|---------|------------|----------|
| employee_id | SERIAL PRIMARY KEY | Уникальный идентификатор сотрудника |
| full_name | VARCHAR(150) | Полное имя сотрудника |
| salary | DECIMAL(10,2) | Оклад |
| position_id | INTEGER (FK) | Ссылка на должность |
| department_id | INTEGER (FK) | Ссылка на структурное подразделение |
| hire_date | DATE | Дата найма |
| branch_id | INTEGER (FK) | Ссылка на филиал |

2. **Positions** (Должности) — хранит информацию о должностях.

| Столбец | Тип данных | Описание |
|---------|------------|----------|
| position_id | SERIAL PRIMARY KEY | Уникальный идентификатор должности |
| position_title | VARCHAR(100) | Название должности |

3. **Departments** (Структурные подразделения) — хранит информацию о структурных подразделениях.

| Столбец | Тип данных | Описание |
|---------|------------|----------|
| department_id | SERIAL PRIMARY KEY | Уникальный идентификатор подразделения |
| department_name | VARCHAR(200) | Название структурного подразделения |
| department_type_id | INTEGER (FK) | Ссылка на тип подразделения |

4. **DepartmentTypes** (Типы подразделений) — хранит информацию о типах подразделений.

| Столбец | Тип данных | Описание |
|---------|------------|----------|
| type_id | SERIAL PRIMARY KEY | Уникальный идентификатор типа |
| type_name | VARCHAR(50) | Название типа (Отдел/Группа/Департамент) |

5. **Branches** (Филиалы) — хранит информацию о филиалах (адресах).

| Столбец | Тип данных | Описание |
|---------|------------|----------|
| branch_id | SERIAL PRIMARY KEY | Уникальный идентификатор филиала |
| address | TEXT | Полный адрес филиала |

6. **Projects** (Проекты) — хранит информацию о проектах.

| Столбец | Тип данных | Описание |
|---------|------------|----------|
| project_id | SERIAL PRIMARY KEY | Уникальный идентификатор проекта |
| project_name | VARCHAR(200) | Название проекта |

7. **EmployeeProjects** (Назначения на проекты) — связывает сотрудников с проектами (многие ко многим).

| Столбец | Тип данных | Описание |
|---------|------------|----------|
| employee_project_id | SERIAL PRIMARY KEY | Уникальный идентификатор назначения |
| employee_id | INTEGER (FK) | Ссылка на сотрудника |
| project_id | INTEGER (FK) | Ссылка на проект |
| assigned_date | TIMESTAMP | Дата назначения |

Схема базы данных:

![ER-диаграмма](https://github.com/nikolaykonkin/database-design-hw/blob/main/schema_diagram.png)

Связи между таблицами:

1. **Employees** → **Positions**: многие к одному (много сотрудников на одной должности)
2. **Employees** → **Departments**: многие к одному (много сотрудников в одном подразделении)
3. **Departments** → **DepartmentTypes**: многие к одному (много подразделений одного типа)
4. **Employees** → **Branches**: многие к одному (много сотрудников в одном филиале)
5. **EmployeeProjects** → **Employees**: многие к одному (много проектов у одного сотрудника)
6. **EmployeeProjects** → **Projects**: многие к одному (много сотрудников на одном проекте)

---

### Задание 2*

Разверните СУБД Postgres на своей хостовой машине, на виртуальной машине или в контейнере docker. Опишите схему, полученную в предыдущем задании, с помощью скрипта SQL. Создайте в вашей полученной СУБД новую базу данных и выполните полученный ранее скрипт для создания вашей модели данных. В качестве решения приложите SQL-скрипт и скриншот диаграммы.

SQL-скрипт:

```sql
-- Создание базы данных (выполняется отдельно)
-- CREATE DATABASE employee_management;

-- Подключение к базе данных
-- \c employee_management;

-- Таблица типов подразделений
CREATE TABLE DepartmentTypes (
    type_id SERIAL PRIMARY KEY,
    type_name VARCHAR(50) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица должностей
CREATE TABLE Positions (
    position_id SERIAL PRIMARY KEY,
    position_title VARCHAR(100) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица структурных подразделений
CREATE TABLE Departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(200) NOT NULL UNIQUE,
    type_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_department_type FOREIGN KEY (type_id)
        REFERENCES DepartmentTypes(type_id) ON DELETE RESTRICT
);

-- Таблица филиалов
CREATE TABLE Branches (
    branch_id SERIAL PRIMARY KEY,
    address TEXT NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица сотрудников
CREATE TABLE Employees (
    employee_id SERIAL PRIMARY KEY,
    full_name VARCHAR(150) NOT NULL,
    salary DECIMAL(10, 2) NOT NULL,
    position_id INTEGER NOT NULL,
    department_id INTEGER NOT NULL,
    branch_id INTEGER NOT NULL,
    hire_date DATE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_employee_position FOREIGN KEY (position_id)
        REFERENCES Positions(position_id) ON DELETE RESTRICT,
    CONSTRAINT fk_employee_department FOREIGN KEY (department_id)
        REFERENCES Departments(department_id) ON DELETE RESTRICT,
    CONSTRAINT fk_employee_branch FOREIGN KEY (branch_id)
        REFERENCES Branches(branch_id) ON DELETE RESTRICT,
    CONSTRAINT chk_salary CHECK (salary > 0)
);

-- Таблица проектов
CREATE TABLE Projects (
    project_id SERIAL PRIMARY KEY,
    project_name VARCHAR(200) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Таблица назначений на проекты (связь многие ко многим)
CREATE TABLE EmployeeProjects (
    employee_project_id SERIAL PRIMARY KEY,
    employee_id INTEGER NOT NULL,
    project_id INTEGER NOT NULL,
    assigned_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_employeeproject_employee FOREIGN KEY (employee_id)
        REFERENCES Employees(employee_id) ON DELETE CASCADE,
    CONSTRAINT fk_employeeproject_project FOREIGN KEY (project_id)
        REFERENCES Projects(project_id) ON DELETE CASCADE,
    UNIQUE (employee_id, project_id)
);

-- Индексы для оптимизации производительности
CREATE INDEX idx_employees_position ON Employees(position_id);
CREATE INDEX idx_employees_department ON Employees(department_id);
CREATE INDEX idx_employees_branch ON Employees(branch_id);
CREATE INDEX idx_employees_hire_date ON Employees(hire_date);
CREATE INDEX idx_departments_type ON Departments(type_id);
CREATE INDEX idx_employeeprojects_employee ON EmployeeProjects(employee_id);
CREATE INDEX idx_employeeprojects_project ON EmployeeProjects(project_id);

-- Триггер для автоматического обновления updated_at
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_departmenttypes_updated_at BEFORE UPDATE ON DepartmentTypes
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_positions_updated_at BEFORE UPDATE ON Positions
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_departments_updated_at BEFORE UPDATE ON Departments
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_branches_updated_at BEFORE UPDATE ON Branches
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_employees_updated_at BEFORE UPDATE ON Employees
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_projects_updated_at BEFORE UPDATE ON Projects
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- Вставка начальных данных (из файла)
INSERT INTO DepartmentTypes (type_name) VALUES
    ('Отдел'),
    ('Группа'),
    ('Департамент')
ON CONFLICT (type_name) DO NOTHING;

INSERT INTO Positions (position_title) VALUES
    ('ведущий QA инженер'),
    ('специалист'),
    ('специалист по персоналу'),
    ('ведущий разработчик'),
    ('ведущий инженер'),
    ('руководель проектов по интеграции'),
    ('старший инженер'),
    ('инженер'),
    ('руководель сервисных проектов'),
    ('разработчик'),
    ('старший разработчик'),
    ('ведущий архектор'),
    ('старший архектор'),
    ('руководель проектов'),
    ('руководель направления разработки')
ON CONFLICT (position_title) DO NOTHING;

INSERT INTO Departments (department_name, type_id) VALUES
    ('Центр компетенций QA Москва', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Отдел')),
    ('Группа сервисной поддержки', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Группа')),
    ('Центр разработки продуктов для digital-маркетинга', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Отдел')),
    ('Департамент Техническая поддержка', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Департамент')),
    ('Группа CRM 2', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Группа')),
    ('Группа первичной диагностики №2', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Группа')),
    ('Группа Billing', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Группа')),
    ('Группа DOC', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Группа')),
    ('Группа ODS', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Группа')),
    ('Группа Rating', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Группа')),
    ('Центр управления сервисами', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Отдел')),
    ('Департамент FBF', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Департамент')),
    ('Центр анализа и архектуры Medio', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Отдел')),
    ('Центр разработки Medio', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Отдел')),
    ('Группа инфраструктуры', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Группа')),
    ('Департамент Rating and Charging', (SELECT type_id FROM DepartmentTypes WHERE type_name = 'Департамент'))
ON CONFLICT (department_name) DO NOTHING;

INSERT INTO Branches (address) VALUES
    ('Приморский край, г. Владивосток, ул Нижнепортовая, д. 1'),
    ('Краснодарский край, г. Краснодар, ул Путевая, д. 1'),
    ('Ростовская обл, г. Ростов-на-Дону, ул 2-я Краснодарская, д. 135/2')
ON CONFLICT (address) DO NOTHING;

INSERT INTO Projects (project_name) VALUES
    ('Итэлма Инженерный корпус'),
    ('Севастополь ТВ'),
    ('Кристалл Доп объем'),
    ('Ростелеком. Гончарная'),
    ('ВТБ Башня PM'),
    ('Газпромбанк Бирюзова'),
    ('Гпб Оазис Кабинет З.'),
    ('Комплекс Pine Creek Доп работы'),
    ('Сбербанк Нижний Новгород'),
    ('Рособоронэкспорт _ PM'),
    ('Ростелеком Академик'),
    ('Оформление планировочных Итэлма'),
    ('ТМК. Сколково'),
    ('16120_1_TUL (ДС5)'),
    ('ИКСпФОН (РД)'),
    ('Европлан'),
    ('Газпромбанк Аквамарин АН'),
    ('Гурзуф'),
    ('Департамент финансов и кадров'),
    ('Пансионат Дельфин (Крым)'),
    ('Ледовая Арена Кристалл РД АИ'),
    ('Сколково'),
    ('Билайн. Ставрополь'),
    ('ТПУ Томск'),
    ('РТИ'),
    ('ИТЭЛМА'),
    ('Билайн. Нижний Новгород'),
    ('Итэлма АМО ЗИЛ'),
    ('Общественное пространство Норильск'),
    ('17110_2_TMK'),
    ('Открытие Спартаковская')
ON CONFLICT (project_name) DO NOTHING;

-- Создание представления для отчета
CREATE OR REPLACE VIEW EmployeeReport AS
SELECT
    e.full_name AS "ФИО сотрудника",
    e.salary AS "Оклад",
    p.position_title AS "Должность",
    dt.type_name AS "Тип подразделения",
    d.department_name AS "Структурное подразделение",
    e.hire_date AS "Дата найма",
    b.address AS "Адрес филиала",
    COALESCE(
        (SELECT STRING_AGG(pr.project_name, ', ' ORDER BY pr.project_name)
         FROM EmployeeProjects ep
         JOIN Projects pr ON ep.project_id = pr.project_id
         WHERE ep.employee_id = e.employee_id),
        'Нет проектов'
    ) AS "Проект на который назначен"
FROM Employees e
JOIN Positions p ON e.position_id = p.position_id
JOIN Departments d ON e.department_id = d.department_id
JOIN DepartmentTypes dt ON d.type_id = dt.type_id
JOIN Branches b ON e.branch_id = b.branch_id
ORDER BY e.employee_id;

-- Функция для массовой загрузки данных сотрудников
CREATE OR REPLACE FUNCTION load_employee_data()
RETURNS VOID AS $$
BEGIN
    INSERT INTO Employees (full_name, salary, position_id, department_id, branch_id, hire_date)
    VALUES
        ('Суханова Арина Руслановна', 103333.00, (SELECT position_id FROM Positions WHERE position_title = 'ведущий QA инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Центр компетенций QA Москва'), (SELECT branch_id FROM Branches WHERE address = 'Приморский край, г. Владивосток, ул Нижнепортовая, д. 1'), '2001-01-13'),
        ('Баранов Георгий Александрович', 12130.00, (SELECT position_id FROM Positions WHERE position_title = 'специалист'), (SELECT department_id FROM Departments WHERE department_name = 'Группа сервисной поддержки'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2012-03-17'),
        ('Вишневская Виктория Матвеевна', 12130.00, (SELECT position_id FROM Positions WHERE position_title = 'специалист по персоналу'), (SELECT department_id FROM Departments WHERE department_name = 'Группа сервисной поддержки'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2017-11-17'),
        ('Алексеев Константин Николаевич', 12366.00, (SELECT position_id FROM Positions WHERE position_title = 'специалист по персоналу'), (SELECT department_id FROM Departments WHERE department_name = 'Группа сервисной поддержки'), (SELECT branch_id FROM Branches WHERE address = 'Ростовская обл, г. Ростов-на-Дону, ул 2-я Краснодарская, д. 135/2'), '2020-06-20'),
        ('Лаптев Владислав Даниилович', 71000.00, (SELECT position_id FROM Positions WHERE position_title = 'ведущий разработчик'), (SELECT department_id FROM Departments WHERE department_name = 'Центр разработки продуктов для digital-маркетинга'), (SELECT branch_id FROM Branches WHERE address = 'Ростовская обл, г. Ростов-на-Дону, ул 2-я Краснодарская, д. 135/2'), '2022-06-16'),
        ('Коновалов Даниил Матвеевич', 62000.00, (SELECT position_id FROM Positions WHERE position_title = 'ведущий разработчик'), (SELECT department_id FROM Departments WHERE department_name = 'Центр разработки продуктов для digital-маркетинга'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2016-11-13'),
        ('Фролов Тимур Тимофеевич', 55000.00, (SELECT position_id FROM Positions WHERE position_title = 'ведущий инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Центр разработки продуктов для digital-маркетинга'), (SELECT branch_id FROM Branches WHERE address = 'Ростовская обл, г. Ростов-на-Дону, ул 2-я Краснодарская, д. 135/2'), '2022-03-17'),
        ('Левина Елизавета Артёмовна', 33000.00, (SELECT position_id FROM Positions WHERE position_title = 'руководель проектов по интеграции'), (SELECT department_id FROM Departments WHERE department_name = 'Департамент Техническая поддержка'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2010-03-13'),
        ('Сафонов Леонид Максимович', 60615.00, (SELECT position_id FROM Positions WHERE position_title = 'ведущий инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа CRM 2'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2023-11-13'),
        ('Журавлев Денис Георгиевич', 33300.00, (SELECT position_id FROM Positions WHERE position_title = 'старший инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа CRM 2'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2016-03-16'),
        ('Ильина Злата Игоревна', 33250.00, (SELECT position_id FROM Positions WHERE position_title = 'старший инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа CRM 2'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2013-05-16'),
        ('Дементьев Лев Маркович', 31000.00, (SELECT position_id FROM Positions WHERE position_title = 'инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа первичной диагностики №2'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2023-03-13'),
        ('Шилов Глеб Николаевич', 32000.00, (SELECT position_id FROM Positions WHERE position_title = 'старший инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа Billing'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2011-01-17'),
        ('Еремеев Денис Степанович', 60300.00, (SELECT position_id FROM Positions WHERE position_title = 'ведущий инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа DOC'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2023-10-17'),
        ('Тарасова Анастасия Даниловна', 33752.00, (SELECT position_id FROM Positions WHERE position_title = 'старший инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа ODS'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2011-02-15'),
        ('Высоцкая Ольга Константиновна', 55000.00, (SELECT position_id FROM Positions WHERE position_title = 'инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа Rating'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2016-03-17'),
        ('Богданова Софья Никитична', 56000.00, (SELECT position_id FROM Positions WHERE position_title = 'старший инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа Rating'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2011-01-17'),
        ('Новиков Марк Евгеньевич', 65600.00, (SELECT position_id FROM Positions WHERE position_title = 'руководель сервисных проектов'), (SELECT department_id FROM Departments WHERE department_name = 'Центр управления сервисами'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2010-07-13'),
        ('Гусева Екатерина Марковна', 60000.00, (SELECT position_id FROM Positions WHERE position_title = 'разработчик'), (SELECT department_id FROM Departments WHERE department_name = 'Департамент FBF'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2016-07-17'),
        ('Воробьев Герман Ильич', 136000.00, (SELECT position_id FROM Positions WHERE position_title = 'старший разработчик'), (SELECT department_id FROM Departments WHERE department_name = 'Департамент FBF'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2013-07-20'),
        ('Григорьева Вера Константиновна', 135200.00, (SELECT position_id FROM Positions WHERE position_title = 'ведущий архектор'), (SELECT department_id FROM Departments WHERE department_name = 'Центр анализа и архектуры Medio'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2010-03-12'),
        ('Осипов Андрей Алексеевич', 116600.00, (SELECT position_id FROM Positions WHERE position_title = 'старший архектор'), (SELECT department_id FROM Departments WHERE department_name = 'Центр анализа и архектуры Medio'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2023-03-12'),
        ('Назарова Мария Альбертовна', 151600.00, (SELECT position_id FROM Positions WHERE position_title = 'ведущий разработчик'), (SELECT department_id FROM Departments WHERE department_name = 'Центр разработки Medio'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2023-05-17'),
        ('Лаптева Анна Максимовна', 33000.00, (SELECT position_id FROM Positions WHERE position_title = 'разработчик'), (SELECT department_id FROM Departments WHERE department_name = 'Центр разработки Medio'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2025-01-17'),
        ('Ситникова Эмилия Николаевна', 132132.00, (SELECT position_id FROM Positions WHERE position_title = 'старший разработчик'), (SELECT department_id FROM Departments WHERE department_name = 'Центр разработки Medio'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2010-03-13'),
        ('Кузнецова Любовь Даниэльевна', 75300.00, (SELECT position_id FROM Positions WHERE position_title = 'старший разработчик'), (SELECT department_id FROM Departments WHERE department_name = 'Центр разработки Medio'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2017-07-13'),
        ('Трофимов Вячеслав Романович', 67100.00, (SELECT position_id FROM Positions WHERE position_title = 'ведущий инженер'), (SELECT department_id FROM Departments WHERE department_name = 'Группа инфраструктуры'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2016-07-13'),
        ('Пантелеева Есения Серафимовна', 76550.00, (SELECT position_id FROM Positions WHERE position_title = 'руководель проектов'), (SELECT department_id FROM Departments WHERE department_name = 'Департамент Rating and Charging'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2013-03-17'),
        ('Зуев Георгий Ильич', 135000.00, (SELECT position_id FROM Positions WHERE position_title = 'руководель направления разработки'), (SELECT department_id FROM Departments WHERE department_name = 'Департамент Rating and Charging'), (SELECT branch_id FROM Branches WHERE address = 'Краснодарский край, г. Краснодар, ул Путевая, д. 1'), '2011-11-12');
END;
$$ LANGUAGE plpgsql;

SELECT load_employee_data();

SELECT * FROM EmployeeReport;

Диаграмма в DBeaver:

![Диаграмма в DBeaver](https://github.com/nikolaykonkin/database-design-hw/raw/main/dbeaver_diagram.png)