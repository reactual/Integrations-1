# @datafire/ebay_buy_feed

Client library for Item Feed Service

## Installation and Usage
```bash
npm install --save @datafire/ebay_buy_feed
```
```js
let ebay_buy_feed = require('@datafire/ebay_buy_feed').create({
  access_token: "",
  refresh_token: "",
  client_id: "",
  client_secret: "",
  redirect_uri: ""
});

ebay_buy_feed.getItemFeed({
  "X-EBAY-C-MARKETPLACE-ID": "",
  "Range": "",
  "feed_scope": "",
  "category_id": "",
  "date": ""
}).then(data => {
  console.log(data);
});
```

## Description

The Feed API provides the ability to download daily TSV_GZIP feed files containing all the newly listed eBay items in a specific category and date.

## Actions

### oauthCallback
Exchange the code passed to your redirect URI for an access_token


```js
ebay_buy_feed.oauthCallback({
  "code": ""
}, context)
```

#### Input
* input `object`
  * code **required** `string`

#### Output
* output `object`
  * access_token `string`
  * refresh_token `string`
  * token_type `string`
  * scope `string`
  * expiration `string`

### oauthRefresh
Exchange a refresh_token for an access_token


```js
ebay_buy_feed.oauthRefresh(null, context)
```

#### Input
*This action has no parameters*

#### Output
* output `object`
  * access_token `string`
  * refresh_token `string`
  * token_type `string`
  * scope `string`
  * expiration `string`

### getItemFeed
The Item feed file is generated each day. This call lets you download a daily TSV_GZIP (tab separated value gzip) Item feed file of all the items that were listed on a specific day in a specific category. For each item, all the item details are returned, except for the item description. The description of each item is excluded because these can be huge and items in an item group (an item with variations, such as size and color) can share the same description. The item descriptions are returned in a separate Descriptions gzip feed file by the getItemDescriptionFeed call. To store the complete item details, you would always run the getItemFeed and getItemDescriptionFeed calls with the same parameters. &nbsp;&nbsp;&nbsp; /item?feed_scope=NEWLY_LISTED&amp;category_id=625&amp;date=20170918 &nbsp;&nbsp;&nbsp; /item_description?feed_scope=NEWLY_LISTED&amp;category_id=625&amp;date=20170918 Items returned in the feed file All items in the file are: In new condition Fixed price (Buy It Now); no auctions From eBay trusted sellers The feed file contains all the items from all the child categories of the specified category. The first line of the file is the header, which indicates the order of the values on each line. Each header is described in the Response fields section on this page. Downloading feed files Item feed files are binary gzip files. If the file is larger than 100 MB, the download must be streamed in chunks. You specify the size of the chunks in bytes using the Range request header. The Content-range response header indicates where in the full resource this partial chunk of data belongs and the total number of bytes in the file. For more information about using these headers, see Retrieving a gzip feed file. Important: The response is always a TSV_GZIP file. But for documentation purposes, the response is shown as JSON fields so that the value returned in each column can be explained. The order of the response fields, shows you the order of the columns in the feed file. Restrictions For a list of supported sites and other restrictions, see API Restrictions.


```js
ebay_buy_feed.getItemFeed({
  "X-EBAY-C-MARKETPLACE-ID": "",
  "Range": "",
  "feed_scope": "",
  "category_id": "",
  "date": ""
}, context)
```

#### Input
* input `object`
  * X-EBAY-C-MARKETPLACE-ID **required** `string`: The ID for the eBay marketplace where the items are hosted. For example: &nbsp;&nbsp;X-EBAY-C-MARKETPLACE-ID = EBAY-US For a list of supported sites see, API Restrictions.
  * Range **required** `string`: This header specifies the range in bytes of the chunks of the gzip file being returned. Format: bytes=startpos-endpos For example, the following retrieves the first 10 MBs of the feed file. &nbsp;&nbsp;Range bytes=0-10485760 For more information about using this headers, see Retrieving a gzip feed file. Maximum: 100 MB
  * feed_scope **required** `string`: Controls the contents of the feed file. Valid Value: NEWLY_LISTED - Contains all items that were listed on the day specified by the date parameter in the category specified by the category_id parameter. The items will be Good 'til Cancelled and non-Good 'til Cancelled items. If the item is a non-Good 'til Cancelled item, the item's end date will be returned in the itemEndDate column. /item?feed_scope=NEWLY_LISTED&amp;category_id=15032&amp;date=20170925
  * category_id **required** `string`: An eBay top-level category ID of the items to be returned in the feed file. The list of eBay category IDs is not published and category IDs are not all the same across all the eBay maketplace. You can use the following techniques to find a category by site. Use the Category Changes page. Use the Taxonomy API getCategoryTree call. For details see Get Categories for Buy APIs. /commerce/taxonomy/v1_beta/category_tree/0 Restriction: Must be a top-level category
  * date **required** `string`: The date of the feed file you want. The Item feed files are generated every day and there are always 7 daily feed files available. Important: The daily generated files are available each day after 9am MST (US Mountain Standard Time), which is -7 hours UTC time. There is a 48 hour latency when generating the files. This means you can download the file for July 10th on July 12 after 9am MST. Format: yyyyMMdd Requirement: Must be within 3-10 days in the past

