---
title: "Glosář aaaAzure - Azure slovník | Microsoft Docs"
description: "Použijte hello Azure Glosář toounderstand cloudu terminologie na hello platformy Azure. Tento krátký Azure slovník obsahuje definice pro běžné podmínky cloud pro Azure."
keywords: "Azure slovník, terminologie cloudu, Azure glosář, definic termínů, podmínky cloudu"
services: na
documentationcenter: na
author: MonicaRush
manager: jhubbard
editor: 
ms.assetid: d7ac12f7-24b5-4bcd-9e4d-3d76fbd8d297
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: monicar
ms.openlocfilehash: 486bbbfc71a48a6ebc39b14f7ab71f8604b7a6b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-hello-azure-platform"></a>Glosář Microsoft Azure: slovník terminologie cloudu na hello platformy Azure

Glosář Hello Microsoft Azure je slovník krátké cloudu terminologie pro hello platformy Azure. Viz také:

* [Microsoft Azure a Amazon Web Services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) -služeb definice Azure a jejich protějšky AWS.<!-- I propose toolink toohttps://azure.microsoft.com/en-us/services/ instead of this -->
* [Cloudová výpočetní podmínky](https://azure.microsoft.com/overview/cloud-computing-dictionary/) -obecné odvětví cloudu podmínky.

## <a name="account"></a>Účet
Účet, který byl použit tooaccess a spravovat předplatné Azure. Často se má označuje tooas účet Azure, i když účet může být některý z těchto: stávající pracovní, školní, nebo osobní účet Microsoft, nebo Office 365 uživatelské jméno a heslo. Můžete taky vytvořit účtu toomanage předplatné Azure při registraci v hello [bezplatnou zkušební verzi](https://azure.microsoft.com).  
V tématu [zaregistrujte si předplatné Azure pomocí svého účtu Office 365](billing/billing-use-existing-office-365-account-azure-subscription.md) a [účty, které můžete používat toosign v](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="api-app"></a>Aplikace API
Jiný název pro [aplikaci služby App Service](#app-service-app).

## <a name="app-service-app"></a>Aplikační služby
Hello výpočetní prostředky, [Azure App Service](app-service/app-service-value-prop-what-is.md) poskytuje k hostování [web nebo webovou aplikaci](app-service-web/app-service-web-overview.md), [webové rozhraní API](app-service-api/app-service-api-apps-why-best-platform.md), nebo [back-end mobilní aplikace](app-service-mobile/app-service-mobile-value-prop.md). Aplikace služby App Service se také označují tooas *App Services*, *webové aplikace*, *aplikace API*, a *mobilní aplikace*.

## <a name="availability-set"></a>Sady dostupnosti.
Kolekce virtuálních počítačů, které se spravují dohromady tooprovide aplikace redundance a spolehlivosti. Hello použijte sady dostupnosti. zajistí, že při událostí do plánované i neplánované údržby bude k dispozici alespoň jeden virtuální počítač.  
V tématu [spravovat virtuální počítače s Windows hello dostupnost](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [spravovat hello dostupnosti virtuálních počítačů Linux](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="classic-model"></a>Model nasazení Azure classic
Jeden ze dvou [modely nasazení](resource-manager-deployment-model.md) používá toodeploy prostředky v Azure (Azure Resource Manager je nový model hello). Některé služby Azure podporují jenom hello modelu nasazení Resource Manager, některé podporují pouze hello modelu nasazení classic a některé podporovat. Hello dokumentace pro každou službu Azure určuje modely podporují.

## <a name="cli"></a>Rozhraní příkazového řádku Azure (CLI)
Rozhraní příkazového řádku, které můžou být použité toomanage Azure services z Windows, systému macOS a Linux.  Některé služby nebo funkce služby lze spravovat pouze pomocí prostředí PowerShell nebo hello rozhraní příkazového řádku. V tématu [rozhraní příkazového řádku Azure 2.0](/cli/azure/overview)

## <a name="powershell"></a>Prostředí Azure PowerShell
Toomanage rozhraní příkazového řádku služby Azure přes příkazový řádek z počítačů s Windows. Některé služby nebo funkce služby lze spravovat pouze pomocí prostředí PowerShell nebo hello rozhraní příkazového řádku.
V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview)

## <a name="arm-model"></a>Model nasazení Azure Resource Manager
Jeden ze dvou [modely nasazení](resource-manager-deployment-model.md) používá toodeploy prostředky v Microsoft Azure (hello jiných je hello modelu nasazení classic). Některé služby Azure podporují jenom hello modelu nasazení Resource Manager, některé podporují pouze hello modelu nasazení classic a některé podporovat. Hello dokumentace pro každou službu Azure určuje modely podporují.

## <a name="fault-domain"></a>Doména selhání
Hello kolekci virtuálních počítačů v nastavení dostupnosti, pravděpodobně se nemusí zdařit v hello stejný čas. Příkladem je skupinu počítačů v racku, které sdílejí společné přepínač zdroje a sítě power. V Azure jsou automaticky hello virtuálních počítačů v nastavení dostupnosti oddělené napříč více domén selhání.  
V tématu [spravovat virtuální počítače s Windows hello dostupnost](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [spravovat hello dostupnosti virtuálních počítačů Linux](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)  

## <a name="geo"></a>geograficky
Definované hranice pro sídle data, která obvykle obsahuje dvě nebo více oblastí. hranice Hello může být v rámci nebo za hranicemi national a jsou ovlivněny daň nařízení. Každé geografické má alespoň jedné oblasti. Příkladem oblastech jsou Asie a Tichomoří a Japonsku. Označuje taky jako *geography*.  
V tématu [oblastí Azure](best-practices-availability-paired-regions.md)

## <a name="geo-replication"></a>geografická replikace
proces Hello automaticky replikace obsah, jako jsou objekty BLOB, tabulek a v rámci pár místní.  
V tématu [aktivní geografickou replikací pro databázi Azure SQL.](sql-database/sql-database-geo-replication-overview.md)
<!-- hello meaning of "geo" in this term seems toobe different than hello meaning provided in hello "geo" entry -->

## <a name="image"></a>Bitové kopie
Soubor, který obsahuje hello operačního systému a konfigurace aplikace, které můžou být použité toocreate libovolný počet virtuálních počítačů. V Azure existují dva typy obrázků: virtuálních počítačů bitovou kopii a bitovou kopii operačního systému. Image virtuálního počítače obsahuje operační systém a všechny disky připojené tooa virtuální počítač při vytvoření bitové kopie hello. Image operačního systému obsahuje pouze zobecněný operačního systému se konfigurace disků bez dat.  
V tématu [vyhledání a vyberte bitových kopií systému Windows virtuálního počítače v Azure pomocí prostředí PowerShell nebo hello rozhraní příkazového řádku](virtual-machines/windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="limits"></a>Omezení
Hello počet prostředků, které je možné vytvořit nebo hello srovnávacího testu výkonu, který lze dosáhnout. Omezení jsou obvykle přidružené odběry, služby a nabídky.  
V tématu [předplatného Azure a omezení služby, kvóty a omezení](azure-subscription-service-limits.md)

## <a name="load-balancer"></a>Nástroj pro vyrovnávání zatížení
Prostředek, který rozděluje příchozí komunikaci mezi počítači v síti. Nástroj pro vyrovnávání zatížení v Azure, distribuuje provoz počítače toovirtual definované v sadě pro vyrovnávání zatížení. A [nástroj pro vyrovnávání zatížení](load-balancer/load-balancer-overview.md) může být internetového nebo může být vnitřní.  

## <a name="mobile-app"></a>mobilní aplikace
Jiný název pro [aplikace App Service](#app-service-app).

## <a name="offer"></a>Nabídka
Hello ceny, kredity a příslušné související podmínky tooan předplatného Azure.  
V tématu hello [stránce s podrobnostmi o nabídka Azure](https://azure.microsoft.com/support/legal/offer-details/)

## <a name="portal"></a>portál
Hello zabezpečený webový portál používá toodeploy a spravovat služby Azure.  Existují dva portály: hello [portál Azure](http://portal.azure.com/) a hello [portálu classic](http://manage.windowsazure.com/). Některé služby jsou k dispozici v obou portálů, zatímco jiné jsou dostupné jenom v jedné nebo jiné hello. Hello [Azure portálu dostupnosti grafu](https://azure.microsoft.com/features/azure-portal/availability/) seznamy služby, které jsou k dispozici v které portálu.

## <a name="region"></a>Oblast
Oblasti v rámci geograficky, která nemá křížové national ohraničení a obsahuje jeden nebo více datových centrech. Ceny, místní služby a nabídka typy jsou zveřejněné na úrovni oblasti hello. Oblast je obvykle spárován s jinou oblast, což může být až tooseveral set miles rychle. Místní páry slouží jako mechanismus pro zotavení po havárii a scénáře s vysokou dostupností. Také označuje tooas *umístění*.  
V tématu [oblastí Azure](best-practices-availability-paired-regions.md)

## <a name="resource"></a>Prostředek
Položka, která je součástí daného řešení Azure. Každá služba Azure umožňuje toodeploy různých typů prostředků, jako jsou databáze nebo virtuální počítače.   
V tématu [přehled Azure Resource Manageru](azure-resource-manager/resource-group-overview.md)

## <a name="resource-group"></a>skupina prostředků
Kontejner ve Správci prostředků, který obsahuje související prostředky pro aplikaci. Skupina prostředků Hello může zahrnovat všechny hello prostředky pro aplikaci nebo jenom prostředky, které jsou logicky seskupeny dohromady. Můžete rozhodnout, jak chcete prostředky tooallocate tooresource skupin podle díky hello nejvhodnější pro vaši organizaci.  
V tématu [přehled Azure Resource Manageru](azure-resource-manager/resource-group-overview.md)

## <a name="arm-template"></a>Šablony Resource Manageru
Soubor JSON deklarativně definující jeden nebo více prostředků Azure, který definuje závislosti mezi hello nasazené prostředky. Hello šablony lze použít toodeploy hello prostředky konzistentně a opakovaně.  
V tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md)

## <a name="resource-provider"></a>Poskytovatel prostředků
Služba poskytující prostředky hello můžete nasadit a spravovat prostřednictvím Resource Manageru. Každý poskytovatel prostředků nabízí operace pro práci s hello prostředky, které jsou nasazeny. Zprostředkovatelé prostředků je přístupná prostřednictvím hello portál Azure, Azure PowerShell a několik programovací sady SDK.  
V tématu [přehled Azure Resource Manageru](azure-resource-manager/resource-group-overview.md)

## <a name="role"></a>Role
Způsob pro řízení přístupu, kterou lze přiřadit toousers, skupinám a službám. Role jsou možné tooperform akce, jako je vytvořit, spravovat a přečtěte si prostředků Azure.  
V tématu [RBAC: předdefinovaných rolí](active-directory/role-based-access-built-in-roles.md)

## <a name="sla"></a>Dohoda o úrovni služeb (SLA)
smlouva Hello, který popisuje závazky společnosti Microsoft pro provozu a připojení. Každá služba Azure má konkrétní SLA.  
V tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/)

## <a name="sas"></a>sdílený přístupový podpis (SAS)
Podpis, která vám umožní toogrant omezený přístup tooa prostředků, bez vystavení klíč účtu. Například [Azure Storage používá SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) toogrant klienta přístup tooobjects například objekty BLOB. [IoT Hub používá SAS](iot-hub/iot-hub-devguide-security.md#security-tokens) toogrant zařízení oprávnění toosend telemetrie.

## <a name="storage-account"></a>Účet úložiště
Účet, který poskytuje přístup k toohello Azure Blob, fronty, tabulky a soubor služby ve službě Azure Storage. název účtu úložiště Hello definuje hello jedinečný obor názvů pro datové objekty Azure Storage.  
V tématu [účty Azure storage](storage/common/storage-create-storage-account.md)

## <a name="subscription"></a>předplatné
Smlouvy zákazníka se společností Microsoft, který jim umožňuje tooobtain Azure services. Nabídka hello zvolené pro předplatné hello se řídí Hello předplatné ceny a související podmínky.
V tématu [Microsoft Online Subscription Agreement](https://azure.microsoft.com/support/legal/subscription-agreement/) a [asociování předplatných Azure se službou Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md)

## <a name="tag"></a>Značka
Indexování pojem, který vám umožní toocategorize prostředky podle požadavků tooyour pro správu nebo fakturaci. Když máte komplexní kolekci prostředků, můžete hello takovým způsobem, který umožňuje hello nejvhodnější použít značky toovisualize tyto prostředky. Můžete například označit prostředky, které ve vaší organizaci poskytovat podobnou roli nebo patří toohello stejného oddělení.  
V tématu [pomocí značky tooorganize vašich prostředků Azure](resource-group-using-tags.md)

## <a name="update-domain"></a>Aktualizace domény
Hello kolekci virtuálních počítačů v nastavení dostupnosti, které jsou aktualizovány v hello stejnou dobu. Virtuální počítače v hello jedné aktualizační doméně se restartují společně během plánované údržby. Azure nikdy nerestartuje víc než jednu aktualizační doménu najednou. Také označuje tooas upgradovací doméně.  
V tématu [spravovat virtuální počítače s Windows hello dostupnost](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [spravovat hello dostupnosti virtuálních počítačů Linux](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vm"></a>virtuální počítač
implementace softwaru Hello fyzický počítač, který běží operační systém. Více virtuálních počítačů lze spustit souběžně na stejném hardwaru hello. Virtuální počítače v Azure, jsou k dispozici v mnoha velikostí.  
V tématu [dokumentace virtuální počítače](https://azure.microsoft.com/documentation/services/virtual-machines/)

## <a name="vm-extension"></a>rozšíření virtuálního počítače.
Prostředek, který implementuje chování nebo funkce, které buď další programy práci nebo zadejte možnost hello toointeract s počítačem, spuštěná. Můžete například použít tooreset rozšíření virtuálních počítačů přístup hello nebo upravte hodnoty vzdáleného přístupu na virtuálním počítači Azure.
<!-- This definition seems obscure toome; maybe a list of examples would work better than a conceptual definition? -->
V tématu [o rozšíření virtuálního počítače a funkcích (Windows)](virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [o rozšíření virtuálního počítače a funkcích (Linux)](virtual-machines/linux/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vnet"></a>virtuální síť
Síť, která poskytuje připojení mezi vašich prostředků Azure, které je izolovaný od všech ostatních klientů Azure. [Azure VPN Gateway](vpn-gateway/vpn-gateway-about-vpngateways.md) umožňuje vytvořit připojení mezi virtuálními sítěmi a [mezi virtuální sítí a místní sítí](vpn-gateway/vpn-gateway-plan-design.md). Můžete plně řídit hello bloky IP adres, nastavení DNS, zásady zabezpečení a směrovací tabulky v rámci virtuální sítě.  
V tématu [Přehled virtuálních sítí](virtual-network/virtual-networks-overview.md)  

## <a name="web-app"></a>Webová aplikace
Jiný název pro [aplikace App Service](#app-service-app).

## <a name="see-also"></a>Viz také

* [Začínáme s Azure](https://azure.microsoft.com/get-started/)
* [Center prostředků cloudu](https://azure.microsoft.com/resources/)  
* [Azure pro obchodní aplikace](https://azure.microsoft.com/overview/business-apps-on-azure/)
* [Azure ve vašem datovém centru](https://azure.microsoft.com/overview/business-apps-on-azure/)

