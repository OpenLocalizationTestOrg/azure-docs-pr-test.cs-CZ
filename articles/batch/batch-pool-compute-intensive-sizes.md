---
title: "aaaUse náročné virtuální počítače Azure pomocí služby Batch | Microsoft Docs"
description: "Jak velikosti tootake výhod podporující RDMA nebo grafický procesor s podporou virtuálních počítačů ve fondech Azure Batch"
services: batch
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: 
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: danlep
ms.openlocfilehash: 6a462a5f2a44ddcec8bf4e5c200d444cac8fafe6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Používání podporující RDMA nebo grafický procesor s podporou instancí ve fondech Batch

toorun určité úlohy Batch můžete chtít tootake výhod určená pro rozsáhlé výpočtu velikosti virtuálního počítače Azure. Například s více instancemi toorun [úlohy MPI](batch-mpi.md), můžete zvolit A8, A9, nebo velikosti H-series, které mají síť rozhraní pro vzdálený přímý paměti přístup (RDMA). Tyto velikosti připojit síť InfiniBand tooan pro komunikaci mezi uzly, které můžou urychlit aplikací MPI. Nebo CUDA aplikace, můžete zvolit N-series velikostí, které zahrnují grafických NVIDIA tesla – Měrná jednotka (GPU) karty.

Tento článek obsahuje pokyny a příklady toouse některé specializované velikostí Azure ve fondech Batch. Specifikace a pozadí najdete v tématu:

* Vysoký výkon výpočetní velikosti virtuálních počítačů ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* Grafický procesor s podporou velikosti virtuálních počítačů ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Předplatné a limity účtu

* **Kvóty** – jeden nebo více kvótách Azure může omezit počet hello nebo typ uzlů můžete přidat tooa fondu služby Batch. Jste pravděpodobnější toobe omezené když zvolíte podporu rdma, GPU povoleno, nebo jiné vícejádrovými velikosti virtuálních počítačů. V závislosti na typu hello účtu Batch, kterou jste vytvořili může použít kvóty hello toohello účet sám sebe nebo tooyour předplatné.

    * Pokud jste vytvořili vašeho účtu Batch v hello **služba Batch** konfigurace, je omezeno hello [kvóty vyhrazené jader na účtu Batch](batch-quota-limit.md#resource-quotas). Ve výchozím nastavení je tato kvóta 20 jader. Samostatné kvótu platí příliš[virtuální počítače s nízkou prioritou](batch-low-pri-vms.md), pokud je používáte. 

    * Pokud jste vytvořili účet hello v hello **uživatele předplatné** konfigurace, vaše předplatné omezuje hello počet jader virtuálního počítače na oblast. V tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md). Vaše předplatné se týká také místní kvóty toocertain velikosti virtuálních počítačů, včetně HPC a GPU instancí. V konfiguraci předplatné hello uživatele použít žádné další kvóty účtu Batch toohello. 

  Může být nutné tooincrease jeden nebo více kvóty při použití specializované velikost virtuálního počítače v dávce. toorequest zvýšení kvóty, otevřete [žádost o podporu online zákazníka](../azure-supportability/how-to-create-azure-support-request.md) zdarma.

