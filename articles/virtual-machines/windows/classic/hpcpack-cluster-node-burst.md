---
title: "aaaAdd shluků uzly tooan HPC Pack clusteru | Microsoft Docs"
description: "Zjistěte, jak tooexpand HPC Pack cluster v Azure na vyžádání přidáním instancí role pracovního procesu spuštěných v rámci cloudové služby"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 24b79a8a-24ad-4002-ae76-75abc9b28c83
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 7ec40ffe76485742c9e458ec49e11805990974e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-on-demand-burst-nodes-tooan-hpc-pack-cluster-in-azure"></a>Přidat na vyžádání "rozšíření" uzly tooan HPC Pack cluster v Azure
Pokud jste nastavili [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) clusteru v Azure, můžete způsob tooquickly škálování hello clusteru kapacity nahoru nebo dolů, bez zachování sadu předkonfigurované výpočetním uzlu virtuálních počítačů. Tento článek ukazuje, jak tooadd na vyžádání "rozšíření" uzly (spuštění v cloudové službě instance role pracovního procesu.) jako výpočetní prostředky tooa hlavního uzlu v Azure. 

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

![Uzly shluku][burst]

pomůžou vám rychle přidání uzlů Azure Hello kroky v tomto článku tooa cloudové HPC Pack hlavního uzlu virtuálního počítače pro nasazení testu nebo testování konceptu. Postup vysoké úrovně Hello jsou hello stejné jako kroky hello příliš "burst tooAzure" tooadd cloudové výpočetní kapacitu tooan místního prostředí HPC Pack clusteru. Podívejte se kurz [nastavení hybridního výpočetního clusteru pomocí sady Microsoft HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Podrobné pokyny pro nasazení v produkčním prostředí, najdete v části [Burst tooAzure pomocí sady Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

## <a name="prerequisites"></a>Požadavky
* **Nasadit virtuální počítač Azure hlavního uzlu HPC Pack** – můžete použít samostatné hlavního uzlu virtuálního počítače nebo ten, který je součástí clusteru s podporou větší. toocreate samostatné hlavního uzlu, najdete v části [nasazení HPC Pack hlavní uzel virtuální počítač Azure](../../virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Automatizované možnosti nasazení clusteru HPC Pack najdete v tématu [možnosti toocreate a spravovat cluster Windows HPC v Azure pomocí sady Microsoft HPC Pack](../../virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
  > [!TIP]
  > Pokud používáte hello [skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md) toocreate hello cluster v Azure, můžete zahrnout uzlů Azure shluků automatického nasazení. Příklady hello v tomto článku.
  > 
  > 
* **Předplatné Azure** -tooadd Azure uzly, můžete zvolit hello stejné předplatné použité toodeploy hello hlavního uzlu virtuálního počítače, nebo jiné předplatné (nebo odběry).
* **Kvóta jader** -může být nutné tooincrease hello kvóty jader, obzvláště pokud se rozhodnete toodeploy několik uzlů Azure s vícejádrovými velikostí. tooincrease kvótu, [otevřete žádosti o podporu online zákazníka](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zdarma.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-hello-azure-nodes"></a>Krok 1: Vytvoření cloudové služby a účet úložiště pro hello uzlů Azure
Použijte hello portál Azure classic nebo ekvivalentní nástroje tooconfigure hello následující prostředky, které jsou potřebné toodeploy Azure uzly:

* Nové služby Azure cloud
* Nový účet úložiště Azure

> [!NOTE]
> Nemusíte znovu použít stávající cloudovou službu ve vašem předplatném. 
> 
> 

**Důležité informace**

* Nakonfigurujte samostatnou cloudovou službu pro každé šablony Azure uzlu, abyste naplánovali toocreate. Můžete však použít stejný účet úložiště pro víc šablony uzlu hello.
* Doporučujeme, abyste v hello najít hello Cloudová služba a hello účet úložiště pro nasazení hello stejné oblasti Azure.

## <a name="step-2-configure-an-azure-management-certificate"></a>Krok 2: Konfigurace certifikát pro správu Azure
tooadd Azure uzly jako výpočetní prostředky, musíte certifikát pro správu na hello hlavního uzlu a nahrání odpovídající certifikátu toohello předplatné Azure použité pro nasazení hello.

V tomto scénáři můžete hello **výchozí certifikát pro správu Azure HPC** který HPC Pack nainstaluje a nakonfiguruje automaticky hlavního uzlu. Tento certifikát je užitečné pro testovací účely a testování konceptu nasazení. toouse tento certifikát, nahrajte 2012\Bin\hpccert.cer C:\Program Files\Microsoft HPC Pack souboru z hlavního uzlu hello předplatné toothe virtuálních počítačů. tooupload hello certifikátu v hello [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **nastavení** > **certifikáty pro správu**.

Certifikát pro správu hello tooconfigure další možnosti, najdete v části [scénáře tooConfigure hello certifikát pro správu Azure pro nasazení Azure Burst](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-toohello-cluster"></a>Krok 3: Nasazení clusteru toohello uzlů Azure
Hello tooadd kroky a spusťte Azure uzly v tomto scénáři jsou obecně hello stejné jako hello kroky hlavnímu uzlu místně. Další informace najdete v tématu hello následující části [kroky tooDeploy uzlů Azure pomocí sady Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Vytvořit šablonu Azure uzlu
* Přidejte cluster Windows HPC toohello uzlů Azure
* Spuštění (zřídit) hello uzlů Azure

Po přidání a spustit hello uzly, jsou připravené pro vás toouse toorun clusteru úlohy.

Pokud narazíte na potíže při nasazování uzlů Azure, najdete v části [řešení nasazení z uzlů Azure pomocí sady Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Další kroky
* toouse velikost instance náročné hello burst uzlů najdete v tématu hello aspekty [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Pokud chcete automaticky zvětšovat a zmenšovat hello výpočetních prostředků podle zatížení clusteru hello Azure najdete v tématu [automaticky zvětšovat a zmenšovat Azure výpočetních prostředků v clusteru služby HPC Pack](hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/hpcpack-cluster-node-burst/burst.png
