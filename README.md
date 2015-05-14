# composer in action

## 說明

介紹 composer 基礎入門

## 官方網站

[https://getcomposer.org/](https://getcomposer.org/)

## 安裝

`curl -sS https://getcomposer.org/installer | php`

## 開始使用 composer

### 建立專案檔案 composer.json

`composer init`

設定完後接著執行

`composer install`

### Autoloading

spl autoloading

```php
require __DIR__ . '/vendor/autoload.php';
```
