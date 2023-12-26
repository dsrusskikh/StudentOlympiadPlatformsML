# StudentOlympiadPlatforms

- В репозитории отсутствует файл **registrationReport-season-89.csv**, так как мне показалось, что по нему есть шанс вычислить определенного участника прошлого сезона олимпиады.
- Рекомендуется запускать чанки после заголовка "Задача 1", для этого добавлен импорт файла **data_platforms.csv**

**Проблема**: для повышения явки на очные площадки проведения заключительного этапа олимпиады необходимо оптимизировать ресурсы вузов-организаторов для повышения явки участников на соревнования.  
  
**Цель**: дать рекомендации по оптимизации распределения участников студенческой олимпиады по площадкам в рамках заключительного этапа нового сезона на основе анализа данных прошлого сезона.  
  
**Задачи**:  
1.1 Определить регионы с наибольшим и с наименьшим количеством участников в прошлом сезоне и учесть их при выборе места для проведения олимпиады по каждому направлению.  
1.2 Сделать визуализацию с распределением популярности площадок по карте России для каждого направления.  
2. Создать модель, которая предскажет явку при сохранении текущих площадок в новом сезоне.  
3.1 Создать модель, предсказывающую вероятность явки участника на площадку с учётом того, что площадка находится в регионе, отличном от региона местонахождения его вуза.  
3.2 Вычислить вероятность того, что участник приедет на площадку не из своего региона.  

##  Задача 1
Сразу проговорю два ключевых момента в преобразовании исходных данных:  
1.	Я объединил все направления в укрупнённые, так как какие-то дисциплины из прошлого сезона пропали, какие-то появились в новом. Кроме того, студенты нередко участвуют сразу в нескольких направлениях, которые часто находятся в одной укрупнённой группе. Такие допущения позволили создать более качественные и универсальные модели.  
2.	Я создал столбец Явка из столбца с баллами за 3 этап. Проще говоря, если участник получил какой-либо балл на этом этапе, то он на нём присутствовал.  

В качестве первого пункта было решено посмотреть на итоговую ситуацию по площадкам в прошлом сезоне. Сперва оценим явку по количеству людей, пришедших на них.  

<img width="461" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/b9e60780-0c83-4afa-9b23-c2acc6786922">  

