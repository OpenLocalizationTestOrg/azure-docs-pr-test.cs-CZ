---
title: "Správa cloudových služeb Azure pomocí Azure Automation | Microsoft Docs"
description: "Další informace o používání služby Azure Automation ke správě cloudových služeb Azure ve velkém měřítku."
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
ms.openlocfilehash: 6b5acac1b8647c324988c316cd5602b3dba98a1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a><span data-ttu-id="4168d-103">Správa cloudových služeb Azure pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4168d-103">Managing Azure Cloud Services using Azure Automation</span></span>
<span data-ttu-id="4168d-104">Tento průvodce vás seznámí s služba Azure Automation a jak může sloužit ke zjednodušení správy Azure cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="4168d-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of your Azure cloud services.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="4168d-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="4168d-105">What is Azure Automation?</span></span>
<span data-ttu-id="4168d-106">[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="4168d-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="4168d-107">Pomocí Azure Automation, dlouhotrvajících, ruční, problematických a často se opakujících úloh je možné automatizovat zvýšit spolehlivost, efektivitu a času na hodnotu pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="4168d-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="4168d-108">Azure Automation nabízí modul provádění vysoce spolehlivé a vysoce dostupné pracovního postupu, který rozšiřuje podle vašich potřeb podle růstu vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="4168d-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales to meet your needs as your organization grows.</span></span> <span data-ttu-id="4168d-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="4168d-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="4168d-110">Nižší provozní režie a uvolněte IT / DevOps zaměstnanci a zaměřit se na práci, kterou přidá obchodní value přesunutím vašeho cloudu spuštění úloh správy se automaticky automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="4168d-110">Lower operational overhead and free up IT / DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a><span data-ttu-id="4168d-111">Jak Azure Automation pomoci spravovat cloudové služby Azure?</span><span class="sxs-lookup"><span data-stu-id="4168d-111">How can Azure Automation help manage Azure cloud services?</span></span>
<span data-ttu-id="4168d-112">Cloudové služby Azure lze spravovat ve službě Azure Automation pomocí rutin prostředí PowerShell, které jsou k dispozici v [nástroje Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="4168d-112">Azure cloud services can be managed in Azure Automation by using the PowerShell cmdlets that are available in the [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="4168d-113">Automatizace Azure má tyto k dispozici cloudové rutiny prostředí PowerShell služby předinstalované, aby mohli provést všechny úkoly správy cloudové služby v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="4168d-113">Azure Automation has these cloud service PowerShell cmdlets available out of the box, so that you can perform all of your cloud service management tasks within the service.</span></span> <span data-ttu-id="4168d-114">Může také párovat tyto rutiny ve službě Azure Automation pomocí rutin pro jinými službami Azure, na automatizují komplexní úlohy v služeb Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="4168d-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="4168d-115">Některé příklad použití Azure Automation pro správu Azure Cloud Services patří:</span><span class="sxs-lookup"><span data-stu-id="4168d-115">Some example uses of Azure Automation to manage Azure Cloud Services include:</span></span>

* [<span data-ttu-id="4168d-116">Nepřetržité nasazení cloudové služby při každé aktualizaci cscfg nebo cspkg v Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="4168d-116">Continous deployment of a Cloud Service whenever cscfg or cspkg is updated in Azure Blob storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [<span data-ttu-id="4168d-117">Restartování instance cloudové služby paralelně jednu upgradovací doménu najednou</span><span class="sxs-lookup"><span data-stu-id="4168d-117">Rebooting Cloud Service instances in parallel, one upgrade domain at a time</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a><span data-ttu-id="4168d-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4168d-118">Next Steps</span></span>
<span data-ttu-id="4168d-119">Teď, když jste se naučili základy Azure Automation a jak může sloužit ke správě cloudových služeb Azure, postupujte podle následujících odkazech na další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4168d-119">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure cloud services, follow these links to learn more about Azure Automation.</span></span>

* [<span data-ttu-id="4168d-120">Přehled služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="4168d-120">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="4168d-121">Můj první runbook</span><span class="sxs-lookup"><span data-stu-id="4168d-121">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="4168d-122">Mapy kurzů Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4168d-122">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)
