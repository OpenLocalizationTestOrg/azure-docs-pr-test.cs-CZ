---
title: "Přizpůsobení: Azure AD SSPR | Microsoft Docs"
description: "Možnosti přizpůsobení pro Azure AD samoobslužném resetování hesla služby"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 4762fffef040f9b409355f9ee0e8cc593e3eea0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Přizpůsobení funkce služby Azure AD pro samoobslužného resetování hesel

Odborníci v oblasti IT vyhledávání toodeploy samoobslužné resetování hesla můžete přizpůsobit toomatch hello zkušeností uživatelů.

## <a name="customize-hello-contact-your-administrator-link"></a>Přizpůsobení hello obraťte se na odkaz na vaši správce

I když SSPR nejsou povolení uživatelé stále "obraťte se na správce" odkaz na hello heslo resetovat portálu.  Kliknutím na tento odkaz e-maily správce žádostí o pomoc při změně hesla uživatele hello. Tento e-mail je odeslán toohello následující příjemci hello následující pořadí:

1. Pokud hello **heslo správce** role je přiřazená, správci k této roli jsou upozorněni.
2. Pokud žádní správci heslo přiřazené, pak správcům hello **správce uživatele** role jsou upozorněni.
3. Pokud žádná z předchozích role hello byly přiřazeny, pak **globální správci** jsou oznámení

Ve všech případech jsou maximálně 100 příjemce oznámení.

Další informace o rolích jiný správce hello a jak hello tooassign je najdete v dokumentu toofind [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md)

### <a name="disable-contact-your-administrator-emails"></a>Obraťte se na vaši e-mailů správce zakázat

Pokud vaše organizace nechce, že správci oznámení o heslo resetovat požadavky, hello následující konfiguraci můžete povolit.

* Povolte samoobslužné resetování hesla pro všechny uživatele. Tato možnost je v části **resetování hesla > vlastnosti**.
    * Pokud nechcete, aby uživatelé tooreset vlastní hesla, můžete určit obor přístupu tooan prázdné skupiny **nedoporučujeme tuto možnost**.
* Přizpůsobení hello helpdesk odkaz tooprovide adresa URL webové nebo mailto: adresy, které uživatelé mohou používat tooget pomoc. Tato možnost je v části **resetování hesla > Přizpůsobení > vlastní technické podpory e-mailu nebo adresa URL**.

## <a name="customize-adfs-sign-in-page-for-sspr"></a>Přizpůsobit přihlašovací stránku služby AD FS pro SSPR

Správci služby AD FS můžete přidat odkaz tootheir přihlašovací stránku podle pokynů hello najít v článku hello [přidat přihlašovací stránku popis](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Pomocí příkazu hello, který odpovídá na serveru AD FS přidá odkaz toohello služby AD FS přihlašovací stránky umožňuje uživatelům tooenter hello hesla pomocí samoobslužné služby pracovního postupu resetování přímo.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-hello-sign-in-and-access-panel-look-and-feel"></a>Přizpůsobení hello přihlášení a přístup k panelu Vzhled a chování

Když uživatelé přistoupí k hello přihlašovací stránky, můžete přizpůsobit logo hello, který se zobrazí spolu s toofit image přihlašovací stránku hello značky společnosti.

Tato grafika jsou zobrazeny v hello následující okolnosti:

* Poté, co uživatel zadá své uživatelské jméno
* Uživatel získá přístup k vlastní adresu url
    * Pomocí předání hello resetování hesla toohello parametr "Wh" stránky, jako je "https://login.microsoftonline.com/?whr=contoso.com"
    * Pomocí předání hello "username" parametr toohello resetování hesla stránky, jako je třeba "https://login.microsoftonline.com/?username=admin@contoso.com"

### <a name="graphics-details"></a>Podrobnosti o grafiky

Hello následující nastavení umožňují toochange hello vizuální vlastnosti přihlašovací stránku hello a najdete v části **Azure Active Directory**, **firemní branding**, **upravit společnosti Branding**

* Obrázek přihlašovací stránky by měl být PNG nebo JPG souboru 1420 × 1200 pixelů a ne větší než 500KB. Doporučujeme, abyste ho toobe přibližně 200 KB pro dosažení co nejlepších výsledků.
* Barva pozadí přihlašovací stránky se používá v s vysokou latencí připojení a musí být v šestnáctkovém formátu RGB hello.
* Obrázek hlavičky by měl být PNG nebo JPG souboru 60 x 280 pixelů a ne větší než 10 KB.
* Odmocnina logo (normální a tmavým motivem) PNG nebo JPG 240 x 240 (s možností změny velikosti) nesmí být větší než 10 KB.

### <a name="sign-in-text-options"></a>Možnosti přihlášení textu

Hello následující nastavení povolí tooadd text toohello přihlašovací stránce relevantní tooyour organizace. Tato nastavení najdete v části **Azure Active Directory**, **firemní branding**, **upravit firemního brandingu**

* **Pomocný parametr název uživatele** nahradí hello příklad text someone@example.com s něco vhodnější pro vaše uživatele, doporučujeme levém výchozí toobe při podpoře interních a externích uživatelů
* **Text přihlašovací stránky** je maximálně 256 znaků. Tento text se zobrazí kdekoli přihlašování uživatelů online a v hello Azure AD Join prostředí ve Windows 10. Použijte tento text pro podmínky použití, pokyny a tipy pro vaše uživatele. **Kdo, zobrazit stránku přihlášení, neposkytují žádné citlivé informace v tomto poli.**

### <a name="keep-me-signed-in-disabled"></a>Možnost zůstat přihlášení je vypnutá.

možnost Hello "zůstat přihlášeni zakázáno" umožňuje uživatelům tooremain při jejich zavřete a znovu otevřete okno jejich prohlížeče přihlášený. Tato možnost neměla vliv trvání relace. Toto nastavení je v části **Azure Active Directory > firemní branding > Upravit firemního brandingu**.

Některé funkce služby SharePoint Online a Office 2010 jsou závislé na uživatele, je možné toocheck toto políčko. Pokud je skrýt tuto možnost, mohou uživatelé získat další a neočekávané přihlášení výzvy.

### <a name="directory-name"></a>Název adresáře

Můžete změnit atribut name hello pod **Azure Active Directory > vlastnosti** tooshow organizace popisný název vidět v portálu hello a automatizovat komunikace. Tato možnost je nejvíce viditelné v podobě hello automatizovaných e-mailů ve formulářích hello, které následují

* Popisný název v e-mailu "Microsoft jménem ukázkové společnosti CONTOSO"
* Řádek předmětu e-mailem "CONTOSO ukázkový e-mailu ověřovací kód účtu"

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR

