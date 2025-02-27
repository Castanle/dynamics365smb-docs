---
title: Synchronize and fulfill sales orders
description: Set up and run import and processing of sales orders from Shopify.
ms.date: 03/25/2024
ms.topic: article
ms.service: dynamics-365-business-central
ms.search.form: 30110, 30111, 30112, 30113, 30114, 30115, 30121, 30122, 30123, 30128, 30129, 30150, 30151, 30145, 30147
author: brentholtorf
ms.author: bholtorf
ms.reviewer: andreipa
---

# Synchronize and fulfill sales orders

This article describes the necessary settings and steps that you must complete to synchronize and fulfill sales orders with Shopify in [!INCLUDE[prod_short](../includes/prod_short.md)].

## Set the import of orders on the Shopify Shop Card

Enter a **currency code** if your online shop uses a different currency than the local currency (LCY). The specified currency must have exchange rates configured. If your online shop uses the same currency as [!INCLUDE[prod_short](../includes/prod_short.md)], leave the field empty. 

You can access the Store Currency in the [Store details](https://www.shopify.com/admin/settings/general) settings in your Shopify Admin. Shopify can be configured to accept different currencies. However, imported orders into [!INCLUDE[prod_short](../includes/prod_short.md)] use store currency.

A regular Shopify order can include costs in addition to the subtotal, such as shipping charges or, if enabled, tips. These amounts are posted directly to the G/L account you want to use for specific transaction types:

* **Shipping Charges Account**
* **Sold Gift Card Account**; learn more at [Gift Card](synchronize-orders.md#gift-cards)
* **Tip account**  

Enable **Auto Create Orders** to automatically create sales documents in [!INCLUDE[prod_short](../includes/prod_short.md)] once the Shopify order is imported.

If you want to automatically release a sales document, turn on the **Auto Release Sales Order** toggle.

If you don't want to send automatic shipping confirmations to customers, turn off the **Send Shipping Confirmation** toggle. Turning the toggle off can be useful if you sell digital goods or want to use another notification mechanism.

If you select the **Shopify Order No. on Doc. Line** field, [!INCLUDE [prod_short](../includes/prod_short.md)] inserts sales lines of the type **Comment** with the Shopify order number.

> [!NOTE]
> The sales document in [!INCLUDE[prod_short](../includes/prod_short.md)] links to the Shopify order, and you can add the **Shopify Order No.** field to the list or card pages for sales orders, invoices, and shipment. To learn more about adding a field, go to [Start personalizing by using the personalization mode](../ui-personalization-user.md#start-personalizing-by-using-the-personalization-mode). 

In the **Tax area priority** field, prioritize how to select a tax area code for addresses on orders. The Shopify order you import contains information about taxes. Taxes are recalculated when you create sales documents, so it's important that the VAT or tax settings are correct in [!INCLUDE[prod_short](../includes/prod_short.md)]. To learn more about taxes, go to [Set Up Taxes for the Shopify Connection](setup-taxes.md).

Specify how you'll process returns and refunds:

* **Blank** specifies that you don't import and process returns and refunds.
* **Import only** specifies that you import information, but you'll manually create the corresponding credit memo.
* **Auto create credit memo** specifies that you import information and [!INCLUDE[prod_short](../includes/prod_short.md)] automatically creates the credit memos. This option requires that you turn on the **Auto Create Sales Order** toggle.

Specify a location for returns, and G/L accounts for refunds for goods and other refunds.

* **Refund Account non-restock Items** specifies a G/L Account No. for items where you don't want to have an inventory correction.
* **Refund Account** specifies a G/L account for the difference in the total refunded amount and the total amount of the items.

Learn more at [Returns and refunds](synchronize-orders.md#returns-and-refunds)

### Shipment method mapping

The **Shipment method code** for sales documents imported from Shopify can be filled in automatically. You need to configure the **Shipment Method Mapping**.

1. Choose the ![Lightbulb that opens the Tell Me feature 1.](../media/ui-search/search_small.png "Tell me what you want to do") icon, enter **Shopify Shops**, then choose the related link.
2. Select the shop for which you want to define a mapping to open the **Shopify Shop Card** page.
3. Choose the **Shipment Method Mapping** action. This automatically creates records for shipping methods defined in the [**Shipping**](https://www.shopify.com/admin/settings/payments) settings in your **Shopify admin**.
4. In the **Name** field, you can see the name of the shipping method from Shopify.
5. Enter the **Shipment Method Code** with the corresponding shipping method in [!INCLUDE[prod_short](../includes/prod_short.md)].

> [!NOTE]  
> If multiple shipping charges are associated with a sales order, only one will be selected as the shipping method and assigned to the sales document.

### Location mapping

The location mapping is required to fill in the **Location Code** for sales documents lines imported from Shopify. This is important when the **Location Mandatory** toggle is enabled on the **Inventory Setup** card, otherwise, you won't be able to create sales documents.

1. Choose the ![Lightbulb that opens the Tell Me feature 1.](../media/ui-search/search_small.png "Tell me what you want to do") icon, enter **Shopify Shops**, then choose the related link.
2. Select the shop for which you want to configure the mapping of locations to open the **Shopify Shop Card** page.
3. Choose the **Locations** action to open the **Shopify Shop Locations**.
4. Choose the **Get Shopify Locations** action to import all the locations defined in Shopify. You can find them in the [**Locations**](https://www.shopify.com/admin/settings/locations) settings in your **Shopify admin** panel. 
5. Enter the **Default Location Code** with the corresponding location in [!INCLUDE[prod_short](../includes/prod_short.md)].

> [!NOTE]  
> Location mapping is also used to sync inventory. To learn more, go to [Sync inventory to Shopify](synchronize-items.md#sync-inventory-to-shopify).
  
## Run the order synchronization

The following procedure describes how to import and update the sales orders.

> [!NOTE]  
> Archived orders in Shopify can't be imported. If you need to check the order status, open the order from the [orders](https://www.shopify.com/admin/orders) page of the **Shopify admin** panel and review order details.
> 
> Deactivate the **Automatically archive the order** option in the **Order Processing** section of the **Checkout** settings in your **Shopify Admin** panel to make sure that all orders are imported to [!INCLUDE[prod_short](../includes/prod_short.md)]. If you need to import archived orders, use the **Unarchive Orders** action on the [Orders](https://www.shopify.com/admin/orders) page of the **Shopify admin** panel. 

1. Choose the ![Lightbulb that opens the Tell Me feature 1.](../media/ui-search/search_small.png "Tell me what you want to do") icon, enter **Shopify Shops**, then choose the related link.
2. Select the shop for which you want to import orders to open the **Shopify Shop Card** page.
3. Choose the **Orders** action.
4. Choose the **Sync Orders From Shopify** action.
5. Define filters on orders as necessary. For example, you can import fully paid orders or the ones with a low-risk level.

   > [!NOTE]  
   > When filtering by tag, you should use filter tokens `@` and `*`. For example, if you want to import orders containing *tag1*, use `@*tag1*`. `@` will ensure the result is case insensitive, while `*` finds orders with multiple tags.

6. Choose the **OK** button.

Alternatively, you can search for the **Sync Orders From Shopify** batch job.

You can schedule the task to be performed automatically. Learn more at [Schedule recurring tasks](background.md#to-schedule-recurring-tasks).

### Under the hood

The Shopify Connector imports orders in two steps:

1. It imports order headers to the **Shopify Orders to Import** table when they match certain conditions:
    
   * They aren't archived. This means you can include or exclude orders from sync by archiving or unarchiving them in the Shopify Admin.
   * They were created or modified after the last sync. This means you can force reimport of a specific order if you modify it—for example, by adding **Notes** or **Tag**.

2. It imports Shopify orders and supplementary information.

   * The Shopify Connector processes all records in the **Shopify Orders to Import** table that match the filter criteria you defined on the **Sync Orders from Shopify** request page. For example, tags, channel, or the fulfillment status. If you haven't specified any filters, it processes all records.
   * When importing a Shopify order, the Shopify Connector requests additional information from Shopify:

     * Order header
     * Order lines
     * Shipping and fulfillment information
     * Transactions
     * Returns and refunds, if configured

The **Shopify Order to Import** page is useful for troubleshooting order import issues. You can assess the orders that are available and take the next steps:

* Check whether an error blocked the import of a specific order, and explore the error's details. Check the **Has Error** field.
* Process only specific orders. You'll need to fill in the **Shop Code** field, select one or more orders, and then choose the **Import Selected Orders** action.
* Delete orders from the **Shopify Order to Import** page to exclude them from the sync.

## Review imported orders

Once the import is completed, you can explore the Shopify order and find all related information, such as the payment transactions, shipping costs, risk level, order attributes and tags, or fulfillments, if the order was already fulfilled in Shopify. You can also see any order confirmation that has been sent to the customer by choosing the **Shopify Status Page** action.

> [!NOTE]  
> You can navigate to the **Shopify Orders** window directly and you'll see orders with the *open* status from all shops. To review completed orders, you need to open the **Shopify Orders** page from the specific **Shopify Shop Card** page.

Before sales documents are created in [!INCLUDE[prod_short](../includes/prod_short.md)], you can use the **Synch order from Shopify** action in the **Shopify Order** page to reimport specific orders.

You can also mark an order as paid, which is useful in a B2B scenario where payments are processed outside Shopify checkout. Choose the **Mark as Paid** action on the **Shopify Order** page. Also, you can mark an order as canceled to start the refund flow in Shopify. Choose the **Cancel Order** action on the **Shopify Order** page, fill in the fields as necessary on the **Shopify Cancel Order** page, and press **OK**. You'll need to run order synchronization to import the updates to [!INCLUDE[prod_short](../includes/prod_short.md)].

## Create sales documents in Business Central

If the **Auto Create Orders** toggle is enabled on the **Shopify Shop Card**, [!INCLUDE[prod_short](../includes/prod_short.md)] tries to create a sales document after the order is imported. If issues such as a missing customer or product occur, you'll need to fix the problems and then create the sales order again.

### To create sales documents

1. Choose the ![Lightbulb that opens the Tell Me feature 1.](../media/ui-search/search_small.png "Tell me what you want to do") icon, enter **Shopify Shops**, then choose the related link.
2. Select the shop for which you want to synchronize orders to open the **Shopify Shop Card** page.
3. Choose the **Orders** action.
4. Select the order for which you want to create a sales document and choose the **Create Sales Documents** action.
5. Choose **Yes**.

If the Shopify order requires fulfillment, a **Sales Order** is created. For fulfilled Shopify orders, such as those orders that contain only a gift card or which are already handled in Shopify, a **Sales Invoice** gets created.

A sales document is created and can be managed by using standard [!INCLUDE[prod_short](../includes/prod_short.md)] functionality.

If you want to recreate sales document, you can use the **Unlink Processed Documents** action in the **Shopify Order** page. Note that this action doesn't delete the already created sales document. You must process it manually.

### Manage missing customers

If your settings prevent creating a customer automatically and a matching customer isn't found, you'll need to assign a customer to the Shopify order manually. There are a few ways to assign customers to orders:

* Assign the **Sell-to Customer No.** and **Bill-to Customer No.** directly on the **Shopify Orders** page by choosing a customer from the list of existing customers.
* Select a customer template, then create and assign the customer via the **Create new customer** action on the **Shopify Orders** page. The Shopify customer must have at least one address. Orders you create via the Shopify POS sales channel are often missing address details.
* Map an existing customer to the related **Shopify Customer** on the **Shopify Customers** page, and then choose the **Find Mapping** action on the **Shopify Orders** page.

### How the connector chooses which customer to use

The *Import order from Shopify* function tries to select customers in the following order:

1. If the **Default Customer No.** field is defined in the **Shopify Customer Template** for the **Ship-to Country/Region Code**, then the **Default Customer No.** is used, regardless of the settings in the **Customer Import From Shopify** and **Customer Mapping Type** fields. Learn more at [Customer Template per Country](synchronize-customers.md#customer-template-per-countryregion).
2. If the **Customer Import From Shopify** is set to *None* and the **Default Customer No.** is defined on the **Shopify Shop Card** page, then the **Default Customer No.** is used.

The next steps depend on the **Customer Mapping Type**.

* If it's **Always take the default customer**, then the connector uses the customer defined in the **Default Customer No.** field on the **Shopify Shop Card** page.
* If it's **By EMail/Phone**, the connector tries to find the existing customer by ID first, then by email, and then by phone number. If the customer isn't found, the connector creates a new customer.
* If it's **By Bill-to Info**, the connector tries to find the existing customer by ID first and then by the bill-to address information. If the customer isn't found, the connector creates a new customer.

> [!NOTE]  
> The connector uses information from the bill-to address and creates the bill-to customer in [!INCLUDE[prod_short](../includes/prod_short.md)]. The sell-to customer is the same as the bill-to customer.

For B2B orders flow is the similar, though connector uses **Default Company No.**, **Company Import From Shopify**, **Company Mapping Type** fields  on the **Shopify Shop Card** page. Notice that there is no **Default Company No.** in the **Shopify Customer Template** as for B2B it is expected to have named customers.

### Different processing rules for orders

You might want to process orders differently based on a rule. For example, orders from a specific sales channel, like POS, should use the default customer, but you want your online store to have real information about the customer.

One way to address this requirement is to create an additional Shopify Shop card and use filters in the **Sync Orders from Shopify** request page.

Example: You have an online store as well as a Shopify POS. For your POS, you want to use a fixed customer, but for your online store you want to create customers in [!INCLUDE[prod_short](../includes/prod_short.md)]. The following procedure lists the high-level steps. To learn more, go to the corresponding help articles.

1. Create a Shopify shop called *STORE* and link it to your Shopify account.
1. Configure item/product synchronization so that this store manages product information.
1. Specify that customers are imported with orders. The connector should find customers by looking for their email address. If it doesn't find an address, it uses the customer template to create a new customer.
1. Create a Shopify shop called *POS* and link it to same Shopify account.
1. Make sure that item/product synchronization is disabled.
1. Select the connector that uses the default customer.
1. Create a recurring job queue entry for Report 30104 **Sync orders from Shopify**. Select **STORE** in the **Shopify Shop Code** field, and use filters to catch all orders except those that the POS sales channel creates. For example, **<>Point of Sale**
1. Create a recurring job queue entry for the Report 30104 **Sync orders from Shopify**. Select **POS** in the **Shopify Shop Code** field, and use filters to catch orders generated by POS sales channel. For example, **Point of Sale**.

Each job queue will import and process orders within the defined filters and use the rules from the corresponding Shopify Shop card. For example, they'll create point of sales orders for the default customer.

>[!Important]
> To avoid conflicts when processing orders, use the same job queue category for both job queue entries.

### Impact of order editing

In Shopify:

|Edit|Impact on Shopify Orders yet not processed in [!INCLUDE[prod_short](../includes/prod_short.md)] | Impact on Shopify Orders yet already processed in [!INCLUDE[prod_short](../includes/prod_short.md)] |
|------|-----------|-----------|
|Change the fulfillment location | Fulfillment location is synched to [!INCLUDE[prod_short](../includes/prod_short.md)]. | Fulfillment location is synched to [!INCLUDE[prod_short](../includes/prod_short.md)].|
|Edit an order and increase quantity|Imported order will use new quantity.| Connector will detect change and mark orders. |
|Edit an order and decrease quantity|Imported order will use new quantity. Shopify refund with 0 amount will be imported that can not be converted into Credit Memo.| Connector will detect change and mark orders. |
|Edit an order and remove existing item |Removed item won't be imported. Shopify refund with 0 amount will be imported that can not be converted into Credit Memo.| Connector will detect change and mark orders. |
|Edit an order and add new item | Original and added items will be imported. | Connector will detect change and mark orders. |
|Process order: fulfill, update payment information | Order header will be updated. |Order header will be updated. The fulfillment won't be synchronized with Shopify.|
|Cancel paid order | Order header will be updated, to be processed separatelly |Connector will detect change and mark orders. |
|Cancel unpaid order | Removed item won't be imported. Shopify refund with 0 amount will be imported that can not be converted into Credit Memo. |Connector will detect change and mark orders. |

In case order was already processed in [!INCLUDE[prod_short](../includes/prod_short.md)] connector will display the following error message: *The order has already been processed in Business Central, but an edition was received from Shopify. Changes were not propagated to the processed order in Business Central. Update the processed documents to match the received data from Shopify. If you wish to force the synchronization use the action "Sync order from Shopify" in the Shopify Order card page.*

Depending on status of created sales document you can perform following actions:
1. Delete created sales document
2. Choose the **Unlink Processed Documents** action to reset the **Processed** indicator.
3. Choose the **Synch order from Shopify** action to update individual order with recent data from Shopify.

In [!INCLUDE[prod_short](../includes/prod_short.md)]:

|Edit|Impact|
|------|-----------|
|Change the location to another location. Post shipment. | Order will be marked as fulfilled. Fulfillment location from Shopify will be used. |
|Decrease quantity. Post shipment. | The Shopify order will be marked as partially fulfilled. |
|Increase quantity. Post shipment. | The fulfillment won't be synchronized with Shopify. Same if fulfilment was split in Shopify but processed as one line in [!INCLUDE[prod_short](../includes/prod_short.md)]. |
|Add a new item. Post shipment. | The Shopify order will be marked as fulfilled. New lines won't be added. |

## Synchronize shipments to Shopify

When a sales order created from a Shopify order is shipped, you can synchronize the shipments with Shopify.

1. Choose the ![Lightbulb that opens the Tell Me feature 1.](../media/ui-search/search_small.png "Tell me what you want to do") icon, enter **Sync Shipments to Shopify**, then choose the related link.
2. Define the filters on shipments as necessary. For example, you can update a shipment posted on a specific date.
3. Choose the **OK** button.

The order in Shopify will be marked as fulfilled. The customer automatically receives a shipment notice email or text message (SMS). If a shipping agent and a tracking code are specified on the shipment, the tracking information is included in the email.

Alternatively, use the **Sync Shipments** action in the Shopify Sales Orders or Shopify Shop pages.

You can schedule the task to be performed in an automated manner. Learn more at [Schedule recurring tasks](background.md#to-schedule-recurring-tasks).

>[!Important]
>The location, including blank location, defined in the Posted Shipment Line must have a matching record in the Shopify Location. Otherwise, this line won't be sent back to Shopify. Learn more at [Location mapping](synchronize-orders.md#location-mapping).

Remember to run **Synchronize Orders from Shopify** to update the fulfillment status of an order in [!INCLUDE[prod_short](../includes/prod_short.md)]. The connector functionality also archives completely paid and fulfilled orders in both Shopify and [!INCLUDE[prod_short](../includes/prod_short.md)], provided the conditions are met. 

### Shipping agents and tracking URL

If the **Posted Sales Shipment** document contains the **Shipping Agent Code** and/or **Package Tracking No.**, this information will be sent to Shopify and to the customer in the shipping confirmation email.

The tracking company is populated in the following order (from highest to lowest) based on the shipping agent record:

* **Shopify Tracking Company**
* **Name**
* **Code**

If the **Package Tracking URL** field is filled in for the shipping agent record, then the shipping confirmation will contain a tracking URL as well.

## Returns and refunds

In an integration between Shopify and [!INCLUDE[prod_short](../includes/prod_short.md)], it's important to be able to synchronize as much business data as possible. That makes it easier to keep your finance and inventory levels up to date in [!INCLUDE[prod_short](../includes/prod_short.md)]. The data you can synchronize includes returns and refunds that were recorded in Shopify Admin or Shopify POS.

Returns and refunds are imported with their related orders if you enabled the processing type on the Shopify Shop Card.

Returns are imported for informational purposes only. There is no processing logic associated with them.

Financial and, if needed, inventory transactions are processed via refunds. Refunds can include products or just amounts—for example, if a merchant decides to compensate shipping charges or some other amount.

You can create sales credit memos for refunds. The credit memos can have the following types of lines:

|Type|No.|Comment|
|-|-|-|
|G/L Account|Sold Gift Card Account| Use for refunds related to gift cards.|
|G/L Account|Refund Account Non-stock | Use for refunds related to products that weren’t restocked. |
|Item |Item No.| Use for refunds related to products that were restocked. Valid for direct refunds or refunds linked to refunds. The location code on the credit more line is set based on the value selected for the return location.|
|G/L Account| Refund Account | Use for other refunded amounts that aren't related to products or gift cards. For example, tips, or if you manually specified an amount to refund in Shopify. |

>[!Note]
>The return locations, including blank locations, defined in the **Shopify Shop Card** are used on the created credit memo. The system ignores the original locations from orders or shipments.

## Gift cards

In the Shopify shop you can sell gift cards, which can be used to pay for real products.

When dealing with gift cards, it's important to enter a value in the **Sold Gift Card Account** field in the **Shopify Shop Card** window. The sold gift card will be synchronized together with orders in line. An applied gift card will also be imported with the order, but now as a transaction. Notice that the gift card doesn't reduce the amount to invoice.

To review the issued and applied gift cards, choose the ![Lightbulb that opens the Tell Me feature.](../media/ui-search/search_small.png "Tell me what you want to do") icon, enter **Gift Cards**, then choose the related link.

## See also

[Get Started with the Connector for Shopify](get-started.md)  
