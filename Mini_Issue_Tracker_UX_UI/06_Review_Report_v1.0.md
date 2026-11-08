На наданих скріншотах фінального макета Wireframes_v2 чітко видно та читаються ідентифікатори фреймів і станів:
S-01 — Authentication Boundary, S-02-A — All Issues, S-02-B — Important Issues, S-02-C — Empty List State, M-01 — Create Issue Modal, S-03 — Issue Details та REF-01 — Reviewer State Reference. Усі текстові мітки, заголовки колонок, значення полів заявок ISS-001..ISS-008, підписи ролей і станів у REF-01 та майстер-компоненти читаються без спотворень. Жоден екран не реконструйовано з попереднього промпту.

Частина А — Перевірка вимог (Included AC, UI-релевантні BR та D-рішення)
Source,Expected,Screen/State,Observed evidence,Result,Remaining test
AC-01.1,Користувач може ввести Title та Description,M-01,"На фреймі M-01 видно однорядкове поле Title та багаторядкове поле Description, кнопки Cancel та Create.",PASS,Перевірка відправки форми у вебзастосунку
AC-01.2,"Title є обов'язковим (""Title is required"")",REF-01 (Create Issue validation),"У картці Missing Title під порожнім полем Title видно повідомлення ""Title is required"".",PASS,Блокування сабміту при порожньому Title
AC-01.3,Description є обов'язковим,REF-01 (Create Issue validation),"У картці Missing Description під порожнім полем Description видно підпис ""Description is required"".",PASS,Блокування сабміту при порожньому Description
AC-01.4 + BR-03,"Після створення заявка отримує ID, status = Open, created_at, author","M-01, S-03, REF-01","У M-01 ці поля не є інпутами; на S-03 відображаються ISS-003, Author: Member A, Created at: 2026-10-18 09:40 та статус; у REF-01 зафіксовано автогенерацію.",PASS,"Автогенерація ID, created_at, author та збереження в БД"
AC-01.5 + BR-04,Після створення issue його priority не встановлено (Not Set),"S-02-A (ISS-006), REF-01",У рядку ISS-006 на S-02-A у колонці Priority відображено Not Set; у M-01 вибір пріоритету відсутній.,PASS,Запис пустого значення пріоритету в БД
AC-02.1,Авторизований член команди може переглянути список зареєстрованих issues,S-02-A,На фреймі S-02-A відображено таблицю Issues List з усіма 8 заявками (ISS-001..ISS-008).,PASS,Інтеграційний тест отримання списку з API
AC-02.2,"Для кожного issue у списку видно: ID, Title, Status, Priority, Assignee","S-02-A, S-02-B","Таблиця містить 5 колонок (ID, Title, Status, Priority, Assignee); для непризначених ISS-005, ISS-006 вказано Unassigned.",PASS,Рендеринг полів БД у таблиці
AC-02.3,Користувач може вибрати issue зі списку та відкрити сторінку деталей,S-02-A/B -> S-03,Видно рядок ISS-003 у списку та відповідний екран деталей S-03. Перехід кліком у прототипі враховано як повідомлення студента.,PARTIAL,Перехід роутингу за ID у коді
AC-02.4,"На сторінці деталей видно: ID, Title, Description, Status, Priority, Assignee, Author, Created at",S-03,"Ліворуч видно ISS-003, назву, повний текст Description, Author: Member A, Created at: 2026-10-18 09:40; праворуч — поточні Assignee (Member A), Priority (High), Status (Resolved).",PASS,Завантаження даних заявки з БД
AC-02.5,Якщо зареєстрованих issues немає — порожній список або повідомлення,S-02-C,"Окремий фрейм S-02-C показує шапку таблиці та повідомлення ""No issues registered yet"".",PASS,Обробка порожньої вибірки з БД
"US-03, AC, п.1",Можна вибрати одного assignee,"S-03, REF-01",Контрол Assignee є single-select випадаючим списком.,PASS,Обмеження 1 виконавця в БД
"US-03, AC, п.2",Assignee повинен бути членом команди,REF-01,У секції Assignee & priority options зазначено вибір виключно з Member A / Member B / Lead.,PASS,Серверна валідація списку команди
"US-03, AC, п.3",Призначати заявку може лише Team Lead,"S-03, REF-01 (Permissions)",У картках Team Lead (та на S-03) дропдаун Assignee і кнопка Apply активні (enabled); у картках Team Member — неактивні (disabled).,PASS,Серверна авторизація ролі Team Lead
"US-03, AC, п.4",Поточний assignee відображається у заявці,"S-02-A/B, S-03",У таблиці S-02 та у селекторі Assignee на S-03 видно поточного виконавця (Member A).,PASS,Збереження та вивід assignee з БД
AC-04.1 + BR-05,"Допустимі значення встановленого Priority: Low, Normal, High, Critical","S-02-A, REF-01",Використано виключно ці 4 рівні (плюс Not Set як індикатор відсутності до встановлення за D-03).,PASS,Валідація enum на бекенді
AC-04.2 + BR-06,Встановлювати та змінювати priority може лише Team Lead,"S-03, REF-01 (Permissions)",Контрол Priority та кнопка Apply активні (enabled) лише для Team Lead і заблоковані (disabled) для Team Member.,PASS,Серверна авторизація ролі Team Lead
AC-05.1..05.3,Поточний status видно для кожного issue у списку та на сторінці деталей,"S-02-A/B, S-03",Статус відображається у колонці Status на S-02-A/B та у блоці контролів на S-03.,PASS,Синхронізація статусу з БД
AC-06.1,Текст коментаря не може бути порожнім,REF-01 (Comments),"Картка Empty validation показує помилку ""Comment text cannot be empty"".",PASS,Валідація непорожнього коментаря
AC-06.2..06.4,"Доданий коментар зберігається і показує текст, автора та дату/час","S-03, REF-01 (Saved)","На S-03 та у картці Saved видно текст коментаря, Author: Member B та Created at: 2026-10-18 11:05.",PASS,Збереження коментаря в БД (persistence)
AC-06.5,Помилка збереження не створює коментар і виводить помилку,REF-01 (Save failure),"Картка Save failure показує банер ""Failed to save comment. Retry."", текст лишається в полі, новий запис до списку не додано.",PASS,Транзакційний відкат при помилці збереження
"AC-07.1..07.3, 07.5 + BR-08.1",Нова заявка Open; 4 статуси; лише 4 дозволені переходи; з Closed переходів немає,REF-01 (Status transitions),"Показано 4 картки: Open -> In Progress, In Progress -> Resolved, Resolved -> In Progress / Closed, Closed -> No transitions (Disabled).",PASS,Перевірка переходів статусів на бекенді і збереження в БД
AC-07.4 + BR-08,Користувач non-assignee не може змінити статус (вкл. Team Lead),"S-03, REF-01 (Permissions)","На S-03 (Lead переглядає ISS-003, де Assignee = Member A) та у картках Non-assignee у REF-01 контрол Status і кнопка Apply мають стан disabled.",PASS,Серверна перевірка поточного виконавця
AC-08.1..08.4 + BR-09.1,"Important Issues: Priority IN {High, Critical} AND Status != Closed (вкл. Resolved)","S-02-B, REF-01","На S-02-B присутні рівно 4 заявки: ISS-001 (Open/Critical), ISS-002 (In Progress/High), ISS-003 (Resolved/High), ISS-007 (Resolved/Critical). ISS-004 (Closed/Critical) та ISS-005, 006, 008 виключені.",PASS,Тест фільтрації списку на бекенді
D-01..D-08,Погоджені UX-рішення та відсутність заборонених функцій,Усі фрейми S-01..REF-01,"Дотримано всі рішення D-01..D-08; немає US-09 attachments, SR-10/11 AI, BR-10 delete, BR-11 history, чекбоксів та page icon placeholders.",PASS,—
"NFR-01, 02, 03, 04","< 1 s response time, Authentication, HTTPS only, Backup",S-01 + примітка в REF-01,"На S-01 позначено межу ""Authentication entry mechanism TBD""; швидкість, HTTPS, бекапи та справжній логін макетом не перевіряються.",NOT VERIFIABLE,Інфраструктурні та навантажувальні тести

