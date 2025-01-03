# Статический сайт в {{ objstorage-full-name }}

С помощью этой инструкции вы научитесь загружать [статические файлы](../../storage/concepts/hosting.md) вашего сайта в [{{ objstorage-name }}](../../storage/) и привязывать к [бакету](../../storage/concepts/bucket.md) доменное имя. Для управления доменом можно воспользоваться сервисом [{{ dns-full-name }}](../../dns/).

По умолчанию сайт доступен только по протоколу HTTP. Чтобы [поддержать](../../storage/operations/hosting/certificate.md) для сайта протокол HTTPS, можно загрузить [сертификат безопасности](../../certificate-manager/concepts/index.md) из [{{ certificate-manager-full-name }}](../../certificate-manager/).

Вы можете создать инфраструктуру для статического сайта в {{ objstorage-full-name }} с помощью одного из инструментов:

* [Консоль управления](../../tutorials/web/static/console.md) — используйте этот способ, чтобы пошагово создать инфраструктуру в консоли управления {{ yandex-cloud }}.
* [{{ TF }}](../../tutorials/web/static/terraform.md) — используйте этот способ, чтобы упростить создание ресурсов и управление ими, используя подход «инфраструктура как код» (IaC). Скачайте пример конфигурации {{ TF }} , а затем разверните инфраструктуру с помощью [{{ TF }}-провайдера {{ yandex-cloud }}]({{ tf-docs-link }}).