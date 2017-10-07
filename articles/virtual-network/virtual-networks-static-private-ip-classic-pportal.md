---
title: "aaaConfigure privátních IP adres pro virtuální počítače (klasické) - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače (klasické) pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: df4bfa6768fc9e66db89785b633ffdb0274dbc46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-portal"></a>Nakonfigurovat privátní IP adresy pro virtuální počítač (klasický) pomocí hello portálu Azure

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení classic hello. Můžete také [spravovat statickou privátní IP adresou v modelu nasazení Resource Manager hello](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Hello ukázkový postup očekávat jednoduché prostředí již vytvořeny. Pokud chcete toorun hello kroky, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a>Jak toospecify statickou privátní IP adresy při vytvoření virtuálního počítače
virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků hello:

1. V prohlížeči přejděte toohttp://portal.azure.com a, v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na tlačítko **nový** > **výpočetní** > **Windows Server 2012 R2 Datacenter**, Všimněte si, že hello **vybrat model nasazení** seznam již ukazuje **Classic**a potom klikněte na **vytvořit**.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. V hello **vytvoření virtuálního počítače** okno, zadejte název hello toobe hello virtuálního počítače vytvořit (*DNS01* v tomto scénáři), hello účet místního správce a heslo.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Klikněte na tlačítko **volitelné konfiguraci** > **sítě** > **virtuální sítě**a potom klikněte na **TestVNet**. Pokud **TestVNet** není k dispozici, ujistěte se, že používáte hello *střed USA* umístění a vytvořili hello testovací prostředí popsané na začátku hello v tomto článku.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. V hello **sítě** okno, ujistěte se, hello podsíť aktuálně vybraný je *front-endu*, pak klikněte na tlačítko **IP adresy**v části **přiřazení IP adresy** klikněte na tlačítko **statické**a potom zadejte *192.168.1.101* pro **IP adresu** jak vidíte níže.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Klikněte na tlačítko **OK** v hello **IP adresy** okno, pak klikněte na tlačítko **OK** v hello **sítě** a klikněte na **OK**v hello **volitelné konfigurace** okno.
7. V hello **vytvoření virtuálního počítače** okně klikněte na tlačítko **vytvořit**. Všimněte si hello dlaždice níže zobrazí v řídicím panelu.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Jak tooretrieve statickou privátní IP adresu pro virtuální počítač
tooview hello privátní IP adresu informace statického pro hello, že virtuální počítač vytvořen s hello kroky uvedené výše, provést následující postup hello.

1. Z portálu Azure Azure hello, klikněte na tlačítko **Procházet vše** > **virtuálních počítačů (klasické)** > **DNS01**  >   **Všechna nastavení** > **IP adresy** a Všimněte si, jak je vidět níže hello přiřazení IP adresy a IP adresu.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Jak tooremove statickou privátní IP adresu z virtuálního počítače
tooremove hello statickou privátní IP adresou z hello, kterou virtuální počítač vytvořili výše, postupujte podle následujících kroků hello.

1. Z hello **IP adresy** uvedené výše, klikněte na **dynamické** toohello napravo od **přiřazení IP adresy**, pak klikněte na tlačítko **Uložit**a potom Klikněte na tlačítko **Ano**.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a>Jak tooadd statickou privátní IP adresa tooan existující virtuální počítač
tooadd statickou privátní IP adresu toohello virtuálních počítačů, které jsou vytvořené pomocí hello výše uvedených kroků, postupujte podle kroků hello níže:

1. Z hello **IP adresy** uvedené výše, klikněte na **statické** toohello napravo od **přiřazení IP adresy**.
2. Typ *192.168.1.101* pro **IP adresu**, pak klikněte na tlačítko **Uložit**a potom klikněte na **Ano**.

## <a name="next-steps"></a>Další kroky
* Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.
* Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.
* Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).

