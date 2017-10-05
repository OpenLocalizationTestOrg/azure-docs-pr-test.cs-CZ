---
title: "Principy odchozí připojení v Azure | Microsoft Docs"
description: "Tento článek vysvětluje, jak Azure umožňuje virtuálním počítačům komunikovat s služby veřejného Internetu."
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
ms.openlocfilehash: 03cb14b5710b6dd17599a3c4eab21380c76c2b40
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="understanding-outbound-connections-in-azure"></a>Principy odchozích připojení v Azure

V Azure virtuální počítač (VM) může komunikovat s koncovými body mimo Azure ve veřejném adresním prostoru IP adres. Když virtuální počítač zahájí odchozího toku do cílového umístění v prostor veřejných IP adres, Azure mapuje privátní IP adresu Virtuálního počítače na veřejnou IP adresu a umožňuje připojit virtuální počítač návratový provoz.

Azure poskytuje tři různých způsobů dosažení odchozí připojení. Každá z metod má svou vlastní možnosti a omezení. Zkontrolujte jednotlivé metody pečlivě na zvolit, které z nich vyhovuje vašim potřebám.

| Scénář | Metoda | Poznámka |
| --- | --- | --- |
| Samostatný virtuální počítač (žádná služba Vyrovnávání zatížení, žádná Instance úroveň veřejná IP adresa) |Výchozí překládat pomocí SNAT |Azure přidruží veřejnou IP adresu pro překládat pomocí SNAT |
| Vyrovnávání zatížení sítě virtuálních počítačů (žádná Instance úroveň veřejná IP adresa ve virtuálním počítači) |Překládat pomocí SNAT pomocí nástroje pro vyrovnávání zatížení |Azure používá veřejnou IP adresu služby Vyrovnávání zatížení pro překládat pomocí SNAT |
| Virtuální počítač s Instance úroveň veřejnou IP adresu (s nebo bez nástroj pro vyrovnávání zatížení) |Překládat pomocí SNAT se nepoužívá. |Azure používá veřejné IP adresy přiřazené k virtuálnímu počítači |

