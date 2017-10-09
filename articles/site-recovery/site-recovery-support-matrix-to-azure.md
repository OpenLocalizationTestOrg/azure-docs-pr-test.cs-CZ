---
title: matici podpory aaaAzure Site Recovery pro replikaci tooAzure | Microsoft Docs
description: "Shrnuje hello podporované operační systémy a součásti služby Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: rajanaki
ms.openlocfilehash: eae1db2ff1392d272f6b2eb0e3410da19d09da7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-on-premises-tooazure"></a>Azure Site Recovery matici podpory pro replikaci z místní tooAzure


Tento článek shrnuje podporované konfigurace a součásti služby Azure Site Recovery při replikaci a obnovení tooAzure. Další informace o požadavcích na Azure Site Recovery najdete v tématu hello [požadavky](site-recovery-prereq.md).


## <a name="support-for-deployment-options"></a>Podporu pro možnosti nasazení

**Nasazení** | **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez nástroje Virtual Machine Manager)** |
--- | --- | ---
**Azure Portal** | Místní úložiště tooAzure virtuální počítače VMware, s Azure Resource Manager nebo classic úložiště a sítě.<br/><br/> Převzetí služeb při selhání tooResource založené na správci nebo classic virtuálních počítačů. | Místní úložiště tooAzure virtuálních počítačů Hyper-V, s Resource Manager nebo classic úložiště a sítě.<br/><br/> Převzetí služeb při selhání tooResource založené na správci nebo classic virtuálních počítačů.
**Portál Classic** | Pouze v režimu údržby. Nové trezory nelze vytvořit. | Pouze v režimu údržby.
**PowerShell** | Není aktuálně podporováno. | Podporuje se


## <a name="support-for-datacenter-management-servers"></a>Podpora pro servery pro správu datového centra

### <a name="virtualization-management-entities"></a>Virtualizace správu entity

**Nasazení** | **Podpora**
--- | ---
**Virtuální počítač VMware nebo fyzický server** | vCenter 6.5, 6.0 nebo 5,5
**Technologie Hyper-V (s nástrojem Virtual Machine Manager)** | System Center Virtual Machine Manager 2016 a System Center Virtual Machine Manager 2012 R2

  >[!Note]
  > System Center Virtual Machine Manager 2016 cloudu s směs systému Windows Server 2016 a 2012 R2 hostitele není aktuálně podporován.

### <a name="host-servers"></a>Hostitelské servery

**Nasazení** | **Podpora**
--- | ---
**Virtuální počítač VMware nebo fyzický server** | vSphere verze 6.5, 6.0, 5.5
**Technologie Hyper-V (s/bez nástroje Virtual Machine Manager)** | Windows Server 2016, Windows Server 2012 R2 s nejnovějšími aktualizacemi.<br></br>Pokud se používá SCVMM hostitelů Windows Server 2016 se mají spravovat nástrojem SCVMM 2016.


  >[!Note]
  >Technologie Hyper-V lokalitě, která zkombinuje hostitele se systémem Windows Server 2016 a 2012 R2 není aktuálně podporován. Tooan alternativní umístění pro obnovení pro virtuální počítače na hostiteli systému Windows Server 2016 se aktuálně nepodporuje.

## <a name="support-for-replicated-machine-os-versions"></a>Podpora pro verze operačního systému replikovaného počítače

Virtuální počítače, které jsou chráněné musí splňovat [požadavky pro Azure](#failed-over-azure-vm-requirements) při replikaci tooAzure.
Hello následující tabulka shrnuje podporu pro replikované operačního systému v různých scénářích nasazení při použití Azure Site Recovery. Tato podpora se vztahuje pro jakoukoli úlohu spuštěnou na hello uvedených operačního systému.

 **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez VMM)** |
