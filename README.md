# Vue Start — інтерактивний курс Vue 3

Україномовний практичний навчальний курс для тих, хто хоче зрозуміти Vue 3, навчитися створювати компоненти та зібрати власний frontend-застосунок.

## Навчання онлайн

**[Відкрити інтерактивний курс Vue](https://ulichigor.github.io/vue-start/)**

Курс має 12 модулів, приклади коду з копіюванням, практичні чеклісти, інтерактивні лабораторії, квізи та фінальний іспит із 12 запитань. Прогрес зберігається в localStorage браузера.

## Програма курсу

1. Знайомство з Vue та середовищем розробки.
2. Single File Components та шаблони.
3. Директиви та керування відображенням.
4. Реактивність: ref та reactive.
5. computed, watch та watchEffect.
6. Події та форми: v-on і v-model.
7. Компоненти: props та emits.
8. Slots та композиція компонентів.
9. Життєвий цикл та template refs.
10. Composables та повторне використання логіки.
11. Маршрутизація, стан і запити до API.
12. Налагодження, тестування, production-збірка та фінальний проєкт.

## Технології репозиторію

- Vue 3.2
- Vue CLI 5
- JavaScript (Options API у стартовому застосунку; Composition API у навчальних прикладах)
- Babel
- ESLint
- GitHub Actions + GitHub Pages

> **Примітка про інструменти:** репозиторій збережено на Vue CLI, щоб навчальний курс відповідав реальній структурі проєкту. Vue CLI перебуває в maintenance mode; для нових застосунків Vue рекомендує create-vue на базі Vite. Після проходження курсу можна виконати окрему міграцію.

## Запуск локально

Потрібен Node.js LTS і npm.

```bash
npm install
npm run serve
```

Після запуску відкрий адресу, яку покаже Vue CLI (типово http://localhost:8080).

## Команди

| Команда | Призначення |
|---|---|
| `npm install` | Встановити залежності |
| `npm run serve` | Запустити сервер розробки з hot reload |
| `npm run build` | Створити production-збірку в `dist/` |
| `npm run lint` | Перевірити JavaScript і Vue-компоненти |
| `npm run lint -- --fix` | Автоматично виправити частину стилістичних проблем |

## Структура

```
vue-start/
├── public/
│   └── index.html       # HTML-шаблон застосунку
├── src/
│   ├── assets/
│   │   └── course.css   # Стилі навчальної платформи
│   ├── components/      # Повторно використовувані компоненти
│   ├── App.vue          # Навчальна платформа та логіка курсу
│   └── main.js          # Точка входу Vue
├── .github/workflows/
│   └── deploy-course.yml
├── package.json
├── vue.config.js
└── README.md
```

## Як влаштований курс

- Перемикач модулів дозволяє переходити між темами.
- Чеклісти практики та завершені модулі зберігаються в браузері.
- Квіз кожного модуля показує правильну відповідь та пояснення.
- Лабораторії демонструють реактивний лічильник, фільтрацію, список задач, двостороннє зв’язування форм і архітектурні схеми.
- Фінальний тест має 12 запитань; прохідний бал — 10/12.
- Кнопка скидання очищує локальний навчальний прогрес.

## Публікація на GitHub Pages

Workflow `.github/workflows/deploy-course.yml` автоматично:
1. встановлює Node.js та залежності через `npm ci`;
2. запускає `npm run build`;
3. завантажує директорію `dist` як Pages artifact;
4. публікує артефакт через GitHub Pages.

У репозиторії відкрий **Settings → Pages → Build and deployment → Source** та вибери **GitHub Actions**. Для адреси `https://ulichigor.github.io/vue-start/` у `vue.config.js` встановлено production `publicPath: '/vue-start/'`.

## Рекомендований подальший розвиток

- Перехід на Vite/create-vue.
- Додавання Vue Router та Pinia до залежностей.
- TypeScript і строгі типи даних.
- Unit та component tests на Vitest + Vue Test Utils.
- API-шар, обробка авторизації та інтеграційні тести.
- CI-перевірки lint і build перед злиттям змін.

## Корисні посилання

- [Vue Guide](https://vuejs.org/guide/introduction.html)
- [Vue Router](https://router.vuejs.org/)
- [Pinia](https://pinia.vuejs.org/)
- [Vue CLI documentation](https://cli.vuejs.org/)
- [Vite](https://vite.dev/)
