---
title: "skupiny zabezpečení sítě aaaManage - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello skupin zabezpečení sítě toomanage pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: faee5ac8-f4c4-4f97-ade5-197a37aad496
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53fb29e60cbc2a535f6cf03e430d9e703e97b216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-portal"></a>Správa skupin zabezpečení sítě pomocí hello portálu Azure

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [vytvářet skupiny Nsg v modelu nasazení classic hello](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Ukázka Hello jednoduché prostředí už vytvořený očekávat níže uvedené příkazy prostředí PowerShell založené na scénář hello výše. Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření nasazením nejprve hello testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů Pokud potřeby a postupujte podle pokynů hello v hello portálu. Hello kroky níže použijte **RG NSG** jako hello název šablony hello skupiny prostředků hello byl nasazen na.

## <a name="create-hello-nsg-frontend-nsg"></a>Vytvoření hello NSG front-endu NSG
toocreate hello **NSG front-endu** NSG jak je znázorněno v hello scénáři výše, postupujte podle kroků hello níže.

1. V prohlížeči přejděte toohttp://portal.azure.com a, v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na tlačítko **procházet >** > **skupin zabezpečení sítě**.
   
    ![Portál Azure – skupiny Nsg](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. V hello **skupin zabezpečení sítě** okně klikněte na tlačítko **přidat**.
   
    ![Portál Azure – skupiny Nsg](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. V hello **vytvořit skupinu zabezpečení sítě** okně Vytvořit skupinu NSG s názvem *NSG front-endu* v hello *RG NSG* skupinu prostředků a pak klikněte na tlačítko **vytvořit**.
   
    ![Portál Azure – skupiny Nsg](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Vytvoření pravidel v existující skupině NSG
toocreate pravidla v existující skupina NSG z hello portál Azure, postupujte podle následujících kroků hello.

1. Klikněte na tlačítko **procházet >** > **skupin zabezpečení sítě**.
2. Hello seznamu skupin Nsg, klikněte na **NSG front-endu** > **příchozí pravidla zabezpečení**
   
    ![Portál Azure – skupina NSG front-endu](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. V seznamu hello **příchozí pravidla zabezpečení**, klikněte na tlačítko **přidat**.
   
    ![Portál Azure – přidání pravidla](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. V hello **přidat příchozí pravidlo zabezpečení** okně Vytvořit pravidlo s názvem *pravidlo webové* s prioritu *200* povolením přístupu prostřednictvím *TCP* tooport *80* tooany virtuálních počítačů z libovolného zdroje a pak klikněte na tlačítko **OK**. Všimněte si, že většinu těchto nastavení jsou již výchozí hodnoty.
   
    ![Portál Azure – nastavení pravidla](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. Za několik sekund zobrazí se nové pravidlo hello v hello NSG.
   
    ![Portál Azure – nové pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. Opakujte kroky too6 toocreate příchozí pravidlo s názvem *rdp pravidlo* s prioritou *250* povolením přístupu prostřednictvím *TCP* tooport *3389* tooany virtuálních počítačů z jakéhokoli zdroje.

## <a name="associate-hello-nsg-toohello-frontend-subnet"></a>Přidružení podsítě front-endu toohello NSG hello
1. Klikněte na tlačítko **procházet >** > **skupiny prostředků** > **RG NSG**.
2. V hello **RG NSG** okně klikněte na tlačítko **...**   >  **TestVNet**.
   
    ![Portál Azure – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. V hello **nastavení** okně klikněte na tlačítko **podsítě** > **front-endu** > **skupinu zabezpečení sítě**  >  **NSG front-endu**.
   
    ![Portál Azure – nastavení podsítě](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. V hello **front-endu** okně klikněte na tlačítko **Uložit**.
   
    ![Portál Azure – nastavení podsítě](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-hello-nsg-backend-nsg"></a>Vytvoření hello NSG NSG back-end
toocreate hello **NSG back-end** NSG a přidružte ji toohello **back-end** podsíť, postupujte podle následujících kroků hello.

1. Opakování hello kroky [vytvořit hello NSG front-endu NSG](#Create-the-NSG-FrontEnd-NSG) toocreate skupinu NSG s názvem *NSG back-end*
2. Opakování hello kroky [vytvořit pravidla v existující skupině](#Create-rules-in-an-existing-NSG) toocreate hello **příchozí** pravidla v tabulce hello.
   
   | Příchozí pravidlo | Odchozí pravidlo |
   | --- | --- |
   | ![Portál Azure – příchozí pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Portál Azure – odchozí pravidlo](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. Opakování hello kroky [přidružit podsíť FrontEnd toohello NSG hello](#Associate-the-NSG-to-the-FrontEnd-subnet) tooassociate hello **NSG back-end** NSG toohello **back-end** podsítě.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[spravovat existující skupiny Nsg](virtual-network-manage-nsg-arm-portal.md)
* [Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.