--- | --- |
64bitová verze systému Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 s v minimálně SP1<br/>*Windows Server 2016* – nepodporuje aktuálně na virtuální počítače VMware a fyzické servery. <br/><br/> Red Hat Enterprise Linux: 5.2 too5.11, 6.1 too6.8, 7.0 too7.3 <br/><br/>Centů operačního systému: 5.2 too5.11, 6.1 too6.8, 7.0 too7.3 <br/><br/>Ubuntu 14.04 LTS server[ (podporované verze jádra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (podporované verze jádra)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Oracle Enterprise Linux 6.4, 6.5 systémem hello Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> SUSE Linux Enterprise Server 11 SP4 <br/>(Upgrade replikaci počítačů z SLES 11 SP3 tooSLES 11 SP4 není podporována. Pokud byl upgradován replikovaného počítače z tooSLES 11SP3 SLES 11 SP4, budete potřebovat toodisable replikace a chránit počítač hello znovu po upgradu hello.) | Všechny hostovaný operační systém [nepodporuje v Azure](https://technet.microsoft.com/library/cc794868.aspx)


>[!IMPORTANT]
>(Platí tooVMware/fyzické servery replikace tooAzure)
>
> Na Red Hat Enterprise Linux Server 7 + a CentOS 7 + servery je podporována verze 3.10.0-514 jádra od verze 9.8 hello služby Azure Site Recovery Mobility.<br/><br/>
> Zákazníci na hello 3.10.0-514 jádra s verzí hello služba Mobility je nižší než verze 9.8 jsou požadované toodisable replikace, aktualizace hello verzi hello Mobility service tooversion 9.8 a pak znovu povolení replikace.


### <a name="supported-ubuntu-kernel-versions-for-vmwarephysical-servers"></a>Podporované verze Ubuntu jádra pro VMware nebo fyzické servery

**Verze** | **Verze služby mobility** | **Verze jádra** |
--- | --- | --- |
14.04 LTS | 9.9 | 3.13.0-24-Generic too3.13.0-117 – obecné<br/>3.16.0-25-Generic too3.16.0-77obecné<br/>3.19.0-18-Generic too3.19.0-80 – obecné<br/>4.2.0-18-Generic too4.2.0-42 – obecné<br/>4.4.0-21-Generic too4.4.0-75 – obecné |
14.04 LTS | 9.10 | 3.13.0-24-Generic too3.13.0-121 – obecné<br/>3.16.0-25-Generic too3.16.0-77obecné<br/>3.19.0-18-Generic too3.19.0-80 – obecné<br/>4.2.0-18-Generic too4.2.0-42 – obecné<br/>4.4.0-21-Generic too4.4.0-81 – obecné |
16.04 LTS | 9.10 | 4.4.0-21-Generic too4.4.0-81 – obecné<br/>4.8.0-34-Generic too4.8.0-56 – obecné<br/>4.10.0-14-Generic too4.10.0 – 24obecné |


## <a name="supported-file-systems-and-guest-storage-configurations-on-linux-vmwarephysical-servers"></a>Podporované souborové systémy a konfigurace úložiště hostovaný v systému Linux (VMware nebo fyzické servery)

Následující Hello souborové systémy a úložiště konfigurace softwaru je podporována v systému Linux servery se systémem VMware nebo fyzických serverů:
* Systémy souborů: ext3, ext4, ReiserFS (Suse Linux Enterprise Server pouze), XFS
* Správce svazků: LVM2
* Software s funkcí Multipath: Mapovač zařízení

Zařízení úložiště Paravirtualized (exportovat paravirtualized ovladače zařízení) nejsou podporovány.<br/>
Blokovat více fronty vstupně-výstupní operace zařízení nejsou podporovány.<br/>
Fyzické servery s řadič úložiště HP CCISS hello nejsou podporovány.<br/>

>[!Note]
> Na hello servery Linux následující adresáře (Pokud nastavený jako samostatné oddíly/souboru systems) musí být na hello stejný disk (disk hello operačního systému) na zdrojovém serveru hello: / (kořenová), / Boot, USR, /usr/local, /var, etc<br/><br/>
> Funkce XFSv5 na XFS systémy, jako je například metadata kontrolního součtu podporované od verze 9.10 hello služba Mobility. Pokud používáte funkce XFSv5, ověřte, že používáte verzi služby Mobility 9.10 nebo novější. Pro oddíl hello můžete hello xfs_info nástroj toocheck hello XFS tzv. Pokud je ftype nastavená too1, potom funkce XFSv5 se používá.
>


## <a name="support-for-network-configuration"></a>Podpora pro konfiguraci sítě
Následující tabulky Hello shrnují podporu konfigurace sítě v různých scénářích nasazení, které používají tooAzure tooreplicate Azure Site Recovery.

### <a name="host-network-configuration"></a>Konfigurace sítě hostitele

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez nástroje Virtual Machine Manager)**
--- | --- | ---
Seskupování síťových adaptérů | Ano<br/><br/>Není podporováno při replikaci fyzické počítače| Ano
SÍTĚ VLAN | Ano | Ano
IPv4 | Ano | Ano
IPv6 | Ne | Ne

### <a name="guest-vm-network-configuration"></a>Konfigurace sítě virtuálních počítačů hosta

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez nástroje Virtual Machine Manager)**
--- | --- | ---
Seskupování síťových adaptérů | Ne | Ne
IPv4 | Ano | Ano
IPv6 | Ne | Ne
Statická IP adresa (Windows) | Ano | Ano
Statická IP adresa (Linux) | Ano <br/><br/>Virtuální počítače je nakonfigurované toouse DHCP na navrácení služeb po obnovení  | Ne
Více síťovými Kartami | Ano | Ano

### <a name="failed-over-azure-vm-network-configuration"></a>Konfigurace sítě virtuálních počítačů Azure při selhání

**Síť Azure** | **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez nástroje Virtual Machine Manager)**
--- | --- | ---
ExpressRoute | Ano | Ano
ILB | Ano | Ano
REŽIM MANAGEOUT | Ano | Ano
Traffic Manager | Ano | Ano
Více síťovými Kartami | Ano | Ano
Rezervovaná IP adresa | Ano | Ano
IPv4 | Ano | Ano
Zachovat zdrojové IP adresy | Ano | Ano


