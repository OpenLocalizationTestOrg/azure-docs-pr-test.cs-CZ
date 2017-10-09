---
title: "aaaProtect rozhraní API pomocí Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooprotect vaše rozhraní API pomocí zásad kvót a omezování zásady (omezení rychlosti)."
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
ms.openlocfilehash: 3113fd277d434da0c051b8b90fd629a102bf4867
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a><span data-ttu-id="52669-103">Ochrana rozhraní API omezením četnosti pomocí Azure API Management</span><span class="sxs-lookup"><span data-stu-id="52669-103">Protect your API with rate limits using Azure API Management</span></span>
<span data-ttu-id="52669-104">Tento průvodce vám ukáže, jak je snadné tooadd ochranu pro váš back-end rozhraní API podle konfigurace zásad kvót a omezování míra s Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="52669-104">This guide shows you how easy it is tooadd protection for your backend API by configuring rate limit and quota policies with Azure API Management.</span></span>

<span data-ttu-id="52669-105">V tomto kurzu vytvoříte "Bezplatnou zkušební verzi" rozhraní API produktu, který umožňuje vývojářům toomake too10 volání za minutu a až tooa maximálně 200 volání za týden tooyour rozhraní API pomocí hello [omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [ Nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) zásady.</span><span class="sxs-lookup"><span data-stu-id="52669-105">In this tutorial, you will create a "Free Trial" API product that allows developers toomake up too10 calls per minute and up tooa maximum of 200 calls per week tooyour API using hello [Limit call rate per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) policies.</span></span> <span data-ttu-id="52669-106">Potom publikovat hello rozhraní API a testování omezení četnosti hello.</span><span class="sxs-lookup"><span data-stu-id="52669-106">You will then publish hello API and test hello rate limit policy.</span></span>

<span data-ttu-id="52669-107">Pro pokročilejší scénáře omezování pomocí hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady, najdete v části [pokročilé omezování požadavků pomocí Azure API Management](api-management-sample-flexible-throttling.md).</span><span class="sxs-lookup"><span data-stu-id="52669-107">For more advanced throttling scenarios using hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies, see [Advanced request throttling with Azure API Management](api-management-sample-flexible-throttling.md).</span></span>

## <span data-ttu-id="52669-108"><a name="create-product"></a>toocreate produktu</span><span class="sxs-lookup"><span data-stu-id="52669-108"><a name="create-product"> </a>toocreate a product</span></span>
<span data-ttu-id="52669-109">V tomto kroku vytvoříte bezplatnou zkušební verzi produktu, který nevyžaduje schválení předplatného.</span><span class="sxs-lookup"><span data-stu-id="52669-109">In this step, you will create a Free Trial product that does not require subscription approval.</span></span>

> [!NOTE]
> <span data-ttu-id="52669-110">Pokud už máte produkt nakonfigurovaný a chcete toouse ho v tomto kurzu, můžete přeskočit příliš[Konfigurace četnosti zásad kvót a omezování] [ Configure call rate limit and quota policies] a postupujte podle pokynů hello kurzu odtamtud se svým produktem místo hello bezplatné zkušební verze produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-110">If you already have a product configured and want toouse it for this tutorial, you can jump ahead too[Configure call rate limit and quota policies][Configure call rate limit and quota policies] and follow hello tutorial from there using your product in place of hello Free Trial product.</span></span>
> 
> 

<span data-ttu-id="52669-111">tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="52669-111">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="52669-113">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Správa vašeho prvního rozhraní API v Azure API Management] [ Manage your first API in Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="52669-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Manage your first API in Azure API Management][Manage your first API in Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="52669-114">Klikněte na tlačítko **produkty** v hello **API Management** nabídky na levém toodisplay hello hello **produkty** stránky.</span><span class="sxs-lookup"><span data-stu-id="52669-114">Click **Products** in hello **API Management** menu on hello left toodisplay hello **Products** page.</span></span>

![Přidání produktu][api-management-add-product]

<span data-ttu-id="52669-116">Klikněte na tlačítko **přidat produkt** toodisplay hello **přidání nového produktu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="52669-116">Click **Add product** toodisplay hello **Add new product** dialog box.</span></span>

![Přidání nového produktu][api-management-new-product-window]

<span data-ttu-id="52669-118">V hello **název** zadejte **bezplatné zkušební verze**.</span><span class="sxs-lookup"><span data-stu-id="52669-118">In hello **Title** box, type **Free Trial**.</span></span>

<span data-ttu-id="52669-119">V hello **popis** pole, typ hello následující text: **Odběratelé, kteří budou mít toorun 10 volání za minutu až tooa maximálně 200 volání za týden. potom bude přístup odepřen.**</span><span class="sxs-lookup"><span data-stu-id="52669-119">In hello **Description** box, type hello following text: **Subscribers will be able toorun 10 calls/minute up tooa maximum of 200 calls/week after which access is denied.**</span></span>

<span data-ttu-id="52669-120">Produkty ve službě API Management můžou být chráněné nebo otevřené.</span><span class="sxs-lookup"><span data-stu-id="52669-120">Products in API Management can be protected or open.</span></span> <span data-ttu-id="52669-121">Chráněných produktů musí být odebírané toobefore, které mohou být použity.</span><span class="sxs-lookup"><span data-stu-id="52669-121">Protected products must be subscribed toobefore they can be used.</span></span> <span data-ttu-id="52669-122">Otevřené produkty můžete používat bez předplatného.</span><span class="sxs-lookup"><span data-stu-id="52669-122">Open products can be used without a subscription.</span></span> <span data-ttu-id="52669-123">Ujistěte se, že **vyžadovat předplatné** je vybrané toocreate chráněný produkt, který vyžaduje předplatné.</span><span class="sxs-lookup"><span data-stu-id="52669-123">Ensure that **Require subscription** is selected toocreate a protected product that requires a subscription.</span></span> <span data-ttu-id="52669-124">Toto je výchozí nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="52669-124">This is hello default setting.</span></span>

<span data-ttu-id="52669-125">Pokud chcete tooreview správce a přijměte nebo odmítněte předplatné pokusí toothis produktu, vyberte **vyžadovat schválení předplatného**.</span><span class="sxs-lookup"><span data-stu-id="52669-125">If you want an administrator tooreview and accept or reject subscription attempts toothis product, select **Require subscription approval**.</span></span> <span data-ttu-id="52669-126">Pokud není zaškrtnuté políčko hello, pokusy o předplatné bude schvalovat automaticky.</span><span class="sxs-lookup"><span data-stu-id="52669-126">If hello check box is not selected, subscription attempts will be auto-approved.</span></span> <span data-ttu-id="52669-127">V tomto příkladu předplatné schvaluje automaticky, takže nezaškrtávejte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="52669-127">In this example, subscriptions are automatically approved, so do not select hello box.</span></span>

<span data-ttu-id="52669-128">tooallow vývojáře účty toosubscribe několikrát toohello nového produktu, vyberte hello **povolit více souběžných předplatných** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="52669-128">tooallow developer accounts toosubscribe multiple times toohello new product, select hello **Allow multiple simultaneous subscriptions** check box.</span></span> <span data-ttu-id="52669-129">Tento kurz několik souběžných předplatných nevyužívá, takže políčko nechte nezaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="52669-129">This tutorial does not utilize multiple simultaneous subscriptions, so leave it unchecked.</span></span>

<span data-ttu-id="52669-130">Po zadání všech hodnot, klikněte na tlačítko **Uložit** toocreate hello produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-130">After all values are entered, click **Save** toocreate hello product.</span></span>

![Produkt přidán][api-management-product-added]

<span data-ttu-id="52669-132">Ve výchozím nastavení jsou nové produkty viditelné toousers v hello **správci** skupiny.</span><span class="sxs-lookup"><span data-stu-id="52669-132">By default, new products are visible toousers in hello **Administrators** group.</span></span> <span data-ttu-id="52669-133">Přidáme tooadd hello **vývojáři** skupiny.</span><span class="sxs-lookup"><span data-stu-id="52669-133">We are going tooadd hello **Developers** group.</span></span> <span data-ttu-id="52669-134">Klikněte na tlačítko **bezplatné zkušební verze**a potom klikněte na hello **viditelnost** kartě.</span><span class="sxs-lookup"><span data-stu-id="52669-134">Click **Free Trial**, and then click hello **Visibility** tab.</span></span>

> <span data-ttu-id="52669-135">Skupiny ve službě API Management jsou použité toomanage hello viditelnost toodevelopers produkty.</span><span class="sxs-lookup"><span data-stu-id="52669-135">In API Management, groups are used toomanage hello visibility of products toodevelopers.</span></span> <span data-ttu-id="52669-136">Produkty udělují viditelnost toogroups a vývojáři můžou zobrazovat a odebírat toohello produkty, které jsou viditelné toohello skupiny, do které patří.</span><span class="sxs-lookup"><span data-stu-id="52669-136">Products grant visibility toogroups, and developers can view and subscribe toohello products that are visible toohello groups in which they belong.</span></span> <span data-ttu-id="52669-137">Další informace najdete v tématu [jak toocreate a používání skupin v Azure API Management][How toocreate and use groups in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="52669-137">For more information, see [How toocreate and use groups in Azure API Management][How toocreate and use groups in Azure API Management].</span></span>
> 
> 

![Přidání skupiny vývojářů][api-management-add-developers-group]

<span data-ttu-id="52669-139">Vyberte hello **vývojáři** zaškrtněte políčko a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="52669-139">Select hello **Developers** check box, and then click **Save**.</span></span>

## <span data-ttu-id="52669-140"><a name="add-api"></a>tooadd rozhraní API toohello produktu</span><span class="sxs-lookup"><span data-stu-id="52669-140"><a name="add-api"> </a>tooadd an API toohello product</span></span>
<span data-ttu-id="52669-141">V tomto kroku kurzu hello přidáme hello Echo API toohello nové bezplatné zkušební verze produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-141">In this step of hello tutorial, we will add hello Echo API toohello new Free Trial product.</span></span>

> <span data-ttu-id="52669-142">Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním Echo API, které je možné použít tooexperiment s a další informace o službě API Management.</span><span class="sxs-lookup"><span data-stu-id="52669-142">Each API Management service instance comes pre-configured with an Echo API that can be used tooexperiment with and learn about API Management.</span></span> <span data-ttu-id="52669-143">Další informace najdete v článku [Správa vašeho prvního rozhraní API ve službě Azure API Management][Manage your first API in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="52669-143">For more information, see [Manage your first API in Azure API Management][Manage your first API in Azure API Management].</span></span>
> 
> 

<span data-ttu-id="52669-144">Klikněte na tlačítko **produkty** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **bezplatné zkušební verze** tooconfigure hello produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-144">Click **Products** from hello **API Management** menu on hello left, and then click **Free Trial** tooconfigure hello product.</span></span>

![Konfigurace produktu][api-management-configure-product]

<span data-ttu-id="52669-146">Klikněte na tlačítko **přidat rozhraní API tooproduct**.</span><span class="sxs-lookup"><span data-stu-id="52669-146">Click **Add API tooproduct**.</span></span>

![Přidání rozhraní API tooproduct][api-management-add-api]

<span data-ttu-id="52669-148">Vyberte **Rozhraní API v programu Echo** a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="52669-148">Select **Echo API**, and then click **Save**.</span></span>

![Přidání rozhraní API v programu Echo][api-management-add-echo-api]

## <span data-ttu-id="52669-150"><a name="policies"></a>tooconfigure četnosti zásad kvót a omezování</span><span class="sxs-lookup"><span data-stu-id="52669-150"><a name="policies"> </a>tooconfigure call rate limit and quota policies</span></span>
<span data-ttu-id="52669-151">Omezení četnosti a kvóty se konfigurují v editoru zásad hello.</span><span class="sxs-lookup"><span data-stu-id="52669-151">Rate limits and quotas are configured in hello policy editor.</span></span> <span data-ttu-id="52669-152">zásady Hello dva přidáme v tomto kurzu jsou hello [omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) zásady.</span><span class="sxs-lookup"><span data-stu-id="52669-152">hello two policies we will be adding in this tutorial are hello [Limit call rate per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) policies.</span></span> <span data-ttu-id="52669-153">Tyto zásady se musí použít v oboru produktu hello.</span><span class="sxs-lookup"><span data-stu-id="52669-153">These policies must be applied at hello product scope.</span></span>

<span data-ttu-id="52669-154">Klikněte na tlačítko **zásady** pod hello **API Management** nabídky na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="52669-154">Click **Policies** under hello **API Management** menu on hello left.</span></span> <span data-ttu-id="52669-155">V hello **produktu** seznamu, klikněte na tlačítko **bezplatné zkušební verze**.</span><span class="sxs-lookup"><span data-stu-id="52669-155">In hello **Product** list, click **Free Trial**.</span></span>

![Zásady produktu][api-management-product-policy]

<span data-ttu-id="52669-157">Klikněte na tlačítko **přidat zásadu** tooimport hello šablony zásad a začněte vytvářet hello zásadami kvót a omezování rychlost.</span><span class="sxs-lookup"><span data-stu-id="52669-157">Click **Add Policy** tooimport hello policy template and begin creating hello rate limit and quota policies.</span></span>

![Přidání zásad][api-management-add-policy]

<span data-ttu-id="52669-159">Míra zásadami kvót a omezování jsou příchozími zásadami, tak kurzor hello pozici v hello příchozího prvku.</span><span class="sxs-lookup"><span data-stu-id="52669-159">Rate limit and quota policies are inbound policies, so position hello cursor in hello inbound element.</span></span>

![Editor zásad][api-management-policy-editor-inbound]

<span data-ttu-id="52669-161">Posuňte hello seznam zásad a najděte hello **omezení četnosti volání podle předplatného** zásadu.</span><span class="sxs-lookup"><span data-stu-id="52669-161">Scroll through hello list of policies and locate hello **Limit call rate per subscription** policy entry.</span></span>

![Příkazy zásad][api-management-limit-policies]

<span data-ttu-id="52669-163">Po hello kurzor je nastavený v hello **příchozí** zásad klikněte na šipku hello vedle položky **omezení četnosti volání podle předplatného** tooinsert jeho šablonu zásad.</span><span class="sxs-lookup"><span data-stu-id="52669-163">After hello cursor is positioned in hello **inbound** policy element, click hello arrow beside **Limit call rate per subscription** tooinsert its policy template.</span></span>

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

<span data-ttu-id="52669-164">Jak je vidět z hello fragment hello zásada umožňuje nastavení omezení pro rozhraní API a operace hello produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-164">As you can see from hello snippet, hello policy allows setting limits for hello product's APIs and operations.</span></span> <span data-ttu-id="52669-165">V tomto kurzu jsme nebude používat tuto funkci, proto můžete odstranit hello **rozhraní api** a **operace** elementy z hello **limit rychlosti** elementu, tak, aby se pouze hello vnější **limit rychlosti** element zůstává, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="52669-165">In this tutorial we will not use that capability, so delete hello **api** and **operation** elements from hello **rate-limit** element, such that only hello outer **rate-limit** element remains, as shown in hello following example.</span></span>

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

<span data-ttu-id="52669-166">V hello bezplatnou zkušební verzi produktu, hello maximální povolená Četnost volání 10 volání za minutu, proto **10** hello hodnotu hello **volání** atribut a **60** pro hello **doby obnovení** atribut.</span><span class="sxs-lookup"><span data-stu-id="52669-166">In hello Free Trial product, hello maximum allowable call rate is 10 calls per minute, so type **10** as hello value for hello **calls** attribute, and **60** for hello **renewal-period** attribute.</span></span>

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

<span data-ttu-id="52669-167">tooconfigure hello **nastavení kvóty využití podle předplatného** zásady, pozice kurzor bezprostředně pod hello nově přidaná **limit rychlosti** v rámci hello **příchozí** element a poté vyhledejte a klikněte na tlačítko hello šipku toohello nalevo od **nastavení kvóty využití podle předplatného**.</span><span class="sxs-lookup"><span data-stu-id="52669-167">tooconfigure hello **Set usage quota per subscription** policy, position your cursor immediately below hello newly added **rate-limit** element within hello **inbound** element, and then locate and click hello arrow toohello left of **Set usage quota per subscription**.</span></span>

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

<span data-ttu-id="52669-168">Podobně toohello **nastavení kvóty využití podle předplatného** zásady, **nastavení kvóty využití podle předplatného** umožňuje zásady CAP k vzdálené ploše pro nastavení na rozhraní API a operace hello produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-168">Similarly toohello **Set usage quota per subscription** policy, **Set usage quota per subscription** policy allows setting caps for on hello product's APIs and operations.</span></span> <span data-ttu-id="52669-169">V tomto kurzu jsme nebude používat tuto funkci, proto můžete odstranit hello **rozhraní api** a **operace** elementy z hello **kvóty** elementu, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="52669-169">In this tutorial we will not use that capability, so delete hello **api** and **operation** elements from hello **quota** element, as shown in hello following example.</span></span>

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

<span data-ttu-id="52669-170">Kvóty můžou být založené na hello počet volání za interval, šířky pásma nebo obou.</span><span class="sxs-lookup"><span data-stu-id="52669-170">Quotas can be based on hello number of calls per interval, bandwidth, or both.</span></span> <span data-ttu-id="52669-171">V tomto kurzu Neprovádíme omezování na základě šířky pásma, proto můžete odstranit hello **šířky pásma** atribut.</span><span class="sxs-lookup"><span data-stu-id="52669-171">In this tutorial, we are not throttling based on bandwidth, so delete hello **bandwidth** attribute.</span></span>

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

<span data-ttu-id="52669-172">V hello bezplatnou zkušební verzi produktu hello kvóta hodnotu 200 volání za týden.</span><span class="sxs-lookup"><span data-stu-id="52669-172">In hello Free Trial product, hello quota is 200 calls per week.</span></span> <span data-ttu-id="52669-173">Zadejte **200** hello hodnotu hello **volání** atributů a potom zadejte **604800** hello hodnotu hello **doby obnovení** atribut.</span><span class="sxs-lookup"><span data-stu-id="52669-173">Specify **200** as hello value for hello **calls** attribute, and then specify **604800** as hello value for hello **renewal-period** attribute.</span></span>

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> <span data-ttu-id="52669-174">Intervaly zásad se zadávají v sekundách.</span><span class="sxs-lookup"><span data-stu-id="52669-174">Policy intervals are specified in seconds.</span></span> <span data-ttu-id="52669-175">toocalculate hello interval pro týden, můžete násobení hello počet dní (7) hello počtem hodin za den (24) hello počtem minut za hodinu (60) a hello počtem sekund za minutu (60): 7 * 24 * 60 * 60 = 604800.</span><span class="sxs-lookup"><span data-stu-id="52669-175">toocalculate hello interval for a week, you can multiply hello number of days (7) by hello number of hours in a day (24) by hello number of minutes in an hour (60) by hello number of seconds in a minute (60): 7 * 24 * 60 * 60 = 604800.</span></span>
> 
> 

<span data-ttu-id="52669-176">Po dokončení konfigurace zásad hello shodovat hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="52669-176">When you have finished configuring hello policy, it should match hello following example.</span></span>

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

<span data-ttu-id="52669-177">Po hello potřeby, jsou zásady nakonfigurované, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="52669-177">After hello desired policies are configured, click **Save**.</span></span>

![Uložení zásad][api-management-policy-save]

## <span data-ttu-id="52669-179"><a name="publish-product"></a> toopublish hello produktu</span><span class="sxs-lookup"><span data-stu-id="52669-179"><a name="publish-product"> </a> toopublish hello product</span></span>
<span data-ttu-id="52669-180">Teď, když hello hello přidání rozhraní API, a jsou nakonfigurovány hello zásady, třeba hello produkt publikovat, aby se může použít vývojáři.</span><span class="sxs-lookup"><span data-stu-id="52669-180">Now that hello hello APIs are added and hello policies are configured, hello product must be published so that it can be used by developers.</span></span> <span data-ttu-id="52669-181">Klikněte na tlačítko **produkty** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **bezplatné zkušební verze** tooconfigure hello produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-181">Click **Products** from hello **API Management** menu on hello left, and then click **Free Trial** tooconfigure hello product.</span></span>

![Konfigurace produktu][api-management-configure-product]

<span data-ttu-id="52669-183">Klikněte na tlačítko **publikovat**a potom klikněte na **Ano, publikovat** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="52669-183">Click **Publish**, and then click **Yes, publish it** tooconfirm.</span></span>

![Publikování produktu][api-management-publish-product]

## <span data-ttu-id="52669-185"><a name="subscribe-account"></a>toosubscribe produktu toohello účtu vývojáře</span><span class="sxs-lookup"><span data-stu-id="52669-185"><a name="subscribe-account"> </a>toosubscribe a developer account toohello product</span></span>
<span data-ttu-id="52669-186">Teď je publikována hello produktu, je k dispozici toobe odběru tooand používají vývojáři.</span><span class="sxs-lookup"><span data-stu-id="52669-186">Now that hello product is published, it is available toobe subscribed tooand used by developers.</span></span>

> <span data-ttu-id="52669-187">Správci instance API Management jsou automaticky odebírané tooevery produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-187">Administrators of an API Management instance are automatically subscribed tooevery product.</span></span> <span data-ttu-id="52669-188">V tomto kroku kurzu jsme odběru jeden hello vývojáře bez oprávnění správce účtů toohello bezplatné zkušební verze produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-188">In this tutorial step, we will subscribe one of hello non-administrator developer accounts toohello Free Trial product.</span></span> <span data-ttu-id="52669-189">Pokud vývojářský účet součástí role správců hello, potom můžete provést tímto krokem projít i v případě, že jste přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="52669-189">If your developer account is part of hello Administrators role, then you can follow along with this step, even though you are already subscribed.</span></span>
> 
> 

<span data-ttu-id="52669-190">Klikněte na tlačítko **uživatelé** na hello **API Management** nabídky na hello left a pak klikněte na název vývojářského účtu hello.</span><span class="sxs-lookup"><span data-stu-id="52669-190">Click **Users** on hello **API Management** menu on hello left, and then click hello name of your developer account.</span></span> <span data-ttu-id="52669-191">V tomto příkladu používáme hello **Clayton Gragg** vývojářský účet.</span><span class="sxs-lookup"><span data-stu-id="52669-191">In this example, we are using hello **Clayton Gragg** developer account.</span></span>

![Konfigurace vývojáře][api-management-configure-developer]

<span data-ttu-id="52669-193">Klikněte na **Přidat předplatné**.</span><span class="sxs-lookup"><span data-stu-id="52669-193">Click **Add Subscription**.</span></span>

![Přidat předplatné][api-management-add-subscription-menu]

<span data-ttu-id="52669-195">Vyberte **Bezplatná zkušební verze** a potom klikněte na **Přihlásit k odběru**.</span><span class="sxs-lookup"><span data-stu-id="52669-195">Select **Free Trial**, and then click **Subscribe**.</span></span>

![Přidat předplatné][api-management-add-subscription]

> [!NOTE]
> <span data-ttu-id="52669-197">V tomto kurzu více souběžných předplatných nejsou povolené pro hello bezplatné zkušební verze produktu.</span><span class="sxs-lookup"><span data-stu-id="52669-197">In this tutorial, multiple simultaneous subscriptions are not enabled for hello Free Trial product.</span></span> <span data-ttu-id="52669-198">Pokud byly, by byl výzvami tooname hello předplatného, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="52669-198">If they were, you would be prompted tooname hello subscription, as shown in hello following example.</span></span>
> 
> 

![Přidat předplatné][api-management-add-subscription-multiple]

<span data-ttu-id="52669-200">Po kliknutí na **přihlásit k odběru**, hello produkt zobrazí v hello **předplatné** seznamu pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="52669-200">After clicking **Subscribe**, hello product appears in hello **Subscription** list for hello user.</span></span>

![Předplatné přidáno][api-management-subscription-added]

## <span data-ttu-id="52669-202"><a name="test-rate-limit"></a>toocall operace a testování omezení četnosti hello</span><span class="sxs-lookup"><span data-stu-id="52669-202"><a name="test-rate-limit"> </a>toocall an operation and test hello rate limit</span></span>
<span data-ttu-id="52669-203">Teď, když hello bezplatnou zkušební verzi produktu nakonfigurovanou a publikovanou, jsme volat operace a testování omezení četnosti hello.</span><span class="sxs-lookup"><span data-stu-id="52669-203">Now that hello Free Trial product is configured and published, we can call some operations and test hello rate limit policy.</span></span>
<span data-ttu-id="52669-204">Portál pro vývojáře toohello přepínač kliknutím **portál pro vývojáře** v pravé horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="52669-204">Switch toohello developer portal by clicking **Developer portal** in hello upper-right menu.</span></span>

![Portál pro vývojáře][api-management-developer-portal-menu]

<span data-ttu-id="52669-206">Klikněte na tlačítko **rozhraní API** v hello horní nabídce a potom klikněte na **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="52669-206">Click **APIs** in hello top menu, and then click **Echo API**.</span></span>

![Portál pro vývojáře][api-management-developer-portal-api-menu]

<span data-ttu-id="52669-208">Klikněte na **GET Resource** a potom klikněte na **Zkouška**.</span><span class="sxs-lookup"><span data-stu-id="52669-208">Click **GET Resource**, and then click **Try it**.</span></span>

![Otevření konzoly][api-management-open-console]

<span data-ttu-id="52669-210">Ponechat hello výchozí hodnoty parametrů a pak vyberte svůj klíč předplatného pro bezplatnou zkušební verzi produktu hello.</span><span class="sxs-lookup"><span data-stu-id="52669-210">Keep hello default parameter values, and then select your subscription key for hello Free Trial product.</span></span>

![Klíč předplatného][api-management-select-key]

> [!NOTE]
> <span data-ttu-id="52669-212">Pokud máte více předplatných, že klíč hello tooselect pro být **bezplatné zkušební verze**, nebo jiný hello zásady, které byly nakonfigurované v předchozích krocích hello nebudou platit.</span><span class="sxs-lookup"><span data-stu-id="52669-212">If you have multiple subscriptions, be sure tooselect hello key for **Free Trial**, or else hello policies that were configured in hello previous steps won't be in effect.</span></span>
> 
> 

<span data-ttu-id="52669-213">Klikněte na tlačítko **odeslat**a potom si prohlédněte hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="52669-213">Click **Send**, and then view hello response.</span></span> <span data-ttu-id="52669-214">Poznámka: hello **stav odpovědi** z **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="52669-214">Note hello **Response status** of **200 OK**.</span></span>

![Výsledky operace][api-management-http-get-results]

<span data-ttu-id="52669-216">Klikněte na tlačítko **odeslat** rychlostí vyšší než hello dovolují zásady omezení 10 volání za minutu.</span><span class="sxs-lookup"><span data-stu-id="52669-216">Click **Send** at a rate greater than hello rate limit policy of 10 calls per minute.</span></span> <span data-ttu-id="52669-217">Po překročení zásad omezení četnosti hello stav odezvy **429 – příliš mnoho požadavků** je vrácen.</span><span class="sxs-lookup"><span data-stu-id="52669-217">After hello rate limit policy is exceeded, a response status of **429 Too Many Requests** is returned.</span></span>

![Výsledky operace][api-management-http-get-429]

<span data-ttu-id="52669-219">Hello **obsah odpovědi** označuje hello zbývající interval předtím, než bude opakování úspěšné.</span><span class="sxs-lookup"><span data-stu-id="52669-219">hello **Response content** indicates hello remaining interval before retries will be successful.</span></span>

<span data-ttu-id="52669-220">Pokud platí zásady omezení četnosti hello 10 volání za minutu, následná volání nebudou úspěšná, dokud neuplyne 60 sekund od hello první hello 10 úspěšných volání toohello produktu před překročením omezení četnosti hello.</span><span class="sxs-lookup"><span data-stu-id="52669-220">When hello rate limit policy of 10 calls per minute is in effect, subsequent calls will fail until 60 seconds have elapsed from hello first of hello 10 successful calls toohello product before hello rate limit was exceeded.</span></span> <span data-ttu-id="52669-221">V tomto příkladu je hello zbývající intervalu 54 sekund.</span><span class="sxs-lookup"><span data-stu-id="52669-221">In this example, hello remaining interval is 54 seconds.</span></span>

## <span data-ttu-id="52669-222"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="52669-222"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="52669-223">Podívejte se na ukázku nastavení kvót a omezení četnosti v hello následující videa.</span><span class="sxs-lookup"><span data-stu-id="52669-223">Watch a demo of setting rate limits and quotas in hello following video.</span></span>

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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Manage your first API in Azure API Management]: api-management-get-started.md
[How toocreate and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers tooa product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API toohello product]: #add-api
[Publish hello product]: #publish-product
[Subscribe a developer account toohello product]: #subscribe-account
[Call an operation and test hello rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
