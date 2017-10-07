---
title: "aaaConfigure virtuální sítě v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tooconfigure existující virtuální síť a podsíť a použít na virtuální počítač s Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: a11ce8315e3c540e44aeacc9c5ee3dde014d4621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Konfigurace virtuální sítě v Azure DevTest Labs
Jak je popsáno v článku hello [přidat virtuální počítač s testovacím tooa artefakty](devtest-lab-add-vm-with-artifacts.md), při vytváření virtuálních počítačů v testovacím prostředí, určíte nakonfigurovaný virtuální sítě. Jeden scénář tohoto postupu je, pokud potřebujete tooaccess hello prostředkům corpnet z virtuálních počítačů pomocí virtuální sítě, která byla konfigurovaná s ExpressRoute nebo VPN typu site-to-site. znázornění technologie Hello následující části Jak tooadd existující virtuální sítě do testovacího prostředí na nastavení virtuální sítě, aby byla k dispozici toochoose při vytváření virtuálních počítačů.

## <a name="configure-a-virtual-network-for-a-lab-using-hello-azure-portal"></a>Konfigurace virtuální sítě pro testovacím prostředí pomocí hello portálu Azure
Hello následující kroky vás provede přidáním existující virtuální síť (a podsíť) tooa testovacího prostředí, aby se můžete použít při vytváření virtuálního počítače v hello stejné testovacího prostředí. 

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
3. Ze seznamu hello labs vyberte požadované prostředí hello. 
4. V okně prostředí hello vyberte **konfigurace**.
5. V testovacím hello **konfigurace** vyberte **virtuální sítě**.
6. Na hello **virtuální sítě** okno, zobrazí se seznam virtuálních sítí, které jsou nakonfigurované pro aktuální prostředí hello a také výchozí hello virtuální se sítí, který se vytvoří pro testovací prostředí. 
7. Vyberte **+ přidat**.
   
    ![Přidat existující tooyour laboratoře virtuální sítě](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. Na hello **virtuální síť** vyberte **[vyberte virtuální síť]**.
   
    ![Vyberte existující virtuální síť](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. Na hello **zvolte virtuální síť** okně, vyberte hello požadované virtuální sítě. Hello se zobrazí okno všechny virtuální sítě hello, které jsou v části hello stejné oblasti v rámci předplatného hello jako hello testovacího prostředí.  
10. Po výběru virtuální sítě, jsou vráceny toohello **virtuální síť** klikněte na tlačítko hello podsíť v seznamu hello v hello dolní části okna hello.

    ![Seznam podsítí](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Zobrazí se okno Hello podsíť testovacího prostředí.

    ![Okno podsíť testovacího prostředí](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. Zadejte **název podsítě testovacího prostředí**.
12. tooallow podsíť toobe, používá v testovacím prostředí vytvoření virtuálního počítače, vyberte **použití v vytvoření virtuálního počítače**.
13. tooenable [sdílené veřejnou IP adresu](devtest-lab-shared-ip.md), vyberte **povolení sdíleného veřejnou IP adresu**.
14. Vyberte tooallow veřejné IP adresy v podsíti, **povolit vytvoření veřejné IP**.
15. V hello **maximální počet virtuálních počítačů na uživatele** pole, zadejte text hello maximální virtuálních počítačů na uživatele pro každou podsíť. Pokud chcete neomezený počet virtuálních počítačů, nechte pole prázdné.
16. Vyberte **OK** tooclose hello podsíť testovacího prostředí okno.
17. Vyberte **Uložit** tooclose hello virtuální sítě okno.
18. Teď, když hello virtuální síť nakonfigurované, můžete vybrat při vytváření virtuálního počítače. 
    toosee jak toocreate virtuálního počítače a zadejte virtuální síť, najdete v článku toohello [přidat virtuální počítač s testovacím tooa artefakty](devtest-lab-add-vm-with-artifacts.md). 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
Po přidání hello potřeby testovacím tooyour virtuální sítě, hello dalším krokem je příliš[přidání testovacího prostředí virtuálních počítačů tooyour](devtest-lab-add-vm-with-artifacts.md).

