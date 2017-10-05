---
title: "Integrace brány aplikací s Azure Security Center | Microsoft Docs"
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
ms.openlocfilehash: 737cdff3140be68cf9d6d396b470dd09c65c52f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a><span data-ttu-id="7f2fd-103">Přehled integrace mezi aplikační brány a Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="7f2fd-103">Overview of integration between Application Gateway and Azure Security Center</span></span>

<span data-ttu-id="7f2fd-104">Zjistěte, jak aplikační brány a Security Center chránit prostředky vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-104">Learn how Application Gateway and Security Center help protect your web application resources.</span></span> <span data-ttu-id="7f2fd-105">Brány firewall webových aplikací aplikace brány (firewall webových aplikací) se integruje s [Security Center](../security-center/security-center-intro.md) zajistit bezproblémovou zobrazení, aby se zabránilo zjistit a reagovat na hrozby do nechráněných webových aplikací ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-105">Application gateway web application firewall (WAF) integrates with [Security Center](../security-center/security-center-intro.md) to provide a seamless view to prevent, detect and respond to threats to unprotected web applications in your environment.</span></span>

## <a name="overview"></a><span data-ttu-id="7f2fd-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="7f2fd-106">Overview</span></span>

<span data-ttu-id="7f2fd-107">Aplikace brány firewall webových aplikací je doporučeným v Centru zabezpečení pro ochranu před zneužitím a ohrožení zabezpečení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-107">Application Gateway WAF is a recommendation in Security Center for protecting web applications from exploits and vulnerabilities.</span></span> <span data-ttu-id="7f2fd-108">Web povoleno prostředky, které nejsou chráněné firewall webových aplikací se zobrazí v Centru zabezpečení jako doporučení vysokou závažností.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-108">Web enabled resources that are not protected by WAF show in the security center as high severity recommendations.</span></span> <span data-ttu-id="7f2fd-109">Doporučení pro brány firewall webové aplikace se zobrazí na **přehled** v části **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-109">Recommendations for web application firewalls are shown on the **Overview** page, under **Applications**.</span></span>

![Integrace s security center][1]

<span data-ttu-id="7f2fd-111">Kliknutím na tlačítko všechna doporučení, otevře se nové okno s podrobnostmi o doporučení týkající brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-111">Clicking any recommendations regarding web application firewall opens a new blade showing the details of the recommendation.</span></span>

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a><span data-ttu-id="7f2fd-112">Přidání brány firewall webových aplikací do stávajícího prostředku</span><span class="sxs-lookup"><span data-stu-id="7f2fd-112">Add a web application firewall to an existing resource</span></span>

<span data-ttu-id="7f2fd-113">Přejděte na **další služby** > **zabezpečení a identita** > **Security Center** a na **Security Center – přehled**  okně klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-113">Navigate to **More Services** > **Security + Identity** > **Security Center** and on the **Security Center - Overview** blade, click **Applications**.</span></span> <span data-ttu-id="7f2fd-114">Na **Security Center – aplikace** okně Tabulka obsahuje seznam aplikací, které Security Center zjistila v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-114">On the **Security Center - Applications** blade, the table contains a list of applications that Security Center detected in your subscription.</span></span>

![webové aplikace][3]

<span data-ttu-id="7f2fd-116">Kliknutím na webovou aplikaci s kritický problém, můžete získat **stav zabezpečení aplikací** okno.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-116">By clicking on a web application with a critical issue, you get the **Application security health** blade.</span></span> <span data-ttu-id="7f2fd-117">Na obrázku níže, webové aplikace, který není chráněný pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-117">In the image below, the web application that is not protected by a web application firewall.</span></span> 

![webové prostředky, které nejsou chráněny.][2]

<span data-ttu-id="7f2fd-119">Klikněte na tlačítko **přidání brány firewall webových aplikací** pod **doporučení** otevřete **přidání brány Firewall webových aplikací** okno.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-119">Click **Add a web application firewall** under **Recommendations** to open the **Add a Web Application Firewall** blade.</span></span>

<span data-ttu-id="7f2fd-120">Pokud není máte existující aplikační bránu, nebo chcete vytvořit nový, klikněte na tlačítko **vytvořit nový** a na **vytvoření nové brány Firewall webových aplikací** a klikněte na **Microsoft – aplikace Brána**.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-120">If you do not have an existing Application Gateway, or want to create a new one, click **Create New** and on the **Create a new Web Application Firewall** blade, and click **Microsoft - Application Gateway**.</span></span> <span data-ttu-id="7f2fd-121">To vás provede kroky k vytvoření aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-121">This takes you through the steps to create an application gateway.</span></span> <span data-ttu-id="7f2fd-122">Webové aplikace je nyní přidána jako chráněný prostředek, Security Center nyní sleduje tento prostředek je chráněn brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-122">At this point, your web application is added as a protected resource, Security Center now tracks that this resource is protected by a web application firewall.</span></span> <span data-ttu-id="7f2fd-123">To se nepřidá jako člen fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-123">This does not add it as a backend pool member.</span></span>

<span data-ttu-id="7f2fd-124">Pokud máte existující aplikační brány, můžete ho **použít existující řešení**</span><span class="sxs-lookup"><span data-stu-id="7f2fd-124">If you have an existing application gateway, you can choose it under **Use existing solution**</span></span>

![okno Přidání brány firewall webových aplikací][4]

