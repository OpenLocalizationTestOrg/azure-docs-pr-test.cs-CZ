---
title: "aaaApplication brány integraci s Azure Security Center | Microsoft Docs"
description: "Tato stránka obsahuje informace o tom, jak je aplikační brána integrovaná do Azure Security Center."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: gwallace
ms.openlocfilehash: 6f6ace105e84c01f525ab02938e81ce040c5c9d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a><span data-ttu-id="a6819-103">Přehled integrace mezi aplikační brány a Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="a6819-103">Overview of integration between Application Gateway and Azure Security Center</span></span>

<span data-ttu-id="a6819-104">Zjistěte, jak aplikační brány a Security Center chránit prostředky vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6819-104">Learn how Application Gateway and Security Center help protect your web application resources.</span></span> <span data-ttu-id="a6819-105">Brány firewall webových aplikací aplikace brány (firewall webových aplikací) se integruje s [Security Center](../security-center/security-center-intro.md) tooprovide tooprevent bezproblémové zobrazení zjišťování a reakce toothreats toounprotected webových aplikací ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6819-105">Application gateway web application firewall (WAF) integrates with [Security Center](../security-center/security-center-intro.md) tooprovide a seamless view tooprevent, detect and respond toothreats toounprotected web applications in your environment.</span></span>

## <a name="overview"></a><span data-ttu-id="a6819-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="a6819-106">Overview</span></span>

<span data-ttu-id="a6819-107">Aplikace brány firewall webových aplikací je doporučeným v Centru zabezpečení pro ochranu před zneužitím a ohrožení zabezpečení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a6819-107">Application Gateway WAF is a recommendation in Security Center for protecting web applications from exploits and vulnerabilities.</span></span> <span data-ttu-id="a6819-108">Web povoleno prostředky, které nejsou chráněné firewall webových aplikací se zobrazí v Centru zabezpečení hello jako doporučení vysokou závažností.</span><span class="sxs-lookup"><span data-stu-id="a6819-108">Web enabled resources that are not protected by WAF show in hello security center as high severity recommendations.</span></span> <span data-ttu-id="a6819-109">Doporučení pro brány firewall webové aplikace se zobrazují na hello **přehled** v části **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a6819-109">Recommendations for web application firewalls are shown on hello **Overview** page, under **Applications**.</span></span>

![Integrace s security center][1]

<span data-ttu-id="a6819-111">Kliknutím na tlačítko všechna doporučení týkající se brány firewall webových aplikací, otevře se nové okno s podrobnostmi hello hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="a6819-111">Clicking any recommendations regarding web application firewall opens a new blade showing hello details of hello recommendation.</span></span>

## <a name="add-a-web-application-firewall-tooan-existing-resource"></a><span data-ttu-id="a6819-112">Přidání webové aplikace brány firewall tooan existující prostředky</span><span class="sxs-lookup"><span data-stu-id="a6819-112">Add a web application firewall tooan existing resource</span></span>

<span data-ttu-id="a6819-113">Přejděte příliš**více služeb** > **zabezpečení a identita** > **Security Center** a na hello **Security Center – přehled**  okně klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a6819-113">Navigate too**More Services** > **Security + Identity** > **Security Center** and on hello **Security Center - Overview** blade, click **Applications**.</span></span> <span data-ttu-id="a6819-114">Na hello **Security Center – aplikace** okně hello tabulka obsahuje seznam aplikací, které Security Center zjistila v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="a6819-114">On hello **Security Center - Applications** blade, hello table contains a list of applications that Security Center detected in your subscription.</span></span>

![webové aplikace][3]

<span data-ttu-id="a6819-116">Kliknutím na webovou aplikaci s problémem důležité získat hello **stav zabezpečení aplikací** okno.</span><span class="sxs-lookup"><span data-stu-id="a6819-116">By clicking on a web application with a critical issue, you get hello **Application security health** blade.</span></span> <span data-ttu-id="a6819-117">V následující hello obrázek text hello webové aplikace, který není chráněný pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a6819-117">In hello image below, hello web application that is not protected by a web application firewall.</span></span> 

