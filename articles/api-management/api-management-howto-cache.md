---
title: "aaaAdd ukládání do mezipaměti tooimprove výkon v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak načíst tooimprove hello latence, šířku pásma a webové služby pro volání služby API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 056ab7cf788218327e30bd5c028b76e3b1977fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-caching-tooimprove-performance-in-azure-api-management"></a><span data-ttu-id="ad15b-103">Přidání ukládání do mezipaměti tooimprove výkon v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="ad15b-103">Add caching tooimprove performance in Azure API Management</span></span>
<span data-ttu-id="ad15b-104">Operace ve službě API Management můžete nakonfigurovat tak, aby odpovědi ukládaly do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ad15b-104">Operations in API Management can be configured for response caching.</span></span> <span data-ttu-id="ad15b-105">Ukládání odpovědí do mezipaměti může v případě dat, která se často nemění, výrazně zlepšit latenci rozhraní API, využití šířky pásma a načítání webových služeb.</span><span class="sxs-lookup"><span data-stu-id="ad15b-105">Response caching can significantly reduce API latency, bandwidth consumption, and web service load for data that does not change frequently.</span></span>

<span data-ttu-id="ad15b-106">Tento průvodce vám ukáže, jak tooadd odpovědi ukládání do mezipaměti pro vaše rozhraní API a nakonfigurovat zásady pro operace rozhraní Echo API ukázkový hello.</span><span class="sxs-lookup"><span data-stu-id="ad15b-106">This guide shows you how tooadd response caching for your API and configure policies for hello sample Echo API operations.</span></span> <span data-ttu-id="ad15b-107">Potom můžete volat operace hello z hello vývojáře portálu tooverify ukládání do mezipaměti v akci.</span><span class="sxs-lookup"><span data-stu-id="ad15b-107">You can then call hello operation from hello developer portal tooverify caching in action.</span></span>

