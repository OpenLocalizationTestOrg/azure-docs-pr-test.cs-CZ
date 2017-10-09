---
title: "Azure AD Connect: Účty a oprávnění | Microsoft Docs"
description: "Toto téma popisuje účty hello používá a vytvořit a oprávněních."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.reviewer: cychua
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: 70a7013e0353d74714ec8a3ff54b3e811789a0b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Účty a oprávnění
Průvodce instalací Hello Azure AD Connect nabízí dva různé cesty:

* Expresní nastavení Průvodce hello vyžaduje další oprávnění.  Toto je, abyste mohli nastavit konfiguraci snadno, aniž byste museli uživatelé toocreate nebo konfigurace oprávnění.
* V části vlastní nastavení hello Průvodce nabízí další možnosti a možnosti. Existují však některé situace, ve kterých je nutné tooensure nastavená správná oprávnění hello sami.

## <a name="related-documentation"></a>Související dokumentace
Pokud jste si dokumentaci hello na [integrace místních identit s Azure Active Directory](../active-directory-aadconnect.md), hello následující tabulka obsahuje odkazy toorelated témata.

|Téma |Odkaz|  
| --- | --- |
|Stažení služby Azure AD Connect | [Stažení služby Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Instalace s expresním nastavením | [Expresní instalace služby Azure AD Connect](./active-directory-aadconnect-get-started-express.md)|
|Instalace s vlastním nastavením | [Vlastní instalace služby Azure AD Connect](./active-directory-aadconnect-get-started-custom.md)|
|Upgrade z nástroje DirSync | [Upgrade ze synchronizačního nástroje služby Azure AD (DirSync)](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|Po instalaci | [Ověření instalace hello a přiřazení licencí](active-directory-aadconnect-whats-next.md)|

## <a name="express-settings-installation"></a>Expresní nastavení instalace
V nastavení Express Průvodce instalací hello požádá o pověření správce podniku AD DS.  Toto je tak místní Active Directory se dá nakonfigurovat s požadovaná oprávnění pro Azure AD Connect. Pokud provádíte upgrade z nástroje DirSync, hello AD DS Enterprise Admins pověření jsou použité tooreset hello heslo pro účet hello používaný nástrojem DirSync. Je také nutné přihlašovací údaje Azure AD globálního správce.

| Stránka Průvodce | Přihlašovací údaje shromážděné | Oprávnění vyžadovaná | Použít pro |
| --- | --- | --- | --- |
| Není k dispozici |Průvodce instalací hello spuštěné uživatele |Správce místní server hello |<li>Vytvoří hello místní účet, který se používá jako hello [synchronizovat účet služby modul](#azure-ad-connect-sync-service-account). |
| Připojit tooAzure AD |Přihlašovací údaje pro adresář Azure AD |Roli globálního správce ve službě Azure AD |<li>Zapíná se synchronizace v hello adresář Azure AD.</li>  <li>Vytvoření hello [účet Azure AD](#azure-ad-service-account) který se používá pro synchronizační průběžné operace ve službě Azure AD.</li> |
| Připojit tooAD DS |Místní přihlašovací údaje služby Active Directory |Člen skupiny Enterprise Admins (EA) hello ve službě Active Directory |<li>Vytvoří [účet](#active-directory-account) ve službě Active Directory a tooit uděluje oprávnění. Tento účet vytvořil je používané tooread a zápis directory informace během synchronizace.</li> |

### <a name="enterprise-admin-credentials"></a>Pověření správce podniku
Tyto přihlašovací údaje se používají jenom při instalaci hello a nepoužívají se po dokončení instalace hello. Hello Enterprise Admins, hello správce domény Ujistěte se, že hello oprávnění služby Active Directory lze nastavit ve všech doménách.

### <a name="global-admin-credentials"></a>Globální přihlašovací údaje správce
Tyto přihlašovací údaje se používají jenom při instalaci hello a nepoužívají se po dokončení instalace hello. Je použité toocreate hello [účet Azure AD](#azure-ad-service-account) používá pro synchronizaci změn tooAzure AD. účet Hello taky umožňuje synchronizaci jako funkci ve službě Azure AD.

### <a name="permissions-for-hello-created-ad-ds-account-for-express-settings"></a>Oprávnění pro hello vytvořili účet služby AD DS pro expresní nastavení
Hello [účet](#active-directory-account) vytvořili pro čtení a zápis tooAD DS mít následující oprávnění, když vytvořený Expresní nastavení hello:

| Oprávnění | Použít pro |
| --- | --- |
| <li>Replikovat změny adresáře</li><li>Replikace adresáře všechny změny |Synchronizace hesla |
| Pro čtení a zápis všech vlastností uživatele |Hybridní importu a serveru Exchange |
| Pro čtení a zápis všech iNetOrgPerson vlastnosti |Hybridní importu a serveru Exchange |
| Skupina všech vlastností čtení/zápisu |Hybridní importu a serveru Exchange |
| Obraťte se na čtení/zápisu všechny vlastnosti |Hybridní importu a serveru Exchange |
| Resetování hesla |Příprava k povolení zpětného zápisu hesla |

## <a name="custom-settings-installation"></a>Vlastní nastavení instalace
Azure AD Connect verze 1.1.524.0 a novějším je hello možnost toolet hello Azure AD Connect průvodce vytvořit hello účet používaný tooconnect tooActive adresáře.  Starší verze vyžadují vytvořený účet hello před instalací hello. Tento účet musí udělit oprávnění Hello lze nalézt v [vytvořit účet služby AD DS hello](#create-the-ad-ds-account). 

| Stránka Průvodce | Přihlašovací údaje shromážděné | Oprávnění vyžadovaná | Použít pro |
| --- | --- | --- | --- |
| Není k dispozici |Průvodce instalací hello spuštěné uživatele |<li>Správce místní server hello</li><li>Pokud používáte plnou instalaci systému SQL Server, musí být uživatel hello správce systému (SA) v systému SQL</li> |Ve výchozím nastavení, vytvoří hello místní účet, který se používá jako hello [synchronizovat účet služby modul](#azure-ad-connect-sync-service-account). Hello účet je vytvořen pouze v případě Dobrý den, správce neurčí určitého účtu. |
| Nainstalovat synchronizační služby, možnost účet služby |AD nebo pověření místního uživatelského účtu |Uživatel, oprávnění pomocí Průvodce instalací hello |Pokud dobrý den, správce určuje účet, tento účet slouží jako hello účet služby pro službu synchronizace hello. |
| Připojit tooAzure AD |Přihlašovací údaje pro adresář Azure AD |Roli globálního správce ve službě Azure AD |<li>Zapíná se synchronizace v hello adresář Azure AD.</li>  <li>Vytvoření hello [účet Azure AD](#azure-ad-service-account) který se používá pro synchronizační průběžné operace ve službě Azure AD.</li> |
| Připojení adresářů |Místní přihlašovací údaje služby Active Directory pro jednotlivé doménové struktury, která je připojená tooAzure AD |Hello oprávnění závisí na funkce, které povolíte a lze nalézt v [vytvořit účet hello služby AD DS](#create-the-ad-ds-account) |Tento účet je používané tooread a zápis directory informace během synchronizace. |
| Servery služby AD FS |Pro každý server v seznamu hello hello Průvodce shromažďuje přihlašovací údaje, když hello přihlašovací údaje uživatele hello spuštěním Průvodce hello jsou nedostatečné tooconnect |Správce domény |Instalace a konfigurace role serveru hello AD FS. |
| Proxy servery webových aplikací |Pro každý server v seznamu hello hello Průvodce shromažďuje přihlašovací údaje, když hello přihlašovací údaje uživatele hello spuštěním Průvodce hello jsou nedostatečné tooconnect |Místní správce v cílovém počítači hello |Instalace a konfigurace role serveru WAP. |
| Přihlašovací údaje pro vztah důvěryhodnosti proxy |Pověření důvěryhodnosti federační služby (hello přihlašovací údaje hello proxy server používá tooenroll k certifikátu důvěryhodnosti z hello služby FS |Účet domény, který slouží jako místní správce serveru hello AD FS |Počáteční registraci certifikátu důvěryhodnosti WAP služby FS. |
| Stránka účtu služby FS služby AD, "Použít možnost účet uživatele domény" |Přihlašovací údaje účtu uživatele AD |Uživatel domény |Hello AD uživatelský účet, jehož pověření jsou k dispozici slouží jako hello přihlašovací účet služby hello služby AD FS. |

### <a name="create-hello-ad-ds-account"></a>Vytvoření účtu hello služby AD DS
Hello účet na hello zadáte **připojení adresářů** stránky musí být v předchozí tooinstallation služby Active Directory.  Musí mít rovněž hello požadované oprávnění udělená. Průvodce instalací Hello neověřuje hello oprávnění a všechny problémy jsou pouze zjištěno, že během synchronizace.

Oprávnění, která budete potřebovat, závisí na volitelné funkce hello povolíte. Pokud máte více domén, je potřeba pro všechny domény v doménové struktuře hello udělit oprávnění hello. Pokud některé z těchto funkcí nepovolíte, hello výchozí **uživatele domény** jsou dostatečná oprávnění.

| Funkce | Oprávnění |
| --- | --- |
| Funkce msDS-ConsistencyGuid |Oprávnění k zápisu zdokumentována atribut msDS-ConsistencyGuid toohello [koncepty návrhu - pomocí msDS-ConsistencyGuid jako sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). | 
| Synchronizace hesla |<li>Replikovat změny adresáře</li>  <li>Replikace adresáře všechny změny |
| Hybridní nasazení systému Exchange |Zápis atributů toohello oprávnění popsaná v [zpětný zápis hybridní Exchange](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) pro uživatele, skupiny a kontakty. |
| Veřejné složky e-mailu Exchange |Atributy toohello oprávnění pro čtení zdokumentována [veřejné složky e-mailu Exchange](active-directory-aadconnectsync-attributes-synchronized.md#exchange-mail-public-folder) pro veřejných složek. | 
| Zpětný zápis hesla |Zápis atributů toohello oprávnění popsaná v [Začínáme se správou hesel](../active-directory-passwords-writeback.md) pro uživatele. |
| Zpětný zápis zařízení |Oprávnění udělená pomocí skriptu prostředí PowerShell, jak je popsáno v [zpětný zápis zařízení](active-directory-aadconnect-feature-device-writeback.md). |
| Zpětný zápis skupin |Čtení, vytvoření, aktualizace a odstranění skupiny objekty, pro synchronizována **skupiny Office 365**.  Další informace najdete v části [zpětný zápis skupin](active-directory-aadconnect-feature-preview.md#group-writeback).|

## <a name="upgrade"></a>Upgrade
Při upgradu z jedné verze nástroje Azure AD Connect tooa nové verze, je třeba hello následující oprávnění:

| Objekt zabezpečení | Oprávnění vyžadovaná | Použít pro |
| --- | --- | --- |
| Průvodce instalací hello spuštěné uživatele |Správce místní server hello |Aktualizace binárních souborů. |
| Průvodce instalací hello spuštěné uživatele |Člen skupiny ADSyncAdmins |Proveďte změny pravidel tooSync a další konfigurace. |
| Průvodce instalací hello spuštěné uživatele |Pokud používáte plnou instalaci systému SQL server: DBO (nebo podobnou) hello synchronizační modul databáze |Proveďte změny úrovně databáze, například aktualizace tabulek s novými sloupci. |

## <a name="more-about-hello-created-accounts"></a>Další informace o hello vytvořené účty
### <a name="active-directory-account"></a>Účet služby Active Directory
Pokud chcete použít Expresní nastavení, je vytvořen účet ve službě Active Directory, který je používán k synchronizaci. Hello vytvořili účet se nachází v kořenové doméně doménové struktury hello v kontejneru Uživatelé hello a má názvu předponu s **MSOL_**. Hello účet je vytvořen s dlouho složité heslo, které nevyprší platnost. Pokud máte zásad hesel ve vaší doméně, ujistěte se, že dlouhá a složitá hesla bude mít možnost pro tento účet.

![Účet AD](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

Pokud chcete použít vlastní nastavení, pak jste zodpovědní za vytváření hello účtu před zahájením instalace hello.

### <a name="azure-ad-connect-sync-service-account"></a>Účtu synchronizační služby Azure AD Connect
Hello synchronizační služba může běžet pod různými účty. Může běžet pod **virtuální účet služby** (Atribut), **skupinový účet spravované služby** (gMSA nebo sMSA) nebo běžného uživatelského účtu. Hello podporované možnosti byly změněny s hello verze dubna 2017 připojení při novou instalací. Pokud provádíte upgrade ze starší verze služby Azure AD Connect, nejsou k dispozici tyto další možnosti.

| Typ účtu | Možnost instalace | Popis |
| --- | --- | --- |
| [Účet služby virtuální](#virtual-service-account) | Rychlé a vlastní, 2017 Duben a novější | To je možnost hello používá pro všechny Expresní instalace, s výjimkou instalace na řadiči domény. Pro vlastní jedná se hello výchozí možnost, pokud se používá jinou možnost. |
| [Účet spravované služby skupiny](#group-managed-service-account) | Vlastní, 2017 Duben a novější | Pokud používáte vzdálený SQL server, pak doporučujeme toouse skupinu účet spravované služby. |
| [Uživatelský účet](#user-account) | Rychlé a vlastní, 2017 Duben a novější | Uživatelský účet s předponou AAD_ je vytvořen pouze během instalace při instalaci v systému Windows Server 2008 a nainstalovat na řadič domény. |
| [Uživatelský účet](#user-account) | Rychlé a vlastní, 2017 března a starší | Místní účet s předponou AAD_ je vytvořen během instalace. Pokud používáte vlastní instalaci, můžete zadat jiný účet. |

Pokud používáte připojení s sestavení z března 2017 nebo starší, pak by neměl resetovat heslo hello v účtu služby hello od Windows zničí hello šifrovacích klíčů z bezpečnostních důvodů. Jiný účet hello účet tooany nelze změnit bez opětovné instalace Azure AD Connect. Pokud upgradu tooa sestavení z 2017 duben nebo novější, pak je podporované toochange hello heslo účtu služby hello ale nelze změnit účet hello používá.

> [!Important]
> Účet služby hello lze nastavit pouze na první instalaci. Není podporováno účet služby hello toochange po dokončení instalace hello.

Toto je tabulku hello výchozí, nedoporučuje, a podporované možnosti pro hello synchronizace účtu služby.

Legenda:

- **Tučné** znamená hello výchozí možnost a ve většině případů hello doporučená možnost.
- *Kurzíva* označuje hello doporučená možnost, pokud není výchozí možnost hello.
- 2008 - výchozí možnost při instalaci v systému Windows Server 2008
- Non-bold – podporované možnost
- Místní účet - místní uživatelský účet na serveru hello
- Účet domény - uživatelský účet domény
- sMSA - [samostatný účet spravované služby](https://technet.microsoft.com/library/dd548356.aspx)
- gMSA - [skupinový účet spravované služby](https://technet.microsoft.com/library/hh831782.aspx)

| | LocalDB</br>Express | LocalDB nebo LocalSQL</br>Vlastní | Vzdálený server SQL</br>Vlastní |
| --- | --- | --- | --- |
| **samostatné nebo pracovní skupiny počítače** | Nepodporuje se | **ATRIBUT**</br>Místní účet (2008)</br>Místní účet |  Nepodporuje se |
| **počítač připojený k doméně** | **ATRIBUT**</br>Místní účet (2008) | **ATRIBUT**</br>Místní účet (2008)</br>Místní účet</br>Účet domény</br>sMSA, gMSA | **gMSA**</br>Účet domény |
| **Řadič domény** | **Účet domény** | *gMSA*</br>**Účet domény**</br>sMSA| *gMSA*</br>**Účet domény**|

#### <a name="virtual-service-account"></a>Účet služby virtuální
Účet služby virtuální je speciální typ účtu, který nemá heslo a je spravována službou Windows.

![ATRIBUT](./media/active-directory-aadconnect-accounts-permissions/aadsyncvsa.png)

Hello Atribut je určený toobe použitá se scénáři, kde jsou hello synchronizační modul a SQL v hello stejný server. Pokud používáte vzdálený SQL, pak doporučujeme toouse [skupinový účet spravované služby](#managed-service-account) místo.

Tato funkce vyžaduje Windows Server 2008 R2 nebo novější. Pokud Azure AD Connect instalujete na Windows Server 2008, pak instalace hello spadne zpět toousing [uživatelský účet](#user-account) místo.

#### <a name="group-managed-service-account"></a>Účet skupiny spravované služby
Pokud používáte vzdálený SQL server, pak doporučujeme toousing **skupiny účet spravované služby**. Další informace o tom, najdete v části služby Active Directory pro účet spravované služby skupiny, tooprepare [přehled účtů spravované služby skupiny](https://technet.microsoft.com/library/hh831782.aspx).

Tato možnost na hello toouse [instalace požadovaných součástí](active-directory-aadconnect-get-started-custom.md#install-required-components) vyberte **použít existující účet služby**a vyberte **účet spravované služby**.  
![ATRIBUT](./media/active-directory-aadconnect-accounts-permissions/serviceaccount.png)  
Je také podporované toouse [samostatný účet spravované služby](https://technet.microsoft.com/library/dd548356.aspx). Ale tyto lze použít pouze v místním počítači hello a neexistuje žádné výhody toouse je přes hello výchozí virtuální službu účet.

Tato funkce vyžaduje Windows Server 2012 nebo novější. Pokud potřebujete toouse se starším operačním systémem a použití vzdáleného SQL, pak je nutné použít [uživatelský účet](#user-account).

#### <a name="user-account"></a>Uživatelský účet
Průvodce instalací hello vytvoří místní účet služby (Pokud nezadáte účet toouse hello v vlastní nastavení). účet Hello je předponou **AAD_** a používat pro hello skutečné synchronizační služby toorun jako. Pokud Azure AD Connect instalujete na řadiči domény, účet hello je vytvořen v doméně hello. Hello **AAD_** účet služby musí být umístěn v doméně hello, pokud:
   - použití vzdáleného serveru se systémem SQL server
   - použít proxy server, který vyžaduje ověření

![Účtu synchronizační služby](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Hello účet je vytvořen s dlouho složité heslo, které nevyprší platnost.

Tento účet je jiné účty používané toostore hello hesla pro hello zabezpečené způsobem. Tato hesla jiných účtů jsou uloženy v zašifrované podobě v databázi hello. Hello privátní klíče pro hello šifrovací klíče jsou chráněny šifrování tajného klíče kryptografické služby hello pomocí Windows Data Protection API (DPAPI).

Pokud používáte plnou instalaci systému SQL Server, je účet služby hello hello DBO hello vytvořit databázi pro synchronizační modul hello. Služba Hello nebude fungovat tak, jak má s jinými oprávněními. Je taky vytvořit přihlašovací jméno SQL.

účet Hello také udělena oprávnění toofiles, klíčů registru a jiných objektů souvisejících toohello synchronizační modul.

### <a name="azure-ad-service-account"></a>Účet služby Azure AD
Pro použití hello synchronizační služby je vytvořen účet ve službě Azure AD. Tento účet lze identifikovat podle jeho zobrazovaný název.

![Účet AD](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

Název Hello hello server hello účtu se používá na je možné zjistit v druhé části hello hello uživatelské jméno. Název serveru hello hello obrázku je FABRIKAMCON. Pokud máte pracovní serverů, každý server má svůj vlastní účet.

účet služby Hello je vytvořen s dlouho složité heslo, které nevyprší platnost. Jsou udělena zvláštní role **účty synchronizace adresáře** má pouze oprávnění tooperform directory synchronizace úlohy. Tento speciální předdefinovaná role nelze udělit mimo hello Průvodce Azure AD Connect. Hello portál Azure ukazuje tento účet s rolí hello **uživatele**.

Existuje omezení 20 účtů služeb synchronizace ve službě Azure AD. seznam hello tooget stávající účty Azure AD služby ve službě Azure AD, spusťte následující rutiny Azure AD PowerShell hello:`Get-AzureADDirectoryRole | where {$_.DisplayName -eq "Directory Synchronization Accounts"} | Get-AzureADDirectoryRoleMember`

tooremove nepoužívané účty služby Azure AD, spusťte následující rutiny Azure AD PowerShell hello:`Remove-AzureADUser -ObjectId <ObjectId-of-the-account-you-wish-to-remove>`

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](../active-directory-aadconnect.md).