Частина В — Доказові проблеми UX
UX-01 (Зрозумілість дій на S-03): На продуктовому фреймі S-03 контрол Status та кнопка Apply відображені у стані disabled (оскільки актор Team Lead переглядає чужу заявку ISS-003), але безпосередньо всередині продуктового фрейму S-03 немає підпису, чому зміна заблокована (пояснення розміщене лише поза фреймом у REF-01).

UX-02 (Ознака клікабельності в S-02-A/B): У таблиці заявок назви в колонці Title візуально оформлені тим самим стилем шрифту, що й текстові колонки ID та Assignee (без підкреслення), що на статичному макеті знижує очевидність переходу в S-03.

UX-03 (Позначення обов'язкових полів у M-01): У модальному вікні M-01 біля підписів обов'язкових полів Title та Description у початковому стані немає маркера *.

UX-04 (Незворотність переходу в Closed): Оскільки зі статусу Closed немає вихідних переходів (BR-08.1), вибір Closed + Apply є фінальною дією без додаткового попередження.

Частина С — Таблиця проблем для рішення людини
DEF-ID / UX-ID,Severity,Location,Source,Evidence,Minimal correction
UX-01,Low,S-03 — Issue Details (блок Status),"AC-07.4, D-05",Поле Status є disabled без підказки причини всередині самого екрана S-03.,"Врахувати при верстці фронтенду: показувати tooltip ""Only current assignee can change status"" при наведенні."
UX-02,Low,"S-02-A, S-02-B (колонка Title)",AC-02.3,Текст Title не має підкреслення посилання на статичному скріншоті.,Врахувати при CSS-верстці: додати cursor: pointer та hover-підкреслення для рядка/назви заявки.
UX-03,Low,M-01 — Create Issue Modal,"AC-01.2, AC-01.3",Відсутній символ * біля лейблів Title та Description.,Додати позначку * до лейблів форми під час верстки компонента.
UX-04,Low,"S-03, REF-01 (Resolved -> Closed)",BR-08.1,Відсутнє попередження про незворотність статусу Closed.,Не змінювати UI.

UI design coverage: COMPLETE.

Prototype evidence: REPORTED.

Implementation tests outstanding: Потребують перевірки у програмній реалізації: збереження даних у БД (persistence), серверна авторизація ролей, транзакційна відмова при помилці коментаря (AC-06.5), автентифікація (NFR-02), час відгуку < 1 s (NFR-01), HTTPS (NFR-03) та резервне копіювання (NFR-04).

Запис Контролю D:

Reviewer: Ярмак Артем Миколайович

Date: 07.11.2026

Зауваження UX-01..UX-03 прийнято як рекомендації для етапу фронтенд-розробки;

UX-04 відхилено, щоб не ускладнювати MVP новими вікнами поза специфікацією.