---
title: "aaaHigh dostupnost a zotavení po havárii pro SQL Server | Microsoft Docs"
description: "Diskuzi o hello různých typů HADR strategie pro SQL Server běžící v Azure Virtual Machines."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: mikeray
ms.openlocfilehash: 2b62d8b30520952ba6b7da7177a2c2de95bea8ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Vysoká dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines

Virtuální počítače Microsoft Azure (VM) se systémem SQL Server může pomoci nižší náklady hello vysokou dostupnost a po havárii (HADR) obnovení databáze řešení. Většina řešení SQL Server HADR jsou podporovány ve virtuálních počítačích Azure, jen Azure i jako hybridní řešení. V řešení jen Azure celý systém HADR hello běží v Azure. V hybridní konfigurace součást řešení hello spuštění v Azure a hello další část běží na místě ve vaší organizaci. Hello flexibilitu hello prostředí Azure vám umožní toomove částečně nebo zcela tooAzure toosatisfy hello nároky a HADR požadavky systému SQL Server databáze systémy.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="understanding-hello-need-for-an-hadr-solution"></a>Principy hello potřebu řešení s HADR
Až tooyou tooensure se, zda má systém databáze hello HADR možnosti, které vyžaduje smlouvu o úrovni služeb hello (SLA). Hello skutečnost, že Azure poskytuje vysokou dostupnost mechanismů, například službou opravy pro cloudové služby a detekce chyb obnovení pro virtuální počítače, samotné nezaručí, že splňujete hello potřeby SLA. Tyto mechanismy chránit hello vysokou dostupnost hello virtuálních počítačů, ale není hello vysoké dostupnosti systému SQL Server spuštěnou uvnitř hello virtuálních počítačů. Je možné pro toofail instance systému SQL Server hello při hello virtuálních počítačů online a v pořádku. Kromě toho povolit i hello vysokou dostupnost mechanismy, které poskytuje Azure výpadek hello virtuálních počítačích kvůli tooevents je to třeba obnovení z selhání softwaru nebo hardwaru a upgrady operačního systému.

