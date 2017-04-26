# Composer in action

composer 為 PHP class (package) 管理工具

[composer 官方網站](https://getcomposer.org/)


## 重點
---- 

- composer 安裝
- composer 基本用法
- 了解 PHP autoloading 
- 了解如何用 composer 管理 namespace / autoload


## 系統需求
----

- PHP 5.3.2+

## 安裝 composer
----

```bash
curl -sS https://getcomposer.org/installer | php
```

### global 執行 composer 

(略) 

```sh
export $COMPOSER_HOME/vendor/bin
```


## 開始使用 composer
----

### 建立專案

`composer init` 可建立 composer 專案

完成後目錄下生成 `composer.json` 

###  管理專案

`composer.json`  是 composer 專案設定檔，內容包含：

- 專案
    - 名稱
    - 描述
    - 關鍵字
    - 類別

- require package 
    - package name
    - package version 限制
    - require-dev package

> composer 會自動處理 package 依賴 

- 管理 namespace / autoload

> 修改 autoload classmap 以後，記得執行 `composer dump-autoload` 更新 autoload.php

- scripts
- config

### composer 內容更新

更新 package 版本、依賴、並重做 `composer.lock`。  
等於重新做 `composer install`

更新所有 package 

```bash
composer update
```

只更新指定 package

```bash
composer update vendor/package vendor/package2
```

批次更新 package

```bash
composer update vendor/*
```


### composer 自我升級

```bash
composer self-update
```


## Packagist
----

[Packagist](https://packagist.org) 是 composer 主要資源庫。尋找相關的 package。 

編輯  `composer.json`  以 require package


### require package

要 require package ，需要 package name 跟 package version

```json
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

或執行下面指令 require package 

```bash
composer require monolog/monolog:1.0.*
```

與直接編輯  `composer.json` 同效果


### composer install

`composer install` 

- 可用參數
    + --prefer-source：
    + --prefer-dist：

> 參數 -v|vv|vvv 能輸出過程訊息，1 for normal output, 2 for more verbose output and 3 for debug  

- composer 會自動安裝  package 依賴

- 安裝後會產生檔案
    + composer.lock
    + vendor/autoload.php
    + vendor/${package} 

> 假如使用 `git` 管理檔案，通常會在 .gitignore 加入 vendor/   


## PHP autoload

PHP autoload 機制可以讓我們能夠在需要這個物件的時候才去真正的引入這個 Class。  
實作法大致為以下：

- __autoload
- spl_autoload
- autoload 與 namespace

composer 能簡單管理 autoload 與 namespace  

### __autoload

透過 function __autoload 來實現

```php
//autoload.php
function __autoload($className) {    
    $filename = __DIR__ . "/classes/" . $className . ".php";
    if (is_readable($filename)) {
        require $filename;
    }
}
```

### spl_autoload 與 spl_autoload_register

(太麻煩了，忽略不用)


### autoload 與 namespace

(略，大致上就是 composer 實作 autoload 的方式)


## 以 composer 管理 autoload 與 namespace  

composer 能輕鬆管理 autoload 與 namespace 。

編輯 `composer.json` 

```json
{
    "autoload": {
        "classmap": [
            "database"
        ],
        "psr-4": {"Acme\\": "src/"}
    }
}
```

### 生成 autoload.php 

`composer install` 後生成 `vendor/autoload.php`，在 .php 引入 autoload.php 以使用 autoload。

```php
require __DIR__ . '/vendor/autoload.php';
```

### 更新 autoload.php 

在某些時候，比如說 `composer.json`  裡面 autoload classmap 內容被修改過了，就需要更新 autoload.php 

`composer dump-autoload` 

### 最佳化 autoloading files

--optimize (-o): 轉換 PSR-0/4 autoloading 到 classmap 獲得更快的載入速度。  
--no-dev: 禁用 autoload-dev 規則。
