---
title: "Ochrana rozhraní API ve službě Azure API Management | Dokumentace Microsoftu"
description: "Seznamte se s možnostmi ochrany rozhraní API pomocí zásad kvót a zásad omezování četnosti."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 450dc368-d005-401d-ae64-3e1a2229b12f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 5553bcb8f9fd38630f694151dc644a684266387c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a><span data-ttu-id="aa13b-103">Ochrana rozhraní API omezením četnosti pomocí Azure API Management</span><span class="sxs-lookup"><span data-stu-id="aa13b-103">Protect your API with rate limits using Azure API Management</span></span>
<span data-ttu-id="aa13b-104">Tento průvodce vám ukáže, jak snadno můžete pomocí služby Azure API Management přidat ochranu rozhraní API vašeho back-endu tím, že nakonfigurujete zásady omezení četnosti a zásady kvót.</span><span class="sxs-lookup"><span data-stu-id="aa13b-104">This guide shows you how easy it is to add protection for your backend API by configuring rate limit and quota policies with Azure API Management.</span></span>

<span data-ttu-id="aa13b-105">V tomto kurzu vytvoříte „bezplatnou zkušební verzi“ produktu s rozhraním API, která vývojářům umožní provádět až 10 volání za minutu a až 200 volání za týden do vašeho rozhraní API pomocí zásad [Omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [Nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota).</span><span class="sxs-lookup"><span data-stu-id="aa13b-105">In this tutorial, you will create a "Free Trial" API product that allows developers to make up to 10 calls per minute and up to a maximum of 200 calls per week to your API using the [Limit call rate per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) policies.</span></span> <span data-ttu-id="aa13b-106">Potom rozhraní API publikujete a zásady omezení četnosti otestujete.</span><span class="sxs-lookup"><span data-stu-id="aa13b-106">You will then publish the API and test the rate limit policy.</span></span>

<span data-ttu-id="aa13b-107">Pokud se zajímáte o pokročilejší scénáře omezování pomocí zásad [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey), podívejte se na článek [Pokročilé omezování požadavků pomocí Azure API Management](api-management-sample-flexible-throttling.md).</span><span class="sxs-lookup"><span data-stu-id="aa13b-107">For more advanced throttling scenarios using the [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies, see [Advanced request throttling with Azure API Management](api-management-sample-flexible-throttling.md).</span></span>

## <span data-ttu-id="aa13b-108"><a name="create-product"> </a>Vytvoření produktu</span><span class="sxs-lookup"><span data-stu-id="aa13b-108"><a name="create-product"> </a>To create a product</span></span>
<span data-ttu-id="aa13b-109">V tomto kroku vytvoříte bezplatnou zkušební verzi produktu, který nevyžaduje schválení předplatného.</span><span class="sxs-lookup"><span data-stu-id="aa13b-109">In this step, you will create a Free Trial product that does not require subscription approval.</span></span>

> [!NOTE]
> <span data-ttu-id="aa13b-110">Pokud už máte produkt nakonfigurovaný a chcete ho v tomto kurzu použít, můžete přeskočit na článek [Konfigurace zásad omezení četnosti a zásad kvót][Configure call rate limit and quota policies] a postupovat v kurzu odtamtud se svým produktem místo bezplatné zkušební verze produktu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-110">If you already have a product configured and want to use it for this tutorial, you can jump ahead to [Configure call rate limit and quota policies][Configure call rate limit and quota policies] and follow the tutorial from there using your product in place of the Free Trial product.</span></span>
> 
> 

<span data-ttu-id="aa13b-111">Začněte tak, že na webu Azure Portal dané služby API Management kliknete na **Portál vydavatele**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-111">To get started, click **Publisher portal** in the Azure Portal for your API Management service.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="aa13b-113">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Správa vašeho prvního rozhraní API v Azure API Management][Manage your first API in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="aa13b-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Manage your first API in Azure API Management][Manage your first API in Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="aa13b-114">V nabídce **API Management** na levé straně klikněte na **Produkty** a zobrazte stránku **Produkty**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-114">Click **Products** in the **API Management** menu on the left to display the **Products** page.</span></span>

![Přidání produktu][api-management-add-product]

<span data-ttu-id="aa13b-116">Kliknutím na **Přidat produkt** zobrazíte dialogové okno **Přidání nového produktu**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-116">Click **Add product** to display the **Add new product** dialog box.</span></span>

![Přidání nového produktu][api-management-new-product-window]

<span data-ttu-id="aa13b-118">Do pole **Nadpis** zadejte text **Bezplatná zkušební verze**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-118">In the **Title** box, type **Free Trial**.</span></span>

<span data-ttu-id="aa13b-119">Do pole **Popis** zadejte následující text: **Předplatitelé můžou spustit 10 volání za minutu až do maximálního počtu 200 volání za týden. Potom bude přístup odepřen.**</span><span class="sxs-lookup"><span data-stu-id="aa13b-119">In the **Description** box, type the following text: **Subscribers will be able to run 10 calls/minute up to a maximum of 200 calls/week after which access is denied.**</span></span>

<span data-ttu-id="aa13b-120">Produkty ve službě API Management můžou být chráněné nebo otevřené.</span><span class="sxs-lookup"><span data-stu-id="aa13b-120">Products in API Management can be protected or open.</span></span> <span data-ttu-id="aa13b-121">V případě chráněných produktů se musíte nejdřív přihlásit k jejich odběru a až potom je můžete používat.</span><span class="sxs-lookup"><span data-stu-id="aa13b-121">Protected products must be subscribed to before they can be used.</span></span> <span data-ttu-id="aa13b-122">Otevřené produkty můžete používat bez předplatného.</span><span class="sxs-lookup"><span data-stu-id="aa13b-122">Open products can be used without a subscription.</span></span> <span data-ttu-id="aa13b-123">Pokud chcete vytvořit chráněný produkt, který vyžaduje předplatné, nezapomeňte vybrat možnost **Vyžadovat předplatné**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-123">Ensure that **Require subscription** is selected to create a protected product that requires a subscription.</span></span> <span data-ttu-id="aa13b-124">Toto je výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="aa13b-124">This is the default setting.</span></span>

<span data-ttu-id="aa13b-125">Pokud chcete, aby pokusy o přihlášení k odběru produktu kontroloval a následně přijímal nebo odmítal správce, vyberte možnost **Vyžadovat schválení předplatného**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-125">If you want an administrator to review and accept or reject subscription attempts to this product, select **Require subscription approval**.</span></span> <span data-ttu-id="aa13b-126">Pokud necháte políčko nezaškrtnuté, pokusy o přihlášení k odběru budou schvalovány automaticky.</span><span class="sxs-lookup"><span data-stu-id="aa13b-126">If the check box is not selected, subscription attempts will be auto-approved.</span></span> <span data-ttu-id="aa13b-127">V tomto příkladu se předplatné schvaluje automaticky, proto políčko nezaškrtávejte.</span><span class="sxs-lookup"><span data-stu-id="aa13b-127">In this example, subscriptions are automatically approved, so do not select the box.</span></span>

<span data-ttu-id="aa13b-128">Pokud chcete vývojářským účtům povolit přihlášení k vícenásobným odběrům nového produktu, zaškrtněte políčko **Povolit více souběžných předplatných**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-128">To allow developer accounts to subscribe multiple times to the new product, select the **Allow multiple simultaneous subscriptions** check box.</span></span> <span data-ttu-id="aa13b-129">Tento kurz několik souběžných předplatných nevyužívá, takže políčko nechte nezaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="aa13b-129">This tutorial does not utilize multiple simultaneous subscriptions, so leave it unchecked.</span></span>

<span data-ttu-id="aa13b-130">Po zadání všech hodnot klikněte na **Uložit** a vytvořte produkt.</span><span class="sxs-lookup"><span data-stu-id="aa13b-130">After all values are entered, click **Save** to create the product.</span></span>

![Produkt přidán][api-management-product-added]

<span data-ttu-id="aa13b-132">Ve výchozím nastavení jsou nové produkty viditelné pro uživatele ve skupině **Správci**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-132">By default, new products are visible to users in the **Administrators** group.</span></span> <span data-ttu-id="aa13b-133">Teď přidáme skupinu **Vývojáři**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-133">We are going to add the **Developers** group.</span></span> <span data-ttu-id="aa13b-134">Klikněte na **Bezplatná zkušební verze** a potom na kartu **Viditelnost**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-134">Click **Free Trial**, and then click the **Visibility** tab.</span></span>

> <span data-ttu-id="aa13b-135">Ve službě API Management se ke správě viditelnosti produktů pro vývojáře používají skupiny.</span><span class="sxs-lookup"><span data-stu-id="aa13b-135">In API Management, groups are used to manage the visibility of products to developers.</span></span> <span data-ttu-id="aa13b-136">Produkty udělují viditelnost skupinám a vývojáři můžou zobrazovat a odebírat produkty, které jsou viditelné pro skupinu, do které patří.</span><span class="sxs-lookup"><span data-stu-id="aa13b-136">Products grant visibility to groups, and developers can view and subscribe to the products that are visible to the groups in which they belong.</span></span> <span data-ttu-id="aa13b-137">Další informace najdete v článku [Vytvoření a používání skupin v Azure API Management][How to create and use groups in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="aa13b-137">For more information, see [How to create and use groups in Azure API Management][How to create and use groups in Azure API Management].</span></span>
> 
> 

![Přidání skupiny vývojářů][api-management-add-developers-group]

<span data-ttu-id="aa13b-139">Zaškrtněte políčko **Vývojáři** a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-139">Select the **Developers** check box, and then click **Save**.</span></span>

## <span data-ttu-id="aa13b-140"><a name="add-api"> </a>Přidání rozhraní API do produktu</span><span class="sxs-lookup"><span data-stu-id="aa13b-140"><a name="add-api"> </a>To add an API to the product</span></span>
<span data-ttu-id="aa13b-141">V tomto kroku kurzu přidáme rozhraní API v programu Echo do nového produktu v bezplatné zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="aa13b-141">In this step of the tutorial, we will add the Echo API to the new Free Trial product.</span></span>

> <span data-ttu-id="aa13b-142">Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním API programu Echo, které můžete použít k experimentování a seznámení se službou API Management.</span><span class="sxs-lookup"><span data-stu-id="aa13b-142">Each API Management service instance comes pre-configured with an Echo API that can be used to experiment with and learn about API Management.</span></span> <span data-ttu-id="aa13b-143">Další informace najdete v článku [Správa vašeho prvního rozhraní API ve službě Azure API Management][Manage your first API in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="aa13b-143">For more information, see [Manage your first API in Azure API Management][Manage your first API in Azure API Management].</span></span>
> 
> 

<span data-ttu-id="aa13b-144">V nabídce **API Management** na levé straně klikněte na **Produkty**, potom na **Bezplatná zkušební verze** a nakonfigurujte produkt.</span><span class="sxs-lookup"><span data-stu-id="aa13b-144">Click **Products** from the **API Management** menu on the left, and then click **Free Trial** to configure the product.</span></span>

![Konfigurace produktu][api-management-configure-product]

<span data-ttu-id="aa13b-146">Klikněte na **Přidat rozhraní API do produktu**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-146">Click **Add API to product**.</span></span>

![Přidání rozhraní API do produktu][api-management-add-api]

<span data-ttu-id="aa13b-148">Vyberte **Rozhraní API v programu Echo** a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-148">Select **Echo API**, and then click **Save**.</span></span>

![Přidání rozhraní API v programu Echo][api-management-add-echo-api]

## <span data-ttu-id="aa13b-150"><a name="policies"> </a>Konfigurace zásad kvót a zásad omezení četnosti volání</span><span class="sxs-lookup"><span data-stu-id="aa13b-150"><a name="policies"> </a>To configure call rate limit and quota policies</span></span>
<span data-ttu-id="aa13b-151">Omezení četnosti a kvóty se konfigurují v editoru zásad.</span><span class="sxs-lookup"><span data-stu-id="aa13b-151">Rate limits and quotas are configured in the policy editor.</span></span> <span data-ttu-id="aa13b-152">Dvě zásady, které v tomto kurzu budeme přidávat, jsou [Omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [Nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota).</span><span class="sxs-lookup"><span data-stu-id="aa13b-152">The two policies we will be adding in this tutorial are the [Limit call rate per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) policies.</span></span> <span data-ttu-id="aa13b-153">Tyto zásady se musí použít na obor produktu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-153">These policies must be applied at the product scope.</span></span>

<span data-ttu-id="aa13b-154">V nabídce **API Management** na levé straně klikněte na **Zásady**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-154">Click **Policies** under the **API Management** menu on the left.</span></span> <span data-ttu-id="aa13b-155">V seznamu **Produkt** klikněte na položku **Bezplatná zkušební verze**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-155">In the **Product** list, click **Free Trial**.</span></span>

![Zásady produktu][api-management-product-policy]

<span data-ttu-id="aa13b-157">Kliknutím na **Přidat zásady** proveďte import šablony zásad a začněte vytvářet zásady omezení četnosti a zásady kvót.</span><span class="sxs-lookup"><span data-stu-id="aa13b-157">Click **Add Policy** to import the policy template and begin creating the rate limit and quota policies.</span></span>

![Přidání zásad][api-management-add-policy]

<span data-ttu-id="aa13b-159">Zásady omezení četnosti a zásady kvót jsou příchozími zásadami, proto kurzor umístěte do příchozího prvku.</span><span class="sxs-lookup"><span data-stu-id="aa13b-159">Rate limit and quota policies are inbound policies, so position the cursor in the inbound element.</span></span>

![Editor zásad][api-management-policy-editor-inbound]

<span data-ttu-id="aa13b-161">Posouváním seznamem zásad vyhledejte záznam zásady **Omezení četnosti volání podle předplatného**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-161">Scroll through the list of policies and locate the **Limit call rate per subscription** policy entry.</span></span>

![Příkazy zásad][api-management-limit-policies]

<span data-ttu-id="aa13b-163">Po umístění kurzoru v prvku **inbound** zásad klikněte na šipku vedle položky **Omezení četnosti volání podle předplatného** a vložte jeho šablonu zásad.</span><span class="sxs-lookup"><span data-stu-id="aa13b-163">After the cursor is positioned in the **inbound** policy element, click the arrow beside **Limit call rate per subscription** to insert its policy template.</span></span>

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

<span data-ttu-id="aa13b-164">Jak je vidět ve fragmentu kódu, zásada umožňuje nastavení omezení pro operace a rozhraní API produktu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-164">As you can see from the snippet, the policy allows setting limits for the product's APIs and operations.</span></span> <span data-ttu-id="aa13b-165">V tomto kurzu tuto schopnost využívat nebudeme, takže z elementu **rate-limit** odstraňte elementy **api** a **operation**, aby zůstal jenom vnější element **rate-limit**, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-165">In this tutorial we will not use that capability, so delete the **api** and **operation** elements from the **rate-limit** element, such that only the outer **rate-limit** element remains, as shown in the following example.</span></span>

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

<span data-ttu-id="aa13b-166">V případě bezplatné zkušební verze produktu je maximální povolená četnost volání 10 volání za minutu, proto do atributu **call** zadejte hodnotu**10** a do atributu **renewal-period** zadejte hodnotu **60**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-166">In the Free Trial product, the maximum allowable call rate is 10 calls per minute, so type **10** as the value for the **calls** attribute, and **60** for the **renewal-period** attribute.</span></span>

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

<span data-ttu-id="aa13b-167">Pokud chcete nakonfigurovat zásadu **Nastavení kvóty využití podle předplatného**, umístěte kurzor bezprostředně pod nově přidaný element **rate-limit** v elementu **inbound** a potom vyhledejte a klikněte na šipku nalevo od **Nastavení kvóty využití podle předplatného**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-167">To configure the **Set usage quota per subscription** policy, position your cursor immediately below the newly added **rate-limit** element within the **inbound** element, and then locate and click the arrow to the left of **Set usage quota per subscription**.</span></span>

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

<span data-ttu-id="aa13b-168">Podobně jako zásada **Nastavení kvóty využití podle předplatného** umožňuje i zásada **Nastavení kvóty využití podle předplatného** nastavení omezení pro operace a rozhraní API produktu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-168">Similarly to the **Set usage quota per subscription** policy, **Set usage quota per subscription** policy allows setting caps for on the product's APIs and operations.</span></span> <span data-ttu-id="aa13b-169">V tomto kurzu tuto schopnost využívat nebudeme, takže z elementu **quota** odstraňte elementy **api** a **operation**, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-169">In this tutorial we will not use that capability, so delete the **api** and **operation** elements from the **quota** element, as shown in the following example.</span></span>

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

<span data-ttu-id="aa13b-170">Kvóty můžou být založené na počtu volání za interval, na šířce pásma nebo na obojím.</span><span class="sxs-lookup"><span data-stu-id="aa13b-170">Quotas can be based on the number of calls per interval, bandwidth, or both.</span></span> <span data-ttu-id="aa13b-171">V tomto kurzu neprovádíme omezování na základě šířky pásma, proto můžete odstranit atribut **bandwidth** (šířka pásma).</span><span class="sxs-lookup"><span data-stu-id="aa13b-171">In this tutorial, we are not throttling based on bandwidth, so delete the **bandwidth** attribute.</span></span>

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

<span data-ttu-id="aa13b-172">V rámci bezplatné zkušební verze produktu má kvóta hodnotu 200 volání za týden.</span><span class="sxs-lookup"><span data-stu-id="aa13b-172">In the Free Trial product, the quota is 200 calls per week.</span></span> <span data-ttu-id="aa13b-173">Do atributu **calls** zadejte hodnotu **200** a potom do atributu **renewal-period** zadejte hodnotu **604800**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-173">Specify **200** as the value for the **calls** attribute, and then specify **604800** as the value for the **renewal-period** attribute.</span></span>

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> <span data-ttu-id="aa13b-174">Intervaly zásad se zadávají v sekundách.</span><span class="sxs-lookup"><span data-stu-id="aa13b-174">Policy intervals are specified in seconds.</span></span> <span data-ttu-id="aa13b-175">Pokud chcete vypočítat interval pro týden, můžete počet dní (7) vynásobit počtem hodin za den (24), počtem minut za hodinu (60) a počtem sekund za minutu (60): 7 × 24 × 60 × 60 = 604 800.</span><span class="sxs-lookup"><span data-stu-id="aa13b-175">To calculate the interval for a week, you can multiply the number of days (7) by the number of hours in a day (24) by the number of minutes in an hour (60) by the number of seconds in a minute (60): 7 * 24 * 60 * 60 = 604800.</span></span>
> 
> 

<span data-ttu-id="aa13b-176">Zásady by po dokončení konfigurace měly odpovídat následujícímu příkladu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-176">When you have finished configuring the policy, it should match the following example.</span></span>

```xml
<policies>
    <inbound>
        <rate-limit calls="10" renewal-period="60">
        </rate-limit>
        <quota calls="200" renewal-period="604800">
        </quota>
        <base />

</inbound>
<outbound>

    <base />

    </outbound>
</policies>
```

<span data-ttu-id="aa13b-177">Po nakonfigurování požadovaných zásad klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-177">After the desired policies are configured, click **Save**.</span></span>

![Uložení zásad][api-management-policy-save]

## <span data-ttu-id="aa13b-179"><a name="publish-product"> </a> Publikování produktu</span><span class="sxs-lookup"><span data-stu-id="aa13b-179"><a name="publish-product"> </a> To publish the product</span></span>
<span data-ttu-id="aa13b-180">Když jste přidali rozhraní API a nakonfigurovali zásady, je třeba produkt publikovat, aby ho vývojáři mohli začít používat.</span><span class="sxs-lookup"><span data-stu-id="aa13b-180">Now that the the APIs are added and the policies are configured, the product must be published so that it can be used by developers.</span></span> <span data-ttu-id="aa13b-181">V nabídce **API Management** na levé straně klikněte na **Produkty**, potom na **Bezplatná zkušební verze** a nakonfigurujte produkt.</span><span class="sxs-lookup"><span data-stu-id="aa13b-181">Click **Products** from the **API Management** menu on the left, and then click **Free Trial** to configure the product.</span></span>

![Konfigurace produktu][api-management-configure-product]

<span data-ttu-id="aa13b-183">Klikněte na **Publikovat** a potvrďte kliknutím na **Ano, publikovat**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-183">Click **Publish**, and then click **Yes, publish it** to confirm.</span></span>

![Publikování produktu][api-management-publish-product]

## <span data-ttu-id="aa13b-185"><a name="subscribe-account"> </a>Přihlášení vývojářského účtu k odběru produktu</span><span class="sxs-lookup"><span data-stu-id="aa13b-185"><a name="subscribe-account"> </a>To subscribe a developer account to the product</span></span>
<span data-ttu-id="aa13b-186">Teď, když je produkt publikovaný, se vývojáři můžou přihlásit k jeho odběru a můžou ho začít používat.</span><span class="sxs-lookup"><span data-stu-id="aa13b-186">Now that the product is published, it is available to be subscribed to and used by developers.</span></span>

> <span data-ttu-id="aa13b-187">Správci instance API Management se automaticky přihlašují k odběru každého produktu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-187">Administrators of an API Management instance are automatically subscribed to every product.</span></span> <span data-ttu-id="aa13b-188">V tomto kroku kurzu přihlásíme jeden vývojářský účet bez oprávnění správce k odběru bezplatné zkušební verze produktu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-188">In this tutorial step, we will subscribe one of the non-administrator developer accounts to the Free Trial product.</span></span> <span data-ttu-id="aa13b-189">Pokud je vývojářský účet součástí role správců, můžete tímto krokem projít i v případě, že už jste k odběru přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="aa13b-189">If your developer account is part of the Administrators role, then you can follow along with this step, even though you are already subscribed.</span></span>
> 
> 

<span data-ttu-id="aa13b-190">V nabídce **API Management** na levé straně klikněte na **Uživatelé** a potom klikněte na název vývojářského účtu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-190">Click **Users** on the **API Management** menu on the left, and then click the name of your developer account.</span></span> <span data-ttu-id="aa13b-191">V tomto příkladu používáme vývojářský účet **Clayton Gragg**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-191">In this example, we are using the **Clayton Gragg** developer account.</span></span>

![Konfigurace vývojáře][api-management-configure-developer]

<span data-ttu-id="aa13b-193">Klikněte na **Přidat předplatné**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-193">Click **Add Subscription**.</span></span>

![Přidat předplatné][api-management-add-subscription-menu]

<span data-ttu-id="aa13b-195">Vyberte **Bezplatná zkušební verze** a potom klikněte na **Přihlásit k odběru**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-195">Select **Free Trial**, and then click **Subscribe**.</span></span>

![Přidat předplatné][api-management-add-subscription]

> [!NOTE]
> <span data-ttu-id="aa13b-197">V tomto kurzu není v případě bezplatné zkušební verze produktu povoleno více souběžných předplatných.</span><span class="sxs-lookup"><span data-stu-id="aa13b-197">In this tutorial, multiple simultaneous subscriptions are not enabled for the Free Trial product.</span></span> <span data-ttu-id="aa13b-198">Pokud by bylo, zobrazila by se výzva jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-198">If they were, you would be prompted to name the subscription, as shown in the following example.</span></span>
> 
> 

![Přidat předplatné][api-management-add-subscription-multiple]

<span data-ttu-id="aa13b-200">Po kliknutí na **Přihlásit k odběru** se produkt zobrazí v uživatelském seznamu **Předplatné**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-200">After clicking **Subscribe**, the product appears in the **Subscription** list for the user.</span></span>

![Předplatné přidáno][api-management-subscription-added]

## <span data-ttu-id="aa13b-202"><a name="test-rate-limit"> </a>Volání operace a testování omezení četnosti</span><span class="sxs-lookup"><span data-stu-id="aa13b-202"><a name="test-rate-limit"> </a>To call an operation and test the rate limit</span></span>
<span data-ttu-id="aa13b-203">Když už máte bezplatnou zkušební verzi produktu nakonfigurovanou a publikovanou, můžete začít volat operace a testovat omezení četnosti.</span><span class="sxs-lookup"><span data-stu-id="aa13b-203">Now that the Free Trial product is configured and published, we can call some operations and test the rate limit policy.</span></span>
<span data-ttu-id="aa13b-204">Kliknutím na **Portál pro vývojáře** v pravé horní nabídce přejděte na portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="aa13b-204">Switch to the developer portal by clicking **Developer portal** in the upper-right menu.</span></span>

![Portál pro vývojáře][api-management-developer-portal-menu]

<span data-ttu-id="aa13b-206">Klikněte v horní nabídce na **Rozhraní API** a potom vyberte **Rozhraní API v programu Echo**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-206">Click **APIs** in the top menu, and then click **Echo API**.</span></span>

![portálu pro vývojáře][api-management-developer-portal-api-menu]

<span data-ttu-id="aa13b-208">Klikněte na **GET Resource** a potom klikněte na **Zkouška**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-208">Click **GET Resource**, and then click **Try it**.</span></span>

![Otevření konzoly][api-management-open-console]

<span data-ttu-id="aa13b-210">Ponechte výchozí hodnoty parametrů a potom vyberte svůj klíč předplatného pro bezplatnou zkušební verzi produktu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-210">Keep the default parameter values, and then select your subscription key for the Free Trial product.</span></span>

![Klíč předplatného][api-management-select-key]

> [!NOTE]
> <span data-ttu-id="aa13b-212">Pokud máte více předplatných, vyberte klíč pro **bezplatnou zkušební verzi**, jinak zásady nakonfigurované v předchozích krocích nebudou platit.</span><span class="sxs-lookup"><span data-stu-id="aa13b-212">If you have multiple subscriptions, be sure to select the key for **Free Trial**, or else the policies that were configured in the previous steps won't be in effect.</span></span>
> 
> 

<span data-ttu-id="aa13b-213">Klikněte na **Odeslat** a potom si zobrazte odezvu.</span><span class="sxs-lookup"><span data-stu-id="aa13b-213">Click **Send**, and then view the response.</span></span> <span data-ttu-id="aa13b-214">Všimněte si, že **stav odezvy** je **200 – OK**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-214">Note the **Response status** of **200 OK**.</span></span>

![Výsledky operace][api-management-http-get-results]

<span data-ttu-id="aa13b-216">Klikněte na **Odeslat** víckrát, než dovolují zásady omezení četnosti (10 volání za minutu).</span><span class="sxs-lookup"><span data-stu-id="aa13b-216">Click **Send** at a rate greater than the rate limit policy of 10 calls per minute.</span></span> <span data-ttu-id="aa13b-217">Po překročení zásad omezení četnosti se vrátí stav odezvy **429 – Příliš mnoho požadavků**.</span><span class="sxs-lookup"><span data-stu-id="aa13b-217">After the rate limit policy is exceeded, a response status of **429 Too Many Requests** is returned.</span></span>

![Výsledky operace][api-management-http-get-429]

<span data-ttu-id="aa13b-219">**Obsah odezvy** zobrazuje zbývající délku intervalu, po kterém bude opakování úspěšné.</span><span class="sxs-lookup"><span data-stu-id="aa13b-219">The **Response content** indicates the remaining interval before retries will be successful.</span></span>

<span data-ttu-id="aa13b-220">Pokud platí zásady omezení četnosti v počtu 10 volání za minutu, následná volání nebudou úspěšná, dokud neuplyne 60 sekund od prvních 10 úspěšných volání produktu před překročením omezení četnosti volání.</span><span class="sxs-lookup"><span data-stu-id="aa13b-220">When the rate limit policy of 10 calls per minute is in effect, subsequent calls will fail until 60 seconds have elapsed from the first of the 10 successful calls to the product before the rate limit was exceeded.</span></span> <span data-ttu-id="aa13b-221">V tomto příkladu je zbývající délka intervalu 54 sekund.</span><span class="sxs-lookup"><span data-stu-id="aa13b-221">In this example, the remaining interval is 54 seconds.</span></span>

## <span data-ttu-id="aa13b-222"><a name="next-steps"> </a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa13b-222"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="aa13b-223">V následujícím videu si pusťte ukázku nastavení kvót a omezení četnosti.</span><span class="sxs-lookup"><span data-stu-id="aa13b-223">Watch a demo of setting rate limits and quotas in the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Manage your first API in Azure API Management]: api-management-get-started.md
[How to create and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
