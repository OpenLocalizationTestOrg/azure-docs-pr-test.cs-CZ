---
title: "aaaNetwork architektura přehled o prostředí App Service"
description: "Přehled architektury ofApp topologie sítě služby prostředí."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 13d03a37-1fe2-4e3e-9d57-46dfb330ba52
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 3cbc86883f5687a9ada35a3ab2f577a450a3fa0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-architecture-overview-of-app-service-environments"></a>Přehled síťové architektury ve službě App Service Environment
## <a name="introduction"></a>Úvod
Prostředí App Service se vytváří vždy v rámci podsítí [virtuální sítě] [ virtualnetwork] -aplikace běžící ve službě App Service Environment může komunikovat s privátní koncové body umístěné v rámci hello stejné virtuální topologie sítě.  Vzhledem k tomu, že zákazníci můžou zamknout součástí infrastruktury svojí virtuální sítě, je důležité toounderstand hello typy toků komunikace sítě, které se služby App Service Environment.

## <a name="general-network-flow"></a>Tok obecné sítě
Používá-li aplikaci služby prostředí řízení, veřejné virtuální adresy IP (VIP) pro aplikace, všechny příchozí přenos dorazí na tuto veřejnou virtuální IP.  To zahrnuje přenosů dat HTTP a HTTPS pro aplikace, a také ostatní přenosy FTP, funkce vzdáleného ladění a operace správy Azure.  Úplný seznam hello určité porty (požadované a volitelné), které jsou dostupné na veřejné VIP hello najdete článku hello na [řízení příchozí provoz] [ controllinginboundtraffic] tooan App Service Environment. 

Prostředí App Service také podporují spouštění aplikace, které jsou vázány jen tooa virtuální sítě interní adresa, nazývaná také jen tooas adresu ILB (nástroj pro vyrovnávání zatížení pro vnitřní).  Na ILB povoleno App Service Environment, HTTP a HTTPS provozy pro aplikace a také vzdáleného ladění volání dorazí na hello ILB adresu.  U většiny běžných konfigurací s ILB App Service Environment přenosy FTP/FTPS také dorazí na hello ILB adresu.  Operace správy Azure bude stále toku tooports 454/455 na hello veřejných virtuálních IP adres z ILB, ale povoleno App Service Environment.

Následující diagram Hello ukazuje přehled hello různé sítě příchozí a odchozí toky pro služby App Service Environment, kde aplikace hello jsou vázané tooa veřejná virtuální IP adresa:

![Obecné síťovými toky][GeneralNetworkFlows]

Služby App Service Environment může komunikovat s celou řadu koncové body privátní zákazníka.  Například aplikace běžící v App Service Environment může připojit toodatabase servery spuštěné v virtuální počítače IaaS v hello hello stejné virtuální síťové topologie.

> [!IMPORTANT]
> Prohlížení hello síťový diagram, jsou v jiné podsíti z hello App Service Environment nasazené hello "ostatní výpočetní prostředky". Nasazení prostředků v hello stejné podsíti s hello App Service Environment bude blokovat připojení z prostředků toothose App Service Environment (s výjimkou směrování konkrétní intra-App Service Environment). Nasazení tooa jiné podsítě místo toho (v hello stejnou virtuální síť). Hello App Service Environment bude možné tooconnect. Není potřeba žádná další konfigurace.
> 
> 

Služby App Service Environment také komunikovat s databáze Sql a Azure Storage prostředky potřebné pro správu a provozování služby App Service Environment.  Některé prostředky hello Sql a úložiště, které komunikují služby App Service Environment jsou umístěné v hello stejné oblasti jako hello App Service Environment, zatímco jiné jsou umístěny v vzdálené oblastech Azure.  V důsledku toho toohello odchozí připojení k Internetu je vždy vyžaduje App Service Environment toofunction správně. 

Vzhledem k tomu, že služby App Service Environment je nasazený na podsíť, skupiny zabezpečení sítě může být použité toocontrol příchozí provoz toohello podsítě.  Podrobnosti o tom, jak toocontrol příchozí provoz tooan App Service Environment, viz následující hello [článku][controllinginboundtraffic].

Podrobnosti o tom tooallow odchozí internetové připojení ze služby App Service Environment, najdete v části hello následujícího článku o práci s [Express Route][ExpressRoute].  Hello stejný přístup popsanou v článku hello se použije při práci s připojení Site-to-Site a pomocí vynuceného tunelování.

## <a name="outbound-network-addresses"></a>Odchozí síťové adresy
Pokud služby App Service Environment provede odchozí volání, je vždy přidružen hello odchozí volání IP adresu.  Hello konkrétní IP adresu, která se používá závisí na tom, jestli se nachází v rámci hello virtuální síťové topologie, nebo mimo hello virtuální síťové topologie koncový bod hello volané.

