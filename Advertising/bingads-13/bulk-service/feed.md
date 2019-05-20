---
title: "Feed Record - Bulk"
ms.service: bing-ads-bulk-service
ms.topic: "article"
author: "eric-urban"
ms.author: "eur"
description: Describes the Feed fields in a Bulk file.
dev_langs:
  - csharp
---
# Feed Record - Bulk
> [!NOTE]
> The Bulk API for feeds and documentation are subject to change.

Defines a feed that can be downloaded and uploaded in a bulk file.

> [!NOTE]
> Not everyone has this feature yet. If you don't, don't worry. It's coming soon. 

You can have 100 feeds per account (this maximum number includes all feed types) and the maximum number of feed items (rows) per account is 5 million.

You can download all *Feed* records in the account by including the [DownloadEntity](downloadentity.md) value of *Feeds* in the [DownloadCampaignsByAccountIds](downloadcampaignsbyaccountids.md) or [DownloadCampaignsByCampaignIds](downloadcampaignsbycampaignids.md) service request. Additionally the download request must include the [EntityData](datascope.md#entitydata) scope. For more details about the Bulk service including best practices, see [Bulk Download and Upload](../guides/bulk-download-upload.md).

The following Bulk CSV example would add a new page feed and ad customizer feed with one [Feed Item](feed-item.md) for each.

```csv
Type,Status,Id,Parent Id,Sub Type,Campaign,Ad Group,Client Id,Modified Time,Start Date,End Date,Device Preference,Keyword,Match Type,Target,Physical Intent,Name,Ad Schedule,Audience Id,Feed Name,Custom Attributes
Format Version,,,,,,,,,,,,,,,,6,,,,
Feed,Active,-20,,PageFeed,,,PageFeedClientIdGoesHere,,,,,,,,,,,,MyPageFeedName,"{""pagefeed"":[{""name"":""Page Url"",""feedAttributeType"":""Url"",""isPartOfKey"":true},{""name"":""Custom Label"",""feedAttributeType"":""StringList""}]}"
Feed,Active,-21,,AdCustomizerFeed,,,AdCustomizerFeedClientIdGoesHere,,,,,,,,,,,,MyAdCustomizerFeedName,"{""adCustomizerFeed"":[{""name"":""DateTimeName"",""feedAttributeType"":""DateTime""},{""name"":""Int64Name"",""feedAttributeType"":""Int64""},{""name"":""PriceName"",""feedAttributeType"":""Price""},{""name"":""StringName"",""feedAttributeType"":""String"",""isPartOfKey"":true}]}"
Feed Item,Active,-200,-20,,,,20;200,,05/22/2019 00:00:00,05/30/2019 00:00:00,,,,,,,,,,"{""Page Url"":""https://contoso.com/3001"",""Custom Label"":[""Label_1_3001"",""Label_2_3001""]}"
Feed Item,Active,-210,-21,,,,21;210,,05/22/2019 00:00:00,05/30/2019 00:00:00,,value,Broad,,PeopleIn,,(Sunday[09:00-17:00]),,,"{""DateTimeName"":""2019/05/22 00:00:00"",""Int64Name"":237601,""PriceName"":""$601"",""StringName"":""s237601""}"
```

For a *Feed* record, the following attribute fields are available in the [Bulk File Schema](bulk-file-schema.md). 

- [Client Id](#clientid)
- [Custom Attributes](#customattributes)
- [Feed Name](#feedname)
- [Id](#id)
- [Modified Time](#modifiedtime)
- [Parent Id](#parentid)
- [Status](#status)
- [Sub Type](#subtype)

## <a name="clientid"></a>Client Id
Used to associate records in the bulk upload file with records in the results file. The value of this field is not used or stored by the server; it is simply copied from the uploaded record to the corresponding result record. It may be any valid string to up 100 in length.

**Add:** Optional  
**Update:** Optional    
**Delete:** Read-only  

## <a name="customattributes"></a>Custom Attributes
The attributes are customized for each feed [Sub Type](#subtype), and define what information about your products or services you want to insert into your ads.

The custom attributes are represented in the bulk file as a JSON string. For more details see [feedAttributeType](#customattributes-feedattributetype), [id](#customattributes-id), [isPartOfKey](#customattributes-ispartofkey), and [name](#customattributes-name) below.

Here are example custom attributes that you could upload for a page feed.

```json
{
	"pagefeed": [
		{
			"name": "Page Url",
			"feedAttributeType": "Url",
			"isPartOfKey": true
		},
		{
			"name": "Custom Label",
			"feedAttributeType": "StringList"
		}
	]
}
```

> [!NOTE]
> In the comma separated bulk file you'll need to surround the list of attributes, each attribute key, and each attribute value with an extra set of double quotes e.g., the above JSON string would be written as *"{""pagefeed"":[{""name"":""Page Url"",""feedAttributeType"":""Url"",""isPartOfKey"":true},{""name"":""Custom Label"",""feedAttributeType"":""StringList""}]}"*.

Here's an example Bulk download where you'll also get read-only attributes for the page feed e.g., `id`: 

```json
{
	"pagefeed": [
		{
			"id": 1,
			"name": "Page Url",
			"feedAttributeType": "Url",
			"isPartOfKey": true
		},
		{
			"id": 2,
			"name": "Custom Label",
			"feedAttributeType": "StringList"
		}
	]
}
```

**Add:** Required. For an ad customizer feed you must set at least one attribute with [name](#customattributes-name) and [feedAttributeType](#customattributes-feedattributetype) keys. For a page feed you must set at least one attribute with [name](#customattributes-name) set to "Page Url". Only the [name](#customattributes-name), [feedAttributeType](#customattributes-feedattributetype) and [isPartOfKey](#customattributes-ispartofkey) keys are honored. The [id](#customattributes-id) key will be ignored if you include it.  
**Update:** Read-only. You cannot add or remove custom attributes after the feed has been created. You can create a new feed, and delete the current feed as needed.    
**Delete:** Read-only  

### <a name="customattributes-feedattributetype"></a>feedAttributeType
The data type of each custom attribute. You define the data type in the feed record, and then set values in the feed item. So long as each custom attribute [name](#customattributes-name) is unique within the feed you can define multiple attributes with the same data type.

There are four different `feedAttributeType` data types you can set for ad customizer feeds: 

|feedAttributeType|Use cases|Accepted feed item values|
|-----|-----|-----|
|String|Product names, product categories, descriptions|Any letters, numbers, or symbols|
|Int64|Inventory count, number of colors available|Any whole number|
|Price|Product cost, sale discount|Any number (including decimals) and valid currency characters|
|DateTime|Event start time, last day of a sale|yyyy/mm/dd hh:mm:ss<br/>To default to midnight at the beginning of the day, you can omit the hh:mm:ss part.|

For example we can define the custom attributes of an ad customizer feed.   

```json
{
	"adCustomizerFeed": [
		{
			"name": "DateTimeName",
			"feedAttributeType": "DateTime"
		},
		{
			"name": "Int64Name",
			"feedAttributeType": "Int64"
		},
		{
			"name": "PriceName",
			"feedAttributeType": "Price"
		},
		{
			"name": "StringName",
			"feedAttributeType": "String",
			"isPartOfKey": true
		}
	]
}
```

Then we can map each feed [name](#customattributes-name) i.e., "DateTimeName", "Int64Name", "PriceName", and "StringName" in the [Feed Item](feed-item.md#customattributes) upload: 

```json
{
	"DateTimeName": "2019/05/22 00:00:00",
	"Int64Name": 237601,
	"PriceName": "$601",
	"StringName": "s237601"
}
```

There are two different `feedAttributeType` data types you can set for page feeds: 

> [!NOTE]
> The `feedAttributeType` is optional for "Page Feed" and "Custom Label" named attributes. If you do not set `feedAttributeType` the service uses the respective "Url" and "StringList" types. If you do set `feedAttributeType`, then it must be set to the supported value per custom attribute [name](#customattributes-name). 

|feedAttributeType|Use cases|Accepted feed item values|
|-----|-----|-----|
|StringList|Labels that allow you to group the URLs within the feed.|You can include between one to ten custom labels per [Feed Item](feed-item.md#customattributes).<br/>Each custom label is represented as a list item in JSON notation. For example the custom label portion of the [Feed Item](feed-item.md#customattributes) could be written as *""Custom Label"":[""Label_1_3001"",""Label_2_3001""]*|
|Url|Contains the URL of your website to include in the feed.|You must include one URL per [Feed Item](feed-item.md#customattributes).|

For example we can define the custom attributes of page feed.   

```json
{
	"pagefeed": [
		{
			"name": "Page Url",
			"feedAttributeType": "Url",
			"isPartOfKey": true
		},
		{
			"name": "Custom Label",
			"feedAttributeType": "StringList"
		}
	]
}
```

Then we can map each feed [name](#customattributes-name) i.e., "Page Url" and "Custom Label" in the [Feed Item](feed-item.md#customattributes) upload: 

```json
{
	"Page Url": "https://contoso.com/3001",
	"Custom Label": [
		"Label_1_3001",
		"Label_2_3001"
	]
}
```

### <a name="customattributes-id"></a>id
The `id` attribute is a unique read-only Microsoft Advertising identifier for the feed.

### <a name="customattributes-ispartofkey"></a>isPartOfKey
The `isPartOfKey` determines whether or not the values for a custom attribute must be unique across all [Feed Item](feed-item.md) records that roll up to the feed. If the `isPartOfKey` is set to "True" the values must be unique, and otherwise you can upload duplicate values for the same custom attribute. 

### <a name="customattributes-name"></a>name
The `name` attribute is used to map a distinct custom attribute across both the feed and [Feed Item](feed-item.md#customattributes-name). Effectively this is how you ensure that a specific feed item rolls up to the same "column" in the feed. In the example above the "DateTimeName", "Int64Name", "PriceName", and "StringName" names are used in both the feed and feed item. 

## <a name="feedname"></a>Feed Name
The name of the feed. 

The name must be unique (case-insensitive) among all feeds within the account. The name can contain a maximum of 128 characters. 

**Add:** Required  
**Update:** Optional. If no value is set for the update, this setting is not changed.    
**Delete:** Read-only  

## <a name="id"></a>Id
The system generated identifier of the feed.

**Add:** Optional. You must either leave this field empty, or specify a negative identifier. A negative identifier set for the feed can then be referenced in the *Id* field of dependent record types such as [Feed Item](feed-item.md). This is recommended if you are adding new feeds and feed items in the same Bulk file. For more information, see [Bulk File Schema Reference Keys](../bulk-service/bulk-file-schema.md#referencekeys).  
**Update:** Read-only and Required  
**Delete:** Read-only and Required  

## <a name="modifiedtime"></a>Modified Time
The date and time that the entity was last updated. The value is in Coordinated Universal Time (UTC).

> [!NOTE]
> The date and time value reflects the date and time at the server, not the client. For information about the format of the date and time, see the dateTime entry in [Primitive XML Data Types](https://go.microsoft.com/fwlink/?linkid=859198).

**Add:** Read-only  
**Update:** Read-only  
**Delete:** Read-only  

## <a name="parentid"></a>Parent Id
The system generated identifier of the account that contains the feed.

This bulk field maps to the *Id* field of the [Account](account.md) record.

**Add:** Read-only  
**Update:** Read-only  
**Delete:** Read-only  

## <a name="status"></a>Status
The status of the ad.

Possible values are *Active* or *Deleted*. 

**Add:** Optional. The default value is *Active*.  
**Update:** Optional. If no value is set for the update, this setting is not changed.    
**Delete:** Required. The Status must be set to *Deleted*.

## <a name="subtype"></a>Sub Type
The feed sub type is included in the Bulk download file for readability.

The possible values include *PageFeed* and *AdCustomizerFeed*.

**Add:** Read-only  
**Update:** Read-only  
**Delete:** Read-only  
