---
title: aaaInstall Endpoint Protection v Azure Security Center | Microsoft Docs
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** nainstalovat Endpoint Protection **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: fea891810e042e4d4f6e55094c0cd4de013ba68a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-endpoint-protection-in-azure-security-center"></a>Nainstalovat službu Endpoint Protection v Azure Security Center
Azure Security Center doporučuje nainstalovat službu endpoint protection v Azure virtuální počítače (VM), pokud ještě není povolené aplikace endpoint protection. Toto doporučení se vztahují pouze tooWindows virtuálních počítačů.

> [!NOTE]
> Tento příklad nasazení používá Antimalware od Microsoftu. V tématu [integrace partnera v Azure Security Center](security-center-partner-integration.md#partners-that-integrate-with-security-center) seznam partnerů integrovat Security Center.  
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Tento dokument není to podrobný průvodce.
>
>

1. V hello **doporučení** vyberte **nainstalovat službu Endpoint Protection**.
   ![Vyberte možnost nainstalovat službu Endpoint Protection][1]
2. Hello **nainstalovat službu Endpoint Protection** otevře se okno se seznamem virtuálních počítačů bez ochrany koncového bodu. Vyberte ze seznamu hello hello virtuálních počítačů, které chcete tooinstall aplikace endpoint protection na a klikněte na tlačítko **nainstalovat na virtuálních počítačích**.
   ![Vyberte virtuální počítače tooinstall Endpoint Protection na][2]
3. Hello **vyberte Endpoint Protection** otevře se okno tooallow jste tooselect hello řešení ochrany koncových bodů chcete toouse. V tomto příkladu budeme vyberte **Antimalware od Microsoftu**.
   ![Vyberte aplikace Endpoint Protection][3]
4. Další informace o řešení ochrany koncových bodů hello se zobrazí. Vyberte **Vytvořit**.
   ![Vytvořte antimalwarové řešení][4]
5. Zadejte nastavení konfigurace hello vyžaduje na hello **přidat rozšíření** a pak vyberte **OK**. toolearn Další informace o nastavení konfigurace hello, najdete v části [výchozí a vlastní konfigurace antimalwarových](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../security/azure-security-antimalware.md) je teď aktivní na hello vybrané virtuální počítače.

## <a name="see-also"></a>Viz také
Tento článek ukázal, jak tooimplement hello Security Center doporučení "Nainstalovat Endpoint Protection." toolearn Další informace o povolení Antimalware od Microsoftu v Azure, najdete v části hello následujícím dokumentu:

* [Antimalware od Microsoftu pro cloudové služby a virtuální počítače](../security/azure-security-antimalware.md) – zjistěte, jak toodeploy Antimalware od Microsoftu.

toolearn Další informace o Security Center, najdete v části hello následující dokumenty:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
