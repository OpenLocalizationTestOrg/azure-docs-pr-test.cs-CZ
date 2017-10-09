---
title: "aaaResolution pro virtuální počítače a instance rolí"
description: "Název řešení scénáře pro Azure IaaS, hybridní řešení, která mezi jiné cloudové služby, služby Active Directory a pomocí serveru DNS "
services: virtual-network
documentationcenter: na
author: GarethBradshawMSFT
manager: carmonm
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2016
ms.author: telmos
ms.openlocfilehash: 0ec7903cf200c1d04d75601a5b0cefe4268f3dcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="name-resolution-for-vms-and-role-instances"></a>Překlad názvů pro instance rolí a VM
V závislosti na tom, jak používáte Azure toohost IaaS a PaaS, hybridní řešení může být nutné tooallow hello virtuálních počítačů a instancí rolí vytvoříte toocommunicate mezi sebou. I když tato komunikace lze provést pomocí IP adresy, je mnohem jednodušší toouse názvy, které můžete snadno zapamatovaných a se nemění. 

Pokud instance rolí a virtuální počítače hostované v Azure potřebovat tooresolve domény názvy toointernal IP adresy, mohou používat jednu ze dvou metod:

* [Rozlišení názvů Azure](#azure-provided-name-resolution)
* [Překlad názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server) (které může předávat dotazy toohello serverů DNS poskytnutých platformou Azure) 

Typ Hello překladu adres, které můžete použít závisí na tom, jak vaše virtuální počítače a instance rolí musí toocommunicate mezi sebou.

**Hello následující tabulka uvádí scénáře a odpovídající název řešení řešení:**

| **Scénář** | **Řešení** | **Přípona** |
| --- | --- | --- |
| Překlad mezi instancí role nebo virtuální počítače umístěné v hello stejné cloudové služby nebo virtuální sítě |[Rozlišení názvů Azure](#azure-provided-name-resolution) |název hostitele nebo plně kvalifikovaný název domény |
| Překlad mezi instancí role nebo umístěné v různých virtuálních sítích virtuálních počítačů |Spravované zákazníkem servery DNS předávání dotazů mezi virtuálními sítěmi pro řešení Azure (DNS proxy).  V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server) |Pouze plně kvalifikovaný název domény |
| Překlad názvu počítače a služby v místě z instancí role nebo virtuálních počítačů v Azure |Spravované zákazníkem servery DNS (např. místní řadič domény, řadiče místní domény jen pro čtení nebo sekundární DNS synchronizovat pomocí přenosy zóny).  V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server) |Pouze plně kvalifikovaný název domény |
| Řešení Azure názvy hostitelů z místního počítače |Předávaly dotazy tooa spravované zákazníkem proxy server DNS v hello odpovídající virtuální síť, předá hello proxy server tooAzure dotazy pro překlad. V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server) |Pouze plně kvalifikovaný název domény |
| Reverse DNS pro interní IP adresy |[Překlad názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server) |neuvedeno |
| Překlad mezi virtuální počítače nebo instance rolí, které jsou umístěné v různých cloudové služby, není ve virtuální síti |Není k dispozici. Připojení mezi virtuální počítače a instance rolí v různých cloudové služby není podporované mimo virtuální síť. |neuvedeno |

## <a name="azure-provided-name-resolution"></a>Rozlišení názvů Azure
Společně s překladu názvů DNS veřejné Azure poskytuje interní překlad adres pro virtuální počítače a instance rolí, které se nacházejí v hello stejné virtuální síti nebo cloudovou službu.  Virtuální počítače nebo instance v cloudové službě sdílejí hello stejné DNS přípona (takže bude stačit hello hostname samostatně), ale v různých cloudové služby klasické virtuální sítě mají různé přípony DNS, takže hello plně kvalifikovaný název domény je potřebné tooresolve názvy různých cloudových služeb.  Ve virtuálních sítích v modelu nasazení Resource Manager hello hello přípona DNS je konzistentní napříč hello virtuální sítě (takže hello plně kvalifikovaný název domény není nezbytný) a názvy DNS se dá přiřadit tooboth síťové karty a virtuální počítače. I když Azure překlad nevyžaduje žádnou konfiguraci, není hello příslušnou volbu pro všechny scénáře nasazení, jak je vidět na výše uvedené tabulce hello.

