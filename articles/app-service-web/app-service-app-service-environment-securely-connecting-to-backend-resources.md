---
title: "aaaSecurely připojení tooBackEnd prostředky ze služby App Service Environment"
description: "Informace o tom, jak toosecurely připojení toobackend prostředků ze služby App Service Environment."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 6311d3fc301512ea3c4ed8f14f268f75755aa415
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securely-connecting-toobackend-resources-from-an-app-service-environment"></a>Bezpečně připojení tooBackend prostředky ze služby App Service Environment
## <a name="overview"></a>Přehled
Vzhledem k tomu, že služby App Service Environment se vždy vytvoří v **buď** virtuální síť Azure Resource Manager **nebo** modelu nasazení classic [virtuální sítě] [ virtualnetwork], odchozí připojení ze App Service Environment tooother back-endové prostředky můžete toku výhradně prostřednictvím hello virtuální sítě.  Nedávné změny provedené v června 2016 ASEs také lze nasadit do virtuálních sítí, které používají rozsahů veřejných adres nebo RFC1918 adresní prostory (tj. privátní adresy).  

Například může být SQL Server běžící na clusteru s podporou virtuálních počítačů s port 1433 uzamčené.  koncový bod Hello může být ACLd tooonly povolit přístup z jiných zdrojů na hello stejné virtuální síti.  

Další příklad citlivé koncových bodů může spustit místně a může být připojené tooAzure prostřednictvím buď [Site-to-Site] [ SiteToSite] nebo [Azure ExpressRoute] [ ExpressRoute] připojení.  V důsledku toho pouze prostředky ve virtuální sítě připojen toohello Site-to-Site nebo ExpressRoute tunely bude možné tooaccess místní koncové body.

Pro všechny z těchto scénářů aplikace běžící na služby App Service Environment být schopný toosecurely připojí toohello různé servery a prostředky.  Odchozí provoz z aplikace běžící v koncových bodů App Service Environment tooprivate v hello stejné virtuální síti (nebo připojený toohello stejné virtuální síti), bude pouze toku hello virtuální síti.  Tooprivate odchozí přenosy, které koncové body nebude toku přes hello veřejného Internetu.

Jeden přímý přístup paměti platí toooutbound provoz ze App Service Environment tooendpoints v rámci virtuální sítě.  Prostředí App Service se nemůže připojit koncové body virtuálních počítačů, které jsou umístěné v hello **stejné** podsítě jako hello App Service Environment.  Za normálních okolností nemělo by se jednat problém, dokud prostředí App Service jsou nasazeny do podsítě vyhrazené pro výhradní použití pouze hello App Service Environment.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Odchozí připojení a požadavky na DNS
Pro App Service Environment toofunction správně, se vyžaduje odchozí přístup toovarious koncových bodů. Úplný seznam hello externí koncové body používané App Service Environment je v části "Vyžaduje připojení k síti" hello hello [konfiguraci sítě pro ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) článku.

Prostředí služby App Service vyžaduje platnou konfiguraci pro virtuální síť hello infrastruktury služby DNS.  Pokud pro všechny důvod hello dojde ke změně konfigurace DNS po vytvoření služby App Service Environment, vývojáři vynutit App Service Environment toopick až hello novou konfiguraci DNS.  Aktivuje restartu postupného prostředí pomocí ikony "Restartovat" hello umístěný na začátku hello hello App Service Environment okna Správa portálu hello způsobí hello prostředí toopick až hello novou konfiguraci DNS.

Dále je doporučeno, aby žádné vlastní servery DNS v hello virtuální síti vnet se instalační program před čas předchozí toocreating služby App Service Environment.  Pokud je při vytváření služby App Service Environment se změnila konfigurace DNS virtuální sítě, které způsobí selhání procesu vytvoření služby App Service Environment hello.  V souvislosti podobnou Pokud vlastní server DNS existuje na hello druhém konci VPN gateway a hello DNS server je nedostupná nebo není k dispozici, hello selžou i proces vytvoření služby App Service Environment.

## <a name="connecting-tooa-sql-server"></a>Připojení tooa systému SQL Server
Obvyklé konfigurace systému SQL Server má koncový bod naslouchání na portu 1433:

![Koncový bod serveru SQL][SqlServerEndpoint]

Existují dva přístupy pro omezení provozu toothis koncový bod:

* [Sítě seznamy řízení přístupu] [ NetworkAccessControlLists] (sítě ACL)
* [Skupiny zabezpečení sítě][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>Omezení přístupu k síti seznamu ACL
Port 1433 lze zabezpečit pomocí seznamu řízení přístupu síti.  Příklad Hello níže povolených programů klienta adres pocházející z uvnitř virtuální sítě a blokuje přístup k tooall ostatní klienty.

![Příklad seznamu řízení přístupu sítě][NetworkAccessControlListExample]

Všechny aplikace spuštěné v App Service Environment v hello stejné virtuální síti jako hello systému SQL Server bude instance systému SQL Server nemůže tooconnect toohello pomocí hello **interní virtuální síť** IP adresu pro virtuální počítač systému SQL Server hello.  

Hello příklad připojovacího řetězce následující odkazy hello systému SQL Server pomocí jeho privátní IP adresy.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

I když hello virtuální počítač má také veřejný koncový bod, pokusy o připojení pomocí hello veřejnou IP adresu se odmítne kvůli hello sítě ACL. 

## <a name="restricting-access-with-a-network-security-group"></a>Omezení přístupu ke skupině zabezpečení sítě
Alternativní způsob pro přístup k zabezpečení je skupina zabezpečení sítě.  Skupiny zabezpečení sítě může být použité tooindividual virtuálních počítačů nebo tooa podsíť obsahující virtuální počítače.

První skupinu zabezpečení sítě potřebuje toobe vytvořit:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Omezení přístupu tooonly interní provoz VNet je velmi jednoduchý ke skupině zabezpečení sítě.  výchozí pravidla Hello ve skupině zabezpečení sítě pouze povolit přístup z jiných klientů sítě v hello stejné virtuální síti.

V důsledku zamykání přístup tooSQL Server je stejně jednoduché jako použití skupinu zabezpečení sítě s jeho výchozí pravidla tooeither hello virtuální počítače s SQL serverem nebo hello podsíť obsahující hello virtuálních počítačů.

Následující ukázka Hello platí zabezpečení skupiny toohello obsahující podsíť sítě:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

Konečný výsledek Hello je sada pravidel zabezpečení, která blokují externí přístup při povolení přístupu interní virtuální sítě:

![Výchozí pravidla zabezpečení sítě][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Začínáme
Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

tooget začít s prostředí App Service najdete v části [Úvod tooApp Service Environment][IntroToAppServiceEnvironment]

Podrobnosti ohledně ovládání tooyour příchozí komunikaci služby App Service Environment najdete v tématu [řízení tooan příchozí komunikaci služby App Service Environment][ControlInboundASE]

Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
