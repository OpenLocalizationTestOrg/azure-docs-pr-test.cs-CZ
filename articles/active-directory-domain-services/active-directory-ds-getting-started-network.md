---
title: "Azure Active Directory Domain Services: Začínáme | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí portálu Azure (Preview)"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: 7f420d60862adf61e4f21e5abac2932a742bd55d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a>Povolit Azure Active Directory Domain Services pomocí portálu Azure (Preview)


## <a name="before-you-begin"></a>Než začnete
Přečtěte si článek [Důležité informace o sítích pro Azure Active Directory Domain Services](active-directory-ds-networking.md).


## <a name="task-2-configure-network-settings"></a>Úloha 2: Konfigurace nastavení sítě
Další úlohou konfigurace je vytvoření virtuální sítě Azure a vyhrazené podsítě v něm. V této podsíti v rámci své virtuální sítě povolíte službu Azure Active Directory Domain Services. Také můžete vybrat existující virtuální síť a vytvořit vyhrazený podsítě v něm.

1. Klikněte na tlačítko **virtuální síť** vyberte virtuální síť.
2. Na **zvolte virtuální síť** okně se zobrazí všechny existující virtuální sítě. Zobrazí pouze virtuální sítě, které patří do skupiny prostředků a umístění Azure, které jste vybrali na **Základy** stránce průvodce.

3. Vyberte virtuální síť, ve kterém by měl být povolili Azure AD Domain Services. Klikněte na tlačítko **vytvořit nový**, pokud chcete vytvořit novou virtuální síť. Důrazně doporučujeme používat vyhrazené podsíť pro Azure AD Domain Services. Pokud vyberete existující virtuální síť, [vytvořit vyhrazený podsíť pomocí virtuální sítě rozšíření](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) a pak vyberte této podsíti. 

    ![Vyberte virtuální síť](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. Klikněte na tlačítko **podsíť** vybrat vyhrazené podsítě v této virtuální síti, ve které chcete povolit vaší nové spravované domény. V **vytvořit podsíť** okno, zadejte název pro podsíť a klikněte na tlačítko **OK** po dokončení. Můžete například vytvořte podsíť s názvem "DomainServices", a usnadňuje pro ostatní správci zjistit, co je nasazen v rámci podsítě.

    ![Vyberte podsítí v rámci virtuální sítě](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Pokyny pro výběr podsíť**
  > 1. Použijte vyhrazenou podsíť pro Azure AD Domain Services. Nenasazujte případných dalších virtuálních počítačů na této podsíti. Tato konfigurace umožňuje nakonfigurovat skupiny zabezpečení sítě (Nsg) pro zatížení nebo virtuální počítače bez nutnosti přerušení vaší spravované domény. Podrobnosti najdete v tématu [sítě důležité informace týkající se Azure Active Directory Domain Services](active-directory-ds-networking.md).
  2. Nevybírejte podsíť brány pro nasazení služby Azure AD Domain Services, protože se nejedná o podporovanou konfiguraci.
  3. Zajistěte, aby podsíť, kterou jste vybrali dostatek prostoru k dispozici adres - aspoň 3 až 5 dostupných IP adres.
  >

5. Až budete hotovi, klikněte na tlačítko **OK** přesunout na **skupiny správců** stránce průvodce.


## <a name="next-step"></a>Další krok
[Úloha 3: Konfigurace skupiny pro správu a povolení služby Azure AD Domain Services](active-directory-ds-getting-started-admingroup.md)
