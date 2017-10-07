---
title: "Zavedení: Samoobslužné resetování hesla Azure AD | Dokumentace Microsoftu"
description: "Tipy pro úspěšné zavedení samoobslužného resetování hesla Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 73d31679b38ff009a767335adaebc49fbc5a75b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="roll-out-password-reset-for-users"></a>Zavedení resetování hesla pro uživatele

Většina zákazníků postupujte podle kroků hello, které následují tooensure smooth zavedení SSPR funkcí.

1. [Povolte resetování hesla ve svém adresáři](active-directory-passwords-getting-started.md).
2. [Nakonfigurujte oprávnění místní služby AD pro zpětný zápis hesla](active-directory-passwords-how-it-works.md#active-directory-permissions).
3. [Nakonfigurovat zpětný zápis hesla](active-directory-passwords-writeback.md#configuring-password-writeback) toowrite hesla z Azure AD zpátky tooyour místní adresář
4. [Přiřaďte a ověřte požadované licence](active-directory-passwords-licensing.md).
5. Pokud chcete, aby tooroll se postupně, které můžete volitelně limit resetování hesla tooa skupiny uživatelů tooroll hello funkce rezervace pomalu v čase. toodo to nastavit hello **samoobslužné služby heslo resetovat povoleno** přepnutí z **každý uživatel** příliš**skupinu** a vyberte tooenable skupiny zabezpečení pro resetování hesla. Hello členy této skupiny musí mít přiřazené licence toothem a je skvělým způsobem tooenable [skupinu na základě licencování](active-directory-passwords-licensing.md#enable-group-or-user-based-licensing).
6. Naplnění hello minimální sadu [Data ověřování](active-directory-passwords-data.md), podle vaše zásady.
7. Jak naučit vaše uživatele toouse SSPR, odesláním pokyny tooshow je jak tooregister a jak tooreset.
    > [!NOTE]
    > Otestujte samoobslužné resetování hesla pomocí uživatele, a ne správce, protože Microsoft pro účty typu správce Azure vynucuje požadavky na silné ověřování. Další informace týkající se zásad hesla správce hello najdete v tématu naše [podrobné informace článku](active-directory-passwords-how-it-works.md).

8. Můžete zvolit tooenforce registrace v libovolném bodě a jejich ověřovací informace po určité době vyžadují tooreconfirm uživatele. Pokud nechcete, aby vaši uživatelé toohave tooregister, můžete [nasadit bez nutnosti registrace koncového uživatele pro vytvoření nového hesla](active-directory-passwords-data.md).
9. V čase, přečtěte si uživatelé registrace a pomocí zobrazení hello [reporting poskytuje Azure AD](active-directory-passwords-reporting.md).

## <a name="email-based-rollout"></a>Zavedení přes e-mail

Mnoho zákazníků najít že e-mailové kampaně, jednoduché toouse pokyny, je hello nejjednodušší způsob, jak tooget uživatelé toouse SSPR. [Vytvořili jsme tři jednoduché e-maily, které můžete použít jako šablony toohelp v rámci procesu zavádění řešení.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* **Už brzo** e-mailové šablony toobe použít hello týdnů, nebo počet dnů před zavedení toolet uživatelé věděli, potřebují toodo něco.
* **Nyní k dispozici** e-mailové šablony toobe používá hello den spuštění toodrive uživatelé tooregister a potvrdit jejich data ověřování, takže je možné použít SSPR, když ji potřebují.
* **Zaregistrujte si připomenutí** e-mailové šablony pro několik dní tooweeks po nasazení tooremind uživatelé tooregister a potvrdit jejich data ověřování.

## <a name="creating-your-own-password-portal"></a>Vytvoření vlastního portálu hesel

Řadu zákazníkům větší zvolte toohost webovou stránku a vytvořit kořenový záznam DNS, jako je https://passwords.contoso.com. Jejich naplnit tuto stránku resetování hesla odkazy toohello Azure AD, registrace, portálů změnu hesla a další informace specifické pro resetování hesla. V e-mailovou komunikaci nebo letáků, můžete poslat, potom můžete zahrnout partnerské, snadno zapamatovatelný, adresa URL, mohou uživatelé toowhen potřebují toouse hello služby.

* Portál pro resetování hesla – https://passwordreset.microsoftonline.com/
* Portál pro registraci k resetování hesla – http://aka.ms/ssprsetup
* Portál pro změnu hesla – https://account.activedirectory.windowsazure.com/ChangePassword.aspx

## <a name="using-enforced-registration"></a>Použití vynucené registrace

Pokud chcete vaši uživatelé tooregister pro resetování hesla, můžete vynutit je tooregister při přihlášení pomocí služby Azure AD. Tuto možnost můžete povolit ze svého adresáře na **resetování hesla** okno povolením hello **tooRegister vyžadují uživatele při přihlášení** možnost na hello **registrace** Karta.

Správce může vyžadovat toore registrace uživatele po určitou dobu podle nastavení hello **počet dní, než budou uživatelé vyzváni tooreconfirm své informace o ověřování** mezi 0 730 dnů.

Když povolíte tuto možnost, uživatelům podepisování se zobrazí zpráva s informací, jejich správce vyžaduje je tooverify své informace o ověřování.

## <a name="populate-authentication-data"></a>Naplnění ověřovacích dat

Pokud jste [naplnit data ověřování pro vaše uživatele](active-directory-passwords-data.md), a uživatelé nepotřebují tooregister teprve pak ji bude možné toouse SSPR resetování hesla. Tak dlouho, dokud uživatelé mají hello ověřování dat definovaných splňující zásady resetování hesel hello jste definovali, jsou uživatelé moct tooreset jejich hesla.

## <a name="disabling-self-service-password-reset"></a>Zakázání samoobslužného resetování hesla

Zakázání samoobslužné resetování hesla je jednoduché, otevřete klientovi Azure AD a budete příliš**resetování hesla**, **vlastnosti**a výběr **nikdo** pod  **Samoobslužné resetování hesla povoleno**

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR
