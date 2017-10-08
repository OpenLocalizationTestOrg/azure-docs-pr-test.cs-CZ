---
title: "skupiny Nsg aaaManage pomocí hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage existující skupiny Nsg pomocí hello portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.openlocfilehash: ad9a4060bd81bae4597ad5a4f59622e10cd214cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-nsgs-using-hello-portal"></a>Správa skupin Nsg pomocí portálu hello

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-manage-nsg-arm-portal.md)
> * [PowerShell](virtual-network-manage-nsg-arm-ps.md)
> * [Azure CLI](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Načtení informací o
Můžete zobrazit stávající skupiny Nsg, načíst pravidla pro existující skupiny NSG a zjistit, jaké prostředky skupinu NSG je přidružena k.

### <a name="view-existing-nsgs"></a>Zobrazit existující skupiny Nsg

tooview všechny existující skupiny Nsg v předplatném, dokončení hello následující kroky:

1. V prohlížeči přejděte toohttp://portal.azure.com a, v případě potřeby se přihlaste pomocí účtu Azure.

2. Klikněte na tlačítko **procházet >** > **skupin zabezpečení sítě**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Zkontrolujte hello seznam skupin Nsg v hello **skupin zabezpečení sítě** okno.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a>Zobrazení skupiny Nsg ve skupině prostředků

tooview hello seznam skupin Nsg v hello **RG NSG** skupinu prostředků, dokončení hello následující kroky:

1. Klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...** .

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. V seznamu hello prostředků, vyhledejte položky zobrazení hello NSG ikonu, jak je znázorněno v hello **prostředky** okno níže.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a>Seznam všech pravidel pro skupiny NSG

pravidla hello tooview skupinu NSG s názvem **NSG front-endu**, dokončení hello následující kroky:

1. Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.

2. V hello **nastavení** , klikněte na **příchozí pravidla zabezpečení**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. Hello **příchozí pravidla zabezpečení** jak je uvedeno níže, zobrazí se okno.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. V hello **nastavení** , klikněte na **odchozí pravidla zabezpečení** toosee hello odchozí pravidla.

    > [!NOTE]
    > tooview výchozích pravidel, klikněte na tlačítko hello **výchozí pravidla** ikonu hello horní části okna hello, který zobrazuje hello pravidla.
    >

### <a name="view-nsgs-associations"></a>Zobrazte přidružení skupiny Nsg

tooview jaké prostředky hello **NSG front-endu** NSG je spojený s, dokončení hello následující kroky:

1. Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.

2. V hello **nastavení** , klikněte na **podsítě** tooview podsítě, které jsou přidružené toohello NSG.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. V hello **nastavení** , klikněte na **síťových rozhraní** tooview jaké jsou síťové adaptéry přidružené toohello NSG.

## <a name="manage-rules"></a>Spravovat pravidla
Můžete přidat pravidla tooan existující skupina NSG, upravit stávající pravidla a odstranit pravidla.

### <a name="add-a-rule"></a>Přidání pravidla
pravidlo, které povoluje tooadd **příchozí** provoz tooport **443** z toohello všechny počítače **NSG front-endu** NSG, dokončení hello následující kroky:

1. Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.
2. V hello **nastavení** , klikněte na **příchozí pravidla zabezpečení**.
3. V hello **příchozí pravidla zabezpečení** okně klikněte na tlačítko **přidat**. Potom v hello **přidat příchozí pravidlo zabezpečení** okno, zadejte hodnoty hello, jak je uvedeno níže a potom klikněte na **OK**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    Za několik sekund, Všimněte si hello nové pravidlo v hello **příchozí pravidla zabezpečení** okno.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Změna pravidla
pravidlo hello toochange vytvořili výše tooallow příchozí provoz z hello **Internet** pouze, dokončení hello následující kroky:

1. Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.
2. V hello **nastavení** , klikněte na pravidlo hello vytvořili výše.
3. V hello **povolit https** okno, změna hello **zdroj** vlastnost, jak je uvedeno níže a pak klikněte na **Uložit**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Odstranění pravidla

toodelete hello pravidlo vytvořené výše, dokončení hello následující kroky:

1. Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.
2. V hello **nastavení** , klikněte na pravidlo hello vytvořili výše.
3. V hello **povolit https** okně klikněte na tlačítko **odstranit**a potom klikněte na **Ano**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Správa přidružení
Můžete přidružit toosubnets NSG a síťových karet. Můžete také zrušit přidružení skupiny NSG ze všech prostředků, které je přidružené k.

### <a name="associate-an-nsg-tooa-nic"></a>Přidružit NSG tooa síťový adaptér
tooassociate hello **NSG front-endu** NSG toohello **TestNICWeb1** síťového adaptéru, dokončení hello následující kroky:

1. Z hello **skupin zabezpečení sítě** okno nebo hello **prostředky** uvedené výše, klikněte na **NSG front-endu**.
2. V hello **nastavení** , klikněte na **síťových rozhraní** > **přidružit** > **TestNICWeb1**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Zrušit přidružení skupiny NSG z síťový adaptér

toodissociate hello **NSG front-endu** NSG z hello **TestNICWeb1** síťového adaptéru, dokončení hello následující kroky:

1. Z hello portálu Azure, klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **TestNICWeb1**.

2. V hello **TestNICWeb1** okně klikněte na tlačítko **změnit zabezpečení...**   >  **Žádné**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> Můžete také použít toto okno tooassociate hello seskupování tooany existující skupina NSG.
>

### <a name="dissociate-an-nsg-from-a-subnet"></a>Zrušit přidružení skupiny NSG z podsítě

toodissociate hello **NSG front-endu** NSG z hello **front-endu** podsíť, dokončení hello následující kroky:

1. Z hello portálu Azure, klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **TestVNet**.

2. V hello **nastavení** okně klikněte na tlačítko **podsítě** > **front-endu** > **skupinu zabezpečení sítě**  >  **Žádné**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. V hello **front-endu** okně klikněte na tlačítko **Uložit**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-tooa-subnet"></a>Přidružení podsíť tooa NSG

tooassociate hello **NSG front-endu** NSG toohello **FronEnd** znovu podsíť, dokončení hello následující kroky:

1. Z hello portálu Azure, klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **TestVNet**.
2. V hello **nastavení** okně klikněte na tlačítko **podsítě** > **front-endu** > **skupinu zabezpečení sítě**  >  **NSG front-endu**.
3. V hello **front-endu** okně klikněte na tlačítko **Uložit**.

> [!NOTE]
> Můžete také přidružit podsíť NSG tooa z thh NSG na **nastavení** okno.
>

## <a name="delete-an-nsg"></a>Odstranit skupinu NSG
Skupinu NSG můžete odstranit, pouze pokud je tooany prostředku není přiřazen. toodelete skupinu NSG dokončení hello následující kroky:.

1. Z hello portálu Azure, klikněte na tlačítko **skupiny prostředků >** > **RG NSG** > **...**   >  **NSG front-endu**.
2. V hello **nastavení** okně klikněte na tlačítko **síťových rozhraní**.
3. Pokud nejsou žádné síťové adaptéry uvedené, klikněte na tlačítko hello síťový adaptér a postupujte podle kroku 2 v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC).
4. Opakujte krok 3 pro každý síťový adaptér.
5. V hello **nastavení** okně klikněte na tlačítko **podsítě**.
6. Pokud neexistují žádné podsítě uvedena, klikněte na hello podsítě a postupujte podle kroků 2 a 3 v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet).
7. Posune levém toohello **NSG front-endu** okno, pak klikněte na tlačítko **odstranit** > **Ano**.

    ![Portál Azure – skupiny Nsg](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Další kroky
* [Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.