> [!NOTE]
> V případě hello webových a pracovních rolí můžete taky přejít hello interní IP adresy instancí role podle hello role název a číslo instance pomocí hello Azure Service Management REST API. Další informace najdete v tématu [služby referenční dokumentace rozhraní API REST správy](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features-and-considerations"></a>Funkce a důležité informace
**Funkce:**

* Snadné použití: v pořadí toouse Azure překlad není nutná žádná konfigurace.
* služba překladu názvu Hello pro Azure je vysoce dostupný, ukládá hello potřebovat toocreate a spravovat clustery serverů DNS.
* Můžete použít ve spojení s vlastními tooresolve servery DNS místní i Azure názvy hostitelů.
* Rozlišení názvů se mezi instancí role virtuálních počítačů v rámci hello stejné cloudové služby bez nutnosti plně kvalifikovaný název domény.
* Název řešení je k dispozici mezi virtuálními počítači ve virtuálních sítích, které používají hello modelu nasazení Resource Manager, bez nutnosti hello plně kvalifikovaný název domény. Při překladu názvů v různých cloudové služby, virtuálních sítí v modelu nasazení classic hello vyžadují hello plně kvalifikovaný název domény. 
* Názvy hostitelů, které nejlépe popisují vaše nasazení, můžete místo práce s automaticky generovaných názvů.

**Aspekty:**

* přípona DNS vytvořit Azure Hello nemůže být upraven.
* Nelze ručně zaregistrovat vlastní záznamy.
* Služba WINS a NetBIOS nejsou podporovány. (Virtuální počítače v Průzkumníku Windows nejde zobrazit.)
* Názvy hostitelů musí být kompatibilní s DNS (uživatel musí použít jenom 0 – 9, a – z a '-' a nesmí začínat ani končit '-'. Viz část 3696 RFC 2.)
* Pro každý virtuální počítač je omezen provoz dotazu DNS. To by neměla mít vliv na většinu aplikací.  Pokud je dodržena omezení požadavků, ujistěte se, zda je povoleno ukládání do mezipaměti na straně klienta.  Další podrobnosti najdete v tématu [získávání hello většina z Azure překlad](#Getting-the-most-from-Azure-provided-name-resolution).
* Pouze virtuální počítače v hello první 180 cloudové služby jsou registrované pro každou virtuální síť v modelu nasazení classic. To neplatí toovirtual sítí v modelu nasazení Resource Manager.

### <a name="getting-hello-most-from-azure-provided-name-resolution"></a>Získávání hello většina z Azure překlad
**Ukládání do mezipaměti na straně klienta:**

Ne každý dotaz DNS musí toobe odesílají přes síť hello.  Ukládání do mezipaměti na straně klienta pomáhá snížit latenci a zlepšit odolnost toonetwork blips vyřešte opakující se dotazy DNS z místní mezipaměti.  Záznamy DNS obsahovat Time To Live (TTL) umožňující hello mezipaměti toostore hello záznam pro stejně dlouho bez dopadu záznamů aktuálnosti tak ukládání do mezipaměti na straně klienta je vhodná pro většinu situacích.

Výchozí Hello klienta DNS systému Windows má integrovanou mezipaměť DNS.  Některé Linux distribucích nezahrnují ukládání do mezipaměti ve výchozím nastavení, je doporučeno, jeden přidat tooeach virtuálního počítače s Linuxem (po kontrole, zda není k dispozici místní mezipaměti již).

Existuje několik různých DNS ukládání do mezipaměti balíčků dostupné, například dnsmasq, zde jsou dnsmasq tooinstall hello kroků na nejběžnější distribucích hello:

* **Ubuntu (používá resolvconf)**:
  * Stačí nainstalujte balíček dnsmasq hello ("sudo instalace výstižný get dnsmasq").
* **SUSE (používá netconf)**:
  * instalovat balíček dnsmasq hello ("sudo zypper instalace dnsmasq") 
  * Povolení služby dnsmasq hello ("Povolit dnsmasq.service systemctl") 
  * Spusťte službu dnsmasq hello ("systemctl počáteční dnsmasq.service") 
  * Upravit "/ atd/sysconfig/síť/config" a změňte NETCONFIG_DNS_FORWARDER = "" příliš "dnsmasq"
  * aktualizace resolv.conf ("netconfig update") tooset hello mezipaměti jako hello místní překladač služby DNS
* **OpenLogic (používá NetworkManager)**:
  * instalovat balíček dnsmasq hello ("sudo yum instalace dnsmasq")
  * Povolení služby dnsmasq hello ("Povolit dnsmasq.service systemctl")
  * Spusťte službu dnsmasq hello ("systemctl počáteční dnsmasq.service")
  * Přidání "předřazení domény názvové servery 127.0.0.1;" too"/etc/dhclient-eth0.conf"
  * Restartujte hello síťové služby ("sítě restartování služby") tooset hello mezipaměti jako hello místní překladač služby DNS

> [!NOTE]
> balíček 'dnsmasq' Hello je pouze jeden z hello mnoho mezipaměti DNS k dispozici pro Linux.  Před jeho použitím, zkontrolujte jeho vhodnosti pro konkrétních potřeb a žádné jiné mezipaměti je nainstalovaná.
> 
> 

**Opakování na straně klienta:**

DNS je primárně protokol UDP.  Jako hello protokolu UDP nezaručuje doručení zpráv, je logika opakovaných pokusů zpracována v samotný protokol DNS hello.  Každý klient DNS (operačního systému) může vykazovat různé opakování logiku v závislosti na předvoleb creators hello:

* Operační systémy Windows opakujte po 1 druhý a pak znovu po jiné 2, 4 a jiné 4 sekundy. 
* Hello výchozí Linux instalace opakování po 5 sekund.  Je doporučeno toochange tento tooretry 5krát v 1 druhý intervalech.  

toocheck hello aktuální nastavení na virtuální počítač s Linuxem, 'cat /etc/resolv.conf' a podívejte se na řádku "možnosti" hello, například:

    options timeout:1 attempts:5

soubor resolv.conf Hello je obvykle automaticky generovaný a by neměla být upravována.  Hello konkrétní kroky pro přidání řádku hello možnosti se liší podle distro:

* **Ubuntu** (používá resolvconf):
  * Přidat too'/etc/resolveconf/resolv.conf.d/head řádku možnosti hello. 
  * Spustit tooupdate 'resolvconf -u.
* **SUSE** (používá netconf):
  * Přidejte 'timeout:1 pokusů: 5' toohello NETCONFIG_DNS_RESOLVER_OPTIONS = "" parametr v/atd/sysconfig nebo sítě nebo konfigurace 
  * Spustit tooupdate 'netconfig aktualizace.
* **OpenLogic** (používá NetworkManager):
  * Přidat too'/etc/NetworkManager/dispatcher.d/11-dhclient "echo" možnosti timeout:1 pokusů: 5"." 
  * Spustit tooupdate, restart služby sítě.

## <a name="name-resolution-using-your-own-dns-server"></a>Překlad názvů pomocí serveru DNS
Existuje několik situací, kde potřeb název řešení může přejít nad rámec hello funkce poskytované službou Azure, například při použití domén služby Active Directory, nebo pokud vyžadují překlad DNS mezi virtuálními sítěmi (virtuální sítě).  toocover tyto scénáře Azure poskytuje schopnost hello jste toouse vlastní servery DNS.  

Servery DNS v rámci virtuální sítě může předat tooAzure dotazy DNS rekurzivní překladače tooresolve názvy hostitelů v rámci této virtuální sítě.  Řadiči domény (DC) běží v Azure můžete například reagovat tooDNS dotazy pro jeho domény a předávat všechny ostatní tooAzure dotazy.  To umožňuje virtuálním počítačům toosee místních prostředků (prostřednictvím hello řadiče domény) a Azure názvy hostitelů (prostřednictvím předávání hello).  Přístup tooAzure rekurzivní překladače je k dispozici prostřednictvím hello virtuální IP adresy 168.63.129.16.

Předávání DNS také umožňuje překlad DNS mezi virtuálními a umožňuje vaší místní počítače tooresolve Azure názvy hostitelů.  V pořadí tooresolve název hostitele Virtuálního počítače, server DNS hello virtuální počítač se musí nacházet v hello stejné virtuální sítě a být tooAzure dotazy tooforward nakonfigurovaný název hostitele.  Jako přípona DNS hello se liší v každé virtuální síť, můžete použít podmíněné předávání pravidla toosend DNS dotazuje toohello opravte virtuální sítě pro překlad.  Hello následující obrázek ukazuje dvě virtuální sítě a k místní síti provádění překlad DNS mimo virtuální síť pomocí této metody.  Předávání DNS příklad je k dispozici v hello [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) a [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Mezi virtuálními DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Při použití Azure překlad IP adres příponu interní DNS (*. internal.cloudapp.net) je zadaná tooeach virtuálních počítačů pomocí systému DHCP.  To umožňuje rozlišování názvu hostitele jako název hostitele hello, které jsou záznamy v zóně internal.cloudapp.net hello.  Pokud používáte vlastní název řešení řešení, zadat hello IDN přípona není tooVMs, protože ji naruší jiné DNS architektury (jako je připojený k doméně scénáře).  Místo toho jsme představují zástupce nefunkční (reddog.microsoft.com).  

V případě potřeby příponu DNS interní hello se dá určit pomocí rozhraní API prostředí PowerShell nebo hello:

* Pro virtuální sítě v modelech nasazení Resource Manager, je k dispozici prostřednictvím hello hello příponu [karty síťového rozhraní](https://msdn.microsoft.com/library/azure/mt163668.aspx) prostředků nebo prostřednictvím hello [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) rutiny.    
* Ve model nasazení classic, je k dispozici prostřednictvím hello hello příponu [získat rozhraní API nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) volání nebo prostřednictvím hello [Get-AzureVM – ladění](https://msdn.microsoft.com/library/azure/dn495236.aspx) rutiny.

Pokud předávání dotazy tooAzure nebude vyhovovat vašim potřebám, budete potřebovat tooprovide řešení DNS.  Bude potřeba řešení DNS:

* Zadejte odpovídající název hostitele řešení, například prostřednictvím [DDNS](virtual-networks-name-resolution-ddns.md).  Všimněte si, pokud pomocí DDNS může být nutné toodisable DNS úklid záznamů o zapůjčení DHCP Azure a jsou velmi dlouhé a proces úklidu může odebrat DNS zaznamenává předčasně. 
* Zadejte odpovídající rekurzivní překlad názvů tooallow rozlišení názvů externí domény.
* Být přístupné (TCP a UDP na port 53) z klientů hello slouží a být schopný tooaccess hello Internetu.
* Nutné zabezpečit před přístupem z hello Internetu, toomitigate hrozbami v podobě externí agenty.

> [!NOTE]
> Pro nejlepší výkon, při použití virtuálních počítačích Azure jako servery DNS, by mělo být zakázáno IPv6 a [veřejná IP adresa na úrovni Instance](virtual-networks-instance-level-public-ip.md) by měla být přiřazená serveru DNS tooeach virtuálních počítačů.  Pokud se rozhodnete toouse systému Windows Server jako DNS server, [v tomto článku](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) poskytuje další výkonu analýzy a optimalizace.
> 
> 

### <a name="specifying-dns-servers"></a>Určení serverů DNS
Pokud používáte vlastní servery DNS, Azure poskytuje možnost toospecify hello více serverů DNS na jednu virtuální síť nebo na jednu síť rozhraní (Resource Manager) nebo cloudové služby (klasické).  Servery DNS zadané pro cloudové služby nebo síťová rozhraní získat přednost přes uvedenými hello virtuální sítě.

> [!NOTE]
> Vlastnosti síťového připojení, například server DNS IP adresy, by neměla být upravována přímo v rámci virtuálních počítačů Windows může získat vymazat během služby retušovat při získá hello virtuální síťový adaptér nahradit. 
> 
> 

Pokud pomocí modelu nasazení Resource Manager hello, servery DNS může být zadaná v hello portálu, rozhraní API nebo šablony ([virtuální síť](https://msdn.microsoft.com/library/azure/mt163661.aspx), [seskupování](https://msdn.microsoft.com/library/azure/mt163668.aspx)) nebo prostředí PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [seskupování](https://msdn.microsoft.com/library/mt619370.aspx)).

Při použití modelu nasazení classic hello, servery DNS pro virtuální síť hello lze zadat v hello portál nebo [hello *konfigurace sítě* soubor](https://msdn.microsoft.com/library/azure/jj157100).  Pro cloudové služby, jsou určené servery DNS hello prostřednictvím [hello *konfigurace služby* soubor](https://msdn.microsoft.com/library/azure/ee758710) nebo v prostředí PowerShell ([New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [!NOTE]
> Pokud změníte hello nastavení DNS pro virtuální sítě nebo virtuální počítač, který je už nasazená, je třeba toorestart každý ovlivněné virtuální počítač pro hello změny tootake vliv.
> 
> 

## <a name="next-steps"></a>Další kroky
Model nasazení Resource Manager:

* [Vytvořit nebo aktualizovat virtuální síť](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Vytvořit nebo aktualizovat karty síťového rozhraní](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [Nový AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
* [Nové AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

Model nasazení Classic:

* [Schéma konfigurace služby Azure](https://msdn.microsoft.com/library/azure/ee758710)
* [Schéma konfigurace virtuální sítě](https://msdn.microsoft.com/library/azure/jj157100)
* [Konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md) 

