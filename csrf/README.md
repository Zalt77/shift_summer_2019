# Уязвимость

## Описание
CSRF (Cross Site Request Forgery — «межсайтовая подделка запроса»). Эта атака позволяет производить различные действия на уязвимом сайте от имени аутентифицированного пользователя. Для её реализации злоумышленник "заманивает" жертву на сайт, содержащий скрытую форму уже заполненную злоумышленником. Эта форма должна отправляться на уязвимый сайт. Она содержит значения, которые может выполнить только авторизованный пользователь. Браузер жертвы, при загрузке сайта злоумышленника, обрабатывает эту форму, т.е. осуществляет отправку формы на уязвимый сайт со значениями, заранее написанными злоумышленником. По стандартам HTTP при обращении к сайту браузер должен отправить запрос с приложением файла куки, в котором содержится идентификатор авторизации, если она была. Тем самым уязвимый сайт не подозревает, что форма была отправлена злоумышленником, а не пользователем.

## Классификация
Web application vulnerability

## Условия
- ОС: любая
- язык: любой
- компоненты: cookie
- настройки: исполнение JS 

## Детектирование
- Код ревью
- Исследовать на небезопасные методы POST,PUT,DELETE,PATCH
- Проверка на наличие CSRF токена

## Эксплуатация
Эксплуатация заключается в привлечении пользователя на сайт с уязвимостью,ожидая что пользователь авторизован на сайте(существует сессионая кука) не защищенного от CSRF,для которого у нас есть скрипт выполнения какого-то действия

### Инструменты
- знание JS

## Ущерб
- Кража конфиденциальной информации
- Манипуляция онлайн опросами
- Установка вредоносного ПО

## Защита
Популярные варианты защиты от CSRF
1. CSRF token 
2. Double submit cookie
3. Content-Type based protection
4. Referer-based protection
5. Password confirmation / websudo
6. SameSite Cookies

Существует 3 метода использования токенов для защиты web-сервисов от CSRF атак:

Synchronizer Tokens (Statefull)
Double Submit Cookie (Stateless)
Encrypted Token (Stateless)
- Synchronizer Tokens
Простой подход, использующийся повсеместно. Требует хранения токена на стороне сервера.
- Double Submit Cookie
Этот подход не требует хранения данных на стороне сервера.Идея в том, чтобы отдать токен клиенту двумя методами: в куках и в одном из параметров ответа (header или внутри HTML).
- Encrypted Token
Основная идея — если вы зашифруете надежным алгоритмом какие-то данные и передадите их клиенту, то клиент не сможет их подделать, не зная ключа. Этот подход не требует использования cookie. Токен передаётся клиенту только в параметрах ответа.
В данном подходе токеном являются факты, зашифрованные ключом. Минимально необходимые факты — это идентификатор пользователя и timestamp времени генерации токена. Ключ не должен быть известен клиенту.

### Основные меры
- Генерировать новый токен на каждый запроc
- Ограничиваем время жизни cookie, которое содержит токен, разумным значением. 
- Делаем cookie недоступной из JS (ставим HTTPOnly=true)
- Размер токена не менее 32 байт.
- Генерируем токен криптографически стойким генератором псевдослучайных чисел.

## Обход защиты
1. XSS (cross-sitescripting)
2. Dangling markup
3. Уязвимый субдомен
4. Cookie injection
5. Spoof Referer