---
title: "aaaOverview IPv6 pro vyrovnávání zatížení Azure | Microsoft Docs"
description: "Principy podporu IPv6 pro vyrovnávání zatížení Azure a virtuálních počítačů s vyrovnáváním zatížení."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: "protokol IPv6, nástroje pro vyrovnávání zatížení azure, duálním zásobníkem, veřejnou IP adresu, nativní protokol ipv6, mobilní, iot"
ms.assetid: 6a1d583f-a305-40fd-a94b-fa42e1943bbb
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
ms.openlocfilehash: 5b203f77d86cc1ad455f4ebb297097aef46b658d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Přehled protokolu IPv6 pro vyrovnávání zatížení Azure

Internetové služby Vyrovnávání zatížení můžete nasadit pomocí adresy IPv6. Kromě toho tooIPv4 připojení, to umožňuje hello následující možnosti:

* Nativní začátku do konce IPv6 připojení mezi klienty veřejného Internetu a virtuální počítače (VM) Azure prostřednictvím hello nástroj pro vyrovnávání zatížení.
* Nativní klient server IPv6 odchozí připojení mezi virtuální počítače a veřejné klienty používající Internetu IPv6.

Hello následující obrázek znázorňuje hello IPv6 funkce nástroje pro vyrovnávání zatížení Azure.

![Nástroje pro vyrovnávání zatížení Azure s IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Po nasazení klientem IPv4 nebo IPv6 povolené Internetu může komunikovat s hello veřejné adresy IPv4 nebo IPv6 (nebo názvy hostitelů) hello nástroj pro vyrovnávání zatížení Azure internetové. směrování pro vyrovnávání zatížení Hello hello pakety toohello privátní IPv6 adresy IPv6 hello virtuálních počítačů pomocí překlad síťových adres (NAT). Hello IPv6 Internetový klient nemůže komunikovat přímo se hello IPv6 adresa hello virtuálních počítačů.

## <a name="features"></a>Funkce

Poskytuje nativní podporu IPv6 pro virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager:

1. Vyrovnávání zatížení sítě IPv6 služby klientům IPv6 na hello Internetu
2. Nativní protokol IPv6 a IPv4 koncových bodů na virtuálních počítačích ("dva skládaný")
3. Příchozí a odchozí spouštěná nativní IPv6 připojení
4. Podporované protokoly, jako je například TCP, UDP a HTTP (S) povolte celou řadu architektury služby

## <a name="benefits"></a>Výhody

Tato funkce umožňuje hello následující klíčové výhody:

* Splňují předpisy government vyžadující, aby se nové aplikace přístupné pouze tooIPv6 klienty
* Povolit mobilní a Internet věcí (IOT) vývojáři toouse skládaný duální virtuální počítače Azure (IPv4 + IPv6) tooaddress hello rostoucí mobile & trhů IOT

## <a name="details-and-limitations"></a>Podrobnosti a omezení

Podrobnosti

* Hello služba Azure DNS obsahuje záznamy, název IPv4 A i IPv6 AAAA a odpoví oba záznamy pro vyrovnávání zatížení hello. Klient Hello zvolí které toocommunicate adresy (IPv4 nebo IPv6) s.
* Když virtuální počítač zahájí připojení tooa veřejné připojení Internetu IPv6 zařízení, je hello Virtuálního počítače zdrojovou adresu IPv6 přeložit síťových adres (NAT) toohello veřejnou IPv6 adresu služby Vyrovnávání zatížení hello.
* Virtuální počítače s operačním systémem Linux hello musí být nakonfigurované tooreceive IPv6 IP adresu prostřednictvím DHCP. Řadu hello Linux obrázků v hello Galerie Azure jsou již nakonfigurována toosupport IPv6 bez úprav. Další informace najdete v tématu [DHCPv6 konfiguraci pro virtuální počítače s Linuxem](load-balancer-ipv6-for-linux.md)
* Pokud se rozhodnete toouse stavu testu se nástroj pro vyrovnávání zatížení, vytvořit test paměti IPv4 a jeho použití s hello IPv4 i IPv6 koncové body. Pokud služba hello na virtuální počítač přestane fungovat, hello IPv4 i IPv6 koncové body se vyjímají z otočení.

Omezení

* Nelze přidat pravidla Vyrovnávání zatížení IPv6 v hello portálu Azure. Hello pravidla vytvářet jenom pomocí hello šablony, rozhraní příkazového řádku, prostředí PowerShell.
* Nemusí upgradovat stávající virtuální počítače toouse IPv6 adresy. Je nutné nasadit nové virtuální počítače.
* Jedna adresa IPv6 je možné přiřadit tooa jedno síťové rozhraní v jednotlivých virtuálních počítačů.
* veřejné adresy IPv6 Hello nelze přiřadit tooa virtuálních počítačů. Jejich lze přiřadit pouze tooa nástroj pro vyrovnávání zatížení.
* Nelze nakonfigurovat hello zpětného vyhledávání DNS pro vaše veřejné adresy IPv6.
* Hello virtuálních počítačů s adresami IPv6 hello nemůžou být členy skupiny cloudové služby Azure. Mohou být připojené tooan Azure Virtual Network (VNet) a vzájemně komunikovat prostřednictvím jejich adresy IPv4.
* Privátní IPv6 adresy můžou být nasazené na jednotlivé virtuální počítače ve skupině prostředků, ale nelze nasadit do skupiny prostředků prostřednictvím sady škálování.
* Virtuální počítače Azure se nelze připojit pomocí IPv6 tooother virtuální počítače, další služby Azure nebo místní zařízení. Přes protokol IPv6 mohou komunikovat pouze pro vyrovnávání zatížení Azure hello. Však může komunikovat s tyto prostředky, které pomocí protokolu IPv4.
* Skupina zabezpečení sítě (NSG) ochrany pro protokol IPv4 je podporována v nasazeních duální sada protokolů (IPv4 + IPv6). Skupiny Nsg se nevztahují toohello IPv6 koncové body.
* koncový bod Hello IPv6 na virtuální počítač není hello zveřejněné přímo toohello Internetu. Je za službou Vyrovnávání zatížení. Pouze hello portů zadaných v hello pravidla nástroje pro vyrovnávání zatížení jsou přístupné přes protokol IPv6.
* Změna hello IdleTimeout parametr pro protokol IPv6 je **aktuálně není podporována**. Hello výchozí hodnota je 4 minut.
* Změna hello loadDistributionMethod parametr pro protokol IPv6 je **aktuálně není podporována**.
* Vyhrazené IP adresy IPv6 (kde IPAllocationMethod = statické) jsou **aktuálně není podporována**.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toodeploy Vyrovnávání zatížení s IPv6.

* [Dostupnost podle oblasti IPv6](https://go.microsoft.com/fwlink/?linkid=828357)
* [Nasazení Vyrovnávání zatížení s IPv6 pomocí šablony](load-balancer-ipv6-internet-template.md)
* [Nasazení Vyrovnávání zatížení s IPv6 pomocí Azure PowerShell](load-balancer-ipv6-internet-ps.md)
* [Nasazení Vyrovnávání zatížení s IPv6 pomocí rozhraní příkazového řádku Azure](load-balancer-ipv6-internet-cli.md)
