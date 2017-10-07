---
title: "aaaProduct šablon ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak se obsah hello toocustomize produktu hello stránky v portálu pro vývojáře Azure API Management hello."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 49f9254c-4c5f-4ed4-9c8d-798f44e805ee
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 60600299287aad87f9b621782ab5ceb866601d03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="product-templates-in-azure-api-management"></a>Šablony produktu v Azure API Management
Azure API Management poskytuje že Hello možnost toocustomize hello obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu. Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a hello editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a zadané sadu lokalizované [řetězce prostředků](api-management-template-resources.md#strings), [ Prostředky glyfy](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu tooconfigure hello obsah hello stránek podle potřeby pomocí těchto šablon.  
  
 Hello šablony v této části Povolit toocustomize hello obsah stránky produktu hello na portál pro vývojáře hello.  
  
-   [Seznam produktů](#ProductList)  
  
-   [Produktu](#Product)  
  
> [!NOTE]
>  Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci hello, ale jsou toochange subjektu z důvodu vylepšení toocontinuous. Hello za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře hello přechodem toohello potřeby jednotlivých šablony. Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
##  <a name="ProductList"></a>Seznam produktů  
 Hello **seznam produktů** šablona vám umožní toocustomize hello textu hello produktu seznamu stránky v portálu pro vývojáře hello.  
  
 ![Seznam produktů](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")  
  
### <a name="default-template"></a>Výchozí šablony  
  
```xml  
<search-control></search-control>  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if products.size > 0 %}  
    <ul class="list-unstyled">  
    {% for product in products %}  
        <li>  
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>  
            {{product.description}}  
        </li>     
    {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a>Ovládací prvky  
 Hello `Product list` šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).  
  
-   [ovládací prvek stránkování](api-management-page-controls.md#paging-control)  
  
-   [ovládací prvek vyhledávání](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a>Datový model  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Stránkování|[Stránkování](api-management-template-data-model-reference.md#Paging) entity.|informace o stránkování Hello pro kolekci produkty hello.|  
|Filtrování|[Filtrování](api-management-template-data-model-reference.md#Filtering) entity.|filtrování informace hello produkty seznam stránku Hello.|  
|Produkty|Kolekce [produktu](api-management-template-data-model-reference.md#Product) entity.|Hello produkty viditelné toohello aktuálního uživatele.|  
  
### <a name="sample-template-data"></a>Ukázková data šablony  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 2,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Filtering": {  
        "Pattern": null,  
        "Placeholder": "Search products"  
    },  
    "Products": [  
        {  
            "Id": "56f9445ffaf7560049060001",  
            "Title": "Starter",  
            "Description": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
            "Terms": "",  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        },  
        {  
            "Id": "56f9445ffaf7560049060002",  
            "Title": "Unlimited",  
            "Description": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
            "Terms": null,  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        }  
    ]  
}  
```  
  
##  <a name="Product"></a>Produktu  
 Hello **produktu** šablona vám umožní toocustomize hello textu hello produktu stránky v portálu pro vývojáře hello.  
  
 ![Stránka portálu produktu vývojáře](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")  
  
### <a name="default-template"></a>Výchozí šablony  
  
```xml  
<h2>{{Product.Title}}</h2>  
<p>{{Product.Description}}</p>  
  
{% assign replaceString0 = '{0}' %}  
  
{% if Limits and Limits.size > 0 %}  
<h3>{% localized "ProductDetailsStrings|WebProductsUsageLimitsHeader"%}</h3>  
<ul>  
  {% for limit in Limits %}  
  <li>{{limit.DisplayName}}</li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if apis.size > 0 %}  
<p>  
  <b>  
    {% if apis.size == 1 %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockSingleApisCount" %}{% endcapture %}  
    {% else %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockMultipleApisCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture apisCount %}{{apis.size}}{% endcapture %}  
    {{ apisCountText | replace : replaceString0, apisCount }}  
  </b>  
</p>  
  
<ul>  
  {% for api in Apis %}  
  <li>  
    <a href="/docs/services/{{api.Id}}">{{api.Name}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if subscriptions.size > 0 %}  
<p>  
  <b>  
    {% if subscriptions.size == 1 %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockSingleSubscriptionsCount" %}{% endcapture %}  
    {% else %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockMultipleSubscriptionsCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture subscriptionsCount %}{{subscriptions.size}}{% endcapture %}  
    {{ subscriptionsCountText | replace : replaceString0, subscriptionsCount }}  
  </b>  
</p>  
  
<ul>  
  {% for subscription in subscriptions %}  
  <li>  
    <a href="/developer#{{subscription.Id}}">{{subscription.DisplayName}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
{% if CannotAddBecauseSubscriptionNumberLimitReached %}  
<b>{% localized "ProductDetailsStrings|TextblockSubscriptionLimitReached" %}</b>  
{% elsif CannotAddBecauseMultipleSubscriptionsNotAllowed == false %}  
<subscribe-button></subscribe-button>  
{% endif %}  
```  
  
### <a name="controls"></a>Ovládací prvky  
 Hello `Product list` šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).  
  
-   [přihlášení k odběru tlačítko](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a>Datový model  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Produkt|[Produktu](api-management-template-data-model-reference.md#Product)|Zadaný produkt Hello.|  
|IsDeveloperSubscribed|Logická hodnota|Jestli hello aktuální uživatel je přihlášený toothis produktu.|  
|Hodnotu SubscriptionState|Číslo|Stav Hello hello předplatného. Je možné stavy:<br /><br /> -   `0 - suspended`– hello předplatného je zablokovaná, a hello odběratele nelze volat všechny rozhraní API produktu hello.<br />-   `1 - active`– hello předplatné je aktivní.<br />-   `2 - expired`– hello předplatné dosaženo datum vypršení platnosti a bylo deaktivováno.<br />-   `3 - submitted`– žádosti o odběr hello nebylo provedeno hello developer, ale má ještě schválení nebo odmítnutí.<br />-   `4 - rejected`– hello předplatné požadavek byl odmítnut správcem.<br />-   `5 - cancelled`– hello předplatné zrušil správce nebo vývojáře hello.|  
|Omezení|Pole|Tato vlastnost je zastaralá a by se neměla používat.|  
|DelegatedSubscriptionEnabled|Logická hodnota|Jestli [delegování](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) je pro toto předplatné povolená.|  
|DelegatedSubscriptionUrl|Řetězec|Pokud je povoleno delegování, hello delegována adresu URL odběru.|  
|IsAgreed|Logická hodnota|Pokud produkt hello podmínky, jestli aktuální uživatel hello souhlasila toohello podmínky.|  
|Předplatná|Kolekce [předplatné Souhrn](api-management-template-data-model-reference.md#SubscriptionSummary) entity.|Hello odběry toohello produktu.|  
|Rozhraní API|Kolekce [rozhraní API](api-management-template-data-model-reference.md#API) entity.|Hello rozhraní API v tomto produktu.|  
|CannotAddBecauseSubscriptionNumberLimitReached|Logická hodnota|Jestli hello aktuální uživatel je vhodné toosubscribe toothis produktu s ohledem toohello předplatné limit.|  
|CannotAddBecauseMultipleSubscriptionsNotAllowed|Logická hodnota|Jestli hello aktuální uživatel je vhodné toosubscribe toothis produktu s ohledem na toomultiple odběry nebo není povolené.|  
  
### <a name="sample-template-data"></a>Ukázková data šablony  
  
```json  
{  
    "Product": {  
        "Id": "56f9445ffaf7560049060001",  
        "Title": "Starter",  
        "Description": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
        "Terms": "",  
        "ProductState": 1,  
        "AllowMultipleSubscriptions": false,  
        "MultipleSubscriptionsCount": 1  
    },  
    "IsDeveloperSubscribed": true,  
    "SubscriptionState": 1,  
    "Limits": [],  
    "DelegatedSubscriptionEnabled": false,  
    "DelegatedSubscriptionUrl": null,  
    "IsAgreed": false,  
    "Subscriptions": [  
        {  
            "Id": "56f9445ffaf7560049070001",  
            "DisplayName": "Starter  (default)"  
        }  
    ],  
    "Apis": [  
        {  
            "id": "56f9445ffaf7560049040001",  
            "name": "Echo API",  
            "description": null,  
            "serviceUrl": "http://echoapi.cloudapp.net/api",  
            "path": "echo",  
            "protocols": [  
                2  
            ],  
            "authenticationSettings": null,  
            "subscriptionKeyParameterNames": null  
        }  
    ],  
    "CannotAddBecauseSubscriptionNumberLimitReached": false,  
    "CannotAddBecauseMultipleSubscriptionsNotAllowed": true  
}  
```

## <a name="next-steps"></a>Další kroky
Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).
