---
title: aaaHow tooCreate v1 App Service Environment
description: "Popis toku vytvoření app service prostředí v1"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 95feb33854eee5bac02fa68b066e2fc10eb3fede
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-app-service-environment-v1"></a>Jak tooCreate v1 App Service Environment 

> [!NOTE]
> Tento článek je o hello App Service Environment v1. Není k dispozici novější verze hello služby App Service Environment, je snazší toouse a běží na výkonnější infrastruktury. Další informace o nové verzi hello začínat hello toolearn [toohello Úvod App Service Environment](../app-service/app-service-environment/intro.md).
> 

### <a name="overview"></a>Přehled
Hello prostředí App Service (App Service Environment) je možnost Premium služby Azure App Service, který doručí funkci rozšířené konfigurace, která není k dispozici v hello víceklientské razítka. Funkce App Service Environment Hello v podstatě nasadí hello Azure App Service do virtuální sítě zákazníka. toogain lépe porozumět hello možnosti nabízí prostředí App Service číst hello [novinky služby App Service Environment] [ WhatisASE] dokumentaci.

### <a name="before-you-create-your-ase"></a>Před vytvořením vaší App Service Environment
Je důležité toobe vědět hello věcí, které nelze změnit. Tyto aspekty, které o vaší App Service Environment nelze změnit po vytvoření jsou:

* Umístění
* Předplatné
* Skupina prostředků
* Virtuální síť použít
* Podsítě používané 
* Velikost podsítě

Pokud výběr virtuální sítě a určení podsíť, ujistěte se, zda je dostatečně velké na to tooaccomodate případnému budoucímu růstu. 

### <a name="creating-an-app-service-environment-v1"></a>Vytvoření aplikace služby prostředí v1
toocreate App Service Environment v1, budete potřebovat toosearch hello Azure Marketplace pro ***App Service Environment v1***, nebo prostřednictvím nový -> Web + mobilní zařízení -> Služba App Service Environment. toocreate ASEv1:

1. Zadejte jméno hello vaší App Service Environment. Hello název, který je zadán pro App Service Environment hello se použije pro aplikace hello vytvořené v hello App Service Environment. Pokud název hello App Service Environment je appsvcenvdemo název subdomény hello by. *appsvcenvdemo.p.azurewebsites.net*. Pokud jste tedy vytvořili aplikaci s názvem *mytestapp* by být adresovatelné v *mytestapp.appsvcenvdemo.p.azurewebsites.net*. Prázdné znaky nelze použít v hello název vaší App Service Environment. Pokud používáte velkých písmen v názvu text hello, bude název domény hello celkový malá verze hello s tímto názvem. Pokud používáte ILB pak název App Service Environment není používáno ve vašem subdomény, ale je místo toho výslovně uveden během vytváření App Service Environment
   
    ![][1]
2. Vyberte své předplatné. Hello předplatné použité pro vaše App Service Environment je také hello jednu, která všechny aplikace v tomto App Service Environment bude vytvořena s. Nelze umístit vaše App Service Environment ve virtuální síti, která je v jiném předplatném.
3. Vyberte nebo zadejte novou skupinu prostředků. Skupina prostředků Hello používá pro vaše App Service Environment musí hello stejné, který se používá pro vaši virtuální síť. Pokud vyberete existující virtuální síť pak hello výběr skupiny prostředků pro vaše App Service Environment bude aktualizované tooreflect u vaší virtuální sítě.
   
    ![][2]
4. Proveďte požadovaná nastavení virtuální sítě a umístění. Můžete vybrat toocreate nové virtuální sítě nebo vyberte existující virtuální síť. Pokud vyberete novou virtuální síť, potom můžete zadat název a umístění. Hello novou síť VNet bude mít 192.168.250.0/23 rozsah adres hello a podsíť s názvem **výchozí** který je definován jako 192.168.250.0/24. Můžete taky jednoduše vybrat existující Classic nebo Resource Manager virtuální sítě. Výběr typu VIP Hello Určuje, pokud vaše App Service Environment je přímo přístupný z hello Internetu (externí) nebo pokud používá interní nástroj pro vyrovnávání zatížení (ILB). Další informace o nich přečíst toolearn [interní pro vyrovnávání zatížení pomocí služby App Service Environment][ILBASE]. Pokud vyberete typ externí virtuální IP adresy můžete vybrat, kolik externího systému hello adresy IP je vytvořena s pro účely IPSSL. Pokud vyberete interní musíte toospecify hello subdomény, který bude používat vaše App Service Environment. ASEs lze nasadit do virtuálních sítí, které používají *buď* rozsahů veřejných adres *nebo* RFC1918 adresní prostory (tj. privátní adresy). V pořadí toouse virtuální síť s veřejnou adresu rozsahu budete potřebovat toocreate hello virtuální síť předem. Když vyberete existující virtuální síť, která budete potřebovat toocreate novou podsíť během vytváření App Service Environment. **V portálu hello nelze použít předem vytvořené podsíť. Pokud vytvoříte vaší App Service Environment pomocí šablony správce prostředků, můžete vytvořit App Service Environment s existující podsítí.** toocreate App Service Environment ze šablony hello informace použijte, [vytváření služby App Service Environment z šablony] [ ILBAseTemplate] a zde [z šablonyvytvářeníILBAppServiceEnvironment] [ASEfromTemplate].

### <a name="details"></a>Podrobnosti
App Service Environment je vytvořen s 2 končí Front a 2 pracovní procesy. Hello Front končí fungovat jako koncové body hello protokolu HTTP nebo HTTPS a odesílat provoz toohello pracovních procesů, které jsou hello rolí, které jsou hostiteli aplikace. Můžete upravit hello množství po vytvoření App Service Environment a můžete také nastavit pravidel škálování na těchto fondů zdrojů. Další podrobnosti ohledně ruční škálování, Správa a sledování služby App Service Environment přejděte sem: [jak tooconfigure služby App Service Environment][ASEConfig] 

V podsíti hello používá hello App Service Environment může existovat pouze hello jeden App Service Environment. podsíť Hello nelze použít pro jakoukoli jinou hodnotu než hello App Service Environment

### <a name="after-app-service-environment-v1-creation"></a>Po vytvoření služby App Service Environment v1
Po vytvoření App Service Environment můžete upravit:

* Počet Front končí (minimální: 2)
* Počet pracovních procesů (minimální: 2)
* Množství IP adres, které jsou k dispozici pro protokol SSL IP
* Výpočetní velikostí prostředků používané hello front-end nebo pracovníci (minimální velikost Front-endu je P2)

Existují další podrobnosti ohledně ruční škálování, správu a monitorování zde prostředí App Service: [jak tooconfigure služby App Service Environment][ASEConfig] 

Informace o automatické škálování je Průvodce zde: [jak tooconfigure škálování App Service Environment][ASEAutoscale]

Existují další závislosti, které nejsou k dispozici pro přizpůsobení například hello databáze a úložiště. Jsou zpracovávány v Azure a dodávají s hello systému. Hello systému úložiště podporuje až too500 GB pro hello celý App Service Environment a databáze hello se upraví Azure podle potřeb škálování hello hello systému.

## <a name="getting-started"></a>Začínáme
Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

tooget začít s App Service Environment v1, najdete v části [Úvod toohello v1 App Service Environment][WhatisASE]

Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
