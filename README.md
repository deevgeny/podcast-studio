# Студия подкастов
Аудиоплеер для студии подкастов

## Описание проекта

## Запуск проекта

## Заметки
### Создание нового проекта
Создаем новую директорию, переходим в нее и инициализируем проект.
```bash
# Создаем новую директорию и переходим в нее
mkdir project
cd project

# Инициализируем новый проект и вводим название и остальные атрибуты в промпте
npm init

# Либо сразу инициализируем проект с названием и дефолтными значениями
npm init --yes --package-name="название-проекта"
```
### Установка и настройка webpack
Краткое руководство по установке и базовой настройке webpack для фронтенд проекта.
https://webpack.js.org/guides/getting-started/
#### Установка и базовая кофигурация сборки
1. Устанавливаем webpack
```bash
# Устанавливаем webpack
npm install --save-dev webpack webpack-cli
# npm i -D webpack webpack-cli
```
2. Создаем конфигурационный файл `webpack.config.js` с конфигурацией сборки.
```js
const path = require('path');

module.exports = {
  entry: './src/index.ts',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
```
3. Добавляем команду сборки в файл `package.json`.
```json
{
  "scripts": {
    "build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
}
```
#### Поддержка TypeScript
1. Устанавливаем компилятор и загрузчик TypeScript.
```bash
npm install --save-dev typescript ts-loader
```
2. Создаем файл `tsconfig.json` с содержимым.
```json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx": "react",
    "allowJs": true,
    "moduleResolution": "node"
  }
}
```
3. Добавляем в файл `webpack.config.js` конфигурацию для TypeScript.
```js
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
```
4. Чтобы исправить проблему импортов, добавляем в файл `tsconfig.json` конфигурацию.
```json
{
    "allowSyntheticDefaultImports" : true,
    "esModuleInterop" : true
}
```
Пример решения проблемы импорта в самих модулях:
```ts
import _ from 'lodash'; // Не работает в TypeScript
import * as _ from 'lodash'; // Исправление импорта
```
#### Настройка dev сервера
Просмотр листинга ошибок Source Map
1. Добавляем конфиг в файл `tsconfig.json`.
```json
{
    "compilerOptions": {
        "sourceMap": true,
    }
}
```
2. Включаем source map в бандл `webpack.config.js`.
```js
module.exports = {
  entry: './src/index.ts',
  devtool: 'inline-source-map',
  ...
}
```
3. Устанавливаем dev-server.
```bash
npm install --save-dev webpack-dev-server
```
4. Добавляем в `webpack.config.js` конфигурацию.
```js
module.exports = {
  devServer: {
    static: './dist',
  },
  optimization: {
    runtimeChunk: 'single',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  },
}
```
5. Добавляем скрипт запуска сервера в `package.json`.
```json
{
    "scripts": {
        "start": "webpack serve --open",
    }
}
```