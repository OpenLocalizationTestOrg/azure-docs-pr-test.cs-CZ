---
title: "aaaHigh dostupnosti a zotavení po havárii systému SAP HANA v Azure (velké instance) | Microsoft Docs"
description: "Vytvoření vysoké dostupnosti a plán pro zotavení po havárii SAP HANA v Azure (velké instance)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0c0967f54cf29bbb275eb7cda9d36608488add9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure 

Vysoká dostupnost a zotavení po havárii jsou důležité aspekty vaše důležité SAP HANA spuštěných na serverech Azure (velké instance). Jeho důležité toowork s SAP, vaše systémový Integrátor nebo architekt tooproperly Microsoft a implementujte hello strategie právo vysokou dostupnosti nebo zotavení po havárii. Je také plánovaného bodu obnovení hello důležité tooconsider a cíli času obnovení, které jsou specifické tooyour prostředí.

## <a name="high-availability"></a>Vysoká dostupnost

Společnost Microsoft podporuje SAP HANA vysokou dostupnost metody "mimo pole hello", mezi které patří:

- **Replikace úložiště:** hello systému úložiště možnost tooreplicate všechny tooanother umístění dat (v rámci, nebo odděleně od, hello stejného datového centra). SAP HANA funguje nezávisle na tuto metodu.
- **Replikace systému HANA:** hello replikace všech dat v samostatném systému SAP HANA SAP HANA tooa. plánovanou dobu obnovení Hello je minimalizován prostřednictvím replikace dat v pravidelných intervalech. SAP HANA podporuje asynchronní, synchronní režimy v paměti a synchronní (doporučeno pouze pro SAP HANA hello systémy, které jsou ve stejném datovém centru nebo méně než 100 KM od sebe). V aktuální návrhu hello HANA velké instance razítek, která replikaci systému HANA slouží pouze pro vysokou dostupnost.
- **Automatické převzetí služeb při selhání hostitele:** toouse řešení místní selhání obnovení jako alternativní toosystem replikace. Pokud hlavní uzel hello stane nedostupným, jsou v režimu Škálováním na více systémů nakonfigurovat jeden nebo více uzlů SAP HANA pohotovostní a SAP HANA automaticky převezme tooanother uzlu.

Další informace o vysoké dostupnosti SAP HANA najdete následující informace SAP hello:

