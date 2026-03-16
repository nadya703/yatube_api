# Project Structure

```
└── api_yatube
    ├── postman_collection
    │   ├── CRUD_for_yatube.postman_collection.json
    │   ├── README.md
    │   └── set_up_data.sh
    ├── tests
    │   ├── __pycache__
    │   │   ├── __init__.cpython-310.pyc
    │   │   ├── conftest.cpython-310-pytest-6.2.4.pyc
    │   │   ├── test_auth.cpython-310-pytest-6.2.4.pyc
    │   │   ├── test_comment.cpython-310-pytest-6.2.4.pyc
    │   │   ├── test_group.cpython-310-pytest-6.2.4.pyc
    │   │   ├── test_post.cpython-310-pytest-6.2.4.pyc
    │   │   └── test_settings.cpython-310-pytest-6.2.4.pyc
    │   ├── fixtures
    │   │   ├── __pycache__
    │   │   │   ├── __init__.cpython-310.pyc
    │   │   │   ├── fixture_data.cpython-310-pytest-6.2.4.pyc
    │   │   │   └── fixture_user.cpython-310-pytest-6.2.4.pyc
    │   │   ├── __init__.py
    │   │   ├── fixture_data.py
    │   │   └── fixture_user.py
    │   ├── __init__.py
    │   ├── conftest.py
    │   ├── test_auth.py
    │   ├── test_comment.py
    │   ├── test_group.py
    │   ├── test_post.py
    │   └── test_settings.py
    ├── yatube_api
    │   ├── api
    │   │   ├── __pycache__
    │   │   │   ├── __init__.cpython-310.pyc
    │   │   │   ├── admin.cpython-310.pyc
    │   │   │   ├── apps.cpython-310.pyc
    │   │   │   ├── models.cpython-310.pyc
    │   │   │   ├── permissions.cpython-310.pyc
    │   │   │   ├── serializers.cpython-310.pyc
    │   │   │   ├── urls.cpython-310.pyc
    │   │   │   └── views.cpython-310.pyc
    │   │   ├── migrations
    │   │   │   ├── __pycache__
    │   │   │   │   └── __init__.cpython-310.pyc
    │   │   │   └── __init__.py
    │   │   ├── __init__.py
    │   │   ├── admin.py
    │   │   ├── apps.py
    │   │   ├── models.py
    │   │   ├── permissions.py
    │   │   ├── serializers.py
    │   │   ├── tests.py
    │   │   ├── urls.py
    │   │   └── views.py
    │   ├── posts
    │   │   ├── __pycache__
    │   │   │   ├── __init__.cpython-310.pyc
    │   │   │   ├── admin.cpython-310.pyc
    │   │   │   ├── apps.cpython-310.pyc
    │   │   │   └── models.cpython-310.pyc
    │   │   ├── migrations
    │   │   │   ├── __pycache__
    │   │   │   │   ├── __init__.cpython-310.pyc
    │   │   │   │   └── 0001_initial.cpython-310.pyc
    │   │   │   ├── __init__.py
    │   │   │   └── 0001_initial.py
    │   │   ├── __init__.py
    │   │   ├── admin.py
    │   │   ├── apps.py
    │   │   ├── models.py
    │   │   ├── urls.py
    │   │   └── views.py
    │   ├── yatube_api
    │   │   ├── __pycache__
    │   │   │   ├── __init__.cpython-310.pyc
    │   │   │   ├── settings.cpython-310.pyc
    │   │   │   ├── urls.cpython-310.pyc
    │   │   │   └── wsgi.cpython-310.pyc
    │   │   ├── __init__.py
    │   │   ├── settings.py
    │   │   ├── urls.py
    │   │   └── wsgi.py
    │   ├── db.sqlite3
    │   └── manage.py
    ├── project.md
    ├── pytest.ini
    ├── README.md
    ├── requirements.txt
    ├── setup.cfg
    ├── test_results.md
    ├── test.http
    └── tests.md

```

# File Contents

## api_yatube/README.md

```markdown
# api_yatube

api_yatube
```

## api_yatube/postman_collection/CRUD_for_yatube.postman_collection.json

