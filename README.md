# Требования к входным данным:

В базе данных должны присутствовать следующие таблицы:

**Для домов:**
- buildings
- functional_objects
- physical_objects

**Для населения:**
- administrative_units
- municipalities
- age_sex_administrative_units
- age_sex_municipalities
- age_sex_social_administrative_units

Для прогноза изменения населения используется файлы из scripts/input_data  
там информация о численности населения в городе (Питер) по возрастам.

Все данные по населению за 2019г.  

# Алгоритм обработки данных:

**1. Прогноз изменения численности населения по возрастам до 2030г. на основе данных за предыдущие года.**
- **1.1.** Данные неполные и с пропусками, поэтому также восстанавливаются пропуски и пробелы в данных.
- **1.2.** При заданном годе прогнозирования (>2019) данные о численности берутся из прогноза на этот год.
- **1.3.** При заданной суммарной численности данные прогноза корректируются под суммарную заданную численность.
- **1.4.** Данные по населению по возрастам корректируются в соответствии с прогнозной численностью.

**2. Расчет данных по соц.группам по возрастам для муниципалитетов**
- **2.1.** На основании % людей в муниципалитете относительно административного округа и % людей в соц. группе в административном округе
           получаем оценку численности соц. групп в муниципальном округе.

**3. Расчет населения по домам**
- **3.1.** Расчет максимальной и вероятной численности населения в домах относительно населения в городе  
           (по округам и муниципалитетам).
- **3.1.1.** Используемые параметры:  
             - Минимальное число квадратных метров на человека: 9;  
             - Точность балансировки: 1.
- **3.2.** Расчет соц. групп по домам (суммарно по возрастам).
- **3.3.** Расчет соц. групп по домам по возрастам.

# CLI:

`-h` -> help, вывод описания взаимодействия с файлом и работы с командами.  

###### Подключение к БД:  

`--db-addr` - адрес обращения. По умолчанию: '172.17.0.1'.  
`--db-port` - порт. По умолчанию: 5432.  
`--db-name` - название базы данных. По умолчанию: 'city_db_final'.  
`--db-user` - имя пользователя. По умолчанию: 'postgres'.  
`--db-pass` - пароль, также задано значение по умолчанию.  

###### Аргументы:  

`--year` - год, на который нужно спрогнозировать изменение населения. По умолчанию: 2022.  
`--city-id` - id города, для которого нужно сделать расчет. По умолчанию: 1 (Санкт-Петербург).  
`--set-population` - задать суммарное значеие численности населения в городе (например, 10000000). По умолчанию не задано.  
