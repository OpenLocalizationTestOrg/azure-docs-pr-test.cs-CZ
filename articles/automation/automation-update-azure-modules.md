---
title: "Aktualizovat Azure modulů ve službě Azure Automation | Microsoft Docs"
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
ms.openlocfilehash: ed8c97b642d406a05817ec6c67f31a1b4bce93b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a><span data-ttu-id="c096d-103">Postup aktualizace modulů prostředí Azure PowerShell ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="c096d-103">How to update Azure PowerShell modules in Azure Automation</span></span>

<span data-ttu-id="c096d-104">Nejběžnější modulů prostředí Azure PowerShell jsou dostupné ve výchozím nastavení v jednotlivých účtů Automation.</span><span class="sxs-lookup"><span data-stu-id="c096d-104">The most common Azure PowerShell modules are provided by default in each Automation account.</span></span>  <span data-ttu-id="c096d-105">Tým služby Azure aktualizuje Azure moduly pravidelně, tak v účtu Automation jsme poskytují způsob, jak můžete aktualizovat moduly v účtu, jakmile je nová verze dostupná z portálu.</span><span class="sxs-lookup"><span data-stu-id="c096d-105">The Azure team updates the Azure modules regularly, so in the Automation account we provide a way for you to update the modules in the account when new versions are available from the portal.</span></span>  

<span data-ttu-id="c096d-106">Protože moduly se pravidelně aktualizují v product group, změny můžou nastat pomocí rutiny zahrnuté, což může mít negativní vliv na vaše sady runbook v závislosti na typu změnu, jako je například přejmenování parametr nebo zcela místo začne rutiny.</span><span class="sxs-lookup"><span data-stu-id="c096d-106">Because modules are updated regularly by the product group, changes can occur with the  included cmdlets, which may negatively impact your runbooks depending on the type of change, such as renaming a parameter or deprecating a cmdlet entirely.</span></span> <span data-ttu-id="c096d-107">Nechcete-li vaše sady runbook a procesy, které budou automatizovat, které mají vliv, doporučujeme testování a ověření než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c096d-107">To avoid impacting your runbooks and the processes they automate, it is strongly recommended that you test and validate before proceeding.</span></span>  <span data-ttu-id="c096d-108">Pokud nemáte určené pro tento účel vyhrazený účet Automation, zvažte vytvoření jednoho tak, aby při vývoji vaší sady runbook, kromě iterativní změny jako aktualizace moduly Powershellu můžete otestovat mnoha různých scénářů a permutací.</span><span class="sxs-lookup"><span data-stu-id="c096d-108">If you do not have a dedicated Automation account intended for this purpose, consider creating one so that you can test many different scenarios and permutations during the development of your runbooks, in addition to iterative changes such as updating the PowerShell modules.</span></span>  <span data-ttu-id="c096d-109">Po ověření výsledky a jste použili potřebné změny, pokračovat v migraci žádné sady runbook, který vyžaduje úpravu koordinace a provést aktualizaci, jak je popsáno níže v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c096d-109">After the results are validated and you have applied any changes required, proceed with coordinating the migration of any runbooks that required modification and perform the update as described below in production.</span></span>     

## <a name="updating-azure-modules"></a><span data-ttu-id="c096d-110">Aktualizace Azure moduly</span><span class="sxs-lookup"><span data-stu-id="c096d-110">Updating Azure Modules</span></span>