```json
{
  "info": {
    "_postman_id": "bb17d0a0-e44d-4a4e-8248-d121d7e1a7e5",
    "name": "CRUD_for_yatube",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json",
    "_exporter_id": "16131548"
  },
  "item": [
    {
      "name": "auth_tests",
      "item": [
        {
          "name": "get_token_for_regular_user // No Auth",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На POST-запрос с корректными данными должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test(\"Ответ должен содержать ключ `token`\", function () {",
                  "    pm.expect(",
                  "        pm.response,",
                  "        \"Ответ должен содержать ключ `token`\"",
                  "    ).to.have.jsonBody(\"token\");",
                  "    pm.collectionVariables.set(\"userToken\", pm.response.json().token);",
                  "    pm.collectionVariables.set(\"userUsername\", JSON.parse(request.data).username);",
                  "});",
                  ""
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"username\": \"regular_user\",\n    \"password\": \"iWannaBeAdmin\"\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/api-token-auth/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "api-token-auth", ""]
            }
          },
          "response": []
        },
        {
          "name": "get_token_for_admin // No Auth",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На POST-запрос с корректными данными должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test(\"Ответ должен содержать ключ `token`\", function () {",
                  "    pm.expect(",
                  "        pm.response,",
                  "        \"Ответ должен содержать ключ `token`\"",
                  "    ).to.have.jsonBody(\"token\");",
                  "    pm.collectionVariables.set(\"adminToken\", pm.response.json().token);",
                  "    pm.collectionVariables.set(\"adminUsername\", JSON.parse(request.data).username);",
                  "});",
                  ""
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"username\": \"root\",\n    \"password\": \"5eCretPaSsw0rD\"\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/api-token-auth/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "api-token-auth", ""]
            }
          },
          "response": []
        }
      ],
      "auth": {
        "type": "noauth"
      },
      "event": [
        {
          "listen": "prerequest",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        },
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        }
      ]
    },
    {
      "name": "group_tests",
      "item": [
        {
          "name": "get_group_list // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"array\",",
                  "    \"items\": {",
                  "        \"type\": \"object\",",
                  "        \"properties\":{",
                  "            \"id\": {\"type\": \"number\"},",
                  "            \"title\": {\"type\": \"string\"},",
                  "            \"slug\": {\"type\": \"string\"},",
                  "            \"description\": {\"type\": \"string\"}",
                  "        },",
                  "        \"required\": [\"id\", \"title\", \"slug\", \"description\"],",
                  "        \"additionalProperties\": false",
                  "    }",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос зарегистрированного пользователя должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "    pm.collectionVariables.set(\"group_id\", responseData[0].id);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "GET",
            "header": [],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/groups/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "groups", ""]
            }
          },
          "response": []
        },
        {
          "name": "get_group_details // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"properties\":{",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"title\": {\"type\": \"string\"},",
                  "        \"slug\": {\"type\": \"string\"},",
                  "        \"description\": {\"type\": \"string\"}",
                  "    },",
                  "    \"required\": [\"id\", \"title\", \"slug\", \"description\"],",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос зарегистрированного пользователя должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "GET",
            "header": [
              {
                "key": "Authorization",
                "value": "Token {{Token}}",
                "type": "text",
                "disabled": true
              }
            ],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/groups/{{group_id}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "groups", "{{group_id}}", ""]
            }
          },
          "response": []
        }
      ],
      "auth": {
        "type": "apikey",
        "apikey": [
          {
            "key": "value",
            "value": "Token {{userToken}}",
            "type": "string"
          },
          {
            "key": "key",
            "value": "Authorization",
            "type": "string"
          }
        ]
      },
      "event": [
        {
          "listen": "prerequest",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        },
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        }
      ]
    },
    {
      "name": "post_tests",
      "item": [
        {
          "name": "create_post_without_group // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"pub_date\", \"image\", \"group\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"pub_date\": {\"type\": \"string\"},",
                  "        \"image\": {\"type\": [\"string\", \"null\"]},",
                  "        \"group\": {\"type\": [\"number\", \"null\"]}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 201\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"Зарегистрированный пользователь должен иметь возможность создать пост без указания группы\"",
                  "    ).to.be.eql(\"Created\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});",
                  "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                  "    pm.expect(responseData.author).to.eql(",
                  "        pm.collectionVariables.get(\"userUsername\"),",
                  "        \"Убедитесь, что поле `author` содержит `username` пользователя, отправившего запрос\"",
                  "    );",
                  "    const requestData = JSON.parse(request.data);",
                  "    pm.expect(responseData.text).to.eql(",
                  "        requestData.text,",
                  "        \"Убедитесь, что поле `text` содержит текст, отправленный в запросе\"",
                  "    );",
                  "    pm.expect(",
                  "        responseData.group,",
                  "        \"Убедитесь, что поле `group` содержит `null`, если в запросе не передан id группы\"",
                  "    ).to.be.null;",
                  "    pm.collectionVariables.set(\"post_without_group\", responseData.id);",
                  "});",
                  ""
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"text\": \"Пост зарегистрированного пользователя.\"\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "posts", ""]
            }
          },
          "response": []
        },
        {
          "name": "create_post_with_group // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"pub_date\", \"image\", \"group\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"pub_date\": {\"type\": \"string\"},",
                  "        \"image\": {\"type\": [\"string\", \"null\"]},",
                  "        \"group\": {\"type\": [\"number\", \"null\"]}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 201\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"Зарегистрированный пользователь должен иметь возможность создать пост с указанием группы для этого поста\"",
                  "    ).to.be.eql(\"Created\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});",
                  "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                  "    pm.expect(responseData.author).to.eql(",
                  "        pm.collectionVariables.get(\"userUsername\"),",
                  "        \"Убедитесь, что поле `author` содержит `username` пользователя, отправившего запрос\"",
                  "    );",
                  "    const requestData = JSON.parse(request.data);",
                  "    pm.expect(responseData.text).to.eql(",
                  "        requestData.text,",
                  "        \"Убедитесь, что поле `text` содержит текст, отправленный в запросе\"",
                  "    );",
                  "    pm.expect(responseData.group).to.eql(",
                  "        requestData.group,",
                  "        \"Убедитесь, что поле `group` содержит id группы, отправленный в запросе\"",
                  "    );",
                  "    pm.collectionVariables.set(\"post_with_group\", responseData.id);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"text\": \"Пост с группой\",\n    \"group\": {{group_id}}\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "posts", ""]
            }
          },
          "response": []
        },
        {
          "name": "get_post_list // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"array\",",
                  "    \"items\": {",
                  "        \"type\": \"object\",",
                  "        \"properties\":{",
                  "            \"id\": {\"type\": \"number\"},",
                  "            \"author\": {\"type\": \"string\"},",
                  "            \"text\": {\"type\": \"string\"},",
                  "            \"pub_date\": {\"type\": \"string\"},",
                  "            \"image\": {\"type\": [\"string\", \"null\"]},",
                  "            \"group\": {\"type\": [\"number\", \"null\"]}",
                  "        },",
                  "        \"required\": [\"id\", \"author\", \"text\", \"pub_date\", \"image\", \"group\"],",
                  "        \"additionalProperties\": false",
                  "    }",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос зарегистрированного пользователя должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "GET",
            "header": [],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "posts", ""]
            }
          },
          "response": []
        },
        {
          "name": "get_post_details // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"pub_date\", \"image\", \"group\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"pub_date\": {\"type\": \"string\"},",
                  "        \"image\": {\"type\": [\"string\", \"null\"]},",
                  "        \"group\": {\"type\": [\"number\", \"null\"]}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос зарегистрированного пользователя должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});",
                  ""
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "GET",
            "header": [],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_without_group}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "posts", "{{post_without_group}}", ""]
            }
          },
          "response": []
        },
        {
          "name": "update_post_with_patch_request // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"pub_date\", \"image\", \"group\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"pub_date\": {\"type\": \"string\"},",
                  "        \"image\": {\"type\": [\"string\", \"null\"]},",
                  "        \"group\": {\"type\": [\"number\", \"null\"]}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На корректный запрос автора поста должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});",
                  "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                  "    pm.expect(responseData.author).to.eql(",
                  "        pm.collectionVariables.get(\"userUsername\"),",
                  "        \"Убедитесь, что поле `author` содержит `username` пользователя, отправившего запрос\"",
                  "    );",
                  "    const requestData = JSON.parse(request.data);",
                  "    pm.expect(responseData.text).to.eql(",
                  "        requestData.text,",
                  "        \"Убедитесь, что поле `text` содержит текст, отправленный в запросе\"",
                  "    );",
                  "    pm.expect(responseData.group).to.eql(",
                  "        requestData.group,",
                  "        \"Убедитесь, что поле `group` содержит id группы, отправленный в запросе\"",
                  "    );",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "PATCH",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"text\": \"Добавляем пост в группу\",\n    \"group\": {{group_id}}\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_without_group}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "posts", "{{post_without_group}}", ""]
            }
          },
          "response": []
        },
        {
          "name": "update_post_with_put_request // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"pub_date\", \"image\", \"group\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"pub_date\": {\"type\": \"string\"},",
                  "        \"image\": {\"type\": [\"string\", \"null\"]},",
                  "        \"group\": {\"type\": [\"number\", \"null\"]}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На корректный запрос автора поста должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});",
                  "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                  "    pm.expect(responseData.author).to.eql(",
                  "        pm.collectionVariables.get(\"userUsername\"),",
                  "        \"Убедитесь, что поле `author` содержит `username` пользователя, отправившего запрос\"",
                  "    );",
                  "    const requestData = JSON.parse(request.data);",
                  "    pm.expect(responseData.text).to.eql(",
                  "        requestData.text,",
                  "        \"Убедитесь, что поле `text` содержит текст, отправленный в запросе\"",
                  "    );",
                  "    pm.expect(",
                  "        responseData.group,",
                  "        \"Убедитесь, что поле `group` содержит `null`, если в запросе в качестве значения данного поля передан `null`\"",
                  "    ).to.be.null;",
                  "    pm.collectionVariables.set(\"post_without_group\", responseData.id);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "PUT",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"text\": \"Обновленный текст через PUT-запрос\",\n    \"group\": null\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_without_group}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "posts", "{{post_without_group}}", ""]
            }
          },
          "response": []
        },
        {
          "name": "delete_post // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Статус-код ответа должен быть 204\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос автора поста должен возвращаться ответ со статусом 204\"",
                  "    ).to.be.eql(\"No Content\");",
                  "});",
                  ""
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "DELETE",
            "header": [],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_without_group}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "posts", "{{post_without_group}}", ""]
            }
          },
          "response": []
        }
      ],
      "auth": {
        "type": "apikey",
        "apikey": [
          {
            "key": "value",
            "value": "Token {{userToken}}",
            "type": "string"
          },
          {
            "key": "key",
            "value": "Authorization",
            "type": "string"
          }
        ]
      },
      "event": [
        {
          "listen": "prerequest",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        },
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        }
      ]
    },
    {
      "name": "comment_tests",
      "item": [
        {
          "name": "create_comment // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"created\", \"post\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"created\": {\"type\": \"string\"},",
                  "        \"post\": {\"type\": \"number\"}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 201\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"Корректный запрос зарегистрированного пользователя должен возвращать ответ со статусом 201\"",
                  "    ).to.be.eql(\"Created\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});",
                  "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                  "    pm.expect(responseData.author).to.eql(",
                  "        pm.collectionVariables.get(\"userUsername\"),",
                  "        \"Убедитесь, что поле `author` содержит `username` пользователя, отправившего запрос\"",
                  "    );",
                  "    const requestData = JSON.parse(request.data);",
                  "    pm.expect(responseData.text).to.eql(",
                  "        requestData.text,",
                  "        \"Убедитесь, что поле `text` содержит текст, отправленный в запросе\"",
                  "    );",
                  "    pm.expect(responseData.post).to.eql(",
                  "        pm.collectionVariables.get(\"post_with_group\"),",
                  "        \"Убедитесь, что поле `post` содержит id поста, указанный в url при запросе\"",
                  "    );",
                  "    pm.collectionVariables.set(\"comment_id\", responseData.id);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"text\": \"Тестовый комментарий\"\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": [
                "api",
                "v1",
                "posts",
                "{{post_with_group}}",
                "comments",
                ""
              ]
            }
          },
          "response": []
        },
        {
          "name": "get_comments // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"array\",",
                  "    \"items\": {",
                  "        \"type\": \"object\",",
                  "        \"properties\":{",
                  "            \"id\": {\"type\": \"number\"},",
                  "            \"author\": {\"type\": \"string\"},",
                  "            \"text\": {\"type\": \"string\"},",
                  "            \"created\": {\"type\": \"string\"},",
                  "            \"post\": {\"type\": [\"number\", \"null\"]}",
                  "        },",
                  "        \"required\": [\"id\", \"author\", \"text\", \"created\", \"post\"],",
                  "        \"additionalProperties\": false",
                  "    }",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос зарегистрированного пользователя должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "GET",
            "header": [],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": [
                "api",
                "v1",
                "posts",
                "{{post_with_group}}",
                "comments",
                ""
              ]
            }
          },
          "response": []
        },
        {
          "name": "get_comment_details // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"created\", \"post\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"created\": {\"type\": \"string\"},",
                  "        \"post\": {\"type\": \"number\"}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос зарегистрированного пользователя должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});",
                  "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                  "    pm.expect(responseData.id).to.eql(",
                  "        pm.collectionVariables.get(\"comment_id\"),",
                  "        \"Убедитесь, что поле `post` содержит id поста, указанный в url при запросе\"",
                  "    );",
                  "    pm.expect(responseData.post).to.eql(",
                  "        pm.collectionVariables.get(\"post_with_group\"),",
                  "        \"Убедитесь, что поле `post` содержит id поста, указанный в url при запросе\"",
                  "    );",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "GET",
            "header": [],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": [
                "api",
                "v1",
                "posts",
                "{{post_with_group}}",
                "comments",
                "{{comment_id}}",
                ""
              ]
            }
          },
          "response": []
        },
        {
          "name": "update_comment_with_patch_request // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"created\", \"post\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"created\": {\"type\": \"string\"},",
                  "        \"post\": {\"type\": \"number\"}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На корректный запрос автора комментария должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});",
                  "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                  "    pm.expect(responseData.author).to.eql(",
                  "        pm.collectionVariables.get(\"userUsername\"),",
                  "        \"Убедитесь, что поле `author` содержит `username` пользователя, отправившего запрос\"",
                  "    );",
                  "    const requestData = JSON.parse(request.data);",
                  "    pm.expect(responseData.text).to.eql(",
                  "        requestData.text,",
                  "        \"Убедитесь, что поле `text` содержит текст, отправленный в запросе\"",
                  "    );",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "PATCH",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"text\": \"Новый текст для комментария\"\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": [
                "api",
                "v1",
                "posts",
                "{{post_with_group}}",
                "comments",
                "{{comment_id}}",
                ""
              ]
            }
          },
          "response": []
        },
        {
          "name": "update_comment_with_put_request // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"created\", \"post\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"created\": {\"type\": \"string\"},",
                  "        \"post\": {\"type\": \"number\"}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 200\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На корректный запрос автора комментария должен возвращаться ответ со статусом 200\"",
                  "    ).to.be.eql(\"OK\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "});",
                  "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                  "    pm.expect(responseData.author).to.eql(",
                  "        pm.collectionVariables.get(\"userUsername\"),",
                  "        \"Убедитесь, что поле `author` содержит `username` пользователя, отправившего запрос\"",
                  "    );",
                  "    const requestData = JSON.parse(request.data);",
                  "    pm.expect(responseData.text).to.eql(",
                  "        requestData.text,",
                  "        \"Убедитесь, что поле `text` содержит текст, отправленный в запросе\"",
                  "    );",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "PUT",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"text\": \"Текст изменен через PUT-запрос\"\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": [
                "api",
                "v1",
                "posts",
                "{{post_with_group}}",
                "comments",
                "{{comment_id}}",
                ""
              ]
            }
          },
          "response": []
        },
        {
          "name": "delete_comment // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Статус-код ответа должен быть 204\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос автора комментария должен возвращаться ответ со статусом 204\"",
                  "    ).to.be.eql(\"No Content\");",
                  "});",
                  ""
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "DELETE",
            "header": [],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": [
                "api",
                "v1",
                "posts",
                "{{post_with_group}}",
                "comments",
                "{{comment_id}}",
                ""
              ]
            }
          },
          "response": []
        },
        {
          "name": "create_comment_for_permission_tests // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "const responseData = pm.response.json();",
                  "const responseSchema = {",
                  "    \"type\": \"object\",",
                  "    \"required\": [\"id\", \"author\", \"text\", \"created\", \"post\"],",
                  "    \"properties\": {",
                  "        \"id\": {\"type\": \"number\"},",
                  "        \"author\": {\"type\": \"string\"},",
                  "        \"text\": {\"type\": \"string\"},",
                  "        \"created\": {\"type\": \"string\"},",
                  "        \"post\": {\"type\": \"number\"}",
                  "    },",
                  "    \"additionalProperties\": false",
                  "};",
                  "",
                  "pm.test(\"Статус-код ответа должен быть 201\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"Корректный запрос зарегистрированного пользователя должен возвращать ответ со статусом 201\"",
                  "    ).to.be.eql(\"Created\");",
                  "});",
                  "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                  "    pm.response.to.have.jsonSchema(responseSchema);",
                  "    pm.collectionVariables.set(\"comment_id_for_permission_tests\", responseData.id);",
                  "});"
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "method": "POST",
            "header": [],
            "body": {
              "mode": "raw",
              "raw": "{\n    \"text\": \"Комментарий для проверки ограничения прав доступа\"\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            },
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": [
                "api",
                "v1",
                "posts",
                "{{post_with_group}}",
                "comments",
                ""
              ]
            }
          },
          "response": []
        }
      ],
      "auth": {
        "type": "apikey",
        "apikey": [
          {
            "key": "value",
            "value": "Token {{userToken}}",
            "type": "string"
          },
          {
            "key": "key",
            "value": "Authorization",
            "type": "string"
          }
        ]
      },
      "event": [
        {
          "listen": "prerequest",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        },
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        }
      ]
    },
    {
      "name": "negative_tests",
      "item": [
        {
          "name": "create_update_delete_group",
          "item": [
            {
              "name": "create_group // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 405\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"API не должен предоставлять возможность создания группы\"",
                      "    ).to.be.eql(\"Method Not Allowed\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "POST",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"title\": \"Тестовая группа\",\n    \"slug\": \"test-group\",\n    \"description\": \"Проверка возможности создания группы\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/groups/{{group_id}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "groups", "{{group_id}}", ""]
                }
              },
              "response": []
            },
            {
              "name": "update_group_with_patch_request // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 405\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"API не должен предоставлять возможность редактирования группы\"",
                      "    ).to.be.eql(\"Method Not Allowed\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PATCH",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"title\": \"Тестовая группа\",\n    \"slug\": \"test-group\",\n    \"description\": \"Проверка возможности создания группы\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/groups/{{group_id}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "groups", "{{group_id}}", ""]
                }
              },
              "response": []
            },
            {
              "name": "update_group_with_put_request // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 405\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"API не должен предоставлять возможность редактирования группы\"",
                      "    ).to.be.eql(\"Method Not Allowed\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PUT",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"title\": \"Тестовая группа\",\n    \"slug\": \"test-group\",\n    \"description\": \"Проверка возможности создания группы\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/groups/{{group_id}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "groups", "{{group_id}}", ""]
                }
              },
              "response": []
            },
            {
              "name": "delete_group // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 405\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"API не должен предоставлять возможность удаления группы\"",
                      "    ).to.be.eql(\"Method Not Allowed\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "DELETE",
                "header": [],
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/groups/{{group_id}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "groups", "{{group_id}}", ""]
                }
              },
              "response": []
            }
          ],
          "event": [
            {
              "listen": "prerequest",
              "script": {
                "type": "text/javascript",
                "exec": [""]
              }
            },
            {
              "listen": "test",
              "script": {
                "type": "text/javascript",
                "exec": [""]
              }
            }
          ]
        },
        {
          "name": "update_delete_post_not_author_user",
          "item": [
            {
              "name": "update_post_not_author_with_patch_request // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 403\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"У пользователя не должно быть возможности изменять чужие посты\"",
                      "    ).to.be.eql(\"Forbidden\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PATCH",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"Убираем пост из группы\",\n    \"group\": null\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
                }
              },
              "response": []
            },
            {
              "name": "update_post_not_author_with_put_request // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 403\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"У пользователя не должно быть возможности изменять чужие посты\"",
                      "    ).to.be.eql(\"Forbidden\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PUT",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"Убираем пост из группы\",\n    \"group\": null\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
                }
              },
              "response": []
            },
            {
              "name": "delete_post_not_author // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 403\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"У пользователя не должно быть возможности удалять чужие посты\"",
                      "    ).to.be.eql(\"Forbidden\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "DELETE",
                "header": [],
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
                }
              },
              "response": []
            }
          ]
        },
        {
          "name": "create_update_with_corrupted_data",
          "item": [
            {
              "name": "create_post_with_empty_text // User",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 400\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе пользователя на создание поста в поле `text` содержится пустая строка - должен вернуться ответ со статусом 400\"",
                      "    ).to.be.eql(\"Bad Request\");",
                      "});",
                      ""
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "POST",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", ""]
                }
              },
              "response": []
            },
            {
              "name": "create_post_with_invalid_group // User",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 400\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе пользователя на создание поста в поле `group` содержится идентификатор несуществующей группы - должен вернуться ответ со статусом 400\"",
                      "    ).to.be.eql(\"Bad Request\");",
                      "});",
                      ""
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "POST",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"Пост с несуществующей группой\",\n    \"group\": -1\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", ""]
                }
              },
              "response": []
            },
            {
              "name": "patch_post_with_empty_text // User",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 400\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе автора на редактирование поста поле `text` содержит пустую строку - должен вернуться ответ со статусом 400\"",
                      "    ).to.be.eql(\"Bad Request\");",
                      "});",
                      ""
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PATCH",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
                }
              },
              "response": []
            },
            {
              "name": "put_for_post_with_non_existing_group // User",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 400\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если запрос автора на редактирование поста содержит в поле `group` данные несуществующей группы - должен вернуться ответ со статусом 400\"",
                      "    ).to.be.eql(\"Bad Request\");",
                      "});",
                      ""
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PUT",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"Запрос с некорректными данными для поля `group`\",\n    \"group\": 99999\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
                }
              },
              "response": []
            },
            {
              "name": "patch_comment_with_empty_text // User",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 400\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе пользователя на создание комментария в поле `text` содержится пустая строка - должен вернуться ответ со статусом 400\"",
                      "    ).to.be.eql(\"Bad Request\");",
                      "});",
                      ""
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "POST",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "{{post_with_group}}",
                    "comments",
                    ""
                  ]
                }
              },
              "response": []
            },
            {
              "name": "patch_comment_with_empty_text // User",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 400\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе автора на редактирование комментария в поле `text` содержится пустая строка - должен вернуться ответ со статусом 400\"",
                      "    ).to.be.eql(\"Bad Request\");",
                      "});",
                      ""
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PATCH",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id_for_permission_tests}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "{{post_with_group}}",
                    "comments",
                    "{{comment_id_for_permission_tests}}",
                    ""
                  ]
                }
              },
              "response": []
            }
          ],
          "auth": {
            "type": "apikey",
            "apikey": [
              {
                "key": "value",
                "value": "Token {{userToken}}",
                "type": "string"
              },
              {
                "key": "key",
                "value": "Authorization",
                "type": "string"
              }
            ]
          },
          "event": [
            {
              "listen": "prerequest",
              "script": {
                "type": "text/javascript",
                "exec": [""]
              }
            },
            {
              "listen": "test",
              "script": {
                "type": "text/javascript",
                "exec": [""]
              }
            }
          ]
        },
        {
          "name": "update_delete_comment_not_author",
          "item": [
            {
              "name": "update_comment_not_author_with_patch_request // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 403\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"У пользователя не должно быть возможности изменять чужие комментарии\"",
                      "    ).to.be.eql(\"Forbidden\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PATCH",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"Изменение чужого комментария\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id_for_permission_tests}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "{{post_with_group}}",
                    "comments",
                    "{{comment_id_for_permission_tests}}",
                    ""
                  ]
                }
              },
              "response": []
            },
            {
              "name": "update_comment_not_auth_with_put_request // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 403\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"У пользователя не должно быть возможности изменять чужие комментарии\"",
                      "    ).to.be.eql(\"Forbidden\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PUT",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"Изменение чужого комментария через PUT-запрос\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id_for_permission_tests}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "{{post_with_group}}",
                    "comments",
                    "{{comment_id_for_permission_tests}}",
                    ""
                  ]
                }
              },
              "response": []
            },
            {
              "name": "delete_comment_not_author // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 403\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"У пользователя не должно быть возможности удалять чужие комментарии\"",
                      "    ).to.be.eql(\"Forbidden\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "DELETE",
                "header": [],
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id_for_permission_tests}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "{{post_with_group}}",
                    "comments",
                    "{{comment_id_for_permission_tests}}",
                    ""
                  ]
                }
              },
              "response": []
            }
          ]
        },
        {
          "name": "unauthorized_user_tests",
          "item": [
            {
              "name": "posts_crud_no_auth",
              "item": [
                {
                  "name": "create_post_without_group // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности создать пост\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "POST",
                    "header": [],
                    "body": {
                      "mode": "raw",
                      "raw": "{\n    \"text\": \"Пост незарегистрированного пользователя\"\n}",
                      "options": {
                        "raw": {
                          "language": "json"
                        }
                      }
                    },
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": ["api", "v1", "posts", ""]
                    }
                  },
                  "response": []
                },
                {
                  "name": "get_post_list // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности читать посты\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "GET",
                    "header": [],
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": ["api", "v1", "posts", ""]
                    }
                  },
                  "response": []
                },
                {
                  "name": "get_post_details // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности читать посты\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "GET",
                    "header": [],
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
                    }
                  },
                  "response": []
                },
                {
                  "name": "update_post_with_patch_request // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности редактировать посты\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "PATCH",
                    "header": [],
                    "body": {
                      "mode": "raw",
                      "raw": "{\n    \"text\": \"Незарегистрированный пользователь редактирует пост\"\n}",
                      "options": {
                        "raw": {
                          "language": "json"
                        }
                      }
                    },
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
                    }
                  },
                  "response": []
                },
                {
                  "name": "update_post_with_put_request // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности редактировать посты\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "PUT",
                    "header": [],
                    "body": {
                      "mode": "raw",
                      "raw": "{\n    \"text\": \"Незарегистрированный пользователь редактирует пост\"\n}",
                      "options": {
                        "raw": {
                          "language": "json"
                        }
                      }
                    },
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
                    }
                  },
                  "response": []
                },
                {
                  "name": "delete_post // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности удалять посты\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "DELETE",
                    "header": [],
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
                    }
                  },
                  "response": []
                }
              ]
            },
            {
              "name": "comments_crud_no_auth",
              "item": [
                {
                  "name": "create_comment // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности создать комментарии\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "POST",
                    "header": [],
                    "body": {
                      "mode": "raw",
                      "raw": "{\n    \"text\": \"Комментарий незарегистрированного пользователя\"\n}",
                      "options": {
                        "raw": {
                          "language": "json"
                        }
                      }
                    },
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": [
                        "api",
                        "v1",
                        "posts",
                        "{{post_with_group}}",
                        "comments",
                        ""
                      ]
                    }
                  },
                  "response": []
                },
                {
                  "name": "get_comments // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности читать комментарии\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "GET",
                    "header": [],
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": [
                        "api",
                        "v1",
                        "posts",
                        "{{post_with_group}}",
                        "comments",
                        ""
                      ]
                    }
                  },
                  "response": []
                },
                {
                  "name": "get_comment_details // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности читать комментарии\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "GET",
                    "header": [],
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id_for_permission_tests}}/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": [
                        "api",
                        "v1",
                        "posts",
                        "{{post_with_group}}",
                        "comments",
                        "{{comment_id_for_permission_tests}}",
                        ""
                      ]
                    }
                  },
                  "response": []
                },
                {
                  "name": "updete_comment_with_patch_request // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности редактировать комментарии\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "PATCH",
                    "header": [],
                    "body": {
                      "mode": "raw",
                      "raw": "{\n    \"text\": \"Комментарий отредактирован незарегистрированным пользователем\"\n}",
                      "options": {
                        "raw": {
                          "language": "json"
                        }
                      }
                    },
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id_for_permission_tests}}/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": [
                        "api",
                        "v1",
                        "posts",
                        "{{post_with_group}}",
                        "comments",
                        "{{comment_id_for_permission_tests}}",
                        ""
                      ]
                    }
                  },
                  "response": []
                },
                {
                  "name": "update_commetn_with_put_request // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности редактировать комментарии\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "PUT",
                    "header": [],
                    "body": {
                      "mode": "raw",
                      "raw": "{\n    \"text\": \"Текст изменен через PUT-запрос\"\n}",
                      "options": {
                        "raw": {
                          "language": "json"
                        }
                      }
                    },
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id_for_permission_tests}}/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": [
                        "api",
                        "v1",
                        "posts",
                        "{{post_with_group}}",
                        "comments",
                        "{{comment_id_for_permission_tests}}",
                        ""
                      ]
                    }
                  },
                  "response": []
                },
                {
                  "name": "delete_comment // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности удалять комментарии\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "DELETE",
                    "header": [],
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/comments/{{comment_id_for_permission_tests}}/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": [
                        "api",
                        "v1",
                        "posts",
                        "{{post_with_group}}",
                        "comments",
                        "{{comment_id_for_permission_tests}}",
                        ""
                      ]
                    }
                  },
                  "response": []
                }
              ]
            },
            {
              "name": "group_tests // No Auth",
              "item": [
                {
                  "name": "get_group_list // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности получать информацию о группах\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "GET",
                    "header": [],
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/groups/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": ["api", "v1", "groups", ""]
                    }
                  },
                  "response": []
                },
                {
                  "name": "get_group_details // No Auth",
                  "event": [
                    {
                      "listen": "test",
                      "script": {
                        "exec": [
                          "pm.test(\"Статус-код ответа должен быть 401\", function () {",
                          "    pm.expect(",
                          "        pm.response.status,",
                          "        \"Незарегистрированный пользователь не должен иметь возможности получать информацию о группах\"",
                          "    ).to.be.eql(\"Unauthorized\");",
                          "});",
                          ""
                        ],
                        "type": "text/javascript"
                      }
                    }
                  ],
                  "request": {
                    "method": "GET",
                    "header": [
                      {
                        "key": "Authorization",
                        "value": "Token {{Token}}",
                        "type": "text",
                        "disabled": true
                      }
                    ],
                    "url": {
                      "raw": "http://127.0.0.1:8000/api/v1/groups/{{group_id}}/",
                      "protocol": "http",
                      "host": ["127", "0", "0", "1"],
                      "port": "8000",
                      "path": ["api", "v1", "groups", "{{group_id}}", ""]
                    }
                  },
                  "response": []
                }
              ]
            }
          ],
          "auth": {
            "type": "noauth"
          },
          "event": [
            {
              "listen": "prerequest",
              "script": {
                "type": "text/javascript",
                "exec": [""]
              }
            },
            {
              "listen": "test",
              "script": {
                "type": "text/javascript",
                "exec": [""]
              }
            }
          ]
        },
        {
          "name": "404_tests",
          "item": [
            {
              "name": "get_group_404 // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 404\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе к `/api/v1/groups/{group_id}/` указан id несуществующей группы, код ответа должен быть 404\"",
                      "    ).to.be.eql(\"Not Found\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "GET",
                "header": [],
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/groups/99999/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "groups", "99999", ""]
                }
              },
              "response": []
            },
            {
              "name": "get_post_404 // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 404\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе к `/api/v1/posts/{post_id}/` указан id несуществующего поста, код ответа должен быть 404\"",
                      "    ).to.be.eql(\"Not Found\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "GET",
                "header": [],
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/99999/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", "99999", ""]
                }
              },
              "response": []
            },
            {
              "name": "patch_post_404 // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 404\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе к `/api/v1/posts/{post_id}/` указан id несуществующего поста, код ответа должен быть 404\"",
                      "    ).to.be.eql(\"Not Found\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PATCH",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"Новый текст для несуществующего поста\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/99999/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", "99999", ""]
                }
              },
              "response": []
            },
            {
              "name": "delete_post_404 // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 404\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе к `/api/v1/posts/{post_id}/` указан id несуществующего поста, код ответа должен быть 404\"",
                      "    ).to.be.eql(\"Not Found\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "DELETE",
                "header": [],
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/99999/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", "99999", ""]
                }
              },
              "response": []
            },
            {
              "name": "get_comment_404 // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 404\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе к `/api/v1/posts/{post_id}/comments/{commint_id}/` указаны id несуществующих объектов, код ответа должен быть 404\"",
                      "    ).to.be.eql(\"Not Found\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "GET",
                "header": [],
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/99999/comments/99999/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "99999",
                    "comments",
                    "99999",
                    ""
                  ]
                }
              },
              "response": []
            },
            {
              "name": "patch_comment_404 // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 404\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе к `/api/v1/posts/{post_id}/comments/{commint_id}/` указаны id несуществующих объектов, код ответа должен быть 404\"",
                      "    ).to.be.eql(\"Not Found\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "PATCH",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"text\": \"Новый текст для несуществующего комментария\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/99999/comments/99999/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "99999",
                    "comments",
                    "99999",
                    ""
                  ]
                }
              },
              "response": []
            },
            {
              "name": "delete_comment_404 // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 404\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе к `/api/v1/posts/{post_id}/comments/{commint_id}/` указаны id несуществующих объектов, код ответа должен быть 404\"",
                      "    ).to.be.eql(\"Not Found\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "DELETE",
                "header": [],
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/99999/comments/99999/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "99999",
                    "comments",
                    "99999",
                    ""
                  ]
                }
              },
              "response": []
            },
            {
              "name": "get_existing_comment_with_non_existing_post_id // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "pm.test(\"Статус-код ответа должен быть 404\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Если в запросе к `/api/v1/posts/{post_id}/comments/{commint_id}/` указан id несуществующего поста, код ответа должен быть 404\"",
                      "    ).to.be.eql(\"Not Found\");",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "GET",
                "header": [],
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/99999/comments/{{comment_id_for_permission_tests}}/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "99999",
                    "comments",
                    "{{comment_id_for_permission_tests}}",
                    ""
                  ]
                }
              },
              "response": []
            }
          ]
        },
        {
          "name": "create_from_another_author",
          "item": [
            {
              "name": "create_post_from_another_author // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "const responseData = pm.response.json();",
                      "const responseSchema = {",
                      "    \"type\": \"object\",",
                      "    \"required\": [\"id\", \"author\", \"text\", \"pub_date\", \"image\", \"group\"],",
                      "    \"properties\": {",
                      "        \"id\": {\"type\": \"number\"},",
                      "        \"author\": {\"type\": \"string\"},",
                      "        \"text\": {\"type\": \"string\"},",
                      "        \"pub_date\": {\"type\": \"string\"},",
                      "        \"image\": {\"type\": [\"string\", \"null\"]},",
                      "        \"group\": {\"type\": [\"number\", \"null\"]}",
                      "    },",
                      "    \"additionalProperties\": false",
                      "};",
                      "",
                      "pm.test(\"Статус-код ответа должен быть 201\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Запрос зарегистрированного пользователя, содержащий данные для полей `text` и `author`, должен возвращать ответ со статусом 201\"",
                      "    ).to.be.eql(\"Created\");",
                      "});",
                      "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                      "    pm.response.to.have.jsonSchema(responseSchema);",
                      "});",
                      "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                      "    pm.expect(responseData.author).to.eql(",
                      "        pm.collectionVariables.get(\"adminUsername\"),",
                      "        \"Убедитесь, что поле `author` содержит `username` пользователя, отправившего запрос\"",
                      "    );",
                      "    pm.collectionVariables.set(\"negative_test_post\", responseData.id);",
                      "});",
                      ""
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "POST",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"author\": \"{{userToken}}\",\n    \"text\": \"Создаем пост от лица другого пользователя\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": ["api", "v1", "posts", ""]
                }
              },
              "response": []
            },
            {
              "name": "create_comment_from_another_user // Admin",
              "event": [
                {
                  "listen": "test",
                  "script": {
                    "exec": [
                      "const responseData = pm.response.json();",
                      "const responseSchema = {",
                      "    \"type\": \"object\",",
                      "    \"required\": [\"id\", \"author\", \"text\", \"created\", \"post\"],",
                      "    \"properties\": {",
                      "        \"id\": {\"type\": \"number\"},",
                      "        \"author\": {\"type\": \"string\"},",
                      "        \"text\": {\"type\": \"string\"},",
                      "        \"created\": {\"type\": \"string\"},",
                      "        \"post\": {\"type\": \"number\"}",
                      "    },",
                      "    \"additionalProperties\": false",
                      "};",
                      "",
                      "pm.test(\"Статус-код ответа должен быть 201\", function () {",
                      "    pm.expect(",
                      "        pm.response.status,",
                      "        \"Запрос зарегистрированного пользователя, содержащий данные для полей `text` и `author`, должен возвращать ответ со статусом 201\"",
                      "    ).to.be.eql(\"Created\");",
                      "});",
                      "pm.test('Структура ответа должна соответствовать ожидаемой', function () {",
                      "    pm.response.to.have.jsonSchema(responseSchema);",
                      "});",
                      "pm.test(\"Ответ должен содержать ожидаемые данные\", function () {",
                      "    pm.expect(responseData.author).to.eql(",
                      "        pm.collectionVariables.get(\"adminUsername\"),",
                      "        \"Убедитесь, что поле `author` содержит `username` пользователя, отправившего запрос\"",
                      "    );",
                      "});"
                    ],
                    "type": "text/javascript"
                  }
                }
              ],
              "request": {
                "method": "POST",
                "header": [],
                "body": {
                  "mode": "raw",
                  "raw": "{\n    \"author\": \"{{userToken}}\",\n    \"text\": \"Тестовый комментарий\"\n}",
                  "options": {
                    "raw": {
                      "language": "json"
                    }
                  }
                },
                "url": {
                  "raw": "http://127.0.0.1:8000/api/v1/posts/{{negative_test_post}}/comments/",
                  "protocol": "http",
                  "host": ["127", "0", "0", "1"],
                  "port": "8000",
                  "path": [
                    "api",
                    "v1",
                    "posts",
                    "{{negative_test_post}}",
                    "comments",
                    ""
                  ]
                }
              },
              "response": []
            }
          ]
        }
      ],
      "auth": {
        "type": "apikey",
        "apikey": [
          {
            "key": "value",
            "value": "Token {{adminToken}}",
            "type": "string"
          },
          {
            "key": "key",
            "value": "Authorization",
            "type": "string"
          }
        ]
      },
      "event": [
        {
          "listen": "prerequest",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        },
        {
          "listen": "test",
          "script": {
            "type": "text/javascript",
            "exec": [""]
          }
        }
      ]
    },
    {
      "name": "tear_down",
      "item": [
        {
          "name": "delete_post_with_group // User",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Статус-код ответа должен быть 204\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос автора поста должен возвращаться ответ со статусом 204\"",
                  "    ).to.be.eql(\"No Content\");",
                  "});",
                  ""
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "auth": {
              "type": "apikey",
              "apikey": [
                {
                  "key": "value",
                  "value": "Token {{userToken}}",
                  "type": "string"
                },
                {
                  "key": "key",
                  "value": "Authorization",
                  "type": "string"
                }
              ]
            },
            "method": "DELETE",
            "header": [],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{post_with_group}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "posts", "{{post_with_group}}", ""]
            }
          },
          "response": []
        },
        {
          "name": "delete_negative_test_post // Admin",
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Статус-код ответа должен быть 204\", function () {",
                  "    pm.expect(",
                  "        pm.response.status,",
                  "        \"На запрос автора поста должен возвращаться ответ со статусом 204\"",
                  "    ).to.be.eql(\"No Content\");",
                  "});",
                  ""
                ],
                "type": "text/javascript"
              }
            }
          ],
          "request": {
            "auth": {
              "type": "apikey",
              "apikey": [
                {
                  "key": "value",
                  "value": "Token {{adminToken}}",
                  "type": "string"
                },
                {
                  "key": "key",
                  "value": "Authorization",
                  "type": "string"
                }
              ]
            },
            "method": "DELETE",
            "header": [],
            "url": {
              "raw": "http://127.0.0.1:8000/api/v1/posts/{{negative_test_post}}/",
              "protocol": "http",
              "host": ["127", "0", "0", "1"],
              "port": "8000",
              "path": ["api", "v1", "posts", "{{negative_test_post}}", ""]
            }
          },
          "response": []
        }
      ]
    }
  ],
  "event": [
    {
      "listen": "prerequest",
      "script": {
        "type": "text/javascript",
        "exec": [""]
      }
    },
    {
      "listen": "test",
      "script": {
        "type": "text/javascript",
        "exec": [""]
      }
    }
  ],
  "variable": [
    {
      "key": "userToken",
      "value": ""
    },
    {
      "key": "userUsername",
      "value": ""
    },
    {
      "key": "adminToken",
      "value": ""
    },
    {
      "key": "adminUsername",
      "value": ""
    },
    {
      "key": "group_id",
      "value": ""
    },
    {
      "key": "post_without_group",
      "value": ""
    },
    {
      "key": "post_with_group",
      "value": ""
    },
    {
      "key": "comment_id",
      "value": ""
    },
    {
      "key": "comment_id_for_permission_tests",
      "value": ""
    },
    {
      "key": "negative_test_post",
      "value": ""
    }
  ]
}
```

