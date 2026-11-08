1. Матриця прав доступу (Permission Matrix — 4 контексти ролей)
Assignee — це відношення користувача до конкретної заявки, а не глобальна роль у системі.
Дія (Action),Team Member (non-assignee),Team Member (assignee),Team Lead (non-assignee),Team Lead (assignee),Джерело (Source / D-ID)
View issues & details,Так (Enabled),Так (Enabled),Так (Enabled),Так (Enabled),"BR-01, US-02, US-05, US-08"
Create issue,Так (Enabled),Так (Enabled),Так (Enabled),Так (Enabled),"BR-02, US-01"
Add comment,Так (Enabled),Так (Enabled),Так (Enabled),Так (Enabled),US-06 (AC-06.1..06.5)
Assign / Reassign issue,Ні (Disabled),Ні (Disabled),Так (Enabled),Так (Enabled),"US-03 (пункт 3), D-04, D-05"
Set / Change priority,Ні (Disabled),Ні (Disabled),Так (Enabled),Так (Enabled),"BR-06, AC-04.2, D-05"
Change status,Ні (Disabled),Так (за BR-08.1),Ні (Disabled),Так (за BR-08.1),"BR-08, BR-08.1, AC-07.4, D-05"

2. Матриця переходів статусів (Status Transitions Matrix за BR-08.1)
Поточний статус \ Цільовий статус,Open,In Progress,Resolved,Closed,Доступні опції у UI
Open,Поточний (не перехід),Дозволено,Заборонено,Заборонено,Лише In Progress
In Progress,Заборонено,Поточний (не перехід),Дозволено,Заборонено,Лише Resolved
Resolved,Заборонено,Дозволено (повернення в роботу),Поточний (не перехід),Дозволено,"In Progress, Closed"
Closed,Заборонено,Заборонено,Заборонено,Поточний (не перехід),Немає вихідних переходів (контрол Disabled)

3. Точний предикат Important Issues
Приклад 1 (ISS-003: Priority = High, Status = Resolved): Входить до Important Issues (True), оскільки Resolved != Closed (Source: BR-09.1, AC-08.1).

Приклад 2 (ISS-004: Priority = Critical, Status = Closed): НЕ входить до Important Issues (False), оскільки Status == Closed виключає заявку незалежно від пріоритету (Source: AC-08.4).

Приклад 3 (ISS-006: Priority = Not Set, Status = Open): НЕ входить до Important Issues (False), оскільки відсутній пріоритет не дорівнює High або Critical (Source: BR-04, BR-09, AC-08.1..08.3).

4. Контракти контролів екранів (Controls Specification)
Екран S-02 (Стани S-02-A, S-02-B, S-02-C):

C-02.1: Кнопка Create Issue (Button, завжди enabled для автентифікованих, Trigger: Click -> відкриває M-01, Source: US-01, D-01).

C-02.2: Вкладки All Issues / Important Issues (Tabs, enabled, перемикають стани S-02-A та S-02-B, Source: US-02, US-08, D-01).

C-02.3: Посилання Issue Title у рядку таблиці (Link, enabled, Trigger: Click -> відкриває S-03 відповідної заявки, Source: AC-02.3).

Модальне вікно M-01 (Create Issue):

