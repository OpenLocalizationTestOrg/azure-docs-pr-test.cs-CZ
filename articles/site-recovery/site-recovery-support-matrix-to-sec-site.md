---
title: "matice aaaSupport pro replikaci tooa sekundární lokalitu s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje hello podporované operační systémy a součásti služby Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/24/2017
ms.author: raynew
ms.openlocfilehash: 0b2bbc86aff52308d5a90a56d7a3ff4286877740
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="support-matrix-for-replication-tooa-secondary-site-with-azure-site-recovery"></a>Podporu pro replikaci tooa sekundární lokalitu s Azure Site Recovery

Tento článek shrnuje, co je podporováno při použití Azure Site Recovery tooreplicate tooa sekundární místní lokalitu.

## <a name="deployment-options"></a>Možnosti nasazení

**Nasazení** | **VMware nebo fyzický server** | **Technologie Hyper-V (s/bez SCVMM)**
--- | --- | --- | ---
**Azure Portal** | Místní lokalita VMware toosecondary virtuálních počítačů VMware.<br/><br/> Stáhnout hello [InMage Scout uživatelská příručka](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) (není k dispozici v hello portál Azure). | Místní virtuální počítače Hyper-V v cloudech VMM tooa sekundární VMM cloudu.<br></br> Nepodporuje bez VMM  <br/><br/> Standardní technologie Hyper-V replikaci jenom. Síť SAN není podporována.
**Portál Classic** | Pouze v režimu údržby. Nové trezory nelze vytvořit. | Pouze v režimu údržby<br></br> Nepodporuje bez SCVMM
**PowerShell** | Nepodporuje se | Podporuje se<br></br> Nepodporuje bez SCVMM

## <a name="on-premises-servers"></a>Místní servery

### <a name="virtualization-servers"></a>Virtualizace serverů

**Nasazení** | **Podpora**
--- | ---
**Virtuální počítač VMware nebo fyzický server** | vSphere 6.0, 5.5 nebo 5.1 s nejnovější aktualizací
**Technologie Hyper-V (s nástrojem VMM)** | VMM 2016 a VMM 2012 R2

  >[!Note]
  > Cloudy VMM 2016 s směs systému Windows Server 2016 a 2012 R2 hostitelů nejsou aktuálně podporovány.

### <a name="host-servers"></a>Hostitelské servery

**Nasazení** | **Podpora**
--- | ---
**Virtuální počítač VMware nebo fyzický server** | vCenter 5.5 nebo 6.0 (podpora pro verzi 5.5 funkce)
**Technologie Hyper-V (bez VMM)** | Podporované konfigurace pro replikaci tooa sekundární lokality
**Technologie Hyper-V s nástrojem VMM** | Windows Server 2016 a Windows Server 2012 R2 s nejnovějšími aktualizacemi hello.<br/><br/> Hostitelé systému Windows Server 2016 se mají spravovat nástrojem VMM 2016.

## <a name="support-for-replicated-machine-os-versions"></a>Podpora pro verze operačního systému replikovaného počítače
Hello následující tabulka shrnuje podporu operačního systému v různých scénářích nasazení došlo při používání Azure Site Recovery. Tato podpora se vztahuje pro jakoukoli úlohu spuštěnou na hello uvedených operačního systému.

**VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
64bitová verze systému Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 s v minimálně SP1<br/><br/> Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos verze 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4 nebo 6.5 systémem hello Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 | Všechny hostované operační systém [podporovaná technologií Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)

>[!Note]
>Je možné replikovat pouze počítače se systémem Linux s hello následující úložiště: souboru systému (EXT3, ETX4, ReiserFS, XFS); Mapovač vícenásobný software zařízení; Správce svazků (LVM2).
>Fyzické servery s HP CCISS řadič úložiště nejsou podporovány.
>systém souborů ReiserFS Hello je podporována pouze na SUSE Linux Enterprise Server 11 SP3.

## <a name="network-configuration"></a>Konfigurace sítě

### <a name="hosts"></a>Hostitelé

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
Seskupování síťových adaptérů | Ano | Ano
SÍTĚ VLAN | Ano | Ano
IPv4 | Ano | Ano
IPv6 | Ne | Ne

### <a name="guest-vms"></a>Hostované virtuální počítače

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
Seskupování síťových adaptérů | Ne | Ne
IPv4 | Ano | Ano
IPv6 | Ne | Ne
Statická IP adresa (Windows) | Ano | Ano
Statická IP adresa (Linux) | Ano | Ano
Více síťovými Kartami | Ano | Ano


## <a name="storage"></a>Úložiště

### <a name="host-storage"></a>Hostování úložiště

**Úložiště (hostitel)** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
SYSTÉM SOUBORŮ NFS | Ano | Není k dispozici
SMB 3.0 | Není k dispozici | Ano
SÍŤ SAN (ISCSI) | Ano | Ano
S více cestami (MPIO) | Ano | Ano

### <a name="guest-or-physical-server-storage"></a>Host nebo fyzický server úložiště

**Konfigurace** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
VMDK | Ano | Není k dispozici
VHD/VHDX | Není k dispozici | Ano (až too16 disky)
Fin 2 virtuálních počítačů | Není k dispozici | Ano
Sdílený disk clusteru | Ano  | Ne
Šifrované disku | Ne | Ne
ROZHRANÍ UEFI| Ne | Není k dispozici
SYSTÉM SOUBORŮ NFS | Ne | Ne
SMB 3.0 | Ne | Ne
RDM | Ano | Není k dispozici
Disk > 1 TB | Ne | Ano
Svazek s prokládané disku > 1 TB<br/><br/> LVM | Ano | Ano
Prostory úložiště | Ne | Ano
Přidat nebo odebrat aktivní disku | Ne | Ne
Vyloučení disku | Ne | Ano
S více cestami (MPIO) | Není k dispozici | Ano

## <a name="vaults"></a>trezory

**Akce** | **VMware nebo fyzický server** | **Technologie Hyper-V (s nástrojem VMM)**
--- | --- | ---
Přesunutí trezorů v rámci skupiny prostředků (v rámci nebo předplatných) | Ne | Ne
Přesunout úložiště, sítě, virtuální počítače Azure mezi skupinami prostředků (v rámci nebo předplatných) | Ne | Ne

## <a name="provider-and-agent"></a>Zprostředkovatel a agent

**Název** | **Popis** | **Nejnovější verzi** | **Podrobnosti**
--- | --- | --- | --- | ---
**Zprostředkovatele Azure Site Recovery** | Koordinuje komunikaci mezi místními servery a Azure <br/><br/> Pokud VMM server není nainstalována na místní servery VMM nebo na servery Hyper-V | 5.1.19 ([dostupná z portálu](http://aka.ms/downloaddra)) | [Nejnovější funkce a opravy](https://support.microsoft.com/kb/3155002)
**Služba mobility** | Koordinuje replikaci mezi místními servery VMware nebo fyzických serverů a hello sekundární lokality<br/><br/> Nainstalovat na virtuální počítač VMware nebo fyzických serverů, které chcete tooreplicate  | Není k dispozici (je k dispozici z portálu) | Není k dispozici


## <a name="next-steps"></a>Další kroky

- [Replikace virtuálních počítačů technologie Hyper-V ve VMM cloudy tooa sekundární lokality](site-recovery-vmm-to-vmm.md)
- [Replikovat virtuální počítače VMware a fyzické servery tooa sekundární lokality](site-recovery-vmware-to-vmware.md)
