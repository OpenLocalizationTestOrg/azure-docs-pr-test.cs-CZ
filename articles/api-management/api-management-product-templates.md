---
title: "Šablony produktu v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit obsah stránky produktu v portálu pro vývojáře Azure API Management."
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
ms.openlocfilehash: 9ddbb9860b437cb3e7334bdf5891f2fba1cffb76
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="product-templates-in-azure-api-management"></a><span data-ttu-id="01ca6-103">Šablony produktu v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="01ca6-103">Product templates in Azure API Management</span></span>
<span data-ttu-id="01ca6-104">Azure API Management poskytuje schopnost přizpůsobit obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="01ca6-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="01ca6-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a lokalizované zadaný sadu [řetězce prostředků](api-management-template-resources.md#strings), [glyfy prostředky](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu při konfiguraci obsahu stránek, podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="01ca6-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="01ca6-106">Šablony v této části umožňují přizpůsobit obsah stránky produktu v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="01ca6-106">The templates in this section allow you to customize the content of the product pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="01ca6-107">Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="01ca6-107">Product list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="01ca6-108">Produktu</span><span class="sxs-lookup"><span data-stu-id="01ca6-108">Product</span></span>](#Product)  
  
> [!NOTE]
>  <span data-ttu-id="01ca6-109">Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci, ale mohou být změněna z důvodu průběžné vylepšení.</span><span class="sxs-lookup"><span data-stu-id="01ca6-109">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="01ca6-110">Za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře přechodem na jednotlivé požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="01ca6-110">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="01ca6-111">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="01ca6-111">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="01ca6-112"><a name="ProductList"></a>Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="01ca6-112"><a name="ProductList"></a> Product list</span></span>  
 <span data-ttu-id="01ca6-113">**Seznam produktů** šablona umožňuje přizpůsobit text stránky seznam produktu v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="01ca6-113">The **Product list** template allows you to customize the body of the product list page in the developer portal.</span></span>  
  
 <span data-ttu-id="01ca6-114">![Seznam produktů](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span><span class="sxs-lookup"><span data-stu-id="01ca6-114">![Products list](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="01ca6-115">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="01ca6-115">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="01ca6-116">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="01ca6-116">Controls</span></span>  
 <span data-ttu-id="01ca6-117">`Product list` Šablona může používat následující [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="01ca6-117">The `Product list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="01ca6-118">ovládací prvek stránkování</span><span class="sxs-lookup"><span data-stu-id="01ca6-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
-   [<span data-ttu-id="01ca6-119">ovládací prvek vyhledávání</span><span class="sxs-lookup"><span data-stu-id="01ca6-119">search-control</span></span>](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a><span data-ttu-id="01ca6-120">Datový model</span><span class="sxs-lookup"><span data-stu-id="01ca6-120">Data model</span></span>  
  
|<span data-ttu-id="01ca6-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="01ca6-121">Property</span></span>|<span data-ttu-id="01ca6-122">Typ</span><span class="sxs-lookup"><span data-stu-id="01ca6-122">Type</span></span>|<span data-ttu-id="01ca6-123">Popis</span><span class="sxs-lookup"><span data-stu-id="01ca6-123">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="01ca6-124">Stránkování</span><span class="sxs-lookup"><span data-stu-id="01ca6-124">Paging</span></span>|<span data-ttu-id="01ca6-125">[Stránkování](api-management-template-data-model-reference.md#Paging) entity.</span><span class="sxs-lookup"><span data-stu-id="01ca6-125">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="01ca6-126">Informace o stránkování pro kolekci produkty.</span><span class="sxs-lookup"><span data-stu-id="01ca6-126">The paging information for the products collection.</span></span>|  
|<span data-ttu-id="01ca6-127">Filtrování</span><span class="sxs-lookup"><span data-stu-id="01ca6-127">Filtering</span></span>|<span data-ttu-id="01ca6-128">[Filtrování](api-management-template-data-model-reference.md#Filtering) entity.</span><span class="sxs-lookup"><span data-stu-id="01ca6-128">[Filtering](api-management-template-data-model-reference.md#Filtering) entity.</span></span>|<span data-ttu-id="01ca6-129">Filtrování informace pro stránku seznam produktů.</span><span class="sxs-lookup"><span data-stu-id="01ca6-129">The filtering information for the products list page.</span></span>|  
|<span data-ttu-id="01ca6-130">Produkty</span><span class="sxs-lookup"><span data-stu-id="01ca6-130">Products</span></span>|<span data-ttu-id="01ca6-131">Kolekce [produktu](api-management-template-data-model-reference.md#Product) entity.</span><span class="sxs-lookup"><span data-stu-id="01ca6-131">Collection of [Product](api-management-template-data-model-reference.md#Product) entities.</span></span>|<span data-ttu-id="01ca6-132">Produkty viditelné pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="01ca6-132">The products visible to the current user.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="01ca6-133">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="01ca6-133">Sample template data</span></span>  
  
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
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "Terms": "",  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        },  
        {  
            "Id": "56f9445ffaf7560049060002",  
            "Title": "Unlimited",  
            "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "Terms": null,  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        }  
    ]  
}  
```  
  
##  <span data-ttu-id="01ca6-134"><a name="Product"></a>Produktu</span><span class="sxs-lookup"><span data-stu-id="01ca6-134"><a name="Product"></a> Product</span></span>  
 <span data-ttu-id="01ca6-135">**Produktu** šablona umožňuje přizpůsobení textu stránky produktu v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="01ca6-135">The **Product** template allows you to customize the body of the product page in the developer portal.</span></span>  
  
 <span data-ttu-id="01ca6-136">![Stránka portálu produktu vývojáře](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span><span class="sxs-lookup"><span data-stu-id="01ca6-136">![Developer portal product page](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="01ca6-137">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="01ca6-137">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="01ca6-138">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="01ca6-138">Controls</span></span>  
 <span data-ttu-id="01ca6-139">`Product list` Šablona může používat následující [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="01ca6-139">The `Product list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="01ca6-140">přihlášení k odběru tlačítko</span><span class="sxs-lookup"><span data-stu-id="01ca6-140">subscribe-button</span></span>](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a><span data-ttu-id="01ca6-141">Datový model</span><span class="sxs-lookup"><span data-stu-id="01ca6-141">Data model</span></span>  
  
|<span data-ttu-id="01ca6-142">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="01ca6-142">Property</span></span>|<span data-ttu-id="01ca6-143">Typ</span><span class="sxs-lookup"><span data-stu-id="01ca6-143">Type</span></span>|<span data-ttu-id="01ca6-144">Popis</span><span class="sxs-lookup"><span data-stu-id="01ca6-144">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="01ca6-145">Produkt</span><span class="sxs-lookup"><span data-stu-id="01ca6-145">Product</span></span>|[<span data-ttu-id="01ca6-146">Produktu</span><span class="sxs-lookup"><span data-stu-id="01ca6-146">Product</span></span>](api-management-template-data-model-reference.md#Product)|<span data-ttu-id="01ca6-147">Zadaný produkt.</span><span class="sxs-lookup"><span data-stu-id="01ca6-147">The specified product.</span></span>|  
|<span data-ttu-id="01ca6-148">IsDeveloperSubscribed</span><span class="sxs-lookup"><span data-stu-id="01ca6-148">IsDeveloperSubscribed</span></span>|<span data-ttu-id="01ca6-149">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="01ca6-149">boolean</span></span>|<span data-ttu-id="01ca6-150">Jestli má aktuální uživatel je přihlášen k tohoto produktu.</span><span class="sxs-lookup"><span data-stu-id="01ca6-150">Whether the current user is subscribed to this product.</span></span>|  
|<span data-ttu-id="01ca6-151">Hodnotu SubscriptionState</span><span class="sxs-lookup"><span data-stu-id="01ca6-151">SubscriptionState</span></span>|<span data-ttu-id="01ca6-152">Číslo</span><span class="sxs-lookup"><span data-stu-id="01ca6-152">number</span></span>|<span data-ttu-id="01ca6-153">Stav odběru.</span><span class="sxs-lookup"><span data-stu-id="01ca6-153">The state of the subscription.</span></span> <span data-ttu-id="01ca6-154">Je možné stavy:</span><span class="sxs-lookup"><span data-stu-id="01ca6-154">Possible states are:</span></span><br /><br /> <span data-ttu-id="01ca6-155">-   `0 - suspended`– předplatného je zablokovaný a odběrateli nelze volat všechny rozhraní API produktu.</span><span class="sxs-lookup"><span data-stu-id="01ca6-155">-   `0 - suspended` – the subscription is blocked, and the subscriber cannot call any APIs of the product.</span></span><br /><span data-ttu-id="01ca6-156">-   `1 - active`– je předplatné aktivní.</span><span class="sxs-lookup"><span data-stu-id="01ca6-156">-   `1 - active` – the subscription is active.</span></span><br /><span data-ttu-id="01ca6-157">-   `2 - expired`– předplatné dosaženo datum vypršení platnosti a bylo deaktivováno.</span><span class="sxs-lookup"><span data-stu-id="01ca6-157">-   `2 - expired` – the subscription reached its expiration date and was deactivated.</span></span><br /><span data-ttu-id="01ca6-158">-   `3 - submitted`– žádosti o odběr byl proveden vývojáře, ale má ještě schválení nebo odmítnutí.</span><span class="sxs-lookup"><span data-stu-id="01ca6-158">-   `3 - submitted` – the subscription request has been made by the developer, but has not yet been approved or rejected.</span></span><br /><span data-ttu-id="01ca6-159">-   `4 - rejected`– žádosti o odběr byl odepřen správcem.</span><span class="sxs-lookup"><span data-stu-id="01ca6-159">-   `4 - rejected` – the subscription request has been denied by an administrator.</span></span><br /><span data-ttu-id="01ca6-160">-   `5 - cancelled`– předplatné zrušil vývojáře nebo správce.</span><span class="sxs-lookup"><span data-stu-id="01ca6-160">-   `5 - cancelled` – the subscription has been cancelled by the developer or administrator.</span></span>|  
|<span data-ttu-id="01ca6-161">Omezení</span><span class="sxs-lookup"><span data-stu-id="01ca6-161">Limits</span></span>|<span data-ttu-id="01ca6-162">Pole</span><span class="sxs-lookup"><span data-stu-id="01ca6-162">array</span></span>|<span data-ttu-id="01ca6-163">Tato vlastnost je zastaralá a by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="01ca6-163">This property is deprecated and should not be used.</span></span>|  
|<span data-ttu-id="01ca6-164">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="01ca6-164">DelegatedSubscriptionEnabled</span></span>|<span data-ttu-id="01ca6-165">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="01ca6-165">boolean</span></span>|<span data-ttu-id="01ca6-166">Jestli [delegování](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) je pro toto předplatné povolená.</span><span class="sxs-lookup"><span data-stu-id="01ca6-166">Whether [delegation](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) is enabled for this subscription.</span></span>|  
|<span data-ttu-id="01ca6-167">DelegatedSubscriptionUrl</span><span class="sxs-lookup"><span data-stu-id="01ca6-167">DelegatedSubscriptionUrl</span></span>|<span data-ttu-id="01ca6-168">Řetězec</span><span class="sxs-lookup"><span data-stu-id="01ca6-168">string</span></span>|<span data-ttu-id="01ca6-169">Pokud je povoleno delegování, adresu URL delegované předplatného.</span><span class="sxs-lookup"><span data-stu-id="01ca6-169">If delegation is enabled, the delegated subscription URL.</span></span>|  
|<span data-ttu-id="01ca6-170">IsAgreed</span><span class="sxs-lookup"><span data-stu-id="01ca6-170">IsAgreed</span></span>|<span data-ttu-id="01ca6-171">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="01ca6-171">boolean</span></span>|<span data-ttu-id="01ca6-172">Pokud produkt má podmínky, zda má aktuální uživatel souhlas s podmínkami.</span><span class="sxs-lookup"><span data-stu-id="01ca6-172">If the product has terms, whether the current user has agreed to the terms.</span></span>|  
|<span data-ttu-id="01ca6-173">Předplatná</span><span class="sxs-lookup"><span data-stu-id="01ca6-173">Subscriptions</span></span>|<span data-ttu-id="01ca6-174">Kolekce [předplatné Souhrn](api-management-template-data-model-reference.md#SubscriptionSummary) entity.</span><span class="sxs-lookup"><span data-stu-id="01ca6-174">Collection of [Subscription summary](api-management-template-data-model-reference.md#SubscriptionSummary) entities.</span></span>|<span data-ttu-id="01ca6-175">Odběry produktu.</span><span class="sxs-lookup"><span data-stu-id="01ca6-175">The subscriptions to the product.</span></span>|  
|<span data-ttu-id="01ca6-176">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="01ca6-176">Apis</span></span>|<span data-ttu-id="01ca6-177">Kolekce [rozhraní API](api-management-template-data-model-reference.md#API) entity.</span><span class="sxs-lookup"><span data-stu-id="01ca6-177">Collection of [API](api-management-template-data-model-reference.md#API) entities.</span></span>|<span data-ttu-id="01ca6-178">Rozhraní API v tomto produktu.</span><span class="sxs-lookup"><span data-stu-id="01ca6-178">The APIs in this product.</span></span>|  
|<span data-ttu-id="01ca6-179">CannotAddBecauseSubscriptionNumberLimitReached</span><span class="sxs-lookup"><span data-stu-id="01ca6-179">CannotAddBecauseSubscriptionNumberLimitReached</span></span>|<span data-ttu-id="01ca6-180">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="01ca6-180">boolean</span></span>|<span data-ttu-id="01ca6-181">Jestli má aktuální uživatel nemá oprávnění k odběru tohoto produktu s ohledem na limitu předplatného.</span><span class="sxs-lookup"><span data-stu-id="01ca6-181">Whether the current user is eligible to subscribe to this product with regard to the subscription limit.</span></span>|  
|<span data-ttu-id="01ca6-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span><span class="sxs-lookup"><span data-stu-id="01ca6-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span></span>|<span data-ttu-id="01ca6-183">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="01ca6-183">boolean</span></span>|<span data-ttu-id="01ca6-184">Jestli má aktuální uživatel nemá oprávnění k odběru tohoto produktu s ohledem na více předplatných, nebo není povolené.</span><span class="sxs-lookup"><span data-stu-id="01ca6-184">Whether the current user is eligible to subscribe to this product with regard to multiple subscriptions being allowed or not.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="01ca6-185">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="01ca6-185">Sample template data</span></span>  
  
```json  
{  
    "Product": {  
        "Id": "56f9445ffaf7560049060001",  
        "Title": "Starter",  
        "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
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

## <a name="next-steps"></a><span data-ttu-id="01ca6-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01ca6-186">Next steps</span></span>
<span data-ttu-id="01ca6-187">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="01ca6-187">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>