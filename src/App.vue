<template>
  <div class="app-shell">
    <header class="topbar">
      <a class="brand" href="#top"><span class="brand-mark">V</span><span>VUE<span class="muted">.START</span></span></a>
      <div class="top-progress"><span>{{ progress }}% завершено</span><div class="progress-track"><i :style="{width:progress+'%'}"></i></div></div>
      <a class="github-link" href="https://github.com/UlichIgor/vue-start" target="_blank" rel="noopener noreferrer">GitHub ↗</a>
    </header>
    <main id="top" class="layout">
      <aside class="sidebar">
        <div class="side-label">НАВЧАЛЬНА ПРОГРАМА</div>
        <button v-for="(lesson,i) in lessons" :key="lesson.title" class="nav-item" :class="{active:current===i,done:done.includes(i)}" @click="current=i">
          <span class="nav-number">{{ done.includes(i) ? '✓' : String(i+1).padStart(2,'0') }}</span><span>{{ lesson.title }}</span>
        </button>
        <div class="side-bottom"><span class="side-label">ТВІЙ ПРОГРЕС</span><strong>{{ done.length }} із {{ lessons.length }} модулів</strong><button class="reset" @click="resetProgress">Скинути прогрес</button></div>
      </aside>
      <section class="main-content">
        <section class="hero">
          <div class="eyebrow"><span></span> ПРАКТИЧНИЙ КУРС · VUE 3 · УКРАЇНСЬКОЮ</div>
          <h1>Від першого компонента<br><em>до власного Vue-застосунку.</em></h1>
          <p>Вивчай Vue 3 крок за кроком: реактивність, компоненти, Composition API, роботу з даними, архітектуру та публікацію. Кожна тема має приклади, вправи й перевірку знань.</p>
          <div class="hero-tags"><span>12 модулів</span><span>12 практичних квізів</span><span>Фінальний іспит</span><span>Інтерактивні лабораторії</span></div>
          <a href="#lesson" class="start-button">Почати навчання ↓</a>
        </section>
        <div class="notice"><strong>Як проходити курс</strong><p>Читай пояснення, аналізуй приклади, запускай інтерактивні вправи, виконуй чеклісти та відповідай на питання. Прогрес зберігається локально в цьому браузері. Для роботи з кодом на комп'ютері використовуй команди npm із README.</p></div>
        <article id="lesson" class="lesson-card">
          <div class="lesson-kicker">МОДУЛЬ {{ String(current+1).padStart(2,'0') }} / {{ lessons.length }}</div>
          <h2>{{ lesson.title }}</h2><p class="intro">{{ lesson.intro }}</p>
          <section v-for="section in lesson.sections" :key="section.heading" class="lesson-section">
            <h3>{{ section.heading }}</h3><p v-if="section.text">{{ section.text }}</p>
            <ul v-if="section.points"><li v-for="point in section.points" :key="point">{{ point }}</li></ul>
            <div v-if="section.code" class="code-panel"><div class="code-head"><span>ПРИКЛАД КОДУ</span><button @click="copyCode(section.code)">{{ copied===section.heading ? 'Скопійовано ✓' : 'Копіювати' }}</button></div><pre><code>{{ section.code }}</code></pre></div>
            <p v-if="section.note" class="tip">{{ section.note }}</p>
          </section>
          <section v-if="lesson.lab" class="lab">
            <div class="lab-tag">ІНТЕРАКТИВНА ЛАБОРАТОРІЯ</div><h3>{{ lesson.lab.title }}</h3><p>{{ lesson.lab.description }}</p>
            <template v-if="lesson.lab.type==='counter'">
              <div class="counter"><button @click="counter--">−</button><strong>{{ counter }}</strong><button @click="counter++">+</button></div><p class="lab-output">Значення реактивного стану: <b>{{ counter }}</b></p><button class="small-action" @click="counter=0">Скинути лічильник</button>
            </template>
            <template v-else-if="lesson.lab.type==='filter'">
              <input v-model="filterText" class="lab-input" placeholder="Введи частину назви: наприклад, vue"><ul class="demo-list"><li v-for="item in filteredItems" :key="item">{{ item }}</li></ul><p class="lab-output">Знайдено елементів: {{ filteredItems.length }}</p>
            </template>
            <template v-else-if="lesson.lab.type==='todo'">
              <form class="todo-form" @submit.prevent="addTodo"><input v-model="newTodo" class="lab-input" placeholder="Нова задача..." aria-label="Нова задача"><button class="small-action">Додати</button></form>
              <ul class="todo-list"><li v-for="(todo,i) in todos" :key="i"><label><input v-model="todo.done" type="checkbox"><span :class="{struck:todo.done}">{{ todo.text }}</span></label><button @click="todos.splice(i,1)" aria-label="Видалити задачу">×</button></li></ul>
              <p class="lab-output">Виконано: {{ todos.filter(t=>t.done).length }} / {{ todos.length }}</p>
            </template>
            <template v-else-if="lesson.lab.type==='form'">
              <label class="lab-label">Твоє ім'я<input v-model.trim="formName" class="lab-input" placeholder="Наприклад, Олена"></label><label class="lab-label">Email<input v-model.trim="formEmail" class="lab-input" type="email" placeholder="name@example.com"></label><div class="lab-output">{{ formName ? 'Привіт, '+formName+'!' : 'Заповни форму, щоб побачити реактивне оновлення.' }} <span v-if="formEmail">Контакт: {{ formEmail }}</span></div>
            </template>
            <template v-else>
              <div class="concept-flow"><span v-for="(step,i) in lesson.lab.steps" :key="step"><b>{{ i+1 }}</b>{{ step }}</span></div>
            </template>
          </section>
          <section class="practice">
            <h3>Практичні завдання</h3><label v-for="(task,i) in lesson.tasks" :key="task" class="task-row"><input type="checkbox" :checked="!!tasks[current+'-'+i]" @change="toggleTask(i,$event.target.checked)"><span>{{ task }}</span></label>
          </section>
          <section class="quiz">
            <div class="lab-tag">ПЕРЕВІР СЕБЕ</div><h3>{{ lesson.quiz.question }}</h3>
            <button v-for="(answer,i) in lesson.quiz.answers" :key="answer" class="answer" :class="{correct:quizAnswers[current]===i && i===lesson.quiz.correct,wrong:quizAnswers[current]===i && i!==lesson.quiz.correct}" :disabled="quizAnswers[current]!==undefined" @click="answerQuiz(i)">{{ String.fromCharCode(65+i) }}. {{ answer }}</button>
            <p v-if="quizAnswers[current]!==undefined" class="explanation" role="status">{{ quizAnswers[current]===lesson.quiz.correct ? 'Правильно! ' : 'Спробуй ще раз у наступному повторенні. ' }}{{ lesson.quiz.explanation }}</p>
          </section>
          <div class="lesson-actions"><button class="secondary" :disabled="current===0" @click="current--">← Попередній</button><button class="complete" @click="markComplete">{{ done.includes(current) ? 'Модуль завершено ✓' : 'Позначити завершеним' }}</button><button class="secondary" :disabled="current===lessons.length-1" @click="current++">Наступний →</button></div>
        </article>
        <article class="lesson-card exam-card">
          <div class="lesson-kicker">ФІНАЛЬНА АТЕСТАЦІЯ</div><h2>Перевір свої знання</h2><p class="intro">12 запитань за матеріалами курсу. Для успішного складання потрібно набрати не менше 10 правильних відповідей.</p>
          <form @submit.prevent="submitExam">
            <fieldset v-for="(q,i) in exam" :key="q.question" class="exam-question"><legend>{{ i+1 }}. {{ q.question }}</legend><label v-for="(a,j) in q.answers" :key="a" class="exam-option"><input v-model="examAnswers[i]" type="radio" :value="j"> {{ a }}</label></fieldset>
            <button class="complete" type="submit">Перевірити іспит</button>
          </form>
          <div v-if="examResult!==null" class="exam-result" role="status"><h3>{{ examResult }} / {{ exam.length }} — {{ examResult>=10 ? 'Іспит складено!' : 'Потрібно повторити матеріал' }}</h3><p>{{ examResult>=10 ? 'Вітаємо! Ти завершив навчальну програму Vue.' : 'Переглянь модулі та спробуй ще раз. Для успіху потрібно 10 правильних відповідей.' }}</p><ol><li v-for="q in exam" :key="q.explanation">{{ q.explanation }}</li></ol></div>
        </article>
        <footer class="footer"><span>VUE.START · LEARN BY BUILDING</span><a href="https://vuejs.org/guide/introduction.html" target="_blank" rel="noopener noreferrer">Офіційна документація Vue ↗</a></footer>
      </section>
    </main>
  </div>
