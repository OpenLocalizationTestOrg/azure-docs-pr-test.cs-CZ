---
title: "spolupráce aaaAzure Active Directory s B2B licencování pokyny | Microsoft Docs"
description: "Azure Active Directory s B2B, které nevyžaduje žádný spolupráce placené licence služby Azure AD, ale můžete také platby funkce za uživatele typu Host B2B"
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
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: sasubram
ms.custom: it-pro
ms.openlocfilehash: 8e15d66209b090bff210674ecdacc88cd642dcc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Doprovodné materiály k licencování spolupráce Azure Active Directory s B2B

Můžete vytvořit uživatele typu Host tooinvite možnosti spolupráce Azure AD B2B do vaší tooallow klienta Azure AD je tooaccess služby Azure AD a dalším prostředkům ve vaší organizaci. Pokud chcete, aby funkce toopaid Azure AD tooprovide přístupu, musí mít licenci uživatele typu Host spolupráce B2B příslušné licence Azure AD. 

Zejména:
* Volné možnosti Azure AD jsou k dispozici pro uživatele typu Host bez další licence.
* Pokud chcete, aby tooprovide přístup toopaid Azure AD funkce tooB2B uživatelé, musí mít dostatek licencí toosupport tyto uživatele typu Host B2B.
* Klient služby pozváním s placenou licenci Azure AD má B2B spolupráce použití rights tooan další pět B2B hosta uživatele pozvat toohello klienta.
* Hello zákazníka, který vlastní hello pozvání klienta musí být hello jeden toodetermine třeba kolik uživatelů spolupráce B2B placené možnosti služby Azure AD. V závislosti na placené služby Azure AD hello funkce, které chcete použít pro vaše uživatele typu Host, musí mít dostatečně Azure AD placené licence uživatelům spolupráce toocover B2B v hello stejné poměr 5:1.

Uživatel guest spolupráce B2B je přidána jako uživatel z do partnerské firmy, není zaměstnanec vaší organizace nebo zaměstnanec různých obchodních ve vašem konglomerátu. Uživatel guest B2B můžete přihlásit pomocí externí přihlašovací nebo přihlašovací údaje ve vlastnictví ve vaší organizaci, jak je popsáno v tomto článku. 

Jinými slovy B2B licencování nastavena není jak hello uživatel se ověřuje, ale spíš hello vztah hello uživatele tooyour organizace. Pokud tito uživatelé nejsou partnery, zachází jinak s podmínkami licenční smlouvy. Nejsou považovány za toobe uživatele spolupráce B2B pro účely licencování i v případě, že jejich UserType je označena jako "Guest." By měly být licencované za normálních okolností na jednu licenci na uživatele. Mezi tyto uživatele patří:
* Vaši zaměstnanci
* Zaměstnanci přihlášení pomocí externí identity
* Zaměstnanec různých obchodních ve vašem konglomerátu


## <a name="licensing-examples"></a>Licencování příklady
- Zákazník chce tooinvite 100 B2B spolupráce uživatele tooits klienta Azure AD. Zákazník Hello přiřadí správu přístupu a zřizování pro všechny uživatele, ale 50 uživatelů také vyžadovat vícefaktorové ověřování a podmíněného přístupu. Tohoto zákazníka, musí si koupit 10 licence Azure AD Basic a 10 toocover licence Azure AD Premium P1 tito uživatelé B2B správně. Pokud zákazník hello plánuje funkce ochrany identit toouse s B2B uživateli, Azure AD Premium P2 licence toocover hello pozvánku, uživatelé musí mít v hello stejné poměr 5:1.
- Zákazník má 10 zaměstnanci, kteří mají všechny aktuálně licenci s Azure AD Premium P1. Nyní chtějí tooinvite 60 B2B uživatelů, kteří všechny vyžadovat vícefaktorové ověřování (MFA). V části pravidla licencování hello 5:1, hello zákazník musí mít nejméně 12 toocover licence Azure AD Premium P1 všichni uživatelé 60 spolupráce B2B. Vzhledem k tomu, že už mají 10 Premium P1 licence pro jejich 10 zaměstnanci, mají práva tooinvite 50 B2B uživatelů s Premium P1 funkcí, jako je vícefaktorové ověřování. V tomto příkladu pak se musí zakoupit 2 Další Premium P1 licence toocover hello zbývající 10 B2B spolupráce uživatelů.

> [!NOTE]
> Neexistuje žádný způsob ještě tooassign licence přímo toohello B2B uživatelé tooenable tato práva uživatele spolupráce B2B.

Hello zákazníka, který vlastní hello pozvání klienta musí být hello jeden toodetermine třeba kolik uživatelů spolupráce B2B placené možnosti služby Azure AD. V závislosti na tom, které placené funkce Azure AD, kterou chcete použít pro vaše uživatele typu Host musí mít dostatečně Azure AD placené licence toocover B2B spolupráce uživatele v poměru hello 5:1. 

## <a name="additional-licensing-details"></a>Další podrobnosti o licencování
- Není nutné tooactually přiřazení licence tooB2B uživatelské účty. Na základě hello 5:1 poměru, licencování je automaticky vypočítat a.
- Pokud ne, placené licence služby Azure AD existuje v hello klienta, každých pozvané uživatele získá hello práva, která hello Azure AD Free edition nabídky.
- Pokud spolupráce uživatel už má placené služby Azure AD B2B licence z jejich organizace, spotřebování není jedním z licence spolupráce hello B2B hello pozváním klienta.

## <a name="advanced-discussion-what-are-hello-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Rozšířené diskusní: co jsou hello licenční požadavky při nemůžeme přidat uživatele z konglomerátu organizace jako "členy" pomocí vašich rozhraní API?
Uživatel typu Host B2B je ten, který je pozvat z toowork organizace partnera s hello hostitele organizace. Obvykle všech ostatních případech není způsobilá jako B2B i ji používá funkce B2B. Podívejme se na dvou případech konkrétně:

1. Pokud hostitel žádostí zaměstnanec pomocí adresy příjemce
  * Tento scénář není kompatibilní se zásadami naše licencování a nedoporučuje se používat.

2. Pokud organizace hostitele přidá uživatele z jiné organizace, konglomerátu
  1. V takovém případě hello uživatel vyzván pomocí rozhraní API B2B, ale tento případ se nedá tradičně B2B. V ideálním případě by měl mít tyto organizace pozvání hello jiných uživatelů orgs jako členy (našem rozhraní API umožňuje které). V takovém případě licence mít toobe přiřazené toothese členy pro ně tooaccess prostředky v hello pozvání organizace.

  2. Některé organizace rozhodnout tooadd hello toobe jiné organizace uživatele přidat jako "Guest" jako zásada. Jsou zde dva případy:
      * Hello konglomerátu organizace už používá Azure AD a hello pozvánku, uživatelé jsou licenci v hello druhé organizace: v tomto případě není Očekáváme, že pozvané uživatele tooneed toofollow hello 1:5 vzorec nastíněny dříve v tomto dokumentu. 

      * Hello konglomerátu organizace nepoužívá Azure AD nebo nemá dostatek licencí: V tomto případě postupujte podle hello 1:5 vzorec nastíněny dříve v tomto dokumentu.

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [elementy Hello hello e-mailová pozvánka pro spolupráci B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení](active-directory-b2b-api.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
