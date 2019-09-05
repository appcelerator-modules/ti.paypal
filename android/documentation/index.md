# Ti.Paypal Module

## Description

The Ti.Paypal module gives you access to payments via paypal for goods and services.
Note that Apple does not accept payment through any means other than the in-app
purchasing store for extensions and subscriptions for your app.

## Getting Started

View the [Using Titanium Modules](http://docs.appcelerator.com/platform/latest/#!/guide/Using_Titanium_Modules) document for instructions on getting
started with using this module in your application.

## Accessing the Ti.Paypal Module

To access this module from JavaScript, you would do the following:

	var Paypal = require("ti.paypal");

## Functions

### Ti.Paypal.createPaypalButton({...})

Creates and returns a [Ti.Paypal.PaypalButton][] object, which is a view.  Note that the view's
size determines the amount of space on the screen it takes up, _not_ the size of
the Paypal button, which is determined by a constant.

WARNING: A paypal button may only be used once! It is designed to be created for one transaction. If the user cancels
the transaction, or goes through with it, you must recreate the paypal button before allowing them to touch it again.

Takes one argument, a dictionary containing the following properties:

* appID[string]: The appID issued by Paypal for your application
* language[string]: The language for the UI. Default value: "en_US"
* textStyle[int]: The text for the button; can be _Ti.Paypal.PAYPAL\_TEXT\_PAY_ or _Ti.Paypal.PAYPAL\_TEXT\_DONATE_, defaults to _Ti.Paypal.PAYPAL\_TEXT\_PAY_.
* buttonStyle[int]: A constant indicating the style & size of the button. If the textStyle property is set to _Ti.Paypal.PAYPAL\_TEXT\_DONATE_, the word "Pay" in the buttons is replaced by "Donate". The language of the button will also change based on the language you pass into the "language" property or the auto detected language on the phone.
* paypalEnvironment[int]: A constant indicating the environment the button runs in
* feePaidByReceiver[boolean]: Whether or not the Paypal fees are paid by the receiver; if false, any fees are paid by the sender. Defaults to false.
* enableShipping[boolean]: Whether or not to select/include shipping information. Defaults to true.
* cancelUrl[string]
* returnUrl[string]

To submit a simple payment, you should also set the following property:

* payment[dictionary]: A dictionary describing the parameters of the payment. Properties are:
	* paymentType[int]: A constant indicating the type of payment made. Default: PAYPAL\_TYPE\_PERSONAL.
	* paymentSubtype[int]: The payment subtype for a "SERVICES" type payment (see enumerated values section).  Applicable only if you have been approved for special pricing plans. For any transactionType other than  "TYPE\_SERVICE" or if you have not been approved for special pricing plans use "SUBTYPE\_NOT\_SET" (the default value) as the transactionSubType. Default: SUBTYPE\_NOT\_SET.
	* subtotal[double]: The amount of the payment, not including tax and shipping amounts
	* tax[double]: Tax on the payment  
	* shipping[double]: Shipping cost for the transaction (only for HARD_GOODS)  
	* currency[string]: An identifier for the currency the transaction uses; defaults to "USD".
	* recipient[string]: The Paypal user who receives the payment.  
	* customID[string]: A custom ID for the whole order; this is any ID that you would like to have associated with the payment.
	* ipnUrl[string]: Your Instant Payment Notification URL. This url will be hit by the Paypal server upon completion of the payment.
	* merchantName[string]: The name of the merchant the recipient represents.
	* memo[string]: A memo to include with the order. This memo will be part of the notification sent by Paypal to the necessary parties.
	* invoiceItems[dictionary[]]: An array of dictionaries describing the individual items in this payment:
		* name[string]: The name of the item.
		* itemID[string]: The ID of the item.
		* totalPrice[double]: The total price for this item.
		* itemPrice[double]: The price for one of this item.
		* itemCount[int]: The total number of this item included in the transaction.

Or, to submit an advanced payment (such as a chained or parallel payment), set the following property. Note that the only programmatic difference between chained and parallel payments is the "isPrimary" property set on one of your advanced payments.

* advancedPayment[dictionary]: A dictionary describing the parameters of the advanced payment. Properties are:
	* currency[string]: An identifier for the currency the transaction uses; defaults to "USD".
	* ipnUrl[string]: Your Instant Payment Notification URL. This url will be hit by the Paypal server upon completion of the payment.
	* memo[string]: A memo to include with the order. This memo will be part of the notification sent by Paypal to the necessary parties.
	* payments[dictionary[]]: An array of dictionaries describing the individual payments involved:
		* isPrimary[bool]: Whether or not this is the primary payment. If one payment has this, this is a chained payment. If none, then it is a parallel payment.
		* paymentType[int]: A constant indicating the type of payment made. Default: PAYPAL\_TYPE\_PERSONAL.
		* paymentSubtype[int]: The payment subtype for a "SERVICES" type payment (see enumerated values section).  Applicable only if you have been approved for special pricing plans. For any transactionType other than  "TYPE\_SERVICE" or if you have not been approved for special pricing plans use "SUBTYPE\_NOT\_SET" (the default value) as the paymentSubtype. Default: SUBTYPE\_NOT\_SET.
		* subtotal[double]: The amount of the payment, not including tax and shipping amounts
		* tax[double]: Tax on the payment  
		* shipping[double]: Shipping cost for the transaction (only for HARD_GOODS)  
		* recipient[string]: The Paypal user who receives the payment.  
		* customID[string]: A custom ID for the whole order; this is any ID that you would like to have associated with the payment.
		* merchantName[string]: The name of the merchant the recipient represents.
		* invoiceItems[dictionary[]]: An array of dictionaries describing the individual items in this payment:
			* name[string]: The name of the item.
			* itemID[string]: The ID of the item.
			* totalPrice[double]: The total price for this item.
			* itemPrice[double]: The price for one of this item.
			* itemCount[int]: The total number of this item included in the transaction.

Or, to submit a Preapproval for payment, set the following properties.

* preapprovalKey[string]: The preapproval key you received from PayPal. Please reference PayPal's documentation for more information on how preapproval works, and what steps are necessary to get this key.
* preapproval[dictionary]: A dictionary describing the parameters of the preapproval. Properties are:
	* type[int]: A constant indicating the type of preapproval agreement to make. Default: PREAPPROVAL\_TYPE\_AGREE\_AND\_PAY.
	* currency[string]: An identifier for the currency the transaction uses; defaults to "USD".
	* dayOfMonth[int]: The day of the month.
	* dayOfWeek[int]: Use the "Preapproval Days" constants below.
	* isApproved[bool]
	* paymentPeriod[int]: Use the "Preapproval Period" constants below.
	* pinRequired[bool]
	* maxAmountPerPayment[double]
	* maxNumberOfPayments[int]
	* maxNumberOfPaymentsPerPeriod[int]
	* maxTotalAmountOfAllPayments[double]
	* merchantName[string]: The name of the merchant the recipient represents.
	* ipnUrl[string]: Your Instant Payment Notification URL. This url will be hit by the Paypal server upon completion of the payment.
	* memo[string]: A memo to include with the order. This memo will be part of the notification sent by Paypal to the necessary parties.

__NOTE__: Once set on a button, payment information CANNOT be changed.


## Properties

## Properties - Text Types for Paypal Button

### Ti.Paypal.PAYPAL\_TEXT\_PAY[int] (read-only)
A constant for a button's "textStyle" property causing "Pay" to be displayed.

### Ti.Paypal.PAYPAL\_TEXT\_DONATE[int] (read-only)
A constant for a button's "textStyle" property causing "Donate" to be displayed.

## Properties - Payment Types

### Ti.Paypal.PAYMENT\_TYPE\_GOODS[int] (read-only)
A constant for a button's "transactionType" property which denotes the sale of physical goods. This is the default payment type.

### Ti.Paypal.PAYMENT\_TYPE\_SERVICE[int] (read-only)
A constant for a button's "transactionType" property which denotes the sale of a service.

### Ti.Paypal.PAYMENT\_TYPE\_PERSONAL[int] (read-only)
A constant for a button's "transactionType" property which denotes a personal payment.

### Ti.Paypal.PAYMENT\_TYPE\_NONE[int] (read-only)
A constant for a button's "transactionType" property which denotes a payment where no money is exchanged.

## Properties - Preapproval Types

### Ti.Paypal.PREAPPROVAL\_TYPE\_AGREE\_AND\_PAY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_TYPE\_AGREE[int] (read-only)

## Properties - Preapproval Days

### Ti.Paypal.PREAPPROVAL\_DAY\_SUNDAY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_DAY\_MONDAY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_DAY\_TUESDAY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_DAY\_WEDNESDAY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_DAY\_THURSDAY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_DAY\_FRIDAY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_DAY\_SATURDAY[int] (read-only)
                                                         
## Properties - Preapproval Periods

### Ti.Paypal.PREAPPROVAL\_PERIOD\_ANNUALLY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_PERIOD\_BIWEEKLY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_PERIOD\_DAILY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_PERIOD\_MONTHLY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_PERIOD\_NONE[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_PERIOD\_SEMIMONTHLY[int] (read-only)
### Ti.Paypal.PREAPPROVAL\_PERIOD\_WEEKLY[int] (read-only)

## Properties - Button Sizes

### Ti.Paypal.BUTTON\_152x33[int] (read-only)
A paypal button sized for a 152x33 pixel view.

### Ti.Paypal.BUTTON\_194x37[int] (read-only)
A paypal button sized for a 194x37 pixel view.

### Ti.Paypal.BUTTON\_278x43[int] (read-only)
A paypal button sized for a 278x43 pixel view.

### Ti.Paypal.BUTTON\_294x45[int] (read-only)
A paypal button sized for a 294x45 pixel view.

## Properties - Environments

### Ti.Paypal.PAYPAL\_ENV_LIVE[int] (read-only)
Sets paypal to use the production servers for processing payments.
Payments are actual transactions.

### Ti.Paypal.PAYPAL\_ENV_SANDBOX[int] (read-only)
Sets paypal to use the testing servers for processing payments.
Payments use testing funds in the development sandbox.

### Ti.Paypal.PAYPAL\_ENV_NONE[int] (read-only)
Sets paypal to use internal demonstration data for payments and server operations.

## Properties - Payment Subtypes
The payment subtype for a "SERVICES" type payment (see enumerated values section).  Applicable only if you have been
approved for special pricing plans. For any transactionType other than  "TYPE\_SERVICE" or if you have not been approved
for special pricing plans use "SUBTYPE\_NOT\_SET" (the default value) as the transactionSubType.

### Ti.Paypal.PAYMENT\_SUBTYPE\_NONE [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_AFFILIATE [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_B2B [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_PAYROLL [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_REBATES [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_REFUNDS [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_REIMBURSEMENTS [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_DONATIONS [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_UTILITIES [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_TUITION [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_GOVERNMENT [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_INSURANCE [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_REMITTANCES [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_RENT [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_MORTGAGE [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_MEDICAL [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_CHILDCARE [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_EVENTS [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_CONTRACTORS [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_ENTERTAINMENT [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_TOURISM [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_INVOICE [int] (read-only)
### Ti.Paypal.PAYMENT\_SUBTYPE\_TRANSFER [int] (read-only)


## Events
For legacy support, this module fires all of the events from created [Ti.Paypal.PaypalButton][]. Adding event listeners
here is deprecated, and will be removed in a future version of the module.

## Usage
See example.

## Author

Stephen Tramer

## Module History

View the [change log](changelog.html) for this module.

## Feedback and Support

Please direct all questions, feedback, and concerns to [info@appcelerator.com](mailto:info@appcelerator.com?subject=Android%20PayPal%20Module).

## License
Copyright(c) 2010-2013 by Appcelerator, Inc. All Rights Reserved. Please see the LICENSE file included in the distribution for further details.

[Ti.Paypal.PaypalButton]: paypalButton.html
