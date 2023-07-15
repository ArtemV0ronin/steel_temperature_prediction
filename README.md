# Предсказание температуры стали на этапе обработки

## Описание и цель проекта
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
![top-10]([http://url/to/img.png](https://github.com/ArtemV0ronin/steel_temperature_prediction/blob/main/Важность%20хар-к%20топ-10.jpg)https://github.com/ArtemV0ronin/steel_temperature_prediction/blob/main/Важность%20хар-к%20топ-10.jpg)
