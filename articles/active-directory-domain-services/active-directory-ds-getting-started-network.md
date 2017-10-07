---
title: "Azure Active Directory Domain Services: Začínáme | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)"
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
ms.openlocfilehash: 7695dabb11df8d9dcfdac24996edf021af2e7f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a>Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)


## <a name="before-you-begin"></a>Než začnete
Odkazovat příliš[požadavky sítě pro Azure Active Directory Domain Services](active-directory-ds-networking.md).


## <a name="task-2-configure-network-settings"></a>Úloha 2: Konfigurace nastavení sítě
Další úlohou konfigurace Hello je toocreate virtuální sítě Azure a vyhrazené podsítě v něm. V této podsíti v rámci své virtuální sítě povolíte službu Azure Active Directory Domain Services. Vyberte existující virtuální síť, kde můžete vytvořit hello vyhrazené podsítě v něm.

1. Klikněte na tlačítko **virtuální síť** tooselect virtuální sítě.
2. Na hello **zvolte virtuální síť** okně se zobrazí všechny existující virtuální sítě. Zobrazí pouze hello virtuální sítě, které patří toohello skupinu prostředků a umístění Azure, které jste vybrali na hello **Základy** stránce průvodce.

3. Vyberte hello virtuální síť, ve kterém by měl povolit Azure AD Domain Services. Klikněte na tlačítko **vytvořit nový**, pokud dáváte přednost toocreate nové virtuální sítě. Důrazně doporučujeme používat vyhrazené podsíť pro Azure AD Domain Services. Pokud vyberete existující virtuální síť, [vytvořit vyhrazený podsíť pomocí rozšíření virtuální sítě hello](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) a pak vyberte této podsíti. 

    ![Vyberte virtuální síť](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. Klikněte na tlačítko **podsíť** toopick hello vyhrazené podsítě v této virtuální síti, v rámci které tooenable nové spravované domény. V hello **vytvořit podsíť** okno, zadejte název pro podsíť hello a klikněte na tlačítko **OK** po dokončení. Například vytvořte podsíť s názvem hello 'DomainServices', a usnadňuje pro ostatní správci toounderstand co je nasazen v rámci podsítě hello.

    ![Vyberte podsíť v rámci virtuální sítě hello](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Pokyny pro výběr podsíť**
  > 1. Použijte vyhrazenou podsíť pro Azure AD Domain Services. Nenasazujte žádné jiné podsíti toothis virtuálních počítačů. Tuto konfiguraci můžete tooconfigure skupin zabezpečení sítě (Nsg) pro zatížení nebo virtuální počítače bez nutnosti přerušení vaší spravované domény. Podrobnosti najdete v tématu [sítě důležité informace týkající se Azure Active Directory Domain Services](active-directory-ds-networking.md).
  2. Nevybírejte hello podsíť brány pro nasazení služby Azure AD Domain Services, protože se nejedná o podporovanou konfiguraci.
  3. Zajistěte, aby byl dostatek prostoru k dispozici adres - IP adres k dispozici minimálně 3 až 5 této podsíti hello, které jste vybrali.
  >

5. Až budete hotovi, klikněte na tlačítko **OK** toomove na toohello **skupiny správců** hello průvodce.


## <a name="next-step"></a>Další krok
[Úloha 3: Konfigurace skupiny pro správu a povolení služby Azure AD Domain Services](active-directory-ds-getting-started-admingroup.md)
