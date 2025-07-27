
## Packages to install for a fresh Laravel project
Here is a list of default packages to install in a new fresh Laravel project 


### **1- Laravel UI Auth**

Link : [https://github.com/laravel/ui](https://github.com/laravel/ui)

Execute the command
``` php
composer require laravel/ui
```

``` php
php artisan ui bootstrap --auth
```
Or if you want to use Vue.js or ReactJS
``` php
php artisan ui vue --auth
php artisan ui react --auth
```

Now, install related Node packages
``` console
npm install
npm run dev
```


### **2- Laravel Permission**

Link : [https://github.com/spatie/laravel-permission](https://github.com/spatie/laravel-permission)

Execute the command
``` php
composer require spatie/laravel-permission
```

``` php
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```

In  **`App\Models\User.php`** file, add this

``` console
use Spatie\Permission\Traits\HasRoles;
```

Add this in User model class 

``` console
use HasFactory, Notifiable, HasRoles;
```


Now, install related Node packages
``` php
php artisan optimize:clear
php artisan migrate
```


### **3- JWT Auth**

Link : [php-open-source-saver/jwt-auth](php-open-source-saver/jwt-auth)

Execute the command
``` php
composer require php-open-source-saver/jwt-auth
```

``` php
php artisan vendor:publish --provider="PHPOpenSourceSaver\JWTAuth\Providers\LaravelServiceProvider"
```

Now, generate secret key for JWT
``` php
php artisan jwt:secret
```

In  **`App\Models\User.php`** file, add this

``` console
use PHPOpenSourceSaver\JWTAuth\Contracts\JWTSubject;
```

and add implementation of JWTSubject to User class **`implements JWTSubject`**

Then complete the User class with the two followin method

``` console

/**
    * Get the identifier that will be stored in the subject claim of the JWT.
    *
    * @return mixed
    */
public function getJWTIdentifier()
{
    return $this->getKey();
}

/**
    * Return a key value array, containing any custom claims to be added to the JWT.
    *
    * @return array
    */
public function getJWTCustomClaims()
{
    return [];
}
```




### **4- Swagger**

Link : [https://github.com/DarkaOnLine/L5-Swagger](https://github.com/DarkaOnLine/L5-Swagger)

Execute the command
``` php
composer require darkaonline/l5-swagger
```


### **5- Bootstrap 5 with Vite**

Link : [https://getbootstrap.com/docs/5.3/getting-started/vite/](https://getbootstrap.com/docs/5.3/getting-started/vite/)

* Install Vite

``` console
npm i --save-dev vite
```

* Install Bootstrap

``` console
npm i --save bootstrap @popperjs/core
```

``` console
npm install bootstrap-icons --save-dev
```

* In addition to Vite and Bootstrap, we need another dependency (Sass) to properly import and bundle Bootstrap’s CSS.

``` console
npm i --save-dev sass
```

* Create new file **`app.scss`** in resources folder **`resources/sass/app.scss`**

Add the following line in the file

**`@import 'bootstrap/scss/bootstrap';`**

Add the following line in the file to import icons

**`@import 'bootstrap-icons/font/bootstrap-icons.css';`**


* Make sure that in the file **`app.js`** in folder **`resources/js/app.js`** there the line

**`import './bootstrap';`**

Then, add also the line in the app.js file

**`import * as bootstrap from 'bootstrap';`**


* In your main blade file **`layout.app.blade.php`** or any other main layout file, add the following Vite line to make sure that CSS and JS is loaded

**`@vite(['resources/sass/app.scss', 'resources/js/app.js'])`**

* At the project root folder, make sure that the file **`vite.config.js`** contains the line

**`input : ['resources/sass/app.scss', 'resources/js/app.js']`**
So you must have the following content

``` console
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/sass/app.scss', 'resources/js/app.js'],
            refresh: true,
        }),
        tailwindcss(),
    ],
});
```




### **Final step to launch the project**

``` console
php artisan key:generate

npm install

npm run build

php artisan migrate

composer run dev
```

