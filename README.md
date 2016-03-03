Dota Science Challenge Hackathon
================================

Хакатон по предсказанию результата матчей Dota 2 в турнире Shanghai Major 2016.

### Как участвовать?

- Заполните [форму регистрации](http://goo.gl/forms/uItP82XCOQ), мы вышлем вам API-ключ, по которому можно будет отправлять предсказания.
- Если вы уже получили ключ — читайте описание API далее.
- [Лидерборд предсказаний](http://alchemist.dotascience.com/leaderboard/)
- [Таблица матчей](http://alchemist.dotascience.com/matches/)
- [Схема турнира Shanghai Major 2016](http://wiki.teamliquid.net/dota2/Shanghai_Major/2016#Main_Event)
- Начинать отправлять предсказания можно уже сейчас

### Поставновка задачи и качество предсказания

### API Для отправки предсказаний

Взаимодействие с системой сбора предсказаний происходит через HTTP API, почти все функции API требуют ключ. Ключ выдается индивидуально каждому участнику (или команде). Ключ необходимо указать в запросе в виде HTTP-заголовка `Key: <your-personal-key>`, либо в качестве параметра URL: `http://...?key=<your-personal-key>`.

#### Список матчей

Запрос: `GET http://alchemist.dotascience.com/api/matches`

Пример ответа:
```python
{
  "matches": [
    {
      "id": 13,
      "status": 2,  # Состояние матча
                    # 0 - Аннонсирован
                    # 1 - Идет в данный момент
                    # 2 - Завершен (известен победитель)
      
      "cell": 4,           # Положение матча в турнирной таблице
      "order_in_cell": 1,  # Номер игры, в случае если команды 
                           # играют несколько матчей (best-of-3)
      
      "participants": [   # Команды-участники
        {
          "team_id": 4, 
          "team_name": "EHOME"
        }, 
        {
          "team_id": 2244697, 
          "team_name": "Team Archon"
        }
      ],
      
      "steam_match_id": 2191757537,   # match_id в Steam API 
                                      # (известен после начала матча)
    }, 
    ...
  ]
}
```

#### Отправка предсказания

Запрос: `POST http://alchemist.dotascience.com/api/match/<id>/prediction`

Пример содержимого запроса:
```python
{
  "team_id": 2244697,  # id команды
  "probability": 0.6   # вероятность победы указанной команды в матче
}
```

Пример ответа:
```python
{
  "ok": true,
  "time": 1457003701.0  # unixtime полученного предсказания
}
```

#### Список отправленных предсказаний

Запрос: `GET http://alchemist.dotascience.com/api/match/<id>/prediction`

Пример ответа:
```pytohn
{
  "account_name": "peter",
  "match_id": 13, 
  "predictions": [
    {
      "team_id": 2244697, 
      "probability": 0.6, 
      "time": 1457003701.0
    },
    ...
  ]
}
```

#### Качество предсказаний команд (лидерборд)

Запрос: `GET http://alchemist.dotascience.com/api/leaderboard`

Пример ответа:
```python
{
  "update_time": 1457003923.0,   # время последнего обновления таблицы результатов
  "leaderboard": [
    {
      "account_id": 2, 
      "account_name": "avsirotkin",     # аккаунт участника

      "predicted_matches": 6,   # матчей, для которых отправлено предсказание
      "predictions": 15,        # всего отправлено предсказаний по всем матчам
      
      "score_max": 0.3713518765363535,  # максимальный скор среди всех матчей
      "score_sum": 0.15635789186437565, # суммарный ...
      "score_avg": 0.02605964864406261, # средний по всем предсказанным матчам ...


      "match_scores": [          # результат предсказания отдельно по каждому матчу 
        {
          "match_id": 7, 
          "num_predictions": 5,                     # число предсказаний по матчу
          "score": 0.3713518765363535,              # итоговый скор
          "last_prediction_time": 1456966298.0,     # время последнего учтенного предсказания
          "winner_probability": 0.646782198327222   # последняя (до начала матча) предсказанная вероятность 
                                                    # победы команды, которая в итоге победила
        },
        ...
      ]
    }
```

### Примеры работы с API

#### Python

Удобно использовать библиотеку [`requests`](http://docs.python-requests.org/en/master/).

```python
>>> import requests
>>> api_key = 'BC5141CAA...518F53'

# получение списка матчей
>>> resp = requests.get('http://alchemist.dotascience.com/api/matches', headers={'Key': api_key})
>>> resp.json()
{'matches': [...]}

# отправка предсказания
>>> resp = requests.post('http://alchemist.dotascience.com/api/match/13/prediction', 
...                     json={'team_id': 2244697, 'probability': 0.6}, headers={'Key': api_key})
>>> resp.json()
{'ok': True, 'time': 1457003701.0}
```

#### R

TBD
