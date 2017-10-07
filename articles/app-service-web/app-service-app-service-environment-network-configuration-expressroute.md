---
title: "aaaNetwork podrobnosti o konfiguraci pro práci s Express Route"
description: "Podrobnosti konfigurace sítě pro spuštění prostředí App Service ve virtuální sítě připojen tooan okruh ExpressRoute."
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 34b49178-2595-4d32-9b41-110c96dde6bf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2016
ms.author: stefsch
ms.openlocfilehash: 85bbc45cfed619485957ee2a898aa0a7604173cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Podrobnosti o konfiguraci sítě pro službu App Service Environment používající ExpressRoute
## <a name="overview"></a>Přehled
Zákazníci mohou připojit [Azure ExpressRoute] [ ExpressRoute] okruhu tootheir virtuální síťové infrastruktury, tak rozšířit jejich místní sítě tooAzure.  Služby App Service Environment lze vytvořit v této podsíti [virtuální sítě] [ virtualnetwork] infrastruktury.  Aplikace běžící na hello App Service Environment pak může navázat zabezpečené připojení tooback-end prostředky dostupné jenom přes hello připojení ExpressRoute.  

Služby App Service Environment se dají vytvářet v **buď** virtuální síť Azure Resource Manager **nebo** virtuální síť modelu nasazení classic.  Nedávné změny provedené v června 2016, ASEs také teď můžou být nasazené do virtuální sítě, které používají rozsahů veřejných adres nebo RFC1918 adresní prostory (tj. privátní adresy). 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Vyžaduje síťové připojení
Existují požadavky na síťové připojení pro prostředí App Service, která nemusí být splněny původně v tooan virtuální síť připojení ExpressRoute.  Služby App Service Environment vyžadovat všechny následující hello v pořadí toofunction správně:

