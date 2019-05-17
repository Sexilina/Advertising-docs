---
title: "Feed Item Record - Bulk"
ms.service: bing-ads-bulk-service
ms.topic: "article"
author: "eric-urban"
ms.author: "eur"
description: Describes the Feed Item fields in a Bulk file.
dev_langs:
  - csharp
---
# Feed Item Record - Bulk
> [!NOTE]
> The Bulk API for feeds and documentation are subject to change.

Defines a feed that can be downloaded and uploaded in a bulk file.

> [!NOTE]
> Not everyone has this feature yet. If you don't, don't worry. It's coming soon. 

> [!NOTE]
> Duplicate feed names are allowed in the same account. 

You can download all *Feed Item* records in the account by including the [DownloadEntity](downloadentity.md) value of *FeedItems* in the [DownloadCampaignsByAccountIds](downloadcampaignsbyaccountids.md) or [DownloadCampaignsByCampaignIds](downloadcampaignsbycampaignids.md) service request. Additionally the download request must include the [EntityData](datascope.md#entitydata) scope. For more details about the Bulk service including best practices, see [Bulk Download and Upload](../guides/bulk-download-upload.md).

The following Bulk CSV example would add a new page feed and ad customizer [Feed](feed.md) with one feed item for each. 

```csv
Type,Status,Id,Parent Id,Sub Type,Campaign,Ad Group,Client Id,Modified Time,Start Date,End Date,Device Preference,Keyword,Match Type,Target,Physical Intent,Name,Ad Schedule,Audience Id,Feed Name,Custom Attributes
Format Version,,,,,,,,,,,,,,,,6,,,,
Feed,Active,-20,,PageFeed,,,PageFeedClientIdGoesHere,,,,,,,,,,,,MyPageFeedName,"{""pagefeed"":[{""name"":""Page Url"",""isPartOfKey"":true},{""name"":""Custom Label""}]}"
Feed,Active,-21,,AdCustomizerFeed,,,AdCustomizerFeedClientIdGoesHere,,,,,,,,,,,,MyAdCustomizerFeedName,"{""adCustomizerFeed"":[{""name"":""DateTimeName"",""feedAttributeType"":""DateTime""},{""name"":""Int64Name"",""feedAttributeType"":""Int64""},{""name"":""PriceName"",""feedAttributeType"":""Price""},{""name"":""StringName"",""feedAttributeType"":""String"",""isPartOfKey"":true}]}"
Feed Item,Active,-200,-20,,,,20;200,,05/22/2019 00:00:00,05/30/2019 00:00:00,,,,,,,,,,"{""Page Url"":""https://contoso.com/3001"",""Custom Label"":[""Label_1_3001""]}"
Feed Item,Active,-210,-21,,,,21;210,,05/22/2019 00:00:00,05/30/2019 00:00:00,,value,Broad,,PeopleIn,,(Sunday[09:00-17:00]),,,"{""DateTimeName"":""2019/05/22 00:00:00"",""Int64Name"":237601,""PriceName"":""$601"",""StringName"":""s237601""}"
```

For a *Feed Item* record, the following attribute fields are available in the [Bulk File Schema](bulk-file-schema.md). 

- [Ad Group](#adgroup)
- [Ad Schedule](#adschedule)
- [Audience Id](#audienceid)
- [Campaign](#campaign)
- [Client Id](#clientid)
- [Custom Attributes](#customattributes)
- [Device Preference](#devicepreference)
- [End Date](#enddate)
- [Id](#id)
- [Keyword](#keyword)
- [Match Type](#matchtype)
- [Modified Time](#modifiedtime)
- [Parent Id](#parentid)
- [Physical Intent](#physicalintent)
- [Start Date](#startdate)
- [Status](#status)
- [Sub Type](#target)
- [Target](#id)

## <a name="adgroup"></a>Ad Group
The name of the ad group where this ad extension is associated or removed.  

**Add:** Read-only and Required  
**Delete:** Read-only and Required  

> [!NOTE]
> For add and delete, you must specify either the [Parent Id](#parentid) or both of the [Ad Group](#adgroup) and [Campaign](#campaign) fields. 

## <a name="adschedule"></a>Ad Schedule
The list of day and time ranges that you want the ad extension to be displayed with your ads. Each day and time range includes the scheduled day of week, start/end hour, and start/end minute. Each day and time range is enclosed by left and right parentheses, and separated from other day and time ranges with a semicolon (;) delimiter. Within each day and time range the format is *Day[StartHour:StartMinue-EndHour:EndMinute]*.

The possible values of *StartHour* range from 00 to 23, where *00* is equivalent to 12:00AM and *12* is 12:00PM.

The possible values of *EndHour* range from 00 to 24, where *00* is equivalent to 12:00AM and *12* is 12:00PM.

The possible values of *StartMinute* and *EndMinute* range from 00 to 60.

The following example demonstrates day and time ranges during weekdays from 9:00AM through 9:00PM: *(Monday[09:00-21:00]);(Tuesday[09:00-21:00]);(Wednesday[09:00-21:00]);(Thursday[09:00-21:00]);(Friday[09:00-21:00])*

**Add:** Optional. If you do not set this field, then ad extensions will be eligible for scheduling anytime during the calendar start and end dates.  
**Update:** Optional. If no value is set for the update, this setting is not changed. The individual day and time ranges cannot be updated. You can effectively update the day and time ranges by sending a new set that should replace the prior set. If you do not set this field, then the existing settings will be retained. If you set this field to *delete_value*, then you are effectively removing all existing day and time ranges.    
**Delete:** Read-only  

## <a name="audienceid"></a>Audience Id
The Microsoft Advertising identifier of the remarketing list associated with the campaign.

This bulk field maps to the *Id* field of the [Remarketing List](remarketing-list.md) record.

**Add:** Read-only and Required for some use cases. You must either specify the [Audience](#audience) or [Audience Id](#audienceid) field. If you set the [Audience Id](#audienceid) field, you must either specify an existing remarketing list identifier or specify a negative identifier that is equal to the *Id* field of the parent [Remarketing List](remarketing-list.md) record. If the [Audience Id](#audienceid) field is not set, then you must set the [Audience](#audience) field as a logical key to the same value as the *Audience* field of the [Remarketing List](remarketing-list.md) record. Any of these options are recommended if you are adding new Campaign Negative Remarketing List Associations with new remarketing lists in the same Bulk file. For more information, see [Bulk File Schema Reference Keys](../bulk-service/bulk-file-schema.md#referencekeys).  
**Update:** Read-only    
**Delete:** Read-only  

## <a name="campaign"></a>Campaign
The name of the campaign that contains the ad group where this ad extension is associated or removed.

**Add:** Read-only  
**Delete:** Read-only  

> [!NOTE]
> For add and delete, you must specify either the [Parent Id](#parentid) or both of the [Ad Group](#adgroup) and [Campaign](#campaign) fields. 

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
			"isPartOfKey": true
		},
		{
			"name": "Custom Label"
		}
	]
}
```

> [!NOTE]
> In the comma separated bulk file you'll need to surround the list of attributes, each attribute key, and each attribute value with an extra set of double quotes e.g., the above JSON string would be written as *"{""pagefeed"":[{""name"":""Page Url"",""isPartOfKey"":true},{""name"":""Custom Label""}]}"*.

Here's an example Bulk download where you'll also get read-only attributes for the page feed e.g., `id`: 

```json
{
	"pagefeed": [
		{
			"id": 1,
			"name": "Page Url",
			"isPartOfKey": true
		},
		{
			"id": 2,
			"name": "Custom Label"
		}
	]
}
```

Here are example custom attributes that you could upload for an ad customizer feed.

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

**Add:** Required. For an ad customizer feed you must set at least one attribute with [name](#customattributes-name) and [feedAttributeType](#customattributes-feedattributetype) keys. For a page feed you must set at least one attribute with [name](#customattributes-name) set to "Page Url" and [isPartOfKey](#customattributes-ispartofkey) key set to *true*. Only the [name](#customattributes-name), [feedAttributeType](#customattributes-feedattributetype) and [isPartOfKey](#customattributes-ispartofkey) keys are honored. The [id](#customattributes-id) key will be ignored if you include it.  
**Update:** Read-only. You cannot add or remove custom attributes after the feed has been created. You can create a new feed, and delete the current feed as needed.    
**Delete:** Read-only  

### <a name="customattributes-feedattributetype"></a>feedAttributeType
The data type of each custom attribute. 

There are four different `feedAttributeType` values you can set for ad customizers:

|feedAttributeType|Use cases|Accepted values|
|-----|-----|-----|
|String|Product names, product categories, descriptions|Any letters, numbers, or symbols|
|Int64|Inventory count, number of colors available|Any whole number|
|Price|Product cost, sale discount|Any number (including decimals) and valid currency characters|
|DateTime|Event start time, last day of a sale|yyyy/mm/dd hh:mm:ss (24-hour time; the hh:mm:ss is optional)|

The `feedAttributeType` key is not applicable for page feeds.

### <a name="customattributes-id"></a>id
The `id` attribute is a unique read-only Microsoft Advertising identifier for the feed.

### <a name="customattributes-ispartofkey"></a>isPartOfKey
The `isPartOfKey` determines whether the attribute must be unique for each of [Feed Item](feed-item.md) that belongs to this feed. 

### <a name="customattributes-name"></a>name
The `name` attribute is the name you'll use later when referring to a specific custom attribute in each [Feed Item](feed-item.md). 

## <a name="devicepreference"></a>Device Preference
This field determines whether the preference is for the ad extension to be displayed on mobile devices or all devices.

Possible values are *All* and *Mobile*, which differ from values used in the campaign management service. 

The default value is *All*.

In the bulk download and upload results file, this field will never be empty. If you did not specify a device preference, the default value of *All* will be returned.

**Add:** Optional  
**Update:** Optional. If no value is set for the update, this setting is not changed. If you set this field to the *delete_value* string, the prior setting is removed. If you set this field to *delete_value*, then you are effectively resetting to the default value of *All*.    
**Delete:** Read-only  

## <a name="enddate"></a>End Date
The ad extension scheduled end date string formatted as *MM/DD/YYYY*.

The end date is inclusive. For example, if you set this field to 12/31/2019, the ad extensions will stop being shown at 11:59 PM on 12/31/2019.

**Add:** Optional. If you do not specify an end date, the ad extensions will continue to be delivered unless you pause the associated campaigns, ad groups, or ads.  
**Update:** Optional. If no value is set for the update, this setting is not changed. The end date can be shortened or extended, as long as the start date is either null or occurs before the new end date. If you do not set this field, then the existing settings will be retained. If you set this field to *delete_value*, then you are effectively removing the end date and the ad extensions will continue to be delivered unless you pause the associated campaigns, ad groups, or ads.    
**Delete:** Read-only  

## <a name="id"></a>Id
The system generated identifier of the feed.

**Add:** Optional. You must either leave this field empty, or specify a negative identifier. A negative identifier set for the feed can then be referenced in the *Id* field of dependent record types such as [Feed Item](feed-item.md). This is recommended if you are adding new feeds and feed items in the same Bulk file. For more information, see [Bulk File Schema Reference Keys](../bulk-service/bulk-file-schema.md#referencekeys).  
**Update:** Read-only and Required  
**Delete:** Read-only and Required  

## <a name="keyword"></a>Keyword
The keyword text.

The text can contain a maximum of 100 characters.

You should specify the keyword in the locale of the Language value that you specified for the ad group to which the keyword belongs.

If the *Match Type* is Broad, your application should handle broad match modifiers. Broad match modified keywords can be modified or prefixed with a plus sign (+), to require that specific terms in your keyword be present in the search query. In the download file, broad match modified terms are preceded by an extra space character. For example if the keyword text is "+Hawaii +hotel", the text included in the bulk download file is " +Hawaii +hotel". For bulk upload you may specify the keyword with or without the preceding space. For more information about broad match modifiers, see [Budget and Bid Strategies](../guides/budget-bid-strategies.md).

**Add:** Required  
**Update:** Read-only    
**Delete:** Read-only

## <a name="matchtype"></a>Match Type
The type of match to compare the keyword and the user's search term.

The supported match type values for a keyword are *Broad*, *Exact* and *Phrase*.

**Add:** Required  
**Update:** Read-only    
**Delete:** Read-only  

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

## <a name="physicalintent"></a>Physical Intent
Determines whether a person must be physically located in the targeted location in order for the ad to display.

The following values are supported. The default value is *PeopleInOrSearchingForOrViewingPages*.
  - Use *PeopleInOrSearchingForOrViewingPages* if you want to show ads to people in, searching for, or viewing pages about your targeted location.  
  - Use *PeopleSearchingForOrViewingPages* if you want to show ads to people searching for or viewing pages about your targeted location.  
  - Use *PeopleIn* if you want to show ads to people in your targeted location.  

**Add:** Optional  
**Update:** Optional. If no value is set for the update, this setting is not changed. If you set this field, then it must be set to a valid value i.e., *PeopleInOrSearchingForOrViewingPages*, *PeopleSearchingForOrViewingPages*, or *PeopleIn*.  
**Delete:** Read-only  

## <a name="startdate"></a>Start Date
The ad extension scheduled start date string formatted as *MM/DD/YYYY*.

The start date is inclusive. For example, if you set *StartDate* to 5/5/2019, the ad extensions will start being shown at 12:00 AM on 5/5/2019.

**Add:** Optional. If you do not specify a start date, the ad extensions are immediately eligible to be scheduled during the day and time ranges.  
**Update:** Optional. If no value is set for the update, this setting is not changed. The start date can be shortened or extended, as long as the end date is either null or occurs after the new start date. If you do not set this field, then the existing settings will be retained. If you set this field to *delete_value*, then you are effectively removing the start date and the ad extensions are immediately eligible to be scheduled during the day and time ranges.    
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

## <a name="target"></a>Target
The identifier of the location that you want to target with the corresponding *Bid Adjustment*. The location identifier corresponds to the *ID* field of the geographical locations file. For more information, see [Geographical Location Codes](../guides/geographical-location-codes.md) and [GetGeoLocationsFileUrl](../campaign-management-service/getgeolocationsfileurl.md).

**Add:** Required  
**Update:** Required  
**Delete:** Read-only  