#### Output
* output [ItemResponse](#itemresponse)

### getItemDescriptionFeed
The Description feed file is generated each day. This call lets you download a daily TSV_GZIP (tab separated value gzip) Description feed file containing the descriptions of all the items that were listed on a specific day in a specific category. To store the complete item details, you would always run the getItemFeed and getItemDescriptionFeed calls with the same parameters. &nbsp;&nbsp;&nbsp; /item?feed_scope=NEWLY_LISTED&amp;category_id=625&amp;date=20170918 &nbsp;&nbsp;&nbsp; /item_description?feed_scope=NEWLY_LISTED&amp;category_id=625&amp;date=20170918 Combining the Description and Item feed files The Description feed file contains only the itemId, itemGroupId and description columns. The value of the description column is BASE64 encoded. For each row, there will be values in either itemId or itemGroupId. The description column will always contain a value. itemGroupId - The value in this column is the ID of an item group (an item with variations, such as size and color) where the items in the group share the same description. Even though the itemGroupId represents more than one item, the itemGroupId and description are returned only once for the entire group. In this case, there will be values in the itemGroupId and description columns. You match the value of itemGroupId from the Description feed file with the value of primaryItemGroupId from the Item feed file for the same day and category. itemId - The value in this column is the ID of an item that is not part of an item group or (in rare cases) the item is part of an item group but does not share a description with other items in the group. In this case, there will be values in the itemId and description columns. You match the value of itemId from the Description feed file with the value of itemId from the Item feed file for the same day and category. The file will contain the descriptions for all the items or item groups from all the child categories of the category. The first line of the file is the header, which indicates the order of the values on each line. Each column is described in the Response fields section on this page. Downloading feed files Description feed files are very large so the gzip file, which is binary, is streamed in chunks. You specify the size of the chunks in bytes using the Range request header. The Content-range response header indicates where in the full resource this partial chunk of data belongs and the total number of bytes in the file. For more information about using these headers, see Retrieving a gzip feed file. Important: The response is always a TSV_GZIP file. But for documentation purposes, the response is shown as JSON fields so that the value returned in each column can be explained. The order of the response fields, shows you the order of the columns in the feed file. Restrictions For a list of supported sites and other restrictions, see API Restrictions.


```js
ebay_buy_feed.getItemDescriptionFeed({
  "X-EBAY-C-MARKETPLACE-ID": "",
  "Range": "",
  "feed_scope": "",
  "category_id": "",
  "date": ""
}, context)
```

#### Input
* input `object`
  * X-EBAY-C-MARKETPLACE-ID **required** `string`: The ID for the eBay marketplace where the items are hosted. For example: &nbsp;&nbsp;X-EBAY-C-MARKETPLACE-ID = EBAY-US For a list of supported sites see, API Restrictions.
  * Range **required** `string`: This header specifies the range in bytes of the chunks of the gzip file being returned. Format: bytes=startpos-endpos For example, the following retrieves the first 10 MBs of the feed file. &nbsp;&nbsp;Range bytes=0-10485760 For more information about using this headers, see Retrieving a gzip feed file. Maximum: 100 MB
  * feed_scope **required** `string`: Controls the contents of the feed file. Valid Value: NEWLY_LISTED - Contains all items that were listed on the day specified by the date parameter in the category specified by the category_id parameter. The items will be Good 'til Cancelled and non-Good 'til Cancelled items. If the item is a non-Good 'til Cancelled item, the item's end date will be returned in the itemEndDate column. /item_description?feed_scope=NEWLY_LISTED&amp;category_id=15032&amp;date=20170925
  * category_id **required** `string`: An eBay top-level category ID of the items to be returned in the feed file. The list of eBay category IDs is not published and category IDs are not all the same across all the eBay maketplace. You can use the following techniques to find a category by site. Use the Category Changes page. Use the Taxonomy API getCategoryTree call. For details see Get Categories for Buy APIs. /commerce/taxonomy/v1_beta/category_tree/0
  * date **required** `string`: The date of the feed file you want. The Description feed files are generated every day and there are always 7 daily feed files available. There is a 48 hour latency when generating the files. This means on July 10, the latest feed file you can download is July 8. In other words, if you wanted the file generated on July 10th, you would need to wait until July 12th. Note: The generated files are stored using MST (US Mountain Standard Time), which is -7 hours UTC time. Format: yyyyMMdd Requirement: Must be within 3-10 days in the past

#### Output
* output [ItemDescriptionResponse](#itemdescriptionresponse)



## Definitions

### Item
* Item `object`: The type that defines the fields for the data returned by the <b> getItemFeed</b> call.
  * acceptedPaymentMethods `string`: Indicates whether you can checkout using the Buy Order API. If this column contains PAYPAL, you can use the Buy Order API to checkout and purchase the item. If this column is empty, you must use another method for checkout.
  * availability `string`: An enumeration representing the item's availability (possibility of being in purchasable). For implementation help, refer to <a href='https://developer.ebay.com/devzone/rest/api-ref/feed/types/AvailabilityEnum.html'>eBay API documentation</a>
  * availabilityThreshold `integer`: The 'display item quantity' threshold value the seller has set in the seller preferences on eBay.
  * availabilityThresholdType `string`: This value will be null or MORE_THAN, which indicates that the seller has more that the 'quantity display preference' value in stock for this item. The value of this column depends on how the seller has set their item quantity display preference. Let's say the following are the quantity display preferences the seller can choose between. Display &quot;More than 10 available&quot; in your listing (if applicable) If the seller enables this preference, this column returns 'MORE_THAN' as long as there are more than 10 items in the inventory. If the quantity is equal to 10 or drops below 10, this column will be empty and the actual quantity of the item is returned in the estimatedAvailableQuantity column. Display the exact quantity in your listings If the seller enables this preference, the availabilityThresholdType column will be empty and the actual quantity of the item is returned in the estimatedAvailableQuantity column. For implementation help, refer to <a href='https://developer.ebay.com/devzone/rest/api-ref/feed/types/AvailabilityThresholdEnum.html'>eBay API documentation</a>
  * brand `string`: The name brand of the item, such as Nike, Apple, etc.
  * buyingOptions `string`: The purchase option available for the item, such as FIXED_PRICE.
  * category `string`: The label of the category. For example: Toys &amp; Hobbies|Action Figures|Comic Book Heroes
  * categoryId `string`: The ID of the category of the item. For example: The ID for Toys &amp; Hobbies|Action Figures|Comic Book Heroes is 158671.
  * condition `string`: The text describing the condition of the item. For example: NEW.
  * conditionId `string`: The identifier of the condition of the item.
  * epid `string`: The eBay product identifier of a product from the eBay product catalog. You can use this value in the Browse API search call to retrieve items for this product and in the Marketing API calls to retrieve 'also viewed' and 'also bought' products to encourage upselling and cross-selling.
  * estimatedAvailableQuantity `integer`: The estimated number of items that are available for purchase.
  * gtin `string`: The unique Global Trade Item Number of the item as defined by http://www.gtin.info. This can be a UPC (Universal Product Code), EAN (European Article Number), or an ISBN (International Standard Book Number) value.
  * imageAlteringProhibited `boolean`: A boolean that indicates whether the images can be altered. If the value is true, you cannot modify the image. Note: Due to image licensing agreements and other legal concerns, modification (including resizing) of some images is strictly prohibited. These images are for display as-is only.
  * imageUrl `string`: The URL to the primary image of the item.
  * itemEndDate `string`: A timestamp indicating when the item's sale period will end based on its start date and duration. For Good 'Til Cancelled' items, no value is returned in this column. Format: UTC (yyyy-MM-ddThh:mm:ss.sssZ).
  * itemId `string`: The unique identifier of the eBay item.
  * itemLocationCountry `string`: The country where the item is physically located.
  * localizedAspects `string`: The name/value pairs for the aspects of the item, which are BASE64 encoded. The information label is separated by a pipe (|), the aspect name and value are separated by a colon (:) and the name/value pairs are separated by a semicolon (;). Example without Label Encoded: encodedName:encodedValue;encodedName:encodedValue;encodedName:encodedValue Decoded: Size:XL;Color:Red;Sleeves:Long Example with Label Encoded: encodedLabel|encodedName:encodedValue;encodedName:encodedValue;encodedLabel|encodedName:encodedValue Decoded: Product Identifiers|GTIN:0190198066633;BRAND:Apple;Product Key Features|Model:iPhone 7 Note: The separators ( | : ; ) are not encoded. You must decode each label, name, and value separately. You cannot decode the entire string. For more information, see Encoded Aspects in the Buying Integration Guide.
  * mpn `string`: The manufacturer part number, which is a unique number that identifies a specific product.
  * priceCurrency `string`: The currency used for the price of the item. Generally, this is the currency used by the country of eBay site offering the item. For implementation help, refer to <a href='https://developer.ebay.com/devzone/rest/api-ref/feed/types/CurrencyCodeEnum.html'>eBay API documentation</a>
  * priceValue `string`: The price of the item.
  * primaryItemGroupId `string`: The unique identifier for the item group that contains this item. An item group is an item that has various aspect differences, such as color, size, storage capacity, etc.
  * primaryItemGroupType `string`: The item group type. For example: SELLER_DEFINED_VARIATIONS, indicates that the item group was created by the seller.
  * refundMethod `string`: An enumeration value that indicates how a buyer is refunded when an item is returned. For implementation help, refer to <a href='https://developer.ebay.com/devzone/rest/api-ref/feed/types/RefundMethodEnum.html'>eBay API documentation</a>
  * restockingFeePercentage `string`: The restocking fee percentage that the seller has set on the item. Sellers have the option of setting no restocking fee for an item, or they can set the percentage to 10, 15, or 20 percent. For example, if the cost of the item was $100, and the restocking percentage was 20 percent, the buyer would be charged $20 to return that item. This means that instead of receiving a $100 refund, they would received $80 due to the restocking fee. Values Returned: NoRestockingFee, Percent_10, Percent_15, and Percent_20
  * returnInstructions `string`: The text written by the seller describing what the buyer needs to do in order to return the item.
  * returnMethod `string`: An enumeration value that indicates the alternative methods for a full refund when an item is returned. This column will have data if the seller offers the buyer an item replacement or exchange instead of a monetary refund. For implementation help, refer to <a href='https://developer.ebay.com/devzone/rest/api-ref/feed/types/ReturnMethodEnum.html'>eBay API documentation</a>
  * returnPeriodUnit `string`: An enumeration value that indicates period of time being used to measure the duration, such as business days, months, or years. See the TimeDurationUnitEnum type for a list of possible time-measuring units. For implementation help, refer to <a href='https://developer.ebay.com/devzone/rest/api-ref/feed/types/TimeDurationUnitEnum.html'>eBay API documentation</a>
  * returnPeriodValue `integer`: The amount of time the buyer has to return the item after the purchase date. This can be shown in the number of years, months, days, hours, or minutes. If this value is '30', and the returnPeriodUnit value is 'DAY', the return period is 30 days.
  * returnShippingCostPayer `string`: The party responsible for the return shipping costs when an item is returned. This will be either buyer or seller. For implementation help, refer to <a href='https://developer.ebay.com/devzone/rest/api-ref/feed/types/ReturnShippingCostPayerEnum.html'>eBay API documentation</a>
  * returnsAccepted `boolean`: Indicates whether the seller accepts returns for the item.
  * sellerFeedbackPercentage `string`: The percentage of the total positive feedback.
  * sellerFeedbackScore `string`: The feedback score of the seller. This value is based on the ratings from eBay members that bought items from this seller.
  * sellerItemRevision `string`: An identifier generated/incremented when a seller revises the item. There are two types of item revisions; seller changes, such as changing the title and eBay system changes, such as changing the quantity when an item is purchased. This ID is changed only when the seller makes a change to the item.
  * sellerTrustLevel `string`: An enumeration value representing the eBay status of the seller. For implementation help, refer to <a href='https://developer.ebay.com/devzone/rest/api-ref/feed/types/SellerTrustLevelEnum.html'>eBay API documentation</a>
  * sellerUsername `string`: The seller's eBay user name.
  * title `string`: The seller created title of the item.

### ItemDescription
* ItemDescription `object`: The type that defines fields for the data returned by the <b> getItemDescriptionFeed</b> call. The first row is the header, which indicates the order of the values for each item. The header labels match the fields that are described below. 
  * description `string`: The seller created description of the item, which is BASE64 encoded. Descriptions can contain HTML code, links, etc. and are often very large.
  * itemGroupId `string`: The unique identifier for the item group (items with variations, such as a shirt in multiple sizes) that contains this item. If this is null, it indicates this item is not part of an item group.
  * itemId `string`: The unique identifier of the eBay item.

### ItemDescriptionResponse
* ItemDescriptionResponse `object`: The type that defines the array for the fields returned by the <b> getItemDescriptionFeed</b> call.
  * itemDescriptions `array`: The container for the array of items returned by the getItemDescriptionFeed call. The data is tab separated and the first row is the header, which indicates the order of the values for each item. The header labels match the fields that are described below.
    * items [ItemDescription](#itemdescription)

### ItemResponse
* ItemResponse `object`: The type that defines the array for the items returned by the <b> getItemFeed</b> call. 
  * items `array`: The container for the array of items returned by the getItemFeed call. The data is tab separated and the first row will be the header, which indicates the order of the values on each line. The header labels match the fields that are described below.
    * items [Item](#item)