## api_yatube/postman_collection/README.md

```markdown
## Postman-коллекция для проверки API

Файл `CRUD_for_yatube.postman_collection` содержит postman-коллекцию - набор заранее подготовленных запросов для проверки работы API.

## Подготовка Django-проекта к запуску коллекции:

1. Проверьте, что виртуальное окружение развёрнуто и активировано, зависимости проекта установлены.
2. Перейдите в директорию _postman_collection_ и командой `bash set*up_data.sh`` запустите bash-скрипт для создания необходимых для работы коллекции объектов в базе данных.  
   *Внимание, скрипт предварительно очищает существующую базу данных.\_
3. Перейдите в директорию с файлом `manage.py` и запустите тестовый сервер.

## Загрузка коллекции в Postman:

1. Запустите Postman;
2. В левом верхнем углу нажмите `File` -> `Import`;
3. Во всплывающем окне будет предложено перетащить в него файл с коллекцией либо выбрать файл через окно файлового менеджера.
   Загрузите файл `CRUD_for_yatube.postman_collection` в Postman.

## Запуск коллекции:

1. После выполнения предыдущих шагов, в левой части окна Postman во вкладке `Collections` появилась импортированная коллекция.
   Наведите на нее, нажмите на три точки напротив названия коллекции и в выпадающем списке выберите `Run collection`. В центре экрана появится список запросов коллекции,
   а в правой части экрана - меню для настройки параметров запуска;
