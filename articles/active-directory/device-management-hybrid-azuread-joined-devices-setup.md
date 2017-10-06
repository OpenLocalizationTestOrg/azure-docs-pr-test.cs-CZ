---
title: "zařízení připojená k hybridní tooconfigure aaaHow Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak zařízení připojená k hybridní tooconfigure Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: f97ea436eca2833d8a9843acd19e5c633bc0fc07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-hybrid-azure-active-directory-joined-devices"></a>Jak zařízení připojená k hybridní tooconfigure Azure Active Directory

Se správou zařízení ve službě Azure Active Directory (Azure AD) můžete zajistit, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů. Další podrobnosti najdete v tématu [Úvod toodevice správy ve službě Azure Active Directory](device-management-introduction.md).

Pokud máte prostředí místní služby Active Directory a chcete toojoin tooAzure vaše zařízení připojená k doméně AD, můžete k tomu lze konfigurace hybridní Azure AD, které jsou připojené k zařízení. Hello téma vám poskytne hello související kroky. 


## <a name="before-you-begin"></a>Než začnete

Před zahájením konfigurace zařízení ve vašem prostředí připojená k hybridní Azure AD, byste si měli přečíst s hello Podporované scénáře a omezení hello.  

tooimprove hello čitelnost hello popisy, toto téma používá hello následujících podmínek: 

- **Aktuální zařízení se systémem Windows** -tento termín se vztahuje toodomain připojené zařízení se systémem Windows 10 nebo Windows Server 2016.
- **Zařízení se systémem Windows nižší úrovně** -tento termín se vztahuje tooall **podporované** připojený k doméně zařízení Windows, která nejsou spuštěné Windows 10 ani systému Windows Server 2016.  


### <a name="windows-current-devices"></a>Aktuální zařízení se systémem Windows

- Pro zařízení se systémem plochy operační systém Windows hello, doporučujeme používat Windows 10 Anniversary Update (verze 1607) nebo novější. 
- registrace zařízení se systémem Windows aktuální Hello **je** podporované v nefederované prostředích, jako je konfigurace synchronizace hodnoty hash hesla.  


### <a name="windows-down-level-devices"></a>Zařízení se systémem Windows nižší úrovně

- jsou podporovány následující zařízení se systémem Windows nižší úrovně Hello:
    - Windows 8.1
    - Windows 7
    - Windows Server 2012 R2
    - Windows Server 2012
    - Windows Server 2008 R2
