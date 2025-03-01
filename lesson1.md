## 1. Вводная лекция. Инструменты для разработчика на МК.
### План лекции:
- Вступление
- Теория
  - Стандарты программирования
  - Система контроля версий. Git, GitFlow.
- Практика
  - Установка IDE для разработки ПО на С и Python.

### Теория
#### Стандарты программирования
Хороший стиль программирования: если ты читаешь код через неделю и понимаешь о чем он. 
Вставляем ссылку на книжку Мартина и дальше описываем почему он идет нахрен в микроконтроллерах. 

В каждом вашем проекте Вы пишете программу для себя. Не для друзей, не для коллег и не для сообщества. Вы пишите код для себя. И поэтому код нужно писать так, что бы ты понял как он работает без долгих терзаний мозга. 
Плохой стиль: 
Привести плохое именование, кривые дефайны
Если вы работаете в команде, то вам нужно договориться о том как вы именуете переменные, какие отступы и другой формат-стайл policy. В целом для языков уже есть базовые policy и вы можете придерживаться следующих правил:
1) Именование переменных должно быть читаемым
2) Избегать функций длиннее 30+ строк.
3) переиспользовать ваш код (организуя функции), если у вас одни и те же действия используются несколько раз. Так вы не выстрелите себе в ногу, когда нужно будет поменять общее поведение в программе.
4) Не переусердствовать с п2 и п3, если действия похожи, но делают координально разные вещи, и организация функции требует введения особых аргументов функции, то от такого решщения проще отказаться, чем вспоминать через пару недель а что же это за флаги такие
5) Разделять программу на файлы и использовать инкапсуляцию.

#### Система контроля версий. Git, GitFlow.
При работе в команде вам однозначно понадобится git. Так же и gitFlow.

**Что же такое git?**

**Git - это система контроля версий придуманная Линусом Торвальдсом для совместной работы программистов.**

Основные принципы работы с Git:
1. Локальный репозиторий:
   - Git работает с локальными репозиториями, которые хранятся на вашем компьютере. Это позволяет работать автономно без необходимости подключения к интернету.
2. Снимки состояния:
   - Git сохраняет снимки состояния проекта в виде коммитов. Каждый коммит представляет собой полную запись всех файлов в репозитории на момент создания коммита.
3. История изменений:
   - Git отслеживает историю изменений в проекте. Вы можете легко вернуться к любому предыдущему состоянию проекта, используя команды для просмотра и восстановления истории.
4. Ветвление и слияние:
   - Ветвление (branching) позволяет создавать новые ветки для разработки новых функций или исправлений без влияния на основную ветку. Слияние (merging) используется для объединения изменений из одной ветки в другую.
5. Распределенная система:
   - Git является распределенной системой управления версиями. Это означает, что каждый участник имеет полную копию репозитория, включая всю историю изменений. Это обеспечивает высокую надежность и скорость работы.
6. Отмена изменений:
   - Git предоставляет возможность отменять изменения на различных уровнях: отмена последнего коммита, отмена неотправленных изменений и т.д.
7. Работа с удаленным репозиторием:
   - Git позволяет работать с удаленными репозиториями, такими как GitHub, GitLab или Bitbucket. Вы можете отправлять свои изменения на удаленный репозиторий и получать последние обновления из него.
8. Конфликты при слиянии:
   - При слиянии веток могут возникать конфликты, если одни и те же строки были изменены в разных ветках. Git автоматически выявляет эти конфликты, и вам нужно будет их разрешить вручную.
9. Теги:
   - Теги (tags) используются для маркирования важных точек в истории проекта, таких как релизы. Они позволяют легко ссылаться на конкретные версии проекта.
10. Эффективное использование команд:
    - Git предоставляет множество команд для выполнения различных операций. Освоение базовых команд (git add, git commit, git push, git pull, git branch, git merge) позволит эффективно управлять проектом.

## TODO: Добавить QR для git и Пушкин 
https://habr.com/ru/companies/skillbox/articles/413161/


Теперь мы поняли что такое GIT. Как с ним работать мы поймем дальше на практических занятиях.

Теперь нам нужно разобраться со второй частью.

**GitFlow**

В Своей работе программисты почти всегда работают в командах и когда продукт компании выходит за пределы hello world очень важно расставлять вехи. Веха - Это жизненный этап программного обеспечения который гарантирует какое то работающее состояние программы.  

В Git есть ветки (Branch), которые пользователь в принципе может именовать как ему захочется. 
GitFlow Предлагает нам вести несколько веток проектов для разделения состояния программного кода. 

Первая ветка — master. Это базовая ветка со стабильно работающим кодом. В эту ветку программист заливает уже отлаженный код и именно сюда приходят пользователи для скачивания вашего кода.
Вторая ветка — develop. Это ветка в которой сейчас ведется автивная разработка. Код здесь должен запускаться, но не обязан работать и обладать полным функционалом. 
Ветки с индексом feature/. Когда программист хочет написать новую фишку программы от создает фичу. Фича наследуется от develop и позволяет программисту не мешая другим разрабам пилить новую функцию и не мешать другим. После окончания работы над фичей программист делает PullRequest  и вливает все свои изменения в develop. Если во время пулреквеста есть конфликты, то программист их решает и устраняет. 

Промежуточно на ветке develop могут проходить тестирования и тестировщих проверяет работу всей программы на предмет падения программы в результате затрагивания каких либо частей программы. Если такое произошло, то появляется issue для устранения бага. Программист которому поручают решение багов созхдает ветку с индексом bugfix в которой он решает возникший вопрос и делает пулреквест в девелоп с подтвержением на исправление бага. Тестирвоание повторяется. 

После того как программисты сделали 2, 3,5, 10, 100 своих фич, отладили все баги и они удовлетворили заказчика ПО по функциям они делают pullRequest в master. На этом этапе код уже должен быть стабильным и иметь либо список knowBugs или не иметь известных разрабам багов.
Обычно при заливании в мастер на многих серверах запускается автоматическое развертывание системы на боевые сервера. 
Поэтому важно понимать, к каким действиям приведут ваши pullRequest.

Если в работе программы у конечного пользователя выявился критический баг, устранение которого не терпит отлагательства, то от ветки master запускается ветка /hotfix, в которой устраняются критические баги. После их устранения — ветка сливается сразу в мастер.
 
Если баг не критичный, то создается issue и в порядке разработки бага устраняется программистами и готовится к релизам в рабочем порядке

Итак резюмируем:
- в master стабильный боевой код
- в develop код готовый к следующим релизам или тестированию
- в /feature/моя_фича я работаю над новыми функциями программы
- в /bugfix/мой_баг я исправляю известные баги программы
- в /hotfix/мой_просак я исправляю критические баги, которые нужно исправить немедленно.
- Вот в целом и все что нужно знать о GitFlow.  За мои 10 лет разработки данный принцип разработки показался для меня самым удобным.
 
Пока знаний по инструментам хватит. Начнем наше погружение.

### Практика
- Установка IDE для разработки ПО на С и Python.