</template>

<script>
const lessons = [
{title:'Знайомство з Vue та середовищем',intro:'Розберися, що таке Vue, для яких задач він підходить і як влаштований саме цей стартовий репозиторій.',sections:[
{heading:'Що таке Vue',text:'Vue — прогресивний JavaScript-фреймворк для створення користувацьких інтерфейсів. Він дозволяє описувати інтерфейс декларативно: ти задаєш стан і правила відображення, а Vue оновлює DOM, коли стан змінюється. Vue можна додавати поступово або використовувати як основу повноцінного SPA.'},
{heading:'Що вже є в репозиторії',points:['Vue 3.2 — основна бібліотека інтерфейсу.','Vue CLI 5 — інструмент розробки, локальний сервер і production-збірка.','src/main.js — точка входу, яка створює Vue-застосунок і монтує його в #app.','src/App.vue — кореневий компонент, з якого починається інтерфейс.','public/index.html — HTML-шаблон, куди збирається застосунок.','package.json — залежності та команди npm.']},
{heading:'Підготовка середовища',code:`node --version\nnpm --version\nnpm install\nnpm run serve`,text:'Встанови Node.js LTS, відкрий термінал у папці проєкту й виконай команди послідовно. npm install встановлює залежності за package-lock.json. npm run serve запускає сервер розробки; термінал покаже локальну адресу.'},
{heading:'Vue 3 і версія проєкту',text:'Цей репозиторій використовує Vue 3 разом із Vue CLI. Vue CLI — зрілий, але вже не рекомендований для нових проєктів інструмент; для нового Vue-проєкту команда Vue рекомендує create-vue на базі Vite. У курсі ми навмисно вивчаємо наявний стек, а не переписуємо його без потреби.',note:'Для першого запуску цього репозиторію використовуй наявні npm scripts. Міграцію на Vite варто планувати окремим завданням після розуміння поточної збірки.'}],
tasks:['Перевір версії Node.js та npm.','Встанови залежності командою npm install.','Запусти npm run serve і відкрий локальну адресу.'],quiz:{question:'Який файл є точкою входу Vue-застосунку в цьому репозиторії?',answers:['src/main.js','README.md','public/favicon.ico'],correct:0,explanation:'src/main.js імпортує кореневий компонент, створює застосунок через createApp() і монтує його в DOM.'},lab:{title:'Ланцюжок запуску застосунку',description:'Подивись, як файли стартового проєкту пов’язані між собою.',type:'flow',steps:['public/index.html: контейнер #app','src/main.js: createApp(App)','src/App.vue: кореневий компонент','Vue рендерить інтерфейс у браузері']}},
{title:'Single File Components та шаблони',intro:'Навчися читати й створювати .vue-файли, поєднуючи HTML-подібний шаблон, JavaScript і CSS в одному компоненті.',sections:[
{heading:'Анатомія .vue-файлу',text:'Single File Component (SFC) — файл із розширенням .vue. Зазвичай він містить три секції: template для структури, script для логіки та style для оформлення. Атрибут scoped обмежує дію стилів компонентом.',code:`<template>\n  <section class="welcome">\n    <h1>{{ title }}</h1>\n  </section>\n</template>\n\n<script>\nexport default {\n  name: "WelcomeCard",\n  data() { return { title: "Привіт, Vue!" } }\n}\n</script>\n\n<style scoped>\n.welcome { padding: 1rem; }\n</style>`},
{heading:'Template syntax',text:'У шаблоні можна використовувати інтерполяцію {{ expression }} для тексту, v-bind або скорочення : для атрибутів, а також директиви v-if, v-for, v-on та інші. У виразах шаблону використовуй просту логіку; складні обчислення винеси в computed або методи.'},
{heading:'Як компонент потрапляє на сторінку',code:`import { createApp } from "vue"\nimport App from "./App.vue"\n\ncreateApp(App).mount("#app")`,text:'createApp(App) створює екземпляр застосунку. mount("#app") шукає HTML-елемент із таким селектором і монтує туди дерево компонентів.'}],
tasks:['Створи компонент ProfileCard.vue у src/components.','Додай template, script і scoped style.','Імпортуй компонент у App.vue та відобрази його.'],quiz:{question:'Яка секція SFC містить HTML-подібну розмітку компонента?',answers:['<style>','<template>','<script>'],correct:1,explanation:'Секція template описує структуру інтерфейсу, яку Vue компілює у функції рендерингу.'},lab:{title:'Складові компонента',description:'Кожна секція має окрему відповідальність.',type:'flow',steps:['template — що показати','script — як працює','style — як виглядає']}},
{title:'Директиви та керування відображенням',intro:'Навчися умовно показувати елементи, створювати списки, зв’язувати атрибути та обробляти класи.',sections:[
{heading:'Умовний рендеринг',text:'v-if додає або прибирає елемент із DOM залежно від умови. v-else-if і v-else утворюють ланцюжок. v-show натомість залишає елемент у DOM і перемикає CSS display. Часто змінювані елементи зручніше перемикати через v-show.' ,code:`<p v-if="isLoggedIn">Вітаємо в кабінеті!</p>\n<p v-else>Увійди, щоб продовжити.</p>\n\n<div v-show="isVisible">Цей блок перемикає display</div>`},
{heading:'Списки та key',text:'v-for повторює шаблон для масиву або об’єкта. Для списків компонентів завжди надавай стабільний :key, зазвичай ID запису. Не використовуй індекс як key, якщо список можна сортувати, видаляти або переставляти.' ,code:`<ul>\n  <li v-for="user in users" :key="user.id">\n    {{ user.name }}\n  </li>\n</ul>`},
{heading:'Динамічні атрибути й класи',code:`<a :href="profileUrl" :class="{ active: isActive }">\n  Профіль\n</a>\n<button :disabled="isSaving">Зберегти</button>`,text:'v-bind або : зв’язує атрибут із JavaScript-виразом. Для класів можна передати об’єкт або масив. disabled, href, src, aria-label та інші атрибути можна прив’язувати динамічно.'}],
tasks:['Зроби блок із v-if та v-else.','Відобрази масив елементів через v-for і стабільний key.','Додай умовний клас active через :class.'],quiz:{question:'Для чого потрібен :key у списку v-for?',answers:['Для прискорення CSS','Щоб Vue стабільно ідентифікував елементи списку','Щоб автоматично створити ID у базі даних'],correct:1,explanation:'key допомагає Vue правильно зіставляти вузли під час оновлення, особливо коли список змінюється.'},lab:{title:'Фільтрація елементів',description:'Зміни текст фільтра — список і кількість результатів оновлюються автоматично.',type:'filter'}},
{title:'Реактивність: ref та reactive',intro:'Це ядро Vue: зрозумій, як зміна даних автоматично оновлює інтерфейс.',sections:[
{heading:'Що означає реактивність',text:'Реактивність — механізм відстеження залежностей між станом і кодом, який його читає. Коли реактивне значення змінюється, Vue запускає потрібні оновлення. Тобі не потрібно вручну шукати DOM-елемент і змінювати його текст.'},
{heading:'ref() для примітивних і будь-яких значень',code:`import { ref } from "vue"\n\nconst count = ref(0)\nfunction increment() {\n  count.value++\n}`,text:'ref() повертає реактивний об’єкт із властивістю value. У JavaScript коді доступ здійснюється через .value. У template верхньорівневі ref зазвичай автоматично розгортаються, тому можна писати {{ count }}.'},
{heading:'reactive() для об’єктів',code:`import { reactive } from "vue"\n\nconst user = reactive({\n  name: "Олена",\n  points: 10\n})\nuser.points += 5`,text:'reactive() створює реактивний Proxy для об’єкта. Звертайся до властивостей напряму, без .value. Не замінюй змінну reactive-об’єкта новим об’єктом, якщо хочеш зберегти реактивне посилання; для таких сценаріїв часто краще ref.'},
{heading:'Коли що обрати',points:['ref — універсальний варіант, добре працює з примітивами та об’єктами.','reactive — зручний для групи пов’язаних властивостей об’єкта.','Не деструктуруй властивості reactive напряму без toRefs(), якщо потрібна реактивність.','У Composition API імпортуй ref/reactive з vue.']}],
tasks:['Створи ref-лічильник і кнопку збільшення.','Створи reactive-об’єкт користувача.','Перевір, що зміни відображаються без ручного оновлення DOM.'],quiz:{question:'Як змінити ref у JavaScript-функції?',answers:['count++','count.value++','count.set()'],correct:1,explanation:'У JavaScript ref читають і змінюють через .value. У шаблоні Vue розгортає його автоматично.'},lab:{title:'Реактивний лічильник',description:'Кнопки змінюють стан. Vue сам оновлює число в інтерфейсі.',type:'counter'}},
{title:'computed, watch та watchEffect',intro:'Розділяй похідні значення й побічні ефекти. Це робить код передбачуваним і легшим для підтримки.',sections:[
{heading:'computed — похідні значення',text:'computed створює кешоване значення, залежне від реактивних джерел. Поки залежності не змінюються, Vue повторно не виконує обчислення. Використовуй computed для фільтрованих списків, підсумків, форматованих назв та іншої логіки відображення.',code:`import { ref, computed } from "vue"\nconst price = ref(120)\nconst quantity = ref(2)\nconst total = computed(() => price.value * quantity.value)`},
{heading:'watch — реакція на конкретну зміну',text:'watch дозволяє виконати дію, коли змінюється задане джерело. Типові приклади: зберегти налаштування, синхронізувати URL або завантажити дані за новим пошуковим запитом. Для асинхронних операцій продумай обробку помилок і застарілих запитів.',code:`watch(searchQuery, async (newQuery, oldQuery) => {\n  if (!newQuery.trim()) return\n  await loadResults(newQuery)\n})`},
{heading:'watchEffect — автоматичне відстеження',text:'watchEffect одразу запускає функцію й автоматично відстежує реактивні значення, які були прочитані під час виконання. Він зручний для коротких побічних ефектів, але watch часто зрозуміліший, коли потрібно явно вказати джерело та отримати старе значення.'},
{heading:'Поширена помилка',note:'Не використовуй watch там, де достатньо computed. Обчислення даних — computed; виконання побічної дії у відповідь на зміну — watch/watchEffect.'}],
tasks:['Обчисли загальну суму через computed.','Додай watch для пошукового рядка.','Поясни, чим computed відрізняється від watch.'],quiz:{question:'Для чого найкраще підходить computed?',answers:['Для похідного значення на основі реактивних даних','Для реєстрації маршруту','Для створення HTML-елемента вручну'],correct:0,explanation:'computed призначений для похідних значень і кешує результат до зміни залежностей.'},lab:{title:'Пошук із computed-фільтром',description:'Список відфільтровується на основі реактивного значення input.',type:'filter'}},
{title:'Події та форми: v-on і v-model',intro:'Зроби інтерфейс інтерактивним: реагуй на кліки, введення, submit і валідуй дані форм.',sections:[
{heading:'Обробники подій',text:'v-on або скорочення @ прив’язує DOM-подію до методу чи виразу. Наприклад, @click="save" викликає функцію save. Модифікатори .prevent, .stop, .once та клавіатурні модифікатори дозволяють описувати типовий сценарій прямо в шаблоні.',code:`<button @click="count++">Додати</button>\n<form @submit.prevent="submitForm">\n  <input v-model="email" type="email">\n  <button type="submit">Надіслати</button>\n</form>`},
{heading:'Двостороннє зв’язування v-model',text:'v-model синхронізує значення поля форми з реактивним станом. Працює з input, textarea, select, checkbox і radio. Для модифікацій доступні .trim, .number і .lazy. Пам’ятай: v-model — це зручний синтаксичний цукор над value та подією оновлення.'},
{heading:'Валідація та UX',points:['Використовуй відповідні типи полів: email, tel, number.','Показуй зрозумілий текст помилки біля поля.','Вимикай кнопку під час надсилання, щоб уникнути повторного submit.','Перевіряй дані також на сервері: клієнтська валідація не є захистом.','Завжди підписуй поля через label.']}],
tasks:['Зроби форму з ім’ям та email через v-model.','Оброби submit із @submit.prevent.','Покажи повідомлення після успішного введення.'],quiz:{question:'Що робить модифікатор .prevent?',answers:['Зупиняє bubbling','Викликає preventDefault() для події','Видаляє елемент із DOM'],correct:1,explanation:'.prevent скасовує стандартну дію браузера для події, наприклад перезавантаження сторінки під час submit.'},lab:{title:'Жива форма',description:'Введи дані та спостерігай за оновленням інтерфейсу через v-model.',type:'form'}},
{title:'Компоненти: props та emits',intro:'Розбий інтерфейс на незалежні частини й організуй обмін даними між батьківськими та дочірніми компонентами.',sections:[
{heading:'Навіщо потрібні компоненти',text:'Компонент — незалежний повторно використовуваний блок інтерфейсу з власною розміткою, логікою та стилями. Розділяй інтерфейс за відповідальністю: Header, ProductCard, SearchForm, UserProfile. Це спрощує підтримку, тестування та повторне використання.'},
{heading:'Props: дані від батька до дитини',code:`<UserCard :name="user.name" :role="user.role" />\n\n<script setup>\nconst props = defineProps({\n  name: { type: String, required: true },\n  role: { type: String, default: "Гість" }\n})\n</script>`,text:'Props — вхідні параметри компонента. Дочірній компонент не повинен змінювати props напряму. Якщо потрібно змінити дані, повідом батьківський компонент через подію.'},
{heading:'Emits: події від дитини до батька',code:`<script setup>\nconst emit = defineEmits(["select"])\nfunction choose() { emit("select", 42) }\n</script>\n\n<button @click="choose">Обрати</button>\n\n<!-- Батьківський компонент -->\n<ItemCard @select="handleSelect" />`,text:'Дитина генерує подію через emit, а батько слухає її в шаблоні. Так дані рухаються вниз через props, а сигнали про дії — вгору через emits.'},
{heading:'Комунікація без хаосу',points:['Не передавай десятки непов’язаних props — переглянь межі компонента.','Не змінюй props напряму.','Називай події дієсловами або описом факту: save, select, update.','Для спільного стану між далекими компонентами розглянь provide/inject або store.']}],
tasks:['Створи компонент AlertMessage.vue з prop type.','Додай кнопку, яка генерує emit("close").','Оброби подію в батьківському компоненті.'],quiz:{question:'Який напрямок передавання даних є типовим для props?',answers:['Від дочірнього компонента до батьківського','Від батьківського компонента до дочірнього','Лише між браузерами'],correct:1,explanation:'Props передають вхідні дані згори вниз; emits дозволяють дочірньому компоненту повідомляти батька про події.'},lab:{title:'Потік даних між компонентами',description:'Запам’ятай односпрямований потік даних.',type:'flow',steps:['Батько передає props','Дитина показує дані','Дитина генерує emit','Батько обробляє подію']}},
{title:'Slots та композиція компонентів',intro:'Створи гнучкі компоненти-контейнери, у які батьківський компонент передає власний вміст.',sections:[
{heading:'Що таке slot',text:'Slot — місце в шаблоні дочірнього компонента, куди Vue вставляє вміст, переданий батьком. Це дозволяє створити універсальну картку або модальне вікно без жорсткої прив’язки до конкретного тексту.',code:`<!-- BaseCard.vue -->\n<template>\n  <article class="card">\n    <slot />\n  </article>\n</template>\n\n<!-- Батько -->\n<BaseCard>\n  <h2>Заголовок картки</h2>\n  <p>Вміст передається через slot.</p>\n</BaseCard>`},
{heading:'Іменовані слоти',code:`<template>\n  <section class="modal">\n    <header><slot name="header" /></header>\n    <main><slot /></main>\n    <footer><slot name="footer" /></footer>\n  </section>\n</template>\n\n<ModalWindow>\n  <template #header>Підтвердження</template>\n  Ви справді хочете продовжити?\n  <template #footer><button>Так</button></template>\n</ModalWindow>`,text:'Іменовані слоти дозволяють передавати окремі фрагменти для header, body та footer. Скорочення # еквівалентне v-slot:.'},
{heading:'Scoped slots',text:'Scoped slot дозволяє дочірньому компоненту передати дані назад у слот через атрибути. Батько отримує їх через slot props і сам вирішує, як відобразити елементи. Це корисно для універсальних списків і таблиць.'}],
tasks:['Створи BaseCard із default slot.','Додай іменовані слоти header та footer.','Передай дані через scoped slot і використай їх у батьківському шаблоні.'],quiz:{question:'Що дозволяє slot?',answers:['Передати вміст із батьківського компонента всередину дочірнього','Завантажити CSS з сервера','Створити глобальну змінну'],correct:0,explanation:'Слоти роблять компонент-контейнер гнучким: його вміст визначає батьківський компонент.'},lab:{title:'Композиція інтерфейсу',description:'Уяви компонент як контейнер із визначеними областями.',type:'flow',steps:['Батько передає контент','Default slot вставляє основний блок','Named slots задають окремі зони']}},
{title:'Життєвий цикл та template refs',intro:'Зрозумій, коли компонент створюється, оновлюється й видаляється, а також як звернутися до DOM-елемента.',sections:[
{heading:'Життєвий цикл компонента',text:'Vue-компонент проходить етапи створення, монтування, оновлення та демонтування. Composition API надає хуки onMounted, onUpdated, onUnmounted та інші. Вони дозволяють запускати код у правильний момент життєвого циклу.',code:`import { onMounted, onUnmounted } from "vue"\n\nonMounted(() => {\n  console.log("Компонент змонтовано")\n})\n\nonUnmounted(() => {\n  // Очистити таймери та підписки\n})`},
{heading:'Чому потрібне очищення',text:'Якщо компонент підписався на подію window, створив таймер або відкрив стороннє з’єднання, під час демонтування треба прибрати цей ресурс. Інакше можливі витоки пам’яті та небажані оновлення після закриття компонента.'},
{heading:'Template refs',code:`<script setup>\nimport { ref, onMounted } from "vue"\nconst inputEl = ref(null)\nonMounted(() => inputEl.value?.focus())\n</script>\n\n<template><input ref="inputEl"></template>`,text:'Template ref дає доступ до DOM-вузла або екземпляра дочірнього компонента. Використовуй його для фокусу, вимірювання елемента чи інтеграції з бібліотекою, а не для звичайного керування станом інтерфейсу.'}],
tasks:['Додай onMounted для логування.','Створи таймер і очищай його в onUnmounted.','Встанови фокус на input через template ref.'],quiz:{question:'Який хук викликається після монтування компонента?',answers:['onUnmounted','onMounted','computed'],correct:1,explanation:'onMounted виконується після того, як компонент змонтовано в DOM.'},lab:{title:'Життєвий цикл',description:'Основні етапи існування компонента.',type:'flow',steps:['setup: підготовка стану','mounted: DOM готовий','updated: стан змінився','unmounted: очищення ресурсів']}},
{title:'Composables та повторне використання логіки',intro:'Винось повторювану реактивну логіку в composable-функції та формуй чистішу архітектуру.',sections:[
{heading:'Що таке composable',text:'Composable — функція JavaScript, назва якої зазвичай починається з use, що використовує Composition API для інкапсуляції та повторного використання стану й поведінки. Вона повертає реактивні значення та функції.'},
{heading:'Приклад useCounter',code:`// src/composables/useCounter.js\nimport { ref, computed } from "vue"\n\nexport function useCounter(initial = 0) {\n  const count = ref(initial)\n  const doubled = computed(() => count.value * 2)\n  const increment = () => count.value++\n  const reset = () => { count.value = initial }\n  return { count, doubled, increment, reset }\n}`,text:'Тепер різні компоненти можуть викликати useCounter() і отримувати власний екземпляр стану. Якщо потрібен спільний стан між усіма компонентами, його треба винести у спільний модуль або store.'},
{heading:'Правила хорошого composable',points:['Одна зрозуміла відповідальність: useFetch, useCounter, useLocalStorage.','Поверни тільки те, що потрібно споживачу.','Описуй побічні ефекти та очищай підписки.','Не створюй неочікуваних глобальних singleton-станів.','Використовуй TypeScript у більших проєктах для контрактів даних.']}],
tasks:['Створи src/composables/useCounter.js.','Поверни count, increment і reset.','Використай composable у двох різних компонентах.'],quiz:{question:'Який префікс зазвичай використовують для composable-функцій?',answers:['get','use','makeComponent'],correct:1,explanation:'Composable-функції традиційно називають із префіксом use, наприклад useCounter або useFetch.'},lab:{title:'Як працює повторне використання',description:'Composable відділяє логіку від конкретного представлення.',type:'flow',steps:['Composable інкапсулює стан','Компонент викликає use-функцію','Компонент отримує реактивні дані','Інший компонент повторно використовує логіку']}},
{title:'Маршрутизація, стан і запити до API',intro:'Дізнайся, як перетворити компонентний інтерфейс на застосунок із кількома сторінками, спільним станом і зовнішніми даними.',sections:[
{heading:'Vue Router',text:'Vue Router — офіційний маршрутизатор Vue. Він пов’язує URL із компонентами сторінок, підтримує вкладені маршрути, параметри, навігаційні guards і lazy loading. У поточному package.json цього репозиторію Vue Router ще не встановлений; приклад показує, що додати під час розширення застосунку.',code:`npm install vue-router@4\n\n// router/index.js\nimport { createRouter, createWebHistory } from "vue-router"\nimport HomeView from "../views/HomeView.vue"\n\nexport default createRouter({\n  history: createWebHistory(),\n  routes: [{ path: "/", component: HomeView }]\n})`},
{heading:'Спільний стан і Pinia',text:'Для стану, який потрібен багатьом незалежним компонентам, використовують централізоване сховище. Pinia — рекомендований store для Vue. Він допомагає організувати state, getters і actions. Не перенось туди кожне локальне поле форми: локальний стан краще залишати в компоненті.',code:`npm install pinia\n\nimport { createPinia } from "pinia"\napp.use(createPinia())`},
{heading:'Запити до API',text:'Для HTTP-запитів можна використовувати вбудований fetch. Враховуй loading, error, порожню відповідь, скасування застарілого запиту та обробку HTTP-статусу. Не зберігай секретні API-ключі у фронтенд-коді.',code:`const loading = ref(false)\nconst error = ref(null)\nconst users = ref([])\n\nasync function loadUsers() {\n  loading.value = true\n  error.value = null\n  try {\n    const response = await fetch("/api/users")\n    if (!response.ok) throw new Error("Не вдалося завантажити дані")\n    users.value = await response.json()\n  } catch (err) { error.value = err.message }\n  finally { loading.value = false }\n}`},
{heading:'Архітектура папок',points:['src/views — сторінки, пов’язані з маршрутами.','src/components — повторно використовувані компоненти.','src/composables — повторно використовувана логіка.','src/router — конфігурація маршрутів.','src/stores — Pinia stores.','src/services — API-клієнти та зовнішні сервіси.']}],
tasks:['Спроєктуй три маршрути: /, /about і /profile.','Опиши, який стан має бути локальним, а який спільним.','Створи функцію fetch із loading та error.'],quiz:{question:'Який інструмент рекомендований для маршрутизації у Vue?',answers:['Vue Router','ESLint','Babel'],correct:0,explanation:'Vue Router — офіційний маршрутизатор Vue, який пов’язує URL і компоненти сторінок.'},lab:{title:'Архітектура застосунку',description:'Розподіляй код за призначенням, щоб проєкт залишався зрозумілим.',type:'flow',steps:['views — сторінки','components — UI-блоки','composables — логіка','stores — спільний стан','services — API']}},
{title:'Налагодження, тестування та якість коду',intro:'Навчися знаходити помилки не навмання, а через інструменти розробника, ESLint і перевірку сценаріїв.',sections:[
{heading:'Інструменти браузера',points:['Console — JavaScript-помилки, попередження та власні повідомлення.','Network — статуси запитів, відповіді API, завантаження ресурсів.','Elements — DOM і CSS, які реально відображаються.','Vue Devtools — дерево компонентів, props, events і стан Vue-застосунку.']},
{heading:'ESLint у цьому проєкті',text:'У package.json уже є ESLint і eslint-plugin-vue. Команда npm run lint перевіряє код відповідно до конфігурації. Лінтер допомагає знаходити типові помилки та підтримувати однаковий стиль, але не замінює тестування.',code:`npm run lint\nnpm run lint -- --fix`},
{heading:'Тестування компонентів',text:'Тестуй поведінку, яку бачить користувач: чи показується повідомлення, чи кнопка змінює стан, чи форма показує помилку. Для Vue 3 популярна комбінація Vitest і Vue Test Utils. У цьому репозиторії вони поки не встановлені — їх можна додати окремим етапом.',code:`npm install -D vitest @vue/test-utils jsdom`},
{heading:'План ручного тестування',points:['Порожній стан: що бачить користувач без даних?','Завантаження: чи зрозуміло, що запит виконується?','Помилка API: чи є повідомлення та повторна спроба?','Мобільний екран: чи доступні всі дії?','Клавіатура: чи видно фокус і чи можна пройти інтерфейс без миші?']}],
tasks:['Запусти npm run lint і виправ попередження.','Перевір Console та Network у браузері.','Опиши 5 тестових сценаріїв для своєї форми.'],quiz:{question:'Яка вкладка DevTools допоможе знайти помилку HTTP-запиту?',answers:['Network','Elements','Sources лише для CSS'],correct:0,explanation:'Network показує запити, HTTP-статуси, заголовки й відповіді сервера.'},lab:{title:'Цикл перевірки якості',description:'Налагодження — це повторюваний процес, а не одноразова дія.',type:'flow',steps:['Відтворити проблему','Перевірити Console і Network','Знайти причину','Виправити код','Повторити сценарій']}},
{title:'Production-збірка та публікація',intro:'Заверши курс практичним релізом: перевір production build, зрозумій base URL і підготуй проєкт до GitHub Pages.',sections:[
{heading:'Production build',text:'npm run build компілює застосунок у статичні файли директорії dist. Код мінімізується, ресурси оптимізуються та отримують хешовані імена. Саме dist, а не src, публікується на статичному хостингу.',code:`npm run build\n\n# Перевір вміст dist/\n# Локально переглянь production-збірку\nnpm run serve`},
{heading:'Base URL для GitHub Pages',text:'Якщо репозиторій опублікований як https://username.github.io/vue-start/, застосунку потрібен publicPath: "/vue-start/". Для власного домену або root-сайту налаштування буде іншим. Неправильний base path часто призводить до 404 для JS і CSS.'},
{heading:'Автоматичний deployment',text:'GitHub Actions може запускати npm ci, npm run build, завантажувати dist як Pages artifact і публікувати його. Workflow повинен мати permissions contents: read, pages: write, id-token: write та використовувати офіційні actions/configure-pages, upload-pages-artifact і deploy-pages.'},
{heading:'Чекліст релізу',points:['npm ci завершується без помилок.','npm run lint не показує проблем.','npm run build створює dist.','Перевірені маршрути та ресурси з base URL.','Немає секретів у клієнтському коді.','GitHub Pages налаштований на GitHub Actions.','Після релізу перевірено сайт у браузері.'],note:'Статичний GitHub Pages не запускає Node.js сервер чи backend. Для API, авторизації та приватних секретів потрібен окремий серверний сервіс.'}],
tasks:['Виконай npm run build.','Перевір, що директорія dist створена.','Перевір налаштування GitHub Pages та відкрий опублікований сайт.'],quiz:{question:'Яку директорію потрібно публікувати після Vue CLI build?',answers:['src','node_modules','dist'],correct:2,explanation:'Vue CLI генерує готові статичні файли у dist, саме цю директорію розгортають на статичному хостингу.'},lab:{title:'Шлях до публікації',description:'Побудуй правильний ланцюжок від коду до сайту.',type:'flow',steps:['Git push у main','GitHub Actions: npm ci','npm run build → dist','Upload Pages artifact','Deploy GitHub Pages']}},
{title:'Фінальний проєкт: застосунок задач',intro:'Закріпи навички, спроєктувавши невеликий застосунок керування задачами з компонентами, формами, реактивністю та збереженням стану.',sections:[
{heading:'Технічне завдання',points:['Створи інтерфейс із заголовком і коротким описом застосунку.','Додай форму створення задачі з перевіркою порожнього значення.','Відображай список задач через v-for із унікальним id як key.','Дозволь позначати задачу виконаною та видаляти її.','Додай фільтри: усі, активні, виконані.','Винеси TaskItem і TaskForm в окремі компоненти.','Використай props для даних та emits для дій.','Збережи дані у localStorage через composable або watch.','Перевір адаптивність, доступність та production-збірку.']},
{heading:'Рекомендована структура',code:`src/\n├── main.js\n├── App.vue\n├── components/\n│   ├── TaskForm.vue\n│   ├── TaskItem.vue\n│   └── TaskFilters.vue\n├── composables/\n│   └── useTasks.js\n├── assets/\n└── styles/\n    └── main.css`},
{heading:'Критерії готовності',points:['Додавання, завершення та видалення задач працює.','Фільтри показують правильні результати.','Стан не губиться після перезавантаження браузера.','Компоненти мають чіткі props/emits контракти.','Немає помилок у Console і проблем lint.','npm run build завершується успішно.']},
{heading:'Що вивчати далі',text:'Після базового курсу переходь до TypeScript, Vue Router, Pinia, тестування з Vitest, доступності, продуктивності та інтеграції з API. Офіційний Vue Guide — найкраще місце для поглиблення знань.'}],
tasks:['Створи застосунок задач із компонентами.','Додай фільтрацію та збереження стану.','Пройди фінальний іспит і опублікуй production build.'],quiz:{question:'Яка практика найкраще допомагає масштабувати Vue-застосунок?',answers:['Увесь код у одному App.vue','Компоненти з чіткою відповідальністю та повторно використовувані composables','Маніпуляція DOM у кожному обробнику'],correct:1,explanation:'Поділ на компоненти, composables, views, stores і services робить застосунок зрозумілим та підтримуваним.'},lab:{title:'Архітектура фінального завдання',description:'Збери застосунок із незалежних, зрозумілих частин.',type:'flow',steps:['TaskForm: створення','useTasks: стан і логіка','TaskItem: одна задача','TaskFilters: фільтрація','App: композиція інтерфейсу']}}
];

