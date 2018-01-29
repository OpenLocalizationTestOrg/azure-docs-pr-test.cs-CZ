---
title: "Nástroj pro vyrovnávání zatížení několika virtuálními IP adresami pro Azure. | Microsoft Docs"
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
ms.date: 09/25/2017
ms.author: chkuhtz
ms.openlocfilehash: 1045a18f5fd9739a6028198deea129e9e621f127
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="multiple-vips-for-azure-load-balancer"></a>Více virtuálních IP adres pro Azure Load Balancer

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Vyrovnávání zatížení můžete načíst vyrovnat služby na více porty, více IP adres nebo obojí. Definice nástroje pro vyrovnávání zatížení veřejné a interní můžete použít pro toky Vyrovnávání zatížení v rámci sadu virtuálních počítačů.

Tento článek popisuje základní informace o této schopnosti, důležité koncepty a omezení. Pokud chcete vystavit služby na jednu IP adresu, najdete pokyny, zjednodušené [veřejné](load-balancer-get-started-internet-portal.md) nebo [interní](load-balancer-get-started-ilb-arm-portal.md) konfigurací služby Vyrovnávání zatížení. Přidání několika virtuálními IP adresami není přírůstkové k konfigurací jedné virtuální IP adresy. Pomocí koncepty v tomto článku, můžete rozbalit zjednodušená konfigurace kdykoli.

Když definujete k pro vyrovnávání zatížení Azure, front-end a back-end konfigurace jsou připojené pomocí pravidel. Test stavu odkazuje pravidlo se používá k určení jak nové toků se odesílají do uzlu ve fondu back-end. Front-endu je definována pomocí virtuální IP (VIP), což je 3 řazené kolekce členů skládá z adresy IP (veřejné nebo interní), přenosový protokol (UDP nebo TCP) a číslo portu. Vyhrazené IP adresy je IP adresa na Azure virtuální síťovou kartu připojenou k virtuálnímu počítači ve fondu back-end.

Následující tabulka obsahuje některé příklady front-endové konfigurace:

| VIRTUÁLNÍ ADRESA IP. | IP adresa | Protokol | port |
| --- | --- | --- | --- |
| 1 |65.52.0.1 |TCP |80 |
| 2 |65.52.0.1 |TCP |*8080* |
| 3 |65.52.0.1 |*UDP* |80 |
| 4 |*65.52.0.2* |TCP |80 |

V tabulce jsou čtyři různé frontends. Frontends č. 1, #2 a #3 jsou jedné virtuální IP adresy s více pravidel. Se používá stejnou IP adresu, ale portu nebo protokolu se liší pro každý front-endu. Frontends č. 1 a #4 jsou příklad několika virtuálními IP adresami, kde stejný front-endu protokol a port se opětovně použít napříč několika virtuálními IP adresami.

Azure Vyrovnávání zatížení poskytuje flexibilitu při definování pravidel Vyrovnávání zatížení. Pravidlo deklaruje, jak adresu a port front-endu je namapovaný na cílovou adresu a port na back-end. Zda jsou porty back-end opětovně použít napříč pravidla závisí na typu pravidla. Každý typ pravidla má specifické požadavky, které můžou ovlivnit návrh a konfigurace testu hostitele. Existují dva typy pravidel:

1. Výchozí pravidlo s opakované použití žádné back-end port
2. Pravidlo plovoucí IP adresa, kde jsou opakovaně porty back-end

Azure Vyrovnávání zatížení můžete kombinovat oba typy pravidel na stejnou konfiguraci služby Vyrovnávání zatížení. Nástroje pro vyrovnávání zatížení můžete používat současně pro daný virtuální počítač nebo libovolnou kombinaci tak dlouho, dokud dodržovat omezení pravidla. Jaký typ pravidla zvolíte závisí na požadavky na aplikace a složitost podpora této konfigurace. Byste měli zvážit typů pravidlo, které jsou vhodné pro váš scénář.

Nám prozkoumat tyto další scénáře od výchozí chování.

## <a name="rule-type-1-no-backend-port-reuse"></a>Pravidlo typu #1: žádné opakované použití portu back-end

