---
title: "aaaAlerts ověřování v Azure Security Center | Microsoft Docs"
description: "Tento dokument vám pomůže toovalidate hello výstrah zabezpečení v Azure Security Center."
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
ms.openlocfilehash: 030e9e74303758192eedaf517f1cb0d2e4a7852e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="alerts-validation-in-azure-security-center"></a><span data-ttu-id="5d9dc-103">Ověřování výstrah ve službě Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="5d9dc-103">Alerts Validation in Azure Security Center</span></span>
<span data-ttu-id="5d9dc-104">Tento dokument vám pomůže se naučit jak tooverify Pokud váš systém byl správně nakonfigurován pro Azure Security Center výstrahy.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-104">This document helps you learn how tooverify if your system is properly configured for Azure Security Center alerts.</span></span>

## <a name="what-are-security-alerts"></a><span data-ttu-id="5d9dc-105">Co jsou výstrahy zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="5d9dc-105">What are security alerts?</span></span>
<span data-ttu-id="5d9dc-106">Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vaše prostředky Azure, sítě hello a připojených partnerských řešení, jako jsou brány firewall a endpoint protection řešení, toodetect a výstrah toothreats.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-106">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and connected partner solutions, like firewall and endpoint protection solutions, toodetect and alert you toothreats.</span></span> <span data-ttu-id="5d9dc-107">Čtení [správy a zda odpovídá toosecurity výstrahy v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) Další informace o výstrahách zabezpečení a čtení [pochopení výstrah zabezpečení v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) toolearn další o hello různé typy výstrah.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-107">Read [Managing and responding toosecurity alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) for more information about security alerts, and read [Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) toolearn more about hello different types of alerts.</span></span>

## <a name="alert-validation"></a><span data-ttu-id="5d9dc-108">Ověřování výstrah</span><span class="sxs-lookup"><span data-stu-id="5d9dc-108">Alert validation</span></span>
<span data-ttu-id="5d9dc-109">Po instalaci agenta Security Center ve vašem počítači, postupujte podle hello kroků z počítače hello místo prostředků hello napadení toobe hello výstrahy:</span><span class="sxs-lookup"><span data-stu-id="5d9dc-109">After Security Center agent is installed on your computer, follow hello steps below from hello computer where you want toobe hello attacked resource of hello alert:</span></span>

1. <span data-ttu-id="5d9dc-110">Zkopírujte plochu spustitelný soubor (pro příklad calc.exe) toohello počítače nebo jiného adresáře usnadnění vaší práce.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-110">Copy an executable (for example calc.exe) toohello computer’s desktop, or other directory of your convenience.</span></span>
2. <span data-ttu-id="5d9dc-111">Přejmenujte tento soubor příliš**ASC_AlertTest_662jfi039N.exe**.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-111">Rename this file too**ASC_AlertTest_662jfi039N.exe**.</span></span>
3. <span data-ttu-id="5d9dc-112">Otevřete příkazový řádek se hello a spusťte tento soubor s parametrem (pouze název falešných argument), jako například: *ASC_AlertTest_662jfi039N.exe - foo*</span><span class="sxs-lookup"><span data-stu-id="5d9dc-112">Open hello command prompt and execute this file with an argument (just a fake argument name), such as: *ASC_AlertTest_662jfi039N.exe -foo*</span></span>
4. <span data-ttu-id="5d9dc-113">Počkejte 5 minut too10 a otevřete výstrahy Security Center.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-113">Wait 5 too10 minutes and open Security Center Alerts.</span></span> <span data-ttu-id="5d9dc-114">Existuje byste měli najít výstrahy podobné toofollowing, jeden:</span><span class="sxs-lookup"><span data-stu-id="5d9dc-114">There you should find an alert similar toofollowing one:</span></span>

    ![Ověřování výstrah](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

<span data-ttu-id="5d9dc-116">Při revizi tuto výstrahu, zkontrolujte, zda pole hello povoleno auditování argumenty, zobrazí se jako true.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-116">When reviewing this alert, make sure hello field Arguments Auditing Enabled appears as true.</span></span> <span data-ttu-id="5d9dc-117">Pokud se zobrazí hodnotu false, je třeba argumenty příkazového řádku tooenable auditování.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-117">If it shows false, you need tooenable command-line arguments auditing.</span></span> <span data-ttu-id="5d9dc-118">Můžete povolit tuto možnost, pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5d9dc-118">You can enable this option using hello following command line:</span></span>

<span data-ttu-id="5d9dc-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span><span class="sxs-lookup"><span data-stu-id="5d9dc-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span></span>


## <a name="see-also"></a><span data-ttu-id="5d9dc-120">Viz také</span><span class="sxs-lookup"><span data-stu-id="5d9dc-120">See also</span></span>
<span data-ttu-id="5d9dc-121">Tento článek se zavedl toohello výstrahy ověření procesu.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-121">This article introduced you toohello alerts validation process.</span></span> <span data-ttu-id="5d9dc-122">Teď, když jste obeznámeni s toto ověření, zkuste hello následující články:</span><span class="sxs-lookup"><span data-stu-id="5d9dc-122">Now that you're familiar with this validation, try hello following articles:</span></span>

* <span data-ttu-id="5d9dc-123">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span><span class="sxs-lookup"><span data-stu-id="5d9dc-123">[Managing and responding toosecurity alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span></span> <span data-ttu-id="5d9dc-124">Zjistěte, jak toomanage výstrah a incidentů toosecurity reakce ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-124">Learn how toomanage alerts, and respond toosecurity incidents in Security Center.</span></span>
* <span data-ttu-id="5d9dc-125">[Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="5d9dc-125">[Security health monitoring in Azure Security Center](security-center-monitoring.md).</span></span> <span data-ttu-id="5d9dc-126">Zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-126">Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="5d9dc-127">[Principy výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span><span class="sxs-lookup"><span data-stu-id="5d9dc-127">[Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span></span> <span data-ttu-id="5d9dc-128">Další informace o různých typech hello výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-128">Learn about hello different types of security alerts.</span></span>
* <span data-ttu-id="5d9dc-129">[Průvodce odstraňováním potíží pro službu Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span><span class="sxs-lookup"><span data-stu-id="5d9dc-129">[Azure Security Center Troubleshooting Guide](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span></span> <span data-ttu-id="5d9dc-130">Zjistěte, jak tootroubleshoot běžné problémy ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-130">Learn how tootroubleshoot common issues in Security Center.</span></span> 
* <span data-ttu-id="5d9dc-131">[Azure Security Center – nejčastější dotazy](security-center-faq.md).</span><span class="sxs-lookup"><span data-stu-id="5d9dc-131">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="5d9dc-132">Nejčastější dotazy o použití hello služby najít.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-132">Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="5d9dc-133">[Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/).</span><span class="sxs-lookup"><span data-stu-id="5d9dc-133">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="5d9dc-134">Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="5d9dc-134">Find blog posts about Azure security and compliance.</span></span>

