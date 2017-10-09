---
title: "aaaData zabezpečení a doporučené postupy šifrování | Microsoft Docs"
description: "Tento článek obsahuje sadu osvědčené postupy pro zabezpečení dat a šifrování pomocí součástí možnosti Azure."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: yurid
ms.openlocfilehash: 5057c85ed3107921462a40045e716675ea41e4bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Doporučené postupy zabezpečení služby Azure Data a šifrování
Jeden z hello klíče toodata ochrany v cloudu hello je monitorování účtů pro hello možné stavy, která může nastat vaše data a jaké ovládací prvky jsou k dispozici pro tento stav. Za účelem hello dat Azure šifrování osvědčené postupy pro zabezpečení a doporučení hello bude kolem hello následující stavy dat:

* Na rest: To zahrnuje všechny informace, které kontejnerů, objektů úložiště a typy, které existují staticky na fyzickém médiu, být ho magnetické nebo optický disk.
* Na cestě: Při se přenosu dat mezi komponenty, umístění nebo programy, například přes síť hello přes service bus (z místní toocloud a naopak, včetně hybridních připojení, jako je například ExpressRoute), nebo při vstupu a výstupu proces, ho je představit jako za provozu.

V tomto článku se budeme zabývat kolekce dat Azure zabezpečení a šifrování osvědčené postupy. Tyto doporučené postupy jsou odvozeny od našich zkušeností s Azure data zabezpečení a šifrování a hello činnost zákazníků jako sami.

Pro každý osvědčený postup vysvětlíme:

* Jaké hello osvědčeným postupem je
* Proč chcete tooenable, že osvědčeným postupem
* Pokud selže tooenable hello osvědčený postup, co mohou být výsledek hello
* Osvědčeným postupem toohello možné alternativy
* Jak můžete získat informace tooenable hello osvědčený postup

Tento článek zabezpečení dat v Azure a osvědčené postupy pro šifrování je založen na konsenzu stanovisko a možnostech platformy Azure a sady funkcí, které jsou ve hello dobu, kdy byla zapsána v tomto článku. Názory a technologie mění v čase a budou v tomto článku v pravidelných intervalech tooreflect aktualizovat tyto změny.

Azure data zabezpečení a šifrování osvědčené postupy popsané v tomto článku patří:

* Vynutit ověřování Multi-Factor authentication
* Řízení přístupu (RBAC) na základě role pomocí
* Šifrování virtuálních počítačů Azure
* Použití modelů zabezpečení hardwaru
* Spravovat zabezpečení pracovních stanic
* Zapnout šifrování dat SQL
* Ochranu přenášených dat
* Vynutit šifrování dat na úrovni souborů

## <a name="enforce-multi-factor-authentication"></a>Vynutit ověřování Multi-Factor Authentication
Hello prvním krokem při přístupu k datům a řízení v nástroji Microsoft Azure je tooauthenticate hello uživatelem. [Azure Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md) je metoda ověření identity uživatele pomocí jiné metody než jenom uživatelské jméno a heslo. Tato metoda ověřování pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení.

Povolením Azure MFA pro uživatele přidáte druhou vrstvu zabezpečení toouser přihlášení a transakce. V takovém případě transakci může získávat přístup k dokumentu umístěná na souborovém serveru nebo ve vaší službě SharePoint Online. Azure MFA také pomáhá IT tooreduce hello pravděpodobnost, že ohrožené pověření budou mít přístup tooorganization data.

Například: Pokud vynutit Azure MFA pro uživatele a konfigurovat toouse telefonního hovoru nebo textové zprávy jako ověřování, pokud dojde k ohrožení bezpečnosti přihlašovací údaje hello uživatele, nebude útočník hello být schopný tooaccess jakémukoli prostředku, protože mu nebude mít přístup toouser phone. Organizace, které nepřidávejte tento další vrstvu ochrany identit budou náchylnější pro útoku krádeží přihlašovacích údajů, což může vést toodata ohrožení.

Jeden alternativní pro organizace, které mají tookeep hello ověřování řízení místní je toouse [Azure Multi-Factor Authentication Server](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), označované taky jako MFA na místě. Pomocí této metody bude stále možné tooenforce služba Multi-Factor authentication, a zajistit přitom ochranu hello MFA místní server.

