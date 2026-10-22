# Push для Tich Connect

Управляемый форк Sygnal обслуживает приложение `app.tich.connect`.
Deploy OS project: `connect-ios`. Образ собирается из зафиксированного коммита
через `docker build -f docker/Dockerfile .`; production использует digest.

Шлюз получает события Synapse и отправляет APNs-уведомления. Текст E2EE
расшифровывает расширение NSE на устройстве через наш Matrix SDK и Яндекс CDN.
Сервер не получает ключи шифрования переписки.

Для TestFlight нужен pushkin `app.tich.connect.ios.prod`, type `apns`,
platform `production`, topic `app.tich.connect`, convert_device_token_to_hex
`true`, send_badge_counts `false`. Debug использует отдельный pushkin
`app.tich.connect.ios.dev` с platform `sandbox`.

APNs keyfile, key_id и team_id доставляются только из SOPS Deploy OS.
До регистрации ключа и smoke на устройстве push остаётся неподтверждённым.
Healthcheck использует Python из образа и не требует отсутствующего curl.

Проверка `Docker checks` собирает образ и сохраняет его как CI artifact.
Публикация в upstream DockerHub доступна только репозиторию Element.
