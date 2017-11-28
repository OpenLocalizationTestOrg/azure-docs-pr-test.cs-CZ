---
title: "aaaUpdate Azure modulů ve službě Azure Automation | Microsoft Docs"
description: "Tento článek popisuje, jak můžete nyní aktualizovat společnými moduly prostředí Azure PowerShell, které jsou dostupné ve výchozím nastavení ve službě Azure Automation."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: afa404a643349f044e55be2280e435b575049dec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-azure-powershell-modules-in-azure-automation"></a><span data-ttu-id="8285b-103">Jak tooupdate modulů prostředí Azure PowerShell ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="8285b-103">How tooupdate Azure PowerShell modules in Azure Automation</span></span>

<span data-ttu-id="8285b-104">Nejběžnější modulů prostředí Azure PowerShell Hello jsou dostupné ve výchozím nastavení v jednotlivých účtů Automation.</span><span class="sxs-lookup"><span data-stu-id="8285b-104">hello most common Azure PowerShell modules are provided by default in each Automation account.</span></span>  <span data-ttu-id="8285b-105">aktualizace tým Azure Hello hello Azure moduly pravidelně, tak v hello účet Automation jsme poskytnout způsob, jak vám tooupdate hello moduly v účtu hello Jakmile je nová verze dostupná z portálu, hello.</span><span class="sxs-lookup"><span data-stu-id="8285b-105">hello Azure team updates hello Azure modules regularly, so in hello Automation account we provide a way for you tooupdate hello modules in hello account when new versions are available from hello portal.</span></span>  

<span data-ttu-id="8285b-106">Protože moduly jsou pravidelně aktualizovány skupinou hello produktu, změny můžou nastat s rutinami hello zahrnuty, což může mít negativní vliv na vaše sady runbook v závislosti na typu hello změny, jako je například přejmenování parametr nebo zcela místo začne rutiny.</span><span class="sxs-lookup"><span data-stu-id="8285b-106">Because modules are updated regularly by hello product group, changes can occur with hello  included cmdlets, which may negatively impact your runbooks depending on hello type of change, such as renaming a parameter or deprecating a cmdlet entirely.</span></span> <span data-ttu-id="8285b-107">tooavoid sady runbook a hello, které mají vliv procesy jejich automatizovat, důrazně doporučujeme testování a ověření než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="8285b-107">tooavoid impacting your runbooks and hello processes they automate, it is strongly recommended that you test and validate before proceeding.</span></span>  <span data-ttu-id="8285b-108">Pokud nemáte určené pro tento účel vyhrazený účet Automation, zvažte vytvoření jeden, mohli otestovat mnoha různých scénářů a permutací během vývoje hello sadu runbook, přidání tooiterative změnám jako aktualizace hello Moduly prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8285b-108">If you do not have a dedicated Automation account intended for this purpose, consider creating one so that you can test many different scenarios and permutations during hello development of your runbooks, in addition tooiterative changes such as updating hello PowerShell modules.</span></span>  <span data-ttu-id="8285b-109">Po ověření hello výsledky a jste použili potřebné změny, pokračujte koordinace hello migrace jakékoli sady runbook, který vyžaduje úpravu a provést aktualizaci hello, jak je popsáno níže v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8285b-109">After hello results are validated and you have applied any changes required, proceed with coordinating hello migration of any runbooks that required modification and perform hello update as described below in production.</span></span>     

## <a name="updating-azure-modules"></a><span data-ttu-id="8285b-110">Aktualizace Azure moduly</span><span class="sxs-lookup"><span data-stu-id="8285b-110">Updating Azure Modules</span></span>