Další informace o Azure MFA, přečtěte si článek hello [Začínáme s Azure Multi-Factor Authentication v cloudu hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Řízení přístupu (RBAC) na základě Role pomocí
Omezit přístup podle hello [potřebovat tooknow](https://en.wikipedia.org/wiki/Need_to_know) a [nejnižší oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) Principy zabezpečení. To je nutné pro organizace, které mají tooenforce zásady zabezpečení pro přístup k datům. Azure na základě rolí řízení přístupu (RBAC) může být použité tooassign oprávnění toousers, skupinám a aplikacím v určitých oboru. předplatné, skupinu prostředků nebo jediný zdroj, může být Hello rozsah přiřazení role.

Můžete využít [předdefinované role RBAC](../active-directory/role-based-access-built-in-roles.md) v Azure tooassign oprávnění toousers. Zvažte použití *Přispěvatel účet úložiště* pro operátorům cloudu, které je třeba toomanage účty úložiště a *Classic Přispěvatel účet úložiště* role toomanage klasické účty úložiště. Operátoři cloudu, které potřebuje toomanage virtuální počítače a účet úložiště, zvažte přidání je příliš*Přispěvatel virtuálních počítačů* role.

Organizace, které nebudou vynucovat řízení přístupu dat s využitím funkcí, jako je RBAC může být poskytnutí další oprávnění, než je nezbytné pro své uživatele. To může způsobit ohrožení zabezpečení toodata tak, že někteří uživatelé s toodata přístup, který by neměl mít na prvním místě hello.

Další informace o Azure RBAC přečíst článek hello [řízení přístupu](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Šifrování virtuálních počítačů Azure
Pro mnoho společností [šifrování dat v klidovém stavu](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) je povinný krok k suverenity data o ochraně osobních údajů a dodržování předpisů a data. Azure Disk Encryption umožňuje tooencrypt správci IT Windows a Linux IaaS virtuálního počítače (VM) disky. Azure Disk Encryption využívá hello oborový standard BitLocker funkce systému Windows a DM-Crypt funkce hello Linux tooprovide svazku šifrování pro hello operačního systému a datové disky hello.

Můžete využít Azure Disk Encryption toohelp chránit a chrání vaše data toomeet vaše organizace požadavky na zabezpečení a dodržování předpisů. Organizace měli také zvážit použití šifrování toohelp zmírnit rizika související toounauthorized data access. Dále je doporučeno, šifrování jednotky předchozí toowriting citlivá data toothem.

Zkontrolujte že tooencrypt objemy dat Virtuálního počítače a spouštěcí svazek v pořadí tooprotect data uložená v účtu úložiště Azure. Zabezpečení hello šifrovacích klíčů a tajných klíčů s využitím [Azure Key Vault](../key-vault/key-vault-whatis.md).

Pro místní servery Windows zvažte následující osvědčené postupy šifrování hello:

* Použití [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) pro šifrování dat
* Ukládání informací pro obnovení ve službě AD DS.
* Pokud žádné obavy, že klíče Bitlockeru ohrožený, doporučujeme, abyste buď formátu hello jednotky tooremove všechny instance hello BitLocker metadata z jednotky hello nebo dešifrovat a znovu zašifrovat celou jednotku hello.

Organizace, které není vynuceno šifrování dat jsou vyšší pravděpodobnost toobe zveřejněné toodata problémy s integritou, jako jsou uživatelé škodlivý nebo neautorizovaných serverů krádež dat a dojde k ohrožení účty získání neoprávněného přístupu toodata ve formátu zrušte. Kromě těchto rizik musí prokázat společností, které mají toocomply s předpisy odvětví, že jsou péče a používáte správné zabezpečení hello, ovládací prvky zabezpečení dat tooenhance.

Další informace o Azure Disk Encryption přečíst článek hello [Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Použijte moduly hardwarového zabezpečení
Odvětví šifrování řešení využívají data tooencrypt tajných klíčů. Proto je důležité, aby tyto klíče jsou bezpečně uložené. Správa klíčů stane nedílnou součástí ochrany dat, vzhledem k tomu, že bude využít toostore tajných klíčů, které jsou používané tooencrypt data.

Používá Azure disk encryption [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) toohelp můžete řídit a spravovat disku šifrovacích klíčů a tajných klíčů v trezoru klíčů předplatného, a zajistit, že jsou všechna data v disky virtuálního počítače hello zašifrovaná přinejmenším ve vaší službě Azure úložiště. Měli byste použít Azure Key Vault tooaudit klíčů a použití zásad.

Existuje mnoho související toonot rizika spojená s potřebné ovládací prvky zabezpečení ve místní tooprotect hello tajné klíče, které byly použité tooencrypt vaše data. Pokud útočníci přístup toohello tajných klíčů, budou se moct toodecrypt hello dat a můžou mít přístup k informacím tooconfidential.

Další informace o obecná doporučení pro certifikát správy v Azure pomocí přečtení článku hello [správy certifikátů ve službě Azure: doporučené a nedoporučené postupy](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Další informace o Azure Key Vault najdete v tématu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Spravovat zabezpečení pracovních stanic
Koncový bod hello od hello valná většina hello útoky cíl hello koncového uživatele, se stane jednu primární bodů hello útoku. Pokud útočník ohrožuje hello koncový bod, mohl využívat hello uživatelské přihlašovací údaje toogain přístup tooorganization na data. Většina útoků koncového bodu je možné tootake výhod hello fakt, že koncoví uživatelé jsou správci v jejich místní pracovní stanice.

Tato rizika můžete omezit použitím zabezpečené správy pracovní stanice. Doporučujeme vám, že používáte [privilegovaného přístupu pracovní stanice (TLAPA)](https://technet.microsoft.com/library/mt634654.aspx) prostor pro útoky hello tooreduce v pracovních stanicích. Tyto pracovní stanice pro správu zabezpečení můžete zmírnit některé z těchto útoků zajistit vašich dat je bezpečnější. Ujistěte se, že toouse TLAPA tooharden a zámek dolů pracovní stanici. Jedná se důležitý krok tooprovide vysoké zabezpečení záruky pro citlivé účty, úlohy a ochranu dat.

Nedostatečná služby endpoint protection může ohrozit vaše data, zkontrolujte, zda tooenforce zásady zabezpečení na všech zařízeních, které jsou používané tooconsume data, bez ohledu na umístění dat hello (cloudové nebo místní).

Další informace o privilegovaný přístup k pracovní stanici přečíst článek hello [zabezpečení privilegovaného přístupu](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>Zapnout šifrování dat SQL
[Azure SQL Database transparentní šifrování dat](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) pomáhá chránit před ohrožením hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování hello databáze, přidružených záloh a soubory protokolu transakcí v klidovém stavu bez vyžadují aplikace toohello změny.  Šifrování TDE zašifruje úložiště hello celé databáze pomocí symetrického klíče volané hello šifrovací klíč databáze.

I v případě, že šifrované hello celý úložiště, je velmi důležité tooalso šifrování databáze sám sebe. Toto je implementace hello obrany v hloubka přístupu, ochrany dat. Pokud používáte [Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) a chcete tooprotect citlivá data například platebních karet nebo čísla sociálního pojištění, můžete šifrovat databáze s FIPS 140-2 ověřené 256 256bitového šifrování AES splňuje požadavky hello mnoho oborové standardy (například HIPAA, PCI).

Je důležité toounderstand, která soubory související příliš[rozšíření fondu vyrovnávací paměti](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) nejsou šifrují, pokud databáze je zašifrovaná pomocí šifrování TDE. Je nutné použít nástroje šifrování na úrovni systému souborů jako BitLocker nebo hello [systém souborů EFS](https://technet.microsoft.com/library/cc700811.aspx) (EFS) pro BPE související soubory.

Od autorizovaným uživatelem, jako je zabezpečení správce nebo správce databáze přístup hello data i v případě databáze hello je šifrován TDE, také postupujte podle doporučení hello níže:

* Ověřování serveru SQL na úrovni databáze hello
* Pomocí role RBAC ověřování Azure AD
* Uživatelé a aplikace by měla používat samostatné účty tooauthenticate. Tímto způsobem můžete omezit hello oprávnění udělená toousers a aplikace a snížení rizika hello škodlivých aktivit
* Implementace zabezpečení na úrovni databáze pomocí pevné databázové role (například db_datareader nebo db_datawriter), nebo můžete vytvořit vlastní role pro vaše aplikace toogrant výslovná oprávnění tooselected databázové objekty

Organizace, které nejsou pomocí šifrování na úrovni databáze může být více náchylný na útoky, které mohou ohrozit dat umístěných v databázích SQL.

Další informace o šifrování SQL TDE přečíst článek hello [transparentní šifrování dat s Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Ochranu přenášených dat
Základní součástí strategie ochrany dat by měly být ochrany dat během přenosu. Vzhledem k tomu, že data bude přesunutí a zpět z mnoho míst, hello obecné doporučení je vždy použít data tooexchange protokoly SSL/TLS v různých umístěních. V některých případech může být vhodné tooisolate hello celý komunikační kanál mezi místní a cloudové infrastruktury pomocí virtuální privátní sítě (VPN).

Pro přesun mezi vaší místní infrastruktury a Azure data měli byste zvážit příslušná bezpečnostní opatření, například HTTPS nebo VPN.

Pro organizace, které potřebují toosecure přístup z více tooAzure nacházejí na místních pracovní stanice, použijte [Azure site-to-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Organizace, které potřebují přístup toosecure z jedné pracovní stanice nachází místní tooAzure, použijte [Point-to-Site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Větších datových sad se dají přesunout přes vyhrazené vysokorychlostní propojení WAN, jako [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Pokud si zvolíte toouse ExpressRoute, můžete také šifrování dat hello hello úrovni aplikace pomocí [SSL/TLS](https://support.microsoft.com/kb/257591) nebo jiné protokoly pro zvýšení ochrany.

Pokud jsou interakci s Azure Storage prostřednictvím hello portálu Azure, všechny transakce dojít přes HTTPS. [Rozhraní API REST úložiště](https://msdn.microsoft.com/library/azure/dd179355.aspx) přes protokol HTTPS může být také použít toointeract s [Azure Storage](https://azure.microsoft.com/services/storage/) a [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organizace, které nesplní tooprotect přenášených dat budou náchylnější pro [útokům man-in-the-middle](https://technet.microsoft.com/library/gg195821.aspx), [odposlouchávání](https://technet.microsoft.com/library/gg195641.aspx) a zneužití relace. Tyto útoky lze získat přístup k datům tooconfidential hello prvním krokem.

Další informace o Azure VPN možnost přečíst článek hello [plánování a návrhu pro bránu VPN](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Vynutit šifrování dat na úrovni souborů
Další vrstvu zabezpečení, která může zvýšit úroveň zabezpečení pro vaše data hello je šifrování hello souboru samostatně, bez ohledu na umístění souboru hello.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) toohelp zásady šifrování, identity a autorizace používá zabezpečit soubory a e-mailu. Azure RMS funguje napříč více zařízeními – telefony, tablety a počítače pomocí ochrany v rámci vaší organizace i mimo vaši organizaci. Tato možnost je možné, protože Azure RMS přidá úroveň ochrany, která zůstává s hello data, i když opustí prostory vaší organizace.

Pokud používáte Azure RMS tooprotect vaše soubory, používáte standardní kryptografie s plnou podporu systému [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Pokud využíváte Azure RMS pro ochranu dat, máte hello záruku toho, že hello je ochrana stále hello souborem, i když je zkopírovaný toostorage, který není pod kontrolou hello oddělení IT, třeba do cloudového úložiště. Hello stejné dochází u souborů, které sdílejí prostřednictvím e-mailu, hello soubor je chráněn jako e-mailovou zprávu tooan přílohy, s pokyny, jak tooopen hello chráněné přílohy.

Při plánování pro Azure RMS přijetí doporučujeme hello následující:

* Nainstalujte hello [aplikace sdílení RMS](https://technet.microsoft.com/library/dn339006.aspx). Tato aplikace se integruje s Office aplikace nainstalováním Office doplňku tak, aby uživatelé mohli snadno chránit soubory přímo.
* Nakonfigurujte aplikace a toosupport služby Azure RMS
* Vytvoření [vlastní šablony](https://technet.microsoft.com/library/dn642472.aspx) , podle vlastních podnikových požadavků. Příklad: šablonu pro horní tajných dat, která má být použita v všechny hlavní tajný klíč související s e-mailů.

Organizace, které jsou slabé na [klasifikace dat](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) a ochranu souborů může být náchylnější úniku toodata. Bez ochrany správný soubor organizace nebude mít tooobtain podnikových statistik, monitorování možného zneužití a zabránit toofiles škodlivým přístupem.

Další informace o Azure RMS pomocí přečtení článku hello [Začínáme se službou Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).
