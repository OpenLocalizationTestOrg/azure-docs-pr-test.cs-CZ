---
title: "aaaProvide zabezpečení kontaktní údaje v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooprovide zabezpečení kontaktní údaje v Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 6c378c9c84c6e4fce70b2541e4cc121018700269
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Zadejte kontaktní údaje zabezpečení v Azure Security Center
Azure Security Center doporučí, pokud jste to ještě neudělali zadejte kontaktní údaje zabezpečení pro vaše předplatné Azure. Tyto informace použije Microsoft toocontact vám, zda text hello Microsoft Security Response Center (MSRC) zjistí, že zákaznických údajů měl strana nezákonné nebo neoprávněný přístup k. Střediska MSRC provádí, vyberte možnost zabezpečení monitorování hello síť Azure a infrastruktury a přijímá stížností intelligence a zneužití ohrožení od jiných výrobců.

E-mailové oznámení se odesílají na hello první denní výskyt výstrahy a jenom pro výstrahy s vysokou závažností. Předvolby e-mailu lze konfigurovat pouze pro zásady předplatného. Skupiny prostředků v rámci předplatného zdědí tato nastavení.

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **zadejte kontaktní údaje zabezpečení**.
   ![Zadejte kontakt zabezpečení][1]
2. Otevře se okno hello **zadejte kontaktní údaje zabezpečení**. Vyberte hello předplatného Azure tooprovide kontaktní informace na.
   ![Poskytnutí podrobností kontaktů zabezpečení][2]
3. Druhý **zadejte kontaktní údaje zabezpečení** otevře se okno.

   * Zadejte hello zabezpečení kontaktní e- mailových adres oddělených čárkami. Není k dispozici několik toohello limit e-mailové adresy, které můžete zadat.
   * Zadejte jeden zabezpečení kontaktní telefonní číslo v mezinárodním.
   * e-mailů tooreceive o vysokou závažností výstrahy, zapněte možnost hello **zaslat mi e-maily o výstrahách**.
   * V budoucích hello budete mít hello možnost toosend e-mailové oznámení toosubscription vlastníky. Tato možnost není aktuálně k dispozici.
   * Vyberte **OK** tooapply hello zabezpečení obraťte se na informace tooyour předplatné.

## <a name="see-also"></a>Viz také
toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
