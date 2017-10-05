---
title: "Správa virtuálních počítačů pomocí Azure Automation | Microsoft Docs"
description: "Další informace o používání služby Azure Automation ke správě virtuálních počítačů Azure ve velkém měřítku."
services: virtual-machines-windows, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: ce49f83a-f409-42ee-af74-a8ea2caa9ae8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2016
ms.author: timlt
ms.openlocfilehash: 15653c5d653ae538bdb66eaf0daee12c35858b45
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-virtual-machines-using-azure-automation"></a><span data-ttu-id="a3057-103">Správa virtuálních počítačů Azure pomocí Azure Automation</span><span class="sxs-lookup"><span data-stu-id="a3057-103">Managing Azure Virtual Machines using Azure Automation</span></span>
<span data-ttu-id="a3057-104">Tento průvodce vás seznámí s služba Azure Automation a jak ji můžete použít k zjednodušení správy virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="a3057-104">This guide introduces you to the Azure Automation service and how it can be used to simplify managing your Azure virtual machines.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="a3057-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="a3057-105">What is Azure Automation?</span></span>
<span data-ttu-id="a3057-106">[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="a3057-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="a3057-107">Pomocí Azure Automation, dlouhotrvajících, ruční, problematických a často se opakujících úloh je možné automatizovat zvýšit spolehlivost, efektivitu a času na hodnotu pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="a3057-107">By using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated to increase reliability, efficiency, and time-to-value for your organization.</span></span>

<span data-ttu-id="a3057-108">Azure Automation nabízí modul provádění vysoce spolehlivé a vysoce dostupné pracovního postupu, který rozšiřuje podle vašich potřeb podle růstu vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="a3057-108">Azure Automation provides a highly reliable and highly available workflow execution engine that scales to meet your needs as your organization grows.</span></span> <span data-ttu-id="a3057-109">Ve službě Azure Automation procesů může být spuštěna ručně, systémy třetích stran nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a3057-109">In Azure Automation, processes can be kicked off manually, by third-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="a3057-110">Můžete snížit provozní režie a uvolněte IT a zaměstnanci DevOps a zaměřit se na práci, kterou přidá obchodní hodnotu spuštěním cloudu úlohy správy automaticky pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a3057-110">You can lower operational overhead and free up IT and DevOps staff to focus on work that adds business value by running your cloud management tasks automatically with Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-virtual-machines"></a><span data-ttu-id="a3057-111">Jak Azure Automation pomoci spravovat virtuální počítače Azure?</span><span class="sxs-lookup"><span data-stu-id="a3057-111">How can Azure Automation help manage Azure virtual machines?</span></span>
<span data-ttu-id="a3057-112">Virtuální počítače lze spravovat ve službě Azure Automation pomocí [prostředí Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3057-112">Virtual machines can be managed in Azure Automation by using [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="a3057-113">Služby Azure Automation zahrnuje rutiny prostředí Azure PowerShell, můžete provést všechny úkoly správy virtuálního počítače v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="a3057-113">Azure Automation includes the Azure PowerShell cmdlets, so you can perform all of your virtual machine management tasks within the service.</span></span> <span data-ttu-id="a3057-114">Může také párovat rutiny ve službě Azure Automation pomocí rutin pro jinými službami Azure, na automatizují komplexní úlohy napříč službami Azure a systémech třetích stran.</span><span class="sxs-lookup"><span data-stu-id="a3057-114">You can also pair the cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and third-party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3057-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a3057-115">Next steps</span></span>
<span data-ttu-id="a3057-116">Teď, když jste se naučili základy Azure Automation a jak může sloužit ke správě virtuálních počítačích Azure, další informace:</span><span class="sxs-lookup"><span data-stu-id="a3057-116">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure virtual machines, learn more:</span></span>

* [<span data-ttu-id="a3057-117">Přehled služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="a3057-117">Azure Automation Overview</span></span>](../../automation/automation-intro.md)
* [<span data-ttu-id="a3057-118">Můj první runbook</span><span class="sxs-lookup"><span data-stu-id="a3057-118">My first runbook</span></span>](../../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="a3057-119">Mapy kurzů Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a3057-119">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)

