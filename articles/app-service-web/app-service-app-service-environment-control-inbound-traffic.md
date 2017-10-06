---
title: "aaaHow tooControl příchozí provoz tooan App Service Environment"
description: "Informace o tom, jak toocontrol pravidla zabezpečení sítě tooconfigure příchozí provoz tooan App Service Environment."
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: 
ms.assetid: 4cc82439-8791-48a4-9485-de6d8e1d1a08
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: stefsch
ms.openlocfilehash: e7c6e6201db6a1ea77f7a2eee29a3b5445175495
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocontrol-inbound-traffic-tooan-app-service-environment"></a>Jak tooControl příchozí provoz tooan App Service Environment
## <a name="overview"></a>Přehled
Služby App Service Environment se dají vytvářet v **buď** virtuální síť Azure Resource Manager **nebo** modelu nasazení classic [virtuální sítě][virtualnetwork].  V době hello vytváření služby App Service Environment lze definovat nové virtuální sítě a novou podsíť.  Služby App Service Environment Alternativně lze vytvořit v existující virtuální síť a existující podsíť.  S změny provedené v červnu 2016 ASEs také lze nasadit do virtuálních sítí, které používají rozsahů veřejných adres nebo RFC1918 adresní prostory (tj. privátní adresy).  Podrobnosti o vytvoření služby App Service Environment najdete v části [jak tooCreate služby App Service Environment][HowToCreateAnAppServiceEnvironment].

Služby App Service Environment musí vždy vytvořit v podsíti, protože podsíť poskytuje hranici sítě, který lze použít toolock dolů příchozí provoz za nadřazeného zařízení a služeb tak, aby přenosů dat HTTP a HTTPS je pouze přijímán z konkrétní nadřazeného IP adresy.

Příchozí a odchozí síťový provoz na podsíť je řízena pomocí [skupinu zabezpečení sítě][NetworkSecurityGroups]. Řízení příchozí provoz vyžaduje vytvoření pravidla zabezpečení sítě ve skupině zabezpečení sítě a pak přiřazení hello sítě zabezpečení skupiny hello podsíť obsahující hello App Service Environment.

Jakmile skupinu zabezpečení sítě je přidělen tooa podsíť, tooapps příchozí provoz v App Service Environment není povolené nebo blokované podle hello hello povolit a zakázat pravidla definovaná v hello skupinu zabezpečení sítě.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="inbound-network-ports-used-in-an-app-service-environment"></a>Příchozí síťové porty používané v prostředí služby App Service
Před zamykání příchozí síťový provoz s skupinu zabezpečení sítě, je důležité tooknow hello sadu požadované a volitelné síťové porty používané služby App Service Environment.  Omylem zavřením vypnout porty toosome provozu může dojít ke ztrátě funkcí ve službě App Service Environment.

Hello následuje seznam portů používaných služby App Service Environment. Všechny porty jsou **TCP**, pokud není uvedeno jinak jasně:

* 454: **požadované port** používá infrastrukturu Azure pro správu a údržbu prostředí App Service přes SSL.  Provoz toothis portu nejsou blokovány.  Tento port je vždy vázané toohello veřejných virtuálních IP adres služby App Service Environment.
* 455: **požadované port** používá infrastrukturu Azure pro správu a údržbu prostředí App Service přes SSL.  Provoz toothis portu nejsou blokovány.  Tento port je vždy vázané toohello veřejných virtuálních IP adres služby App Service Environment.
* 80: výchozí port pro příchozí provoz tooapps HTTP spuštěné v plány služby App Service ve službě App Service Environment.  Na povoleno ILB App Service Environment tento port je vázané toohello ILB adresa hello App Service Environment.
* 443: výchozí port pro příchozí provoz tooapps SSL spuštěné v plány služby App Service ve službě App Service Environment.  Na povoleno ILB App Service Environment tento port je vázané toohello ILB adresa hello App Service Environment.
* 21: řídicí kanál pro službu FTP.  Tento port můžete bezpečně zablokuje, pokud není používán FTP.  Na povoleno ILB App Service Environment může být tento port vázané toohello ILB adresu pro App Service Environment.
* 990: řídicí kanál pro FTPS.  Tento port můžete bezpečně zablokuje, pokud není používán FTPS.  Na povoleno ILB App Service Environment může být tento port vázané toohello ILB adresu pro App Service Environment.
* 10001-10020: datové kanály pro službu FTP.  Jako s hello řídicí kanál, tyto porty lze bezpečně blokovat Pokud není používán FTP.  Na povoleno ILB App Service Environment může být tento port vázané toohello App Service Environment pro ILB adresu.
* 4016: používá pro vzdálené ladění pomocí sady Visual Studio 2012.  Tento port můžete bezpečně zablokuje, pokud není používán hello funkce.  Na povoleno ILB App Service Environment tento port je vázané toohello ILB adresa hello App Service Environment.
* 4018: používá pro vzdálené ladění pomocí sady Visual Studio 2013.  Tento port můžete bezpečně zablokuje, pokud není používán hello funkce.  Na povoleno ILB App Service Environment tento port je vázané toohello ILB adresa hello App Service Environment.
* 4020: používá pro vzdálené ladění pomocí sady Visual Studio 2015.  Tento port můžete bezpečně zablokuje, pokud není používán hello funkce.  Na povoleno ILB App Service Environment tento port je vázané toohello ILB adresa hello App Service Environment.

