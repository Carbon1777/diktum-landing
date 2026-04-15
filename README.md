# diktum-landing

Статические страницы для Diktum, обслуживаемые через GitHub Pages.
Содержимое публично и не содержит секретов. Основной репо приватный.

- `auth-confirmed.html` — страница, на которую Supabase Auth редиректит
  пользователя после подтверждения email. Прописана в Supabase
  Dashboard → Auth → URL configuration → Site URL.
- `index.html` — заглушка-редирект на `diktumweb.ru`.

После деплоя основного сайта на `diktumweb.ru` (сессия 22) этот репо
можно оставить как зеркало или удалить.
