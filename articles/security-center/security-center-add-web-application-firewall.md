---
title: "Přidání brány firewall webových aplikací v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak implementovat Azure Security Center doporučení ** přidat webové aplikace brány firewall ** a ** dokončení ochrany aplikace **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: terrylan
ms.openlocfilehash: d04a07237029953d8a9b20704d85e852ce45d867
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a><span data-ttu-id="9d504-103">Přidání brány firewall webových aplikací v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="9d504-103">Add a web application firewall in Azure Security Center</span></span>
<span data-ttu-id="9d504-104">Azure Security Center může doporučujeme, abyste přidali brány firewall webových aplikací (firewall webových aplikací) z partnera společnosti Microsoft k zabezpečení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9d504-104">Azure Security Center may recommend that you add a web application firewall (WAF) from a Microsoft partner to secure your web applications.</span></span> <span data-ttu-id="9d504-105">Tento dokument vás příklad toho, jak použít toto doporučení provede.</span><span class="sxs-lookup"><span data-stu-id="9d504-105">This document walks you through an example of how to apply this recommendation.</span></span>

<span data-ttu-id="9d504-106">Doporučení firewall webových aplikací je zobrazený pro všechny veřejné přístupných IP adresy (IP úrovni Instance nebo IP skupinu s vyrovnáváním zatížení), skupinu zabezpečení sítě spojenou s otevřete příchozí webovými porty (80,443).</span><span class="sxs-lookup"><span data-stu-id="9d504-106">A WAF recommendation is shown for any public facing IP (either Instance Level IP or Load Balanced IP) that has an associated network security group with open inbound web ports (80,443).</span></span>

