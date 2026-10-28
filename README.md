# CounterRooms

Сайт-список комнат для Counter-Fly. Один файл `index.html`, работает на GitHub Pages. Регистрации нет: игрок вводит ник и (по желанию) ставит аватарку. Профиль хранится в браузере.

## Выложить на GitHub
1. Создай публичный репозиторий, загрузи `index.html`.
2. Settings → Pages → Branch: `main`, папка `/ (root)` → Save.
3. Сайт откроется по ссылке `https://ТВОЙ_НИК.github.io/ИМЯ_РЕПО/`.

Без Firebase сайт работает в демо-режиме: комнаты видны только в твоём браузере.

## Подключить Firebase (бесплатно), чтобы комнаты видели все
1. https://console.firebase.google.com → Add project.
2. Build → Authentication → Sign-in method → включить **Anonymous** (это невидимый вход без почты, нужен только чтобы удалять свои комнаты).
3. Build → Firestore Database → Create database.
4. Firestore → Rules → вставить и опубликовать:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    match /rooms/{id} {
      allow read: if true;
      allow create: if request.auth != null
        && request.resource.data.uid == request.auth.uid
        && request.resource.data.code is string
        && request.resource.data.code.size() <= 20
        && request.resource.data.hostAvatar.size() < 20000;
      allow delete: if request.auth != null && resource.data.uid == request.auth.uid;
    }
  }
}
```
5. Project settings → Your apps → Web (`</>`) → скопировать `apiKey`, `authDomain`, `projectId`, `appId` в блок `FB` в начале скрипта в `index.html`.
6. Authentication → Settings → Authorized domains → добавить `ТВОЙ_НИК.github.io`.

На сервере хранятся только комнаты: код, ник, аватарка, время.
