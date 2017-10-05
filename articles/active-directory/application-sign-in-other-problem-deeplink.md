---
title: "Potíže při přihlašování k aplikaci pomocí odkazu deeplink | Microsoft Docs"
description: "Řešení potíží při přístupu k aplikaci z adresy URL odkazu deeplink pomocí služby Azure AD"
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
ms.openlocfilehash: 798015ab68afc65378cffc75afec9c7f91fc1926
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>Potíže při přihlašování k aplikaci pomocí odkazu deeplink

Přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) k zobrazení a spustit cloudové aplikace, aby správce Azure AD má je přístup povolen. 

Tyto aplikace jsou konfigurovány jménem uživatele na portálu Azure AD. Aplikace musí být správně nakonfigurovaná a přiřadit uživatele nebo skupiny, kterých je uživatel členem skupiny chcete vidět aplikaci na přístupovém panelu.

Přístup uživatele nebo přímých odkazů URL jsou odkazy, které vaši uživatelé mohou využít k přístupu k aplikacím heslo SSO přímo z jejich řádky adresu URL prohlížeče. Přechodem na tento odkaz, uživatelé budou automaticky přihlášení k aplikaci bez nutnosti nejprve přejít na Panel přístupu. Toto je stejný odkaz, který uživatelé používat pro přístup k těmto aplikacím z Spouštěč aplikace Office 365.

## <a name="general-issues-to-check-first"></a>Běžné problémy a proveďte nejprve kontrolu

-   Zajistěte, aby vaše použití **prohlížeče** , splňuje minimální požadavky na přístupový Panel.

-   Zajistěte, aby prohlížeče uživatele přidala adresu URL aplikace pro jeho **Důvěryhodné servery**.

-   Ujistěte se, zkontrolujte, že aplikace je **nakonfigurované** správně.

-   Zkontrolujte, zda je účet uživatele **povoleno** pro přihlášení.

-   Zkontrolujte, zda je účet uživatele **není uzamčen.**

-   Zkontrolujte, že uživatele **není platnost nebo zapomenete heslo.**

-   Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.

-   Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.

-   Ujistěte se, že uživatele **ověřování kontaktní údaje** je aktuální povolit Multi-Factor Authentication nebo podmíněného přístupu zásad, která budou vynucena.

-   Zkontrolujte, zda také zkuste vymazání souborů cookie v prohlížeči a zkusit se přihlásit znovu.

## <a name="checking-the-deeplink"></a>Kontrola odkazu deeplink

Pokud chcete zkontrolovat, jestli máte správné odkazu deeplink, postupujte podle následujících kroků:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

  * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

7.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

8.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

9.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

10. Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

   * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

11. Vyberte aplikaci, kterou chcete kontrolu odkazu deeplink pro.

12. Najít popisek **adresu URL pro přístup uživatelů**. Tato adresa URL by měl odpovídat můžete odkazu deeplink.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Postup instalace rozšíření přístup k panelu prohlížeče

Chcete-li nainstalovat rozšíření přístup k panelu prohlížeče, postupujte následujícím způsobem:

1.  Otevřete [přístupový Panel](https://myapps.microsoft.com) v jednom z podporovaných prohlížečích a přihlaste se jako **uživatele** ve službě Azure AD.

2.  Klikněte na tlačítko **SSO heslo aplikace** na přístupovém panelu.

3.  V řádku, požádá o instalaci softwaru, vyberte **instalovat nyní**.

4.  Založené na prohlížeči budete přesměrováni na odkaz ke stažení. **Přidat** rozšíření do prohlížeče.

5.  Pokud prohlížeč zobrazí dotaz, vyberte buď **povolit** nebo **povolit** rozšíření.

6.  Po instalaci **restartujte** relaci prohlížeče.

7.  Přihlaste se do přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO

Rozšíření pro Chrome a Firefox může také stáhnout z přímé odkazy níže:

-   [Rozšíření pro Chrome přístup panely](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozšíření panelu Firefox přístup](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Postup konfigurace hesel jednotné přihlašování pro aplikaci Galerie Azure AD

Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:

-   [Přidat aplikaci z Galerie Azure AD](#add-an-application-from-the-Azure-AD-gallery)

-   [Konfigurace aplikace pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Přidat aplikaci z Galerie Azure AD

Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:

1.  Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.

6.  V **zadejte název** textbox z **přidat z Galerie** části, zadejte název aplikace.

7.  Vyberte aplikaci, kterou chcete nakonfigurovat pro jednotné přihlašování.

8.  Před přidáním aplikace, můžete změnit její název ze **název** textové pole.

9.  Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.

Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.

### <a name="configure-the-application-for-password-single-sign-on"></a>Konfigurace aplikace pro heslo jednotné přihlašování

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

9.  [Přiřazení uživatelů k aplikaci](#how-to-assign-a-user-to-an-application-directly).

10. Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele. Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Postup konfigurace hesel jednotné přihlašování pro aplikace bez Galerie

Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:

-   [Přidání aplikace bez Galerie](#add-a-non-gallery-application)

-   [Konfigurace aplikace pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Přidání aplikace bez Galerie

Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:

1.  Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.

6.  Klikněte na tlačítko **Galerie jiná aplikace.**

7.  Zadejte název vaší aplikace v **název** textové pole. Vyberte **přidat.**

Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.

### <a name="configure-the-application-for-password-single-sign-on"></a>Konfigurace aplikace pro heslo jednotné přihlašování

Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

    1.  Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.

7.  Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.

8.  Vyberte režim **založené na heslech přihlášení.**

9.  Zadejte **přihlašovací adresa URL**. Toto je adresa URL, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci. Zkontrolujte, zda že přihlašovací pole jsou viditelné na adrese URL.

10. Přiřazení uživatelů k aplikaci.

11. Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele. Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Postup přiřazení uživatele k aplikaci přímo

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

Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace na přístupovém panelu.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Pokud tyto kroky řešení potíží není vyřešte problém. 

Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:

-   ID chyby korelace

-   Hlavní název uživatele (e-mailovou adresu uživatele)

-   TenantID

-   Typ prohlížeče

-   Dojde k časové pásmo a čas nebo období při chybě

-   Fiddler trasování

## <a name="next-steps"></a>Další kroky
[Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