## <a name="support-for-storage"></a>Podpora pro úložiště
Následující tabulky Hello shrnují podporu konfigurace úložiště v různých scénářích nasazení, které používají tooAzure tooreplicate Azure Site Recovery.

### <a name="host-storage-configuration"></a>Konfigurace úložiště hostitele

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez nástroje Virtual Machine Manager)**
--- | --- | --- | ---
SYSTÉM SOUBORŮ NFS | Ano pro VMware<br/><br/> Ne pro fyzické servery | Není k dispozici
SMB 3.0 | Není k dispozici | Ano
SÍŤ SAN (ISCSI) | Ano | Ano
S více cestami (MPIO)<br></br>Testovány s: Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM pro CLARiiON | Ano | Ano

### <a name="guest-or-physical-server-storage-configuration"></a>Host nebo konfigurace úložiště fyzického serveru

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez nástroje Virtual Machine Manager)**
--- | --- | ---
VMDK | Ano | Není k dispozici
VHD/VHDX | Není k dispozici | Ano
Fin 2 virtuálních počítačů | Není k dispozici | Ano
ROZHRANÍM EFI/UEFI| Ne | Ano
Sdílený disk clusteru | Ne | Ne
Šifrované disku | Ne | Ne
SYSTÉM SOUBORŮ NFS | Ne | Není k dispozici
SMB 3.0 | Ne | Ne
RDM | Ano<br/><br/> Není k dispozici pro fyzické servery | Není k dispozici
Disk > 1 TB | Ano<br/><br/>Až 4095 GB | Ano<br/><br/>Až 4095 GB
Disk s velikostí sektoru 4K | Ano | Ano, podporovány pro virtuální počítače generace 1<br/><br/>Není podporován pro virtuální počítače generace 2.
Svazek s prokládané disku > 1 TB<br/><br/> Správa logických LVM svazků | Ano | Ano
Prostory úložiště | Ne | Ano
Přidat nebo odebrat aktivní disku | Ne | Ne
Vyloučení disku | Ano | Ano
S více cestami (MPIO) | Není k dispozici | Ano

**Úložiště Azure** | **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez nástroje Virtual Machine Manager)**
--- | --- | ---
LRS | Ano | Ano
GRS | Ano | Ano
RA-GRS | Ano | Ano
Studeného úložiště | Ne | Ne
Horkého úložiště| Ne | Ne
Šifrování v rest(SSE)| Ano | Ano
Storage úrovně Premium | Ano | Ano
Import a export služby | Ne | Ne


## <a name="support-for-azure-compute-configuration"></a>Podpora pro Azure výpočetní konfigurace

**Výpočetní funkce** | **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez nástroje Virtual Machine Manager)**
--- | --- | --- 
Skupiny dostupnosti | Ano | Ano
ROZBOČOVAČE | Ano | Ano  
Managed Disks | Ano | Ano<br/><br/>Navrácení služeb po obnovení tooon místní z virtuálního počítače Azure s spravované disky není aktuálně podporován.

## <a name="failed-over-azure-vm-requirements"></a>Požadavky na virtuální počítač Azure při selhání

Můžete nasadit Site Recovery tooreplicate virtuální počítače a fyzické servery se systémem žádný operační systém nepodporuje v Azure. To zahrnuje většinu verzí systému Windows a Linux. Místní virtuální počítače, které budete chtít tooreplicate musí odpovídat hello při replikaci tooAzure následující požadavky pro Azure.