> [!NOTE]
> <span data-ttu-id="ad15b-108">Informace o ukládání položek do mezipaměti podle klíče pomocí výrazů zásad najdete v článku [Vlastní ukládání do mezipaměti ve službě Azure API Management](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="ad15b-108">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="ad15b-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ad15b-109">Prerequisites</span></span>
<span data-ttu-id="ad15b-110">Předtím, než následující hello kroky v tomto průvodci, musíte mít instanci služby API Management s rozhraním API a produkt nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="ad15b-110">Before following hello steps in this guide, you must have an API Management service instance with an API and a product configured.</span></span> <span data-ttu-id="ad15b-111">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="ad15b-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

## <span data-ttu-id="ad15b-112"><a name="configure-caching"></a>Konfigurace operace pro ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ad15b-112"><a name="configure-caching"> </a>Configure an operation for caching</span></span>
<span data-ttu-id="ad15b-113">V tomto kroku zkontrolujete nastavení hello mezipaměti hello **GET Resource (cached)** operaci hello ukázkovém rozhraní Echo API.</span><span class="sxs-lookup"><span data-stu-id="ad15b-113">In this step, you will review hello caching settings of hello **GET Resource (cached)** operation of hello sample Echo API.</span></span>

> [!NOTE]
> <span data-ttu-id="ad15b-114">Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním Echo API, které je možné použít tooexperiment s a další informace o službě API Management.</span><span class="sxs-lookup"><span data-stu-id="ad15b-114">Each API Management service instance comes preconfigured with an Echo API that can be used tooexperiment with and learn about API Management.</span></span> <span data-ttu-id="ad15b-115">Další informace najdete v článku [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="ad15b-115">For more information, see [Get started with Azure API Management][Get started with Azure API Management].</span></span>
> 
> 

<span data-ttu-id="ad15b-116">tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="ad15b-116">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="ad15b-117">Tím přejdete portál vydavatele toohello API Management.</span><span class="sxs-lookup"><span data-stu-id="ad15b-117">This takes you toohello API Management publisher portal.</span></span>

![Portál vydavatele][api-management-management-console]

<span data-ttu-id="ad15b-119">Klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="ad15b-119">Click **APIs** from hello **API Management** menu on hello left, and then click **Echo API**.</span></span>

![Echo API][api-management-echo-api]

<span data-ttu-id="ad15b-121">Klikněte na tlačítko hello **operace** a pak klikněte hello **GET Resource (cached)** operaci provést z hello **Operations** seznamu.</span><span class="sxs-lookup"><span data-stu-id="ad15b-121">Click hello **Operations** tab, and then click hello **GET Resource (cached)** operation from hello **Operations** list.</span></span>

![Operace rozhraní API v programu Echo][api-management-echo-api-operations]

<span data-ttu-id="ad15b-123">Klikněte na tlačítko hello **ukládání do mezipaměti** hello tooview karta ukládání do mezipaměti nastavení pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="ad15b-123">Click hello **Caching** tab tooview hello caching settings for this operation.</span></span>

![Karta Ukládání do mezipaměti][api-management-caching-tab]

<span data-ttu-id="ad15b-125">tooenable ukládání do mezipaměti pro operace, vyberte hello **povolit** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="ad15b-125">tooenable caching for an operation, select hello **Enable** check box.</span></span> <span data-ttu-id="ad15b-126">V tomto příkladu je ukládání do mezipaměti povoleno.</span><span class="sxs-lookup"><span data-stu-id="ad15b-126">In this example, caching is enabled.</span></span>

<span data-ttu-id="ad15b-127">Každá odpověď operace se ukládá do klíčů na základě hodnot hello v hello **podle parametrů řetězce dotazu** a **podle hlaviček** pole.</span><span class="sxs-lookup"><span data-stu-id="ad15b-127">Each operation response is keyed, based on hello values in hello **Vary by query string parameters** and **Vary by headers** fields.</span></span> <span data-ttu-id="ad15b-128">Pokud chcete toocache více odpovědí na základě parametrů řetězce dotazu nebo hlavičky, můžete je nakonfigurovat v těchto dvou polích.</span><span class="sxs-lookup"><span data-stu-id="ad15b-128">If you want toocache multiple responses based on query string parameters or headers, you can configure them in these two fields.</span></span>

<span data-ttu-id="ad15b-129">**Doba trvání** Určuje dobu vypršení platnosti hello odpovědí hello do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ad15b-129">**Duration** specifies hello expiration interval of hello cached responses.</span></span> <span data-ttu-id="ad15b-130">V tomto příkladu je hello interval **3600** sekund, což je ekvivalentní tooone hodina.</span><span class="sxs-lookup"><span data-stu-id="ad15b-130">In this example, hello interval is **3600** seconds, which is equivalent tooone hour.</span></span>

<span data-ttu-id="ad15b-131">Pomocí hello konfigurace v tomto příkladu ukládání do mezipaměti, první požadavek toohello hello **GET Resource (cached)** operace vrátí odpověď z back-end službu hello.</span><span class="sxs-lookup"><span data-stu-id="ad15b-131">Using hello caching configuration in this example, hello first request toohello **GET Resource (cached)** operation returns a response from hello backend service.</span></span> <span data-ttu-id="ad15b-132">Tato odpověď bude do mezipaměti, s klíči hello zadaný hlavičky a dotaz řetězec parametry.</span><span class="sxs-lookup"><span data-stu-id="ad15b-132">This response will be cached, keyed by hello specified headers and query string parameters.</span></span> <span data-ttu-id="ad15b-133">Následující volání operace toohello s odpovídající parametry, budou mít hello do mezipaměti odpovědi vrácené, dokud nevyprší platnost hello mezipaměti Doba trvání intervalu.</span><span class="sxs-lookup"><span data-stu-id="ad15b-133">Subsequent calls toohello operation, with matching parameters, will have hello cached response returned, until hello cache duration interval has expired.</span></span>

## <span data-ttu-id="ad15b-134"><a name="caching-policies"></a>Hello zkontrolujte zásady ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ad15b-134"><a name="caching-policies"> </a>Review hello caching policies</span></span>
<span data-ttu-id="ad15b-135">V tomto kroku zkontrolujte hello ukládání do mezipaměti nastavení pro hello **GET Resource (cached)** operaci hello ukázkovém rozhraní Echo API.</span><span class="sxs-lookup"><span data-stu-id="ad15b-135">In this step, you review hello caching settings for hello **GET Resource (cached)** operation of hello sample Echo API.</span></span>

<span data-ttu-id="ad15b-136">Když jsou nakonfigurované nastavení ukládání do mezipaměti pro operaci v hello **ukládání do mezipaměti** ukládání do mezipaměti pro hello operaci přidány zásady.</span><span class="sxs-lookup"><span data-stu-id="ad15b-136">When caching settings are configured for an operation on hello **Caching** tab, caching policies are added for hello operation.</span></span> <span data-ttu-id="ad15b-137">Tyto zásady můžete zobrazit a upravit v editoru zásad hello.</span><span class="sxs-lookup"><span data-stu-id="ad15b-137">These policies can be viewed and edited in hello policy editor.</span></span>

<span data-ttu-id="ad15b-138">Klikněte na tlačítko **zásady** z hello **API Management** nabídce hello vlevo a pak vyberte **Echo API / GET Resource (cached)** z hello **operace**rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ad15b-138">Click **Policies** from hello **API Management** menu on hello left, and then select **Echo API / GET Resource (cached)** from hello **Operation** drop-down list.</span></span>

![Operace rozsahu zásad][api-management-operation-dropdown]

<span data-ttu-id="ad15b-140">V editoru zásad hello zobrazí hello zásady pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="ad15b-140">This displays hello policies for this operation in hello policy editor.</span></span>

![Editor zásad služby API Management][api-management-policy-editor]

<span data-ttu-id="ad15b-142">Hello definice zásad této operace obsahuje zásady hello, které definují hello konfigurace ukládání do mezipaměti, které jste zkontrolovali pomocí hello **ukládání do mezipaměti** v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="ad15b-142">hello policy definition for this operation includes hello policies that define hello caching configuration that were reviewed using hello **Caching** tab in hello previous step.</span></span>

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
            <vary-by-header>Accept</vary-by-header>
            <vary-by-header>Accept-Charset</vary-by-header>
        </cache-lookup>
        <rewrite-uri template="/resource" />
    </inbound>
    <outbound>
        <base />
        <cache-store caching-mode="cache-on" duration="3600" />
    </outbound>
</policies>
```

> [!NOTE]
> <span data-ttu-id="ad15b-143">Toohello změny provedené v editoru zásad hello zásady ukládání do mezipaměti se projeví na hello **ukládání do mezipaměti** příslušné operace a naopak.</span><span class="sxs-lookup"><span data-stu-id="ad15b-143">Changes made toohello caching policies in hello policy editor will be reflected on hello **Caching** tab of an operation, and vice-versa.</span></span>
> 
> 

## <span data-ttu-id="ad15b-144"><a name="test-operation"></a>Volání operace a testování ukládání do mezipaměti hello</span><span class="sxs-lookup"><span data-stu-id="ad15b-144"><a name="test-operation"> </a>Call an operation and test hello caching</span></span>
<span data-ttu-id="ad15b-145">toosee hello ukládání do mezipaměti, v akci, jsme hello operaci volat z portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="ad15b-145">toosee hello caching in action, we can call hello operation from hello developer portal.</span></span> <span data-ttu-id="ad15b-146">Klikněte na tlačítko **portál pro vývojáře** v pravé horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ad15b-146">Click **Developer portal** in hello top right menu.</span></span>

![Portál pro vývojáře][api-management-developer-portal-menu]

<span data-ttu-id="ad15b-148">Klikněte na tlačítko **rozhraní API** v hello horní nabídce a potom vyberte **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="ad15b-148">Click **APIs** in hello top menu, and then select **Echo API**.</span></span>

![Echo API][api-management-apis-echo-api]

> <span data-ttu-id="ad15b-150">Pokud máte pouze jedno rozhraní API nakonfigurovaný nebo viditelné tooyour účet, klikněte na rozhraní API přejdete přímo toohello operací pro toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ad15b-150">If you have only one API configured or visible tooyour account, then clicking APIs takes you directly toohello operations for that API.</span></span>
> 
> 

<span data-ttu-id="ad15b-151">Vyberte hello **GET Resource (cached)** operace a pak klikněte na tlačítko **otevřít konzolu**.</span><span class="sxs-lookup"><span data-stu-id="ad15b-151">Select hello **GET Resource (cached)** operation, and then click **Open Console**.</span></span>

![Otevření konzoly][api-management-open-console]

<span data-ttu-id="ad15b-153">Hello konzole vám umožní tooinvoke operací přímo z portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="ad15b-153">hello console allows you tooinvoke operations directly from hello developer portal.</span></span>

![Konzola][api-management-console]

<span data-ttu-id="ad15b-155">Ponechte hello výchozí hodnoty pro **param1** a **param2**.</span><span class="sxs-lookup"><span data-stu-id="ad15b-155">Keep hello default values for **param1** and **param2**.</span></span>

<span data-ttu-id="ad15b-156">Vyberte hello požadovaný klíč hello **klíč předplatného** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ad15b-156">Select hello desired key from hello **subscription-key** drop-down list.</span></span> <span data-ttu-id="ad15b-157">Pokud má váš účet jenom jedno předplatné, bude už klíč vybrán.</span><span class="sxs-lookup"><span data-stu-id="ad15b-157">If your account has only one subscription, it will already be selected.</span></span>

<span data-ttu-id="ad15b-158">Zadejte **sampleheader: value1** v hello **hlavičky požadavku** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ad15b-158">Enter **sampleheader:value1** in hello **Request headers** text box.</span></span>

<span data-ttu-id="ad15b-159">Klikněte na tlačítko **HTTP Get** a poznamenejte si hello hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ad15b-159">Click **HTTP Get** and make a note of hello response headers.</span></span>

<span data-ttu-id="ad15b-160">Zadejte **sampleheader: value2** v hello **hlavičky požadavku** textového pole a pak klikněte na tlačítko **HTTP Get**.</span><span class="sxs-lookup"><span data-stu-id="ad15b-160">Enter **sampleheader:value2** in hello **Request headers** text box, and then click **HTTP Get**.</span></span>

<span data-ttu-id="ad15b-161">Všimněte si, že hodnota hello **sampleheader** stále **value1** v odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="ad15b-161">Note that hello value of **sampleheader** is still **value1** in hello response.</span></span> <span data-ttu-id="ad15b-162">Zkuste několik různých hodnot a Všimněte si, že hello odpovědi v mezipaměti z hello první volání se vrátí.</span><span class="sxs-lookup"><span data-stu-id="ad15b-162">Try some different values and note that hello cached response from hello first call is returned.</span></span>

<span data-ttu-id="ad15b-163">Zadejte **25** do hello **param2** pole a pak klikněte na **HTTP Get**.</span><span class="sxs-lookup"><span data-stu-id="ad15b-163">Enter **25** into hello **param2** field, and then click **HTTP Get**.</span></span>

<span data-ttu-id="ad15b-164">Všimněte si, že hodnota hello **sampleheader** hello odpovědi je teď v **value2**.</span><span class="sxs-lookup"><span data-stu-id="ad15b-164">Note that hello value of **sampleheader** in hello response is now **value2**.</span></span> <span data-ttu-id="ad15b-165">Protože výsledky operace hello jsou klíčovány pomocí řetězce dotazu, předchozí odpověď uložená v mezipaměti hello nebyla vrácena.</span><span class="sxs-lookup"><span data-stu-id="ad15b-165">Because hello operation results are keyed by query string, hello previous cached response was not returned.</span></span>

## <span data-ttu-id="ad15b-166"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad15b-166"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="ad15b-167">Další informace o zásadách ukládání do mezipaměti najdete v tématu [zásady ukládání do mezipaměti] [ Caching policies] v hello [zásady API managementu][API Management policy reference].</span><span class="sxs-lookup"><span data-stu-id="ad15b-167">For more information about caching policies, see [Caching policies][Caching policies] in hello [API Management policy reference][API Management policy reference].</span></span>
* <span data-ttu-id="ad15b-168">Informace o ukládání položek do mezipaměti podle klíče pomocí výrazů zásad najdete v článku [Vlastní ukládání do mezipaměti ve službě Azure API Management](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="ad15b-168">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review hello caching policies]: #caching-policies
[Call an operation and test hello caching]: #test-operation
[Next steps]: #next-steps
