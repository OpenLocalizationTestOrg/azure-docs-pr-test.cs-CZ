---
title: "režimy aaaResource Manager a Service Management (klasické) nasazení | Microsoft Docs"
description: "Přečtěte si hello rozdíly mezi Resource Manager a modely nasazení classic."
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
ms.openlocfilehash: e14f815ba9d79d9dd8f83b45bda78d0eba0bec56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-deployment-models"></a>Modely nasazení Azure
Hello platformy Azure je v přechodném stavu.  Jestli chcete novou tooAzure nebo jste použili ji pro let, je důležité toounderstand některé změny klíče hello provádíme za toohello platformy během tento přechod.

Všechny prostředky Azure podporují jedno nebo obě hello následující modely nasazení:

* **Správce prostředků:** jde hello nejnovější model nasazení pro prostředky Azure. Většina novější prostředky již podporují tento model nasazení a nakonec se všechny prostředky.   
* **Klasické:** tento model podporuje většinu existující prostředky Azure ještě dnes. Nové prostředky přidat tooAzure nebude podporovat tento model.

Hello dokumentace pro každé prostředků Azure. Podrobnosti modelů služby, která ho lze vytvořit pomocí.

## <a name="why-does-this-matter"></a>Proč této věci?
Ho záleží pro hello následujících důvodů:

