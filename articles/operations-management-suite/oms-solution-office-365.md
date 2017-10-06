---
title: "aaaOffice 365 řešení v Operations Management Suite (OMS) | Microsoft Docs"
description: "Tento článek poskytuje podrobné informace o konfiguraci a použití řešení hello Office 365 v OMS.  Obsahuje podrobný popis záznamů hello Office 365 vytvořené v analýzy protokolů."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: a1507745251ff015abb785bae8352fea7cea0734
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="office-365-solution-in-operations-management-suite-oms"></a>Řešení Office 365 v Operations Management Suite (OMS)

![Logo Office 365](media/oms-solution-office-365/icon.png)

Hello řešení Office 365 pro Operations Management Suite (OMS) vám umožní toomonitor prostředí Office 365 v analýzy protokolů.  

- Monitorování aktivit uživatelů na vaše vzorce používání tooanalyze účtů Office 365 a také identifikovat trendy chování. Například můžete rozbalit konkrétní využití scénáře, jako jsou například soubory, které jsou sdíleny mimo vaši organizaci nebo hello nejoblíbenější webů služby SharePoint.
- Sledovat změny v konfiguraci správce aktivity tootrack nebo operations vysoká oprávnění.
- Zjištění a prozkoumat chování nežádoucí uživatele, který lze přizpůsobit potřebám vaší organizace.
- Předvedení auditu a dodržování předpisů. Například můžete sledovat přístup operací se soubory na důvěrné soubory, které vám můžou pomoct s hello procesu audit a dodržování předpisů.
- Proveďte provozní řešení potíží pomocí příkazu vyhledávání OMS nad data Office 365 aktivit vaší organizace.

## <a name="prerequisites"></a>Požadavky
Následující Hello je požadovaná předchozí toothis řešení je nainstalován a nakonfigurován.