- [SAP HANA vysokou dostupnost dokument White Paper](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [Příručka pro správu SAP HANA](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy Video o replikaci systému SAP HANA](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP podporu Poznámka #1999880 – nejčastější dotazy na replikaci systému SAP HANA](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP podporu Poznámka #2165547 – SAP HANA zálohování a obnovení v rámci prostředí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [Pro Exchange hardwaru minimální nebo nula. výpadků SAP podporu Poznámka #1984882 – pomocí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="disaster-recovery"></a>Zotavení po havárii

SAP HANA v Azure (velké instance) je k dispozici v dvě Azure oblasti v geopolitické oblasti. Mezi hello dva velké instance razítka dvou různých oblastí je přímého síťového připojení pro replikaci dat během zotavení po havárii. Hello replikaci dat hello je na základě infrastruktury úložiště. ve výchozím nastavení není provádí replikace Hello. Dokončí hello zákazníka konfigurace, které seřazené zotavení po havárii hello. V aktuální návrhu hello nelze použít replikaci HANA systému pro zotavení po havárii.

Však využít tootake hello zotavení po havárii, budete potřebovat toostart toodesign hello síťové připojení toohello dvou různých oblastech Azure. toodo tedy potřebujete připojení okruh Azure ExpressRoute z místního ve vaši hlavní oblast Azure a jiné okruh připojení z místní tooyour zotavení po havárii oblasti. Tato míra by mělo obsahovat situace, ve kterém k dokončení oblasti Azure, včetně umístění směrovače (MSEE) Microsoft enterprise edge, má problém.

Jako druhá míra se můžete připojit virtuální sítě Azure, které se připojují tooSAP HANA v Azure (velké instance) v jedné oblasti tooboth hello těchto okruhů ExpressRoute. Tato míra řeší případ, kdy jenom jeden hello MSEE umístění, které se připojí vaše místní umístění pomocí Azure ocitne mimo službu.

Hello následující obrázek znázorňuje hello optimální konfigurace pro zotavení po havárii:

![Optimální konfigurace pro zotavení po havárii](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

Hello optimální případ zotavení po havárii konfigurace sítě hello je toohave dvou okruhů ExpressRoute z místní toohello dvou různých oblastech Azure. Jeden okruh přejde tooregion #1 spuštěna instance produkční. druhý okruh ExpressRoute Hello přejde tooregion #2, spuštěné některé instance HANA nevýrobní prostředí. (To je důležité, pokud celou oblast Azure, včetně hello MSEE a velkých instance razítka, přejde vypnout hello mřížky.)

Jako druhá míra hello různých virtuálních sítí jsou připojené toohello různé okruhy ExpressRoute, které jsou připojené tooSAP HANA v Azure (velké instance). Hello místo, kde MSEE selhává, nebo můžete snížit hello plánovaného bodu obnovení pro zotavení po havárii můžete obejít probereme později.

Hello další požadavky pro instalaci zotavení po havárii jsou:

- SAP HANA musí pořadí v Azure (velké instance) SKU systému hello stejnou velikost jako provozním SKU a nasaďte je do oblasti hello zotavení po havárii. Tato instance může být použité toorun test, izolovaného prostoru nebo QA HANA instance.
- Musíte uspořádat profil zotavení po havárii pro každou z vaší SAP HANA na SKU Azure (velké instance), které chcete toorecover v lokalitě hello zotavení po havárii, v případě potřeby. Tato akce vede toohello přidělení úložiště svazky, které jsou cílem hello hello replikace úložiště z vaší oblasti produkční do oblasti hello zotavení po havárii.

Po hello předcházející požadavky splňujete, je vaší zodpovědností toostart hello úložiště replikace. V infrastruktuře úložiště hello používá pro SAP HANA v Azure (velké instance) je základ hello replikace úložiště snímků úložiště. toostart hello zotavení po havárii replikace, je třeba tooperform:

- Snímek spouštěcích logické jednotky, jak je popsáno výše.
- Snímek související HANA svazků, jak je popsáno výše.

Po provedení tyto snímky počáteční replika hello svazky se nasadí na hello svazky, které jsou spojeny s profilem zotavení po havárii v oblasti hello zotavení po havárii.

Následně se nejnovější úložiště snímků hello používá každou hodinu tooreplicate hello rozdílů, které vývoji na hello svazky úložiště.

Hello plánovaného bodu obnovení, které je dosaženo pomocí této konfigurace je 60 minut too90. tooachieve lepší obnovení bodu v případě zotavení po havárii hello, kopie hello HANA zálohování transakčního protokolu ze SAP HANA na Azure (velké instance) toohello cíl jiné oblasti Azure. tooachieve tento cíl bodu obnovení hello následující:

1. Zálohování hello HANA transakce se přihlaste jako často možném příliš/hana / / zálohy protokolu.
2. Zkopírujte hello zálohování transakčního protokolu, po dokončení tooan Azure virtuální počítač (VM), který je ve virtuální síti, která je připojena toohello SAP HANA na serveru Azure (velké instance).
3. Z tohoto virtuálního počítače zkopírujte hello zálohování tooa virtuální počítač, který je ve virtuální síti v oblasti hello zotavení po havárii.
4. V této oblasti v hello virtuálního počítače zachovat hello transakce zálohy protokolu.

V případě havárie, po hello zotavení po havárii profil nasazený na skutečné serveru, zkopírujte zálohování transakčního protokolu hello z virtuálních počítačů toohello hello SAP HANA v Azure (velké instance), který je teď primárním serverem hello v oblasti hello zotavení po havárii, a Tyto zálohy obnovte. Toto obnovení je možné, protože stav hello HANA na discích hello zotavení po havárii je, že HANA snímku. Toto je hello posunutí bod pro další obnovení zálohy protokolu transakcí.

## <a name="backup-and-restore"></a>Zálohování a obnovení

Jeden z hello nejdůležitější aspekty toooperating databáze se ujistěte se, že hello databáze se dají chránit z různých závažné události. Tyto události může být způsobeno nic z přírodní katastrofy toosimple uživatele chyb.

Zálohování databáze, s hello možnost toorestore ho tooany bodu v čase (například před odstraněním někdo důležitá data), umožňuje obnovení tooa stavu, který je jako zavřete jako možným způsobem toohello bylo dříve, než dojde k přerušení hello.

Pro dosažení co nejlepších výsledků se musí provádět dva typy zálohování:

- Zálohování databáze
- Zálohování transakčního protokolu

Kromě zálohování databáze toofull provést na úrovni aplikace, můžete být i důkladnější provedením zálohy snímků úložiště. Provádění záloh protokolu je také důležité pro obnovení databáze hello (a tooempty hello protokoly z již potvrzené transakce).

SAP HANA v Azure (velké instance) nabízí dvě možnosti pro zálohování a obnovení:

- Provést sami (DIY). Po výpočtu tooensure dostatek místa na disku, proveďte úplné zálohování databáze a protokolu pomocí metody zálohování na disk (toothose disky). V průběhu času hello zálohy jsou zkopírované tooan účtu úložiště Azure (po nastavení Azure souborovém serveru s prakticky neomezené úložiště) nebo použít trezoru zálohování Azure nebo úložiště Azure uloží málo používaná data. Další možností je toouse nástroj ochrany dat třetích stran, jako je Commvault toostore hello zálohy, jakmile jsou zkopírovány tooa účet úložiště. Hello DIY možnost zálohování může být také nutné pro data, která potřebují toobe uložené delší dobu pro dodržování předpisů a auditování.
- Použijte hello zálohování a obnovení funkce, které hello podpůrné infrastruktuře SAP HANA v Azure (velké instance) poskytuje. Tato možnost plnit hello potřebu zálohování, a umožňuje téměř zastaralé (s výjimkou případů, kde jsou vyžadovány pro dodržování předpisů pro účely zálohování dat) ručního zálohování. Hello zbytek této části adresy hello zálohování a obnovení funkce, které nabízí s HANA (velké instance).

> [!NOTE]
> Hello snímku technologie, která je používána hello základní infrastruktury HANA (velké instance) má závislost na snímky SAP HANA. Snímky SAP HANA ve spojení s SAP HANA víceklientské databáze kontejnery nefungují. Tato metoda zálohování v důsledku toho nelze použít toodeploy SAP HANA víceklientské databáze kontejnery.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Pomocí snímků úložiště SAP HANA v Azure (velké instance)

infrastruktura úložiště Hello základní SAP HANA v Azure (velké instance) podporuje hello představu o úložiště snímků svazků. Zálohování a obnovení pro určitý svazek, podporují s hello následující aspekty:

- Místo zálohy databáze jsou na základě časté prováděné úložiště snímků svazků.
- Hello úložiště snímků zahájí SAP HANA snímku, než se provede hello úložiště snímků. Tento snímek SAP HANA je bod hello instalační program pro případné protokolu obnovení po obnovení snímku úložiště hello.
- V okamžiku hello kde hello úložiště snímků je proveden úspěšně je odstraněn hello SAP HANA snímek.
- Zálohy protokolů jsou často prováděné a uložená v hello protokolu zálohování svazku nebo v Azure.
- Pokud musíte obnovit databázi hello tooa určité bodu v čase, požadavku tooMicrosoft podporu Azure (produkční výpadek) nebo SAP HANA na Azure Service Management toorestore tooa určité úložiště snímků (například o plánované obnovení systému izolovaného prostoru tooits původního stavu).
- Hello SAP HANA snímek, který je součástí hello úložiště snímků je bod posunutí pro použití zálohy protokolu, které byly provedeny a uložené po pořízení snímku úložiště hello.
- Tyto zálohy protokolu se provádějí toorestore hello databáze back tooa určité bodu v čase.

Zadání hello zálohování\_název vytvoří snímek hello následujících svazků:

- Hana nebo dat
- Hana a protokolování
- Hana nebo protokolu\_zálohování (připojit jako zálohování do hana nebo protokolu)
- / sdílené Hana

### <a name="storage-snapshot-considerations"></a>Aspekty volby úložiště snímků

>[!NOTE]
>Úložiště snímků jsou _není_ poskytuje bezplatně, protože musí být přidělený dalšího volného místa.

konkrétní mechanismy Hello úložiště snímků pro SAP HANA v Azure (velké instance) patří:

- Konkrétní úložiště snímků (na hello bodu v čase, kdy se provede) spotřebovává velmi málo úložiště.
- Jako obsahu změny dat a hello obsah SAP HANA data, která soubory změnit na hello svazek úložiště musí hello snímku toostore hello původní blokování obsahu.
- Hello úložiště snímků nárůstu velikosti. existuje Hello delší hello snímku, bude hello větší hello úložiště snímků.
- Dobrý den další změny toohello svazku databáze SAP HANA průběhu životnosti hello úložiště snímku, bude hello větší hello místo spotřeba úložiště snímku hello.

SAP HANA v Azure (velké instance) se dodává s velikostí pevný svazek pro svazek protokolu a data SAP HANA hello. Provádění snímky tyto svazky eats do místo ve svazku, takže je vaše odpovědnosti tooschedule úložiště snímků (v rámci hello SAP HANA na proces Azure [velké instance]).

Hello následující části obsahují informace o provádění tyto snímky, včetně obecná doporučení:

- I když hello hardware tolerovat 255 snímků na svazku, důrazně doporučujeme, abyste zůstat dobře pod tuto hodnotu.
- Před provedením úložiště snímků, monitorování a sledování volného místa.
- Nižší číslo hello úložiště snímků podle volného místa. Může být nutné toolower hello počet snímků, které můžete zachovat, nebo může být nutné tooextend hello svazky. (Další úložiště můžete uspořádat v jednotkách 1 TB.)
- Během aktivity, například přesun dat do SAP HANA s nástroji pro migraci systému (s R3load nebo po obnovení databáze SAP HANA ze zálohy) důrazně doporučujeme nebude provádět všechny snímky úložiště. (Pokud migrace systému probíhá na nový systém SAP HANA, úložiště snímků nebude potřeba provést toobe.)
- Během větší uspořádání SAP HANA tabulek úložiště snímků je nutno Pokud je to možné.
- Úložiště snímků jsou možnosti pro hello zotavení po havárii požadovaných tooengaging SAP HANA v Azure (velké instance).

### <a name="setting-up-storage-snapshots"></a>Nastavení úložiště snímků

1. Zajistěte, aby Perl nainstalovanou v operačním systému Linux hello na serveru (velké instance) HANA hello.
2. Upravit/etc/ssh/ssh\_konfigurace tooadd hello řádku _Mac hmac-sha1_.
3. Vytvoření účtu SAP HANA zálohování uživatele na hello hlavní uzel pro každou instanci SAP HANA, spuštěný (pokud existuje).
4. Klient SAP HANA HDB Hello musí být nainstalován na všech serverech SAP HANA (velké instance).
5. Na serveru SAP HANA (velké instance) první hello každé oblasti musí být veřejný klíč vytvořený tooaccess hello základní infrastrukturu úložiště, která řídí vytváření snímku.
6. Zkopírujte skript hello azure\_hana\_backup.pl z umístění toohello/Scripts **hdbsql** Dobrý den instalace SAP HANA.
7. Kopírování hello HANABackupDetails.txt souboru z toohello/Scripts stejné umístění jako hello Perl skriptu.
8. Upravte soubor HANABackupDetails.txt hello v případě potřeby u specifikace hello odpovídající zákazníka.

### <a name="step-1-install-sap-hana-hdbclient"></a>Krok 1: Instalace SAP HANA HDBClient

Hello Linux nainstalován na SAP HANA v Azure (velké instance) obsahuje složky hello a skripty potřebné tooexecute SAP HANA úložiště snímků pro účely zálohování a zotavení po havárii. Je však vaše odpovědnosti tooinstall SAP HANA HDBclient při instalaci SAP HANA. (Microsoft nainstaluje hello HDBclient ani SAP HANA.)

### <a name="step-2-change-etcsshsshconfig"></a>Krok 2: Změnit/etc/ssh/ssh\_konfigurace

Změnit/etc/ssh/ssh\_konfigurace přidáním _Mac hmac-sha1_ řádek, jak je vidět tady:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>Krok 3: Vytvoření veřejný klíč

Na hello první SAP HANA na serveru Azure (velké instance) v každé oblasti Azure vytvořit infrastrukturu veřejných klíčů toobe používá tooaccess hello úložiště, takže můžete pořídit snímky. veřejný klíč Hello zajistí, že heslo není požadovaná toosign v úložišti toohello a že nejsou spravovány hesla pověření. V systému Linux na hello SAP HANA (velké instance) serveru spusťte následující příkaz toogenerate hello veřejný klíč hello:
```
  ssh-keygen –t dsa –b 1024
```
nové umístění Hello je _/root/.ssh/id\_dsa.pub. Nezadávejte skutečné přístupové heslo, jinak bude požadované tooenter hello heslo při každém přihlášení. Místo toho stiskněte **Enter** dvakrát tooremove hello zadejte požadavek přístupové heslo pro přihlášení.

Zkontrolujte, zda tento veřejný klíč hello byl opraven podle očekávání změnou too/root/.ssh/ složky a potom provádění hello toomake **ls** příkaz. Pokud se nachází hello klíč, můžete ji zkopírovat spuštěním hello následující příkaz:

![Veřejný klíč se zkopíruje spuštěním tohoto příkazu](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

V tomto okamžiku obraťte se na SAP HANA na Azure Service Management a zadejte klíč hello. Hello zástupce služeb používá hello veřejného klíče tooregister v hello základní infrastruktury úložiště.

### <a name="step-4-create-an-sap-hana-user-account"></a>Krok 4: Vytvoření účtu uživatele SAP HANA

Vytvořte uživatelský účet SAP HANA studia SAP HANA pro účely zálohování. Tento účet musí mít následující oprávnění hello: _správce zálohování_ a _katalogu čtení_. V tomto příkladu hello uživatelské jméno SCADMIN se vytvoří.

![Vytvoření uživatele v HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-hello-sap-hana-user-account"></a>Krok 5: Autorizace hello SAP HANA uživatelský účet

Autorizovat hello SAP HANA uživatelský účet (toobe používá skripty hello bez vyžadování ověřování při každém spuštění skriptu hello). Hello SAP HANA příkaz `hdbuserstore` umožňuje vytvoření hello SAP HANA uživatelský klíč, který je uložený na jeden nebo více uzlů SAP HANA. uživatelský klíč Hello také umožňuje hello uživatele tooaccess SAP HANA bez nutnosti toomanage hesla z v rámci hello skriptování proces, který je popsán později.

>[!IMPORTANT]
>Spuštění hello následující příkaz jako `_root_`. Hello skriptu, jinak hodnota nemůže pracovat správně.

Zadejte hello `hdbuserstore` příkaz takto:

![Zadejte příkaz hdbuserstore hello](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

V následujícím příkladu, kde je uživatel hello SCADMIN01 a název hostitele hello je lhanad01, hello hello příkaz je:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Spravujte všechny skriptování na jednom serveru pro škálování HANA instance. V tomto příkladu musí být pro každého hostitele tak, aby odráží hello hostitele, který je klíč související toohello změněn hello SAP HANA klíč SCADMIN01. To znamená, se mění hello účet záloh SAP HANA s hello instance počet hello HANA DB **lhanad**. Hello klíč musí mít oprávnění správce na hostiteli hello, které je přiřazen, a hello zálohování uživatele Škálováním na více systémů musí mít přístup práva tooall SAP HANA instance.
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

### <a name="step-6-copy-items-from-hello-scripts-folder"></a>Krok 6: Kopírovat položky ze složky/Scripts hello

Kopírování hello následující položky z hello/skriptů složky (obsažené na hello gold bitovou kopii instalace hello) toohello pracovní adresář pro **hdbsql**. Pro aktuální HANA instalace, tento adresář se /hana/shared/D01/exe/linuxx86\_64/hdb.
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
Zkopírujte následující položky, pokud používají Škálováním na více systémů hello nebo OLAP:
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
soubor HANABackupCustomerDetails.txt Hello je upravitelnými takto pro nasazení škálování. Je hello řízení a konfigurační soubor pro hello skript, který běží hello úložiště snímků. Byste měli obdržet hello _název zálohy úložiště_ a _úložiště IP adresu_ ze SAP HANA na Azure Service Management při nasazení vaší instance. Nelze upravit hello pořadí řazení, nebo mezery všech proměnných hello nebo skriptu hello správně spustit.

Pro škálování nasazení bude vypadat hello konfigurační soubor:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Created by customer using hdbuserstore
HANA Backup Name: SCADMIND01
```
Pro konfiguraci škálovaného na více systémů bude vypadat hello HANABackupCustomerDetailsBW.txt souboru:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 10.254.15.21
Node 1 HANA instance number: 01
Node 1 HANA Backup Name: SCADMIN01
Node 2 IP Address: 10.254.15.22
Node 2 HANA instance number: 02
Node 2 HANA Backup Name: SCADMIN02
Node 3 IP Address: 10.254.15.23
Node 3 HANA instance number: 03
Node 3 HANA Backup Name: SCADMIN03
Node 4 IP Address: 10.254.15.24
Node 4 HANA instance number: 04
Node 4 HANA Backup Name: SCADMIN04
Node 5 IP Address: 10.254.15.25
Node 5 HANA instance number: 05
Node 5 HANA Backup Name: SCADMIN05
Node 6 IP Address: 10.254.15.26
Node 6 HANA instance number: 06
Node 6 HANA Backup Name: SCADMIN06
Node 7 IP Address: 10.254.15.27
Node 7 HANA instance number: 07
Node 7 HANA Backup Name: SCADMIN07
Node 8 IP Address: 10.254.15.28
Node 8 HANA instance number: 08
Node 8 HANA Backup Name: SCADMIN08
```
>[!NOTE]
>V současné době pouze podrobnosti o 1 uzlu se používají v hello skutečné HANA úložiště snímků skriptu. Doporučujeme, abyste otestovali tooor přístup ze všech uzlů HANA tak, že pokud se hlavní uzel zálohování hello někdy změní, již zajistíte, že jiného uzlu může trvat jeho místo změnou hello podrobnosti v uzlu 1.

toocheck pro hello správné konfigurace v konfiguračním souboru hello nebo instancí HANA toohello správné připojení, spusťte některý z následujících skriptů hello:
- Pro škálování konfigurace (nezávislou na pracovním vytížení SAP):

 ```
testHANAConnection.pl
```
- Pro škálovatelnou konfiguraci:

 ```
testHANAConnectionBW.pl
```

Zajistěte, aby byl tuto instanci hello hlavní HANA servery HANA tooall vyžaduje přístup. Neexistují žádné parametry toohello skript, ale musíte dokončit hello odpovídající HANABackupCustomerDetails / HANABackupCustomerDetailsBW souboru skriptu toorun hello správně. Vzhledem k tomu, že se vrátí jenom hello prostředí příkaz kódy chyb, není možné pro hello skriptu tooerror zkontrolujte všechny instance. I tak hello skriptu zadejte některé užitečné komentáře toodouble kontrola.

toorun hello skriptu:
```
 ./testHANAConnection.pl
```
 Pokud skript hello úspěšně získá stav hello hello HANA instance, zobrazí zprávu, zda text hello HANA připojení bylo úspěšné.

Kromě toho je druhý typ skriptu můžete použít možnost toosign toocheck hello hlavní HANA instanci serveru v toohello úložiště. Před spuštěním hello azure\_hana\_zálohování (\_bw) PL skriptu, je třeba spustit skript další hello. Pokud svazek obsahuje žádné snímky, je možné toodetermine zda hello svazek je jednoduše prázdný nebo není ssh selhání tooobtain hello snímku podrobnosti. Z tohoto důvodu hello skript spustí dva kroky:

- Ověří, jestli tato konzola hello úložiště dostupné.
- Vytvoří snímek testu nebo fiktivní, pro každý svazek HANA instance.

Z tohoto důvodu je zahrnuta jako argument hello HANA instance. Znovu není možné tooprovide Chyba při ověřování pro připojení hello úložiště, ale hello skriptu poskytuje užitečné pomocné parametry, pokud se nezdaří spuštění hello.

spuštění skriptu Hello jako:
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
Nebo ji spustit jako:
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
skript Hello také zobrazí zprávu, že budete mít toosign v odpovídajícím způsobem tooyour nasadit úložiště klienta, který se věnuje hello logické jednotky (LUN) používané hello instance serveru, které vlastníte.

Před spuštěním hello první úložiště založené na snímek zálohy, spusťte hello další skripty toomake se, že tato konfigurace hello je správný.

Po spuštění těchto skriptů, můžete odstranit snímky hello spuštěním:
```
./removeTestStorageSnapshot.pl <hana instance>
```
Nebo
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="step-7-perform-on-demand-snapshots"></a>Krok 7: Provedení snímků na vyžádání

Provádět na vyžádání snímků (a také naplánovat pravidelné snímky pomocí procesu cron) podle postupu popsaného tady.

Pro škálování konfigurace spusťte následující skript hello:
```
./azure_hana_backup.pl lhanad01 customer 20
```
Konfigurace Škálováním na více systémů spusťte následující skript hello:
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
skript Škálováním na více systémů Hello provádí některé další kontroly toomake se, že všechny servery HANA je přístupná, a všechny instance HANA vrátí příslušný stav instance hello před pokračováním vytváření snímků SAP HANA nebo úložiště.

vyžadují se následující argumenty Hello:

- Hello HANA instance vyžadují zálohování.
- Předpona Hello snímku pro hello úložiště snímků.
- Hello počet snímků toobe zachovány pro konkrétní předponu hello.

```
./azure_hana_backup.pl lhanad01 customer 20
```

Hello spuštění skriptu hello vytvoří snímek hello úložiště v těchto tří různých fází:

- Spusťte HANA snímku.
- Spusťte snímku úložiště.
- Odeberte hello HANA snímku.

Spusťte skript hello voláním z hello HDB složku spustitelného souboru zkopírované do. Se zálohuje alespoň hello tyto svazky, ale také zálohuje jakýkoli svazek, který má název instance SAP HANA explicitní hello v název svazku hello.
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
Doba uchování Hello je výhradně spravovat, s hello počet snímků odeslána jako parametr při spuštění skriptu hello (například 20 uvedený výše). Hello množství času, proto je funkce hello dobu spuštění a hello počet snímků ve volání hello hello skriptu. Překročí-li hello počet snímků, které jsou zachovány hello číslo, které jsou pojmenované jako parametr v hello volání hello skriptu, hello nejstarší úložiště snímku tento popisek (v našem případě předchozí _vlastní_) je odstraněn, než se nový snímek spustit. To znamená hello číslo, kterou přiřadíte jako hello poslední parametr hello volání je hello číslo můžete použít toocontrol hello počet snímků.

>[!NOTE]
>Jakmile změníte hello štítek, hello počítání znovu spustí.

Je nutné tooinclude hello HANA instance název, který poskytuje SAP HANA na Azure Service Management jako argument jejich vytváření snímků v prostředích s více uzly. V prostředích s jedním uzlem hello název hello SAP HANA na jednotce Azure (velké instance) je dostačující, ale přesto se doporučuje název instance HANA hello.

Kromě toho můžete můžete zálohovat spouštěcí volumes\LUNs pomocí hello stejný skriptu. Je nutné zálohovat spouštěcí svazek alespoň jednou při prvním spuštění HANA, i když vám doporučujeme týdenní nebo noční plán zálohování pro spouštění v procesu cron. Spíše než přidat název instance SAP HANA, vložte _spouštěcí_ jako hello argument do skriptu hello následujícím způsobem:
```
./azure_hana_backup boot customer 20
```
Hello stejné zásady uchovávání informací je poskytované také toohello spouštěcí svazek. Použijte na vyžádání snímky, jak je popsáno dříve, pro zvláštní případy, například během upgradu SAP vylepšení balíčku (EHP), nebo když potřebujete toocreate snímku odlišné úložiště.

Doporučujeme vám tooperform naplánované úložiště snímků pomocí procesu cron a doporučujeme použít hello stejný skriptu pro všechny zálohy a zotavení po havárii potřebám (změny hello skriptu vstupy toomatch hello různé požadovaný čas zálohování). Tyto jsou všechny naplánované jinak v procesu cron v závislosti na jejich čas spuštění: hodinové, 12 hodin, denní nebo týdenní. plán cron Hello je navrženou toocreate úložiště snímků, které odpovídají hello dřív popsané uchování označování pro dlouhodobé odlehlého zálohování. Hello skript obsahuje příkazy tooback zálohování všech svazků produkčního, v závislosti na jejich požadovanou četnost (data a soubory protokolu jsou zálohovány každou hodinu, zatímco hello spouštěcí svazek je zálohovat denně).

Hello položek v hello následující skript cron spouštět každou hodinu v hello desetinu minutu, každých 12 hodin v hello desetinu minutu a každý den v hello desetinu minutu. Hello cron, které úlohy se vytvářejí tak, že pouze jeden úložiště snímků SAP HANA probíhá během konkrétní hodina, tak, aby hello hodinové a denní zálohy se nevyskytují v hello, že stejný čas (12:10:00). toohelp optimalizovat vytváření snímků a replikace, SAP HANA na Azure Service Management poskytuje hello doporučená dobu toorun můžete své zálohy.

Hello výchozí cron plánování v /etc/crontab vypadá takto:
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
V předchozích pokynů cron hello získat hello HANA svazky (bez spouštěcí svazek) každou hodinu snímku s popiskem hello. Tyto snímků 66 zachovány. Kromě toho jsou uchovány 14 snímky s popiskem hello 12 hodin. Potenciálně získat HODINOVĚ snímky pro tři dny a snímky 12 hodin pro jiné čtyř dní, které vám celý týden snímků.

Plánování v rámci procesu cron může být složité, protože pouze jeden skript má být spuštěn v jakémkoli čase, pokud hello skripty rozkládají o několik minut. Pokud chcete denní zálohy pro dlouhodobé uchovávání, denní snímek je uložen spolu s snímku 12 hodin (s uchování počtem 7 každý) nebo hodinové snímku hello je postupný tootake místní 10 minut později. Pouze jeden denní snímek je uložen v svazek provozního hello.
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
frekvence Hello tady jsou jenom příklady. tooderive optimální počet snímků, hello používá následující kritéria:

- Požadavky v cíli času obnovení pro obnovení bodu v čase.
- Využití místa.
- Požadavky v rámci obnovování bodu cíl a cíli času obnovení pro zotavení po případné havárii.
- Závěrečné provádění záloh úplné databáze HANA proti disky. Vždy, když úplnou zálohu databáze na disky, nebo _backint_ rozhraní, se provádí, se nezdaří spuštění hello úložiště snímků. Pokud máte v plánu zálohování úplné databáze tooexecute nad úložiště snímků, ujistěte se, že během této doby je zakázána hello provádění úložiště snímků.

>[!IMPORTANT]
> použití Hello úložiště snímků pro SAP HANA zálohy je platný jenom v případě, že hello snímky jsou prováděny ve spojení s zálohy protokolu SAP HANA. Tyto zálohy nutné toobe možné toocover hello protokolu časových období mezi hello úložiště snímků. Pokud jste nastavili toousers závazků v okamžiku obnovení 30 dní, je třeba hello následující:

- Možnost tooaccess snímek úložiště, který je 30 dní.
- Souvislý protokolu zálohování přes hello posledních 30 dnů.

V rozsahu hello záloh protokolu vytvořte snímek také hello protokol o zálohování svazku. Ale být zda tooperform protokolu pravidelných záloh, aby bylo možné:

- Hello souvislý protokolu zálohy, musel tooperform v okamžiku obnovení.
- Zabránit s nedostatkem místa na svazku protokolu hello SAP HANA.

Jeden z kroků poslední hello je tooschedule SAP HANA zálohování protokolů v SAP HANA Studio. Hello SAP HANA protokol o zálohování cílového místa je vytvořena speciálně hello hana nebo protokolu\_zálohování svazku s /hana/log/backups hello přípojný bod.

![Plán zálohování SAP HANA přihlásí SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Můžete vybrat zálohování, které jsou častější, než každých 15 minut. Někteří uživatelé i provést zálohování protokolu každou minutu, i když není doporučeno probíhající _přes_ 15 minut.

posledním krokem Hello je tooperform na základě souborů zálohování (po hello počáteční instalace SAP HANA) toocreate jeden zálohování položku, která musí existovat v rámci zálohování katalogu hello. V opačném případě SAP HANA nelze zahájit zadaného protokolu zálohování.

![Proveďte zálohování toocreate na základě souborů jednu položku Zálohování](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-hello-number-and-size-of-snapshots-on-hello-disk-volume"></a>Monitorování hello počet a velikost snímků na diskovém svazku hello

Na svazku konkrétní úložiště můžete sledovat počet hello snímky a spotřebu úložiště hello snímků. Hello `ls` příkaz nezobrazí hello snímek adresáře nebo soubory. Ale hello operační systém Linux příkaz `du` nemá s hello následující příkazy:

- `du –sh .snapshot`poskytuje celkem všechny snímky v adresáři hello snímku.
- `du –sh --max-depth=1`Zobrazí všechny snímky, které jsou uloženy ve složce .snapshot hello a hello velikost jednotlivých snímků.
- `du –hc`poskytuje hello celkovou velikost používané všechny snímky.

Použijte tyto příkazy toomake jistotu, že nejsou hello snímky, které jsou provedeny a uloženy využívá všechny hello úložiště na svazcích hello.

### <a name="reducing-hello-number-of-snapshots-on-a-server"></a>Snižuje hello počet snímků na serveru

Jak je popsáno výše, můžete snížit počet hello určité popisky snímků, které jsou uloženy. Hello poslední dva parametry tooinitiate příkaz hello snímku se popisek a hello počet snímků, které chcete tooretain.
```
./azure_hana_backup.pl lhanad01 customer 20
```
V předchozím příkladu hello popisek snímku hello je _zákazníka_ a hello počet snímků, se tento štítek toobe uchovávají _20_. Jak můžete reagovat spotřeba toodisk místa, můžete chtít tooreduce hello počet uložené snímky. snadno Hello tooreduce hello počet snímků je toorun hello skriptu s hello poslední parametr sady too5:
```
./azure_hana_backup.pl lhanad01 customer 5
```
V důsledku spouštění skriptu hello s tímto nastavením, hello počet snímků, včetně nového úložiště hello snímku, je _5_.

 >[!NOTE]
 > Tento skript snižuje hello počet snímků, pouze pokud je více než jednu hodinu stará hello nejnovější předchozí snímek. skript Hello nedojde k odstranění snímků, které jsou menší než hodinu stará.

Tato omezení jsou související toohello volitelné zotavení po havárii funkce nabízené.

Pokud již nechcete toomaintain sadu snímky s tuto předponu, můžete spustit skript hello se _0_ jako hello uchování číslo tooremove všechny snímky odpovídající tuto předponu. Odebrání všechny snímky však může ovlivnit hello možnosti zotavení po havárii.

### <a name="recovering-toohello-most-recent-hana-snapshot"></a>Obnovení toohello nejnovější HANA snímku

V případě hello zaznamenáte rozbalovací produkční scénář, hello procesem obnovení ze snímku úložiště lze inicializovat jako zákazník incidentu s SAP HANA na Azure Service Management. Neočekávané scénáři může být vysokou naléhavost věci, pokud byla data odstraněna produkční systému a hello pouze způsobem tooretrieve hello dat je toorestore hello produkční databázi.

Na hello druhé straně, obnovení v okamžiku může být nízkou naléhavost a plánované předem dní. Toto obnovení s SAP HANA můžete naplánovat na Azure Service Management místo vyvolání problém s vysokou prioritou. Například může plánování tootry upgrade hello SAP softwaru s použitím nového balíčku vylepšení a potom potřebovat toorevert zpět tooa snímek, který představuje stav hello před provedením upgradu hello EHP.

Před vydáním hello žádost, musíte toodo určitou přípravu. SAP HANA týmu Azure Service Management může potom hello požadavek zpracovat a zadejte hello obnovit svazky. Potom je tooyou toorestore hello HANA databáze založené na hello snímky. Tady je způsob tooprepare pro hello požadavků:

>[!NOTE]
>Uživatelské rozhraní se můžou lišit od hello následující snímky obrazovky, v závislosti na hello verze SAP HANA, který používáte.

1. Rozhodněte, které toorestore snímku. Pokud jste se nedostanete budou obnoveny pouze hello hana nebo datový svazek.

2. Vypněte instanci HANA hello.

 ![Vypnout instanci HANA hello](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Odpojte hello datové svazky na každém uzlu HANA databáze. Hello obnovení snímku hello selže, pokud nejsou hello datové svazky.

 ![Odpojení hello datové svazky na každém uzlu databáze HANA](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Otevřete podporu Azure požadavek tooinstruct hello obnovení konkrétní snímku.

 - Při obnovení hello: SAP HANA na Azure Service Management může vás tooattend tooensure konferenční volání, která je ztratili žádná data.

 - Po obnovení hello: SAP HANA na Azure Service Management vás upozorní, jakmile byla obnovena hello úložiště snímků.

5. Po dokončení procesu obnovení hello znovu připojte všechny datové svazky.

 ![Znovu připojte všechny datové svazky](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Možnosti obnovení v rámci SAP HANA Studio, vyberte, pokud nepocházejí automaticky po opětovném tooHANA DB prostřednictvím SAP HANA Studio. Hello následující příklad ukazuje poslední HANA snímek toohello obnovení. Úložiště snímků Vloží jednu HANA snímku a když obnovujete toohello nejnovější úložiště snímků, měla by být hello nejnovější HANA snímku. (Pokud obnovujete tooolder úložiště snímků, je třeba toolocate pořízení snímku HANA hello podle hello čas hello úložiště snímků.)

 ![Vyberte možnosti obnovení studia SAP HANA](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Vyberte **obnovit hello tooa konkrétní data zálohování nebo úložiště snímek databáze**.

 ![okno "Zadejte typ obnovení" Hello](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Vyberte **zadejte zálohování bez katalogu**.

 ![okno "Zadejte umístění zálohy" Hello](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. V hello **cílový typ** seznamu, vyberte **snímku**.

 ![okno "Zadejte hello zálohování tooRecover" Hello](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Klikněte na tlačítko **Dokončit** proces obnovení toostart hello.

 ![Klikněte na tlačítko "Dokončit" proces obnovení toostart hello](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. databáze HANA Hello obnovení a obnovit toohello HANA snímek, který je součástí hello úložiště snímků.

 ![HANA databáze je obnovena a obnovené toohello HANA snímku](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-toohello-most-recent-state"></a>Obnovování stavu nejnovější toohello

Hello následující proces obnoví hello HANA snímek, který je součástí hello úložiště snímků. Poté obnoví hello protokolu zálohování toohello nejnovější stav transakce hello databáze před obnovením hello úložiště snímků.

>[!IMPORTANT]
>Než budete pokračovat, ujistěte se, že máte úplný a souvislý řetězec zálohy protokolu transakcí. Bez tyto zálohy nelze obnovit, aktuální stav databáze hello hello.

1. Proveďte kroky 1 – 6 hello předcházející postup v "Obnovení toohello nejnovější HANA snímek."

2. Vyberte **obnovit nejnovější stav tooits hello databáze**.

 ![Vyberte možnost "Obnovit nejnovější stav tooits hello databáze"](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Zadejte umístění hello hello nejnovější zálohy protokolu HANA. umístění Hello musí toocontain z nejnovější stav hello HANA snímku toohello všechny zálohy protokolu transakcí HANA.

 ![Zadejte umístění hello hello nejnovější zálohy protokolu HANA](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Vyberte zálohu jako základní z které toorecover hello databáze. V našem příkladu je to hello HANA snímku, která byla součástí hello úložiště snímků. (Jenom jeden snímek je uvedena v následující snímek obrazovky hello).

 ![Vyberte zálohu jako základní z které toorecover hello databáze](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Vymazat hello **použití rozdílové zálohy** zaškrtávací políčko, pokud nejsou k dispozici rozdílů mezi časem hello hello HANA snímku a nejnovější stav hello.

 ![Vymazat hello "Použití rozdílové zálohy" zaškrtávací políčko Pokud neexistují žádné rozdíly](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Na obrazovce Přehled hello, klikněte na tlačítko **Dokončit** postup obnovení toostart hello.

 ![Klikněte na tlačítko "Dokončit" na obrazovce Přehled hello](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-tooanother-point-in-time"></a>Obnovení tooanother bodu v čase
toorecover tooa bodu v čase mezi hello HANA snímku (součástí snímku úložiště hello) a ten, který je novější než hello HANA snímku v okamžiku obnovení hello následující:

1. Ujistěte se, že máte všechny zálohy protokolu transakcí z hello HANA snímku toohello dobu, kterou chcete toorecover.
2. Začněte hello postupem v části "Stavu nejnovější obnovování toohello."
3. V kroku 2 postupu hello v hello **zadejte typ obnovení** vyberte **následující toohello databáze hello obnovení bodu v čase**, zadejte hello bodu v čase a potom proveďte kroky 3 až 6.

## <a name="monitoring-hello-execution-of-snapshots"></a>Monitorování provádění hello snímků

Je třeba spuštění hello toomonitor úložiště snímků. zapíše výstupní soubor tooa Hello skript, který provede snímku úložiště a uloží ho toohello stejné umístění jako hello tyto skripty. Samostatný soubor je napsán pro každý snímek. výstup Hello každého souboru jasně ukazuje hello různých fází, které se spustí skript snímku hello:

- Hledání hello svazky, které je třeba toocreate snímku
- Hledání hello snímky převzaty z těchto svazků
- Odstranění případné existující snímky toomatch hello počet snímků, které jste zadali
- Vytvoření snímku HANA
- Vytvoření snímku úložiště hello přes hello svazky
- Odstranění snímku HANA hello
- Přejmenování hello poslední snímek příliš**.0**

Hello nejdůležitější část skriptu hello jsou tyto:
```
**********************Creating HANA snapshot**********************
Creating hello HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting hello HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Zobrazí se od této ukázky jak hello skriptu záznamy hello vytvoření snímku HANA hello. V případě hello Škálováním na více systémů je tento proces zahájit v hello hlavního uzlu. hlavní uzel Hello zahájí hello synchronní vytváření snímků hello na všechny uzly pracovního procesu hello. Potom pořízení snímku úložiště hello. Po úspěšném spuštění hello hello úložiště snímků je odstraněn hello HANA snímek.
