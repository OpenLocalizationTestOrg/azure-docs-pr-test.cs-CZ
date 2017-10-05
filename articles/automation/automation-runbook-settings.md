---
title: "Nastavení sady Runbook | Microsoft Docs"
description: "Popisuje konfiguraci nastavení pro sady runbook v Azure Automation a jak ji pomocí portálu pro správu Azure a prostředí Windows PowerShell změnit."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: a726f20c-a952-48b8-88ee-36d76aa3ac61
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: bwren
ms.openlocfilehash: 20ecbc270e61d234e026e6ba2634c7aad63b3355
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="runbook-settings"></a><span data-ttu-id="51cc1-103">Nastavení runbooku</span><span class="sxs-lookup"><span data-stu-id="51cc1-103">Runbook settings</span></span>
<span data-ttu-id="51cc1-104">Každá sada runbook ve službě Azure Automation má několik nastavení, která pomáhají identifikovat a změnit její chování při protokolování.</span><span class="sxs-lookup"><span data-stu-id="51cc1-104">Each runbook in Azure Automation has multiple settings that help it to be identified and to change its logging behavior.</span></span> <span data-ttu-id="51cc1-105">Každá z těchto nastavení je popsána níže následují procedury na tom, jak je upravit.</span><span class="sxs-lookup"><span data-stu-id="51cc1-105">Each of these settings is described below followed by procedures on how to modify them.</span></span>

## <a name="settings"></a><span data-ttu-id="51cc1-106">Nastavení</span><span class="sxs-lookup"><span data-stu-id="51cc1-106">Settings</span></span>
### <a name="name-and-description"></a><span data-ttu-id="51cc1-107">Název a popis</span><span class="sxs-lookup"><span data-stu-id="51cc1-107">Name and description</span></span>
<span data-ttu-id="51cc1-108">Název sady runbook nelze změnit po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="51cc1-108">You cannot change the name of a runbook after it has been created.</span></span> <span data-ttu-id="51cc1-109">Popis je volitelný a může mít až 512 znaků.</span><span class="sxs-lookup"><span data-stu-id="51cc1-109">The Description is optional and can be up to 512 characters.</span></span>

