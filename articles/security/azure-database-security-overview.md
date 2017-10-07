---
title: "Přehled zabezpečení databáze aaaAzure | Microsoft Docs"
description: "Tento článek obsahuje přehled funkcí zabezpečení hello databáze Azure."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: TomSh
ms.openlocfilehash: 13f14b99d15800e85e9906a9d167eb0adf2135de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-security-overview"></a>Přehled zabezpečení databáze Azure

Zabezpečení je velmi důležité při správě databáze a byla vždy prioritu pro Azure SQL Database. Azure SQL Database podporuje připojení zabezpečení se pravidla brány firewall a šifrování připojení. Podporuje ověřování pomocí uživatelského jména a hesla a Azure Active Directory, ověřování, který používá identity spravované službou Azure Active Directory. Autorizace používá řízení přístupu na základě rolí.

Azure SQL Database podporuje šifrování provedením v reálném čase šifrování a dešifrování databáze, přidružených záloh a souborů protokolů transakci bez nutnosti změny toohello aplikace.

Společnost Microsoft poskytuje další způsoby tooencrypt podnikových dat:

-   Úrovni buněk i buněk dat nebo šifrování tooencrypt konkrétní sloupce s různými šifrovacími klíči.
-   Pokud potřebujete modul hardwarového zabezpečení nebo Centrální správa ve vaší hierarchii šifrovací klíče, zvažte použití Azure Key Vault se systémem SQL Server ve virtuálním počítači Azure.
-   Vždy šifrovaný (momentálně ve verzi preview) umožňuje tooapplications transparentní šifrování a umožňuje klientům tooencrypt citlivá data uvnitř klientské aplikace bez sdílení hello šifrovací klíče s databází SQL.