1. <span data-ttu-id="8285b-111">V modulech hello je okno účtu Automation existuje možnost **moduly Azure aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="8285b-111">In hello Modules blade of your Automation account there is an option called **Update Azure Modules**.</span></span>  <span data-ttu-id="8285b-112">Je vždy povolena.</span><span class="sxs-lookup"><span data-stu-id="8285b-112">It is always enabled.</span></span><br><br> <span data-ttu-id="8285b-113">![Aktualizovat možnost moduly Azure v okně moduly](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span><span class="sxs-lookup"><span data-stu-id="8285b-113">![Update Azure Modules option in Modules blade](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span></span>

2. <span data-ttu-id="8285b-114">Klikněte na tlačítko **moduly Azure aktualizace** a zobrazí se oznámení o potvrzení, která požaduje, pokud chcete, aby toocontinue.</span><span class="sxs-lookup"><span data-stu-id="8285b-114">Click **Update Azure Modules** and you will be presented with a confirmation notification that asks you if you want toocontinue.</span></span><br><br> <span data-ttu-id="8285b-115">![Moduly Azure oznámení o aktualizaci](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span><span class="sxs-lookup"><span data-stu-id="8285b-115">![Update Azure Modules notification](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span></span>

3. <span data-ttu-id="8285b-116">Klikněte na tlačítko **Ano** a zahájí proces aktualizace modulu hello.</span><span class="sxs-lookup"><span data-stu-id="8285b-116">Click **Yes** and hello module update process will begin.</span></span>  <span data-ttu-id="8285b-117">proces aktualizace Hello trvá o 15-20 minut tooupdate hello následující moduly:</span><span class="sxs-lookup"><span data-stu-id="8285b-117">hello update process takes about 15-20 minutes tooupdate hello following modules:</span></span>

  * <span data-ttu-id="8285b-118">Azure</span><span class="sxs-lookup"><span data-stu-id="8285b-118">Azure</span></span>
  * <span data-ttu-id="8285b-119">Azure.Storage</span><span class="sxs-lookup"><span data-stu-id="8285b-119">Azure.Storage</span></span>
  * <span data-ttu-id="8285b-120">AzureRm.Automation</span><span class="sxs-lookup"><span data-stu-id="8285b-120">AzureRm.Automation</span></span>
  * <span data-ttu-id="8285b-121">AzureRm.Compute</span><span class="sxs-lookup"><span data-stu-id="8285b-121">AzureRm.Compute</span></span>
  * <span data-ttu-id="8285b-122">AzureRm.Profile</span><span class="sxs-lookup"><span data-stu-id="8285b-122">AzureRm.Profile</span></span>
  * <span data-ttu-id="8285b-123">AzureRm.Resources</span><span class="sxs-lookup"><span data-stu-id="8285b-123">AzureRm.Resources</span></span>
  * <span data-ttu-id="8285b-124">AzureRm.Sql</span><span class="sxs-lookup"><span data-stu-id="8285b-124">AzureRm.Sql</span></span>
  * <span data-ttu-id="8285b-125">AzureRm.Storage</span><span class="sxs-lookup"><span data-stu-id="8285b-125">AzureRm.Storage</span></span>

    <span data-ttu-id="8285b-126">Pokud hello moduly jsou již až toodate, proces hello dokončí za několik sekund.</span><span class="sxs-lookup"><span data-stu-id="8285b-126">If hello modules are already up toodate, then hello process will complete in a few seconds.</span></span>  <span data-ttu-id="8285b-127">Po dokončení procesu aktualizace hello budete informováni.</span><span class="sxs-lookup"><span data-stu-id="8285b-127">When hello update process completes you will be notified.</span></span><br><br> ![Aktualizovat stav aktualizace moduly Azure](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> <span data-ttu-id="8285b-129">Služby Azure Automation bude používat nejnovější moduly hello ve vašem účtu Automation, při spuštění novou naplánovanou úlohu.</span><span class="sxs-lookup"><span data-stu-id="8285b-129">Azure Automation will use hello latest modules in your Automation account when a new scheduled job is run.</span></span>    

<span data-ttu-id="8285b-130">Pokud používáte rutiny z těchto modulů prostředí Azure PowerShell ve vaší sady runbook toomanage Azure prostředky a potom se mají tooperform tento proces aktualizace každý měsíc nebo tak hello tooassure, zda máte nejnovější moduly.</span><span class="sxs-lookup"><span data-stu-id="8285b-130">If you use cmdlets from these Azure PowerShell modules in your runbooks toomanage Azure resources, then you will want tooperform this update process every month or so tooassure that you have hello latest modules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8285b-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8285b-131">Next steps</span></span>

* <span data-ttu-id="8285b-132">toolearn informace o integrační moduly a jak toocreate vlastní moduly toofurther integrovat automatizace s jinými systémy, služby nebo řešení, najdete v části [moduly integrace](automation-integration-modules.md).</span><span class="sxs-lookup"><span data-stu-id="8285b-132">toolearn more about Integration Modules and how toocreate custom modules toofurther integrate Automation with other systems, services, or solutions, see [Integration Modules](automation-integration-modules.md).</span></span>

* <span data-ttu-id="8285b-133">Zvažte použití integrace zdroj ovládacího prvku [Githubu Enterprise](automation-scenario-source-control-integration-with-github-ent.md) nebo [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) toocentrally spravovat a řídit verzích portfolio automatizace sady runbook a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8285b-133">Consider source control integration using [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) or [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) toocentrally manage and control releases of your Automation runbook and configuration portfolio.</span></span>  
