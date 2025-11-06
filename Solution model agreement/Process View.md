Ниже — компактные, практичные правила для бизнес-аналитиков по описанию бизнес-процессов в ArchiMate® 3.2. Они рассчитаны на использование “как есть” в корпоративной практике и согласованы с официальной спецификацией и рекомендованной литературой.

# 1) Что такое Process View и когда его использовать

**Process View** — это вид (view) архитектуры, где фокус на течении работ (behavior), событиях и данных процесса, а также на ролях/акторах, которые выполняют работу. Он отвечает на вопросы: _кто выполняет что, в какой последовательности, какие данные читаются/записываются и какие сервисы задействованы_.  
В ArchiMate это достигается элементами слоя Business (Process, Function, Event, Role/Actor, Business Object) и связями Triggering/Flow/Access/Assignment/Serving; при необходимости допускаются «соседние» слои (Application/Data) для явного указания ИТ-сервисов и данных. ([opengroup.org](https://www.opengroup.org/sites/default/files/docs/downloads/n221p.pdf?utm_source=chatgpt.com "ArchiMate 3.2 Specification Reference Cards"))

Рекомендуемая детализация:

- **L1 (сквозной процесс)** — 5-12 шагов, только ключевые события/данные/внешние сервисы.
    
- **L2 (подпроцессы)** — раскрытие узлов L1 в подпроцессы, добавляем ключевые Business Objects и Application Services.
    
- **L3 (операционный уровень)** — детальные шаги, точки доступа к данным, бизнес-правила, обработка исключений.
    

# 2) Разрешённые элементы (и альтернативы запретам)

Обязательный базовый набор:

- **Business Process** — основная единица поведения процесса.
    
- **Business Event** — событие, инициирующее/завершающее/ветвящее поведение.
    
- **Business Role** (и при необходимости **Business Actor**) — «кто» выполняет шаги.
    
- **Business Function** — для устойчивых «функциональных областей», если нужно сгруппировать шаги процесса по компетенции.
    
- **Business Object / Contract / Product** — для информации, артефактов или предложения ценности, если это критично для понимания процесса.
    
- **Application Service / Application Component / Data Object** — чтобы показать опору процесса на ИТ-сервисы и данные. ([opengroup.org](https://www.opengroup.org/sites/default/files/docs/downloads/n221p.pdf?utm_source=chatgpt.com "ArchiMate 3.2 Specification Reference Cards"))
    

**Запрет:** _Business Interaction_ **не использовать** (см. §7 — вместо него применяйте Process + Role/Actor и связи Flow/Triggering/Serving). Описание элемента и его предназначение см. в справочниках по ArchiMate, но в этих правилах он исключён из практики моделирования. ([help.bizzdesign.com](https://help.bizzdesign.com/articles/horizzon-help/archimate-business-layer-elements?utm_source=chatgpt.com "ArchiMate Business Layer elements"))

# 3) Связи: что с чем соединяем (и как именно)

Ниже — только то, что реально потребуется на процессных схемах.

## 3.1 Assignment (назначение)

**От:** Business Role/Actor → **к:** Business Process/Function/Event/Service. Показывает, кто выполняет поведение или предоставляет сервис. Применять последовательно на каждом шаге, где важно «владение действием». ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))

## 3.2 Triggering (триггер)

**От:** Event/Process → **к:** Process/Event. Отражает причинно-временную связь (что запускает/завершает что). Использовать для «открывающих»/«закрывающих» событий, эскалаций, таймеров. ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))

## 3.3 Flow (поток)

**От:** Process/Function/Event → **к:** Process/Function/Event. Передача «чего-то» между шагами (сообщение, работа, материальный объект), но **не** данных (для данных — Access). Используйте для «эстафеты» между шагами и команд. ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))

## 3.4 Serving (предоставление)

**От:** провайдер (например, Application Service или Business Service) → **к:** потребителю (Process/Role и т.п.). Показывает, какой сервис поддерживает процесс/шаг. На L1/L2 достаточно указывать ключевые сервисы. ([sparxsystems.com](https://sparxsystems.com/resources/user-guides/16.0/large-print/model-domains/languages/archimate.pdf?utm_source=chatgpt.com "ArchiMate Modeling Language"))

## 3.5 Access (доступ к данным) — ключевые правила

**Семантика:** модель «поведение/активная структура → пассивная структура». В процессном виде это означает: шаг процесса **читает/пишет** объект данных/бизнес-объект. ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))

**Откуда/куда:**

- **От:** Business Process / Function / Event **или** Active Structure (Role/Actor, Application Component)
    
- **К:** Business Object / Data Object (пассивные структуры) ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))
    

**Типы доступа (обязательно задавать):**

- **Read (R)** — чтение.
    
- **Write (W)** — запись/изменение/создание/удаление.
    
