---
title: "aaaCompare B2B spolupráce a v Azure Active Directory B2C | Microsoft Docs"
description: "Jaký je rozdíl hello spolupráce Azure Active Directory B2B a Azure AD B2C?"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 34d88b9a7d023e077568e8df3d5e1610ae05b361
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Porovnání spolupráce B2B a B2C v Azure Active Directory

Spolupráce B2B Azure Active Directory (Azure AD) a Azure AD B2C můžete toowork s externími uživateli ve službě Azure AD. Ale jak porovnávají?


Možnosti spolupráce B2B |     Azure AD B2C samostatné nabídky
-------- | --------
Určený pro: organizace, které mají uživatelé možnost tooauthenticate toobe od organizace partnera, bez ohledu na to zprostředkovatele identity. | Určený pro: pozvání zákazníkům mobilní zařízení a webové aplikace, zda jednotlivce, institucionální nebo organizace zákazníků do služby Azure AD.
Podporované identit: zaměstnanci s pracovní nebo školní účty, partneři s pracovní nebo školní účty nebo všechny e-mailovou adresu. Brzy toosupport přímé federace.  | Podporované identit: spotřebitelské uživatele s účty místních aplikací (všechny e-mailové adresy nebo uživatelské jméno) ani žádný podporované sociálních identitu pomocí přímého federation.
Které directory hello partnera uživatelé jsou v: partnera uživatele organizaci externí hello se spravují v hello stejného adresáře jako zaměstnanci, ale s poznámkami speciálně. Je možné spravovat hello stejný způsobem jako zaměstnanci, může být přidané toohello stejné skupiny a tak dále  | Které entitami uživatelů zákazníků hello adresáře jsou v: V adresáři aplikace hello. Spravované samostatně z organizace hello zaměstnanců a partnera adresář (pokud existuje.
Jednotné přihlašování (SSO) tooall Azure AD připojené aplikace je podporována. Můžete například zadat přístup tooOffice 365 nebo místními aplikacemi a aplikacemi SaaS tooother například služby Salesforce nebo Workday.  |  Jednotné přihlašování toocustomer vlastní aplikace v klientech hello Azure AD B2C je podporována. Jednotné přihlašování tooOffice 365 nebo Microsoft a Microsoft SaaS aplikace tooother není podporována.
Životní cyklus partnera: spravuje hello hostitele nebo pozvání organizace.  | Životní cyklus zákazníka: samoobslužný postup nebo spravované aplikace hello.
Zásady zabezpečení a dodržování předpisů: spravuje hello hostitele nebo pozvání organizace.  | Zásady zabezpečení a dodržování předpisů: spravuje aplikace hello.
Branding: Brand hostitele nebo pozvání organizace se používá.  |    Branding: Spravované aplikace. Obvykle se obvykle toobe produktu s hello organizace směrem do hello pozadí značky.
Další informace: [příspěvku na blogu](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [dokumentace](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  | Další informace: [stránky produktu](https://azure.microsoft.com/en-us/services/active-directory-b2c/), [dokumentace](https://docs.microsoft.com/en-us/azure/active-directory-b2c/)


### <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání role uživatele tooa spolupráce B2B](active-directory-b2b-add-guest-to-role.md)
* [Delegovat pozvánek spolupráce B2B](active-directory-b2b-delegate-invitations.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
* [Získání podpory pro spolupráci B2B](active-directory-b2b-support.md)