Pokud je koncový bod hello volané **mimo** hello virtuální síťové topologie, pak se hello je odchozí adresy (neboli hello odchozí NAT adresy), který se používá hello veřejných virtuálních IP adres z hello App Service Environment.  Tato adresa naleznete v hello portálové uživatelské rozhraní pro hello App Service Environment v okně Vlastnosti.

![Odchozí IP adresu][OutboundIPAddress]

Tuto adresu můžete také určit pro ASEs, které mají pouze veřejné VIP vytváření aplikace v hello App Service Environment, a pak provedením *nslookup* na adrese aplikace hello. Hello výsledné IP adresa je hello veřejné VIP, jak hello App Service Environment pro odchozí NAT adresu.

Pokud je koncový bod hello volané **uvnitř** o hello virtuální síťové topologie, bude hello odchozí adresy volání aplikace hello hello interní IP adresu hello jednotlivých výpočetních prostředků, která je spuštěna aplikace hello.  Ale není trvalé mapování virtuální sítě interní IP adresy tooapps.  Aplikace lze pohybovat mezi různé výpočetní prostředky a hello fond dostupných výpočetních prostředků ve službě App Service Environment, můžete změnit z důvodu tooscaling operace.

Ale vzhledem k tomu, že služby App Service Environment se vždy nachází v rámci jedné podsítě, že je zaručeno, že hello interní IP adresu výpočtový prostředek spuštění aplikace bude vždy v rámci hello rozsah CIDR podsítě hello.  Výsledkem je pokud se používá podrobných seznamů řízení přístupu nebo skupiny zabezpečení sítě toosecure přístup tooother koncové body v rámci virtuální sítě hello, hello rozsahu podsítě obsahující hello App Service Environment musí toobe udělen přístup.

Hello následující diagram znázorňuje tyto koncepty podrobněji:

![Odchozí síťové adresy][OutboundNetworkAddresses]

V hello výše diagram:

* Vzhledem k tomu, že hello veřejných virtuálních IP adres z hello App Service Environment je 192.23.1.2, který je hello odchozí IP adresu použitou při volání příliš koncové body "Internet".
* Hello rozsah CIDR hello obsahující podsíť pro hello App Service Environment je 10.0.1.0/26.  Další koncové body v rámci hello stejné infrastruktury virtuální sítě se zobrazí volání z aplikace jako pocházející z někde v tomto rozsahu adres.

## <a name="calls-between-app-service-environments"></a>Volání mezi služby App Service Environment
Složitější situace může nastat, pokud nasazujete více prostředí App Service v hello stejné virtuální síti a odchozí volání z jedné služby App Service Environment tooanother App Service Environment.  Tyto typy křížová volání bude také považována za "Internet" volání služby App Service Environment.

Hello následující diagram ukazuje příklad Vrstvená architektura aplikace na jeden App Service Environment (např.) "Přední dveře" webové aplikace) na (třeba interní aplikace API back-end není určena toobe přístupné z Internetu hello) druhý App Service Environment volání aplikace. 

![Volání mezi služby App Service Environment][CallsBetweenAppServiceEnvironments] 

Příklad výše hello App Service Environment "App Service Environment jeden" v hello má 192.23.1.2 odchozí IP adresu.  Pokud aplikace spuštěné v App Service Environment provede aplikace tooan odchozí volání systémem druhý App Service Environment ("App Service Environment dvě") umístěný ve stejné virtuální síti hello hello odchozí volání se považuje za volání "Internet".  Proto se zobrazí druhý App Service Environment hello síťový provoz přicházejících na hello jako pocházející z 192.23.1.2 (tj. není hello podsítě rozsah adres hello první App Service Environment).

I když volání mezi různé prostředí App Service jsou považovány za "Internet" volání, když jsou oba prostředí App Service v hello stejné oblasti Azure hello síťový provoz zůstane nainstalována na místní síť Azure hello a nebude fyzicky toku přes hello veřejného Internetu.  Proto můžete použít skupinu zabezpečení sítě na podsíť hello hello hello druhý App Service Environment tooonly povolit příchozí volání z první App Service Environment (jehož odchozí IP adresa je 192.23.1.2), čímž zajišťuje zabezpečenou komunikaci mezi hello Služby App Service Environment.

## <a name="additional-links-and-information"></a>Další odkazy a informace
Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

Podrobnosti o příchozí porty používané prostředí App Service a pomocí toocontrol skupiny zabezpečení sítě příchozí provoz je k dispozici [sem][controllinginboundtraffic].

Podrobnosti o použití uživatelsky definované trasy toogrant odchozí internetové přístup tooApp prostředí Service je k dispozici v tomto [článku][ExpressRoute]. 

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

