---
title: "Azure Active Directory B2C: Reference – důvěřovat architektury | Microsoft Docs"
description: "Téma o vlastní zásady Azure Active Directory B2C a hello Identity rozhraní Framework"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: d9634da72cb136ac165dd32e735622b5d0e22ec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="define-trust-frameworks-with-azure-ad-b2c-identity-experience-framework"></a>Definování rozhraní vztah důvěryhodnosti s Azure AD B2C Identity rozhraní Framework

Azure Active Directory B2C (Azure AD B2C) vlastní zásady, které používají hello Identity rozhraní Framework zadejte vaše organizace centralizované službou. Tato služba snižuje složitost hello federaci identit ve velkých komunitě zájmu. složitost Hello je omezená tooa jeden vztah důvěryhodnosti a exchange jednom metadat.

Azure vlastní zásady AD B2C, které používají hello Identity rozhraní Framework tooenable jste hello tooanswer následující otázky:

- Jaké jsou hello právní, zabezpečení, ochrany osobních údajů a zásady ochrany dat, které musí být použito?
- Kdo jsou hello kontakty a co se stane akreditované účastník procesy hello?
- Kdo hello získá informace poskytovatelů identit (také označované jako "zprostředkovatelů deklarací identity") a co nabízejí?
- Kdo jsou hello schválené předávající strany (a volitelně, co bude potřebovat)?
- Jaké jsou hello technické "na přenosová hello" požadavky na interoperabilitu pro účastníky?
- Co jsou pravidla provozní "runtime" hello, které musí být vynucuje pro výměnu informací o identitě digitální?

tooanswer vytvořit všechny tyto otázky, Azure AD B2C vlastní zásady, které používají hello Identity rozhraní Framework použití hello důvěřovat Framework (TF). Pojďme se tato konstrukce a co nabízí.

## <a name="understand-hello-trust-framework-and-federation-management-foundation"></a>Pochopení hello důvěřovat Framework a federaci management foundation

Hello důvěřovat Framework je zapsaný specifikace hello identitu, zabezpečení, ochrany osobních údajů a data protection zásad, které musí odpovídat toowhich účastníky v komunitě vás zajímají.

Federovaných identit poskytuje základ pro dosažení záruku identity koncového uživatele v Internetu měřítku. Delegováním identity management toothird stran, lze opětovně použít jedinou digitální identitu pro koncového uživatele s víc předávajících stran.  

Záruku identity vyžaduje, aby zprostředkovatelů identity (IdPs) a poskytovatelé atribut (AtPs) splňovat toospecific zabezpečení, ochrany osobních údajů a provozní zásady a postupy.  Pokud nemůžou provést přímý kontrol, předávající strany (RPs) musí vyvíjet vztahy důvěryhodnosti s hello IdPs a AtPs vybírá toowork s.  

S růstem hello počet příjemci a zprostředkovatelé identity digitálních informací je obtížné toocontinue pairwise management tyto vztahy důvěryhodnosti, nebo i hello pairwise exchange hello technické metadat, které je nutné připojení k síti .  Federační centra jste dosáhli jenom omezené úspěch v řešení těchto problémů.

### <a name="what-a-trust-framework-specification-defines"></a>Definuje jaké specifikaci důvěřovat Framework
Sady TFs jsou hello linchpins modelu hello Framework důvěřovat otevřete Identity Exchange (OIX), kde každý community týkající se řídí konkrétní TF specifikací. Definuje TF specifikace:

- **Hello metriky pro komunity hello zájmu hello definicí zabezpečení a ochrana osobních údajů:**
    - Hello úrovně záruky (LOA), které jsou nabízeny nebo vyžadují účastníky; například uspořádanou sadu hodnocení spolehlivosti pro hello pravosti digitální identitu informací.
    - Hello úrovně ochrany (LOP), které jsou nabízeny nebo vyžadují účastníky; například uspořádanou sadu hodnocení spolehlivosti pro hello ochrana informací digitální identitu, kterou provádí služba účastníky v komunitě hello, které vás zajímají.

- **Hello popis hello digitální identitu informace, které má nabízí/požadované účastníků**.

