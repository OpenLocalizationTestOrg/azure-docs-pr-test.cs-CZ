---
title: "aaaEnable transparentní šifrování dat v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** povolit transparentní dat šifrování **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 94c6e9a1feddaa48faac6c835d416c4d131cd5c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Povolit transparentní šifrování dat v Azure Security Center
Azure Security Center doporučí, povolte šifrování transparentní dat (TDE) u databází SQL, pokud již není povolené šifrování TDE. Šifrování TDE chrání vaše data a pomáhá splnit požadavky na dodržování předpisů šifrováním vaší databáze, přidružených záloh a souborů protokolů transakci v klidu, bez nutnosti změny tooyour aplikace. víc najdete v části toolearn [transparentní šifrování dat s Azure SQL Database](https://msdn.microsoft.com/library/dn948096).

Toto doporučení platí toohello pouze; služby Azure SQL neobsahuje SQL běžících na virtuálních počítačích.

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **povolit transparentní šifrování dat**.
   ![Povolení transparentního šifrování dat][1]
2. Tím se otevře hello **povolit transparentní šifrování dat v databázích SQL** okno. Vyberte SQL databázi tooenable TDE na.
   ![Vyberte databázi SQL tooenable TDE na][2]
3. Na hello **transparentní šifrování dat** vyberte **ON** pod šifrování dat a vyberte **Uložit** nejvyšší pásu karet hello hello okna.
   ![Zapnout šifrování TDE][3]

   Jakmile je povolené šifrování TDE na hello vybrané databáze SQL, hello **stav šifrování** změní příliš**šifrovaný**.    

   ![Stav šifrování][4]

## <a name="see-also"></a>Viz také
Tento článek ukázal, jak tooimplement hello Security Center doporučení "Povolit transparentní šifrování dat." toolearn Další informace o šifrování TDE SQL, najdete v části hello následující:

* [Transparentní šifrování dat s databází Azure SQL](https://msdn.microsoft.com/library/dn948096)
* [Začínáme s transparentní šifrování dat (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
