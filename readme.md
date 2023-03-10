Базовые функции для работы с данными
Библиотека pandas может читать многие форматы, включая: .csv,
.xslx, .xls, .txt, sql и многие другие. Полный список по ссылке
Чтобы подключить библиотеку к Вашей программе необходимо
написать следующее:
import pandas as pd
Напоминание: as(alias) - псевдоним. Мы можем сократить название
все библиотеки до 2-х букв.
Прочтем файл .csv(он находится в Google Colab в папке sample_data) с 

# Базовые функции

df.head()  -- вывод основной функции
df.tail()    -- вывод последних элементов n = ...  задаем искомое количество элементов
df.shape   -- дает возможность узнать размер нашей таблицы (количество строк и столбцов)
df.isnull()  -- поиск нулевых значений
df.isnull().sum() -- сложение всех элементов в нашей таблице для поиска нулевых значений
df.dtypes  -- посмотреть тип данных строки
df.columns  -- посмотреть тип данных столбца

# Выборка данных:
df['latitude'] -- выборка данных из конкретного столбца
df[['latitude',population]] -- выборка данных из нескольких столбцов

## Задача :
Необходимо вывести столбец total_rooms у которого медианный возраст здания(housing_median_age) меньше 20

df[df['housing_median_age'] < 20 ]

df[df['housing_median_age'] < 20 ] ['total_rooms']      -- выбор определенного столбца


## & - выполнение всех условий

## | - выполнение хотя бы одного условия

df[(df['housing_median_age'] < 20)  & (df['housing_median_age'] > 10) ] ['total_rooms']


# Простая статистика

print(df['population'].max()) -- вывод максимального значения из столбца
print(df['population'].min()) -- вывод минимального значения из столбца
print(df['population'].mean()) -- вывод среднего значения
print(df['population'].sum()) -- вывод суммы всех значений
print(df['population'].median()) --

df.describe() -- вывод всех функций , описанных выше


# Изображаем статистические отношения

Scatterplot(точечный график) Математическая диаграмма,изображающая значения двух переменных в виде точек на декартовой плоскости.Библиотека seaborn без труда принимает pandas DataFrame(таблицу).Чтобы изобразить отношения между двумя столбцами достаточно указать ,какой столбец отобразитьт по оси x ,а какой по оси y

[] 1 import seaborn as sns

Изображение точек долготы по отношению к широте.

[] 1 sns.scatterplot(data = df, x = "longitude", y = "latitude")

Отношение,чем выше количество семей,тем выше количество людей и соответсвенно комнат.


[] 1 sns.scatterplot(data = df, x = "households", y = "population", hue = " total rooms")

1 sns.scatterplot(data = df, x = "households", y = "population", hue = " total rooms", size = 10)

## Мы можем визуализировать сразу несколько отношений,используя класс PairGird внутри seaborn . PairGird принимает как аргумент pandas DataFrame  и визуализирует все возможные отношения между ними,в соответствии с выбранным типом графика :

cols = ['population','median_income','housing_median_age','median_hose_value']
g = sns.PairGird(df[cols])
g.map(sns.scatterplot)


# Линейные графики

Хорошо подойдут, если есть временная или какая-либо иная последовательность и значения, которые могут
меняться в зависимости от нее. Для генерации линейных графиков в seaborn используется relplot функцию. Она
также принимает DataFrame, x, y - столбцы.
Для визуализации выбирается тип line:
sns.relplot(x="latitude", y="median_house_value", kind="line", data=df)

Можно видеть, что в определенных местах долготы цена за дома резко подскакивает.
Попробуем визуализировать longitude по отношения к median_house_value и поймем в чем же дело, почему
цена так резко подскакивает.
Пример:
sns.relplot(x = 'longitude', y = 'median_house_value', kind = 'line', data = df)

Можно видеть, что в определенных местах широты цена за дома также очень высока.
Используя точечный график можно визуализировать эти отношения с большей четкостью. Скорее всего резкий
рост цен связан с близостью к ценному объекту, повышающему качество жизни, скорее всего побережью
океана или реки.
Пример:
sns.scatterplot(data=df, x="latitude", y="longitude", hue="median_house_value")


# Гистограмма:

Способ представления табличных данных в графическом виде — в виде столбчатой диаграммы. По оси x обычно
указывают значение, а по оси y - встречаемость(кол-во таких значений в выборке).
Пример:
sns.histplot(data=df, x="median_income")

Можно видеть что у большинства семей доход находится между значениями 2 и 6. И только очень небольшое
количество людей обладают доходом > 10.
Изобразим гистограмму по housing_median_age.
Пример:
sns.histplot(data = df, x = 'housing_median_age')

Распределение по возрасту более равномерное. Большую часть жителей составляют люди в возрасте от 20 до
40 лет. Но и молодежи не мало. Также очень много пожилых людей > 50 лет медианный возраст.
Давайте посмотрим медианный доход у пожилых жителей.
Пример:
sns.histplot(data=df[df['housing_median_age']>50], x="median_income")

Давайте разобьем возрастные группы на 3 категории те кто моложе 20 лет, от 20 до 50 и от 50, чтобы посмотреть
влияет ли это на доход.
Пример:
df.loc[df['housing_median_age'] <= 20, 'age_group'] = 'Молодые'
df.loc[(df['housing_median_age'] > 20) & (df['housing_median_age'] <= 50), 'age_group'] = 'Ср. возраст'
df.loc[df['housing_median_age'] > 50, 'age_group'] = 'Пожилые'
Что в этом случае происходит внутри таблицы? Добавился новый столбец age_group, в котором будет указана


Применим group_by, чтобы получить среднее значение.
df.groupby('age_group')['median_income'].mean().plot(kind='bar')

Seaborn так же позволяет нам смотреть распределение по многим параметрам. Давайте поделим группы по доходам
на 2. Те у кого медианный доход выше 6 и те у кого меньше. Изобразим дополнительное измерение с помощью
оттенка в виде возрастных групп и групп по доходам.
df.loc[df['median_income'] > 6, 'income_group'] = 'rich'
df.loc[df['median_income'] < 6, 'income_group'] = 'everyone_else'

