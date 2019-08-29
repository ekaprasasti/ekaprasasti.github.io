---
layout: post
title:  'Laravel Passport Authentication'
image: '/assets/img/laravel-passport-authentication/generate-key.png'
date:   2019-08-29 00:05:03
tags:
- PHP
- Laravel
- Learn
description: 'Dari pada tulisannya cuman sampe sini mending sekalian deh saya pelajarin dan tulis lagi tutorial cara bikin autentikasi di laravel pake passport di artikel ini.'
categories:
- Programming
- Learn
serie: PHP
---

Gak enak emang ketika kita dapet legacy code yang gak ada dokumentasinya, bahkan gak ada standard yang jelas code style pada sebuah tim atau perusahaan. Biasanya (bukan standarnya, karena standar setiap perusahaan beda) di tempat saya kerja untuk laravel handle api itu pasti pake JWT, ketika saya dapet legacy code dan harus handle ambil data credential user dari token saya panggil class JWTAuth, ternyata class not found, usut punya usut ternyata ini project pake Passport untuk handle authentikasi nya. Cari sana sini gimana cara ambil dan ekstrak data token dari passport gampang sintaksnya cuman pake `$request->user('api')` aja. Agak kagok emang karena emang saya belum pernah pake passport sebelumnya.

Dari pada tulisannya cuman sampe sini mending sekalian deh saya pelajarin dan tulis lagi tutorial cara bikin autentikasi di laravel pake passport di artikel ini.

## Step 1 Install Laravel

```
laravel new laravel-passport
```

Setalah itu kita buat database di mysql dan config di file .env credential database. Contohnya seperti ini.



## Step 2 Install Laravel Passport Library

```
composer require laravel/passport
```

![env](/assets/img/laravel-passport-authentication/env.png)

## Step 3 Jalankan Migration

```
php artisan migrate
```

![env](/assets/img/laravel-passport-authentication/migration.png)

Karena kita sudah menginstall passport maka laravel menjalankan migration tambahan untuk membuat table oauth.

![table](/assets/img/laravel-passport-authentication/table.png)

## Step 4 Generate Keys

```
php artisan passport:install
```

Command ini akan membuat encryption keys untuk men-generate access token.

![generate key](/assets/img/laravel-passport-authentication/generate-key.png)

> In addition, the command will create "personal access" and "password grant" clients which will be used to generate access tokens. [Laravel Docs](https://laravel.com/docs/5.8/passport#installation)

## Step 5 Passport Config

Pada model user, buka file `app/User.php` dan tambahkan kode berikut.

```php
<?php


namespace App;

use Laravel\Passport\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens;
}
```

Lalu buka file `app/Providers/AuthServiceProvider.php`, dan panggil `Laravel\Passport` dan juga tambahkan kode `Passport::routes()` pada function `boot`.

```php
<?php


namespace App\Providers;


use Laravel\Passport\Passport;


class AuthServiceProvider extends ServiceProvider
{
    public function boot()
    {
        Passport::routes();
    }
}
```

> This method will register the routes necessary to issue access tokens and revoke access tokens, clients, and personal access tokens. [Laravel Docs](https://laravel.com/docs/5.8/passport#installation)

Setelah itu pada file `config/auth.php`, set api driver ke `passport`.

```php
'guards' => [
    'api' => [
        'driver' => 'passport',
        'provider' => 'users',
        'hash' => false,
    ],
],
```

> This will instruct your application to use Passport's *TokenGuard* when authenticating incoming API requests. [Laravel Docs](https://laravel.com/docs/5.8/passport#installation)

## Step 6 Membuat API Routes

Kita akan membuat rooting yang di butuhkan dengan menulis kode berikut pada file `routes/api.php`.

```php
Route::group([
    'prefix' => 'auth'
], function () {
    Route::post('login', 'AuthController@login');
    Route::post('signup', 'AuthController@signup');


    Route::group([
        'middleware' => 'auth:api'
    ], function () {
        Route::get('logout', 'AuthController@logout');
        Route::get('user', 'AuthController@user');
    });
});
```

## Step 7 Membuat Controller

Kita akan provide controller dan method apa saja yang di butuhkan pada routes yang sudah kita define sebelumnya. Buat file `app/AuthController.php` dan ketikan kode berikut.

```php
<?php


namespace App\Http\Controllers;


use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Carbon\Carbon;
use App\User;


class AuthController extends Controller
{
    public function signup(Request $request)
    {
        $request->validate([
            'name' => 'required|string',
            'email' => 'required|string|email|unique:users',
            'password' => 'required|string'
        ]);


        $user = new User([
            'name' => $request->name,
            'email' => $request->email,
            'password' => bcrypt($request->password)
        ]);


        $user->save();


        return response()->json([
            'message' => 'Successfully created user!'
        ], 201);
    }


    public function login(Request $request)
    {
        $request->validate([
            'email' => 'required|string|email',
            'password' => 'required|string',
            'remember_me' => 'boolean'
        ]);


        // ambil array value email dan password
        $credentials = request(['email', 'password']);


        // cek credential cocok gak dgn yg di database
        if (!Auth::attempt($credentials)) {
            return response()->json([
                'message' => 'Unauthorized'
            ], 401);
        }


        // $request->user() returns an instance of the authenticated user..
        $user = $request->user();


        // https://laravel.com/docs/5.8/passport#managing-personal-access-tokens
        $tokenResult = $user->createToken('Personal Access Token');
        $token = $tokenResult->token;


        if ($request->remember_me) {
            // kasih expired seminggu
            $token->expires_at = Carbon::now()->addWeeks(1);
        }


        // save token expired kalo gak remember me expirednya default sekitar 5 jam
        $token->save();


        return response()->json([
            'access_token' => $tokenResult->accessToken,
            'token_type' => 'Bearer',
            'expires_at' => Carbon::parse(
                $tokenResult->token->expires_at
            )->toDateTimeString()
        ]);
    }


    public function user(Request $request)
    {
        // jangan lupa kasih header authorization Bearer token
        return response()->json($request->user());
    }


    public function logout(Request $request)
    {
        // unauthorized token
        $request->user()->token()->revoke();


        return response()->json([
            'message' => 'Successfully logged out'
        ]);
    }
}
```

Setelah itu bisa kalian coba di postman atau client api manapun yang kalian suka routing yang telah kita buat. Jika ada pertanyaan dan kesulitan kita diskusi di kolom komentar ya. Oh ya tutorial ini saya pelajari di medium [Alfredo Barron](https://medium.com/modulr/create-api-authentication-with-passport-of-laravel-5-6-1dc2d400a7f) dan bisa juga sourcecode nya di [github saya](https://github.com/ekaprasasti).