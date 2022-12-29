# Задача: http rest api сервер
На сервере должны быть запущены:
nginx - как балансировщик
+
2 процесса api_server.py на питон
api_server должен принимать http запросы двух видов:
1. Отправка данных на сервер. Http метод POST, Формат JSON вида {"lat": 34.2423, "lng": -
100.2344, "temperature": 40, "time": "2022-04-20T10:01:01Z"}
api_server проверяет формат входящих данных и корректность величин. Значение time не 
должно опережать текущее время больше чем на минуту.
Если формат или величины не корректны то сервер возвращает http статус 400 и JSON с 
описанием ошибки вида {"status": 400, "message": <error description>}
Если все корректно то сервер сохраняет запись в локальной базе данных PostgreSQL или 
SQLite и возвращает http статус 200
2. Запрос всех точек с сервера не дальше определенного радиуса вокруг заданной точки. Http 
метод GET. Формат вида ?lat=30.27623&lng=50.2378&radius=200.5
radius в километрах не должен быть больше 500 км, землю можно считать круглой. Если в 
запросе радиус больше 500км или значения координат не допустимы или формат запроса не 
корректен,
то сервер должен вернуть ошибку 400 описание ошибки аналогично первому запросу.
Если параметры запроса корректны сервер должен вернуть статус 200 и JSON массив со 
всеми точками из базы в заданном радиусе. Формат точки аналогичен первому запросу.
api_server должен периодически удалять из базы точки со значением времени старее 2 дней 
относительно текущего.
api_server должен вести логи используя стандартный модуль logging и записывать в лог 
данные запросов и ошибки. Лог файлы должны храниться 5 дней, затем удаляться
api_server должен уметь быстро работать с базой данных содержащей миллионы записей.
api_server должен быть асинхронным на основе gevent wsgi/flask
для работы с базой данных можно использовать модули sqlite3 или psycopg2
Результат который я ожидаю:
конфигурационный файл для nginx;
собственно код api сервера на питон;
скрипт или команда curl для отправки JSON файла с точкой на сервер;
скрипт или команда curl для получения массива точек с сервера
