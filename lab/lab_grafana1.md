# Лабораторна робота №6. Основи роботи з Grafana. 
Завдання лабораторної роботи навчитися основам роботи з Grafana у парі з базою даних часових рядів Influxdb.

## Порядок виконання лабораторної роботи

### 1.Створення безкоштовного облікового запису на Grafana Cloud Portal

- [ ] На порталі [Grafana Cloud Portal](https://grafana.com/) перейдіть на сторінку створення безкоштовного акаунта. 
![](7_1media/1.png) 

Рис.1. Портал Grafana Cloud Portal

- [ ] Заповніть форму або використайте Google, GitHub чи Microsoft акаунти для реєстрації.
  ![](7_1media/2.png) 

Рис.2. Форма реєстрації на порталі Grafana

- [ ] У наступному вікні можете залишити все за замовченням і натисніть `finish setup`

![image-20231120141224406](7_1media/image-20231120141224406.png)

- [ ] Через деякий час для вас буде розгорнутий віртуальний контейнер і ви попадете стартову сторінку натисніть в правому верхньому кутку`I'm already familiar with Grafana. Skip setup` щоб перейти на Grafana Cloud.

![image-20231120142357564](7_1media/image-20231120142357564.png)

- [ ] Наступного разу, коли Ви будете заходити на https://grafana.com/ , після входження в систему запускати `Grafana`  з порталу Grafana Cloud треба через My Account ->плитка Grafana -> Launch.
   ![](7_1media/3.png) 

Рис.3. Запуск Grafana з порталу Grafana Cloud Portal

- [ ] Після входження в Grafana Cloud у вас з'явиться  

![image-20231120144412785](7_1media/image-20231120144412785.png)

### 2.Створення джерела даних в Grafana

#### 2.1. Генерація API токена для доступу до бакету "firstbucket" в Influxdb з Grafana

- [ ] В Influxdb UI (<https://cloud2.influxdata.com/signup>) перейдіть в розділ `API TOKENS`: пункт меню **Load Data** -> **API Tokens**.
- [ ] Натисніть **+ Generate API Token** у верхньому правому куті, оберіть опцію **Custom API Token**.
- [ ] У вікні "Generate a custom API token" в поле description вставте `Access to firstbucket from Grafana`. В розділі **Resourses** для firstbucket оберіть опції читання та запису. 

![](7_1media/4.png)

Рис.4. Налаштування токена доступу

- [ ] Натисніть **Generate**.
- [ ] Скопіюйте та збережіть згенерований токен.

![](7_1media/1.gif) 

Запис.1. Генерація API токена для доступу до бакету "firstbucket"

#### 2.2. Створення джерела даних Influxdb в Grafana

- [ ] В інтерфейсі користувача Grafana перейдіть в пункт меню `Connections/Data Sources` (шестерня зліва внизу).

![image-20231120145149486](7_1media/image-20231120145149486.png)

Рис.5. Налаштування Grafana

- [ ] В розділі **Data sources** натисніть `Add new data source`.

![image-20231120145420085](7_1media/image-20231120145420085.png)

- [ ] В якості джерела даних оберіть `InfluxDB`.

- [ ] В налаштуваннях джерела даних InfluxDB поле **Name** залиште без змін, тобто `InfluxDB`.
- [ ] Активуйте опцію **Default **біля поля **Name** .
- [ ] Для опції **Query Language** залиште мову `flux`. 

![image-20231120224254147](7_1media/image-20231120145639882.png)

Запис.2. Створення джерела даних в Grafana

У попередній лабораторній роботі в якості перегляду даних в InfluxDB використоувалася мова fluxQL (SQL подібна мова запитів). У цій та наступній роботі використоуватиметься більша потужна мова Flux.

- [ ] В розділі налаштувань **HTTP** джерела даних InfluxDB для поля **URL** скопіюйте `Cluster URL` з налаштувань організації в Influxdb UI (див [лабораторну роботу Influx](lab_influx1.md)).

![image-20231120150141337](7_1media/image-20231120150141337.png)

Запис.3. Створення джерела даних в Grafana

- [ ] В розділі налаштувань **AUTH**  опція `Basic auth` повинна бути деактивована
- [ ] В розділі налаштувань **InfluxDB Details** в поле  **Organization** скопіюйте `Organization ID` з налаштувань організації в Influxdb UI (Influxdb UI -> пункт меню Account -> Organization settings -> Organization ID).
- [ ] В поле **Token** вставте токен, створений в пункті 2.1.
- [ ] В поле **Default Bucket** напишіть `firstbucket`.
- [ ] Натисніть кнопку `Save & test`.

Після цього, якщо підключення вдалося, виведеться напис: "datasource is working. 1 buckets found"

![image-20231120225052403](7_1media/image-20231120225052403.png)

#### 2.3. Відновлення запису даних в influxdb

- [ ] Запустіть OPCUA сервер `UaCPPServer` та `node-red`, тобто відновіть запис даних з OPC UA сервера в `influxdb`, який було налаштовано в попередніх лабораторних роботах по Influxdb.

### 3.Створення першого Dashboard

#### 3.1. Створення загальної директорії для зберігання дашбоардів лабораторної роботи

- [ ] Перейдіть в розділ Dashboards

![image-20231120225535879](7_1media/image-20231120225535879.png) 

- [ ] У правому меню виберіть  `New->Folder` 
- [ ] Дайте назву новій директорії – `Lab7`.
- [ ] Натисніть кнопку `Create`.

#### 3.2. Створення дашбоарду

Після створення директорії, за замовчуванням, відкриваються дашбоарди, які зберігаються в даній папці. Оскільки вона пуста, пропонується створити новий дашбоард.

- [ ] Натисніть кнопку `+ Create Dashboard`
- [ ] У вікні пропозиції виберіть `+ Add visualization`, це додасть одну панель візуалізації  
- [ ] У вікні `Select Data Source` виберіть `InfluxDB`
- [ ] Збережіть дашбоард під іменем `Main` - для цього натисніть кнопку `Save` у правому верхньому кутку

![image-20231120230417848](7_1media/image-20231120230417848.png)

![image-20231120230655863](7_1media/image-20231120230655863.png)

Рис.6. Збереження дашбоарду

#### 3.3. Навігація в Grafana

Для навігації між різними директоріями та дашбоардами використовується головний пункт меню Dashboards

- [ ] Наведіть курсор на іконку Dashboards 
- [ ] Зверніть увагу, що через це меню доступні всі створені вами директорії, а також системна директорія **GrafanaCloud**. Звідси також можна створити новий дашбоард або нову директорію.
- [ ] Натисніть на назву папки `Lab7`, відкрийте дашбоард `Main`.

#### 3.4. Редагування інформаційної панелі (Panel)

- [ ] При створенні дашбоарда `Main`, Ви також створили одну панель. Наразі вона немає жодних налаштувань і містить опис `Panel Title`.  Використовуючи меню перейдіть в режим редагування.

![image-20231120231413076](7_1media/image-20231120231413076.png)

Рис.7. Перехід в редагування панелі

##### 3.4.1. Налаштування діапазону відображення та періодичності

- [ ] Переконайтеся, що в режимі редагування панелі встановлено тип візуалізації `Time series`.
- [ ] Встановіть часовий діапазон для відображення – `30 хвилин`.

![image-20231120231959965](7_1media/image-20231120231959965.png)

Рис.8. Редагування панелі

- [ ] Переконайтеся, що в якості джерела даних (`Data source`) встановлено `InfluxDB` (під час конфігурування джерела даних InfluxDB ми означили його як джерело даних за замовчуванням). В іншому випадку оберіть `InfluxDB` з випадального списку. 

![image-20231120232251679](7_1media/image-20231120232251679.png)

Рис.9. Редагування панелі Data Source

- [ ] В поле запиту Flux вставте запит для читання температур всіх кондиціонерів та їх агрегування (розрахунок середнього значення щохвилини). 

```js
from(bucket: "firstbucket")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "Building")
  |> filter(fn: (r) => r["_field"] == "Temperature")
  |> filter(fn: (r) => r["device"] == "AirConditioner_1" or r["device"] == "AirConditioner_10" or r["device"] == "AirConditioner_2" or r["device"] == "AirConditioner_3" or r["device"] == "AirConditioner_4" or r["device"] == "AirConditioner_5" or r["device"] == "AirConditioner_6" or r["device"] == "AirConditioner_7" or r["device"] == "AirConditioner_8" or r["device"] == "AirConditioner_9")
  |> aggregateWindow(every: 1m, fn: mean, createEmpty: false)
```

Детальніше мова flux буде розглядатися в наступній лабораторній роботі. Тут трохи прокоментуємо що саме відбувається в цьому коді. Спочатку з бакета  `firstbucket` вибираються дані, які далі обробляються в конвеєрній обробці, яка позначається `|>`. Спочатку вказується діапазон вибірки по часу, потім йде фільтрація  даних за службовим полем `_measurement`, `field`, та тегом `device` (один з вказаних по or). З вибраними даними відбувається агрегація, де за кожну хвилину розраховується середнє і видається результат. 

- [ ] Натисніть кнопку `Apply`.

![image-20231120232741146](7_1media/image-20231120232741146.png)

Рис.10. Редагування панелі

- [ ] Розтягніть панель на всю ширину екрана (дашбоарду).

![image-20231120233028056](7_1media/image-20231120233028056.png)

- [ ] Збережіть зміни дашбоарду (дискета у правому верхньому куті дашбоарду), виставте опцію `Save current time range as dashboard default` (залишити виставлений часовий діапазон як діапазон дашбоарду за замовчуванням).

![image-20231120233216673](7_1media/image-20231120233216673.png)

Запис.4. Збереження дашбоарду

- [ ] Вручну оновіть дашбоард (іконка оновлення в правому верхньому куті дашбоарду ![](7_1media/11.png)).
- [ ] Встановіть період оновлення дашбоарду – 1 хвилина. 

![image-20231120233452354](7_1media/image-20231120233452354.png)

Рис.11. Період оновлення дашбоарду

- [ ] Збережіть дашбоард.

##### 3.4.2. Налаштування зовнішнього вигляду графіків

- [ ] Поверніться до редагування панелі. Для цього натисніть на меню панелі `...` та оберіть опцію `Edit`.
- [ ] Для панелі встановіть наступні налаштування на вкладці `All` на правій бічній панелі:

- Panel options -> Title: `Temperature`
- Tooltip -> Tooltip mode: `All`
- Tooltip -> Values sort order: `Ascending`
- Legend -> Legend mode: `Table`
- Legend -> Legend placement: `Right`
- Legend -> Legend values: `Last`
- Graph styles ->Style:`Line`, Line interpolation: `Smooth`
- Graph styles -> Point size: `3`
- Axis -> Placement: `Left`
- Axis -> Label: `Temperature`
- Standard options -> Unit: `Celsium (°C)`
- Standard options -> Decimals: `2`
- Thresholds -> колір - `red` , значення `72 `
- Thresholds -> Show thresholds: `As lines`

- [ ] Встановіть налаштування на вкладці `Overrides`: назва трендів замість "Temperature AirConditioner_1, ... , Temperature AirConditioner_10" – `Air Conditioner1, ... , Air Conditioner 10` відповідно. Для цього:

  - [ ] натисніть на кнопку `+ Add field override`
  - [ ] Оберіть опцію `Field with name`
  - [ ] З випадального списку оберіть `Temperature AirConditioner_1`
  - [ ] Натисніть на `+ Add override property`
  - [ ] Оберіть властивість Standard options -> Display name
  - [ ] Дайте назву поля `Air Conditioner 1`
  - [ ] Повторіть процедуру для ще одного кондиціонера


![image-20231120235014125](7_1media/image-20231120235014125.png)

Запис.5. Налаштування властивості override

- [ ] Збережіть налаштування панелі – кнопка `Apply`.
- [ ] Збережіть дашбоард. Зовнішній вигляд має бути схожим як на рис. нижче

![image-20231120235456065](7_1media/image-20231120235456065.png)

##### 3.4.3. Робота з панеллю в режимі виконання

- [ ] Спробуйте змінити часовий проміжок відображення дашбоарду за допомогою `time picker`, а також виділенням потрібної області на графіку.
- [ ] Спробуйте відображати різні тренди, активуючи їх в легенді.  

![](7_1media/6.gif)

Запис.6. Зміна часового діапазону для відображення

#### 3.5. Налаштування змінної дашбоарду для динамічного вибору кондиціонера для відображення на панелі (аналогічно до змінних Influxdb)

##### 3.6.1. Ознайомлення із можливостями Explore

Для відлагодження запитів до баз даних, Grafana надає окремий інструмент Explore.

- [ ] Перейдіть до пункту бокового меню `Explore`.

![image-20231121085839640](7_1media/image-20231121085839640.png)

Рис.15. Explore

- [ ] Переконайтеся, що в якості джерела даних обрано `InfluxDB`.

![](7_1media/18.png)

Рис.16. Explore

- [ ] В поле **A** для запиту вставте запит на мові Flux для читання всіх значень тегу **device** за останній рік. Запит наведено нижче:

```js
import "influxdata/influxdb/v1"
v1.tagValues(bucket: "firstbucket", tag: "device", predicate: (r) => true, start: -1y)
```

- [ ] Натисніть кнопку `Run query`.
- [ ] Під полем для запиту перегляньте таблицю з результатом виконання запиту.

![](7_1media/19.png)

Рис.16. Результат запиту

##### 3.6.2. Створення змінної `device` дашбоарду Main

Змінні дають можливість використовувати змінні властивості, якими оперує користувач. У цьому пункті буде створена змінна з іменем `device`, перелік значень якої буде зчитуватися з бакета, як це було зроблено вище. Далі користувач зможе вибирати одне або кілька значень для фільтрування вибірки.

- [ ] Відкрийте дашбоард `Main` (через пункт меню Browse).
- [ ] Відкрийте налаштування дашбоарду (шестерня в верхньому правому кутку дашбоарду Dashboard Settings).
- [ ] Перейдіть до пункту `Variables`. 

- [ ] Натисніть `Add variable`

- [ ] Налаштуйте загальні настройки змінної наступним чином:

- Type: `Query`
- Name: `device`
- Label: `Device`

![image-20231121090519605](7_1media/image-20231121090519605.png)

- [ ] У налаштуваннях запиту `Query options` зробіть наступне

- Data source – `InfluxDB`,

```js
import "influxdata/influxdb/v1"
v1.tagValues(bucket: "firstbucket", tag: "device", predicate: (r) => true, start: -1y)
```

- Refresh – `On dashboard load`
- Multi-value: активувати опцію
- Include All option: активувати опцію

![image-20231121090806363](7_1media/image-20231121090806363.png)

Рис.17. Налаштування опцій запиту  для змінної дашбоарду

- [ ] Натисніть `Apply`.
- [ ] Натисніть `Save dashboard` а потім підьвердіть кнопкою `save`.

##### 3.6.3 Використання змінної `device` в запиті

- [ ] Поверніться до редагування панелі `Temperature` (через кнопку три крапки на панелі).
- [ ] Замініть фрагмент запиту з `filter(fn: (r) => r["device"]`, який наведений нижче 

```js
|> filter(fn: (r) => r["device"] == "AirConditioner_1" or r["device"] == "AirConditioner_10" or r["device"] == "AirConditioner_2" or r["device"] == "AirConditioner_3" or r["device"] == "AirConditioner_4" or r["device"] == "AirConditioner_5" or r["device"] == "AirConditioner_6" or r["device"] == "AirConditioner_7" or r["device"] == "AirConditioner_8" or r["device"] == "AirConditioner_9")
```

**на**

```js
|> filter(fn: (r) => r["device"]  =~ /^${device:regex}$/ )
```

Звернення до змінної реалізується за допомогою `$`+назва змінної. Щоб забезпечити множинний вибір девайсів для відображення на графіку, окрім звернення до змінної безпосередньо – застосували регулярний вираз.

- [ ] Застосуйте зміни панелі (кнопка `Apply`) та збережіть дашбоард.
- [ ] Перевірте роботу змінної Device, змінюючи її, в тому числі використовуючи множинний вибір.

![](7_1media/8.gif)

Запис.8. Перевірка роботи змінних дашбоарду

#### 3.7. Налаштування візуалізації `Stat` для відображення останнього значення температури в числовому вигляді

- [ ] Скопіюйте запит з панелі `Temperature`.

- [ ] Додайте нову інформаційну панель в дашбоард **Main**. Для цього натисніть `Add` ->`Visualisation` у верхньому правому куті дашбоарду.
- [ ] Замість `Time Series` оберіть візуалізацію `Stat`.
- [ ] Вставте скопійований запит з панелі `Temperature`.

- [ ] Для панелі встановіть наступні налаштування на вкладці `All`:

- Panel options -> Title: `${device}`
- Repeat options -> Repeat by variable: `device`
- Repeat options -> Max per row: `8`
- Value options -> Show: `Calculate`
- Value options -> Calculation: `Last*`
- Stat styles -> Graph mode: `None`
- Standard options -> Unit: `Celsium (°C)`
- Standard options -> Decimals: `1`
- Standard options -> Color scheme: `From thresholds`
- Thresholds -> `72: red`

- [ ] Застосуйте зміни панелі (кнопка `Apply`) та збережіть дашбоард.

- [ ] Перевірте роботу змінної Device, змінюючи її, в тому числі використовуючи множинний вибір.

![image-20231121092539476](7_1media/image-20231121092539476.png)

Рис.18. Дашбоард Main

### 4.Створення дашбоарду журнал подій – `Events`

#### 4.1. Організація запису змін станів кондиціонерів в Influxdb

- [ ] Імпортуйте в node-red в потік **OPC UA to influxdb** наведені нижче вузли:

```json
[
    {
        "id": "40ebca3949a03fc6",
        "type": "inject",
        "z": "42cdb79f439ee463",
        "name": "",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": true,
        "onceDelay": 0.1,
        "topic": "",
        "payloadType": "date",
        "x": 110,
        "y": 200,
        "wires": [
            [
                "deed70ad5094bdfe"
            ]
        ]
    },
    {
        "id": "deed70ad5094bdfe",
        "type": "function",
        "z": "42cdb79f439ee463",
        "name": "multiplesubscribe",
        "func": "msg.payload=10000;\nfor(i=1;i<11;i++){\n   msg.topic=`ns=3;s=AirConditioner_${i}.State`;\n   node.send(msg);\n    }\n",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 290,
        "y": 200,
        "wires": [
            [
                "1c7613ca498e131c"
            ]
        ]
    },
    {
        "id": "1c7613ca498e131c",
        "type": "OpcUa-Client",
        "z": "42cdb79f439ee463",
        "endpoint": "501a96153c7c8d57",
        "action": "subscribe",
        "deadbandtype": "a",
        "deadbandvalue": 1,
        "time": "1",
        "timeUnit": "s",
        "certificate": "n",
        "localfile": "",
        "localkeyfile": "",
        "securitymode": "None",
        "securitypolicy": "None",
        "folderName4PKI": "",
        "name": "",
        "x": 500,
        "y": 200,
        "wires": [
            [
                "500d758fa01f947b"
            ]
        ]
    },
    {
        "id": "500d758fa01f947b",
        "type": "switch",
        "z": "42cdb79f439ee463",
        "name": "",
        "property": "topic",
        "propertyType": "msg",
        "rules": [
            {
                "t": "cont",
                "v": "ns=3;s=AirConditioner",
                "vt": "str"
            }
        ],
        "checkall": "true",
        "repair": false,
        "outputs": 1,
        "x": 615,
        "y": 200,
        "wires": [
            [
                "8c90ebf4b87e9109"
            ]
        ],
        "l": false
    },
    {
        "id": "8c90ebf4b87e9109",
        "type": "function",
        "z": "42cdb79f439ee463",
        "name": "",
        "func": "let str=\"\";\nlet obj={};\nstr=msg.topic.replace(\"ns=3;s=\",\"\");\nobj.device=str.replace(\".State\",\"\");\nobj.fields={State:msg.payload};\nobj.timestamp=msg.serverTimestamp;\nmsg={};\nmsg.payload=obj;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 675,
        "y": 200,
        "wires": [
            [
                "a153c3bec6acf22a"
            ]
        ],
        "l": false
    },
    {
        "id": "a153c3bec6acf22a",
        "type": "join",
        "z": "42cdb79f439ee463",
        "name": "",
        "mode": "custom",
        "build": "array",
        "property": "payload",
        "propertyType": "msg",
        "key": "topic",
        "joiner": "\\n",
        "joinerType": "str",
        "accumulate": false,
        "timeout": "1",
        "count": "",
        "reduceRight": false,
        "reduceExp": "",
        "reduceInit": "",
        "reduceInitType": "num",
        "reduceFixup": "",
        "x": 775,
        "y": 200,
        "wires": [
            [
                "0ba540df0fcba817"
            ]
        ],
        "l": false
    },
    {
        "id": "0ba540df0fcba817",
        "type": "function",
        "z": "42cdb79f439ee463",
        "name": "msgtoinflux",
        "func": "let datastore=[];\nconst State = new Map([\n  [0, 'Вимкнено'],\n  [1, 'Увімкнено'],\n  ]);\nclass sample {\n  constructor(payload) {\n    this.data={\n        measurement:'Building_log',\n        tags: {\n            device: payload.device,\n        },\n        fields: {message:payload.device+\" \"+State.get(payload.fields.State)},\n        timestamp: new Date(payload.timestamp).getTime()\n    }\n  }\n}\nfor (let i=0; i<msg.payload.length;i++){\n    dataobj=new sample(msg.payload[i]);\n    datastore.push(dataobj.data);\n}\nmsg.payload = {\n    bucket:'firstbucket',\n    precision: 'ms',\n    data:datastore,\n};\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 930,
        "y": 200,
        "wires": [
            [
                "260f8e645e064f0c"
            ]
        ]
    },
    {
        "id": "501a96153c7c8d57",
        "type": "OpcUa-Endpoint",
        "endpoint": " opc.tcp://DESKTOP-E871FG5:48010",
        "secpol": "None",
        "secmode": "None",
        "login": false
    }
]
```

- [ ] З'єднайте вузол `msgtoinflux` з наявним вузлом `InfluxDBwrite`.

![](7_1media/22.png)

Рис.19. Нові вузли для node-red

- [ ] Зробіть деплой.

Імпортовані вузли забезпечуються опитування стану 10-ти кондиціонерів і запис цього стану у вигляді текстового повідомлення в той самий бакет Influxdb (`firstbucket`) проте в інший **measurement** – `Building_log`.

#### 4.2. Створення дашбоарду `Events`

- [ ] У дашбоарді `Main` виберіть кондиціонери 1, 10 та 2. 
- [ ] Скопіюйте повністю URL дашбоарду `Main`  в блокнот, він знадобиться далі.

![](7_1media/23.png)

Рис.20. Посилання на дашбоард Main

- [ ] Зверніть увагу, що посилання містить назви змінних/змінної активної для дашбоарду, а саме:

`&var-device=AirConditioner_1&var-device=AirConditioner_10&var-device=AirConditioner_2`

- [ ] В папці `Lab7` створіть дашбоард 
- [ ] Додайте нову панель в дашбоард у якості джерела даних виберіть Influx DB
- [ ] Оберіть візуалізацію типу `Table`.

- [ ] В поле запиту вставте наступний запит:

```js
from(bucket: "firstbucket")
  |> range(start: -1y)
  |> filter(fn: (r) => r["_measurement"] == "Building_log")
  |> filter(fn: (r) => r["_field"] == "message")
  |> group(columns:["_field"])
  |> sort(columns: ["_time"], desc: true) 
  |> drop(columns:["_measurement"])
```

- [ ] Для панелі встановіть наступні налаштування на вкладці `All`:
- Panel options -> Title: `Events`
- Header and footer -> Show table header: `деактивувати опцію`

- [ ] Для панелі встановіть наступні налаштування на вкладці `Overrides`:

  - [ ] Натисніть `+Add field Override`.
  - [ ] Оберіть опцію `field with name`.
  - [ ] З випадального списку оберіть `device (base field name)`.
  - [ ] Натисніть `+Add override property`.
  - [ ] Оберіть опцію `Data Links -> data links`.
  - [ ] Натисніть `Add link`.
  - [ ] У поле URL вставте посилання на дашбоард **`Main`** , який був скопійований в блокнот, змінивши налаштування device, тобто:

  **з**

  ```js
  https://******.grafana.net/d/*******/main?orgId=1&var-device=AirConditioner_1&var-device=AirConditioner_10&var-device=AirConditioner_2
  ```

  **на**

  ```js
  https://******.grafana.net/d/*******/main?orgId=1&var-device=${__value.raw}
  ```

  \*зірочками позначена індивідуальна для кожного частина посилання*

  - [ ] Активуйте опцію `Open in new tab`.
  - [ ] Натисніть `Save`, збережіть дашбоард з назвою `Events`.

  ![](7_1media/24.png)

Рис.21. Data link комірки таблиці

- [ ] Застосуйте зміни для панелі та збережіть дашбоард.
- [ ] Розтягніть панель на всю сторінку. 
- [ ] За допомогою OPC UA клієнта `UA Expert`, викличте метод  `Stop` для зміни станів кількох кондиціонерів. Вручну оновіть дашбоард `Events` та перегляньте результат.
- [ ] Викличте методи`Start` (якщо кондиціонер в стопі) або `Stop` (якщо кондиціонер в старті) для тих же кондиціонерів. Перегляньте результат в дашбоарді. 

![](7_1media/10.gif)

![](7_1media/11.gif)

Запис.10. Генерація подій (вкл/викл кондиціонер)

- [ ] Натисніть на посилання довільної комірки третього стовпчика.
- [ ] Зверніть увагу, що в новій вкладці відкрився дашбоард `Main`, з активною однією змінною, на назву якої ви натиснули.

### 5.Створення анотацій

#### 5.1. Створення анотації вручну

Анотації дозволяють робити помітки на графіку з прив'язкою до часу, щоб забезпечити контекст.

- [ ] Відкрийте панель дашбоард `Main` зробіть клік лівою кнопкою миші по графіку на панелі типу Time series .
- [ ] Натисніть `Add annotation`.
- [ ] В поле description напишіть `test annotation`.
- [ ] Натисніть `Save`.
- [ ] Зверніть увагу на результат - відобразиться вертикальна пунктирна лінія. При наведенні мишки на нижню частину буде відображена анотація.

![](7_1media/12.gif)

Запис.11. Ручне створення анотацій

#### 5.2. Створення автоматичних анотацій з прив'язкою до подій з журналу подій

- [ ] Відкрийте налаштування дашбоарду `Main` (шестерня в верхньому правому кутку дашбоарду).
- [ ] У лівому меню перейдіть до пункту `Annotations`.
- [ ] Натисніть кнопку `Add annotation query`.
- [ ] Налаштуйте анотацію наступним чином:

- Name: `Event`
- Data source: `InfluxDB`
- Enabled: активуйте опцію
- Color: по бажанню
- Query:

```js
from(bucket: "firstbucket")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "Building_log")
  |> filter(fn: (r) => r["device"] =~ /^${device:regex}$/)
  |> filter(fn: (r) => r["_field"] == "message")
```

- [ ] Натисніть `Save dashboard`
- [ ] Поверніться до перегляду дашбоарду `Main`.
- [ ] Оновіть дашбоард.
- [ ] Згенеруйте події шляхом увімкнення / вимкнення деяких кондиціонерів за допомогою клієнта OPC UA.
- [ ] Поверніться до перегляду дашбоарду `Main`.
- [ ] Зверніть увагу на залежність відображуваних анотацій від активованих змінних дашбоарду. Поясніть це.

### 6.Означення стартового дашбоарду

- [ ] Натисніть зірочку біля назви дашбоарду `Main`.

![](7_1media/25.png)

Рис.22. Додавання дашбоарду до улюблених

Таким чином дашбоард додано до улюблених.

- [ ] Наведіть курсор на іконку Акаунта в правому верхньому кутку і виберіть `Profile`



![image-20231121115028713](7_1media/image-20231121115028713.png)

- [ ] Перейдіть до пункту `Preferences`.
- [ ] Змініть поле **Home dashboard** на `Lab7/ Main`.
- [ ] Натисніть `Save`.

![image-20231121115214836](7_1media/image-20231121115214836.png)

Рис.23. Налаштування акаунта

- [ ] Перевірте дане налаштування, перейшовши на сторінку `Home` Grafana. (Бокове меню Dashboards -> Home)

### 7.Повноекранний режим

- [ ] Для переходу в режим повноекранного переглядання дашбоарду необхідно:
-  Сховати панель пошуку



![image-20231121115508536](7_1media/image-20231121115508536.png)

- Натиснути `Enable kiosk mode`

![image-20231121120718016](7_1media/image-20231121120718016.png)

Рис.24. Перехід в повноекранний режим

- [ ] Щоб вийти з повноекранного режиму, натисніть `ESC`.