const exam = [
['Який файл монтує Vue-застосунок у DOM?',['src/main.js','README.md','package.json'],0,'main.js викликає createApp(App).mount("#app").'],
['Яка секція SFC описує розмітку?',['script','template','style'],1,'template містить декларативну розмітку компонента.'],
['Яка директива умовно додає елемент у DOM?',['v-if','v-for','v-model'],0,'v-if створює або прибирає елемент залежно від умови.'],
['Як змінити ref у JavaScript?',['value++','ref.value++','ref.update()'],1,'У script доступ до значення ref здійснюється через .value.'],
['Для чого призначений computed?',['Похідні кешовані значення','HTTP-запити','Реєстрація компонентів'],0,'computed обчислює похідне значення та кешує його до зміни залежностей.'],
['Що робить v-model?',['Синхронізує значення поля з реактивним станом','Видаляє поле','Створює маршрут'],0,'v-model забезпечує двостороннє зв’язування значення форми зі станом.'],
['Як передають дані від батька до дитини?',['emits','props','slots only'],1,'props передають вхідні дані вниз по дереву компонентів.'],
['Що дозволяє slot?',['Передавати вміст у компонент-контейнер','Запускати API','Створювати CSS-клас'],0,'Slot вставляє контент батьківського компонента у визначене місце дочірнього.'],
['Який хук запускається після монтування?',['onMounted','onUnmounted','watch'],0,'onMounted викликається після додавання компонента до DOM.'],
['Що таке composable?',['Повторно використовувана функція Composition API','Тип HTML-тега','Файл конфігурації Babel'],0,'Composable інкапсулює та повторно використовує реактивну логіку.'],
['Яка бібліотека є офіційним маршрутизатором Vue?',['Pinia','Vue Router','ESLint'],1,'Vue Router керує URL і відображенням компонентів сторінок.'],
['Яку папку генерує Vue CLI build?',['src','dist','public'],1,'Production build генерує готові статичні файли у dist.']
];

