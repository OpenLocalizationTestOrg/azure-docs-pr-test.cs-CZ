---
title: "Rychlý start: Samoobslužné resetování hesla Azure AD | Dokumentace Microsoftu"
description: "Rychlé nasazené samoobslužného resetování hesla Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 4fed3a1c690fd6423ee5d3e5baef690d8896fbe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-azure-ad-self-service-password-reset"></a>Rychlý start: Samoobslužné resetování hesla Azure AD

> [!IMPORTANT]
> **Jste tady, protože máte potíže s přihlášením?** Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md).

## <a name="rapidly-deploy-self-service-password-reset"></a>Rychlé nasazené samoobslužného resetování hesla

Samoobslužného obnovení hesla (SSPR) nabízí jednoduchý znamená, že pro tooreset uživatelé tooempower správci IT nebo odemknutí jejich hesla nebo účty. Hello systému zahrnuje podrobné sestavy tootrack uživatelům při používání systému hello společně s tooalert oznámení můžete toomisuse nebo zneužití.

Tento průvodce předpokládá, že již máte funkčního tenanta Azure AD ve zkušební verzi nebo s licencí. Pokud potřebujete pomoc, nastavení služby Azure AD, najdete v článku hello [Začínáme s Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).

1. Ve svém existujícím tenantovi Azure AD vyberte **Resetování hesla**.

2. Z hello **"Vlastnosti"** obrazovky v části "Samoobslužné služby heslo resetovat povolené" vyberte jednu z následujících hello možnost hello
    * Nikdo - žádné jeden je možné toouse SSPR funkce
    * Skupina – konkrétní Azure AD pouze členové skupiny, abyste zvolili jsou možné toouse SSPR funkce
    * Každý uživatel - všichni uživatelé s účty v klientovi služby Azure AD jsou možné toouse SSPR funkce

3. Z hello **"Metody ověřování"** zvolte obrazovky
    * Počet metody požadováno tooreset – podporujeme alespoň jednu nebo dvě maximálně
    * Metody dostupné toousers - potřebujeme alespoň jeden ale ho nikdy škodí jak toohave se navíc řešením, které jsou k dispozici
        * **E-mailu** odešle e-mail s uživatelem toohello kód je nakonfigurované ověřování e-mailová adresa
        * **Mobilní telefon** poskytuje hello uživatele hello volba tooreceive volání nebo textu pomocí kódu tootheir nakonfigurovaná mobilní telefonní číslo
        * **Telefon do kanceláře** volání hello uživatele s tootheir kód nakonfigurované office telefonní číslo
        * **Bezpečnostní otázky** vyžaduje, abyste toochoose
            * Počet otázek vyžadovaných tooregister - hello minimální pro úspěšnou registraci, což znamená, uživatel může vybrat tooanswer další toocreate otázky, které toopull z fondu. Tuto možnost můžete nastavit od 3 až 5 a musí být větší než nebo roven hodnotě toohello počet tooreset požadované otázky.
                * Když kliknete na tlačítko "Vlastní" hello, při výběru bezpečnostní otázky lze přidat vlastní dotazy
            * Počet otázek vyžadovaných tooreset – můžete nastavit od 3 až 5 otázky toobe správně zodpovězena, před povolením heslo toobe uživatelů, resetování nebo odemknout.

4. Doporučené: **"Vlastní"** vám umožní toochange hello "Obraťte se na správce" odkaz toopoint tooa stránky nebo e-mailovou adresu definujete

5. Volitelné: hello **"Registrace"** obrazovky poskytuje správcům hello možnosti:
    * Vyžadovat tooregister uživatele při přihlášení
    * Počet dní, než budou uživatelé vyzváni tooreconfirm své informace o ověřování

6. Volitelné: hello **"Oznámení"** obrazovky poskytuje správcům možnost hello:
    * Upozornit uživatele na resetování hesla
    * Upozornit všechny správce na resetování hesla jiného správce

**Právě jste pro svého tenanta Azure AD nakonfigurovali samoobslužné resetování hesla**. Můžete zde zastavit nebo pokračovat v tooconfigure synchronizace tooan hesla místní domény služby AD.

> [!NOTE]
> Otestujte samoobslužné resetování hesla pomocí uživatele, a ne správce, protože Microsoft pro účty typu správce Azure vynucuje požadavky na silné ověřování. Další informace týkající se zásad hesla správce hello najdete v tématu naše [článek zásady hesla](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-tooexisting-identity-source"></a>Nastavit zdroj synchronizací tooexisting identity

tooenable místní tooAzure synchronizaci identit AD, je nutné tooinstall a nakonfigurovat [Azure AD Connect](./connect/active-directory-aadconnect.md) na serveru ve vaší organizaci. Tato aplikace zpracovává synchronizaci uživatelů a skupin ze stávající klientovi Azure AD zdroj tooyour identity.

* [Upgrade z nástroje DirSync nebo Azure AD Sync tooAzure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Začínáme se službou Azure AD Connect s použitím expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md)
* [Nakonfigurovat zpětný zápis hesla](active-directory-passwords-writeback.md#configuring-password-writeback) toowrite hesla z Azure AD zpátky tooyour místního adresáře.

## <a name="disabling-self-service-password-reset"></a>Zakázání samoobslužného resetování hesla

Zakázání samoobslužné resetování hesla je jednoduché, otevřete klientovi Azure AD a budete příliš**resetování hesla > vlastnosti** > vyberte **žádné** pod **samoobslužného resetování hesla Povoleno**

### <a name="learn-more"></a>Další informace
Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR

## <a name="next-steps"></a>Další kroky

V tento rychlý start když jste se naučili jak resetování hesla pomocí samoobslužné služby tooconfigure pro vaše uživatele. toocontinue toohello Azure portálu toocomplete, postupovat podle těchto kroků hello odkaz níže toohello portálu.

> [!div class="nextstepaction"]
> [Povolení samoobslužného resetování hesla](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

