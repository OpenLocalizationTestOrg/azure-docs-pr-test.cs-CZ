---
title: "aktualizace systému aaaApply v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello doporučení služby Azure Security Center ** použít aktualizace systému ** a ** po systému aktualizace ** restartuje."
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
ms.openlocfilehash: 02024f1558b4758c09141fe1934c2e1a9845cc96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-system-updates-in-azure-security-center"></a>Použít aktualizace systému v Azure Security Center
Azure Security Center monitoruje denně Windows a Linux virtuálních počítačů (VM) pro chybějící aktualizace operačního systému. Security Center načte seznam dostupných zabezpečení a důležité aktualizace ze služby Windows Update nebo Windows Server Update Services (WSUS), podle toho, která je nakonfigurovaná služba virtuální počítač s Windows.  Security Center také zkontroluje nejnovější aktualizace hello v systémech Linux. Pokud virtuální počítač je chybějící aktualizace systému, Security Center bude doporučujeme použít aktualizace systému

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **aktualizace systému**.

   ![Nainstalovat aktualizace systému][1]
2. Hello **aktualizace systému** otevře se okno se seznamem chybějící aktualizace systému virtuálních počítačů. Vyberte virtuální počítač.

   ![Vyberte virtuální počítač][2]
3. Otevře se okno se seznamem chybějící aktualizace pro tento virtuální počítač. Vyberte aktualizace systému. V tomto příkladu budeme vyberte KB3156016.

   ![Chybějící aktualizace zabezpečení][3]

4. Postupujte podle kroků hello v hello **aktualizací zabezpečení** okno tooapply hello chybějící aktualizace.

   ![Aktualizace zabezpečení][4]

## <a name="reboot-after-system-updates"></a>Restartovat po aktualizacích systému
1. Vrátí toohello **doporučení** okno. Nový záznam vygenerovalo po použití aktualizací systému, nazývá **restartovat po aktualizacích systému**. Tato položka způsobem zjistíte, je nutné, aby tooreboot hello virtuálních počítačů toocomplete hello proces použití aktualizací systému.

   ![Restartovat po aktualizacích systému][5]
2. Vyberte **restartovat po aktualizacích systému**. Tím se otevře **restartování je aktualizace systému čekající toocomplete** okno se seznamem virtuálních počítačů, je nutné, aby toorestart toocomplete hello použít proces aktualizace systému.

   ![Čeká se na restartování][6]

Restartujte hello virtuálních počítačů z Azure toocomplete hello procesu.

## <a name="see-also"></a>Viz také
toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
