---
title: "aaaLayered Architektura zabezpečení s prostředí App Service"
description: "Implementace architektura vrstveného zabezpečení s prostředí App Service."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 73ce0213-bd3e-4876-b1ed-5ecad4ad5601
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: stefsch
ms.openlocfilehash: 0627ba6fa849908506fe62c451c888c147cabc03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Implementace architektura vrstveného zabezpečení pomocí služby App Service Environment
## <a name="overview"></a>Přehled
Vzhledem k tomu, že prostředí App Service poskytuje izolovaném prostředí nasadit do virtuální sítě, vývojáři můžou vytvářet architektura vrstveného zabezpečení poskytuje různé úrovně přístupu k síti pro každý fyzický aplikační vrstvě.

Společné přání je toohide rozhraní API back EndY z obecné přístup k Internetu a povolit pouze toobe rozhraní API, které jsou volány nadřazeného webové aplikace.  [Skupin zabezpečení (Nsg) sítě] [ NetworkSecurityGroups] jde použít na podsítě, který obsahuje prostředí App Service toorestrict veřejný přístup tooAPI aplikace.

Hello následující diagram ukazuje příklad architekturu a WebAPI na základě aplikaci nasadit na služby App Service Environment.  Tři instance aplikace samostatný webový nasazené na tři samostatné prostředí App Service, ujistěte se, back-end volání toohello stejné WebAPI aplikaci.

![Konceptuální architektura][ConceptualArchitecture] 

Hello zeleného znaménka plus znamenat, že hello skupinu zabezpečení sítě v podsíti hello obsahující "apiase" nadřazeného webové aplikace, umožňuje příchozí volání z hello jako dobře volání ze sebe samotné.  Ale hello stejnou skupinu zabezpečení sítě výslovně odepře přístup toogeneral příchozí provoz z Internetu hello. 

Hello zbývající část tohoto tématu provede hello kroky potřebné tooconfigure skupinu zabezpečení sítě hello v podsíti hello obsahující "apiase".

## <a name="determining-hello-network-behavior"></a>Určení hello chování sítě
Pořadí tooknow jaké zabezpečení sítě, které jsou potřebné pravidla, je nutné toodetermine, kteří klienti sítě bude možné tooreach hello App Service Environment obsahující hello aplikace API a které klienti budou Blokovaní.

Vzhledem k tomu [skupin zabezpečení (Nsg) sítě] [ NetworkSecurityGroups] jsou použité toosubnets a prostředí App Service jsou nasazeny do podsítí, platí pravidla hello obsažené v skupinu NSG příliš**všechny** aplikace běžící na služby App Service Environment.  Pomocí hello ukázková architektura v tomto článku, jakmile skupinu zabezpečení sítě je použité toohello podsíť obsahující všechny aplikace běžící na hello "apiase" App Service Environment bude chráněn hello "apiase" stejné sady pravidel zabezpečení. 

* **Hello odchozí IP adresu z nadřazeného volajícím určit:** co je hello IP adresu nebo adresy nadřazeného volající hello?  Tyto adresy potřebovat toobe explicitně povolen přístup v hello NSG.  Vzhledem k tomu, že volání mezi prostředí App Service jsou považovány za volání "Internet", znamená to hello odchozí IP adresu přiřazenou tooeach z hello tři nadřazeného prostředí App Service musí toobe přistupovat v hello skupina NSG pro podsíť "apiase" hello.   Další informace o určení hello odchozí IP adresu, aplikace běžící ve službě App Service Environment, najdete v tématu hello [síťovou architekturu] [ NetworkArchitecture] článek s přehledem.
* **Aplikace API back-end hello potřebovat toocall samotné?**  Někdy přehlíženým a jemně bod je hello scénář, kde hello back-end aplikace potřebuje toocall sám sebe.  Pokud se aplikace API back-end na služby App Service Environment musí toocall sám sebe, je také zpracovaná jako volání na "Internet".  V hello ukázková architektura to vyžaduje povolení přístupu z hello odchozí IP adresy hello "apiase" App Service Environment také.

## <a name="setting-up-hello-network-security-group"></a>Nastavení hello skupinu zabezpečení sítě
Jakmile sada hello odchozí IP adresy jsou známé, hello dalším krokem je tooconstruct skupinu zabezpečení sítě.  Skupiny zabezpečení sítě lze vytvořit pro virtuální sítě, jakož i klasické virtuální sítě na základě i Resource Manager.  Následující příklady Hello ukazují, vytvoření a konfiguraci skupiny NSG na klasické virtuální sítě pomocí prostředí Powershell.

Pro hello ukázkové architektury hello prostředí jsou umístěny v jihu USA, takže prázdné skupiny NSG se vytvoří v této oblasti:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Nejprve explicitního Povolit pravidlo se přidá infrastruktury Azure správy hello uvedených v článku hello na [příchozí provoz] [ InboundTraffic] pro prostředí App Service.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

V dalším kroku dvě pravidla se přidají tooallow HTTP a HTTPS volání z hello první nadřazeného App Service Environment ("fe1ase").

    #Grant access toorequests from hello first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Omývá a postup opakujte pro hello druhý a třetí nadřazeného prostředí App Service ("fe2ase" a "fe3ase").

    #Grant access toorequests from hello second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access toorequests from hello third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Nakonec udělte přístup toohello odchozí IP adresu hello back-end API App Service Environment, aby můžete volat zpět do sebe sama.

    #Allow apps on hello apiase environment toocall back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Žádná další pravidla zabezpečení sítě je nutné toobe nakonfigurovat, protože každý NSG obsahuje sadu výchozích pravidel, která blokují příchozí přístup z hello Internetu ve výchozím nastavení.

Níže jsou uvedeny Hello úplný seznam pravidel v hello skupinu zabezpečení sítě.  Všimněte si, jak hello poslední pravidlo, které zvýrazní, blokuje příchozí přístup ze všech volající kromě těch, které mají výslovně udělil přístup.

![Konfigurace skupiny NSG][NSGConfiguration] 

posledním krokem Hello je tooapply hello NSG toohello podsítě, která obsahuje hello "apiase" App Service Environment.  

     #Apply hello NSG toohello backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

S podsítí toohello hello skupina NSG se použije hello tři nadřazeného prostředí App Service a hello App Service Environment obsahující hello rozhraní API back-end, jsou povoleny pouze toocall do prostředí "apiase" hello.

## <a name="additional-links-and-information"></a>Další odkazy a informace
Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

Informace o [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md). 

Principy [odchozí IP adresy] [ NetworkArchitecture] a prostředí App Service.

[Síťové porty] [ InboundTraffic] používá prostředí App Service.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
