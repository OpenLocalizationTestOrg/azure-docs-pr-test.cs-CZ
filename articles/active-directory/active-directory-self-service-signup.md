---
title: "aaaWhat je Samoobslužná registrace pro Azure? | Dokumentace Microsoftu"
description: "Přehled samoobslužné registrace, Azure, jak toomanage hello procesu registrace a jak tootake přes název domény DNS."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: dbf3b59e3807e98f7bf39f3d5591fcde01667323
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-self-service-signup-for-azure"></a>Co je Samoobslužná registrace pro Azure?
Toto téma vysvětluje proces hello samoobslužné registrace a jak tootake přes název domény DNS.  

## <a name="why-use-self-service-signup"></a>Proč používat samoobslužné registrace?
* Tooservices zákazníci GET, které mají být rychlejší.
* Vytvořte e mailové nabídky pro službu.
* Vytvořte e mailové registrace toky, které rychle povolit uživatelům toocreate identit pomocí jejich aliasy snadno zapamatovat pracovní e-mailu.
* Nespravované Azure adresáře mohou být provedeny později do spravovaných adresáře a znovu použít pro jiné služby.

## <a name="terms-and-definitions"></a>Termíny a definice
* **Samoobslužná registrace do**: hello metodu, pomocí kterého uživatel zaregistruje cloudovou službu a má identitu automaticky vytvoří pro ně v Azure Active Directory (Azure AD), na základě jejich e-mailovou doménu.
* **Nespravované Azure directory**: Toto je hello adresáře, kde se má vytvořit danou identitu. Nespravované adresáře se adresář, který má žádný globální správce.
* **Ověřit e-mailu uživatel**: Jedná se o typ uživatelského účtu ve službě Azure AD. Uživatel, který má identity vytvoří automaticky po registraci na nabídku samoobslužné služby se označuje jako uživatel s ověřenou e-mailu. Ověření e-mailu uživatel je členem regulární adresáře označené creationmethod = EmailVerified.

## <a name="user-experience"></a>Činnost koncového uživatele
Předpokládejme například, že uživatele, jejichž e-mailu je Dan@BellowsCollege.com obdrží citlivé soubory e-mailem. Hello soubory chráněné službou Azure Rights Management (Azure RMS). Ale Dana pro organizaci, univerzity, kterou vlnovce, nebyl zaregistrovali do služby Azure RMS, ani nasadil Active Directory RMS. V takovém případě Dana můžete si zaregistrovat bezplatné předplatné tooRMS pro jednotlivce v pořadí tooread hello chráněné soubory.

Pokud Dana hello prvního uživatele s e-mailovou adresu z BellowsCollege.com toosign pro tento samoobslužný nabídky, pak adresář nespravované vytvoří se pro BellowsCollege.com ve službě Azure AD. Pokud jiných uživatelů z domény BellowsCollege.com hello zaregistrovat této nabídky nebo podobné nabídky samoobslužné služby, budou mít i uživatele ověřit e-mailových účtů vytvořených v rámci hello stejné nespravované adresáři v Azure.

## <a name="admin-experience"></a>Z pohledu správce
Správce, který vlastní název domény DNS hello adresář nespravované Azure můžete převzít kontrolu nad nebo sloučení hello directory po prokázání vlastnictví. Hello další části popisují z pohledu správce hello podrobněji, ale tady je Shrnutí:

* Pokud převezmete adresář nespravované Azure, jednoduše stát hello globální správce adresáře nespravované hello. Někdy se označuje jako interní převzetí.
* Sloučení nespravované Azure directory, přidejte název domény DNS hello hello nespravované directory tooyour spravované Azure adresáře a bylo vytvořeno mapování uživatelů prostředků proto uživatelé můžou pokračovat tooaccess služeb bez přerušení. Někdy se označuje jako externí převzetí.

## <a name="what-gets-created-in-azure-active-directory"></a>Co se vytvoří v Azure Active Directory?
#### <a name="directory"></a>Adresář
* Adresář služby Azure Active Directory pro doménu hello je vytvořen, jeden adresář v každé doméně.
* adresář Azure AD Hello nemá žádné globální správce.

#### <a name="users"></a>Uživatelé
* Pro každý uživatel, který zaregistruje je vytvořen objekt uživatele v adresáři služby Azure AD hello.
* Každý objekt uživatele je označena jako externí.
* Každý uživatel přístup toohello služba, která jejich registraci aplikace.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Jak deklarace samoobslužné služby Azure AD adresář pro doménu I vlastní?
Samoobslužné služby Azure AD můžete deklarace adresář tak, že provedete ověření domény. Ověření domény prokáže hello vlastní domény tak, že vytvoříte záznamy DNS.

Existují dva způsoby toodo převzetí DNS z adresáře služby Azure AD:

