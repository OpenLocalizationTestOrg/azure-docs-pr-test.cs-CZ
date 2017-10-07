---
title: "aaaMATLAB clusterů na virtuálních počítačích | Microsoft Docs"
description: "Použít toocreate virtuální počítače Microsoft Azure MATLAB distribuovaný Server výpočetních clusterů toorun paralelní MATLAB úlohy náročné na výkon"
services: virtual-machines-windows
documentationcenter: 
author: mscurrell
manager: timlt
editor: 
ms.assetid: e9980ce9-124a-41f1-b9ec-f444c8ea5c72
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Windows
ms.workload: infrastructure-services
ms.date: 05/09/2016
ms.author: markscu
ms.openlocfilehash: 266749dbdcfefac42c94b74aa612bfee18075a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Vytvoření MATLAB distribuovaný Server výpočetních clusterů na virtuálních počítačích Azure
Použití Microsoft Azure virtuální počítačů toocreate jeden nebo více MATLAB distribuovaný výpočetní Server clusterů toorun paralelní MATLAB úlohy náročné na výkon. Instalaci softwaru MATLAB distribuovaný Server Computing toouse virtuálního počítače jako základní bitová kopie a použít šablonu Azure rychlý start nebo skript Azure PowerShell (k dispozici na [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) toodeploy a správě clusteru hello. Po nasazení připojte toohello clusteru toorun úlohy.

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>O MATLAB a MATLAB distribuované Computing serveru
Hello [MATLAB](http://www.mathworks.com/products/matlab/) platformy je optimalizovaná pro řešení problémů technické a vědecké. Uživatelé MATLAB s rozsáhlé simulace a úloh zpracování dat můžete použít společnost MathWorks paralelní výpočty produkty toospeed si jejich výpočetně náročných úloh využitím výpočetní clustery a mřížky služeb. [Paralelní výpočty nástrojů](http://www.mathworks.com/products/parallel-computing/) umožňuje uživatelům MATLAB paralelní aplikace a využít výhod více jádry grafickými procesory a výpočetní clustery. [MATLAB distribuovaný Server Computing](http://www.mathworks.com/products/distriben/) umožňuje uživatelům tooutilize MATLAB mnoho počítačů ve výpočetním clusteru.

Pomocí virtuální počítače Azure můžete vytvořit MATLAB distribuovaný Server výpočetních clusterů, kteří mají všechny hello stejné mechanismy dostupné toosubmit paralelní práce jako místní clustery, jako je například interaktivní úlohy, úlohy batch, nezávislé úlohy, a komunikaci úlohy. Použití Azure ve spojení s platformou MATLAB hello má mnoho výhod oproti tooprovisioning a pomocí tradiční místní hardware: velikostí rozsah virtuálního počítače, vytváření clusterů na vyžádání, platíte jenom za výpočetní prostředky hello používáte, a modely tootest Hello možnost ve velkém měřítku.  

## <a name="prerequisites"></a>Požadavky
* **Klientský počítač** – po nasazení budete potřebovat toocommunicate počítače klienta se systémem Windows s Azure a hello MATLAB distribuovaný Server Computing clusteru.
* **Prostředí Azure PowerShell** -najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) tooinstall ho na klientském počítači.
* **Předplatné Azure** – Pokud nemáte předplatné, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut. Pro větší clustery zvažte průběžnými platbami předplatné nebo jiné možnosti nákupu.
* **Kvóta jader** -tooincrease hello základní kvóta toodeploy může být nutné, velké clusteru nebo víc než jednom clusteru MATLAB distribuovaný Server Computing. tooincrease kvótu, [otevřete žádosti o podporu online zákazníka](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zdarma.
* **Licence MATLAB, paralelní výpočty sada nástrojů a MATLAB distribuovaný Server Computing** -hello skripty předpokládá, že hello [správce licencí hostované společnost MathWorks](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) se používá pro všechny licence.  
* **Software MATLAB distribuovaný Server Computing** -bude nainstalována na virtuální počítač, který se použije jako hello základní image virtuálního počítače pro cluster hello virtuálních počítačů.

## <a name="high-level-steps"></a>Kroky vysoké úrovně
toouse Azure virtuální počítače pro své clustery serverů distribuované Computing MATLAB hello se vyžadují následující postup vysoké úrovně. Podrobné pokyny naleznete v dokumentaci k hello doplňujícími hello rychlý start šablonu a skripty na [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Vytvořte základní image virtuálního počítače**  

   * Stáhněte a nainstalujte software MATLAB distribuovaný Server Computing na tento virtuální počítač.

     > [!NOTE]
     > Tento proces může trvat několik hodin, ale máte toodo ho po pro každou verzi MATLAB použijete.   
     >
     >
2. **Vytvořte jeden nebo více clusterů**  

   * Použijte hello zadaný skript prostředí PowerShell nebo použijte hello rychlý start šablony toocreate clusteru z hello základní image virtuálního počítače.   
   * Správa clusterů hello pomocí skriptu prostředí PowerShell hello zadaný, což vám umožní toolist, pozastavení, opětovné spuštění a odstranění clusterů.

## <a name="cluster-configurations"></a>Konfigurace clusteru
V současné době skriptu pro vytváření clusteru hello a šablony umožňují toocreate jeden MATLAB distribuovaný Server Computing topologie. Pokud chcete, vytvořte jeden nebo více dalších clusterů s každý cluster s odlišným počtem pracovní virtuální počítače, pomocí různých velikostí virtuálních počítačů a tak dále.

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB klienta a cluster v Azure
Hello MATLAB klientský uzel, uzel MATLAB plánovače úloh a MATLAB distribuovaný výpočetní Server "pracovník" uzly jsou všechny nakonfigurované jako virtuální počítače Azure ve virtuální síti, jak je znázorněno v následující hello obrázek.


* toouse hello clusteru, připojit pomocí vzdálené plochy toohello klientský uzel. uzel Hello klient spustí hello MATLAB klienta.
* Hello klientský uzel má sdílení souborů, které mají přístup všechny pracovní procesy.
* Správce licencí hostované společnost MathWorks se používá pro hello licence kontroluje všechny MATLAB softwaru.
* Ve výchozím nastavení je na pracovní hello virtuální počítače vytvoří jeden pracovní proces MATLAB distribuovaný Server Computing za jádra, ale můžete zadat všechny číslo.

## <a name="use-an-azure-based-cluster"></a>Použití clusteru služby založené na Azure
Jako u jiných typů MATLAB distribuovaný Server výpočetních clusterů, musíte toouse hello profil Správce clusteru v hello MATLAB klienta (v klientovi hello virtuálních počítačů) toocreate profil MATLAB plánovače úloh clusteru.

![Profil Správce clusteru](./media/matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Další kroky
* Pro podrobné pokyny toodeploy a spravovat MATLAB distribuovaný Server výpočetní clustery v Azure, najdete v části hello [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) úložiště obsahující hello šablony a skripty.
* Přejděte toohello [společnost MathWorks lokality](http://www.mathworks.com/) pro podrobnou dokumentaci pro MATLAB a MATLAB distribuovaný Server Computing.