- Organizace předplatné služeb Office 365.
- Přihlašovací údaje pro uživatelský účet, který je globálním správcem.
- tooreceive data auditu, je nutné [konfigurace auditování](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) v rámci vašeho předplatného Office 365.  Všimněte si, že [auditování poštovní schránky](https://technet.microsoft.com/library/dn879651.aspx) je samostatně nakonfigurovaný.  Přesto můžete nainstalovat hello řešení a shromažďovat další data, pokud není nakonfigurováno auditování.
 


## <a name="management-packs"></a>Sady Management Pack
Toto řešení nenainstaluje žádné sady management Pack v připojených skupin pro správu.
  

## <a name="configuration"></a>Konfigurace
Jednou budete [přidat předplatné tooyour řešení hello Office 365](../log-analytics/log-analytics-add-solutions.md), máte tooconnect ho tooyour předplatné služeb Office 365.

1. Přidat tooyour řešení pro správu výstrah hello pracovním prostorem OMS pomocí procesu hello popsané v [přidat řešení](../log-analytics/log-analytics-add-solutions.md).
2. Přejděte příliš**nastavení** na portálu OMS hello.
3. V části **připojené zdroje**, vyberte **Office 365**.
4. Klikněte na **připojení Office 365**.<br>![Spojení jednotlivých Office 365](media/oms-solution-office-365/configure.png)
5. Přihlaste se pomocí účtu, který je globálním správcem pro vaše předplatné tooOffice 365. 
6. Objeví se Hello předplatné s hello úlohy, které bude monitorovat hello řešení.<br>![Spojení jednotlivých Office 365](media/oms-solution-office-365/connected.png) 


## <a name="data-collection"></a>Shromažďování dat
### <a name="supported-agents"></a>Podporovaní agenti
Hello řešení Office 365 nepodporuje načtení dat ze všech hello [OMS agenti](../log-analytics/log-analytics-data-sources.md).  Načte data přímo z Office 365.

### <a name="collection-frequency"></a>Četnost shromažďování dat
Odešle Office 365 [webhooku oznámení](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) s podrobná data tooLog Analytics pokaždé, když se vytvoří záznam.

## <a name="using-hello-solution"></a>Pomocí řešení hello
Když přidáte hello Office 365 pracovním prostorem OMS tooyour řešení, hello **Office 365** dlaždice se přidá tooyour OMS řídicího panelu. Tuto dlaždici zobrazí počet a grafické znázornění hello počet počítačů ve vašem prostředí a jejich dodržování předpisů aktualizací.<br><br>
![Dlaždice souhrnu Office 365](media/oms-solution-office-365/tile.png)  

Klikněte na hello **Office 365** dlaždice tooopen hello **Office 365** řídicího panelu.

![Řídicí panel Office 365](media/oms-solution-office-365/dashboard.png)  

řídicí panel Hello obsahuje sloupce hello hello následující tabulka. Každý sloupec uvádí hello top deset výstrahy podle odpovídající počet, sloupce kritéria pro hello zadaný rozsah oboru a času. Můžete spustit hledání protokolu, které poskytuje celý seznam hello kliknutím najdete všechny dolnímu hello hello sloupce nebo kliknutím na záhlaví sloupce hello.

| Sloupec | Popis |
|:--|:--|
| Operace | Poskytuje informace o hello aktivní uživatelé z vašich všechny monitorované předplatných Office 365. Bude také možné toosee hello počet aktivit, které dojít v průběhu času.
| Výměna | Ukazuje rozpis hello aktivity systému Exchange Server, například Add-Mailbox oprávnění nebo Set-poštovní schránky. |
| SharePoint | Zobrazuje hello nejvyšší aktivity, že uživatelé provádět na dokumenty služby SharePoint. Když přejdete k podrobnostem z této dlaždice, hello vyhledávání stránka zobrazuje podrobnosti o hello tyto aktivity, jako je například hello cílovém dokumentu a hello umístění této aktivity. Například pro událost přistupovat k souboru, bude možné toosee hello dokument, který se přistupuje, jeho přidružený účet názvu a IP adresy. |
| Azure Active Directory | Obsahuje hlavní uživatelské aktivity, například Resetovat heslo uživatele a pokusů o přihlášení. Když přejdete k podrobnostem, bude možné toosee hello podrobnosti o tyto aktivity, jako jsou hello výsledný stav. Toto je nejčastěji užitečné, pokud chcete, aby toomonitor podezřelých aktivit v Azure Active Directory. |




## <a name="log-analytics-records"></a>Záznamy služby Log Analytics

Mají všechny záznamy vytvořené v pracovní prostor analýzy protokolů hello řešením hello Office 365 **typ** z **OfficeActivity**.  Hello **OfficeWorkload** vlastnost určuje, které záznam hello služby Office 365 odkazuje příliš Exchange azureactivedirectory selhala, SharePoint nebo OneDrive.  Hello **RecordType** vlastnost určuje typ hello operace.  Hello vlastnosti pro každý typ operace se bude lišit a jsou uvedené v následujících tabulkách hello.

### <a name="common-properties"></a>Běžné vlastnosti
Hello následující vlastnosti jsou společné záznamy tooall Office 365.

| Vlastnost | Popis |
|:--- |:--- |
| Typ | *OfficeActivity* |
| Když | IP adresa Hello hello zařízení, která byla použita při protokolu byla zaznamenána aktivita hello. Hello IP adresa se zobrazí ve formátu adresa IPv4 nebo IPv6. |
| OfficeWorkload | Služby Office 365, která hello záznam odkazuje.<br><br>Azureactivedirectory selhala<br>Výměna<br>SharePoint|
| Operace | Název Hello hello aktivity uživatele nebo správce.  |
| Kódu organizace | Hello identifikátor GUID pro klienta Office 365 vaší organizace. Tato hodnota bude vždy být hello stejné pro vaši organizaci, bez ohledu na to služby hello Office 365, ve kterém it dojde. |
| recordType | Typ operace provádí. |
| ResultStatus | Určuje, zda akce hello (zadaný v hello vlastnost operaci) byla úspěšná, nebo ne. Možné hodnoty jsou úspěšné, PartiallySucceded nebo se nezdařilo. Aktivita správce systému Exchange, hodnota hello je buď True nebo False. |
| ID uživatele | Hello hlavní název uživatele (hlavní název uživatele) hello uživatele, který provedl hello akci, která byla vygenerována v záznamu hello protokolována; například my_name@my_domain_name. Všimněte si, že zaznamenává aktivity prováděné účty systému (například SHAREPOINT\system nebo NTAUTHORITY\SYSTEM) jsou také zahrnuty. | 
| UserKey | Alternativní ID pro uživatele hello identifikovaného v hello vlastnost ID uživatele.  Tato vlastnost bude zahrnovat například hello passport jedinečné ID (identifikátor PUID) pro události se provádí uživatelů na SharePoint, Onedrivu pro firmy a Exchange. Tato vlastnost může také určit hello stejnou hodnotu jako hello vlastnost ID uživatele pro události, ke kterým dochází v jiných službách a událostech provádí účty systému|
| UserType | Hello typ uživatele, které provádí operace hello.<br><br>Správce<br>Aplikace<br>DcAdmin<br>Regulární<br>Rezervováno<br>ServicePrincipal<br>Systémový |


### <a name="azure-active-directory-base"></a>Základní Azure Active Directory
Hello následující vlastnosti jsou společné záznamy tooall Azure Active Directory.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | Azureactivedirectory selhala |
| recordType     | Azureactivedirectory selhala |
| AzureActiveDirectory_EventType | Hello typu události Azure AD. |
| ExtendedProperties | Hello rozšířené vlastnosti události hello Azure AD. |


### <a name="azure-active-directory-account-logon"></a>Azure přihlašovací účet služby Active Directory
Tyto záznamy vytvářejí, když se pokusí toolog na uživatele služby Active Directory.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | Azureactivedirectory selhala |
| recordType     | AzureActiveDirectoryAccountLogon |
| Aplikace | Hello aplikace, která spustí událost přihlášení účtu hello, jako je například Office 15. |
| Klient | Podrobnosti o klientovi hello zařízení, operačního systému zařízení a prohlížeč zařízení, která byla použita pro hello události přihlášení účtu hello. |
| loginStatus | Tato vlastnost je přímo z OrgIdLogon.LoginStatus. mapování Hello různé zajímavé nezdařených pokusů o přihlášení může provést výstrahy algoritmy. |
| UserDomain | Hello klienta Identity informace (TÍ). | 


### <a name="azure-active-directory"></a>Azure Active Directory
Tyto záznamy jsou vytvořeny při změna nebo přidání probíhají tooAzure objektů služby Active Directory.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | Azureactivedirectory selhala |
| recordType     | Azureactivedirectory selhala |
| AADTarget | Hello uživatele, který na byla provedena akce hello (identifikovaný hello vlastnost operace). |
| Objektu actor | uživatel Hello nebo objektu služby, které provádí akce hello. |
| ActorContextId | Hello GUID hello organizace, která hello objektu actor patří. |
| ActorIpAddress | Hello objektu actor IP adresu ve formátu adresy IPV4 nebo IPV6. |
| InterSystemsId | Hello GUID, které sledují hello akce mezi komponentami v rámci služby hello Office 365. |
| IntraSystemId |   Hello identifikátor GUID, které se generují ve službě Azure Active Directory tootrack hello akce. |
| SupportTicketId | Hello zákaznickou podporu ID lístku pro akci hello v situacích, "act-on-behalf-of". |
| TargetContextId | Hello GUID hello organizace, která hello cílový uživatel patří. |


### <a name="data-center-security"></a>Datové Centrum zabezpečení
Tyto záznamy jsou vytvořené z dat auditu zabezpečení dat Center.  

| Vlastnost | Popis |
|:--- |:--- |
| EffectiveOrganization | Název Hello hello klienta, který hello zvýšení oprávnění nebo rutiny byl cílená na. |
| ElevationApprovedTime | Dobrý den, kdy byla schválena hello zvýšení časové razítko. |
| ElevationApprover | Název Hello správce společnosti Microsoft. |
| ElevationDuration | Doba trvání Hello, pro které hello byl aktivní zvýšení oprávnění. |
| ElevationRequestId |  Jedinečný identifikátor pro požadavek hello zvýšení oprávnění. |
| ElevationRole | bylo vyžádáno Hello role hello zvýšení oprávnění. |
| ElevationTime | Hello počáteční čas hello zvýšení oprávnění. |
| Start_Time | Hello počáteční čas provádění rutiny hello. |


### <a name="exchange-admin"></a>Správce systému Exchange
Tyto záznamy jsou vytvořeny při změně konfigurace tooExchange.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | Výměna |
| recordType     | ExchangeAdmin |
| ExternalAccess |  Určuje, zda byl hello rutinu spustit uživatel v organizaci, pracovníky datového centra společnosti Microsoft nebo účtu služby datacenter nebo delegovaný správce. Hello hodnotu False označuje, že byla spuštěna rutinu hello uživatel ve vaší organizaci. Hello hodnotu True označuje, že byla spuštěna rutinu hello pracovníky datacenter, účet služby datacenter nebo delegovaný správce. |
| ModifiedObjectResolvedName |  Toto je popisný název uživatele hello hello objektu, který hello rutina, byla změněna. To se zaznamená pouze v případě, že rutina hello změní hello objektu. |
| Název organizace | Název Hello hello klienta. |
| OriginatingServer | Název Hello hello serveru, ze které hello byl proveden rutiny. |
| Parametry | Hello název a hodnotu pro všechny parametry, které byly používány s hello rutinu, která je definovaný v hello vlastnost Operations. |


### <a name="exchange-mailbox"></a>Poštovní schránky serveru Exchange
Tyto záznamy jsou vytvořeny při změny nebo přidání probíhají tooExchange poštovních schránek.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | Výměna |
| recordType     | ExchangeItem |
| ClientInfoString | Informace o hello e-mailového klienta, který byl použité tooperform hello operace, například verze prohlížeče, verze aplikace Outlook a informace o mobilních zařízeních. |
| Client_IPAddress | IP adresa Hello hello zařízení, která byla použita při protokolu byla zaznamenána hello operaci. Hello IP adresa se zobrazí ve formátu adresa IPv4 nebo IPv6. |
| ClientMachineName | Hello název počítače, který je hostitelem hello klientovi aplikace Outlook. |
| ClientProcessName | Hello e-mailového klienta, který byl použité tooaccess hello poštovní schránky. |
| ClientVersion | verze Hello hello e-mailového klienta. |
| InternalLogonType | Vyhrazeno pro interní použití. |
| Logon_Type | Označuje typ hello uživatele, který přístup hello poštovní schránky a provést operaci hello, kterým byl přihlášen. |
| LogonUserDisplayName |    Hello uživatelsky přívětivý název hello uživatele, který provedl operaci hello. |
| LogonUserSid | Hello SID hello uživatele, který provedl operaci hello. |
| MailboxGuid | Hello Exchange GUID hello poštovní schránky, který byl přístupný. |
| MailboxOwnerMasterAccountSid | Účet vlastníka poštovní schránky hlavní SID účtu. |
| MailboxOwnerSid | Hello SID vlastníka hello poštovní schránky. |
| MailboxOwnerUPN | e-mailovou adresu Hello hello osoby, která vlastní hello poštovní schránky, který byl přístupný. |


### <a name="exchange-mailbox-audit"></a>Auditování poštovní schránky systému Exchange
Tyto záznamy jsou vytvořeny při vytvoření položky auditu poštovní schránky.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | Výměna |
| recordType     | ExchangeItem |
| Položka | Představuje položku hello, při které hello byla provedena operace | 
| SendAsUserMailboxGuid | Hello Exchange GUID hello poštovní schránky, který byl přístup k toosend e-mailu. |
| SendAsUserSmtp | Adresa SMTP hello uživatele, který jde o zosobňovaného. |
| SendonBehalfOfUserMailboxGuid | Hello Exchange GUID hello poštovní schránky, který byl přístup k e-mailu toosend jménem. |
| SendOnBehalfOfUserSmtp | Adresa SMTP uživatele hello na jehož jménem hello e-mailu se odesílají. |


### <a name="exchange-mailbox-audit-group"></a>Skupiny auditování poštovní schránky serveru Exchange
Tyto záznamy jsou vytvořeny při změny nebo přidání probíhají tooExchange skupiny.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | Výměna |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Informace o jednotlivých položkách ve skupině hello. |
| CrossMailboxOperations | Označuje, pokud operace hello zapojená víc než jednu poštovní schránku. |
| DestMailboxId | Nastavte jenom v případě, že parametr CrossMailboxOperations hello má hodnotu True. Určuje poštovní schránky cíl hello identifikátor GUID. |
| DestMailboxOwnerMasterAccountSid | Nastavte jenom v případě, že parametr CrossMailboxOperations hello má hodnotu True. Určuje hello SID pro hlavní účet hello SID vlastníka poštovní schránky cíl hello. |
| DestMailboxOwnerSid | Nastavte jenom v případě, že parametr CrossMailboxOperations hello má hodnotu True. Určuje hello SID hello cíl poštovní schránky. |
| DestMailboxOwnerUPN | Nastavte jenom v případě, že parametr CrossMailboxOperations hello má hodnotu True. Určuje hello UPN hello vlastníka hello cíl poštovní schránky. |
| DestFolder | Hello cílovou složku pro operace, jako je například přesunout. |
| Složka | Hello složku, kde je umístěn skupinu položek. |
| Složky |     Informace o složkách na zdrojový hello zapojenou do činnosti; Pokud například složky jsou vybrané a pak je odstranit. |


### <a name="sharepoint-base"></a>Základní služby SharePoint
Tyto vlastnosti jsou společné záznamy tooall služby SharePoint.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | Určuje, že k události došlo ve službě SharePoint. Možné hodnoty jsou SharePoint nebo ObjectModel. |
| Typ položky | Hello typ objektu, který byl přístup nebo upravil. V tématu hello ItemType tabulky Podrobnosti o hello typy objektů. |
| MachineDomainInfo | Informace o zařízení synchronizační operace. Tyto informace je nahlášeno pouze v případě, že je k dispozici v žádosti o hello. |
| MachineId |   Informace o zařízení synchronizační operace. Tyto informace je nahlášeno pouze v případě, že je k dispozici v žádosti o hello. |
| Site_ | Hello GUID hello lokality, kde se nachází hello soubor nebo složku, ke kterému hello uživatel přístup. |
| Source_Name | Hello entity, který aktivoval hello auditovat operace. Možné hodnoty jsou SharePoint nebo ObjectModel. |
| UserAgent | Informace o hello uživatele klienta nebo prohlížeče. Tato informace jsou poskytovány hello klienta nebo prohlížeče. |


### <a name="sharepoint-schema"></a>Schéma služby SharePoint
Tyto záznamy jsou vytvořeny při provedení změn konfigurace tooSharePoint.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Volitelný řetězec pro vlastní události. |
| Event_Data |  Volitelné datové části pro vlastní události. |
| ModifiedProperties | Vlastnost Hello je zahrnuté pro správce události, jako je například přidávání uživatele jako člen webu nebo skupinu pro správu kolekce webu. Vlastnost Hello obsahuje název hello hello vlastnosti, které bylo změněno (například hello správce webu skupiny), nová hodnota hello změněn vlastnost (takové hello uživatel, který byl přidán jako správce webu) a předchozí hodnotu hello hello změnit objekt hello. |


### <a name="sharepoint-file-operations"></a>Operace se soubory služby SharePoint
Tyto záznamy jsou vytvořeny v odpovědi toofile operace ve službě SharePoint.

| Vlastnost | Popis |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | Hello příponu souboru, který je zkopírovat nebo přesunout. Tato vlastnost se zobrazí pouze pro FileCopied a FileMoved události. |
| DestinationFileName | Název Hello hello souboru, který je zkopírovat nebo přesunout. Tato vlastnost se zobrazí pouze pro FileCopied a FileMoved události. |
| DestinationRelativeUrl | Adresa URL Hello hello cílovou složku, kde je soubor zkopírovat nebo přesunout. kombinace Hello hello hodnot pro parametry SiteURL, DestinationRelativeURL a DestinationFileName je hello stejné jako hello hodnotu pro vlastnost ObjectID hello, což je úplná cesta název hello hello souboru, který jste zkopírovali. Tato vlastnost se zobrazí pouze pro FileCopied a FileMoved události. |
| SharingType | Typ Hello oprávnění, které byly přiřazeny toohello uživatele, který byl hello prostředků sdílet s sdílení. Tento uživatel je určen parametrem UserSharedWith hello. |
| Site_Url | Adresa URL Hello hello lokality, kde se nachází hello soubor nebo složku, ke kterému hello uživatel přístup. |
| SourceFileExtension | Přípona souboru Hello hello souboru, který byl přístupný uživatelem hello. Tato vlastnost je prázdná, pokud je složka hello objekt, který byl přístupný. |
| SourceFileName |  Název Hello hello souboru nebo složce, ke kterému hello uživatel přístup. |
| SourceRelativeUrl | Adresa URL Hello hello složky, která obsahuje soubor hello přístup uživatele hello. kombinace Hello hello hodnot pro parametry SiteURL, SourceRelativeURL a SourceFileName hello je hello stejné jako hello hodnotu pro vlastnost ObjectID hello, což je hello úplnou cestu pro soubor hello přístup uživatele hello. |
| UserSharedWith |  Hello uživatel, který prostředek se sdílet s. |




## <a name="sample-log-searches"></a>Ukázky hledání v protokolech
Hello následující tabulka obsahuje ukázkový protokol hledání aktualizace záznamů shromažďují toto řešení.

| Dotaz | Popis |
| --- | --- |
|Počet všech operací hello na předplatné služeb Office 365 |Typ = OfficeActivity | měření count() operace. |
|Použití webů služby SharePoint|Typ = OfficeActivity OfficeWorkload = služby sharepoint | míra count() jako počet podle SiteUrl | řazení počet asc.|
|Operace se soubory přístup podle typu uživatele|Typ = OfficeActivity OfficeWorkload = sharepoint operaci = FileAccessed | měření count() podle UserType.|
|Hledání s konkrétní – klíčové slovo|`Type=OfficeActivity OfficeWorkload=azureactivedirectory "MyTest"`|
|Externí akce monitorování na Exchange|`Type=OfficeActivity OfficeWorkload=exchange ExternalAccess = true`|



## <a name="troubleshooting"></a>Řešení potíží

Pokud vaše řešení Office 365 není shromažďování dat podle očekávání, zkontrolujte stav na portál OMS hello **nastavení** -> **připojené zdroje** -> **Office 365** . Hello následující tabulka popisuje každý stav.

| Status | Popis |
|:--|:--|
| Aktivní | Hello předplatné služeb Office 365 je aktivní a hello úlohy je úspěšně připojeno tooyour pracovním prostorem OMS. |
| Čekající na vyřízení | Hello předplatné služeb Office 365 je aktivní, ale hello úlohy připojený pracovním prostorem OMS tooyour úspěšně ještě není. Hello prvním připojení odběru hello Office 365, všechny úlohy hello bude tento stav dokud úspěšně připojeno. Počkejte prosím 24 hodin pro všechny tooActive tooswitch úlohy hello. |
| Neaktivní | Hello předplatné služeb Office 365 je v neaktivním stavu. Zkontrolujte stránku Správce služeb Office 365 podrobnosti. Po aktivaci předplatné služeb Office 365, zrušit propojení s vaším pracovním prostorem OMS a propojit jej znovu toostart přijímá data. |



## <a name="next-steps"></a>Další kroky
* Pomocí protokolu vyhledávání v [analýzy protokolů](../log-analytics/log-analytics-log-searches.md) tooview podrobná data aktualizace.
* [Vytvořit vlastní řídicí panely](../log-analytics/log-analytics-dashboards.md) toodisplay váš oblíbený vyhledávací dotazy Office 365.
* [Vytvářet výstrahy](../log-analytics/log-analytics-alerts.md) toobe proaktivně oznámení důležité aktivit Office 365.  
