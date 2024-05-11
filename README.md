# Тестовое задание для отбора на Летнюю ИТ-школу КРОК по разработке

## Условие задания
Будучи тимлидом команды разработки, вы получили от менеджера проекта задачу повысить скорость разработки. Звучит, как начало плохого анекдота, но, тем не менее, решение вам все же нужно найти. В ходе размышлений и изучений различного внешнего опыта других команд разработки вы решили попробовать инструменты геймификации. То есть применить техники и подходы игрового характера с целью повышения вовлеченности команды в решение задач.

Вами была придумана рейтинговая таблица самых активных контрибьютеров за спринт. Что это значит в теории: по окончании итерации (4 рабочие недели) выгружается список коммитов, сделанных в релизную ветку продукта, и на его основе вычисляются трое самых активных разработчиков, сделавших наибольшее количество коммитов. В зависимости от занятого места, разработчик получает определенное количество внутренней валюты вашей компании, которую он впоследствии может обменять на какие-то товары из внутреннего магазина.

На практике вы видите решение следующим образом: на следующий день после окончания спринта в 00:00 запускается автоматическая процедура, которая забирает файл с данными о коммитах в релизную ветку, сделанных в период спринта, после чего выполняется поиск 3-х самых активных контрибьютеров. Имена найденных разработчиков записываются в файл, который впоследствии отправляется вам на почту.

В рамках практической реализации данной задачи вам необходимо разработать процедуру формирование отчета “Топ-3 контрибьютера”. Данная процедура принимает на вход текстовый файл (commits.txt), содержащий данные о коммитах (построчно). Каждая строка содержит сведения о коммите в релизную ветку в формате: “_<Имя пользователя> <Сокращенный хэш коммита> <Дата и время коммита>_”.
Например: AIvanov 25ec001 2024-04-24T13:56:39.492

К данным предъявляются следующие требования:
- имя пользователя может содержать латинские символы в любом регистре, цифры (но не начинаться с них), а также символ "_";
- сокращенный хэш коммита представляет из себя строку в нижнем регистре, состояющую из 7 символов: букв латинского алфавита, а также цифр;
- дата и время коммита в формате YYYY-MM-ddTHH:mm:ss.

В результате работы процедура формирует новый файл (result.txt), содержащий информацию об именах 3-х самых активных пользователей по одному в каждой строке в порядке убывания места в рейтинге. Пример содержимого файла:
AIvanov
AKalinina
CodeKiller777

Ручной ввод пути к файлу (через консоль, через правку переменной в коде и т.д.) недопустим. Необходимость любых ручных действий с файлами в процессе работы программы будут обнулять решение.

## Автор решения
Иванов Иван Владимирович
## Описание реализации
Сначала мы объявляем дата класс *Constants*, где будут храниться все наши константы.
Затем создаем *defaultdict(set)*, где ключом будет имя пользователя, а значением set из хэшей его коммитов.
Структуру set выбранна из-за того, что она решает проблему с возможными выбросами в данных такие как повторяющиеся коммиты.
Т.е. если у одного и того же пользователя встретиться 2 одинаковых хэша, то мы посчитаем его только один раз.
Далее идет проверка строки на совпадение с регулярным выражением, которое описывает условия:
- имя пользователя может содержать латинские символы в любом регистре, цифры (но не начинаться с них), а также символ "_";
- сокращенный хэш коммита представляет из себя строку в нижнем регистре, состояющую из 7 символов: букв латинского алфавита, а также цифр;
- дата и время коммита в формате YYYY-MM-ddTHH:mm:ss.

Дата и время проверяется только на то, что на месте цифр стоят цифры.
Следующая проверка - это проверка даты. Программа считывает текущее время и проверяет, что дата соответствует формату 'YYYY-MM-ddTHH:mm:ss' и что она была лежит в интервале [начало спринта; время запуска программы(начало спринта + 28 дней)].
Если все проверки пройдены, то по ключу имени добавляется хэш.
В случае, когда строка не прошла одну из проверок, то это записывается как warning в файл log.txt.
После того как мы считали все коммиты, то находим 3 максимальных элемента за O(N) по времени и O(1) по памяти. Записываем результаты в файл *resul.txt*. 
## Инструкция по сборке и запуску решения
Если не установлен python3, то:

```
sudo apt install python3
```

Запуск надо производить из папки, где лежит *commits.txt*!!!

Если файл *commits.txt* лежит в папке с main.py, то запуск происходит так:

```
pytnon3 main.py
```

Если файл main.py лежит в другой папке, то запуск будет происходить так:
```
python3 {путь к main.py}
```
