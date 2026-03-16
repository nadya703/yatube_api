# Project Structure

```
├── tests
│   ├── __pycache__
│   ├── fixtures
│   │   ├── __pycache__
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
└── pytest.ini
```

# File Contents

## tests/**init**.py

```python

```

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