- **Read/Write (RW)** — и чтение, и запись.  
    Тип задаётся как свойство/метка связи (во многих инструментах — поле _Access_Type_). Визуально допускаются варианты стрелок/двунаправленности, но в модели направление логически идёт **от поведения/активной структуры к данным**. ([archimatetool.com](https://www.archimatetool.com/downloads/archi/Archi%20User%20Guide.pdf?utm_source=chatgpt.com "Archi User Guide"))
    

**Практика:**

- На L1 ставьте **только R/W/RW** на ключевые объекты.
    
- На L2/L3 показывайте доступ на каждом шаге, избегайте «висящих» объектов без Access.
    
- Не путайте **Flow** (передача работы) и **Access** (доступ к данным). ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))
    

## 3.6 Realization (реализация)

**От:** поведение/структура низшего уровня (например, Process → Business Service; Application Component → Application Service) **к:** более абстрактному. Используйте, чтобы показать, какой процесс/компонент реализует сервис. ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))

## 3.7 Composition/Aggregation

**От:** «целое» → **к:** «часть». Применяйте для иерархий процессов (L1 содержит L2, и т.д.). Не используйте для последовательности — для неё есть Triggering/Flow. ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))

## 3.8 Association / Specialization / Junctions

Используются ограниченно: Association — для прочих слабосемантических связей (по возможности избегайте и заменяйте на более точные), Specialization — когда действительно нужна таксономия; Junctions — для ветвлений/слияний потоков одного вида. ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))

# 4) Правила именования (elements & relations)

**Общие:**

- **Язык:** корпоративный (RU/EN) — единообразно внутри модели.
    
- **Формат имени процесса:** _Глагол_ + _Объект_ + _(Контекст)_, например: «Обработать заявку (онлайн-канал)».
    
- **Уровень в имени (опционально):** `L1/L2/L3` в конце или в атрибуте «Уровень».
    
- **Без аббревиатур**, если они не стандартизованы в глоссарии.
    
- **Сервисы:** как _результат/возможность_ в форме существительного: «Предоставление статуса заказа».
    
- **Объекты данных:** в единственном числе, класс сущности: «Заявка», «Договор», «Платёж».
    
- **События:** в совершенном виде: «Заявка получена», «Платёж отклонён».
    
- **Связи:** имена не требуются, **кроме** случаев Access (если инструмент не поддерживает типы) — тогда в имени связи укажите `R/W/RW`. Рекомендуется заполнять свойство типа доступа, а не имя. 
    

# 5) Минимальный состав атрибутов (заполнять обязательно)

## 5.1 Для **Business Process** (и подпроцессов)

- **Код**: `BP-<уровень>-<номер>` (напр., `BP-L2-015`)
    
- **Название**
    
- **Версия**
    
- **Статус жизненного цикла** (Draft/Approved/Deprecated)
    
- **Владелец процесса** (роль/подразделение)
    
- **Последний редактор**
    
- **Дата последней актуализации**
    
- **Уровень детализации** (L1/L2/L3)
    
- **Цель/результат** (кратко)
    
- **Метрики** (KPI, SLA — если применимо)
    
- **Связанные политики/регламенты** (ссылки)
    
- **Связанные сервисы** (Business/Application Services — ссылки на элементы)
    

## 5.2 Для **Business Event**

- **Код**: `BEV-<номер>`
    
- **Название** (в форме результата)
    
- **Тип** (External/Internal/Timer/Error)
    
- **Версия / Статус / Дата актуализации**
    
- **Источник/Приёмник** (если известно)
    

## 5.3 Для **Business Role / Actor**

- **Код**: `BR-<номер>` / `BA-<номер>`
    
- **Название**
    
- **Единица оргструктуры** (если Actor)
    
- **Владелец роли**
    
- **Связанные компетенции/допуски** (ссылка)
    

## 5.4 Для **Business Function**

- **Код**: `BF-<номер>`
    
- **Название**
    
- **Область ответственности**
    
- **Связанные процессы (родитель/часть)**
    

## 5.5 Для **Business Object**

- **Код**: `BO-<номер>`
    
- **Название** (класс данных)
    
- **Описание/состав ключевых атрибутов**
    
- **Качество данных/владелец данных**
    
- **Связь с доменной моделью/справочником**
    

## 5.6 Для **Application Service / Component / Data Object**

- **Коды**: `AS-`, `AC-`, `DO-`
    
- **Название**
    
- **Версия / Статус**
    
- **Владелец ИТ-сервиса/компонента**
    
- **Соглашения (SLA/OLA)** — если есть
    
- **Системы-источники/назначения (для DO)**
    

## 5.7 Для **связей**

- **Тип связи** (обяз.)
    
- **Специальные свойства:**
    
    - **Access** → _Access_Type_ = `Read` | `Write` | `ReadWrite`
        
    - **Association Directed** (если инструмент поддерживает)
        
- **Основание** (ссылка на требование/регламент/артефакт анализа)
    
