---
title: "aaaRestrict přístupu prostřednictvím internetových koncových bodů v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** omezit přístup prostřednictvím internetové koncový bod **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: ee72497088618d4db29b5ae4183f4fe77b498423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Omezení přístupu prostřednictvím internetových koncových bodů v Azure Security Center
Azure Security Center doporučí, omezení přístupu prostřednictvím internetových koncových bodů, pokud žádné skupiny zabezpečení sítě (Nsg) má jednu nebo více příchozí pravidla, která umožňují přístup z "žádné" zdrojové IP adresy. Přístup k otevírání příliš "žádné" může povolit útočníci tooaccess vašich prostředků. Security Center doporučí, že upravíte tyto příchozích pravidel toorestrict přístup toosource IP adresy, které skutečně potřebují přístup.

Toto doporučení se generuje pro všechny port jiný web, který má "žádný" jako zdroj.

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení. Není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení okno**, vyberte **omezit přístup prostřednictvím internetové koncový bod**.

   ![Omezit přístup přes internetový koncový bod][1]
2. Otevře se okno hello **omezit přístup prostřednictvím internetové koncový bod**. Toto okno obsahuje seznam hello virtuální počítače (VM) s příchozích pravidel, které vytvářejí potenciálním potížím se zabezpečením. Vyberte virtuální počítač.

   ![Vyberte virtuální počítač][2]
3. Hello **NSG** okno zobrazuje informace o skupinu zabezpečení sítě, související příchozích pravidel a hello přidružené virtuálních počítačů. Vyberte **upravit příchozí pravidla** tooproceed s úpravy příchozího pravidla.

   ![Okno skupina zabezpečení sítě][3]
4. Na hello **příchozí pravidla zabezpečení** okně vyberte hello tooedit příchozí pravidlo. V tomto příkladu budeme vyberte **AllowWeb**.

   ![Příchozí pravidla zabezpečení][4]

   Všimněte si, můžete také vybrat **výchozí pravidla** toosee hello sadu výchozích pravidel obsažené ve všech skupin Nsg. výchozí pravidla Hello nelze odstranit, ale protože je jim přiřazená s nižší prioritou, dají se přepsat pravidly hello, které vytvoříte. Další informace o [výchozí pravidla](../virtual-network/virtual-networks-nsg.md#default-rules).

   ![Výchozí pravidla][5]
5. Na hello **AllowWeb** okně Upravit vlastnosti hello hello příchozí pravidlo, které hello **zdroj** je IP adresa nebo blok IP adres. toolearn Další informace o vlastnosti hello hello příchozí pravidlo, najdete v části [pravidla NSG](../virtual-network/virtual-networks-nsg.md#nsg-rules).

   ![Upravit pravidlo pro příchozí][6]

## <a name="see-also"></a>Viz také
Tento článek ukázal, jak tooimplement hello Security Center doporučení "omezit přístup prostřednictvím internetové koncový bod." toolearn Další informace o povolení skupiny Nsg a pravidla, najdete v části hello následující:

* [Co je skupina zabezpečení sítě (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Jak hello skupiny Nsg toomanage pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md)– zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md)– zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md)– zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md)– přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/)– získejte nejnovější informace zabezpečení Azure hello a informace.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
