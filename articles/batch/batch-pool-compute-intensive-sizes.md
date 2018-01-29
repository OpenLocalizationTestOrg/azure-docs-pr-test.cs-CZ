---
title: "Pomocí náročné virtuálních počítačích Azure Batch | Microsoft Docs"
description: "Jak chcete využít výhod podporující RDMA nebo grafický procesor s podporou velikosti virtuálních počítačů ve fondech Azure Batch"
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
ms.date: 10/31/2017
ms.author: danlep
ms.openlocfilehash: 26cab5ba892d892e035bd94c52cacabd23eebd0c
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Používání podporující RDMA nebo grafický procesor s podporou instancí ve fondech Batch

Pokud chcete spustit určité úlohy Batch, můžete využít výhod určená pro rozsáhlé výpočtu velikosti virtuálního počítače Azure. Chcete-li například spustit víc instancí [úlohy MPI](batch-mpi.md), můžete zvolit A8, A9, nebo velikosti H-series, které mají síť rozhraní pro vzdálený přímý paměti přístup (RDMA). Tyto velikosti připojit k síti InfiniBand pro komunikaci mezi uzly, které můžou urychlit aplikací MPI. Nebo CUDA aplikace, můžete zvolit N-series velikostí, které zahrnují grafických NVIDIA tesla – Měrná jednotka (GPU) karty.

Tento článek obsahuje pokyny a příklady používat některé z Azure specializované velikosti ve fondech Batch. Specifikace a pozadí najdete v tématu:

* Vysoký výkon výpočetní velikosti virtuálních počítačů ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* Grafický procesor s podporou velikosti virtuálních počítačů ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Předplatné a limity účtu