**Entity** | **Požadavky** | **Podrobnosti**
--- | --- | ---
**Hostovaný operační systém** | Replikace technologie Hyper-V tooAzure: Site Recovery podporuje všechny operační systémy, které jsou [nepodporuje v Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Pro fyzický server replikace a VMware: Zkontrolujte hello Windows a Linux [požadavky](site-recovery-vmware-to-azure-classic.md) | Kontrola předpokladů se nezdaří, pokud není podporován.
**Architektura operačního systému hosta** | 64bitová verze | Kontrola předpokladů se nezdaří, pokud není podporován
**Velikost disku operačního systému** | Pokud replikujete až too2048 GB **virtuální počítače VMware nebo fyzických serverů tooAzure**.<br/><br/>Až 2048 GB pro **technologie Hyper-V generace 1** virtuálních počítačů.<br/><br/>Až 300 GB pro **virtuálních počítačů technologie Hyper-V generace 2**.  | Kontrola předpokladů se nezdaří, pokud není podporován
**Počet disků operačního systému** | 1 | Kontrola předpokladů se nezdaří, pokud není podporován.
**Počet datových disků** | Pokud 64 nebo méně replikujete **virtuální počítače VMware tooAzure**; 16 nebo méně Pokud replikujete **tooAzure virtuálních počítačů Hyper-V** | Kontrola předpokladů se nezdaří, pokud není podporován
**Velikost datového disku virtuálního pevného disku** | Až too4095 GB | Kontrola předpokladů se nezdaří, pokud není podporován
**Síťové adaptéry** | Několik adaptérů jsou podporovány. |
**Sdílený virtuální pevný disk** | Nepodporuje se | Kontrola předpokladů se nezdaří, pokud není podporován
**FC disku** | Nepodporuje se | Kontrola předpokladů se nezdaří, pokud není podporován
**Formát pevného disku** | VIRTUÁLNÍ PEVNÝ DISK <br/><br/> VHDX | I když VHDX není aktuálně podporovaná v Azure, Site Recovery při selhání tooAzure automaticky převádí VHDX tooVHD. Pokud jste navrácení služeb po obnovení tooon místní hello virtuální počítače zůstávají formátu VHDX toouse hello.
**Nástroj BitLocker** | Nepodporuje se | Než začnete chránit virtuální počítač, musí se zakázat nástroj BitLocker.
**Název virtuálního počítače.** | 1 až 63 znaků. Tooletters s omezeným přístupem, číslice a pomlčky. název virtuálního počítače Hello musí začínat a končit písmenem nebo číslicí. | Aktualizujte hodnotu hello v hello vlastnosti virtuálního počítače ve službě Site Recovery.
**Typ virtuálního počítače** | 1. generace<br/><br/> Generace 2 – Windows | Virtuální počítače generace 2 se typ disku operačního systému basic (která zahrnuje jednu nebo dvě datové svazky naformátované jako VHDX) a menší než 300 GB místa na disku jsou podporovány.<br></br>Virtuální počítače s Linuxem generace 2 nejsou podporované. [Další informace](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="support-for-recovery-services-vault-actions"></a>Podpora pro akce trezoru služeb zotavení

**Akce** | **VMware nebo fyzický server** | **Technologie Hyper-V (bez Virtual Machine Manager)** | **Technologie Hyper-V (s nástrojem Virtual Machine Manager)**
--- | --- | --- | ---
Přesunutí trezoru rámci skupiny prostředků<br/><br/> V rámci a napříč odběrů | Ne | Ne | Ne
Přesunout úložiště, sítě, virtuální počítače Azure mezi skupinami prostředků<br/><br/> V rámci a napříč odběrů | Ne | Ne | Ne


## <a name="support-for-provider-and-agent"></a>Podpora pro zprostředkovatel a Agent

**Název** | **Popis** | **Nejnovější verzi** | **Podrobnosti**
--- | --- | --- | --- | ---
**Zprostředkovatele Azure Site Recovery** | Koordinuje komunikaci mezi místními servery a Azure <br/><br/> Nainstalovat na místní servery nástroje Virtual Machine Manager nebo na servery Hyper-V, pokud neexistuje žádný server nástroje Virtual Machine Manager | 5.1.19 ([dostupná z portálu](http://aka.ms/downloaddra)) | [Nejnovější funkce a opravy](https://support.microsoft.com/kb/3155002)
**Azure Unified instalace nástroje Site Recovery (VMware tooAzure)** | Koordinuje komunikaci mezi místními servery VMware a Azure <br/><br/> Nainstalovat na místní servery VMware | 9.3.4246.1 (k dispozici z portálu) | [Nejnovější funkce a opravy](https://support.microsoft.com/kb/3155002)
**Služba mobility** | Koordinuje replikaci mezi místními VMware servery/fyzické servery a Azure nebo sekundární lokality<br/><br/> Nainstalovat na virtuální počítač VMware nebo fyzických serverů chcete tooreplicate  | Není k dispozici (je k dispozici z portálu) | Není k dispozici
**Agenta Microsoft Azure Recovery Services (MARS)** | Koordinuje replikaci mezi virtuální počítače Hyper-V a Azure<br/><br/> Nainstalována na místní servery Hyper-V (s nebo bez serveru Virtual Machine Manager) | Nejnovější verze agenta ([dostupná z portálu](http://aka.ms/latestmarsagent)) |






## <a name="next-steps"></a>Další kroky
[Kontrola požadavků](site-recovery-prereq.md)