* **Dostupnost v oblastech** – náročné virtuální počítače nemusí být k dispozici v hello oblastech, kde můžete vytvořit účty Batch. toocheck, že velikost je k dispozici, najdete v části [produkty podle oblasti](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Závislosti

Hello RDMA a možnosti GPU velikostí náročné jsou podporovány pouze v určitých operačních systémech. V závislosti na operačním systému může být nutné tooinstall nebo nakonfigurovat další ovladače nebo jiný software. Následující tabulky Hello shrnují tyto závislosti. Najdete v článcích propojené podrobnosti. Možnosti fondů služby Batch tooconfigure najdete dále v tomto článku.


### <a name="linux-pools---virtual-machine-configuration"></a>Linux fondy - konfigurace virtuálního počítače

| Velikost | Schopnost | Operační systémy | Požadovaný software | Nastavení fondu. |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | SUSE Linux Enterprise Server 12 HPC nebo<br/>Na základě centOS HPC<br/>(Azure Marketplace) | Intel MPI 5 | Povolit komunikaci mezi uzly, zakažte provedení souběžné úlohy |
| [NC řady *](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms) | Tesla – měrná K80 NVIDIA GPU | Ubuntu 16.04 LTS.<br/>Red Hat Enterprise Linux 7.3, nebo<br/>Distribuce založené na CentOS 7.3<br/>(Azure Marketplace) | Ovladače NVIDIA CUDA Toolkit 8.0 | Není k dispozici | 
| [Řady VS](../virtual-machines/linux/n-series-driver-setup.md#install-grid-drivers-for-nv-vms) | Tesla – měrná M60 NVIDIA GPU | Ubuntu 16.04 LTS<br/>Red Hat Enterprise Linux 7.3<br/>Distribuce založené na CentOS 7.3<br/>(Azure Marketplace) | 4.3 mřížky NVIDIA ovladače | Není k dispozici |

* Připojení RDMA na virtuálních počítačích NC24r je podporované na základě CentOS 7.3 HPC s Intel MPI.



### <a name="windows-pools---virtual-machine-configuration"></a>Fondy Windows - konfigurace virtuálního počítače

| Velikost | Schopnost | Operační systémy | Požadovaný software | Nastavení fondu. |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2 nebo<br/>Windows Server 2012 (Azure Marketplace) | Microsoft MPI 2012 R2 nebo novější, nebo<br/> Intel MPI 5<br/><br/>Rozšíření virtuálního počítače Azure HpcVMDrivers | Povolit komunikaci mezi uzly, zakažte provedení souběžné úlohy |
| [NC řady *](../virtual-machines/windows/n-series-driver-setup.md) | Tesla – měrná K80 NVIDIA GPU | Windows Server 2016 nebo <br/>Windows Server 2012 R2 (Azure Marketplace) | Tesla – měrná NVIDIA ovladače nebo ovladače CUDA Toolkit 8.0| Není k dispozici | 
| [Řady VS](../virtual-machines/windows/n-series-driver-setup.md) | Tesla – měrná M60 NVIDIA GPU | Windows Server 2016 nebo<br/>Windows Server 2012 R2 (Azure Marketplace) | 4.3 mřížky NVIDIA ovladače | Není k dispozici |

* Připojení RDMA na virtuálních počítačích NC24r je podporována v systému Windows Server 2012 R2 s příponou HpcVMDrivers a Microsoft MPI nebo Intel MPI.

### <a name="windows-pools---cloud-services-configuration"></a>Fondy Windows - konfigurace cloudových služeb

> [!NOTE]
> N-series velikosti nejsou podporovány ve fondech Batch s konfigurace hello cloudových služeb.
>

| Velikost | Schopnost | Operační systémy | Požadovaný software | Nastavení fondu. |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2<br/>Windows Server 2012, nebo<br/>Windows Server 2008 R2 (hostovaného operačního systému rodiny) | Microsoft MPI 2012 R2 nebo novější, nebo<br/>Intel MPI 5<br/><br/>Rozšíření virtuálního počítače Azure HpcVMDrivers | Povolit komunikaci mezi uzly<br/> zakázat provedení souběžné úlohy |





## <a name="pool-configuration-options"></a>Možnosti konfigurace fondu

tooconfigure specializované velikost virtuálního počítače pro fondu Batch, hello rozhraní API služby Batch a nástroje poskytují různé možnosti tooinstall požadované softwaru nebo ovladačů, včetně:

* [Spouštěcí úkol](batch-api-basics.md#start-task) -nahrát instalační balíček jako tooan souborů prostředků účtu úložiště Azure v hello stejné oblasti jako hello účtu Batch. Vytvořte počáteční úlohu příkazového řádku tooinstall hello zdrojový soubor bezobslužně při spuštění hello fondu. Další informace najdete v tématu hello [dokumentace k REST API](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask).

  > [!NOTE] 
  > spouštěcí úkol Hello musíte spustit s oprávněními zvýšenými (správce) a musí počkat na úspěšné.
  >

* [Balíček aplikace](batch-application-packages.md) – přidat komprimované instalační balíček tooyour účtu Batch a nakonfigurovat odkaz na balíček ve fondu hello. Toto nastavení odešle a unzips hello balíčku na všech uzlech ve fondu hello. Pokud je balíček hello instalační program, vytvořte spuštění úloh příkazového řádku toosilently instalace hello aplikaci na všech uzlech fondu. Volitelně můžete nainstalujte hello balíček při úloha je naplánovaná toorun na uzlu.

* [Obrázek vlastní fond](batch-api-basics.md#pool) – vytvoření vlastní Windows nebo vyžadované bitovou kopii virtuálního počítače s Linuxem, která obsahuje ovladače, softwaru nebo jiná nastavení pro hello velikost virtuálního počítače. Pokud jste vytvořili vašeho účtu Batch v konfigurace odběru hello uživatele, zadejte vlastní image hello fondu Batch. (Vlastní obrázky nejsou podporovány v účtech v konfiguraci služby Batch hello). Vlastní obrázky lze použít pouze s fondy v konfiguraci virtuálního počítače hello.

  > [!IMPORTANT]
  > Ve fondech Batch nemůžete použít aktuálně vlastní image vytvořené s spravované disky nebo s storage úrovně Premium.
  >



* [Batch loděnice](https://github.com/Azure/batch-shipyard) automaticky nakonfiguruje transparentně s kontejnerizované úlohy v Azure Batch toowork hello GPU a RDMA. Batch loděnice zcela vycházejí s konfiguračními soubory. Existuje mnoho ukázka recepturách konfigurace k dispozici umožňující GPU a RDMA úlohy, jako například hello [CNTK GPU recepturách](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) který předem nakonfiguruje ovladače grafického procesoru na virtuálních počítačích N-series a načte kognitivní nástrojů Microsoft software jako bitovou kopii Docker.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Příklad: Microsoft MPI u fondu virtuální počítač A8

toorun aplikací Windows MPI ve fondu Azure A8 uzlů, je nutné tooinstall podporované MPI implementace. Tady je ukázka kroky tooinstall [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) na Windows fondu pomocí balíčku aplikace Batch.

1. Stáhnout hello [instalační balíček](http://go.microsoft.com/FWLink/p/?LinkID=389556) (MSMpiSetup.exe) pro nejnovější verzi Microsoft MPI hello.
2. Vytvořte soubor zip balíčku hello.
3. Nahrajte účtu Batch tooyour hello balíčku. Pokyny najdete v tématu hello [balíčky aplikací](batch-application-packages.md) pokyny. Zadejte id aplikace, jako *MSMPI*, a verze, jako *8.1*. 
4. V konfiguraci hello cloudových služeb s hello požadovaného počtu uzlů a škálování pomocí hello rozhraní API služby Batch nebo portálu Azure, vytvořte fond. Hello následující tabulka uvádí nastavení tooset ukázka až MPI v bezobslužném režimu pomocí spouštěcí úkol:

| Nastavení | Hodnota |
| ---- | ----- | 
| **Obrázek – typ** | Cloud Services |
| **Řada operačního systému** | Windows Server 2012 R2 (řada operačního systému 4) |
| **Velikost uzlu** | A8 Standard |
| **Komunikace internodium povoleno** | True |
| **Maximální počet úkolů na uzlu** | 1 |
| **Odkazy na balíček aplikace** | MSMPI |
| **Spouštěcí úkol povoleno** | True<br>**Příkazový řádek** - `"cmd /c %AZ_BATCH_APP_PACKAGE_MSMPI#8.1%\\MSMpiSetup.exe -unattend -force"`<br/>**Identita uživatele** -autouser fondu, správce<br/>**Počkejte úspěch** – True

## <a name="example-nvidia-tesla-drivers-on-nc-vm-pool"></a>Příklad: NVIDIA tesla – měrná ovladačů ve fondu virtuálních počítačů NC

toorun CUDA aplikací ve fondu uzlů Linux NC, je nutné tooinstall CUDA Toolkit 8.0 na uzlech hello. Hello Toolkit nainstaluje potřebné ovladače NVIDIA tesla – měrná GPU hello. Zde je ukázka kroky toodeploy vlastní image Ubuntu 16.04 LTS s ovladači GPU hello:

1. Nasaďte virtuální počítač Azure NC6 systémem Ubuntu 16.04 LTS. Můžete například vytvořte hello virtuálních počítačů v oblasti USA – jih centrální hello. Ujistěte se, že vytvoříte hello virtuálních počítačů s standardního úložiště, a *bez* spravované disky.
2. Postupujte podle hello kroky tooconnect toohello virtuálních počítačů a [instalaci ovladačů CUDA](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms).
3. Zrušení zřízení hello agenta systému Linux a pak zachytíte image virtuálního počítače s Linuxem pomocí příkazů hello Azure CLI 1.0. Pokyny najdete v tématu [zachytit virtuální počítač Linux spuštěné v Azure](../virtual-machines/linux/capture-image-nodejs.md). Poznamenejte si hello Image identifikátor URI.
  > [!IMPORTANT]
  > Nepoužívejte Azure CLI 2.0 příkazy toocapture hello image pro Azure Batch. Příkazy hello 2.0 rozhraní příkazového řádku aktuálně zachytit pouze virtuální počítače vytvořené pomocí spravovaného disky.
  >
4. Vytvořte účet Batch se hello konfigurace odběru uživatele, v oblasti, která podporuje virtuální počítače názvového kontextu.
5. Pomocí hello rozhraní API služby Batch nebo portál Azure, vytvoření fondu pomocí vlastní image hello a s hello požadovaného počtu uzlů a škálování. Hello následující tabulka uvádí nastavení fondu ukázkových hello bitové kopie:

| Nastavení | Hodnota |
| ---- | ---- |
| **Obrázek – typ** | Vlastní Image |
| **Vlastní Image** | Obrázek URI hello formuláře`https://yourstorageaccountdisks.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd` |
| **Uzel agenta SKU** | batch.Node.Ubuntu 16.04 |
| **Velikost uzlu** | NC6 Standard |



## <a name="next-steps"></a>Další kroky

* toorun úloh MPI ve fondu Azure Batch, najdete v části hello [Windows](batch-mpi.md) nebo [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) příklady.

* Příklady úloh GPU na Batch najdete v tématu hello [Batch loděnice](https://github.com/Azure/batch-shipyard/) recepty.
