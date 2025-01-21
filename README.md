# Skillbox Search Engine

### Курсовая работа по курсу "Профессия Разработчик на C++ с нуля" SKILLBOX

## Описание

Skillbox Search Engine — это локальный поисковый движок для работы с текстовыми данными. Приложение представляет собой консольное приложение, настраиваемое через файлы формата JSON. Основная функциональность заключается в индексировании текстовых файлов, обработке запросов и предоставлении результатов поиска в виде ранжированного списка по каждому запросу.

### Принцип работы

1. **Загрузка конфигурации**: Программа загружает адреса и названия текстовых файлов для индексации из файла `config.json`.
2. **Индексирование документов**: Программа предварительно индексирует файлы, создавая и заполняя частотный справочник.
3. **Обработка запросов**: Пользовательские запросы считываются из файла `requests.json`.
4. **Поиск и ранжирование**: Программа формирует и ранжирует результаты поиска релевантных документов.
5. **Вывод результатов**: Результаты поиска записываются в файл `answers.json`.

## Компоненты проекта и реализация

### /Src

В папке `/Src` содержатся реализации следующих сервисных классов:

#### ConverterJSON

Этот класс отвечает за:

- Считывание данных из файлов формата JSON.
- Обработку и преобразование данных.
- Формирование ответов в формате JSON.

В работе используется библиотека [JSON for Modern C++](https://github.com/nlohmann/json) (Copyright © 2013-2022 Niels Lohmann).

**Примеры файлов конфигурации и запросов**:

- **config.json** (шаблон):

  ```json
  {
    "config": 
      {
        "name": "SkillboxSearchEngine",
        "version": "1.0.1",
        "max_responses": 5
      },
    "files":
      [
        "resources/file001.txt",
        "resources/file002.txt",
        "resources/file003.txt",
        "resources/file004.txt"
      ]
  }
  ```

  **Ограничения**:
  - Поля `config` и `files` обязательны.
  - Версия конфигурационного файла должна совпадать с версией проекта.
  - Каждый текстовый файл должен содержать не более 1000 слов и 100 символов в каждом слове.
  - Исключения обрабатываются программой.

- **requests.json** (шаблон):

  ```json
  {
    "requests":
      [
        "some words...",
        "some words...",
        "some words...",
        "some words..."
      ]
  }
  ```

  **Ограничения**:
  - Поле `requests` обязательно.
  - Допускается до 1000 запросов, каждый из которых может содержать до 10 слов.

- **answers.json** (шаблон):

  ```json
  {
    "answers":
      {
        "request0001":
          {
            "relevance":
              {
                "result": "true"
              },
            "documents": [
              "docid: 0, rank: 1.000000",
              "docid: 1, rank: 0.500000",
              "docid: 2, rank: 0.250000",
              "docid: 3, rank: 0.250000",
              "docid: 4, rank: 0.250000"
            ]
          },
        "request0002":
          {
            "relevance":
              {
                "result": "true"
              },
            "documents": [
              "docid: 0, rank: 0.769"
            ]
          },
        "request0003":
          {
            "relevance": {
              "result": "false"
            }
          }
      }
  }
  ```

  **Принципы формирования**:
  - Если запрос не находит релевантных документов, указывается результат `false` (например, `request0003`).
  - Если релевантен только один документ, выводится один результат (например, `request0002`).
  - Если релевантных документов несколько, они перечисляются с ранжированием (например, `request0001`).

#### InvertedIndex

Этот класс:

- Обрабатывает и хранит базу текстовых документов.
- Индексирует документы, создавая базу поисковых индексов.
- Возвращает список индексов для каждого документа на основе клиентского запроса.

#### SearchServer

Этот класс:

- Обрабатывает массив клиентских запросов.
- Использует класс `InvertedIndex` для формирования ранжированного списка релевантных документов.

### /Tests

В папке `/Tests` расположены модульные тесты (`Tests.cpp`), выполненные с использованием Google Testing and Mocking Framework. Подключение осуществляется через URL из GitHub.
