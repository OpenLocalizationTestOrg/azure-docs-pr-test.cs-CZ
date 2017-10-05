---
title: "Přidání ukládání do mezipaměti ke zlepšení výkonu služby Azure API Management | Dokumentace Microsoftu"
description: "Naučte se zlepšit latenci, spotřebu šířky pásma a načítání webových služeb při volání služby API Management."
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
ms.openlocfilehash: 59c595f0d5ce849f44c46fdb6cab0b44d35fffa0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-caching-to-improve-performance-in-azure-api-management"></a><span data-ttu-id="e0c22-103">Přidání ukládání do mezipaměti ke zlepšení výkonu služby Azure API Management</span><span class="sxs-lookup"><span data-stu-id="e0c22-103">Add caching to improve performance in Azure API Management</span></span>
<span data-ttu-id="e0c22-104">Operace ve službě API Management můžete nakonfigurovat tak, aby odpovědi ukládaly do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e0c22-104">Operations in API Management can be configured for response caching.</span></span> <span data-ttu-id="e0c22-105">Ukládání odpovědí do mezipaměti může v případě dat, která se často nemění, výrazně zlepšit latenci rozhraní API, využití šířky pásma a načítání webových služeb.</span><span class="sxs-lookup"><span data-stu-id="e0c22-105">Response caching can significantly reduce API latency, bandwidth consumption, and web service load for data that does not change frequently.</span></span>

<span data-ttu-id="e0c22-106">Tento průvodce vám ukáže, jak přidat ukládání odpovědí do mezipaměti pro rozhraní API a jak nakonfigurovat zásady pro operace ukázkového rozhraní Echo API.</span><span class="sxs-lookup"><span data-stu-id="e0c22-106">This guide shows you how to add response caching for your API and configure policies for the sample Echo API operations.</span></span> <span data-ttu-id="e0c22-107">Potom můžete operaci volat z portálu pro vývojáře a ověřit ukládání do mezipaměti v akci.</span><span class="sxs-lookup"><span data-stu-id="e0c22-107">You can then call the operation from the developer portal to verify caching in action.</span></span>