<span data-ttu-id="9d504-107">Security Center doporučuje zřízení firewall webových aplikací, které pomáhají bránit proti útokům na cílení na vaše webové aplikace na virtuální počítače a služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="9d504-107">Security Center recommends that you provision a WAF to help defend against attacks targeting your web applications on virtual machines and on App Service Environment.</span></span> <span data-ttu-id="9d504-108">Je aplikaci služby prostředí (App Service Environment) [Premium](https://azure.microsoft.com/pricing/details/app-service/) služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="9d504-108">An App Service Environment (ASE) is a [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps.</span></span> <span data-ttu-id="9d504-109">Další informace o App Service Environment, najdete v článku [dokumentace k aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="9d504-109">To learn more about ASE, see the [App Service Environment Documentation](../app-service/app-service-app-service-environments-readme.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9d504-110">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="9d504-110">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="9d504-111">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="9d504-111">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="9d504-112">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="9d504-112">Implement the recommendation</span></span>
1. <span data-ttu-id="9d504-113">V **doporučení** vyberte **zabezpečení webové aplikace pomocí brány firewall webových aplikací**.</span><span class="sxs-lookup"><span data-stu-id="9d504-113">In the **Recommendations** blade, select **Secure web application using web application firewall**.</span></span>
   <span data-ttu-id="9d504-114">![Zabezpečení webové aplikace][1]</span><span class="sxs-lookup"><span data-stu-id="9d504-114">![Secure web Application][1]</span></span>
2. <span data-ttu-id="9d504-115">V **zabezpečení webových aplikací pomocí brány firewall webových aplikací** okně vyberte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9d504-115">In the **Secure your web applications using web application firewall** blade, select a web application.</span></span> <span data-ttu-id="9d504-116">**Přidání brány Firewall webových aplikací** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="9d504-116">The **Add a Web Application Firewall** blade opens.</span></span>
   <span data-ttu-id="9d504-117">![Přidání brány firewall webových aplikací][2]</span><span class="sxs-lookup"><span data-stu-id="9d504-117">![Add a web application firewall][2]</span></span>
3. <span data-ttu-id="9d504-118">Můžete použít existující brány firewall webových aplikací, pokud je k dispozici nebo můžete vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="9d504-118">You can choose to use an existing web application firewall if available or you can create a new one.</span></span> <span data-ttu-id="9d504-119">V tomto příkladu nejsou žádné existující WAFs k dispozici, vytvoříme firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9d504-119">In this example, there are no existing WAFs available so we create a WAF.</span></span>
4. <span data-ttu-id="9d504-120">Pokud chcete vytvořit firewall webových aplikací, vyberte ze seznamu partnerů integrované řešení.</span><span class="sxs-lookup"><span data-stu-id="9d504-120">To create a WAF, select a solution from the list of integrated partners.</span></span> <span data-ttu-id="9d504-121">V tomto příkladu jsme vyberte **brány Firewall webových aplikací Barracuda**.</span><span class="sxs-lookup"><span data-stu-id="9d504-121">In this example, we select **Barracuda Web Application Firewall**.</span></span>
5. <span data-ttu-id="9d504-122">**Brány Firewall webových aplikací Barracuda** otevře se okno, takže získáte informace o partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="9d504-122">The **Barracuda Web Application Firewall** blade opens providing you information about the partner solution.</span></span> <span data-ttu-id="9d504-123">Vyberte **vytvořit** v okně informace.</span><span class="sxs-lookup"><span data-stu-id="9d504-123">Select **Create** in the information blade.</span></span>

   ![Okno informace o brány firewall][3]

6. <span data-ttu-id="9d504-125">**Nové brány Firewall webových aplikací** otevře se okno, kde můžete provádět **konfigurace virtuálního počítače** kroky a zadejte **firewall webových aplikací informace**.</span><span class="sxs-lookup"><span data-stu-id="9d504-125">The **New Web Application Firewall** blade opens, where you can perform **VM Configuration** steps and provide **WAF Information**.</span></span> <span data-ttu-id="9d504-126">Vyberte **konfigurace virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="9d504-126">Select **VM Configuration**.</span></span>
7. <span data-ttu-id="9d504-127">V **konfigurace virtuálního počítače** okno, zadejte požadované informace o číselníku virtuálního počítače, který běží firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9d504-127">In the **VM Configuration** blade, you enter information required to spin up the virtual machine that runs the WAF.</span></span>
   <span data-ttu-id="9d504-128">![Konfigurace virtuálního počítače][4]</span><span class="sxs-lookup"><span data-stu-id="9d504-128">![VM configuration][4]</span></span>
8. <span data-ttu-id="9d504-129">Vraťte se na **nové brány Firewall webových aplikací** a vyberte **firewall webových aplikací informace**.</span><span class="sxs-lookup"><span data-stu-id="9d504-129">Return to the **New Web Application Firewall** blade and select **WAF Information**.</span></span> <span data-ttu-id="9d504-130">V **firewall webových aplikací informace** okně nakonfigurujete firewall webových aplikací, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="9d504-130">In the **WAF Information** blade, you configure the WAF itself.</span></span> <span data-ttu-id="9d504-131">Krok 7 umožňuje nakonfigurovat virtuální počítač, na kterém běží firewall webových aplikací a krok 8 umožňuje zřídit firewall webových aplikací, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="9d504-131">Step 7 allows you to configure the virtual machine on which the WAF runs and step 8 enables you to provision the WAF itself.</span></span>

## <a name="finalize-application-protection"></a><span data-ttu-id="9d504-132">Dokončit ochranu aplikace</span><span class="sxs-lookup"><span data-stu-id="9d504-132">Finalize application protection</span></span>
1. <span data-ttu-id="9d504-133">Vraťte se na **doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="9d504-133">Return to the **Recommendations** blade.</span></span> <span data-ttu-id="9d504-134">Nový záznam vygenerovalo po vytvoření firewall webových aplikací, nazývá **dokončit ochranu aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9d504-134">A new entry was generated after you created the WAF, called **Finalize application protection**.</span></span> <span data-ttu-id="9d504-135">Tato položka způsobem zjistíte, že je potřeba dokončit proces ve skutečnosti připojení firewall webových aplikací v rámci virtuální sítě Azure tak, aby ho může chránit aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d504-135">This entry lets you know that you need to complete the process of actually wiring up the WAF within the Azure Virtual Network so that it can protect the application.</span></span>

   ![Dokončit ochranu aplikace][5]

2. <span data-ttu-id="9d504-137">Vyberte **dokončit ochranu aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9d504-137">Select **Finalize application protection**.</span></span> <span data-ttu-id="9d504-138">Otevře se nové okno.</span><span class="sxs-lookup"><span data-stu-id="9d504-138">A new blade opens.</span></span> <span data-ttu-id="9d504-139">Uvidíte, že se webové aplikace, které musí mít jeho provoz přesměruje.</span><span class="sxs-lookup"><span data-stu-id="9d504-139">You can see that there is a web application that needs to have its traffic rerouted.</span></span>
3. <span data-ttu-id="9d504-140">Vyberte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9d504-140">Select the web application.</span></span> <span data-ttu-id="9d504-141">Otevře se okno umožňující kroky pro dokončení instalace brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9d504-141">A blade opens that gives you steps for finalizing the web application firewall setup.</span></span> <span data-ttu-id="9d504-142">Proveďte kroky a pak vyberte **omezení přenosu**.</span><span class="sxs-lookup"><span data-stu-id="9d504-142">Complete the steps, and then select **Restrict traffic**.</span></span> <span data-ttu-id="9d504-143">Security Center pak provede telefonického kabeláž.</span><span class="sxs-lookup"><span data-stu-id="9d504-143">Security Center then does the wiring-up for you.</span></span>

   ![Omezení přenosu][6]

> [!NOTE]
> <span data-ttu-id="9d504-145">Přidáním těchto aplikací na vaše stávající nasazení firewall webových aplikací můžete chránit několika webových aplikací ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="9d504-145">You can protect multiple web applications in Security Center by adding these applications to your existing WAF deployments.</span></span>
>
>

<span data-ttu-id="9d504-146">Nyní jsou plně integrované protokoly z této firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9d504-146">The logs from that WAF are now fully integrated.</span></span> <span data-ttu-id="9d504-147">Security Center můžete spustit automaticky shromažďování a analyzování protokolů tak, aby vám ho můžete surface výstrahy důležité zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9d504-147">Security Center can start automatically gathering and analyzing the logs so that it can surface important security alerts to you.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d504-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d504-148">Next steps</span></span>
<span data-ttu-id="9d504-149">Tento dokument vám ukázal, jak provést doporučení Security Center "Přidat webovou aplikaci."</span><span class="sxs-lookup"><span data-stu-id="9d504-149">This document showed you how to implement the Security Center recommendation "Add a web application."</span></span> <span data-ttu-id="9d504-150">Další informace o konfiguraci brány firewall webových aplikací, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="9d504-150">To learn more about configuring a web application firewall, see the following:</span></span>

* [<span data-ttu-id="9d504-151">Konfigurace brány Firewall webových aplikací (firewall webových aplikací) pro služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="9d504-151">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

<span data-ttu-id="9d504-152">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="9d504-152">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="9d504-153">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="9d504-153">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="9d504-154">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="9d504-154">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="9d504-155">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="9d504-155">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="9d504-156">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="9d504-156">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="9d504-157">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="9d504-157">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="9d504-158">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="9d504-158">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