2. В правом меню включите фунцию `Persist responses for a session` - это даст возможность посмотреть ответы API после запуска коллекции;
3. Нажмите кнопку `Run <название коллекции>`;
4. В центре экрана отобразится результат запуска коллекции и тестов. Провалившиеся тесты можно отфильтровать, перейдя во вкладку `Failed`.
   Посмотрите детали выполненного запроса и полученного ответа: для этого нажмите на тест.

P.S.: очистка базы данных от тестовых постов и комментариев в случае падения тестов не гарантирована.

## Ограничения от разработчиков Postman

В бесплатной версии программы Postman есть техническое ограничение: коллекцию можно беспрепятственно запускать 25 раз в месяц.  
После исчерпания этого лимита Postman не превратится в тыкву: он по-прежнему будет запускать коллекции, но запуск иногда будет блокироваться на 30 секунд (иногда дважды подряд), и в это время в интерфейсе программы будет появляться предложение приобрести платную версию.  
Вы можете купить платную версию, а можете просто продолжить пользоваться бесплатной версией, время от времени прерываясь на просмотр рекламы.

Для отправки отдельных запросов никаких ограничений нет.
```

## api_yatube/postman_collection/set_up_data.sh

```bash
case "$OSTYPE" in
    msys*)    python=python ;;
    cygwin*)  python=python ;;
    *)        python=python3 ;;
esac

cd ../yatube_api/
$python manage.py migrate
$python manage.py flush --no-input
echo "from django.contrib.auth import get_user_model; User = get_user_model(); \
     u, _ = User.objects.get_or_create(username='root'); u.is_superuser = True; u.is_staff = True; u.email = 'root@admin.ru'; u.set_password('5eCretPaSsw0rD'); u.save(); \
     u, _ = User.objects.get_or_create(username='regular_user'); u.is_superuser = False; u.is_staff = False; u.email = 'user@not-admin.ru'; u.set_password('iWannaBeAdmin'); u.save(); \
     from posts.models import Group; Group.objects.get_or_create(title='TestGroup', slug='test-group', description='Some text.');" | $python manage.py shell >/dev/null 2>&1
echo "Setup done."

```

## api_yatube/project.md

```markdown

```

## api_yatube/pytest.ini

```ini
[pytest]
python_paths = yatube_api/
DJANGO_SETTINGS_MODULE = yatube_api.settings
norecursedirs = env/*
addopts = -vv -p no:cacheprovider
testpaths = tests/
python_files = test_*.py

```

## api_yatube/requirements.txt

```txt
asgiref==3.5.2
attrs==22.1.0
Django==3.2
djangorestframework==3.12.4
iniconfig==1.1.1
packaging==21.3
Pillow==9.3.0
pluggy==0.13.1
py==1.11.0
pyparsing==3.0.9
pytest==6.2.4
pytest-django==4.4.0
pytest-pythonpath==0.7.3
pytz==2022.6
sqlparse==0.4.3
toml==0.10.2
```

## api_yatube/setup.cfg

```cfg
[flake8]
ignore =
    W503,
    F811
exclude =
    tests/,
    */migrations/,
    venv/,
    env/
per-file-ignores =
    */settings.py:E501
max-complexity = 10

```

## api_yatube/test.http

```http
POST http://127.0.0.1:8000/api/v1/api-token-auth/
content-type: application/json

{
    "username": "orbuzik",
    "password": "qwerty12345"
}

###

GET http://127.0.0.1:8000/api/v1/posts/
Authorization: Token eccd99424e05853f16f0feaf09ba179d8c3e8eb6

###

GET http://127.0.0.1:8000/api/v1/groups/
Authorization: Token eccd99424e05853f16f0feaf09ba179d8c3e8eb6

###

POST http://127.0.0.1:8000/api/v1/posts/
Authorization: Token eccd99424e05853f16f0feaf09ba179d8c3e8eb6
content-type: application/json

{
    "text": "Вечером собрались в редакции Русской мысли, чтобы поговорить о народном театре. Проект Шехтеля всем нравится.",
}
```

## api_yatube/test_results.md

```markdown
============================= test session starts ==============================
platform darwin -- Python 3.10.11, pytest-6.2.4, py-1.11.0, pluggy-0.13.1 -- /Users/orbuzik/Documents/API-интерфейсы/Спринт 9/api_yatube/.venv/bin/python3.10
django: settings: yatube_api.settings (from ini)
rootdir: /Users/orbuzik/Documents/API-интерфейсы/Спринт 9/api_yatube, configfile: pytest.ini, testpaths: tests/
plugins: pythonpath-0.7.3, django-4.4.0
collecting ... collected 49 items

tests/test_auth.py::TestAuthAPI::test_auth PASSED [ 2%]
tests/test_auth.py::TestAuthAPI::test_auth_with_invalid_data PASSED [ 4%]
tests/test_comment.py::TestCommentAPI::test_comments_not_found PASSED [ 6%]
tests/test_comment.py::TestCommentAPI::test_comments_get_unauth PASSED [ 8%]
tests/test_comment.py::TestCommentAPI::test_comment_author_and_post_are_read_only PASSED [ 10%]
tests/test_comment.py::TestCommentAPI::test_comments_id_available PASSED [ 12%]
tests/test_comment.py::TestCommentAPI::test_comments_id_unauth_get PASSED [ 14%]
tests/test_comment.py::TestCommentAPI::test_comment_id_auth_get PASSED [ 16%]
tests/test_post.py::TestPostAPI::test_post_not_found PASSED [ 18%]
tests/test_post.py::TestPostAPI::test_post_not_auth PASSED [ 20%]
tests/test_post.py::TestPostAPI::test_post_get_current PASSED [ 22%]
tests/test_comment.py::TestCommentAPI::test_comments_get PASSED [ 24%]
tests/test_comment.py::TestCommentAPI::test_comments_post_auth_with_valid_data PASSED [ 26%]
tests/test_comment.py::TestCommentAPI::test_comments_auth_post_with_invalid_data PASSED [ 28%]
tests/test_comment.py::TestCommentAPI::test_comment_change_by_auth_with_valid_data[put] PASSED [ 30%]
tests/test_comment.py::TestCommentAPI::test_comment_change_by_auth_with_valid_data[patch] PASSED [ 32%]
tests/test_comment.py::TestCommentAPI::test_comment_change_by_not_author_with_valid_data[put] PASSED [ 34%]
tests/test_comment.py::TestCommentAPI::test_comment_change_by_not_author_with_valid_data[patch] PASSED [ 36%]
tests/test_comment.py::TestCommentAPI::test_comment_change_not_auth_with_valid_data[put] PASSED [ 38%]
tests/test_comment.py::TestCommentAPI::test_comment_change_not_auth_with_valid_data[patch] PASSED [ 40%]
tests/test_comment.py::TestCommentAPI::test_comment_delete_by_author PASSED [ 42%]
tests/test_comment.py::TestCommentAPI::test_comment_delete_by_unauth PASSED [ 44%]
tests/test_group.py::TestGroupAPI::test_group_not_found PASSED [ 46%]
tests/test_group.py::TestGroupAPI::test_group_not_auth PASSED [ 48%]
tests/test_group.py::TestGroupAPI::test_group_auth_get PASSED [ 51%]
tests/test_group.py::TestGroupAPI::test_group_create PASSED [ 53%]
tests/test_group.py::TestGroupAPI::test_group_get_post PASSED [ 55%]
tests/test_group.py::TestGroupAPI::test_group_page_not_found PASSED [ 57%]
tests/test_group.py::TestGroupAPI::test_group_page_not_auth PASSED [ 59%]
tests/test_group.py::TestGroupAPI::test_group_page_auth_get PASSED [ 61%]
tests/test_post.py::TestPostAPI::test_posts_auth_get PASSED [ 63%]
tests/test_post.py::TestPostAPI::test_post_create_auth_with_invalid_data PASSED [ 65%]
tests/test_post.py::TestPostAPI::test_post_create_auth_with_valid_data PASSED [ 67%]
tests/test_post.py::TestPostAPI::test_post_unauth_create PASSED [ 69%]
tests/test_post.py::TestPostAPI::test_post_change_auth_with_valid_data[put] PASSED [ 71%]
tests/test_post.py::TestPostAPI::test_post_change_auth_with_valid_data[patch] PASSED [ 73%]
tests/test_post.py::TestPostAPI::test_post_change_not_auth_with_valid_data[put] PASSED [ 75%]
tests/test_post.py::TestPostAPI::test_post_change_not_auth_with_valid_data[patch] PASSED [ 77%]
tests/test_post.py::TestPostAPI::test_post_change_not_author_with_valid_data[put] PASSED [ 79%]
tests/test_post.py::TestPostAPI::test_post_change_not_author_with_valid_data[patch] PASSED [ 81%]
tests/test_post.py::TestPostAPI::test_post_patch_auth_with_invalid_data[put] PASSED [ 83%]
tests/test_post.py::TestPostAPI::test_post_patch_auth_with_invalid_data[patch] PASSED [ 85%]
tests/test_post.py::TestPostAPI::test_post_delete_by_author PASSED [ 87%]
tests/test_post.py::TestPostAPI::test_post_delete_not_author PASSED [ 89%]
tests/test_post.py::TestPostAPI::test_post_unauth_delete_current PASSED [ 91%]
tests/test_settings.py::TestSettings::test_drf_in_installed_apps[rest_framework] PASSED [ 93%]
tests/test_settings.py::TestSettings::test_drf_in_installed_apps[rest_framework.authtoken] PASSED [ 95%]
tests/test_settings.py::TestSettings::test_api_in_installed_apps PASSED [ 97%]
tests/test_settings.py::TestSettings::test_auth_settings PASSED [100%]

============================= 49 passed in 11.25s ==============================
```

## api_yatube/tests/**init**.py

```python

```

## api_yatube/tests/**pycache**/**init**.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/**pycache**/conftest.cpython-310-pytest-6.2.4.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/**pycache**/test_auth.cpython-310-pytest-6.2.4.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/**pycache**/test_comment.cpython-310-pytest-6.2.4.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/**pycache**/test_group.cpython-310-pytest-6.2.4.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/**pycache**/test_post.cpython-310-pytest-6.2.4.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/**pycache**/test_settings.cpython-310-pytest-6.2.4.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/conftest.py

```python
import sys
import os


root_dir = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
sys.path.append(root_dir)

BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
root_dir_content = os.listdir(BASE_DIR)
PROJECT_DIR_NAME = 'yatube_api'

if (
        PROJECT_DIR_NAME not in root_dir_content
        or not os.path.isdir(os.path.join(BASE_DIR, PROJECT_DIR_NAME))
):
    raise AssertionError(
        f'В директории `{BASE_DIR}` не найдена папка c проектом '
        f'`{PROJECT_DIR_NAME}`. Убедитесь, что у вас верная структура '
        'проекта.'
    )

MANAGE_PATH = os.path.join(BASE_DIR, PROJECT_DIR_NAME)
project_dir_content = os.listdir(MANAGE_PATH)
FILENAME = 'manage.py'

if FILENAME not in project_dir_content:
    raise AssertionError(
        f'В директории `{MANAGE_PATH}` не найден файл `{FILENAME}`. '
        f'Убедитесь, что у вас верная структура проекта.'
    )

pytest_plugins = [
    'tests.fixtures.fixture_user',
    'tests.fixtures.fixture_data',
]

```

## api_yatube/tests/fixtures/**init**.py

```python

```

## api_yatube/tests/fixtures/**pycache**/**init**.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/fixtures/**pycache**/fixture_data.cpython-310-pytest-6.2.4.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/fixtures/**pycache**/fixture_user.cpython-310-pytest-6.2.4.pyc

```pyc
[Binary file content not included]
```

## api_yatube/tests/fixtures/fixture_data.py

```python
import tempfile

import pytest


@pytest.fixture
def post(user):
    from posts.models import Post
    image = tempfile.NamedTemporaryFile(suffix=".jpg").name
    return Post.objects.create(text='Тестовый пост 1',
                               author=user, image=image)


@pytest.fixture
def post_2(user, group_1):
    from posts.models import Post
    return Post.objects.create(text='Тестовый пост 2',
                               author=user, group=group_1)


@pytest.fixture
def comment_1_post(post, user):
    from posts.models import Comment
    return Comment.objects.create(author=user, post=post, text='Коммент 1')


@pytest.fixture
def comment_2_post(post, another_user):
    from posts.models import Comment
    return Comment.objects.create(author=another_user, post=post,
                                  text='Коммент 2')


@pytest.fixture
def another_post(another_user):
    from posts.models import Post
    return Post.objects.create(text='Тестовый пост 2', author=another_user)


@pytest.fixture
def comment_1_another_post(another_post, user):
    from posts.models import Comment
    return Comment.objects.create(author=user, post=another_post,
                                  text='Коммент 12')


@pytest.fixture
def group_1():
    from posts.models import Group
    return Group.objects.create(title='Группа 1', slug='group_1')


@pytest.fixture
def group_2():
    from posts.models import Group
    return Group.objects.create(title='Группа 2', slug='group_2')

```

## api_yatube/tests/fixtures/fixture_user.py

```python
import pytest


@pytest.fixture
def password():
    return '1234567'


@pytest.fixture
def user(django_user_model, password):
    return django_user_model.objects.create_user(
        username='TestUser',
        password=password
    )


@pytest.fixture
def another_user(django_user_model, password):
    return django_user_model.objects.create_user(
        username='TestUser2',
        password=password
    )


@pytest.fixture
def token(user):
    from rest_framework.authtoken.models import Token
    token, _ = Token.objects.get_or_create(user=user)
    return token.key


@pytest.fixture
def user_client(token):
    from rest_framework.test import APIClient

    client = APIClient()
    client.credentials(HTTP_AUTHORIZATION=f'Token {token}')
    return client

```

## api_yatube/tests/test_auth.py

```python
from http import HTTPStatus


class TestAuthAPI:

    def test_auth(self, client, user, password):
        response = client.post(
            '/api/v1/api-token-auth/',
            data={'username': user.username, 'password': password}
        )
        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/api-token-auth/` не найдена, проверьте этот '
            'адрес в *urls.py*.'
        )
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что POST-запрос к `/api/v1/api-token-auth/` '
            'возвращает ответ с кодом 200.'
        )

        auth_data = response.json()
        assert 'token' in auth_data, (
            'Проверьте, что ответ на POST-запрос с валидными данными к '
            '`/api/v1/api-token-auth/` содержит токен.'
        )

    def test_auth_with_invalid_data(self, client, user):
        response = client.post('/api/v1/api-token-auth/', data={})
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            'Проверьте, что POST-запрос к `/api/v1/api-token-auth/` '
            'с некорректными данными возвращает ответ со статусовм 400.'
        )

```

## api_yatube/tests/test_comment.py

