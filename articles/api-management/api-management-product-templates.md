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
# <a name="product-templates-in-azure-api-management"></a><span data-ttu-id="abd22-103">Šablony produktu v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="abd22-103">Product templates in Azure API Management</span></span>
<span data-ttu-id="abd22-104">Azure API Management poskytuje že Hello možnost toocustomize hello obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="abd22-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="abd22-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a hello editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a zadané sadu lokalizované [řetězce prostředků](api-management-template-resources.md#strings), [ Prostředky glyfy](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu tooconfigure hello obsah hello stránek podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="abd22-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="abd22-106">Hello šablony v této části Povolit toocustomize hello obsah stránky produktu hello na portál pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="abd22-106">hello templates in this section allow you toocustomize hello content of hello product pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="abd22-107">Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="abd22-107">Product list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="abd22-108">Produktu</span><span class="sxs-lookup"><span data-stu-id="abd22-108">Product</span></span>](#Product)  
  
> [!NOTE]
>  <span data-ttu-id="abd22-109">Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci hello, ale jsou toochange subjektu z důvodu vylepšení toocontinuous.</span><span class="sxs-lookup"><span data-stu-id="abd22-109">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="abd22-110">Hello za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře hello přechodem toohello potřeby jednotlivých šablony.</span><span class="sxs-lookup"><span data-stu-id="abd22-110">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="abd22-111">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="abd22-111">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="abd22-112"><a name="ProductList"></a>Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="abd22-112"><a name="ProductList"></a> Product list</span></span>  
 <span data-ttu-id="abd22-113">Hello **seznam produktů** šablona vám umožní toocustomize hello textu hello produktu seznamu stránky v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="abd22-113">hello **Product list** template allows you toocustomize hello body of hello product list page in hello developer portal.</span></span>  
  
 <span data-ttu-id="abd22-114">![Seznam produktů](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span><span class="sxs-lookup"><span data-stu-id="abd22-114">![Products list](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="abd22-115">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="abd22-115">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="abd22-116">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="abd22-116">Controls</span></span>  
 <span data-ttu-id="abd22-117">Hello `Product list` šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="abd22-117">hello `Product list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="abd22-118">ovládací prvek stránkování</span><span class="sxs-lookup"><span data-stu-id="abd22-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
-   [<span data-ttu-id="abd22-119">ovládací prvek vyhledávání</span><span class="sxs-lookup"><span data-stu-id="abd22-119">search-control</span></span>](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a><span data-ttu-id="abd22-120">Datový model</span><span class="sxs-lookup"><span data-stu-id="abd22-120">Data model</span></span>  
  
|<span data-ttu-id="abd22-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="abd22-121">Property</span></span>|<span data-ttu-id="abd22-122">Typ</span><span class="sxs-lookup"><span data-stu-id="abd22-122">Type</span></span>|<span data-ttu-id="abd22-123">Popis</span><span class="sxs-lookup"><span data-stu-id="abd22-123">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="abd22-124">Stránkování</span><span class="sxs-lookup"><span data-stu-id="abd22-124">Paging</span></span>|<span data-ttu-id="abd22-125">[Stránkování](api-management-template-data-model-reference.md#Paging) entity.</span><span class="sxs-lookup"><span data-stu-id="abd22-125">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="abd22-126">informace o stránkování Hello pro kolekci produkty hello.</span><span class="sxs-lookup"><span data-stu-id="abd22-126">hello paging information for hello products collection.</span></span>|  
|<span data-ttu-id="abd22-127">Filtrování</span><span class="sxs-lookup"><span data-stu-id="abd22-127">Filtering</span></span>|<span data-ttu-id="abd22-128">[Filtrování](api-management-template-data-model-reference.md#Filtering) entity.</span><span class="sxs-lookup"><span data-stu-id="abd22-128">[Filtering](api-management-template-data-model-reference.md#Filtering) entity.</span></span>|<span data-ttu-id="abd22-129">filtrování informace hello produkty seznam stránku Hello.</span><span class="sxs-lookup"><span data-stu-id="abd22-129">hello filtering information for hello products list page.</span></span>|  
|<span data-ttu-id="abd22-130">Produkty</span><span class="sxs-lookup"><span data-stu-id="abd22-130">Products</span></span>|<span data-ttu-id="abd22-131">Kolekce [produktu](api-management-template-data-model-reference.md#Product) entity.</span><span class="sxs-lookup"><span data-stu-id="abd22-131">Collection of [Product](api-management-template-data-model-reference.md#Product) entities.</span></span>|<span data-ttu-id="abd22-132">Hello produkty viditelné toohello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="abd22-132">hello products visible toohello current user.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="abd22-133">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="abd22-133">Sample template data</span></span>  
  
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
  
##  <span data-ttu-id="abd22-134"><a name="Product"></a>Produktu</span><span class="sxs-lookup"><span data-stu-id="abd22-134"><a name="Product"></a> Product</span></span>  
 <span data-ttu-id="abd22-135">Hello **produktu** šablona vám umožní toocustomize hello textu hello produktu stránky v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="abd22-135">hello **Product** template allows you toocustomize hello body of hello product page in hello developer portal.</span></span>  
  
 <span data-ttu-id="abd22-136">![Stránka portálu produktu vývojáře](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span><span class="sxs-lookup"><span data-stu-id="abd22-136">![Developer portal product page](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="abd22-137">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="abd22-137">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="abd22-138">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="abd22-138">Controls</span></span>  
 <span data-ttu-id="abd22-139">Hello `Product list` šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="abd22-139">hello `Product list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="abd22-140">přihlášení k odběru tlačítko</span><span class="sxs-lookup"><span data-stu-id="abd22-140">subscribe-button</span></span>](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a><span data-ttu-id="abd22-141">Datový model</span><span class="sxs-lookup"><span data-stu-id="abd22-141">Data model</span></span>  
  
|<span data-ttu-id="abd22-142">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="abd22-142">Property</span></span>|<span data-ttu-id="abd22-143">Typ</span><span class="sxs-lookup"><span data-stu-id="abd22-143">Type</span></span>|<span data-ttu-id="abd22-144">Popis</span><span class="sxs-lookup"><span data-stu-id="abd22-144">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="abd22-145">Produkt</span><span class="sxs-lookup"><span data-stu-id="abd22-145">Product</span></span>|[<span data-ttu-id="abd22-146">Produktu</span><span class="sxs-lookup"><span data-stu-id="abd22-146">Product</span></span>](api-management-template-data-model-reference.md#Product)|<span data-ttu-id="abd22-147">Zadaný produkt Hello.</span><span class="sxs-lookup"><span data-stu-id="abd22-147">hello specified product.</span></span>|  
|<span data-ttu-id="abd22-148">IsDeveloperSubscribed</span><span class="sxs-lookup"><span data-stu-id="abd22-148">IsDeveloperSubscribed</span></span>|<span data-ttu-id="abd22-149">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="abd22-149">boolean</span></span>|<span data-ttu-id="abd22-150">Jestli hello aktuální uživatel je přihlášený toothis produktu.</span><span class="sxs-lookup"><span data-stu-id="abd22-150">Whether hello current user is subscribed toothis product.</span></span>|  
|<span data-ttu-id="abd22-151">Hodnotu SubscriptionState</span><span class="sxs-lookup"><span data-stu-id="abd22-151">SubscriptionState</span></span>|<span data-ttu-id="abd22-152">Číslo</span><span class="sxs-lookup"><span data-stu-id="abd22-152">number</span></span>|<span data-ttu-id="abd22-153">Stav Hello hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="abd22-153">hello state of hello subscription.</span></span> <span data-ttu-id="abd22-154">Je možné stavy:</span><span class="sxs-lookup"><span data-stu-id="abd22-154">Possible states are:</span></span><br /><br /> <span data-ttu-id="abd22-155">-   `0 - suspended`– hello předplatného je zablokovaná, a hello odběratele nelze volat všechny rozhraní API produktu hello.</span><span class="sxs-lookup"><span data-stu-id="abd22-155">-   `0 - suspended` – hello subscription is blocked, and hello subscriber cannot call any APIs of hello product.</span></span><br /><span data-ttu-id="abd22-156">-   `1 - active`– hello předplatné je aktivní.</span><span class="sxs-lookup"><span data-stu-id="abd22-156">-   `1 - active` – hello subscription is active.</span></span><br /><span data-ttu-id="abd22-157">-   `2 - expired`– hello předplatné dosaženo datum vypršení platnosti a bylo deaktivováno.</span><span class="sxs-lookup"><span data-stu-id="abd22-157">-   `2 - expired` – hello subscription reached its expiration date and was deactivated.</span></span><br /><span data-ttu-id="abd22-158">-   `3 - submitted`– žádosti o odběr hello nebylo provedeno hello developer, ale má ještě schválení nebo odmítnutí.</span><span class="sxs-lookup"><span data-stu-id="abd22-158">-   `3 - submitted` – hello subscription request has been made by hello developer, but has not yet been approved or rejected.</span></span><br /><span data-ttu-id="abd22-159">-   `4 - rejected`– hello předplatné požadavek byl odmítnut správcem.</span><span class="sxs-lookup"><span data-stu-id="abd22-159">-   `4 - rejected` – hello subscription request has been denied by an administrator.</span></span><br /><span data-ttu-id="abd22-160">-   `5 - cancelled`– hello předplatné zrušil správce nebo vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="abd22-160">-   `5 - cancelled` – hello subscription has been cancelled by hello developer or administrator.</span></span>|  
|<span data-ttu-id="abd22-161">Omezení</span><span class="sxs-lookup"><span data-stu-id="abd22-161">Limits</span></span>|<span data-ttu-id="abd22-162">Pole</span><span class="sxs-lookup"><span data-stu-id="abd22-162">array</span></span>|<span data-ttu-id="abd22-163">Tato vlastnost je zastaralá a by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="abd22-163">This property is deprecated and should not be used.</span></span>|  
|<span data-ttu-id="abd22-164">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="abd22-164">DelegatedSubscriptionEnabled</span></span>|<span data-ttu-id="abd22-165">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="abd22-165">boolean</span></span>|<span data-ttu-id="abd22-166">Jestli [delegování](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) je pro toto předplatné povolená.</span><span class="sxs-lookup"><span data-stu-id="abd22-166">Whether [delegation](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) is enabled for this subscription.</span></span>|  
|<span data-ttu-id="abd22-167">DelegatedSubscriptionUrl</span><span class="sxs-lookup"><span data-stu-id="abd22-167">DelegatedSubscriptionUrl</span></span>|<span data-ttu-id="abd22-168">Řetězec</span><span class="sxs-lookup"><span data-stu-id="abd22-168">string</span></span>|<span data-ttu-id="abd22-169">Pokud je povoleno delegování, hello delegována adresu URL odběru.</span><span class="sxs-lookup"><span data-stu-id="abd22-169">If delegation is enabled, hello delegated subscription URL.</span></span>|  
|<span data-ttu-id="abd22-170">IsAgreed</span><span class="sxs-lookup"><span data-stu-id="abd22-170">IsAgreed</span></span>|<span data-ttu-id="abd22-171">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="abd22-171">boolean</span></span>|<span data-ttu-id="abd22-172">Pokud produkt hello podmínky, jestli aktuální uživatel hello souhlasila toohello podmínky.</span><span class="sxs-lookup"><span data-stu-id="abd22-172">If hello product has terms, whether hello current user has agreed toohello terms.</span></span>|  
|<span data-ttu-id="abd22-173">Předplatná</span><span class="sxs-lookup"><span data-stu-id="abd22-173">Subscriptions</span></span>|<span data-ttu-id="abd22-174">Kolekce [předplatné Souhrn](api-management-template-data-model-reference.md#SubscriptionSummary) entity.</span><span class="sxs-lookup"><span data-stu-id="abd22-174">Collection of [Subscription summary](api-management-template-data-model-reference.md#SubscriptionSummary) entities.</span></span>|<span data-ttu-id="abd22-175">Hello odběry toohello produktu.</span><span class="sxs-lookup"><span data-stu-id="abd22-175">hello subscriptions toohello product.</span></span>|  
|<span data-ttu-id="abd22-176">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="abd22-176">Apis</span></span>|<span data-ttu-id="abd22-177">Kolekce [rozhraní API](api-management-template-data-model-reference.md#API) entity.</span><span class="sxs-lookup"><span data-stu-id="abd22-177">Collection of [API](api-management-template-data-model-reference.md#API) entities.</span></span>|<span data-ttu-id="abd22-178">Hello rozhraní API v tomto produktu.</span><span class="sxs-lookup"><span data-stu-id="abd22-178">hello APIs in this product.</span></span>|  
|<span data-ttu-id="abd22-179">CannotAddBecauseSubscriptionNumberLimitReached</span><span class="sxs-lookup"><span data-stu-id="abd22-179">CannotAddBecauseSubscriptionNumberLimitReached</span></span>|<span data-ttu-id="abd22-180">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="abd22-180">boolean</span></span>|<span data-ttu-id="abd22-181">Jestli hello aktuální uživatel je vhodné toosubscribe toothis produktu s ohledem toohello předplatné limit.</span><span class="sxs-lookup"><span data-stu-id="abd22-181">Whether hello current user is eligible toosubscribe toothis product with regard toohello subscription limit.</span></span>|  
|<span data-ttu-id="abd22-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span><span class="sxs-lookup"><span data-stu-id="abd22-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span></span>|<span data-ttu-id="abd22-183">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="abd22-183">boolean</span></span>|<span data-ttu-id="abd22-184">Jestli hello aktuální uživatel je vhodné toosubscribe toothis produktu s ohledem na toomultiple odběry nebo není povolené.</span><span class="sxs-lookup"><span data-stu-id="abd22-184">Whether hello current user is eligible toosubscribe toothis product with regard toomultiple subscriptions being allowed or not.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="abd22-185">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="abd22-185">Sample template data</span></span>  
  
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

## <a name="next-steps"></a><span data-ttu-id="abd22-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abd22-186">Next steps</span></span>
<span data-ttu-id="abd22-187">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="abd22-187">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
