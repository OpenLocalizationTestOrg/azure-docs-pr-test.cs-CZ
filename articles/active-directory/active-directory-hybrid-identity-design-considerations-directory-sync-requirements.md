---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity – určení požadavků na synchronizaci adresáře | Microsoft Docs"
description: "Určit, jaké požadavky jsou nezbytné k synchronizaci všech uživatelů hello mezi na = místní a cloud pro hello enterprise."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6646e3792c65f37c3d62eecdb6c6f3bd257f04f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-directory-synchronization-requirements"></a>Určení požadavků na synchronizaci adresáře
Synchronizace slouží k poskytování identity v cloudu hello na základě jejich identity místní uživatelé. Zda se bude používat synchronizované účet pro ověřování nebo federovaného ověřování, hello uživatelé budou potřebovat stále toohave identity v cloudu hello.  Tato identita potřebovat toobe udržuje a pravidelně aktualizuje.  aktualizace Hello mohou mít mnoho forem, od title změny toopassword změny.  

Spusťte vyhodnocením hello organizace místní řešení a uživatelské požadavky identity. Toto testování je důležité toodefine hello technické požadavky na tom, jak bude vytvářené a udržované v cloudu hello identity uživatelů.  Pro většinu organizací je místní služba Active Directory a hello místní adresář, který uživatelé budou podle synchronizovány z, ale v některých případech to nebudou hello případ.  

Ujistěte se, zda text hello tooanswer následující otázky:

* Máte jednu doménovou strukturu AD, několik nebo žádné?
  
  * Kolik adresáře Azure AD bude můžete být synchronizaci?
    
    1. Používáte filtrování?
    2. Máte více serverů Azure AD Connect plánované?
* Aktuálně máte synchronizace nástroje místně?
  
  * Pokud ano, podporuje vaši uživatelé, pokud uživatelé používají virtuální adresář/integrace identit?
* Máte k dispozici žádné jiné na místní adresáře, které chcete toosynchronize (například adresář LDAP, databáze HR atd)?
  * Budete to žádné GALSync toobe?
  * Co je hello aktuální stav UPN ve vaší organizaci? 
  * Máte na jiný adresář, který uživatelé ověřování proti?
  * Používá vaše společnost Microsoft Exchange?
    * Jejich plánování toho, že hybridní nasazení systému exchange?

Teď, když máte představu o synchronizačních požadavcích, musíte toodetermine, který nástroj se správnou jeden toomeet hello tyto požadavky.  Společnost Microsoft poskytuje několik nástrojů pro integraci adresáře tooaccomplish a synchronizace.  V tématu hello [hybridní identita directory integrace nástrojů srovnávací tabulka](active-directory-hybrid-identity-design-considerations-tools-comparison.md) Další informace. 

Nyní když máte požadavků na synchronizaci a hello nástroj, který bude dosáhnout vaší společnosti, musíte tooevaluate hello aplikace, které používají tyto služby adresáře. Toto testování je důležité toodefine hello toointegrate technické požadavky těchto toohello cloudové aplikace. Ujistěte se, zda text hello tooanswer následující otázky:

* Budou tyto aplikace přesunutý toohello cloudu a použít hello adresář?
* Jsou nějaké speciální atributy, které potřebují toobe synchronizované toohello cloudu, aby se tyto aplikace můžete úspěšně používat?
* Tyto aplikace potřebovat toobe znovu zapsat tootake výhod cloudové ověřování?
* Tyto aplikace budou toolive místní při uživatelé přistupovat k nim pomocí hello cloudové identity?

Budete také potřebovat toodetermine hello zabezpečení požadavky a omezení synchronizace adresářů. Toto testování je důležité tooget seznam hello požadavků, které bude potřeba v pořadí toocreate a udržovat identity uživatele v cloudu hello. Ujistěte se, zda text hello tooanswer následující otázky:

* Kde budou serveru pro synchronizaci hello umístěné?
* Bude se připojený k doméně?
* Bude hello server nacházet v omezené síti za bránou firewall, jako je například DMZ?
  * Bude moct tooopen hello vyžaduje brány firewall porty toosupport synchronizace?
* Máte v plánu zotavení po havárii pro server synchronizace hello?
* Máte účet s hello správná oprávnění pro všechny doménové struktury, které chcete toosynch s?
  * Pokud vaše společnost neví hello odpověď pro tuto otázku, projděte si část hello "Oprávnění pro synchronizaci hesel" v článku hello [hello instalace služby Azure Active Directory Sync](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) a zjistit, pokud již máte účet se tato oprávnění nebo pokud potřebujete toocreate jeden.
* Pokud máte doménovou strukturou třeba synchronizace je doménové struktury hello synchronizační server dokáže tooget tooeach?

> [!NOTE]
> Ujistěte se, že tootake poznámky u každé odpovědi a pochopit hello důvody, které hello odpovědí. [Stanovení požadavků na reakce na incidenty](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) budou přenášeny po hello možnosti, které jsou k dispozici. Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.
> 
> 

## <a name="next-steps"></a>Další kroky
[Stanovení požadavků na službu Multi-Factor authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