* Odchozí připojení tooAzure úložiště koncových bodů sítě po celém světě na oba porty 80 a 443.  To zahrnuje koncové body, které jsou umístěné v hello stejné oblasti jako hello App Service Environment, stejně jako koncové body úložiště, které jsou umístěné v **jiných** oblastech Azure.  Koncové body Azure úložiště vyřešit v rámci hello následující domény DNS: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* a *file.core.windows.net*.  
* Odchozí síťové připojení toohello službu Azure souborů na port 445.
* Odchozí síťové připojení tooSql DB koncové body umístěné v hello stejné oblasti jako hello App Service Environment.  Koncové body SQL DB vyřešit v rámci hello následující domény: *database.windows.net*.  To vyžaduje otevření přístup tooports 1433, 11000 11999 a 14000 14999.  Podrobnosti najdete v části [Tento článek na použití portu Sql Database verze 12](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
* Odchozí připojení toohello správu Azure roviny koncových bodů sítě (ASM i ARM koncových bodů).  To zahrnuje odchozí připojení tooboth *management.core.windows.net* a *management.azure.com*. 
* Odchozí připojení k síti příliš*ocsp.msocsp.com*, *mscrl.microsoft.com* a *crl.microsoft.com*.  Toto je funkce potřebné toosupport SSL.
* Hello konfiguraci DNS pro virtuální síť hello musí být schopen překladu hello koncové body a domény uvedený v hello starší body.  Pokud tyto koncové body nelze přeložit, se nezdaří pokusy o vytvoření služby App Service Environment a budou označeny stávající prostředí App Service není v pořádku.
* Je nutný pro komunikaci se servery DNS odchozí přístup na port 53.
* Pokud existuje vlastního serveru DNS na hello druhém konci brána sítě VPN, musí být dostupný z podsítě hello obsahující hello App Service Environment hello DNS server. 
* Hello odchozí síťovou cestu nelze procházet i podnikové interní proxy, ani může být platnost tunelovým propojením tooon místní.  V tom změny hello efektivní adres NAT odchozího provozu z hello App Service Environment.  Změna adres NAT hello odchozího provozu služby App Service Environment způsobí, že připojení, které toomany selhání koncových bodů hello uvedené výše.  Výsledkem neúspěšných pokusech o vytvoření služby App Service Environment, stejně jako dříve pořádku prostředí App Service bude označena jako není v pořádku.  
* Příchozí síťové porty toorequired přístupu pro prostředí App Service musí být povoleno, jak je popsáno v tomto [článku][requiredports].

požadavky na DNS Hello můžete splnit zajištěním platný infrastruktura DNS je nakonfigurován a udržovat hello virtuální sítě.  Pokud pro všechny důvod hello dojde ke změně konfigurace DNS po vytvoření služby App Service Environment, vývojáři vynutit App Service Environment toopick až hello novou konfiguraci DNS.  Aktivuje restartu postupného prostředí pomocí hello "Restartovat" ikonu hello horní části okna Správa služby App Service Environment hello v hello [portál Azure] [ NewPortal] způsobí, že toopick prostředí hello až hello novou konfiguraci DNS.

Hello příchozí síťový přístup požadavky lze splnit konfigurací [skupinu zabezpečení sítě] [ NetworkSecurityGroups] na hello App Service Environment pro podsíť tooallow hello vyžaduje přístup jak je popsáno v této [článku][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Povolení odchozího síťového připojení pro služby App Service Environment
Ve výchozím nastavení nově vytvořený okruh ExpressRoute inzeruje výchozí trasu, která umožňuje odchozí připojení k Internetu.  V této konfiguraci se na službu App Service Environment bude možné tooconnect tooother koncové body Azure.

Ale obvyklé konfigurace zákazníka je toodefine vlastní výchozí trasa (0.0.0.0/0), který vynutí odchozí internetové přenosy tooinstead toku místní.  Tento tok provozu vždy dělí prostředí App Service, protože odchozí přenosy hello je blokované místně nebo NAT'd tooan nerozpoznatelný sadu adresy, které přestane fungovat v různých koncové body Azure.

řešení Hello je toodefine jeden (nebo více) trasy definované uživatelem (udr) v podsíti hello, který obsahuje hello App Service Environment.  UDR definuje tras konkrétní podsítě, které bude dodržení místo hello výchozí trasu.

Pokud je to možné doporučujeme toouse hello následující konfigurace:

* Konfigurace ExpressRoute Hello inzeruje 0.0.0.0/0 a ve výchozím nastavení vynucené tunelových propojení všechny odchozí přenosy na místě.
* Hello UDR použít toohello podsíť obsahující hello App Service Environment definuje 0.0.0.0/0 s další typ směrování Internet (příkladem je dále dolů v tomto článku).

Hello celkové požadavky tyto kroky je úrovni podsítě hello UDR má přednost před hello ExpressRoute vynuceného tunelování, čímž zajišťuje odchozí přístup k Internetu z hello App Service Environment.

> [!IMPORTANT]
> Hello trasy definované v UDR **musí** být dost specifický na příliš zděděným přes všechny trasy inzerované podle hello konfigurace ExpressRoute.  Následující příklad Hello používá rozsah adres široký 0.0.0.0/0 hello a jako takový se dá potenciálně omylem přepsat inzerování tras pomocí podrobnější rozsahy adres.
> 
> Prostředí App Service nejsou podporovány s konfigurací ExpressRoute, **mezi Inzerovat trasy z hello cesty veřejného partnerského vztahu cesta toohello soukromého partnerského vztahu**.  Konfigurace ExpressRoute, které mají veřejné partnerské vztahy nakonfigurované, obdrží inzerování trasy od společnosti Microsoft pro velké sady rozsahů adres Microsoft Azure IP.  Pokud tyto rozsahy adres ohlášené mezi na hello cestou soukromého partnerského vztahu, hello konečným výsledkem je, že všechny odchozí síťových paketů z podsítě hello App Service Environment je bude tunelovým propojením force tooa zákazníka místní síťové infrastruktury.  Tento tok sítě se aktuálně nepodporuje s prostředí App Service.  Jedním z problémů toothis řešení je směrování mezi – reklamu toostop z hello cesty veřejného partnerského vztahu cesta toohello soukromého partnerského vztahu.
> 
> 

Základní informace o trasy definované uživatelem je k dispozici v tomto [přehled][UDROverview].  

Informace o vytváření a konfigurace trasy definované uživatelem je k dispozici v tomto [jak tooGuide][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Ukázková konfigurace UDR služby App Service Environment
**Předpoklady**

1. Nainstalovat Azure Powershell z hello [Azure stáhne stránky] [ AzureDownloads] (s datem červen 2015 nebo novější).  V části "Nástroje příkazového řádku" existuje propojení "Instalace" v části "Windows Powershell", který bude instalovat nejnovější rutiny prostředí Powershell hello.
2. Doporučuje se, že jedinečnou podsíť je vytvořený pro výhradní použití služby App Service Environment.  Díky této podsíti toohello udr použít hello otevřou jenom odchozí přenosy pro hello App Service Environment.
3. **Důležité**: nenasazujte hello App Service Environment, dokud **po** dodržíte hello následující kroky konfigurace.  To zajistí, že odchozí síťové připojení je k dispozici teprve toodeploy služby App Service Environment.

**Krok 1: Vytvoření tabulky pojmenovanou trasu**

Hello následující fragment kódu vytvoří směrovací tabulku názvem "DirectInternetRouteTable" v oblasti Azure USA – západ hello:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Krok 2: Vytvoření jednu nebo víc tras ve směrovací tabulce hello**

Budete potřebovat tooadd jeden nebo více tras toohello směrovací tabulku v pořadí tooenable odchozí přístup k Internetu.  

Hello doporučenému přístupu pro konfiguraci odchozí přístup toohello Internetu je toodefine trasu pro 0.0.0.0/0 jako vidíte níže.

    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Mějte na paměti, že 0.0.0.0/0 je adresa široký rozsah a jako takový bude přepsáno podrobnější rozsahy adres inzerovaných hello ExpressRoute.  toore-iteraci hello starší doporučení, UDR s trasou 0.0.0.0/0 slouží ve spojení s konfigurací ExressRoute, pouze inzeruje také 0.0.0.0/0. 

Jako alternativu můžete si stáhnout seznam rozsahy CIDR používá Azure komplexní a aktualizované.  Hello Xml soubor obsahující všechny rozsahy hello Azure IP adres je dostupný z hello [Microsoft Download Center][DownloadCenterAddressRanges].  

Všimněte si, ale že časem změnit tyto rozsahy proto očekávání pravidelné ruční aktualizace toohello uživatelem definované trasy tookeep synchronizované.  Navíc vzhledem k tomu, že je výchozí horní limit 100 tras v jednom UDR, budete potřebovat příliš "představuje souhrn" hello Azure IP adresu rozsahy toofit v rámci omezení trasy hello 100, udržování Pamatujte, že UDR zadaných tras nutné toobe podrobnější než hello trasy inzerované ve vaší ExpressRoute.  

**Krok 3: Přidružení hello trasy tabulky toohello podsíť obsahující hello App Service Environment**

poslední krok konfigurace Hello je tooassociate hello trasy tabulky toohello podsíť, kde bude nasazen hello App Service Environment.  Hello následující příkaz přidruží toohello "DirectInternetRouteTable" hello "ASESubnet", která bude obsahovat nakonec služby App Service Environment.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Krok 4: Závěrečné kroky**

Jakmile hello směrovací tabulka je vázané toohello podsíť, se doporučuje toofirst testovací a potvrďte, že hello určený vliv.  Například nasazení virtuálního počítače do hello podsítě a ověřte, že:

* Koncové body mimo Azure a odchozí přenosy tooboth Azure uvedených výše v tomto článku je **není** toku dolů hello okruh ExpressRoute.  Je velmi důležité tooverify, které toto chování, vzhledem k tomu, že pokud je odchozí provoz z podsítě hello stále vynucené tunelovým propojením místní, vytváření služby App Service Environment bude vždy nezdaří. 
* Vyhledávání DNS pro koncové body hello již bylo zmíněno dříve všechny řešíme správně. 

Po potvrzení hello výše kroky jsou toodelete hello virtuálního počítače je nutné, protože hello podsíť musí toobe "prázdný" v hello hello čas, je vytvořena App Service Environment.

Potom pokračujte ve vytváření služby App Service Environment!

## <a name="getting-started"></a>Začínáme
Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

tooget začít s prostředí App Service najdete v části [Úvod tooApp Service Environment][IntroToAppServiceEnvironment]

Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com


<!-- IMAGES -->
