---
title: ProductMatchCountReportColumn Value Set - Reporting
ms.service: bing-ads-reporting-service
ms.topic: article
author: eric-urban
ms.author: eur
description: Defines the attributes and performance statistics columns that you can include in the ProductMatchCountReportRequest.
---
# ProductMatchCountReportColumn Value Set - Reporting
Defines the attributes and performance statistics columns that you can include in the [ProductMatchCountReportRequest](productmatchcountreportrequest.md).

The attribute columns that you include in a report can affect how the statistics are aggregated. In other words the number of rows increase by a factor of the unique attributes. For more information, see [Columns that Group the Data](../guides/reports.md#columnsdata).

For a list of columns that you must include, please see the [Required Columns](#requiredcolumns) section below.

To see how far back hourly, daily, weekly, monthly, yearly and summary aggregated data can be retrieved for a report, see [Report Data Retention Time Periods](../guides/report-data-retention-time-periods.md).

## Syntax
```xml
<xs:simpleType name="ProductMatchCountReportColumn" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:restriction base="xs:string">
    <xs:enumeration value="CustomerId" />
    <xs:enumeration value="CustomerName" />
    <xs:enumeration value="AccountId" />
    <xs:enumeration value="AccountNumber" />
    <xs:enumeration value="AccountName" />
    <xs:enumeration value="CampaignId" />
    <xs:enumeration value="CampaignName" />
    <xs:enumeration value="AdGroupId" />
    <xs:enumeration value="AdGroupName" />
    <xs:enumeration value="ProductGroup" />
    <xs:enumeration value="PartitionType" />
    <xs:enumeration value="AdGroupCriterionId" />
    <xs:enumeration value="MatchedProductsAtCampaign" />
    <xs:enumeration value="MatchedProductsAtAdGroup" />
    <xs:enumeration value="MatchedProductsAtProductGroup" />
  </xs:restriction>
</xs:simpleType>
```

## <a name="values"></a>Values

|Value|Description|
|-----------|---------------|
|<a name="accountid"></a>AccountId|The Microsoft Advertising assigned identifier of an account.|
|<a name="accountname"></a>AccountName|The account name.|
|<a name="accountnumber"></a>AccountNumber|The Microsoft Advertising assigned number of an account.|
|<a name="adgroupcriterionid"></a>AdGroupCriterionId|The Microsoft Advertising assigned identifier of an ad group criterion.|
|<a name="adgroupid"></a>AdGroupId|The Microsoft Advertising assigned identifier of an ad group.|
|<a name="adgroupname"></a>AdGroupName|The ad group name.|
|<a name="campaignid"></a>CampaignId|The Microsoft Advertising assigned identifier of a campaign.|
|<a name="campaignname"></a>CampaignName|The campaign name.|
|<a name="customerid"></a>CustomerId|The Microsoft Advertising assigned identifier of a customer.|
|<a name="customername"></a>CustomerName|The customer name.|
|<a name="matchedproductsatadgroup"></a>MatchedProductsAtAdGroup|The number of products per ad group that matched your product group targets.|
|<a name="matchedproductsatcampaign"></a>MatchedProductsAtCampaign|The number of products per campaign that matched your product group targets.|
|<a name="matchedproductsatproductgroup"></a>MatchedProductsAtProductGroup|The number of products per product group that matched your product group targets.|
|<a name="partitiontype"></a>PartitionType|The product partition type.|
|<a name="productgroup"></a>ProductGroup|The forward slash ("/") delimited list of product conditions, reported as Operand = Attribute. The attribute values are not surrounded by "" (double quotes). The category level is appended to the attribute values if applicable e.g., "(1st Level)", "(2nd Level)", etcetera. Here is an example: * \ Category=Animals & Pet Supplies(1st Level) \ Category=Pet Supplies(2nd Level) \ Category=Bird Supplies(3rd Level). The "*" (single asterisk) refers to a product group that matches everything else besides the other filters for the product group.|

## <a name="remarks"></a>Remarks
### <a name="requiredcolumns"></a>Required Columns
The report must include at least one of the following attribute columns and at least one performance statistics column. For more information, see [Report Attributes and Performance Statistics](../guides/report-attributes-performance-statistics.md).

|Column|
|----------|
|AccountName|
|CampaignName|

One or more of the MatchedProductsAtAdGroup, MatchedProductsAtCampaign, or MatchedProductsAtProductGroup performance statistics columns are also required.

## Requirements
Service: [ReportingService.svc v12](https://reporting.api.bingads.microsoft.com/Api/Advertiser/Reporting/v12/ReportingService.svc)  
Namespace: https\://bingads.microsoft.com/Reporting/v12  

## Used By
[ProductMatchCountReportRequest](productmatchcountreportrequest.md)  