```python
from http import HTTPStatus

import pytest

from posts.models import Comment


class TestCommentAPI:
    TEXT_FOR_COMMENT = 'Новый комментарий'

    def test_comments_not_found(self, user_client, post):
        response = user_client.get(f'/api/v1/posts/{post.id}/comments/')
        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/posts/{post.id}/comments/` не найдена, '
            'проверьте этот адрес в *urls.py*.'
        )

    def test_comments_get_unauth(self, client, post, comment_1_post):
        response = client.get(f'/api/v1/posts/{post.id}/comments/')
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что GET-запрос от неавторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ со '
            'статусом 401.'
        )

    def check_comment_data(self,
                           response_data,
                           request_method_and_url,
                           db_comment=None):
        expected_fields = ('id', 'text', 'author', 'post', 'created')
        for field in expected_fields:
            assert field in response_data, (
                'Проверьте, что при запросе авторизованного пользователя к '
                f'{request_method_and_url} в ответе содержится поле '
                f'комментария `{field}`.'
            )
        if db_comment:
            assert response_data['author'] == db_comment.author.username, (
                'Проверьте, что при запросе авторизованного пользователя к '
                f'{request_method_and_url} в ответе содержится поле '
                'комментария `author`, в котором указан `username` автора.'
            )
            assert response_data['id'] == db_comment.id, (
                'Проверьте, что при запросе авторизованного пользователя на '
                f'{request_method_and_url} в ответе содержится корректный '
                '`id` комментария.'
            )

    @pytest.mark.django_db(transaction=True)
    def test_comments_get(self, user_client, post, comment_1_post,
                          comment_2_post, comment_1_another_post):
        response = user_client.get(f'/api/v1/posts/{post.id}/comments/')
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращается ответ со '
            'статусом 200.'
        )
        test_data = response.json()
        assert isinstance(test_data, list), (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращаются данные в виде '
            'списка.'
        )
        assert len(test_data) == Comment.objects.filter(post=post).count(), (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращаются данные обо '
            'всех комментариях к посту.'
        )

        comment = Comment.objects.filter(post=post).first()
        test_comment = test_data[0]
        self.check_comment_data(
            test_comment,
            'GET-запрос к `/api/v1/posts/{post.id}/comments/`',
            db_comment=comment
        )

    @pytest.mark.django_db(transaction=True)
    def test_comments_post_auth_with_valid_data(self, user_client, post,
                                                user, another_user):
        comments_count = Comment.objects.count()
        data = {
            'text': self.TEXT_FOR_COMMENT,
        }
        response = user_client.post(
            f'/api/v1/posts/{post.id}/comments/',
            data=data
        )
        assert response.status_code == HTTPStatus.CREATED, (
            'Проверьте, что POST-запрос с корректными данными от '
            'авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ со '
            'статусом 201.'
        )

        test_data = response.json()
        assert isinstance(test_data, dict), (
            'Проверьте, что POST-запрос авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ, '
            'содержащий данные нового комментария в виде словаря.'
        )
        assert test_data.get('text') == data['text'], (
            'Проверьте, что POST-запрос авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ, '
            'содержащий текст нового комментария в неизменном виде.'
        )
        self.check_comment_data(
            test_data,
            'POST-запрос к `/api/v1/posts/{post.id}/comments/`'
        )

        assert test_data.get('author') == user.username, (
            'Проверьте, что при создании '
            'комментария через POST-запрос к '
            '`/api/v1/posts/{post.id}/comments/` авторизованный пользователь '
            'получит ответ, в котором будет поле `author` с именем '
            'пользователя, отправившего запрос.'
        )
        assert comments_count + 1 == Comment.objects.count(), (
            'Проверьте, что POST-запрос авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` создает новый комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_comments_auth_post_with_invalid_data(self, user_client, post):
        comments_count = Comment.objects.count()

        response = user_client.post(
            f'/api/v1/posts/{post.id}/comments/',
            data={}
        )
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            'Проверьте, что POST-запрос с некорректными данными от '
            'авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ со '
            'статусом 400.'
        )
        assert comments_count == Comment.objects.count(), (
            'Проверьте, что при POST-запросе с некорректными данными к '
            '`/api/v1/posts/{post.id}/comments/` новый комментарий не '
            'создаётся.'
        )

    def test_comment_author_and_post_are_read_only(self, user_client, post):
        response = user_client.post(
            f'/api/v1/posts/{post.id}/comments/',
            data={}
        )
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            'Проверьте, что POST-запрос с некорректными данными от '
            'авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ со '
            'статусом 400.'
        )
        data = set(response.json())
        assert not {'author', 'post'}.intersection(data), (
            'Проверьте, что для эндпоинта '
            '`/api/v1/posts/{post.id}/comments/` для полей `author` и `post` '
            'установлен свойство "Только для чтения"'
        )


    def test_comments_id_available(self, user_client, post, comment_1_post):
        response = user_client.get(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/posts/{post.id}/comments/{comment.id}/` '
            'не найдена, проверьте этот адрес в *urls.py*.'
        )

    def test_comments_id_unauth_get(self, client, post, comment_1_post):
        response = client.get(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что для неавторизованного пользователя GET-запрос на '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ со статусом 401.'
        )

    def test_comment_id_auth_get(self, user_client, post,
                                 comment_1_post, user):
        response = user_client.get(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code == HTTPStatus.OK, (
            'Страница `/api/v1/posts/{post.id}/comments/{comment.id}/` не '
            'найдена, проверьте этот адрес в *urls.py*.'
        )

        test_data = response.json()
        assert test_data.get('text') == comment_1_post.text, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ, содержащий текст комментария.'
        )
        assert test_data.get('author') == user.username, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ, содержащий `username` автора комментария.'
        )
        assert test_data.get('post') == post.id, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ, содержащий `id` поста.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_comment_change_by_auth_with_valid_data(self,
                                                    user_client,
                                                    post,
                                                    comment_1_post,
                                                    comment_2_post,
                                                    http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/',
            data={'text': self.TEXT_FOR_COMMENT}
        )
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.OK, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` '
            'возвращает ответ со статусом 200.'
        )

        db_comment = Comment.objects.filter(id=comment_1_post.id).first()
        assert db_comment, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` не '
            'удаляет редактируемый комментарий.'
        )
        assert db_comment.text == self.TEXT_FOR_COMMENT, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` '
            'вносит изменения в комментарий.'
        )
        response_data = response.json()
        self.check_comment_data(
            response_data,
            request_method_and_url=(
                f'{http_method} -запрос к '
                '`/api/v1/posts/{post.id}/comments/{comment.id}/`'
            ),
            db_comment=db_comment
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_comment_change_by_not_author_with_valid_data(self,
                                                          user_client,
                                                          post,
                                                          comment_2_post,
                                                          http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(
            f'/api/v1/posts/{post.id}/comments/{comment_2_post.id}/',
            data={'text': self.TEXT_FOR_COMMENT}
        )
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.FORBIDDEN, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` для '
            'чужого комментария возвращает ответ со статусом 403.'
        )

        db_comment = Comment.objects.filter(id=comment_2_post.id).first()
        assert db_comment.text != self.TEXT_FOR_COMMENT, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` для '
            'чужого комментария не вносит изменения в комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_comment_change_not_auth_with_valid_data(self,
                                                     client,
                                                     post,
                                                     comment_1_post,
                                                     http_method):
        request_func = getattr(client, http_method)
        response = request_func(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/',
            data={'text': self.TEXT_FOR_COMMENT}
        )
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            f'Проверьте, что для неавторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` '
            'возвращает ответ со статусом 401.'
        )
        db_comment = Comment.objects.filter(id=comment_1_post.id).first()
        assert db_comment.text != self.TEXT_FOR_COMMENT, (
            f'Проверьте, что для неавторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` не '
            'вносит изменения в комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_comment_delete_by_author(self, user_client,
                                      post, comment_1_post):
        response = user_client.delete(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code == HTTPStatus.NO_CONTENT, (
            'Проверьте, что для автора комментария DELETE-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ со статусом 204.'
        )

        test_comment = Comment.objects.filter(id=post.id).first()
        assert not test_comment, (
            'Проверьте, что DELETE-запрос автора комментария к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` удаляет '
            'комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_comment_delete_by_author(self, user_client,
                                      post, comment_2_post):
        response = user_client.delete(
            f'/api/v1/posts/{post.id}/comments/{comment_2_post.id}/'
        )
        assert response.status_code == HTTPStatus.FORBIDDEN, (
            'Проверьте, что DELETE-запрос авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` чужого '
            'комментария возвращает ответ со статусом 403.'
        )
        db_comment = Comment.objects.filter(id=comment_2_post.id).first()
        assert db_comment, (
            'Проверьте, что для авторизованного пользователя DELETE-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` для чужого '
            'комментария не удаляет комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_comment_delete_by_unauth(self, client, post, comment_1_post):
        response = client.delete(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что для неавторизованного пользователя DELETE-запрос '
            'к `/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ со статусом 401.'
        )
        db_comment = Comment.objects.filter(id=comment_1_post.id).first()
        assert db_comment, (
            'Проверьте, что для неавторизованного пользователя DELETE-запрос '
            'к `/api/v1/posts/{post.id}/comments/{comment.id}/` не удаляет '
            'комментарий.'
        )

```

## api_yatube/tests/test_group.py

```python
from http import HTTPStatus

import pytest

from posts.models import Group, Post


class TestGroupAPI:

    @pytest.mark.django_db(transaction=True)
    def test_group_not_found(self, client, group_1):
        response = client.get('/api/v1/groups/')

        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/groups/` не найдена, проверьте этот адрес в '
            '*urls.py*.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_not_auth(self, client, group_1):
        response = client.get('/api/v1/groups/')
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что `/api/v1/groups/` при запросе от '
            'неавторизованного пользователя возвращаете ответ со статусом '
            '401.'
        )

    def check_group_info(self, group_info, url):
        assert 'id' in group_info, (
            f'Ответ на GET-запрос к `{url}` содержит неполную информацию о '
            'группе. Проверьте, что поле `id` добавлено в список полей '
            '`fields` сериализатора модели `Group`.'
        )
        assert 'title' in group_info, (
            f'Ответ на GET-запрос к `{url}` содержит неполную информацию о '
            'группе. Проверьте, что поле `title` добавлено в список полей '
            '`fields` сериализатора модели `Group`.'
        )
        assert 'slug' in group_info, (
            f'Ответ на GET-запрос к `{url}` содержит неполную информацию о '
            'группе. Проверьте, что поле `slug` добавлено в список полей '
            '`fields` сериализатора модели `Group`.'
        )
        assert 'description' in group_info, (
            f'Ответ на GET-запрос к `{url}` содержит неполную информацию о '
            'группе. Проверьте, что поле `description` добавлено в список '
            'полей `fields` сериализатора модели `Group`.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_auth_get(self, user_client, group_1, group_2):
        response = user_client.get('/api/v1/groups/')
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/groups/` возвращает ответ со статусом 200.'
        )

        test_data = response.json()
        assert isinstance(test_data, list), (
            'Проверьте, что для авторизованного пользователя '
            'GET-запрос к `/api/v1/groups/` возвращает информацию о группах '
            'в виде списка.'
        )
        assert len(test_data) == Group.objects.count(), (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/groups/` возвращает информацию обо всех существующих '
            'группах.'
        )

        test_group = test_data[0]
        self.check_group_info(test_group, '/api/v1/groups/')

    @pytest.mark.django_db(transaction=True)
    def test_group_create(self, user_client, group_1, group_2):
        data = {'title': 'Группа  номер 3'}
        response = user_client.post('/api/v1/groups/', data=data)
        assert response.status_code == HTTPStatus.METHOD_NOT_ALLOWED, (
            'Убедитесь, что группу можно создавать только через админку, '
            'а при попытке создать ее через API возвращается статус 405.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_get_post(self, user_client, post_2):
        response = user_client.get('/api/v1/posts/')
        assert response.status_code == HTTPStatus.OK, (
            'Страница `/api/v1/posts/` не найдена, проверьте этот адрес в '
            '*urls.py*.'
        )

        test_data = response.json()
        assert len(test_data) == Post.objects.count(), (
            'Проверьте, что при GET-запросе к `/api/v1/posts/` '
            'в возвращаются и посты, принадлежащие группам.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_page_not_found(self, client, group_1):
        response = client.get(f'/api/v1/groups/{group_1.id}/')
        assert response.status_code != 404, (
            'Страница `/api/v1/groups/{group_id}` не найдена, проверьте этот '
            'адрес в *urls.py*.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_page_not_auth(self, client, group_1):
        response = client.get(f'/api/v1/groups/{group_1.id}/')
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что при запросе от неавторизованного пользователя к '
            '`/api/v1/groups/{group_id}/` возвращается ответ со статусом '
            '401.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_page_auth_get(self, user_client, group_1):
        response = user_client.get(f'/api/v1/groups/{group_1.id}/')
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/groups/{group_id}/` возвращается ответ со статусом 200.'
        )

        test_data = response.json()
        assert isinstance(test_data, dict), (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/groups/{group_id}/` информация о группе возвращается в '
            'виде словаря.'
        )
        self.check_group_info(test_data, '/api/v1/groups/{group_id}/')

```

## api_yatube/tests/test_post.py

```python
from http import HTTPStatus

import pytest

from posts.models import Post


class TestPostAPI:
    VALID_DATA = {'text': 'Поменяли текст статьи'}

    def test_post_not_found(self, client, post):
        response = client.get('/api/v1/posts/')

        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/posts/` не найдена, проверьте этот адрес в '
            '*urls.py*.'
        )

    def test_post_not_auth(self, client, post):
        response = client.get('/api/v1/posts/')

        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что при GET-запросе неавторизованного пользователя к '
            '`/api/v1/posts/` возвращается ответ со статусом 401.'
        )

    def check_post_data(self,
                        response_data,
                        request_method_and_url,
                        db_post=None):
        expected_fields = ('id', 'text', 'author', 'pub_date')
        for field in expected_fields:
            assert field in response_data, (
                'Проверьте, что для авторизованного пользователя ответ на '
                f'{request_method_and_url} содержит поле `{field}` постов.'
            )
        if db_post:
            assert response_data['author'] == db_post.author.username, (
                'Проверьте, что при запросе авторизованного пользователя к '
                f'{request_method_and_url} ответ содержит поле `author` с '
                'именем автора каждого из постов.'
            )
            assert response_data['id'] == db_post.id, (
                'Проверьте, что при запросе авторизованного пользователя к '
                f'{request_method_and_url} в ответе содержится корректный '
                '`id` поста.'
            )

    @pytest.mark.django_db(transaction=True)
    def test_posts_auth_get(self, user_client, post, another_post):
        response = user_client.get('/api/v1/posts/')
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/` возвращает статус 200.'
        )

        test_data = response.json()
        assert isinstance(test_data, list), (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/` возвращает список.'
        )

        assert len(test_data) == Post.objects.count(), (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/` возвращает список всех постов.'
        )

        db_post = Post.objects.first()
        test_post = test_data[0]
        self.check_post_data(
            test_post,
            'GET-запрос к `/api/v1/posts/`',
            db_post
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_create_auth_with_invalid_data(self, user_client):
        posts_count = Post.objects.count()
        response = user_client.post('/api/v1/posts/', data={})
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            'Проверьте, что для авторизованного пользователя POST-запрос с '
            'некорректными данными к `/api/v1/posts/` возвращает ответ со '
            'статусом 400.'
        )
        assert posts_count == Post.objects.count(), (
            'Проверьте, что POST-запрос к `/api/v1/posts/` с некорректными '
            'данными не создает новый пост.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_create_auth_with_valid_data(self, user_client, user):
        post_count = Post.objects.count()

        data = {'text': 'Статья номер 3'}
        response = user_client.post('/api/v1/posts/', data=data)
        assert response.status_code == HTTPStatus.CREATED, (
            'Проверьте, что для авторизованного пользователя  POST-запрос с '
            'корректными данными к `/api/v1/posts/` возвращает ответ со '
            'статусом 201.'
        )

        test_data = response.json()
        assert isinstance(test_data, dict), (
            'Проверьте, что для авторизованного пользователя POST-запрос к '
            '`/api/v1/posts/` возвращает ответ, содержащий данные нового '
            'поста в виде словаря.'
        )
        self.check_post_data(test_data, 'POST-запрос к `/api/v1/posts/`')
        assert test_data.get('text') == data['text'], (
            'Проверьте, что для авторизованного пользователя POST-запрос к '
            '`/api/v1/posts/` возвращает ответ, содержащий текст нового '
            'поста в неизменном виде.'
        )
        assert test_data.get('author') == user.username, (
            'Проверьте, что для авторизованного пользователя при создании '
            'поста через POST-запрос к `/api/v1/posts/` ответ содержит поле '
            '`author` с именем пользователя, отправившего запрос.'
        )
        assert post_count + 1 == Post.objects.count(), (
            'Проверьте, что POST-запрос с корректными данными от '
            'авторизованного пользователя к `/api/v1/posts/` создает новый '
            'пост.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_unauth_create(self, client, user, another_user):
        posts_conut = Post.objects.count()

        data = {'author': another_user.id, 'text': 'Статья номер 3'}
        response = client.post('/api/v1/posts/', data=data)
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что POST-запрос неавторизованного пользователя к '
            '`/api/v1/posts/` возвращает ответ со статусом 401.'
        )

        assert posts_conut == Post.objects.count(), (
            'Проверьте, что POST-запрос неавторизованного пользователя к '
            '`/api/v1/posts/` не создает новый пост.'
        )

    def test_post_get_current(self, user_client, post):
        response = user_client.get(f'/api/v1/posts/{post.id}/')

        assert response.status_code == HTTPStatus.OK, (
            'Страница `/api/v1/posts/{id}/` не найдена, проверьте этот адрес '
            'в *urls.py*.'
        )

        test_data = response.json()
        self.check_post_data(
            test_data,
            'GET-запрос к `/api/v1/posts/{id}/`',
            post
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_post_change_auth_with_valid_data(self, user_client, post,
                                              http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(f'/api/v1/posts/{post.id}/',
                                data=self.VALID_DATA)
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.OK, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{id}/` вернётся ответ со статусом '
            '200.'
        )

        test_post = Post.objects.filter(id=post.id).first()
        assert test_post, (
            f'Проверьте, что {http_method}-запрос авторизованного '
            'пользователя к `/api/v1/posts/{id}/` не удаляет редактируемый '
            'пост.'
        )
        assert test_post.text == self.VALID_DATA['text'], (
            f'Проверьте, что {http_method}-запрос авторизованного '
            'пользователя к `/api/v1/posts/{id}/` вносит изменения в пост.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_post_change_not_auth_with_valid_data(self, client, post,
                                                  http_method):
        request_func = getattr(client, http_method)
        response = request_func(f'/api/v1/posts/{post.id}/',
                                data=self.VALID_DATA)
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            f'Проверьте, что {http_method}-запрос неавторизованного '
            'пользователя к `/api/v1/posts/{id}/` возвращает ответ со '
            'статусом 401.'
        )
        db_post = Post.objects.filter(id=post.id).first()
        assert db_post.text != self.VALID_DATA['text'], (
            f'Проверьте, что {http_method}'
            '-запрос неавторизованного пользователя к `/api/v1/posts/{id}/` '
            'не вносит изменения в пост.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_post_change_not_author_with_valid_data(self, user_client,
                                                    another_post, http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(f'/api/v1/posts/{another_post.id}/',
                                data=self.VALID_DATA)
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.FORBIDDEN, (
            f'Проверьте, что {http_method}'
            '-запрос авторизованного пользователя к `/api/v1/posts/{id}/` '
            'для чужого поста возвращает ответ со статусом 403.'
        )

        db_post = Post.objects.filter(id=another_post.id).first()
        assert db_post.text != self.VALID_DATA['text'], (
            f'Проверьте, что {http_method}'
            '-запрос авторизованного пользователя к `/api/v1/posts/{id}/` '
            'для чужого поста не вносит изменения в пост.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_post_patch_auth_with_invalid_data(self, user_client, post,
                                               http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(f'/api/v1/posts/{post.id}/',
                                data={'text': {}},
                                format='json')
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            f'Проверьте, что {http_method}'
            '-запрос с некорректными данными от авторизованного пользователя '
            'к `/api/v1/posts/{id}/` возвращает ответ с кодом 400.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_delete_by_author(self, user_client, post):
        response = user_client.delete(f'/api/v1/posts/{post.id}/')
        assert response.status_code == HTTPStatus.NO_CONTENT, (
            'Проверьте, что для автора поста DELETE-запрос к '
            ' `/api/v1/posts/{id}/` возвращает ответ со статусом 204.'
        )

        test_post = Post.objects.filter(id=post.id).first()
        assert not test_post, (
            'Проверьте, что DELETE-запрос автора поста к '
            ' `/api/v1/posts/{id}/` удаляет этот пост.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_delete_not_author(self, user_client, another_post):
        response = user_client.delete(f'/api/v1/posts/{another_post.id}/')
        assert response.status_code == HTTPStatus.FORBIDDEN, (
            'Проверьте, что DELETE-запрос авторизованного пользователя к '
            '`/api/v1/posts/{id}/` чужого поста '
            'вернёт ответ со статусом 403.'
        )

        test_post = Post.objects.filter(id=another_post.id).first()
        assert test_post, (
            'Проверьте, что авторизованный пользователь не может удалить '
            'чужой пост.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_unauth_delete_current(self, client, post):
        response = client.delete(f'/api/v1/posts/{post.id}/')
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что DELETE-запрос неавторизованного пользователя '
            'к `/api/v1/posts/{id}/` вернёт ответ со статусом 401.'
        )
        test_post = Post.objects.filter(id=post.id).first()
        assert test_post, (
            'Проверьте, что DELETE-запрос неавторизованного пользователя '
            'к `/api/v1/posts/{id}/` не удаляет запрошенный пост.'
        )

```

## api_yatube/tests/test_settings.py

```python
import pytest
from django.conf import settings


class TestSettings:

    @pytest.mark.parametrize('app', ('rest_framework',
                                     'rest_framework.authtoken'))
    def test_drf_in_installed_apps(self, app):
        assert hasattr(settings, 'INSTALLED_APPS'), (
            'Убедитель, что настройки проекта содержат переменную '
            '`INSTALLED_APPS`.'
        )
        assert app in settings.INSTALLED_APPS, (
            f'`{app}` отсутствует в `INSTALLED_APPS` в настройках '
            'приложения. Убедитесь, что необходимые модули Django '
            'REST Framework добавлены в `INSTALLED_APPS` в настройках проекта.'
        )

    def test_api_in_installed_apps(self):
        assert hasattr(settings, 'INSTALLED_APPS'), (
            'Убедитель, что настройки проекта содержат переменную '
            '`INSTALLED_APPS`.'
        )
        assert {'api', 'api.apps.ApiConfig'}.intersection(
            set(settings.INSTALLED_APPS)
        ), (
            'Убедитесь, что приложение `api` добавлено в `INSTALLED_APPS` в '
            'настройках проекта.'
        )

    def test_auth_settings(self):
        assert hasattr(settings, 'REST_FRAMEWORK'), (
            'Проверьте, что настройка `REST_FRAMEWORK` добавлена в файл '
            '`settings.py`'
        )

        assert 'DEFAULT_AUTHENTICATION_CLASSES' in settings.REST_FRAMEWORK, (
            'Проверьте, что добавили ключ `DEFAULT_AUTHENTICATION_CLASSES` в '
            '`REST_FRAMEWORK` файла `settings.py`'
        )
        assert (
            'rest_framework.authentication.TokenAuthentication' in
            settings.REST_FRAMEWORK['DEFAULT_AUTHENTICATION_CLASSES']
        ), (
            'Проверьте, что в списке `DEFAULT_AUTHENTICATION_CLASSES` в '
            '`REST_FRAMEWORK` содержится '
            '`rest_framework.authentication.TokenAuthentication`.'
        )

```

## api_yatube/tests.md

```markdown
# Project Structure
```

├── tests
│ ├── **pycache**
│ ├── fixtures
│ │ ├── **pycache**
│ │ ├── **init**.py
│ │ ├── fixture_data.py
│ │ └── fixture_user.py
│ ├── **init**.py
│ ├── conftest.py
│ ├── test_auth.py
│ ├── test_comment.py
│ ├── test_group.py
│ ├── test_post.py
│ └── test_settings.py
└── pytest.ini

````

# File Contents

## tests/**init**.py

```python

````

## tests/conftest.py

```python
import sys
import os


root_dir = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
sys.path.append(root_dir)

BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
root_dir_content = os.listdir(BASE_DIR)
PROJECT_DIR_NAME = 'yatube_api'

if (
        PROJECT_DIR_NAME not in root_dir_content
        or not os.path.isdir(os.path.join(BASE_DIR, PROJECT_DIR_NAME))
):
    raise AssertionError(
        f'В директории `{BASE_DIR}` не найдена папка c проектом '
        f'`{PROJECT_DIR_NAME}`. Убедитесь, что у вас верная структура '
        'проекта.'
    )

MANAGE_PATH = os.path.join(BASE_DIR, PROJECT_DIR_NAME)
project_dir_content = os.listdir(MANAGE_PATH)
FILENAME = 'manage.py'

if FILENAME not in project_dir_content:
    raise AssertionError(
        f'В директории `{MANAGE_PATH}` не найден файл `{FILENAME}`. '
        f'Убедитесь, что у вас верная структура проекта.'
    )

pytest_plugins = [
    'tests.fixtures.fixture_user',
    'tests.fixtures.fixture_data',
]

```

## tests/fixtures/**init**.py

```python

```

## tests/fixtures/fixture_data.py

```python
import tempfile

import pytest


@pytest.fixture
def post(user):
    from posts.models import Post
    image = tempfile.NamedTemporaryFile(suffix=".jpg").name
    return Post.objects.create(text='Тестовый пост 1',
                               author=user, image=image)


@pytest.fixture
def post_2(user, group_1):
    from posts.models import Post
    return Post.objects.create(text='Тестовый пост 2',
                               author=user, group=group_1)


@pytest.fixture
def comment_1_post(post, user):
    from posts.models import Comment
    return Comment.objects.create(author=user, post=post, text='Коммент 1')


@pytest.fixture
def comment_2_post(post, another_user):
    from posts.models import Comment
    return Comment.objects.create(author=another_user, post=post,
                                  text='Коммент 2')


@pytest.fixture
def another_post(another_user):
    from posts.models import Post
    return Post.objects.create(text='Тестовый пост 2', author=another_user)


@pytest.fixture
def comment_1_another_post(another_post, user):
    from posts.models import Comment
    return Comment.objects.create(author=user, post=another_post,
                                  text='Коммент 12')


@pytest.fixture
def group_1():
    from posts.models import Group
    return Group.objects.create(title='Группа 1', slug='group_1')


@pytest.fixture
def group_2():
    from posts.models import Group
    return Group.objects.create(title='Группа 2', slug='group_2')

```

## tests/fixtures/fixture_user.py

```python
import pytest


@pytest.fixture
def password():
    return '1234567'


@pytest.fixture
def user(django_user_model, password):
    return django_user_model.objects.create_user(
        username='TestUser',
        password=password
    )


@pytest.fixture
def another_user(django_user_model, password):
    return django_user_model.objects.create_user(
        username='TestUser2',
        password=password
    )


@pytest.fixture
def token(user):
    from rest_framework.authtoken.models import Token
    token, _ = Token.objects.get_or_create(user=user)
    return token.key


@pytest.fixture
def user_client(token):
    from rest_framework.test import APIClient

    client = APIClient()
    client.credentials(HTTP_AUTHORIZATION=f'Token {token}')
    return client

```

## tests/test_auth.py

```python
from http import HTTPStatus


class TestAuthAPI:

    def test_auth(self, client, user, password):
        response = client.post(
            '/api/v1/api-token-auth/',
            data={'username': user.username, 'password': password}
        )
        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/api-token-auth/` не найдена, проверьте этот '
            'адрес в *urls.py*.'
        )
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что POST-запрос к `/api/v1/api-token-auth/` '
            'возвращает ответ с кодом 200.'
        )

        auth_data = response.json()
        assert 'token' in auth_data, (
            'Проверьте, что ответ на POST-запрос с валидными данными к '
            '`/api/v1/api-token-auth/` содержит токен.'
        )

    def test_auth_with_invalid_data(self, client, user):
        response = client.post('/api/v1/api-token-auth/', data={})
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            'Проверьте, что POST-запрос к `/api/v1/api-token-auth/` '
            'с некорректными данными возвращает ответ со статусовм 400.'
        )

