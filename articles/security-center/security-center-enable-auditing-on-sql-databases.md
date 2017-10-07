---
title: "aaaEnable auditování a zjišťování hrozeb SQL databáze v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** povolit auditování a zjišťování hrozeb v SQL databáze **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: c94140acf37cabaca3e681ba5db79d6827e7b9db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a>Povolení auditování a zjišťování hrozeb u databází SQL v Azure Security Center
Azure Security Center bude doporučujeme vám zapnout auditování a zjišťování hrozeb pro všechny SQL databáze, pokud auditování a detekce hrozeb ještě není povolené. Auditování a hrozeb, že detekce vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.

Jakmile jste zapnuli auditování můžete konfigurovat výstrahy zabezpečení tooreceive detekce hrozeb pro nastavení a e-mailů. Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze. To vám umožní toodetect a hrozeb toopotential reagovat, když k nim dojde.

Toto doporučení platí toohello pouze; služby Azure SQL neobsahuje SQL běžících na virtuálních počítačích.

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **povolení auditování a detekce hrozeb v databázích SQL**.  Tím se otevře hello **povolení auditování a detekce hrozeb v databázích SQL** okno.

   ![Povolení auditování pro databáze SQL][1]
2. Vyberte auditování databáze SQL tooenable na. Tím se otevře hello **auditování a detekce hrozeb** okno.

3. Na hello **auditování a detekce hrozeb** vyberte **ON** pod **auditování**.

   ![Zapnout auditování a zjišťování hrozeb][2]
4. Postupujte podle kroků hello v [detekce hrozeb databáze SQL v hello portál Azure](../sql-database/sql-database-threat-detection-portal.md) tooturn na a konfigurace detekce hrozeb a tooconfigure hello seznam e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklých aktivit.

## <a name="see-also"></a>Viz také
Tento článek ukázal, jak tooimplement hello Security Center doporučení "Povolit auditování a detekce hrozeb v databázích SQL." toolearn Další informace o zabezpečení databáze SQL, najdete v části hello následující:

* [Zabezpečení služby SQL Database](../sql-database/sql-database-security-overview.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