C-01.1: Поле Title (Text Input, обов'язкове; при порожньому значенні показує "Title is required", Source: AC-01.1, AC-01.2).

C-01.2: Поле Description (Multiline Textarea, обов'язкове; при порожньому значенні показує "Description is required", Source: AC-01.1, AC-01.3).

C-01.3: Кнопка Create (Primary Button, створює заявку зі значеннями за замовчуванням: авто-ID, Status = Open, Priority = Not Set, Assignee = Unassigned, Author = current user, Created at = current timestamp, Source: AC-01.4, AC-01.5, D-03, D-04).

C-01.4: Кнопка Cancel (Secondary Button, закриває M-01 без збереження, Source: D-01, D-07).

Екран S-03 (Issue Details):

C-03.1: Кнопка Return to Issues List (Link/Button, повертає на попередній стан S-02-A або S-02-B, Source: D-07).

C-03.2: Контрол Assignee Select + кнопка Apply (Dropdown з переліком членів команди: Member A, Member B, Lead + Button; enabled лише коли current_user.role == Team Lead, інакше disabled, Source: US-03, D-04, D-05, D-06).

C-03.3: Контрол Priority Select + кнопка Apply (Dropdown зі значеннями Low, Normal, High, Critical + Button; якщо не встановлено — показує placeholder/badge Not Set; enabled лише для Team Lead, інакше disabled, Source: US-04, BR-04..06, D-03, D-05, D-06).

C-03.4: Контрол Status Select + кнопка Apply (Dropdown лише з дозволеними за BR-08.1 наступними статусами + Button; enabled лише коли current_user == issue.assignee AND issue.status != Closed, інакше disabled, Source: US-07, BR-08, BR-08.1, AC-07.4, D-05, D-06).

C-03.5: Поле Comment Input + кнопка Add Comment (Textarea + Button, enabled для всіх автентифікованих користувачів навіть у статусі Closed; валідує непорожній текст за AC-06.1 та обробляє помилку збереження за AC-06.5, Source: US-06).

5. Матриця простежуваності (Traceability Matrix — Усі 33 AC + BR + NFR)
Source ID / Пункт,UIR-ID,Screen / State,Control ID або Annotation,Test ID,Verification Level (Макет / Прототип / Реалізація)
AC-01.1,UIR-01,M-01,"C-01.1 (Title), C-01.2 (Description)",T-01,Макет (наявність полів) + Прототип
AC-01.2,UIR-02,M-01 (Validation state),"C-01.1 + текст ""Title is required""",T-02,Макет (state example) / Реалізація (блокування відправки)
AC-01.3,UIR-03,M-01 (Validation state),"C-01.2 + текст ""Description is required""",T-02,Макет (state example) / Реалізація
AC-01.4,UIR-04,S-03 ( після Create),"Поля Read-only: ID, Status=Open, Created at, Author",T-01,Макет + Прототип (підготовлений результат; генерація БД — у реалізації)
AC-01.5 + BR-04,UIR-05,"S-03, S-02-A",Відображення Priority: Not Set (на прикладі нової заявки та ISS-006),"T-01, T-04",Макет (візуальний стан відсутності пріоритету)
AC-02.1,UIR-06,S-02-A,Таблиця всіх заявок All Issues,T-09,Макет + Прототип
AC-02.2,UIR-07,"S-02-A, S-02-B","Колонки таблиці: ID, Title, Status, Priority, Assignee","T-08, T-09",Макет (читабельність усіх 5 колонок)
AC-02.3,UIR-08,"S-02-A, S-02-B -> S-03",C-02.3 (Клік на Title відкриває S-03),T-09,Прототип (hotspot connection)
AC-02.4,UIR-09,S-03,"Відображення: ID, Title, Description, Status, Priority, Assignee, Author, Created at","T-01, T-09",Макет (наявність усіх 8 атрибутів на S-03)
AC-02.5,UIR-10,S-02-C (Empty state),Повідомлення про відсутність зареєстрованих issues,T-09,Макет (окремий стан фрейму S-02-C)
"US-03, AC, пункт 1",UIR-11,S-03,C-03.2 (Single-select Dropdown для одного Assignee),T-03,Макет (тип контролу single-select)
"US-03, AC, пункт 2",UIR-12,"S-03, REF-01","C-03.2 (Опції списку: лише Member A, Member B, Lead)",T-03,Макет / Реалізація (перевірка списку користувачів)
"US-03, AC, пункт 3",UIR-13,"S-03, REF-01",C-03.2 (Enabled для Lead; Disabled для Member),T-03,Макет (UI-стан) / Реалізація (серверна авторизація)
"US-03, AC, пункт 4",UIR-14,"S-02-A/B, S-03",Поле Assignee у деталях і колонці списку,T-03,Макет
AC-04.1 + BR-05,UIR-15,"S-03, REF-01","C-03.3 (Опції вибору: лише Low, Normal, High, Critical)",T-04,Макет (перелік опцій без Not Set як опції вибору)
AC-04.2 + BR-06,UIR-16,"S-03, REF-01",C-03.3 (Enabled для Lead; Disabled для Member),T-04,Макет (UI-стан) / Реалізація (серверна перевірка)
AC-05.1,UIR-17,"S-02-A/B, S-03",Бейдж/значення Status для кожної заявки,T-05,Макет
AC-05.2,UIR-18,S-03,Відображення поточного Status у деталях заявки,T-05,Макет
AC-05.3,UIR-19,"S-02-A, S-02-B",Колонка Status у списку заявок,T-05,Макет
AC-06.1,UIR-20,"S-03, REF-01",C-03.5 (Приклад помилки валідації порожнього коментаря),T-07,Макет (state example) / Реалізація
AC-06.2,UIR-21,S-03,Блок збережених коментарів під заявкою,T-07,Макет (відображення) / Реалізація (persistence у БД)
AC-06.3,UIR-22,S-03,Відображення Author у кожному збереженому коментарі,T-07,Макет
AC-06.4,UIR-23,S-03,Відображення Created at (дата/час) у кожному коментарі,T-07,Макет
AC-06.5,UIR-24,"S-03, REF-01",C-03.5 (Стан помилки збереження Save error без додавання запису),T-07,Макет (візуальний приклад) / Реалізація (транзакційність)
AC-07.1 + BR-03,UIR-25,S-03 (нова заявка),Початковий Status = Open після створення,T-01,Макет + Прототип
AC-07.2 + BR-07,UIR-26,"S-02, S-03, REF-01","Лише 4 статуси: Open, In Progress, Resolved, Closed",T-05,Макет (усі 4 стани представлені в REF-01 та списку)
AC-07.3,UIR-27,S-03,C-03.4 (Оновлений статус залишається видимим на S-03),T-05,Макет / Реалізація (збереження в БД)
AC-07.4 + BR-08,UIR-28,"S-03, REF-01","C-03.4 (Disabled для всіх non-assignee, включно з Team Lead на ISS-003)",T-06,Макет (канонічний приклад на S-03 та матриця в REF-01)
AC-07.5 + BR-08.1,UIR-29,"S-03, REF-01",C-03.4 (Дозволені лише 4 переходи; у Closed переходи відсутні),T-05,Макет (REF-01 state examples)
AC-08.1 + BR-09.1,UIR-30,S-02-B,Рядки ISS-002 (High/In Progress) та ISS-003 (High/Resolved) присутні,T-08,Макет (перевірка рядків таблиці S-02-B)
AC-08.2 + BR-09.1,UIR-31,S-02-B,Рядки ISS-001 (Critical/Open) та ISS-007 (Critical/Resolved) присутні,T-08,Макет (перевірка рядків таблиці S-02-B)
AC-08.3,UIR-32,S-02-B,"Рядки ISS-005 (Normal), ISS-006 (Not Set), ISS-008 (Low) відсутні в S-02-B",T-08,Макет (порівняння S-02-A та S-02-B)
AC-08.4,UIR-33,S-02-B,Рядок ISS-004 (Critical/Closed) відсутній у S-02-B,T-08,Макет (порівняння S-02-A та S-02-B)
NFR-02 + BR-01,UIR-34,S-01,"Заглушка межі автентифікації ""Authentication entry mechanism TBD""",T-10,Макет (наявність S-01) / Реалізація (справжній вхід)
"NFR-01, 03, 04",UIR-35,Поза UI (Примітка),"Не мають окремих екранів (<1s, HTTPS, Backup)",—,NOT VERIFIABLE у макеті; перевіряється при деплої/навантаженні

6. Специфікація тестів (T-01..T-10)
T-01 (Create): Актор: Member A. Precondition: Відкрито M-01. Дія: Ввести Title і Description, натиснути Create. Очікуваний результат: Перехід на S-03 нової заявки (Status = Open, Priority = Not Set, Assignee = Unassigned, видно ID, Author, Created at). Межа доказу: Persistence у БД макетом не доводиться.

T-02 (Validation): Актор: Member A. Precondition: Відкрито M-01. Дія: Залишити Title порожнім, натиснути Create. Очікуваний результат: Заявка не створюється, видно помилку "Title is required".

T-03 (Assignment): Актор: Team Lead vs Member A на S-03. Очікуваний результат: Для Lead контрол C-03.2 активний (вибір одного учасника команди), для Member — disabled. Межа доказу: Серверна авторизація перевіряється в коді.

T-04 (Priority): Актор: Team Lead vs Member A на S-03. Очікуваний результат: Для Lead доступні опції Low, Normal, High, Critical (без опції скидання в Not Set); для Member контрол disabled.

T-05 (Status Transitions): Актор: Поточний Assignee. Очікуваний результат: У REF-01 та S-03 доступні лише 4 переходи (Open->In Progress, In Progress->Resolved, Resolved->In Progress, Resolved->Closed). У стані Closed контрол disabled.

T-06 (Lead Status Restriction): Актор: Team Lead переглядає ISS-003 (де Assignee = Member A). Очікуваний результат: Assignee та Priority enabled, але Status disabled (за AC-07.4).

T-07 (Comments): Актор: Будь-який. Перевірка 3 станів: 1) порожній текст -> помилка валідації; 2) успіх -> коментар з текстом, автором і часом у списку; 3) збій збереження -> повідомлення про помилку, коментар НЕ додано до списку (AC-06.5).

T-08 (Important Filter): Актор: Будь-який. Дія: Перемкнутися з S-02-A на S-02-B. Очікуваний результат: З 8 тестових заявок видно рівно 4 (ISS-001, ISS-002, ISS-003, ISS-007).

T-09 (Navigation & Empty State): Перевірка переходів між S-02-A <-> S-02-B, відкриття M-01 та закриття через Cancel, переходу в S-03 та повернення назад, а також наявності порожнього стану S-02-C.

T-10 (Scope Compliance): Огляд усіх фреймів на відсутність Attachments (US-09), AI recommendations (SR-10/11), кнопки видалення (BR-10), історії змін (BR-11) та чекбоксів у таблиці.