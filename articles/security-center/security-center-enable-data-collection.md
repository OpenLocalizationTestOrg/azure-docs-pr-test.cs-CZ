---
title: "Povolení shromažďování dat v Azure Security Center | Microsoft Docs"
description: " Informace o povolení shromažďování dat v Azure Security Center. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 7e9ad8cd8c77c57c37dc208b86b3727a4e1dc7b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-data-collection-in-azure-security-center"></a>Povolení shromažďování dat v Azure Security Center

> [!NOTE]
> Od začátku června 2017 bude Security Center používat ke shromažďování a ukládání dat agenta Microsoft Monitoring Agent. Další informace najdete v tématu [Azure Security Center platformy migrace](security-center-platform-migration.md). Informace v tomto článku představují funkce služby Security Center po přechodu na agenta Microsoft Monitoring Agent.
>
>

Security Center shromažďuje data z vašich virtuálních počítačů za účelem posouzení jejich stavu, poskytování doporučení zabezpečení a upozorňování na hrozby. Pokud nejprve přístup k Security Center, máte možnost Povolit shromažďování dat pro všechny virtuální počítače ve vašem předplatném. Pokud se shromažďování dat není povolené, Security Center doporučí, zapnout shromažďování dat v zásadách zabezpečení pro toto předplatné.

Při shromažďování dat je povolené, Security Center zřizuje agenta Microsoft Monitoring Agent do všech existujících podporované virtuální počítače Azure a všechny nové, které jsou vytvořeny. Microsoft Monitoring Agent hledá různé konfigurace související se zabezpečením. Kromě toho operační systém vyvolá událost protokolu událostí. Mezi příklady těchto údajů patří: typ a verze operačního systému, protokoly operačního systému (protokoly událostí systému Windows), spuštěné procesy, název počítače, IP adresy, přihlášený uživatel a ID klienta. Microsoft Monitoring Agent přečte položky protokolu událostí a konfigurací a zkopíruje data do pracovního prostoru pro analýzu. Microsoft Monitoring Agent taky zkopíruje soubory se stavem systému do pracovního prostoru.

Pokud používáte úroveň Free služby Security Center, můžete zakázat shromažďování dat z virtuálních počítačů tak, že zakážete shromažďování dat v zásadě zabezpečení. Zakázání shromažďování dat omezením posouzení zabezpečení pro virtuální počítače. Další informace najdete v tématu [zakázání shromažďování dat](#disabling-data-collection). Jsou povoleny snímků disku virtuálního počítače a kolekce artefaktů i v případě, že shromažďování dat je zakázané. Shromažďování dat je vyžadován pro odběry ve standardní vrstvě služby Security Center.

> [!NOTE]
> Další informace o volné a Standard Security Center [cenové úrovně](security-center-pricing.md).
>
>

## <a name="implement-the-recommendation"></a>Implementace doporučení

> [!NOTE]
> Tento dokument vám tuto službu představí formou ukázkového nasazení. Tento dokument není to podrobný průvodce.
>
>

1. V **doporučení** vyberte **povolení shromažďování dat pro odběry**.  Tím se otevře **shromažďování dat zapnout** okno.
   ![Okno doporučení][2]
2. Na **shromažďování dat zapnout** okně vyberte své předplatné. **Zásady zabezpečení** otevře se okno pro toto předplatné.
3. Na **zásady zabezpečení** vyberte **na** pod **shromažďování dat** automaticky shromažďovat protokoly. Zapnutí zřizuje kolekce dat rozšíření monitorování všech aktuálních a nových podporované virtuální počítače v rámci předplatného.
4. Vyberte **Uložit**.
5. Vyberte **OK**.

## <a name="disabling-data-collection"></a>Zakázání shromažďování dat
Pokud používáte úroveň Free služby Security Center, můžete zakázat shromažďování dat z virtuálních počítačů v každém okamžiku tak, že zakážete shromažďování dat v zásadě zabezpečení. Shromažďování dat je vyžadován pro odběry ve standardní vrstvě služby Security Center.

1. Vraťte se do **Security Center** a vyberte **zásad** dlaždici. Tím se otevře **zabezpečení zásady definovat zásady, za předplatné** okno.
   ![Vyberte dlaždici zásad][5]
2. Na **zabezpečení zásady definovat zásady, za předplatné** okně vyberte odběr, který chcete zakázat shromažďování dat.
3. **Zásady zabezpečení** otevře se okno pro toto předplatné.  Vyberte **vypnout** pod shromažďování dat.
4. Vyberte **Uložit** nejvyšší pásu karet.

## <a name="next-steps"></a>Další kroky
Tento článek vám ukázal, jak provést doporučení Security Center "Povolit shromažďování dat." Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
- [Zabezpečení dat v Azure Security Center](security-center-data-security.md) -další způsob správy a zabezpečení ve službě Security Center.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) – Získejte nejnovější informace o zabezpečení Azure.

<!--Image references-->
[2]: ./media/security-center-enable-data-collection/recommendations.png
[3]: ./media/security-center-enable-data-collection/data-collection.png
[4]: ./media/security-center-enable-data-collection/storage-account.png
[5]: ./media/security-center-enable-data-collection/policy.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