![Obrázek víc virtuálními IP adresami](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

V tomto scénáři se virtuální IP adresy front-endu nakonfigurovány takto:

| VIRTUÁLNÍ ADRESA IP. | IP adresa | Protokol | port |
| --- | --- | --- | --- |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

DIP je cílem příchozí tok. Každý virtuální počítač ve fondu back-end zpřístupní požadované služby v jedinečný port vyhrazené IP adresy. Tato služba je spojeno s front-endu prostřednictvím definice pravidla.

Jsme definovali dvě pravidla:

| Pravidlo | Mapování front-endu | Do fondu back-end |
| --- | --- | --- |
| 1 |![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 |![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Dokončení mapování nástroji pro vyrovnávání zatížení Azure je teď následujícím způsobem:

| Pravidlo | Virtuální IP adresy IP adresu | Protokol | port | Cíl | port |
| --- | --- | --- | --- | --- | --- |
| ![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |Vyhrazené IP adresy IP adresu |80 |
| ![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |Vyhrazené IP adresy IP adresu |81 |

Každé pravidlo musí vytvořit toku s jedinečnou kombinaci cílové IP adresy a cílového portu. Pomocí různých cílový port toku, více pravidel doručovat toky do stejné DIP na jiné porty.

Sondy stavu jsou vždy směrovala vyhrazené IP adresy virtuálního počítače. Musíte si zajistěte, aby vaše testu odráží stav virtuálního počítače.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Pravidlo typu #2: opakované použití portu back-end pomocí plovoucí IP adresa

Azure nástroj pro vyrovnávání zatížení poskytuje možnost opakovaně použít front-endový port mezi několika virtuálními IP adresami bez ohledu na typ pravidla používat. Kromě toho některé aplikace scénáře raději nebo vyžadovat stejný port, který bude používat více instancí aplikace na jeden virtuální počítač ve fondu back-end. Běžných příkladů opakované použití portu obsahovat clustering pro vysokou dostupnost, sítě, virtuální zařízení a vystavení několik koncových bodů protokolu TLS bez znova šifrovat.

Pokud chcete znovu použít back-endový port napříč více pravidel, je nutné povolit plovoucí IP adresa v definice pravidla.

Plovoucí IP adresa je část, která se označuje jako přímý Server vrátit (DSR). DSR se skládá ze dvou částí: topologii toku a IP adres schéma mapování. Na úrovni platformy nástroj pro vyrovnávání zatížení Azure vždy funguje v topologii DSR toku bez ohledu na to, jestli je zapnutá plovoucí IP adresa, nebo ne. To znamená, že část odchozí tok je vždy správně přepisuje, aby toku přímo zpět k počátku.

S typem výchozí pravidla zpřístupní Azure tradiční schéma mapování IP adres pro snadné použití vyrovnávání zatížení. Povolení plovoucí IP adresa změní schéma mapování IP adres, povolit za účelem vyšší flexibility, jak je popsáno níže.

Následující obrázek znázorňuje tuto konfiguraci:

![Obrázek víc virtuálními IP adresami](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

V tomto scénáři má každý virtuální počítač ve fondu back-end tři síťových rozhraní:

* Vyhrazené IP adresy: virtuální síťový adaptér přidružený virtuální počítač (Konfigurace IP Síťových prostředků Azure)
* VIP1: rozhraní zpětné smyčky v rámci hostovaného operačního systému, který je nakonfigurovaný s IP adresou VIP1
* VIP2: rozhraní zpětné smyčky v rámci hostovaného operačního systému, který je nakonfigurovaný s IP adresou VIP2

> [!IMPORTANT]
> Konfigurace logické rozhraní se provádí v rámci hostovaného operačního systému. Tato konfigurace není provést nebo spravované přes Azure. Bez této konfiguraci nebude fungovat pravidla. Stav testu definice používat vyhrazené IP adresy virtuálního počítače, nikoli logické VIP. Proto služby, musíte zadat odpovědi testu portu vyhrazené IP adresy, které odráží stav služby nabízených na logické VIP.

Předpokládejme stejný front-endovou konfiguraci jako v předchozím scénáři:

| VIRTUÁLNÍ ADRESA IP. | IP adresa | Protokol | port |
| --- | --- | --- | --- |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

Jsme definovali dvě pravidla:

| Pravidlo | Mapování front-endu | Do fondu back-end |
| --- | --- | --- |
| 1 |![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (v VM1 a virtuálního počítače 2) |
| 2 |![Pravidlo](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (v VM1 a virtuálního počítače 2) |

V následující tabulce jsou uvedeny dokončení mapování nástroji pro vyrovnávání zatížení:

| Pravidlo | Virtuální IP adresy IP adresu | Protokol | port | Cíl | port |
| --- | --- | --- | --- | --- | --- |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |stejné jako VIP (65.52.0.1) |stejné jako VIP (80) |
| ![VIRTUÁLNÍ ADRESA IP.](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |stejné jako VIP (65.52.0.2) |stejné jako VIP (80) |

Cílovým serverem příchozí tok je adresa VIP na rozhraní zpětné smyčky ve virtuálním počítači. Každé pravidlo musí vytvořit toku s jedinečnou kombinaci cílové IP adresy a cílového portu. Pomocí různých cílovou IP adresu toku, opakované použití portu je možné na stejného virtuálního počítače. Služby je vystaven pro vyrovnávání zatížení pomocí vytvoření vazby virtuální IP adresy IP adresu a port rozhraní příslušných zpětné smyčky.

Všimněte si, že v tomto příkladu nezmění cílový port. I když se jedná o scénář plovoucí IP adresa, podporuje nástroj pro vyrovnávání zatížení Azure také definice pravidla přepsání cílový port back-end a aby se liší od cílový port front-endu.

Typ pravidla plovoucí IP adresa je základ pro několik vzorů konfigurace služby Vyrovnávání zatížení. Příkladem, který je aktuálně k dispozici je [technologie AlwaysOn serveru SQL s více naslouchací procesy](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) konfigurace. V průběhu času jsme se dokumentů z uvedených scénářů.

## <a name="limitations"></a>Omezení

* Více konfigurací virtuálních IP adres jsou podporovány pouze s virtuální počítače IaaS.
* S tímto pravidlem plovoucí IP adresa musí vaše aplikace používat DIP pro odchozí toky. Pokud vaše aplikace se váže k adresa VIP konfigurována na rozhraní zpětné smyčky v hostovaný operační systém, pak není k dispozici přepsání odchozího toku překládat pomocí SNAT a toku nezdaří.
* Veřejné IP adresy mají vliv na fakturace. Další informace najdete v tématu [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Limity předplatného použít. Další informace najdete v tématu [omezení služby](../azure-subscription-service-limits.md#networking-limits) podrobnosti.
