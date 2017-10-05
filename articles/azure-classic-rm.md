---
title: "Režimy (klasické) nasazení Resource Manager a Správa služby | Microsoft Docs"
description: "Přečtěte si rozdíly mezi Resource Manager a modely nasazení classic."
services: virtual-network
documentationcenter: 
author: telmosampaio
manager: carmonm
editor: 
tags: azure-resource-manager,azure-service-management
redirect_url: ./azure-resource-manager/resource-manager-deployment-model
ms.assetid: 18a235d8-38ac-4886-9e56-b3855c73ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: telmos
ms.openlocfilehash: 0f451efa239dd36d82229f01cc385d6df46654f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-deployment-models"></a>Modely nasazení Azure
Platformy Azure je v přechodném stavu.  Ať už jste Azure ještě nepoužívali nebo jste použili ji pro let, je důležité si uvědomit, některé z klíčových změn, které provádíme za účelem platformou během tento přechod.

Všechny prostředky Azure podporují jedno nebo obě následující modely nasazení:

* **Správce prostředků:** Toto je nejnovější model nasazení pro prostředky Azure. Většina novější prostředky již podporují tento model nasazení a nakonec se všechny prostředky.   
* **Klasické:** tento model podporuje většinu existující prostředky Azure ještě dnes. Tento model nebude podporovat nové prostředky přidat do Azure.

Dokumentace pro každé prostředků Azure. Podrobnosti modelů služby, která ho lze vytvořit pomocí.

## <a name="why-does-this-matter"></a>Proč této věci?
Ho záleží z následujících důvodů:

