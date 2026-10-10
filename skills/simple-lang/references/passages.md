# Full passages: before and after

Use this file with `SKILL.md` for a full rewrite of a long text. It holds complete before/after passages in English and Russian. It never adds or changes a rule.

## Contents

1. English
2. Русский

---

## 1. English

### Procedure

BAD:
> Before getting started with the deployment process, it's generally a good idea to make sure that all of the relevant tests have been executed and are passing, after which the deployment can be initiated by the operator via the execution of the deploy script, keeping in mind that the staging environment configuration verification step shouldn't be skipped.

GOOD:
> 1. Run all tests. Make sure that they pass.
> 2. Check the staging configuration. Do not skip this step.
> 3. Run the deploy script.

### Description

BAD:
> The system is designed in such a way that incoming requests are subjected to validation by the middleware layer, and in the event that validation is not successful, the rejection of the request occurs with the generation of an appropriate error response.

GOOD:
> The middleware validates each incoming request. If validation fails, the middleware rejects the request and returns an error response.

### Error message

BAD:
> Oops! Something went wrong while we were trying to process your request. Please try again later or feel free to contact support if the problem persists!

GOOD:
> Request failed: the payment service did not respond (timeout after 30 s). Try again. If the error repeats, contact support with error ID {id}.

---

## 2. Русский

### IT-процедура

До:

> В целях обеспечения корректного функционирования системы необходимо
> осуществить проверку статуса сервиса аутентификации пользователей портала
> на предмет наличия ошибок. В случае обнаружения ошибок рекомендуется
> произвести перезапуск сервиса, при этом следует отметить, что перед
> перезапуском должно быть выполнено сохранение текущих логов, так как
> в противном случае данные логов будут утеряны, что может затруднить
> последующий анализ причин возникновения проблемы.

После:

> 1. Проверьте статус сервиса аутентификации: `systemctl status auth-service`.
> 2. Если ошибок нет, пропустите шаги 3 и 4.
> 3. Сохраните логи: `journalctl -u auth-service > auth.log`.
> 4. **ОСТОРОЖНО:** перезапуск стирает текущие логи. Перезапустите сервис: `systemctl restart auth-service`.

### Техническое описание

До:

> Данный модуль является одним из ключевых компонентов подсистемы обработки
> очереди входящих сообщений пользователей и осуществляет выполнение
> валидации получаемых данных, после чего производится их сохранение
> в базу данных, при этом в настоящее время обеспечивается поддержка
> двух форматов сообщений.

После:

> Модуль входит в подсистему, которая обрабатывает очередь входящих сообщений
> пользователей. Модуль проверяет полученные данные и сохраняет их в базу данных.
>
> Сейчас модуль поддерживает два формата сообщений.

### Сообщение об ошибке

До:

> Произошла ошибка при осуществлении попытки сохранения внесённых вами
> изменений в связи с отсутствием соединения с сервером. Рекомендуется
> произвести проверку подключения к сети интернет и повторить попытку позже.

После:

> Не удалось сохранить изменения: нет соединения с сервером.
>
> Проверьте подключение к интернету. Затем нажмите «Повторить».
>
> [Повторить] [Отмена]
