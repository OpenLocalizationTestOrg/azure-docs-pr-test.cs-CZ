---
title: "Zřizování aplikace Galerie tooan Azure AD uživatelů tooconfigure aaaHow | Microsoft Docs"
description: "Jak můžete snadno konfigurovat bohaté uživatelský účet zajišťování a rušení zajištění tooapplications již uveden v hello Azure AD Application Gallery"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2c28e59a3ac8f221ed93b2f6b0b1221f7604af23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-user-provisioning-tooan-azure-ad-gallery-application"></a>Jak uživatel tooconfigure zřizování tooan Azure AD application Gallery

*Zřizování účtu uživatele* spočívá hello vytváření, aktualizaci nebo zakázání záznamů účet uživatele v úložišti profil místního uživatele aplikace. Většina cloudu a aplikace SaaS ukládání hello role uživatele a oprávnění v profilu úložiště vlastní místní uživatele a přítomnost takové uživatelský záznam v jejich místní úložiště je *požadované* pro jeden toowork přihlašování a přístupu.

V hello portálu Azure, hello **zřizování** klikněte v levém navigačním podokně hello pro podnikové aplikace zobrazuje, jaké zřizování režimy jsou podporovány pro tuto aplikaci. To může být jednu ze dvou hodnot:

## <a name="configuring-an-application-for-manual-provisioning"></a>Konfigurace aplikace pro ruční zřizování

*Ruční* zřizování znamená, že uživatelské účty musí být vytvořen ručně pomocí metody hello poskytované tuto aplikaci. To může znamenat přihlašování portálu pro správu pro tuto aplikaci a přidání uživatelů pomocí webového uživatelského rozhraní. Nebo může být odesílání tabulku s podrobnostmi účet uživatele, používá mechanismus poskytované tuto aplikaci. Zadaný hello aplikací nebo aplikace kontaktní hello vývojáře toodetermine wat mechanismy jsou dostupné dokumentaci hello.

Pokud ruční režim pouze hello zobrazí se pro danou aplikaci, znamená to, že žádné automatické Azure AD zřizování konektor zatím nebyla vytvořena pro aplikaci hello. Nebo znamená to, že aplikace hello nemá podpora hello předběžné uživatelské rozhraní API pro správu při které toobuild konektoru automatické zřizování.

Pokud chcete toorequest podporu pro automatické zřizování pro danou aplikaci, můžete vyplnit požadavek na <http://aka.ms/aadapprequest>.

## <a name="configuring-an-application-for-automatic-provisioning"></a>Konfigurace aplikace pro automatické zřizování

*Automatické* znamená, že byla vyvinuta Azure AD zřizování konektor pro tuto aplikaci. Další informace o hello Azure AD zřizování služby a jak to funguje, najdete v části [automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).

Další informace o tom, jak tooprovision konkrétní uživatele a skupiny tooan aplikace, najdete v části [Správa zřizování účtu uživatele pro podnikové aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).

Hello skutečné kroky požadované tooenable a nakonfigurovat automatické zřizování lišit v závislosti na aplikaci hello.

>[!NOTE]
>Byste měli začít hledání hello kurz konkrétní toosetting instalace si zřizování pro vaši aplikaci a pomocí těchto kroků tooconfigure aplikace hello i Azure AD toocreate hello zřizování připojení. 
>
>

Kurzy aplikace naleznete na adrese [seznamu kurzy o tooIntegrate SaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

Tooconsider důležité při nastavování zřizování být tooreview a konfigurace mapování atributů hello a pracovní postupy, které definují vlastnosti toku z Azure AD toohello aplikace které uživatele (nebo skupiny). To zahrnuje nastavení hello "odpovídající vlastnost", který se použité toouniquely identifikovat a odpovídají uživatele nebo skupiny mezi hello dvěma systémy. Další informace o tomto důležité procesu.

## <a name="next-steps"></a>Další kroky
[Přizpůsobení mapování atributů zřizování pro aplikace SaaS ve službě Azure Active Directory uživatelů](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

