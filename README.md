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
  'team_id': 2244697,  # id команды
  'probability': 0.6,  # вероятность победы указанной команды в матче
}
```

Пример ответа:

### Примеры использования API

#### Python

```python
import requests

resp = requests.get('http://alchemist.dotascience.com/api/matches', headers={'Key': })

```

#### R

TBD
