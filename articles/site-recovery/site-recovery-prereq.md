---
title: "aaaPrerequisites pro replikaci tooAzure pomocí Azure Site Recovery | Microsoft Docs"
description: "Další informace o hello požadavky na replikaci virtuálních počítačů a fyzických počítačů tooAzure pomocí služby Azure Site Recovery hello."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: rajanaki
ms.openlocfilehash: 0e32ab7cd7c65a3f67ffa2f2c15af189c15b6f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="prerequisites-for-replication-from-on-premises-tooazure-by-using-site-recovery"></a>Předpoklady pro replikaci z místní tooAzure pomocí Site Recovery

> [!div class="op_single_selector"]
> * [Replikace z Azure tooAzure](site-recovery-azure-to-azure-prereq.md)
> * [Replikace z místní tooAzure](site-recovery-prereq.md)

Azure Site Recovery můžete podporovat vaše obchodní kontinuitu a po havárii (BCDR) strategie tím, že orchestruje replikaci virtuálních počítačů (VM) Azure tooanother oblast Azure. Site Recovery také replikuje na místní fyzických serverů a virtuálních počítačů toohello cloudu (Azure) nebo tooa sekundárního datacentra. Pokud ve vaší primární lokalitě dojde k výpadku, můžete převzít tooa sekundárního umístění tookeep aplikace a úlohy, které jsou k dispozici. Primární umístění back tooyour může selhat, když primární umístění hello vrátí toonormal operace. Další informace o Site Recovery najdete v tématu [co je Site Recovery?](site-recovery-overview.md).

V tomto článku jsme shrnují hello požadavky od Site Recovery replikaci z tooAzure místní počítač.

Můžete odeslat všechny komentáře v dolní části hello hello článku. Také můžete pokládat technické dotazy v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="azure-requirements"></a>Požadavky na Azure

