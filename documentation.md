### **Background Documentation**

**Braintree/Venmo Documentation Excerpts:**

* In your store's Braintree configuration, you can customize the Venmo checkout button's color and width. Note, other images such as Venmo's full logo and shortened 'V' logo are included in the assets. Ensure that you follow Venmo's guidelines when making other style changes.  
* Braintree has multiple reports to reconcile your Venmo transactions including your statement... For companies located in the US, we can also provide a report for the 1099-K tax form.  
* Your site must support USD transactions, as it is the only currency supported.   
* Braintree supports processing payments in USD.  
* Just like with other payment types, you can issue voids and full or partial refunds for Venmo transactions. You can do this in the Control Panel or via the Braintree API. Venmo requires that refunds are issued within 180 days of the initial sale.  
* Shoppers must be on a device with a United States IP address \[and\] must use an address in the United States at checkout. Shoppers must have USD selected as their transaction currency.  
* If shipping physical goods in multiple shipments, you can capture the total authorized amount across several partial settlements using Transaction: Submit For Partial Settlement.  
* Business Eligibility: You can use Venmo for payments only with United States-based business entities. Support for this feature from outside of the US is not currently available.  
* You should specify how you will use the customer's Venmo account by passing `paymentMethodUsage` in the create call:  
  * `multi_use`: Request authorization for future payments (vaulting allowed)  
  * `single_use`: Request authorization for a one-time payment (vaulting not allowed)  
* If `VenmoPaymentMethodUsage` is set to `SINGLE_USE`:  
  * A validation error will be returned if attempting to vault via `PaymentMethod.create` or `Customer.create`.  
  * The transaction will be processed normally but the nonce will not be vaulted if `store-in-vault` or `store-in-vault-on-success` is passed during `transaction.sale`.  
* The payment method usage will affect the customer flow by:  
  * Displaying different phrases to the customer on the transaction consent page (e.g. 'Authorize Business Name to pay with Venmo' vs. 'Authorize Business Name to pay with Venmo for future purchases').  
  * Not displaying a merchant connection on the Connected Businesses page in the Venmo app when the `paymentMethodUsage` property is `SINGLE_USE`.  
  * Allowing customers to update their funding instrument on the Connected Businesses page in the Venmo app when the `paymentMethodUsage` property is `MULTI_USE`.  
* For Venmo, the default integration for Braintree is through a redirect to a hosted payments page. Start by creating a new transaction with the following required fields: C\#, Go, Java, PHP, Python, TypeScript.  
* Vaulting: For a seamless experience, you can store the payment method token on your servers (vaulting). This allows future transactions to occur without requiring the customer to perform an 'app switch' back to the Venmo app.  
* In the GraphQL API, this is represented by the `PaymentMethod` type, with its usage field containing a value of `SINGLE_USE`... Any calls to the vaulting mutation with such a single-use payment method will result in an error.  
* For client-side vaulting... `paymentMethodUsage` on the `BTVenmoRequest` must be set to `.multiUse`. If this property is set to `false`, you can still vault the Venmo account on your server, provided that `paymentMethodUsage` is not set to `.singleUse`.  
* While the business doesn't need an account, you will need a personal Venmo account to test the integration. This account must be logged into on a mobile device for testing purposes. If you do not have a personal account, you can create one via the Venmo website.

**Stripe Documentation Excerpts:**

* Stripe separates what you sell (Product) from how and when you charge for it (Price). A Product represents the offering itself. It's stable and changes rarely. A Price defines billing logic: amount, currency, billing interval, and charge behavior.  
* Coupons are supported. Pricing in RevenueCat Charts and customer events will reflect any coupons applied to a Stripe purchase.  
* For `past_due subscriptions`, paying the latest associated invoice transitions the subscription to active. Note that active doesn't indicate that all outstanding invoices associated with the subscription have been paid.  
* Marking an invoice as 'paid out of band' signifies that the invoice has been paid outside of Stripe.. Just as when invoices transition to a `'paid'` status because of payment in Stripe, this action is final, and the invoice cannot be altered after being marked as paid out of band.  
* If the amount due for the invoice is less than Stripe's minimum allowed charge per currency, the invoice is automatically marked paid, and we add the amount due to the customer's credit balance which is applied to the next invoice.  
* The difference is that `invoice.payment_succeeded` events are sent for successful invoice payments, but aren't sent when you mark an invoice as `paid_out_of_band. invoice.paid events`, however, are triggered for both successful payments and out of band payments.  
* In order to set a customer's subscription status back to `'active'` from an `'unpaid'` or `'past_due'` state, you will need to open their most recent invoice and successfully pay it. Paying any other invoice that is not the most recent invoice will not change the subscription's status.  
* When you set `payment_behavior to allow_incomplete`: Stripe immediately attempts to collect payment on the invoice. If the payment fails, the subscription's status changes to incomplete.  
* Updating the quantity on a subscription many times in an hour may result in rate limiting. If you need to bill for a frequently changing quantity, consider integrating usage-based billing instead.
