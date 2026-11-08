Підтвердження джерела: Прочитано файл SD_Spec.docx (повна специфікація Mini Issue Tracker). Доступні всі розділи: §0 Customer Request, §1 Discovery / Problem Analysis, §2 Product Vision & Initial Scope, §3 Stakeholder Needs & Business Rules, §4 Prioritization & Candidate MVP, §5 Detailed Functional Requirements (US-01..US-09), §6 Quality Attributes & Security (NFR), §7 Feasibility, Estimation & Risk Assessment, §8 MVP / Release Scope, §9 UX/UI & Interaction Design.

1. Product Purpose, Actors та межі релізу
Product Purpose: Внутрішня вебсистема (web application) для невеликої команди розробників, призначена для централізованої реєстрації, відстеження та координації вирішення проблем програмного продукту з метою скорочення часу від реєстрації проблеми до її призначення та вирішення (Source: §1 Problem Statement, Product Business Goal; §2 Product Vision; KC-01, KC-03 | Тип: SOURCE).

Primary Users (Actors):

Team Member (у системі 2 попередньо задані користувачі: Member A, Member B) (Source: §1 Primary users, Open Questions; §3 BR-01 | Тип: SOURCE).

Team Lead (у системі 1 попередньо заданий користувач: Lead) (Source: §1 Primary users, Open Questions; §3 BR-01 | Тип: SOURCE).

Secondary Stakeholders (не беруть участі в основних сценаріях MVP): Product Owner / замовник, System Administrator (Source: §1 Stakeholders | Тип: SOURCE).

2. Межі Release Scope (за остаточним §8, а не Candidate MVP §4)
Included in Release MVP: US-01, US-02, US-03, US-04, US-05, US-06, US-07, US-08 (Source: §8 Release MVP | Тип: SOURCE).

Deferred (відкладено за межі першого релізу через високу технічну складність R-03): US-09 (Attach files to an issue / SR-09) (Source: §7 Recommended scope adjustment, §8 Deferred | Тип: SOURCE).

Future (майбутні релізи): SR-10 (Automatic issue categorization), SR-11 (Automatic priority recommendation) (Source: §4, §8 Future, KC-06, BR-12 | Тип: SOURCE).

Explicitly Out of Scope: Зовнішні клієнти (KC-04), інтеграція з GitHub/GitLab/Jira (KC-05), мобільний застосунок (A-07, §2), видалення заявок (BR-10, §1 Open Questions), користувацька історія змін (BR-11, §1 Open Questions), Customer support portal, Billing, Source-code hosting, CI/CD, Time tracking, Full project management (§2 Out of Scope) (Тип: SOURCE).

3. Включені User Stories та Acceptance Criteria (8 US, 33 змістові AC)
US-01: Create Issue (Actor: Team Member / будь-який автентифікований член команди, Source: §5 US-01, §3 BR-02 | Тип: SOURCE)

Мета: Створити заявку з назвою та описом, щоб проблема була зафіксована і відстежувалася.

AC-01.1: Користувач може ввести Title та Description.

AC-01.2: Title є обов'язковим (у Gherkin-прикладі для AC-02 задано повідомлення: "Title is required").

AC-01.3: Description є обов'язковим.

AC-01.4: Після створення заявка отримує: ID, status = Open, created_at, author (поточний автентифікований користувач згідно з Gherkin AC-01).

AC-01.5: Після створення issue його priority не встановлено (Not Set / пусте значення за BR-04, BR-05).

US-02: View Issues (Actor: Team Member / автентифікований член команди, Source: §5 US-02 | Тип: SOURCE)

Мета: Переглядати зареєстровані issues та інспектувати їхні поточні деталі.

AC-02.1: Авторизований член команди може переглянути список зареєстрованих issues.

AC-02.2: Для кожного issue у списку відображаються щонайменше: ID, Title, Status, Priority, Assignee (якщо він призначений).

AC-02.3: Користувач може вибрати issue зі списку та відкрити сторінку його деталей.

AC-02.4: На сторінці issue відображаються: ID, Title, Description, Status, Priority, Assignee (якщо він призначений), Author, Created at.

AC-02.5: Якщо зареєстрованих issues немає, система відображає порожній список або повідомлення про відсутність issues.

US-03: Assign Issue to the responsible team member (Actor: Team Lead, Source: §5 US-03 | Тип: SOURCE)

Мета: Призначити заявку члену команди, щоб було зрозуміло, хто відповідає за її виконання.

US-03, AC, пункт 1: Можна вибрати одного assignee (підкріплено A-04).

US-03, AC, пункт 2: Assignee повинен бути членом команди (KC-04, BR-01).

US-03, AC, пункт 3: Призначати заявку може лише Team Lead (§1 Open Questions).

US-03, AC, пункт 4: Поточний assignee відображається у заявці.

US-04: Set Issue Priority (Actor: Team Lead, Source: §5 US-04 | Тип: SOURCE)

Мета: Встановлювати пріоритет заявки для визначення порядку роботи.

AC-04.1: Допустимі значення встановленого Priority: Low, Normal, High, Critical.

AC-04.2: Встановлювати та змінювати priority може лише Team Lead (BR-06).

US-05: Track Issue Progress (Actor: Team Member, Source: §5 US-05 | Тип: SOURCE)

Мета: Бачити поточний статус заявки для розуміння прогресу роботи.

