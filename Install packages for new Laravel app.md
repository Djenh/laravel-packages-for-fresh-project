
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

In  **`bootstrap\app.php`** file, add/complete this

``` console
->withMiddleware(function (Middleware $middleware): void {
    $middleware->alias([
        'role' => \Spatie\Permission\Middleware\RoleMiddleware::class,
        'permission' => \Spatie\Permission\Middleware\PermissionMiddleware::class,
        'role_or_permission' => \Spatie\Permission\Middleware\RoleOrPermissionMiddleware::class,
    ]);
})
```

In  **`config\auth.php`** file, add/complete this

``` console
'guards' => [
    'api' => [
        'driver' => 'jwt', // Or 'jwt' if using JWT, or 'passport' if using Passport, or 'sanctum' if using Laravel Sanctum
        'provider' => 'users',
    ],
    'web' => [
        'driver' => 'session',
        'provider' => 'users',
    ],    
],
```

Now, clear cache and migrate tables

``` php
php artisan permission:cache-reset
```

``` php
php artisan cache:clear
```

``` php
php artisan optimize:clear
```

``` php
php artisan migrate
```


### **3- JWT Auth**

Link : [https://github.com/PHP-Open-Source-Saver/jwt-auth](https://github.com/PHP-Open-Source-Saver/jwt-auth)

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

public function getJWTIdentifier()
{
    return $this->getKey();
}


public function getJWTCustomClaims()
{
    return [];
}
```

In  the file `app\config\auth.php`, add this in authentication guards key.

``` console

'api' => [
    'driver' => 'jwt',
    'provider' => 'users',
],
```

To get the logged in user, use the following syntax
``` php
$user = Auth::guard('api')->user();
```





### **4- Swagger**

Link : [https://github.com/DarkaOnLine/L5-Swagger](https://github.com/DarkaOnLine/L5-Swagger)

Execute the command
``` php
composer require darkaonline/l5-swagger
```

``` php
php artisan vendor:publish --provider "L5Swagger\L5SwaggerServiceProvider"
```


Open the base Controller file at `App\Http\Controllers\Controller.php` and add the following lines

``` console
use OpenApi\Attributes as OA;

#[OA\Info(
    version: "1.0.0",
    title: "MyApp API Documentation"
)]
#[OA\SecurityScheme(
    securityScheme: 'bearerAuth',
    type: 'http',
    scheme: 'bearer',
    bearerFormat: 'JWT'
)]
```

And then in any Controller, like `UserController.php` 

``` console
use OpenApi\Attributes as OA;

class UserController extends Controller {
#[OA\Get(
    path: '/api/users',
    tags: ['User'],
    summary: 'Liste des utilisateurs',
    security: [['bearerAuth' => []]],
    parameters: [
    new OA\Parameter(
        name: 'page',
        in: 'query',
        required: false,
        schema: new OA\Schema(type: 'integer', example: 1)
    )
    ],
    responses: [
    new OA\Response(
        response: 200,
        description: 'Liste des utilisateurs',
        content: new OA\JsonContent(
            properties: [
                new OA\Property(property: 'data', type: 'array', items: new OA\Items(
                    properties: [
                        new OA\Property(property: 'id', type: 'integer'),
                        new OA\Property(property: 'name', type: 'string'),
                        new OA\Property(property: 'email', type: 'string'),
                        new OA\Property(property: 'uuid', type: 'string'),
                    ]
                ))
            ]
        )
    ),
    new OA\Response(response: 401, description: 'Non authentifié')
    ]
)]
public function index()
{
// Code
}
}
```


To generate docs, execute
``` php
php artisan l5-swagger:generate
```

You can access your documentation at `/api/documentation` endpoint.

For more information about the Swagger, check the documentation from [https://github.com/zircote/swagger-php](https://github.com/zircote/swagger-php)




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

* Create new file **`app.scss`** in resources folder `resources/sass/app.scss`

Add the following line in the file

``` console
@import 'bootstrap/scss/bootstrap';
```


Add the following line in the same file to import icons

``` console
@import 'bootstrap-icons/font/bootstrap-icons.css';
```


* Make sure that in the file `app.js` in folder `resources/js/app.js` there is the line

**`import './bootstrap';`**

Then, add also the line in the app.js file

**`import * as bootstrap from 'bootstrap';`**


* In your main blade file `layout.app.blade.php` or any other main layout file, add the following Vite line to make sure that CSS and JS is loaded

``` console
@vite(['resources/sass/app.scss', 'resources/js/app.js'])
```


* At the project root folder, make sure that the file **`vite.config.js`** contains the line


`input : ['resources/sass/app.scss', 'resources/js/app.js']`
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

