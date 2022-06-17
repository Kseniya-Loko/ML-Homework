# ML-Homework

Задание №2.
Выполнялось индивидуально.
Локонцева Ксения.

В качестве анализа я выбрала 2 различных корпуса текстов - это тексты архива газеты «Медицинская газета» с 2006 по 2022 года и подборку текстов из Telegram каналов о медицине, которые были получены в json-формате. 

![image](https://user-images.githubusercontent.com/90756002/174292756-8f586d0e-c567-4e68-8b75-484f900891fc.png)

Рис. 1. Результат распределения по количеству тегов разных категорий в тексте «Медицинской газеты».

![image](https://user-images.githubusercontent.com/90756002/174292835-3d1a75c6-6cf6-4333-adaf-1840a1c1d88f.png)

Рис. 2. Результат распределения количества тегов разных категорий в тексте Telegram-каналов о медицине.

На этих данных можно отследить следующую закономерность: в тексте «Медицинской газеты» в большей степени присутствуют названия организаций, локации и даты. И в отличие от текстов из Telegram, мы видим появление категории money - то есть, упоминания сумм денег. 
В Telegram в большом количестве присутствуют числа, имена и также даты. Но при этом есть отсутствующая в текстах медицинской газеты категория «work_of_art», куда вошли такие словосочетания как «ГМО» и «Тяжелобольной дома». 

В тексте Telegram встречаются ошибки распознавания тега сущности, такие как «Гугл» как person (org), «Спойлер» как person, «Тамифлю» как person (product), «Доширака» как org (product), «Кстати» как person.
Стилистика результатов выдачи на текстах «Медицинской газеты» отличается более конкретным содержанием - дано полное название огранизаций, более конкретно указан временной период, стиль написания более формальный.

Выводы работы модели на двух данных текстах следующая - модель хорошо идентифицирует такие именованные сущности как person, org, gpe, date, money, ordinal и cardinal, но гораздо хуже идентифицирует такие категории как work_of_art. Часто аббревиатуры также попадают в несвойственные им категории, например, вместо opg в person или product. 

К проблемам работы с моделью относится то, что происходит перегрузка ОЗУ в облачном пространстве Google Colab, если используются слишком большие объемы данных (20 Мб текста и 57 Мб текста). В результате чего текст анализируется не целиком, а только его часть. Данную ошибку я не совсем поняла, как решить, в данном случае я выбирала в качестве разделителя sep=”!”, который позволял выделить в тексте блоки и токенизировать их (данных блоков в корпусе «Медицинской газеты» получилось более 4000. Также в случае работы с корпусом «Медицинской газеты» при записи и загрузке csv-файла с таблицей именованных сущностей и тегов к ним, я получила пустые колонки с тегами. 
Возможно, для корректной работы кода требуется предобработка текста (например, разделение его на абзацы через перевод каретки) и решение по оптимизации, которое бы могло позволить обработать очень большой объем текста.

В случае с вопросно-ответной моделью, ни в первом, ни во втором варианте текста он не выдал значимых результатов, а только пустые колонки в графе relation.

Далее я решила попробовать сделать то же самое, но на примере литературного текста Александра Булгакова «Собачье сердце» - его объем 5811 абзацев.
В данном случае, я не делала парсинг, а просто взяла текст в формате .txt для проверки работы модели QA. 
С данным текстом модель выдала результаты гораздо более соотносимые с тегами, однако вопрос-ответная модель показала не очень релевантные результаты, которые изредка соотносили person с существительным, например, «Преображенский - профессор», но чаще находилась корреляция с какими-то действиями или характеристиками персоны, например «Швондер - растерянно взял трубку и молвил». 
Определение именованных сущностей сработало, однако в данном случае тоже часто появлялись ошибки определения сущности, например, слова с большой буквы в начале предложения иногда определялись как имя, например Июнь - person, Юбчонка - как person. Кроме того, встречались имена в формате ФИО, которые не определялись совсем, например - С.С. Заяицкий (579 строка в таблице из архива results.zip NERQA_dog_heart_person.csv и далее в этом же предложении). 

![image](https://user-images.githubusercontent.com/90756002/174292929-a10d302e-9dab-4533-9ae9-c017030fc55e.png)

Рис. 3. Распределение NER - сущностей в тексте Михаила Булгакова «Собачье сердце»

Художественный текст по сравнению с текстами из СМИ значительно перевешивает по количеству сущностей с тегом person. Второе и третье место занимают теги, относящиеся к времени и месту.