- **Комментарий** (опционально: бизнес-правило, примечание к исключению) ([archimatetool.com](https://www.archimatetool.com/downloads/archi/Archi%20User%20Guide.pdf?utm_source=chatgpt.com "Archi User Guide"))
    

# 6) Шаблоны построения Process View

- **Каркас L1:** цепочка **Business Process** с **Triggering** между ними; старт/финиш как **Business Event**; ключевые **Business Objects** через **Access (R/W)**; опорные **Application Services** через **Serving**; исполнители через **Assignment** к шагам. ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))
    
- **Декомпозиция:** иерархия **Composition** (процесс → подпроцесс).
    
- **Перекладывание ответственности:** **Flow** между шагами разных ролей; «передача» пакета работ без изменения данных (данные — только Access). ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))
    
- **Данные:** каждый значимый шаг имеет хотя бы один Access к объекту; на L3 отображайте RW там, где есть модификация, и R — где только чтение. ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/dependency-relationship-part-1-access-relationship-archimate/?utm_source=chatgpt.com "Part 2 - What is Access Relationship in ArchiMate?"))
    

# 7) Чем заменить _Business Interaction_ (полный отказ)

Вместо _Business Interaction_ используйте:

- **Business Process** (или подпроцесс) для совместной деятельности,
    
- **Business Roles/Actors** с **Assignment** на этот процесс,
    
- **Flow/Triggering** между процессами/событиями разных ролей,
    
- при необходимости **Business Collaboration** (если нужно явно показать «команду» из нескольких ролей), но предпочтительно обойтись Process + Roles для простоты процессного вида. Официальные справочники описывают Interaction, но в нашей методике он исключён для единообразия. ([goodea.eu](https://www.goodea.eu/archimate/reference/?utm_source=chatgpt.com "ArchiMate 3.2 reference"))
    

# 8) Качество моделей и «чек-лист» перед публикацией

1. **Полнота метаданных:** у каждого элемента/связи заполнены обязательные атрибуты (коды, версии, даты, владельцы).
    
2. **Однозначность потоков:** не смешиваем Flow (передача работы) и Access (данные).
    
3. **Исполнители назначены:** у значимых шагов есть Assignment к Role/Actor.
    
4. **Сервисы показаны лаконично:** Serving только для опорных сервисов.
    
5. **Иерархия и уровни:** L1→L2→L3 через Composition; уровни и коды согласованы.
    
6. **Запрещённые элементы:** нет Business Interaction.
    
7. **Трассируемость:** связи помечены источниками (регламент, требования, инвентарь данных/сервисов).
    

---

# Источники

- **The Open Group: ArchiMate® 3.2 — справочные материалы и таблицы связей.** Использовано для определения типов элементов и связей, их направленности и применения (в т.ч. Triggering, Flow, Serving, Access). ([opengroup.org](https://www.opengroup.org/sites/default/files/docs/downloads/n221p.pdf?utm_source=chatgpt.com "ArchiMate 3.2 Specification Reference Cards"))
    
- **Gerben Wierda — “Mastering ArchiMate Edition 3.2”.** Использовано для практических приёмов моделирования и композиции процессных видов (актуальность 3.2). ([booktopia.com.au](https://www.booktopia.com.au/mastering-archimate-edition-3-2-gerben-wierda/book/9789083143439.html?srsltid=AfmBOoqXFRcKvvfSfNy2N4d3QLV_swOaHd_w8oSsv1THXWdUM53-UiYE&utm_source=chatgpt.com "Mastering ArchiMate Edition 3.2 by Gerben Wierda"))
    
- **Wierda: ArchiMate® 3.2 sheets (метамодель/элементы).** Для сверки перечня элементов бизнес-слоя и их ролей. ([R&A IT Strategy & Architecture](https://ea.rna.nl/wp-content/uploads/2023/08/archimate-sheets-en-20230805-s.pdf?utm_source=chatgpt.com "ArchiMate® 3.2 Metamodel -- Core"))
    
- **Bizzdesign Help — отношения ArchiMate и типы Access.** Для уточнения допускаемых связей и системных свойств Access_Type. ([help.bizzdesign.com](https://help.bizzdesign.com/articles/horizzon-help/archimate-relationships?utm_source=chatgpt.com "ArchiMate relationships"))
    
- **Visual Paradigm — руководства по отношениям и viewpoint’ам.** Для формулировок практики Process View и разграничения Flow/Triggering/Access. ([ArchiMate Resources for FREE](https://archimate.visual-paradigm.com/archimate-notation-part-8-relationships/?utm_source=chatgpt.com "ArchiMate Notation: Part 8 - Relationships"))
    
- **Sparx Systems (EA) — рекомендации по Serving между слоями.** Для примеров связи сервисов приложений с процессами. ([sparxsystems.com](https://sparxsystems.com/resources/user-guides/16.0/large-print/model-domains/languages/archimate.pdf?utm_source=chatgpt.com "ArchiMate Modeling Language"))
    

Если хотите, подготовлю шаблоны (Archi/EA/BiZZdesign) с предзаполненными свойствами и палитрой, а также чек-листом проверки — скажите, под какой инструмент.