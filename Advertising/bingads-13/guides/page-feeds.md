---
title: "Page Feeds"
ms.service: "bing-ads"
ms.topic: "article"
author: "eric-urban"
ms.author: "eur"
description: Add page feeds to fine-tune ad copy for Dynamic Search Ads auto-targets. 
---
# Page Feeds
> [!NOTE]
> The Bulk API for feeds and documentation are subject to change.  

> [!NOTE]
> This feature is currently available in Canada, France, Germany, the United Kingdom, and the United States. 

Page feeds allow you to easily upload all the relevant URLs for your dynamic search ads campaigns. This ensures maximum website coverage and enables the labeling and targeting of specific URLs via custom labels.

Why use page feeds?
- Improve page freshness: Each time a feed is uploaded, the pages in the feed automatically recrawled, so if the pages aren't already in the Bing index of your website, they'll be added. This allows your dynamic search ad campaigns to display the most current version of your website.  
- Fine-tune auto-targets and ad copy: You have full control over what pages are included in an auto-target and as a result, can be more descriptive in your ad copy to drive further engagement.  

You can have 100 feeds per account (this maximum number includes all feed types) and the maximum number of feed items (rows) per account is 5 million.

## <a name="upload-pagefeed"></a>Upload page feeds

You can upload page feeds and feed items with the Bulk service. 
- The [Feed](../bulk-service/feed.md) record defines the name and data type of attributes that are allowed for the corresponding feed items. 
- The [Feed Item](../bulk-service/feed-item.md) record defines additional information about your products or services and under what conditions that information should be inserted into your ads. The Microsoft Advertising system attributes define under what conditions each feed item should be inserted into your ads, whereas the custom attributes define what information about your products or services you want to insert into your ads. Your page feed items can be referenced from all ads in your Microsoft Advertising account by default. 

> [!NOTE]
> The [Feed](../bulk-service/feed.md) and [Feed Item](../bulk-service/feed-item.md) record types are used for both ad customizer feeds and page feeds. When you download feeds and feed items, be sure to check the "Sub Type" column to find out whether the data is applicable for an ad customizer feed or page feed.  

For page feeds you can use attributes named "Page Url" and "Custom Label".  

|Attribute Name|Attribute Data Type|Description|
|-----|-----|-----|
|Page Url|Url|Contains the URLs of your website to include in the feed.<br/><br/>You must include exactly one page URL per feed item.|
|Custom Label|StringList|Labels that allow you to group the URLs within the feed.<br/><br/>You can include between one to ten custom labels per feed item.|

You might visualize the feed column names and field values in a table: 

|Page Url (Url)|Custom Label (StringList)|
|-----|-----|
|https://contoso.com/3001|Label_1_3001|
|https://contoso.com/3001|Label_2_3001|

You could upload the page feed and feed items via the Bulk API as follows:

```csv
Type,Status,Id,Parent Id,Sub Type,Campaign,Ad Group,Client Id,Modified Time,Start Date,End Date,Device Preference,Keyword,Match Type,Target,Physical Intent,Name,Ad Schedule,Audience Id,Feed Name,Custom Attributes
Format Version,,,,,,,,,,,,,,,,6,,,,
Feed,Active,-20,,PageFeed,,,PageFeedClientIdGoesHere,,,,,,,,,,,,MyPageFeedName,"{""pagefeed"":[{""name"":""Page Url"",""feedAttributeType"":""Url"",""isPartOfKey"":true},{""name"":""Custom Label"",""feedAttributeType"":""StringList""}]}"
Feed Item,Active,-200,-20,,,,20;200,,05/22/2019 00:00:00,05/30/2019 00:00:00,,,,,,,,,,"{""Page Url"":""https://contoso.com/3001"",""Custom Label"":[""Label_1_3001"",""Label_2_3001""]}"
```

## <a name="associate-pagefeed"></a>Associate a page feed

Page feeds can be associated at the campaign level, which also applies to all the ad groups within the campaign.



## <a name="customlabel-autotarget"></a>Create custom label auto targets


## See Also
[Dynamic Search Ads](dynamic-search-ads.md)  
[Ad Customizer Feeds](ad-customizer-feeds.md)  