**Požadavek** | **Podrobnosti**
--- | ---
**Účet Azure** | A [účet Microsoft Azure](http://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
**Služba Site Recovery** | Další informace o cenách pro hello služba Azure Site Recovery najdete v tématu [cenách za Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Azure Storage** | Je třeba data toostore replikovat účtu Azure Storage. účet úložiště Hello musí být v hello trezoru služeb zotavení Azure stejné oblasti jako hello. Virtuální počítače Azure se vytvoří, když dojde k převzetí služeb při selhání.<br/><br/> V závislosti na modelu prostředků hello chcete toouse převzetí služeb při selhání virtuálního počítače Azure, můžete nastavit účet pomocí hello [modelu nasazení Azure Resource Manager](../storage/common/storage-create-storage-account.md) nebo hello [modelu nasazení classic](../storage/common/storage-create-storage-account.md).<br/><br/>Můžete použít [geograficky redundantní úložiště](../storage/common/storage-redundancy.md#geo-redundant-storage) nebo místně redundantní úložiště. Doporučujeme použít geograficky redundantní úložiště. S geograficky redundantní úložiště byla zajištěna odolnost dat, pokud oblastního výpadku nebo pokud není možné obnovit primární oblast hello.<br/><br/> Můžete použít účet úložiště Azure úrovně standard, nebo můžete použít Azure Premium Storage. [Storage úrovně Premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) se obvykle používá u virtuálních počítačů, které je třeba neustále vysoké vstupně-výstupní výkon a nízkou latencí. Storage úrovně premium můžete hostovat virtuální počítač I náročnými úlohy. Pokud použijete službu Storage úrovně Premium pro replikovaná data, potřebujete také účet úložiště úrovně Standard. Standardní účet úložiště ukládá protokoly replikace, které zaznamenání dat tooon místní probíhající změny.<br/><br/>
**Omezení úložiště** | Nelze přesunout účty úložiště, které použijete v Site Recovery tooa jiné skupině prostředků nebo přesunout tooor použití s jiné předplatné.<br/><br/> V současné době replikace toopremium účty úložiště v centrální Indie a – Jih, Indie není k dispozici.
**Síť Azure** | Budete potřebovat síť Azure, toowhich virtuální počítače Azure připojí po převzetí služeb při selhání. Hello síť Azure musí být v hello stejné oblasti jako hello trezoru služeb zotavení.<br/><br/> V hello portálu Azure, můžete vytvořit síť Azure pomocí hello [modelu nasazení Resource Manager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo hello [modelu nasazení classic](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).<br/><br/> Pokud budete replikovat z tooAzure System Center Virtual Machine Manager (VMM), musíte nastavit mapování sítě mezi sítěmi virtuálních počítačů nástroje VMM a sítě Azure. Tím se zajistí, že virtuální počítače Azure po převzetí služeb při selhání připojit tooappropriate sítě.
**Omezení sítě** | Nelze přesunout sítě účty, které použijete v Site Recovery tooa jiné skupině prostředků nebo přesunout tooor použití s jiné předplatné.
**Mapování sítě** | Pokud budete replikovat virtuální počítače Microsoft Hyper-V v cloudech VMM, musíte nastavit mapování sítě. Tím se zajistí, že virtuální počítače Azure připojit tooappropriate sítě, když jste vytvořili po převzetí služeb při selhání.

> [!NOTE]
> Hello následující části popisují hello požadavky pro různé součásti hello prostředí zákazníků. Další informace o podpoře pro konkrétní konfigurace najdete v tématu hello [matici podpory](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-vms-or-physical-windows-or-linux-servers-tooazure"></a>Zotavení po havárii virtuálních počítačů VMware nebo fyzických serverů tooAzure systému Windows nebo Linux
Hello následující součásti jsou zapotřebí pro obnovení po havárii virtuálních počítačů VMware nebo fyzických serverů systému Windows nebo Linux. Toto jsou kromě těch, které jsou popsané v toohello [požadavky pro Azure](#azure-requirements).


### <a name="configuration-server-or-additional-process-server"></a>Konfigurace serveru nebo další procesového serveru

Nastavte na místním počítači jako hello konfigurace serveru tooorchestrate komunikace mezi hello místními servery a Azure. na místním počítači Hello také spravuje data replikace. <br/></br>

*   **VMware vCenter nebo vSphere hostitele požadovaných součástí**

    | **Komponenta** | **Požadavky** |
    | --- | --- |
    | **vSphere** | Musí mít jeden nebo více hypervisory VMware vSphere.<br/><br/>Hypervisory musí být spuštěna vSphere verze 6.0, 5.5 nebo 5.1, s hello nejnovější aktualizace.<br/><br/>Doporučujeme, aby vSphere hostitelů a serverů vCenter být v hello stejné síťové jako hello procesový server. Pokud jste nastavili vyhrazené procesový server, jedná se hello síť, kde se nachází hello konfigurační server. |
    | **vCenter** | Doporučujeme nasadit VMware vCenter server toomanage hostitelů vSphere. Musí být spuštěna vCenter verze 6.0 nebo 5.5, s nejnovějšími aktualizacemi hello.<br/><br/>**Omezení**: Site Recovery nepodporuje replikaci mezi instancemi řešení vMotion vCenter. Úložiště DRS a Storage vMotion také nejsou podporovány na hlavním cíli virtuálních počítačů po opětovné ochrany operaci.||

* **Požadavky replikovaného počítače**

    | **Komponenta** | **Požadavky** |
    | --- | --- |
    | **Místní počítače** (virtuální počítače VMware) | Replikované virtuální počítače musí mít nástroje VMware nainstalovaná a spuštěná.<br/><br/> Virtuální počítače, musí splňovat [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) pro vytvoření virtuálního počítače Azure.<br/><br/>Kapacita disku na každém chráněném počítači nemůže být větší než 1,023 GB. <br/><br/>Minimální 2 GB volného místa na disku pro instalaci hello je vyžadována pro instalaci součásti.<br/><br/>Pokud chcete konzistence tooenable více virtuálních počítačů, musíte otevřít port 20004 v místní bráně firewall hello virtuálních počítačů.<br/><br/>Názvy počítačů musí být mezi 1 a 63 znaků (můžete použít písmena, číslice a pomlčky). Hello název musí začínat písmenem nebo číslicí a končit písmenem nebo číslicí. <br/><br/>Až poprvé povolíte replikaci pro počítač, můžete upravit hello název Azure.<br/><br/> |
    | **Počítače s Windows** (fyzické nebo VMware) | Hello počítač musí používat jednu z následujících akcí hello podporované 64bitové operační systémy: <br/>-Windows Server 2012 R2<br/>– Windows Server 2012<br/>-Windows Server 2008 R2 s aktualizací SP1 nebo novější verze<br/><br/> Hello operační systém musí být nainstalován na disku jednotky C. hello operačního systému musí být základní disk systému Windows a nikoli dynamické. Hello datový disk může být dynamické.<br/><br/>|
    | **Počítače se systémem Linux** (fyzické nebo VMware) | Hello počítač musí používat jednu z následujících akcí hello podporované 64bitové operační systémy: <br/>-Red Hat Enterprise Linux 7.2, 7.1, 6.8 nebo 6.7<br/>-Centos 7.2, 7.1, 7.0, 6.8, 6.7, 6.6 nebo 6.5<br/>-Ubuntu 14.04 LTS serveru (seznam verzí podporované jádra pro Ubuntu najdete v tématu [podporované operační systémy](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Oracle Enterprise Linux verze 6.5 nebo 6.4, spuštěný buď hello Red Hat kompatibilní s jádrem nebo nedělitelné Enterprise jádra verze 3 (UEK3)<br/>-SUSE Linux Enterprise Server 11 SP4 nebo SUSE Linux Enterprise Server 11 SP3<br/><br/>Položky, které mapují hello místního hostitele název tooIP adres přidružených všechny síťové adaptéry musí mít vaše soubory/etc/hosts na chráněných počítačích.<br/><br/>Po převzetí služeb při selhání Pokud chcete tooconnect tooan virtuálního počítače Azure se systémem Linux pomocí klienta Secure Shell (SSH), ověřte, zda na počítači hello chráněné hello SSH službu toostart automaticky při spuštění systému. Také se ujistěte, že povolit pravidla brány firewall se SSH připojení toohello chráněný počítač.<br/><br/>název hostitele Hello, přípojné body, zařízení a Linux systémové cesty a názvy souborů (například/etc / a USR) musí být v angličtině jenom.<br/><br/>Hello následující adresáře (Pokud nastavený jako samostatné oddíly nebo systému souborů) musí být na hello stejný disk (disk hello operačního systému) na zdrojovém serveru hello:<br/>- kořenovém<br/>- / spouštěcí<br/>-usr<br/>-/ usr/místní<br/>-/var<br/>- / atd<br/><br/>V současné době XFS v5 funkcí, jako například metadata kontrolního součtu, nejsou podporovány službou Site Recovery v systémech souborů s XFS. Ověřte, jestli nejsou vaše systémy souborů XFS funkcí v5. Pro oddíl hello můžete hello xfs_info nástroj toocheck hello XFS tzv. Pokud **ftype** je nastaven příliš**1**, XFS v5 funkce jsou používány.<br/><br/>Na serverech Red Hat Enterprise Linux 7 a CentOS 7 hello lsof nástroj musí být nainstalovaná a k dispozici.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-tooazure-no-vmm"></a>Zotavení po havárii tooAzure virtuálních počítačů Hyper-V (bez VMM)

Hello následující součásti jsou zapotřebí pro obnovení po havárii virtuálních počítačů technologie Hyper-V v cloudech VMM. Toto jsou kromě těch, které jsou popsané v toohello [požadavky pro Azure](#azure-requirements).

| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Hostitel Hyper-V** |Jeden nebo více místními servery musí používat Windows Server 2012 R2 s nejnovější aktualizace hello a povolenou roli technologie Hyper-V hello nebo Microsoft Hyper-V Server 2012 R2.<br/><br/>Servery Hyper-V musí mít jeden nebo více virtuálních počítačů.<br/><br/>Servery Hyper-V musí být připojené toohello Internetu, buď přímo nebo prostřednictvím proxy serveru.<br/><br/>Servery Hyper-V musí mít hello opravy popsané v článku znalostní báze hello [2961977](https://support.microsoft.com/kb/2961977) nainstalována.
|**Zprostředkovatel a agent**| Během nasazování Site Recovery nainstalujete zprostředkovatele Azure Site Recovery hello. instalace zprostředkovatele Hello nainstaluje taky hello agenta služeb zotavení Azure na každém serveru technologie Hyper-V, který je spuštěn virtuální počítače, který chcete tooprotect. <br/><br/>Všechny servery technologie Hyper-V v Site Recovery vault musí mít hello stejné verze hello zprostředkovatel a hello agent.<br/><br/>tooconnect tooSite obnovení přes hello Internetu, musí zprostředkovatel Hello. Může být odeslán provoz přímo nebo prostřednictvím proxy serveru. Založený na protokolu HTTPS proxy není podporováno. Hello proxy serveru musí umožňovat přístup toohello následující adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Pokud máte na serveru hello pravidla brány firewall založená na adresu IP, ujistěte se, že hello pravidla umožňují komunikaci tooAzure.<br/><br/> Povolit hello [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653)a protokol HTTPS (port 443).<br/><br/> Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a hello západní USA (používá se pro přístup k řízení a identity management).


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-tooazure"></a>Zotavení po havárii virtuálních počítačů technologie Hyper-V v nástroji VMM cloudů tooAzure

Hello následující součásti jsou zapotřebí pro obnovení po havárii virtuálních počítačů technologie Hyper-V v cloudech VMM. Toto jsou kromě těch, které jsou popsané v toohello [požadavky pro Azure](#azure-requirements).

| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Nástroj Virtual Machine Manager** |Musí mít jeden nebo více serverů VMM běžících na System Center 2012 R2 nebo novější. Každý server VMM musí mít minimálně jeden cloud nakonfigurovaný. <br/><br/>V cloudu, musíte mít:<br/>-Jeden nebo více skupinu hostitelů VMM.<br/>-Jeden nebo více hostitelské servery Hyper-V nebo clusterů v každé skupině hostitelů.<br/><br/>Další informace o nastavení cloudů VMM najdete v tématu [jak toocreate cloudu v nástroji Virtual Machine Manager 2012](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx). |
| **Hyper-V** |Hostitelské servery Hyper-V musí běžet minimálně Windows Server 2012 R2 s povolenou roli hello technologie Hyper-V nebo Microsoft Hyper-V Server 2012 R2. musí být nainstalované nejnovější aktualizace Hello.<br/><br/> Server Hyper-V musí mít jeden nebo více virtuálních počítačů.<br/><br/> Hostitelský server Hyper-V nebo cluster, který obsahuje virtuální počítače, které chcete tooreplicate musí být spravovaný v cloudu VMM.<br/><br/>Servery Hyper-V musí být připojené toohello Internetu, buď přímo nebo prostřednictvím proxy serveru.<br/><br/>Servery Hyper-V musí mít hello opravy popsané v článku znalostní báze hello [2961977](https://support.microsoft.com/kb/2961977) nainstalována.<br/><br/>Hostitelské servery Hyper-V pro tooAzure replikace dat potřebovat přístup k Internetu. |
| **Zprostředkovatel a agent** |Během nasazování Azure Site Recovery nainstalujte zprostředkovatele Azure Site Recovery na serveru VMM hello. Nainstalujte agenta služeb zotavení na hostitele Hyper-V. Hello zprostředkovatel a agent, potřebujete tooconnect tooAzure přímo přes hello Internet nebo prostřednictvím proxy serveru. Proxy server založený na protokolu HTTPS se nepodporuje. Hello proxy server na hello VMM server a hostitelé technologie Hyper-V musí umožňovat přístup k: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Pokud máte na serveru VMM hello pravidla brány firewall založená na adresu IP, ujistěte se, že hello pravidla umožňují komunikaci tooAzure.<br/><br/> Povolit hello [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) a HTTPS (port 443).<br/><br/>Povolte rozsahy IP adres pro hello oblast Azure pro vaše předplatné a hello západní USA (používá se pro přístup k řízení a identity management).<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Požadavky replikovaného počítače

| **Komponenta** | **Podrobnosti** |
| --- | --- |
| **Chráněné virtuální počítače** | Site Recovery podporuje všechny operační systémy, které jsou podporovány [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).<br/><br/>Virtuální počítače musí splňovat hello [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) pro vytvoření virtuálního počítače Azure. Názvy počítačů musí být mezi 1 a 63 znaků (můžete použít písmena, číslice a pomlčky). Hello název musí začínat písmenem nebo číslicí a končit písmenem nebo číslicí. <br/><br/>Název virtuálního počítače hello můžete upravit, můžete po povolení replikace pro hello virtuálních počítačů. <br/><br/> Kapacita disku pro každý chráněný počítač nemůže být větší než 1,023 GB. Virtuální počítač může mít too16 disky (až too16 TB).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-tooa-customer-owned-site"></a>Zotavení po havárii virtuálních počítačů technologie Hyper-V v nástroji VMM cloudů tooa vlastněných zákazníků lokality

Hello následující součásti jsou zapotřebí pro obnovení po havárii virtuálních počítačů technologie Hyper-V ve VMM cloudy tooa vlastněných zákazníků Web. Toto jsou kromě těch, které jsou popsané v toohello [požadavky pro Azure](#azure-requirements).

| **Komponenta** | **Podrobnosti** |
| --- | --- |
| **Nástroj Virtual Machine Manager** |  Doporučujeme nasadit na server VMM v hello primární a sekundární lokality hello.<br/><br/> Můžete [replikace mezi cloudy na jednom serveru VMM](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). tooreplicate mezi cloudy na jednom serveru VMM, budete potřebovat aspoň dva cloudy, které jsou nakonfigurované na serveru VMM hello.<br/><br/> Servery VMM musí používat minimálně System Center 2012 SP1, s nejnovějšími aktualizacemi hello.<br/><br/> Každý server VMM musí mít minimálně jeden cloud. Všechny cloudy musí mít hello nastavit profil kapacity Hyper-V. <br/><br/>Cloudy musí mít jeden nebo více skupin hostitelů VMM. Další informace o nastavení cloudů VMM najdete v tématu [Příprava pro nasazení Azure Site Recovery](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Hyper-V** | Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí hello technologie Hyper-V povolena a mít hello nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V musí mít jeden nebo více virtuálních počítačů.<br/><br/>  Hostitelské servery Hyper-V se musí nacházet ve skupinách hostitelů v hello primárních a sekundárních cloudech VMM.<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012 R2, doporučujeme nainstalovat hello aktualizaci popsanou v článku znalostní báze Knowledge Base [2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Pokud se spuštění technologie Hyper-V v clusteru na Windows Server 2012 a máte statické IP adresy na základě clusteru, zprostředkovatele clusteru se nevytvoří automaticky. Je nutné ručně nakonfigurovat zprostředkovatele clusteru hello. Další informace o zprostředkovateli hello clusteru najdete v tématu [konfigurovat hello repliky roli zprostředkovatele clusteru na clusteru replikace](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Poskytovatel** | Během nasazování Site Recovery nainstalujte zprostředkovatele Azure Site Recovery hello na serverech VMM. Hello zprostředkovatel komunikováním se službou Site Recovery prostřednictvím protokolu HTTPS (port 443) tooorchestrate replikace. Dojde k replikaci dat mezi hello primární a sekundární servery Hyper-V přes hello LAN nebo prostřednictvím připojení VPN.<br/><br/> Hello zprostředkovatele, který je spuštěn na serveru VMM hello potřebuje přístup k toohello následující adresy URL:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>zprostředkovatele Site Recovery Hello musí umožňovat komunikaci brány firewall z toohello servery VMM hello [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653)a povolit protokol HTTPS (port 443) hello. |


## <a name="url-access"></a>Adresa URL přístup
Tyto adresy URL musí být dostupný z hostitelské servery technologie Hyper-V, VMware a VMM:

|**ADRESA URL** | **VMM tooVMM** | **VMM tooAzure** | **TooAzure technologie Hyper-V** | **VMware tooAzure** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | Povolit | Povolit | Povolit | Povolit |
|``*.backup.windowsazure.com`` | Nepožaduje se | Povolit | Povolit | Povolit |
|``*.hypervrecoverymanager.windowsazure.com`` | Povolit | Povolit | Povolit | Povolit |
|``*.store.core.windows.net`` | Povolit | Povolit | Povolit | Povolit |
|``*.blob.core.windows.net`` | Nepožaduje se | Povolit | Povolit | Povolit |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Nepožaduje se | Nepožaduje se | Nepožaduje se | Povolit stažení SQL |
|``time.windows.com`` | Povolit | Povolit | Povolit | Povolit|
|``time.nist.gov`` | Povolit | Povolit | Povolit | Povolit |
