# memory-repository
homework
import json
import requests
from pydantic import BaseModel

print('Введите название страны или города')
strana = input()
base_url = 'https://api.openweathermap.org/data/2.5/'

class Pogoda(BaseModel):
    desc: str
    temp: str
    temp_min: str
    temp_max: str


class URL(BaseModel):
    params: dict = {'q': strana, 'appid': '3e0d3c82a44e6e9610778370cd8c75d4', 'lang': 'ru', 'units': 'metric'}

url = URL()


r = requests.get(base_url + 'weather', params=url.params)
if r.status_code == 200:
    a = json.loads(r.text)
    am = a['main']
    aw = a['weather']
    aw = aw[0]
    aw_desc = 'погода: '+aw['description']
    am_temp = 'Температура: '+str(am['temp'])
    am_temp_min = 'Минимальная температура: '+str(am['temp_min'])
    am_temp_max = 'Максимальная температура: '+str(am['temp_max'])
    result = {
        'desc': aw_desc,
        'temp': am_temp,
        'temp_min': am_temp_min,
        'temp_max': am_temp_max,
    }
    pogoda = Pogoda(**result)
    print('Сейчас в '+strana)
    print(pogoda.desc)
    print(pogoda.temp)
    print(pogoda.temp_min)
    print(pogoda.temp_max)
else:
    print('Error', r.status_code)