* **Kvóty** – [kvóty vyhrazené jader na účtu Batch](batch-quota-limit.md#resource-quotas) může omezit počet nebo typ uzlů, které přidáte do fondu služby Batch. Se pravděpodobně k dosažení kvótu, když zvolíte podporu rdma, grafický procesor s podporou či jiné vícejádrovými velikosti virtuálních počítačů. Ve výchozím nastavení je tato kvóta 20 jader. Samostatné kvóta se vztahuje na [virtuální počítače s nízkou prioritou](batch-low-pri-vms.md), pokud je používáte. 

Pokud budete muset požádat o zvýšení kvóty, otevřete [žádost o podporu online zákazníka](../azure-supportability/how-to-create-azure-support-request.md) zdarma.

* **Dostupnost v oblastech** – náročné virtuální počítače nemusí být k dispozici v oblastech, kde můžete vytvořit účty Batch. Pokud chcete zkontrolovat, že velikost je k dispozici, najdete v části [produkty podle oblasti](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Závislosti

Funkce RDMA a GPU velikostí náročné jsou podporovány pouze v určitých operačních systémech. V závislosti na operačním systému může být potřeba instalovat nebo konfigurovat další ovladače nebo jiný software. Následující tabulka představuje souhrn tyto závislosti. Najdete v článcích propojené podrobnosti. Možnosti konfigurace fondy Batch najdete dále v tomto článku.


### <a name="linux-pools---virtual-machine-configuration"></a>Linux fondy - konfigurace virtuálního počítače

| Velikost | Schopnost | Operační systémy | Požadovaný software | Nastavení fondu. |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | Ubuntu 16.04 LTS,<br/>SUSE Linux Enterprise Server 12 HPC, nebo<br/>Na základě centOS HPC<br/>(Azure Marketplace) | Intel MPI 5 | Povolit komunikaci mezi uzly, zakažte provedení souběžné úlohy |
| [NC řady *](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-ncv2-and-nd-vms) | NVIDIA Tesla K80 GPU | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.3, nebo<br/>Distribuce založené na CentOS 7.3<br/>(Azure Marketplace) | Ovladače NVIDIA CUDA Toolkit 9.0 | neuvedeno | 
| [Řady VS](../virtual-machines/linux/n-series-driver-setup.md#install-grid-drivers-for-nv-vms) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.3, nebo<br/>Distribuce založené na CentOS 7.3<br/>(Azure Marketplace) | 4.3 mřížky NVIDIA ovladače | neuvedeno |

* Připojení RDMA na virtuálních počítačích NC24r je podporována v Ubuntu 16.04 LTS nebo na základě CentOS 7.3 HPC (z Azure Marketplace) s Intel MPI.



### <a name="windows-pools---virtual-machine-configuration"></a>Fondy Windows - konfigurace virtuálního počítače

| Velikost | Schopnost | Operační systémy | Požadovaný software | Nastavení fondu. |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2 or<br/>Windows Server 2012 (Azure Marketplace) | Microsoft MPI 2012 R2 nebo novější, nebo<br/> Intel MPI 5<br/><br/>Rozšíření virtuálního počítače Azure HpcVMDrivers | Povolit komunikaci mezi uzly, zakažte provedení souběžné úlohy |
| [NC řady *](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla K80 GPU | Windows Server 2016 nebo <br/>Windows Server 2012 R2 (Azure Marketplace) | Tesla – měrná NVIDIA ovladače nebo CUDA Toolkit 9.0 ovladače| neuvedeno | 
| [Řady VS](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Windows Server 2016 nebo<br/>Windows Server 2012 R2 (Azure Marketplace) | 4.3 mřížky NVIDIA ovladače | neuvedeno |

* Připojení RDMA na virtuálních počítačích NC24r je podporována v systému Windows Server 2012 R2 (z Azure Marketplace) s příponou HpcVMDrivers a Microsoft MPI nebo Intel MPI.

### <a name="windows-pools---cloud-services-configuration"></a>Fondy Windows - konfigurace cloudových služeb

> [!NOTE]
> N-series velikosti nejsou podporovány ve fondech Batch s konfiguraci cloudových služeb.
>

| Velikost | Schopnost | Operační systémy | Požadovaný software | Nastavení fondu. |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2,<br/>Windows Server 2012, or<br/>Windows Server 2008 R2 (hostovaného operačního systému rodiny) | Microsoft MPI 2012 R2 nebo novější, nebo<br/>Intel MPI 5<br/><br/>Rozšíření virtuálního počítače Azure HpcVMDrivers | Povolit komunikaci mezi uzly<br/> zakázat provedení souběžné úlohy |





## <a name="pool-configuration-options"></a>Možnosti konfigurace fondu

Ke konfiguraci specializované velikost virtuálního počítače pro fondu Batch, nástrojů pro rozhraní API služby Batch a zadejte několik možností instalace požadovaného softwaru a ovladače, včetně:

* [Spouštěcí úkol](batch-api-basics.md#start-task) -nahrajte balíček instalace jako soubor prostředků pro účet úložiště Azure ve stejné oblasti jako účet Batch. Vytvoření spuštění úloh příkazového řádku pro tichou instalaci souboru prostředků při spuštění fondu. Další informace najdete v tématu [dokumentace k REST API](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask).

  > [!NOTE] 
  > Spouštěcí úkol musíte spustit s oprávněními zvýšenými (správce) a musí počkat na úspěšné.
  >

* [Balíček aplikace](batch-application-packages.md) – přidat komprimované instalační balíček do vašeho účtu Batch a nakonfigurovat odkaz na balíček ve fondu. Toto nastavení odešle a unzips balíček na všech uzlech ve fondu. Pokud tento balíček je instalační program, vytvořte počáteční úlohu příkazového řádku k tiché instalaci aplikace na všech uzlech fondu. Volitelně můžete nainstalujte balíček, když je naplánován ke spuštění na uzlu.

* [Obrázek vlastní fond](batch-custom-images.md) – vytvoření vlastní image Windows nebo virtuálního počítače s Linuxem obsahující ovladače, softwaru, nebo jiné nastavení potřebných pro velikost virtuálního počítače. 

* [Batch loděnice](https://github.com/Azure/batch-shipyard) automaticky konfiguruje GPU a RDMA transparentně pracovat s kontejnerizované úlohy na Azure Batch. Batch loděnice zcela vycházejí s konfiguračními soubory. Existuje mnoho ukázka recepturách konfigurace k dispozici umožňující GPU a RDMA úlohy, jako [CNTK GPU recepturách](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) který předem nakonfiguruje ovladače grafického procesoru na virtuálních počítačích N-series a načte kognitivní nástrojů Microsoft software jako bitovou kopii Docker.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Příklad: Microsoft MPI u fondu virtuální počítač A8

Ke spouštění aplikací Windows MPI ve fondu Azure A8 uzlů, musíte nainstalovat podporovanou MPI implementace. Tady jsou ukázkový postup instalace [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) na Windows fondu pomocí balíčku aplikace Batch.

1. Stažení [instalační balíček](http://go.microsoft.com/FWLink/p/?LinkID=389556) (MSMpiSetup.exe) na nejnovější verzi Microsoft MPI.
2. Vytvořte soubor zip balíčku.
3. Nahrání balíčku k účtu Batch. Pokyny najdete v tématu [balíčky aplikací](batch-application-packages.md) pokyny. Zadejte id aplikace, jako *MSMPI*, a verze, jako *8.1*. 
4. Pomocí rozhraní API služby Batch nebo portálu Azure, vytvořte fond v konfiguraci cloudových služeb s požadovaný počet uzlů a škálování. Následující tabulka uvádí nastavení ukázkových nastavit MPI v bezobslužném režimu pomocí spouštěcí úkol:

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

Ke spuštění CUDA aplikací ve fondu uzlů Linux NC, musíte nainstalovat CUDA Toolkit 9.0 na uzlech. Sada nástrojů nainstaluje potřebné ovladače NVIDIA tesla – měrná GPU. Zde jsou ukázkový postup nasazení vlastní image Ubuntu 16.04 LTS s ovladači GPU:

1. Nasaďte virtuální počítač Azure NC6 systémem Ubuntu 16.04 LTS. Můžete například vytvořte virtuální počítač v oblasti USA – jih centrální. Zajistěte, aby vytvořit virtuální počítač se spravovaným diskem.
2. Postupujte podle kroků pro připojení k virtuálnímu počítači a [instalaci ovladačů CUDA](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-ncv2-and-nd-vms).
3. Zrušení zřízení agenta systému Linux a potom [zachycení bitové kopie virtuálního počítače s Linuxem](../virtual-machines/linux/capture-image.md).
4. Vytvořte účet Batch v oblasti, která podporuje virtuální počítače názvového kontextu.
5. Pomocí rozhraní API služby Batch nebo portál Azure, vytvořte fond [pomocí vlastní image](batch-custom-images.md) a požadovaný počet uzlů a škálování. Následující tabulka uvádí nastavení fondu ukázka bitové kopie:

| Nastavení | Hodnota |
| ---- | ---- |
| **Obrázek – typ** | Vlastní Image |
| **Vlastní Image** | Název bitové kopie |
| **Uzel agenta SKU** | batch.node.ubuntu 16.04 |
| **Velikost uzlu** | NC6 Standard |



## <a name="next-steps"></a>Další kroky

* Spouštění úloh MPI ve fondu Azure Batch, najdete v článku [Windows](batch-mpi.md) nebo [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) příklady.

* Příklady úloh GPU na Batch, najdete v článku [Batch loděnice](https://github.com/Azure/batch-shipyard/) recepty.