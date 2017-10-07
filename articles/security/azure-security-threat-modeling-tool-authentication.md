---
title: "aaaAuthentication - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 06c1b1aebab25e6fb5b666d24ecd9d86085d656c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-authentication--mitigations"></a>Zabezpečení rámce: Ověřování | Způsoby zmírnění rizik 
| Produktům a službám | Článek |
| --------------- | ------- |
| **Webové aplikace**    | <ul><li>[Zvažte použití standardní ověřovací mechanismus tooauthenticate tooWeb aplikace](#standard-authn-web-app)</li><li>[Aplikace musí bezpečně zpracovávají scénáře selhání ověřování](#handle-failed-authn)</li><li>[Povolit krok nahoru nebo Adaptivní ověřování](#step-up-adaptive-authn)</li><li>[Ujistěte se, že jsou správně uzamčené rozhraní pro správu](#admin-interface-lockdown)</li><li>[Implementace zapomněli jste heslo funkce bezpečně](#forgot-pword-fxn)</li><li>[Uplatňování zásady hesla a účtu](#pword-account-policy)</li><li>[Implementujte ovládací prvky tooprevent username – výčet](#controls-username-enum)</li></ul> |
| **Database** | <ul><li>[Pokud je to možné, použijte ověřování systému Windows pro připojení tooSQL serveru](#win-authn-sql)</li><li>[Pokud je to možné používejte ověřování Azure Active Directory pro tooSQL připojení databáze](#aad-authn-sql)</li><li>[Když se používá režim ověřování systému SQL, ujistěte se, že účet a heslo, zásady se vynucují v systému SQL server](#authn-account-pword)</li><li>[Nepoužívejte v databázích s omezením ověřování SQL.](#autn-contained-db)</li></ul> |
| **Centra událostí Azure** | <ul><li>[Podle zařízení ověřování přihlašovacích údajů pomocí tokeny SaS](#authn-sas-tokens)</li></ul> |
| **Hranice vztahů důvěryhodnosti Azure** | <ul><li>[Povolení Azure Multi-Factor Authentication pro Azure správce](#multi-factor-azure-admin)</li></ul> |
| **Hranice vztahů důvěryhodnosti Service Fabric** | <ul><li>[Omezit anonymní přístup tooService Fabric Cluster](#anon-access-cluster)</li><li>[Zajistěte, aby byl certifikát klienta uzlu Service Fabric liší od certifikátu – uzly](#fabric-cn-nn)</li><li>[Použít clustery infrastruktury tooservice klienti tooauthenticate AAD](#aad-client-fabric)</li><li>[Ujistěte se, že certifikáty infrastruktury služby jsou získány z schválené certifikační autoritou (CA)](#fabric-cert-ca)</li></ul> |
| **Serveru identit** | <ul><li>[Použijte standardní ověřování scénáře nepodporuje serveru identit](#standard-authn-id)</li><li>[Přepsání serveru identit výchozí hello škálovatelné alternativou tokenu mezipaměti.](#override-token)</li></ul> |
| **Počítač hranice vztahů důvěryhodnosti** | <ul><li>[Ujistěte se, že binární soubory nasazené aplikace jsou digitálně podepsané.](#binaries-signed)</li></ul> |
| **WCF** | <ul><li>[Povolit ověřování při připojování tooMSMQ fronty ve WCF](#msmq-queues)</li><li>[Není nastavena toonone clientCredentialType zpráv WCF](#message-none)</li><li>[Není nastavena toonone clientCredentialType přenosu WCF](#transport-none)</li></ul> |
| **Webové rozhraní API** | <ul><li>[Zajistěte, aby techniky standardní ověřování použité toosecure webová rozhraní API](#authn-secure-api)</li></ul> |
| **Azure AD** | <ul><li>[Použijte standardní ověřování scénáře podporované službou Azure Active Directory](#authn-aad)</li><li>[Přepsání hello výchozí ADAL mezipamětí tokenů škálovatelné alternativou](#adal-scalable)</li><li>[Zajistěte, aby TokenReplayCache opětovného přehrání hello použité tooprevent tokenů ověřování ADAL](#tokenreplaycache-adal)</li><li>[Použití knihovny ADAL toomanage token požadavky od klientů tooAAD OAuth2 (nebo místní AD)](#adal-oauth2)</li></ul> |
| **Brána pole IoT** | <ul><li>[Ověření zařízení připojující se toohello brána pole](#authn-devices-field)</li></ul> |
| **Brána IoT cloudu** | <ul><li>[Ujistěte se, že se ověří zařízení připojující se tooCloud brány](#authn-devices-cloud)</li><li>[Použít pověření ověřování podle zařízení](#authn-cred)</li></ul> |
| **Azure Storage** | <ul><li>[Ujistěte se, že tento pouze hello požadované kontejnery a objekty BLOB jsou uvedeny anonymní přístup pro čtení](#req-containers-anon)</li><li>[Udělit omezený přístup tooobjects v úložišti Azure pomocí SAS nebo SAP](#limited-access-sas)</li></ul> |

## <a id="standard-authn-web-app"></a>Zvažte použití standardní ověřovací mechanismus tooauthenticate tooWeb aplikace

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| Podrobnosti | <p>Ověřování je proces hello prokáže, kde entity svou identitu, obvykle prostřednictvím přihlašovacích údajů, jako je například uživatelské jméno a heslo. Existuje více ověřovací protokoly dostupné kterých lze považovat za. Některé z nich, jsou uvedeny níže:</p><ul><li>Klientské certifikáty</li><li>Na základě systému Windows</li><li>Na základě formulářů</li><li>Federační - služby AD FS</li><li>Federační – Azure AD</li><li>Federační - serveru identit</li></ul><p>Zvažte použití standardní ověřovací mechanismus tooidentify hello zdroje procesu</p>|

## <a id="handle-failed-authn"></a>Aplikace musí bezpečně zpracovávají scénáře selhání ověřování

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| Podrobnosti | <p>Aplikace, které explicitně ověřování uživatelů musí zpracovávat scénáře neúspěšné ověřování, které musí securely.hello ověřovací mechanismus:</p><ul><li>Odepřít přístup tooprivileged prostředky, pokud se ověření nezdaří</li><li>Zobrazit obecnou chybovou zprávu po selhání ověření a dojde k odepření přístupu</li></ul><p>Test pro:</p><ul><li>Ochranu privilegovaných prostředků po neúspěšných přihlášení</li><li>Zobrazí se obecnou chybovou zprávu o selhání ověřování a přístup odepřen událostí</li><li>Účty jsou zakázány po nadměrný počet neúspěšných pokusů o přihlášení</li><ul>|

## <a id="step-up-adaptive-authn"></a>Povolit krok nahoru nebo Adaptivní ověřování

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| Podrobnosti | <p>Ověřte hello aplikace má další autorizace (například krok nebo Adaptivní ověřování pomocí služby Multi-Factor authentication, například odeslání jednorázového HESLA v serveru SMS, e-mailu atd. nebo dotaz na opětovné ověření), je před udělením přístupu postiženy hello uživatele toosensitive informace. Toto pravidlo platí také pro provádění důležité změny tooan účet nebo akce</p><p>Také to znamená, že přizpůsobení hello ověřování má toobe implementované v například, takovým způsobem, který aplikace hello správně vynucuje kontextová autorizace tak jako toonot povolit neoprávněné manipulaci prostřednictvím v příkladu manipulaci parametr</p>|

## <a id="admin-interface-lockdown"></a>Ujistěte se, že jsou správně uzamčené rozhraní pro správu

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| Podrobnosti | první řešení Hello je toogrant přístup jenom určité zdrojové IP rozsah toohello rozhraní pro správu. Pokud toto řešení nebude možné, než je vždy doporučujeme tooenforce přechody nebo Adaptivní ověřování pro přihlášení do rozhraní pro správu hello |

## <a id="forgot-pword-fxn"></a>Implementace zapomněli jste heslo funkce bezpečně

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| Podrobnosti | <p>Hello je nejdřív thing tooverify, který zapomněli jste, že heslo a jiných cest obnovení odeslat odkaz včetně časově omezené aktivace token spíše než hello heslo sám sebe. Další ověřování podle konfigurace soft tokeny (například SMS tokenu, nativní mobilní aplikace atd.) může být nutná také před odesláním odkazu hello přes. Druhý, můžete nesmí hello uživatelé účet uzamčen a zároveň probíhá proces hello nové heslo.</p><p>To může vést tooa útok vždy, když útočník rozhodne toointentionally uzamčení hello uživatelé s automatizované útoku. Třetí vždy, když hello novou žádost o heslo bylo nastaveno v průběhu, uvítací zprávu, kterou je zobrazit by měl být zobecněn ve výčtu uživatelské jméno tooprevent pořadí. Čtvrté vždy zakáže použití hello staré hesel a implementovat zásady silné heslo.</p> |

## <a id="pword-account-policy"></a>Uplatňování zásady hesla a účtu

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| Podrobnosti | <p>Zásady účet a heslo v souladu s zásad organizace a osvědčené postupy by měla být implementována.</p><p>toodefend před hrubou silou a slovník na základě uhodnutí: zásady silné heslo musí být implementovaná tooensure, že uživatelé vytvářet složité heslo (např. minimální délka 12 znaků, alfanumerické a speciální znaky).</p><p>Zásady uzamčení účtu mohou být prováděny v hello následujícím způsobem:</p><ul><li>**Logicky uzamčení:** to může být vhodný pro ochranu před útoky hrubou silou vaši uživatelé. Například pokaždé, když uživatel hello zadá chybné heslo aplikace hello třikrát může zamknout hello účet pro několik minut v pořadí tooslow dolů hello proces hrubou vynucení jeho hesla. Díky tomu méně ziskové pro tooproceed útočník hello. Pokud byste byli tooimplement pevný opatření na více systémů zámek pro tento příklad by dosáhnout a "Dos" podle trvale uzamykání účtů. Alternativně může aplikace generovat Jednorázovým (jedno heslo času) a odešle out-of-band (prostřednictvím e-mailu, sms atd.) toohello uživatele. Jiná možnost může být tooimplement CAPTCHA, po dosažení prahová hodnota počtu neúspěšných pokusů o přihlášení.</li><li>**Pevné uzamčení:** tento typ uzamčení bude použito, vždy, když zjistit uživatele napadení vaší aplikace a čítač mu prostřednictvím trvale uzamčení účtu, dokud tým odpovědi měl toodo čas jejich forenzních. Po tento proces můžete rozhodnout toogive hello uživatele zpět svůj účet nebo proveďte další právní proti ní. Tento typ přístupu hello útočník zabrání další narušující vaše aplikace a infrastrukturu.</li></ul><p>toodefend proti útokům na výchozí a předvídatelný účtů, ověřte, zda všechny klíče a hesla jsou nahraditelné a jsou generované nebo změní za čas instalace.</p><p>Pokud má aplikace hello tooauto-generování hesel, zajistěte, aby hello generované hesla jsou náhodných a mají vysokou šifrování.</p>|

## <a id="controls-username-enum"></a>Implementujte ovládací prvky tooprevent username – výčet

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Všechny chybové zprávy musí být zobecněn ve výčtu uživatelské jméno tooprevent pořadí. Také v některých případech se nelze vyhnout informace vracena v odkudkoli třeba na stránce registrace. Zde je třeba toouse omezení rychlosti metody jako CAPTCHA tooprevent útoku automatizované uživatelem se zlými úmysly. |

## <a id="win-authn-sql"></a>Pokud je to možné, použijte ověřování systému Windows pro připojení tooSQL serveru

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Místní |
| **Atributy**              | Verze SQL – všechny |
| **Odkazy**              | [SQL Server – volba režimu ověřování](https://msdn.microsoft.com/library/ms144284.aspx) |
| **Kroky** | Ověřování systému Windows používá bezpečnostní protokol Kerberos, poskytuje vynucení zásad hesel s ohledem toocomplexity ověřování pro silná hesla, poskytuje podporu pro uzamčení účtu a vypršení platnosti hesla.|

## <a id="aad-authn-sql"></a>Pokud je to možné používejte ověřování Azure Active Directory pro tooSQL připojení databáze

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | SQL Azure |
| **Atributy**              | Verze SQL - V12 |
| **Odkazy**              | [Připojení tooSQL ověřování databáze s pomocí pomocí Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/) |
| **Kroky** | **Minimální verze:** Azure SQL Database V12 požadované tooallow Azure SQL Database toouse AAD ověřování proti hello Microsoft Directory |

## <a id="authn-account-pword"></a>Když se používá režim ověřování systému SQL, ujistěte se, že účet a heslo, zásady se vynucují v systému SQL server

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Zásady hesel systému SQL Server](https://technet.microsoft.com/library/ms161959(v=sql.110).aspx) |
| **Kroky** | Pokud používáte ověřování systému SQL Server, přihlášení se vytvoří v systému SQL Server, které nejsou založené na uživatelské účty systému Windows. Hello uživatelské jméno a heslo hello jsou vytvořené pomocí serveru SQL Server a uložené v systému SQL Server. SQL Server můžete použít mechanismy zásady hesel systému Windows. Použije hello použít stejné zásady složitost a vypršení platnosti v systému Windows toopasswords použit v rámci systému SQL Server. |

## <a id="autn-contained-db"></a>Nepoužívejte v databázích s omezením ověřování SQL.

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | OnPrem, SQL Azure |
| **Atributy**              | Verze - MSSQL2012, verze SQL - SQL verze 12 |
| **Odkazy**              | [Osvědčené postupy zabezpečení pomocí databáze s omezením](http://msdn.microsoft.com/library/ff929055.aspx) |
| **Kroky** | Hello absenci zásadu vynucené heslo může zvýšit pravděpodobnost hello slabé pověření zavedeno v databázi s omezením. Ověřování systému Windows využívají. |

## <a id="authn-sas-tokens"></a>Podle zařízení ověřování přihlašovacích údajů pomocí tokeny SaS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Centra událostí Azure | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Ověřování a zabezpečení modelu přehled služby Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Kroky** | <p>model zabezpečení služby Event Hubs Hello je založena na kombinaci tokenů sdíleného přístupového podpisu (SAS) a zdroje událostí. Název vydavatele Hello představuje hello DeviceID, která přijímá hello token. To by pomoci přidružit hello tokeny vygeneroval s hello příslušné zařízení.</p><p>Všechny zprávy jsou označené původce na straně služby povolení detekce falšování pokusy o původu ve datové části. Při ověřování zařízení, generovat na zařízení SaS token vymezená tooa jedinečný vydavatele.</p>|

## <a id="multi-factor-azure-admin"></a>Povolení Azure Multi-Factor Authentication pro Azure správce

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Hranice vztahů důvěryhodnosti Azure | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Co je Azure Multi-Factor Authentication?](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) |
| **Kroky** | <p>Vícefaktorové ověřování (MFA) je metoda ověřování, který vyžaduje více než jednu metodu ověřování a přidá velmi důležitou druhou vrstvu zabezpečení toouser přihlášení a transakce. Funguje tím, že jakékoliv dva nebo více hello následující metody ověření:</p><ul><li>Něco znáte (obvykle heslo)</li><li>Něco co uživatel má (důvěryhodné zařízení, která není duplikovaná snadno, například telefon)</li><li>Něco že se (biometrika)</li><ul>|

## <a id="anon-access-cluster"></a>Omezit anonymní přístup tooService Fabric Cluster

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Hranice vztahů důvěryhodnosti Service Fabric | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Prostředí – Azure  |
| **Odkazy**              | [Scénáře zabezpečení clusteru Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security) |
| **Kroky** | <p>Clustery musí být vždy zabezpečené tooprevent neoprávněného uživatele z připojení clusteru tooyour, zejména v případě, že je v něm spuštěny úlohy v produkčním prostředí.</p><p>Při vytváření service fabric cluster, ujistěte se, že tento režim zabezpečení hello je nastaven příliš "zabezpečené" a nakonfigurujte certifikát serveru hello požadované X.509. Vytváření clusteru služby "nezabezpečené" vám umožní žádné tooit tooconnect anonymního uživatele, pokud vystavuje toohello koncové body správy veřejného Internetu.</p>|

## <a id="fabric-cn-nn"></a>Zajistěte, aby byl certifikát klienta uzlu Service Fabric liší od certifikátu – uzly

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Hranice vztahů důvěryhodnosti Service Fabric | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Prostředí – Azure, prostředí – samostatný |
| **Odkazy**              | [Zabezpečení certificate Service Fabric klientský uzel](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#_client-to-node-certificate-security), [Connect tooa zabezpečení clusteru pomocí klientského certifikátu](https://azure.microsoft.com/documentation/articles/service-fabric-connect-to-secure-cluster/) |
| **Kroky** | <p>Při vytváření clusteru hello buď prostřednictvím hello portálu Azure, šablony Resource Manageru nebo šablonu JSON samostatné zadáním certifikát klienta správce nebo klientský certifikát uživatele konfigurace uzlu klientů certifikát zabezpečení.</p><p>Hello Správce klientů a uživatelů klientské certifikáty, které zadáte musí být jiný než hello primární a sekundární certifikáty, které zadáte pro zabezpečení mezi uzly.</p>|

## <a id="aad-client-fabric"></a>Použít clustery infrastruktury tooservice klienti tooauthenticate AAD

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Hranice vztahů důvěryhodnosti Service Fabric | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Prostředí – Azure |
| **Odkazy**              | [Cluster scénáře zabezpečení - doporučení zabezpečení](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#security-recommendations) |
| **Kroky** | Clustery se systémem na platformě Azure také můžete zabezpečit přístup koncových bodů pro správu toohello pomocí Azure Active Directory (AAD), kromě klientských certifikátů. Pro Azure clusterů se doporučuje použít AAD zabezpečení tooauthenticate klientů a certifikáty pro zabezpečení mezi uzly.|

## <a id="fabric-cert-ca"></a>Ujistěte se, že certifikáty infrastruktury služby jsou získány z schválené certifikační autoritou (CA)

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Hranice vztahů důvěryhodnosti Service Fabric | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Prostředí – Azure |
| **Odkazy**              | [X.509 – certifikáty a Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#x509-certificates-and-service-fabric) |
| **Kroky** | <p>Service Fabric používá certifikáty X.509 serveru za účelem ověřování totožnosti uzly a klienty.</p><p>Některé důležité věci tooconsider při používání certifikátů v service Fabric:</p><ul><li>Certifikáty používané v clusterech spuštění úlohy v produkčním prostředí by měla být vytvořen pomocí správně nakonfigurovaných certifikát služby Windows Server nebo získat ze schválené certifikační autoritou (CA). Hello certifikační Autority může být schválené externí certifikační Autority nebo správně spravované interní infrastruktury veřejných klíčů (PKI)</li><li>Nikdy nepoužívejte žádné dočasných nebo testovací certifikáty v produkčním prostředí, které jsou vytvořeny pomocí nástrojů, jako je MakeCert.exe</li><li>Můžete použít certifikát podepsaný svým držitelem, ale měli tak učinit pouze pro testovací clustery a ne v produkčním prostředí</li></ul>|

## <a id="standard-authn-id"></a>Použijte standardní ověřování scénáře nepodporuje serveru identit

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Serveru identit | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [IdentityServer3 - hello velký obrázek](https://identityserver.github.io/Documentation/docsv2/overview/bigPicture.html) |
| **Kroky** | <p>Níže jsou hello typické interakce nepodporuje serveru identit:</p><ul><li>Prohlížeče komunikovat s webovými aplikacemi</li><li>Webové aplikace komunikovat s webových rozhraní API (někdy na své vlastní, někdy jménem uživatele)</li><li>Aplikace založené na prohlížeči komunikovat s webových rozhraní API</li><li>Nativní aplikace komunikovat s webových rozhraní API</li><li>Serverové aplikace komunikovat s webových rozhraní API</li><li>Webová rozhraní API komunikovat s webových rozhraní API (někdy na své vlastní, někdy jménem uživatele)</li></ul>|

## <a id="override-token"></a>Přepsání serveru identit výchozí hello škálovatelné alternativou tokenu mezipaměti.

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Serveru identit | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Nasazení serveru identity - ukládání do mezipaměti](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Kroky** | <p>IdentityServer má jednoduché předdefinované mezipaměti v paměti. To je vhodné pro nativní aplikace v menším měřítku, není vhodné pro mid vrstvy a back-end aplikace hello následujících důvodů:</p><ul><li>Tyto aplikace jsou dostupné přes mnoho uživatelů najednou. Ukládání všech přístupových tokenů v hello stejné úložiště, vytvoří izolace problémů a představuje výzvy při fungování ve velkém měřítku: mnoho uživatelů, každý s tolik tokeny jako prostředky hello hello přistupuje aplikace jejich jménem, může znamenat velký čísla a velmi náročná vyhledávání operace</li><li>Tyto aplikace se obvykle nasazují na Distribuovaná topologie, kde více uzlů musí mít přístup toohello stejné mezipaměti</li><li>V mezipaměti tokeny musí překonat proces recykluje a deaktivací</li><li>Pro všechny hello výše důvodů při implementaci webové aplikace se doporučuje toooverride hello výchozí Identity tokenu mezipaměti serveru škálovatelné alternativou například Azure Redis cache</li></ul>|

## <a id="binaries-signed"></a>Ujistěte se, že binární soubory nasazené aplikace jsou digitálně podepsané.

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Počítač hranice vztahů důvěryhodnosti | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Ujistěte se, že binární soubory nasazené aplikace jsou digitálně podepsané, tak, aby lze ověřit integritu hello hello binárních souborů|

## <a id="msmq-queues"></a>Povolit ověřování při připojování tooMSMQ fronty ve WCF

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, NET Framework 3 |
| **Atributy**              | Není k dispozici |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx) |
| **Kroky** | Program selže tooenable ověřování při připojování tooMSMQ fronty, útočník může odeslat anonymně toohello fronty zpráv pro zpracování. Pokud ověření není použité tooconnect tooan MSMQ fronty použít toodeliver program tooanother zpráv, útočník může odeslat zprávu anonymní, který je škodlivý.|

### <a name="example"></a>Příklad
Hello `<netMsmqBinding/>` prvek hello WCF konfiguračního souboru níže dá pokyn WCF toodisable ověřování při připojování frontu MSMQ tooan pro doručování zpráv.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""None"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```
Konfigurace služby MSMQ toorequire domény systému Windows nebo ověření certifikátu po celou dobu pro všechny příchozí nebo odchozí zprávy.

### <a name="example"></a>Příklad
Hello `<netMsmqBinding/>` prvek hello WCF konfiguračního souboru níže dá pokyn WCF tooenable certifikát ověřování při připojování frontu MSMQ tooan. Hello klienta je ověřen pomocí certifikátů X.509. Hello klientský certifikát musí být v úložišti certifikátů hello hello serveru existuje.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""Certificate"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```

## <a id="message-none"></a>Není nastavena toonone clientCredentialType zpráv WCF

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Rozhraní .NET framework 3 |
| **Atributy**              | Typ pověření klienta - None |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | Hello absenci ověřování znamená všem uživatelům je možné tooaccess této služby. Služba, která neověřuje svým klientům umožňuje přístup uživatelům tooall. Nakonfigurujte tooauthenticate aplikace hello proti pověření klienta. Tento krok můžete provést nastavením hello zpráva clientCredentialType tooWindows nebo certifikát. |

### <a name="example"></a>Příklad
```
<message clientCredentialType=""Certificate""/>
```

## <a id="transport-none"></a>Není nastavena toonone clientCredentialType přenosu WCF

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecná rozhraní .NET Framework 3 |
| **Atributy**              | Typ pověření klienta - None |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | Hello absenci ověřování znamená všem uživatelům je možné tooaccess této služby. Služba, která neověřuje svým klientům umožňuje všechny uživatele tooaccess její funkce. Nakonfigurujte tooauthenticate aplikace hello proti pověření klienta. Tento krok můžete provést nastavením hello přenosu clientCredentialType tooWindows nebo certifikát. |

### <a name="example"></a>Příklad
```
<transport clientCredentialType=""Certificate""/>
```

## <a id="authn-secure-api"></a>Zajistěte, aby techniky standardní ověřování použité toosecure webová rozhraní API

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Ověřování a autorizace v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api), [externí ověřovací služby s rozhraním ASP.NET Web API (C#)](http://www.asp.net/web-api/overview/security/external-authentication-services) |
| **Kroky** | <p>Ověřování je proces hello prokáže, kde entity svou identitu, obvykle prostřednictvím přihlašovacích údajů, jako je například uživatelské jméno a heslo. Existuje více ověřovací protokoly dostupné kterých lze považovat za. Některé z nich, jsou uvedeny níže:</p><ul><li>Klientské certifikáty</li><li>Na základě systému Windows</li><li>Na základě formulářů</li><li>Federační - služby AD FS</li><li>Federační – Azure AD</li><li>Federační - serveru identit</li></ul><p>Odkazy v části odkazy hello poskytují nízké úrovně podrobnosti o jednotlivých hello schémat ověřování, jak mohou být implementována toosecure webového rozhraní API.</p>|

## <a id="authn-aad"></a>Použijte standardní ověřování scénáře podporované službou Azure Active Directory

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure AD | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Scénáře ověřování pro Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/), [Azure Active Directory ukázky kódu](https://azure.microsoft.com/documentation/articles/active-directory-code-samples/), [Příručka pro vývojáře Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-developers-guide/) |
| **Kroky** | <p>Azure Active Directory (Azure AD) zjednodušuje ověřování pro vývojáře a poskytovat identity jako služby, podpora pro standardní protokoly, jako je například OAuth 2.0 a OpenID Connect. V následující tabulce jsou hello pět scénáře primární aplikace podporované službou Azure AD:</p><ul><li>Webové prohlížeče tooWeb aplikace: uživatel potřebuje toosign ve tooa webové aplikaci, která je zabezpečená službou Azure AD</li><li>Jediné stránce aplikace (SPA): Uživatel potřebuje toosign tooa jednostránkové aplikace, která je zabezpečená službou Azure AD</li><li>Nativní aplikace tooWeb rozhraní API: nativní aplikaci spuštěnou v telefonu, tabletu nebo tooauthenticate musí počítač uživatele tooget prostředky z webového rozhraní API, která je zabezpečená službou Azure AD</li><li>Webové rozhraní API aplikace tooWeb: webová aplikace musí tooget prostředky z webového rozhraní API, které jsou zabezpečené službou Azure AD</li><li>Démon procesu nebo serverové aplikace tooWeb rozhraní API: démon aplikaci nebo serveru bez webového uživatelského rozhraní musí tooget prostředky z webového rozhraní API, které jsou zabezpečené službou Azure AD</li></ul><p>Podrobnosti implementace nízké úrovně naleznete toohello odkazy v části odkazy hello</p>|

## <a id="adal-scalable"></a>Přepsání hello výchozí ADAL mezipamětí tokenů škálovatelné alternativou

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure AD | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Moderní ověřování s Azure Active Directory pro webové aplikace](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/), [pomocí Redis jako ADAL mezipamětí tokenů](https://blogs.msdn.microsoft.com/mrochon/2016/09/19/using-redis-as-adal-token-cache/)  |
| **Kroky** | <p>Hello mezipaměť používá ADAL (Active Directory Authentication Library), je mezipaměti v paměti, které jsou závislé na statické úložiště, k dispozici úrovni procesu. Když tato metoda funguje u nativních aplikací, není vhodné pro mid vrstvy a back-end aplikace hello následujících důvodů:</p><ul><li>Tyto aplikace jsou dostupné přes mnoho uživatelů najednou. Ukládání všech přístupových tokenů v hello stejné úložiště, vytvoří izolace problémů a představuje výzvy při fungování ve velkém měřítku: mnoho uživatelů, každý s tolik tokeny jako prostředky hello hello přistupuje aplikace jejich jménem, může znamenat velký čísla a velmi náročná vyhledávání operace</li><li>Tyto aplikace se obvykle nasazují na Distribuovaná topologie, kde více uzlů musí mít přístup toohello stejné mezipaměti</li><li>V mezipaměti tokeny musí překonat proces recykluje a deaktivací</li></ul><p>Pro všechny hello výše důvodů při implementaci webové aplikace se doporučuje toooverride hello výchozí ADAL mezipamětí tokenů škálovatelné alternativou například Azure Redis cache.</p>|

## <a id="tokenreplaycache-adal"></a>Zajistěte, aby TokenReplayCache opětovného přehrání hello použité tooprevent tokenů ověřování ADAL

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure AD | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Moderní ověřování s Azure Active Directory pro webové aplikace](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/) |
| **Kroky** | <p>Vlastnost TokenReplayCache Hello umožňuje vývojářům toodefine mezipaměti opětovného přehrání tokenu, úložiště, které lze použít pro ukládání žádné token tokeny hello za účelem ověření, který lze použít více než jednou.</p><p>Toto je míra proti útoku běžné hello vhodně názvem opětovného přehrání tokenu útoku: útočník brání hello token odeslaný při přihlášení se může pokusit toosend ho znovu toohello aplikace ("" Přehrajte si jej znovu) pro vytvoření nové relace. Například v OIDC kódu grant tok, po úspěšné ověření uživatele, žádost o příliš "/ signin-oidc" koncový bod hello předávající strany se provádí s "požadavku id_token", "kódu" a "stavu" parametry.</p><p>Hello předávající strany ověří tento požadavek a vytvoří novou relaci. Pokud nežádoucí osoba zaznamená tuto žádost a replays ho, si můžete vytvořit úspěšné relace a uživatel hello falešná identita. Hello přítomnost hodnotu nonce hello v OpenID Connect můžete omezit, ale není plně eliminovat hello případech, ve kterých hello útok úspěšně použity. tooprotect jejich vlastních aplikací, vývojáři můžete poskytnout implementaci ITokenReplayCache a přiřadit tooTokenReplayCache instance.</p>|

### <a name="example"></a>Příklad
```C#
// ITokenReplayCache defined in ADAL
public interface ITokenReplayCache
{
bool TryAdd(string securityToken, DateTime expiresOn);
bool TryFind(string securityToken);
}
```

### <a name="example"></a>Příklad
Zde je ukázka implementace rozhraní ITokenReplayCache hello. (Prosím přizpůsobit a implementovat vaše specifické pro projekt ukládání do mezipaměti framework)
```C#
public class TokenReplayCache : ITokenReplayCache
{
    private readonly ICacheProvider cache; // Your project-specific cache provider
    public TokenReplayCache(ICacheProvider cache)
    {
        this.cache = cache;
    }
    public bool TryAdd(string securityToken, DateTime expiresOn)
    {
        if (this.cache.Get<string>(securityToken) == null)
        {
            this.cache.Set(securityToken, securityToken);
            return true;
        }
        return false;
    }
    public bool TryFind(string securityToken)
    {
        return this.cache.Get<string>(securityToken) != null;
    }
}
```
mezipaměť Hello implementována má toobe odkazovaná v možnostech OIDC prostřednictvím hello "Parametry tokenvalidationparameters." vlastnost následujícím způsobem.
```C#
OpenIdConnectOptions openIdConnectOptions = new OpenIdConnectOptions
{
    AutomaticAuthenticate = true,
    ... // other configuration properties follow..
    TokenValidationParameters = new TokenValidationParameters
    {
        TokenReplayCache = new TokenReplayCache(/*Inject your cache provider*/);
    }
}
```

Prosím Všimněte si, že tootest hello efektivita této konfigurace přihlášení do místní OIDC chráněné aplikace a zachycení hello požadavku příliš`"/signin-oidc"` koncového bodu v aplikaci fiddler. Pokud hello ochrany není na místě, přehrání této žádosti v aplikaci fiddler nastaví nového souboru cookie relace. Při požadavku hello je přehrány po přidání hello TokenReplayCache ochrany, aplikace hello vyvolá výjimku následujícím způsobem:`SecurityTokenReplayDetectedException: IDX10228: hello securityToken has previously been validated, securityToken: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ1......`

## <a id="adal-oauth2"></a>Použití knihovny ADAL toomanage token požadavky od klientů tooAAD OAuth2 (nebo místní AD)

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure AD | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [ADAL](https://azure.microsoft.com/documentation/articles/active-directory-authentication-libraries/) |
| **Kroky** | <p>Hello Azure AD authentication Library (ADAL) umožňuje klienta aplikace vývojáři tooeasily ověření uživatelé toocloud nebo místní služby Active Directory (AD) a získat přístupové tokeny zabezpečení volání rozhraní API.</p><p>ADAL obsahuje mnoho funkcí, zkontrolujte ověření snazší pro vývojáře, jako je třeba asynchronní podpora, konfigurovat tokenu mezipaměti, která ukládá přístupové tokeny a obnovovacích tokenů, automatické aktualizace tokenu, když vyprší platnost přístupového tokenu a obnovovací token je k dispozici a další.</p><p>Zpracování většinu složitost hello, ADAL může pomoci vývojáře zaměřená na obchodní logiku ve svých aplikacích a snadno zabezpečení prostředků, aniž by musel být odborník na zabezpečení. Samostatné knihovny jsou k dispozici pro rozhraní .NET, JavaScript (klient a Node.js), iOS, Android a Java.</p>|

## <a id="authn-devices-field"></a>Ověření zařízení připojující se toohello brána pole

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána pole IoT | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Ujistěte se, že každé zařízení je ověřována hello brána pole před přijetím data z nich a před usnadnění nadřazeného komunikace s hello Cloudová brána. Také se ujistěte, že zařízení připojit s na každé zařízení přihlašovacích údajů, aby jednotlivých zařízení je možné jednoznačně identifikovat.|

## <a id="authn-devices-cloud"></a>Ujistěte se, že se ověří zařízení připojující se tooCloud brány

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána IoT cloudu | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, C#, Node.JS,  |
| **Atributy**              | Není k dispozici, brány volba - Azure IoT Hub |
| **Odkazy**              | Není k dispozici, [Azure IoT hub pro rozhraní .NET](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/), [Začínáme se službou IoT hub wih a uzel JS](https://azure.microsoft.com/documentation/articles/iot-hub-node-node-getstarted), [zabezpečení IoT SAS a certifikáty](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/), [úložiště Git](https://github.com/Azure/azure-iot-sdks/tree/master/node) |
| **Kroky** | <ul><li>**Obecné:** zařízení hello ověřit pomocí zabezpečení TLS (Transport Layer) nebo protokol IPSec. Infrastruktura by měla podporovat použití předsdílený klíč (PSK) na těchto zařízeních, které nelze zpracovat úplné asymetrické šifrování. Využijte Azure AD, Oauth.</li><li>**C#:** při vytváření DeviceClient instance, ve výchozím nastavení, hello vytvořit metoda vytvoří instanci DeviceClient používající toocommunicate protokol AMQP hello službou IoT Hub. toouse hello protokol HTTPS, použijte hello přepsání metody vytvořit hello, která vám umožní toospecify hello protokolu. Pokud používáte hello protokol HTTPS, měli byste také přidat hello `Microsoft.AspNet.WebApi.Client` NuGet balíček tooyour projektu tooinclude hello `System.Net.Http.Formatting` oboru názvů.</li></ul>|

### <a name="example"></a>Příklad
```C#
static DeviceClient deviceClient;

static string deviceKey = "{device key}";
static string iotHubUri = "{iot hub hostname}";

var messageString = "{message in string format}";
var message = new Message(Encoding.ASCII.GetBytes(messageString));

deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

await deviceClient.SendEventAsync(message);
```

### <a name="example"></a>Příklad
**Node.JS: ověřování**
#### <a name="symmetric-key"></a>Symetrický klíč
* Vytvoření centra IoT v azure
* Vytvořit položku v registru identit zařízení hello
    ```javascript
    var device = new iothub.Device(null);
    device.deviceId = <DeviceId >
    registry.create(device, function(err, deviceInfo, res) {})
    ```
* Vytvoření simulovaného zařízení
    ```javascript
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>SharedAccessKey=<SharedAccessKey>';
    var client = clientFromConnectionString(connectionString);
    ```
#### <a name="sas-token"></a>SAS Token
* Získá generované interně při použití symetrický klíč, ale nemůžeme můžete vygenerovat a použít jej explicitně také
* Definujte protokol:`var Http = require('azure-iot-device-http').Http;`
* Vytvořte sas token:
    ```javascript
    resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();
    var deviceName = "<deviceName >";
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    var toSign = resourceUri + '\n' + expires;
    // using crypto
    var decodedPassword = new Buffer(signingKey, 'base64').toString('binary');
    const hmac = crypto.createHmac('sha256', decodedPassword);
    hmac.update(toSign);
    var base64signature = hmac.digest('base64');
    var base64UriEncoded = encodeURIComponent(base64signature);
    // construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "%2fdevices%2f"+deviceName+"&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
    ```
* Připojte pomocí tokenu sas: 
    ```javascript
    Client.fromSharedAccessSignature(sas, Http); 
    ```
#### <a name="certificates"></a>Certifikáty
* Generovat vlastní podepsané X509 certifikát pomocí kteréhokoli nástroj například OpenSSL toogenerate .cert a s příponou Key soubory toostore hello certifikát a hello klíč v uvedeném pořadí
* Zřídit zařízení, které přijímá zabezpečené připojení pomocí certifikátů.
    ```javascript
    var connectionString = '<connectionString>';
    var registry = iothub.Registry.fromConnectionString(connectionString);
    var deviceJSON = {deviceId:"<deviceId>",
    authentication: {
        x509Thumbprint: {
        primaryThumbprint: "<PrimaryThumbprint>",
        secondaryThumbprint: "<SecondaryThumbprint>"
        }
    }}
    var device = deviceJSON;
    registry.create(device, function (err) {});
    ```
* Připojení zařízení pomocí certifikátu
    ```javascript
    var Protocol = require('azure-iot-device-http').Http;
    var Client = require('azure-iot-device').Client;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>x509=true';
    var client = Client.fromConnectionString(connectionString, Protocol);
    var options = {
        key: fs.readFileSync('./key.pem', 'utf8'),
        cert: fs.readFileSync('./server.crt', 'utf8')
    }; 
    // Calling setOptions with hello x509 certificate and key (and optionally, passphrase) will configure hello client //transport toouse x509 when connecting tooIoT Hub
    client.setOptions(options);
    //call fn tooexecute after hello connection is set up
    client.open(fn);
    ```

## <a id="authn-cred"></a>Použít pověření ověřování podle zařízení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána IoT cloudu  | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Volba brány - Azure IoT Hub |
| **Odkazy**              | [Tokeny zabezpečení Azure IoT Hub](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/) |
| **Kroky** | Použití za použití tokeny SaS pověření ověřování zařízení na základě klíč zařízení nebo klientský certifikát, místo IoT Hub úrovni sdílených zásad přístupu. Zabrání se tak opakované použití hello ověřování tokenů jednoho zařízení nebo pole brány jiným |

## <a id="req-containers-anon"></a>Ujistěte se, že tento pouze hello požadované kontejnery a objekty BLOB jsou uvedeny anonymní přístup pro čtení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | StorageType – objekt Blob |
| **Odkazy**              | [Správa toocontainers anonymní přístup pro čtení a objekty BLOB](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/), [sdílené přístupové podpisy, část 1: vysvětlení modelu SAS hello](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) |
| **Kroky** | <p>Ve výchozím nastavení kontejner a všechny objekty BLOB v něm může být přístupné pouze vlastník hello hello účtu úložiště. toogive anonymní uživatelé oprávnění ke čtení tooa kontejner a jeho objekty BLOB, jeden můžete nastavit hello kontejneru oprávnění tooallow veřejný přístup. Anonymní uživatelé mohou číst objekty BLOB do kontejneru, veřejně přístupný bez ověřování hello požadavku.</p><p>Kontejnery poskytují následující možnosti pro správu přístupu kontejneru hello:</p><ul><li>Úplný veřejný přístup pro čtení: přes anonymní dotazy můžete číst data kontejnerů a objektů blob. Klienti, můžete vytvořit výčet objektů BLOB v kontejneru hello přes anonymní žádost, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště hello.</li><li>Veřejný přístup pro objekty BLOB pouze pro čtení: data objektu Blob v tomto kontejneru mohou číst přes anonymní žádost, ale kontejneru data nejsou k dispozici. Klienty nelze vytvořit výčet objektů BLOB v kontejneru hello přes anonymní dotazy</li><li>Žádný veřejný přístup pro čtení: kontejnerů a objektů blob dat může číst pouze vlastníka účtu hello</li></ul><p>Anonymní přístup je nejvhodnější pro scénáře, kde některé objekty BLOB musí být vždy k dispozici pro anonymní přístup pro čtení. Pro řízení citlivější jeden můžete vytvořit sdílený přístupový podpis, který umožňuje toodelegate omezený přístup pomocí různých oprávnění a v zadaném časovém intervalu. Zajištění, aby kontejnery a objekty BLOB, které mohou obsahovat potenciálně citlivá data, nejsou anonymní přístup omylem</p>|

## <a id="limited-access-sas"></a>Udělit omezený přístup tooobjects v úložišti Azure pomocí SAS nebo SAP

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici |
| **Odkazy**              | [Sdílené přístupové podpisy, část 1: Pochopení hello SAS modelu](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/), [sdílené přístupové podpisy, část 2: vytvoření a použití SAS s úložištěm Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/), [přístupu tooobjects v účtu pomocí toodelegate Sdílené přístupové podpisy uložené zásady a přístupu](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies) |
| **Kroky** | <p>Použití sdíleného přístupového podpisu (SAS) je efektivní způsob toogrant omezený přístup tooobjects v úložiště účet tooother klienty, bez nutnosti přístupový klíč účtu tooexpose. Hello SAS je identifikátor URI, který zahrnuje v jeho parametry dotazu všechny hello informace potřebné pro ověřený přístup k prostředku úložiště tooa. tooaccess prostředky úložiště s hello SAS, hello klienta stačí toopass v odpovídající konstruktor toohello hello SAS nebo metoda.</p><p>SAS můžete použít, pokud chcete přístup tooresources tooprovide v vašeho klienta tooa účet úložiště, která nemůže být považován za důvěryhodný hello klíč účtu. Klíče účtu úložiště zahrnují jak primární a sekundární klíč, které obě udělit přístup pro správu tooyour účet a všechny hello prostředky v ní. Vystavení buď klíče účtu otevře toohello možnost škodlivý nebo nedbalosti použití vašeho účtu. Sdílené přístupové podpisy zadejte alternativní bezpečné, který umožňuje ostatní klienty tooread, zápisu a odstranění dat ve vašem účtu úložiště podle toohello oprávnění, která jste udělena a bez nutnosti hello klíč účtu.</p><p>Pokud máte logickou sadu parametrů, které jsou podobné pokaždé, když, použití uložené přístup zásad (SAP) je lepší představu. Protože pomocí SAS odvozené od zásadu uložené přístupu vám dává toorevoke hello možnost, že SAS okamžitě, je, že hello doporučuje nejlepší postup tooalways používají uložené Pokud je to možné zásady přístupu.</p>|
