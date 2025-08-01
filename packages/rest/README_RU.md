# @auth-strategy-manager/rest

Стратегия REST API для auth-strategy-manager.

## 🌍 Документация на других языках

- [🇺🇸 English (Английский)](README.md)
- [🇷🇺 Русский (Текущий)](README_RU.md)

## Установка

```bash
npm install @auth-strategy-manager/rest @auth-strategy-manager/core axios
```

## Использование

```typescript
import { AuthStrategyManager } from '@auth-strategy-manager/core';
import { RestStrategy } from '@auth-strategy-manager/rest';
import axios from 'axios';

// Создание кастомного axios инстанса
const axiosInstance = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 5000
});

// Создание REST стратегии
const restStrategy = new RestStrategy({
  name: 'my-rest',
  tokenKey: 'access_token',
  signInUrl: 'https://myapp.com/sign-in',
  axiosInstance,
  
  // URL endpoints
  checkAuth: { url: '/auth/check-auth', method: 'GET' },
  signIn: { url: '/auth/sign-in', method: 'POST' },
  signUp: { url: '/auth/sign-up', method: 'POST' },
  signOut: { url: '/auth/sign-out', method: 'POST' },
  refresh: { url: '/auth/refresh', method: 'POST' },
  
  // Кастомная функция извлечения токена
  getToken: (response: unknown) => (response as any).data?.access_token || (response as any).access_token
});

// Использование с менеджером стратегий
const authManager = new AuthStrategyManager([restStrategy]);

// Вход с кастомными данными
const loginResult = await restStrategy.signIn<unknown, AxiosRequestConfig>({
  data: {
    username: 'user@example.com',
    password: 'password123'
  }
});

// Проверка аутентификации
const isAuthenticated = await restStrategy.checkAuth();

// Выход из системы
await restStrategy.signOut();

// Очистка состояния
restStrategy.clear();
```

## Конфигурация

### RestConfig

```typescript
type RestConfig = {
  check: UrlConfig;
  signIn: UrlConfig;
  signUp: UrlConfig;
  signOut: UrlConfig;
  refresh: UrlConfig;
  name?: string;
  tokenKey?: string;
  signInUrl?: string;
  axiosInstance?: AxiosInstance;
  getToken?: (response: unknown, url?: string) => string;
};

type UrlConfig = {
  url: string;
  method?: string;
};
```

### Параметры

- `checkAuth` - Endpoint для проверки аутентификации
- `signIn` - Endpoint для входа пользователя
- `signUp` - Endpoint для регистрации пользователя
- `signOut` - Endpoint для выхода пользователя
- `refresh` - Endpoint для обновления токена
- `name` - Имя стратегии (по умолчанию: 'rest')
- `tokenKey` - Ключ хранилища для токена (по умолчанию: 'access')
- `signInUrl` - URL для перенаправления после выхода
- `axiosInstance` - Кастомный axios инстанс
- `getToken` - Кастомная функция для извлечения токена из ответа

## API

### RestStrategy

#### Конструктор

```typescript
constructor(config: RestConfig)
```

#### Методы

- `checkAuth(): Promise<boolean>` - Проверка аутентификации
- `signIn<T = unknown, D = undefined>(config?: D): Promise<T>` - Вход пользователя
- `signUp<T = unknown, D = undefined>(config?: D): Promise<T>` - Регистрация пользователя
- `signOut(): Promise<void>` - Выход пользователя
- `refreshToken(): Promise<void>` - Обновление токена
- `clear(): void` - Очистка состояния аутентификации

#### Свойства

- `name: string` - Имя стратегии
- `axiosInstance: AxiosInstance` - Axios инстанс
- `token?: string` - Текущий токен
- `isAuthenticated: boolean` - Статус аутентификации

## Хранение токенов

Токены хранятся в `sessionStorage` с настроенным `tokenKey`.

## Лицензия

ISC 
