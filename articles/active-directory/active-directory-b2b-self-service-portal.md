---
title: "Služba aaaSelf registrační portál pro spolupráci Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce Azure Active Directory s B2B podporuje vaše vztahy povolením obchodní partnery tooselectively přístup k podnikovým aplikacím"
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
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: c78920ecf812f7efc06a8b54b1fff834c32904f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Samoobslužný portál pro registraci spolupráce Azure AD B2B

Zákazníci můžete dělat mnohem s hello integrované funkce, které jsou k dispozici prostřednictvím našich správce IT [portál Azure](https://portal.azure.com) a na našem [Panel přístupu aplikace](https://myapps.microsoft.com) pro koncové uživatele. Ale máme také informace, že podnikům nutnost pracovního postupu registrace hello toocustomize B2B uživatelé toofit potřebám své organizace. Dělají to pomocí [našem rozhraní API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Diskuze s našich zákazníků vidíte, že jeden běžné potřebovat zvýšení až nad všemi ostatními. Hello pozvání organizace nemusí vědět předem, který text hello, že jsou jednotlivé externími spolupracovníky, kteří potřebují přístup k prostředkům tootheir. Však chtěli způsob, jak pro uživatele z partnerských společností příliš zaregistrovat sami pomocí sady zásad tohoto hello pozvání ovládací prvky organizace. Tento scénář je možné provádět prostřednictvím rozhraní API, takže jsme publikovaná projektu na Githubu, který se právě který: [ukázkový projekt Githubu](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Naše Githubu projektu ukazuje, jak můžete použít rozhraní API organizace a poskytovat na základě zásad, samoobslužné funkce registrace pro jejich důvěryhodným partnerům s pravidla, která určují hello aplikace, které získají přístup k. Uživatelům z partnerských můžete získat přístup tooresources, kdy je potřebují, bezpečně, bez nutnosti hello pozvání zaváděním toomanually organizace je. Můžete snadno nasadit hello projektu do předplatného služby Azure podle svého výběru.

## <a name="as-is-code"></a>Jako-je kód

Mějte na paměti, že tento kód je k dispozici jako použití ukázkové toodemonstrate hello Azure Active Directory s B2B pozvání rozhraní API. Je nutné přizpůsobit svým týmem vývojářů nebo partnera a by měl být zkontrolovány před nasazením v produkční scénář.

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:
* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [elementy Hello hello e-mailová pozvánka pro spolupráci B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)