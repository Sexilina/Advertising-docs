---
title: GetKeywordDemographics Service Operation
ms.service: bing-ads-ad-insight
ms.topic: article
author: eric-urban
ms.author: eur
---
# GetKeywordDemographics Service Operation
Gets the age and gender of users who have searched for the specified keywords.

## <a name="request"></a>Request Elements
The *GetKeywordDemographicsRequest* object defines the [body](#request-body) and [header](#request-header) elements of the service operation request. The elements must be in the same order as shown in the [Request SOAP](#request-soap). 

### <a name="request-body"></a>Request Body Elements

|Element|Description|Data Type|
|-----------|---------------|-------------|
|<a name="keywords"></a>Keywords|An array of keywords for which you want to get demographics data. The data is broken out by device type. The array can contain a maximum of 1,000 keywords, and each keyword can contain a maximum of 100 characters.|**string**|
|<a name="language"></a>Language|The language in which the keywords are written.<br /><br />For possible values, see [Ad Languages](~/guides/ad-languages.md).|**string**|
|<a name="publishercountry"></a>PublisherCountry|The country code of the country/region to use as the source of the demographics data.<br /><br />The country/region that you specify must support the language specified in the *Language* element.<br /><br />For possible values, see [Geographical Location Codes](~/guides/ad-languages.md).|**string**|
|<a name="device"></a>Device|A list of one or more of the following device types: Computers, NonSmartphones, Smartphones, Tablets. The default is Computers.<br /><br />The response includes keyword demographics data for the device types that you specify only, if available.|**string**|

### <a name="request-header"></a>Request Header Elements
[!INCLUDE[request-header](./includes/request-header.md)]

## <a name="response"></a>Response Elements
The *GetKeywordDemographicsResponse* object defines the [body](#response-body) and [header](#response-header) elements of the service operation response. The elements are returned in the same order as shown in the [Response SOAP](#response-soap).

### <a name="response-body"></a>Response Body Elements

|Element|Description|Data Type|
|-----------|---------------|-------------|
|<a name="keyworddemographicresult"></a>KeywordDemographicResult|An array of [KeywordDemographicResult](../ad-insight/keyworddemographicresult.md) data objects. The order of the items corresponds directly to the keywords specified in the request. If there is no demographic data available for a keyword, the keyword will be included in the list, but the *KeywordDemographics* element of the corresponding [KeywordDemographicResult](../ad-insight/keyworddemographicresult.md) will be null.<br /><br />Each [KeywordDemographicResult](../ad-insight/keyworddemographicresult.md)  contains an array of [KeywordDemographic](../ad-insight/keyworddemographic.md) data objects.  The array contains an item for each device specified in the request. Each [KeywordDemographic](../ad-insight/keyworddemographic.md) contains the percentage of time that users of a certain age and gender searched for the specified keyword.|[KeywordDemographicResult](keyworddemographicresult.md) array|

### <a name="response-header"></a>Response Header Elements
[!INCLUDE[response-header](./includes/response-header.md)]

## <a name="request-soap"></a>Request SOAP
The following template shows the order of the [body](#request-body) and [header](#request-header) elements for the SOAP request.

```xml
<s:Envelope xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header xmlns="Microsoft.Advertiser.AdInsight.Api.Service.V11">
    <Action mustUnderstand="1">GetKeywordDemographics</Action>
    <ApplicationToken i:nil="false">ValueHere</ApplicationToken>
    <AuthenticationToken i:nil="false">ValueHere</AuthenticationToken>
    <CustomerAccountId i:nil="false">ValueHere</CustomerAccountId>
    <CustomerId i:nil="false">ValueHere</CustomerId>
    <DeveloperToken i:nil="false">ValueHere</DeveloperToken>
    <Password i:nil="false">ValueHere</Password>
    <UserName i:nil="false">ValueHere</UserName>
  </s:Header>
  <s:Body>
    <GetKeywordDemographicsRequest xmlns="Microsoft.Advertiser.AdInsight.Api.Service.V11">
      <Keywords i:nil="false" xmlns:a1="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
        <a1:string>ValueHere</a1:string>
      </Keywords>
      <Language i:nil="false">ValueHere</Language>
      <PublisherCountry i:nil="false">ValueHere</PublisherCountry>
      <Device i:nil="false" xmlns:a1="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
        <a1:string>ValueHere</a1:string>
      </Device>
    </GetKeywordDemographicsRequest>
  </s:Body>
</s:Envelope>
```

## <a name="response-soap"></a>Response SOAP
The following template shows the order of the [body](#response-body) and [header](#response-header) elements for the SOAP response.

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header xmlns="Microsoft.Advertiser.AdInsight.Api.Service.V11">
    <TrackingId d3p1:nil="false" xmlns:d3p1="http://www.w3.org/2001/XMLSchema-instance">ValueHere</TrackingId>
  </s:Header>
  <s:Body>
    <GetKeywordDemographicsResponse xmlns="Microsoft.Advertiser.AdInsight.Api.Service.V11">
      <KeywordDemographicResult xmlns:e483="http://schemas.datacontract.org/2004/07/Microsoft.BingAds.Advertiser.AdInsight.Api.DataContract.V11.Entity" d4p1:nil="false" xmlns:d4p1="http://www.w3.org/2001/XMLSchema-instance">
        <e483:KeywordDemographicResult>
          <e483:Keyword d4p1:nil="false">ValueHere</e483:Keyword>
          <e483:KeywordDemographics d4p1:nil="false">
            <e483:KeywordDemographic>
              <e483:Device d4p1:nil="false">ValueHere</e483:Device>
              <e483:Age18_24>ValueHere</e483:Age18_24>
              <e483:Age25_34>ValueHere</e483:Age25_34>
              <e483:Age35_49>ValueHere</e483:Age35_49>
              <e483:Age50_64>ValueHere</e483:Age50_64>
              <e483:Age65Plus>ValueHere</e483:Age65Plus>
              <e483:AgeUnknown>ValueHere</e483:AgeUnknown>
              <e483:Female>ValueHere</e483:Female>
              <e483:Male>ValueHere</e483:Male>
              <e483:GenderUnknown>ValueHere</e483:GenderUnknown>
            </e483:KeywordDemographic>
          </e483:KeywordDemographics>
        </e483:KeywordDemographicResult>
      </KeywordDemographicResult>
    </GetKeywordDemographicsResponse>
  </s:Body>
</s:Envelope>
```

## <a name="example"></a>Code Syntax
```csharp
protected async Task<GetKeywordDemographicsResponse> GetKeywordDemographicsAsync(
	IList<string> keywords,
	string language,
	string publisherCountry,
	IList<string> device)
{
	var request = new GetKeywordDemographicsRequest
	{
		Keywords = keywords,
		Language = language,
		PublisherCountry = publisherCountry,
		Device = device
	};

	return (await AdInsight.CallAsync((s, r) => s.GetKeywordDemographicsAsync(r), request));
}
```
```java
static GetKeywordDemographicsResponse getKeywordDemographics(
	ArrayOfstring keywords,
	string language,
	string publisherCountry,
	ArrayOfstring device)
{
	GetKeywordDemographicsRequest request = new GetKeywordDemographicsRequest();

	request.setKeywords(keywords);
	request.setLanguage(language);
	request.setPublisherCountry(publisherCountry);
	request.setDevice(device);

	return AdInsight.getService().getKeywordDemographics(request);
}
```
```php
static function GetKeywordDemographics(
	$keywords,
	$language,
	$publisherCountry,
	$device)

	$getKeywordDemographicsRequest = new GetKeywordDemographicsRequest();

	$request->Keywords = $keywords;
	$request->Language = $language;
	$request->PublisherCountry = $publisherCountry;
	$request->Device = $device;

	return $AdInsightProxy->GetService()->GetKeywordDemographics($request);
}
```
```python
response=adinsight.GetKeywordDemographics(
	Keywords=KeywordsHere,
	Language=LanguageHere,
	PublisherCountry=PublisherCountryHere,
	Device=DeviceHere
)
```

## Requirements
Service: [AdInsightService.svc v11](https://adinsight.api.bingads.microsoft.com/Api/Advertiser/AdInsight/v11/AdInsightService.svc)  
Namespace: Microsoft.Advertiser.AdInsight.Api.Service.V11  