1. <span data-ttu-id="c096d-111">V modulech je okno účtu Automation existuje možnost **moduly Azure aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="c096d-111">In the Modules blade of your Automation account there is an option called **Update Azure Modules**.</span></span>  <span data-ttu-id="c096d-112">Je vždy povolena.</span><span class="sxs-lookup"><span data-stu-id="c096d-112">It is always enabled.</span></span><br><br> <span data-ttu-id="c096d-113">![Aktualizovat možnost moduly Azure v okně moduly](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span><span class="sxs-lookup"><span data-stu-id="c096d-113">![Update Azure Modules option in Modules blade](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span></span>

2. <span data-ttu-id="c096d-114">Klikněte na tlačítko **moduly Azure aktualizace** a zobrazí se oznámení o potvrzení, která požaduje, pokud chcete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c096d-114">Click **Update Azure Modules** and you will be presented with a confirmation notification that asks you if you want to continue.</span></span><br><br> <span data-ttu-id="c096d-115">![Moduly Azure oznámení o aktualizaci](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span><span class="sxs-lookup"><span data-stu-id="c096d-115">![Update Azure Modules notification](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span></span>

3. <span data-ttu-id="c096d-116">Klikněte na tlačítko **Ano** a zahájí proces aktualizace modulu.</span><span class="sxs-lookup"><span data-stu-id="c096d-116">Click **Yes** and the module update process will begin.</span></span>  <span data-ttu-id="c096d-117">Proces aktualizace trvá asi 15-20 minut aktualizovat následující moduly:</span><span class="sxs-lookup"><span data-stu-id="c096d-117">The update process takes about 15-20 minutes to update the following modules:</span></span>

  * <span data-ttu-id="c096d-118">Azure</span><span class="sxs-lookup"><span data-stu-id="c096d-118">Azure</span></span>
  * <span data-ttu-id="c096d-119">Azure.Storage</span><span class="sxs-lookup"><span data-stu-id="c096d-119">Azure.Storage</span></span>
  * <span data-ttu-id="c096d-120">AzureRm.Automation</span><span class="sxs-lookup"><span data-stu-id="c096d-120">AzureRm.Automation</span></span>
  * <span data-ttu-id="c096d-121">AzureRm.Compute</span><span class="sxs-lookup"><span data-stu-id="c096d-121">AzureRm.Compute</span></span>
  * <span data-ttu-id="c096d-122">AzureRm.Profile</span><span class="sxs-lookup"><span data-stu-id="c096d-122">AzureRm.Profile</span></span>
  * <span data-ttu-id="c096d-123">AzureRm.Resources</span><span class="sxs-lookup"><span data-stu-id="c096d-123">AzureRm.Resources</span></span>
  * <span data-ttu-id="c096d-124">AzureRm.Sql</span><span class="sxs-lookup"><span data-stu-id="c096d-124">AzureRm.Sql</span></span>
  * <span data-ttu-id="c096d-125">AzureRm.Storage</span><span class="sxs-lookup"><span data-stu-id="c096d-125">AzureRm.Storage</span></span>

    <span data-ttu-id="c096d-126">Pokud moduly jsou již aktuální, proces dokončí za několik sekund.</span><span class="sxs-lookup"><span data-stu-id="c096d-126">If the modules are already up to date, then the process will complete in a few seconds.</span></span>  <span data-ttu-id="c096d-127">Po dokončení procesu aktualizace budete informováni.</span><span class="sxs-lookup"><span data-stu-id="c096d-127">When the update process completes you will be notified.</span></span><br><br> ![Aktualizovat stav aktualizace moduly Azure](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> <span data-ttu-id="c096d-129">Služby Azure Automation bude používat nejnovější modulů ve vašem účtu Automation, při spuštění novou naplánovanou úlohu.</span><span class="sxs-lookup"><span data-stu-id="c096d-129">Azure Automation will use the latest modules in your Automation account when a new scheduled job is run.</span></span>    

<span data-ttu-id="c096d-130">Pokud používáte rutiny z těchto modulů prostředí Azure PowerShell ve vašich sadách runbook ke správě prostředků Azure, je třeba provést proces aktualizace každý měsíc, nebo proto, aby zajistil, že máte nejnovější moduly.</span><span class="sxs-lookup"><span data-stu-id="c096d-130">If you use cmdlets from these Azure PowerShell modules in your runbooks to manage Azure resources, then you will want to perform this update process every month or so to assure that you have the latest modules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c096d-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c096d-131">Next steps</span></span>

* <span data-ttu-id="c096d-132">Další informace o integrační moduly a jak vytvořit vlastní moduly k další integraci automatizace s jinými systémy, služby nebo řešení, najdete v části [moduly integrace](automation-integration-modules.md).</span><span class="sxs-lookup"><span data-stu-id="c096d-132">To learn more about Integration Modules and how to create custom modules to further integrate Automation with other systems, services, or solutions, see [Integration Modules](automation-integration-modules.md).</span></span>

* <span data-ttu-id="c096d-133">Zvažte použití integrace zdroj ovládacího prvku [Githubu Enterprise](automation-scenario-source-control-integration-with-github-ent.md) nebo [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) centrálně spravovat a řídit verzích portfolio automatizace sady runbook a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c096d-133">Consider source control integration using [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) or [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) to centrally manage and control releases of your Automation runbook and configuration portfolio.</span></span>  