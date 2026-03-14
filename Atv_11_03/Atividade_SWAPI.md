# Atividade de QA com APIs — SWAPI

**Tempo estimado:** 30–40 minutos  
**Ferramenta:** Google Colab  
**Objetivo:** explorar uma API pública, analisar respostas JSON e criar testes simples com Python.

Este notebook consulta a API pública SWAPI. Caso a internet falhe, o sistema utiliza um **modo de fallback local** com dados simulados para permitir a continuidade da atividade.

---

# Instruções da Atividade

Nesta atividade foram realizadas as seguintes tarefas:

- Consulta de endpoints da API SWAPI
- Observação do **status code**
- Análise do **JSON retornado**
- Pensamento crítico como **QA**
- Automação de validações usando **assert**

---

# Código Principal

```python
import requests
import json

FIXTURES = {
    "https://swapi.dev/api/people/10/": {
        "status_code": 200,
        "json": {
            "name": "Obi-Wan Kenobi"
        }
    }
}

class FallbackResponse:
    def __init__(self, url, status_code, payload, source):
        self.url = url
        self.status_code = status_code
        self._payload = payload
        self.source = source
        self.text = json.dumps(payload, ensure_ascii=False)

    def json(self):
        return self._payload

def get_swapi(url, timeout=20):
    try:
        resp = requests.get(url, timeout=timeout)
        payload = resp.json()
        return FallbackResponse(url, resp.status_code, payload, "online")
    except Exception:
        fixture = FIXTURES.get(url)
        if fixture is None:
            raise
        return FallbackResponse(url, fixture["status_code"], fixture["json"], "fallback_local")

def mostrar_resposta(url):
    response = get_swapi(url)
    print("Fonte da resposta:", response.source)
    print("Status code:", response.status_code)
    print(json.dumps(response.json(), indent=4, ensure_ascii=False))
    return response
```

---

# Parte 1 — Explorando a API

Foram consultados os seguintes endpoints:

```
https://swapi.dev/api/people/10/
https://swapi.dev/api/people/13/
https://swapi.dev/api/planets/2/
https://swapi.dev/api/starships/10/
```

Código utilizado:

```python
urls = [
    "https://swapi.dev/api/people/10/",
    "https://swapi.dev/api/people/13/",
    "https://swapi.dev/api/planets/2/",
    "https://swapi.dev/api/starships/10/"
]

for url in urls:
    print("\n==============================")
    print("Consultando:", url)
    mostrar_resposta(url)
```
---

# Resultado da Execução

```
==============================
Consultando: https://swapi.dev/api/people/10/
Fonte da resposta: online
Status code: 200
{
    "name": "Obi-Wan Kenobi",
    "height": "182",
    "mass": "77",
    "hair_color": "auburn, white",
    "skin_color": "fair",
    "eye_color": "blue-gray",
    "birth_year": "57BBY",
    "gender": "male",
    "homeworld": "https://swapi.dev/api/planets/20/",
    "films": [
        "https://swapi.dev/api/films/1/",
        "https://swapi.dev/api/films/2/",
        "https://swapi.dev/api/films/3/",
        "https://swapi.dev/api/films/4/",
        "https://swapi.dev/api/films/5/",
        "https://swapi.dev/api/films/6/"
    ],
    "species": [],
    "vehicles": [
        "https://swapi.dev/api/vehicles/38/"
    ],
    "starships": [
        "https://swapi.dev/api/starships/48/",
        "https://swapi.dev/api/starships/59/",
        "https://swapi.dev/api/starships/64/",
        "https://swapi.dev/api/starships/65/",
        "https://swapi.dev/api/starships/74/"
    ],
    "created": "2014-12-10T16:16:29.192000Z",
    "edited": "2014-12-20T21:17:50.325000Z",
    "url": "https://swapi.dev/api/people/10/"
}

==============================
Consultando: https://swapi.dev/api/people/13/
Fonte da resposta: online
Status code: 200
{
    "name": "Chewbacca",
    "height": "228",
    "mass": "112",
    "hair_color": "brown",
    "skin_color": "unknown",
    "eye_color": "blue",
    "birth_year": "200BBY",
    "gender": "male",
    "homeworld": "https://swapi.dev/api/planets/14/",
    "films": [
        "https://swapi.dev/api/films/1/",
        "https://swapi.dev/api/films/2/",
        "https://swapi.dev/api/films/3/",
        "https://swapi.dev/api/films/6/"
    ],
    "species": [
        "https://swapi.dev/api/species/3/"
    ],
    "vehicles": [
        "https://swapi.dev/api/vehicles/19/"
    ],
    "starships": [
        "https://swapi.dev/api/starships/10/",
        "https://swapi.dev/api/starships/22/"
    ],
    "created": "2014-12-10T16:42:45.066000Z",
    "edited": "2014-12-20T21:17:50.332000Z",
    "url": "https://swapi.dev/api/people/13/"
}

==============================
Consultando: https://swapi.dev/api/planets/2/
Fonte da resposta: online
Status code: 200
{
    "name": "Alderaan",
    "rotation_period": "24",
    "orbital_period": "364",
    "diameter": "12500",
    "climate": "temperate",
    "gravity": "1 standard",
    "terrain": "grasslands, mountains",
    "surface_water": "40",
    "population": "2000000000",
    "residents": [
        "https://swapi.dev/api/people/5/",
        "https://swapi.dev/api/people/68/",
        "https://swapi.dev/api/people/81/"
    ],
    "films": [
        "https://swapi.dev/api/films/1/",
        "https://swapi.dev/api/films/6/"
    ],
    "created": "2014-12-10T11:35:48.479000Z",
    "edited": "2014-12-20T20:58:18.420000Z",
    "url": "https://swapi.dev/api/planets/2/"
}

==============================
Consultando: https://swapi.dev/api/starships/10/
Fonte da resposta: online
Status code: 200
{
    "name": "Millennium Falcon",
    "model": "YT-1300 light freighter",
    "manufacturer": "Corellian Engineering Corporation",
    "cost_in_credits": "100000",
    "length": "34.37",
    "max_atmosphering_speed": "1050",
    "crew": "4",
    "passengers": "6",
    "cargo_capacity": "100000",
    "consumables": "2 months",
    "hyperdrive_rating": "0.5",
    "MGLT": "75",
    "starship_class": "Light freighter",
    "pilots": [
        "https://swapi.dev/api/people/13/",
        "https://swapi.dev/api/people/14/",
        "https://swapi.dev/api/people/25/",
        "https://swapi.dev/api/people/31/"
    ],
    "films": [
        "https://swapi.dev/api/films/1/",
        "https://swapi.dev/api/films/2/",
        "https://swapi.dev/api/films/3/"
    ],
    "created": "2014-12-10T16:59:45.094000Z",
    "edited": "2014-12-20T21:23:49.880000Z",
    "url": "https://swapi.dev/api/starships/10/"
}
```