Geograficky redundantní úložiště (GRS) v Azure, což je implementováno s funkci geografická replikace, navíc nemusí být řešení pro zotavení po havárii odpovídající, pro vaše databáze. Protože geografická replikace asynchronně odešle data, poslední aktualizace může dojít ke ztrátě v hello události po havárii. Další informace týkající se omezeních geografická replikace jsou popsané v hello [geografická replikace není podporována pro data a soubory protokolu na různých discích](#geo-replication-support) části.

## <a name="hadr-deployment-architectures"></a>HADR nasazení architektury
SQL Server HADR technologie, které jsou podporovány v Azure patří:

* [Always On skupiny dostupnosti](https://technet.microsoft.com/library/hh510230.aspx)
* [Always On instance clusteru převzetí služeb při selhání](https://technet.microsoft.com/library/ms189134.aspx)
* [Přesouvání protokolu](https://technet.microsoft.com/library/ms187103.aspx)
* [Zálohování systému SQL Server a obnovení služby úložiště objektů Blob v Azure](https://msdn.microsoft.com/library/jj919148.aspx)
* [Zrcadlení databáze](https://technet.microsoft.com/library/ms189852.aspx) – nepoužívané v systému SQL Server 2016

Je možné toocombine hello technologie společně tooimplement řešení SQL serveru, který má vysokou dostupnost a funkcím pro obnovení po havárii. V závislosti na hello technologie, které používáte může vyžadovat hybridní nasazení tunelového připojení sítě VPN s hello virtuální síť Azure. v níže uvedených částech Hello ukážeme některé hello příklad nasazení architektury.

## <a name="azure-only-high-availability-solutions"></a>Jenom Azure: řešení s vysokou dostupností

Řešení vysoké dostupnosti může mít pro SQL Server na úrovni databáze pomocí skupin dostupnosti Always On - nazývají skupiny dostupnosti. Můžete také vytvořit řešení vysoké dostupnosti na úrovni instance instancemi vždy na převzetí služeb při selhání clusteru – převzetí služeb při selhání instance clusteru. Pro další redundanci můžete vytvořit redundance na obou úrovních vytvořením skupiny dostupnosti na převzetí služeb při selhání instance clusteru. 

| Technologie | Příklad architektury |
| --- | --- |
| **Skupiny dostupnosti** |Hello replik dostupnosti spuštěna ve virtuálních počítačích Azure ve stejné oblasti poskytovat vysokou dostupnost. Je nutné tooconfigure řadič domény virtuálního počítače, protože Windows převzetí služeb při selhání clustering vyžaduje domény služby Active Directory.<br/> ![Skupiny dostupnosti](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Další informace najdete v tématu [konfigurace skupiny dostupnosti v Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Instance clusteru převzetí služeb při selhání** |Instance clusteru převzetí služeb při selhání (FCI), které vyžadují sdílené úložiště, můžete vytvořit 3 různými způsoby.<br/><br/>1. Cluster převzetí služeb při selhání dvěma uzly se systémem ve virtuálních počítačích Azure s použitím připojené úložiště [Windows Server 2016 prostory úložiště – přímé \(S2D\) ](virtual-machines-windows-portal-sql-create-failover-cluster.md) tooprovide softwarový virtuální síť SAN.<br/><br/>2. Cluster převzetí služeb při selhání dvěma uzly se systémem ve virtuálních počítačích Azure s úložištěm podporuje clustering řešení třetí strany. Konkrétní příklad, který používá s DataKeeper najdete v tématu [vysoká dostupnost pro sdílenou složku pomocí clustering převzetí služeb při selhání a 3. stran software s DataKeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>3. Clusteru s podporou převzetí služeb při selhání dvěma uzly ve virtuálních počítačích Azure s vzdálené služby iSCSI Target sdílené blokové úložiště přes ExpressRoute. Například NetApp privátní úložiště (NPS) zpřístupní cíle iSCSI prostřednictvím ExpressRoute s Equinix tooAzure virtuálních počítačů.<br/><br/>Pro sdílené úložiště jiných výrobců a řešení replikace dat měli byste požádat dodavatele hello pro všechna data související tooaccessing problémy na převzetí služeb při selhání.<br/><br/>Všimněte si, že pomocí FCI na [Azure File storage](https://azure.microsoft.com/services/storage/files/) se zatím nepodporuje, protože toto řešení nevyužívá Storage úrovně Premium. Tento brzy toosupport pracujeme. |

## <a name="azure-only-disaster-recovery-solutions"></a>Jenom Azure: řešení zotavení po havárii
Můžete mít řešení zotavení po havárii pro databáze SQL Server v Azure pomocí skupin dostupnosti, zrcadlení databáze nebo zálohování a obnovení pomocí úložiště objektů BLOB.

| Technologie | Příklad architektury |
| --- | --- |
| **Skupiny dostupnosti** |Repliky dostupnosti systémem přes několik datových center ve virtuálních počítačích Azure pro zotavení po havárii. Toto řešení mezi oblastmi chrání před dokončení lokality výpadku. <br/> ![Skupiny dostupnosti](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>V oblasti musí být všechny repliky v rámci hello stejné cloudové služby a hello stejnou virtuální síť. Vzhledem k tomu, že každou oblast, bude mít samostatné virtuální sítě, tato řešení vyžadovat tooVNet připojení virtuální sítě. Další informace najdete v tématu [konfigurace VNet-to-VNet připojení pomocí portálu Azure hello](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Podrobné pokyny najdete v tématu [nakonfigurovat skupinu dostupnosti SQL Server na virtuálních počítačích Azure v různých oblastech](virtual-machines-windows-portal-sql-availability-group-dr.md).|
| **Zrcadlení databáze** |Objekt zabezpečení a zrcadlení a servery v různých datových centrech pro zotavení po havárii. Je nutné nasadit pomocí certifikátů serveru, protože domény služby active directory nemůžou zahrnovat několik datových center.<br/>![Zrcadlení databáze](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Zálohování a obnovení pomocí služby Azure Blob Storage** |Provozní databáze zálohovat přímo tooblob úložiště v jiném datovém centru pro zotavení po havárii.<br/>![Zálohování a obnovení](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Další informace najdete v tématu [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybridního IT: Řešení zotavení po havárii
Můžete mít řešení pro zotavení po havárii pro databáze SQL Server v IT hybridní prostředí pomocí skupin dostupnosti, zrcadlení databáze, přesouvání protokolu a zálohování a obnovení s Azure blog storage.

| Technologie | Příklad architektury |
| --- | --- |
| **Skupiny dostupnosti** |Některé repliky dostupnosti spuštěna ve virtuálních počítačích Azure a další repliky spuštěné místně pro zotavení po havárii webů. Hello pracoviště může být buď místní nebo v datovém centru Azure.<br/>![Skupiny dostupnosti](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Protože všechny repliky dostupnosti musí být v hello stejného clusteru převzetí služeb při selhání, hello clusteru musí span obě sítě (clusteru s podporou převzetí služeb při selhání více podsítí). Tato konfigurace vyžaduje připojení VPN mezi Azure a hello do místní sítě.<br/><br/>Úspěšné zotavení po havárii databází nainstalujete také repliky řadiče domény v lokalitě pro obnovení po havárii hello.<br/><br/>Je možné toouse hello Průvodce přidáním repliky v aplikaci SSMS tooadd Azure repliky tooan existující vždy na skupiny dostupnosti. Další informace najdete v tématu kurz: rozšířit vaše tooAzure vždy na skupiny dostupnosti. |
| **Zrcadlení databáze** |Jeden partnera systémem do virtuálního počítače Azure a hello ostatní spuštěné místně pro zotavení po havárii webů pomocí certifikátů serveru. Partneři nemusí toobe v hello stejné domény služby Active Directory a není třeba žádné připojení k síti VPN.<br/>![Zrcadlení databáze](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Jiné databáze zrcadlení scénář zahrnuje jednu partnera spuštění ve virtuálním počítači Azure a hello jiných spuštěná místně v hello stejné domény služby Active Directory pro zotavení po havárii webů. A [připojení VPN mezi hello Azure virtuální sítě a hello místní sítě](../../../vpn-gateway/vpn-gateway-site-to-site-create.md) je vyžadován.<br/><br/>Úspěšné zotavení po havárii databází nainstalujete také repliky řadiče domény v lokalitě pro obnovení po havárii hello. |
| **Přesouvání protokolu** |Jeden server se systémem do virtuálního počítače Azure a hello ostatní spuštěné místně pro zotavení po havárii webů. Přesouvání protokolu, závisí na sdílení souborů systému Windows, připojení VPN mezi hello virtuální síť Azure a hello místní sítě je vyžadováno.<br/>![Přesouvání protokolu](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Úspěšné zotavení po havárii databází nainstalujete také repliky řadiče domény v lokalitě pro obnovení po havárii hello. |
| **Zálohování a obnovení pomocí služby Azure Blob Storage** |Místní provozní databáze zálohovat přímo tooAzure objektu blob úložiště pro zotavení po havárii.<br/>![Zálohování a obnovení](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Další informace najdete v tématu [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Důležité informace pro SQL Server HADR v Azure
Virtuální počítače Azure, úložiště a síťové mít odlišné provozní vlastnosti než místní, nevirtualizovaný infrastruktury IT. Úspěšné dokončení implementace řešení HADR SQL Server v Azure vyžaduje, abyste pochopili tyto rozdíly a návrh vašeho řešení tooaccommodate je.

### <a name="high-availability-nodes-in-an-availability-set"></a>Uzly vysoké dostupnosti v nastavení dostupnosti
Skupiny dostupnosti v Azure umožní tooplace hello vysokou dostupnost uzly do samostatné domény selhání (FDs) a aktualizace domény (UDs). Pro virtuální počítače Azure toobe umístit do hello stejné nastavení dostupnosti, musíte ho nasadit v hello stejné cloudové služby. Pouze uzly v hello, které mohou být součástí stejné cloudové služby hello stejné skupině dostupnosti. Další informace najdete v tématu [hello Správa dostupnosti virtuálních počítačů](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="failover-cluster-behavior-in-azure-networking"></a>Chování clusteru převzetí služeb při selhání v Azure sítě
Hello bez specifikaci RFC služba DHCP v Azure může způsobit hello vytvoření určité převzetí služeb při selhání clusteru konfigurace toofail, z důvodu název sítě s clustery toohello přiřazeny duplicitní IP adresu, jako je například hello stejnou IP adresu jako jeden z uzlů clusteru hello. Jedná se o problém při implementaci skupin dostupnosti, které závisí na funkci clusteru převzetí služeb při selhání Windows hello.

Při vytvoření clusteru se dvěma uzly a pak přepne do online režimu, podívejte se na scénář hello:

1. Hello clusteru přechodu do režimu online a potom Uzel1 požadavky dynamicky přiřazenou IP adresu pro název sítě s clustery hello.
2. Žádné IP adresy, než Uzel1 na vlastní IP adresu je dán hello službu DHCP, protože hello služba DHCP rozpozná, že žádost o hello pochází z Uzel1 sám sebe.
3. Windows zjistí, že duplicitní adresa je přiřazená tooNODE1 a toohello název sítě clusteru převzetí služeb při selhání a hello výchozí skupiny clusteru selže toocome online.
4. Hello výchozí skupiny clusteru přesune tooNODE2, která zpracovává Uzel1 na IP adresu jako IP adresu clusteru hello a přináší hello výchozí skupiny clusteru online.
5. Když Uzel2 pokusí tooestablish připojení s UZEL1, pakety směrované na Uzel1 nikdy neopustí Uzel2, protože vyřešila Uzel1 na IP adresu tooitself. Uzel2 nelze navázat připojení s UZEL1, pak dojde ke ztrátě kvora a ukončí hello clusteru.
6. V té doby hello Uzel1 může odesílat pakety tooNODE2, ale nemůže odpovědět, UZEL2. Uzel1 dojde ke ztrátě kvora a ukončí hello clusteru.

Tento scénář může zabránit přiřazení nepoužívané statickou IP adresu, jako jsou například místní IP adresu jako 169.254.1.1, název toohello sítě s clustery v pořadí toobring hello síťového názvu clusteru online. toosimplify to zpracovat, najdete v části [Windows konfiguraci clusteru převzetí služeb při selhání v Azure pro skupiny dostupnosti](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Další informace najdete v tématu [konfigurovat skupiny dostupnosti v Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Podpora naslouchací proces skupiny dostupnosti
Naslouchací procesy skupiny dostupnosti jsou podporovány v Azure virtuální počítače se systémem Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016. Tato podpora je možné pomocí hello Vyrovnávání zatížení sítě koncových bodů povoleno na virtuálních počítačích Azure hello, které jsou uzly skupiny dostupnosti. Postupujte podle kroků speciální konfigurace pro hello toowork naslouchací procesy pro oba klientských aplikací, které jsou spuštěné v Azure a také ty spuštěné místně.

Existují dvě hlavní možnosti pro nastavení vaší naslouchací proces: externí (veřejných) nebo interní. hello Hello externí naslouchací proces (veřejné) používá internetové nástroj pro vyrovnávání zatížení a souvisí s veřejná virtuální IP (VIP) které je přístupné přes internet. Interní nástroj používá interní naslouchací proces a hello pouze podporuje klienty v rámci stejné virtuální síti. Buď typ nástroje pro vyrovnávání zatížení, je nutné povolit přímé odpovědi ze serveru. 

Pokud hello skupiny dostupnosti zahrnuje několik podsítí Azure (například nasazení, které protne oblastí Azure), musíte zahrnout hello klienta připojovací řetězec "**MultisubnetFailover = True**". Výsledkem paralelní připojení pokusy o toohello repliky v různých podsítích hello. Pokyny týkající se nastavení naslouchací proces najdete v tématu

* [Konfigurace naslouchací proces ILB pro skupiny dostupnosti v Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
* [Konfigurace o externí naslouchací proces pro skupiny dostupnosti v Azure](../classic/ps-sql-ext-listener.md).

Můžete se pořád připojit repliky dostupnosti tooeach samostatně připojením přímo toohello instance služby. Navíc vzhledem k tomu, že zpětně kompatibilní s klienty zrcadlení databáze skupiny dostupnosti se můžete připojit toohello replik dostupnosti jako databáze zrcadlení partneři jsou nakonfigurovány hello repliky podobný zrcadlení toodatabase:

* Jedna primární replika a jedna sekundární replika
* Hello sekundární repliky nakonfigurován jako bez možnosti čtení (**čitelný sekundární** možnost nastavit příliš**ne**)

Zde je příklad klienta připojovací řetězec, který odpovídá konfiguraci zrcadlení jako databáze toothis pomocí ADO.NET nebo SQL Server Native Client:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Další informace o připojení klientů najdete v tématu:

* [Klíčová slova připojovací řetězec pomocí SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
* [Připojení klienti tooa relace zrcadlení databáze (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
* [TooAvailability naslouchací proces skupiny v IT hybridní připojení](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
* [Naslouchací procesy skupiny dostupnosti, připojení klientů a převzetí služeb při selhání aplikace (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
* [Použití řetězců připojení zrcadlení databáze se skupinami dostupnosti](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Latence sítě v hybridní IT
Měli byste nasadit řešení HADR hello předpokladu, že může být období s vysoké latenci mezi místní sítí a Azure. Při nasazování tooAzure repliky, používejte pro hello synchronizace režim asynchronního potvrzování místo synchronním potvrzováním. Při nasazení serverů zrcadlení databáze místně i v Azure, používat režim vysoce výkonné hello místo hello vysokou bezpečnost režimu.

### <a name="geo-replication-support"></a>Geografická replikace podpory
Geografická replikace v disky Azure nepodporuje hello datový soubor a soubor protokolu hello stejné databáze toobe uložené na různých discích. GRS replikovat změny na každém disku nezávisle a asynchronně. Tento mechanismus zaručuje hello zápisu pořadí v rámci hello geograficky replikované kopírovat, ale ne geograficky replikované kopie více disků jediný disk. Pokud nakonfigurujete databáze toostore jeho datového souboru a jeho soubor protokolu na různých discích, hello obnovit disky po havárii může obsahovat více aktuální kopie hello datový soubor než hello souboru protokolu, které zalomení hello předběžné protokolování v systému SQL Server a hello kyseliny Vlastnosti transakce. Pokud nemáte hello možnost toodisable geografická replikace na účet úložiště hello, byste měli mít všechna data a soubory protokolu pro danou databázi na hello stejný disk. Pokud je nutné použít více než jeden disk z důvodu toohello velikost hello databáze, je třeba toodeploy jedno z řešení obnovení po havárii hello uvedené výše redundanci dat tooensure.

## <a name="next-steps"></a>Další kroky
Pokud potřebujete toocreate virtuální počítač Azure se systémem SQL Server, přečtěte si [zřizování virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).

tooget hello nejlepší výkon ze SQL Server běžící na virtuálním počítači Azure, najdete v části hello pokyny v [osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Další témata související s toorunning systému SQL Server ve virtuálních počítačích Azure, najdete v části [systému SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Další prostředky
* [Instalace nové doménové struktury služby Active Directory v Azure](../../../active-directory/active-directory-new-forest-virtual-machine.md)
* [Vytvoření clusteru převzetí služeb při selhání pro skupiny dostupnosti ve virtuálním počítači Azure](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)

