# Sberbank::Acquiring

[![Build Status](https://travis-ci.org/panasyuk/sberbank-acquiring.svg?branch=master)](https://travis-ci.org/panasyuk/sberbank-acquiring)

## Описание

GEM sberbank-acquiring предоставляет функциональность для взаимодействия с API эквайринга банка Сбербанк.

## Установка

```ruby
# Gemfile
gem 'sberbank-acquiring', github: 'panasyuk/sberbank-acquiring'
```

## Использование

### Sberbank::Acquiring::RestClient

```ruby
rest_client = Sberbank::Acquiring::RestClient.new(username: 'username', password: 'password')
```

#### Создание заказа на 10 рублей
```ruby
rest_client.get(
  'register',
  'amount' => 1000,
  'orderNumber' => 'order#1',
  'returnUrl' => 'https://example.com/sberbank/success'
)
```

```ruby
{
  "orderId" => "f3ced54d-45df-7c1a-f3ce-d54d04b11830",
  "formUrl" => "https://3dsec.sberbank.ru/payment/merchants/sbersafe/payment_ru.html?mdOrder=f3ced54d-45df-7c1a-f3ce-d54d04b11830"
}
```

#### Проверка состояния заказа
```ruby
rest_client.get(
  'getOrderStatusExtended',
  'orderId' => 'f3ced54d-45df-7c1a-f3ce-d54d04b11830'
)
```
или
```ruby
rest_client.get(
  'getOrderStatusExtended',
  'orderNumber' => 'order#1'
)
```

```ruby
{
  "errorCode" => "0",
  "errorMessage" => "Успешно",
  "orderNumber" => "order#2",
  "orderStatus" => 0,
  "actionCode" => -100,
  "actionCodeDescription" => "",
  "amount" => 1000,
  "currency" => "643",
  "date" => 1531643056391,
  "merchantOrderParams" => [],
  "attributes" => [{ "name" => "mdOrder", "value" => "aefeb658-48fb-7f37-aefe-b65804b11830" }],
  "terminalId" => "123456",
  "paymentAmountInfo" => { "paymentState" => "CREATED", "approvedAmount" => 0, "depositedAmount" => 0,  "refundedAmount" => 0},
  "bankInfo" => { "bankCountryCode" => "UNKNOWN", "bankCountryName" => "<Неизвестно>" }
}
```

Запрос состояния заказа с результатом на английском языке:
```ruby
rest_client.get(
  'getOrderStatusExtended',
  'language' => 'en',
  'orderNumber' => 'order#1'
)
```

```ruby
{
  "errorCode" => "0",
  "errorMessage" => "Success",
  "orderNumber" => "order#2",
  "orderStatus" => 0,
  "actionCode" => -100,
  "actionCodeDescription" => "",
  "amount" => 1000,
  "currency" => "643",
  "date" => 1531643056391,
  "merchantOrderParams" => [],
  "attributes" => [{ "name" => "mdOrder", "value" => "aefeb658-48fb-7f37-aefe-b65804b11830" }],
  "terminalId" => "123456",
  "paymentAmountInfo" => { "paymentState" => "CREATED", "approvedAmount" => 0, "depositedAmount" => 0, "refundedAmount" => 0},
  "bankInfo" => { "bankCountryCode" => "UNKNOWN", "bankCountryName" => "<Unknown>" }
}
```

## Разработка

После клонирования репозитория, выполните `bin/setup` чтобы установить зависимости. Затем выполните `rake test`, чтобы запустить тесты. Так же можно запустить интерактивную консоль для экспериментов, выполнив `bin/console`.

## TODO

1. Сделать англоязычную версию README
2. Добавить проверку Callback-уведомлений для асимметричного ключа
3. Добавить frozen_string_literal
N. Profit!!!

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/panasyuk/sberbank-acquiring.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