> [!NOTE]
> <span data-ttu-id="e0c22-108">Informace o ukládání položek do mezipaměti podle klíče pomocí výrazů zásad najdete v článku [Vlastní ukládání do mezipaměti ve službě Azure API Management](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="e0c22-108">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="e0c22-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e0c22-109">Prerequisites</span></span>
<span data-ttu-id="e0c22-110">Než začnete provádět kroky podle této příručky, musíte mít instanci služby API Management s nakonfigurovaným rozhraním API a produktem.</span><span class="sxs-lookup"><span data-stu-id="e0c22-110">Before following the steps in this guide, you must have an API Management service instance with an API and a product configured.</span></span> <span data-ttu-id="e0c22-111">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="e0c22-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

## <span data-ttu-id="e0c22-112"><a name="configure-caching"> </a>Konfigurace operace pro ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e0c22-112"><a name="configure-caching"> </a>Configure an operation for caching</span></span>
<span data-ttu-id="e0c22-113">V tomto kroku zkontrolujete nastavení ukládání do mezipaměti operace **GET Resource (cached)** v ukázkovém rozhraní Echo API.</span><span class="sxs-lookup"><span data-stu-id="e0c22-113">In this step, you will review the caching settings of the **GET Resource (cached)** operation of the sample Echo API.</span></span>

> [!NOTE]
> <span data-ttu-id="e0c22-114">Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním Echo API, které můžete použít k experimentování a seznámení se službou API Management.</span><span class="sxs-lookup"><span data-stu-id="e0c22-114">Each API Management service instance comes preconfigured with an Echo API that can be used to experiment with and learn about API Management.</span></span> <span data-ttu-id="e0c22-115">Další informace najdete v článku [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="e0c22-115">For more information, see [Get started with Azure API Management][Get started with Azure API Management].</span></span>
> 
> 

<span data-ttu-id="e0c22-116">Začněte tak, že na webu Azure Portal dané služby API Management kliknete na **Portál vydavatele**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-116">To get started, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="e0c22-117">Tím přejdete na portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="e0c22-117">This takes you to the API Management publisher portal.</span></span>

![Portál vydavatele][api-management-management-console]

<span data-ttu-id="e0c22-119">Klikněte v nabídce **API Management** na levé straně na **Rozhraní API** a potom na **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-119">Click **APIs** from the **API Management** menu on the left, and then click **Echo API**.</span></span>

![Echo API][api-management-echo-api]

<span data-ttu-id="e0c22-121">Klikněte na kartu **Operace** a potom v seznamu **Operace** klikněte na **GET Resource (cached)**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-121">Click the **Operations** tab, and then click the **GET Resource (cached)** operation from the **Operations** list.</span></span>

![Operace rozhraní API v programu Echo][api-management-echo-api-operations]

<span data-ttu-id="e0c22-123">Pokud chcete zobrazit nastavení ukládání do mezipaměti, klikněte na kartu **Ukládání do mezipaměti**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-123">Click the **Caching** tab to view the caching settings for this operation.</span></span>

![Karta Ukládání do mezipaměti][api-management-caching-tab]

<span data-ttu-id="e0c22-125">Pokud chcete povolit ukládání operace do mezipaměti, zaškrtněte políčko **Povolit**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-125">To enable caching for an operation, select the **Enable** check box.</span></span> <span data-ttu-id="e0c22-126">V tomto příkladu je ukládání do mezipaměti povoleno.</span><span class="sxs-lookup"><span data-stu-id="e0c22-126">In this example, caching is enabled.</span></span>

<span data-ttu-id="e0c22-127">Každá odpověď operace se ukládá do klíčů na základě hodnot v polích **Podle parametrů řetězce dotazu** a **Podle hlaviček**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-127">Each operation response is keyed, based on the values in the **Vary by query string parameters** and **Vary by headers** fields.</span></span> <span data-ttu-id="e0c22-128">Pokud chcete do mezipaměti ukládat více odpovědí na základě parametrů řetězce dotazu nebo na základě hlaviček, můžete to konfigurovat v těchto dvou polích.</span><span class="sxs-lookup"><span data-stu-id="e0c22-128">If you want to cache multiple responses based on query string parameters or headers, you can configure them in these two fields.</span></span>

<span data-ttu-id="e0c22-129">**Doba trvání** určuje dobu vypršení uložení odpovědí v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e0c22-129">**Duration** specifies the expiration interval of the cached responses.</span></span> <span data-ttu-id="e0c22-130">V tomto příkladu je interval **3600** sekund, což odpovídá jedné hodině.</span><span class="sxs-lookup"><span data-stu-id="e0c22-130">In this example, the interval is **3600** seconds, which is equivalent to one hour.</span></span>

<span data-ttu-id="e0c22-131">Když v tomto příkladu použijeme konfiguraci ukládání do mezipaměti, první požadavek na operaci **GET Resource (cached)** vrátí odpověď z back-endové služby.</span><span class="sxs-lookup"><span data-stu-id="e0c22-131">Using the caching configuration in this example, the first request to the **GET Resource (cached)** operation returns a response from the backend service.</span></span> <span data-ttu-id="e0c22-132">Tato odpověď se uloží do mezipaměti, kam bude zadaná podle určených hlaviček a parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="e0c22-132">This response will be cached, keyed by the specified headers and query string parameters.</span></span> <span data-ttu-id="e0c22-133">Následující volání operace (s odpovídající parametry) bude vracet odpověď uloženou v mezipaměti až do okamžiku vypršení doby uložení v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e0c22-133">Subsequent calls to the operation, with matching parameters, will have the cached response returned, until the cache duration interval has expired.</span></span>

## <span data-ttu-id="e0c22-134"><a name="caching-policies"> </a>Kontrola zásad ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e0c22-134"><a name="caching-policies"> </a>Review the caching policies</span></span>
<span data-ttu-id="e0c22-135">V tomto kroku zkontrolujete nastavení ukládání do mezipaměti operace **GET Resource (cached)** v ukázkovém rozhraní Echo API.</span><span class="sxs-lookup"><span data-stu-id="e0c22-135">In this step, you review the caching settings for the **GET Resource (cached)** operation of the sample Echo API.</span></span>

<span data-ttu-id="e0c22-136">Pokud jsou na kartě **Ukládání do mezipaměti** nakonfigurovaná nastavení pro ukládání operace do mezipaměti, budou k operaci přidány zásady ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e0c22-136">When caching settings are configured for an operation on the **Caching** tab, caching policies are added for the operation.</span></span> <span data-ttu-id="e0c22-137">Tyto zásady můžete zobrazit a upravit v editoru zásad.</span><span class="sxs-lookup"><span data-stu-id="e0c22-137">These policies can be viewed and edited in the policy editor.</span></span>

<span data-ttu-id="e0c22-138">V nabídce **API Management** na levé straně klikněte na **Zásady** a potom v rozevíracím seznamu **Operace** vyberte **Echo API / GET Resource (cached)**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-138">Click **Policies** from the **API Management** menu on the left, and then select **Echo API / GET Resource (cached)** from the **Operation** drop-down list.</span></span>

![Operace rozsahu zásad][api-management-operation-dropdown]

<span data-ttu-id="e0c22-140">Zobrazí zásady této operace v editoru zásad.</span><span class="sxs-lookup"><span data-stu-id="e0c22-140">This displays the policies for this operation in the policy editor.</span></span>

![Editor zásad služby API Management][api-management-policy-editor]

<span data-ttu-id="e0c22-142">Definice zásad této operace obsahuje zásady, které definují konfiguraci ukládání do mezipaměti, které jste zkontrolovali pomocí karty **Ukládání do mezipaměti** v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="e0c22-142">The policy definition for this operation includes the policies that define the caching configuration that were reviewed using the **Caching** tab in the previous step.</span></span>

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
> <span data-ttu-id="e0c22-143">Změny zásad ukládání do mezipaměti provedené v editoru zásad se projeví na kartě **Ukládání do mezipaměti** příslušné operace a naopak.</span><span class="sxs-lookup"><span data-stu-id="e0c22-143">Changes made to the caching policies in the policy editor will be reflected on the **Caching** tab of an operation, and vice-versa.</span></span>
> 
> 

## <span data-ttu-id="e0c22-144"><a name="test-operation"> </a>Volání operace a testování ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e0c22-144"><a name="test-operation"> </a>Call an operation and test the caching</span></span>
<span data-ttu-id="e0c22-145">Abyste viděli ukládání do mezipaměti v akci, můžete operaci volat z portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="e0c22-145">To see the caching in action, we can call the operation from the developer portal.</span></span> <span data-ttu-id="e0c22-146">Klikněte na **Portál pro vývojáře** v pravé horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="e0c22-146">Click **Developer portal** in the top right menu.</span></span>

![portálu pro vývojáře][api-management-developer-portal-menu]

<span data-ttu-id="e0c22-148">Klikněte na **Rozhraní API** v horní nabídce a potom vyberte **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-148">Click **APIs** in the top menu, and then select **Echo API**.</span></span>

![Echo API][api-management-apis-echo-api]

> <span data-ttu-id="e0c22-150">Pokud máte nakonfigurované jenom jedno rozhraní API nebo váš účet vidí jenom jedno, můžete kliknutím na rozhraní API přejít přímo na operace tohoto rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e0c22-150">If you have only one API configured or visible to your account, then clicking APIs takes you directly to the operations for that API.</span></span>
> 
> 

<span data-ttu-id="e0c22-151">Vyberte operaci **GET Resource (cached)** a potom klikněte na **Otevřít konzolu**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-151">Select the **GET Resource (cached)** operation, and then click **Open Console**.</span></span>

![Otevření konzoly][api-management-open-console]

<span data-ttu-id="e0c22-153">Konzola umožňuje vyvolání operací přímo z portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="e0c22-153">The console allows you to invoke operations directly from the developer portal.</span></span>

![Konzola][api-management-console]

<span data-ttu-id="e0c22-155">Ponechte výchozí hodnoty pro **param1** a **param2**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-155">Keep the default values for **param1** and **param2**.</span></span>

<span data-ttu-id="e0c22-156">V rozevíracím seznamu **subscription-key** vyberte požadovaný klíč.</span><span class="sxs-lookup"><span data-stu-id="e0c22-156">Select the desired key from the **subscription-key** drop-down list.</span></span> <span data-ttu-id="e0c22-157">Pokud má váš účet jenom jedno předplatné, bude už klíč vybrán.</span><span class="sxs-lookup"><span data-stu-id="e0c22-157">If your account has only one subscription, it will already be selected.</span></span>

<span data-ttu-id="e0c22-158">Do textového pole **Hlavičky požadavků** zadejte text **sampleheader:value1**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-158">Enter **sampleheader:value1** in the **Request headers** text box.</span></span>

<span data-ttu-id="e0c22-159">Klikněte na **HTTP Get** a poznamenejte si hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e0c22-159">Click **HTTP Get** and make a note of the response headers.</span></span>

<span data-ttu-id="e0c22-160">Do textového pole **Hlavičky požadavků** zadejte text **sampleheader:value2** a potom klikněte na **HTTP Get**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-160">Enter **sampleheader:value2** in the **Request headers** text box, and then click **HTTP Get**.</span></span>

<span data-ttu-id="e0c22-161">Všimněte si, že hodnota **sampleheader** zůstává v odpovědi **value1**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-161">Note that the value of **sampleheader** is still **value1** in the response.</span></span> <span data-ttu-id="e0c22-162">Zkuste několik různých hodnot a všimněte si, že se vrátí odpověď uložená v mezipaměti, která pochází z prvního volání.</span><span class="sxs-lookup"><span data-stu-id="e0c22-162">Try some different values and note that the cached response from the first call is returned.</span></span>

<span data-ttu-id="e0c22-163">Do pole **param2** zadejte hodnotu **25** a potom klikněte na **HTTP Get**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-163">Enter **25** into the **param2** field, and then click **HTTP Get**.</span></span>

<span data-ttu-id="e0c22-164">Všimněte si, že hodnota **sampleheader** je teď v odpovědi **value2**.</span><span class="sxs-lookup"><span data-stu-id="e0c22-164">Note that the value of **sampleheader** in the response is now **value2**.</span></span> <span data-ttu-id="e0c22-165">Protože výsledky operace se ukládají do klíčů pomocí řetězce dotazu, předchozí odpověď uložená v mezipaměti se ne vrátila.</span><span class="sxs-lookup"><span data-stu-id="e0c22-165">Because the operation results are keyed by query string, the previous cached response was not returned.</span></span>

## <span data-ttu-id="e0c22-166"><a name="next-steps"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0c22-166"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="e0c22-167">Další informace o zásadách ukládání do mezipaměti najdete v části [Zásady ukládání do mezipaměti][Caching policies] v článku [Zásady API managementu][API Management policy reference].</span><span class="sxs-lookup"><span data-stu-id="e0c22-167">For more information about caching policies, see [Caching policies][Caching policies] in the [API Management policy reference][API Management policy reference].</span></span>
* <span data-ttu-id="e0c22-168">Informace o ukládání položek do mezipaměti podle klíče pomocí výrazů zásad najdete v článku [Vlastní ukládání do mezipaměti ve službě Azure API Management](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="e0c22-168">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>

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


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
