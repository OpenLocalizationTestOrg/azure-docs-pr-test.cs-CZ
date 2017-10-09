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
# <a name="community-tools-toomigrate-iaas-resources-from-classic-tooazure-resource-manager"></a>Komunita nástroje toomigrate IaaS prostředky z classic tooAzure Resource Manager
Tento článek katalogů hello nástroje, které poskytly hello komunity tooassist s migrací prostředky infrastruktury z modelu nasazení classic toohello Azure Resource Manager.

> [!NOTE]
> Tyto nástroje nejsou oficiálně podporována Microsoft Support. Proto jsou open source na Githubu a máme radostí tooaccept PRs opravy nebo další scénáře. tooreport nějaký problém, použijte hello Githubu problémy funkce.
> 
> Migrace s těmito nástroji způsobí, že výpadek pro klasické virtuální počítač. Pokud hledáte platformy podporované migrace, navštivte 
> 
>   * [Platforma podporovaná migrace prostředky infrastruktury z klasického tooAzure Resource Manager zásobníku](migration-classic-resource-manager-overview.md)
>   * [Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md)
>   * [Migrace prostředků IaaS z tooAzure klasického správce prostředků pomocí Azure PowerShell](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Toto je kolekce pomocné nástroje, které jsou vytvořené jako součást enterprise migrace z Azure Service Management tooAzure Resource Manager. Tento nástroj vám umožní tooreplicate infrastruktury do jiné předplatné, které lze použít pro testování migrace a před spuštěním hello migrace na vaše předplatné produkční železo na všechny problémy.

[Dokumentaci k nástroji toohello odkaz](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz je další možnost toomigrate kompletní sadu klasické prostředky Resource Manager IaaS tooAzure IaaS prostředky. Hello migrace může dojít v rámci hello stejné předplatné nebo mezi různých předplatných a typy předplatného (např: CSP odběry).

[Dokumentaci k nástroji toohello odkaz](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Další kroky

* [Přehled migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Pomocí prostředí PowerShell toomigrate IaaS prostředky z classic tooAzure Resource Manager](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Pomocí rozhraní příkazového řádku toomigrate IaaS prostředky z classic tooAzure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Běžné chyby při migraci](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Zkontrolujte hello nejčastěji kladené dotazy týkající se migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

