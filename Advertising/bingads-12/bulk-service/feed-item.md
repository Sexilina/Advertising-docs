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

For a *Feed* record, the following attribute fields are available in the [Bulk File Schema](bulk-file-schema.md). 

Campaign,Ad Group,Start Date,End Date,Device Preference,Keyword,Match Type,Target,Physical Intent,Ad Schedule,Audience Id

- [Ad Group](#adgroup)
- [Campaign](#campaign)
- [Client Id](#clientid)
- [Custom Attributes](#customattributes)
- [Device Preference](#devicepreference)
- [End Date](#enddate)
- [Feed Name](#feedname)
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

## <a name="clientid"></a>Client Id
Used to associate records in the bulk upload file with records in the results file. The value of this field is not used or stored by the server; it is simply copied from the uploaded record to the corresponding result record. It may be any valid string to up 100 in length.

**Add:** Optional  
**Update:** Optional    
**Delete:** Read-only  

## <a name="customattributes"></a>Custom Attributes
The list of descriptions that Bing can use to optimize the ad layout.

Unless you pin one of the descriptions to a specific position, Bing will optimize the ad layout dynamically with the best headlines and descriptions for the user's search query. 

From a data model perspective the descriptions are stored as text assets. The same asset can be used by multiple ads. For example if "Seamless Integration" is a text asset, it will have the same asset identifier across all ads in the same Microsoft Advertising account. 

You must set between 2-4 descriptions. The descriptions are represented in the bulk file as a JSON string. Two descriptions are included in the example JSON below, and the first is pinned to a specific position. The `id` and `text` are properties of the asset, whereas the `editorialStatus` and `pinnedField` are properties of the asset link i.e., the relationship between the asset and the ad. For more details see [editorialStatus](#customattributes-editorialstatus), [id](#customattributes-id), [pinnedField](#customattributes-pinnedfield), and [text](#customattributes-text) below.

```json
[{
	"text": "Find New Customers & Increase Sales!",
	"pinnedField": "Description1"
},
{
	"text": "Start Advertising on Contoso Today."
}]
```

> [!NOTE]
> In the comma separated bulk file you'll need to surround the list of asset links, each attribute key, and each attribute string value with an extra set of double quotes e.g., the above JSON string would be written as *"[{""text"":""Find New Customers & Increase Sales!"",""pinnedField"":""Description1""},{""text"":""Start Advertising on Contoso Today.""}]"*. 

Here's an example Bulk download where you'll also get read-only attributes e.g., `id` and `editorialStatus`: 

```json
[{
	"id": 1,
	"text": "Contoso",
	"pinnedField": "Description1",
	"editorialStatus": "Active"
},
{
	"id": 2,
	"text": "Seamless Integration",
	"editorialStatus": "Active"
}]
```

### <a name="customattributes-editorialstatus"></a>editorialStatus
The `editorialStatus` attribute is read-only when you download the responsive search ad. Possible values are described in the table below.  

|Value|Description|
|-----------|---------------|
|Active|The asset passed editorial review.|
|ActiveLimited|The asset passed editorial review in one or more markets, and one or more elements of the asset is undergoing editorial review in another market. For example the asset passed editorial review for Canada and is still pending review in the United States.|
|Disapproved|The asset failed editorial review.|
|Inactive|One or more elements of the asset is undergoing editorial review.|
|Unknown|Reserved for future use.|

### <a name="customattributes-id"></a>id
The `id` attribute is a unique Microsoft Advertising identifier for the asset in a Microsoft Advertising account. 

The same asset can be used by multiple ads. For example if "Seamless Integration" is a text asset, it will have the same asset identifier across all ads in the same Microsoft Advertising account. After you upload a text asset the result file will include the asset identifier e.g., `""id:""123`, whether the asset is new or already existed in the account's asset library. 

### <a name="customattributes-pinnedfield"></a>pinnedField
To pin an asset to a specific description position, set the `pinnedField` attribute to either "Description1" or "Description2". Unless you have a specific requirement for a text asset, don't pin it and let Bing AI optimize the text placement. 

At least one eligible text asset must be available for each description position, so you cannot pin all of the assets to the same position. For example you can have 3 text assets pinned to *Description1*, so long as you have at least one text asset in the responsive search ad's [Description](#description) list that is either not pinned, or pinned to *Description2*.

When you download an asset that is not pinned, the `pinnedField` attribute is not returned.

### <a name="customattributes-text"></a>text
Each description's `text` attribute must contain at least one word. For efficient use of resources we recommend that you use dynamic text strings such as {keyword} instead of creating new ad copy for each keyword. For more information, see the Microsoft Advertising help article [Automatically customize your ads with dynamic text parameters](https://help.ads.microsoft.com/#apex/3/en/50811/1).

The `text` attribute can contain a countdown function. Regardless of the total length of all unsubstituted countdown parameters, the final displayed countdown will always use 8 characters out of the total characters available. For more details see [Countdown Customizers](../guides/countdown-customizers.md).

The maximum input length of each description's `text` attribute is 1,000 characters including dynamic text strings, and of those 1,000 no more than 90 final characters are allowed after substitution. The ad will fail to display or [default text](https://help.ads.microsoft.com/#apex/3/en/50811/1/#DefaultText) will be used if the length exceeds 90 characters after dynamic text substitution occurs. For languages with double-width characters e.g. Traditional Chinese the maximum input length is 500 characters including dynamic text strings, and of those 500 no more than 45 final characters are allowed after substitution. The ad will fail to display or [default text](https://help.ads.microsoft.com/#apex/3/en/50811/1/#DefaultText) will be used if the length exceeds 45 characters after dynamic text substitution occurs. The double-width characters are determined by the characters you use instead of the character set of the campaign or ad group language settings. Double-width characters include Korean, Japanese and Chinese languages characters as well as Emojis.

The `text` attribute cannot contain the newline (\n) character.

**Add:** Required. The list of 3-15 descriptions is required. Only the [pinnedField](#customattributes-pinnedfield) and [text](#customattributes-text) are honored. Even if the asset already exists in your account, the [id](#customattributes-id) and [editorialStatus](#customattributes-editorialstatus) will be ignored if you include them.  
**Update:** Optional. To retain all of the existing asset links, set or leave this field empty. If you set this field, any descriptions that were previously linked to this ad will be replaced. If you specify this field, a list of 3-15 descriptions is required. Only the [pinnedField](#customattributes-pinnedfield) and [text](#customattributes-text) are honored. Even if the asset already exists in your account, the [id](#customattributes-id) and [editorialStatus](#customattributes-editorialstatus) will be ignored if you include them.   
**Delete:** Read-only  

## <a name="feedname"></a>Feed Name
The name of the feed. 

The [name](campaign.md#campaign) of the [feed campaign](#feedcampaignid) that is created from the [base campaign](#basecampaignid) will match the feed name, and vice versa. If the feed name is updated, the feed campaign's name will automatically be updated to match, and vice versa. 

The name must be unique (case-insensitive) among all campaigns and feeds within the account. The name can contain a maximum of 128 characters. 

**Add:** Required  
**Update:** Optional. If no value is set for the update, this setting is not changed.    
**Delete:** Read-only  

## <a name="id"></a>Id
The system generated identifier of the ad extension.

**Add:** Optional. You must either leave this field empty, or specify a negative identifier. A negative identifier set for the ad extension can then be referenced in the *Id* field of dependent record types such as [Ad Group Image Ad Extension](ad-group-image-ad-extension.md) and [Campaign Image Ad Extension](campaign-image-ad-extension.md). This is recommended if you are adding new ad extensions and new dependent records in the same Bulk file. For more information, see [Bulk File Schema Reference Keys](../bulk-service/bulk-file-schema.md#referencekeys).  
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
The system generated identifier of the account that contains the ad extension.

This bulk field maps to the *Id* field of the [Account](account.md) record.

**Add:** Read-only  
**Update:** Read-only  
**Delete:** Read-only  

## <a name="status"></a>Status
The status of the ad.

Possible values are *Active*, *Paused*, or *Deleted*. 

**Add:** Optional. The default value is *Active*.  
**Update:** Optional. If no value is set for the update, this setting is not changed.    
**Delete:** Required. The Status must be set to *Deleted*.

## <a name="subtype"></a>Sub Type
The feed sub type.

This field is only applicable for campaigns of type *Shopping*, and will be empty for all other campaign types.

We are introducing Cooperative campaigns during calendar year 2018 as a sub type of Microsoft Shopping Campaigns. More details about Cooperative campaigns are coming soon, and whether or not you plan to adopt Cooperative campaigns, you might need to make code changes if your application supports any Microsoft Shopping Campaigns.

When you download campaigns and if the [Campaign Type](#campaigntype) field is set to *Shopping*, please also check the *Sub Type* of each campaign. If the *Sub Type* is not set then it is a standard Microsoft Shopping campaign. If the value is set to *ShoppingCoOperative*, the campaign is a Cooperative campaign with different requirements.  

**Add:** Optional and not applicable for most campaign types. For Cooperative campaigns you must set the sub type to *ShoppingCoOperative*.  
**Update:** Read-only  
**Delete:** Read-only  
