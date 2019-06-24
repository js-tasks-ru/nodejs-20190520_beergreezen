# Чат с использованием websockets

В этой задаче вам потребуется реализовать функциональность чата с использованием технологии 
websockets. Задача состоит из нескольких подзадач:

1. Реализовать получение истории сообщений по адресу `GET /messages`. Этот эндпойнт должен быть
доступен лишь аутентифицированным пользователям. Нет необходимости возвращать все сообщения сразу, 
достаточно будет лишь последних 20. Формат каждого объекта сообщения следующий:
```js
{
    "date": Date, // дата сообщения
    "text": String, // текст сообщения
    id: String, // идентификатор сообщения
    user: String, // Имя (displayName) пользователя, отправившего это сообщение
}
```
2. Реализовать аутентификацию по токену для подключений по Websocket:
       - пользователь передаёт токен в параметре `token` запроса
       - если параметра `token` нет - необходимо вернуть ошибку 
       `new Error("anonymous sessions are not allowed")`
       - если сессии по переданному токену нет - необходимо вернуть ошибку
       `new Error('wrong or expired session token')`
       - объект пользователя необходимо записать в свойство `socket.user` (это нужно для следующей
       подзадачи)  
3. На каждое сообщения пользователя, пришедшее по протоколу Websocket (событие `message`) 
   необходимо:
       - отправлять сообщение `user_message` *всем* пользователям (включая отправителя) следующего 
       формата:
       ```js
       'user_message' // название события
       {
        user: String, // имя (displayName) пользователя
        text: String, // текст сообщения 
        date: Date    // дата получения сообщения
       });
       ```
       - сохранять сообщение в базу, используя те же значения полей