export default {
  name: 'App',
  data() {
    let saved = {};
    try { saved = JSON.parse(localStorage.getItem('vue-start-course-v1') || '{}'); } catch (e) { saved = {}; }
    return {
      lessons, exam, current: 0, done: saved.done || [], tasks: saved.tasks || {},
      quizAnswers: saved.quizAnswers || {}, examAnswers: {}, examResult: saved.examResult ?? null,
      counter: 0, filterText: '', filterItems: ['Vue 3','React','Composition API','JavaScript','Components','Vite'],
      todos: [{text:'Вивчити ref()',done:false},{text:'Створити компонент',done:true}],
      newTodo: '', formName: '', formEmail: '', copied: ''
    };
  },
  computed: {
    lesson() { return this.lessons[this.current]; },
    progress() { return Math.round(this.done.length / this.lessons.length * 100); },
    filteredItems() { return this.filterItems.filter(item => item.toLowerCase().includes(this.filterText.toLowerCase())); }
  },
  watch: {
    done: { deep: true, handler() { this.persist(); } },
    tasks: { deep: true, handler() { this.persist(); } },
    quizAnswers: { deep: true, handler() { this.persist(); } },
    examResult() { this.persist(); }
  },
  methods: {
    persist() { localStorage.setItem('vue-start-course-v1', JSON.stringify({done:this.done,tasks:this.tasks,quizAnswers:this.quizAnswers,examResult:this.examResult})); },
    toggleTask(index, checked) { this.tasks[this.current+'-'+index] = checked; },
    answerQuiz(index) { this.quizAnswers[this.current] = index; },
    markComplete() { if (!this.done.includes(this.current)) this.done.push(this.current); },
    resetProgress() { if (window.confirm('Скинути весь збережений прогрес курсу?')) { this.done=[];this.tasks={};this.quizAnswers={};this.examResult=null;this.examAnswers={}; } },
    addTodo() { const text=this.newTodo.trim(); if(text){this.todos.push({text,done:false});this.newTodo='';} },
    submitExam() { this.examResult=this.exam.reduce((score,q,i)=>score+(Number(this.examAnswers[i])===q[2]?1:0),0); },
    async copyCode(code) { try { await navigator.clipboard.writeText(code);this.copied=this.lesson.sections.find(s=>s.code===code)?.heading || 'code'; } catch(e) { this.copied='';window.alert('Браузер не дозволив скопіювати код. Виділи його вручну.'); } }
  }
};
</script>

<style>
@import './assets/course.css';
</style>
