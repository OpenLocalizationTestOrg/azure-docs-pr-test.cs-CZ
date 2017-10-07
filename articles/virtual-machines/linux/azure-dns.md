---
title: "aaaDNS možnosti překladu pro virtuální počítače s Linuxem v Azure"
description: "Název služby DNS scénáře pro virtuální počítače s Linuxem v Azure IaaS, včetně poskytuje řešení, hybridní externí DNS a přineste si vlastní DNS server."
services: virtual-machines
documentationcenter: na
author: RicksterCDN
manager: timlt
editor: tysonn
ms.assetid: 787a1e04-cebf-4122-a1b4-1fcf0a2bbf5f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2016
ms.author: rclaus
ms.openlocfilehash: 7df2320b6f6b42b479bf4070ea60fe5770a78a41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dns-name-resolution-options-for-linux-virtual-machines-in-azure"></a>Možnosti překlad názvů DNS pro virtuální počítače s Linuxem v Azure
Azure poskytuje překlad názvu DNS ve výchozím nastavení pro všechny virtuální počítače, které se nacházejí v jedné virtuální sítě. Vlastní řešení rozlišení názvu DNS můžete implementovat podle konfigurace služby DNS pro vaše virtuální počítače Azure který je hostitelem. Hello následující scénáře by vám pomohou zvolit ten, který se dá použít pro vaši situaci hello.

* [Rozlišení názvů, které poskytuje Azure](#azure-provided-name-resolution)
* [Překlad názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server)

Typ Hello překlad, který používáte závisí na tom, jak vaše virtuální počítače a instance rolí musí toocommunicate mezi sebou.

Hello následující tabulka uvádí scénáře a odpovídající název řešení řešení:

| **Scénář** | **Řešení** | **Přípona** |
| --- | --- | --- |
| Překlad mezi instance rolí virtuálních počítačů v hello stejné virtuální síti |[Rozlišení názvů, které poskytuje Azure](#azure-provided-name-resolution) |název hostitele nebo plně kvalifikovaný název domény (FQDN) |
| Překlad mezi instance rolí virtuálních počítačů v různých virtuálních sítích |Spravované zákazníkem serverů DNS, které předávání dotazů mezi virtuálními sítěmi pro řešení Azure (DNS proxy). V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server). |Pouze plně kvalifikovaný název domény |
| Řešení místní počítačů a názvy služby z role instance nebo virtuálních počítačů v Azure |Spravované zákazníkem servery DNS (například místní řadič domény, řadiče místní domény jen pro čtení nebo sekundární DNS synchronizovat pomocí přenosy zóny). V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server). |Pouze plně kvalifikovaný název domény |
| Řešení Azure názvy hostitelů z místního počítače |Předávání dotazů tooa spravované zákazníkem proxy server DNS v hello odpovídající virtuální síť. Hello proxy server předává tooAzure dotazy pro překlad. V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server). |Pouze plně kvalifikovaný název domény |
| Reverse DNS pro interní IP adresy |[Překlad názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server) |neuvedeno |

## <a name="name-resolution-that-azure-provides"></a>Rozlišení názvů, které poskytuje Azure
Společně s překladu názvů DNS veřejné Azure poskytuje interní překlad adres pro virtuální počítače a instance rolí, které jsou v hello stejné virtuální síti. Ve virtuálních sítích, které jsou založeny na Azure Resource Manager hello přípona DNS je konzistentní napříč hello virtuální sítě; Hello plně kvalifikovaný název domény není potřeba. Názvy DNS se dá přiřadit tooboth karty síťového rozhraní (NIC) a virtuální počítače. I když hello překladu, který poskytuje Azure nevyžaduje žádnou konfiguraci, není hello příslušnou volbu pro všechny scénáře nasazení, jak vidíte v předcházející tabulce hello.

### <a name="features-and-considerations"></a>Funkce a důležité informace
**Funkce:**

* Překlad názvu požadované toouse, které Azure poskytuje není žádná konfigurace.
* Hello název řešení služba, která poskytuje Azure je vysoce dostupný. Nebudete potřebovat toocreate a spravovat clustery serverů DNS.
* Hello název řešení služba, která poskytuje Azure můžete použít společně s vlastními tooresolve servery DNS místní i Azure názvy hostitelů.
* Název řešení je k dispozici mezi virtuálními počítači ve virtuálních sítích bez nutnosti hello plně kvalifikovaný název domény.
* Můžete použít názvy hostitelů, které nejlépe popisují vaše nasazení místo práce s automaticky generovaných názvů.

**Aspekty:**

