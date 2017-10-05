---
title: "Přehled služby Azure Automation DSC | Microsoft Docs"
description: "Přehled o DSC Azure Automation požadovaného stavu konfigurace (), podmínky a známé problémy"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "prostředí PowerShell dsc, konfigurace požadovaného stavu prostředí powershell dsc azure"
ms.assetid: fd40cb68-c1a6-48c3-bba2-710b607d1555
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: 468321fa6863d78bc0d179fbe5c2ed6195040d50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-dsc-overview"></a><span data-ttu-id="b533f-104">Přehled služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="b533f-104">Azure Automation DSC Overview</span></span>

<span data-ttu-id="b533f-105">Azure Automation DSC je služba Azure, který umožňuje zapisovat a spravovat zkompilovat PowerShell požadovaného stavu konfigurace (DSC) [konfigurace](https://msdn.microsoft.com/powershell/dsc/configurations), import [prostředky DSC](https://msdn.microsoft.com/powershell/dsc/resources)a přiřadit konfigurace pro cílové uzly, všechny v cloudu.</span><span class="sxs-lookup"><span data-stu-id="b533f-105">Azure Automation DSC is an Azure service that allows you to write, manage, and compile PowerShell Desired State Configuration (DSC) [configurations](https://msdn.microsoft.com/powershell/dsc/configurations), import [DSC Resources](https://msdn.microsoft.com/powershell/dsc/resources), and assign configurations to target nodes, all in the cloud.</span></span>

## <a name="why-use-azure-automation-dsc"></a><span data-ttu-id="b533f-106">Proč používat Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b533f-106">Why use Azure Automation DSC</span></span>

<span data-ttu-id="b533f-107">Azure Automation DSC poskytuje několik výhod oproti použití DSC mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="b533f-107">Azure Automation DSC provides several advantages over using DSC outside of Azure.</span></span>

### <a name="built-in-pull-server"></a><span data-ttu-id="b533f-108">Předdefinované načítacího serveru</span><span class="sxs-lookup"><span data-stu-id="b533f-108">Built-in pull server</span></span>

<span data-ttu-id="b533f-109">Poskytuje služby Azure Automation [server DSC za](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) tak, aby cílové uzly automaticky zobrazí konfigurace, podřídit se požadované stavu a zpětně hlásit dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="b533f-109">Azure Automation provides a [DSC pull server](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) so that target nodes automatically receive configurations, conform to the desired state, and report back on their compliance.</span></span>
<span data-ttu-id="b533f-110">Předdefinované vyžádání serveru ve službě Azure Automation se eliminuje potřeba nastavit a spravovat vlastní server vyžádané replikace.</span><span class="sxs-lookup"><span data-stu-id="b533f-110">The built-in pull server in Azure Automation eliminates the need to set up and maintain your own pull server.</span></span>
<span data-ttu-id="b533f-111">Služby Azure Automation, můžete vybrat virtuální nebo fyzická systému Windows nebo Linux počítače, v cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="b533f-111">Azure Automation can target virtual or physical Windows or Linux machines, in the cloud or on-premises.</span></span>

### <a name="management-of-all-your-dsc-artifacts"></a><span data-ttu-id="b533f-112">Správa všech artefaktů DSC</span><span class="sxs-lookup"><span data-stu-id="b533f-112">Management of all your DSC artifacts</span></span>

<span data-ttu-id="b533f-113">Azure Automation DSC přináší stejné vrstva správy [konfigurace požadovaného stavu prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview) jako Azure Automation nabízí skriptů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b533f-113">Azure Automation DSC brings the same management layer to [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) as Azure Automation offers for PowerShell scripting.</span></span>

<span data-ttu-id="b533f-114">Z portálu Azure nebo z prostředí PowerShell můžete spravovat všechny vaše DSC konfigurace, prostředky a cílové uzly.</span><span class="sxs-lookup"><span data-stu-id="b533f-114">From the Azure portal, or from PowerShell, you can manage all your DSC configurations, resources, and target nodes.</span></span>

![Snímek obrazovky okna Azure Automation.](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a><span data-ttu-id="b533f-116">Importovat data pro generování sestav do analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="b533f-116">Import reporting data into Log Analytics</span></span>

<span data-ttu-id="b533f-117">Uzly, které budou spravovat pomocí Azure Automation DSC poslat podrobné údaje o chybách stav předdefinované načítacího serveru.</span><span class="sxs-lookup"><span data-stu-id="b533f-117">Nodes that are managed with Azure Automation DSC send detailed reporting status data to the built-in pull server.</span></span>
<span data-ttu-id="b533f-118">Můžete nakonfigurovat Azure Automation DSC k odesílat tato data do pracovního prostoru analýzy protokolů Microsoft Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="b533f-118">You can configure Azure Automation DSC to send this data to your Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>
<span data-ttu-id="b533f-119">Zjistěte, jak odesílat data o stavu DSC do pracovního prostoru analýzy protokolů, najdete v tématu [dál Azure Automation DSC data pro vytváření sestav k analýze protokolů OMS](automation-dsc-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="b533f-119">To learn how to send DSC status data to your Log Analytics workspace, see [Forward Azure Automation DSC reporting data to OMS Log Analytics](automation-dsc-diagnostics.md).</span></span>

## <a name="introduction-video"></a><span data-ttu-id="b533f-120">Úvodní video</span><span class="sxs-lookup"><span data-stu-id="b533f-120">Introduction video</span></span>

<span data-ttu-id="b533f-121">Raději se díváte, než čtete?</span><span class="sxs-lookup"><span data-stu-id="b533f-121">Prefer watching to reading?</span></span> <span data-ttu-id="b533f-122">Podíváme se na následující videa z květen 2015, když se poprvé oznámeno Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b533f-122">Have a look at the following video from May 2015, when Azure Automation DSC was first announced.</span></span>

>[!NOTE]
><span data-ttu-id="b533f-123">Koncepty a životního cyklu, které jsou popsané v tomto videu jsou sice správné, Azure Automation DSC má posunula hodně dopředu a vzhledem k tomu, že záznamu tohoto videa.</span><span class="sxs-lookup"><span data-stu-id="b533f-123">While the concepts and life cycle discussed in this video are correct, Azure Automation DSC has progressed a lot since this video was recorded.</span></span>
><span data-ttu-id="b533f-124">Nyní je obecně k dispozici, má mnohem rozsáhlejší uživatelské rozhraní na portálu Azure a podporuje mnoho další funkce.</span><span class="sxs-lookup"><span data-stu-id="b533f-124">It is now generally available, has a much more extensive UI in the Azure portal, and supports many additional capabilities.</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a><span data-ttu-id="b533f-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b533f-125">Next steps</span></span>

* <span data-ttu-id="b533f-126">Další informace jak pro zařadit uzly pro správu ve službě Azure Automation DSC, najdete v části [registrace počítačů pro správu Azure Automation DSC.](automation-dsc-onboarding.md)</span><span class="sxs-lookup"><span data-stu-id="b533f-126">To learn how to onboard nodes to be managed with Azure Automation DSC, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md)</span></span>
* <span data-ttu-id="b533f-127">Chcete-li začít používat Azure Automation DSC, přečtěte si téma [Začínáme s Azure Automation DSC.](automation-dsc-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="b533f-127">To get started using Azure Automation DSC, see [Getting started with Azure Automation DSC](automation-dsc-getting-started.md)</span></span>
* <span data-ttu-id="b533f-128">Další informace o kompilaci konfigurace DSC, takže můžete je přiřadit k cílové uzly najdete v tématu [kompilování konfigurace v Azure Automation DSC.](automation-dsc-compile.md)</span><span class="sxs-lookup"><span data-stu-id="b533f-128">To learn about compiling DSC configurations so that you can assign them to target nodes, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md)</span></span>
* <span data-ttu-id="b533f-129">Referenční informace o rutinách prostředí PowerShell pro Azure Automation DSC, najdete v části [rutiny Azure Automation DSC.](/powershell/module/azurerm.automation/#automation)</span><span class="sxs-lookup"><span data-stu-id="b533f-129">For PowerShell cmdlet reference for Azure Automation DSC, see [Azure Automation DSC cmdlets](/powershell/module/azurerm.automation/#automation)</span></span>
* <span data-ttu-id="b533f-130">Informace o cenách naleznete v tématu [ceny Azure Automation DSC.](https://azure.microsoft.com/pricing/details/automation/)</span><span class="sxs-lookup"><span data-stu-id="b533f-130">For pricing information, see [Azure Automation DSC pricing](https://azure.microsoft.com/pricing/details/automation/)</span></span>
* <span data-ttu-id="b533f-131">Příklad použití Azure Automation DSC v kanálu průběžné nasazování, najdete v sekci [průběžné nasazování a Chocolatey IaaS virtuálních počítačů pomocí Azure Automation DSC](automation-dsc-cd-chocolatey.md)</span><span class="sxs-lookup"><span data-stu-id="b533f-131">To see an example of using Azure Automation DSC in a continuous deployment pipeline, see  [Continuous Deployment to IaaS VMs Using Azure Automation DSC and Chocolatey](automation-dsc-cd-chocolatey.md)</span></span>