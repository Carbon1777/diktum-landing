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

Готов. В файл подставлены **два** SHA256 fingerprint'а в массиве —
assetlinks протокол поддерживает несколько ключей для одного
package_name, что покрывает все каналы распространения Diktum:

1. **Google Play App Signing key** —
   `58:62:89:74:46:34:97:BE:59:3E:02:77:A5:1E:3D:65:AD:68:E8:ED:57:32:0E:9A:FF:53:CE:5B:AD:30:2B:E0`
   Взят из Play Console → App integrity → Сертификат для ключа
   подписи приложения → SHA-256. Именно этим ключом Google
   переподписывает финальный APK перед раздачей пользователям через
   Google Play.

2. **Локальный release keystore** —
   `70:10:2A:F7:18:03:6D:20:C3:19:0F:85:44:90:04:93:3D:E0:45:23:C8:60:63:2D:7C:0E:38:14:D3:A5:09:38`
   Извлечён из `Diktum/android/diktum-release.jks` (алиас `diktum`)
   через `keytool`. Также соответствует Upload key SHA-256 в Play
   Console. Покрывает:
   - распространение APK через RuStore (RuStore не переподписывает);
   - прямой sideload (ADB, веб-загрузка APK);
   - случаи когда App Signing будет отключён.

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
