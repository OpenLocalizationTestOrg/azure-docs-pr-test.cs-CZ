---
title: "aaaMultiple VIP pro vyrovnávání zatížení Azure | Microsoft Docs"
description: "Přehled několika virtuálními IP adresami v nástroji pro vyrovnávání zatížení Azure"
services: load-balancer
documentationcenter: na
author: chkuhtz
manager: narayan
editor: 
ms.assetid: 748e50cd-3087-4c2e-a9e1-ac0ecce4f869
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2016
ms.author: chkuhtz
ms.openlocfilehash: e186e1248e7d187e7efd0668dc3244465ce3e156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multiple-vips-for-azure-load-balancer"></a>Více virtuálních IP adres pro Azure Load Balancer

Azure nástroj pro vyrovnávání zatížení můžete tooload vyrovnat služby na více porty, více IP adres nebo obojí. Můžete vytvořit veřejné a interní zatížení vyrovnávání definice tooload vyrovnávání toků mezi sadu virtuálních počítačů.

Tento článek popisuje hello základy tuto možnost, důležité koncepty a omezení. Pokud hodláte tooexpose služby pouze na jednu IP adresu, můžete najít zjednodušené pokyny pro [veřejné](load-balancer-get-started-internet-portal.md) nebo [interní](load-balancer-get-started-ilb-arm-portal.md) konfigurací služby Vyrovnávání zatížení. Přidání několika virtuálními IP adresami je přírůstkové tooa jednu VIP konfigurace. Pomocí hello koncepty v tomto článku, můžete rozbalit zjednodušená konfigurace kdykoli.

Když definujete k pro vyrovnávání zatížení Azure, front-end a back-end konfigurace jsou připojené pomocí pravidel. Hello test stavu odkazuje hello pravidlo je použité toodetermine jak nové toky jsou odesílány tooa uzlu ve fondu back-end hello. front-endu Hello je definována pomocí virtuální IP (VIP), což je 3 řazené kolekce členů skládá z adresy IP (veřejné nebo interní), přenosový protokol (UDP nebo TCP) a číslo portu. Vyhrazené IP adresy je, že IP adresu na Azure virtuálního síťového adaptéru připojený tooa virtuálního počítače ve fondu back-end hello.

Hello následující tabulka obsahuje některé příklady front-endové konfigurace:

| VIRTUÁLNÍ ADRESA IP. | IP adresa | Protokol | port |
| --- | --- | --- | --- |
| 1 |65.52.0.1 |TCP |80 |
| 2 |65.52.0.1 |TCP |*8080* |
| 3 |65.52.0.1 |*UDP* |80 |
| 4 |*65.52.0.2* |TCP |80 |

Hello tabulka obsahuje čtyři různé frontends. Frontends č. 1, #2 a #3 jsou jedné virtuální IP adresy s více pravidel. Hello stejnou IP adresu se používá ale hello portu nebo protokolu se liší pro každý front-endu. Frontends č. 1 a #4 jsou příklad několika virtuálními IP adresami, kde hello stejný front-endu protokol a port se opětovně použít napříč několika virtuálními IP adresami.

Azure Vyrovnávání zatížení poskytuje flexibilitu při definování pravidel Vyrovnávání zatížení hello. Pravidlo deklaruje, jak adresu a port na front-endu hello je namapované toohello cílovou adresu a port na back-end hello. Zda jsou porty back-end opětovně použít napříč pravidla závisí na typu hello hello pravidla. Každý typ pravidla má specifické požadavky, které můžou ovlivnit návrh a konfigurace testu hostitele. Existují dva typy pravidel:

1. znovu použít výchozí pravidlo Hello s žádné back-endový port
2. pravidlo Hello plovoucí IP adresa, kde jsou opakovaně porty back-end

