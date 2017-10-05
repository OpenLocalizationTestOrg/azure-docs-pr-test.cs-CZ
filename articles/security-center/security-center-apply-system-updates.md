---
title: "Použít aktualizace systému v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak implementovat Azure Security Center doporučení ** použít aktualizace systému ** a ** po systému aktualizace ** restartuje."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: 50cdea437db5387813c6a3905d14b6904d2aba34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="apply-system-updates-in-azure-security-center"></a>Použít aktualizace systému v Azure Security Center
Azure Security Center monitoruje denně Windows a Linux virtuálních počítačů (VM) pro chybějící aktualizace operačního systému. Security Center načte seznam dostupných zabezpečení a důležité aktualizace ze služby Windows Update nebo Windows Server Update Services (WSUS), podle toho, která je nakonfigurovaná služba virtuální počítač s Windows.  Security Center také zkontroluje nejnovější aktualizace v systémech Linux. Pokud virtuální počítač je chybějící aktualizace systému, Security Center bude doporučujeme použít aktualizace systému

> [!NOTE]
> Tento dokument vám tuto službu představí formou ukázkového nasazení.  Není to podrobný průvodce.
>
>

## <a name="implement-the-recommendation"></a>Implementace doporučení
1. V **doporučení** vyberte **aktualizace systému**.

   ![Nainstalovat aktualizace systému][1]
2. **Aktualizace systému** otevře se okno se seznamem chybějící aktualizace systému virtuálních počítačů. Vyberte virtuální počítač.

   ![Vyberte virtuální počítač][2]
3. Otevře se okno se seznamem chybějící aktualizace pro tento virtuální počítač. Vyberte aktualizace systému. V tomto příkladu budeme vyberte KB3156016.

   ![Chybějící aktualizace zabezpečení][3]

4. Postupujte podle kroků v **aktualizací zabezpečení** okno použít chybějící aktualizace.

   ![Aktualizace zabezpečení][4]

## <a name="reboot-after-system-updates"></a>Restartovat po aktualizacích systému
1. Vraťte se na **doporučení** okno. Nový záznam vygenerovalo po použití aktualizací systému, nazývá **restartovat po aktualizacích systému**. Tato položka umožňuje vědět, že je potřeba restartovat virtuální počítač pro dokončení procesu použití aktualizace systému.

   ![Restartovat po aktualizacích systému][5]
2. Vyberte **restartovat po aktualizacích systému**. Tím se otevře **restart čeká na dokončení aktualizací systému** procesu aktualizací okno se seznamem virtuálních počítačů, které potřebujete k dokončení použít systém restartovat.

   ![Čeká se na restartování][6]

Restartujte virtuální počítač z Azure do dokončení procesu.

## <a name="see-also"></a>Viz také
Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
