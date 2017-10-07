---
title: "aaaAzure AD připojit víc domén"
description: "Tento dokument popisuje nastavení a konfiguraci víc domén nejvyšší úrovně s O365 a Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 91d87875ceacee4e34f132938e4bb0294fb954e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Podpora více domén pro federaci s Azure AD
Hello následující dokumentace obsahuje pokyny toouse více nejvyšší úrovně domén a subdomén při federaci s Office 365 nebo Azure AD domén.

## <a name="multiple-top-level-domain-support"></a>Podpora více domén nejvyšší úrovně
Federaci několika, domény nejvyšší úrovně s Azure AD vyžaduje určitou další konfiguraci, který není nutný, pokud federaci s jednu doménu nejvyšší úrovně.

Když domény federovaný pomocí služby Azure AD, několik vlastností, které jsou nastavené na hello domény v Azure.  Jeden z nich důležité je IssuerUri.  Toto je identifikátor URI, který se používá ve službě Azure AD tooidentify hello domény, který hello tokenu je přidružený.  Hello URI nepotřebuje tooresolve tooanything ale musí být platný identifikátor URI.  Ve výchozím nastavení, Azure AD Nastaví hodnota toohello identifikátor služby FS hello v místní služby AD FS konfigurace.

> [!NOTE]
> identifikátor služby FS Hello je identifikátor URI, který jednoznačně identifikuje federační služby.  Hello federační služby je instance služby AD FS, která funguje jako hello služby tokenů zabezpečení. 
> 
> 

