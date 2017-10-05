---
title: "Povolit skupin zabezpečení sítě v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** povolit sítě zabezpečení skupiny **."
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
ms.openlocfilehash: 1e034d59d8847f237fa0d4c772344d45cd618576
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Povolit skupin zabezpečení sítě v Azure Security Center
Azure Security Center doporučuje, abyste povolili skupinu zabezpečení sítě (NSG), pokud ještě není povolené. Skupiny Nsg obsahují seznam pravidel seznamu řízení přístupu (ACL), která povolují nebo odpírají síťový provoz instancím virtuálních počítačů ve virtuální síti. Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti. Pokud je skupina zabezpečení sítě přidružená k podsíti, pravidla seznamu ACL platí pro všechny instance virtuálních počítačů v této podsíti. Kromě toho je možné omezit provoz do konkrétního virtuálního počítače další tím, že přidružíte skupinu NSG přímo do tohoto virtuálního počítače. Další informace najdete v další [co je skupina zabezpečení sítě (NSG)?](../virtual-network/virtual-networks-nsg.md)

Pokud nemáte skupiny Nsg povoleno, Security Center nabízí dva doporučení vám: Povolit skupin zabezpečení sítě na podsítě a povolit skupin zabezpečení sítě na virtuálních počítačích. Můžete vybrat úroveň, podsíť nebo virtuální počítač, chcete-li použít skupiny Nsg.

> [!NOTE]
> Tento dokument vám tuto službu představí formou ukázkového nasazení.  Není to podrobný průvodce.
>
>

## <a name="implement-the-recommendation"></a>Implementace doporučení
1. V **doporučení** vyberte **povolit skupin zabezpečení sítě** v podsítích, nebo na virtuálních počítačích.
   ![Povolení skupin zabezpečení sítě][1]
2. Otevře se okno pro **nakonfigurovat chybějící skupiny zabezpečení sítě** pro podsítě nebo pro virtuální počítače, v závislosti na doporučení, kterou jste vybrali. Vyberte virtuální počítač nakonfigurovat skupinu NSG na nebo podsíť.

   ![Konfigurace NSG pro podsíť][2]

   ![Konfigurace NSG pro virtuální počítač][3]
3. Na **zvolit skupinu zabezpečení sítě** okno, nebo vyberte existující skupinu NSG **vytvořit nový** vytvořit skupinu NSG.

   ![Vyberte skupinu zabezpečení sítě][4]

Pokud vytvoříte skupinu NSG, postupujte podle kroků v [Správa skupin Nsg pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) vytvořit skupinu NSG a nastavit pravidla zabezpečení.

## <a name="see-also"></a>Viz také
Tento článek ukázal, jak implementovat Security Center doporučení "Povolit skupin zabezpečení sítě" pro podsítě nebo virtuální počítače. Další informace o povolení skupin Nsg, naleznete v následujících tématech:

* [Co je skupina zabezpečení sítě (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Správa skupin Nsg pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