* interní převzetí (Správce zjistí adresář nespravované Azure a chce tooturn do spravovaných adresáře)
* externí převzetí (správce pokusí tooadd nové domény tootheir spravované Azure adresáře)

Může zajímat ověření vlastní domény, protože adresář nespravované jsou převzetí po uživatel provedl samoobslužné registrace, nebo může být přidání nové domény tooan stávající spravovaný adresář. Například máte doménu s názvem contoso.com a chcete tooadd novou doménu s názvem contoso.co.uk nebo contoso.uk.

## <a name="what-is-domain-takeover"></a>Co je převzetí domény?
Tato část popisuje jak toovalidate, že jste vlastníkem domény

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Co je ověření domény a proč slouží?
Pořadí operací tooperform na adresář vyžaduje Azure AD ověřit vlastnictví domény DNS hello.  Ověření domény hello umožňuje vám tooclaim hello adresář a buď zvýšit úroveň hello samoobslužných služeb directory tooa spravovaný adresář nebo sloučení hello samoobslužné služby adresáře do existující spravované adresáře.

## <a name="examples-of-domain-validation"></a>Příklady ověření domény
Existují dva způsoby toodo DNS převzetí adresáře:

* interní převzetí (například správce zjistí adresář samoobslužné služby, nespravované a chce tooturn do spravovaných adresáře)
* externí převzetí (například k oprávnění správce pokusí tooadd nový adresář tooa spravované domény)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-toobe-a-managed-directory"></a>Interní převzetí - podporovat samoobslužné služby, nespravované directory toobe spravovaný adresář
Když provedete interní převzetí, získá hello directory převést z adresář spravované tooa nespravované adresáře. Je nutné toocomplete DNS název ověření domény, kde vytvoříte záznam MX nebo záznam TXT v zóně DNS hello. Tato akce:

* Ověří, že jste vlastníkem domény hello
* Umožňuje spravovat adresář hello
* Díky hello globální správce adresáře hello

Řekněme, že správce IT z vlnovce univerzity, kterou zjistí, že uživatelé z hello školní zaregistrovali pro samoobslužné služby nabídky. Hello registrovaného vlastníka hello DNS název BellowsCollege.com, hello správce IT může ověřit vlastnictví hello název DNS v Azure a pak převzít nespravované directory hello. Hello adresáře se pak stane spravovaný adresář a hello správce IT je přiřazen hello roli globálního správce pro adresář BellowsCollege.com hello.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externí převzetí - sloučit adresář samoobslužné služby do existující spravované adresář
V externí převzetí již máte spravovaný adresář a chcete, aby všichni uživatelé a skupiny z toojoin nespravované adresář, který spravovat adresář, a nikoli vlastní dva samostatné adresáře.

Jako správce adresáře spravované přidání domény a domény se stane toohave adresář nespravované s ním spojená.

Řekněme například, jsou správce IT a už máte spravovaný adresář pro doménu Contoso.com, název domény, který je registrovaný tooyour organizace. Zjistíte, že uživatelé z vaší organizace provedli Samoobslužná registrace až pro nabídky pomocí doménového názvu e-mailu user@contoso.co.uk, což je jiný název domény, který vaše organizace vlastní. Tito uživatelé nyní mají účty v nespravované adresář pro contoso.co.uk.

Dva samostatné adresáře toomanage, nechcete, aby tak sloučit hello nespravované adresář pro contoso.co.uk do existujícího adresáře spravované IT pro doménu contoso.com.

Externí převzetí způsobem hello stejný proces ověření DNS jako interní převzetí.  Přičemž rozdíl: uživatelů a služeb jsou změnu mapování toohello IT spravovaný adresář.

#### <a name="whats-hello-impact-of-performing-an-external-takeover"></a>Co je hello dopad provedení externí převzetí?
S externí převzetí se vytvoří mapování uživatelů prostředků, uživatelé můžou pokračovat tooaccess služeb bez přerušení. Mnoho aplikací, včetně služby RMS pro jednotlivce, dobře zpracovávají hello mapování uživatelů prostředků a uživatelé můžou pokračovat tooaccess těchto služeb bez změn. Pokud aplikace efektivně nezpracovává hello mapování uživatelů na prostředky, může být externí převzetí explicitně blokované tooprevent uživatele z nízký prostředí.

#### <a name="directory-takeover-support-by-service"></a>Podpora převzetí Directory službou
Aktuálně hello následující podporu převzetí služeb:

* RMS

Hello následující služby bude brzy k dispozici podpora převzetí:

* PowerBI

Následující Hello nepodporují a po externí převzetí vyžadovat další správce akce toomigrate uživatelská data.

* SharePoint nebo OneDrive

