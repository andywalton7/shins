---
title: PRO Call Flows
language_tabs:
  - json: JSON
  - xml: XML
toc_footers: []
includes: []
search: false
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v3.6.6 -->

<h1 id="header">SortedPRO Call Flows</h1>

Ready to get started with SortedPRO? This guide explains some common use cases for PRO's APIs, helping you to see what PRO can do for your business. 

We will cover:

* [Creating and manifesting a consignment with the Classic flow](#classic-flow)
   
   A simple flow to create a consignment, allocate it to a carrier service using criteria of your choosing, retrieve delivery labels, and confirm the delivery with the carrier. 

* [Offering and using delivery options with the Consumer Options flow](#consumer-options-flow) 

   Used when you want to present delivery options to your customer at point of purchase. PRO creates and allocates consignments based on the options the customer selects.

* [Offering and using pickup options with the Consumer Options Pickup flow](#consumer-options-pickup-flow)

   Similar to the **Consumer Options** flow, but used when offering delivery to a pickup/drop-off (PUDO) location rather than home delivery. 

* [Creating a pack order flow from a PRO order with the Order Flex flow](#order-flex-flow)

   Used when you can't guarantee that all parts of a customer's order will be picked, packed and dispatched from the same place at the same time. PRO can generate multiple consignments from a single customer order where required.

* [Using delivery options to create a pack order flow with the Consumer Options Flex flow](#consumer-options-flex-flow)

   Used when you can't guarantee that all parts of a customer's order will be picked, packed and dispatched from the same place at the same time, and you want to present delivery options to your customer at point of purchase.

* [Obtaining and selecting delivery quotes with the Quotes flow](#quotes-flow)

   Used to obtain a full list of potential delivery services for a consignment. Often used to validate a consignment's detail or to enable a customer service operator to get quotes for a customer manually and act on the customer's response.

<aside class="note">
  This guide is intended as a primer for PRO. If you're already familiar with the basics of PRO, or you just need reference info for PRO's APIs, see the <a href="https://docs.electioapp.com/#/api">API reference</a>.

  Sample requests and responses are available in both <strong>JSON</strong> and <strong>XML</strong>. To switch between the two, use the tabs at the top of the right-hand panel.
</aside>

# Getting Started

## Authentication

> Example Authentication Header

```
ocp-apim-subscription-key: [qwerrtyuiioop0987654321]

```

You will need to provide a valid API key in every API call you make to SortedPRO. When a new user account is created, PRO generates a unique API key and allocates it to the new user. To view your API key:

1. Log in to the PRO dashboard and select **Settings > Users & Roles > [User Accounts](https://www.electioapp.com/Company/UserAccounts)** to display the **User Accounts** page. A list of the user accounts that you have access to is displayed.
2. Click the **Edit User** button for your account to display your account details.
3. Click **Show API Key**. PRO prompts you to re-enter your UI password.
4. Enter your password and click **Retrieve API Key** to display your API key.

To use your API key, include it in an `ocp-apim-subscription-key` header when making calls to PRO. If you make an API call to PRO without including an API key, then PRO returns an error with a status code of _401 (Unauthorized)_.

## Specifying Request / Response Format

> Example Content Type / Accept Headers

```json
content-type: application/json
accept: application/json
```

```xml
content-type: application/xml
accept: application/xml
```

PRO's APIs support both JSON and XML content types. PRO expects `application/json` data by default, but you can specify which content type you are sending for each API request if required. To do so, pass a `content-type` header with a value of `application/json`, `text/xml` or `application/xml` (as applicable) in your request. All other content types are invalid.

You can also specify the content type that you want PRO to use in API responses. To do so, pass an `accept` header with a value of `application/json`, `text/xml`, or `application/xml` in your request. If you don't pass an `accept` header then PRO responds with `application/json`.

## Specifying API version

> Example API Version Header

```
electio-api-version: 1.1
```

You should include an `electio-api-version` header specifying the API version to use in all PRO API calls. The current version is _1.1_.

# Classic Flow

![Flow1](source/images/Flow1.png)

> Step 1: Create Consignments endpoint
```
POST https://api.electioapp.com/consignments
```
> Step 2: Allocation endpoints
```
PUT https://api.electioapp.com/allocation/allocate
PUT https://api.electioapp.com/allocation/{consignmentReference}/allocatewithservicegroup/{mpdCarrierServiceGroupReference}
PUT https://api.electioapp.com/allocation/allocatewithcarrierservice
```
> Step 3: Get Labels in Format endpoint
```
GET https://api.electioapp.com/labels/{consignmentReference}/{labelFormat}
```
> Step 4: Manifest Consignments from Query endpoint
```
POST https://api.electioapp.com/consignments/manifestFromQuery
```
Creating a new consignment, allocating it to a suitable carrier service, and then adding it to that service's manifest is perhaps PRO's most basic use case. The **Classic** call flow offers the lightest integration design of all PRO flows, making it easy for your organisation to manage deliveries across multiple carriers.

The **Classic** flow is most useful to your business if:

* You have a single warehouse / fulfilment centre.
* You use a static delivery promise (e.g. Next day delivery before 5pm).
* You want to keep your business logic and technology architecture as simple as possible.

There are four steps to the flow:

1. **Create the consignment** - Use the [Create Consignment](https://docs.electioapp.com/#/api/CreateConsignment) endpoint to record the details of your new consignment.
2. **Allocate the consignment** - Use one of PRO's [Allocation](https://docs.electioapp.com/#/api/AllocateConsignment) endpoints to select the carrier service that your consignment will use. You can nominate a specific service, ask PRO to determine the best service to use from a pre-defined group, or allocate based on pre-set allocation rules.
3. **Get the consignment's labels** - Use the [Get Labels in Format](https://docs.electioapp.com/#/api/GetLabelsinFormat) endpoint to get the delivery label for your consignment.
4. **Manifest the consignment** - Use the [Manifest Consignments from Query](https://docs.electioapp.com/#/api/ManifestConsignmentsFromQuery) endpoint to confirm the consignment with the selected carrier. At this point, the consignment is ready to ship.

This section gives more detail on each step of the flow and provides worked examples.

## Step 1: Creating Consignments

!INCLUDE includes\_create_consignments.md

## Step 2: Allocating Consignments

!INCLUDE includes\_allocating.md

## Step 2a: Allocating using Default Rules

!INCLUDE includes\_allocate_using_default_rules.md

## Step 2b: Allocating from a Service Group

!INCLUDE includes\_allocate_with_service_group.md

## Step 2c: Allocating to a Specific Carrier Service

!INCLUDE includes\_allocate_with_carrier_service.md

## Step 3: Getting Package Labels

!INCLUDE includes\_get_labels_in_format.md

## Step 4: Manifesting a Consignment

!INCLUDE includes\_manifest_consignments_from_query.md

### Next Steps

And we're done! Read on to learn how to allocate consignments based on options presented to the customer at point of purchase, and deal with orders that may require multiple consignments to fulfil. 

# Consumer Options Flow

![Flow2](source/images/Flow2.png)

> Step 1: Delivery Options endpoint
```
POST https://api.electioapp.com/deliveryoptions
```

> Step 2: Select Option endpoint
```
POST https://api.electioapp.com/deliveryoptions/select/{deliveryOptionReference}
```

> Step 3: Get Labels in Format endpoint
```
GET https://api.electioapp.com/labels/{consignmentReference}/{labelFormat}
```

> Step 4: Manifest Consignments from Query endpoint
```
POST https://api.electioapp.com/consignments/manifestFromQuery
```

The **Consumer Options** flow enables you to provide real-time delivery choices - including delivery date, time, and carrier brand - to your customer at point of purchase. After the customer has chosen their preferred option, PRO can create a consignment based on their details, and allocate that consignment to a carrier service based on the customers choice.

The **Consumer Options** flow is most useful to your business if:

* You want to present your customer with a dynamic checkout that offers delivery timeslot options.
* You operate a single warehouse / fulfilment centre.
* You develop and configure your own e-commerce platform.

There are four steps to the flow:

1. **Get delivery options** - Use the [Delivery Options](https://docs.electioapp.com/#/api/DeliveryOptions) endpoint to request a list of available delivery options for the (as yet uncreated) consignment that the customer's order will generate.
2. **Select delivery option** - Use the [Select Option](https://docs.electioapp.com/#/api/SelectOption) endpoint to tell PRO which option the customer selected. At this point, PRO has all the information it needs to create and allocate a consignment.
3. **Get the consignment's labels** - Use the [Get Labels in Format](https://docs.electioapp.com/#/api/GetLabelsinFormat) endpoint to get the delivery label for your consignment.
4. **Manifest the consignment** - Use the [Manifest Consignments from Query](https://docs.electioapp.com/#/api/ManifestConsignmentsFromQuery) endpoint to confirm the consignment with the selected carrier. At this point, the consignment is ready to ship.

This section gives more detail on each step of the flow and provides worked examples. 

## Step 1: Getting Delivery Options

!INCLUDE includes\_getting_delivery_options.md

## Step 2: Selecting a Delivery Option

!INCLUDE includes\_select_option.md

## Step 3: Getting a Package Label

!INCLUDE includes\_get_labels_in_format.md

## Step 4: Manifesting the Consignment

!INCLUDE includes\_manifest_consignments_from_query.md

### Next Steps

The next section explains a similar call flow that enables you to offer pickup options (aka click-and-collect) rather than home delivery timeslots.

# Consumer Options Pickup Flow

![Flow3](source/images/Flow3.png)

> Step 1: Pickup Options endpoint
```
POST https://api.electioapp.com/deliveryoptions/pickupoptions/
```

> Step 2: Select Option endpoint
```
POST https://api.electioapp.com/deliveryoptions/select/{deliveryOptionReference}
```

> Step 3: Get Labels in Format endpoint
```
GET https://api.electioapp.com/labels/{consignmentReference}/{labelFormat}
```

> Step 4: Manifest Consignments from Query endpoint
```
POST https://api.electioapp.com/consignments/manifestFromQuery
```

The **Consumer Options** flow can also be used to power Pick Up / Drop-Off (PUDO) services. By integrating PRO's **Pickup Options** endpoint, you can build click-and-collect functionality that lets your customers select a pickup location and timeslot for their consignment.

There are four steps to the flow:

1. **Get pickup options** - Use the [Pickup Options](https://docs.electioapp.com/#/api/PickupOptions) endpoint to request a list of available delivery locations and timeslots for the (as yet uncreated) consignment that the customer's order will generate.
2. **Select delivery option** - Use the [Select Option](https://docs.electioapp.com/#/api/SelectOption) endpoint to tell PRO which option the customer selected. At this point, PRO has all the information it needs to create and allocate a consignment.
3. **Get the consignment's labels** - Use the [Get Labels in Format](https://docs.electioapp.com/#/api/GetLabelsinFormat) endpoint to get the delivery label for your consignment.
4. **Manifest the consignment** - Use the [Manifest Consignments from Query](https://docs.electioapp.com/#/api/ManifestConsignmentsFromQuery) endpoint to confirm the consignment with the selected carrier. At this point, the consignment is ready to ship.

This section gives more detail on each step of the flow and provides worked examples. 

## Step 1: Getting Pickup Options

!INCLUDE includes\_get_pickup_options.md

## Step 2: Selecting a Pickup Option

!INCLUDE includes\_select_option.md

## Step 3: Downloading Package Labels

!INCLUDE includes\_get_labels_in_format.md

## Step 4: Manifesting Consignments

!INCLUDE includes\_manifest_consignments_from_query.md

### Next Steps

Read on to learn how to use delivery options to fulfil multiple-consignment orders.

# Order Flex Flow

![Flow4](source/images/Flow4.png)

> Step 1: Create Order endpoint
```
POST https://api.electioapp.com/orders
```

> Step 2: Pack Order endpoint
```
POST https://api.electioapp.com/orders/{orderReference}/pack
```

> Step 3: Allocation endpoints
```
PUT https://api.electioapp.com/allocation/allocate
PUT https://api.electioapp.com/allocation/{consignmentReference}/allocatewithservicegroup/{mpdCarrierServiceGroupReference}
PUT https://api.electioapp.com/allocation/allocatewithcarrierservice
```

> Step 4: Get Labels in Format endpoint
```
GET https://api.electioapp.com/labels/{consignmentReference}/{labelFormat}
```

> Step 5: Manifest Consignments from Query endpoint
```
POST https://api.electioapp.com/consignments/manifestFromQuery
```

The **Order Flex** flow enables you to process customer orders comprising items that will ship from different physical locations.

The **Order Flex** flow is useful to your business if:

* You operate multiple warehouses / fulfilment centres, or run a customer marketplace.
* You use drop ship vending.
* You use a static delivery promise (e.g. Order by 5pm to get next day delivery).

There are five steps to the flow:

1. **Create order** - Use the [Create Order](https://docs.electioapp.com/#/api/CreateOrder) endpoint to record the customer's order in PRO.
2. **Pack order** - Use the [Pack Order](https://docs.electioapp.com/#/api/PackOrder) endpoint to create one or more consignments from the order.
3. **Allocate the consignments** - Use one of PRO's [Allocation](https://docs.electioapp.com/#/api/AllocateConsignment) endpoints to select the carrier service that your consignments will use. You can select a specific service, ask PRO to determine the best service to use from a pre-defined group, or allocate based on pre-set allocation rules.
4. **Get delivery labels** - Use the [Get Labels in Format](https://docs.electioapp.com/#/api/GetLabelsinFormat) endpoint to get the delivery label for your consignments.
5. **Manifest the consignments** - Use the [Manifest Consignments from Query](https://docs.electioapp.com/#/api/ManifestConsignmentsFromQuery) endpoint to confirm the consignments with the selected carrier. At this point, the consignments are ready to ship.

This section gives more detail on each step of the flow and provides worked examples. 

## Step 1: Creating the Order

!INCLUDE includes\_create_orders.md

## Step 2: Packing the Order

!INCLUDE includes\_pack_orders.md

## Step 3: Allocating the Consignment

!INCLUDE includes\_allocating.md

<aside class="note">
   You'll need to allocate all of the consignments packed from your order. Bear in mind that <strong><a href="https://docs.electioapp.com/#/api/AllocateUsingDefaultRules">Allocate Using Default Rules</a></strong> and <strong><a href="https://docs.electioapp.com/#/api/AllocateWithCarrierService">Allocate With Carrier Service</a></strong> enable you to allocate multiple consignments at once, but you can only allocate one consignment at a time via <strong><a href="https://docs.electioapp.com/#/api/AllocateConsignmentWithServiceGroup">Allocate Consignment With Service Group</a></strong>. If you allocate via <strong>Allocate Consignment With Service Group</strong> you'll need to make one API call per consignment on the order.
</aside>

## Step 3a: Allocating using Default Rules

!INCLUDE includes\_allocate_using_default_rules.md

## Step 3b: Allocating from a Service Group

!INCLUDE includes\_allocate_with_service_group.md

## Step 3c: Allocating to a Specific Carrier Service

!INCLUDE includes\_allocate_with_carrier_service.md

## Step 4: Getting Package Labels

!INCLUDE includes\_get_labels_in_format.md

<aside class="note">
   You'll need to make one <strong>Get Labels</strong> call per consignment on the order.
</aside>

## Step 5: Manifesting a Consignment

!INCLUDE includes\_manifest_consignments_from_query.md

<aside class="note">
   You'll need to manifest all the consignments on the order.
</aside>

### Next Steps

Finished! The next section explains a similar process, whereby the order is generated from delivery options that the customer selects rather than created up front.

# Consumer Options Flex Flow

![Flow5](source/images/Flow5.png)

> Step 1: Delivery Options endpoint
```
POST https://api.electioapp.com/deliveryoptions
```

> Step 2: Select Delivery Option as an Order endpoint
```
POST https://api.electioapp.com/deliveryoptions/selectorder
```

> Step 3: Pack Order endpoint
```
POST https://api.electioapp.com/orders/{orderReference}/pack
```

> Step 4: Allocation endpoints
```
PUT https://api.electioapp.com/allocation/allocate
PUT https://api.electioapp.com/allocation/{consignmentReference}/allocatewithservicegroup/{mpdCarrierServiceGroupReference}
PUT https://api.electioapp.com/allocation/allocatewithcarrierservice
```

> Step 5: Get Labels in Format endpoint
```
GET https://api.electioapp.com/labels/{consignmentReference}/{labelFormat}
```

> Step 6: Manifest Consignments from Query endpoint
```
POST https://api.electioapp.com/consignments/manifestFromQuery
```

Like the **Consumer Options** flow, the **Consumer Options Flex** flow enables you to provide delivery timeslots to your customer at point of purchase. However, rather than generating a single consignment from the options selected, this flow generates orders, which can then be packed into multiple consignments. 

The **Consumer Options Flex** flow can be used to provide front-end delivery options in circumstances where you cannot guarantee that the contents of your customer's online basket will map directly to a single consignment. For example, you might operate more than one warehouse and so may need to ship some orders in multiple consignments.

The **Consumer Options Flex** flow is useful to your business if:

* You use a distributed business logic and technology architecture.
* You operate multiple warehouses / fulfilment centres.
* You want to present your customer with a dynamic checkout that offers delivery timeslot options.

There are six steps to the flow:

1. **Get delivery options** - Use the [Delivery Options](https://docs.electioapp.com/#/api/DeliveryOptions) endpoint to request a list of available delivery options for the (as yet uncreated) consignment that the customer's purchase will generate.
2. **Select option as an order** -- Use the [Select Delivery Option as an Order](https://docs.electioapp.com/#/api/SelectDeliveryOptionasanOrder) to generate an order from the selected delivery option. 
3. **Pack the order** - Use the [Pack Order](https://docs.electioapp.com/#/api/PackOrder) endpoint to create one or more consignments from the order.
4. **Allocate the consignments** - Use one of PRO's [Allocation](https://docs.electioapp.com/#/api/AllocateConsignment) endpoints to select the carrier service that your consignments will use. You can select a specific service, ask PRO to determine the best service to use from a pre-defined group of services, or allocate based on pre-set allocation rules.
5. **Get the consignment's labels** - Use the [Get Labels in Format](https://docs.electioapp.com/#/api/GetLabelsinFormat) endpoint to get the delivery label for your consignment.
6. **Manifest the consignments** - Use the [Manifest Consignments from Query](https://docs.electioapp.com/#/api/ManifestConsignmentsFromQuery) endpoint to confirm the consignment with the selected carrier. At this point, the consignment is ready to ship.

This section gives more detail on each step of the flow and provides worked examples. 

## Step 1: Getting Options

!INCLUDE includes\_getting_delivery_options.md

<aside class="note">
   Although this guide focuses on generating an order from the <strong>Delivery Options</strong> endpoint, you can also generate orders from pickup options via the <strong>Pickup Options</strong> endpoint. For more information on the <strong>Pickup Options</strong> endpoint, see the <strong><a href="https://docs.electioapp.com/#/api/PickupOptions">Pickup Options</a></strong> page of the API reference.
</aside>

## Step 2: Selecting an Option as an Order

!INCLUDE includes\_select_option_as_order.md

## Step 3: Packing the Order

!INCLUDE includes\_pack_orders.md

## Step 4: Allocating the Consignment

!INCLUDE includes\_allocating.md

<aside class="note">
   You'll need to allocate all of the consignments packed from your order. Bear in mind that <strong><a href="https://docs.electioapp.com/#/api/AllocateUsingDefaultRules">Allocate Using Default Rules</a></strong> and <strong><a href="https://docs.electioapp.com/#/api/AllocateWithCarrierService">Allocate With Carrier Service</a></strong> enable you to allocate multiple consignments at once, but you can only allocate one consignment at a time via <strong><a href="https://docs.electioapp.com/#/api/AllocateConsignmentWithServiceGroup">Allocate Consignment With Service Group</a></strong>. If you allocate via <strong>Allocate Consignment With Service Group</strong> you'll need to make one API call per consignment on the order.
</aside>

## Step 4a: Allocating using Default Rules

!INCLUDE includes\_allocate_using_default_rules.md

## Step 4b: Allocating from a Service Group

!INCLUDE includes\_allocate_with_service_group.md

## Step 4c: Allocating to a Specific Carrier Service

!INCLUDE includes\_allocate_with_carrier_service.md

## Step 5: Getting Shipment Labels

!INCLUDE includes\_get_labels_in_format.md

<aside class="note">
   You'll need to make one <strong>Get Labels</strong> call per consignment on the order.
</aside>

## Step 6: Manifesting the Consignment

!INCLUDE includes\_manifest_consignments_from_query.md

<aside class="note">
   You'll need to manifest all the consignments on the order.
</aside>

### Next Steps

The final section explains how to set up a call flow that enables you to retrieve and select quotes manually.

# Quotes Flow

![Flow6](source/images/Flow6.png)

> Step 1: Create Consignments endpoint
```
POST https://api.electioapp.com/consignments
```

> Step 2: Get Quotes by Consignment Reference endpoint
```
GET https://api.electioapp.com/quotes/consignment/{consignmentReference}
```

> Step 3: Allocate With Quote endpoint
```
PUT https://api.electioapp.com/allocation/{consignmentReference}/allocatewithquote/{quoteReference}
```

> Step 4: Get Labels in Format endpoint
```
GET https://api.electioapp.com/labels/{consignmentReference}/{labelFormat}
```

> Step 5: Manifest Consignments from Query endpoint
```
POST https://api.electioapp.com/consignments/manifestFromQuery
```

The **Quotes** flow is intended as a back-end customer service integration to help you resolve delivery issues that require manual intervention. In this flow, a consignment is created and then allocated to a carrier service based on a specific quote for that consignment.

The **Quotes** flow is useful to your business for:

* Customer contact centre use. 
* In store delivery booking.
* ERP workflows (e.g. SAP, Oracle).

There are five steps to the flow:

1. **Create the consignment** - Use the [Create Consignment](https://docs.electioapp.com/#/api/CreateConsignment) endpoint to record the details of a new consignment.
2. **Get quotes for the consignment** - Use the [Get Quotes by Consignment Reference](https://docs.electioapp.com/#/api/GetQuotesbyConsignmentReference) endpoint to get delivery quotes for the consignment.
3. **Select a quote** - Use the [Allocate With Quote](https://docs.electioapp.com/#/api/AllocateWithQuote) endpoint to select one of the returned quotes.
4. **Get the consignment's labels** - Use the [Get Labels in Format](https://docs.electioapp.com/#/api/GetLabelsinFormat) endpoint to get the delivery label for your consignment.
5. **Manifest the consignment** - Use the [Manifest Consignments from Query](https://docs.electioapp.com/#/api/ManifestConsignmentsFromQuery) endpoint to confirm the consignment with the selected carrier. At this point, the consignment is ready to ship.

This section gives more detail on each step of the flow and provides worked examples. 

## Step 1: Creating a Consignment

!INCLUDE includes\_create_consignments.md

## Step 2: Getting Quotes

!INCLUDE includes\_get_quotes_by_con_ref.md

## Step 3: Selecting a Quote

!INCLUDE includes\_allocate_with_quote.md

## Step 4: Getting Labels

!INCLUDE includes\_get_labels_in_format.md

## Step 5: Manifesting a Consignment

!INCLUDE includes\_manifest_consignments_from_query.md

# More Information

## Metadata

!INCLUDE includes\_metadata.md

## Tags

!INCLUDE includes\_tags.md

## Updating Consignments

!INCLUDE includes\_update_consignments.md

## Updating Orders

!INCLUDE includes\_update_orders.md