![webové prostředky, které nejsou chráněny.][2]

<span data-ttu-id="a6819-119">Klikněte na tlačítko **přidání brány firewall webových aplikací** pod **doporučení** tooopen hello **přidání brány Firewall webových aplikací** okno.</span><span class="sxs-lookup"><span data-stu-id="a6819-119">Click **Add a web application firewall** under **Recommendations** tooopen hello **Add a Web Application Firewall** blade.</span></span>

<span data-ttu-id="a6819-120">Pokud není máte existující aplikační bránu, nebo chcete toocreate nový, klikněte na tlačítko **vytvořit nový** a na hello **vytvoření nové brány Firewall webových aplikací** a klikněte na **Microsoft - Aplikační brána**.</span><span class="sxs-lookup"><span data-stu-id="a6819-120">If you do not have an existing Application Gateway, or want toocreate a new one, click **Create New** and on hello **Create a new Web Application Firewall** blade, and click **Microsoft - Application Gateway**.</span></span> <span data-ttu-id="a6819-121">To vás provede kroky toocreate hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a6819-121">This takes you through hello steps toocreate an application gateway.</span></span> <span data-ttu-id="a6819-122">Webové aplikace je nyní přidána jako chráněný prostředek, Security Center nyní sleduje tento prostředek je chráněn brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a6819-122">At this point, your web application is added as a protected resource, Security Center now tracks that this resource is protected by a web application firewall.</span></span> <span data-ttu-id="a6819-123">To se nepřidá jako člen fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="a6819-123">This does not add it as a backend pool member.</span></span>

<span data-ttu-id="a6819-124">Pokud máte existující aplikační brány, můžete ho **použít existující řešení**</span><span class="sxs-lookup"><span data-stu-id="a6819-124">If you have an existing application gateway, you can choose it under **Use existing solution**</span></span>

![okno Přidání brány firewall webových aplikací][4]

<span data-ttu-id="a6819-126">Přidávání webové aplikace tooan Aplikační brána prostřednictvím Security Center nepřidá hello prostředků jako člen fondu back-end, musí provést na prostředku aplikační brány hello přímo.</span><span class="sxs-lookup"><span data-stu-id="a6819-126">Adding a web application tooan application gateway through Security Center does not add hello resource as a backend pool member, this must be done on hello application gateway resource directly.</span></span>

## <a name="add-a-resource-tooan-existing-web-application-firewall"></a><span data-ttu-id="a6819-127">Přidejte prostředků tooan existující brány firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="a6819-127">Add a resource tooan existing web application firewall</span></span>

<span data-ttu-id="a6819-128">Přejděte příliš**více služeb** > **zabezpečení a identita** > **Security Center** a na hello **Security Center – přehled**  okně klikněte na tlačítko **Partner solutions**.</span><span class="sxs-lookup"><span data-stu-id="a6819-128">Navigate too**More Services** > **Security + Identity** > **Security Center** and on hello **Security Center - Overview** blade, click **Partner solutions**.</span></span> <span data-ttu-id="a6819-129">Zobrazit existující brány aplikace podporující Security Center v hello **partnerských řešení** okno.</span><span class="sxs-lookup"><span data-stu-id="a6819-129">Existing Security Center aware application gateways show in hello **Partner Solutions** blade.</span></span>

![Partnerská řešení][7]

<span data-ttu-id="a6819-131">Klikněte na tlačítko **odkaz aplikace** tooopen hello **odkaz aplikace** okně tady budete mít hello možnosti tooselect stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6819-131">Click **Link app** tooopen hello **Link Applications** blade, here you are given hello options tooselect existing applications.</span></span> <span data-ttu-id="a6819-132">Zvolte tooprotect hello aplikace a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6819-132">Choose hello applications tooprotect and click **OK**.</span></span> <span data-ttu-id="a6819-133">To nepřidá hello webové aplikace toohello back-end fondu hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a6819-133">This does not add hello web application toohello backend pool of hello application gateway.</span></span> <span data-ttu-id="a6819-134">Toto nastaví hello prostředky jako chráněný prostředek, Security Center můžete sledovat jeho.</span><span class="sxs-lookup"><span data-stu-id="a6819-134">This sets hello resources as a protected resource so Security Center can track it.</span></span> <span data-ttu-id="a6819-135">tooadd hello zdroj jako člena fondu back-end, musí provést na hello aplikační bránu, v okně aktuální hello můžete kliknout na **řešení konzoly** toobe prováděné toohello prostředku aplikační brány, kde můžete přidat hello web fond back-end toohello aplikací.</span><span class="sxs-lookup"><span data-stu-id="a6819-135">tooadd hello resource as a backend pool member, this must be done on hello application gateway, from hello current blade you can click **Solution console** toobe taken toohello application gateway resource where you can add hello web application toohello backend pool.</span></span>

