---
title: "nastavení aaaRunbook | Microsoft Docs"
description: "Popisuje hello nastavení konfigurace pro sady runbook ve službě Azure Automation a jak toochange je pomocí obou hello portálu pro správu Azure a prostředí Windows PowerShell."
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
ms.openlocfilehash: 6f0ef09c148355a351464424c22c33df9300f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-settings"></a><span data-ttu-id="128ff-103">Nastavení runbooku</span><span class="sxs-lookup"><span data-stu-id="128ff-103">Runbook settings</span></span>
<span data-ttu-id="128ff-104">Každá sada runbook ve službě Azure Automation má několik nastavení, které jej pomáhají identifikovat toobe a toochange její chování při protokolování.</span><span class="sxs-lookup"><span data-stu-id="128ff-104">Each runbook in Azure Automation has multiple settings that help it toobe identified and toochange its logging behavior.</span></span> <span data-ttu-id="128ff-105">Každá z těchto nastavení je popsána níže následují procedury na postupy toomodify je.</span><span class="sxs-lookup"><span data-stu-id="128ff-105">Each of these settings is described below followed by procedures on how toomodify them.</span></span>

## <a name="settings"></a><span data-ttu-id="128ff-106">Nastavení</span><span class="sxs-lookup"><span data-stu-id="128ff-106">Settings</span></span>
### <a name="name-and-description"></a><span data-ttu-id="128ff-107">Název a popis</span><span class="sxs-lookup"><span data-stu-id="128ff-107">Name and description</span></span>
<span data-ttu-id="128ff-108">Hello název sady runbook nelze změnit po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="128ff-108">You cannot change hello name of a runbook after it has been created.</span></span> <span data-ttu-id="128ff-109">Hello popis je volitelný a může být až too512 znaků.</span><span class="sxs-lookup"><span data-stu-id="128ff-109">hello Description is optional and can be up too512 characters.</span></span>