### <a name="tags"></a><span data-ttu-id="51cc1-110">Značky</span><span class="sxs-lookup"><span data-stu-id="51cc1-110">Tags</span></span>
<span data-ttu-id="51cc1-111">Značky umožňují přiřadit různá slova a slovní spojení, která pomáhají sadu runbook identifikovat.</span><span class="sxs-lookup"><span data-stu-id="51cc1-111">Tags allow you to assign distinct words and phrases to help identify a runbook.</span></span> <span data-ttu-id="51cc1-112">Například když odešlete sady runbook [Galerie prostředí PowerShell](https://www.powershellgallery.com/), zadejte konkrétní značky k identifikaci kategorie, které sada runbook by měl být uvedený v.</span><span class="sxs-lookup"><span data-stu-id="51cc1-112">For example, when you submit a runbook to the [PowerShell Gallery](https://www.powershellgallery.com/), you specify particular tags to identify which categories the runbook should be listed in.</span></span> <span data-ttu-id="51cc1-113">Můžete určit více značek pro sadu runbook oddělte je čárkami.</span><span class="sxs-lookup"><span data-stu-id="51cc1-113">You can specify multiple tags for a runbook by separating them with commas.</span></span>

### <a name="logging"></a><span data-ttu-id="51cc1-114">Protokolování</span><span class="sxs-lookup"><span data-stu-id="51cc1-114">Logging</span></span>
<span data-ttu-id="51cc1-115">Ve výchozím nastavení nejsou podrobné a průběh záznamy zapisují do historie úlohy.</span><span class="sxs-lookup"><span data-stu-id="51cc1-115">By default, Verbose and Progress records are not written to job history.</span></span> <span data-ttu-id="51cc1-116">Můžete změnit nastavení pro konkrétní runbook tyto záznamy protokolu.</span><span class="sxs-lookup"><span data-stu-id="51cc1-116">You can change the settings for a particular runbook to log these records.</span></span> <span data-ttu-id="51cc1-117">Další informace o tyto záznamy, najdete v části [výstup a zprávy Runbooku](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="51cc1-117">For more information on these records, see [Runbook Output and Messages](automation-runbook-output-and-messages.md).</span></span>

## <a name="changing-runbook-settings"></a><span data-ttu-id="51cc1-118">Změna nastavení sady runbook</span><span class="sxs-lookup"><span data-stu-id="51cc1-118">Changing runbook settings</span></span>

### <a name="changing-runbook-settings-with-the-azure-portal"></a><span data-ttu-id="51cc1-119">Změna nastavení sady runbook pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="51cc1-119">Changing runbook settings with the Azure portal</span></span>
<span data-ttu-id="51cc1-120">Můžete změnit nastavení sady runbook na portálu Azure z **nastavení** okno pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="51cc1-120">You can change settings for a runbook in the Azure portal from the **Settings** blade for the runbook.</span></span>

1. <span data-ttu-id="51cc1-121">Na portálu Azure vyberte **automatizace** a pak klikněte na název účtu automation.</span><span class="sxs-lookup"><span data-stu-id="51cc1-121">In the Azure portal, select **Automation** and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="51cc1-122">Vyberte **Runbooky** kartě.</span><span class="sxs-lookup"><span data-stu-id="51cc1-122">Select the **Runbooks** tab.</span></span>
3. <span data-ttu-id="51cc1-123">Klikněte na název sady runbook a přejdete do okna nastavení pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="51cc1-123">Click the name of a runbook and you are directed to the settings blade for the runbook.</span></span> <span data-ttu-id="51cc1-124">Odsud můžete zadat nebo změnit značky, popis sady runbook, nakonfigurovat nastavení trasování a protokolování a přístup k nástrojů podpory, které pomáhají při řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="51cc1-124">From here you can specify or modify tags, the runbook description, configure logging and tracing settings, and access support tools to help you solve problems.</span></span>     

### <a name="changing-runbook-settings-with-windows-powershell"></a><span data-ttu-id="51cc1-125">Změna nastavení sady runbook pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="51cc1-125">Changing runbook settings with Windows PowerShell</span></span>
<span data-ttu-id="51cc1-126">Můžete použít [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) rutiny změnit nastavení pro sady runbook.</span><span class="sxs-lookup"><span data-stu-id="51cc1-126">You can use the [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet to change the settings for a runbook.</span></span> <span data-ttu-id="51cc1-127">Pokud chcete zadat více značek, můžete buď zadat pole nebo jeden řetězec s čárkami hodnoty na parametr značky.</span><span class="sxs-lookup"><span data-stu-id="51cc1-127">If you want to specify multiple tags, you can either provide an array or a single string with comma delimited values to the Tags parameter.</span></span> <span data-ttu-id="51cc1-128">Můžete získat aktuální značky s [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span><span class="sxs-lookup"><span data-stu-id="51cc1-128">You can get the current tags with the [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span></span>

<span data-ttu-id="51cc1-129">Následující vzorové příkazy ukazují, jak nastavit vlastnosti pro sady runbook.</span><span class="sxs-lookup"><span data-stu-id="51cc1-129">The following sample commands show how to set the properties for a runbook.</span></span> <span data-ttu-id="51cc1-130">Tato ukázka přidá tři značky pro existující značky a určuje, zda mají být protokolovány podrobných záznamů.</span><span class="sxs-lookup"><span data-stu-id="51cc1-130">This sample adds three tags to the existing tags and specifies that verbose records should be logged.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a><span data-ttu-id="51cc1-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51cc1-131">Next steps</span></span>
* <span data-ttu-id="51cc1-132">Další informace o vytvoření a ze sady runbook načíst výstupní a chybové zprávy naleznete v tématu [výstup a zprávy Runbooku](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="51cc1-132">To learn how to create and retrieve output and error messages from runbooks, see [Runbook Output and Messages](automation-runbook-output-and-messages.md)</span></span> 
* <span data-ttu-id="51cc1-133">Abyste pochopili, jak přidat sadu runbook, která již byla vyvinuta komunity nebo jiný zdroj, nebo vytvořte vlastní sady runbook viz [vytvoření nebo import Runbooku](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="51cc1-133">To understand how to add a runbook that was already developed by the community or other source, or to create your own runbook see [Creating or Importing a Runbook](automation-creating-importing-runbook.md)</span></span> 