Zobrazení IssuerUri můžete pomocí příkazu Powershellu hello `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Problém mohou nastat, pokud chceme tooadd více než jedné domény nejvyšší úrovně.  Předpokládejme například, že máte instalace federace mezi službou Azure AD a místní prostředí.  K tomuto dokumentu používám bmcontoso.com.  Teď přidali domény druhé, nejvyšší úrovně, bmfabrikam.com.

![Domény](./media/active-directory-multiple-domains/domains.png)

Když jsme pokus tooconvert naše toobe domény bmfabrikam.com federovaný, jsme k chybě.  Hello důvodem je, Azure AD má omezení, který neumožňuje hello IssuerUri vlastnost toohave hello stejnou hodnotu pro více než jedné domény.  

![Chyba Federation](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>Parametr SupportMultipleDomain
tooworkaround, potřebujeme tooadd různých IssuerUri, což lze provést pomocí hello `-SupportMultipleDomain` parametr.  Tento parametr se používá s hello následující rutiny:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Tento parametr umožňuje nakonfigurovat hello IssuerUri tak, aby je založena na hello název domény hello Azure AD.  Toto je jedinečný mezi adresářů ve službě Azure AD.  Pomocí parametru hello úspěšně umožňuje toocomplete příkaz prostředí PowerShell hello.

![Chyba Federation](./media/active-directory-multiple-domains/convert.png)

Prohlížení nastavení hello naší nové domény bmfabrikam.com můžete zjistit hello následující:

![Chyba Federation](./media/active-directory-multiple-domains/settings.png)

Všimněte si, že `-SupportMultipleDomain` nezmění hello ostatní koncové body, které jsou stále probíhá konfigurace toopoint tooour federační službu na adfs.bmcontoso.com.

Dalším krokem, `-SupportMultipleDomain` nemá je, že zajišťuje, že systém hello AD FS obsahuje správnou hodnotu vystavitele hello v tokeny vydané pro Azure AD. Dělá to tak, že část domény hello hello uživatele (UPN) Toto nastavení jako hello domény v hello IssuerUri, tj. přípona https://{upn} / adfs/services/vztah důvěryhodnosti. 

Proto při ověřování tooAzure AD nebo Office 365, hello IssuerUri element v token uživatele hello je použité toolocate hello domény ve službě Azure AD.  Pokud odpovídající nelze nalézt hello ověřování se nezdaří. 

Například pokud uživatele (UPN) je bsimon@bmcontoso.com, toohttp://bmcontoso.com/adfs/services/trust bude nastavena hello IssuerUri element v hello tokenu služby AD FS problémy. To bude odpovídat hello konfigurace Azure AD a ověřování bude úspěšný.

Následující Hello je pravidlo hello vlastní deklarace identity, který implementuje tuto logiku:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> V pořadí toouse hello - SupportMultipleDomain přepínače při pokusu o nové tooadd nebo převést již přidali domén, budete potřebovat toohave nastavení vaší toosupport federovaného vztahu důvěryhodnosti je původně.  
> 
> 

## <a name="how-tooupdate-hello-trust-between-ad-fs-and-azure-ad"></a>Jak tooupdate hello vztah důvěryhodnosti mezi AD FS a Azure
Pokud jste nenastavili hello federovaný vztah důvěryhodnosti mezi AD FS a vaší instanci Azure AD, pravděpodobně bude třeba toore – vytvoření tohoto vztahu důvěryhodnosti.  Důvodem je, že když je původně instalační program bez hello `-SupportMultipleDomain` parametr hello IssuerUri nastavena výchozí hodnota hello.  V hello – snímek obrazovky níže se zobrazují, jestli je nastavený hello IssuerUri toohttps://adfs.bmcontoso.com/adfs/services/trust.

Takže teď, když jsme úspěšně přidali novou doménu portálu hello Azure AD a pokusíte se tooconvert pomocí `Convert-MsolDomaintoFederated -DomainName <your domain>`, se nám získat hello následující chyba.

![Chyba Federation](./media/active-directory-multiple-domains/trust1.png)

Pokud se pokusíte tooadd hello `-SupportMultipleDomain` přepínač obdržíme hello následující chybě:

![Chyba Federation](./media/active-directory-multiple-domains/trust2.png)

Jednoduše pokusu o toorun `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` na hello původní domény dojde také chybě.

![Chyba Federation](./media/active-directory-multiple-domains/trust3.png)

Pomocí kroků hello níže tooadd další domény nejvyšší úrovně.  Pokud jste už přidali domény a nepoužili hello `-SupportMultipleDomain` parametr začněte s hello kroky pro odebírání a aktualizace vašeho původního domény.  Pokud jste nepřidali doména nejvyšší úrovně ještě můžete začít s hello kroky pro přidání domény pomocí prostředí PowerShell Azure AD Connect.

Použijte následující postup tooremove hello Microsoft Online důvěryhodnosti hello a aktualizace vašeho původního domény.

1. Na federačním serveru služby AD FS otevřete **Správa služby AD FS.** 
2. Na levé straně hello rozbalte **vztahy důvěryhodnosti** a **vztahy důvěryhodnosti předávající strany**
3. Na pravém hello, odstraňte hello **Microsoft Office 365 Identity Platform** položku.
   ![Odeberte Online Microsoft](./media/active-directory-multiple-domains/trust4.png)
4. Na počítači, který má [Azure Active Directory modul pro prostředí Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) nainstalován spustit následující hello: `$cred=Get-Credential`.  
5. Zadejte hello uživatelské jméno a heslo pro globálního správce pro doménu hello Azure AD, které jsou federaci s
6. V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`
7. V prostředí PowerShell zadejte `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Toto je pro doménu původního hello.  Výše domén, které má být použít hello:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

Použijte následující postup tooadd hello nové nejvyšší úrovně domény pomocí prostředí PowerShell hello

1. Na počítači, který má [Azure Active Directory modul pro prostředí Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) nainstalován spustit následující hello: `$cred=Get-Credential`.  
2. Zadejte hello uživatelské jméno a heslo pro globálního správce pro doménu hello Azure AD, které jsou federaci s
3. V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`
4. V prostředí PowerShell zadejte`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Pomocí následujících kroků tooadd hello nové nejvyšší úrovně domény pomocí služby Azure AD Connect hello.

1. Spusťte Azure AD Connect z plochy hello a nabídky start
2. Zvolte "Přidat další domény Azure AD" ![přidat další doménu služby Azure AD](./media/active-directory-multiple-domains/add1.png)
3. Zadejte služby Azure AD a pověření služby Active Directory
4. Druhá doména vyberte hello chcete tooconfigure pro federaci.
   ![Přidat další doménu služby Azure AD](./media/active-directory-multiple-domains/add2.png)
5. Kliknutím na tlačítko Instalovat

### <a name="verify-hello-new-top-level-domain"></a>Ověření nové domény nejvyšší úrovně hello
Pomocí příkazu Powershellu hello `Get-MsolDomainFederationSettings -DomainName <your domain>`můžete zobrazit hello aktualizovat IssuerUri.  Následující snímek obrazovky Hello ukazuje, že nastavení federačního hello bylo aktualizováno na našem původní http://bmcontoso.com/adfs/services/trust domény

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

A hello IssuerUri v naší nové domény byl nastaven toohttps://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a>Podpora pro subdomén
Když přidáte subdomény, z důvodu hello způsobem Azure AD zpracovává domén, zdědí nastavení hello nadřazeného hello.  To znamená, že hello IssuerUri musí toomatch hello nadřazené položky.

To umožňuje vyslovte například, že mám bmcontoso.com a poté přidejte corp.bmcontoso.com.  To znamená že hello IssuerUri pro uživatele z corp.bmcontoso.com bude potřebovat toobe **http://bmcontoso.com/adfs/services/trust.**  Ale standardního pravidla hello implementovat výše pro Azure AD, bude vygenerovat token s vystavitele jako **http://corp.bmcontoso.com/adfs/services/trust.** které nebude odpovídat požadovaná hodnota hello domény a ověření se nezdaří.

### <a name="how-tooenable-support-for-sub-domains"></a>Jak tooenable podpora subdomén
V pořadí toowork kolem tohoto hello vztah důvěryhodnosti předávající strany pro Microsoft Online služby AD FS musí toobe aktualizovat.  toodo, musíte nakonfigurovat vlastní pravidlo deklarace identity, aby ji odstraní vypnout všechny subdomén z hello uživatele (UPN) příponu při vytváření vlastní hodnotu vystavitele hello. 

Hello následující deklarace identity se to udělat:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
číslo poslední Hello v regulárním výrazu hello nastavit hello kolik nadřazené domény v kořenové doméně. I tady mají bmcontoso.com, takže jsou potřebné dvě nadřazené domény. Pokud tři nadřazené domény byly toobe zachovány (tj.: corp.bmcontoso.com), pak hello číslo by byl tři. Můžete zadat Eventualy rozsah, hello shody bude vždy provést toomatch hello maximální počet domén. "{2,3}" bude odpovídat dvěma doménami toothree (tj.: bmfabrikam.com a corp.bmcontoso.com).

Pomocí následujících kroků tooadd hello vlastních deklarací identity toosupport subdomén.

1. Správa služby otevřete AD FS
2. Klikněte pravým tlačítkem na vztah důvěryhodnosti Microsoft Online RP hello a zvolte pravidla upravit deklarace identity
3. Vyberte hello třetí pravidlo deklarace identity a nahraďte ![úpravy deklarací identity](./media/active-directory-multiple-domains/sub1.png)
4. Nahraďte hello aktuální deklarace identity:
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Nahraďte deklarace identity](./media/active-directory-multiple-domains/sub2.png)

5. Klikněte na tlačítko Ok.  Kliknutím na tlačítko použít.  Klikněte na tlačítko Ok.  Zavřete správu služby AD FS.

