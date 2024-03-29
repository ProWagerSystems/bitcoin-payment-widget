# bitcoin-payment-widget
Accept cryptocoin payments on any website with a few lines of code. Cross-browser and mobile-friendly with no dependencies. Powered by BitcoinOfAmerica.org

## Quick Start

Insert the following code into your webpage's HTML right before the closing <body> tag:
```
<script>
   bcoa_options = {
      merchantid: "MERCHANTID",
      company: "Your Website",
      confirmations: 3,
      notifyurl: "",
      complete: function() { },
      confirmation: function(n) { },
      cancel: function() { },
   }
</script>   
<link rel="stylesheet" href="https://www.bitcoinofamerica.org/widget/bcoa-widget.css">
<script src="https://www.bitcoinofamerica.org/widget/"></script>
```
  
Then use the following code to create a payment button that opens a widget. Multiple buttons can be used:

```
<button 
    data-coin-type="BTC" 
    data-price-cents="199" 
    data-price-crypto="0"
    data-description="Product Name"
    data-product-id="123"
    data-invoice-id="456"
    data-customer-id="789"
>Pay Now!</button>
```

## Configurable Options

- **merchantid** - Your BitcoinOfAmerica merchant identifier. Can be obtained by signing up for free as a merchant at [bitcoinOfAmerica](https://www.bitcoinofamerica.org)
- **company** - Your store or website name for display in the widget.
- **confirmations** - The number of blockchain confirmations required before widget displays an "order complete" message, and calls the optional *complete* callback. 
- **notifyurl** - This optional URL on your server will be notified each time a confirmation is received. Only the the first 6 payment confirmations will be handled {see **Notification URL** below}.
- **complete** - Callback function. Called when all required confirmations have been received for payment.
- **confirmation** - Callback function. Passes number of confirmations received so far.
- **cancel** - Callback function. Called if customer closes the widget after receiving a payment receiving address.
- **LANGUAGE TRANSLATIONS** - See **Language Translation** below for additional options

## Button Options

- **data-coin-type** - Coin type for payment. Must be one of BTC, LTC, BCH, ECH, or XRP.
- **date-price-cents** - The price of the product or service that you're selling, in pennies. Set to 0 if your price is in satoshi units (crypto) instead of fiat.
- **data-price-crypto** - The price of the product or service that you're selling in crypto units {see **Coin Specific Units** below}.  If your price is in satoshi units (crypto) instead of fiat, set this to 0.
- **data-description** - The product or service your customer is buying. This is displayed to your customer in the widget. 
- **data-product-id** - Optional pass-thru value.
- **data-invoice-id** - Optional pass-thru value.
- **data-customer-id** - Optional pass-thru value.


## Coin Specific Units

```
1 BTC (Bitcoin)      = 100000000 (1e8) satoshi
1 LTC (Litecoin)     = 100000000 (1e8) photons
1 BCH (Bitcoin Cash) = 100000000 (1x8) satoshi
1 ETH (Ethereum)     = 1000000000000000000 wei
1 XRP (Ripple)       = 1000000 (1e6) drops
```

## Notification URL

If a *notification URL* is provided, a confirmation receipt will be sent for each of the first 6 confirmations.

The data in every POST will include:

- **merchantid** - Your BitcoinOfAmrica merchant id. 
- **confirmations** - Number of confirmations. Should contain a value from 1 to 6
- **cointype** - Possible values are BTC, LTC, BCH, ETH, XRP
- **completed** - Will be 0 until the desired number of confirmations have been received, according to the *confirmations* specified in your widget's Configurable Options.
- **amount** - Value of purchase in coin specific units
- **epochtime** - Timestamp. The number of milliseconds since 1970
- **hashed** - POST data in encrypted format for validation. See *Notificication Verification* below.

The data may also include the following optional pass-thru values from your button:

- **data-product-id** 
- **data-invoice-id** 
- **data-customer-id**

## Notification Verification

To authenticate a payment notification, concatenate all POSTed data values except *hashed* and encrypt with HMAC/Sha256 uisng your Bitcionofamerica Secret Key. A match confirms the integrity and authenticity of the notification.

```
PHP Example:

$hash = hash_hmac("sha512", $_POST["merchantid"].$_POST["confirmations"].$_POST["cointype"].$_POST["completed"].$_POST["amount"].$_POST["epochtime"].$_POST["data-product-id"].$_POST["data-invoice-id"].$_POST["data-customer-id"], $YOUR_SECRET_KEY);
if( $hashed == $_POST["hashed"] ) ; // Validated!

```
               
## Langauge Translation

The default phrases used by the widget can be changed using the following bcoa_options properties: 

- **coaLangAreYouSure:** "Are you sure you want to close this window while your payment is being processed?"
- **bcoaLangError:** "Error processing order. Please contact site owner."
- **bcoaLangConfirmations:** "Confirmations ..."
- **bcoaLangComplete:** "&#10004;&nbsp;Payment Complete"
- **bcoaLangPay:** "Pay"
- **bcoaLangIn:** "In"
	