* nemůže být upraven Hello příponu DNS, který vytvoří Azure.
* Nelze ručně zaregistrovat vlastní záznamy.
* Služba WINS a NetBIOS nejsou podporovány.
* Názvy hostitelů musí být kompatibilní s DNS.
    Názvy musí používat pouze 0 – 9,-z, a '-', a nesmí začínat ani končit '-'. Najdete v dokumentu RFC 3696 části 2.
* Pro každý virtuální počítač je omezen provoz dotazu DNS. Omezení by nemělo mít vliv na většinu aplikací.  Pokud je dodržena omezení požadavků, ujistěte se, zda je povoleno ukládání do mezipaměti na straně klienta.  Další informace najdete v tématu [získávání hello většina rozlišování názvů, které Azure poskytuje](#getting-the-most-from-name-resolution-that-azure-provides).

### <a name="getting-hello-most-from-name-resolution-that-azure-provides"></a>Získávání hello většina rozlišování názvů, které poskytuje Azure
**Ukládání do mezipaměti klienta:**

Některé dotazy DNS se neodesílají v síti hello. Ukládání do mezipaměti na straně klienta pomáhá snížit latenci a zlepšit odolnost toonetwork nekonzistence vyřešte opakující se dotazy DNS z místní mezipaměti. Záznamy DNS obsahovat Time To Live (TTL), která umožňuje hello mezipaměti toostore hello záznam pro stejně dlouho bez dopadu na záznamů aktuálnosti. V důsledku toho ukládání do mezipaměti na straně klienta je vhodná pro většinu situace.

Některé Linuxových distribucích nezahrnují ukládání do mezipaměti ve výchozím nastavení. Doporučujeme, abyste po je zkontrolovat, že není k dispozici místní mezipaměti již přidání mezipaměti tooeach Linux virtuálního počítače.

Několik různých DNS ukládání do mezipaměti balíčků, jako je například dnsmasq, jsou k dispozici. Zde jsou dnsmasq tooinstall hello kroků na nejběžnější distribuce hello:

**Ubuntu (používá resolvconf)**
  * Instalovat balíček dnsmasq hello ("sudo instalace výstižný get dnsmasq").

**SUSE (používá netconf)**:
1. Instalovat balíček dnsmasq hello ("sudo zypper instalace dnsmasq").
2. Povolte službu dnsmasq hello ("Povolit dnsmasq.service systemctl").
3. Spusťte službu dnsmasq hello ("systemctl počáteční dnsmasq.service").
4. Upravit "/ atd/sysconfig/síť/config" a změňte NETCONFIG_DNS_FORWARDER = "" příliš "dnsmasq".
5. Aktualizace resolv.conf ("netconfig update") tooset hello mezipaměti jako hello místní překladač služby DNS.

**CentOS softwarem podvodný Wave (dříve OpenLogic; používá NetworkManager)**
1. Instalovat balíček dnsmasq hello ("sudo yum instalace dnsmasq").
2. Povolte službu dnsmasq hello ("Povolit dnsmasq.service systemctl").
3. Spusťte službu dnsmasq hello ("systemctl počáteční dnsmasq.service").
4. Přidání "předřazení domény názvové servery 127.0.0.1;" too"/etc/dhclient-eth0.conf".
5. Restartujte hello síťové služby ("sítě restartování služby") tooset hello mezipaměti jako hello místní překladač služby DNS

> [!NOTE]
> : balíček 'dnsmasq' hello je pouze jeden z hello mnoho DNS ukládá do mezipaměti, které jsou k dispozici pro Linux. Dříve, než ho použijete, zkontrolujte jeho vhodnosti pro vaše potřeby a žádné jiné mezipaměti nainstalovaného.
>
>

**Opakování na straně klienta**

DNS je primárně protokol UDP. Protože hello protokolu UDP nezaručuje doručení zpráv, hello samotný protokol DNS zpracovává logiku opakovaných pokusů. Každý klient DNS (operačního systému) může vykazovat různé opakování logiku v závislosti na předvoleb hello creator:

* Operační systémy Windows opakujte po jeden druhý a pak znovu po další dva, čtyři a jiné čtyři sekund.
* Hello výchozí Linux instalace opakování po pět sekund.  Měli byste změnit toto tooretry pětkrát v intervalech jednu sekundu.  

toocheck hello aktuální nastavení na virtuální počítač s Linuxem, 'cat /etc/resolv.conf' a podívejte se na řádku "možnosti" hello, například:

    options timeout:1 attempts:5

soubor resolv.conf Hello se generuje automaticky a by neměla být upravována. konkrétní kroky, které přidat hello položku Možnosti' řádek se liší podle distribuční Hello:

**Ubuntu** (používá resolvconf)
1. Přidat too'/etc/resolveconf/resolv.conf.d/head řádku možnosti hello ".
2. Spusťte tooupdate "resolvconf -u".

**SUSE** (používá netconf)
1. Přidejte 'timeout:1 pokusů: 5' toohello NETCONFIG_DNS_RESOLVER_OPTIONS = "" parametr v/atd/sysconfig nebo sítě nebo konfigurace.
2. Spusťte tooupdate 'netconfig aktualizace'.

**CentOS softwarem Wave podvodný (dříve OpenLogic)** (používá NetworkManager)
1. Přidat too'/etc/NetworkManager/dispatcher.d/11-dhclient "echo" možnosti timeout:1 pokusů: 5",".
2. Spusťte tooupdate, restart služby sítě".

## <a name="name-resolution-using-your-own-dns-server"></a>Překlad názvů pomocí serveru DNS
Potřeb název řešení může přesahovat hello funkce, které poskytuje Azure. Například může vyžadovat překlad DNS mezi virtuálními sítěmi. toocover v tomto scénáři můžete použít vlastní servery DNS.  

Servery DNS v rámci virtuální sítě může předat dál DNS dotazuje toorecursive překladače z Azure tooresolve názvy hostitelů, které jsou v hello stejné virtuální síti. Server DNS, který běží v Azure můžete například reagovat tooDNS dotazy pro vlastní DNS zóny soubory a předávat všechny ostatní tooAzure dotazy. Tato funkce umožňuje toosee virtuální počítače, vaše záznamy v zóně soubory a názvy hostitelů, které Azure poskytuje (prostřednictvím předávání hello). Přístup k toohello rekurzivní překladače Azure je k dispozici prostřednictvím hello virtuální IP adresy 168.63.129.16.

Předávání DNS také umožňuje překlad DNS mezi virtuálními sítěmi a umožňuje vaší místní počítače tooresolve názvy hostitelů, které poskytuje Azure. tooresolve název hostitele virtuálního počítače, virtuální počítač serveru DNS hello musí nacházet v hello stejné virtuální síti a mít nakonfigurované tooforward hostname dotazy tooAzure. Protože hello přípona DNS se liší v každé virtuální sítě, můžete použít podmíněné předávání pravidla toosend DNS dotazuje toohello opravte virtuální sítě pro překlad. Hello následující obrázek ukazuje dvě virtuální sítě a k místní síti provádění překlad DNS mezi virtuálními sítěmi pomocí této metody:

![Překlad DNS mezi virtuálními sítěmi](./media/azure-dns/inter-vnet-dns.png)

Při použití rozlišení názvů, které Azure poskytuje interní příponu DNS hello zajišťuje tooeach virtuálního počítače pomocí systému DHCP. Pokud použijete vlastní název řešení řešení, tuto příponu není zadaná toovirtual počítače protože hello příponu naruší jiné DNS architektury. toorefer toomachines ve plně kvalifikovaný název domény nebo tooconfigure příponu hello na virtuální počítače, můžete použít PowerShell nebo hello příponu hello toodetermine rozhraní API:

* Pro virtuální sítě, které jsou spravovány nástrojem Azure Resource Manager, je k dispozici prostřednictvím hello hello příponu [karty síťového rozhraní](https://msdn.microsoft.com/library/azure/mt163668.aspx) prostředků. Můžete taky spustit hello `azure network public-ip show <resource group> <pip name>` příkaz toodisplay hello podrobnosti o vaší veřejné IP adresy, která zahrnuje hello plně kvalifikovaný název domény hello síťový adaptér.

Pokud předávání dotazy tooAzure nebude vyhovovat vašim potřebám, musíte tooprovide řešení DNS.  Musí vaše řešení DNS:

* Zadejte odpovídající název hostitele řešení, například prostřednictvím [DDNS](../../virtual-network/virtual-networks-name-resolution-ddns.md). Pokud používáte DDNS, bude pravděpodobně nutné, proces úklidu záznamů DNS toodisable. Zapůjčení DHCP ve službě Azure jsou velmi dlouhé a proces úklidu může odebrat záznamy DNS předčasně.
* Zadejte odpovídající rekurzivní překlad názvů tooallow rozlišení názvů externí domény.
* Být přístupné (TCP a UDP na port 53) z klientů hello, kterému slouží a být schopný tooaccess hello Internetu.
* Nutné zabezpečit před přístupem z hello Internet toomitigate hrozbami v podobě externí agenty.

> [!NOTE]
> Pro nejlepší výkon, pokud používáte virtuální počítače v Azure DNS servery, zakažte IPv6 a přiřadit [veřejná IP adresa na úrovni Instance](../../virtual-network/virtual-networks-instance-level-public-ip.md) tooeach DNS serveru virtuálního počítače.  
>
>
