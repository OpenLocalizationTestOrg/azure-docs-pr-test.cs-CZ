---
title: "aaaConfigure privátních IP adres pro virtuální počítače - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 474161303cdf8cb98e16ffd7cef6b74debdbc49a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-portal"></a>Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí hello portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
> * [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
> * [Azure CLI](virtual-networks-static-private-ip-arm-cli.md)
> * [Portál Azure (klasický)](virtual-networks-static-private-ip-classic-pportal.md)
> * [PowerShell (Classic)](virtual-networks-static-private-ip-classic-ps.md)
> * [Rozhraní příkazového řádku Azure (klasický)](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic hello](virtual-networks-static-private-ip-classic-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Hello ukázkový postup očekávat jednoduché prostředí již vytvořeny. Pokud chcete toorun hello kroky, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-toocreate-a-vm-for-testing-static-private-ip-addresses"></a>Jak toocreate virtuálního počítače pro testování statickou privátní IP adresy
Nelze nastavit statickou privátní IP adresou během vytváření hello virtuálního počítače v režimu nasazení Resource Manager hello pomocí hello portálu Azure. Je nutné nejprve vytvořit hello virtuálních počítačů, vložíte jej nastavit jeho privátní IP toobe statické.

virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet*, postupujte podle následujících kroků hello.

1. V prohlížeči přejděte toohttp://portal.azure.com a, v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na tlačítko **nový** > **výpočetní** > **Windows Server 2012 R2 Datacenter**, Všimněte si, že hello **vybrat model nasazení** seznam již ukazuje **Resource Manager**a potom klikněte na **vytvořit**, jak je vidět na následujícím obrázku hello.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. V hello **Základy** okno, zadejte název hello toobe hello virtuálního počítače vytvořit (*DNS01* v tomto scénáři), hello účet místního správce a hesla, jak je vidět na následujícím obrázku hello.
   
    ![Okno Základy](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. Ujistěte se, zda text hello **umístění** vybraná *střed USA*, pak klikněte na tlačítko **vyberte existující** pod **skupiny prostředků**, pak klikněte na tlačítko **Skupiny prostředků** znovu, pak klikněte na tlačítko *TestRG*a potom klikněte na **OK**.
   
    ![Okno Základy](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. V hello **zvolte velikost** vyberte **A1 standardní**a potom klikněte na **vyberte**.
   
    ![Zvolte velikost okna](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. V **nastavení** okno, ujistěte se, hello následující vlastnosti jsou nastaveny se nastavují s hello níže uvedené hodnoty a potom klikněte na **OK**.
   
    -**Účet úložiště**: *vnetstorage*
   
   * **Síť**: *TestVNet*
   * **Podsíť**: *front-endu*
     
     ![Zvolte velikost okna](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. V hello **Souhrn** okně klikněte na tlačítko **OK**. Všimněte si hello dlaždice níže zobrazí v řídicím panelu.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Jak tooretrieve statickou privátní IP adresu pro virtuální počítač
tooview hello privátní IP adresu informace statického pro hello, že virtuální počítač vytvořen s hello kroky uvedené výše, provést následující postup hello.

1. Z portálu Azure Azure hello, klikněte na tlačítko **Procházet vše** > **virtuální počítače** > **DNS01** > **všechny nastavení** > **síťových rozhraní** a potom klikněte na hello pouze uvedené rozhraní sítě.
   
    ![Nasazení virtuálních počítačů dlaždice](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. V hello **síťové rozhraní** okně klikněte na tlačítko **všechna nastavení** > **IP adresy** a Všimněte si hello **přiřazení** a **IP adresu** hodnoty.
   
    ![Nasazení virtuálních počítačů dlaždice](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a>Jak tooadd statickou privátní IP adresa tooan existující virtuální počítač
tooadd statickou privátní IP adresu toohello virtuálních počítačů, které jsou vytvořené pomocí hello výše uvedených kroků, postupujte podle kroků hello níže:

1. Z hello **IP adresy** uvedené výše, klikněte na **statické** pod **přiřazení**.
2. Typ *192.168.1.101* pro **IP adresu**a potom klikněte na **Uložit**.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> Pokud po kliknutí na **Uložit** si všimnete, že přiřazení hello je stále nastavené příliš**dynamické**, znamená to, že jste zadali hello IP adresa je již používán. Zkuste jinou IP adresu.
> 
> 

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Jak tooremove statickou privátní IP adresu z virtuálního počítače
tooremove hello statickou privátní IP adresou z hello, kterou virtuální počítač vytvořili výše, proveďte následující krok hello:

Z hello **IP adresy** uvedené výše, klikněte na **dynamické** pod **přiřazení**a potom klikněte na **Uložit**.

## <a name="next-steps"></a>Další kroky
* Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.
* Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.
* Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).

