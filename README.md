# Shopify Order Automation using Pipedream

## Overview

This project automates the processing of Shopify orders using Pipedream.

The workflow starts whenever a new order webhook is received. It validates the incoming order based on a few business conditions and, if all conditions are satisfied, sends two automated emails to the customer.

The project was tested using a sample payload that follows the structure of a Shopify `orders/create` webhook. In a production environment, the same workflow can be connected directly to Shopify.

---

## Workflow

The automation performs the following steps:

1. Receives a new order through an HTTP webhook.
2. Checks if:
   - The order contains the tag **MakeOrder**
   - The customer contains the tag **ColdCustomer**
   - The order amount is greater than **₹2500**
3. If all conditions are met:
   - Sends a welcome email immediately.
   - Waits for 5 minutes.
   - Sends a second email informing the customer about a newly unlocked discount.
4. If any of the conditions fail, the workflow stops without sending any emails.

---

## Workflow Architecture


HTTP Trigger
      │
      ▼
Filter
      │
      ▼
Send Welcome Email
      │
      ▼
Delay (5 Minutes)
      │
      ▼
Send Discount Email

---

## Technologies Used

- Pipedream
- HTTP Webhooks
- Shopify Webhook Payload
- Gmail Integration
- Flow Control (Filter)

---

## Sample Webhook Payload

json
{
  "tags": "MakeOrder",
  "total_price": 3000,
  "customer": {
    "tags": "ColdCustomer",
    "email": "example@gmail.com"
  }
}


---

## Testing

The workflow was tested by sending sample HTTP POST requests to the Pipedream webhook endpoint.

### Successful Test

Conditions:

- Order Tag = `MakeOrder`
- Customer Tag = `ColdCustomer`
- Order Amount > ₹2500

Expected Result:

- Welcome email sent immediately.
- Discount email sent after a 5-minute delay.

### Failure Tests

The workflow correctly stops when:

- Order tag is different.
- Customer tag is different.
- Order amount is less than or equal to ₹2500.

---

## Screenshots

The `screenshots` folder contains:

- Workflow overview
- Successful workflow execution
- Welcome email
- Discount email

---

## Notes

- The workflow was tested using a sample payload that mirrors the Shopify `orders/create` webhook structure.
- In a real-world setup, Shopify can be configured to send the webhook directly to the Pipedream endpoint, and no changes to the workflow are required.
- The workflow is modular and can be extended with additional validations or actions based on business requirements.
