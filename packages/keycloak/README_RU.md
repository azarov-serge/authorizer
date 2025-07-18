# @auth-strategy-manager/keycloak

Стратегия Keycloak для auth-strategy-manager.

## 🌍 Документация на других языках

- [🇺🇸 English (Английский)](README.md)
- [🇷🇺 Русский (Текущий)](README_RU.md)

## Установка

```bash
npm install @auth-strategy-manager/keycloak @auth-strategy-manager/core keycloak-js
```

## Использование

```typescript
import { AuthStrategyManager } from '@auth-strategy-manager/core';
import { KeycloakStrategy } from '@auth-strategy-manager/keycloak';

// Создание Keycloak стратегии
const keycloakStrategy = new KeycloakStrategy({
  keycloak: {
    realm: 'my-realm',
    url: 'https://keycloak.example.com',
    clientId: 'my-client'
  },
  loginUrl: 'https://myapp.com/login',
  name: 'my-keycloak',
  only: false
});

// Использование с менеджером стратегий
const authManager = new AuthStrategyManager([keycloakStrategy]);

// Проверка аутентификации
const isAuthenticated = await keycloakStrategy.checkAuth();

// Вход в систему
await keycloakStrategy.signIn();

// Выход из системы
await keycloakStrategy.signOut();

// Обновление токена
await keycloakStrategy.refreshToken(30); // 30 секунд минимальной валидности
```

## Конфигурация

### KeycloakConfig

```typescript
type KeycloakConfig = {
  keycloak: {
    realm: string;
    url: string;
    clientId: string;
  };
  loginUrl?: string;
  name?: string;
  only?: boolean;
};
```

### Параметры

- `keycloak.realm` - Имя realm в Keycloak
- `keycloak.url` - URL сервера Keycloak
- `keycloak.clientId` - Client ID в Keycloak
- `loginUrl` - URL для редиректа после выхода
- `name` - Имя стратегии (по умолчанию: 'keycloak')
- `only` - Если true, доступна только эта стратегия

## API

### KeycloakStrategy

#### Конструктор

```typescript
constructor(config: KeycloakConfig)
```

#### Методы

- `checkAuth(): Promise<boolean>` - Проверка аутентификации
- `signIn<T = unknown, D = undefined>(config?: D): Promise<T>` - Вход пользователя
- `signUp<T = unknown, D = undefined>(config?: D): Promise<T>` - Регистрация пользователя (не реализовано)
- `signOut(): Promise<void>` - Выход пользователя
- `refreshToken<T>(sec?: T): Promise<void>` - Обновление токена

#### Свойства

- `name: string` - Имя стратегии
- `keycloak: Keycloak` - Экземпляр Keycloak
- `token?: string` - Текущий токен
- `isAuthenticated: boolean` - Статус аутентификации

## Лицензия

ISC 