* Funkce platformy Azure, které můžete použít se liší v těchto dvou modelech.  Například prostředky, které jsou vytvořené pomocí Resource Manager modelu nasazení (nebo jenom Resource Manager) se dají vytvořit s [šablon Azure Resource Manageru](azure-resource-manager/resource-group-overview.md#template-deployment), zatímco nelze prostředky, které jsou vytvořené pomocí modelu nasazení Classic.
* Funkce jednotlivých prostředků Azure nebo chování může být odlišné mezi dva modely nebo existují jenom v jednom modelu nebo dalších.  Je třeba Vyrovnávání zatížení provozu napříč virtuální počítače vytvořené pomocí modelu nasazení Classic *implicitní* protože zatížení automaticky rovnoměrně rozdělen mezi virtuální a virtuální počítače jsou členy Cloudová služba Azure počítače v rámci cloudové služby. Virtuální počítače vytvořené pomocí Resource Manager nejsou členy cloudové služby a samostatné Vyrovnávání zatížení Azure prostředek musí být *explicitně* vytvořené pro zatížení vyrovnávání přenosů v rámci více virtuálních počítačů.  
* Jak vytvořit, konfiguraci a správě prostředků Azure se liší mezi tyto dva modely.
* Vytvořený jeden model nasazení prostředků nelze nutně spolupracovat s prostředky, které jsou vytvořené pomocí modelu jiné nasazení. Například můžete vytvořit jeden model nasazení pomocí virtuálních počítačů Azure připojit pouze k Azure virtuální sítě vytvořené pomocí modelu nasazení.    

Základní všechny modely nasazení je programovací rozhraní aplikace (API) pro každého prostředku.  Je [rozhraní API Správce prostředků](https://msdn.microsoft.com/library/azure/dn948464.aspx) pro model nasazení Resource Manager a [Service Management API](https://msdn.microsoft.com/library/azure/ee460799.aspx) pro model nasazení Classic. Vývojářům můžete napsat kód pro interakci s Tato rozhraní API *přímo*.  

IT specialisté však obvykle interakci s Tato rozhraní API *nepřímo* pomocí grafického rozhraní portálu ve webovém prohlížeči, a to pomocí rutin prostředí Azure PowerShell v počítači s Windows nebo na systém Windows pomocí rozhraní příkazového řádku Azure (CLI) , OS X nebo počítače se systémem Linux. Všechny tři z těchto nepřímých metod používaných IT specialista komunikovat přímo s rozhraními API sady. To znamená, že když zavádí nové funkce pro platformu Azure nebo prostředky, je vždy přímo k dispozici prostřednictvím rozhraní API nejdřív pomocí nepřímé metod získat podporu pro nové prostředky a funkce po rozhraní API je k dispozici.  

V níže uvedených částech popisují, jak Azure prostředky jsou konfigurováni pomocí různé modely nasazení prostřednictvím tři nepřímé metody.

## <a name="portals"></a>Portály
Azure má dva portály:

* **[Portál Azure](https://manage.windowsazure.com):** Pokud jste dosud používali Azure nějakou dobu, jste použili tento portál. Umožňuje vytvořit a nakonfigurovat starší prostředky Azure, které podporují modelu nasazení classic. Nelze ho použít k vytvoření nebo konfiguraci prostředků, které podporují pouze správce prostředků. 
* **[Portál Azure preview](https://azure.microsoft.com/overview/preview-portal/):** Pokud používáte novější prostředků Azure, pravděpodobně jste použili tento portál. Slouží k vytvoření a konfiguraci některých prostředků Azure. Nakonec budete moci vytvořit a nakonfigurovat všechny prostředky Azure. Pro některé prostředky, které podporují obou modelů nasazení můžete použít tento portál vytvořit a nakonfigurovat pomocí buď model nasazení prostředku. 

Některé prostředky a funkce jenom vytvořit a nakonfigurované v jedné portál nebo dalších. Některé prostředky nebo funkce (ještě) nelze vytvořit nebo nakonfigurovat buď portálu a můžete nakonfigurovat jen pomocí prostředí PowerShell, rozhraní příkazového řádku nebo obojí. Dokumentace pro každého prostředku Azure podrobnosti, která metoda může být vytvořen pomocí. 

## <a name="powershell"></a>PowerShell
S [prostředí PowerShell](/powershell/azureps-cmdlets-docs) můžete použít příkazový řádek nebo vytvářet skripty k vytvoření a konfigurace prostředků z počítače Windows Azure.  Jednotlivé prostředky Azure mají [rutiny správce prostředků](/powershell/azure/overview), [rutiny pro správu služby](/powershell/azure/overview?view=azuresmps-3.7.0), nebo obojí.  Některé prostředky a funkce mohou být vytvořeny pouze nebo nakonfigurovaný pomocí prostředí PowerShell nebo rozhraní příkazového řádku. V závislosti na prostředek, při použití rutin prostředí PowerShell pro správce prostředků může mít dvě možnosti pro vytváření a konfigurace prostředků Azure:

* **Rutiny prostředí PowerShell pouze:** můžete vytvořit a nakonfigurovat každý prostředků Azure jednotlivě pomocí rutin pro každý zdroj. Můžete to udělat z příkazového řádku nebo včetně více příkazů skriptu PowerShell, který může ukládat a verze.
* **Rutiny prostředí PowerShell pomocí šablony Azure Resource Manager:** prostředí PowerShell můžete použít k vytváření prostředků Azure pomocí šablony Azure Resource Manager. Šablony může být uložený a verzí. Další informace načtením [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md) článku. Několik [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) pro běžné řešení, které je možné stáhnout a upravit příliš neexistují.

## <a name="cli"></a>Rozhraní příkazového řádku
Můžete vytvořit a nakonfigurovat prostředky Azure z Windows, OS X nebo Linux počítačů pomocí rozhraní příkazového řádku.  Pro čtení [nainstalovat Azure CLI](cli-install-nodejs.md) článku nainstalovat rozhraní příkazového řádku na operačním systému výběru. Jako prostředí PowerShell, jsou jiné příkazy, které se mají použít v závislosti na tom, zda vytváříte prostředků pomocí [Resource Manager](xplat-cli-azure-resource-manager.md) nebo [Classic (správou služeb)](virtual-machines/linux/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) modely nasazení.

## <a name="next-steps"></a>Další kroky
* Další informace o [Resource Manager](azure-resource-manager/resource-group-overview.md).
* Pochopit, jak [návrh šablony](best-practices-resource-manager-design-templates.md).

