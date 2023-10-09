---
title: 每次遇到每次忘的 PHP syntax
date: 2023-09-18 22:05:43
tags: PHP
description: 第一次接觸 PHP 專案是在公司的掃描工具，在看著 design document，搭配著混雜著 html 的 PHP file。雖然看得懂關鍵字在幹嘛，但一遇到其他語法沒有的 `=>`, `->`, `::`以及明明叫做 array 卻長得像 map 的東西感到困惑，於是誕生此篇文章，去記錄一開始在遇到 PHP 時，需要翻筆記的語法。
---
## Object Operator (->)
用來存取物件的屬性或是方法，跟其他語言的 `.` 一樣，但在 PHP 中，`.` 被用來當作 string 連接用。
在stack overflow 上的這篇文章 [What does this mean in PHP: -> or => ?](https://stackoverflow.com/questions/14037290/what-does-this-mean-in-php-or)，一樓的留言有各種語言的範例接龍，可以朝聖。
```php
$person = new Person();
$person -> name = 'Samuel';
$person -> run();
```

## Double Arrow Operator (=>)
用來定義陣列的 key 跟 value，跟其他語言的 `:` 相似，而這種 key-value pair 的陣列在 PHP 中被稱為 associative array，跟其他語言的 array 不太相同，反而更像是 map 或是 dictionary。
同時他也可以用來當作 arrow function 的 Syntactic Sugar (PHP 7.4 以上)。
```php
$person = [
  "name" => "Samuel",
  "age" => 24
]
// or
$person = array(
  "name" => "Samuel",
  "age" => 24
)
```

## Scope Resolution Operator (::)
主要用來存取 static property, static method 或是 constant。
```php
class Person {
  const NAME = "Samuel";
  static $age = 24;
  static function run() {
    echo "I'm running";
  }
}
$value = Person::$age;
$method = Person::run();
```