AC-05.1: Поточний status відображається для кожного issue.

AC-05.2: Status відображається на сторінці issue.

AC-05.3: Status відображається у списку issues.

US-06: Add Comment (Actor: Член команди, Source: §5 US-06 | Тип: SOURCE)

Мета: Додавати коментарі до заявки для обговорення проблеми.

AC-06.1: Текст коментаря не може бути порожнім (Comment text cannot be empty).

AC-06.2: Доданий коментар зберігається і відображається для цієї заявки.

AC-06.3: Автор коментаря (Comment author) зберігається і відображається.

AC-06.4: Дата/час створення коментаря (Creation date/time) зберігаються і відображаються.

AC-06.5: Помилка збереження (Failed save) не створює коментар і виводить повідомлення про помилку.

US-07: Update Issue Status (Actor: Член команди, відповідальний за заявку / current Assignee, Source: §5 US-07 | Тип: SOURCE)

Мета: Змінювати статус заявки для відображення поточного стану роботи.

AC-07.1: Нова заявка створюється зі статусом Open (BR-03).

AC-07.2: Допустимі статуси: Open, In Progress, Resolved, Closed (BR-07).

AC-07.3: Після зміни статусу система зберігає нове значення та відображає його при наступному перегляді заявки.

AC-07.4: Користувач, який не є виконавцем заявки (non-assignee), не може змінити її статус (навіть якщо це Team Lead — DERIVED з AC-07.4 + BR-08).

AC-07.5: Система не дозволяє встановити значення статусу поза визначеним переліком.

US-08: View Important Open Issues (Actor: Член команди, Source: §5 US-08 | Тип: SOURCE)

Мета: Швидко бачити важливі відкриті (незакриті) заявки.

AC-08.1: Заявка з priority = High і status != Closed відображається у списку Important Issues.

AC-08.2: Заявка з priority = Critical і status != Closed відображається у списку Important Issues.

AC-08.3: Заявки з priority = Low або Normal не відображаються у списку Important Issues (заявки з Not Set також не входять, бо не відповідають критерію High/Critical за BR-09 — DERIVED).

AC-08.4: Заявки зі status = Closed не відображаються незалежно від priority.

4. UI-релевантні Business Rules (BR)
BR-01: Лише автентифіковані члени команди (2 Team Members, 1 Team Lead).

BR-02: Створювати issue може будь-який автентифікований член команди.

BR-03 + AC-01.4 + AC-07.1: Початковий статус при створенні = Open.

BR-04 + BR-05: При створенні пріоритет не визначений (Not Set / пусте значення). Після встановлення — тільки Low, Normal, High, Critical.

BR-06 + AC-04.2: Встановлювати та змінювати пріоритет може виключно Team Lead.

BR-07 + BR-08 + AC-07.4: Змінювати статус може виключно поточний Assignee заявки.

BR-08.1: Дозволені лише 4 переходи статусу: Open -> In Progress, In Progress -> Resolved, Resolved -> Closed, Resolved -> In Progress. Зі стану Closed вихідних переходів немає.

BR-09 + BR-09.1: Важливі заявки (Important Issues) мають priority IN {High, Critical} AND status != Closed. Заявка у статусі Resolved вважається вирішеною, але ще не закритою, тому залишається в Important Issues.

BR-10: Заявки не видаляються.

BR-11: Історія змін (Change history) у першому релізі не передбачена.

5. Non-Functional Requirements (NFR) з §8
NFR-02: Authentication required — у макеті позначається межею входу (S-01), без проєктування форми пароля/SSO (через ризик R-01).

Вимоги поза перевіркою статичним макетом (перевіряються лише в реалізації/інфраструктурі):

NFR-01: Response time < 1 s.

NFR-03: HTTPS only.

NFR-04: Backup of all data (примітка: у §8 слово + attachments прибрано відносно §6, оскільки US-09 відкладено).

6. Відкриті питання та неоднозначності (Q-ID для вирішення людиною)
Q-01 (Source: §7 R-01, NFR-02): Спосіб реалізації автентифікації потребує уточнення. Як показувати вхід у макеті?

Q-02 (Source: §5 US-01, US-03): Чи має нова заявка призначеного виконавця одразу при створенні та чи може Team Lead перепризначати Assignee пізніше?

Q-03 (Source: §3 BR-04, BR-05): Як візуально подати відсутній пріоритет (Not Set) і чи можна скинути вже встановлений пріоритет назад у Not Set?

Q-04 (Source: §5 US-03, US-04, US-07): Як показувати недоступні за роллю контролі (приховувати чи відображати disabled) та як підтверджувати їх зміну?

7. Source Check
Перевірка кількості: Включено US-01..US-08 (8 історій). Загальна кількість змістових AC = 5 (US-01) + 5 (US-02) + 4 (US-03, ненумеровані) + 2 (US-04) + 3 (US-05) + 5 (US-06) + 5 (US-07) + 4 (US-08) = 33 AC. Жодного критерію не втрачено. Gherkin-сценарії в US-01 враховані як деталізація AC-01.1..01.4.

Перелік виключених вимог із причиною:

US-09 (AC-09.1–AC-09.6, SR-09 Attachments) - перенесено в Deferred через складність зберігання, лімітів 10 МБ та антивірусного сканування (R-03).

SR-10, SR-11 (C-06, BR-12 AI-рекомендації) - перенесено у Future.