<span data-ttu-id="7f2fd-126">Přidání webové aplikace do služby application gateway pomocí Security Center nepřidá prostředek jako člen fondu back-end, musí provést na prostředek služby application gateway přímo.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-126">Adding a web application to an application gateway through Security Center does not add the resource as a backend pool member, this must be done on the application gateway resource directly.</span></span>

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a><span data-ttu-id="7f2fd-127">Přidání prostředku existující brány firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="7f2fd-127">Add a resource to an existing web application firewall</span></span>

<span data-ttu-id="7f2fd-128">Přejděte na **další služby** > **zabezpečení a identita** > **Security Center** a na **Security Center – přehled**  okně klikněte na tlačítko **Partner solutions**.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-128">Navigate to **More Services** > **Security + Identity** > **Security Center** and on the **Security Center - Overview** blade, click **Partner solutions**.</span></span> <span data-ttu-id="7f2fd-129">V zobrazovat existující brány aplikace podporující Security Center **partnerských řešení** okno.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-129">Existing Security Center aware application gateways show in the **Partner Solutions** blade.</span></span>

![Partnerská řešení][7]

<span data-ttu-id="7f2fd-131">Klikněte na tlačítko **odkaz aplikace** otevřete **odkaz aplikace** okně zde budete mít možnost vybrat stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-131">Click **Link app** to open the **Link Applications** blade, here you are given the options to select existing applications.</span></span> <span data-ttu-id="7f2fd-132">Vyberte aplikace, které chcete chránit a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-132">Choose the applications to protect and click **OK**.</span></span> <span data-ttu-id="7f2fd-133">To nepřidává žádné další webové aplikace na fond back-end službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-133">This does not add the web application to the backend pool of the application gateway.</span></span> <span data-ttu-id="7f2fd-134">Toto nastaví prostředky jako chráněný prostředek, Security Center můžete sledovat jeho.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-134">This sets the resources as a protected resource so Security Center can track it.</span></span> <span data-ttu-id="7f2fd-135">Pokud chcete přidat jako člena fondu back-end prostředku, musí provést ve službě application gateway, v okně aktuální, můžete kliknout na **řešení konzoly** potřeba provést k prostředku aplikační brány, kde můžete přidat webovou aplikaci na fond back-end.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-135">To add the resource as a backend pool member, this must be done on the application gateway, from the current blade you can click **Solution console** to be taken to the application gateway resource where you can add the web application to the backend pool.</span></span>

![partnerských řešení aplikací][6]

## <a name="finalize-configuration"></a><span data-ttu-id="7f2fd-137">Dokončení konfigurace</span><span class="sxs-lookup"><span data-stu-id="7f2fd-137">Finalize configuration</span></span>

<span data-ttu-id="7f2fd-138">Security Center zjišťuje aplikace, které jsou přidány do služby application gateway jako chráněný prostředek.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-138">Security Center tracks applications added to an application gateway as a protected resource.</span></span>  <span data-ttu-id="7f2fd-139">Sleduje stav tohoto prostředku a zajistí, že je chráněn aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-139">It monitors the health of this resource and ensures that it is protected by an application gateway.</span></span> <span data-ttu-id="7f2fd-140">Dalším krokem je přidání privátní IP, veřejnou IP adresu nebo síťový adaptér virtuálního počítače do fondu back-end aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-140">The next step is to add the private IP, public IP, or NIC of your virtual machine to the backend pool of the application gateway.</span></span> <span data-ttu-id="7f2fd-141">Dokud se to provádí další doporučení **dokončit ochranu aplikace** se nezobrazí, dokud se přidá k prostředku.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-141">Until this is done an additional recommendation of **Finalize application protection** is shown until the resource is added.</span></span>

![okno Přidání brány firewall webových aplikací][5]

## <a name="security-alerts"></a><span data-ttu-id="7f2fd-143">Výstrahy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7f2fd-143">Security Alerts</span></span>

<span data-ttu-id="7f2fd-144">Přechod na v Security Center **detekce** > **výstrahy zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-144">Within Security Center navigate to **DETECTION** > **Security Alerts**.</span></span>  <span data-ttu-id="7f2fd-145">Zde můžete najít výstrahy firewall webových aplikací pro application Gateway.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-145">Here you find WAF alerts for your application gateways.</span></span> <span data-ttu-id="7f2fd-146">Výstrahy jsou rozdělené podle pravidla firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-146">Alerts are broken down by WAF rule.</span></span>

![výstrahy zabezpečení][8]

<span data-ttu-id="7f2fd-148">Kliknutím na pravidlo poskytne seznam výstrah pro tuto konkrétní pravidlo firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-148">Clicking an rule will provide a list of alerts for that specific WAF rule.</span></span> <span data-ttu-id="7f2fd-149">Každá výstraha zobrazí další podrobnosti o zjištění.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-149">Each alert shows additional details on the finding.</span></span> <span data-ttu-id="7f2fd-150">Podrobnosti zadejte odkaz na službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="7f2fd-150">The details provide a link to the application gateway.</span></span>
 
![Podrobnosti výstrahy][9]

## <a name="next-steps"></a><span data-ttu-id="7f2fd-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f2fd-152">Next steps</span></span>

<span data-ttu-id="7f2fd-153">Informace o povolení brány firewall webových aplikací na existující aplikační brány, najdete [vytvoření nebo aktualizace služby Azure Application Gateway pomocí brány firewall webových aplikací](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span><span class="sxs-lookup"><span data-stu-id="7f2fd-153">To learn how to enable web application firewall on an existing application gateway, visit [Create or update an Azure Application Gateway with web application firewall](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span></span>

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png