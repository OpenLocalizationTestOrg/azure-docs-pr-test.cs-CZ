---
title: "Ověřování výstrah v Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže s ověřováním výstrah zabezpečení ve službě Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 121b5d8f023a9b663d0e7af26dce8f81db27672c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="alerts-validation-in-azure-security-center"></a><span data-ttu-id="c589f-103">Ověřování výstrah ve službě Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="c589f-103">Alerts Validation in Azure Security Center</span></span>
<span data-ttu-id="c589f-104">Pomocí tohoto dokumentu se naučíte ověřovat, jestli je váš systém správně nakonfigurovaný pro výstrahy služby Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="c589f-104">This document helps you learn how to verify if your system is properly configured for Azure Security Center alerts.</span></span>

## <a name="what-are-security-alerts"></a><span data-ttu-id="c589f-105">Co jsou výstrahy zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="c589f-105">What are security alerts?</span></span>
<span data-ttu-id="c589f-106">Služba Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure, sítě a připojených partnerských řešení, jako jsou brány firewall a řešení ochrany koncových bodů, aby zjistila hrozby a mohla vás na ně upozornit.</span><span class="sxs-lookup"><span data-stu-id="c589f-106">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, the network, and connected partner solutions, like firewall and endpoint protection solutions, to detect and alert you to threats.</span></span> <span data-ttu-id="c589f-107">Přečtěte si téma [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts), kde najdete další informace o výstrahách zabezpečení, a téma [Principy výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), kde najdete další informace o různých typech výstrah.</span><span class="sxs-lookup"><span data-stu-id="c589f-107">Read [Managing and responding to security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) for more information about security alerts, and read [Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) to learn more about the different types of alerts.</span></span>

## <a name="alert-validation"></a><span data-ttu-id="c589f-108">Ověřování výstrah</span><span class="sxs-lookup"><span data-stu-id="c589f-108">Alert validation</span></span>
<span data-ttu-id="c589f-109">Po nainstalování agenta Security Center do vašeho počítače postupujte na počítači, na kterém chcete mít napadený prostředek výstrahy, podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="c589f-109">After Security Center agent is installed on your computer, follow the steps below from the computer where you want to be the attacked resource of the alert:</span></span>

1. <span data-ttu-id="c589f-110">Zkopírujte spustitelný soubor (například calc.exe) na plochu počítače nebo do jiného adresáře, který vám vyhovuje.</span><span class="sxs-lookup"><span data-stu-id="c589f-110">Copy an executable (for example calc.exe) to the computer’s desktop, or other directory of your convenience.</span></span>
2. <span data-ttu-id="c589f-111">Přejmenujte tento soubor na **ASC_AlertTest_662jfi039N.exe**.</span><span class="sxs-lookup"><span data-stu-id="c589f-111">Rename this file to **ASC_AlertTest_662jfi039N.exe**.</span></span>
3. <span data-ttu-id="c589f-112">Otevřete příkazový řádek a spusťte tento soubor s argumentem (pouze vymyšlený název argumentu), například: *ASC_AlertTest_662jfi039N.exe -foo*</span><span class="sxs-lookup"><span data-stu-id="c589f-112">Open the command prompt and execute this file with an argument (just a fake argument name), such as: *ASC_AlertTest_662jfi039N.exe -foo*</span></span>
4. <span data-ttu-id="c589f-113">Počkejte 5 až 10 minut a otevřete výstrahy služby Security Center.</span><span class="sxs-lookup"><span data-stu-id="c589f-113">Wait 5 to 10 minutes and open Security Center Alerts.</span></span> <span data-ttu-id="c589f-114">Měla by se tam zobrazit výstraha podobá této:</span><span class="sxs-lookup"><span data-stu-id="c589f-114">There you should find an alert similar to following one:</span></span>

    ![Ověřování výstrah](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

<span data-ttu-id="c589f-116">Při kontrole této výstrahy ověřte, že se v poli Arguments Auditing Enabled (Auditování argumentů povoleno) zobrazuje hodnota true.</span><span class="sxs-lookup"><span data-stu-id="c589f-116">When reviewing this alert, make sure the field Arguments Auditing Enabled appears as true.</span></span> <span data-ttu-id="c589f-117">Pokud se zobrazuje hodnota false, je potřeba povolit auditování argumentů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c589f-117">If it shows false, you need to enable command-line arguments auditing.</span></span> <span data-ttu-id="c589f-118">Tuto možnost můžete povolit pomocí následujícího příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="c589f-118">You can enable this option using the following command line:</span></span>

<span data-ttu-id="c589f-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span><span class="sxs-lookup"><span data-stu-id="c589f-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span></span>


## <a name="see-also"></a><span data-ttu-id="c589f-120">Viz také</span><span class="sxs-lookup"><span data-stu-id="c589f-120">See also</span></span>
<span data-ttu-id="c589f-121">V tomto článku jste se seznámili s procesem ověřování výstrah.</span><span class="sxs-lookup"><span data-stu-id="c589f-121">This article introduced you to the alerts validation process.</span></span> <span data-ttu-id="c589f-122">Teď, když už jste obeznámeni s tímto ověřováním, zkuste následující články:</span><span class="sxs-lookup"><span data-stu-id="c589f-122">Now that you're familiar with this validation, try the following articles:</span></span>

* <span data-ttu-id="c589f-123">[Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span><span class="sxs-lookup"><span data-stu-id="c589f-123">[Managing and responding to security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span></span> <span data-ttu-id="c589f-124">Zjistěte, jak spravovat výstrahy a reagovat na incidenty zabezpečení ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="c589f-124">Learn how to manage alerts, and respond to security incidents in Security Center.</span></span>
* <span data-ttu-id="c589f-125">[Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="c589f-125">[Security health monitoring in Azure Security Center](security-center-monitoring.md).</span></span> <span data-ttu-id="c589f-126">Zjistěte, jak monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="c589f-126">Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="c589f-127">[Principy výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span><span class="sxs-lookup"><span data-stu-id="c589f-127">[Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span></span> <span data-ttu-id="c589f-128">Seznamte se s dalšími typy výstrah zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c589f-128">Learn about the different types of security alerts.</span></span>
* <span data-ttu-id="c589f-129">[Průvodce odstraňováním potíží pro službu Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span><span class="sxs-lookup"><span data-stu-id="c589f-129">[Azure Security Center Troubleshooting Guide](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span></span> <span data-ttu-id="c589f-130">Zjistěte, jak řešit běžné problémy ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="c589f-130">Learn how to troubleshoot common issues in Security Center.</span></span> 
* <span data-ttu-id="c589f-131">[Azure Security Center – nejčastější dotazy](security-center-faq.md).</span><span class="sxs-lookup"><span data-stu-id="c589f-131">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="c589f-132">Přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="c589f-132">Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="c589f-133">[Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/).</span><span class="sxs-lookup"><span data-stu-id="c589f-133">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="c589f-134">Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="c589f-134">Find blog posts about Azure security and compliance.</span></span>