### <a name="tags"></a><span data-ttu-id="128ff-110">Značky</span><span class="sxs-lookup"><span data-stu-id="128ff-110">Tags</span></span>
<span data-ttu-id="128ff-111">Značky povolit, že jste tooassign různá slova a frází toohelp sadu runbook identifikovat.</span><span class="sxs-lookup"><span data-stu-id="128ff-111">Tags allow you tooassign distinct words and phrases toohelp identify a runbook.</span></span> <span data-ttu-id="128ff-112">Například při odeslání runbook toohello [Galerie prostředí PowerShell](https://www.powershellgallery.com/), můžete zadat konkrétní značky tooidentify kategorie, které hello runbook by měl být uvedený v.</span><span class="sxs-lookup"><span data-stu-id="128ff-112">For example, when you submit a runbook toohello [PowerShell Gallery](https://www.powershellgallery.com/), you specify particular tags tooidentify which categories hello runbook should be listed in.</span></span> <span data-ttu-id="128ff-113">Můžete určit více značek pro sadu runbook oddělte je čárkami.</span><span class="sxs-lookup"><span data-stu-id="128ff-113">You can specify multiple tags for a runbook by separating them with commas.</span></span>

### <a name="logging"></a><span data-ttu-id="128ff-114">Protokolování</span><span class="sxs-lookup"><span data-stu-id="128ff-114">Logging</span></span>
<span data-ttu-id="128ff-115">Ve výchozím nastavení nezapisují podrobné a průběh záznamy historie toojob.</span><span class="sxs-lookup"><span data-stu-id="128ff-115">By default, Verbose and Progress records are not written toojob history.</span></span> <span data-ttu-id="128ff-116">Můžete změnit hello nastavení pro konkrétní runbook toolog tyto záznamy.</span><span class="sxs-lookup"><span data-stu-id="128ff-116">You can change hello settings for a particular runbook toolog these records.</span></span> <span data-ttu-id="128ff-117">Další informace o tyto záznamy, najdete v části [výstup a zprávy Runbooku](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="128ff-117">For more information on these records, see [Runbook Output and Messages](automation-runbook-output-and-messages.md).</span></span>

## <a name="changing-runbook-settings"></a><span data-ttu-id="128ff-118">Změna nastavení sady runbook</span><span class="sxs-lookup"><span data-stu-id="128ff-118">Changing runbook settings</span></span>

### <a name="changing-runbook-settings-with-hello-azure-portal"></a><span data-ttu-id="128ff-119">Změna nastavení sady runbook pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="128ff-119">Changing runbook settings with hello Azure portal</span></span>
<span data-ttu-id="128ff-120">Můžete změnit nastavení sady runbook v portálu Azure hello hello **nastavení** okno hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="128ff-120">You can change settings for a runbook in hello Azure portal from hello **Settings** blade for hello runbook.</span></span>

1. <span data-ttu-id="128ff-121">V hello portálu Azure, vyberte **automatizace** a pak klikněte hello název účtu automation.</span><span class="sxs-lookup"><span data-stu-id="128ff-121">In hello Azure portal, select **Automation** and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="128ff-122">Vyberte hello **Runbooky** kartě.</span><span class="sxs-lookup"><span data-stu-id="128ff-122">Select hello **Runbooks** tab.</span></span>
3. <span data-ttu-id="128ff-123">Klikněte na název hello sady runbook a jste okno nastavení směrovanou toohello hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="128ff-123">Click hello name of a runbook and you are directed toohello settings blade for hello runbook.</span></span> <span data-ttu-id="128ff-124">Odsud můžete zadat nebo změnit značky, hello popis sady runbook, konfigurovat protokolování a nastavení pro trasování a přístup k toohelp Podpora nástrojů pro řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="128ff-124">From here you can specify or modify tags, hello runbook description, configure logging and tracing settings, and access support tools toohelp you solve problems.</span></span>     

### <a name="changing-runbook-settings-with-windows-powershell"></a><span data-ttu-id="128ff-125">Změna nastavení sady runbook pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="128ff-125">Changing runbook settings with Windows PowerShell</span></span>
<span data-ttu-id="128ff-126">Můžete použít hello [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) rutiny toochange hello nastavení sady runbook.</span><span class="sxs-lookup"><span data-stu-id="128ff-126">You can use hello [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet toochange hello settings for a runbook.</span></span> <span data-ttu-id="128ff-127">Pokud chcete toospecify více značek, můžete buď zadat pole nebo jeden řetězec s parametrem značky toohello hodnoty oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="128ff-127">If you want toospecify multiple tags, you can either provide an array or a single string with comma delimited values toohello Tags parameter.</span></span> <span data-ttu-id="128ff-128">Můžete získat aktuální značky hello s hello [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span><span class="sxs-lookup"><span data-stu-id="128ff-128">You can get hello current tags with hello [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span></span>

<span data-ttu-id="128ff-129">Hello následující vzorové příkazy ukazují, jak tooset hello vlastností pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="128ff-129">hello following sample commands show how tooset hello properties for a runbook.</span></span> <span data-ttu-id="128ff-130">Tato ukázka přidá tři značky toohello existující značky a určuje, zda mají být protokolovány podrobných záznamů.</span><span class="sxs-lookup"><span data-stu-id="128ff-130">This sample adds three tags toohello existing tags and specifies that verbose records should be logged.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a><span data-ttu-id="128ff-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="128ff-131">Next steps</span></span>
* <span data-ttu-id="128ff-132">jak zjistit, toocreate a načtení výstupní a chybové zprávy ze sady runbook, toolearn [výstup a zprávy Runbooku](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="128ff-132">toolearn how toocreate and retrieve output and error messages from runbooks, see [Runbook Output and Messages](automation-runbook-output-and-messages.md)</span></span> 
* <span data-ttu-id="128ff-133">jak zjistit, tooadd sady runbook, která již vyvinula hello komunity nebo jiné zdroje nebo toocreate vlastní runbook toounderstand [vytvoření nebo import Runbooku](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="128ff-133">toounderstand how tooadd a runbook that was already developed by hello community or other source, or toocreate your own runbook see [Creating or Importing a Runbook](automation-creating-importing-runbook.md)</span></span> 