![partnerských řešení aplikací][6]

## <a name="finalize-configuration"></a><span data-ttu-id="a6819-137">Dokončení konfigurace</span><span class="sxs-lookup"><span data-stu-id="a6819-137">Finalize configuration</span></span>

<span data-ttu-id="a6819-138">Security Center sleduje aplikace tooan aplikační brány přidat jako chráněného prostředku.</span><span class="sxs-lookup"><span data-stu-id="a6819-138">Security Center tracks applications added tooan application gateway as a protected resource.</span></span>  <span data-ttu-id="a6819-139">Sleduje stav hello tento prostředek a zajistí, že je chráněn aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a6819-139">It monitors hello health of this resource and ensures that it is protected by an application gateway.</span></span> <span data-ttu-id="a6819-140">dalším krokem Hello je tooadd hello privátní IP, veřejnou IP adresu nebo síťové KARTĚ fondu back-end toohello virtuálního počítače brány aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a6819-140">hello next step is tooadd hello private IP, public IP, or NIC of your virtual machine toohello backend pool of hello application gateway.</span></span> <span data-ttu-id="a6819-141">Dokud se to provádí další doporučení **dokončit ochranu aplikace** se nezobrazí, dokud se přidá hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="a6819-141">Until this is done an additional recommendation of **Finalize application protection** is shown until hello resource is added.</span></span>

![okno Přidání brány firewall webových aplikací][5]

## <a name="security-alerts"></a><span data-ttu-id="a6819-143">Výstrahy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="a6819-143">Security Alerts</span></span>

<span data-ttu-id="a6819-144">V rámci Security Center přejděte příliš**detekce** > **výstrahy zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="a6819-144">Within Security Center navigate too**DETECTION** > **Security Alerts**.</span></span>  <span data-ttu-id="a6819-145">Zde můžete najít výstrahy firewall webových aplikací pro application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a6819-145">Here you find WAF alerts for your application gateways.</span></span> <span data-ttu-id="a6819-146">Výstrahy jsou rozdělené podle pravidla firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a6819-146">Alerts are broken down by WAF rule.</span></span>

![výstrahy zabezpečení][8]

<span data-ttu-id="a6819-148">Kliknutím na pravidlo poskytne seznam výstrah pro tuto konkrétní pravidlo firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a6819-148">Clicking an rule will provide a list of alerts for that specific WAF rule.</span></span> <span data-ttu-id="a6819-149">Každá výstraha zobrazí další podrobnosti o vyhledání hello.</span><span class="sxs-lookup"><span data-stu-id="a6819-149">Each alert shows additional details on hello finding.</span></span> <span data-ttu-id="a6819-150">Podrobnosti o Hello zadejte bránu odkaz toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6819-150">hello details provide a link toohello application gateway.</span></span>
 
![Podrobnosti výstrahy][9]

## <a name="next-steps"></a><span data-ttu-id="a6819-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6819-152">Next steps</span></span>

<span data-ttu-id="a6819-153">jak brány firewall webových aplikací tooenable na existující aplikační brány, navštivte toolearn [vytvoření nebo aktualizace služby Azure Application Gateway pomocí brány firewall webových aplikací](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span><span class="sxs-lookup"><span data-stu-id="a6819-153">toolearn how tooenable web application firewall on an existing application gateway, visit [Create or update an Azure Application Gateway with web application firewall](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span></span>

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png