---

# Parte 2 — Análise QA da Resposta

```python
analise = {
    "people/10": {
        "status_code": 200,
        "json_parece_completo": True,
        "campos": [
            "name", "height", "mass", "hair_color", "skin_color",
            "eye_color", "birth_year", "gender", "homeworld",
            "films", "species", "vehicles", "starships",
            "created", "edited", "url"
        ],
        "campo_importante": "name"
    },

    "people/13": {
        "status_code": 200,
        "json_parece_completo": True,
        "campos": [
            "name", "height", "mass", "hair_color", "skin_color",
            "eye_color", "birth_year", "gender", "homeworld",
            "films", "species", "vehicles", "starships",
            "created", "edited", "url"
        ],
        "campo_importante": "name"
    },

    "planets/2": {
        "status_code": 200,
        "json_parece_completo": True,
        "campos": [
            "name", "rotation_period", "orbital_period", "diameter",
            "climate", "gravity", "terrain", "surface_water",
            "population", "residents", "films",
            "created", "edited", "url"
        ],
        "campo_importante": "name"
    },

    "starships/10": {
        "status_code": 200,
        "json_parece_completo": True,
        "campos": [
            "name", "model", "manufacturer", "cost_in_credits",
            "length", "max_atmosphering_speed", "crew",
            "passengers", "cargo_capacity", "consumables",
            "hyperdrive_rating", "MGLT", "starship_class",
            "pilots", "films", "created", "edited", "url"
        ],
        "campo_importante": "name"
    }
}
```

### Conclusão da Análise

- Todos os endpoints retornaram **status code 200**
- O **JSON parece completo**
- O campo **`name`** foi considerado o mais importante para validação por identificar o recurso retornado.

---

# Parte 3 — Pensando como QA

Foi pensado nas seguintes validações para o endpoint:

```python
validacoes = [
    "O status code da resposta deve ser 200",
    "O campo 'name' deve existir no JSON retornado",
    "O campo 'name' não pode estar vazio",
    "O campo 'height' deve conter um valor numérico",
    "A consulta a um personagem inexistente (ex: people/9999) deve retornar status code 404"
]
```

---

# Parte 4 — Testes Automatizados com Assert

```python
url = "https://swapi.dev/api/people/10/"

response = get_swapi(url)
dados = response.json()

# Teste 1
assert response.status_code == 200

# Teste 2
assert "name" in dados

# Teste 3
assert dados["name"] != ""

# Teste 4
assert dados["height"].isdigit()

# Teste 5
assert "gender" in dados

print("Todos os testes passaram!")
```

### Resultado

Todos os testes automatizados passaram com sucesso.

---

# Bônus — Teste Negativo

Endpoint testado:

```
https://swapi.dev/api/people/9999/
```

### Código do teste

```python
url = "https://swapi.dev/api/people/9999/"

response = get_swapi(url)
dados = response.json()

assert response.status_code == 404
assert "detail" in dados

print("Erro! Endpoint inválido")
```

### Resultado Esperado

- Status code: **404**
- Campo **detail** indicando que o recurso não foi encontrado.

### Resultado Obtido

O endpoint retornou corretamente **404**, indicando que o personagem não existe.

---
