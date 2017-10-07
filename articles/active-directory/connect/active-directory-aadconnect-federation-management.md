---
title: "aaaActive Directory Federation Services management a přizpůsobení službou Azure AD Connect | Microsoft Docs"
description: "Správa služby AD FS s Azure AD Connect a přizpůsobení uživatele AD FS přihlašování uživatelů s Azure AD Connect a prostředí PowerShell."
keywords: "Služba AD FS, služba AD FS, služby AD FS správy, AAD Connect, připojení, přihlášení, služby AD FS přizpůsobení, opravte federačního vztahu důvěryhodnosti, O365, předávající strany"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 361a2bfd6d7a6993dbe773d6ea039ad1afc6346a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a>Spravovat a přizpůsobit Active Directory Federation Services přes Azure AD Connect
Tento článek popisuje, jak toomanage a přizpůsobit Active Directory Federation Services (AD FS) pomocí služby Azure Active Directory (Azure AD) připojit. Zahrnuje také dalších běžných úkolů služby AD FS, které budete potřebovat toodo pro celou konfiguraci farmy služby AD FS.

| Téma | Co pokrývá |
|:--- |:--- |
| **Správa služby AD FS** | |
| [Oprava hello důvěryhodnosti](#repairthetrust) |Jak toorepair hello federační vztah důvěryhodnosti s Office 365. |
| [Vytvořit federaci s Azure AD pomocí alternativního přihlašovacího ID](#alternateid) | Konfigurace federace pomocí alternativního přihlašovacího ID  |
| [Přidejte server služby AD FS](#addadfsserver) |Jak tooexpand služby AD FS, farmy s dalším serverem služby AD FS. |
| [Přidat server proxy webové aplikace služby AD FS](#addwapserver) |Jak tooexpand služby AD FS, farmy s dalším serverem Proxy webových aplikací (WAP). |
| [Přidání federované domény](#addfeddomain) |Jak tooadd federovanou doménu. |
| [Aktualizovat certifikát SSL hello](active-directory-aadconnectfed-ssl-update.md)| Jak tooupdate hello SSL certificate pro farmu služby AD FS. |
| **Přizpůsobení služby AD FS** | |
| [Přidat vlastní logo nebo obrázku](#customlogo) |Jak přihlášení toocustomize služby AD FS stránky s firemní logo a ilustrace. |
| [Přidejte popis přihlášení](#addsignindescription) |Jak tooadd přihlášení stránce popis. |
| [Upravit pravidla deklarace identity služby AD FS](#modclaims) |Jak toomodify služby AD FS deklarací pro různé scénáře federace. |

## <a name="manage-ad-fs"></a>Správa služby AD FS
Pomocí Průvodce hello Azure AD Connect můžete provádět různé AD FS související úlohy v Azure AD Connect s zásah uživatele. Po dokončení instalace služby Azure AD Connect průvodcem hello spuštěné hello průvodce můžete spustit znovu tooperform další úkoly.

## Oprava hello důvěryhodnosti<a name=repairthetrust></a>
Můžete použít Azure AD Connect toocheck hello aktuální stav hello služby AD FS a vztah důvěryhodnosti služby Azure AD a proveďte příslušné akce toorepair hello důvěryhodnosti. Postupujte podle těchto kroků toorepair vaše Azure AD a služby AD FS důvěřují.

1. Vyberte **AAD opravy a služby AD FS důvěřují** hello seznamu další úkoly.
   ![Opravit AAD a AD FS vztah důvěryhodnosti](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)

2. Na hello **připojit tooAzure AD** stránky, zadejte svoje přihlašovací údaje globálního správce pro Azure AD a klikněte na tlačítko **Další**.
   ![Připojit tooAzure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)

3. Na hello **přihlašovací údaje vzdálený přístup** zadejte hello přihlašovací údaje pro správce domény hello.

   ![Přihlašovací údaje vzdálený přístup](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    Po kliknutí na tlačítko **Další**, Azure AD Connect kontroluje stav certifikátu a zobrazuje všechny problémy.

    ![Stav certifikátů](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    Hello **připraven tooconfigure** stránka zobrazuje hello seznam akcí, které budou provést toorepair hello důvěryhodnosti.

    ![Připraveno tooconfigure](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. Klikněte na tlačítko **nainstalovat** toorepair hello důvěryhodnosti.

> [!NOTE]
> Azure AD Connect může pouze opravit nebo zákona o certifikáty, které jsou podepsané svým držitelem. Azure AD Connect nelze opravit, certifikáty třetích stran.

## Vytvořit federaci s Azure AD pomocí AlternateID<a name=alternateid></a>
Doporučujeme, aby hello místní Name(UPN) hlavní název uživatele a cloud hello hlavní název uživatele jsou zachovány hello stejné. Pokud hello místní UPN používá směrovat domény (např.) Contoso.Local) nebo se nedá změnit z důvodu závislosti aplikací toolocal, doporučujeme nastavení alternativního přihlašovacího ID. Alternativního přihlašovacího ID vám umožní tooconfigure sign-in prostředí, kde uživatelé mohou přihlásit pomocí atributu než jejich UPN, jako je například e-mailu. Hello volbu pro hlavní název uživatele v Azure AD Connect výchozí toohello atribut userPrincipalName ve službě Active Directory. Pokud můžete vybrat jiný atribut pro hlavní název uživatele a jsou federaci pomocí služby AD FS, pak Azure AD Connect se konfigurace služby AD FS pro alternativním přihlašovacím ID Níže je uveden příklad vybrat jiný atribut pro hlavní název uživatele:

![Výběr atributu alternativní ID](media/active-directory-aadconnect-federation-management/attributeselection.png)

Konfigurace alternativního přihlašovacího ID pro službu AD FS zahrnuje dva hlavní kroky:
1. **Konfigurace hello správnou sadu vystavování deklarací**: důvěřovat hello pravidel vystavování deklarací identity v předávající strany hello Azure AD jsou atribut UserPrincipalName hello vybrané upravené toouse hello alternativní ID uživatele hello.
2. **Povolit alternativního přihlašovacího ID v konfiguraci služby AD FS hello**: Konfigurace hello AD FS se aktualizuje tak, aby služba AD FS můžete vyhledat uživatele v hello odpovídající doménovými strukturami pomocí hello alternativní ID. Tato konfigurace je podporována pro službu AD FS v systému Windows Server 2012 R2 (s KB2919355) nebo novější. Pokud server hello AD FS 2012 R2, vyžaduje Azure AD Connect zkontroluje přítomnost hello hello KB. Pokud hello KB není zjištěna, upozornění se zobrazí po dokončení konfigurace, jak je uvedeno níže:

    ![Upozornění na 2012R2 chybí KB](media/active-directory-aadconnect-federation-management/kbwarning.png)

    Konfigurace hello toorectify v případě chybějící KB, nainstalujte hello požadované [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) a pak opravte hello důvěryhodnosti pomocí [opravit AAD a vztah důvěryhodnosti služby AD FS](#repairthetrust).

> [!NOTE]
> Další informace o toomanually alternateID a kroky konfigurace, přečtěte si [konfigurace alternativního přihlašovacího ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)

## Přidejte server služby AD FS<a name=addadfsserver></a>

> [!NOTE]
> server tooadd služby AD FS, Azure AD Connect vyžaduje certifikát PFX hello. Proto můžete tuto operaci provést pouze v případě, že jste nakonfigurovali farmy hello AD FS pomocí Azure AD Connect.

1. Vyberte **nasadit další federační Server**a klikněte na tlačítko **Další**.

   ![Další federační server](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. Na hello **připojit tooAzure AD** stránky, zadejte svoje přihlašovací údaje globálního správce pro Azure AD a klikněte na tlačítko **Další**.

   ![Připojit tooAzure AD](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. Zadejte pověření správce domény hello.

   ![Pověření správce domény](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. Azure AD Connect požádá o hello heslo souboru PFX hello, který jste zadali při konfiguraci nové farmě služby AD FS službou Azure AD Connect. Klikněte na tlačítko **zadejte heslo** tooprovide hello heslo pro soubor PFX hello.

   ![Heslo certifikátu](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![Zadejte certifikát SSL](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. Na hello **servery služby AD FS** stránky, zadejte název serveru hello nebo IP adresu toobe přidat toohello AD FS farmy.

   ![Servery služby AD FS](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. Klikněte na tlačítko **Další**a projít hello konečné **konfigurace** stránky. Po dokončení přidávání hello servery toohello AD FS farmy Azure AD Connect bude mu udělená hello možnost tooverify hello připojení.

   ![Připraveno tooconfigure](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![Instalace byla dokončena](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## Přidání serveru AD FS WAP<a name=addwapserver></a>

> [!NOTE]
> tooadd serveru WAP, Azure AD Connect vyžaduje certifikát PFX hello. Pokud jste nakonfigurovali farmy hello AD FS pomocí Azure AD Connect tedy můžete provádět jenom tuto operaci.

1. Vyberte **nasadit Proxy webových aplikací** hello seznamu dostupných úloh.

   ![Nasazení služby Proxy webových aplikací](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. Zadejte pověření pro globálního správce Azure hello.

   ![Připojit tooAzure AD](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. Na hello **certifikát SSL zadejte** stránky, zadejte heslo hello hello souboru PFX, který jste zadali při konfiguraci farmy hello AD FS službou Azure AD Connect.
   ![Heslo certifikátu](media/active-directory-aadconnect-federation-management/WapServer3.PNG)

    ![Zadejte certifikát SSL](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. Přidejte toobe server hello přidán jako WAP server. Protože hello WAP server nemusí být připojené k toohello domény, Průvodce hello požádá o přidávaný server toohello přihlašovací údaje pro správu.

   ![Přihlašovací údaje správce serveru](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. Na hello **přihlašovací údaje pro vztah důvěryhodnosti Proxy** stránky, zadejte přihlašovací údaje pro správu tooconfigure hello proxy vztahu důvěryhodnosti a přístup hello primární server ve farmě služby AD FS hello.

   ![Přihlašovací údaje pro vztah důvěryhodnosti proxy](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. Na hello **připraven tooconfigure** stránce hello Průvodce zobrazí hello seznam akcí, které budou provedeny.

   ![Připraveno tooconfigure](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. Klikněte na tlačítko **nainstalovat** toofinish hello konfigurace. Po dokončení konfigurace hello hello poskytuje průvodce hello hello tooverify možnost připojení toohello servery. Klikněte na tlačítko **ověřte** toocheck připojení.

   ![Instalace byla dokončena](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## Přidání federované domény<a name=addfeddomain></a>

Je snadno tooadd toobe domény Federovaná pomocí Azure AD pomocí Azure AD Connect. Azure AD Connect přidá hello domény pro federaci a upraví deklarace identity hello pravidla toocorrectly odráží vystavitele hello, pokud máte více domén sdružených se službou Azure AD.

1. tooadd federovanou doménu, vyberte hello úloh **přidat další doménu služby Azure AD**.

   ![Další doménu služby Azure AD](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. Na další stránku hello hello průvodci zadejte přihlašovací údaje hello globálního správce pro Azure AD.

   ![Připojit tooAzure AD](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. Na hello **přihlašovací údaje vzdálený přístup** stránky, zadejte přihlašovací údaje správce domény hello.

   ![Přihlašovací údaje vzdálený přístup](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. Na další stránce hello hello Průvodce poskytuje seznam domén služby Azure AD, které můžete vytvořit federaci vašeho místního adresáře s. Vyberte doménu hello hello seznamu.

   ![Azure AD domain](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    Po výběru domény hello hello Průvodce poskytuje k příslušné informace o dalších akcích, které hello průvodce bude trvat a hello dopad hello konfigurace. V některých případech Pokud vyberete domény, který není ještě ověřit ve službě Azure AD, Průvodce hello vám poskytne informace toohelp ověření domény hello. V tématu [přidat název tooAzure vaši vlastní doménu služby Active Directory](../active-directory-add-domain.md) další podrobnosti.

5. Klikněte na **Další**. Hello **připraven tooconfigure** stránka zobrazuje hello seznam akcí, které provádí Azure AD Connect. Klikněte na tlačítko **nainstalovat** toofinish hello konfigurace.

   ![Připraveno tooconfigure](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> Přidat uživatele z hello federované domény musí být synchronizovány, než bude možné toologin tooAzure AD.

## <a name="ad-fs-customization"></a>Přizpůsobení AD FS
Hello následující části obsahují informace o některých běžných úloh hello, můžete mít tooperform při přizpůsobit přihlašovací stránku služby AD FS.

## Přidat vlastní logo nebo obrázku<a name=customlogo></a>
logo hello toochange hello společnosti, který se zobrazí na hello **přihlášení** stránky, použijte následující syntaxi a rutinu prostředí Windows PowerShell hello.

> [!NOTE]
> Hello doporučená dimenze pro hello logo jsou 260 x 35 při 96 dpi a velikost souboru nepřesahovala 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> Hello *TargetName* parametr je povinný. Hello výchozí motiv vydaný se službou AD FS se nazývá výchozí.

## Přidejte popis přihlášení<a name=addsignindescription></a>
tooadd přihlašovací stránce popis toohello **přihlašovací stránce**, použijte následující syntaxi a rutinu prostředí Windows PowerShell hello.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in tooContoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## Upravit pravidla deklarace identity služby AD FS<a name=modclaims></a>
Služba AD FS podporuje bohaté deklarace jazyk, který můžete použít toocreate vlastní pravidla deklarací identity. Další informace najdete v tématu [hello Role hello jazyka pravidel deklarací identity](https://technet.microsoft.com/library/dd807118.aspx).

Hello následující části popisují, jak můžete napsat vlastní pravidla pro některé scénáře, které se týkají tooAzure AD a federační služby AD FS.

### <a name="immutable-id-conditional-on-a-value-being-present-in-hello-attribute"></a>Neměnné ID podmíněného na hodnotu, která se nachází v atributu hello
Azure AD Connect umožňuje určit atributu toobe použít jako zdrojové ukotvení, pokud jsou objekty synchronizované tooAzure AD. Pokud hello hodnoty ve vlastním atributu hello není prázdná, můžete chtít tooissue deklaraci identity neměnné ID.

Například můžete vybrat **ms-ds-consistencyguid** jako atribut hello hello zdrojové ukotvení a problém **ImmutableID** jako **ms-ds-consistencyguid** v případu hello atribut má hodnotu před ním. Pokud není žádná hodnota proti hello atribut, **objectGuid** jako hello neměnné ID. Hello sadu vlastní pravidla deklarací identity můžete vytvořit, jak je popsáno v následující části hello.

**Pravidlo 1: Atributy dotazu**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

V tomto pravidle, že dotazování hello hodnoty **ms-ds-consistencyguid** a **objectGuid** pro hello uživatele ze služby Active Directory. Změnit název příslušné úložiště hello úložiště název tooan ve vašem nasazení služby AD FS. Také změnit hello deklarace identity typu tooa správné deklarace typu pro federační, jak jsou definovány pro **objectGuid** a **ms-ds-consistencyguid**.

Navíc pomocí **přidat** a není **problém**, vyhnete se přidávání odchozí problém pro entitu hello a můžete použít hodnoty hello jako pomocných hodnot. Po stanovení které toouse hodnotu jako text hello neměnné ID vydáte hello deklarace novější pravidlo

**Pravidlo 2: Zkontrolujte, jestli ms-ds-consistencyguid existuje pro uživatele hello**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Toto pravidlo definuje dočasné příznak **idflag** , je nastaven příliš**useguid** Pokud neexistuje žádné **ms-ds-consistencyguid** vyplněný pro uživatele hello. Logika Hello za to je hello fakt, že služba AD FS neumožňuje prázdné deklarací identity. Takže když přidáte http://contoso.com/ws/2016/02/identity/claims/objectguid deklarace identity a http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid v pravidla 1, v níž se **msdsconsistencyguid** pouze v případě deklarace identity Hodnota Hello je vyplněný pro uživatele hello. Pokud není vyplněné, služby AD FS uvidí, že bude mít prázdnou hodnotu a okamžitě se zahodí. Všechny objekty se mají **objectGuid**, tak, že deklarace bude vždy existovat po provedení pravidla 1.

**Pravidlo 3: Vystavení ms-ds-consistencyguid jako neměnné ID, pokud je k dispozici**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Jedná se implicitní **existují** zkontrolujte. Pokud hodnota hello hello deklarace identity existuje, potom vydat, jako hello neměnné ID. předchozí příklad Hello používá hello **nameidentifier** deklarací identity. Budete mít toochange tento typ toohello odpovídající deklarace identity pro neměnné ID hello ve vašem prostředí.

**Pravidlo 4: Pokud není k dispozici ms-ds-consistencyGuid vydání objectGuid jako neměnné ID**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

V tomto pravidle, se jednoduše kontrola hello dočasné příznak **idflag**. Rozhodnete, zda na základě deklarace identity hello tooissue na jeho hodnotu.

> [!NOTE]
> Hello pořadí těchto pravidel je důležité.

### <a name="sso-with-a-subdomain-upn"></a>Jednotné přihlašování s subdoména UPN
Můžete přidat více než jedné domény toobe Federovaná pomocí Azure AD Connect, jak je popsáno v [přidání nové federované domény](active-directory-aadconnect-federation-management.md#addfeddomain). Deklarace hlavní název (UPN) uživatele hello musí změnit tak, aby odpovídá toohello kořenové domény a není hello subdomén, zprostředkovatele hello vystavitele ID, protože hello federované kořenové domény platí i pro podřízené hello.

Ve výchozím nastavení hello pravidel deklarace identity pro vystavitele, kterého ID je nastaven jako:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Deklarace ID výchozí vystavitele](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

Výchozí pravidlo Hello jednoduše trvá příponu UPN hello a používá je v deklarace ID vystavitele hello. Například Jan je uživatel v sub.contoso.com a je contoso.com sdružených se službou Azure AD. Jan zadá john@sub.contoso.com jako hello uživatelské jméno při přihlášení tooAzure AD. pravidlo deklarace identity Hello výchozí vystavitele ID ve službě AD FS ji zpracovává v hello následujícím způsobem:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Hodnoty deklarace identity:** http://sub.contoso.com/adfs/services/trust/

toohave pouze hello kořenové domény v hello vystavitele hodnoty deklarace identity, změnit hello deklarace identity pravidlo toomatch hello následující:

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Další kroky
Další informace o [možnosti přihlášení uživatele](active-directory-aadconnect-user-signin.md).
