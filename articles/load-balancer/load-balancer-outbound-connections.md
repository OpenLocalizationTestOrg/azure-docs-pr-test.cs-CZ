---
title: "odchozí připojení aaaUnderstanding v Azure | Microsoft Docs"
description: "Tento článek vysvětluje, jak Azure umožňuje toocommunicate virtuálních počítačů služby veřejného Internetu."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 5/31/2017
ms.author: kumud
ms.openlocfilehash: ffe59971b934a16c9f6f78e3b15869a0072320d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-outbound-connections-in-azure"></a>Principy odchozích připojení v Azure

V Azure virtuální počítač (VM) může komunikovat s koncovými body mimo Azure ve veřejném adresním prostoru IP adres. Když virtuální počítač zahájí cíli tooa odchozího toku v prostor veřejných IP adres, Azure mapuje hello Virtuálního počítače privátní IP adresu tooa veřejnou IP adresu a umožňuje hello tooreach návratový provoz virtuálních počítačů.

Azure nabízí tři různé metody tooachieve odchozí připojení. Každá z metod má svou vlastní možnosti a omezení. Každá metoda zkontrolovat si důkladně toochoose, která vyhovuje vašim potřebám.

| Scénář | Metoda | Poznámka |
| --- | --- | --- |
| Samostatný virtuální počítač (žádná služba Vyrovnávání zatížení, žádná Instance úroveň veřejná IP adresa) |Výchozí překládat pomocí SNAT |Azure přidruží veřejnou IP adresu pro překládat pomocí SNAT |
| Vyrovnávání zatížení sítě virtuálních počítačů (žádná Instance úroveň veřejná IP adresa ve virtuálním počítači) |Použití služby Vyrovnávání zatížení hello překládat pomocí SNAT |Azure používá veřejnou IP adresu hello nástroj pro vyrovnávání zatížení pro překládat pomocí SNAT |
| Virtuální počítač s Instance úroveň veřejnou IP adresu (s nebo bez nástroj pro vyrovnávání zatížení) |Překládat pomocí SNAT se nepoužívá. |Azure používá hello veřejné IP adresy přiřazené toohello virtuálních počítačů |

