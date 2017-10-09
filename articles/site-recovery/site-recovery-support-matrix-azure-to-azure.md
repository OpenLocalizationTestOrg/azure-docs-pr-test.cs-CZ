---
title: matici podpory aaaAzure Site Recovery pro replikaci z Azure tooAzure | Microsoft Docs
description: "Shrnuje hello podporované operační systémy a konfigurace pro virtuální počítače Azure (VM) Azure Site Recovery replikaci z jedné oblasti tooanother pro potřeby zotavení po havárii."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/10/2017
ms.author: sujayt
ms.openlocfilehash: 75b2451b4c2069ca4b11deb0efe1329d43879eb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-azure-tooazure"></a>Azure Site Recovery matici podpory pro replikaci z Azure tooAzure


>[!NOTE]
>
> Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.

Tento článek shrnuje podporované konfigurace a součásti služby Azure Site Recovery při replikaci a obnovení virtuálních počítačů Azure z jedné oblasti tooanother oblast.

## <a name="user-interface-options"></a>Možnosti uživatelského rozhraní

**Uživatelské rozhraní** |  **Podporované / nepodporované**
--- | ---
**Azure Portal** | Podporuje se
**Portál Classic** | Nepodporuje se
**PowerShell** | Aktuálně nepodporuje
**REST API** | Aktuálně nepodporuje
**Rozhraní příkazového řádku** | Aktuálně nepodporuje


## <a name="resource-move-support"></a>Podpora přesunutí prostředku

**Typ přesunutí prostředku** | **Podporované / nepodporované** | **Poznámky**  
--- | --- | ---
**Přesunutí trezoru rámci skupiny prostředků** | Nepodporuje se |Trezor služeb zotavení hello nelze přesouvat mezi skupinami prostředků.
**Přesunutí výpočty, úložiště a sítě v rámci skupiny prostředků** | Nepodporuje se |Pokud přesunete po povolení replikace virtuálního počítače (nebo jeho přidružené komponenty, jako je například úložiště a sítě), třeba toodisable replikace a povolení replikace pro virtuální počítač hello znovu.


## <a name="support-for-deployment-models"></a>Podpora pro modely nasazení

**Model nasazení** | **Podporované / nepodporované** | **Poznámky**  
--- | --- | ---
**Classic** | Podporuje se | Můžete replikovat klasické virtuální počítač a obnovit jako virtuální počítač s classic. Nelze obnovit jako virtuální počítač Resource Manager. Pokud nasadíte klasické virtuální počítač bez virtuální sítě a přímo tooan oblast Azure, se nepodporuje.
**Resource Manager** | Podporuje se |

## <a name="support-for-replicated-machine-os-versions"></a>Podpora pro verze operačního systému replikovaného počítače

Hello níže podpory se dá použít pro jakoukoli úlohu spuštěnou na hello uvedených operačního systému.

#### <a name="windows"></a>Windows

- Windows Server 2016 (instalace jádra serveru nebo Server s desktopovým prostředím) *
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 s v minimálně SP1