- **Hello technické zásady pro produkční a spotřeba informací digitální identitu a proto pro měření LOA a LOP. Mezi ně zapsat zásady obvykle patří hello následující kategorie zásady:**
    - Identity kontroly pravopisu systému zásady, například: *jak důrazně se informace o identitě osoby vetted?*
    - Zásady zabezpečení, například: *jak důrazně jsou informace integrity a důvěrnosti chráněné?*
    - Zásady ochrany osobních údajů, například: *co ovládací prvek uživatel mít přes osobní identifikovatelné údaje (PII)*?
    - Funkční schopnost zásady, například: *zprostředkovatele přestane operací, jak funguje kontinuitu a ochranu PII funkce?*

- **Hello technické profily pro produkční a spotřeba digitální identitu informací. Tyto profily zahrnují:**
    - Obor rozhraní, pro které je k dispozici na zadaný LOA digitální identitu informace.
    - Technické požadavky na interoperabilitu ve přenosu.

- **popisy Hello hello různých rolích, můžete provést a hello kvalifikaci, které jsou požadované toofulfill tyto role účastníky v komunitě hello.**

Proto specifikaci TF řídí výměna informací o identitě mezi účastníky hello hello komunity, které vás zajímají: předávající strany, identity a poskytovatelé atribut a atribut ověřovatele.

Specifikace TF je jeden nebo více dokumentů, které slouží jako odkaz pro zásady správného řízení hello hello community týkající se některá z hello kontrolní výraz a spotřeby digitální identitu informací v rámci hello komunity. Je sadu zdokumentovaných zásady a postupy určené tooestablish vztah důvěryhodnosti v hello digitální identity, které se používají pro online transakce mezi členy komunity zájmu.  

Jinými slovy specifikaci TF definuje hello pravidla pro vytváření ekosystém přijatelná federovaných identit pro komunitu.

Aktuálně je rozšířeným dohody o hello benefit takový přístup. Je už nepochybně, specifikace framework usnadňují vývoj hello digitální identitu ekosystémů s ověřitelná charakteristikami zabezpečení, záruky a ochrany osobních údajů, což znamená, že budoucí napříč více komunit týkající se vztah důvěryhodnosti.

Z tohoto důvodu používá Azure AD B2C vlastní zásady, které používají hello Identity rozhraní Framework hello specifikace tvoří základ hello jeho znázornění dat TF toofacilitate interoperability.  

Azure AD B2C vlastní zásady, které využívají hello Framework prostředí Identity představují specifikaci TF jako směs lidské a strojově čitelným data. Některé části tohoto modelu (obvykle oddíly, které jsou více orientované na zásad správného řízení) jsou reprezentována jako odkazuje toopublished zabezpečení a ochrana osobních údajů dokumentaci k zásadám společně s hello související s postupy (pokud existuje). Další části se popisují v podrobností hello konfigurace metadata a runtime pravidla, která usnadňují provozní automatizace.

## <a name="understand-trust-framework-policies"></a>Pochopit zásady důvěryhodnosti Framework

Z hlediska implementace hello TF specifikace se skládá ze sady zásad, které umožňují úplnou kontrolu nad chováním identity a prostředí.  Azure vlastní zásady AD B2C, které využívají hello Identity rozhraní Framework tooauthor můžete povolit a vytvořit vlastní TF prostřednictvím takových deklarativní zásad, které můžete definovat a nakonfigurovat:

- odkaz na dokument Hello nebo odkazy, které definují hello federovaných identit ekosystém hello komunity, který má vztah toohello TF. Jsou odkazy toohello TF dokumentaci. Hello provozní (předdefinovaná) "runtime" pravidla nebo hello uživatele cesty, které automatizace a řízení hello exchange a využití hello deklarací identity. Tyto cesty uživatele jsou přidruženy LOA (a LOP). Zásady můžete mít proto cesty uživatele s různými LOAs (a LOPs).

- Hello zprostředkovatele identity a atribut, nebo hello zprostředkovatelů deklarací, v komunitě hello zájmu a hello technické profilů podporují společně s hello (out-of-band) LOA/LOP pověření, která má vztah toothem.

- Hello integrace s atribut ověřovatele nebo poskytovatelů deklarací identity.

- Hello předávající strany v komunitě hello (podle odvození).

- Hello metadata pro zřízení síťové komunikace mezi účastníky. Tato metadata, společně s hello technické profily, se používají při transakci tooplumb "na přenosová hello" vzájemná funkční spolupráce mezi hello předávající strany a ostatní účastníci komunity.

- Hello převod protokolu, pokud existuje (například SAML, OAuth2, WS-Federation a OpenID Connect).

- požadavky na ověřování Hello.

- Hello vícefaktorového orchestration pokud existuje.

