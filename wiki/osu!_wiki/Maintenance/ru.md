---
outdated_translation: true
outdated_since: 29a5a9f474335b22a431cd6065db4f5dd87e951e
---

# Устройство osu! wiki

*См. также: [Руководство по работе с osu! wiki](/wiki/osu!_wiki/Contribution_guide)*

Здесь рассказывается о технических и административных аспектах osu! wiki, а также приводятся различные [рутинные задачи по обновлению статей](#рутинная-работа), с которыми вы можете помочь. Все обсуждения, связанные с osu! wiki, ведутся на канале `#osu-wiki` [дискорд-сервера osu!](/wiki/Community/Discord_servers#official).

## Администраторы

*Основная статья: [Администраторы osu! wiki](/wiki/People/osu!_wiki_maintainers)*

Администраторы — лица, у которых есть [совместный доступ](https://docs.github.com/ru/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-personal-account-settings/permission-levels-for-a-personal-account-repository#collaborator-access-for-a-repository-owned-by-a-personal-account) к [репозиторию `ppy/osu-wiki`](https://github.com/ppy/osu-wiki/), где хранятся все статьи и новости. Они разбирают связанные с проектом задачи и проблемы, проверяют пулл-реквесты и принимают решения, определяющие развитие osu! wiki.

Администраторы также принимают или отклюняют пулл-реквесты. Если ваши правки пора публиковать, или вам нужно ревью, попросите кого-нибудь из них в канале `#osu-wiki`.

## Технические подробности

### Трекер задач

У osu! wiki есть [трекер задач](https://github.com/ppy/osu-wiki/issues) для идей и предложений, связанных как с самими статьями, так и с вики-движком сайта. Туда стоит публиковать свои идеи, а также сообщения об ошибках в какой-нибудь из статей. Обратите внимание, что **писать туда нужно только про вики**, а прочие официальные проекты osu! живут в других местах:

- [osu!(lazer)](https://github.com/ppy/osu)
- [Веб-сайт osu!](https://github.com/ppy/osu-web/)
- [Проблемы osu!(stable)](https://github.com/ppy/osu-stable-issues)

#### Теги

[Теги](https://github.com/ppy/osu-wiki/labels) — это метки, которые ставятся на пулл-реквесты и задачи на GitHub и отражают их содержание или особенности. Они добавляются администраторами и носят справочный характер (по их названию и так всё понятно). Теги красного цвета являются служебными и не требуют от пользователей никаких действий:

- `rule change`: изменения в правилах (например, в [критериях ранкинга](/wiki/Ranking_criteria)), которые должны быть проверены знающими людьми;
- `blocked`: пулл-реквест пока нельзя принять: в нём либо есть серьёзная проблема, либо он зависит от других изменений;
- `needs rebase`: обилие малосодержательных коммитов, которые нужно объединить и переоформить (обычно это делают сами администраторы перед тем, как влить пулл-реквест).

### Ссылки и перенаправления

У большинства статей в osu! wiki есть короткие ссылки-синонимы, которые настраиваются через файл [`redirect.yaml`](https://github.com/ppy/osu-wiki/blob/master/wiki/redirect.yaml). Они предназначены для использования за пределами вики — например, на форумах или в [чате](/wiki/Client/Interface/Chat_console), где для них существует отдельный синтаксис:

```
Ссылка на критерии ранкинга: [[RC]]
```

Помните, что новые перенаправления должны быть краткими и пригодными к быстрому использованию.

### Автоматизированные проверки {id=ci-checks}

В репозитории osu! wiki настроены [автоматизированные проверки](https://docs.github.com/ru/actions/automating-builds-and-tests/about-continuous-integration) (CI), отлавливающие распространённые ошибки в пулл-реквестах. Список проверок можно найти в файле [`continuous-integration.yml`](https://github.com/ppy/osu-wiki/blob/master/.github/workflows/continuous-integration.yml).

В файле [`package.json`](https://github.com/ppy/osu-wiki/blob/master/package.json) перечислены все плагины ([remark](https://github.com/remarkjs/remark)) для CI (некоторые из них написаны администраторами osu! wiki).

Проверки запускаются для каждого коммита автоматически, если автор изменений уже коммитил в osu! wiki. Чтобы ваш пулл-реквест приняли, необходимо исправить все ошибки, обнаруженные проверками. Их [статус](img/ci-status.png) можно проверить следующим образом:

1. Пролистайте пулл-реквест до конца, найдите в табличке строку `osu-wiki continuous integration` и нажмите в ней на ссылку `Details`.
2. На новой странице найдите пункт `run remark on changed files` и кликните по нему. Около каждой найденной ошибки будет указана её причина и местоположение.

Помимо этого, некоторые ошибки появляются под конкретными изменениями в файлах на вкладке `Files changed`, и увидеть их можно только там.

Если вам нужна помощь в расшифровке автоматически найденных ошибок, обращайтесь в канал `#osu-wiki`.

#### Отключение проверок

Проверки обычно нужны, чтобы отлавливать ошибки в статьях на этапе ревью. Тем не менее, их можно временно отключить на случай, если в логике самой проверки есть ошибка, либо если та выдаёт некорректный результат. Что делать в этом случае, описано ниже. Если вам нужно выключить проверку по другой непредусмотренной причине, поговорите с кем-то из [администраторов вики](/wiki/People/osu!_wiki_maintainers).

Ниже — список всех проверок, которые запускаются автоматически:

| # | Тип проверки | Инструмент | Пояснение | Как обойти |
| :-: | :-- | :-- | :-- | :-- |
| 1 | Размер файлов | [`meta/check-file-sizes.sh`](https://github.com/ppy/osu-wiki/blob/master/meta/check-file-sizes.sh) | Проверяет, чтобы размер файлов изображений умещался в [ограничения для новостей и статей](/wiki/Article_styling_criteria/Formatting#размер-файла) (1 МБ). Для файлов более 0,5 МБ выдаётся предупреждение. | Никак. |
| 2 | Markdown | [remark](https://github.com/remarkjs/remark), запускаемый через [`meta/remark.sh`](https://github.com/ppy/osu-wiki/blob/master/meta/remark.sh) | Проверяет правильность и единообразие разметки Markdown в новостях и статьях. | В описание пулл-реквеста нужно добавить строчку `SKIP_REMARK`. Чтобы подавить конкретную ошибку, над строчкой с ней нужно добавить `<!-- lint ignore rule-name -->`, заменив `rule-name` на нарушаемое правило. |
| 3 | YAML | Команда `check-yaml` из набора [`osu-wiki-tools`](https://github.com/Walavouchey/osu-wiki-tools) | Проверяет структуру YAML в [`redirect.yaml`](https://github.com/ppy/osu-wiki/blob/master/wiki/redirect.yaml) и [метаданных](/wiki/Article_styling_criteria/Formatting#метаданные) статей. | Никак. |
| 4 | Битые ссылки | Команда `check-links` из набора [`osu-wiki-tools`](https://github.com/Walavouchey/osu-wiki-tools) | Проверяет, что все [вики-ссылки](/wiki/Article_styling_criteria/Formatting#вики-ссылки) ведут на существующую статью или новость, либо на их разделы. | В описание пулл-реквеста нужно добавить строчку `SKIP_WIKILINK_CHECK`. |
| 5 | Устаревшие переводы | Команда `check-outdated-articles` из набора [`osu-wiki-tools`](https://github.com/Walavouchey/osu-wiki-tools) | Проверяет, что при обновлении английской версии статьи её переводы [помечены как устаревшие](/wiki/Article_styling_criteria/Formatting#устаревший-перевод). | В описание пулл-реквеста нужно добавить строчку `SKIP_OUTDATED_CHECK`. |

##### Правило для remark [`no-heading-punctuation`](https://github.com/remarkjs/remark-lint/tree/main/packages/remark-lint-no-heading-punctuation)

В конце заголовков не ставят точки, поскольку заголовки — это не предложения. Тем не менее, иногда точку нужно разрешить — например, если она входит в название исполнителя или песни.

```markdown
<!-- lint ignore no-heading-punctuation -->

### Amusing Reflection Rag.
```

##### Правило для remark [`heading-increment`](https://github.com/remarkjs/remark-lint/tree/main/packages/remark-lint-heading-increment)

Уровень (размер) заголовков обычно понижается последовательно, без пропусков. В [инфобоксах](/wiki/Article_styling_criteria/Formatting#инфобокс) разрешены только заголовки 4 и 5 уровней, которые, скорее всего, нарушат это правило.

```markdown
# Список любимых маперов peppy

::: Infobox
<!-- lint ignore heading-increment -->

#### peppy

Создатель osu!.
:::
```

##### Проверка ссылок

*См. также: [Критерии оформления статей/Оформление § Вики-ссылки](/wiki/Article_styling_criteria/Formatting#вики-ссылки)*

Если вы редактируете статью, рекомендуется заодно исправить и битые ссылки, если они там есть. Иногда это становится нетривиальной задачей, либо мешает:

- В случае мелких точечных правок, которые не призваны подправить всю статью целиком;
- Ссылки на разделы, которых ещё (или уже) нет в переводе;
- Перемещение файлов в другое место (если ссылка, ведущая на них, и так была сломана).

##### Устаревшие переводы

*См. также: [Критерии оформления статей/Оформление § Устаревший перевод](/wiki/Article_styling_criteria/Formatting#устаревший-перевод) и [Критерии оформления статей/Содержание § Равенство содержания](/wiki/Article_styling_criteria/Writing#равенство-содержания)*

Пропускать эту проверку (и не помечать переводы как устаревшие) стоит, если ваши правки не меняют смысл и подробности статьи и заключаются в переформулировке фраз, исправлении ошибок, опечаток, и т.д.

### Разработка

osu! wiki интегрирована в веб-сайт osu!: о технических ошибках и своих идеях сообщайте в [трекер задач для веб-сайта](https://github.com/ppy/osu-web/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3Aarea%3Awiki) в репозитории `ppy/osu-web`. Чтобы остальные редакторы знали о найденной вами проблеме, пришлите ссылку на неё в канал `#osu-wiki` (или в соответствующую задачу в трекере).

### Инструменты

Некоторые программы или сервисы облегчают работу с вики, но не относятся напрямую к веб-сайту osu! и потому туда **не** добавлены. Написать подобный инструмент может любой желающий, владеющий навыками программирования. Ниже — несколько полезных примеров:

- [osu-wiki status](https://osu.wiki/status/en): список статей, написанных на каждом языке, и как именно их можно улучшить (перевод, обновление, дописывание). Пожелания по работе можно оставлять в [ppy/osu-wiki#2486](https://github.com/ppy/osu-wiki/issues/2486).
- [osu-wiki-bin](https://github.com/cl8n/osu-wiki-bin): коллекция скриптов на Node.js для автоматических проверок и правок (поиск битых ссылок, обновление статей про группы, поиск с заменой слов и т.д.).
- [osu-wiki-tools](https://github.com/Walavouchey/osu-wiki-tools): утилита на Python для автоматических проверок (поиск битых ссылок, порядок обновления статей), которая запускается в ходе CI
- [scissors](https://github.com/TicClick/scissors): утилита на Rust, проверяющая ошибки в указании флагов или имён пользователей.

## Рутинная работа

*Примечание: на сайте [osu-wiki status](https://osu.wiki/status/en) есть список статей, нуждающихся в доработке.*

Ценность и полнота вики зависят от участия сообщества. Вы можете помочь администраторам и редакторам, если тоже сделаете пару правок. О том, как именно их сделать, см. [Руководство по работе с osu! wiki](/wiki/osu!_wiki/Contribution_guide). Если вы где-то застряли, попросите помощи на канале `#osu-wiki` [дискорд-сервера osu!](/wiki/Community/Discord_servers#official).

### Перевод статей

<!-- note: the GitHub links are intentional here, because they expose many articles of a category at once -->

*Список переводов и их статус: см. [osu-wiki status](https://osu.wiki/status/en)*

osu! wiki читают пользователи со всего мира. Чтобы помочь вашему локальному сообществу и, возможно, привлечь в игру новых крутых игроков, мапперов, моддеров или разработчиков, переведите на русский какую-нибудь статью или обновите устаревшие переводы. Если вы знаете другой язык, убедитесь, что он [поддерживается osu! wiki](/wiki/Article_styling_criteria/Formatting#локали). Помните, что перевод должен [совпадать с оригиналом по содержанию](/wiki/Article_styling_criteria/Writing#равенство-содержания). Если вы хорошо владеете языком и умеете чётко излагать мысли, беритесь за какие-нибудь ключевые статьи, например, [правила](https://github.com/ppy/osu-wiki/tree/master/wiki/Rules) или [критерии ранкинга](https://github.com/ppy/osu-wiki/tree/master/wiki/Ranking_criteria). Новичкам имеет смысл начать с маленьких статей, чтобы ревьюеры могли легко указать на основные ошибки.

Перевод могут добавить и без ревью, если за неделю его никто не проверил.

### Дописывание заготовок

*Примерный объём работы: см. [список статей-заготовок на английском](https://github.com/search?q=stub%3A+true+repo%3Appy%2Fosu-wiki+filename%3Aen.md)*

Некоторые статьи osu! wiki не завершены и содержат лишь базовые сведения. Они самостоятельны, но являются *заготовками*: нужно, чтобы кто-то написал полноценный, завершённый текст. Если вы хорошо знакомы с предметом статьи, то можете доработать её и тем самым поделиться знаниями.

### Расстановка ссылок

Одна из ключевых особенностей вики — *связность*: ссылки на другие статьи помогают читателям следить за ходом мыслей. Чтобы добавить связи, делайте ссылками слова или фразы в тех местах, где это поможет лучше понять предмет статьи. При необходимости можно ссылаться на конкретные разделы статей, либо давать ссылку на [статью с определениями](/wiki/Article_styling_criteria/Formatting#статьи-с-определениями), если у слова есть несколько значений.

### Написание статей

osu! постоянно меняется: появляются новые карты, новые возможности самовыражения и прочие *новые* вещи. Если о чём-то нет статьи, возможно, её стоит написать, чтобы остальные тоже об этом узнали. Очередной турнир или конкурс? Свежая функциональность в osu!? Раскрыта одна из тайн прошлого? Вперёд, за клавиатуру.

### Обновление статей

*Примерный объём работы: см. [список TODO в английских статьях](https://github.com/search?q=TODO+repo%3Appy%2Fosu-wiki+filename%3Aen.md)*

Уже написанные статьи тоже надо обновлять. Если в текст попала ошибка, или тема раскрыта не полностью, или вы просто хотите дописать или поправить статью, не стесняйтесь улучшить osu! wiki. Перед тем, как менять что-то важное или вносить масштабные изменения, стоит обсудить это с остальными: напишите в канал `#osu-wiki` или [заведите новую задачу](https://github.com/ppy/osu-wiki/issues/new).
