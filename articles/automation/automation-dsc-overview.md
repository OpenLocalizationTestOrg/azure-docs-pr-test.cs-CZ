---
title: "aaaAzure Automation DSC přehled | Microsoft Docs"
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
ms.openlocfilehash: 5b8e5104c7b5bed848c015ac26a8b7d1f5b24de9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-dsc-overview"></a><span data-ttu-id="e1d74-104">Přehled služby Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="e1d74-104">Azure Automation DSC Overview</span></span>

<span data-ttu-id="e1d74-105">Azure Automation DSC je služba Azure, který vám umožní toowrite, spravovat a kompilovat PowerShell požadovaného stavu konfigurace (DSC) [konfigurace](https://msdn.microsoft.com/powershell/dsc/configurations), import [prostředky DSC](https://msdn.microsoft.com/powershell/dsc/resources)a přiřaďte Konfigurace uzlů tootarget, všechny v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="e1d74-105">Azure Automation DSC is an Azure service that allows you toowrite, manage, and compile PowerShell Desired State Configuration (DSC) [configurations](https://msdn.microsoft.com/powershell/dsc/configurations), import [DSC Resources](https://msdn.microsoft.com/powershell/dsc/resources), and assign configurations tootarget nodes, all in hello cloud.</span></span>

## <a name="why-use-azure-automation-dsc"></a><span data-ttu-id="e1d74-106">Proč používat Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="e1d74-106">Why use Azure Automation DSC</span></span>

<span data-ttu-id="e1d74-107">Azure Automation DSC poskytuje několik výhod oproti použití DSC mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="e1d74-107">Azure Automation DSC provides several advantages over using DSC outside of Azure.</span></span>

### <a name="built-in-pull-server"></a><span data-ttu-id="e1d74-108">Předdefinované načítacího serveru</span><span class="sxs-lookup"><span data-stu-id="e1d74-108">Built-in pull server</span></span>

<span data-ttu-id="e1d74-109">Poskytuje služby Azure Automation [server DSC za](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) tak, aby cílové uzly automaticky zobrazí konfigurace, odpovídat toohello požadovaného stavu a zpětně hlásit dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="e1d74-109">Azure Automation provides a [DSC pull server](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) so that target nodes automatically receive configurations, conform toohello desired state, and report back on their compliance.</span></span>
<span data-ttu-id="e1d74-110">Hello předdefinované načítacího serveru ve službě Azure Automation eliminuje nutnost tooset hello nahoru a udržovat vlastní server vyžádané replikace.</span><span class="sxs-lookup"><span data-stu-id="e1d74-110">hello built-in pull server in Azure Automation eliminates hello need tooset up and maintain your own pull server.</span></span>
<span data-ttu-id="e1d74-111">Služby Azure Automation, můžete vybrat virtuální nebo fyzická systému Windows nebo Linux počítačů, v hello cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="e1d74-111">Azure Automation can target virtual or physical Windows or Linux machines, in hello cloud or on-premises.</span></span>

### <a name="management-of-all-your-dsc-artifacts"></a><span data-ttu-id="e1d74-112">Správa všech artefaktů DSC</span><span class="sxs-lookup"><span data-stu-id="e1d74-112">Management of all your DSC artifacts</span></span>

<span data-ttu-id="e1d74-113">Azure Automation DSC přináší hello stejné vrstva správy příliš[konfigurace požadovaného stavu prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview) jako Azure Automation nabízí skriptů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e1d74-113">Azure Automation DSC brings hello same management layer too[PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) as Azure Automation offers for PowerShell scripting.</span></span>

<span data-ttu-id="e1d74-114">Od hello portálu Azure, nebo prostředí PowerShell můžete spravovat všechny vaše DSC konfigurace, prostředky a cílové uzly.</span><span class="sxs-lookup"><span data-stu-id="e1d74-114">From hello Azure portal, or from PowerShell, you can manage all your DSC configurations, resources, and target nodes.</span></span>

![Snímek obrazovky okna hello Azure Automation.](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a><span data-ttu-id="e1d74-116">Importovat data pro generování sestav do analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="e1d74-116">Import reporting data into Log Analytics</span></span>

<span data-ttu-id="e1d74-117">Uzly, které budou spravovat pomocí Azure Automation DSC odeslat podrobný stav dat toohello předdefinované vyžádání obsahu serveru sestav.</span><span class="sxs-lookup"><span data-stu-id="e1d74-117">Nodes that are managed with Azure Automation DSC send detailed reporting status data toohello built-in pull server.</span></span>
<span data-ttu-id="e1d74-118">Tento pracovní prostor analýzy protokolů Microsoft Operations Management Suite (OMS) tooyour dat můžete nakonfigurovat toosend Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="e1d74-118">You can configure Azure Automation DSC toosend this data tooyour Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>
<span data-ttu-id="e1d74-119">jak zjistit, toosend DSC stav dat tooyour pracovní prostor analýzy protokolů, toolearn [dál Azure Automation DSC generování sestav dat tooOMS analýzy protokolů](automation-dsc-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="e1d74-119">toolearn how toosend DSC status data tooyour Log Analytics workspace, see [Forward Azure Automation DSC reporting data tooOMS Log Analytics](automation-dsc-diagnostics.md).</span></span>

## <a name="introduction-video"></a><span data-ttu-id="e1d74-120">Úvodní video</span><span class="sxs-lookup"><span data-stu-id="e1d74-120">Introduction video</span></span>

<span data-ttu-id="e1d74-121">Raději se díváte tooreading?</span><span class="sxs-lookup"><span data-stu-id="e1d74-121">Prefer watching tooreading?</span></span> <span data-ttu-id="e1d74-122">Podíváme se na následující videa z květen 2015, když se poprvé oznámeno Azure Automation DSC hello.</span><span class="sxs-lookup"><span data-stu-id="e1d74-122">Have a look at hello following video from May 2015, when Azure Automation DSC was first announced.</span></span>

>[!NOTE]
><span data-ttu-id="e1d74-123">Koncepty hello a životního cyklu, které jsou popsané v tomto videu jsou sice správné, Azure Automation DSC má posunula hodně dopředu a vzhledem k tomu, že záznamu tohoto videa.</span><span class="sxs-lookup"><span data-stu-id="e1d74-123">While hello concepts and life cycle discussed in this video are correct, Azure Automation DSC has progressed a lot since this video was recorded.</span></span>
><span data-ttu-id="e1d74-124">Nyní je obecně k dispozici, má mnohem rozsáhlejší uživatelské rozhraní v hello portál Azure a podporuje mnoho další funkce.</span><span class="sxs-lookup"><span data-stu-id="e1d74-124">It is now generally available, has a much more extensive UI in hello Azure portal, and supports many additional capabilities.</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a><span data-ttu-id="e1d74-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1d74-125">Next steps</span></span>

* <span data-ttu-id="e1d74-126">toolearn způsobu tooonboard uzly toobe spravované pomocí Azure Automation DSC, najdete v [registrace počítačů pro správu Azure Automation DSC.](automation-dsc-onboarding.md)</span><span class="sxs-lookup"><span data-stu-id="e1d74-126">toolearn how tooonboard nodes toobe managed with Azure Automation DSC, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md)</span></span>
* <span data-ttu-id="e1d74-127">tooget spuštění pomocí Azure Automation DSC, najdete v části [Začínáme s Azure Automation DSC.](automation-dsc-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="e1d74-127">tooget started using Azure Automation DSC, see [Getting started with Azure Automation DSC](automation-dsc-getting-started.md)</span></span>
* <span data-ttu-id="e1d74-128">toolearn o kompilaci konfigurace DSC, aby je lze přiřadit tootarget uzlů, najdete v části [kompilování konfigurace v Azure Automation DSC.](automation-dsc-compile.md)</span><span class="sxs-lookup"><span data-stu-id="e1d74-128">toolearn about compiling DSC configurations so that you can assign them tootarget nodes, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md)</span></span>
* <span data-ttu-id="e1d74-129">Referenční informace o rutinách prostředí PowerShell pro Azure Automation DSC, najdete v části [rutiny Azure Automation DSC.](/powershell/module/azurerm.automation/#automation)</span><span class="sxs-lookup"><span data-stu-id="e1d74-129">For PowerShell cmdlet reference for Azure Automation DSC, see [Azure Automation DSC cmdlets](/powershell/module/azurerm.automation/#automation)</span></span>
* <span data-ttu-id="e1d74-130">Informace o cenách naleznete v tématu [ceny Azure Automation DSC.](https://azure.microsoft.com/pricing/details/automation/)</span><span class="sxs-lookup"><span data-stu-id="e1d74-130">For pricing information, see [Azure Automation DSC pricing](https://azure.microsoft.com/pricing/details/automation/)</span></span>
* <span data-ttu-id="e1d74-131">toosee příklad použití Azure Automation DSC v kanálu průběžné nasazování, najdete v části [tooIaaS průběžné nasazování virtuálních počítačů pomocí Azure Automation DSC a Chocolatey](automation-dsc-cd-chocolatey.md)</span><span class="sxs-lookup"><span data-stu-id="e1d74-131">toosee an example of using Azure Automation DSC in a continuous deployment pipeline, see  [Continuous Deployment tooIaaS VMs Using Azure Automation DSC and Chocolatey](automation-dsc-cd-chocolatey.md)</span></span>
