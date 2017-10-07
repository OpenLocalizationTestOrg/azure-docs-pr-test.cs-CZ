---
title: 'Azure AD: Registraci SSPR | Microsoft Docs'
description: "Zaregistrovat data ověření pro hesla pomocí samoobslužné služby Azure AD resetovat"
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
ms.date: 08/07/2017
ms.author: joflore
ms.custom: end-user
ms.openlocfilehash: dfcd0106616218c84d23920b124bed5b202cdd6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="register-for-self-service-password-reset"></a>Registrace pro samoobslužné resetování hesla

> [!IMPORTANT]
> **Jste tady, protože máte potíže s přihlášením?** Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md).

Jako koncový uživatel můžete resetovat heslo nebo odemknout účet bez nutnosti osoba tooa toospeak pomocí samoobslužné resetování hesla (SSPR). Než budete moct použít tuto funkci, máte metody ověřování tooregister nebo potvrďte hello předdefinované metody ověřování, které správce se zobrazí v.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Registrace nebo potvrzení ověřovacích dat pro samoobslužné resetování hesla

1. Hello otevřete webový prohlížeč na vaše zařízení a přejděte toohello [registrační stránku resetování hesla](http://aka.ms/ssprsetup)
2. Zadejte uživatelské jméno a heslo získané od správce.
3. V závislosti na tom, jak se pracovníci IT nakonfigurovali věcí, jednu nebo více z následujících hello možnosti jsou k dispozici pro jste tooconfigure a ověření. Správce může naplnění některé to pro vás, pokud mají vaše oprávnění toouse hello informace.
    * Telefon do kanceláře je pouze možnost toobe nastavuje správce
    * Telefon pro ověření musí být nastaven tooanother telefonní číslo byste měli přístup toolike mobilní telefon, který může přijímat text nebo volání.
    * Ověření e-mailu by mělo být nastavené tooan alternativní e-mailovou adresu, která se zobrazí bez hesla hello potřebujete tooreset.
    * Bezpečnostní otázky vám poskytne seznam dotazů, které pro tooanswer jste schválení správce. Nesmíte používat hello stejné otázku nebo odpovědět více než jednou.
4. Zadejte a ověřte informace hello požadované vaším správcem. Pokud je k dispozici více než jednu možnost, doporučujeme zaregistrovat flexibilitu tooprovide několik metod, když není k dispozici jiné metody (Příklad: cestách a nelze tooaccess váš telefon do kanceláře)

    ![Zaregistrujte si metody ověřování a klikněte na Dokončit][Register].

5. Po dokončení kroku 4 zvolte **Dokončit** a teď bude mít toouse samoobslužného resetování hesla Pokud budete potřebovat budoucí tooin hello.

Pokud zadáte data v hello telefon pro ověření nebo ověřování e-mailu, není zobrazená v hello globálního adresáře. Hello jenom lidé, kteří mohou vidět tato data jsou vám a vašim správcům. Pouze vidíte, že hello odpovědi tooyour bezpečnostní otázky.

Správci můžou vyžadovat tooconfirm můžete vaší metody ověřování po určité době čas toomake se, že máte příslušné metody hello zaregistrován.

## <a name="common-problems-and-their-solutions"></a>Běžné problémy a jejich řešení

 Zde jsou některé běžné případy chyba a jejich řešení:

| Chyb| Jaké chyby se zobrazí?| Řešení |
| --- | --- | --- |
| Zobrazí na stránce ", kontaktujte prosím správce" po zadání ID uživatele | Obraťte se na svého správce <br> <br> Zjistili jsme, že heslo vašeho uživatelského účtu nespravuje společnost Microsoft. V důsledku toho nám nedaří tooautomatically resetování hesla. <br> <br> Toocontact potřebujete další pomoc pracovníci IT. | Tato zpráva se zobrazuje, protože pracovníci oddělení IT spravuje heslo v místním prostředí a neumožňuje tooreset heslo z nemůže získat přístup k odkaz na vaši účtu. <br> <br> tooreset heslo, kontaktujte IT personál přímo pro nápovědu a dát jim vědět, je vhodné tooreset heslo, budou-li povolit tuto funkci můžete.|
| Zobrazí chybu "váš účet není povolen pro resetování hesla" po zadání ID uživatele | Váš účet není povolen pro resetování hesla <br> <br> Je nám líto, ale pracovníci IT nenastavil váš účet pro použití s touto službou. <br> <br> Pokud chcete, můžeme můžete kontaktovat správce ve vaší organizaci tooreset heslo pro vás. | Tato zpráva se zobrazuje, protože pracovníci IT nepovolil resetování hesla pro vaší organizace nemůže získat přístup k odkaz na vaši účtu, nebo není licencovaná toouse hello funkce. <br> <br> tooreset heslo, klikněte na tlačítko kontakt toosend odkaz na správce společnosti tooyour e-mailu je pracovníky IT a dát jim vědět, chcete tooreset heslo, budou-li povolit tuto funkci pro vás. |
| Zobrazí chybu "jsme nemohl ověřit váš účet" po zadání ID uživatele | Nepodařilo se ověřit váš účet <br> <br> Pokud chcete, můžeme můžete kontaktovat správce ve vaší organizaci tooreset heslo pro vás. | Tato zpráva se zobrazuje, protože jsou povolené pro resetování hesla, ale jste tak ještě neučinili toouse hello služby. tooregister pro heslo resetovat, přejděte toohttp://aka.ms/ssprsetup po jste znovu získali přístup tooyour účtu. <br> <br> tooreset heslo, klikněte na tlačítko kontakt na správce odkaz toosend společnosti tooyour e-mailu na IT oddělení. |

## <a name="next-steps"></a>Další kroky

* [Jak toochange hesla pomocí samoobslužné služby heslo resetovat](active-directory-passwords-update-your-own-password.md)
* [Registrační stránka pro resetování hesla](http://aka.ms/ssprsetup)
* [Portál pro resetování hesla](https://passwordreset.microsoftonline.com/)
* [Nemůžete se přihlásit tooyour účtu Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Registrační stránka pro resetování hesla se zobrazenými metodami a tlačítkem Dokončit"