Auditování databáze SQL Azure umožňuje podnikům toorecord události tooan auditu přihlášení Azure Storage. Auditování databáze SQL se také integruje s Microsoft Power BI toofacilitate procházení sestavy a analýzy.

 Databáze SQL Azure může být úzce zabezpečené toosatisfy nejvíce zákonných nebo požadavky na zabezpečení, včetně HIPAA, ISO 27001/27002 a PCI DSS úrovně 1, mimo jiné. Aktuální seznam certifikace dodržování předpisů zabezpečení je k dispozici na hello [webu Microsoft Azure Trust Center](http://azure.microsoft.com/support/trust-center/services/).

Tento článek vás provede základy hello zabezpečení Microsoft databází SQL Azure pro Structured, tabulková a relační Data. Tento článek vám především pomůže začít s prostředky pro ochranu dat, řízením přístupu a proaktivním monitorováním.

Tento přehled zabezpečení databáze Azure článek se zaměřuje na hello následující oblasti:

-   Ochrana dat
-   Řízení přístupu
-   Proaktivní monitorování
-   Centralizované zabezpečení správy
-   Azure Marketplace

## <a name="protect-data"></a>Ochrana dat

Databáze SQL zabezpečuje vašich dat tím, že poskytuje šifrování dat pomocí pohybu [Transport Layer Security](https://support.microsoft.com/kb/3135244)pro data na rest pomocí [transparentní šifrování dat](http://go.microsoft.com/fwlink/?LinkId=526242)a pro data v použití pomocí [ Funkce Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx).

V této části se věnuje:

-   Šifrování v provozu
-   Šifrování v klidovém stavu
-   Šifrování používá (klient)

Pro jiné způsoby tooencrypt data, vezměte v úvahu:

-   [Šifrování na úrovni buněk](https://msdn.microsoft.com/library/ms179331.aspx) tooencrypt určité sloupce nebo i buňky dat s různými šifrovacími klíči.
-   Pokud potřebujete modul hardwarového zabezpečení nebo chcete centrálně spravovat hierarchii šifrovacích klíčů, měli byste uvažovat o [řešení Azure Key Vault s SQL Serverem na virtuálním počítači v Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).

### <a name="encryption-in-motion"></a>Šifrování v provozu

Častých problémů pro všechny aplikace, klient server je hello potřebu ochrany osobních údajů, protože data se přesouvají přes veřejný a privátní sítě. Pokud není šifrovat data přesouvání přes síť, je pravděpodobné hello, můžete zachytit a odcizení neoprávnění uživatelé. Při plánování práce s databázové služby, je nutné toomake jistotu, že data se šifrují mezi hello databáze klientských a serverových i mezi databázové servery, které komunikují mezi sebou a se aplikace střední vrstvy.

Jedním z problémů při správě sítě je zabezpečení dat posílaných mezi aplikacemi přes nedůvěryhodné sítě. Můžete použít [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) tooauthenticate serverů a klientů a pak jeho pomocí zprávy tooencrypt mezi hello ověřený stranami.

V procesu ověřování hello odešle klient TLS/SSL zprávu serveru TLS/SSL tooa a hello server odpoví informacemi hello, že tento server hello je tooauthenticate sám sebe. Hello klient a server provedou další výměnu klíčů relací a hello končí dialog ověřování. Po dokončení ověření může mezi hello serveru a klienta hello pomocí hello klíčů symetrického šifrování, které se vytvoří během procesu ověření hello začít komunikace zabezpečená protokolem SSL.

Všechny tooAzure připojení databáze SQL vyžadovat šifrování (SSL/TLS) na všechny časy dat při tooand "při přenosu" z databáze hello. SQL Azure používá protokol TLS/SSL tooauthenticate serverů a klientů a pak jeho pomocí zprávy tooencrypt mezi hello ověřený stranami. V připojovacím řetězci vaší aplikace musíte zadat parametry tooencrypt hello připojení není tootrust hello certifikát serveru a (Pokud udělá se to pro vás zkopírujte připojovací řetězec mimo hello portálu Azure Classic), jinak hello bude připojení nelze ověřit identitu hello hello serveru a bude náchylný útoky příliš "man-in-the-middle". Pro hello ovladač technologie ADO.NET, například tyto parametry řetězec připojení jsou šifrovat = True a TrustServerCertificate = False.

### <a name="encryption-at-rest"></a>Šifrování v klidovém stavu
Může trvat několik opatření toohelp zabezpečené hello databáze například návrhu zabezpečení systému, šifrování důvěrné prostředky a vytváření brány firewall kolem hello databázové servery. Ale ve scénáři, kde jsou odcizení hello fyzická média (například jednotky nebo záložní pásky), škodlivý strany můžete jenom obnovit nebo připojit databázi hello a procházet daty hello.

Jedno řešení je tooencrypt hello citlivá data v databázi hello a chrání hello klíče, které jsou používané tooencrypt hello data s certifikátem. To brání osobě bez hello klíče pomocí hello data, ale musí být plánované takovou ochranu.

Tento problém, SQL Server a Azure SQL podporují toosolve [transparentní šifrování šifrování dat (TDE)](https://docs.microsoft.com/sql/relational-databases/securityrecryption/transparent-data-encryption-tde). Šifrování TDE šifruje SQL Server a databáze SQL Azure datových souborů, označuje jako šifrování dat v klidovém stavu.

Azure SQL Database transparentní šifrování dat pomáhá chránit před ohrožením hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování hello databáze, přidružených záloh a souborů protokolů transakci bez nutnosti změny toohello aplikace.  

Šifrování TDE zašifruje úložiště hello celé databáze pomocí symetrického klíče volané hello šifrovací klíč databáze. V databázi SQL šifrovací klíč databáze hello je chráněn certifikát integrovaného serveru. certifikát integrovaného serveru Hello je jedinečný pro každý server databáze SQL.

Pokud je databáze v relace GeoDR, je chráněn jiný klíč na každém serveru. Pokud jsou dva databáze připojené toohello sdílejí stejný server hello předdefinované stejný certifikát. Microsoft automaticky otočí tyto certifikáty alespoň jednou za 90 dní. Obecný popis TDE, najdete v části [transparentní šifrování šifrování dat (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Šifrování používá (klient)

Většina úniky dat zahrnuje krádež hello důležitých dat, třeba čísla platebních karet nebo identifikovatelné osobní údaje. Databáze může být poklad troves citlivých informací. Mohou obsahovat osobní data zákazníků, důvěrné informace produktivní a duševní vlastnictví. Ztracené nebo odcizené data, zejména zákaznická data, může způsobit poškození značky, konkurenční nevýhodou a závažné pokutami – dokonce i probíhajících řízení.

![Funkce Always Encrypted](./media/azure-databse-security-overview/azure-database-fig1.png)

[Funkce Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) je funkce určená tooprotect citlivých dat, třeba čísla platebních karet nebo national identifikačními čísly (například USA čísel sociálního pojištění), uloženy v databázi Azure SQL Database nebo SQL Server. Šifrovaný vždy umožňuje klientům tooencrypt citlivá data uvnitř klientské aplikace a nikdy odhalit hello šifrovací klíče toohello databázový stroj (SQL Database nebo SQL Server).

Vždy šifrovaný zajišťuje oddělení mezi uživatelů, kteří vlastní hello data (a můžete ji zobrazit) a ty, kteří spravují hello data (ale musí mít žádný přístup). Zajištěním místní správci databází cloudu operátory databáze nebo jiných vysokou úrovní oprávnění, ale neoprávnění uživatelé získat přístup hello šifrovaná data,

Kromě toho funkce Always Encrypted díky tooapplications transparentní šifrování. Podporou funkce Always Encrypted ovladačů nainstalovaných na hello klientský počítač, aby mohli automaticky šifrování a dešifrování citlivých dat v aplikaci klienta hello. ovladač Hello šifruje data hello citlivé sloupce před předáním hello data toohello databázový stroj a automaticky přepíše dotazy tak, aby se zachovají hello sémantiku toohello aplikace. Podobně hello ovladač transparentně dešifruje data, uložená v sloupců šifrované databáze, obsažené ve výsledcích dotazů.

## <a name="access-control"></a>Řízení přístupu
tooprovide zabezpečení, databáze SQL řídí přístup s pravidla brány firewall omezení připojení pomocí IP adresy, vyžadování uživatelé tooprove mechanismy ověřování své identity a autorizace mechanismy omezení uživatelů toospecific akce a data.

### <a name="database-access"></a>Přístup k databázi

Ochrana dat začíná řízení přístupu tooyour data. Hello datového centra hostujícího vaše data spravuje fyzický přístup, když zabezpečení toomanage brány firewall můžete nakonfigurovat hello síťové vrstvy. Také můžete řídit přístup nakonfigurováním přihlášení pro ověřování a definování oprávnění pro role serveru a databáze.

V této části se věnuje:

-   Brána firewall a pravidla brány firewall
-   Authentication
-   Autorizace

#### <a name="firewall-and-firewall-rules"></a>Brána firewall a pravidla brány firewall

Microsoft Azure SQL Database poskytuje relační databázovou službu pro aplikace Azure a další internetové aplikace. toohelp chránit vaše data, brány firewall zabránit všechny přístup tooyour databázový server, dokud nezadáte, které počítače mají oprávnění. brány firewall Hello uděluje přístup toodatabases podle hello pocházející IP adresu každého požadavku. Další informace najdete v tématu [Přehled pravidel brány firewall služby Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

Hello [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) služba je k dispozici prostřednictvím portu TCP 1433. tooaccess databáze SQL z vašeho počítače, ujistěte se, že klientský počítač brány firewall umožňuje odchozí komunikaci TCP na portu TCP 1433. Pokud ho jiné aplikace nepotřebují, zablokujte na portu TCP 1433 příchozí připojení.

#### <a name="authentication"></a>Authentication

Ověřování databáze SQL odkazuje toohow prokázání své identity při připojování toohello databáze. SQL Database podporuje dva typy ověřování:

-   **Ověřování SQL:** účet jednotné přihlášení se vytvoří při vytvoření logické instance SQL, názvem hello účet odběratele databáze SQL. Tento účet se připojí pomocí [ověřování serveru SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (uživatelské jméno a heslo). Tento účet je že správce v instanci logického serveru hello a pro všechny uživatelské databáze připojené toothat instance. Hello oprávnění hello odběratele účet nemůže být s omezeným přístupem. Existovat může jenom jeden z těchto účtů.
-   **Azure Active Directory Authentication:** [ověřování Azure Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) mechanismus připojující tooMicrosoft Azure SQL Database a SQL Data Warehouse pomocí identit v Azure Active Directory ( Azure AD). Díky této může toocentrally můžete spravovat identity uživatelů databáze.

![Authentication](./media/azure-databse-security-overview/azure-database-fig2.png)

 Výhody ověřování Azure Active Directory:
  - Poskytuje alternativní tooSQL ověření serveru.
  - Také pomáhá zastavit šíření hello identit uživatelů mezi servery databáze & umožňuje otočení heslo na jednom místě.
  - Můžete spravovat oprávnění databáze pomocí externí skupiny (Azure Active Directory).
  - Ukládání hesel se může eliminovat tím, že umožňuje integrované ověřování systému Windows a jiných forem ověřování podporovaných službou Azure Active Directory.

#### <a name="authorization"></a>Autorizace
[Autorizace](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) odkazuje toowhat může uživatel provádět v rámci Azure SQL Database, a tím je řízeno váš uživatelský účet databáze [členství v rolích](https://msdn.microsoft.com/library/ms189121) a [oprávnění na úrovni objektů](https://msdn.microsoft.com/library/ms191291.aspx). Autorizace je proces hello určení zabezpečitelné prostředky, ke kterým přístup objekt zabezpečení a operací, které jsou povoleny pro tyto prostředky.

### <a name="application-access"></a>Přístup k aplikaci

V této části se věnuje:

-   Dynamické maskování dat
-   Zabezpečení na úrovni řádku

#### <a name="dynamic-data-masking"></a>Dynamické maskování dat
Zástupce služby v telefonickém centru může identifikovat volající podle několik číslic jejich číslo sociálního pojištění nebo číslo platební karty, ale tato data, která položky by neměl být plně zveřejněné toohello zástupce služby.

Že masky všechny ale hello poslední čtyři číslice čísla sociálního zabezpečení nebo číslo platební karty v sadě výsledků hello jakýkoli dotaz, může být definováno pravidlo maskování.

![Dynamické maskování dat](./media/azure-databse-security-overview/azure-database-fig3.png)

Například maska příslušná data může být dat definované tooprotect identifikovatelné osobní údaje (PII), tak, aby vývojář může dotazovat provozní prostředí pro účely odstraňování potíží, aniž bychom při tom porušili předpisy.

[SQL databáze dynamického maskování dat](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) omezuje zranitelnost citlivá data pomocí maskování ji uživatelé toonon úrovní oprávnění. Dynamická data maskování je podporována pro verzi 12 hello databáze SQL Azure.

[Dynamická data maskování](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) pomáhá zabránit neoprávněnému přístupu toosensitive dat tím, že vám toodesignate kolik hello citlivá data tooreveal s minimálním dopadem na aplikační vrstvu hello. Je funkce založená na zásadách zabezpečení, která skryje hello citlivá data v hello sadu výsledků dotazu přes pole určené databáze, zatímco hello data v databázi hello se nezmění.


> [!Note]
> Dynamická data maskování se dá nakonfigurovat Dobrý den, správce databáze Azure, správce serveru nebo úředník role zabezpečení.

#### <a name="row-level-security"></a>Zabezpečení na úrovni řádků
Další běžné požadavky zabezpečení pro víceklientské databáze je [zabezpečení na úrovni řádků](https://msdn.microsoft.com/library/dn765131.aspx). Tato funkce umožňuje přístup toorows toocontrol v tabulce databáze na základě charakteristik hello hello uživatele provádění dotazu (například skupinu členství nebo provádění kontextu).

![-Zabezpečení na úrovni řádků](./media/azure-databse-security-overview/azure-database-fig4.png)

logiku omezení přístupu Hello je umístěn v databázové vrstvy hello spíše než tokeny na základě dat hello v jiné aplikační vrstvě. databázový systém Hello platí omezení přístupu hello pokaždé, když dojde k pokusu o tento přístup k datům z libovolné úrovně. Díky systému zabezpečení spolehlivější a robustní snížením hello útoku na zabezpečení systému.

Zabezpečení na úrovni řádků zavádí řízení přístupu predikátem. Nabízí flexibilní, centralizované, na základě predikát hodnocení, které můžete vzít v úvahu metadata nebo dalších kritérií, která určuje hello správce podle potřeby. Predikát Hello se používá jako kritérium toodetermine, zda má uživatel hello hello odpovídající přístup toohello dat založit na atributech uživatelů. Řízení přístupu na základě popisku můžete implementovat pomocí řízení přístupu na základě predikátu.

## <a name="proactive-monitoring"></a>Proaktivní monitorování
Databáze SQL zabezpečuje vašich dat tím, že poskytuje **auditování** a **detekce hrozby** možnosti.

### <a name="auditing"></a>Auditování
Auditování databáze SQL se zvyšuje vaše možnost toogain přehled o události a změny, které se vyskytují v databázi hello, včetně aktualizací a dotazy na hello data.

[Auditování databáze SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) sleduje události databáze a zapisuje tooan protokolu auditování v účtu úložiště Azure. Auditování pomáhá zajistit dodržování předpisů, porozumět databázové aktivitě a získat přehled o nesrovnalostech a anomáliích, které můžou značit problémy obchodního charakteru nebo vzbuzovat podezření na narušení zabezpečení. Auditování umožňuje a zjednodušuje dodržování standardů toocompliance, ale není zajistit dodržování předpisů.

Auditování databáze SQL umožňuje:

-   **Zachovat** monitorovat vybrané události. Můžete definovat kategorie toobe databáze akce auditovat.
-   **Sestava** v databázové aktivitě. Můžete vytvořit předem nakonfigurované sestavy a řídicí panel tooget, rychle začít s aktivity a události vytváření sestav.
-   **Analýza** sestavy. Můžete najít podezřelé události, neobvyklé aktivity a trendy.

Existují dvě metody auditování:

-   **Auditování objektů blob** -protokoly zapisují tooAzure úložiště objektů Blob. Toto je novější auditování metoda, která poskytuje vyšší výkon a podporuje auditování vyšší na úrovni objektů členitosti, větší finanční efektivita.
-   **Auditování tabulka** -protokoly zapisují tooAzure Table Storage.

### <a name="threat-detection"></a>Detekce hrozeb
[Detekce hrozeb Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) detekuje podezřelé aktivity, které indikují potenciální ohrožení. Detekce hrozeb umožňuje toorespond toosuspicious události v databázi hello, jako je například vložení SQL, kdy k nim dojde. Poskytuje výstrahy a umožňuje použití hello auditování databáze SQL Azure tooexplore hello podezřelé události.

![Detekce hrozeb](./media/azure-databse-security-overview/azure-database-fig5.jpg)

Například Injektáž SQL je jedním z hello běžné webové aplikace problémy se zabezpečením na Internetu, použít tooattack datové aplikace hello. Útočníci využít výhod aplikace ohrožení zabezpečení tooinject škodlivý příkazů SQL do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi hello.

Osoby zabezpečení nebo jiné určený správce můžete získat okamžité odeslání oznámení o aktivitách podezřelé databáze, kdy k nim dojde. Každý oznámení poskytuje podrobné informace o hello podezřelé aktivity a doporučuje jak toofurther prozkoumat a zmírnit hrozby hello.        

## <a name="centralized-security-management"></a>Centralizované zabezpečení správy

[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) pomáhá zabránit, zjistit a reagovat toothreats. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

[Security Center](https://docs.microsoft.com/azure/security-center/security-center-sql-database) pomáhá zabezpečit data v databázi SQL tím, že poskytuje přehled o zabezpečení hello serverů a databází. Security Center můžete:

-   Definujte zásady pro šifrování SQL Database a auditování.
-   Monitorování hello zabezpečení prostředků databáze SQL ve vašich předplatných.
-   Rychle identifikovat a opravit problémy se zabezpečením.
-   Integrovat výstrahy z [detekce hrozeb Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
-   Security Center podporuje přístup na základě rolí.

## <a name="azure-marketplace"></a>Azure Marketplace

Hello Azure Marketplace je online služby a aplikace marketplace, která umožňuje zákazníkům tooAzure řešení kolem hello, world začínající podniky a toooffer nezávislé výrobce softwaru (ISV).
kombinuje Azure Marketplace Hello ekosystémů partnera Microsoft Azure do toobetter jednotnou platformu sloužit naše zákazníky a partnery. Klikněte na tlačítko [sem](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) tooglance databáze zabezpečovací produkty k dispozici v Azure Marketplace.

## <a name="next-steps"></a>Další kroky

- Další informace o [zabezpečení vaší databázi SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
- Další informace o [služby Azure Security Center a Azure SQL Database](https://docs.microsoft.com/azure/security-center/security-center-sql-database).
- toolearn Další informace o detekce hrozeb, najdete v části [detekce hrozeb databáze SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
- Další, najdete v části toolearn [SQL zlepšit výkon databáze](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial). 