Явка на некоторые площадки в более доступных регионах находится на крайне низком уровне (меньше 10 человек со всех направлений (например, Владикавказ, Киров, Омск, Калининград), что впустую расходует ресурсы, которые можно было бы перенаправить на пустующие регионы. Далее посмотрим уже на чуть более полезную оценку – процент участников, которые были на площадке от общего числа участников, зарегистрированных на конкретное направление и площадку.  

<img width="473" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/96f43fc8-0a75-4b2a-9e28-ee2dff8ca8da">  

 Как видно, ситуация здесь уже чуть более позитивная, что повлияло на выводы по первому графику. Хотя Киров и Калининград сохранили низкую явку, Омск и Владикавказ оказались довольно востребованными.  
Далее было интересно посмотреть на распределение площадок по стране, для каждого направления была создана карта России с площадками на ней, здесь данные представлены без учёта явки, только общее количество участников для каждой площадки, которое отражено в размере точки для площадки на диаграмме.  

<img width="452" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/0a101854-2cdc-4ca9-b7bb-664e3cd9554b">
<img width="452" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/4e77c4d8-0839-48a9-8533-35bd42636e3d">
<img width="452" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/822c73de-d2bf-4300-b7f6-e7153adcb93b">
<img width="452" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/51b63cf8-e601-4069-8f3b-0c99b8b97edc">
<img width="452" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/b284ef1e-ba7b-44d6-8ba7-207450867da6">
<img width="452" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/ffa8c6d7-b8c8-4f45-a629-dc4b6805dcf7">

Среди площадок слабо представлены те, что находятся в азиатской части страны. Это достаточно аргументированно объясняется тем, что в этой части страны просто нет достаточного количества вузов.  

## Задача 2
Задача заключалось в том, чтобы создать модель машинного обучения, которая могла бы на данных нового сезона, предсказать явку на площадки, если они останутся теми же, что были в прошлом сезоне. Я рассматривал сразу три разных алгоритма – логистическая регрессия, дерево решений и случайный лес. После перебора различных переменных (факторов, которые могут влиять на явку) и гиперпараметров (настройки алгоритмов) я остановился на модели случайного леса, которая предсказывает явку (придет конкретный участник или нет) по дисциплине его участия, городу его университета и городу площадки. Качество модели можно описать через различные метрики, подробно остановлюсь на каждой из них.  

<img width="281" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/77195ad0-c23c-4dde-8ca3-f656fbd1fb82">  

1. Accuracy: 65.22% — Это общий процент правильно классифицированных случаев. Точность в 65.22% означает, что около двух третей предсказаний модели верны.  
2. Precision: 58.81% - Показывает, какой процент предсказанных положительных случаев действительно положительный. В контексте нашей задачи это означает, что когда модель предсказывает появление участника на площадке, она права примерно в 58.81% случаев.
3. Recall: 40.89% — Это мера того, какой процент реальных положительных случаев был правильно идентифицирован моделью. Низкий Recall, как в вашем случае, указывает на то, что многие положительные случаи упускаются моделью, что отражено в Precision, так как Recall это его своеобразный антипод. 
4. F1-score: 48.24% — Это гармоническое среднее Precision и Recall. F1-score учитывает как ложноположительные, так и ложноотрицательные результаты. В вашем случае F1-балл ниже 50%, что указывает на не очень высокое качество модели в целом.  

Эти метрики указывают на то, что модель имеет средний уровень точности, но она довольно часто пропускает положительные случаи (низкий Recall) и имеет умеренное количество ложноположительных результатов (не слишком высокий Precision). Тем не менее для подобных задач это хорошие показатели, так как мы имеем дело с социально-экономическим вопросом, где нередко большое количество латентных факторов оказывают сильное влияние на качество предсказаний, несмотря на то что в наших данных они не отражены.  

*С помощью полученной модели можно предсказать примерную реальную нагрузку для каждой из площадок нового сезона, которые были в прошлом сезоне.*  

## Задача 3
В рамках данной задачи была получена модель, которая также предсказывает явку, но уже по направлению участия, категории участия и по дихотомической переменной приезд из другого региона (1 – регион площадки не совпадал с регионом вуза участника, 0 – регион площадки совпадала с регионом вуза участника). В этот раз я вновь воспользовался случайным лесом, только моделью бинарной классификации. По качеству получилась примерно такая же модель, как и из предыдущей задачи.  

<img width="281" alt="image" src="https://github.com/dsrusskikh/StudentOlympiadPlatforms/assets/88119213/f7fdc94e-076c-45c3-b356-33b6514f0d91">

Accuracy: 64%. Это означает, что в 64% случаев модель правильно предсказывает, приедет ли участник на площадку не из своего региона.  
ROC-AUC: 56%. Показатель ROC-AUC (Area Under the Receiver Operating Characteristic Curve) является одним из ключевых показателей для оценки качества бинарных классификаторов. Значение 56% указывает на среднюю способность модели различать два класса (явка на площадку и не приезд).  

Полученную модель можно использовать после создания распределения участников по площадкам следующим образом:  
1. На основе предсказаний модели можно оценить ожидаемое количество участников и их распределение по направлениям и площадкам. Это поможет в организации необходимых ресурсов, таких как места проведения, оборудование, материалы, персонал.  
2. Используя предсказания модели, можно разработать более целевые стратегии взаимодействия с участниками, например, отправлять напоминания или мотивационные сообщения тем, кто, по мнению модели, менее склонен прийти.  
3. Модель может помочь идентифицировать направления или категории, где ожидается низкая явка, что позволит заранее разработать стратегии по увеличению вовлеченности или перераспределению ресурсов.  


В качестве небольшого дополнения к последней задачи я рассчитал вероятность явки участника на площадку, если та находится в регионе, отличном от региона его вуза, без учёта каких-либо факторов. Получилось, что *вероятность того, что участник приедет на площадку не из своего региона равна 31.53 %*.









