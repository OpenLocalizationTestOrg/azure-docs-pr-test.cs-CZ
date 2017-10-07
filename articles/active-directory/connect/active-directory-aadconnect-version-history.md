---
title: "Azure AD Connect: Historie verzí | Microsoft Docs"
description: "Tento článek obsahuje seznam všech verzích Azure AD Connect a Azure AD Sync"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: ef2797d7-d440-4a9a-a648-db32ad137494
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b55e0f2d426e34ceef9869d5a6d1b0956d8bd076
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Historie verzí
tým služby Azure Active Directory (Azure AD) Hello pravidelně aktualizuje s novými funkcemi a funkce Azure AD Connect. Ne všechny přidané jsou příslušné tooall cílové skupiny.

Tento článek je navrženou toohelp udržování přehledu o hello verze, které byly vydány a toounderstand jestli potřebujete tooupdate toohello nejnovější verze nebo ne.

Toto je seznam Příbuzná témata:


Téma |  Podrobnosti
--------- | --------- |
Kroky tooupgrade z Azure AD Connect | Různé metody příliš[upgrade z předchozí verze toohello nejnovější](active-directory-aadconnect-upgrade-previous-version.md) verzi Azure AD Connect.
Požadovaná oprávnění | Oprávnění požadované tooapply aktualizace, najdete v části [účty a oprávnění](./active-directory-aadconnect-accounts-permissions.md#upgrade).
Ke stažení| [Stažení Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="115610"></a>1.1.561.0
Stav: 23 července 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Opravené problém

* Byl opraven problém způsobující hello out-of-box synchronizační pravidlo "Out tooAD - ImmutableId uživatele" toobe odebrat:

  * Hello problém nastane, když Azure AD Connect se upgraduje, nebo když hello úloh možnost *konfigurace synchronizace aktualizace* v hello Azure AD Connect je Průvodce konfigurace synchronizace použité tooupdate Azure AD Connect.
  
  * Toto synchronizační pravidlo se vztahuje toocustomers, který jste povolili hello [msDS-ConsistencyGuid jako zdrojové ukotvení funkce](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). Tato funkce byla zavedená ve verzi 1.1.524.0 a. Pokud je odebráno synchronizační pravidlo hello, Azure AD Connect můžete už naplnit místní AD DS-ms-ConsistencyGuid atribut s hello hodnota atributu ObjectGuid. Nezabrání noví uživatelé z se zřídí do služby Azure AD.
  
  * Oprava Hello zajistí, že bude během upgradu, nebo při změně konfigurace, již odebráno synchronizační pravidlo této hello tak dlouho, dokud je povolena funkce hello. Pro stávající zákazníky, kteří tento problém týká hello oprava zajišťuje, že se přidá tento synchronizační pravidlo hello zpět po upgradu toothis verzi služby Azure AD Connect.

* Pevné problém způsobující out-of-box synchronizační pravidla toohave přednost před hodnotu, která je menší než 100:

  * Obecně platí přednost před hodnoty 0 - 99 jsou vyhrazené pro vlastní synchronizační pravidla. Během upgradu hello přednost hodnoty out-of-box synchronizační pravidla jsou aktualizované tooaccommodate synchronizační pravidlo změny. Z důvodu problému s toothis out-of-box synchronizační pravidla lze přiřadit prioritu hodnotu, která je menší než 100.
  
  * Oprava Hello zabrání hello problém během upgradu. Ale neobnoví hello přednost hodnoty pro stávající zákazníky, kteří byly ovlivněny hello problém. Samostatnou opravu bude k dispozici v budoucí toohelp hello s hello obnovení.

* Kde hello byl opraven problém [domény a filtrování organizační jednotky obrazovky](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) v hello Azure AD Connect se zobrazuje průvodce *synchronizovat všechny domén a organizačních jednotek* možnost jako vybrané, i když je povolené filtrování založené na organizační jednotku.

*   Byl opraven problém této způsobeny hello [obrazovky konfigurace oddílů adresářů](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) v hello Synchronization Service Manager tooreturn chybu, pokud hello *aktualizovat* po kliknutí na tlačítko. Hello chybová zpráva *"došlo k chybě při aktualizaci domény: objekt nelze toocast typu tootype 'System.Collections.ArrayList'. Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject."* Hello chyba nastane, když byla přidána nové domény AD tooan existující doménové struktuře AD a pokoušíte tooupdate Azure AD Connect pomocí hello tlačítko Aktualizovat.

#### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení

* [Funkce Automatické aktualizace](active-directory-aadconnect-feature-automatic-upgrade.md) byl rozšířené toosupport zákazníkům hello následující konfigurace:
  * Povolili jste funkci zpětný zápis zařízení hello.
  * Povolili jste funkce zpětného zápisu skupiny hello.
  * instalace Hello není Expresní nastavení nebo upgradu nástroje DirSync.
  * Máte více než 100 000 objektů v hello metaverse.
  * Připojujete toomore než jedné doménové struktuře. Expresní instalace pouze připojí tooone doménové struktury.
  * Hello AD Connector. účet už není hello výchozí MSOL_ účet.
  * Hello server je nastaven toobe v pracovním režimu.
  * Povolili jste hello funkce zpětný zápis uživatelů.
  
  >[!NOTE]
  >rozšíření oboru Hello funkce Automatický Upgrade hello ovlivňuje zákazníky službou Azure AD Connect sestavení 1.1.105.0 a po. Pokud nechcete, aby vaše toobe server Azure AD Connect automaticky upgradovat, je nutné spustit následující rutinu na serveru Azure AD Connect: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Další informace o povolení nebo zákaz automatického upgradu, najdete v části tooarticle [Azure AD Connect: automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115580"></a>1.1.558.0
Stav: Neuvolní. Změny v tomto sestavení jsou součástí verze 1.1.561.0.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Opravené problém

* Byl opraven problém způsobující hello out-of-box synchronizační pravidlo "na více systémů tooAD - ImmutableId uživatele" toobe odebrány při aktualizaci konfigurace filtrování založené na organizační jednotku. Toto pravidlo synchronizace je vyžadována pro hello [msDS-ConsistencyGuid jako zdrojové ukotvení funkce](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).

* Kde hello byl opraven problém [domény a filtrování organizační jednotky obrazovky](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) v hello Azure AD Connect se zobrazuje průvodce *synchronizovat všechny domén a organizačních jednotek* možnost jako vybrané, i když je povolené filtrování založené na organizační jednotku.

*   Byl opraven problém této způsobeny hello [obrazovky konfigurace oddílů adresářů](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) v hello Synchronization Service Manager tooreturn chybu, pokud hello *aktualizovat* po kliknutí na tlačítko. Hello chybová zpráva *"došlo k chybě při aktualizaci domény: objekt nelze toocast typu tootype 'System.Collections.ArrayList'. Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject."* Hello chyba nastane, když byla přidána nové domény AD tooan existující doménové struktuře AD a pokoušíte tooupdate Azure AD Connect pomocí hello tlačítko Aktualizovat.

#### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení

* [Funkce Automatické aktualizace](active-directory-aadconnect-feature-automatic-upgrade.md) byl rozšířené toosupport zákazníkům hello následující konfigurace:
  * Povolili jste funkci zpětný zápis zařízení hello.
  * Povolili jste funkce zpětného zápisu skupiny hello.
  * instalace Hello není Expresní nastavení nebo upgradu nástroje DirSync.
  * Máte více než 100 000 objektů v hello metaverse.
  * Připojujete toomore než jedné doménové struktuře. Expresní instalace pouze připojí tooone doménové struktury.
  * Hello AD Connector. účet už není hello výchozí MSOL_ účet.
  * Hello server je nastaven toobe v pracovním režimu.
  * Povolili jste hello funkce zpětný zápis uživatelů.
  
  >[!NOTE]
  >rozšíření oboru Hello funkce Automatický Upgrade hello ovlivňuje zákazníky službou Azure AD Connect sestavení 1.1.105.0 a po. Pokud nechcete, aby vaše toobe server Azure AD Connect automaticky upgradovat, je nutné spustit následující rutinu na serveru Azure AD Connect: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Další informace o povolení nebo zákaz automatického upgradu, najdete v části tooarticle [Azure AD Connect: automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115570"></a>1.1.557.0
Stav: 2017 července

>[!NOTE]
>Toto sestavení není k dispozici toocustomers pomocí funkce připojení automaticky upgradovat hello Azure AD.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Opravené problém
* Oprava problému s hello Initialize-ADSyncDomainJoinedComputerSync rutiny, která způsobila hello ověřené domény nakonfigurované na hello existující služby připojení bodu objektu toobe změnit i v případě, že je stále platná doména. K tomuto problému dochází, když klientovi Azure AD má více než jeden ověřených domén, které lze použít ke konfiguraci spojovacího bodu služby hello.

#### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení
* Zpětný zápis hesla je nyní k dispozici pro preview s Microsoft Azure Government cloudu a Německo cloudu Microsoftu. Další informace o podpoře Azure AD Connect pro instance hello různé služby, najdete v části tooarticle [Azure AD Connect: zvláštní upozornění pro instance](active-directory-aadconnect-instances.md).

* rutina Hello Initialize-ADSyncDomainJoinedComputerSync teď má nový volitelný parametr s názvem AzureADDomain. Tento parametr umožňuje určit, které slouží ke konfiguraci spojovacího bodu služby hello toobe domény ověřit.

### <a name="pass-through-authentication"></a>Předávací ověřování

#### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení
* Název Hello hello agenta požadované pro předávací ověřování byla změněna z hodnoty *Microsoft Azure AD Application Proxy Connector* příliš*agenta služby Microsoft Azure AD Connect ověřování*.

* Povolení ověřování průchozí už umožňuje synchronizaci hodnoty Hash hesla ve výchozím nastavení.


## <a name="115530"></a>1.1.553.0
Stav: Červen 2017

> [!IMPORTANT]
> Existují schéma a synchronizační pravidlo změny uváděné ve toto sestavení. Služba Azure AD Connect synchronizace aktivují úplný Import a úplnou synchronizaci kroků po upgradu. Podrobnosti o změnách hello jsou popsané níže. tootemporarily odložení úplný Import a úplnou synchronizaci kroků po upgradu, získáte tooarticle [jak toodefer úplné synchronizace po upgradu](active-directory-aadconnect-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade).
>
>

### <a name="azure-ad-connect-sync"></a>Synchronizace služby Azure AD Connect

#### <a name="known-issue"></a>Známý problém
* Existuje problém, který ovlivňuje zákazníky, kteří používají [filtrování založené na organizační jednotku](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) s synchronizace Azure AD Connect. Když přejdete toohello [stránka domény a filtrování organizační jednotky](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) v Průvodci hello Azure AD Connect, očekává se hello následující chování:
  * Pokud je povolené filtrování založené na organizační jednotku, hello **synchronizovat vybraných domén a organizačních jednotek** je vybraná možnost.
  * V opačném hello **synchronizovat všechny domén a organizačních jednotek** je vybraná možnost.

Hello problém, který nastane je tento hello **synchronizovat všechny domény a organizační jednotky možnost** je vždycky vybraná při spuštění Průvodce hello.  K tomu dojde i v případě, že na základě organizační jednotky filtrování dříve nakonfigurované. Před uložením změny konfigurace AAD Connect, ujistěte se, zda text hello **synchronizovat vybraných domén a organizačních jednotek možnost** a potvrďte, že jsou všechny organizační jednotky, které je třeba toosynchronize znovu povolena. Jinak založené na organizační jednotku filtrování bude zakázáno.

#### <a name="fixed-issues"></a>Opravené problémy

* Byl opraven problém s zpětný zápis hesla, která umožňuje Azure AD správce tooreset hello heslo místního AD privilegovaný účet uživatele. Hello problém nastane, když Azure AD Connect je uděleno oprávnění resetovat heslo hello přes hello privilegovaného účtu. Hello problému dochází v této verzi služby Azure AD Connect tím, že není Azure AD správce tooreset hello heslo libovolný místní AD privilegovaný účet uživatele, není-li správce hello hello vlastník tohoto účtu. Další informace najdete v části příliš[4033453 informační zpravodaj zabezpečení](https://technet.microsoft.com/library/security/4033453).

* Byl opraven problém související s toohello [msDS-ConsistencyGuid jako zdrojové ukotvení](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funkce, kde Azure AD Connect nemá zpětný zápis tooon místní atribut msDS-ConsistencyGuid AD. Hello problém nastane, když je více místní doménových struktur AD přidat tooAzure AD Connect a hello *identit uživatelů existovat napříč více adresářů možnost* je vybrána. Pokud je použita taková konfigurace, hello výsledné synchronizační pravidla není naplnění hello sourceAnchorBinary atributu v hello úložiště Metaverse. atribut sourceAnchorBinary Hello slouží jako hello zdrojový atribut pro atribut msDS-ConsistencyGuid. Zpětný zápis toohello ms-DSConsistencyGuid atribut se v důsledku toho nedochází. Následující pravidla synchronizace toofix hello problém, byly aktualizované tooensure, který hello sourceAnchorBinary atribut v hello vždy načteny Metaverse:
  * V ze služby Active Directory - InetOrgPerson AccountEnabled.xml
  * V ze služby Active Directory - InetOrgPerson Common.xml
  * V ze služby Active Directory - AccountEnabled.xml uživatele
  * V ze služby Active Directory - Common.xml uživatele
  * V ze služby Active Directory - uživatele připojit SOAInAAD.xml

* Dříve, i pokud hello [msDS-ConsistencyGuid jako zdrojové ukotvení](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funkce není povolena, hello "Out tooAD – ImmutableId uživatele" pravidlo synchronizace přidána tooAzure AD Connect. efekt Hello je neškodné a nezpůsobí zpětný zápis toooccur atribut msDS-ConsistencyGuid. tooavoid nedorozuměním byla přidána logika tooensure, který hello synchronizační pravidlo je přidat jenom, pokud je povolena funkce hello.

* Byl opraven problém způsobující toofail synchronizace hodnoty hash hesla s chybová událost 611. K tomuto problému dochází po jeden nebo více domény, které byly odebrány z řadiče místní AD. Na konci hello jednotlivých cyklů synchronizace hesla, hello synchronizačního souboru cookie vydala pomocí místní AD obsahuje ID vyvolání hello odebrat řadičů domény s USN (Update Sequence Number) na hodnotu 0. Hello Správce synchronizace hesel nelze toopersist synchronizace souboru cookie obsahující USN hodnota je 0 a nezdaří a zobrazí se chybová událost 611. Při další synchronizaci hello cyklus, hello Správce synchronizace hesel opětné hello poslední synchronizace trvalého souboru cookie, který neobsahuje USN hodnotu 0. To způsobí, že hello stejné změny hesla toobe znovu synchronizovat. Pomocí této opravy hello Správce synchronizace hesel správně trvá hello synchronizačního souboru cookie.

* Dříve, i v případě automatický Upgrade byl zakázán pomocí rutiny Set-ADSyncAutoUpgrade hello, hello automatický Upgrade proces pravidelně pokračuje toocheck pro upgrade a spoléhá na postižení toohonor hello stáhli instalační služby. S Tato oprava hello automatický Upgrade procesu už zkontroluje upgrade pravidelně. Oprava Hello automaticky použita při upgradu instalační program pro tuto verzi Azure AD Connect se spustí jednou.

#### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení

* Dříve, hello [msDS-ConsistencyGuid jako zdrojové ukotvení](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funkce byla k dispozici toonew pouze nasazení. Nyní je k dispozici tooexisting nasazení. A konkrétně:
  * tooaccess hello funkce, spusťte Průvodce hello Azure AD Connect a vyberte hello *aktualizace zdrojové ukotvení* možnost.
  * Tato možnost je jenom viditelné tooexisting nasazení, které používají objectGuid jako atribut sourceAnchor.
  * Při konfiguraci hello možnost, hello Průvodce ověří hello stav atribut msDS-ConsistencyGuid hello ve vaší místní službě Active Directory. Pokud atribut hello není nakonfigurováno uživatelských objektů v adresáři hello, použije průvodce hello hello msDS-ConsistencyGuid jako atribut sourceAnchor hello. Pokud atribut hello je nakonfigurovaný na jeden nebo více objektů uživatele v adresáři hello, ukončí hello průvodce atribut hello používá jiné aplikace a není vhodný jako atribut sourceAnchor a nepovoluje tooproceed změnu hello zdrojové ukotvení. Pokud jste si jisti, že tento atribut hello nepoužívá existující aplikace, musíte toocontact podporu pro informace o tom, jak toosuppress hello chyby.

* Konkrétní příliš**userCertificate** atribut na objekty zařízení, Azure AD Connect teď vypadá pro certifikáty hodnoty požadované pro [připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) a filtry se hello rest před synchronizací tooAzure AD. tooenable, které toto chování, pravidlo synchronizace out-of-box hello "Out tooAAD - zařízení připojit SOAInAD" byla aktualizována.

* Azure AD Connect nyní podporuje zpětný zápis Exchange Online **cloudPublicDelegates** atribut tooon místní AD **publicDelegates** atribut. To umožňuje hello scénář, kde poštovní schránku systému Exchange Online lze udělit práva toousers SendOnBehalfTo s místní poštovní schránka systému Exchange. toosupport tuto funkci, nové pravidlo synchronizace out-of-box "Out tooAD – zpětný zápis uživatelů Exchange hybridní PublicDelegates" byla přidána. Toto pravidlo synchronizace je přidat jenom tooAzure AD připojit, pokud je povolena funkce hybridní Exchange.

*   Azure AD Connect nyní podporuje synchronizaci hello **altRecipient** atributů z Azure AD. toosupport byla tato změna následující pravidla synchronizace out-of-box aktualizovat toku atributů tooinclude hello vyžaduje:
  * V ze služby Active Directory – Exchange uživatele
  * Out tooAAD – ExchangeOnline uživatele
  
* Hello **cloudSOAExchMailbox** atribut v hello úložiště Metaverse označuje, zda daný uživatel má poštovní schránka systému Exchange Online nebo ne. Jeho definice byl aktualizovaný tooinclude další RecipientDisplayTypes Exchange Online jako taková zařízení a konferenční místnosti poštovních schránek. tooenable tuto změnu definice hello hello cloudSOAExchMailbox atributu, který se nachází v části pravidla synchronizace out-of-box "V z AAD – uživatel Exchange hybridní", je aktualizovaná z:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

... toohello následující:

```
CBool(
  IIF(IsPresent([cloudMSExchRecipientDisplayType]),(
    IIF([cloudMSExchRecipientDisplayType]=0,True,(
      IIF([cloudMSExchRecipientDisplayType]=2,True,(
        IIF([cloudMSExchRecipientDisplayType]=7,True,(
          IIF([cloudMSExchRecipientDisplayType]=8,True,(
            IIF([cloudMSExchRecipientDisplayType]=10,True,(
              IIF([cloudMSExchRecipientDisplayType]=16,True,(
                IIF([cloudMSExchRecipientDisplayType]=17,True,(
                  IIF([cloudMSExchRecipientDisplayType]=18,True,(
                    IIF([cloudMSExchRecipientDisplayType]=1073741824,True,(
                       IF([cloudMSExchRecipientDisplayType]=1073741840,True,False)))))))))))))))))))),False))

```

* Nastavte následující přidané hello X509Certificate2 kompatibilní s funkcí pro vytváření hodnoty certifikát toohandle výrazy pravidla synchronizace v atributu userCertificate hello:

    ||||
    | --- | --- | --- |
    |CertSubject|CertIssuer|CertKeyAlgorithm|
    |CertSubjectNameDN|CertIssuerOid|CertNameInfo|
    |CertSubjectNameOid|CertIssuerDN|IsCert|
    |CertFriendlyName|CertThumbprint|CertExtensionOids|
    |CertFormat|CertNotAfter|CertPublicKeyOid|
    |CertSerialNumber|CertNotBefore|CertPublicKeyParametersOid|
    |CertVersion|CertSignatureAlgorithmOid|Vyberte|
    |CertKeyAlgorithmParams|CertHashString|kde|
    |||S|

* Následující změny schématu byly tooallow přináší zákazníkům toocreate vlastní synchronizace pravidla tooflow sAMAccountName, domainNetBios a domainFQDN pro objekty skupiny, a také distinguishedName pro uživatelské objekty:

  * Byly přidány následující atributy tooMV schématu:
    * Skupiny: název účtu
    * Skupiny: domainNetBios
    * Skupiny: domainFQDN
    * Uživatel: distinguishedName

  * Byly přidány následující atributy tooAzure schématu konektoru AD:
    * Skupiny: OnPremisesSamAccountName
    * Skupiny: název pro rozhraní NetBIOS
    * Skupiny: Název_domény_dns
    * Uživatel: OnPremisesDistinguishedName

* Hello ADSyncDomainJoinedComputerSync rutiny skript má nyní nový volitelný parametr s názvem AzureEnvironment. Parametr Hello je použité toospecify, které oblasti hello odpovídající klienta Azure Active Directory je hostován v. Platné hodnoty patří:
  * AzureCloud (výchozí)
  * AzureChinaCloud
  * AzureGermanyCloud
  * USGovernment
 
* Aktualizované Editor pravidla synchronizace toouse připojit (namísto zřídit) jako výchozí hodnota hello typu propojení během vytvoření pravidla synchronizace.

### <a name="ad-fs-management"></a>Správa služby AD FS

#### <a name="issues-fixed"></a>Chyby

* Následující adresy URL se nové koncové body služby WS-Federation zavedené službou Azure AD tooimprove odolnost proti výpadku ověřování a přidané tooon místní odeslání odpovědi konfigurace důvěryhodnosti strany služby AD FS:
  * https://ests.Login.microsoftonline.com/Login.srf
  * https://stamp2.Login.microsoftonline.com/Login.srf
  * https://CCS.Login.microsoftonline.com/Login.srf
  * https://CCS-sdf.Login.microsoftonline.com/Login.srf
  
* Byl opraven problém, která způsobila, že hodnota nesprávné deklarace identity služby AD FS toogenerate pro IssuerID. Hello problém nastane, pokud máte více ověřených domén v Azure AD hello klienta a příponu domény hello toogenerate používá atribut userPrincipalName hello hello IssuerID deklarace identity je minimálně 3 úrovně hloubkové (například johndoe@us.contoso.com). Hello problém se vyřeší při aktualizaci regex hello používá pravidla deklarace identity hello.

#### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení
* Dříve funkce správy certifikátů služby AD FS hello poskytované Azure AD Connect se použít jenom s farmy služby AD FS, které jsou spravované přes Azure AD Connect. Teď můžete použít funkci hello s farmy služby AD FS, které nejsou spravované přes Azure AD Connect.

## <a name="115240"></a>1.1.524.0
Vydáno: 2017 může

> [!IMPORTANT]
> Existují schéma a synchronizační pravidlo změny uváděné ve toto sestavení. Služba Azure AD Connect synchronizace aktivují úplný Import a úplnou synchronizaci kroků po upgradu. Podrobnosti o změnách hello jsou popsané níže.
>
>

**Opravené problémy:**

Synchronizace služby Azure AD Connect

* Opravit problém, který způsobuje toooccur automatický Upgrade na server Azure AD Connect hello i v případě, že zákazník hello funkci pomocí rutiny Set-ADSyncAutoUpgrade hello zakázal. Pomocí této opravy hello automatický Upgrade proces na serveru hello stále kontroluje pro upgrade pravidelně, ale hello stažený instalační program ctí konfigurace automatického upgradu hello.
* Během upgradu nástroje DirSync na místě vytvoří Azure AD Connect toobe účet služby Azure AD používané konektorem hello Azure AD pro synchronizaci se službou Azure AD. Po vytvoření účtu hello ověřuje Azure AD Connect s Azure AD pomocí účtu hello. V některých případech ověřování selže kvůli přechodným potížím, které způsobí, že toofail upgradu nástroje DirSync na místě s chybou *"došlo k chybě provádění úlohy konfigurace AAD Sync: AADSTS50034: toosign do této aplikace hello účtu je nutné přidat toohello xxx.onmicrosoft.com directory."* odolnost hello tooimprove upgradu nástroje DirSync, Azure AD Connect se opakuje teď krok ověřování hello.
* Byl problém se sestavením 443, který způsobuje toosucceed upgradu nástroje DirSync na místě, ale nejsou vytvořeny profilů spuštění, které jsou požadované pro synchronizaci adresářů. Opravy logiku je součástí tohoto sestavení Azure AD Connect. Když zákazník upgraduje toothis sestavení, Azure AD Connect zjistí chybí profilů spuštění a vytvoří je.
* Pevné problém způsobující toostart toofail proces synchronizace hesel s 6900 ID události a chyby *"Položku se stejným klíčem již byla přidána hello"*. K tomuto problému dochází v případě, že aktualizujete organizační jednotky filtrování konfigurace tooinclude AD konfigurační oddíl. Tento problém, synchronizace hesel se teď zpracovat toofix synchronizuje změny hesel z pouze oddíly domény AD. Oddíly mimo doménu jako konfigurační oddíl se přeskočí.
* Během instalace Express, vytvoří Azure AD Connect místní účet služby AD DS toobe používá toocommunicate hello AD connector s místním AD. Dříve je vytvořen účet hello s hello PASSWD_NOTREQD příznak nastavený na hello atribut řízení uživatelských účtů a náhodné heslo je nastaven na účtu hello. Nyní Azure AD Connect explicitně odebere příznak PASSWD_NOTREQD hello po hello heslo se nastavuje na účtu hello.
* Pevné problém způsobující toofail upgradu nástroje DirSync s chybou *"došlo k vzájemnému zablokování v systému sql server které snažíme tooacquire zámek aplikace"* když najde atribut mailNickname hello v hello místní schéma služby AD, ale není ohraničené toohello třída objektu uživatele AD.
* Opravené problém způsobující tooautomatically funkce zpětný zápis zařízení být zakázáno, pokud správce aktualizuje konfigurace Azure AD Connect sync pomocí Průvodce Azure AD Connect. To je způsobeno hello Průvodce provádění předběžné kontroly pro hello existující zařízení zpětný zápis konfigurace v místní službě AD a hello kontrola selže. Oprava Hello se kontrola tooskip hello, pokud zpětný zápis zařízení je již povolen dříve.
* tooconfigure filtrování organizační jednotku, můžete použít Průvodce Azure AD Connect hello nebo hello Synchronization Service Manager. Dříve, pokud použijete filtrování hello Azure AD Connect Průvodce tooconfigure organizační jednotky, nové organizační jednotky vytvořit později jsou zahrnuté pro synchronizaci adresářů. Pokud nechcete, aby nové toobe organizační jednotky, které jsou zahrnuty, je nutné nakonfigurovat organizační jednotku filtrování pomocí hello Synchronization Service Manager. Teď můžete dosáhnout hello stejné chování pomocí Průvodce Azure AD Connect.
* Opravit problém, který způsobuje uložené procedury vyžaduje Azure AD Connect toobe vytvořil v rámci hello schéma hello instalace správce, místo v rámci schématu dbo hello.
* Opravit problém, který způsobuje atribut TrackingId hello vrácený Azure AD toobe vynechání v hello připojit Server AAD protokoly událostí. k Hello problému dochází, pokud Azure AD Connect přijme zprávu o přesměrování z Azure AD a Azure AD Connect je zadaný koncový bod nemůže tooconnect toohello. Při řešení potíží s pracovníky technické podpory toocorrelate s protokoly na straně služby Hello TrackingId používá.
* Pokud Azure AD Connect obdrží LargeObject chyba z Azure AD, Azure AD Connect vygeneruje událost s ID události 6941 a zprávou *"hello zřízený objekt je příliš velký. Trim hello počet hodnot atributu na tento objekt."* V hello stejný čas, Azure AD Connect také generuje zavádějící událost s ID události 6900 a zpráva *"Microsoft.Online.Coexistence.ProvisionRetryException: nelze toocommunicate s hello Windows služby Azure Active Directory."* toominimize nejasnostem, Azure AD Connect už generuje hello pozdější událost v případě LargeObject chybě.
* Opravit problém, který způsobuje hello Synchronization Service Manager toobecome reagovat při pokusu o konfiguraci hello tooupdate pro generický konektor LDAP.

**Nové funkce nebo vylepšení:**

Synchronizace služby Azure AD Connect
* Synchronizovat změny pravidel – hello následující synchronizační pravidlo, že byly implementovány změny:
  * Pravidlo synchronizace aktualizované výchozí nastavení toonot export atributů **userCertificate** a **userSMIMECertificate** Pokud hello atributy mají více než 15 hodnoty.
  * Atributy AD **employeeID** a **msExchBypassModerationLink** jsou teď součástí sady pravidel synchronizace výchozí hello.
  * Atribut AD **fotografií** byl odebrán z výchozí sada pravidel synchronizace.
  * Přidat **preferredDataLocation** toohello úložiště Metaverse schéma a schématu na konektor AAD. Zákazníci, kteří chtějí tooupdate, které buď atributy ve službě Azure AD můžete implementovat vlastní tak synchronizovat toodo pravidla. toofind Další informace o atributu hello, najdete v části tooarticle [synchronizace Azure AD Connect: jak toomake toohello změnu výchozí konfigurace – povolení synchronizace PreferredDataLocation](active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-preferreddatalocation).
  * Přidat **userType** toohello úložiště Metaverse schéma a schématu na konektor AAD. Zákazníci, kteří chtějí tooupdate, které buď atributy ve službě Azure AD můžete implementovat vlastní tak synchronizovat toodo pravidla.

* Azure AD Connect nyní automaticky povolí hello použít atributu ConsistencyGuid jako hello zdrojové ukotvení atribut pro místní objekty služby AD. Další, Azure AD Connect naplní hello ConsistencyGuid atributu s hodnotou atributu objectGuid hello, pokud je prázdná. Tato funkce je toonew platí pouze pro nasazení. toofind Další informace o této funkci najdete v části tooarticle [Azure AD Connect: Principy – pomocí msDS-ConsistencyGuid jako sourceAnchor návrh](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).
* Nové řešení potíží s rutinu Invoke-ADSyncDiagnostics byl přidaný toohelp diagnostikovat synchronizaci hodnoty Hash hesla problémy související s. Informace o použití rutiny hello, najdete v části tooarticle [řešení synchronizace hesel s Azure AD Connect sync](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-synchronization).
* Azure AD Connect teď podporuje synchronizace Mail-Enabled veřejné složky objekty z místní AD tooAzure AD. Můžete povolit funkci hello pomocí Průvodce Azure AD Connect v části volitelné funkce. toofind Další informace o této funkci naleznete tooarticle [Office 365 adresáře na základě Edge blokování podporu pro místní e-mailu povolen veřejných složek](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders).
* Azure AD Connect vyžaduje toosynchronize účet služby AD DS z místní AD. Dříve Pokud jste nainstalovali Azure AD Connect pomocí režimu hello Express, je možné zadat hello pověření k účtu správce podniku a Azure AD Connect by vyžaduje účet hello služby AD DS. Ale pro vlastní instalaci a přidání doménových struktur tooan stávajícího nasazení, jste hello tooprovide vyžaduje účet služby AD DS místo. Nyní máte také možnost tooprovide hello hello pověření k účtu správce podniku během vlastní instalaci a umožnit vytvoření účtu služby AD DS hello vyžaduje Azure AD Connect.
* Azure AD Connect teď podporuje SQL AOA. Před instalací Azure AD Connect je nutné povolit SQL AOA. Při instalaci Azure AD Connect zjistí, zda zadaná instance SQL hello je povoleno pro SQL AOA, nebo ne. Pokud je povoleno SQL AOA, Azure AD Connect další hodnoty, pokud SQL AOA je nakonfigurované toouse replikace synchronní nebo asynchronní replikaci. Při nastavování hello naslouchací proces skupiny dostupnosti, se doporučuje, abyste nastavili too0 vlastnost RegisterAllProvidersIP hello. Je to proto, že Azure AD Connect v současné době používá tooSQL tooconnect Nativní klient systému SQL a SQL Native Client hello použití MultiSubNetFailover vlastnosti nepodporuje.
* Pokud používáte LocalDB jako hello databáze pro server Azure AD Connect a byl dosažen limit velikosti 10 GB, spustí se už hello synchronizační služba. Dříve potřebujete tooperform ShrinkDatabase operace na hello LocalDB tooreclaim dostatek volného místa databáze pro toostart hello synchronizační služba. Po kterém, můžete hello použijte Synchronization Service Manager toodelete spuštěním historie tooreclaim více místa na DB. Teď můžete použít toopurge rutiny Start-ADSyncPurgeRunHistory spustit data o historii z prostoru DB tooreclaim LocalDB. Další, tato rutina podporuje offline režim (zadáním hello - parametru offline) může být použit, když hello synchronizační služba není spuštěna. Poznámka: hello offline režimu lze pouze použijí, pokud hello synchronizační služba není spuštěná a databáze hello používá LocalDB.
* povinné, tooreduce hello množství prostoru úložiště Azure AD Connect sync podrobnosti o chybě teď komprimaci před jejich ukládání do databáze LocalDB nebo SQL. Při upgradu ze starší verze Azure AD Connect toothis verze Azure AD Connect provede jednorázovou komprese existující podrobnosti o chybě synchronizace.
* Dříve, po aktualizaci konfigurací filtrování organizační jednotky, můžete musí ručně spustit úplný import tooensure existující objekty jsou správně zahrnutý/vyloučený ze synchronizace adresářů. Nyní, Azure AD Connect automaticky aktivuje úplný import při příští synchronizaci hello cyklus. Další, úplný import pouze se použité toohello AD konektory hello aktualizace týká. Poznámka: tomuto vylepšení je použít tooOU filtrování aktualizace provedené pomocí pouze Průvodce hello Azure AD Connect. Není použitelné tooOU filtrování aktualizací, které jsou vytvořené pomocí hello Synchronization Service Manager.
* Dříve filtrování podle skupiny podporuje uživatelé, skupiny a obraťte se na pouze objekty. Teď na základě skupiny filtrování podporuje také objekty počítače.
* Dříve můžete odstranit data prostoru konektoru bez zakázání plánovače synchronizace Azure AD Connect. Nyní hello odstranění hello Synchronization Service Manager bloky dat prostoru konektoru pokud zjistí, že hello plánovač povolený. Upozornění další, je vrácena tooinform zákazníků týkající se potenciální ztrátě dat, v případě, že se odstraní hello dat prostoru konektoru.
* Dřív musíte zakázat přepis prostředí PowerShell pro Azure AD Connect Průvodce toorun správně. Tento problém je částečně vyřešený. Přepis prostředí PowerShell můžete povolit, pokud používáte konfiguraci synchronizace toomanage Průvodce Azure AD Connect. Pokud používáte konfiguraci služby AD FS toomanage Průvodce Azure AD Connect je nutné zakázat přepis prostředí PowerShell.



## <a name="114860"></a>1.1.486.0
Vydáno: Dubna 2017

**Opravené problémy:**
* Vyřešený problém hello, kde Azure AD Connect nelze úspěšně nainstalovat lokalizované verzi systému Windows Server.

## <a name="114840"></a>1.1.484.0
Vydáno: Dubna 2017

**Známé problémy:**

* Tato verze služby Azure AD Connect nebude úspěšně nainstalovat, pokud jsou splněny všechny následující podmínky hello:
   1. Provádíte buď DirSync místní upgrade nebo novou instalaci služby Azure AD Connect.
   2. Používáte lokalizované verzi systému Windows Server, kde není hello název předdefinované skupiny Administrators na serveru hello "Administrators".
   3. Používáte výchozí hello SQL Server 2012 Express LocalDB nainstalovaná službou Azure AD Connect místo vlastní úplné SQL.

**Opravené problémy:**

Synchronizace služby Azure AD Connect
* Opravit problém, kde plánovače synchronizace hello přeskočí krok celý synchronizace hello, pokud jeden nebo více konektorů chybí profil spuštění pro tento krok synchronizace. Například jste ručně přidali konektor bez vytvoření rozdílový Import spuštění profilu pro něj pomocí hello Synchronization Service Manager. Tato oprava zajišťuje, že plánovače synchronizace hello pokračuje toorun rozdílový Import pro ostatní konektory.
* Byl opraven problém hello synchronizační služba kde okamžitě ukončí, zpracování profil spuštění, když je zaznamená problém s jedním z kroků hello spustit. Tato oprava zajistí, že hello přeskočí synchronizační služby, které krok spustit a pokračuje tooprocess hello rest. Například máte rozdílový Import spuštění profilu pro AD connector s více kroků spuštění (jeden pro každou místní AD domény). Hello synchronizační služba se spustí rozdílový Import s hello jiných domén AD i v případě, že jeden z nich má problémy se síťovým připojením.
* Opravit problém, který způsobuje hello konektoru služby Azure AD aktualizace toobe přeskočeny při automatický Upgrade.
* Byl opraven problém s že příčiny Azure AD Connect tooincorrectly určit, zda hello server je řadič domény během instalace, které zapnout příčiny DirSync upgradu toofail.
* Opravené problém způsobující DirSync místní upgrade toonot vytvořit všechny spuštění profilu pro hello konektoru služby Azure AD.
* Opravit problém, kde hello Synchronization Service Manager uživatelské rozhraní přestane reagovat při pokusu o tooconfigure obecné konektor LDAP.

Správa služby AD FS
* Opravit problém, kde průvodce Azure AD Connect hello selže, pokud byl přesunutý tooanother server hello AD FS primárního uzlu.

Plochy jednotného přihlašování
* Opraven problém hello Průvodce Azure AD Connect, kde hello přihlašovací obrazovky neumožňuje povolit funkci jednotného přihlašování k ploše, pokud vyberete jako svoji možnost přihlášení během instalace nové synchronizace hesel.

**Nové funkce nebo vylepšení:**

Synchronizace služby Azure AD Connect
* Azure AD Connect Sync teď podporuje použití hello virtuální účet služby, účet spravované služby a skupinový účet spravované služby jako svůj účet služby. To platí toonew instalace služby Azure AD Connect jenom. Při instalaci Azure AD Connect:
    * Ve výchozím nastavení Průvodce Azure AD Connect se vytvoří virtuální účet služby a použije jako svůj účet služby.
    * Pokud instalujete na řadiči domény, přejde tooprevious chování, kde se vytvoří účet uživatele domény a místo toho použije jako svůj účet služby Azure AD Connect.
    * Hello výchozí chování můžete přepsat některým z následujících hello:
      * Skupinu účet spravované služby
      * Účet spravované služby
      * Účet uživatele domény
      * Místní uživatelský účet
* Dříve Pokud upgradujete tooa nové sestavení Azure AD Connect obsahující konektory aktualizovat nebo změny pravidel synchronizace, Azure AD Connect se aktivuje cyklus úplné synchronizace. Nyní Azure AD Connect selektivně aktivuje úplný Import kroky pouze pro konektory s aktualizací a úplnou synchronizaci pouze pro konektory s změny synchronizační pravidlo.
* Hello prahovou hodnotu odstranění exportu dříve, platí jenom tooexports, která se spustí pomocí plánovače synchronizace hello. Teď je hello funkce Rozšířené tooinclude exportuje ručně aktivovány hello zákazníka pomocí hello Synchronization Service Manager.
* V klientovi Azure AD je konfigurace služby, které označuje, zda je povolena funkce Synchronizace hesel pro vašeho klienta nebo ne. Dříve je snadné pro hello služby konfigurace toobe nesprávně nakonfigurována přes Azure AD Connect, pokud máte aktivní a na testovacím serveru. Nyní, Azure AD Connect se pokusí konfigurace služby tookeep hello konzistentní s vaší aktivní pouze server Azure AD Connect.
* Průvodce nyní zjistí a vrátí upozornění, pokud Azure AD Connect místní AD nemá Koš služby AD povolena.
* Dříve Export tooAzure AD vyprší časový limit a selže, pokud hello kombinaci velikost hello objektů ve službě hello batch překročí určité prahovou hodnotu. Hello synchronizační služby se nyní znovu tooresend hello objekty v dávkách samostatný, menší Pokud hello potíže.
* Odebrali jsme Hello Správa synchronizace služby klíč aplikace z nabídky Start v systému Windows. Správa šifrovací klíč bude toobe podporované prostřednictvím rozhraní příkazového řádku pomocí miiskmu.exe. Informace o správě šifrovací klíč, najdete v části tooarticle [Abandoning hello Azure AD Connect Sync šifrovací klíč](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-azure-ad-connect-sync-encryption-key).
* Dříve Pokud změníte heslo účtu služby synchronizace Azure AD Connect hello, hello synchronizační služba nebude možné spustit správně opuštění hello šifrovací klíč a heslo účtu služby synchronizace Azure AD Connect hello znovu inicializován. Nyní to se už nevyžaduje.

Plochy jednotného přihlašování

* Průvodce Azure AD Connect již nevyžaduje port 9090 toobe otevřít v síti hello při konfiguraci předávací ověřování a jednotné přihlašování plochy. Jenom port 443 je povinný. 

## <a name="114430"></a>1.1.443.0
Vydáno: 2017 března

**Opravené problémy:**

Synchronizace služby Azure AD Connect
* Byl opraven problém, což způsobí, že toofail Průvodce Azure AD Connect Pokud hello zobrazovaný název hello konektoru služby Azure AD neobsahuje hello počáteční onmicrosoft.com domény přiřazené toohello Azure AD klienta.
* Byl opraven problém, což způsobí, že Azure AD Connect toofail Průvodce při vytváření připojení tooSQL databáze při hello heslo hello účet synchronizační služby obsahuje speciální znaky, třeba apostrof, dvojtečkou a místa.
* Byl opraven problém, což způsobí, že chyba hello "hello dimage má element anchor, který se liší od obrázku hello" toooccur na serveru Azure AD Connect v pracovním režimu, po které jste dočasně vyloučili místní AD objekt ze synchronizace a poté ji znovu pro zahrnuté synchronizace.
* Byl opraven problém, což způsobí, že chyba hello "nalézt rozlišující název objektu hello je fiktivní" toooccur na serveru Azure AD Connect v pracovním režimu, po které jste dočasně vyloučili místní AD objekt ze synchronizace a poté ji znovu zahrnuté pro synchronizaci.

Správa služby AD FS
* Byl opraven problém kde průvodce Azure AD Connect není aktualizovat konfiguraci služby AD FS a nastavit hello správné deklarací na vztah důvěryhodnosti předávající strany po dokončení konfigurace alternativního přihlašovacího ID hello.
* Opravit problém, kde průvodce Azure AD Connect je nelze toocorrectly popisovač AD FS serverů služby účty nakonfigurované formátu userPrincipalName místo sAMAccountName formátu.

Předávací ověřování
* Opravit problém, který způsobuje toofail Průvodce Azure AD Connect, pokud je vybrána možnost předat prostřednictvím ověření, ale registrace jeho konektoru selže.
* Byl opraven problém, což způsobí, že Azure AD Connect Průvodce toobypass ověřovací kontroly na metoda přihlašování vybrána, když je povolena funkce jednotného přihlašování k ploše.

Resetování hesla
* Byl opraven problém, který může způsobit hello Azure AAD Connect server toonot pokus o toore-připojit, pokud byl ukončen hello připojení brány firewall nebo proxy serveru.

**Nové funkce nebo vylepšení:**

Synchronizace služby Azure AD Connect
* Rutina Get-ADSyncScheduler nyní vrátí novou vlastnost typu Boolean s názvem SyncCycleInProgress. Pokud hello vrátil hodnotu je nastavena hodnota true, to že znamená, že je cyklus plánované synchronizace v průběhu.
* Cílovou složku pro uložení instalace služby Azure AD Connect a protokoly instalace se přesunul z %localappdata%\AADConnect too%programdata%\AADConnect tooimprove usnadnění toohello soubory protokolu.

Správa služby AD FS
* Byla přidána podpora pro aktualizaci certifikát SSL služby AD FS farmy.
* Byla přidána podpora pro správu AD FS 2016.
* Nyní můžete určit existující gMSA (skupinový účet spravované služby) během instalace služby AD FS.
* Teď můžete konfigurovat SHA-256 jako algoritmus hash podpisu hello vztahu důvěryhodnosti předávající strany služby Azure AD.

Resetování hesla
* Přináší vylepšení tooallow hello produktu toofunction v prostředích s přísnější pravidla brány firewall.
* Vylepšené připojení spolehlivost tooAzure Service Bus.

## <a name="113800"></a>1.1.380.0
Vydáno: 2016 prosinec

**Opravené problému:**

* Toto sestavení chybí pevné hello problém, kde hello issuerid pravidel deklarace identity pro Active Directory Federation Services (AD FS).

>[!NOTE]
>Toto sestavení není k dispozici toocustomers pomocí funkce připojení automaticky upgradovat hello Azure AD.

## <a name="113710"></a>1.1.371.0
Vydáno: 2016 prosinec

**Známý problém:**

* Toto sestavení chybí Hello issuerid pravidlo deklarace identity služby AD FS. pravidlo deklarace identity issuerid Hello je vyžadován, pokud jsou federaci několika domén v Azure Active Directory (Azure AD). Pokud používáte Azure AD Connect toomanage místní nasazení služby AD FS, upgrade toothis sestavení odebere hello existující issuerid pravidlo deklarace identity z konfigurace služby AD FS. Hello problém můžete vyřešit přidáním pravidla deklarace identity issuerid hello po hello instalace nebo upgradu. Pro informace o přidávání hello issuerid pravidel deklarace identity, naleznete v článku toothis [podpora více domén pro federaci s Azure AD](active-directory-aadconnect-multiple-domains.md).

**Opravené problému:**

* Pokud není Port 9090 otevřený pro odchozí připojení hello, hello Azure AD Connect instalace nebo upgrade selže.

>[!NOTE]
>Toto sestavení není k dispozici toocustomers pomocí funkce připojení automaticky upgradovat hello Azure AD.

## <a name="113700"></a>1.1.370.0
Vydáno: 2016 prosinec

**Známé problémy:**

* Toto sestavení chybí Hello issuerid pravidlo deklarace identity služby AD FS. pravidlo deklarace identity issuerid Hello je vyžadován, pokud jsou federaci několika domén s Azure AD. Pokud používáte Azure AD Connect toomanage místní nasazení služby AD FS, upgrade toothis sestavení odebere hello existující issuerid pravidlo deklarace identity z konfigurace služby AD FS. Hello problém můžete vyřešit přidáním pravidla deklarace identity issuerid hello po instalaci nebo upgradu. Pro informace o přidávání issuerid pravidel deklarace identity, naleznete v článku toothis [podpora více domén pro federaci s Azure AD](active-directory-aadconnect-multiple-domains.md).
* Port 9090 musí být otevřené odchozí toocomplete instalace.

**Nové funkce:**

* Předávací ověřování (Preview).

>[!NOTE]
>Toto sestavení není k dispozici toocustomers pomocí funkce připojení automaticky upgradovat hello Azure AD.

## <a name="113430"></a>1.1.343.0
Vydáno: Listopadu 2016

**Známý problém:**

* Toto sestavení chybí Hello issuerid pravidlo deklarace identity služby AD FS. pravidlo deklarace identity issuerid Hello je vyžadován, pokud jsou federaci několika domén s Azure AD. Pokud používáte Azure AD Connect toomanage místní nasazení služby AD FS, upgrade toothis sestavení odebere hello existující issuerid pravidlo deklarace identity z konfigurace služby AD FS. Hello problém můžete vyřešit přidáním pravidla deklarace identity issuerid hello po instalaci nebo upgradu. Pro informace o přidávání issuerid pravidel deklarace identity, naleznete v článku toothis [podpora více domén pro federaci s Azure AD](active-directory-aadconnect-multiple-domains.md).

**Opravené problémy:**

* V některých případech instalace služby Azure AD Connect se nezdaří, protože je nelze toocreate účet místní služby, jehož heslo splňuje hello úroveň složitosti určeného hello organizace zásady hesel.
* Opraven problém, kde spojení nejsou vyhodnocovány když objekt v prostoru konektoru hello současně je mimo rozsah pro jedno připojení k pravidlo a stát v oboru pro jinou. To může nastat, když máte dva nebo více připojení pravidla, jejichž podmínky spojení se vzájemně vylučují.
* Opraven problém, kde nejsou pravidla synchronizace příchozích dat (ze služby Azure AD), která neobsahují pravidla spojení, zpracovávají, pokud mají nižší prioritu hodnoty než ty, které používají pravidla spojení.

**Vylepšení:**

* Byla přidána podpora pro instalaci Azure AD Connect na Windows Server 2016 Standard nebo vyšší.
* Byla přidána podpora pro použití SQL serveru 2016 jako hello vzdálené databáze pro Azure AD Connect.

## <a name="112810"></a>1.1.281.0
Vydáno: Srpna 2016

**Opravené problémy:**

* Interval změny toosync nepřebírají místní, dokud hello příštím synchronizačním cyklu po dokončení.
* Průvodce Azure AD Connect nepřijímá účet Azure AD, jejichž uživatelské jméno začíná podtržítkem (\_).
* Průvodce Azure AD Connect selže účet Azure AD tooauthenticate hello, pokud heslo účtu hello obsahuje příliš mnoho speciální znaky. Chybová zpráva "nelze toovalidate přihlašovací údaje. Došlo k neočekávané chybě." je vrácen.
* Odinstalace pracovní server zakáže synchronizace hesel v klientovi Azure AD a způsobí, že toofail synchronizace hesel s aktivní server.
* Synchronizace hesel selže v neobvyklých případech po žádné hodnoty hash hesla, který je uložený na hello uživatele.
* Když pracovní režim povolen server Azure AD Connect, není dočasně zakázáno zpětný zápis hesla.
* Průvodce Azure AD Connect nezobrazuje hello vlastní heslo synchronizace a konfigurace zpětný zápis hesla, pokud je server v pracovním režimu. Vždy zobrazuje je jako zakázané.
* Synchronizaci toopassword změny konfigurace a zpětný zápis hesla nejsou trvalé pomocí Průvodce Azure AD Connect, pokud je server v pracovním režimu.

**Vylepšení:**

* Aktualizovat hello ADSyncSyncCycle spuštění rutiny tooindicate toho, jestli je možné toosuccessfully počáteční cyklus nové synchronizace nebo ne.
* Přidání hello Stop-ADSyncSyncCycle rutiny tooterminate synchronizačním cyklu a operace, které jsou aktuálně probíhá.
* Aktualizované hello Stop-ADSyncScheduler rutiny tooterminate synchronizačním cyklu a operace, které jsou aktuálně probíhá.
* Při konfiguraci [rozšíření adresáře](active-directory-aadconnectsync-feature-directory-extensions.md) v průvodce Azure AD Connect, můžete nyní vybrat atribut typu "Teletex řetězec" hello Azure AD.

## <a name="111890"></a>1.1.189.0
Vydáno: Červen 2016

**Opravené problémy a vylepšení:**

* Azure AD Connect je nyní možné nainstalovat na serveru kompatibilní se standardem FIPS.
  * Synchronizace hesel, najdete v části [synchronizace hesel a FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips).
* Opravit problém, kde název NetBIOS nebylo možné přeložit toohello plně kvalifikovaný název domény v hello konektor služby Active Directory.

## <a name="111800"></a>1.1.180.0
Vydáno: Května 2016

**Nové funkce:**

* Varuje a umožňuje ověření domén, pokud nebylo ji provést před spuštěním Azure AD Connect.
* Přidání podpory pro [Microsoft Cloud Německo](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
* Přidání podpory pro hello nejnovější [cloudu Microsoft Azure Government](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) infrastruktury pomocí nové požadavky na adresu URL.

**Opravené problémy a vylepšení:**

* Přidané filtrování toohello Editor pravidla synchronizace toomake ho snadno toofind synchronizačního pravidla.
* Lepší výkon při odstraňování prostoru konektoru.
* Oprava problému při hello stejný objekt byla odstraněna i přidali v hello stejné spustit (volané odstranit nebo přidat).
* Zakázané synchronizační pravidlo už znovu povolí zahrnovala objekty a atributy na upgrade nebo adresáři schématu aktualizujte.

## <a name="111300"></a>1.1.130.0
Vydáno:. Dubna 2016

**Nové funkce:**

* Přidaná podpora pro více hodnot atributů příliš[rozšíření adresáře](active-directory-aadconnectsync-feature-directory-extensions.md).
* Přidání podpory pro další změny konfigurace pro [automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) toobe považuje za vhodné k upgradu.
* Přidat některé rutiny pro [vlastní plánovače](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Vydáno: Března 2016

**Opravené problémy:**

* Provedené se, že expresní instalace nelze použít v systému Windows Server 2008 (pre-R2), protože synchronizace hesla není podporována v tomto operačním systému.
* Upgrade z nástroje DirSync s konfigurací vlastního filtru nefunguje podle očekávání.
* Při upgradu tooa novější verze a neexistují žádné změny konfigurace toohello, nemá být naplánováno úplné import nebo synchronizaci.

## <a name="111100"></a>1.1.110.0
Vydáno: Leden 2016

**Opravené problémy:**

* Upgrade ze starších verzích nefunguje, pokud není hello instalace ve složce C:\Program Files výchozí hello.
* Pokud nainstalujete a vymazat **spustit proces synchronizace hello** na hello konci Průvodce instalací hello spuštěný Průvodce instalací hello podruhé neumožní hello plánovače.
* Hello scheduler nebude fungovat podle očekávání na serverech, kde hello formát data a času US en nepoužívají. Bude také blokovat `Get-ADSyncScheduler` tooreturn správné časy.
* Pokud jste nainstalovali dřívější verze služby Azure AD Connect se službou AD FS jako hello možnost přihlášení a upgradu, nelze znovu spustit Průvodce instalací hello.

## <a name="111050"></a>1.1.105.0
Vydáno: Leden 2016

**Nové funkce:**

* [Automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) funkce pro zákazníky Expresní nastavení.
* Podpora pro globálního správce hello pomocí ověřování Azure Multi-Factor Authentication a Privileged Identity Management v hello Průvodce instalací.
  * Je třeba tooallow vaše tooalso proxy povolit provoz toohttps://secure.aadcdn.microsoftonline-p.com Pokud používáte službu Multi-Factor Authentication.
  * Seznam důvěryhodných webů tooyour https://secure.aadcdn.microsoftonline-p.com tooadd potřebujete pro pracovní tooproperly služby Multi-Factor Authentication.
* Povolit, změna způsobu přihlášení hello uživatele po počáteční instalaci.
* Povolit [domény a organizační jednotky filtrování](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) v Průvodci instalací hello. To také umožňuje připojení tooforests, kde jsou k dispozici všechny domény.
* [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) je součástí toohello synchronizační modul.

**Funkce povýší z preview tooGA:**

* [Zpětný zápis zařízení](active-directory-aadconnect-feature-device-writeback.md).
* [Rozšíření adresáře](active-directory-aadconnectsync-feature-directory-extensions.md).

**Nové funkce verze preview:**

* Hello nový výchozí synchronizačním cyklu interval je 30 minut. Použít toobe tři hodiny pro všechny starší verze. Přidá podporu toochange hello [Plánovač](active-directory-aadconnectsync-feature-scheduler.md) chování.

**Opravené problémy:**

* Hello ověřte, zda stránka domén DNS vždycky nerozpoznal hello domén.
* Výzvy k zadání pověření správce domény při konfiguraci služby AD FS.
* Hello místního účty AD nerozpoznal Průvodce instalací hello, pokud se nachází v doméně s jiného stromu DNS než hello kořenové domény.

## <a name="1091310"></a>1.0.9131.0
Vydáno: Prosince 2015

**Opravené problémy:**

* Synchronizace hesla nemusí fungovat při změně hesla ve službě Active Directory Domain Services (AD DS), ale funguje, když jste nastavili heslo.
* Pokud máte proxy server, tooAzure ověřování, neúspěšného AD během instalace nebo upgradu je zrušena na konfigurační stránku hello.
* Aktualizace z předchozí verze služby Azure AD Connect s plná instance systému SQL Server selže, pokud si nejste správcem systému SQL Server (SA).
* Aktualizace z předchozí verze služby Azure AD Connect s vzdálený SQL Server, zobrazí se chyba "nelze tooaccess hello SQL databáze ADSync" hello.

## <a name="1091250"></a>1.0.9125.0
Vydáno: Listopad 2015

**Nové funkce:**

* Můžete změnit konfiguraci služby AD FS tooAzure AD důvěryhodnosti.
* Můžete aktualizovat schéma služby Active Directory hello a znovu vygenerovat synchronizačního pravidla.
* Můžete zakázat synchronizační pravidlo.
* Můžete definovat "AuthoritativeNull" jako nový literál v synchronizační pravidlo.

**Nové funkce verze preview:**

* [Azure AD Connect Health pro synchronizaci](../connect-health/active-directory-aadconnect-health-sync.md).
* Podpora pro [Azure AD Domain Services](../active-directory-passwords-update-your-own-password.md) synchronizace hesel.

**Nový podporovaný scénář:**

* Podporuje několik organizací místního Exchange. Další informace najdete v tématu [hybridní nasazení s více doménovými strukturami služby Active Directory](https://technet.microsoft.com/library/jj873754.aspx).

**Opravené problémy:**

* Problémy s synchronizací hesla:
  * Objekt přesunout z tooin oboru na více systémů oboru nebude mít jeho heslo synchronizovány. To zahrnuje organizační jednotky a filtrování atributů.
  * Výběr nového tooinclude organizační jednotky synchronizované nevyžaduje úplnou synchronizaci.
  * Pokud je povoleno zakázaný uživatel hello heslo není synchronizovaná.
  * fronty opakování heslo Hello je nekonečno a hello předchozí maximálně 5 000 objektů toobe vyřazeno byla odebrána.
* Nebylo možné tooconnect tooActive Directory s úrovní funkčnosti doménové struktury Windows Server 2016.
* Nebylo možné toochange hello skupiny, který se používá pro filtrování skupiny po počáteční instalaci hello.
* Už vytvoří nový uživatelský profil na server Azure AD Connect hello pro každého uživatele provádění změnu hesla s povolen zpětný zápis hesla.
* Nebylo možné toouse dlouhé celé číslo hodnoty synchronizované pravidla oborů.
* Hello zaškrtávací políčko "zpětný zápis zařízení" zakázáno, pokud jsou řadiče domény nedostupný.

## <a name="1086670"></a>1.0.8667.0
Vydáno: Srpen 2015

**Nové funkce:**

* Hello Azure AD Connect, Průvodce instalací je nyní lokalizované jazyky tooall Windows serveru.
* Přidaná podpora pro účet odemknout při použití správou hesel Azure AD.

**Opravené problémy:**

* Průvodce instalací služby Azure AD Connect dojde k chybě, pokud jiný uživatel pokračuje instalace spíše než hello osoba prvním spuštění instalace hello.
* Pokud se předchozí odinstalace služby Azure AD Connect nezdaří synchronizace Azure AD Connect toouninstall řádně, není možné tooreinstall.
* Nelze nainstalovat Azure AD Connect s použitím Expresní instalace, pokud není uživatel hello v hello kořenové doméně doménové struktury hello nebo pokud se používá jinou než anglickou verzi služby Active Directory.
* Pokud nelze přeložit hello plně kvalifikovaný název domény hello uživatelského účtu Active Directory, zobrazí se zavádějící chybová zpráva "Schématu se nezdařilo toocommit hello".
* Pokud hello účet použitý na hello konektor služby Active Directory se změnila mimo hello průvodce, Průvodce hello selže při dalším spuštění.
* Azure AD Connect se někdy nezdaří tooinstall na řadiči domény.
* Nelze povolit nebo zakázat "Pracovní režim", pokud nebyly přidané atributy rozšíření.
* Zpětný zápis hesla se v některých konfiguracích nezdaří z důvodu nesprávné heslo na hello konektor služby Active Directory.
* DirSync nejde upgradovat, je-li rozlišující název (DN) se používá v filtrování atributů.
* Při použití resetování hesla nadměrnému využití procesoru.

**Funkce odebrané preview:**

* funkce ve verzi preview Hello [zpětný zápis uživatelů](active-directory-aadconnect-feature-preview.md#user-writeback) dočasně odebral podle zpětnou vazbu od našich zákazníků preview. Přidá se později po jsme vyřešili hello poskytuje zpětnou vazbu.

## <a name="1086410"></a>1.0.8641.0
Vydáno: Červen 2015

**Počáteční verze služby Azure AD Connect.**

Název změněné z tooAzure Azure AD Sync AD Connect.

**Nové funkce:**

* [Expresní nastavení](active-directory-aadconnect-get-started-express.md) instalace
* Můžete [konfigurace služby AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
* Můžete [upgrade z nástroje DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md)
* [Prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
* Zavedly [pracovní režim](active-directory-aadconnectsync-operations.md#staging-mode)

**Nové funkce verze preview:**

* [Zpětný zápis uživatelů.](active-directory-aadconnect-feature-preview.md#user-writeback)
* [Zpětný zápis skupin](active-directory-aadconnect-feature-preview.md#group-writeback)
* [Zpětný zápis zařízení.](active-directory-aadconnect-feature-device-writeback.md)
* [Rozšíření adresáře](active-directory-aadconnect-feature-preview.md)

## <a name="104940501"></a>1.0.494.0501
Vydáno: Květen 2015

**Nový požadavek:**

* Azure AD Sync vyžaduje nyní hello rozhraní .NET Framework verze 4.5.1 toobe nainstalována.

**Opravené problémy:**

* Zpětný zápis hesla z Azure AD se nezdařilo s chybou připojení k Azure Service Bus.

## <a name="104910413"></a>1.0.491.0413
Vydáno: Duben 2015

**Opravené problémy a vylepšení:**

* Hello konektor služby Active Directory nezpracovává odstranění správně, pokud je povolená hello koše a je více domén v doménové struktuře hello.
* hello konektoru služby Azure Active Directory vylepšila výkon Hello operace importu.
* Když skupina překročila limit hello členství (ve výchozím nastavení, je nastavený hello limit too50 000 objektů), hello skupina byla odstraněna v Azure Active Directory. Hello novým chováním není hello skupinu odstranit, je vyvolána chyba a nové změny členství neexportují.
* Nový objekt nelze zřídit, pokud dvoufázové instalace odstranění s hello stejné rozlišující název je již k dispozici v prostoru konektoru hello.
* Některé objekty jsou označeny pro synchronizaci během synchronizace delta, i když se nezměnila, připraví se na objekt hello.
* Vynucení synchronizace hesla také odebere seznamu hello upřednostňovaný řadič domény.
* CSExportAnalyzer došlo k problémům se některé stavy objektů.

**Nové funkce:**

* Spojení se můžete připojit příliš "žádné" typ objektu v hello více hodnot.

## <a name="104850222"></a>1.0.485.0222
Vydáno: Únor 2015

**Vylepšení:**

* Import lepší výkon.

**Opravené problémy:**

* Synchronizace hesla ctí hello cloudFiltered atribut, který je používán filtrování atributů. Vyfiltrovaných objektů již nejsou v oboru pro synchronizaci hesel.
* Ve výjimečných případech, kde hello topologie měl mnoho řadičů domény synchronizaci hesel nefunguje.
* Bylo povoleno "Zastavena server" při importu z konektoru služby Azure AD hello po správu zařízení v Azure AD nebo Intune.
* Připojení cizí objekty zabezpečení (FSP) z několika domén ve stejné doménové struktuře způsobí chybu nejednoznačný spojení.

## <a name="104751202"></a>1.0.475.1202
Vydáno: Z prosince 2014

**Nové funkce:**

* Synchronizace hesel pomocí filtrování podle atributů se teď podporuje. Další informace najdete v tématu [synchronizace hesel s filtrování](active-directory-aadconnectsync-configure-filtering.md).
* atribut msDS-ExternalDirectoryObjectID Hello se nezapisují zpět tooActive adresáře. Tato funkce přidává podporu pro aplikace Office 365. Používá OAuth2 tooaccess poštovní schránky v hybridním nasazení Exchange Online a místně.

**Opravené upgradu problémy:**

* Novější verzi hello Pomocníka pro přihlášení je k dispozici na serveru hello.
* Vlastní cesta byla použité tooinstall Azure AD Sync.
* Neplatný vlastní spojení kritérium bloky hello upgrade.

**Ostatní opravy:**

* Opravené hello šablony Office Pro Plus.
* Problémy s pevnou instalace způsobené uživatelská jména, která začínat pomlčkou.
* Opravené ztráta hello sourceAnchor nastavení při spuštění Průvodce instalací hello ještě jednou.
* Opravené trasování událostí pro Windows pro synchronizaci hesel.

## <a name="104701023"></a>1.0.470.1023
Vydáno: Říjen 2014

**Nové funkce:**

* Synchronizace hesel z více místní služby Active Directory tooAzure AD.
* Lokalizované jazyky Windows serveru tooall instalace uživatelského rozhraní.

**Upgrade z GA nebude služba AADSync 1.0**

Pokud už máte Azure AD Sync nainstalovat, je další krok máte tootake v případě, že jste změnili žádné z pravidel synchronizace out-of-box hello. Po dokončení upgradu verze toohello 1.0.470.1023, jsou duplicitní hello synchronizační pravidla, které byly upraveny. Pro každé pravidlo upravené synchronizace hello následující:

1.  Vyhledejte pravidlo synchronizace hello byly upraveny a poznamenejte si hello změny.
* Odstraňte pravidlo synchronizace hello.
* Vyhledejte hello nové synchronizační pravidlo, které vytvoří Azure AD Sync a pak hello změny znovu použijte.

**Oprávnění pro hello účet služby Active Directory**

Hello účet služby Active Directory musí mít udělen další oprávnění toobe možné tooread hello hodnot hash hesel ze služby Active Directory. toogrant Hello oprávnění se s názvem "Replikovat změny adresáře" a "Replikujícím Directory změní všechny." Obě oprávnění jsou hodnot hash hesel požadované toobe možné tooread hello.

## <a name="104190911"></a>1.0.419.0911
Vydáno: Září 2014

**Počáteční verzi Azure AD Sync.**

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
