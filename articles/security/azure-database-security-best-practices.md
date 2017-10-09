---
title: "osvědčené postupy zabezpečení databáze aaaAzure | Microsoft Docs"
description: "Tento článek obsahuje sadu osvědčené postupy pro zabezpečení databáze Azure."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: tomsh
ms.openlocfilehash: 78984291e8f74879c7f738e2ce3176c4be18d154
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-security-best-practices"></a>Osvědčené postupy zabezpečení databáze Azure

Zabezpečení je velmi důležité při správě databáze a byla vždy prioritu pro Azure SQL Database. Vaše databáze může být úzce zabezpečené toohelp uspokojení nejvíce zákonných nebo požadavky na zabezpečení, mimo jiné včetně HIPAA, ISO 27001/27002 a PCI DSS úrovně 1. Aktuální seznam certifikace dodržování předpisů zabezpečení je k dispozici na hello [Microsoft Trust Center lokality](http://azure.microsoft.com/support/trust-center/services/). Můžete také zvolit tooplace na zákonné požadavky na základě vašich databází v konkrétní datových centrech Azure.

V tomto článku se budeme zabývat kolekce osvědčené postupy zabezpečení databáze Azure. Tyto doporučené postupy jsou odvozeny od našich zkušeností s zabezpečení databáze Azure a hello prostředí zákazníků, jako sami.

Pro každý osvědčený postup objasníme:

-   Jaké hello osvědčeným postupem je
-   Proč chcete tooenable, že osvědčeným postupem
-   Pokud selže tooenable hello osvědčený postup, co mohou být výsledek hello
-   Jak můžete získat informace tooenable hello osvědčený postup

Tento článek osvědčené postupy zabezpečení databáze Azure je založený na konsenzu stanovisko a možnostech platformy Azure a nastaví funkce, které jsou ve hello dobu, kdy byla zapsána v tomto článku. Názory a technologie mění v čase a budou v tomto článku v pravidelných intervalech tooreflect aktualizovat tyto změny.

Osvědčené postupy pro databáze Azure zabezpečení popsané v tomto článku patří:

-   Použít pravidla brány firewall, toorestrict přístup k databázi
-   Povolit ověřování databáze
-   Chraňte svá data pomocí šifrování
-   Ochranu přenášených dat
-   Povolit auditování databáze
-   Povolení detekce hrozeb databáze


## <a name="use-firewall-rules-toorestrict-database-access"></a>Použít pravidla brány firewall, toorestrict přístup k databázi

Microsoft Azure SQL Database poskytuje relační databázovou službu pro aplikace Azure a další internetové aplikace. zabezpečení přístupu tooprovide, databáze SQL řídí přístup s pravidla brány firewall omezení připojení pomocí IP adresy, vyžadování uživatelé tooprove mechanismy ověřování své identity a autorizace mechanismy omezení uživatelů toospecific akce a data. Brány firewall zabránit všechny přístup tooyour databázový server, dokud nezadáte, které počítače mají oprávnění. brány firewall Hello uděluje přístup toodatabases podle hello pocházející IP adresu každého požadavku.

![Pravidla brány firewall](./media/azure-database-security-best-practices/azure-database-security-best-practices-Fig1.png)

Hello služby Azure SQL Database je pouze k dispozici prostřednictvím portu TCP 1433. tooaccess databáze SQL z vašeho počítače, ujistěte se, že klientský počítač brány firewall umožňuje odchozí komunikaci TCP na portu TCP 1433. Pokud není skutečně potřeba pro jiné aplikace, blokovala příchozí připojení na portu TCP 1433 pomocí pravidel brány firewall.

Jako součást procesu připojení hello připojení z virtuální počítače Azure jsou přesměrovaného tooa jinou IP adresu a port, jedinečné pro každou roli pracovního procesu. číslo portu Hello je v rozsahu hello od 11000 too11999. Další informace o portech TCP naleznete v tématu [porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database2](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12).

> [!Note]
> Další informace o pravidlech brány firewall pro SQL Database najdete v tématu [Pravidla brány firewall služby SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

## <a name="enable-database-authentication"></a>Povolit ověřování databáze
Databáze SQL podporuje dva typy ověřování, ověřování SQL a Azure Active Directory Authentication (ověřování Azure AD).

**Ověřování SQL** se doporučuje v následujících případech:

-   Umožňuje prostředí toosupport SQL Azure s smíšený operační systémy, kde všichni uživatelé nejsou ověřené doménou systému Windows.
-   Umožňuje SQL Azure toosupport starší aplikace a aplikace třetích stran, které vyžadují ověřování systému SQL Server.
-   Umožňuje uživatelům tooconnect z domén neznámý nebo nedůvěryhodný. Například aplikace, kde Zákazníci zavedené připojení s přiřazený přihlášení serveru SQL tooreceive hello stav jejich objednávek.
-   Webové aplikace umožňuje toosupport SQL Azure, u kterých uživatelé vytvářet své vlastní identity.
-   Umožňuje, aby software vývojáři toodistribute na základě svých aplikacích pomocí hierarchie komplexní oprávnění známé, přednastavení přihlášení serveru SQL.

> [!Note]
> Ověřování systému SQL Server však nelze použít bezpečnostní protokol Kerberos.

Pokud používáte **ověřování SQL** musíte:

-   Spravujte hello silné přihlašovací údaje.
-   Ochranu hello přihlašovacích údajů v připojovacím řetězci hello.
-   (Potenciálně) Chraňte hello přihlašovacích údajů předaných přes síť hello z hello webový server toohello databáze. Další informace najdete v části [postupy: připojení tooSQL v technologii ASP.NET 2.0 pomocí serveru SQL ověřování](https://msdn.microsoft.com/library/ms998300.aspx).

**Ověřování Azure Active Directory** mechanismus připojující tooMicrosoft Azure SQL Database a [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) pomocí identit v Azure Active Directory (Azure AD). Při ověřování Azure AD můžete centrálně spravovat hello identity uživatelů, databáze a další služby Microsoftu v jednom centrálním místě. Centrální správa ID umožňuje uživatelům toomanage databáze na jednom místě a zjednodušuje správu oprávnění. Například hello následující výhody:

-   Poskytuje alternativní tooSQL ověření serveru.
-   Pomáhá zastavit šíření hello identit uživatelů serverů databáze.
-   Umožňuje otočení heslo na jednom místě.
-   Zákazníci můžete spravovat oprávnění databáze pomocí externí skupiny (AAD).
-   Ukládání hesel se může eliminovat tím, že umožňuje integrované ověřování systému Windows a jiných forem ověřování podporovaných službou Azure Active Directory.
-   Azure AD ověřování pomocí identity tooauthenticate uživatelé databáze s omezením na úrovni databáze hello.
-   Azure AD podporuje ověřování na základě tokenu pro připojování tooSQL databáze aplikace.
-   Ověřování služby Azure AD podporuje služby AD FS (federation domény) nebo ověřování nativní uživatele a heslo pro místní Azure Active Directory bez synchronizace domény.
-   Azure AD podporuje připojení z SQL Server Management Studio, které používají Universal ověřování služby Active Directory, která zahrnuje Multi-Factor Authentication (MFA). Vícefaktorové ověřování zahrnuje silné ověřování s celou řadu možností snadno ověření – telefonní hovor, textová zpráva, čipové karty s PIN kód nebo oznámení mobilní aplikace. Další informace najdete v tématu [SSMS podpora pro Azure AD MFA s SQL Database a SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication).

kroky konfigurace Hello zahrnout hello následující tooconfigure postupy a pomocí ověřování Azure Active Directory.

-   Vytvořit a naplnit Azure AD.
-   Volitelné: Přidružení nebo změňte hello služby active directory, který je aktuálně přidružena předplatného Azure.
-   Vytvoření správce Azure Active Directory pro server Azure SQL nebo [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
- Nakonfigurujte klientské počítače.
-   Vytvořte uživatele databáze s omezením vaší databáze namapované tooAzure identit AD.
-   Připojte databáze tooyour pomocí identit Azure AD.

Podrobnosti o informace můžete najít [zde](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

## <a name="protect-your-data-using-encryption"></a>Chraňte svá data pomocí šifrování

[Azure SQL Database transparentní šifrování dat (šifrování TDE)](https://msdn.microsoft.com/library/dn948096.aspx) pomáhá chránit před ohrožením hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování hello databáze, přidružených záloh a soubory protokolu transakcí v klidovém stavu bez vyžadují aplikace toohello změny. Šifrování TDE zašifruje úložiště hello celé databáze pomocí symetrického klíče volané hello šifrovací klíč databáze.

I v případě, že šifrované hello celý úložiště, je velmi důležité tooalso šifrování databáze sám sebe. Toto je implementace hello obrany v hloubka přístupu, ochrany dat. Pokud používáte Azure SQL Database a chcete tooprotect citlivých dat jako platebních karet nebo čísla sociálního pojištění, můžete šifrovat databáze s FIPS 140-2 ověřit 256bitová šifrování AES splňuje požadavky hello mnoho oborových standardů (například HIPAA, PCI).

Je důležité toounderstand, která soubory související příliš[vyrovnávací paměti rozšíření fondu (BPE)](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) nejsou šifrují, pokud databáze je zašifrovaná pomocí šifrování TDE. Je nutné použít nástroje šifrování na úrovni systému souborů, jako je [BitLocker](https://technet.microsoft.com/library/cc732774) nebo hello [systému souborů EFS (ENCRYPTING File System)](https://technet.microsoft.com/library/cc700811.aspx) pro BPE související soubory.

Od autorizovaným uživatelem, jako je zabezpečení správce nebo správce databáze přístup hello data i v případě databáze hello je šifrován TDE, také postupujte podle doporučení hello níže:

-   Povolte ověřování SQL na úrovni databáze hello.
-   Ověřování pomocí služby Azure AD pomocí [role RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is).
-   Uživatelé a aplikace by měla používat samostatné účty tooauthenticate. Tímto způsobem můžete omezit hello oprávnění udělená toousers a aplikace a snížení rizika hello škodlivých aktivit.
-   Implementace zabezpečení na úrovni databáze pomocí pevné databázové role (například db_datareader nebo db_datawriter), nebo můžete vytvořit vlastní role pro vaše aplikace toogrant výslovná oprávnění tooselected databázové objekty.

Pro jiné způsoby tooencrypt data, vezměte v úvahu:

-   [Šifrování na úrovni buněk](https://msdn.microsoft.com/library/ms179331.aspx) tooencrypt určité sloupce nebo i buňky dat s různými šifrovacími klíči.
-   Šifrování používá pomocí funkce Always Encrypted: [vždy šifrována](https://msdn.microsoft.com/library/mt163865.aspx) umožňuje klientům tooencrypt citlivá data uvnitř klientské aplikace a nikdy odhalit hello šifrovací klíče toohello databázový stroj (SQL Database nebo SQL Server). V důsledku toho vždy šifrována zajišťuje oddělení mezi uživatelů, kteří vlastní hello data (a můžete ji zobrazit) a ty, kteří spravují hello data (ale musí mít žádný přístup).
-   Pomocí zabezpečení na úrovni řádků: zabezpečení na úrovni řádků umožňuje zákazníkům toocontrol přístup toorows v tabulce databáze na základě charakteristik hello hello uživatele provádění dotazu (například skupinu členství nebo provádění kontextu). Další informace najdete v tématu [Zabezpečení na úrovni řádku](https://msdn.microsoft.com/library/dn765131).

## <a name="protect-data-in-transit"></a>Ochranu přenášených dat
Základní součástí strategie ochrany dat by měly být ochrany dat během přenosu. Vzhledem k tomu, že data bude přesunutí a zpět z mnoho míst, hello obecné doporučení je vždy použít data tooexchange protokoly SSL/TLS v různých umístěních. V některých případech může být vhodné tooisolate hello celý komunikační kanál mezi místní a cloudové infrastruktury pomocí virtuální privátní sítě (VPN).

Pro přesun mezi vaší místní infrastruktury a Azure data měli byste zvážit příslušná bezpečnostní opatření, například HTTPS nebo VPN.

Pro organizace, které potřebují toosecure přístup z více tooAzure nacházejí na místních pracovní stanice, použijte [Azure site-to-site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

Pro organizace, které potřebují toosecure přístup z jednotlivých pracovních stanic se nachází v místě nebo tooAzure mimo pracoviště, zvažte použití [Point-to-Site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Větších datových sad se dají přesunout přes vyhrazené vysokorychlostní propojení WAN, jako [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Pokud si zvolíte toouse ExpressRoute, můžete také šifrování dat hello hello úrovni aplikace pomocí [SSL/TLS](https://support.microsoft.com/kb/257591) nebo jiné protokoly pro zvýšení ochrany.

Pokud jsou interakci s Azure Storage prostřednictvím hello portálu Azure, všechny transakce dojít přes HTTPS. [Rozhraní API REST úložiště](https://msdn.microsoft.com/library/azure/dd179355.aspx) přes protokol HTTPS může být také použít toointeract s [Azure Storage](https://azure.microsoft.com/services/storage/) a [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organizace, které nesplní tooprotect přenášených dat budou náchylnější pro [útokům man-in-the-middle](https://technet.microsoft.com/library/gg195821.aspx), [odposlouchávání](https://technet.microsoft.com/library/gg195641.aspx) a zneužití relace. Tyto útoky lze získat přístup k datům tooconfidential hello prvním krokem.

Další informace o Azure VPN možnost přečíst článek hello toolearn [plánování a návrhu pro bránu VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-plan-design).

## <a name="enable-database-auditing"></a>Povolit auditování databáze
Auditování instanci databázového stroje SQL Server hello nebo jednotlivé databáze zahrnuje sledování a protokolování události na hello databázového stroje. SQL Server audit umožňuje vytvářet audity serveru, které může obsahovat specifikace auditu serveru pro události na úrovni serveru a specifikace auditu databáze pro databázi úroveň události. Auditované události může být napsán protokoly událostí toohello nebo tooaudit soubory.

Existuje několik úrovní auditování pro SQL Server, v závislosti na státní nebo standardy požadavky pro instalaci. SQL Server Audit poskytuje hello nástroje a procesy tooenable, úložiště a audity zobrazení musí mít u různých objektů serveru a databáze.

[Auditování databáze SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) sleduje události databáze a zapisuje tooan protokolu auditování v účtu úložiště Azure.

Auditování pomáhá zajistit dodržování předpisů, porozumět databázové aktivitě a získat přehled o nesrovnalostech a anomáliích, které můžou značit problémy obchodního charakteru nebo vzbuzovat podezření na narušení zabezpečení.

Auditování umožňuje a zjednodušuje dodržování standardů toocompliance, ale není zajistit dodržování předpisů.

Další informace o auditování databáze toolearn a jak tooenable, přečtěte si článek hello [povolit auditování a zjišťování hrozeb na serverech SQL v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="enable-database-threat-detection"></a>Povolení detekce hrozeb databáze
Detekce hrozeb SQL umožňuje toodetect a toopotential hrozeb reagovat, když k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity. Obdržíte výstrahu při databáze podezřelé aktivity, potenciální ohrožení zabezpečení a prostřednictvím injektáže SQL, jakož i nezvyklé databázové přístupové vzorce. Detekce hrozeb SQL výstrahy zadejte podrobnosti podezřelých aktivit a doporučujeme akce jak tooinvestigate a zmírnit hrozby hello.

Například Injektáž SQL je jedním z hello běžné webové aplikace problémy se zabezpečením na Internetu, použít tooattack datové aplikace hello. Útočníci využít výhod aplikace ohrožení zabezpečení tooinject škodlivý příkazů SQL do pole pro zadání aplikací, před nedodržením nebo upravovat data v databázi hello.

toolearn o tooset až detekce hrozeb pro vaše databáze v hello Azure portálu najdete [detekce hrozeb databáze SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Kromě toho detekce hrozeb SQL integruje výstrahy s [Azure Security Center](https://azure.microsoft.com/services/security-center/). Doporučujeme vám tootry ho za 60 dnů pro uvolnění.

Další informace o databázi detekce hrozeb toolearn a jak tooenable, přečtěte si článek hello [povolit auditování a zjišťování hrozeb na serverech SQL v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="conclusion"></a>Závěr
Databáze Azure je platforma robustní databáze, s celou řadu funkcí zabezpečení, které splňují mnoho organizace i regulačních požadavků. Vám může pomoct chránit data pomocí řízení dat tooyour hello fyzický přístup a použití různých možností pro zabezpečení dat v hello souboru - column-úrovni nebo řádek s transparentní šifrování dat, šifrování na úrovni buněk nebo zabezpečení na úrovni řádků. Vždy šifrovaný také umožní operace proti šifrovaná data, ke zjednodušení procesu hello aktualizací aplikace. Přístup k protokolům tooauditing aktivity databáze SQL se pak poskytuje hello informace, které potřebujete, abyste tooknow jak a kdy je přístup k datům.

## <a name="next-steps"></a>Další kroky
- toolearn Další informace o pravidla brány firewall, najdete v části [pravidla brány Firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).
- toolearn o uživatelích a přihlášení, najdete v části [spravovat přihlášení](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).
- Podívejte se kurz [zabezpečení vaší databázi SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
