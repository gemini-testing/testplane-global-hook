# hermione-global-hook

## Обзор

Используйте плагин [hermione-global-hook][hermione-global-hook], чтобы вынести общую логику из своих тестов в специальные обработчки для `beforeEach` и `afterEach` хуков.

Часто, перед тем как запустить очередной hermione-тест, нужно выполнить определенную подготовительную работу, например:
* очистить все cookies;
* почистить localStorage;
* инициализировать какую-либо переменную теста.

Чтобы не прописывать эти действия в каждом тесте, вы можете описать их в настройках плагина в виде функции для хука `beforeEach`.

Аналогично, после завершения основных проверок в hermione-тесте, вы можете захотеть всегда проверять наличие ошибок в клиентском коде, срабатывание нужных метрик и т. п.

Чтобы не прописывать эти действия в каждом тесте, вы можете описать их в настройках плагина в виде функции для хука `afterEach`.

## Установка

```bash
npm install -D hermione-global-hook
```

## Настройка

Необходимо подключить плагин в разделе `plugins` конфига `hermione`:

```javascript
module.exports = {
    plugins: {
        'hermione-global-hook': {
            beforeEach: async function() {
                await this.browser.deleteCookie(); // Например, мы хотим всегда очищать cookies перед запуском теста
            },
            afterEach: async function() {
                await this.browser.execute(function() {
                    try {
                        localStorage.clear(); // И всегда очищать за собой localStorage после завершения теста
                    } catch (e) { }
                });
            }
        },

        // другие плагины гермионы...
    },

    // другие настройки гермионы...
};
```

### Расшифровка параметров конфигурации

| **Параметр** | **Тип** | **По&nbsp;умолчанию** | **Описание** |
| :--- | :---: | :---: | :--- |
| enabled | Boolean | true | Включить / отключить плагин. |
| beforeEach | Function | null | Асинхронная функция-обработчик, которая будет выполняться перед запуском каждого теста. |
| afterEach | Function | null | Асинхронная функция-обработчик, которая будет выполняться после завершения каждого теста. |

## Полезные ссылки

* [Исходники плагина hermione-global-hook][hermione-global-hook]
* [Команда browser.deleteCookie](https://webdriver.io/docs/api/webdriver/#deletecookie)
* [Команда browser.execute](https://webdriver.io/docs/api/browser/execute)
* [Команда browser.executeAsync](https://webdriver.io/docs/api/browser/executeAsync)

[hermione-global-hook]: https://github.com/gemini-testing/hermione-global-hook