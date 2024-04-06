# WebpageParser

В рамках проекта проведен `парсинг интернет-страниц`, в том числе:
- проведена работа с HTML-тегами и DOM
- использована библиотека BeautifulSoup4 (bs4)
- для представления данных выбрана библиотека pandas

## websites_parsing

С помощью метода readline() построчно обрабатывается документ websites.md (10 адресов), результат передается в переменную data. Далее из data извлекается строка (link), которая содержит ссылку на сайт.

```python
link = data.rstrip('\n')[1: -1].split('|')[2]
```

Для получения информации об интернет-странице производится get-запрос (библиотека requests) по URL-адресу, который записан в переменную link. 

```python
try:
    res = requests.get(link, headers=headers)
    return res.text
except:
    return 'RuntimeError: ошибка ссылки'
```

Результат - распечатанный html-код страницы. Пользователю предлагается выбрать, выводить информацию о следующем ресурсе, либо завершить работу

```python
print(get_html_code(link))
print(f'\nУСПЕШНО: "{name}" {link}\n')
```

_Для демонстрации в коде проекта приведен пример вывода_


## etherscan_parsing

Для подробного анализа используется ресурс https://etherscan.io.
Первый этап аналогичен вышесказанному. Далее с помощью библиотеки BeautifulSoup производится поиск информации о названии и стоимости криптовалют по тэгам.

```python
crypto = soup.find_all('div', class_='hash-tag text-truncate fw-medium')
crypto_price = soup.find_all('div', class_='d-inline')
```

После генерируются списки crypto_list и crypto_price_list, собираются в словарь. Далее - сортировка и преобразование в DataFrame.

```python
result = pd.DataFrame.from_dict(crypto_and_price_sorted, orient='index').reset_index()
result.columns = ['crypto', 'price']
```
_Для демонстрации в коде проекта приведен вывод_