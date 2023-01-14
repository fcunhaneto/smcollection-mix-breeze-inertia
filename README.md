# Laravel Mix+Breeze+Inertia

### 1. Install Breeze + Inertia

```
composer require laravel/breeze --dev

php artisan breeze:install vue

npm install
```

### 2. Install Laravel Mix

```
npm install -D laravel-mix
```

### 3. Create a webpack.mix.js file in the root of your project:

```
const mix = require('laravel-mix');

mix.js('resources/js/app.js', 'public/js').vue()
    .postCss('resources/css/app.css', 'public/css', [
        //
    ])
    .alias({
        '@': 'resources/js',
    });

if (mix.inProduction()) {
    mix.version();
};
```

### 4. Replace in package.json

```
 "scripts": {
    "dev": "vite",
    "build": "vite build"
},
```

to:

```
"scripts": {
    "dev": "npm run development",
    "development": "mix",
    "watch": "mix watch",
    "watch-poll": "mix watch -- --watch-options-poll=1000",
    "hot": "mix watch --hot",
    "prod": "npm run production",
    "production": "mix --production"
},
```

### 5. Replace app.js code to this:

```
import './bootstrap';

import { createApp, h } from 'vue';
import { createInertiaApp } from '@inertiajs/inertia-vue3';
import { InertiaProgress } from '@inertiajs/progress';
import { ZiggyVue } from '../../vendor/tightenco/ziggy/dist/vue.m';

const appName = window.document.getElementsByTagName('title')[0]?.innerText || 'Laravel';

createInertiaApp({
    title: (title) => `${title} - ${appName}`,
    resolve: (name) => require(`./Pages/${name}.vue`),
    setup({ el, app, props, plugin }) {
        return createApp({ render: () => h(app, props) })
            .use(plugin)
            .use(ZiggyVue)
            .mount(el);
    },
});

InertiaProgress.init({ color: '#4B5563' });
```

### 6. In app.blade.php replace

```
@vite(['resources/js/app.js', "resources/js/Pages/{$page['component']}.vue"])
```

### to:

```
<link rel="stylesheet" href="{{ mix('css/app.css') }}">
<script src="{{ mix('js/app.js') }}" defer></script>
```

### 7. Replace in .env:

```
VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"

```

### to:

```
MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
```

### 8. Remove vite and laravel-vite-plugin and vite.config.js

```
npm remove vite laravel-vite-plugin
rm vite.config.js
```
