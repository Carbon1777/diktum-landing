# .well-known/

Файлы для верификации Universal Links (iOS) и App Links (Android).
Обслуживаются GitHub Pages по адресам:

- `https://diktumweb.ru/.well-known/apple-app-site-association`
- `https://diktumweb.ru/.well-known/assetlinks.json`

## Перед публикацией — обязательно подставить реальные значения

### `apple-app-site-association`

Готов. `appID` = `WVC52Q5669.com.carbon1777.diktum` — взято из
`Diktum/ios/Runner.xcodeproj/project.pbxproj`:
- `DEVELOPMENT_TEAM[sdk=iphoneos*] = WVC52Q5669` — production Team ID.
- `PRODUCT_BUNDLE_IDENTIFIER = com.carbon1777.diktum` — production bundle.

Если в Apple Developer аккаунте Team ID изменится (например при
смене юр.лица), нужно будет обновить файл.

### `assetlinks.json`

В файл уже подставлены **два** SHA256 fingerprint'а в массиве —
assetlinks протокол поддерживает несколько ключей для одного
package_name:

1. **Локальный release keystore** —
   `70:10:2A:F7:18:03:6D:20:C3:19:0F:85:44:90:04:93:3D:E0:45:23:C8:60:63:2D:7C:0E:38:14:D3:A5:09:38`
   Извлечён из `Diktum/android/diktum-release.jks` (алиас `diktum`).
   Подходит для:
   - APK, распространяемых напрямую (RuStore, прямой sideload, ADB);
   - Google Play если App Signing by Google Play **отключён** в Console.

2. **Play App Signing key** — пока placeholder
   `REPLACE_WITH_PLAY_APP_SIGNING_SHA256_IF_PLAY_APP_SIGNING_ENABLED`.
   Нужен ТОЛЬКО если в Google Play включён App Signing by Google Play
   (по умолчанию для новых приложений). Если включён:
   1. Play Console → выбрать Diktum → Setup → App integrity → App
      Signing Key → SHA-256 certificate fingerprint.
   2. Скопировать строку и подставить в `assetlinks.json`.
   3. Если App Signing **отключён** — удалить эту строку из массива
      (оставить только локальный fingerprint), JSON станет
      `"sha256_cert_fingerprints": ["70:10:..."]`.

Проверить какой режим включён: Play Console → App integrity → App
Signing Key — если там есть «Google managed key», значит App Signing
включён.

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
