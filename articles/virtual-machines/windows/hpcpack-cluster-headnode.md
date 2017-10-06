---
title: "aaaCreate hlavnímu uzlu HPC Pack virtuální počítač Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure portal a hello nasazení Resource Manager modelu toocreate hlavního uzlu na virtuální počítač Azure Microsoft HPC Pack 2012 R2."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: e6a13eaf-9124-47b4-8d75-2bc4672b8f21
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 3ddefb74b053a48a15f1ba1ca8edbc0192da51a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hello-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Vytvořit virtuální počítač Azure s image pořízenou prostřednictvím Marketplace hello hlavního uzlu clusteru HPC Pack
Použití [bitovou kopii virtuálního počítače Microsoft HPC Pack 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) z hello Azure Marketplace a hello Azure portálu toocreate hello hlavního uzlu v clusteru HPC. Tuto bitovou kopii virtuálního počítače HPC Pack je založená na Windows Server 2012 R2 Datacenter s HPC Pack 2012 R2 Update 3 předinstalován. Pomocí tohoto hlavního uzlu pro testování konceptu nasazení sady HPC Pack v Azure. Poté můžete přidat výpočetní uzly toohello clusteru toorun úloh prostředí HPC.

> [!TIP]
> toodeploy dokončení clusteru HPC Pack 2012 R2 v Azure, který zahrnuje hello hlavního uzlu a výpočetní uzly, doporučujeme použít automatizované metodu. Mezi možnosti patří hello [skript nasazení HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) a šablony správce prostředků, jako je například hello [HPC Pack clusteru pro úlohy Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/). Šablony Resource Manageru jsou také k dispozici pro [clusterů Microsoft HPC Pack 2016](https://github.com/MsHpcPack/HPCPack2016/tree/master/newcluster-templates). 
> 
> 

## <a name="planning-considerations"></a>Aspekty plánování
Jak ukazuje následující obrázek hello, nasadíte hello HPC Pack hlavního uzlu v doméně služby Active Directory v virtuální sítě Azure.

![Hlavního uzlu HPC Pack][headnode]

* **Doména služby Active Directory**: hello HPC Pack 2012 R2 hlavního uzlu musí být připojené k doméně služby Active Directory tooan v Azure před zahájením služby HPC hello na hello virtuálních počítačů. Jak je znázorněno v tomto článku, testování konceptu nasazení, můžete zvýšit úroveň hello virtuálních počítačů, které vytvoříte pro hello hlavního uzlu jako řadič domény před spuštěním služby HPC hello. Další možností je toodeploy řadič samostatné domény a doménové struktury v Azure toowhich, které připojíte hello hlavního uzlu virtuálního počítače.

* **Model nasazení**: pro většinu nová nasazení, Microsoft doporučuje používat model nasazení Resource Manager hello. Tento článek předpokládá, že používáte tento model nasazení.

* **Virtuální síť Azure**: Pokud používáte hello Resource Manager nasazení modelu toodeploy hello hlavního uzlu, zadejte, nebo vytvořte virtuální síť Azure. Virtuální síť hello použijte, pokud potřebujete toojoin hello hlavního uzlu tooan existující doménu služby Active Directory. Je také nutné ho novější tooadd výpočetním uzlu clusteru toohello virtuálních počítačů.

## <a name="steps-toocreate-hello-head-node"></a>Kroky toocreate hello hlavního uzlu
Následují základní kroky toouse hello Azure portálu toocreate virtuální počítač Azure pro hlavního uzlu HPC Pack hello pomocí modelu nasazení Resource Manager hello. 

1. Pokud chcete toocreate nové doménové služby Active Directory v Azure s řadičem domény samostatné virtuální počítače, jednou z možností je toouse [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc). Pro jednoduché ověření konceptu nasazení má přesně tooomit tento krok a konfigurace hlavního uzlu hello virtuální počítač jako řadič domény. Tato možnost je popsán dále v tomto článku.
2. Na hello [HPC Pack 2012 R2 na Windows Server 2012 R2 stránce](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) v hello Azure Marketplace, klikněte na **vytvořit virtuální počítač**. 
3. Hello portálu na hello **HPC Pack 2012 R2 na Windows Server 2012 R2** stránky, vyberte hello **Resource Manager** modelu nasazení a pak klikněte na tlačítko **vytvořit**.
   
    ![Obrázek HPC Pack][marketplace]
4. Použít nastavení hello portálu tooconfigure hello a vytvořit hello virtuálních počítačů. Pokud jste nový tooAzure, postupujte podle kurzu hello [vytvoření virtuálního počítače s Windows v portálu Azure hello](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Pro ověření konceptu nasazení můžete obvykle přijmout výchozí hello nebo doporučené nastavení.
   
   > [!NOTE]
   > Pokud chcete toojoin hello hlavního uzlu tooan stávající doméně Active Directory v Azure, ujistěte se, že při vytváření hello virtuálních počítačů musíte zadat hello virtuální sítě pro tuto doménu.
   > 
   > 
5. Po vytvoření hello virtuálních počítačů a hello je spuštěný virtuální počítač, [připojit toohello virtuálních počítačů](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pomocí vzdálené plochy. 
6. Připojit k doménové struktury Active Directory domény hello virtuálních počítačů tooan volbou jedné z hello následující možnosti:
   
   * Pokud jste vytvořili hello virtuálního počítače do virtuální sítě Azure s existující doménové struktuře domény, připojte doménové struktury toohello hello virtuálních počítačů pomocí standardních nástrojů Správce serveru nebo prostředí Windows PowerShell. Potom restartujte.
   * Pokud jste vytvořili hello virtuálních počítačů v nové virtuální sítě (bez existující doménové struktuře domény), pak povýšíte hello virtuální počítač jako řadič domény. Použijte standardní postup tooinstall a nakonfigurujte role Active Directory Domain Services hello hello hlavního uzlu. Podrobné pokyny najdete v tématu [nainstalovat nové Windows Server 2012 Active Directory doménové struktury](https://technet.microsoft.com/library/jj574166.aspx).
7. Po hello virtuální počítač běží a je připojený k tooan doménové struktury služby Active Directory spusťte služby HPC Pack hello následujícím způsobem:
   
    a. Připojte toohello hlavního uzlu virtuálního počítače pomocí účtu domény, který je členem místní skupiny Administrators hello. Například použijte účet správce hello, kterou vytvoříte, když jste vytvořili hello hlavního uzlu virtuálního počítače.
   
    b. Pro výchozí konfigurace hlavního uzlu spusťte prostředí Windows PowerShell jako správce a zadejte následující hello:
   
    ```PowerShell
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```
   
    Ho může trvat několik minut, než toostart služby HPC Pack hello.
   
    Pro další hlavního uzlu možnosti konfigurace, zadejte `get-help HPCHNPrepare.ps1`.

## <a name="next-steps"></a>Další kroky
* Teď můžete pracovat s hello hlavního uzlu v clusteru HPC Pack. Například spusťte Správce clusteru HPC a dokončení hello [nasazení na seznam úkolů](https://technet.microsoft.com/library/jj884141.aspx).
* Pokud chcete, aby tooincrease hello clusteru výpočetní kapacity na vyžádání, přidejte [Azure burst uzly](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) v cloudové službě. 
* Zkuste spustit test zatížení v clusteru hello. Příklad najdete v tématu hello HPC Pack [Příručka Začínáme](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/hpcpack-cluster-headnode/marketplace.png