```

## tests/test_comment.py

```python
from http import HTTPStatus

import pytest

from posts.models import Comment


class TestCommentAPI:
    TEXT_FOR_COMMENT = 'Новый комментарий'

    def test_comments_not_found(self, user_client, post):
        response = user_client.get(f'/api/v1/posts/{post.id}/comments/')
        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/posts/{post.id}/comments/` не найдена, '
            'проверьте этот адрес в *urls.py*.'
        )

    def test_comments_get_unauth(self, client, post, comment_1_post):
        response = client.get(f'/api/v1/posts/{post.id}/comments/')
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что GET-запрос от неавторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ со '
            'статусом 401.'
        )

    def check_comment_data(self,
                           response_data,
                           request_method_and_url,
                           db_comment=None):
        expected_fields = ('id', 'text', 'author', 'post', 'created')
        for field in expected_fields:
            assert field in response_data, (
                'Проверьте, что при запросе авторизованного пользователя к '
                f'{request_method_and_url} в ответе содержится поле '
                f'комментария `{field}`.'
            )
        if db_comment:
            assert response_data['author'] == db_comment.author.username, (
                'Проверьте, что при запросе авторизованного пользователя к '
                f'{request_method_and_url} в ответе содержится поле '
                'комментария `author`, в котором указан `username` автора.'
            )
            assert response_data['id'] == db_comment.id, (
                'Проверьте, что при запросе авторизованного пользователя на '
                f'{request_method_and_url} в ответе содержится корректный '
                '`id` комментария.'
            )

    @pytest.mark.django_db(transaction=True)
    def test_comments_get(self, user_client, post, comment_1_post,
                          comment_2_post, comment_1_another_post):
        response = user_client.get(f'/api/v1/posts/{post.id}/comments/')
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращается ответ со '
            'статусом 200.'
        )
        test_data = response.json()
        assert isinstance(test_data, list), (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращаются данные в виде '
            'списка.'
        )
        assert len(test_data) == Comment.objects.filter(post=post).count(), (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращаются данные обо '
            'всех комментариях к посту.'
        )

        comment = Comment.objects.filter(post=post).first()
        test_comment = test_data[0]
        self.check_comment_data(
            test_comment,
            'GET-запрос к `/api/v1/posts/{post.id}/comments/`',
            db_comment=comment
        )

    @pytest.mark.django_db(transaction=True)
    def test_comments_post_auth_with_valid_data(self, user_client, post,
                                                user, another_user):
        comments_count = Comment.objects.count()
        data = {
            'text': self.TEXT_FOR_COMMENT,
        }
        response = user_client.post(
            f'/api/v1/posts/{post.id}/comments/',
            data=data
        )
        assert response.status_code == HTTPStatus.CREATED, (
            'Проверьте, что POST-запрос с корректными данными от '
            'авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ со '
            'статусом 201.'
        )

        test_data = response.json()
        assert isinstance(test_data, dict), (
            'Проверьте, что POST-запрос авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ, '
            'содержащий данные нового комментария в виде словаря.'
        )
        assert test_data.get('text') == data['text'], (
            'Проверьте, что POST-запрос авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ, '
            'содержащий текст нового комментария в неизменном виде.'
        )
        self.check_comment_data(
            test_data,
            'POST-запрос к `/api/v1/posts/{post.id}/comments/`'
        )

        assert test_data.get('author') == user.username, (
            'Проверьте, что при создании '
            'комментария через POST-запрос к '
            '`/api/v1/posts/{post.id}/comments/` авторизованный пользователь '
            'получит ответ, в котором будет поле `author` с именем '
            'пользователя, отправившего запрос.'
        )
        assert comments_count + 1 == Comment.objects.count(), (
            'Проверьте, что POST-запрос авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` создает новый комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_comments_auth_post_with_invalid_data(self, user_client, post):
        comments_count = Comment.objects.count()

        response = user_client.post(
            f'/api/v1/posts/{post.id}/comments/',
            data={}
        )
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            'Проверьте, что POST-запрос с некорректными данными от '
            'авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ со '
            'статусом 400.'
        )
        assert comments_count == Comment.objects.count(), (
            'Проверьте, что при POST-запросе с некорректными данными к '
            '`/api/v1/posts/{post.id}/comments/` новый комментарий не '
            'создаётся.'
        )

    def test_comment_author_and_post_are_read_only(self, user_client, post):
        response = user_client.post(
            f'/api/v1/posts/{post.id}/comments/',
            data={}
        )
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            'Проверьте, что POST-запрос с некорректными данными от '
            'авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/` возвращает ответ со '
            'статусом 400.'
        )
        data = set(response.json())
        assert not {'author', 'post'}.intersection(data), (
            'Проверьте, что для эндпоинта '
            '`/api/v1/posts/{post.id}/comments/` для полей `author` и `post` '
            'установлен свойство "Только для чтения"'
        )


    def test_comments_id_available(self, user_client, post, comment_1_post):
        response = user_client.get(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/posts/{post.id}/comments/{comment.id}/` '
            'не найдена, проверьте этот адрес в *urls.py*.'
        )

    def test_comments_id_unauth_get(self, client, post, comment_1_post):
        response = client.get(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что для неавторизованного пользователя GET-запрос на '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ со статусом 401.'
        )

    def test_comment_id_auth_get(self, user_client, post,
                                 comment_1_post, user):
        response = user_client.get(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code == HTTPStatus.OK, (
            'Страница `/api/v1/posts/{post.id}/comments/{comment.id}/` не '
            'найдена, проверьте этот адрес в *urls.py*.'
        )

        test_data = response.json()
        assert test_data.get('text') == comment_1_post.text, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ, содержащий текст комментария.'
        )
        assert test_data.get('author') == user.username, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ, содержащий `username` автора комментария.'
        )
        assert test_data.get('post') == post.id, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ, содержащий `id` поста.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_comment_change_by_auth_with_valid_data(self,
                                                    user_client,
                                                    post,
                                                    comment_1_post,
                                                    comment_2_post,
                                                    http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/',
            data={'text': self.TEXT_FOR_COMMENT}
        )
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.OK, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` '
            'возвращает ответ со статусом 200.'
        )

        db_comment = Comment.objects.filter(id=comment_1_post.id).first()
        assert db_comment, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` не '
            'удаляет редактируемый комментарий.'
        )
        assert db_comment.text == self.TEXT_FOR_COMMENT, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` '
            'вносит изменения в комментарий.'
        )
        response_data = response.json()
        self.check_comment_data(
            response_data,
            request_method_and_url=(
                f'{http_method} -запрос к '
                '`/api/v1/posts/{post.id}/comments/{comment.id}/`'
            ),
            db_comment=db_comment
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_comment_change_by_not_author_with_valid_data(self,
                                                          user_client,
                                                          post,
                                                          comment_2_post,
                                                          http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(
            f'/api/v1/posts/{post.id}/comments/{comment_2_post.id}/',
            data={'text': self.TEXT_FOR_COMMENT}
        )
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.FORBIDDEN, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` для '
            'чужого комментария возвращает ответ со статусом 403.'
        )

        db_comment = Comment.objects.filter(id=comment_2_post.id).first()
        assert db_comment.text != self.TEXT_FOR_COMMENT, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` для '
            'чужого комментария не вносит изменения в комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_comment_change_not_auth_with_valid_data(self,
                                                     client,
                                                     post,
                                                     comment_1_post,
                                                     http_method):
        request_func = getattr(client, http_method)
        response = request_func(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/',
            data={'text': self.TEXT_FOR_COMMENT}
        )
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            f'Проверьте, что для неавторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` '
            'возвращает ответ со статусом 401.'
        )
        db_comment = Comment.objects.filter(id=comment_1_post.id).first()
        assert db_comment.text != self.TEXT_FOR_COMMENT, (
            f'Проверьте, что для неавторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{post.id}/comments/{comment.id}/` не '
            'вносит изменения в комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_comment_delete_by_author(self, user_client,
                                      post, comment_1_post):
        response = user_client.delete(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code == HTTPStatus.NO_CONTENT, (
            'Проверьте, что для автора комментария DELETE-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ со статусом 204.'
        )

        test_comment = Comment.objects.filter(id=post.id).first()
        assert not test_comment, (
            'Проверьте, что DELETE-запрос автора комментария к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` удаляет '
            'комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_comment_delete_by_author(self, user_client,
                                      post, comment_2_post):
        response = user_client.delete(
            f'/api/v1/posts/{post.id}/comments/{comment_2_post.id}/'
        )
        assert response.status_code == HTTPStatus.FORBIDDEN, (
            'Проверьте, что DELETE-запрос авторизованного пользователя к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` чужого '
            'комментария возвращает ответ со статусом 403.'
        )
        db_comment = Comment.objects.filter(id=comment_2_post.id).first()
        assert db_comment, (
            'Проверьте, что для авторизованного пользователя DELETE-запрос к '
            '`/api/v1/posts/{post.id}/comments/{comment.id}/` для чужого '
            'комментария не удаляет комментарий.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_comment_delete_by_unauth(self, client, post, comment_1_post):
        response = client.delete(
            f'/api/v1/posts/{post.id}/comments/{comment_1_post.id}/'
        )
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что для неавторизованного пользователя DELETE-запрос '
            'к `/api/v1/posts/{post.id}/comments/{comment.id}/` возвращает '
            'ответ со статусом 401.'
        )
        db_comment = Comment.objects.filter(id=comment_1_post.id).first()
        assert db_comment, (
            'Проверьте, что для неавторизованного пользователя DELETE-запрос '
            'к `/api/v1/posts/{post.id}/comments/{comment.id}/` не удаляет '
            'комментарий.'
        )

```

## tests/test_group.py

```python
from http import HTTPStatus

import pytest

from posts.models import Group, Post


class TestGroupAPI:

    @pytest.mark.django_db(transaction=True)
    def test_group_not_found(self, client, group_1):
        response = client.get('/api/v1/groups/')

        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/groups/` не найдена, проверьте этот адрес в '
            '*urls.py*.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_not_auth(self, client, group_1):
        response = client.get('/api/v1/groups/')
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что `/api/v1/groups/` при запросе от '
            'неавторизованного пользователя возвращаете ответ со статусом '
            '401.'
        )

    def check_group_info(self, group_info, url):
        assert 'id' in group_info, (
            f'Ответ на GET-запрос к `{url}` содержит неполную информацию о '
            'группе. Проверьте, что поле `id` добавлено в список полей '
            '`fields` сериализатора модели `Group`.'
        )
        assert 'title' in group_info, (
            f'Ответ на GET-запрос к `{url}` содержит неполную информацию о '
            'группе. Проверьте, что поле `title` добавлено в список полей '
            '`fields` сериализатора модели `Group`.'
        )
        assert 'slug' in group_info, (
            f'Ответ на GET-запрос к `{url}` содержит неполную информацию о '
            'группе. Проверьте, что поле `slug` добавлено в список полей '
            '`fields` сериализатора модели `Group`.'
        )
        assert 'description' in group_info, (
            f'Ответ на GET-запрос к `{url}` содержит неполную информацию о '
            'группе. Проверьте, что поле `description` добавлено в список '
            'полей `fields` сериализатора модели `Group`.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_auth_get(self, user_client, group_1, group_2):
        response = user_client.get('/api/v1/groups/')
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/groups/` возвращает ответ со статусом 200.'
        )

        test_data = response.json()
        assert isinstance(test_data, list), (
            'Проверьте, что для авторизованного пользователя '
            'GET-запрос к `/api/v1/groups/` возвращает информацию о группах '
            'в виде списка.'
        )
        assert len(test_data) == Group.objects.count(), (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/groups/` возвращает информацию обо всех существующих '
            'группах.'
        )

        test_group = test_data[0]
        self.check_group_info(test_group, '/api/v1/groups/')

    @pytest.mark.django_db(transaction=True)
    def test_group_create(self, user_client, group_1, group_2):
        data = {'title': 'Группа  номер 3'}
        response = user_client.post('/api/v1/groups/', data=data)
        assert response.status_code == HTTPStatus.METHOD_NOT_ALLOWED, (
            'Убедитесь, что группу можно создавать только через админку, '
            'а при попытке создать ее через API возвращается статус 405.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_get_post(self, user_client, post_2):
        response = user_client.get('/api/v1/posts/')
        assert response.status_code == HTTPStatus.OK, (
            'Страница `/api/v1/posts/` не найдена, проверьте этот адрес в '
            '*urls.py*.'
        )

        test_data = response.json()
        assert len(test_data) == Post.objects.count(), (
            'Проверьте, что при GET-запросе к `/api/v1/posts/` '
            'в возвращаются и посты, принадлежащие группам.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_page_not_found(self, client, group_1):
        response = client.get(f'/api/v1/groups/{group_1.id}/')
        assert response.status_code != 404, (
            'Страница `/api/v1/groups/{group_id}` не найдена, проверьте этот '
            'адрес в *urls.py*.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_page_not_auth(self, client, group_1):
        response = client.get(f'/api/v1/groups/{group_1.id}/')
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что при запросе от неавторизованного пользователя к '
            '`/api/v1/groups/{group_id}/` возвращается ответ со статусом '
            '401.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_group_page_auth_get(self, user_client, group_1):
        response = user_client.get(f'/api/v1/groups/{group_1.id}/')
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/groups/{group_id}/` возвращается ответ со статусом 200.'
        )

        test_data = response.json()
        assert isinstance(test_data, dict), (
            'Проверьте, что при GET-запросе авторизованного пользователя к '
            '`/api/v1/groups/{group_id}/` информация о группе возвращается в '
            'виде словаря.'
        )
        self.check_group_info(test_data, '/api/v1/groups/{group_id}/')

