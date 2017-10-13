---
title: "Azure Active Directory hybridní identity aspekty návrhu - určit požadavky řízení přístupu | Microsoft Docs"
description: "Obsahuje identitu a identifikovat požadavky přístup k prostředkům pro uživatele v hybridním prostředí."
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
ms.openlocfilehash: 6404940da460461632616fe49f055d50c2a7aba3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Určete požadavky řízení přístupu pro vaše řešení hybridní identity
Při organizace je návrhu jejich hybridní řešení identit může také používat tuto příležitost ke kontrole požadavků na prostředky, které jsou plánování a zpřístupněte ji pro uživatele přístup. Přístup k datům mezi všechny čtyři pilíře identity, které jsou:

* Správa
* Authentication
* Autorizace
* Auditování

Oddíly, které řídí se vztahují na ověřování a autorizace ve další podrobnosti, auditování a správu jsou součástí životního cyklu hybridní identity. Čtení [určit úlohy správy hybridní identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) pro další informace o těchto funkcích.

> [!NOTE]
> Čtení [The čtyři pilíře z Identity - Identity Management stáří hybridního IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) Další informace o každé z nich tyto pilíře chybí.
> 
> 

## <a name="authentication-and-authorization"></a>Ověřování a autorizace
Existují různé scénáře pro ověřování a autorizaci, tyto scénáře bude mít specifické požadavky, které musí splňovat hybridní řešení identit, které společnost přechází přijmout. Scénáře zahrnující komunikace Business to Business (B2B) můžete přidat další výzvy pro správce IT, vzhledem k tomu, že bude nutné zajistit, že organizace používá metodu ověřování a autorizace může komunikovat s jejich obchodními partnery. Během procesu návrhu pro požadavky na ověřování a autorizaci zkontrolujte odpovědi na tyto otázky:

* Bude vaše organizace ověřování a autorizaci jenom uživatelé nacházející se v jejich systém správy identity?
  * Jsou všechny plány pro scénáře B2B?
  * Pokud ano, už znáte protokoly, které (SAML, OAuth, protokol Kerberos, tokeny nebo certifikáty) se použije k připojení obou firmy?
* Podporuje řešení hybridní identity, která se má přijmout podporu těchto protokolů?

Dalším důležitým bodem ke zvážení je, kde budou umístěné úložiště ověřování, který se použije uživatelů a partnery a modelu správy, který se má použít. Vezměte v úvahu následující dvě jádra:

* Centralizované: v tomto modelu přihlašovací údaje uživatele, zásad a správu může být centralizovaný místně nebo v cloudu.
* Hybridní: v tomto modelu přihlašovací údaje uživatele, zásad a správu bude centralizovaný místně a replikované v cloudu.

Modelu zavede vaší organizace se bude lišit podle jejich obchodní požadavky, je vhodné zodpovědět následující otázky k identifikaci, kde se bude nacházet systém správy identit a správu režimu použít:

* Vaše organizace aktuálně má správy identit místně?
  * Pokud ano, jejich plánování zajistit jeho?
  * Existují nějaké požadavky nařízení nebo dodržování předpisů, vaše organizace postupujte této určují, kde by měl být umístěn do systému správy identity?
* Vaše organizace používá jednotné přihlašování pro aplikace umístěné v místě nebo v cloudu?
  * Pokud ano, přijetí modelu hybridní identity vliv tento proces?

## <a name="access-control"></a>Access Control
Při ověřování a autorizace jsou základní prvky pro povolení přístupu k firemním datům prostřednictvím ověření uživatele, je také důležité řídit úroveň přístupu, který tito uživatelé budou mít a úroveň správci přístupu bude mít přes prostředky jejich správu. Hybridní řešení identit musí být schopen poskytnout podrobné přístup k prostředkům, delegování a řízení přístupu na základě rolí. Ujistěte se, že odpovědi následující otázky týkající se řízení přístupu:

* Má vaše společnost více než jeden uživatel s oprávněním vyšší úrovně oprávnění ke správě systému identity?
  * Pokud ano, každý uživatel potřebuje stejnou úroveň přístupu?
* Vaše společnost potřebovat delegovat přístup k uživatelům spravovat konkrétní prostředky?
  * Pokud ano, jak často k tomu dojde?
* Vaše společnost potřebovat k integraci funkcí řízení přístupu mezi místními a cloudovými prostředky?
* Omezit přístup k prostředkům na základě některé podmínky potřebovat vaše společnost?
* By vaše společnost nějaké jakékoli aplikace, která potřebuje vlastního ovládacího prvku přístup k některým prostředkům?
  * Pokud ano, kde jsou tyto aplikace umístěné (místně nebo v cloudu)?
  * Pokud ano, kde jsou ty, cílové prostředky uložené (místně nebo v cloudu)?

> [!NOTE]
> Zajistěte, aby poznamenejte každou odpověď a pochopit důvody odpověď. [Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po dostupných možností a výhod i nevýhod jednotlivých možností.  Odpovědi na tyto otázky budou vybírat, která možnost nejlépe vyhovuje vašim obchodním potřebám.
> 
> 

## <a name="next-steps"></a>Další kroky
[Stanovení požadavků na reakce na incidenty](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

