---
title: "aaaManage virtuálních počítačů pomocí Azure Automation | Microsoft Docs"
description: "Další informace o jak hello služby Azure Automation lze použít toomanage virtuálních počítačů Azure ve velkém měřítku."
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
ms.openlocfilehash: bfe7b3a51b6e82bd7cd5b0a83df7226476ed4f36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-virtual-machines-using-azure-automation"></a><span data-ttu-id="0f3e5-103">Správa virtuálních počítačů Azure pomocí Azure Automation</span><span class="sxs-lookup"><span data-stu-id="0f3e5-103">Managing Azure Virtual Machines using Azure Automation</span></span>
<span data-ttu-id="0f3e5-104">Tento průvodce vás seznámí toohello služba Azure Automation a jak může sloužit toosimplify spravovat virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="0f3e5-104">This guide introduces you toohello Azure Automation service and how it can be used toosimplify managing your Azure virtual machines.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="0f3e5-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="0f3e5-105">What is Azure Automation?</span></span>
<span data-ttu-id="0f3e5-106">[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="0f3e5-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="0f3e5-107">Pomocí Azure Automation, může být časově náročné, ruční, problematických a často se opakujících úloh automatizované tooincrease spolehlivost, efektivitu a času na hodnotu pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="0f3e5-107">By using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability, efficiency, and time-to-value for your organization.</span></span>

<span data-ttu-id="0f3e5-108">Azure Automation nabízí modulu provádění vysoce spolehlivé a vysoce dostupné pracovního postupu, který přizpůsobí vašim potřebám toomeet podle růstu vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="0f3e5-108">Azure Automation provides a highly reliable and highly available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="0f3e5-109">Ve službě Azure Automation procesů může být spuštěna ručně, systémy třetích stran nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="0f3e5-109">In Azure Automation, processes can be kicked off manually, by third-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="0f3e5-110">Můžete snížit provozní režie a uvolněte IT a DevOps služební toofocus na práci, kterou přidá obchodní hodnotu spuštěním cloudu úlohy správy automaticky pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="0f3e5-110">You can lower operational overhead and free up IT and DevOps staff toofocus on work that adds business value by running your cloud management tasks automatically with Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-virtual-machines"></a><span data-ttu-id="0f3e5-111">Jak Azure Automation pomoci spravovat virtuální počítače Azure?</span><span class="sxs-lookup"><span data-stu-id="0f3e5-111">How can Azure Automation help manage Azure virtual machines?</span></span>
<span data-ttu-id="0f3e5-112">Virtuální počítače lze spravovat ve službě Azure Automation pomocí [prostředí Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f3e5-112">Virtual machines can be managed in Azure Automation by using [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="0f3e5-113">Služby Azure Automation zahrnuje rutin prostředí Azure PowerShell text hello, takže můžete provádět všechny úkoly správy virtuálního počítače v rámci služby hello.</span><span class="sxs-lookup"><span data-stu-id="0f3e5-113">Azure Automation includes hello Azure PowerShell cmdlets, so you can perform all of your virtual machine management tasks within hello service.</span></span> <span data-ttu-id="0f3e5-114">Hello rutin ve službě Azure Automation s hello rutiny pro ostatní služby Azure, tooautomate složité úlohy může také párovat napříč službami Azure a systémech třetích stran.</span><span class="sxs-lookup"><span data-stu-id="0f3e5-114">You can also pair hello cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and third-party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f3e5-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f3e5-115">Next steps</span></span>
<span data-ttu-id="0f3e5-116">Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage virtuální počítače Azure, další informace:</span><span class="sxs-lookup"><span data-stu-id="0f3e5-116">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure virtual machines, learn more:</span></span>

* [<span data-ttu-id="0f3e5-117">Přehled služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="0f3e5-117">Azure Automation Overview</span></span>](../../automation/automation-intro.md)
* [<span data-ttu-id="0f3e5-118">Můj první runbook</span><span class="sxs-lookup"><span data-stu-id="0f3e5-118">My first runbook</span></span>](../../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="0f3e5-119">Mapy kurzů Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="0f3e5-119">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)