- registrace zařízení se systémem Windows nižší úrovně Hello **je** podporované v prostředích nefederovaných prostřednictvím bezproblémové jednotné přihlašování [Azure Active Directory bezproblémové jednotné přihlašování](https://aka.ms/hybrid/sso).
- registrace zařízení se systémem Windows nižší úrovně Hello **není** podporovaná pro zařízení s použitím profilů roamingu. Pokud se spoléháte na cestovních profilů nebo nastavení, používají systém Windows 10.



## <a name="prerequisites"></a>Požadavky

Než začnete, povolení hybridního Azure AD, které jsou připojené k zařízení ve vaší organizaci, je nutné toomake, zda jsou spuštěny na aktuální verzi Azure AD connect.

Azure AD Connect:

- Zachová hello přidružení mezi hello účet počítače v místní službě Active Directory (AD) a objekt hello zařízení ve službě Azure AD. 
- Povoluje další zařízení související s funkcí, jako Windows Hello pro firmy.



## <a name="configuration-steps"></a>Kroky konfigurace

Můžete nakonfigurovat hybridní zařízení připojených k Azure AD pro různé typy zařízení platformy systému Windows. Toto téma obsahuje hello požadované kroky pro všechny scénáře obvyklá konfigurace.  

Použijte následující tabulku tooget přehled hello kroky, které jsou požadovány pro váš scénář hello:  



| Kroky                                      | Synchronizace hodnot hash aktuální a heslo systému Windows | Aktuální Windows a federation | Windows nižší úrovně |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Krok 1: Konfigurace spojovacího bodu služby | ![Zaškrtnout][1]                            | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |
| Krok 2: Nastavení vystavování deklarací identity           |                                        | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |
| Krok 3: Zapnutí zařízení s Windows 10      |                                        |                                | ![Zaškrtnout][1]        |
| Krok 4: Řízení nasazení a zavedení     | ![Zaškrtnout][1]                            | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |
| Krok 5: Ověření připojeného k zařízení          | ![Zaškrtnout][1]                            | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |



## <a name="step-1-configure-service-connection-point"></a>Krok 1: Konfigurace spojovacího bodu služby

objekt Spojovací bod připojení služby Hello používá zařízení během registrace toodiscover hello informace klienta Azure AD. Ve vaší místní služby Active Directory (AD) musí existovat hello spojovací bod služby objektu pro zařízení s Azure AD, které jsou připojené k hybridní hello v hello názvový kontext oddílu konfigurace doménové struktury hello počítače. Není k dispozici pouze jeden názvovém kontextu konfigurace pro každou doménovou strukturu. V konfiguraci s více doménovými strukturami služby Active Directory musí existovat spojovací bod služby hello ve všech doménových strukturách obsahující počítače, připojený k doméně.

Můžete použít hello [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) rutiny tooretrieve hello názvovém kontextu konfigurace doménové struktury.  

Pro doménovou strukturu s názvem domény služby Active Directory hello *fabrikam.com*, je v názvovém kontextu konfigurace hello:

`CN=Configuration,DC=fabrikam,DC=com`

V doménové struktuře se nachází v objektu hello spojovací bod služby pro hello automatické registrace zařízení připojených k doméně:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

V závislosti na tom, jak jste nasadili Azure AD Connect hello spojovací bod služby objekt může mít již byla konfigurována.
Můžete ověřit existenci hello hello objekt a načíst hodnoty hello zjišťování pomocí hello následující skript prostředí Windows PowerShell: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

Hello **$scp. Klíčová slova** výstup zobrazuje informace o klienta hello Azure AD, například:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Pokud spojovací bod služby hello neexistuje, můžete ho vytvořit spuštěním hello `Initialize-ADSyncDomainJoinedComputerSync` rutiny na serveru Azure AD Connect. Pověření správce podniku je požadovaná toorun tuto rutinu.  
rutina Hello:

- Vytvoří spojovací bod služby hello v doménové struktuře služby Active Directory hello, Azure AD Connect je připojena k. 
- Vyžaduje, abyste toospecify hello `AdConnectorAccount` parametr. Toto je hello účet, který je nakonfigurovaný jako účet konektoru ve službě Azure AD connect služby Active Directory. 


Hello skript je ukázkou příklad, pomocí rutiny hello. V tento skript `$aadAdminCred = Get-Credential` vyžaduje, abyste tootype uživatelské jméno. Je třeba tooprovide hello uživatelské jméno ve formátu hlavního názvu (UPN) uživatele hello (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

Hello `Initialize-ADSyncDomainJoinedComputerSync` rutiny:

- Používá modul Active Directory PowerShell hello, které závisí na službě Active Directory Web Services spuštěných na řadiči domény. Služba Active Directory Web Services je podporována v řadičích domény se systémem Windows Server 2008 R2 nebo novější.
- Je podporovaná jenom rozhraním hello **MSOnline PowerShell verze modulu 1.1.166.0**. toodownload tento modul použít [odkaz](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).   

Pro řadiče domény se systémem Windows Server 2008 nebo dřívějších verzí použijte níže uvedený spojovací bod služby hello toocreate skript hello.

V konfiguraci s více doménovými strukturami byste měli použít následující skript spojovací bod služby hello toocreate v každé doménové struktuře, kde existují počítače hello:
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC

    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()


## <a name="step-2-setup-issuance-of-claims"></a>Krok 2: Nastavení vystavování deklarací identity

V federované Azure AD konfigurace zařízení závisí na službě Active Directory Federation Services (AD FS) nebo 3. stran místní federační služby tooauthenticate tooAzure AD. Zařízení ověření tooget přístupu k tokenu tooregister proti hello Azure Active Directory Device Registration Service (Azure DRS).

Windows, které aktuální zařízení ověřování pomocí integrovaného ověřování systému Windows tooan aktivní WS-Trust koncový bod (verze 1.3 nebo 2005) hostované hello místní federační služby.

> [!NOTE]
> Při použití služby AD FS buď **adfs/services/vztah důvěryhodnosti nebo 13 nebo windowstransport** nebo **služby AD FS nebo služby nebo vztahu důvěryhodnosti/2005/windowstransport** musí být povolena. Pokud používáte hello ověřování webový proxy server, ujistěte se také, že tento koncový bod publikovaný prostřednictvím proxy serveru hello. Uvidíte, jaké koncových bodů jsou povolené prostřednictvím konzoly pro správu služby AD FS hello pod **služby > Koncové body**.
>
>Pokud nemáte jako místní federační služby AD FS, postupujte podle pokynů hello vaše dodavatele toomake se, že podporují WS-Trust 1.3 nebo 2005 koncových bodů a že jsou tyto publikovaných prostřednictvím hello Metadata Exchange souboru (MEX).

Hello následující deklarace identity musí existovat v tokenu hello přijatých Azure DRS pro toocomplete registrace zařízení. Azure DRS vytvoří objekt zařízení ve službě Azure AD pomocí některé z těchto informací, které se pak použije v Azure AD Connect tooassociate hello nově vytvořený objekt zařízení hello počítači účet místní.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Pokud máte více než jeden název ověřené domény, je třeba tooprovide hello následující deklarace identity pro počítače:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Pokud jsou již vydání deklaraci identity ImmutableID (například alternativního přihlašovacího ID) je třeba tooprovide jeden odpovídající deklarace identity pro počítače:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

V následujících částech hello najdete informace o:
 
- Hello hodnoty by měla mít jednotlivých deklarací identity
- Jak bude vypadat definice ve službě AD FS

definice Hello vám pomůže tooverify, zda jsou hodnoty hello přítomen nebo pokud potřebujete toocreate je.

> [!NOTE]
> Pokud nepoužíváte místní federačního serveru služby AD FS, postupujte podle dodavatele pokyny toocreate hello odpovídající konfiguraci tooissue tyto deklarace.

### <a name="issue-account-type-claim"></a>Deklarace typu účtu problém

**`http://schemas.microsoft.com/ws/2012/01/accounttype`**– Tato deklarace identity musí obsahovat hodnotu **DJ**, který identifikuje hello zařízení jako počítač připojený k doméně. Ve službě AD FS můžete přidat pravidlo transformace vystavování, které vypadá takto:

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-hello-computer-account-on-premises"></a>Problém objectGUID hello počítači účet místního

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**– Tato deklarace identity musí obsahovat hello **objectGUID** hodnotu hello místní účet počítače. Ve službě AD FS můžete přidat pravidlo transformace vystavování, které vypadá takto:

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-hello-computer-account-on-premises"></a>Problém objectSID hello počítači účet místního

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**– Tato deklarace identity musí obsahovat hello hello **objectSid** hodnotu hello místní účet počítače. Ve službě AD FS můžete přidat pravidlo transformace vystavování, které vypadá takto:

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Vystavovat issuerID pro počítač, pokud ověření více názvů domén ve službě Azure AD

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**– Tato deklarace identity musí obsahovat hello identifikátor URI (Uniform Resource) všech hello ověřit názvy domén, které se připojují pomocí hello místní federační služby (služby AD FS nebo 3. stran) vydávající hello token. Ve službě AD FS můžete přidat pravidla transformace vystavení, které vypadají stejně, jako hello těch, které jsou níže v tom, že ty, které konkrétní pořadí po hello výše. Všimněte si daného jedno pravidlo tooexplicitly problém hello pravidla pro uživatele je nezbytné. V pravidlech hello níže se přidá první pravidlo identifikace uživatele nebo ověřování počítače.

    @RuleName = "Issue account type with hello value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


Ve výše uvedené, hello deklarací

- `$<domain>`je adresa URL služby hello AD FS
- `<verified-domain-name>`je zástupný symbol potřebujete tooreplace s jedním názvů ověřené domény ve službě Azure AD



Další informace o názvech ověřené domény najdete v tématu [přidat tooAzure název vlastní domény služby Active Directory](active-directory-add-domain.md).  
tooget seznam vaše společnost ověřené domény, můžete použít hello [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) rutiny. 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Vystavovat ImmutableID pro počítač, pokud pro uživatele k dispozici jeden (například alternativního přihlašovacího ID, je nastavena)

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**– Tato deklarace identity musí obsahovat platnou hodnotu pro počítače. Ve službě AD FS můžete vytvořit pravidlo pro vystavování transformace následujícím způsobem:

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-toocreate-hello-ad-fs-issuance-transform-rules"></a>Pomocník skriptu toocreate hello AD FS pravidla transformace při vystavení

Hello následující skript vám pomůže s hello vytvoření hello vystavování transformace pravidel popsaných výše.

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with hello value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a>Poznámky 

- Tento skript připojí hello pravidla toohello existující pravidla. Nespouštějte hello skriptu dvakrát protože hello sada pravidel by byl přidán dvakrát. Ujistěte se, že neexistují žádná pravidla odpovídající před spuštěním skriptu hello znovu v těchto deklarací identity (v odpovídajících podmínkách hello).

- Pokud máte více ověřených názvů domén (jak je znázorněno v portálu hello Azure AD nebo pomocí rutiny Get-MsolDomains hello), nastavte hodnotu hello **$multipleVerifiedDomainNames** v hello skriptu příliš**$true**. Také se ujistěte, že odeberete všechny existující issuerid deklaraci, která může být vytvořen přes Azure AD Connect nebo prostřednictvím jiným způsobem. Tady je příklad pro toto pravidlo:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Pokud jste již vystavily **ImmutableID** deklarací identity pro uživatelské účty, nastavte hodnotu hello **$immutableIDAlreadyIssuedforUsers** v hello skriptu příliš**$true**.

## <a name="step-3-enable-windows-down-level-devices"></a>Krok 3: Zapnutí zařízení se systémem Windows nižší úrovně

Pokud některé z vašich zařízení připojených k doméně Windows nižší úrovně zařízení, budete muset:

- Nastavte zásady v Azure AD tooenable tooregister zařízení uživatelů.
 
- Konfigurace vaší místní federační služby tooissue deklarace identity toosupport **integrované ověřování systému Windows (IWA)** pro registraci zařízení.
 
- Přidejte hello Azure AD zařízení ověřování koncový bod toohello místní intranetové zóny, který certifikát tooavoid výzvy při ověřování zařízení hello.

### <a name="set-policy-in-azure-ad-tooenable-users-tooregister-devices"></a>Nastavte zásady v Azure AD tooenable tooregister zařízení uživatelů

tooregister zařízení se systémem Windows nižší úrovně, je nutné toomake zkontrolujte, jestli je nastavená této hello nastavení uživatelů tooallow tooregister zařízení ve službě Azure AD. V hello portálu Azure můžete najít toto nastavení v části:

`Azure Active Directory > Users and groups > Device settings`
    
Hello tyto zásady musí být nastaven příliš**všechny**: **uživatelé mohou registrovat svá zařízení s Azure AD**

![Registrace zařízení](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a>Nakonfigurujte místní služby FS 

Vaše místní federační služba musí podporovat vydávající hello **authenticationmehod** a **wiaormultiauthn** deklarace identity při přijímání ověření požadavku toohello Azure AD předávající strany, která uchovává resouce_params parametr s hodnotou kódovaného jak je uvedené níže:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Pokud takový požadavek pochází, hello místní federační služby musí ověřit hello uživatele s využitím integrované ověřování systému Windows a na úspěch, vydá hello následující dvě deklarace identity:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

Ve službě AD FS musíte přidat tuto metodu ověřování hello předává prostřednictvím pravidel transformace pro vystavování.  

**tooadd toto pravidlo:**

1. V konzole pro správu hello AD FS přejděte příliš`AD FS > Trust Relationships > Relying Party Trusts`.
2. Klikněte pravým tlačítkem na objekt hello vztahu důvěryhodnosti předávající strany pro aplikaci Microsoft Office 365 Identity Platform a vyberte **upravit pravidla deklarací identity**.
3. Na hello **pravidlech transformace vystavení** vyberte **přidat pravidlo**.
4. V hello **pravidlo deklarace identity** seznam šablon, vyberte **odesílat deklarace pomocí vlastního pravidla**.
5. Vyberte **Další**.
6. V hello **název pravidla deklarací** zadejte **pravidlo deklarace identity metoda ověřování**.
7. V hello **pravidlo deklarace identity** pole, typ hello následující pravidlo:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Na federačním serveru, zadejte příkaz prostředí PowerShell hello níže po nahrazení  **\<RPObjectName\>**  hello přijímající strany u objektu s názvem pro vztah důvěryhodnosti objektu předávající strany služby Azure AD. Tento objekt obvykle jmenuje **Microsoft Office 365 Identity Platform**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-hello-azure-ad-device-authentication-end-point-toohello-local-intranet-zones"></a>Přidejte hello Azure AD zařízení ověřování koncový bod toohello místní Intranet zóny

Při ověřování uživatele v zařízení zaregistrovat tooAzure AD můžete posouvat nastavení zásady tooyour připojená k doméně tooadd hello následující adresu URL toohello zóny místního intranetu v aplikaci Internet Explorer zobrazí výzvu tooavoid certifikátu:

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a>Krok 4: Řízení nasazení a zavedení

Když jste dokončili postup hello vyžaduje, jsou připojená k doméně připravené tooautomatically připojení k Azure AD:

- Všechny připojené k doméně zařízení se systémem Windows 10 Anniversary Update a Windows Server 2016 automaticky zaregistrovat s Azure AD v zařízení restartujte nebo přihlášení uživatele. 

- S Azure AD při restartování zařízení hello po dokončení operace připojení hello zaregistrovat nová zařízení.

- Registruje zařízení, které byly dříve Azure AD (například pro Intune) přechod příliš "*připojený k doméně, zaregistrováno v AAD*"; ale trvá delší dobu pro tento proces toocomplete ve všech zařízeních kvůli toohello normálního toku doména a uživatelské aktivity.

### <a name="remarks"></a>Poznámky

- Můžete vytvořit zásad skupiny objekt toocontrol hello zavedení automatické registrace Windows 10 a Windows Server 2016 počítačů připojených k doméně.

- Windows 10. listopadu 2015 aktualizace automaticky spojí se službou Azure AD **pouze** Pokud hello zavedení nastavení objektu zásad skupiny.

- toorollout počítačů se systémem Windows nižší úrovně, můžete nasadit [balíček služby Windows Installer](#windows-installer-packages-for-non-windows-10-computers) toocomputers, kterou jste vybrali.

- Pokud jste push hello zásad skupiny objekt tooWindows 8.1 připojená k doméně, dojde k pokusu o spojení; nedoporučujeme ale, že používáte hello [balíček služby Windows Installer](#windows-installer-packages-for-non-windows-10-computers) toojoin všechna zařízení Windows nižší úrovně. 

### <a name="create-a-group-policy-object"></a>Vytvoření objektu zásad skupiny 

zavedení hello toocontrol aktuální počítačů se systémem Windows, měli byste nasadit hello **registraci připojený k doméně počítače jako zařízení** zařízení toohello objekt zásad skupiny chcete tooregister. Například můžete nasadit hello zásad tooan organizační jednotku nebo skupinu zabezpečení tooa.

**tooset hello zásad:**

1. Otevřete **správce serveru**a potom přejděte příliš`Tools > Group Policy Management`.
2. Přejděte toohello domény uzlu, který odpovídá toohello domény, kam chcete tooactivate automatickou registraci aktuální počítačů se systémem Windows.
3. Klikněte pravým tlačítkem na **objekty zásad skupiny**a potom vyberte **nový**.
4. Zadejte název objektu zásad skupiny. Například * hybridní Azure AD join. 
5. Klikněte na **OK**.
6. Klikněte pravým tlačítkem na váš nový objekt zásad skupiny a potom vyberte **upravit**.
7. Přejděte příliš**konfigurace počítače** > **zásady** > **šablony pro správu** > **Windows Součásti** > **registrace zařízení**. 
8. Klikněte pravým tlačítkem na **registraci připojený k doméně počítače jako zařízení**a potom vyberte **upravit**.
   
   > [!NOTE]
   > Tato šablona zásad skupiny byla přejmenována z dřívějších verzí konzoly pro správu zásad skupiny hello. Pokud používáte starší verzi konzoly hello, přejděte příliš`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`. 

7. Vyberte **povoleno**a potom klikněte na **použít**.
8. Klikněte na **OK**.
9. Odkaz hello tooa umístění objektu zásad skupiny podle svého výběru. Například můžete propojit jej tooa určité organizační jednotce. Můžete také propojit ho tooa konkrétní bezpečnostní skupiny počítačů, které automaticky připojit k službě Azure AD. tooset tuto zásadu pro všechny připojené k doméně Windows 10 a Windows Server 2016 počítače ve vaší organizaci, odkaz hello zásad skupiny objekt toohello domény.

### <a name="windows-installer-packages-for-non-windows-10-computers"></a>Balíčky Instalační služby systému Windows pro počítače s Windows 10

toojoin připojený k doméně nižší úrovně počítače se systémem Windows ve federovaném prostředí, můžete stáhnout a nainstalovat tyto balíček Instalační služby systému Windows (.msi) ze služby Stažení softwaru na hello [připojení k pracovní ploše Microsoft pro počítače s Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=53554)stránky.

Balíček hello můžete nasadit pomocí systém distribuce softwaru jako je System Center Configuration Manager. Hello balíček podporuje možnosti standardní bezobslužnou instalaci hello hello *quiet* parametr. System Center Configuration Manager aktuální větev nabízí další výhody z dřívějších verzí, jako je hello možnost tootrack dokončení registrace. Další informace najdete v tématu [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).

Instalační program Hello vytvoří naplánovanou úlohu na hello systému, který běží v kontextu uživatele hello. Při přihlášení uživatele hello tooWindows, aktivuje se úloha Hello. Úloha Hello bezobslužně spojí hello zařízení s Azure AD s pověřeními uživatele hello po ověřování pomocí integrovaného ověřování systému Windows. toosee hello naplánované úlohy, v zařízení hello přejděte příliš**Microsoft** > **k síti na pracovišti**, a potom přejděte toohello Knihovna plánovače úloh.

## <a name="step-5-verify-joined-devices"></a>Krok 5: Ověření připojeného k zařízení

Úspěšné připojeného k zařízení ve vaší organizaci můžete zkontrolovat pomocí hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) rutiny v hello [modul Azure Active Directory PowerShell](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Hello výstup této rutiny zobrazuje zařízení, které jsou zaregistrované a propojit s Azure AD. tooget všechna zařízení, použijte hello **– všechny** parametr a potom filtru je pomocí hello **deviceTrustType** vlastnost. Připojené k doméně zařízení mít hodnotu **připojený k doméně**.

## <a name="next-steps"></a>Další kroky

* [Úvod toodevice správy ve službě Azure Active Directory](device-management-introduction.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
