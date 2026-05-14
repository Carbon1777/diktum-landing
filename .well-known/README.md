# .well-known/

Файлы для верификации Universal Links (iOS) и App Links (Android).
Обслуживаются GitHub Pages по адресам:

- `https://diktumweb.ru/.well-known/apple-app-site-association`
- `https://diktumweb.ru/.well-known/assetlinks.json`

## Перед публикацией — обязательно подставить реальные значения

### `apple-app-site-association`

`REPLACE_WITH_TEAMID` → 10-символьный Apple Team ID.
Найти:
- Xcode → Runner target → Signing & Capabilities → Team dropdown.
- developer.apple.com → Membership → Team ID.

Результат: например `ABC1234XYZ.ru.diktumweb.diktum`.

### `assetlinks.json`

`REPLACE_WITH_PLAY_APP_SIGNING_SHA256` → SHA256 fingerprint
производственной подписи Android.

Если используется **Play App Signing** (по умолчанию для новых приложений
в Google Play):
1. Play Console → выбрать приложение → Setup → App integrity → App
   Signing Key → SHA-256 certificate fingerprint.
2. Скопировать строку формата `AB:CD:...` (64 hex символа парами).

Если без Play App Signing — fingerprint собственного keystore:
```bash
keytool -list -v -keystore <release.jks> -alias <alias> | grep SHA256
```

## Проверка после публикации

```bash
curl -I https://diktumweb.ru/.well-known/apple-app-site-association
# должно вернуть 200 OK, Content-Type не критичен для iOS 13+

curl -I https://diktumweb.ru/.well-known/assetlinks.json
# должно вернуть 200 OK, Content-Type: application/json

curl https://diktumweb.ru/.well-known/apple-app-site-association | jq .
# валидный JSON

curl https://diktumweb.ru/.well-known/assetlinks.json | jq .
# валидный JSON
```

## Откат

Удалить файлы — приложение продолжает работать через обычные ссылки
(пользователь видит баннер «Open in Safari» вместо deep-link в app).