Pokud nechcete, aby virtuální počítač ke komunikaci s koncovými body mimo Azure ve veřejném adresním prostoru IP adres, můžete použít skupiny zabezpečení sítě (NSG) zablokovat přístup. Pomocí skupin Nsg je podrobněji popsána v [brání veřejného připojení](#preventing-public-connectivity).

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Samostatný virtuální počítač s žádné Instance úroveň veřejnou IP adresu

V tomto scénáři virtuální počítač není součástí fondu vyrovnávání zatížení Azure a nemá adresu Instance úroveň veřejné IP (splnění), který je přiřazen. Když virtuální počítač se vytváří odchozího toku, překládá Azure privátní zdrojovou IP adresu odchozího toku veřejné Zdrojová IP adresa. Veřejnou IP adresu použitou pro tuto odchozího toku není Konfigurovatelný a nepočítá proti odběru limitu prostředků veřejné IP. Azure používá zdroje síťové adresy překlad (SNAT) a provést tuto funkci. Dočasné porty veřejné IP adresy se používají k rozlišení jednotlivých toky pochází ve virtuálním počítači. Překládat pomocí SNAT dynamicky přiděluje dočasné porty, když se vytváří toky. V tomto kontextu jsou dočasné porty používané pro překládat pomocí SNAT označovány jako porty překládat pomocí SNAT.

Konečný prostředek, který může dojít k vyčerpání jsou porty překládat pomocí SNAT. Je důležité pochopit, jak se spotřebovávají. Jeden port překládat pomocí SNAT obsazením za toku do jedné cílové IP adresy. Pro více toků se stejnou IP adresou cílového využívá každý tok jediný port překládat pomocí SNAT. Tím se zajistí na toky jedinečná při pocházely ze stejné veřejnou IP adresu na stejnou cílovou IP adresu. Více toků jednotliví na jinou cílovou IP adresu využívají jediný port překládat pomocí SNAT na cílový. Cílovou IP adresu provede tyto toky jedinečný.

Můžete použít [analýzy protokolů pro vyrovnávání zatížení](load-balancer-monitor-log.md) a [výstrahy protokoly událostí pro monitorování překládat pomocí SNAT portu vyčerpání zprávy](load-balancer-monitor-log.md#alert-event-log). Když vyčerpání prostředků překládat pomocí SNAT port, odchozí toky nezdaří, dokud překládat pomocí SNAT porty jsou vydal existující toky. Nástroj pro vyrovnávání zatížení používá časový limit nečinnosti 4minutové opětovného získání porty překládat pomocí SNAT.

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>Vyrovnávání zatížení sítě virtuálních počítačů s žádné Instance úroveň veřejnou IP adresu

Virtuální počítač v tomto scénáři je součástí fondu vyrovnávání zatížení Azure.  Virtuální počítač nemá veřejnou IP adresu přiřazen. Prostředek pro vyrovnávání zatížení musí být nakonfigurované na pravidlo můžete propojit veřejnou IP front-endovou s fondem back-end.  Pokud nezadáte tuto konfiguraci, chování je, jak je popsáno v předchozí části pro [samostatný virtuální počítač s žádné Instance úroveň veřejnou IP adresu](load-balancer-outbound-connections.md#standalone-vm-with-no-instance-level-public-ip-address).

Při vyrovnávání zatížení sítě virtuálního počítače vytvoří odchozí tok, znamená, že Azure privátní zdrojovou IP adresu odchozího toku veřejnou IP adresu veřejnou front-endovou Vyrovnávání zatížení. Azure používá zdroje síťové adresy překlad (SNAT) a provést tuto funkci. Dočasné porty nástroje pro vyrovnávání zatížení veřejnou IP adresu se používají k rozlišení jednotlivých toky pochází ve virtuálním počítači. Překládat pomocí SNAT dynamicky přiděluje dočasné porty, když se vytváří odchozí toky. V tomto kontextu jsou dočasné porty používané pro překládat pomocí SNAT označovány jako porty překládat pomocí SNAT.

Konečný prostředek, který může dojít k vyčerpání jsou porty překládat pomocí SNAT. Je důležité pochopit, jak se spotřebovávají. Jeden port překládat pomocí SNAT obsazením za toku do jedné cílové IP adresy. Pro více toků se stejnou IP adresou cílového využívá každý tok jediný port překládat pomocí SNAT. Tím se zajistí na toky jedinečná při pocházely ze stejné veřejnou IP adresu na stejnou cílovou IP adresu. Více toků jednotliví na jinou cílovou IP adresu využívají jediný port překládat pomocí SNAT na cílový. Cílovou IP adresu provede tyto toky jedinečný.

Můžete použít [analýzy protokolů pro vyrovnávání zatížení](load-balancer-monitor-log.md) a [výstrahy protokoly událostí pro monitorování překládat pomocí SNAT portu vyčerpání zprávy](load-balancer-monitor-log.md#alert-event-log). Když vyčerpání prostředků překládat pomocí SNAT port, odchozí toky nezdaří, dokud překládat pomocí SNAT porty jsou vydal existující toky. Nástroj pro vyrovnávání zatížení používá časový limit nečinnosti 4minutové opětovného získání porty překládat pomocí SNAT.

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>Virtuální počítač s adresu veřejná IP adresa na úrovni Instance (s nebo bez nástroj pro vyrovnávání zatížení)

V tomto scénáři má virtuální počítač instanci úroveň veřejné IP splnění přiřazen. Nezáleží, jestli je virtuální počítač nebo ne vyrovnáváním zatížení. Pokud se používá splnění, se nepoužije zdrojový síťové adresy překlad (SNAT). Virtuální počítač používá splnění pro všechny odchozí toky. Pokud vaše aplikace iniciuje mnoho odchozí toky a dochází k vyčerpání překládat pomocí SNAT, měli byste zvážit přiřazení splnění předejdete omezení překládat pomocí SNAT.

## <a name="discovering-the-public-ip-used-by-a-given-vm"></a>Zjišťování veřejné IP adresy používané daného virtuálního počítače

Existuje mnoho způsobů, jak určit IP adresu zdrojového veřejné odchozí připojení. OpenDNS poskytuje službu, která můžete zobrazit veřejnou IP adresu vašeho virtuálního počítače. Použití příkazu nslookup, můžete odeslat dotaz DNS pro název myip.opendns.com do překladače OpenDNS. Služba vrátí zdrojové IP adresy, který se používá k odeslání dotazu. Při spuštění následující dotaz z virtuálního počítače, je odpověď na veřejné IP adresy použít pro tento virtuální počítač.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Nelze navázat připojení veřejného

Někdy je žádoucí pro virtuální počítač povolit vytváření odchozího toku nebo může být požadavek na správu, které cíle je dostupný odchozí toky. V takovém případě použijete [skupiny zabezpečení sítě (NSG)](../virtual-network/virtual-networks-nsg.md) ke správě cílů, které mohou být využity virtuálního počítače. Když použijete skupinu NSG k virtuálnímu počítači s vyrovnáváním zatížení, je potřeba věnovat pozornost [výchozí značky](../virtual-network/virtual-networks-nsg.md#default-tags) a [výchozí pravidla](../virtual-network/virtual-networks-nsg.md#default-rules).

Je nutné zajistit, že virtuální počítač může přijímat žádosti o kontrolu stavu z nástroje pro vyrovnávání zatížení Azure. Pokud skupina NSG blokuje žádostí o stav testu z výchozí značka AZURE_LOADBALANCER, selže test stavu vašeho virtuálního počítače a virtuálního počítače je označena. Nástroj pro vyrovnávání zatížení zastaví odesílání nové toky do tohoto virtuálního počítače.

## <a name="limitations"></a>Omezení

Pokud [více (veřejné) IP adresy, které jsou přidružené Vyrovnávání zatížení](load-balancer-multivip-overview.md), všechny tyto veřejné IP adresy jsou kandidátem pro odchozí toky.

Azure používá nepodporovaný algoritmus můžete určit počet dostupných portů překládat pomocí SNAT na základě velikosti fondu.  To se nedá konfigurovat v tuto chvíli.

Je důležité rememember, který počet dostupných portů překládat pomocí SNAT nepřekládá přímo k počtu připojení. Podrobnosti viz výše pro konkrétní na kdy a jak jsou přiděleny překládat pomocí SNAT porty a jak spravovat tento vyčerpatelným prostředek.
