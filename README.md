# CounterRooms

Сайт-список комнат для Counter-Fly. Один файл `index.html`, работает на GitHub Pages.

## Выложить на GitHub
1. Создай репозиторий, загрузи `index.html`.
2. Settings → Pages → Branch: `main`, папка `/ (root)` → Save.
3. Через минуту сайт откроется по ссылке `https://ТВОЙ_НИК.github.io/ИМЯ_РЕПО/`.

Без шага Firebase сайт работает в демо-режиме: данные видны только в твоём браузере.

## Подключить Firebase (бесплатно), чтобы комнаты видели все
1. https://console.firebase.google.com → Add project.
2. Build → Authentication → Sign-in method → включить Email/Password.
3. Build → Firestore Database → Create database.
4. Firestore → Rules → вставить и опубликовать:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    match /users/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
    match /rooms/{id} {
      allow read: if true;
      allow create: if request.auth != null
        && request.resource.data.uid == request.auth.uid
        && request.resource.data.code is string
        && request.resource.data.code.size() <= 20;
      allow delete: if request.auth != null && resource.data.uid == request.auth.uid;
    }
  }
}
```
5. Project settings → Your apps → Web (`</>`) → скопировать `apiKey`, `authDomain`, `projectId`, `appId` в блок `FB` в начале скрипта в `index.html`.
6. Authentication → Settings → Authorized domains → добавить `ТВОЙ_НИК.github.io`.

Что хранится: почта, ник, ID профиля, комнаты. Пароль хранит сам Firebase Authentication в зашифрованном виде.