- Sdílené schématu pro všechny hello deklarace identity, které jsou k dispozici a mapování tooparticipants komunity zájmu.

- Všechny hello deklarací transformace, společně s hello minimalizace možné data v tomto kontextu, toosustain hello exchange a využití hello deklarací identity.

- Vazba Hello a šifrování.

- Hello deklarací úložiště.

### <a name="understand-claims"></a>Pochopení deklarace identity

> [!NOTE]
> Souhrnně označujeme tooall hello možné typy informací o identitě, který může být vyměňují jako "deklarace identity": deklarace identity o přihlašovacích údajů koncového uživatele ověřování, identity prověřování, komunikace zařízení, fyzického umístění, osobní identifikaci atributy a tak dále.  
>
> Používáme hello termín "deklarace identity" – místo "atributy" – protože online transakcí, nejsou tyto artefakty dat faktů, které jsou přímo ověřovány hello předávající strany. Místo jsou kontrolní výrazy nebo deklarace identity o faktů, pro které hello musí předávající strany vytvořte požadovaný transakce dostatečná spolehlivosti toogrant hello koncového uživatele.  
>
> Používáme termín hello "deklarace identity" technologie Azure AD B2C jsou vlastní zásady, které používají hello Identity Framework prostředí exchange hello toosimplify všech typů informací digitální identitu konzistentním způsobem bez ohledu na tom, zda text hello základní protokol je definována pro načtení ověřování nebo atribut uživatele.  Podobně používáme termín hello "deklarací zprostředkovatelé" toocollectively odkazovat tooidentity zprostředkovatelé, zprostředkovatelé atribut a atribut ověřovatele jsme nechcete, aby toodistinguish mezi vlastní specifické funkce.   

Proto se řídí výměna informací o identitě mezi předávající strany, identity a poskytovatelé atribut a atribut ověřovatele. Zprostředkovatelé atribut je požadován pro ověřování předávající strany a tím i určovat, které identity. Se mají považovat jako jazyk specifické pro doménu (DSL), který je jazyk počítače, který se specializuje na určité domény aplikace s použitím dědičnosti, *Pokud* příkazy, polymorfismus.

Tyto zásady určují hello strojově čitelným část hello TF vytvořit v Azure AD B2C vlastní zásady využití hello Identity Framework prostředí. Obsahují všechny hello provozní údaje, včetně metadat zprostředkovatelů deklarací identit a technické profily, definice schémat deklarace identity, funkce transformace deklarací identity a cesty uživatele, které jsou vyplněny v toofacilitate provozní orchestration a automatizace.  

Budou se předpokládá, že toobe *životností dokumenty* vzhledem k tomu, že je dobré pravděpodobné, že se časem týkající se aktivní účastníky hello deklarované v zásadách hello změnit jejich obsah. Je také hello potenciální, který hello podmínky a ujednání pro účastník, který se může změnit.  

Federační nastavování a údržbu jsou významně zjednodušit tím, jak různé deklarace zprostředkovatelé/ověřovatele připojení nebo nechte (hello komunity reprezentována) stínění předávající strany z probíhající vztah důvěryhodnosti a připojení že změny konfigurace hello sady zásad.

Vzájemná funkční spolupráce je jiné významné výzvu. Další deklarace identity zprostředkovatelé/ověřovatele musí být integrován, protože předávající strany se pravděpodobně toosupport všechny nezbytné protokoly hello. Vlastní zásady služby Azure AD B2C podporuje standardní protokoly vyřešit tento problém a použitím konkrétního uživatele cesty hello tootranspose požadavky, když předávající strany a poskytovatelé atribut nepodporují stejný protokol.  

Cesty uživatele patří protokol profily a metadata, která jsou použité tooplumb "na přenosová hello" vzájemná funkční spolupráce mezi hello předávající strany a ostatní účastníci. Existují také provozní runtime pravidla, která jsou zprávy s informacemi o exchange požadavků a odpovědí tooidentity použité pro vynucování souladu se zásadami publikované jako součást specifikace TF hello. Hello představu o cesty uživatele je klíče toohello přizpůsobení zkušeností zákazníků se hello. Je také osvětluje fungování hello systému na úrovni protokolu hello.

Na základě předávající strany aplikací a portály, v závislosti na jejich kontextu vyvolat vlastní zásady Azure AD B2C, využívají hello Identity Framework prostředí předávání hello název konkrétní zásadu a získat přesněji hello chování a informace Exchange, které chtějí bez žádné muss, fuss nebo rizik.
