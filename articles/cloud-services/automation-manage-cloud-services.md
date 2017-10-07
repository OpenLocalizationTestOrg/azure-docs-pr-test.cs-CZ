---
title: "aaaManage Azure Cloud Services pomocí Azure Automation | Microsoft Docs"
description: "Informace o tom, jak hello služby Azure Automation může být použité toomanage cloudových služeb Azure ve velkém měřítku."
services: cloud-services, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
ms.openlocfilehash: 8e920fb94955466bfec71cc332444f5f0ee497a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a><span data-ttu-id="8a78e-103">Správa cloudových služeb Azure pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="8a78e-103">Managing Azure Cloud Services using Azure Automation</span></span>
<span data-ttu-id="8a78e-104">Tento průvodce vás seznámí toohello služba Azure Automation a jak může být použité toosimplify správy Azure cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="8a78e-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of your Azure cloud services.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="8a78e-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="8a78e-105">What is Azure Automation?</span></span>
<span data-ttu-id="8a78e-106">[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="8a78e-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="8a78e-107">Pomocí Azure Automation, může být časově náročné, ruční, problematických a často se opakujících úloh, automatizované tooincrease spolehlivost, efektivitu a toovalue čas pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="8a78e-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="8a78e-108">Azure Automation nabízí modulu provádění vysoce spolehlivé a vysoce dostupné pracovního postupu, který přizpůsobí vašim potřebám toomeet podle růstu vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="8a78e-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="8a78e-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="8a78e-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="8a78e-110">Nižší provozní režie a uvolněte IT / DevOps zaměstnanci toofocus na práci, kterou přidá obchodní hodnotu přesunutím vaší toobe úlohy správy cloudu automaticky spouštěné v Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="8a78e-110">Lower operational overhead and free up IT / DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a><span data-ttu-id="8a78e-111">Jak Azure Automation pomoci spravovat cloudové služby Azure?</span><span class="sxs-lookup"><span data-stu-id="8a78e-111">How can Azure Automation help manage Azure cloud services?</span></span>
<span data-ttu-id="8a78e-112">Cloudové služby Azure lze spravovat ve službě Azure Automation pomocí rutin prostředí PowerShell text hello, které jsou k dispozici v hello [nástroje Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="8a78e-112">Azure cloud services can be managed in Azure Automation by using hello PowerShell cmdlets that are available in hello [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="8a78e-113">Automatizace Azure má tyto k dispozici cloudové rutiny prostředí PowerShell služby předinstalované hello, aby mohli provést všechny úkoly správy cloudové služby v rámci služby hello.</span><span class="sxs-lookup"><span data-stu-id="8a78e-113">Azure Automation has these cloud service PowerShell cmdlets available out of hello box, so that you can perform all of your cloud service management tasks within hello service.</span></span> <span data-ttu-id="8a78e-114">Tyto rutiny ve službě Azure Automation pomocí hello rutin pro ostatní služby Azure, tooautomate složité úlohy může také párovat napříč službami Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="8a78e-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="8a78e-115">Některé příklad používá z toomanage Azure Automation, které patří Azure Cloud Services:</span><span class="sxs-lookup"><span data-stu-id="8a78e-115">Some example uses of Azure Automation toomanage Azure Cloud Services include:</span></span>

* [<span data-ttu-id="8a78e-116">Nepřetržité nasazení cloudové služby při každé aktualizaci cscfg nebo cspkg v Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="8a78e-116">Continous deployment of a Cloud Service whenever cscfg or cspkg is updated in Azure Blob storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [<span data-ttu-id="8a78e-117">Restartování instance cloudové služby paralelně jednu upgradovací doménu najednou</span><span class="sxs-lookup"><span data-stu-id="8a78e-117">Rebooting Cloud Service instances in parallel, one upgrade domain at a time</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a><span data-ttu-id="8a78e-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a78e-118">Next Steps</span></span>
<span data-ttu-id="8a78e-119">Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage cloudové služby Azure, použijte tyto odkazy toolearn Další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="8a78e-119">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure cloud services, follow these links toolearn more about Azure Automation.</span></span>

* [<span data-ttu-id="8a78e-120">Přehled služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="8a78e-120">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="8a78e-121">Můj první runbook</span><span class="sxs-lookup"><span data-stu-id="8a78e-121">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="8a78e-122">Mapy kurzů Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="8a78e-122">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)
