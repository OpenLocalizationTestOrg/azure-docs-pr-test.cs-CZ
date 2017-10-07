---
title: "aaaManage Azure Storage pomocí Azure Automation."
description: "Informace o tom, jak hello služby Azure Automation může být použité toomanage Azure Storage ve velkém měřítku."
services: storage, automation
documentationcenter: 
author: jodoglevy
manager: eamono
editor: 
ms.assetid: bac41c39-1d95-46aa-a481-ec17bbb21b40
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: eamono
ms.openlocfilehash: 5edfba1a9ce1f9c81b5bd0889de19099f3d86fc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-storage-using-azure-automation"></a><span data-ttu-id="f4929-103">Správa úložiště Azure pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="f4929-103">Managing Azure Storage using Azure Automation</span></span>
<span data-ttu-id="f4929-104">Tento průvodce vás seznámí toohello služba Azure Automation a jak může být použité toosimplify správy úložiště Azure BLOB, tabulek a front.</span><span class="sxs-lookup"><span data-stu-id="f4929-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of your Azure Storage blobs, tables, and queues.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="f4929-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="f4929-105">What is Azure Automation?</span></span>
<span data-ttu-id="f4929-106">[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="f4929-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="f4929-107">Pomocí Azure Automation, dlouhotrvajících, ruční, problematických a často opakované úlohy jde automatizované tooincrease spolehlivost a efektivitu a snížit čas toovalue pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="f4929-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability and efficiency, and reduce time toovalue for your organization.</span></span>

<span data-ttu-id="f4929-108">Azure Automation nabízí modulu provádění vysoce spolehlivé a vysoce dostupné pracovního postupu, který přizpůsobí vašim potřebám toomeet podle růstu vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="f4929-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="f4929-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f4929-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="f4929-110">Nižší provozní režie a uvolněte IT / DevOps zaměstnanci toofocus na práci, kterou přidá obchodní hodnotu přesunutím vaší toobe úlohy správy cloudu automaticky spouštěné v Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="f4929-110">Lower operational overhead and free up IT / DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-storage"></a><span data-ttu-id="f4929-111">Jak Azure Automation pomoci spravovat úložiště Azure?</span><span class="sxs-lookup"><span data-stu-id="f4929-111">How can Azure Automation help manage Azure Storage?</span></span>
<span data-ttu-id="f4929-112">Úložiště Azure lze spravovat ve službě Azure Automation pomocí rutin prostředí PowerShell text hello, které jsou k dispozici v [prostředí Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="f4929-112">Azure Storage can be managed in Azure Automation by using hello PowerShell cmdlets that are available in [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="f4929-113">Automatizace Azure má tyto rutiny prostředí PowerShell úložiště k dispozici předinstalované hello, takže provedením všech objektů blob, tabulky a fronty úloh správy v rámci služby hello.</span><span class="sxs-lookup"><span data-stu-id="f4929-113">Azure Automation has these Storage PowerShell cmdlets available out of hello box, so that you can perform all of your blob, table, and queue management tasks within hello service.</span></span> <span data-ttu-id="f4929-114">Tyto rutiny ve službě Azure Automation pomocí hello rutin pro ostatní služby Azure, tooautomate složité úlohy může také párovat napříč službami Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="f4929-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="f4929-115">Hello [Galerie runbooků automatizace Azure](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) obsahuje celou řadu produktu team a Komunita sady runbook tooget začít s automatizací správy Azure Storage, ostatní služby Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="f4929-115">hello [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contains a variety of product team and community runbooks tooget started automating management of Azure Storage, other Azure services, and 3rd party systems.</span></span> <span data-ttu-id="f4929-116">Galerie runbooků patří:</span><span class="sxs-lookup"><span data-stu-id="f4929-116">Gallery runbooks include:</span></span>

* [<span data-ttu-id="f4929-117">Odebrání úložiště objektů BLOB Azure, které jsou některé dní starý pomocí služby Automation</span><span class="sxs-lookup"><span data-stu-id="f4929-117">Remove Azure Storage Blobs that are Certain Days Old Using Automation Service</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [<span data-ttu-id="f4929-118">Objekt Blob stažení ze služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f4929-118">Download a Blob from Azure Storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [<span data-ttu-id="f4929-119">Zálohovat všechny disky pro jeden virtuální počítač Azure nebo pro všechny virtuální počítače v cloudové službě</span><span class="sxs-lookup"><span data-stu-id="f4929-119">Backup all disks for a single Azure VM or for all VMs in a Cloud Service</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a><span data-ttu-id="f4929-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4929-120">Next Steps</span></span>
<span data-ttu-id="f4929-121">Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage, které Azure úložiště objektů BLOB, tabulek a, postupujte podle těchto odkazů toolearn Další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="f4929-121">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure Storage blobs, tables, and queues, follow these links toolearn more about Azure Automation.</span></span>

<span data-ttu-id="f4929-122">Zobrazit kurz pro Azure Automation hello [vytvoření nebo import runbooku ve službě Azure Automation](../../automation/automation-creating-importing-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="f4929-122">See hello Azure Automation tutorial [Creating or importing a runbook in Azure Automation](../../automation/automation-creating-importing-runbook.md).</span></span>

