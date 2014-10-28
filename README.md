# in-app-purchase

©Nobuyori Takahashi < <voltrue2@yahoo.com> >

A Node.js module for In-App-Purchase validation for iOS and Android.

### Methods

####.validate(receipt [string or object], callback [function])

Validates an in-app-purchase receipt.

For Apple validation, receipt is a string.

For Google validation, receipt is an object { data: xxx, signature: yyyy }

####.isValidated(response [object])

Returns a boolean.

### GooglePlay Public Key Files

For GooglePlay, in-app-purchase module needs to have the public key file(s).

The module requires the file(s) to be name in a certain way:

For sandbox, the file name should be: `iap-sandbox`.

For production, the file name should be: `iap-live`.

### Public Key File(s)

Google Play has only one public key for both production and sandbox, but the module gives you an option to separate the public keys for development and testing.

If you do not need to have different public keys, simply use the same public key in both files.

### Configurations

The module needs to call `.config()` before it can execute `.setup()` correctly.

Example:

```
var inAppPurchase = require('in-app-purchase');
inAppPurchase.config({
	googlePublicKeyPath: "path/to/public/key/directory/" // this is the path to the directory containing iap-sanbox/iap-live files
});
```

### How To Use It

Example: Apple

```javascript
var iap = require('in-app-purchase');
/*
For google iap, you need to name your public key file as:
iap-sandbox or iap-live
*/
iap.config({
	googlePublicKeyPath: "/path/to/google/public/key/dir/"
});
iap.setup(function (error) {
	if (error) {
		return console.error('something went wrong...');
	}
	// iap is ready
	iap.validate(iap.APPLE, appleReceipt, function (err, appleRes) {
		if (err) {
			return console.error(err);
		}
		if (iap.isValidated(appRes)) {
			// yay good!
		}
	});
});
```

Example: Google

```javascript
var iap = require('in-app-purchase');
/*
For google iap, you need to name your public key file as:
iap-sanbox or iap-live
*/
iap.config({
	googlePublicKeyPath: "/path/to/google/public/key/dir/"
});
iap.setup(function (error) {
	if (error) {
		return console.error('something went wrong...');
	}
	/*
		google receipt must be provided as an object
		{
			"data": "{stringified data object}",
			"signature": "signature from google"
		}
	*/
	// iap is ready
	iap.validate(iap.GOOGLE, googleReceipt, function (err, googleRes) {
		if (err) {
			return console.error(err);
		}
		if (iap.isValidated(googleRes)) {
			// yay good!
		}
	});
});
```

## Test

`in-app-purchase` module provides unit tests. In order the make use of the unit tests, you must provide receipts for iOS/Android in files.

To test the module you may execute the following commands in the root directory of the module:

For iOS:

```
make test-apple path=/path/to/your/apple/receipt/file
```

For Android:

In order to test google's in-app-billing, you must provide the public key file in addition to your google receipt file.

NOTE: the receipt file for google must contain javascript object format something similar to:

```
{"data":"google sent data","signature":"signature string"}
```

```
make test-google path=/path/to/your/google/receipt/file pk=/path/you/the/directory/of/your/google/public/key/
```