## <a name="outbound-connectivity-and-dns-requirements"></a>Odchozí připojení a požadavky na DNS
Pro App Service Environment toofunction správně, je také vyžaduje odchozí přístup toovarious koncových bodů. Úplný seznam hello externí koncové body používané App Service Environment je v části "Vyžaduje připojení k síti" hello hello [konfiguraci sítě pro ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) článku.

Prostředí služby App Service vyžaduje platnou konfiguraci pro virtuální síť hello infrastruktury služby DNS.  Pokud pro všechny důvod hello dojde ke změně konfigurace DNS po vytvoření služby App Service Environment, vývojáři vynutit App Service Environment toopick až hello novou konfiguraci DNS.  Aktivuje restartu postupného prostředí pomocí hello "Restartovat" ikonu hello horní části okna Správa služby App Service Environment hello v hello [portál Azure] [ NewPortal] způsobí, že toopick prostředí hello až hello novou konfiguraci DNS.

Dále je doporučeno, aby žádné vlastní servery DNS v hello virtuální síti vnet se instalační program před čas předchozí toocreating služby App Service Environment.  Pokud je při vytváření služby App Service Environment se změnila konfigurace DNS virtuální sítě, které způsobí selhání procesu vytvoření služby App Service Environment hello.  V souvislosti podobnou Pokud vlastní server DNS existuje na hello druhém konci VPN gateway a hello DNS server je nedostupná nebo není k dispozici, hello selžou i proces vytvoření služby App Service Environment.

## <a name="creating-a-network-security-group"></a>Vytvoření skupiny zabezpečení sítě
Úplné podrobnosti o tom, jak fungují skupiny zabezpečení sítě najdete v tématu hello následující [informace][NetworkSecurityGroups].  označuje níže úpravy v příkladu Azure Service Management Hello skupin zabezpečení sítě, se zaměřením na konfiguraci a použití podsíť tooa skupiny zabezpečení sítě, který obsahuje služby App Service Environment.

**Poznámka:** skupin zabezpečení sítě pomocí se dají konfigurovat graficky hello [portálu Azure](https://portal.azure.com) nebo pomocí prostředí Azure PowerShell.

Skupiny zabezpečení sítě jsou nejprve vytvořit jako samostatné entity přidružené předplatnému. Vzhledem k tomu, že skupiny zabezpečení sítě je vytvořeno v oblasti Azure, zkontrolujte tuto skupinu zabezpečení sítě hello se vytvoří v hello stejné oblasti jako hello App Service Environment.

Následující Hello ukazuje vytvoření skupiny zabezpečení sítě:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Po vytvoření skupiny zabezpečení sítě je jeden nebo více pravidel zabezpečení sítě se přidají tooit.  Vzhledem k tomu, že hello sadu pravidel v průběhu času mění, doporučujeme toospace out hello schématu číslování se použijí pro pravidlo priority toomake ho snadno tooinsert další pravidla v čase.

Následující příklad Hello ukazuje pravidlo, které explicitně udělí přístup toohello správu porty vyžadované toomanage hello infrastrukturu Azure a spravovat služby App Service Environment.  Všimněte si, že veškerý provoz správy probíhá přes protokol SSL a je zabezpečená službou klientské certifikáty, takže i když jsou otevřené porty hello budou nepřístupné všechny entita než infrastruktury pro správu Azure.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP


Při zamykání přístup tooport 80 a 443 příliš "schovat" služby App Service Environment za nadřazeného zařízení nebo služby, budete potřebovat tooknow hello nadřazeného IP adresu.  Například pokud používáte brány firewall webových aplikací (firewall webových aplikací), hello firewall webových aplikací bude mít svůj vlastní IP adresu (nebo adresy), které používá při připojení přes proxy server provoz tooa podřízené App Service Environment.  Budete potřebovat toouse tuto IP adresu v hello *SourceAddressPrefix* parametr pravidla zabezpečení sítě.

V příkladu hello níže příchozí provoz z konkrétní IP adresu nadřazeného výslovně povolené.  Hello adresu *1.2.3.4* slouží jako zástupný symbol pro IP adresu hello nadřazenému firewall webových aplikací.  Změnit hello hodnota toomatch hello adresa použitá nadřazeného zařízení nebo službou.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

V případě potřeby podpora FTP hello následující pravidla slouží jako šablona toogrant přístup toohello FTP řízení port a portů datového kanálu.  Vzhledem k tomu, že FTP je stavová protokol, nemusí být možné tooroute FTP přenosy přes tradiční zařízení brány firewall nebo proxy protokolu HTTP nebo HTTPS.  V takovém případě budete potřebovat tooset hello *SourceAddressPrefix* rozsahu vývojáře nebo nasazení počítačů, na které FTP klientských počítačích jinou hodnotu tooa – například hello IP adres. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Poznámka:** rozsah portů datového kanálu hello může změnit během období preview hello.)

