---
title: "životnosti tokenu aaaConfigurable v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooset životnosti pro tokeny vydané službou Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 06f5b317-053e-44c3-aaaa-cf07d8692735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: billmath
ms.custom: aaddev
ms.reviewer: anchitn
ms.openlocfilehash: 0d4c8545981c5463cc7c95f669167bbc38230123
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Konfigurovat životnosti tokenu v Azure Active Directory (Public Preview)
Můžete zadat hello životnost tokenem vydaným službou Azure Active Directory (Azure AD). Můžete nastavit životnosti tokenu pro všechny aplikace ve vaší organizaci, pro aplikaci víceklientské (více organizace) nebo pro objekt určité služby ve vaší organizaci.

> [!NOTE]
> Tato funkce je aktuálně ve verzi Public Preview. Připravit toorevert nebo odeberte všechny změny. Hello funkce je dostupná v libovolné předplatné služby Azure Active Directory během verzi Public Preview. Ale když funkce hello je obecně dostupná, některé aspekty funkcí hello může vyžadovat [Azure Active Directory Premium](active-directory-get-started-premium.md) předplatné.
>
>

Objekt zásady ve službě Azure AD, představuje sadu pravidel, které vynucuje u jednotlivých aplikací, nebo na všechny aplikace v organizaci. Každý typ zásad se strukturou jedinečnou sadu vlastností, které jsou použité tooobjects toowhich, které jsou přiřazeni.

Zásady můžete určit jako hello výchozí zásady pro vaši organizaci. zásady Hello je použité tooany aplikace v organizaci hello, tak dlouho, dokud není přepsána zásady s vyšší prioritou. Také můžete přiřadit zásady toospecific aplikace. Hello pořadí podle priority se liší podle typu zásady.


## <a name="token-types"></a>Typy tokenů

Můžete nastavit dobu životnosti tokenu zásady pro tokeny obnovení, tokeny přístupu, tokeny relace a tokeny typu ID.

### <a name="access-tokens"></a>Přístupové tokeny
Použití přístupu klientů tokeny tooaccess k chráněnému prostředku. Přístupový token lze použít pouze pro konkrétní kombinaci uživatele, klienta a prostředků. Přístupové tokeny nejde odvolat a jsou platné až do vypršení jejich platnosti. Škodlivý objektu actor, který má získat token přístupu můžete použít pro rozsah celé jeho životnosti. Nastavení doby platnosti hello přístupový token je kompromis mezi zlepšení výkonu systému a roste hello množství času, který hello klienta bude mít přístup po hello uživatelský účet je zakázán. Vylepšené systému výkonu se dosáhne snížením hello počet klient potřebuje tooacquire nový přístupový token.

### <a name="refresh-tokens"></a>Obnovovacích tokenů
Když klient získá token tooaccess k přístupu k chráněnému prostředku, obdrží klient hello obnovovací token a přístupový token. token obnovení Hello je použité tooobtain nové přístupu nebo aktualizace tokenu páry když vyprší platnost hello aktuální přístupový token. Token obnovení je kombinací vázané tooa uživatele a klienta. Obnovovací token se dají odvolávat a kontroluje platnost tokenu hello pokaždé, když se používá hello token.

