---
title: "Chyby na stránce aplikace po přihlášení | Microsoft Docs"
description: "Postup řešení potíží s Azure AD přihlášení při vlastní aplikace vysílá chybu"
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
ms.openlocfilehash: a8cd93256f79ece268ec3411dfbdf590f4b24447
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a>Chyby na stránce aplikace po přihlášení

V tomto scénáři podepsané uživatele v Azure AD, ale aplikace zobrazuje chybu neumožňuje uživateli úspěšně dokončit přihlášení toku. V tomto scénáři aplikace nepřijímá problém odpovědi službou Azure AD.

Existují některé možné důvody, proč nebylo aplikace přijmout odpověď z Azure AD. Pokud chyba v aplikaci není dostatečně zrušte vědět, co je v odpovědi chybí pak:

-   Pokud je aplikace galerii Azure AD, ověřte jste postupovali podle pokynů v článku [ladění na základě SAML jednotného přihlašování k aplikacím v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Pomocí některého nástroje, například [Fiddler](http://www.telerik.com/fiddler) zaznamenat žádost SAML, odpověď SAML a tokenu SAML.

-   Odpověď SAML sdílet se na dodavatele aplikace potřebujete vědět, co chybí.

## <a name="missing-attributes-in-the-saml-response"></a>Chybějící atributy v odpovědi SAML

Pokud chcete přidat atribut v konfiguraci služby Azure AD k odeslání v odpovědi Azure AD, použijte následující postup:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

   * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.

7.  Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.

8.  Klikněte na tlačítko **prohlížení a úpravy atributy všechny ostatní uživatele v části** **uživatelské atributy** části, chcete-li upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.

   Chcete-li přidat atribut:

   * Klikněte na tlačítko **přidat atribut**. Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.

   * Klikněte na tlačítko **uložit.** Zobrazí nový atribut v tabulce.

9.  Uložte konfiguraci.

Při příštím přihlášení uživatele k aplikaci, Azure AD odeslat nový atribut v odpovědi SAML.

## <a name="the-application-expects-a-different-user-identifier-value-or-format"></a>Aplikace se očekává formát nebo jinou hodnotu identifikátor uživatele

Přihlášení k aplikaci se nedaří protože odpověď SAML chybí atributy, jako je například role nebo protože aplikace je očekáván jiný formát atribut EntityID.

## <a name="add-an-attribute-in-the-azure-ad-application-configuration"></a>Přidáte atribut v konfiguraci aplikace Azure AD:

Chcete-li změnit hodnotu identifikátoru uživatele, postupujte podle následujících kroků:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

   * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.

7.  Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.

8.  V části **uživatelské atributy**, vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.

## <a name="change-entityid-user-identifier-format"></a>Změna formátu EntityID (identifikátor uživatele)

Pokud aplikace očekává jiného formátu pro atribut EntityID. Potom nebudete moci vybrat formát EntityID (identifikátor uživatele), který odešle aplikaci v odpovědi po ověření uživatele Azure AD.

Azure AD vyberte formát pro atribut NameID (identifikátor uživatele) na základě hodnoty vybraných nebo formát požadovanou aplikaci v SAML AuthRequest. Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy.

## <a name="the-application-expects-a-different-signature-method-for-the-saml-response"></a>Aplikace očekává jiný podpis metody pro odpověď SAML

Chcete-li změnit, které části tokenu SAML jsou digitálně podepsané službou Azure Active Directory. Postupujte následovně:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

  * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.

7.  Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.

8.  Klikněte na tlačítko **zobrazit upřesňující nastavení podpisový certifikát** pod **SAML podpisový certifikát** části.

9.  Vyberte odpovídající **podepisování možnost** očekává aplikací:

  * Přihlašování SAML odpovědi

  * Přihlašování SAML odpovědi a kontrolní výraz

  * Přihlašovací kontrolního výrazu SAML

Při příštím přihlášení uživatele k aplikaci, Azure AD přihlášení součástí vybrané odpověď SAML.

## <a name="the-application-expects-the-signing-algorithm-to-be-sha-1"></a>Aplikace očekává podpisový algoritmus SHA-1

Ve výchozím nastavení Azure AD přihlášení pomocí algoritmu většina zabezpečení tokenu SAML. Změna přihlašovací algoritmus SHA-1 se nedoporučuje, pokud požadované aplikací.

Chcete-li změnit podpisový algoritmus, postupujte podle následujících kroků:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

   * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.

7.  Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.

8.  Klikněte na tlačítko **zobrazit upřesňující nastavení podpisový certifikát** pod **SAML podpisový certifikát** části.

9.  Vyberte algoritmus SHA-1, v **podpisový algoritmus**.

Při příštím přihlášení uživatele k aplikaci, Azure AD přihlášení tokenu SAML pomocí algoritmu SHA-1.

## <a name="next-steps"></a>Další kroky
[Postup ladění na základě SAML jednotného přihlašování k aplikacím v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
