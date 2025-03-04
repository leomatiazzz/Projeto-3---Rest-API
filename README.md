# REST API - Projeto 3

Este projeto consiste em uma API REST desenvolvida com Django e Django Ninja para gerenciar alunos e suas progressões em um sistema de graduação baseado em faixas.

## 📌 Tecnologias Utilizadas

- Python
- Django
- Django Ninja
- Pillow
- SQLite (ou outro banco de dados configurado)

## 📂 Estrutura do Projeto

```
📁 core/
 ├── 📁 treino/   # Aplicação principal
 │   ├── models.py   # Modelos de dados
 │   ├── api.py      # Endpoints da API
 │   ├── schemas.py  # Schemas Pydantic para validação
 │   ├── views.py    # Lógica de visualização
 │   ├── urls.py     # Rotas da aplicação
 ├── settings.py  # Configurações do Django
 ├── urls.py      # Rotas principais
 ├── manage.py    # Comando de gerenciamento do Django
```

## 🚀 Como Executar o Projeto

### 1️⃣ Criar e Ativar Ambiente Virtual

**Linux:**
```sh
python3 -m venv venv
source venv/bin/activate
```

**Windows:**
```sh
python -m venv venv
venv\Scripts\Activate
```

Caso ocorra erro de permissão no Windows, execute:
```sh
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

### 2️⃣ Instalar Dependências
```sh
pip install django pillow django-ninja
```

### 3️⃣ Criar o Projeto Django
```sh
django-admin startproject core .
```

### 4️⃣ Criar e Configurar o App
```sh
python manage.py startapp treino
```

### 5️⃣ Rodar o Servidor
```sh
python manage.py runserver
```

## 🔗 Endpoints da API

### 📌 Criar Aluno
- **Método:** `POST`
- **Endpoint:** `/api/`
- **Payload:**
```json
{
    "nome": "João Silva",
    "email": "joao@email.com",
    "faixa": "B",
    "data_nascimento": "2000-01-01"
}
```

### 📌 Listar Alunos
- **Método:** `GET`
- **Endpoint:** `/api/aluno/`

### 📌 Consultar Progresso do Aluno
- **Método:** `GET`
- **Endpoint:** `/api/progresso_aluno/?email_aluno={email}`

### 📌 Atualizar Aluno
- **Método:** `PUT`
- **Endpoint:** `/api/alunos/{id}`
- **Payload:**
```json
{
    "nome": "João Silva Atualizado",
    "faixa": "A"
}
```

## 📜 Modelos

### 📌 Modelo de Aluno
```python
class Alunos(models.Model):
    nome = models.CharField(max_length=255)
    email = models.EmailField(unique=True)
    faixa = models.CharField(max_length=1, choices=[('B', 'Branca'), ('A', 'Azul'), ('R', 'Roxa'), ('M', 'Marrom'), ('P', 'Preta')])
    data_nascimento = models.DateField(null=True, blank=True)
```

### 📌 Modelo de Aulas Concluídas
```python
class AulasConcluidas(models.Model):
    aluno = models.ForeignKey(Alunos, on_delete=models.CASCADE)
    data = models.DateField(auto_now_add=True)
    faixa_atual = models.CharField(max_length=1, choices=faixa_choices)
```

## 📈 Função de Cálculo de Progressão
```python
import math

def calculate_lessons_to_upgrade(n):
    d = 1.47
    k = 30 / math.log(d)
    aulas = k * math.log(n + d)
    return round(aulas)
```

## 📑 Licença

Este projeto é apenas para fins educacionais e não possui uma licença específica.
