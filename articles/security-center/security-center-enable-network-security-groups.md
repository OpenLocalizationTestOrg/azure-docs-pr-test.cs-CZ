---
title: "aaaEnable skupin zabezpečení sítě v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** povolit sítě zabezpečení skupiny **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2f70fe432aa452f833a5c322d13102ebbd6dbb69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Povolit skupin zabezpečení sítě v Azure Security Center
Azure Security Center doporučuje, abyste povolili skupinu zabezpečení sítě (NSG), pokud ještě není povolené. Skupiny Nsg obsahují seznam pravidel seznamu řízení přístupu (ACL), která povolují nebo odpírají síťový provoz tooyour instance virtuálních počítačů ve virtuální síti. Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti. Pokud je skupina NSG přidružená k podsíti, pravidla seznamu ACL hello platí tooall hello instance virtuálních počítačů v této podsíti. Kromě toho tooan provoz jednotlivých virtuálních počítačů, je možné omezit tím, že přidružíte skupinu NSG další přímo toothat virtuálních počítačů. víc najdete v části toolearn [co je skupina zabezpečení sítě (NSG)?](../virtual-network/virtual-networks-nsg.md)

Pokud nemáte skupiny Nsg povoleno, Security Center uvede dvě tooyou doporučení: Povolit skupin zabezpečení sítě na podsítě a povolit skupin zabezpečení sítě na virtuálních počítačích. Můžete vybrat úroveň, podsíť nebo virtuální počítač, tooapply skupiny Nsg.

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **povolit skupin zabezpečení sítě** v podsítích, nebo na virtuálních počítačích.
   ![Povolení skupin zabezpečení sítě][1]
2. Otevře se okno hello **nakonfigurovat chybějící skupiny zabezpečení sítě** pro podsítě nebo pro virtuální počítače, v závislosti na hello doporučení, kterou jste vybrali. Vyberte podsíť nebo virtuální počítač tooconfigure skupinu NSG na.

   ![Konfigurace NSG pro podsíť][2]

   ![Konfigurace NSG pro virtuální počítač][3]
3. Na hello **zvolit skupinu zabezpečení sítě** okno, nebo vyberte existující skupinu NSG **vytvořit nový** toocreate skupinu NSG.

   ![Vyberte skupinu zabezpečení sítě][4]

Pokud vytvoříte skupinu NSG, postupujte podle kroků hello v [jak hello skupiny Nsg toomanage pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) toocreate skupinu NSG a nastavte pravidla zabezpečení.

## <a name="see-also"></a>Viz také
Tento článek vám ukázal, jak tooimplement hello Security Center doporučení "Povolit skupin zabezpečení sítě" pro podsítě nebo virtuální počítače. Další informace o povolení skupin Nsg, toolearn najdete hello následující:

* [Co je skupina zabezpečení sítě (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Jak hello skupiny Nsg toomanage pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
