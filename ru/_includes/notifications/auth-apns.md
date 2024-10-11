Вы можете аутентифицироваться на мобильной платформе APNs с помощью _токена_ или _сертификата_:
* Для аутентификации с помощью токена вам понадобятся следующие данные:
  * **Токен** — чтобы получить файл токена в формате `.p8`, создайте ключ аутентификации в своей учетной записи разработчика Apple: **Certificates, Identifiers & Profiles** → **Keys** → ![image](../../_assets/console-icons/circle-plus-fill.svg). Скачать файл токена можно только один раз.
  * **Идентификатор токена** — узнайте идентификатор в учетной записи разработчика Apple: **Certificates, Identifiers & Profiles** → **Keys**. Убедитесь, что идентификатор соответствует токену, который вы хотите применить. Должен содержать 10 символов.
  * **Идентификатор разработчика (Team ID)** — указан в правом верхнем углу вашей учетной записи разработчика Apple. Должен содержать 10 символов: только цифры и буквы латинского алфавита.
  * **Идентификатор приложения (Bundle ID)** — узнайте [Bundle ID](https://developer.apple.com/documentation/appstoreconnectapi/list_bundle_ids) в учетной записи разработчика Apple: **Certificates, Identifiers & Profiles** → **Identifiers** или в приложении Xcode: **Target** → **General** → **Identity**. Может содержать только цифры, буквы латинского алфавита, дефисы и точки.
* Для аутентификации с помощью сертификата понадобятся следующие данные:
  * **Сертификат** — файл сертификата SSL в формате `.p12`.
  * **Закрытый ключ сертификата**. 

  Подробнее о сертификате см. в [документации Apple](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/establishing_a_certificate-based_connection_to_apns#2947597).

Аутентификация с токеном является предпочтительной, как более современная.