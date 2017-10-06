---
title: "aaaEnable auditování a zjišťování hrozeb na serverech SQL v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** povolení auditování a detekce hrozeb na SQL servery **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 042fca4d-7dab-4172-8614-e8c21ccb4960
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: b082c48cdbc386f14e677f4e13a7f306f37fd0e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a>Povolení auditování a zjišťování hrozeb na serverech SQL v Azure Security Center
Azure Security Center bude vhodné zapnout auditování a detekce hrozeb pro všechny databáze na serverech Azure SQL, pokud auditování již není povolena. Auditování a hrozeb, že detekce vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.

Jakmile jste zapnuli auditování můžete konfigurovat výstrahy zabezpečení tooreceive detekce hrozeb pro nastavení a e-mailů. Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze. To vám umožní toodetect a hrozeb toopotential reagovat, když k nim dojde.

Toto doporučení platí toohello pouze; služby Azure SQL neobsahuje SQL Server běžící na virtuálních počítačů ve službách infrastruktury Azure (Azure IaaS).

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **povolení auditování a detekce hrozeb na serverech SQL**.  Tím se otevře hello **povolení auditování a detekce hrozeb na serverech SQL** okno.

   ![Povolení auditování pro servery SQL][1]
2. Vyberte SQL server tooenable auditování a zjišťování hrozeb na. Tím se otevře hello **auditování a detekce hrozeb** okno.

3. Na hello **auditování a detekce hrozeb** vyberte **ON** pod **auditování**.

   ![Zapnout nastavení auditování][2]
4. Postupujte podle kroků hello v [auditování databáze SQL v portálu Azure hello](../sql-database/sql-database-auditing-portal.md) tooconfigure úložiště, kde bude uložena vaše protokoly auditu. Hello odběru účet úložiště pro shromažďování dat je hello výchozí účet úložiště.
5. Postupujte podle kroků hello v [začít pracovat s detekce hrozeb databáze SQL](../sql-database/sql-database-threat-detection.md) tooturn na a konfigurace detekce hrozeb a tooconfigure hello seznam e-mailů, které budou dostávat upozornění zabezpečení při zjištění nezvyklých aktivit.

## <a name="see-also"></a>Viz také
Tento článek ukázal, jak tooimplement hello Security Center doporučení "Povolení auditování a detekce na serverech SQL hrozby." toolearn Další informace o zabezpečení databáze SQL, najdete v části hello následující:

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
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