>[!NOTE]
>
> \*Windows Server 2016 Nano Server není podporována.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6.8, 7.0, 7.1, 7.2, 7.3
- CentOS verze 6.5, 6.6, 6.7, 6.8, 7.0, 7.1, 7.2, 7.3
- Ubuntu 14.04 LTS Server [ (podporované verze jádra)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server [ (podporované verze jádra)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Oracle Enterprise Linux 6.4, 6.5 systémem hello Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3)
- SUSE Linux Enterprise Server 11 SP3

>[!NOTE]
>
> Ověřování a přihlaste se na základě Ubuntu servery pomocí hesla a používání hello cloudu init balíček tooconfigure cloudu virtuálních počítačů, může být založené na heslech přihlášení po převzetí služeb při selhání (v závislosti na konfiguraci hello cloudinit.) zakázány Založené na heslech přihlášení může být znovu zapnout na virtuálním počítači hello resetováním hesla hello z nabídky nastavení hello (v rámci hello podporu + část věnovanou řešení potíží) hello při selhání virtuálního počítače na hello portálu Azure.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Podporované verze Ubuntu jádra pro virtuální počítače Azure

**Verze** | **Verze služby mobility** | **Verze jádra** |
--- | --- | --- |
14.04 LTS | 9.9 | 3.13.0-24-Generic too3.13.0-117 – obecné<br/>3.16.0-25-Generic too3.16.0-77obecné<br/>3.19.0-18-Generic too3.19.0-80 – obecné<br/>4.2.0-18-Generic too4.2.0-42 – obecné<br/>4.4.0-21-Generic too4.4.0-75 – obecné |
14.04 LTS | 9.10 | 3.13.0-24-Generic too3.13.0-121 – obecné<br/>3.16.0-25-Generic too3.16.0-77obecné<br/>3.19.0-18-Generic too3.19.0-80 – obecné<br/>4.2.0-18-Generic too4.2.0-42 – obecné<br/>4.4.0-21-Generic too4.4.0-81 – obecné |
16.04 LTS | 9.10 | 4.4.0-21-Generic too4.4.0-81 – obecné<br/>4.8.0-34-Generic too4.8.0-56 – obecné<br/>4.10.0-14-Generic too4.10.0 – 24obecné |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Podporované souborové systémy a konfigurace úložiště hosta na virtuálních počítačích Azure spuštěn operační systém Linux.

* Systémy souborů: ext3, ext4, ReiserFS (Suse Linux Enterprise Server pouze), XFS
* Správce svazků: LVM2
* Software s funkcí Multipath: Mapovač zařízení

## <a name="region-support"></a>Oblasti podpory

Můžete replikovat a obnovení virtuálních počítačů mezi všechny dvěma oblastmi v rámci hello stejné zeměpisné clusteru.

**Zeměpisná clusteru** | **Oblasti Azure**
-- | --
Amerika | Východní Kanada, Kanada centrální, Jižní střed USA, střed USA – Západ, východní USA, Východ USA 2, západ USA, západní USA 2, střed USA, střed USA – sever
Evropa | Spojené království – Západ, Spojené království – Jih, Severní Evropa, západní Evropa
Asie | Indie – Jih, střed, jihovýchodní Asie, východní Asie, Japonsko – východ, Japonsko – Západ, Korejská – střed, Korejská jih
Austrálie   | Austrálie – východ, Austrálie – jihovýchod

>[!NOTE]
>
> Oblasti Brazílie – jih můžete provádět pouze replikaci a převzetí služeb při selhání tooone jihu USA, Západ střední USA, Východ USA, Východ USA 2, západní USA, západní USA 2 a Sever střední USA oblastí a selhání zpět.


## <a name="support-for-compute-configuration"></a>Podpora pro konfiguraci výpočtů

**Konfigurace** | **Podporované/nepodporované** | **Poznámky**
--- | --- | ---
Velikost | Jakékoli velikosti virtuálního počítače Azure s jader procesoru alespoň 2 a 1 GB paměti RAM | Odkazovat příliš[velikosti virtuálního počítače Azure](../virtual-machines/windows/sizes.md)
Skupiny dostupnosti | Podporuje se | Pokud použijete výchozí možnost hello během kroku replikaci povolit portálu, skupina dostupnosti hello je automaticky vytvořit, podle konfigurace oblast zdroje. Můžete změnit hello cíl skupinou dostupnosti ve ' replikované položky > Nastavení > výpočty a síť > skupiny dostupnosti, kdykoli.
Hybridní použití zvýhodnění (ROZBOČOVAČ) virtuálních počítačů | Podporuje se | Pokud hello zdrojového virtuálního počítače má licenci ROZBOČOVAČE povoleno, převzetí služeb při selhání virtuálního počítače nebo hello testovací převzetí služeb při selhání také používá licence hello ROZBOČOVAČE.
Škálovací sady virtuálních počítačů | Nepodporuje se |
Publikovaná Microsoft Azure Galerie obrázků- | Podporuje se | Podporováno také hello virtuální počítač běží na podporovaný operační systém pomocí Site Recovery
Azure Gallery Image - publikovaná třetích stran | Podporuje se | Podporováno také hello virtuální počítač běží na podporovaný operační systém pomocí Site Recovery.
Vlastní Image - publikovaná třetích stran | Podporuje se | Podporováno také hello virtuální počítač běží na podporovaný operační systém pomocí Site Recovery.
Virtuální počítače migrovat pomocí Site Recovery | Podporuje se | Pokud je VMware nebo fyzický počítač migrovat tooAzure pomocí Site Recovery, potřebujete toouninstall hello starší verze služby mobility a restartujte počítač hello před replikace ho tooanother oblast Azure.

## <a name="support-for-storage-configuration"></a>Podpora pro konfigurace úložiště

**Konfigurace** | **Podporované/nepodporované** | **Poznámky**
--- | --- | ---
Maximální velikost disku operačního systému | 1023 GB | Odkazovat příliš[disky, které jsou používány virtuálními počítači.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Velikost disku maximum dat. | 1023 GB | Odkazovat příliš[disky, které jsou používány virtuálními počítači.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Počet datových disků | Až 64 podporuje konkrétní velikost virtuálního počítače Azure | Odkazovat příliš[velikosti virtuálního počítače Azure](../virtual-machines/windows/sizes.md)
Dočasné disku | Vždy z replikace vyloučit. | Dočasné disk je vyloučený z replikace vždy. Neměli vložit žádná trvalá data na dočasné disku podle Azure pokyny. Odkazovat příliš[dočasným diskovým na virtuálních počítačích Azure](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) další podrobnosti.
Míry na disku hello změny dat | Maximální počet 6 MB/s na disk | Pokud frekvence změny hello průměr dat na disku hello je nad rámec 6 MB/s nepřetržitě, nebudou aktualizovány replikace. Ale pokud je shluků příležitostně dat a hello míry změny dat je větší než 6 MB/s jistou dobu a dodává se, replikace budou aktualizovány. V takovém případě může se zobrazit body obnovení mírně zpožděné.
Disky na účty úložiště standard storage | Podporuje se |
Disky na prémiové účty úložiště | Podporuje se | Pokud virtuální počítač obsahuje disky, které jsou rozloženy účty úložiště standard a premium, můžete vybrat jinou cílovou účtu úložiště pro každý disk tooensure máte hello stejné konfigurace úložiště v cílová oblast
Standardní disky spravované | Nepodporuje se |  
Pro prémiové disky spravované | Nepodporuje se |
Prostory úložiště | Podporuje se |         
Šifrování v klidovém stavu (SSE) | Podporuje se | Pro účty úložiště mezipaměti a cíle můžete vybrat účet úložiště SSE povolena.     
Azure Disk Encryption (ADE) | Nepodporuje se |
Přidat nebo odebrat aktivní disku | Nepodporuje se | Je-li přidat nebo odebrat datový disk na hello virtuálních počítačů, třeba toodisable replikace a povolit replikaci znovu hello virtuálních počítačů.
Vyloučení disku | Nepodporuje se|   Ve výchozím nastavení je vyloučen dočasné disku.
LRS | Podporuje se |
GRS | Podporuje se |
RA-GRS | Podporuje se |
ZRS | Nepodporuje se |  
Aktivní a studeného úložiště | Nepodporuje se | Disky virtuálního počítače nejsou podporovány na studených a aktivní úložiště

>[!IMPORTANT]
> Ujistěte se, postupujte podle hello [pokynů pro virtualizované úložiště](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) pro váš zdroj Azure virtuální počítače tooavoid žádné problémy s výkonem. Pokud budete postupovat podle hello výchozí nastavení, Site Recovery vytvoří hello požadované účty úložiště na základě hello zdroj konfigurace. Pokud vlastní nastavení a vyberte vlastní nastavení, ujistěte se, postupujte hello (.. / storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) jako zdrojové virtuální počítače.

## <a name="support-for-network-configuration"></a>Podpora pro konfiguraci sítě
**Konfigurace** | **Podporované/nepodporované** | **Poznámky**
--- | --- | ---
Síťové rozhraní (NIC) | Až do maximální počet síťových adaptérů nepodporuje konkrétní velikost virtuálního počítače Azure | Síťové adaptéry se vytvoří při vytvoření hello virtuálních počítačů v rámci testovací převzetí služeb při selhání nebo převzetí služeb při selhání. Hello počet síťových adaptérů na hello převzetí služeb při selhání virtuálního počítače závisí na hello počet síťových adaptérů hello zdroj, který má virtuální počítač v době hello povolení replikace. Pokud jste přidat nebo odebrat síťovou kartu po povolení replikace, neovlivní počet síťový adaptér na hello převzetí služeb při selhání virtuálního počítače.
Internetový nástroj pro vyrovnávání zatížení | Podporuje se | Je třeba tooassociate hello předem nakonfigurované pro vyrovnávání zatížení pomocí služby azure automation skriptu v plánu obnovení.
Interní nástroj pro vyrovnávání zatížení | Podporuje se | Je třeba tooassociate hello předem nakonfigurované pro vyrovnávání zatížení pomocí služby azure automation skriptu v plánu obnovení.
Veřejná IP adresa| Podporuje se | Třeba tooassociate již existující toohello veřejné IP síťový adaptér nebo vytvořit a přidružit toohello síťový adaptér pomocí služby azure automation skriptu v plánu obnovení.
Skupina NSG na síťovou kartu (Resource Manager)| Podporuje se | Je nutné tooassociate hello NSG toohello síťový adaptér pomocí služby azure automation skriptu v plánu obnovení.  
Skupina NSG na podsítě (Resource Manager a klasický)| Podporuje se | Je nutné tooassociate hello NSG toohello síťový adaptér pomocí služby azure automation skriptu v plánu obnovení.
Skupina NSG na virtuálním počítači (klasické)| Podporuje se | Je nutné tooassociate hello NSG toohello síťový adaptér pomocí služby azure automation skriptu v plánu obnovení.
Vyhrazená IP adresa (statickou IP adresu) / zachovat zdrojové IP adresy | Podporuje se | Pokud hello síťovou kartu v hello zdrojového virtuálního počítače má konfiguraci statické IP adresy a hello cílové podsíti má hello stejnou IP Adresou, k dispozici, je přiřazen toohello převzetí služeb při selhání virtuálního počítače. Pokud hello cílové podsíti nemá hello hello stejnou IP Adresou k dispozici, jednu z dostupných IP adres v podsíti hello je vyhrazený pro tento virtuální počítač. Můžete zadat pevné IP zvoleného v ' replikované položky > Nastavení > výpočty a síť > síťových rozhraní se. Můžete vybrat hello síťovou kartu a zadejte hello podsíť a IP podle svého výběru.
Dynamické IP| Podporuje se | Pokud hello síťovou kartu v hello zdrojového virtuálního počítače má konfigurace s dynamickými IP, hello síťovou kartu v hello převzetí služeb při selhání virtuálního počítače je také dynamické ve výchozím nastavení. Můžete zadat pevné IP zvoleného v ' replikované položky > Nastavení > výpočty a síť > síťových rozhraní se. Můžete vybrat hello síťovou kartu a zadejte hello podsíť a IP podle svého výběru.
Integrace Traffic Manageru | Podporuje se | Můžete předkonfigurovat váš správce provozu tak, že provoz hello je koncový bod směrované toohello v oblasti zdroje v pravidelných intervalech a toohello koncový bod v cílové oblasti v případě převzetí služeb při selhání.
Spravovat Azure DNS | Podporuje se |
Vlastní DNS  | Podporuje se |    
Neověřené Proxy | Podporuje se | Odkazovat příliš[sítě pokyny dokumentu.](site-recovery-azure-to-azure-networking-guidance.md)    
Ověřené Proxy | Nepodporuje se | Pokud hello virtuální počítač používá ověřené proxy pro odchozí připojení, nelze replikovat, pomocí Azure Site Recovery.  
TooSite Site VPN s místním (s nebo bez ExpressRoute)| Podporuje se | Ujistěte se, že skupiny Nsg a udr hello jsou nakonfigurovány tak, že provoz obnovení lokality hello není směrované tooon místní. Odkazovat příliš[sítě pokyny dokumentu.](site-recovery-azure-to-azure-networking-guidance.md)  
TooVNET připojení virtuální sítě | Podporuje se | Odkazovat příliš[sítě pokyny dokumentu.](site-recovery-azure-to-azure-networking-guidance.md)  


## <a name="next-steps"></a>Další kroky
- Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md)
- Začněte chránit vaše úlohy [replikace virtuálních počítačů Azure](site-recovery-azure-to-azure.md)
