## Laravel Mpesa Package by [Akika Digital](https://akika.digital)

This Laravel package provides convenient methods for integrating [Mpesa Daraja API's](https://developer.safaricom.co.ke/APIs) functionalities into your Laravel application.

## Installation

You can install the package via composer:

```bash
composer require akika/laravel-mpesa
```

After installing the package, publish the configuration file using the following command:

```bash
php artisan mpesa:install
```

This will generate a mpesa.php file in your config directory where you can set your Mpesa credentials and other configuration options.

## .env file Setup

Add the following configurations into the .env file

```
MPESA_ENV=
MPESA_CONSUMER_KEY=
MPESA_CONSUMER_SECRET=
MPESA_PASSKEY=
MPESA_INITIATOR_NAME=
MPESA_INITIATOR_PASSWORD=
MPESA_STK_VALIDATION_URL=
MPESA_STK_CONFIRMATION_URL=
MPESA_STK_CALLBACK_URL=
MPESA_BALANCE_RESULT_URL=
MPESA_BALANCE_TIMEOUT_URL=
MPESA_TRANSACTION_STATUS_RESULT_URL=
MPESA_TRANSACTION_STATUS_TIMEOUT_URL=
MPESA_B2C_TIMEOUT_URL=
MPESA_B2C_RESULT_URL=
MPESA_B2B_TIMEOUT_URL=
MPESA_B2B_RESULT_URL=
MPESA_REVERSAL_TIMEOUT_URL=
MPESA_REVERSAL_RESULT_URL=
MPESA_BILL_OPTIN_CALLBACK_URL=
MPESA_TAX_REMITTANCE_TIMEOUT_URL=
MPESA_TAX_REMITTANCE_RESULT_URL=
```

## Usage

### Initializing Mpesa

```php
use Akika\LaravelMpesa\Mpesa;

$mpesa = new Mpesa();
```

### Fetching Token

You can fetch the token required for Mpesa API calls:

```php
$token = $mpesa->fetchToken();
```

Safaricom generates Tokens in the below format.

```bash
{
   "access_token": "c9SQxWWhmdVRlyh0zh8gZDTkubVF",
   "expires_in":"3599"
}
```

The token is valid for 3599 seconds. This Token is saved in the tokens table and can be used for 55 minutes or 3549 seconds. After that, a new token is generated. This reduces the number of calls when performing transactions.

Therefore, the response we return is just the access_token. From the above example, we return:

```bash
c9SQxWWhmdVRlyh0zh8gZDTkubVF
```

### Getting Account Balance

To use the Mpesa functionalities, you need to initialize the Mpesa class:

```php
$balance = $mpesa->getBalance();
```

### C2B Transactions

#### Registering URLs for C2B Transactions

You can register validation and confirmation URLs for C2B transactions:

```php
$response = $mpesa->c2bRegisterUrl();
```

#### Simulating C2B Transactions

You can simulate payment requests from clients:

```php
$response = $mpesa->c2bSimulate($amount, $phoneNumber, $billRefNumber, $commandID);
```

#### Initiating STK Push

You can initiate online payment on behalf of a customer:

```php
$response = $mpesa->stkPush($accountNumber, $phoneNumber, $amount, $transactionDesc);
```

#### Querying STK Push Status

You can query the result of a STK Push transaction:

```php
$response = $mpesa->stkPushStatus($checkoutRequestID);
```

#### Reversing Transactions

You can reverse a C2B M-Pesa transaction:

```php
$response = $mpesa->reverse($transactionId, $amount, $receiverShortCode, $remarks);
```

### Business to Customer (B2C) Transactions

You can perform Business to Customer transactions:

```php
$response = $mpesa->b2cTransaction($oversationId, $commandID, $msisdn, $amount, $remarks, $ocassion);
```

### Business to Business (B2B) Transactions

You can perform Business to Business transactions:

```php
$response = $mpesa->b2bPaybill($destShortcode, $amount, $remarks, $accountNumber, $requester);
```

### QR Code Generation

You can generate QR codes for making payments:

```php
$response = $mpesa->dynamicQR($merchantName, $refNo, $amount, $trxCode, $cpi, $size);
```

### Bill Manager

You can optin to the bill manager service and send invoices:

```php
$response = $mpesa->billManagerOptin($email, $phoneNumber);

$response = $mpesa->sendInvoice($reference, $billedTo, $phoneNumber, $billingPeriod, $invoiceName, $dueDate, $amount, $items);
```

### Tax Remittance

You can remit tax to the government:

```php
$response = $mpesa->taxRemittance($amount, $receiverShortCode, $accountReference, $remarks);
```

## Function Responses

All responses, except the token generation response, confirm to the responses documented on the daraja portal.

## License

The Laravel Mpesa package is open-sourced software licensed under the MIT license. See the LICENSE file for details.