Pokud se používá vzdálené ladění pomocí sady Visual Studio, hello následující pravidla ukazují, jak přistupovat k toogrant.  Vzhledem k tomu, že každou verzi používá jiný port pro vzdálené ladění je samostatné pravidlo pro všechny podporované verze sady Visual Studio.  Stejně jako u přístup FTP, nemusí vzdálené ladění provoz procházet skrz tradiční firewall webových aplikací nebo zařízení proxy správně.  Hello *SourceAddressPrefix* můžete místo toho nastavit rozsah IP adres toohello počítačů vývojáře Visual Studio.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-tooa-subnet"></a>Přiřazení skupiny zabezpečení sítě tooa podsítě
Skupina zabezpečení sítě má výchozí pravidlo zabezpečení, čímž se odmítne přístup tooall externích přenosů.  Hello výsledek z kombinace pravidla zabezpečení sítě hello popsané výše a hello výchozí pravidlo zabezpečení blokuje příchozí přenos, je, že pouze provoz z rozsahů adres zdroje přidružené *povolit* akce bude mít tooapps provoz toosend spuštěná ve službě App Service Environment.

Po naplnění skupinu zabezpečení sítě pomocí pravidel zabezpečení musí toobe přiřazené podsítě toohello obsahující hello App Service Environment.  příkaz přiřazení Hello odkazuje na název obou hello hello virtuální sítě, které se hello App Service Environment nachází, a také název hello hello podsítě, které bylo vytvořeno hello App Service Environment.  

Následující příklad Hello ukazuje skupinu zabezpečení sítě, který je právě přiřazován tooa podsíť a virtuální sítě:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Po úspěšném přiřazení skupiny zabezpečení sítě hello (přiřazení hello je dlouhotrvající operace a může trvat několik minut toocomplete), pouze příchozí provoz odpovídající *povolit* pravidla se úspěšně připojit aplikace hello aplikace Prostředí služby.

Pro úplnost hello následující příklad ukazuje, jak tooremove a proto DIS přidružení zabezpečení sítě hello skupinu z podsítě hello:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Zvláštní upozornění pro explicitní IP SSL
Pokud aplikace je konfigurován s adresou IP SSL explicitní (použít *pouze* tooASEs, které mají veřejné VIP), místo použití hello výchozí IP adresu hello App Service Environment, HTTP a HTTPS provozu, pokračovat v podsíti hello v rámci jiné sady jiné porty než porty 80 a 443.

jednotlivé dvojice Hello porty používané každou adresu IP SSL naleznete v hello portálu uživatelské rozhraní v okně UX hello App Service Environment na podrobnosti.  Vyberte možnost "všechna nastavení"--> "IP adresy".  okno Hello "IP adresy" je příklad všechny explicitně nakonfigurovanou IP SSL adresy pro hello App Service Environment, společně s pár hello speciální port, který je použité tooroute přenosů dat HTTP a HTTPS přidružené každou IP SSL adresu.  Je tento pár port, který potřebuje toobe používá se pro parametry DestinationPortRange hello při konfiguraci pravidel ve skupině zabezpečení sítě.

Když aplikace v App Service Environment je nakonfigurované toouse IP SSL, externí zákazníky, se nezobrazí a není třeba tooworry o hello speciální port pár mapování.  Provoz toohello aplikace bude procházet normálně toohello nakonfigurované IP SSL adresu.  Hello překlad toohello speciální port pár automaticky provede interně při hello konečné fáze směrování provozu do hello podsíť obsahující hello App Service Environment. 

## <a name="getting-started"></a>Začínáme
tooget začít s prostředí App Service najdete v části [Úvod tooApp Service Environment][IntroToAppServiceEnvironment]

Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

Podrobnosti ohledně aplikace ze služby App Service Environment bezpečně připojené toobackend prostředků najdete v tématu [bezpečně připojení tooBackend prostředky ze služby App Service Environment][SecurelyConnecttoBackend]

Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->

