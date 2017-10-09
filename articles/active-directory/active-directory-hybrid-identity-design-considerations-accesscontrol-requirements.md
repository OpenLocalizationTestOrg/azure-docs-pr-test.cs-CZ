---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - určit požadavky řízení přístupu | Microsoft Docs"
description: "Zahrnuje hello identity a identifikace požadavků na prostředky pro uživatele v hybridním prostředí přístup."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: f0c22629f732a4c13ee7a24456651bec7637c387
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Určete požadavky řízení přístupu pro vaše řešení hybridní identity
Při organizace je návrhu jejich hybridní řešení identit může také používat tuto možnost tooreview přístup k požadavkům hello prostředky, se plánováním toomake je k dispozici pro uživatele. přístup k datům Hello mezi všechny čtyři pilíře identity, které jsou:

* Správa
* Authentication
* Autorizace
* Auditování

Hello oddíly, které řídí se vztahují na ověřování a autorizace ve další podrobnosti, auditování a správu jsou součástí hello hybridní identity životního cyklu. Čtení [určit úlohy správy hybridní identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) pro další informace o těchto funkcích.

> [!NOTE]
> Čtení [hello čtyři pilíře z Identity - Identity Management hello stáří hybridního IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) Další informace o každé z nich tyto pilíře chybí.
> 
> 

## <a name="authentication-and-authorization"></a>Ověřování a autorizace
Existují různé scénáře pro ověřování a autorizaci, tyto scénáře bude mít specifické požadavky, které musí splňovat hello hybridní řešení identit, je hello společnosti tooadopt probíhající. Scénáře zahrnující obchodní tooBusiness (B2B) komunikace můžete přidat další výzvu pro správce IT vzhledem k tomu, že bude nutné tooensure, který hello ověřování a autorizace metodu používanou hello organizace může komunikovat s jejich obchodními partnery. Během procesu ověřování a autorizaci požadavků na návrh hello Ujistěte se, že hello následující otázky jsou zodpovězeny:

* Bude vaše organizace ověřování a autorizaci jenom uživatelé nacházející se v jejich systém správy identity?
  * Jsou všechny plány pro scénáře B2B?
  * Pokud ano, už víte, které protokoly (SAML, OAuth, protokol Kerberos, tokeny nebo certifikáty) se být použité tooconnect obou firmy?
* Podporuje řešení hybridní identity hello budete tooadopt tyto protokoly?

Jinou důležitá tooconsider bod je, kde budou umístěné hello ověřování úložiště, který se použije uživatelů a partnery a toobe model správy hello používá. Vezměte v úvahu hello následující dvě základní možnosti:

* Centralizované: v této modelu hello přihlašovací údaje uživatele, zásad a správu může být centralizovaný místně nebo v cloudu hello.
* Hybridní: v této modelu hello přihlašovací údaje uživatele, zásad a správu bude centralizovaný místně a replikované v cloudu hello.

Vaše organizace zavede modelu se bude lišit podle tootheir obchodní požadavky, je vhodné tooanswer hello následující otázky tooidentify, kde bude systém správy identit hello nacházet a hello toouse správu režimu:

* Vaše organizace aktuálně má správy identit místně?
  * Pokud ano, proveďte jejich plánování tookeep ho?
  * Existují nějaké požadavky nařízení nebo dodržování předpisů, vaše organizace postupujte této určují, kde by měl být umístěn systém správy identit hello?
* Vaše organizace používá jednotné přihlašování pro aplikace umístěné v místě nebo v cloudu hello?
  * Pokud ano, hello přijetí modelu hybridní identity vliv tento proces?

## <a name="access-control"></a>Access Control
Při ověřování a autorizace jsou základní prvky tooenable přístup toocorporate data prostřednictvím ověření uživatele, je také důležité toocontrol hello úroveň přístupu, který tito uživatelé budou mít a hello úroveň správci přístupu bude mít přes hello prostředky, které budou spravovat. Hybridní řešení identit musí být schopný tooprovide tooresources granulární přístup a delegování řízení přístupu na základě rolí. Ujistěte se, že hello následující otázky jsou zodpovězeny týkající se řízení přístupu:

* Vaše společnost má více než jeden uživatel s oprávněním vyšší úrovně oprávnění toomanage systému identity?
  * Pokud ano, každý uživatel potřebovat hello stejnou úroveň přístupu?
* By vaše podnikové potřeby toodelegate přístup toousers toomanage konkrétní prostředky?
  * Pokud ano, jak často k tomu dojde?
* Vaše společnost potřebovat možnosti řízení přístupu toointegrate mezi místními a cloudovými prostředky?
* Vaše společnost potřebovat tooresources toolimit přístup podle podmínek toosome?
* By vaše společnost nějaké jakékoli aplikace, která potřebuje vlastního ovládacího prvku, přístup k prostředkům toosome?
  * Pokud ano, kde jsou tyto aplikace umístěné (místně nebo v cloudu hello)?
  * Pokud ano, kde jsou ty, cílové prostředky uložené (místně nebo v cloudu hello)?

> [!NOTE]
> Ujistěte se, že tootake poznámky u každé odpovědi a pochopit hello důvody, které hello odpovědí. [Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po hello dostupných možností a výhod i nevýhod jednotlivých možností.  Odpovědi na tyto otázky budou vybírat, která možnost nejlépe vyhovuje vašim obchodním potřebám.
> 
> 

## <a name="next-steps"></a>Další kroky
[Stanovení požadavků na reakce na incidenty](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

