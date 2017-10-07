---
title: "mapování v Azure Active Directory deklarace identity uživatele spolupráce aaaB2B | Microsoft Docs"
description: "referenční dokumentace pro Azure Active Directory s B2B spolupráce mapování deklarace identity"
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
ms.openlocfilehash: 9e26085e91a6004b2f11286ae9c1df133bd47341
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Mapování v Azure Active Directory deklarace identity uživatele spolupráce B2B

Přizpůsobení hello deklarace identity vystavené v tokenu SAML hello uživatelům spolupráce B2B Azure podporuje služby Active Directory (Azure AD). Když se uživatel ověřuje toohello aplikace, Azure AD vydá token toohello aplikace SAML, který obsahuje informace (nebo deklarace identity) o hello uživateli, který jedinečně identifikuje je. Ve výchozím nastavení jedná se o uživatelské jméno, e-mailová adresa, jméno a příjmení hello uživatele. Můžete zobrazit nebo upravit hello deklarace identity odeslat v hello aplikace toohello tokenu SAML kartě atributy hello.

Existují dvě možné důvody, proč může být nutné tooedit hello deklarace identity vystavené v tokenu SAML hello.

1. aplikace Hello byla zapsána toorequire jinou sadu deklarací identity identifikátory URI nebo hodnot deklarací identity

2. Vaše aplikace vyžaduje hello NameIdentifier deklarace identity toobe něco jiného než hello hlavní uživatelské jméno uložené ve službě Azure Active Directory.

  ![Zobrazení deklarace identity v tokenu SAML](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Informace o tom, jak tooadd a úpravy deklarací, projděte si tento článek na přizpůsobení deklarace identity, [přizpůsobení deklarace identity vystavené v tokenu hello SAML pro předběžně integrované aplikace v Azure Active Directory](develop/active-directory-saml-claims-customization.md). Pro spolupráci B2B uživatelů, mapování NameID a UPN mezi klienta nelze z bezpečnostních důvodů.


## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání role uživatele tooa spolupráce B2B](active-directory-b2b-add-guest-to-role.md)
* [Delegovat pozvánek B2bB spolupráce](active-directory-b2b-delegate-invitations.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