```

## tests/test_post.py

```python
from http import HTTPStatus

import pytest

from posts.models import Post


class TestPostAPI:
    VALID_DATA = {'text': 'Поменяли текст статьи'}

    def test_post_not_found(self, client, post):
        response = client.get('/api/v1/posts/')

        assert response.status_code != HTTPStatus.NOT_FOUND, (
            'Страница `/api/v1/posts/` не найдена, проверьте этот адрес в '
            '*urls.py*.'
        )

    def test_post_not_auth(self, client, post):
        response = client.get('/api/v1/posts/')

        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что при GET-запросе неавторизованного пользователя к '
            '`/api/v1/posts/` возвращается ответ со статусом 401.'
        )

    def check_post_data(self,
                        response_data,
                        request_method_and_url,
                        db_post=None):
        expected_fields = ('id', 'text', 'author', 'pub_date')
        for field in expected_fields:
            assert field in response_data, (
                'Проверьте, что для авторизованного пользователя ответ на '
                f'{request_method_and_url} содержит поле `{field}` постов.'
            )
        if db_post:
            assert response_data['author'] == db_post.author.username, (
                'Проверьте, что при запросе авторизованного пользователя к '
                f'{request_method_and_url} ответ содержит поле `author` с '
                'именем автора каждого из постов.'
            )
            assert response_data['id'] == db_post.id, (
                'Проверьте, что при запросе авторизованного пользователя к '
                f'{request_method_and_url} в ответе содержится корректный '
                '`id` поста.'
            )

    @pytest.mark.django_db(transaction=True)
    def test_posts_auth_get(self, user_client, post, another_post):
        response = user_client.get('/api/v1/posts/')
        assert response.status_code == HTTPStatus.OK, (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/` возвращает статус 200.'
        )

        test_data = response.json()
        assert isinstance(test_data, list), (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/` возвращает список.'
        )

        assert len(test_data) == Post.objects.count(), (
            'Проверьте, что для авторизованного пользователя GET-запрос к '
            '`/api/v1/posts/` возвращает список всех постов.'
        )

        db_post = Post.objects.first()
        test_post = test_data[0]
        self.check_post_data(
            test_post,
            'GET-запрос к `/api/v1/posts/`',
            db_post
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_create_auth_with_invalid_data(self, user_client):
        posts_count = Post.objects.count()
        response = user_client.post('/api/v1/posts/', data={})
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            'Проверьте, что для авторизованного пользователя POST-запрос с '
            'некорректными данными к `/api/v1/posts/` возвращает ответ со '
            'статусом 400.'
        )
        assert posts_count == Post.objects.count(), (
            'Проверьте, что POST-запрос к `/api/v1/posts/` с некорректными '
            'данными не создает новый пост.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_create_auth_with_valid_data(self, user_client, user):
        post_count = Post.objects.count()

        data = {'text': 'Статья номер 3'}
        response = user_client.post('/api/v1/posts/', data=data)
        assert response.status_code == HTTPStatus.CREATED, (
            'Проверьте, что для авторизованного пользователя  POST-запрос с '
            'корректными данными к `/api/v1/posts/` возвращает ответ со '
            'статусом 201.'
        )

        test_data = response.json()
        assert isinstance(test_data, dict), (
            'Проверьте, что для авторизованного пользователя POST-запрос к '
            '`/api/v1/posts/` возвращает ответ, содержащий данные нового '
            'поста в виде словаря.'
        )
        self.check_post_data(test_data, 'POST-запрос к `/api/v1/posts/`')
        assert test_data.get('text') == data['text'], (
            'Проверьте, что для авторизованного пользователя POST-запрос к '
            '`/api/v1/posts/` возвращает ответ, содержащий текст нового '
            'поста в неизменном виде.'
        )
        assert test_data.get('author') == user.username, (
            'Проверьте, что для авторизованного пользователя при создании '
            'поста через POST-запрос к `/api/v1/posts/` ответ содержит поле '
            '`author` с именем пользователя, отправившего запрос.'
        )
        assert post_count + 1 == Post.objects.count(), (
            'Проверьте, что POST-запрос с корректными данными от '
            'авторизованного пользователя к `/api/v1/posts/` создает новый '
            'пост.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_unauth_create(self, client, user, another_user):
        posts_conut = Post.objects.count()

        data = {'author': another_user.id, 'text': 'Статья номер 3'}
        response = client.post('/api/v1/posts/', data=data)
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что POST-запрос неавторизованного пользователя к '
            '`/api/v1/posts/` возвращает ответ со статусом 401.'
        )

        assert posts_conut == Post.objects.count(), (
            'Проверьте, что POST-запрос неавторизованного пользователя к '
            '`/api/v1/posts/` не создает новый пост.'
        )

    def test_post_get_current(self, user_client, post):
        response = user_client.get(f'/api/v1/posts/{post.id}/')

        assert response.status_code == HTTPStatus.OK, (
            'Страница `/api/v1/posts/{id}/` не найдена, проверьте этот адрес '
            'в *urls.py*.'
        )

        test_data = response.json()
        self.check_post_data(
            test_data,
            'GET-запрос к `/api/v1/posts/{id}/`',
            post
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_post_change_auth_with_valid_data(self, user_client, post,
                                              http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(f'/api/v1/posts/{post.id}/',
                                data=self.VALID_DATA)
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.OK, (
            f'Проверьте, что для авторизованного пользователя {http_method}'
            '-запрос к `/api/v1/posts/{id}/` вернётся ответ со статусом '
            '200.'
        )

        test_post = Post.objects.filter(id=post.id).first()
        assert test_post, (
            f'Проверьте, что {http_method}-запрос авторизованного '
            'пользователя к `/api/v1/posts/{id}/` не удаляет редактируемый '
            'пост.'
        )
        assert test_post.text == self.VALID_DATA['text'], (
            f'Проверьте, что {http_method}-запрос авторизованного '
            'пользователя к `/api/v1/posts/{id}/` вносит изменения в пост.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_post_change_not_auth_with_valid_data(self, client, post,
                                                  http_method):
        request_func = getattr(client, http_method)
        response = request_func(f'/api/v1/posts/{post.id}/',
                                data=self.VALID_DATA)
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            f'Проверьте, что {http_method}-запрос неавторизованного '
            'пользователя к `/api/v1/posts/{id}/` возвращает ответ со '
            'статусом 401.'
        )
        db_post = Post.objects.filter(id=post.id).first()
        assert db_post.text != self.VALID_DATA['text'], (
            f'Проверьте, что {http_method}'
            '-запрос неавторизованного пользователя к `/api/v1/posts/{id}/` '
            'не вносит изменения в пост.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_post_change_not_author_with_valid_data(self, user_client,
                                                    another_post, http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(f'/api/v1/posts/{another_post.id}/',
                                data=self.VALID_DATA)
        http_method = http_method.upper()
        assert response.status_code == HTTPStatus.FORBIDDEN, (
            f'Проверьте, что {http_method}'
            '-запрос авторизованного пользователя к `/api/v1/posts/{id}/` '
            'для чужого поста возвращает ответ со статусом 403.'
        )

        db_post = Post.objects.filter(id=another_post.id).first()
        assert db_post.text != self.VALID_DATA['text'], (
            f'Проверьте, что {http_method}'
            '-запрос авторизованного пользователя к `/api/v1/posts/{id}/` '
            'для чужого поста не вносит изменения в пост.'
        )

    @pytest.mark.django_db(transaction=True)
    @pytest.mark.parametrize('http_method', ('put', 'patch'))
    def test_post_patch_auth_with_invalid_data(self, user_client, post,
                                               http_method):
        request_func = getattr(user_client, http_method)
        response = request_func(f'/api/v1/posts/{post.id}/',
                                data={'text': {}},
                                format='json')
        assert response.status_code == HTTPStatus.BAD_REQUEST, (
            f'Проверьте, что {http_method}'
            '-запрос с некорректными данными от авторизованного пользователя '
            'к `/api/v1/posts/{id}/` возвращает ответ с кодом 400.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_delete_by_author(self, user_client, post):
        response = user_client.delete(f'/api/v1/posts/{post.id}/')
        assert response.status_code == HTTPStatus.NO_CONTENT, (
            'Проверьте, что для автора поста DELETE-запрос к '
            ' `/api/v1/posts/{id}/` возвращает ответ со статусом 204.'
        )

        test_post = Post.objects.filter(id=post.id).first()
        assert not test_post, (
            'Проверьте, что DELETE-запрос автора поста к '
            ' `/api/v1/posts/{id}/` удаляет этот пост.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_delete_not_author(self, user_client, another_post):
        response = user_client.delete(f'/api/v1/posts/{another_post.id}/')
        assert response.status_code == HTTPStatus.FORBIDDEN, (
            'Проверьте, что DELETE-запрос авторизованного пользователя к '
            '`/api/v1/posts/{id}/` чужого поста '
            'вернёт ответ со статусом 403.'
        )

        test_post = Post.objects.filter(id=another_post.id).first()
        assert test_post, (
            'Проверьте, что авторизованный пользователь не может удалить '
            'чужой пост.'
        )

    @pytest.mark.django_db(transaction=True)
    def test_post_unauth_delete_current(self, client, post):
        response = client.delete(f'/api/v1/posts/{post.id}/')
        assert response.status_code == HTTPStatus.UNAUTHORIZED, (
            'Проверьте, что DELETE-запрос неавторизованного пользователя '
            'к `/api/v1/posts/{id}/` вернёт ответ со статусом 401.'
        )
        test_post = Post.objects.filter(id=post.id).first()
        assert test_post, (
            'Проверьте, что DELETE-запрос неавторизованного пользователя '
            'к `/api/v1/posts/{id}/` не удаляет запрошенный пост.'
        )

```

## tests/test_settings.py

```python
import pytest
from django.conf import settings


class TestSettings:

    @pytest.mark.parametrize('app', ('rest_framework',
                                     'rest_framework.authtoken'))
    def test_drf_in_installed_apps(self, app):
        assert hasattr(settings, 'INSTALLED_APPS'), (
            'Убедитель, что настройки проекта содержат переменную '
            '`INSTALLED_APPS`.'
        )
        assert app in settings.INSTALLED_APPS, (
            f'`{app}` отсутствует в `INSTALLED_APPS` в настройках '
            'приложения. Убедитесь, что необходимые модули Django '
            'REST Framework добавлены в `INSTALLED_APPS` в настройках проекта.'
        )

    def test_api_in_installed_apps(self):
        assert hasattr(settings, 'INSTALLED_APPS'), (
            'Убедитель, что настройки проекта содержат переменную '
            '`INSTALLED_APPS`.'
        )
        assert {'api', 'api.apps.ApiConfig'}.intersection(
            set(settings.INSTALLED_APPS)
        ), (
            'Убедитесь, что приложение `api` добавлено в `INSTALLED_APPS` в '
            'настройках проекта.'
        )

    def test_auth_settings(self):
        assert hasattr(settings, 'REST_FRAMEWORK'), (
            'Проверьте, что настройка `REST_FRAMEWORK` добавлена в файл '
            '`settings.py`'
        )

        assert 'DEFAULT_AUTHENTICATION_CLASSES' in settings.REST_FRAMEWORK, (
            'Проверьте, что добавили ключ `DEFAULT_AUTHENTICATION_CLASSES` в '
            '`REST_FRAMEWORK` файла `settings.py`'
        )
        assert (
            'rest_framework.authentication.TokenAuthentication' in
            settings.REST_FRAMEWORK['DEFAULT_AUTHENTICATION_CLASSES']
        ), (
            'Проверьте, что в списке `DEFAULT_AUTHENTICATION_CLASSES` в '
            '`REST_FRAMEWORK` содержится '
            '`rest_framework.authentication.TokenAuthentication`.'
        )

```

## pytest.ini

```ini
[pytest]
python_paths = yatube_api/
DJANGO_SETTINGS_MODULE = yatube_api.settings
norecursedirs = env/*
addopts = -vv -p no:cacheprovider
testpaths = tests/
python_files = test_*.py
```

````

## api_yatube/yatube_api/api/__init__.py

```python

````

## api_yatube/yatube_api/api/**pycache**/**init**.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/api/**pycache**/admin.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/api/**pycache**/apps.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/api/**pycache**/models.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/api/**pycache**/permissions.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/api/**pycache**/serializers.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/api/**pycache**/urls.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/api/**pycache**/views.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/api/admin.py

```python

```

## api_yatube/yatube_api/api/apps.py

```python
from django.apps import AppConfig


class ApiConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'api'

```

## api_yatube/yatube_api/api/migrations/**init**.py

```python

```

## api_yatube/yatube_api/api/migrations/**pycache**/**init**.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/api/models.py

```python

```

## api_yatube/yatube_api/api/permissions.py

```python
# yatube_api/api/permissions.py
from rest_framework import permissions


class IsAuthorOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in permissions.SAFE_METHODS:
            return True
        return obj.author == request.user

```

## api_yatube/yatube_api/api/serializers.py

```python
from rest_framework import serializers
from posts.models import Group, Post, Comment


class GroupSerializer(serializers.ModelSerializer):
    class Meta:
        model = Group
        fields = ('id', 'title', 'slug', 'description')


class PostSerializer(serializers.ModelSerializer):
    author = serializers.SlugRelatedField(
        slug_field='username', read_only=True)

    class Meta:
        model = Post
        fields = ('id', 'text', 'author', 'image', 'group', 'pub_date')


class CommentSerializer(serializers.ModelSerializer):
    author = serializers.SlugRelatedField(
        slug_field='username', read_only=True)

    class Meta:
        model = Comment
        fields = ('id', 'author', 'post', 'text', 'created')
        read_only_fields = ('post',)

```

## api_yatube/yatube_api/api/tests.py

```python

```

## api_yatube/yatube_api/api/urls.py

```python
from django.urls import include, path
from rest_framework import routers
from rest_framework.authtoken import views
from .views import GroupViewSet, PostViewSet, CommentViewSet

router = routers.DefaultRouter()
router.register('posts', PostViewSet)
router.register('groups', GroupViewSet)


urlpatterns = [
    path('api-token-auth/', views.obtain_auth_token),
    path('', include(router.urls)),

    path('posts/<int:post_id>/comments/', CommentViewSet.as_view({
        'get': 'list',
        'post': 'create'
    }), name='post-comments'),

    path('posts/<int:post_id>/comments/<int:pk>/', CommentViewSet.as_view({
        'get': 'retrieve',
        'put': 'update',
        'patch': 'partial_update',
        'delete': 'destroy'
    }), name='post-comment-detail'),
]

```

## api_yatube/yatube_api/api/views.py

```python
from rest_framework import viewsets, permissions
from posts.models import Group, Post, Comment
from .serializers import GroupSerializer, PostSerializer, CommentSerializer
from .permissions import IsAuthorOrReadOnly


class GroupViewSet(viewsets.ReadOnlyModelViewSet):
    queryset = Group.objects.all()
    serializer_class = GroupSerializer


class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    permission_classes = [permissions.IsAuthenticated, IsAuthorOrReadOnly]

    def perform_create(self, serializer):
        serializer.save(author=self.request.user)


class CommentViewSet(viewsets.ModelViewSet):
    serializer_class = CommentSerializer
    permission_classes = [permissions.IsAuthenticated, IsAuthorOrReadOnly]

    def get_queryset(self):
        post_id = self.kwargs.get('post_id')
        return Comment.objects.filter(post_id=post_id)

    def perform_create(self, serializer):
        post_id = self.kwargs.get('post_id')
        serializer.save(author=self.request.user, post_id=post_id)

```

## api_yatube/yatube_api/db.sqlite3

```sqlite3
[Binary file content not included]
```

## api_yatube/yatube_api/manage.py

```python
#!/usr/bin/env python
"""Django's command-line utility for administrative tasks."""
import os
import sys


def main():
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'yatube_api.settings')
    try:
        from django.core.management import execute_from_command_line
    except ImportError as exc:
        raise ImportError(
            "Couldn't import Django. Are you sure it's installed and "
            "available on your PYTHONPATH environment variable? Did you "
            "forget to activate a virtual environment?"
        ) from exc
    execute_from_command_line(sys.argv)


if __name__ == '__main__':
    main()

```

## api_yatube/yatube_api/posts/**init**.py

```python

```

## api_yatube/yatube_api/posts/**pycache**/**init**.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/posts/**pycache**/admin.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/posts/**pycache**/apps.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/posts/**pycache**/models.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/posts/admin.py

```python
from django.contrib import admin

from .models import Comment, Group, Post


class PostAdmin(admin.ModelAdmin):
    list_display = ('pk', 'text', 'pub_date', 'author')
    search_fields = ('text',)
    list_filter = ('pub_date',)
    empty_value_display = '-пусто-'


admin.site.register(Post, PostAdmin)
admin.site.register(Group)
admin.site.register(Comment)

```

## api_yatube/yatube_api/posts/apps.py

```python
from django.apps import AppConfig


class PostsConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'posts'

```

## api_yatube/yatube_api/posts/migrations/0001_initial.py

```python
# Generated by Django 3.2 on 2022-12-02 20:28

from django.conf import settings
from django.db import migrations, models
import django.db.models.deletion


class Migration(migrations.Migration):

    initial = True

    dependencies = [
        migrations.swappable_dependency(settings.AUTH_USER_MODEL),
    ]

    operations = [
        migrations.CreateModel(
            name='Group',
            fields=[
                ('id', models.BigAutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                ('title', models.CharField(max_length=200)),
                ('slug', models.SlugField(unique=True)),
                ('description', models.TextField()),
            ],
        ),
        migrations.CreateModel(
            name='Post',
            fields=[
                ('id', models.BigAutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                ('text', models.TextField()),
                ('pub_date', models.DateTimeField(auto_now_add=True, verbose_name='Дата публикации')),
                ('image', models.ImageField(blank=True, null=True, upload_to='posts/')),
                ('author', models.ForeignKey(on_delete=django.db.models.deletion.CASCADE, related_name='posts', to=settings.AUTH_USER_MODEL)),
                ('group', models.ForeignKey(blank=True, null=True, on_delete=django.db.models.deletion.SET_NULL, related_name='posts', to='posts.group')),
            ],
        ),
        migrations.CreateModel(
            name='Comment',
            fields=[
                ('id', models.BigAutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                ('text', models.TextField()),
                ('created', models.DateTimeField(auto_now_add=True, db_index=True, verbose_name='Дата добавления')),
                ('author', models.ForeignKey(on_delete=django.db.models.deletion.CASCADE, related_name='comments', to=settings.AUTH_USER_MODEL)),
                ('post', models.ForeignKey(on_delete=django.db.models.deletion.CASCADE, related_name='comments', to='posts.post')),
            ],
        ),
    ]

```

## api_yatube/yatube_api/posts/migrations/**init**.py

```python

```

## api_yatube/yatube_api/posts/migrations/**pycache**/0001_initial.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/posts/migrations/**pycache**/**init**.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/posts/models.py

```python
from django.contrib.auth import get_user_model
from django.db import models

User = get_user_model()


class Group(models.Model):
    title = models.CharField(max_length=200)
    slug = models.SlugField(unique=True)
    description = models.TextField()

    def __str__(self):
        return self.title


class Post(models.Model):
    text = models.TextField()
    pub_date = models.DateTimeField(
        'Дата публикации', auto_now_add=True
    )
    author = models.ForeignKey(
        User, on_delete=models.CASCADE, related_name='posts'
    )
    image = models.ImageField(
        upload_to='posts/', null=True, blank=True
    )  # поле для картинки
    group = models.ForeignKey(
        Group, on_delete=models.SET_NULL,
        related_name='posts', blank=True, null=True
    )

    def __str__(self):
        return self.text


class Comment(models.Model):
    author = models.ForeignKey(
        User, on_delete=models.CASCADE, related_name='comments'
    )
    post = models.ForeignKey(
        Post, on_delete=models.CASCADE, related_name='comments'
    )
    text = models.TextField()
    created = models.DateTimeField(
        'Дата добавления', auto_now_add=True, db_index=True
    )

```

## api_yatube/yatube_api/posts/urls.py

```python

```

## api_yatube/yatube_api/posts/views.py

```python

```

## api_yatube/yatube_api/yatube_api/**init**.py

```python

```

## api_yatube/yatube_api/yatube_api/**pycache**/**init**.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/yatube_api/**pycache**/settings.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/yatube_api/**pycache**/urls.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/yatube_api/**pycache**/wsgi.cpython-310.pyc

```pyc
[Binary file content not included]
```

## api_yatube/yatube_api/yatube_api/settings.py

```python
"""Django settings for yatube project."""

from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent


# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'm%(5u7nv9j2%@3xb%#c3p-$9&0$kq$j6l@9+@ogairu48a+dy+'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = ['*']


# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'posts.apps.PostsConfig',
    'api.apps.ApiConfig',
    'rest_framework',
    'rest_framework.authtoken',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'yatube_api.urls'
TEMPLATES_DIR = BASE_DIR / 'templates'
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [TEMPLATES_DIR],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'yatube_api.wsgi.application'


# Database

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}


# Password validation

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]

# Internationalization

LANGUAGE_CODE = 'ru-RU'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_L10N = True

USE_TZ = True


# Static files (CSS, JavaScript, Images)

STATIC_URL = '/static/'

MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'


# Custom
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],

    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ]
}

```

## api_yatube/yatube_api/yatube_api/urls.py

```python
from django.conf import settings
from django.conf.urls.static import static
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('api.urls')),
]


if settings.DEBUG:
    urlpatterns += static(
        settings.MEDIA_URL, document_root=settings.MEDIA_ROOT
    )
    urlpatterns += static(
        settings.STATIC_URL, document_root=settings.STATIC_ROOT
    )

```

## api_yatube/yatube_api/yatube_api/wsgi.py

```python
"""
WSGI config for yatube_api project.

It exposes the WSGI callable as a module-level variable named ``application``.

For more information on this file, see
https://docs.djangoproject.com/en/3.2/howto/deployment/wsgi/
"""

import os

from django.core.wsgi import get_wsgi_application

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'yatube_api.settings')

application = get_wsgi_application()

```
