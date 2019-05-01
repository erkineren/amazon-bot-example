## Amazon Bot Example

This repository was created for the question on the stackoverflow.

[https://stackoverflow.com/questions/55939207/simple-html-dom-not-requesting-amazon-page/55939679](https://stackoverflow.com/questions/55939207/simple-html-dom-not-requesting-amazon-page/55939679)

## Usage
```shell
git clone https://github.com/erkineren/amazon-bot-example.git .
composer install
```

## Code

```php

<?php

require __DIR__ . '/vendor/autoload.php';
require 'simple_html_dom.php';
use Curl\Curl;

// initialize curl
// you can install via "composer require php-curl-class/php-curl-class"
$curl = new Curl();

// set cookies
$curl->setCookieFile(__DIR__ . '/cookies.txt');
$curl->setCookieJar(__DIR__ . '/cookies.txt');

// decode gzip encoded because amazon is using gzip
$curl->setOpt(CURLOPT_ENCODING , "gzip");

// set request header like a browser
$curl->setHeaders([
    'accept' => 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3',
    'accept-encoding' => 'gzip, deflate, br',
    'accept-language' => 'en,tr;q=0.9',
    'user-agent' => 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36',
]);

// request
$curl->get('https://www.amazon.com/Apple-iPhone-Plus-Unlocked-32GB/dp/B01N6ZAR0D/');

// get raw response
$response = $curl->getRawResponse();

// parser
$html = new simple_html_dom();

// load from string html
$html->load($response);

// find price and print
$price = $html->find('#price', 0)->plaintext;
echo $price;


```