Azure Vyrovnávání zatížení můžete toomix oba typy pravidel na hello stejné konfigurace služby Vyrovnávání zatížení. Nástroj pro vyrovnávání zatížení Hello můžete je používat současně pro daný virtuální počítač nebo libovolnou kombinaci tak dlouho, dokud dodržovat hello omezení hello pravidla. Jaký typ pravidla můžete zvolit, závisí na hello požadavky vaší aplikace a hello složitosti podpora této konfigurace. Byste měli zvážit typů pravidlo, které jsou vhodné pro váš scénář.

Nám prozkoumat tyto další scénáře od hello výchozí chování.

## <a name="rule-type-1-no-backend-port-reuse"></a>Pravidlo typu #1: žádné opakované použití portu back-end

![Obrázek víc virtuálními IP adresami](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

V tomto scénáři jsou hello front-endu VIP nakonfigurovány takto:

| VIRTUÁLNÍ ADRESA IP. | IP adresa | Protokol | port |
| --- | --- | --- | --- |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

Hello vyhrazené IP adresy je cílem hello hello příchozí tok. Ve fondu back-end hello každý virtuální počítač zpřístupňuje hello potřeby službu na jedinečný port vyhrazené IP adresy. Tato služba je spojeno s front-endu hello prostřednictvím definice pravidla.

Jsme definovali dvě pravidla:

| Pravidlo | Mapování front-endu | toobackend fondu |
| --- | --- | --- |
| 1 |![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 |![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

dokončení mapování Hello nástroji pro vyrovnávání zatížení Azure je teď následujícím způsobem:

| Pravidlo | Virtuální IP adresy IP adresu | Protokol | port | Cíl | port |
| --- | --- | --- | --- | --- | --- |
| ![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |Vyhrazené IP adresy IP adresu |80 |
| ![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |Vyhrazené IP adresy IP adresu |81 |

Každé pravidlo musí vytvořit toku s jedinečnou kombinaci cílové IP adresy a cílového portu. Podle různých cílový port hello hello toku můžete více pravidel poskytovat toky toohello stejné vyhrazené IP adresy na jiné porty.

Sondy stavu jsou vždy směrovanou toohello vyhrazené IP adresy virtuálního počítače. Musíte si zajistěte, aby vaše testu odráží stav hello hello virtuálních počítačů.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Pravidlo typu #2: opakované použití portu back-end pomocí plovoucí IP adresa

Azure nástroj pro vyrovnávání zatížení poskytuje flexibilitu hello tooreuse hello front-endový port mezi několika virtuálními IP adresami bez ohledu na typ pravidla hello používá. Kromě toho některé aplikace scénáře přednost nebo vyžadovat hello stejný port toobe používá více instancí aplikace na jeden virtuální počítač ve fondu back-end hello. Běžných příkladů opakované použití portu obsahovat clustering pro vysokou dostupnost, sítě, virtuální zařízení a vystavení několik koncových bodů protokolu TLS bez znova šifrovat.

Pokud chcete tooreuse hello back-endový port napříč více pravidel, je nutné povolit plovoucí IP adresa v hello Definice pravidla.

Plovoucí IP adresa je část, která se označuje jako přímý Server vrátit (DSR). DSR se skládá ze dvou částí: topologii toku a IP adres schéma mapování. Na úrovni platformy nástroj pro vyrovnávání zatížení Azure vždy funguje v topologii DSR toku bez ohledu na to, jestli je zapnutá plovoucí IP adresa, nebo ne. Znamená, že hello odchozí součástí tokem je vždy správně přepsaná tooflow přímo back toohello původu.

S hello výchozí pravidlo typ zpřístupní Azure tradiční schéma mapování IP adres pro snadné použití vyrovnávání zatížení. Povolení plovoucí IP adresa změní tooallow schéma mapování IP adres hello za účelem vyšší flexibility, jak je vysvětleno níže.

Hello následující diagram znázorňuje tuto konfiguraci:

![Obrázek víc virtuálními IP adresami](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

V tomto scénáři má každý virtuální počítač ve fondu back-end hello tři síťových rozhraní:

* Vyhrazené IP adresy: virtuální síťový adaptér přidružený hello virtuálního počítače (IP konfigurace Síťových prostředků Azure)
* VIP1: rozhraní zpětné smyčky v rámci hostovaného operačního systému, který je nakonfigurovaný s IP adresou VIP1
* VIP2: rozhraní zpětné smyčky v rámci hostovaného operačního systému, který je nakonfigurovaný s IP adresou VIP2

> [!IMPORTANT]
> Konfigurace Hello hello logické rozhraní se provádí v rámci hello hostovaný operační systém. Tato konfigurace není provést nebo spravované přes Azure. Bez této konfiguraci nebude fungovat hello pravidla. Stav testu definice použít hello DIP hello virtuálních počítačů nemusí hello logické VIP. Proto služby, musíte zadat odpovědi testu portu vyhrazené IP adresy, které odpovídala hello stav služby hello nabízí v logické VIP hello.

Předpokládejme hello stejný front-endovou konfiguraci jako v předchozím scénáři hello:

| VIRTUÁLNÍ ADRESA IP. | IP adresa | Protokol | port |
| --- | --- | --- | --- |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

Jsme definovali dvě pravidla:

| Pravidlo | Mapování front-endu | toobackend fondu |
| --- | --- | --- |
| 1 |![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (v VM1 a virtuálního počítače 2) |
| 2 |![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (v VM1 a virtuálního počítače 2) |

Hello následující tabulka ukazuje dokončení mapování hello nástroji pro vyrovnávání zatížení hello:

| Pravidlo | Virtuální IP adresy IP adresu | Protokol | port | Cíl | port |
| --- | --- | --- | --- | --- | --- |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |stejné jako VIP (65.52.0.1) |stejné jako VIP (80) |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |stejné jako VIP (65.52.0.2) |stejné jako VIP (80) |

Cílový Hello hello příchozí tok je hello virtuální adresy IP adresu na hello rozhraní zpětné smyčky v hello virtuálních počítačů. Každé pravidlo musí vytvořit toku s jedinečnou kombinaci cílové IP adresy a cílového portu. Pomocí různých hello cílové IP adresy hello toku, je možné na opakované použití portu hello stejného virtuálního počítače. Služby je nástroj pro vyrovnávání zatížení zveřejněné toohello pomocí vytvoření vazby toohello virtuálních IP adres pro IP adresu a port hello příslušných zpětné smyčky rozhraní.

Všimněte si, že v tomto příkladu nezmění hello cílový port. I když se jedná o scénář plovoucí IP adresa, nástroj pro vyrovnávání zatížení Azure podporuje také definovat pravidlo toorewrite hello back-end cílový port a toomake se liší od hello front-endu cílový port.

Hello typ pravidla plovoucí IP adresa je hello foundation několik schémat konfigurace služby Vyrovnávání zatížení. Příkladem, který je aktuálně k dispozici je hello [technologie AlwaysOn serveru SQL s více naslouchací procesy](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) konfigurace. V průběhu času jsme se dokumentů z uvedených scénářů.

## <a name="limitations"></a>Omezení

* Více konfigurací virtuálních IP adres jsou podporovány pouze s virtuální počítače IaaS.
* S pravidlem hello plovoucí IP adresa musí vaše aplikace používat hello vyhrazené IP adresy pro odchozí toky. Pokud vaše aplikace vytvoří vazbu toohello virtuální adresy IP adresou nakonfigurovanou na hello rozhraní zpětné smyčky v hello hostovaný operační systém, pak překládat pomocí SNAT není k dispozici toorewrite hello odchozího toku a hello toku nezdaří.
* Veřejné IP adresy mají vliv na fakturace. Další informace najdete v tématu [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Limity předplatného použít. Další informace najdete v tématu [omezení služby](../azure-subscription-service-limits.md#networking-limits) podrobnosti.