Je důležité toomake rozdíl mezi důvěrné klienty a veřejné. Další informace o různých typech klientů najdete v tématu [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Token životnosti s tokeny obnovení důvěrné klienta
Důvěrné klienti jsou aplikace, které můžete bezpečně uložit heslo klienta (tajný klíč). Mohou prokázat, že požadavky přicházejí od aplikace hello klienta a nikoli z škodlivý objektu actor. Například webová aplikace je důvěrné klienta, protože tajný klíč klienta může ukládat na webovém serveru hello. Nevystavené. Protože tyto toky jsou bezpečnější, hello výchozí životnosti tokenů aktualizace vydané toothese toky je `until-revoked`, nelze změnit pomocí zásad a nebude na resetování hesla dobrovolná odvolán.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Token životnosti s tokeny obnovení veřejné klienta

Veřejné klientů nelze bezpečně uložit heslo klienta (tajný klíč). Aplikace pro iOS nebo Android nelze například obfuskováním tajný klíč z hello vlastníka prostředku, tak bude považován za veřejné klienta. Můžete nastavit zásady na prostředcích tooprevent tokeny obnovení z veřejné klientů starší než získat nový pár tokenu přístupu nebo aktualizaci během zadaného období. (toodo hello tento, použijte vlastnost aktualizovat Token maximální neaktivní doba.) Můžete taky použít zásady tooset už jsou podmínky přijaty lhůtu, po které hello obnovovacích tokenů. (toodo hello tento, použijte vlastnost aktualizovat Token maximální stáří.) Můžete upravit hello životnost aktualizace tokenu toocontrol, kdy a jak často hello je uživatel přihlašovací údaje požadované tooreenter místo se bezobslužně k novému ověření, při použití veřejných klientské aplikace.

### <a name="id-tokens"></a>ID tokeny
ID tokeny jsou předávány toowebsites a nativních klientů. ID tokeny obsahovat profil informací o uživateli. ID token je vázané tooa konkrétní kombinaci uživatele a klienta. ID tokeny považovány za platný až do vypršení jejich platnosti. Obvykle aplikace webového odpovídá uživatele pro uživatele hello vydané dobu platnosti relace v průběhu životnosti toohello aplikace hello hello ID tokenu. Můžete upravit hello životnost ID tokenu toocontrol jak často webové aplikace hello vyprší platnost relace hello aplikace a jak často vyžaduje toobe uživatele hello k novému ověření službou Azure AD (bezobslužná nebo interaktivní).

### <a name="single-sign-on-session-tokens"></a>Tokeny jedné relace přihlášení
Když uživatel ověřuje s Azure AD a vybere hello **zůstat přihlášeni** políčko relaci přihlašování (SSO) se naváže hello uživatele prohlížeče a Azure AD. token Hello jednotné přihlašování, v hello formu souboru cookie, představuje tuto relaci. Poznámka: Tento token relace jednotného přihlašování k hello není vázané tooa konkrétní prostředků nebo klientská aplikace. Tokeny relace jednotného přihlašování se dají odvolávat a jejich platnost kontroluje pokaždé, když se používají.

Azure AD používá dva typy tokenů relace jednotného přihlašování: trvalé a zajišťováno. Trvalé relace tokeny jsou uloženy jako trvalé soubory cookie prohlížeče hello. Tokeny zajišťováno relace jsou uloženy jako soubory cookie relace. (Souborů cookie relací jsou při zavření prohlížeče hello zničen.)

Zajišťováno relace tokeny mají životnost 24 hodin. Trvalé tokeny mají životnost 180 dní. Kdykoli tokenu jednotného přihlašování k relace se používá v rozsahu období platnosti, doba platnosti hello je rozšířeno jiný 24 hodin nebo 180 dnů, v závislosti na typu tokenu hello. Pokud token relace jednotného přihlašování se nepoužívá v rozsahu období platnosti, bude považován za platnost a již byla přijata.

Můžete použít zásady tooset hello čas po hello první relace token vydán nad rámec které hello token relace už přijata. (toodo hello tento, použijte vlastnost relace tokenu maximální stáří.) Můžete upravit hello životnost relace tokenu toocontrol, kdy a jak často je uživatel přihlašovací údaje požadované tooreenter místo bezobslužně ověřovaného, pokud používáte webovou aplikaci.

### <a name="token-lifetime-policy-properties"></a>Vlastnosti zásad životnost tokenu
Životnost tokenu zásad je typ objektu zásad, který obsahuje pravidla životnost tokenu. Použití vlastnosti hello hello zásad toocontrol zadaný token životnosti. Pokud je nastavené žádné zásady, systém hello vynucuje hodnotu doby života výchozí hello.

### <a name="configurable-token-lifetime-properties"></a>Vlastnosti konfigurovat životnost tokenu
| Vlastnost | Řetězec vlastnosti zásad | Ovlivňuje | Výchozí | Minimální | Maximální počet |
| --- | --- | --- | --- | --- | --- |
| Životnost tokenu přístupu |AccessTokenLifetime |Přístupové tokeny, tokeny typu ID, typu SAML2 tokeny |1 hodina |10 minut |1 den |
| Aktualizace tokenu maximální doba neaktivní |MaxInactiveTime |Obnovovacích tokenů |14 dnů |10 minut |90 dnů |
| Single-Factor aktualizace tokenu maximální stáří |MaxAgeSingleFactor |Obnovovacích tokenů (pro všechny uživatele) |Dokud odvolat |10 minut |Dokud odvolat<sup>1</sup> |
| Maximální stáří tokenu Multi-Factor aktualizace |MaxAgeMultiFactor |Obnovovacích tokenů (pro všechny uživatele) |Dokud odvolat |10 minut |Dokud odvolat<sup>1</sup> |
| Maximální stáří tokenu Single-Factor relace |MaxAgeSessionSingleFactor<sup>2</sup> |Tokeny relace (trvalá nebo zajišťováno) |Dokud odvolat |10 minut |Dokud odvolat<sup>1</sup> |
| Maximální stáří tokenu Multi-Factor relace |MaxAgeSessionMultiFactor<sup>3</sup> |Tokeny relace (trvalá nebo zajišťováno) |Dokud odvolat |10 minut |Dokud odvolat<sup>1</sup> |

* <sup>1</sup>365 dnů je hello maximální explicitní délku, která se dá nastavit pro tyto atributy.
* <sup>2</sup>Pokud **MaxAgeSessionSingleFactor** není nastaven, tato hodnota trvá hello **MaxAgeSingleFactor** hodnotu. Pokud ani parametr je nastaven, vlastnost hello trvá hello výchozí hodnota (dokud odvolat).
* <sup>3</sup>Pokud **MaxAgeSessionMultiFactor** není nastaven, tato hodnota trvá hello **MaxAgeMultiFactor** hodnotu. Pokud ani parametr je nastaven, vlastnost hello trvá hello výchozí hodnota (dokud odvolat).

### <a name="exceptions"></a>Výjimky
| Vlastnost | Ovlivňuje | Výchozí |
| --- | --- | --- |
| Aktualizace tokenu maximální stáří (vydán pro federované uživatele, kteří mají dostatečná odvolání informace<sup>1</sup>) |Obnovovacích tokenů (vydán pro federované uživatele, kteří mají dostatečná odvolání informace<sup>1</sup>) |12 hodin |
| Aktualizace tokenu neaktivní doba Max (vydán pro důvěrné klientů) |Obnovovacích tokenů (vydán pro důvěrné klientů) |90 dnů |
| Aktualizace tokenu maximální stáří (vydán pro důvěrné klientů) |Obnovovacích tokenů (vydán pro důvěrné klientů) |Dokud odvolat |

* <sup>1</sup>federovaný uživatelé, kteří mají dostatečná odvolání informace zahrnují všechny uživatele, kteří nemají hello "LastPasswordChangeTimestamp" atribut synchronizované. Tito uživatelé mají tato krátké maximální stáří AAD je proto nelze tooverify Pokud toorevoke tokeny, které jsou svázané tooan starý přihlašovací údaje (například hesla, která byla změněna) a musí se změnami zpět častěji tooensure tento uživatel hello a přidružené tokeny jsou stále v dobré stojící. tooimprove toto prostředí klienta, které admins musíte zajistit, že se synchronizuje hello "LastPasswordChangeTimestamp" atribut (to je možné nastavit u objektu uživatele hello pomocí prostředí Powershell nebo prostřednictvím nebude služba AADSync).

### <a name="policy-evaluation-and-prioritization"></a>Vyhodnocení zásad a stanovení priorit
Můžete vytvořit a potom přiřadit konkrétní aplikaci životnost tokenu zásad tooa, tooyour organizace a tooservice objekty. Víc zásad může použít tooa konkrétní aplikaci. Hello životnost tokenu zásady, které se projeví řídí následujícími pravidly:

* Pokud zásadu explicitně přiřazený toohello instančního objektu, se vynutí.
* Pokud žádné zásady jsou explicitně přiřazené toohello instanční objekt, je vyžadována zásadu explicitně přiřazeny toohello nadřazené organizace hello instanční objekt.
* Pokud žádné zásady explicitně přiřazeny toohello instanční objekt nebo toohello organizace, je vyžadována hello zásady přiřazené toohello aplikace.
* Pokud žádná zásada byla přiřazena toohello služby objekt zabezpečení, hello organizaci nebo objekt aplikace hello, hello výchozí hodnoty je vyžadována. (Viz tabulka hello v [konfigurovat životnost tokenu vlastnosti](#configurable-token-lifetime-properties).)

Další informace o hello vztah mezi objekty aplikací a hlavní objekty služby najdete v tématu [aplikace a služby hlavní objekty ve službě Azure Active Directory](active-directory-application-objects.md).

Token platnosti vyhodnotí ve hello dobu, kdy se používá hello token. Hello zásada s nejvyšší prioritou hello na hello aplikace, která je přistupuje projeví.

> [!NOTE]
> Tady je příklad scénáře.
>
> Uživatel chce tooaccess dva webové aplikace: webové aplikace A a B. webové aplikace
> 
> Faktory:
> * Obě webové aplikace jsou v hello stejné nadřazené organizace.
> * Token 1 zásad životního cyklu relace tokenu maximální stáří osm hodin je nastaven jako výchozí hello nadřazené organizace.
> * Webové aplikace A je použití regular webovou aplikaci a není propojené tooany zásady.
> * Webovou aplikaci B se používá pro vysoce citlivých procesů. Jeho instančního objektu je propojené tooToken 2 zásad životního cyklu, který má relace tokenu maximální stáří 30 minut.
>
> Na 12:00 PM hello uživatel spustí novou relaci prohlížeče a pokusů tooaccess A. webové aplikace hello uživatel přesměrovaného tooAzure AD a se zobrazí výzva, toosign v. Tím se vytvoří soubor cookie, který obsahuje token relace v prohlížeči hello. uživatel Hello je back tooWeb přesměrovaného aplikace A k tokenu ID, který hello uživatele tooaccess hello aplikacím.
>
> Ve 12:15 hello uživatel pokusí tooaccess B. webové aplikace hello prohlížeče přesměrování tooAzure AD, který zjistí hello souboru cookie relace. Objekt B aplikace webové služby je propojené tooToken 2 zásad životního cyklu, ale je také součástí hello nadřazené organizace, výchozí Token 1 zásad životního cyklu. Token 2 zásad životního cyklu využívá vliv, protože zásady tooservice propojené objekty mají vyšší prioritu než výchozí zásady organizace. Hello relace token původně vydán v rámci hello posledních 30 minut, takže bude považován za platný. uživatel Hello je back tooWeb přesměrovaného aplikaci B k tokenu ID, která jim udělí přístup.
>
> : 00: 00 je hello pokusů tooaccess A. webové aplikace hello uživatel přesměrovaného tooAzure AD. Webové aplikace A není propojené tooany zásady, ale protože je v organizaci s výchozí Token 1 zásad životního cyklu, tyto zásady projeví. je zjištěna Hello souboru cookie relace, který byl původně vystavil v rámci hello posledních 8 hodin. uživatel Hello je back tooWeb bezobslužně přesměrovaného aplikace A s ID nový token. uživatel Hello není požadovaná tooauthenticate.
>
> Hned potom hello uživatel pokusí tooaccess B. webové aplikace hello uživatel je přesměrovaného tooAzure AD. Jako dříve, tokenu 2 životnost zásad se projeví. Vzhledem k tomu hello token vydán víc než 30 minutami, hello uživatele je výzvami tooreenter jejich přihlašovací údaje. Jsou vydávány ID token a relace zcela nový token. uživatel Hello přístup B. webové aplikace
>
>

## <a name="configurable-policy-property-details"></a>Podrobnosti vlastnost konfigurovat zásady
### <a name="access-token-lifetime"></a>Životnost tokenu přístupu
**Řetězec:** AccessTokenLifetime

**Ovlivňuje:** přístupové tokeny, tokeny typu ID

**Souhrn:** tato zásada určuje, jak dlouho přístup a tokeny typu ID pro tento prostředek jsou považovány za platné. Snižuje dobu životnosti tokenu přístupu vlastnost hello snižuje riziko hello přístupového tokenu nebo ID token používá škodlivý objektu actor pro delší dobu. (Tyto tokeny nelze odvolat.) kompromis Hello je nepříznivě ovlivňuje výkon, protože hello tokeny toobe nahradit častěji.

### <a name="refresh-token-max-inactive-time"></a>Aktualizace tokenu maximální doba neaktivní
**Řetězec:** MaxInactiveTime

**Ovlivňuje:** obnovovacích tokenů

**Souhrn:** tato zásada určuje, kolik token obnovení může být, než klient již nelze používat ji tooretrieve nový pár tokenu přístupu nebo aktualizace při pokusu o tooaccess tento prostředek. Vzhledem k tomu, že nový token obnovení je obvykle vrácena, pokud se používá token obnovení, zabraňuje tato zásada přístupu, pokud klient hello pokusí tooaccess jakémukoli prostředku, pomocí tokenu obnovení aktuální hello během hello zadané časové období.

Tato zásada vynutí uživatelů, kteří nebyly aktivní na jejich klienta tooreauthenticate tooretrieve nový token obnovení.

Hello vlastnost aktualizovat Token maximální neaktivní doba musí být nastavena tooa nižší hodnotu než hello Single-Factor tokenu maximální stáří a vlastnosti Multi-Factor aktualizovat Token maximální stáří hello.

### <a name="single-factor-refresh-token-max-age"></a>Single-Factor aktualizace tokenu maximální stáří
**Řetězec:** MaxAgeSingleFactor

**Ovlivňuje:** obnovovacích tokenů

**Souhrn:** tento ovládací prvky zásad jak dlouho uživatele můžete použít obnovení tokenu tooget a nové přístupu nebo aktualizace tokenu pár po jejich poslední úspěšně ověření pomocí pouze jeden faktor. Poté, co uživatel ověří a obdrží token nové aktualizace, hello uživatel může použít tok tokenu hello aktualizace pro hello zadané období času. (To je hodnota true, dokud nebude odvolaný hello aktuální obnovovací token a není zbývající nepoužité po dobu delší než hello neaktivní doba.) V tomto okamžiku je uživatel hello vynucené tooreauthenticate tooreceive nový token obnovení.

Uživatelé tooauthenticate snížit maximální stáří hello vynutí častěji. Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost tooa hodnotu menší než hello Multi-Factor aktualizovat Token maximální stáří vlastnost tedy rovna tooor.

### <a name="multi-factor-refresh-token-max-age"></a>Maximální stáří tokenu Multi-Factor aktualizace
**Řetězec:** MaxAgeMultiFactor

**Ovlivňuje:** obnovovacích tokenů

**Souhrn:** tento ovládací prvky zásad jak dlouho uživatele můžete použít obnovení tokenu tooget a nové přístupu nebo aktualizace tokenu pár po jejich poslední úspěšně ověření pomocí několika faktory. Poté, co uživatel ověří a obdrží token nové aktualizace, hello uživatel může použít tok tokenu hello aktualizace pro hello zadané období času. (To je hodnota true, dokud nebude odvolaný hello aktuální obnovovací token a není po dobu delší než hello neaktivní doba.) V tomto okamžiku jsou uživatelé přinucení tooreauthenticate tooreceive nový token obnovení.

Uživatelé tooauthenticate snížit maximální stáří hello vynutí častěji. Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost tooa hodnotu, která je rovna tooor větší než hello Single-Factor aktualizovat Token maximální stáří vlastnosti.

### <a name="single-factor-session-token-max-age"></a>Maximální stáří tokenu Single-Factor relace
**Řetězec:** MaxAgeSessionSingleFactor

**Ovlivňuje:** tokeny relace (trvalá nebo zajišťováno)

**Souhrn:** této zásady ovládací prvky jak dlouho může uživatel používat relace tokenu tooget nové ID a relace token po jejich poslední úspěšně ověření pomocí pouze jeden faktor. Poté, co uživatel ověří a obdrží token novou relaci, hello uživatel může použít tok tokenu hello relace pro hello zadané období času. (To platí, dokud, hello aktuální relace token není odvolaný a jestli nevypršela platnost.) Po hello zadané časové období, uživatel hello je tooreceive vynucené tooreauthenticate token novou relaci.

Uživatelé tooauthenticate snížit maximální stáří hello vynutí častěji. Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost tooa hodnotu, která je rovna tooor menší než hello Multi-Factor relace tokenu maximální stáří vlastnost.

### <a name="multi-factor-session-token-max-age"></a>Maximální stáří tokenu Multi-Factor relace
**Řetězec:** MaxAgeSessionMultiFactor

**Ovlivňuje:** tokeny relace (trvalá nebo zajišťováno)

**Souhrn:** této zásady ovládací prvky jak dlouho může uživatel používat relace tokenu tooget nové ID a relace token po hello naposledy musí úspěšně ověřit pomocí několika faktory. Poté, co uživatel ověří a obdrží token novou relaci, hello uživatel může použít tok tokenu hello relace pro hello zadané období času. (To platí, dokud, hello aktuální relace token není odvolaný a jestli nevypršela platnost.) Po hello zadané časové období, uživatel hello je tooreceive vynucené tooreauthenticate token novou relaci.

Uživatelé tooauthenticate snížit maximální stáří hello vynutí častěji. Protože jedním Multi-Factor authentication je považován za méně bezpečné než vícefaktorového ověřování, doporučujeme nastavit tuto vlastnost tooa hodnotu, která je rovna tooor větší než hello Single-Factor relace tokenu maximální stáří vlastnosti.

## <a name="example-token-lifetime-policies"></a>Příkladem zásad životnost tokenu
Mnoho scénářů je možné ve službě Azure AD při vytváření a správa životnosti tokenu pro aplikace, objekty služby a celkové organizaci. V této části jsme provede několik běžných scénářů zásady, které můžete použít nové pravidel pro:

* Životnost tokenu
* Token maximální doba neaktivní
* Maximální stáří tokenu

V příkladech hello dozvíte, jak:

* Správa organizace výchozí zásady
* Vytvoření zásady pro přihlášení k webové
* Vytvořit zásadu pro nativní aplikaci, která volá webové rozhraní API
* Spravovat pokročilé zásady

### <a name="prerequisites"></a>Požadavky
V hello následující příklady vytvářet, aktualizovat, odkaz a odstranit zásady pro aplikace, objekty služby a celkové organizaci. Pokud jste nový tooAzure AD, doporučujeme Další informace o [jak tooget na Azure AD klienta](active-directory-howto-tenant.md) předtím, než budete pokračovat v těchto příkladech.  

tooget spuštění hello následující kroky:

1. Stáhněte si nejnovější hello [Azure AD PowerShell modulu verze Public Preview](https://www.powershellgallery.com/packages/AzureADPreview).
2. Spustit hello `Connect` příkaz toosign v tooyour účet správce Azure AD. Spusťte tento příkaz pokaždé, když zahájit novou relaci.

    ```PowerShell
    Connect-AzureAD -Confirm
    ```

3. toosee všechny zásady, které byly vytvořeny ve vaší organizaci, spusťte hello následující příkaz. Tento příkaz spusťte po většinu operací v hello následující scénáře. Spuštění příkazu hello také pomáhá získat hello ** ** zásad.

    ```PowerShell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a>Příklad: Správa organizace výchozí zásady
V tomto příkladu vytvoříte zásadu, která umožňuje uživatelům přihlásit se méně často v rámci celé organizace. toodo, vytvořte zásadu životnost tokenu pro Single-Factor aktualizovat tokeny, které se použije v organizaci. zásady Hello je použité tooevery aplikace ve vaší organizaci a tooeach instanční objekt, který ještě nemá zásady nastavené.

1. Vytvořte zásadu životnost tokenu.

    1.  Nastavit hello Single-Factor aktualizovat Token příliš "dokud odvolaný." Hello token nevyprší, dokud je odvolat přístup. Vytvořte hello následující definice zásady:

        ```PowerShell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    2.  toocreate hello zásady, spusťte následující příkaz hello:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    3.  toosee nové zásady a zásady hello tooget **ObjectId**spusťte hello následující příkaz:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Aktualizujte zásady hello.

    Můžete rozhodnout, že hello první zásad nastavených v tomto příkladu není striktní, protože vaše služba vyžaduje. spuštění vaší Single-Factor aktualizovat Token tooexpire dvou dní tooset hello následující příkaz:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId FROM GET COMMAND> -DisplayName "OrganizationDefaultPolicyUpdatedScenario" -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a>Příklad: Vytvoření zásady pro přihlášení k webové

V tomto příkladu vytvoříte zásadu, která vyžaduje tooauthenticate uživatelé častěji ve vaší webové aplikaci. Tato zásada nastaví životnost hello hello tokenů přístupu nebo ID a hello maximální stáří objekt relace Multi-Factor tokenu toohello služby vaší webové aplikace.

1. Vytvořte zásadu životnost tokenu.

    Tato zásada pro webové přihlášení, nastaví životnost tokenu přístupu nebo ID hello a hello maximální relace single-factor tokenu stáří tootwo hodin.

    1.  toocreate hello zásady, spusťte tento příkaz:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  toosee nové zásady a zásady hello tooget **ObjectId**spusťte hello následující příkaz:

        ```PowerShell
        Get-AzureADPolicy
        ```

2.  Přiřaďte hello zásad tooyour instanční objekt. Musíte taky tooget hello **ObjectId** z instanční objekt. 

    1.  toosee objekty všechna firemní služby, můžete dotazovat [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Nebo v [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), přihlaste se tooyour účet Azure AD.

    2.  Pokud máte hello **ObjectId** objektu služby spustit následující příkaz hello:

        ```PowerShell
        Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
        ```


### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Příklad: Vytvoření zásady pro nativní aplikaci, která volá webové rozhraní API
V tomto příkladu vytvoříte zásadu, která vyžaduje uživatelé tooauthenticate méně často. zásady Hello také prodlouží hello množství času, který uživatel může být neaktivní, než uživatele hello musí novému ověření. zásady Hello je použité toohello webového rozhraní API. Když nativní aplikaci hello vyžádá hello webového rozhraní API jako prostředek, je tato zásada použitá.

1. Vytvořte zásadu životnost tokenu.

    1.  toocreate striktní zásady pro webové rozhraní API, spusťte následující příkaz hello:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  toosee nové zásady a zásady hello tooget **ObjectId**spusťte hello následující příkaz:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Přiřaďte hello zásad tooyour webového rozhraní API. Musíte taky tooget hello **ObjectId** vaší aplikace. Hello toofind nejlepší způsob, jak vaše aplikace **ObjectId** je toouse hello [portál Azure](https://portal.azure.com/).

   Pokud máte hello **ObjectId** vaší aplikace, spusťte následující příkaz hello:

        ```PowerShell
        Add-AzureADApplicationPolicy -Id <ObjectId of hello Application> -RefObjectId <ObjectId of hello Policy>
        ```


### <a name="example-manage-an-advanced-policy"></a>Příklad: Správa pokročilé zásady
V tomto příkladu vytvoříte několik zásad, toolearn fungování hello priority systému. Také dozvíte, jak toomanage víc zásad, které jsou použité tooseveral objekty.

1. Vytvořte zásadu životnost tokenu.

    1.  toocreate výchozí zásadu organizace, která nastaví dny hello Single-Factor aktualizovat Token too30 doba platnosti, spusťte následující příkaz hello:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2.  toosee nové zásady a zásady hello tooget **ObjectId**spusťte hello následující příkaz:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Přiřaďte hello zásad tooa instanční objekt.

    Teď máte zásadu, která se použije toohello celé organizace. Možná chcete tuto zásadu 30denní toopreserve pro objekt konkrétní služby, ale změnit hello organizace výchozí zásady toohello horní limit počtu "dokud odvolaný."

    1.  toosee objekty všechna firemní služby, můžete dotazovat [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Nebo v [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), přihlaste se pomocí účtu Azure AD.

    2.  Pokud máte hello **ObjectId** objektu služby spustit následující příkaz hello:

            ```PowerShell
            Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
            ```
        
3. Sada hello `IsOrganizationDefault` příznak toofalse:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. Vytvořte novou zásadu výchozí organizace:

    ```PowerShell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    Nyní máte hello původní zásady propojené tooyour instanční objekt a nové zásady hello je nastaven jako výchozí zásady vaší organizace. Je důležité tooremember, že zásady tooservice objekty mají přednost před výchozí zásady organizace.

## <a name="cmdlet-reference"></a>Reference k rutinám

### <a name="manage-policies"></a>Správa zásad

Můžete použít následující rutiny toomanage zásady hello.

#### <a name="new-azureadpolicy"></a>Nové AzureADPolicy

Vytvoří novou zásadu.

```PowerShell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Definition</code> |Pole stringified formátu JSON, který obsahuje všechny zásady hello pravidla. | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |Řetězec název zásady hello. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |V případě hodnoty true, nastaví jako výchozí zásady organizace hello hello zásad. Pokud je hodnota false, neprovede žádnou akci. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |Typ zásad. Pro token životnosti vždy používejte "TokenLifetimePolicy." | `-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[Nepovinné] |Nastaví alternativní ID pro zásadu hello. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy
Získá všechny zásady služby Azure AD nebo zadanou zásadu.

```PowerShell
Get-AzureADPolicy
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code>[Nepovinné] |**ObjectId (Id)** hello zásady, které chcete. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject
Získá všechny aplikace a objekty služby, které jsou propojené tooa zásad.

```PowerShell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** hello zásady, které chcete. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a>Set-AzureADPolicy
Aktualizuje existující zásady.

```PowerShell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** hello zásady, které chcete. |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |Řetězec název zásady hello. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;Definition</code>[Nepovinné] |Pole stringified formátu JSON, který obsahuje všechny zásady hello pravidla. |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;IsOrganizationDefault</code>[Nepovinné] |V případě hodnoty true, nastaví jako výchozí zásady organizace hello hello zásad. Pokud je hodnota false, neprovede žádnou akci. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code>[Nepovinné] |Typ zásad. Pro token životnosti vždy používejte "TokenLifetimePolicy." |`-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[Nepovinné] |Nastaví alternativní ID pro zásadu hello. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a>Odebrat AzureADPolicy
Odstranění hello zadat zásady.

```PowerShell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** hello zásady, které chcete. | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a>Zásady aplikací
Můžete použít následující rutiny pro zásady aplikací hello.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Přidat AzureADApplicationPolicy
Odkazy hello zadat zásady tooan aplikace.

```PowerShell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** aplikace hello. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectId** hello zásad. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy
Získá hello zásady, která je přiřazena tooan aplikace.

```PowerShell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** aplikace hello. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Odebrat AzureADApplicationPolicy
Odebere zásadu z aplikace.

```PowerShell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** aplikace hello. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectId** hello zásad. | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a>Hlavní zásady služby
Můžete použít následující rutiny pro hlavní zásady služby hello.

#### <a name="add-azureadserviceprincipalpolicy"></a>Přidat AzureADServicePrincipalPolicy
Odkazy hello určené zásadě tooa instanční objekt.

```PowerShell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** aplikace hello. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectId** hello zásad. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy
Získá všechny zásady propojené toohello zadaný instanční objekt.

```PowerShell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** aplikace hello. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Odebrat AzureADServicePrincipalPolicy
Odebere hello zásad hello zadaný instanční objekt.

```PowerShell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| Parametry | Popis | Příklad |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** aplikace hello. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectId** hello zásad. | `-PolicyId <ObjectId of Policy>` |