* Funkce Hello platformy Azure, které můžete použít se liší v těchto dvou modelech.  Například prostředky, které jsou vytvořené pomocí modelu nasazení Resource Manager hello (nebo jenom Resource Manager) může být vytvořen pomocí [šablon Azure Resource Manageru](azure-resource-manager/resource-group-overview.md#template-deployment), zatímco nelze prostředky, které jsou vytvořené pomocí modelu nasazení Classic hello.
* Hello funkce jednotlivých prostředků Azure nebo chování může lišit mezi dva modely hello nebo existují jenom v jednom modelu nebo hello jiné.  Je třeba Vyrovnávání zatížení provozu napříč virtuální počítače vytvořené pomocí modelu nasazení Classic hello *implicitní* protože zatížení automaticky rovnoměrně rozdělen mezi virtuální a virtuální počítače jsou členy Cloudová služba Azure počítače v rámci cloudové služby. Virtuální počítače vytvořené pomocí Resource Manager nejsou členy cloudové služby, a samostatný nástroj pro vyrovnávání zatížení Azure prostředek musí být *explicitně* vytvořit tooload vyrovnávání přenosů mezi několik virtuálních počítačů.  
* Jak vytvořit, konfiguraci a správě prostředků Azure se liší mezi tyto dva modely.
* Vytvořený jeden model nasazení prostředků nelze nutně spolupracovat s prostředky, které jsou vytvořené pomocí modelu jiné nasazení. Například může být virtuální počítače Azure vytvořené pomocí jeden model nasazení pouze tooAzure připojené virtuální sítě vytvořené pomocí hello stejném modelu nasazení.    

Základní všechny modely nasazení hello je programovací rozhraní aplikace (API) pro každého prostředku.  Je [rozhraní API Správce prostředků](https://msdn.microsoft.com/library/azure/dn948464.aspx) pro model nasazení Resource Manager hello a [Service Management API](https://msdn.microsoft.com/library/azure/ee460799.aspx) pro model nasazení Classic hello. Vývojářům psát kód toointeract s Tato rozhraní API *přímo*.  

IT specialisté však obvykle interakci s Tato rozhraní API *nepřímo* pomocí grafického rozhraní portálu ve webovém prohlížeči pomocí prostředí Azure PowerShell v počítači s Windows, nebo pomocí rutiny hello rozhraní příkazového řádku Azure (CLI) na buď Počítač, Windows, OS X nebo Linux. Všechny tři z těchto nepřímých metod používaných hello IT specialista komunikovat přímo s hello rozhraní API. To znamená, že pokud je nová funkce přináší toohello Azure platformy nebo prostředky, je vždy přímo k dispozici prostřednictvím hello rozhraní API, nejprve s hello nepřímé metody získání podpory pro hello nové prostředky a funkce po hello rozhraní API je k dispozici.  

v níže uvedených částech Hello vysvětlují, jak Azure prostředky jsou konfigurováni pomocí hello různé modely nasazení prostřednictvím hello tři nepřímých metod.

## <a name="portals"></a>Portály
Azure má dva portály:

* **[Portál Azure](https://manage.windowsazure.com):** Pokud jste dosud používali Azure nějakou dobu, jste použili tento portál. Je použité toocreate a konfigurace starší prostředků Azure, které podporují hello modelu nasazení classic. Nelze používat toocreate nebo konfiguraci prostředků, které podporují pouze správce prostředků. 
* **[Portál Azure preview](https://azure.microsoft.com/overview/preview-portal/):** Pokud používáte novější prostředků Azure, pravděpodobně jste použili tento portál. Může být použité toocreate a nakonfigurovat některé prostředky Azure. Nakonec můžete být schopný toocreate a nakonfigurovat všechny prostředky Azure. Pro některé prostředky, které podporují obou modelů nasazení můžete tento portál se použít toocreate a nakonfigurovat pomocí buď model nasazení prostředku. 

Některé prostředky a funkce jenom vytvořit a nakonfigurovat v jednom portál nebo hello jiné. Některé prostředky nebo funkce (ještě) nelze vytvořit nebo nakonfigurovat buď portálu a lze konfigurovat pouze v prostředí PowerShell, hello rozhraní příkazového řádku, nebo obojí. Hello dokumentace pro každého prostředku Azure podrobnosti, která metoda může být vytvořen pomocí. 

## <a name="powershell"></a>PowerShell
S [prostředí PowerShell](/powershell/azureps-cmdlets-docs) můžete použít příkazovém řádku nebo autora toocreate skripty a konfigurace prostředků z počítače Windows Azure.  Jednotlivé prostředky Azure mají [rutiny správce prostředků](/powershell/azure/overview), [rutiny pro správu služby](/powershell/azure/overview?view=azuresmps-3.7.0), nebo obojí.  Některé prostředky a funkce mohou být vytvořeny pouze nebo nakonfigurovaný pomocí prostředí PowerShell nebo hello rozhraní příkazového řádku. V závislosti na hello prostředků, při použití rutin prostředí PowerShell pro správce prostředků může mít dvě možnosti pro vytváření a konfigurace prostředků Azure:

* **Rutiny prostředí PowerShell pouze:** můžete vytvořit a nakonfigurovat každý prostředků Azure jednotlivě pomocí rutiny hello pro každého prostředku. Můžete to udělat z příkazového řádku nebo včetně více příkazů skriptu PowerShell, který může ukládat a verze.
* **Rutiny prostředí PowerShell pomocí šablony Azure Resource Manager:** můžete použít PowerShell toocreate Azure prostředky pomocí šablony Azure Resource Manager. Šablony může být uložený a verzí. Další informace načtením hello [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md) článku. Několik [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) pro běžné řešení, které je možné stáhnout a upravit příliš neexistují.

## <a name="cli"></a>Rozhraní příkazového řádku
Můžete vytvořit a nakonfigurovat prostředky Azure z Windows, OS X nebo Linux počítačů pomocí hello rozhraní příkazového řádku.  Čtení hello [hello instalace rozhraní příkazového řádku Azure](cli-install-nodejs.md) článku tooinstall hello rozhraní příkazového řádku v operačním systému výběru. Jako prostředí PowerShell, jsou jiné příkazy, které se mají použít v závislosti na tom, zda vytváříte prostředků pomocí [Resource Manager](xplat-cli-azure-resource-manager.md) nebo hello [Classic (správou služeb)](virtual-machines/linux/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) modely nasazení.

## <a name="next-steps"></a>Další kroky
* Další informace o [Resource Manager](azure-resource-manager/resource-group-overview.md).
* Pochopit, jak příliš[návrh šablony](best-practices-resource-manager-design-templates.md).

