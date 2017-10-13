---
title: "Postup konfigurace hesel jednotné přihlašování pro jiný Galerie applicationn | Microsoft Docs"
description: "Jak nakonfigurovat vlastní aplikace bez Galerie pro zabezpečení založené na heslech jednotné přihlašování, když není uveden v galerii aplikací Azure AD"
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
ms.openlocfilehash: f629ec99824199306ebf825901beaa99d83d434d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Postup konfigurace hesel jednotné přihlašování pro aplikace bez Galerie

Kromě volby najít v galerii aplikací Azure AD, máte také možnost Přidat **aplikace bez Galerie** při aplikaci, kterou má není v seznamu uvedeno. Díky této funkci můžete přidat libovolné aplikace, která již existuje ve vaší organizaci, nebo jakékoli aplikaci jiného výrobce, který můžete použít od dodavatele, který není již součástí [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

Jakmile přidáte aplikace bez galerie, potom můžete nakonfigurovat jednu metodu přihlašování tato aplikace používá výběrem **jednotné přihlašování** navigační položka na podniková aplikace v [portálu Azure](https://portal.azure.com/).

Jeden z jednotného přihlašování metod k dispozici je [založené na heslech jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) možnost. Pomocí **přidání aplikace bez Galerie** prostředí, můžete integrovat jakékoli aplikace, která vykreslí uživatelské jméno ve formátu HTML a heslo vstupní pole, i když není v našem sadu předem integrovaných aplikací.

Na stránce oškrabávání technologie, která je součástí přístupového panelu linku, která umožňuje automaticky rozpoznat uživatelské jméno a heslo vstupní pole, bezpečně uložit pro konkrétní aplikaci instanci, je způsob, jakým tento postup funguje. Bezpečně přehráním uživatelských jmen a hesel do těchto polí, když uživatel přejde k dané aplikaci na panel přístupu aplikace.

To je skvělý způsob, jak začít rychle integraci jakékoliv aplikace do Azure AD a umožňuje:

-   Integrovat **všechny aplikace na světě** s klientovi Azure AD, takže pokud vykreslí uživatelské jméno a heslo vstupní pole HTML

-   Povolit **jednotné přihlašování pro vaše uživatele** bezpečně ukládání a přehrání uživatelských jmen a hesel pro aplikaci jste integrované s Azure AD

-   **Automaticky rozpoznat vstup** polí pro každou aplikaci, a umožňují ručně zjistit těchto polí pomocí rozšíření prohlížeče Panel přístupu, v případě, že je nenajde automatického zjištění

-   **Podpora aplikací, které vyžadují více polí přihlášení** pro aplikace, které vyžadují více než jen uživatelské jméno a heslo pole pro přihlášení

-   **Přizpůsobení popisků** vstupní pole uživatelské jméno a heslo se uživatelům zobrazí na [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) při jejich zadat své přihlašovací údaje

-   Povolit vaše **uživatelé** zajistit vlastní uživatelská jména a hesla pro všechny existující účty aplikace zadávání v ručně na [aplikace přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Povolit **členem obchodní skupině** a zadat uživatelská jména a hesla přiřazen k uživateli pomocí [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funkce

-   Povolit **správce** k určení uživatelských jmen a hesel přiřazen k uživateli pomocí přihlašovacích údajů aktualizace funkcí při [přiřazení uživatele k aplikaci](#_How_to_configure_1)

-   Povolit **správce** k určení sdílené uživatelské jméno nebo heslo použité pro skupinu uživatelů pomocí přihlašovacích údajů aktualizace funkcí při [přiřazení skupiny k aplikaci](#assign-an-application-to-a-group-directly)

Níže popisuje, jak můžete povolit [založené na heslech jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) do jakékoli aplikace, která přidáte pomocí **přidání aplikace bez Galerie** prostředí.

## <a name="overview-of-steps-required"></a>Přehled kroků potřebných

Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:

-   [Přidání aplikace bez Galerie](#add-a-non-gallery-application)

-   [Konfigurace aplikace pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

-   [Přiřazení aplikace k uživatele nebo skupinu](#assign-the-application-to-a-user-or-a-group)

    -   [Přiřazení uživatele k aplikaci přímo](#assign-a-user-to-an-application-directly)

    -   [Přiřadit přímo aplikaci pro skupinu](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a>Přidání aplikace bez Galerie

Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:

1.  Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno

6.  Klikněte na tlačítko **Galerie jiná aplikace.**

7.  Zadejte název vaší aplikace v **název** textové pole. Vyberte **přidat.**

Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.

## <a name="configure-the-application-for-password-single-sign-on"></a>Konfigurace aplikace pro heslo jednotné přihlašování

Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

  * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.

7.  Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.

8.  Vyberte režim **založené na heslech přihlášení.**

9.  Zadejte **přihlašovací adresa URL**. Toto je adresa URL, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci. Zkontrolujte, zda že přihlašovací pole jsou viditelné na adrese URL.

10. Přiřazení uživatelů k aplikaci.

11. Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele. Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.

## <a name="assign-a-user-to-an-application-directly"></a>Přiřazení uživatele k aplikaci přímo

Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

  * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.

7.  Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.

8.  Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.

9.  Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.

10. Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Najeďte myší **uživatele** v seznamu na nich **políčko**. Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.

13. Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.

14. **Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.

15. Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.

## <a name="assign-an-application-to-a-group-directly"></a>Přiřadit přímo aplikaci pro skupinu

Jeden nebo více skupin přiřadit přímo k aplikaci, postupujte podle následujících kroků:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

  * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.

7.  Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.

8.  Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.

9.  Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.

10. Zadejte **název úplné skupiny** vás zajímá přiřazení do skupiny **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Najeďte myší **skupiny** v seznamu na nich **políčko**. Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do skupiny **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli **přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko k přidání do této skupiny **vybrané** seznamu.

13. Po dokončení výběru skupiny klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.

14. **Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit ke skupinám, které jste vybrali.

15. Klikněte **přiřadit** tlačítko přiřazení aplikace k vybraným skupinám.

Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace na přístupovém panelu.

## <a name="next-steps"></a>Další kroky
[Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