## <a name="how-tooperform-a-dns-domain-name-takeover"></a>Jak tooperform doménu DNS název převzetí
Máte několik možností tooperform ověření domény (a pokud chcete provést převzetí):

1. Portál pro správu Azure

   Pomocí tohoto postupu přidání domény převzetím aktivaci.  Pokud adresář již existuje pro hello domény, budete mít možnost tooperform hello externí převzetí.

   Přihlaste se toohello portálu Azure pomocí svých přihlašovacích údajů.  Přejděte tooyour existující adresář a potom příliš**přidáním domény**.
2. Office 365

   Můžete použít možnosti hello na hello [spravovat domény](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) stránky v Office 365 toowork s doménami a záznamy DNS. V tématu [ověřte svoji doménu v Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).
3. Windows PowerShell

   Hello následující kroky jsou požadované tooperform ověřování pomocí prostředí Windows PowerShell.

   | Krok | Rutiny toouse |
   | --- | --- |
   | Vytvořit objekt přihlašovacích údajů |Get-Credential |
   | Připojit tooAzure AD |Connect-MsolService |
   | získat seznam domén |Get-MsolDomain |
   | Vytvoření výzvu |Get-MsolDomainVerificationDns |
   | Vytvořit záznam DNS |To udělat na serveru DNS |
   | Ověření výzvy hello |Confirm-MsolEmailVerifiedDomain |

Například:

1. Připojit tooAzure AD pomocí hello přihlašovacích údajů, které byly použité toorespond toohello samoobslužné služby nabídky:

        import-module MSOnline
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
2. Získáte seznam domén:

    Get-MsolDomain
3. Spusťte toocreate rutiny Get-MsolDomainVerificationDns hello výzvu:

    Get-MsolDomainVerificationDns – DomainName *your_domain_name* – DnsTxtRecord režimu

    Například:

    Get-MsolDomainVerificationDns – DomainName contoso.com – DnsTxtRecord režimu
4. Zkopírujte hello (hello výzvy) vrácená hodnota z tohoto příkazu.

    Například:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9
5. V oboru názvů veřejné DNS vytvořte záznam DNS txt, který obsahuje hello hodnotu, kterou jste zkopírovali v předchozím kroku hello.

    Hello název pro tento záznam je hello název nadřazené domény hello, takže pokud vytvoříte tento záznam o prostředku pomocí role DNS hello ze systému Windows Server, ponechte název záznamu hello prázdné a právě vložit hodnotu hello do textového pole pro hello
6. Spusťte hello potvrdit MsolDomain rutiny tooverify hello výzvy:

    Potvrďte MsolEmailVerifiedDomain - DomainName *your_domain_name*

    Například:

    Potvrďte MsolEmailVerifiedDomain - DomainName contoso.com

Úspěšné výzvy vrátí toohello řádku bez chyby.

## <a name="how-do-i-control-self-service-settings"></a>Jak řídit nastavení samoobslužné služby?
Správci mají dvou ovládacích prvků samoobslužné služby ještě dnes. Kontroly nad:

* Zda uživatelé mohou připojit hello directory e-mailem.
* Zda uživatelé mohou licence sami pro aplikace a služby.

### <a name="how-can-i-control-these-capabilities"></a>Jak můžete řídit tyto funkce?
Správce můžete nakonfigurovat tyto možnosti, pomocí těchto parametrů rutiny Set-MsolCompanySettings Azure AD:

* **AllowEmailVerifiedUsers** Určuje, jestli uživatel může vytvořit nebo připojit k nespravované adresáři. Pokud nastavíte tento parametr příliš$ false, a ne ověřit e-mailu uživatelé mohou připojit hello adresáře.
* **AllowAdHocSubscriptions** hello možnosti pro uživatele tooperform samoobslužné služby zaregistrovat ovládací prvky. Pokud nastavíte tento parametr příliš$ hodnotu false, žádní uživatelé mohou provádět samoobslužné registrace.

### <a name="how-do-hello-controls-work-together"></a>Jak ovládací prvky hello spolupracují?
Tyto dva parametry lze použít v kombinaci toodefine přesnější kontrolu nad samoobslužné registrace. Hello následující příkaz například povolí uživatelé tooperform samoobslužné registrace, ale pouze, pokud tito uživatelé již máte účet ve službě Azure AD (jinými slovy, uživatele, kteří se potřebují ověření e-mailu účet toobe vytvořit nelze provést Samoobslužná registrace nahoru):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Hello následující vývojový diagram vysvětluje všechny různé kombinace hello pro tyto parametry a výsledný podmínky hello hello adresář a Samoobslužná registrace do.

![][1]

Další informace a příklady, jak toouse těchto parametrů, najdete v části [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="see-also"></a>Viz také
* [Jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Referenční informace k rutinám Azure](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
