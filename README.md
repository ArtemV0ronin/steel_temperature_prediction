# Предсказание температуры стали на этапе обработки
Ноутбук проекта находится в файле [15(f)_steel_temperature_prediction.ipynb](https://github.com/ArtemV0ronin/steel_temperature_prediction/blob/main/15(f)_steel_temperature_prediction.ipynb)

## Задача проекта
Металлургический комбинат ООО «Так закаляем сталь» решил уменьшить потребление электроэнергии на этапе обработки стали, чтобы оптимизировать производственные расходы. Необходимо построить модель машинного обучения для предсказания температуры стали с целевой метрикой **MAE** не более **6,8**.

## Описание данных
Данные состоят из файлов, полученных из разных источников:
- `data_arc_new.csv` — данные об электродах;
- `data_bulk_new.csv` — данные о подаче сыпучих материалов (объём);
- `data_bulk_time_new.csv` *—* данные о подаче сыпучих материалов (время);
- `data_gas_new.csv` — данные о продувке сплава газом;
- `data_wire_new.csv` — данные о проволочных материалах (объём);
- `data_wire_time_new.csv` — данные о проволочных материалах (время);
- `data_temp_new.csv` — **результаты измерения температуры (целевой признак)**.

Во всех файлах столбец `key` содержит номер партии. В файлах может быть несколько строк с одинаковым значением `key`: они соответствуют разным итерациям обработки.

## Используемый стек
![Static Badge](https://img.shields.io/badge/sklearn-red)
![Static Badge](https://img.shields.io/badge/LinearRegression-red)
![Static Badge](https://img.shields.io/badge/RandomForestRegressor-red)
![Static Badge](https://img.shields.io/badge/CatBoostRegressor-red)
![Static Badge](https://img.shields.io/badge/LGBMRegressor-red)
![Static Badge](https://img.shields.io/badge/KNN-red)
![Static Badge](https://img.shields.io/badge/GridSearchCV-red)
![Static Badge](https://img.shields.io/badge/StandardScaler-red)
![Static Badge](https://img.shields.io/badge/pandas-red)
![Static Badge](https://img.shields.io/badge/numpy-red)
![Static Badge](https://img.shields.io/badge/matplotlib-red)
![Static Badge](https://img.shields.io/badge/seaborn-red)
![Static Badge](https://img.shields.io/badge/phik-red)
![Static Badge](https://img.shields.io/badge/tqdm-red)
![Static Badge](https://img.shields.io/badge/time-red)

---

## Итоги проекта
В ходе работы над проектом была разработана модель машинного обучения на базе модели **LightGBM**, прогнозирующая температуру стали.

Метрика **MAE** модели на тестовой выборке = **6.041.** При этом метрика константной модели по медиане = 8.7.

Гиперпараметры итоговой модели:

<table>
<tr>
  <th>Параметр</th>
  <th>Значение</th>
</tr>
<tr>
  <td>boosting_type</td>
  <td>gbdt</td>
</tr>    
<tr>
  <td>learning_rate</td>
  <td>0.26</td>
</tr>   
<tr>
  <td>max_depth</td>
  <td>2</td>
</tr> 
<tr>
  <td>n_estimators</td>
  <td>108</td>
</tr>
<tr>
  <td>random_state</td>
  <td>80523</td>
</tr>
</table>

ТОП-10 важных характеристик модели и доля их важности в %:

![top-10](https://github.com/ArtemV0ronin/steel_temperature_prediction/blob/main/media/Важность%20хар-к%20топ-10.jpg)

## Отчет о выполнении проекта

1 В ходе Исследовательского анализа и предобработки данных выполнено:

- пропуски удалены или заменены на 0, т.к. замеры не производились;
- аномалии обработаны;
- объекты проверены на явные дубликаты;
- ключи по которым есть только 1 запись удалены;
- оставлены только уникальные значения ключей;
- ключи установлены в роли индексов;
- данные сагрегированы по ключам;
- выделен целевой признак - конечная температура `time_end`;
- выделены дополнительные признаки: 
    - начальная температура `temp_start`;
    - разница между первым и последним замерами температуры `time_delta`;
    - полная мощность `full_power`;
    - энергия `energy`.

2 В ходе Подготовки к обучению моделей выполнена важная работа по подготовке тренировочных данных:

- все датасеты объеденены по пересечению ключей;
- выделены тестовая и тренировочная выборки в соотношении 25/75;
- итоговые объедененные данные предобработаны:
    - удалены столбцы с нулевыми значениями;
    - удалены коррелирующие (в том числе и нелинейно) признаки;
    - удалены аномалии по методу KNN.

3 В ходе Обучения моделей были обучены 3 различных вида моделей:

- простая линейная регрессия LinearRegression;
- "деревянная" модель RandomForestRegressor;
- 2 модели градиентного бустинга - CatBoostRegressor и LGBMRegressor.

  Перебор гиперпараметров осуществлялся автоматически с помощью GreadSearchCV на кросс-валидации.

4 Рекомендации по улучшению проекта:

1) Увеличить кол-во данных.   
Для улучшения качества модели,  после объединения датасетов, необходимо больше данных.   

2) Уточнить о заполнении пропусков.   
В датасетах по добавкам сыпучих материалов и проволоки имелось очень большое кол-во пропусков. Необходимо уточнить, действительно ли пропуск означает отсутствие добавок.  

3) Провести более детальный анализ важности признаков.  
Получив в п.6 график важности характеристик, методом перебора были исключены все малозначимые хар-ки. Но метрика качества при этом не улучшалась. Т.к. найденная модель более чем удовлетворяла условиям задачи, то дальнейший анализ не проводился. Необходимо провести аналзи важности признаков и по другим моделям. На основании результатов попробовать исключать малозначимые признаки.

4) Попробовать другие модели. Например XGBoost.

