---
title: "Nástroje pro aaaCommunity – přesunout tooAzure klasické prostředky Resource Manager | Microsoft Docs"
description: "Tento článek katalogů hello nástroje, které poskytly hello komunity toohelp migrovat prostředky infrastruktury z modelu nasazení classic toohello Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 67d5624a14d12a2d9eb46eb12aef461b706d4589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="community-tools-toomigrate-iaas-resources-from-classic-tooazure-resource-manager"></a><span data-ttu-id="b6e14-103">Komunita nástroje toomigrate IaaS prostředky z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b6e14-103">Community tools toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>
<span data-ttu-id="b6e14-104">Tento článek katalogů hello nástroje, které poskytly hello komunity tooassist s migrací prostředky infrastruktury z modelu nasazení classic toohello Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b6e14-104">This article catalogs hello tools that have been provided by hello community tooassist with migration of IaaS resources from classic toohello Azure Resource Manager deployment model.</span></span>

> [!NOTE]
> <span data-ttu-id="b6e14-105">Tyto nástroje nejsou oficiálně podporována Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="b6e14-105">These tools are not officially supported by Microsoft Support.</span></span> <span data-ttu-id="b6e14-106">Proto jsou open source na Githubu a máme radostí tooaccept PRs opravy nebo další scénáře.</span><span class="sxs-lookup"><span data-stu-id="b6e14-106">Therefore they are open sourced on GitHub and we're happy tooaccept PRs for fixes or additional scenarios.</span></span> <span data-ttu-id="b6e14-107">tooreport nějaký problém, použijte hello Githubu problémy funkce.</span><span class="sxs-lookup"><span data-stu-id="b6e14-107">tooreport an issue, use hello GitHub issues feature.</span></span>
> 
> <span data-ttu-id="b6e14-108">Migrace s těmito nástroji způsobí, že výpadek pro klasické virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b6e14-108">Migrating with these tools will cause downtime for your classic Virtual Machine.</span></span> <span data-ttu-id="b6e14-109">Pokud hledáte platformy podporované migrace, navštivte</span><span class="sxs-lookup"><span data-stu-id="b6e14-109">If you're looking for platform supported migration, visit</span></span> 
> 
>   * [<span data-ttu-id="b6e14-110">Platforma podporovaná migrace prostředky infrastruktury z klasického tooAzure Resource Manager zásobníku</span><span class="sxs-lookup"><span data-stu-id="b6e14-110">Platform supported migration of IaaS resources from Classic tooAzure Resource Manager stack</span></span>](migration-classic-resource-manager-overview.md)
>   * [<span data-ttu-id="b6e14-111">Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b6e14-111">Technical Deep Dive on Platform supported migration from Classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md)
>   * [<span data-ttu-id="b6e14-112">Migrace prostředků IaaS z tooAzure klasického správce prostředků pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b6e14-112">Migrate IaaS resources from Classic tooAzure Resource Manager using Azure PowerShell</span></span>](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a><span data-ttu-id="b6e14-113">AsmMetadataParser</span><span class="sxs-lookup"><span data-stu-id="b6e14-113">AsmMetadataParser</span></span>
<span data-ttu-id="b6e14-114">Toto je kolekce pomocné nástroje, které jsou vytvořené jako součást enterprise migrace z Azure Service Management tooAzure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b6e14-114">This is a collection of helper tools created as part of enterprise migrations from Azure Service Management tooAzure Resource Manager.</span></span> <span data-ttu-id="b6e14-115">Tento nástroj vám umožní tooreplicate infrastruktury do jiné předplatné, které lze použít pro testování migrace a před spuštěním hello migrace na vaše předplatné produkční železo na všechny problémy.</span><span class="sxs-lookup"><span data-stu-id="b6e14-115">This tool allows you tooreplicate your infrastructure into another subscription which can be used for testing migration and iron out any issues before running hello migration on your Production subscription.</span></span>

[<span data-ttu-id="b6e14-116">Dokumentaci k nástroji toohello odkaz</span><span class="sxs-lookup"><span data-stu-id="b6e14-116">Link toohello tool documentation</span></span>](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a><span data-ttu-id="b6e14-117">migAz</span><span class="sxs-lookup"><span data-stu-id="b6e14-117">migAz</span></span>
<span data-ttu-id="b6e14-118">migAz je další možnost toomigrate kompletní sadu klasické prostředky Resource Manager IaaS tooAzure IaaS prostředky.</span><span class="sxs-lookup"><span data-stu-id="b6e14-118">migAz is an additional option toomigrate a complete set of classic IaaS resources tooAzure Resource Manager IaaS resources.</span></span> <span data-ttu-id="b6e14-119">Hello migrace může dojít v rámci hello stejné předplatné nebo mezi různých předplatných a typy předplatného (např: CSP odběry).</span><span class="sxs-lookup"><span data-stu-id="b6e14-119">hello migration can occur within hello same subscription or between different subscriptions and subscription types (ex: CSP subscriptions).</span></span>

[<span data-ttu-id="b6e14-120">Dokumentaci k nástroji toohello odkaz</span><span class="sxs-lookup"><span data-stu-id="b6e14-120">Link toohello tool documentation</span></span>](https://github.com/Azure/migAz)

## <a name="next-steps"></a><span data-ttu-id="b6e14-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6e14-121">Next Steps</span></span>

* [<span data-ttu-id="b6e14-122">Přehled migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b6e14-122">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="b6e14-123">Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b6e14-123">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="b6e14-124">Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b6e14-124">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="b6e14-125">Pomocí prostředí PowerShell toomigrate IaaS prostředky z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b6e14-125">Use PowerShell toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="b6e14-126">Pomocí rozhraní příkazového řádku toomigrate IaaS prostředky z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b6e14-126">Use CLI toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="b6e14-127">Běžné chyby při migraci</span><span class="sxs-lookup"><span data-stu-id="b6e14-127">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="b6e14-128">Zkontrolujte hello nejčastěji kladené dotazy týkající se migrace prostředky infrastruktury z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b6e14-128">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