Pokud nechcete, aby toocommunicate virtuálních počítačů s koncovými body mimo Azure ve veřejném adresním prostoru IP adres, můžete použít skupiny zabezpečení sítě (NSG) tooblock přístup. Pomocí skupin Nsg je podrobněji popsána v [brání veřejného připojení](#preventing-public-connectivity).

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Samostatný virtuální počítač s žádné Instance úroveň veřejnou IP adresu

V tomto scénáři hello virtuální počítač není součástí fondu vyrovnávání zatížení Azure a nemá o instanci úrovně veřejné IP splnění adresu přiřazenou tooit. Při hello virtuálního počítače vytvoří odchozí tok, znamená, že Azure hello privátní zdrojovou IP adresu hello odchozího toku tooa veřejné zdrojové IP adresy. Hello veřejnou IP adresu použitou pro tuto odchozího toku není Konfigurovatelný a nepočítá proti hello předplatné limitu prostředků veřejné IP. Azure používá zdroje síťové adresy překlad (SNAT) tooperform tuto funkci. Dočasné porty hello veřejné IP adresy jsou jednotlivé toků použít toodistinguish původce hello virtuálních počítačů. Překládat pomocí SNAT dynamicky přiděluje dočasné porty, když se vytváří toky. V tomto kontextu jsou hello dočasné porty používané pro překládat pomocí SNAT odkazované tooas překládat pomocí SNAT porty.

Konečný prostředek, který může dojít k vyčerpání jsou porty překládat pomocí SNAT. Je důležité toounderstand, jak se spotřebovávají. Jeden port překládat pomocí SNAT obsazením podle toku tooa jedné cílové IP adresy. Pro více toků toohello stejnou cílovou IP adresu, každý tok využívá jediný port překládat pomocí SNAT. Tím se zajistí hello toky jedinečné v případě pochází z hello stejné veřejnou IP adresu toohello stejnou cílovou IP adresu. Více toků každou tooa jinou cílovou IP adresu používat jediný port překládat pomocí SNAT na cílový. Díky Hello cílové IP adresy je hello toky jedinečný.

Můžete použít [analýzy protokolů pro vyrovnávání zatížení](load-balancer-monitor-log.md) a [toomonitor výstrahy protokoly událostí pro zprávy vyčerpání port překládat pomocí SNAT](load-balancer-monitor-log.md#alert-event-log). Když vyčerpání prostředků překládat pomocí SNAT port, odchozí toky nezdaří, dokud překládat pomocí SNAT porty jsou vydal existující toky. Nástroj pro vyrovnávání zatížení používá časový limit nečinnosti 4minutové opětovného získání porty překládat pomocí SNAT.

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>Vyrovnávání zatížení sítě virtuálních počítačů s žádné Instance úroveň veřejnou IP adresu

V tomto scénáři hello virtuální počítač je součástí fondu vyrovnávání zatížení Azure.  Hello virtuální počítač nemá veřejnou IP adresu přiřadit tooit. Hello prostředek pro vyrovnávání zatížení musí být nakonfigurované pravidlo toolink hello veřejnou IP front-endovou s fondem back-end hello.  Pokud nezadáte tuto konfiguraci, hello chování je, jak je popsáno v části hello výše pro [samostatný virtuální počítač s žádné Instance úroveň veřejnou IP adresu](load-balancer-outbound-connections.md#standalone-vm-with-no-instance-level-public-ip-address).

Při hello Vyrovnávání zatížení sítě virtuálního počítače vytvoří odchozí tok, znamená, že Azure hello privátní zdrojovou IP adresu hello odchozího toku toohello veřejnou IP adresu hello veřejné Vyrovnávání zatížení, front-endu. Azure používá zdroje síťové adresy překlad (SNAT) tooperform tuto funkci. Dočasné porty Vyrovnávání zatížení hello veřejné IP adresy jsou jednotlivé toků použít toodistinguish původce hello virtuálních počítačů. Překládat pomocí SNAT dynamicky přiděluje dočasné porty, když se vytváří odchozí toky. V tomto kontextu jsou hello dočasné porty používané pro překládat pomocí SNAT odkazované tooas překládat pomocí SNAT porty.

Konečný prostředek, který může dojít k vyčerpání jsou porty překládat pomocí SNAT. Je důležité toounderstand, jak se spotřebovávají. Jeden port překládat pomocí SNAT obsazením podle toku tooa jedné cílové IP adresy. Pro více toků toohello stejnou cílovou IP adresu, každý tok využívá jediný port překládat pomocí SNAT. Tím se zajistí hello toky jedinečné v případě pochází z hello stejné veřejnou IP adresu toohello stejnou cílovou IP adresu. Více toků každou tooa jinou cílovou IP adresu používat jediný port překládat pomocí SNAT na cílový. Díky Hello cílové IP adresy je hello toky jedinečný.

Můžete použít [analýzy protokolů pro vyrovnávání zatížení](load-balancer-monitor-log.md) a [toomonitor výstrahy protokoly událostí pro zprávy vyčerpání port překládat pomocí SNAT](load-balancer-monitor-log.md#alert-event-log). Když vyčerpání prostředků překládat pomocí SNAT port, odchozí toky nezdaří, dokud překládat pomocí SNAT porty jsou vydal existující toky. Nástroj pro vyrovnávání zatížení používá časový limit nečinnosti 4minutové opětovného získání porty překládat pomocí SNAT.

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>Virtuální počítač s adresu veřejná IP adresa na úrovni Instance (s nebo bez nástroj pro vyrovnávání zatížení)

V tomto scénáři má hello virtuálních počítačů instanci úroveň veřejné IP splnění přiřazené tooit. Nezáleží, jestli hello virtuálního počítače je Vyrovnávané nebo ne. Pokud se používá splnění, se nepoužije zdrojový síťové adresy překlad (SNAT). Hello virtuální počítač používá hello splnění pro všechny odchozí toky. Pokud vaše aplikace iniciuje mnoho odchozí toky a dochází k vyčerpání překládat pomocí SNAT, měli byste zvážit přiřazení splnění tooavoid omezení překládat pomocí SNAT.

## <a name="discovering-hello-public-ip-used-by-a-given-vm"></a>Zjišťování hello veřejné IP adresy používané daného virtuálního počítače

Existuje mnoho způsobů toodetermine hello veřejné zdrojovou IP adresu odchozí připojení. OpenDNS poskytuje službu, která lze zobrazit hello veřejnou IP adresu vašeho virtuálního počítače. Použití příkazu nslookup hello, můžete odeslat dotaz DNS pro hello název myip.opendns.com toohello OpenDNS překladač. Služba Hello vrací hello zdrojovou IP adresu, která byla použité toosend hello dotazu. Při spuštění hello následujících dotazu z virtuálního počítače je hello odpovědi hello používá veřejnou IP adresu pro tento virtuální počítač.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Nelze navázat připojení veřejného

Někdy je žádoucí pro virtuální počítač toobe povolené toocreate odchozího toku nebo může být požadavek toomanage, které cíle je dostupný odchozí toky. V takovém případě použijete [skupiny zabezpečení sítě (NSG)](../virtual-network/virtual-networks-nsg.md) toomanage hello cíle tohoto hello dosáhnout virtuálních počítačů. Když použijete skupinu NSG tooa Vyrovnávání zatížení sítě virtuálních počítačů, musíte toopay pozornost toohello [výchozí značky](../virtual-network/virtual-networks-nsg.md#default-tags) a [výchozí pravidla](../virtual-network/virtual-networks-nsg.md#default-rules).

Je nutné zajistit, že tento hello virtuálních počítačů může přijímat žádosti o kontrolu stavu z nástroje pro vyrovnávání zatížení Azure. Pokud NSG bloky stavu testu požadavků z hello AZURE_LOADBALANCER výchozí značka, váš test stavu virtuálního počítače se nezdaří a hello virtuálního počítače je označena. Nástroj pro vyrovnávání zatížení zastaví odesílání nové toky toothat virtuálních počítačů.

## <a name="limitations"></a>Omezení

Pokud [více (veřejné) IP adresy, které jsou přidružené Vyrovnávání zatížení](load-balancer-multivip-overview.md), všechny tyto veřejné IP adresy jsou kandidátem pro odchozí toky.

Azure používá algoritmus toodetermine hello počet překládat pomocí SNAT porty, které jsou k dispozici na základě velikosti hello hello fondu.  To se nedá konfigurovat v tuto chvíli.

Je důležité, že toorememember, který hello počet dostupných portů překládat pomocí SNAT nepřekládá přímo toonumber připojení. Viz výše pro konkrétní na při a jak jsou přiděleny překládat pomocí SNAT porty a toomanage tento vyčerpatelným prostředek.
