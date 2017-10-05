---
title: "Vysoká dostupnost a zotavení po havárii systému SAP HANA v Azure (velké instance) | Microsoft Docs"
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
ms.openlocfilehash: f95e944fc3ec3a831d97386443eb644420ae54dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure 

Vysoká dostupnost a zotavení po havárii jsou důležité aspekty vaše důležité SAP HANA spuštěných na serverech Azure (velké instance). Je důležité pro práci s SAP, vaše systémový Integrátor nebo Microsoft správně architektury a implementovat strategie právo vysokou dostupnosti nebo zotavení po havárii. Také je potřeba vzít v úvahu plánovaného bodu obnovení a cíli času obnovení, které jsou specifické pro vaše prostředí.

## <a name="high-availability"></a>Vysoká dostupnost

Společnost Microsoft podporuje SAP HANA vysokou dostupnost metody "se na pole a", mezi které patří:

- **Replikace úložiště:** schopnost systému úložiště replikovat všechna data do jiného umístění (v rámci, nebo odděleně od stejného datového centra). SAP HANA funguje nezávisle na tuto metodu.
- **Replikace systému HANA:** replikace všech dat v SAP HANA na samostatném systému SAP HANA. Plánovanou dobu obnovení je minimalizován prostřednictvím replikace dat v pravidelných intervalech. SAP HANA podporuje asynchronní, synchronní v paměti a synchronní režimy (doporučeno pouze pro SAP HANA systémy, které jsou ve stejném datovém centru nebo méně než 100 KM od sebe). V aktuální návrhu HANA velké instance razítka replikaci systému HANA slouží pouze pro vysokou dostupnost.
- **Automatické převzetí služeb při selhání hostitele:** místní selhání obnovení řešení použít jako alternativu k replikaci systému. Pokud hlavní uzel stane nedostupným, jsou v režimu Škálováním na více systémů nakonfigurovat jeden nebo více uzlů SAP HANA pohotovostní a SAP HANA automaticky převezme jiný uzel.

Další informace o vysoké dostupnosti SAP HANA najdete následující informace SAP:

- [SAP HANA vysokou dostupnost dokument White Paper](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [Příručka pro správu SAP HANA](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy Video o replikaci systému SAP HANA](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP podporu Poznámka #1999880 – nejčastější dotazy na replikaci systému SAP HANA](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP podporu Poznámka #2165547 – SAP HANA zálohování a obnovení v rámci prostředí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [Pro Exchange hardwaru minimální nebo nula. výpadků SAP podporu Poznámka #1984882 – pomocí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="disaster-recovery"></a>Zotavení po havárii

SAP HANA v Azure (velké instance) je k dispozici v dvě Azure oblasti v geopolitické oblasti. Mezi dvěma velké instance razítka ze dvou různých oblastech je přímého síťového připojení pro replikaci dat během zotavení po havárii. Replikace dat je na základě infrastruktury úložiště. Ve výchozím nastavení se provádí replikace. Dokončí konfigurace zákazníka, které seřazené zotavení po havárii. V aktuální návrhu HANA systému replikace nelze použít pro zotavení po havárii.

Však využít výhod zotavení po havárii, je nutné spustit návrhu připojení k síti na dvou různých oblastech Azure. To pokud chcete udělat, je třeba připojení okruh Azure ExpressRoute z místního v vaši hlavní oblast Azure a jiný okruh připojení z lokálních pro vaši oblast zotavení po havárii. Tato míra by mělo obsahovat situace, ve kterém k dokončení oblasti Azure, včetně umístění směrovače (MSEE) Microsoft enterprise edge, má problém.

Jako druhá míra se můžete připojit virtuální sítě Azure, která se připojují k SAP HANA v Azure (velké instance) v jedné oblasti do obou těchto okruhy ExpressRoute. Tato míra řeší případ, kdy jenom jedna z umístění MSEE, který se připojí vaše místní umístění pomocí Azure ocitne mimo službu.

Následující obrázek znázorňuje optimální konfigurace pro zotavení po havárii:

![Optimální konfigurace pro zotavení po havárii](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

Optimální případ zotavení po havárii konfigurace sítě je mít dva okruhy ExpressRoute z místního na dvou různých oblastech Azure. Jeden okruh přejde na oblast #1 spuštěna instance produkční. Druhý okruh ExpressRoute přejde na oblast #2, spuštěné některé instance HANA nevýrobní prostředí. (To je důležité, pokud celou oblast Azure, včetně MSEE a velkých instance razítka, přejde vypnout mřížky.)

Jako druhá míra jsou různých virtuálních sítí připojené k různým okruhy ExpressRoute, které jsou připojené k SAP HANA v Azure (velké instance). Místo, kde MSEE selhává, nebo můžete snížit plánovaného bodu obnovení pro zotavení po havárii můžete obejít probereme později.

Další požadavky pro instalaci zotavení po havárii jsou:

- Musí pořadí SAP HANA na Azure (velké instance) SKU stejnou velikost jako provozním SKU a nasaďte je do oblasti zotavení po havárii. Tyto instance slouží ke spuštění testu, izolovaného prostoru nebo QA HANA instance.
- Musíte pro každou z vaší SAP HANA na SKU Azure (velké instance), které chcete obnovit v lokalitě zotavení po havárii v případě potřeby uspořádat profil zotavení po havárii. Tato akce vede k přidělení úložiště svazky, které jsou cílem replikace úložiště z vaší oblasti produkční do oblasti zotavení po havárii.

Pokud jsou splněny požadavky na předchozí je vaší povinností spustit replikaci úložiště. V infrastruktuře úložiště používá pro SAP HANA v Azure (velké instance) je základ replikace úložiště snímků úložiště. Pokud chcete spustit replikaci zotavení po havárii, budete muset provést:

- Snímek spouštěcích logické jednotky, jak je popsáno výše.
- Snímek související HANA svazků, jak je popsáno výše.

Po provedení těchto snímků počáteční repliky svazků se nasadí na svazcích, které jsou spojeny s profilem zotavení po havárii v oblasti zotavení po havárii.

Následně se používá nejnovější snímku úložiště každou hodinu pro replikaci rozdílů, které vývoji na svazky úložiště.

Cíl bodu obnovení, které je dosaženo pomocí této konfigurace se od 60 až 90 minut. K dosažení lepšího plánovaného bodu obnovení v případě zotavení po havárii, zkopírujte zálohování transakčního protokolu HANA z SAP HANA v Azure (velké instance) oblasti Azure. K dosažení tohoto cíle bodu obnovení, postupujte takto:

1. Zálohování transakce HANA protokolu možné se často jako /hana/log/backup.
2. Zkopírujte zálohování transakčního protokolu, po ukončení práce do Azure virtuálního počítače (VM), který je ve virtuální síti, která je připojena k SAP HANA na serveru Azure (velké instance).
3. Z tohoto virtuálního počítače zkopírujte zálohování na virtuální počítač, který je ve virtuální síti v oblasti zotavení po havárii.
4. Zachovat transakce zálohy protokolu v této oblasti ve virtuálním počítači.

V případě havárie po nasazení profilu zotavení po havárii na skutečné serveru, zkopírujte zálohování transakčního protokolu z virtuálního počítače do SAP HANA v Azure (velké instance), který je teď primárním serverem v oblasti zotavení po havárii a ty obnovení zálohování. Toto obnovení je možné, protože stav HANA na discích zotavení po havárii je, že HANA snímku. Toto je posunutí bod pro další obnovení zálohy protokolu transakcí.

## <a name="backup-and-restore"></a>Zálohování a obnovení

Jedním z nejdůležitějších aspektů do provozní databáze je Ujistěte se, že databáze se dají chránit z různých závažné události. Tyto události může být způsobeno nic z přírodní katastrofy chyby jednoduché uživatele.

Zálohování databáze, možnost obnovit do libovolného bodu v čase (například před odstraněním někdo důležitá data), umožňuje obnovení do stavu, který je co nejblíže k způsob, jakým ho byl před došlo k narušení.

Pro dosažení co nejlepších výsledků se musí provádět dva typy zálohování:

- Zálohování databáze
- Zálohování transakčního protokolu

Kromě zálohování úplné databáze provést na úrovni aplikace můžete být i důkladnější provedením zálohy snímků úložiště. Provádění záloh protokolu je také důležité k obnovení databáze (a pro prázdný protokoly z již potvrzené transakce).

SAP HANA v Azure (velké instance) nabízí dvě možnosti pro zálohování a obnovení:

- Provést sami (DIY). Po můžete vypočítat zajistit dostatek místa na disku, proveďte úplné zálohování databáze a protokolu pomocí metody zálohy disku (na tyto disky). V čase zálohování se zkopírují do účtu úložiště Azure (po nastavení Azure souborovém serveru s prakticky neomezené úložiště), nebo použijte trezoru zálohování Azure nebo úložiště Azure uloží málo používaná data. Další možností je použít nástroj ochrany dat třetích stran, jako je například Commvault, k ukládání záloh potom, co se zkopíruje na účet úložiště. DIY možnost zálohování může být také nutné pro data, která je potřeba uchovat po delší dobu pro dodržování předpisů a auditování.
- Použít funkci zálohování a obnovení funkce, která poskytuje základní infrastruktura SAP HANA v Azure (velké instance). Tuto možnost splňuje potřeby pro zálohování a ručního zálohování umožňuje téměř zastaralé (s výjimkou případů, kde jsou vyžadovány pro dodržování předpisů pro účely zálohování dat). Zbývající část tohoto oddílu řeší funkci zálohování a obnovení, které nabízí s HANA (velké instance).

> [!NOTE]
> Snímek technologie, která je používána základní infrastruktura HANA (velké instance) má závislost na snímky SAP HANA. Snímky SAP HANA ve spojení s SAP HANA víceklientské databáze kontejnery nefungují. Tato metoda zálohování v důsledku toho nelze použít k nasazení SAP HANA víceklientské databáze kontejnerů.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Pomocí snímků úložiště SAP HANA v Azure (velké instance)

Infrastrukturu úložiště základní SAP HANA v Azure (velké instance) podporuje představu o úložiště snímků svazků. Zálohování a obnovení pro určitý svazek, podporují se následující aspekty:

- Místo zálohy databáze jsou na základě časté prováděné úložiště snímků svazků.
- Úložiště snímků zahájí SAP HANA snímku, než se provede snímku úložiště. Tento snímek SAP HANA je bod instalační program pro případné protokolu obnovení po obnovení snímku úložiště.
- V místě, kde je snímku úložiště byl úspěšně proveden je odstraněn snímek SAP HANA.
- Zálohy protokolů jsou často prováděné a uložená v zálohování svazku protokolu nebo v Azure.
- Pokud databázi musíte je obnovit do určité míry v čase, požadavku na podporu společnosti Microsoft Azure (produkční výpadek) nebo SAP HANA na Azure Service Management k obnovení určité úložiště snímků (například plánované obnovení systému izolovaného prostoru pro jeho původní stav).
- SAP HANA snímek, který je součástí úložiště snímků je bod posunutí pro použití zálohy protokolu, které byly provedeny a uložené po pořízení snímku úložiště.
- Tyto zálohy protokolů jsou přesměrováni na Obnovit databázi zpět do určité míry v čase.

Určení zálohování\_název vytvoří snímek následujících svazků:

- Hana nebo dat
- Hana a protokolování
- Hana nebo protokolu\_zálohování (připojit jako zálohování do hana nebo protokolu)
- / sdílené Hana

### <a name="storage-snapshot-considerations"></a>Aspekty volby úložiště snímků

>[!NOTE]
>Úložiště snímků jsou _není_ poskytuje bezplatně, protože musí být přidělený dalšího volného místa.

Konkrétní mechanismů úložiště snímků pro SAP HANA v Azure (velké instance) patří:

- Konkrétní úložiště snímků (v bodě v čase, kdy se provede) spotřebovává velmi málo úložiště.
- Jako obsahu změny dat a obsah SAP HANA data, která soubory změnit na svazek úložiště je potřeba uložit původní obsah bloku snímku.
- Úložiště snímků nárůstu velikosti. Čím delší snímku existuje, tím větší bude snímku úložiště.
- Další změny provedené v průběhu životnosti úložiště snímku, čím vyšší stane spotřeby prostor úložiště snímku svazku databáze SAP HANA.

SAP HANA v Azure (velké instance) se dodává s velikostí pevný svazek pro SAP HANA svazek protokolu a data. Provádění snímky tyto svazky eats do místo ve svazku, takže je vaší povinností při plánování úložiště snímků (v rámci SAP HANA na proces Azure [velké instance]).

Následující části obsahují informace pro provedení tyto snímky, včetně obecná doporučení:

- I když hardware tolerovat 255 snímků na svazku, důrazně doporučujeme, abyste zůstat dobře pod tuto hodnotu.
- Před provedením úložiště snímků, monitorování a sledování volného místa.
- Nižší počet snímků úložiště podle volného místa. Možná budete muset snížit počet snímků, které můžete zachovat, nebo možná budete muset rozšířit svazky. (Další úložiště můžete uspořádat v jednotkách 1 TB.)
- Během aktivity, například přesun dat do SAP HANA s nástroji pro migraci systému (s R3load nebo po obnovení databáze SAP HANA ze zálohy) důrazně doporučujeme nebude provádět všechny snímky úložiště. (Pokud migrace systému probíhá na nový systém SAP HANA, úložiště snímků nebude muset provést.)
- Během větší uspořádání SAP HANA tabulek úložiště snímků je nutno Pokud je to možné.
- Úložiště snímků jsou předpokladem pro zapojení možnosti zotavení po havárii SAP HANA v Azure (velké instance).

### <a name="setting-up-storage-snapshots"></a>Nastavení úložiště snímků

1. Zajistěte, aby Perl nainstalovanou v operačním systému Linux na serveru HANA (velké instance).
2. Upravit/etc/ssh/ssh\_konfigurace přidat řádek _Mac hmac-sha1_.
3. Vytvoření účtu uživatele zálohování SAP HANA na hlavní uzel pro každou instanci SAP HANA, spuštěný (pokud existuje).
4. Klient SAP HANA HDB musí být nainstalován na všech serverech SAP HANA (velké instance).
5. Na prvním serveru SAP HANA (velké instance) každou oblast, musí být vytvořený veřejný klíč pro přístup k podkladové infrastruktury úložiště, který řídí vytváření snímku.
6. Zkopírujte skript azure\_hana\_backup.pl z/Scripts umístění **hdbsql** instalace SAP HANA.
7. Zkopírujte soubor HANABackupDetails.txt z/Scripts do stejného umístění jako skript Perlu.
8. Upravte soubor HANABackupDetails.txt v případě potřeby u specifikace odpovídající zákazníka.

### <a name="step-1-install-sap-hana-hdbclient"></a>Krok 1: Instalace SAP HANA HDBClient

Linux nainstalován na SAP HANA v Azure (velké instance) obsahuje složky a skripty potřebné k provedení SAP HANA úložiště snímků pro účely zálohování a zotavení po havárii. Je však za to, že instalaci SAP HANA HDBclient, když instalujete SAP HANA. (Microsoft nainstaluje HDBclient ani SAP HANA.)

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

Na první SAP HANA na serveru Azure (velké instance) v každé oblasti Azure vytvořte veřejný klíč, který se má použít pro přístup k infrastruktury úložiště, takže můžete vytvářet snímky. Veřejný klíč zajistí, že pro přihlášení k úložišti není vyžadováno heslo a že nejsou spravovány heslo pověření. V systému Linux na serveru SAP HANA (velké instance) spusťte následující příkaz pro vytvoření veřejný klíč:
```
  ssh-keygen –t dsa –b 1024
```
Nové umístění je _/root/.ssh/id\_dsa.pub. Nezadávejte skutečné přístupové heslo, jinak bude nutné zadat heslo při každém přihlášení. Místo toho stiskněte **Enter** dvakrát možnosti odeberete požadavek zadejte přístupové heslo pro přihlášení.

Zkontrolujte, že veřejný klíč byl opraven podle očekávání změnou /root/.ssh/ složky a potom provádění **ls** příkaz. Pokud je klíč přítomen, můžete ji zkopírovat tak, že spustíte následující příkaz:

![Veřejný klíč se zkopíruje spuštěním tohoto příkazu](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

V tomto okamžiku obraťte SAP HANA na Azure Service Management a zadat klíč. Zástupce služeb používá veřejný klíč a zaregistrujte ho v podkladové infrastruktury úložiště.

### <a name="step-4-create-an-sap-hana-user-account"></a>Krok 4: Vytvoření účtu uživatele SAP HANA

Vytvořte uživatelský účet SAP HANA studia SAP HANA pro účely zálohování. Tento účet musí mít následující oprávnění: _správce zálohování_ a _katalogu čtení_. V tomto příkladu uživatelské jméno SCADMIN se vytvoří.

![Vytvoření uživatele v HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Krok 5: Autorizace uživatelský účet SAP HANA

Autorizovat SAP HANA uživatelský účet (který se má použít skripty bez nutnosti autorizace pokaždé, když se skript spouští). Příkaz SAP HANA `hdbuserstore` umožňuje vytvoření SAP HANA uživatelský klíč, který je uložený na jeden nebo více uzlů SAP HANA. Uživatelský klíč také umožňuje uživateli přístup k SAP HANA bez nutnosti Správa hesel z v rámci skriptování procesu, který je popsán později.

>[!IMPORTANT]
>Spusťte následující příkaz jako `_root_`. Skript, jinak hodnota nemůže pracovat správně.

Zadejte `hdbuserstore` příkaz takto:

![Zadejte příkaz hdbuserstore](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

V následujícím příkladu, kde je uživatel SCADMIN01 a název hostitele je lhanad01, příkaz je:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Spravujte všechny skriptování na jednom serveru pro škálování HANA instance. V tomto příkladu musí být klíč SAP HANA SCADMIN01 změněn pro každého hostitele tak, aby odráží hostitele, která souvisí s klíč. To znamená, účet záloh SAP HANA se mění se počet instancí HANA databáze, **lhanad**. Klíč musí mít oprávnění správce na hostiteli, který je přiřazen, a zálohování uživatele Škálováním na více systémů, musíte mít přístupová práva na všechny instance SAP HANA.
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

### <a name="step-6-copy-items-from-the-scripts-folder"></a>Krok 6: Kopírovat položky ze složky/Scripts

Zkopírujte následující položky ze složky/Scripts (obsažené na bitovou kopii gold instalace) do pracovní adresář pro **hdbsql**. Pro aktuální HANA instalace, tento adresář se /hana/shared/D01/exe/linuxx86\_64/hdb.
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
Pokud používají Škálováním na více systémů nebo OLAP, zkopírujte následující položky:
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
Soubor HANABackupCustomerDetails.txt je upravitelnými takto pro nasazení škálování. Je ovládací prvek a konfigurační soubor pro skript, který běží úložiště snímků. Byste měli obdržet _název zálohy úložiště_ a _úložiště IP adresu_ ze SAP HANA na Azure Service Management při nasazení vaší instance. Pořadí, nelze upravit, řazení, nebo mezer všech proměnných, nebo skript nejde správně spustit.

Pro škálování nasazení bude vypadat konfiguračního souboru:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Created by customer using hdbuserstore
HANA Backup Name: SCADMIND01
```
Pro konfiguraci škálovaného na více systémů bude vypadat HANABackupCustomerDetailsBW.txt souboru:
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
>V současné době pouze podrobnosti o 1 uzlu se používají ve skutečné skriptu HANA úložiště snímků. Doporučujeme, abyste otestovali přístup do nebo ze všech uzlů HANA tak, že pokud se hlavní uzel zálohování někdy změní, již zajistíte, že jiného uzlu může trvat jeho místo změnou podrobnosti v uzlu 1.

Zkontrolujte správné konfigurace v konfiguračním souboru nebo správné připojení k instancím HANA, spusťte některý z následujících skriptů:
- Pro škálování konfigurace (nezávislou na pracovním vytížení SAP):

 ```
testHANAConnection.pl
```
- Pro škálovatelnou konfiguraci:

 ```
testHANAConnectionBW.pl
```

Zajistěte, aby hlavní instance HANA přístup na všechny požadované servery HANA. Neexistují žádné parametry skriptu, ale je třeba provést odpovídající HANABackupCustomerDetails / HANABackupCustomerDetailsBW souborů pro skript, který chcete spustit správně. Vzhledem k tomu, že se vrátí jenom kódy chyb prostředí příkaz, není možné skript tak, aby všechny instance Kontrola chyb. I tak skript zadejte některé užitečné komentáře můžete znovu zkontrolovat.

Pro spuštění skriptu:
```
 ./testHANAConnection.pl
```
 Pokud skript úspěšně získá stav instance HANA, zobrazí se zpráva, že HANA připojení bylo úspěšné.

Kromě toho je druhý typ skriptu, které můžete použít ke kontrole hlavní HANA instance serveru možnost přihlásit se do úložiště. Před spuštěním azure\_hana\_zálohování (\_bw) PL skriptu, je třeba spustit další skript. Pokud svazek obsahuje žádné snímky, je možné určit, zda je svazek jednoduše prázdný nebo není ssh získání podrobných informací o snímku se nezdařilo. Z tohoto důvodu se skript spustí dva kroky:

- Ověří, že konzole úložiště je dostupný.
- Vytvoří snímek testu nebo fiktivní, pro každý svazek HANA instance.

Z tohoto důvodu je zahrnuta jako argument HANA instance. Znovu není možné zajistit kontrolu chyb pro připojení k úložišti, ale skript poskytuje užitečné pomocné parametry, pokud se nezdaří spuštění.

Spuštění skriptu jako:
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
Nebo ji spustit jako:
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
Skript také zobrazí zprávu, že budete moci správně přihlásit ke klientovi nasazené úložiště, který je založen na logickou jednotku (LUN), které používá instance serveru, které vlastníte.

Před spuštěním první zálohy založené na snímku úložiště, spusťte další skripty a ujistěte se, že konfigurace je správná.

Po spuštění těchto skriptů, můžete spuštěním odstranit snímky:
```
./removeTestStorageSnapshot.pl <hana instance>
```
Nebo
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="step-7-perform-on-demand-snapshots"></a>Krok 7: Provedení snímků na vyžádání

Provádět na vyžádání snímků (a také naplánovat pravidelné snímky pomocí procesu cron) podle postupu popsaného tady.

Pro škálování konfigurace spusťte následující skript:
```
./azure_hana_backup.pl lhanad01 customer 20
```
Konfigurace Škálováním na více systémů spusťte následující skript:
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
Skript Škálováním na více systémů nepodporuje některé další kontrolu a ujistěte se, že všechny servery HANA je přístupná, a všechny instance HANA vrátí příslušný stav instance před pokračováním vytváření snímků SAP HANA nebo úložiště.

Následující argumenty jsou povinné:

- Instance HANA vyžadují zálohování.
- Předpona snímku pro úložiště snímku.
- Počet snímků, která se mají uchovat pro konkrétní předponu.

```
./azure_hana_backup.pl lhanad01 customer 20
```

Provádění skriptu vytvoří snímek úložiště v těchto tří různých fází:

- Spusťte HANA snímku.
- Spusťte snímku úložiště.
- Odeberte HANA snímku.

Spusťte skript voláním z HDB složku spustitelného souboru, který jste zkopírovali do. Zálohuje alespoň následujících svazků, ale také zálohuje jakýkoli svazek, který má explicitní název instance SAP HANA v název svazku.
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
Doba uchování je výhradně spravovat, s počet snímků odeslána jako parametr při spuštění skriptu (například 20 uvedený výše). Množství času, proto je funkce, doba provádění a počet snímků ve volání skriptu. Pokud počet snímků, které jsou zachovány překračuje počet, který je pojmenován jako parametr ve volání skriptu, nejstarší úložiště snímku tento popisek (v našem případě předchozí _vlastní_) je odstraněn před provedením nový snímek. To znamená počet poskytnout jako poslední parametr volání je číslo můžete použít k řízení počet snímků.

>[!NOTE]
>Jakmile změníte štítek, počítání znovu spustí.

Je nutné zahrnout název instance HANA, které poskytuje SAP HANA na Azure Service Management jako argument jejich vytváření snímků v prostředích s více uzly. V prostředích s jedním uzlem název SAP HANA na jednotce Azure (velké instance) je dostačující, ale přesto se doporučuje název instance HANA.

Kromě toho můžete spouštěcí volumes\LUNs pomocí stejného skriptu zálohovat. Je nutné zálohovat spouštěcí svazek alespoň jednou při prvním spuštění HANA, i když vám doporučujeme týdenní nebo noční plán zálohování pro spouštění v procesu cron. Spíše než přidat název instance SAP HANA, vložte _spouštěcí_ jako argument do skriptu následujícím způsobem:
```
./azure_hana_backup boot customer 20
```
Stejné zásady uchovávání informací je zaměřena na spouštěcí svazek. Použijte na vyžádání snímky, jak je popsáno dříve, pro zvláštní případy, například během upgradu SAP vylepšení balíčku (EHP), nebo pokud budete potřebovat pro vytvoření snímku odlišné úložiště.

Doporučujeme provést plánované úložiště snímků pomocí procesu cron a doporučujeme použít stejný skriptu pro všechny zálohy a zotavení po havárii potřebám (úprava vstupy skript tak, aby odpovídaly různých požadovaný čas zálohování). Tyto jsou všechny naplánované jinak v procesu cron v závislosti na jejich čas spuštění: hodinové, 12 hodin, denní nebo týdenní. Plán cron slouží k vytvoření snímků úložiště, které odpovídají dříve popsaných uchování štítky pro dlouhodobé odlehlého zálohování. Skript obsahuje příkazy zálohovat všechny svazky produkčního, v závislosti na jejich požadovanou četnost (data a soubory protokolu jsou zálohovány každou hodinu, zatímco spouštěcí svazek je zálohovat denně).

Položky v následující skript cron spouštět každou hodinu v desetinu minutu každých 12 hodin na desetinu minutu a každý den v desetinu minutu. Cron, které úlohy se vytvářejí tak, že pouze jeden úložiště snímků SAP HANA probíhá během konkrétní hodina, tak, aby hodinové a denní zálohy se nevyskytují ve stejnou dobu (12:10:00). Chcete-li optimalizovat vytváření snímků a replikace, SAP HANA na Azure Service Management poskytuje doporučené čas pro spuštění zálohování.

Cron výchozí plánování v /etc/crontab vypadá takto:
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
V předchozích pokynů cron získat HANA svazky (bez spouštěcí svazek) každou hodinu snímku s popiskem. Tyto snímků 66 zachovány. Kromě toho jsou uchovány 14 snímky s popiskem 12 hodin. Potenciálně získat HODINOVĚ snímky pro tři dny a snímky 12 hodin pro jiné čtyř dní, které vám celý týden snímků.

Plánování v rámci procesu cron může být složité, protože pouze jeden skript má být spuštěn v jakémkoli čase, pokud skripty rozkládají o několik minut. Pokud chcete denní zálohy pro dlouhodobé uchovávání, denní snímek je uložen spolu s snímku 12 hodin (s uchování počtem 7 každý) nebo hodinové snímku rozložena proběhla 10 minut později. Pouze jeden denní snímek je udržováno na produkční svazku.
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
Frekvence tady jsou jenom příklady. Odvození vaší optimální počet snímků, použijte následující kritéria:

- Požadavky v cíli času obnovení pro obnovení bodu v čase.
- Využití místa.
- Požadavky v rámci obnovování bodu cíl a cíli času obnovení pro zotavení po případné havárii.
- Závěrečné provádění záloh úplné databáze HANA proti disky. Vždy, když úplnou zálohu databáze na disky, nebo _backint_ rozhraní, se provádí, se nezdaří spuštění úložiště snímků. Pokud máte v plánu provést zálohování úplné databáze nad úložiště snímků, ujistěte se, že během této doby je zakázána provádění úložiště snímků.

>[!IMPORTANT]
> Používání úložiště snímků pro SAP HANA zálohy je platný jenom v případě, že snímky jsou prováděny ve spojení s zálohy protokolu SAP HANA. Tyto zálohy protokolu musí být schopný tak, aby pokrývalo časových období mezi snímky úložiště. Pokud jste nastavili závazek uživatelům v okamžiku obnovení 30 dní, budete potřebovat následující:

- Možnost pro přístup k snímek úložiště, který je 30 dní.
- Souvislý protokolu zálohování za posledních 30 dní.

V oblasti zálohy protokolů se vytvoření snímku objemu protokol o zálohování. Nezapomeňte však provést zálohování protokolu regulární, aby bylo možné:

- Mít zálohy souvislý protokolů, které jsou potřebné k obnovení bodu v čase.
- Zabránit spuštění nedostatek místa na svazku protokolu SAP HANA.

Jeden z poslední kroků je naplánovat zálohování protokolů SAP HANA v SAP HANA Studio. SAP HANA protokol o zálohování cílového místa je speciálně vytvořený hana nebo protokolu\_zálohování svazku s přípojný bod /hana/log/backups.

![Plán zálohování SAP HANA přihlásí SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Můžete vybrat zálohování, které jsou častější, než každých 15 minut. Někteří uživatelé i provést zálohování protokolu každou minutu, i když není doporučeno probíhající _přes_ 15 minut.

Posledním krokem je vytvoření jedné zálohování položku, která musí existovat v rámci katalogu zálohování provést zálohování na základě souborů (po počáteční instalaci SAP HANA). V opačném případě SAP HANA nelze zahájit zadaného protokolu zálohování.

![Vytvořit zálohu na základě souborů k vytvoření jedné položky zálohování](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Monitorování počtu a velikosti snímků na diskovém svazku

Na svazku konkrétní úložiště můžete sledovat počet snímků a spotřebu úložiště snímků. `ls` Příkaz nezobrazí snímek adresáře nebo soubory. Ale příkaz operační systém Linux `du` nemá pomocí následujících příkazů:

- `du –sh .snapshot`poskytuje celkem všechny snímky v adresáři snímku.
- `du –sh --max-depth=1`Zobrazí všechny snímky, které jsou uloženy ve složce .snapshot a velikosti jednotlivých snímků.
- `du –hc`poskytuje celkovou velikost používané všechny snímky.

Pomocí těchto příkazů a ujistěte se, že snímky, které jsou provedeny a uloženy nejsou využívá všechny úložiště na svazcích.

### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Omezení počtu snímků na serveru

Jak je popsáno výše, můžete snížit počet určité popisky snímků, které jsou uloženy. Poslední dva parametry příkazu zahájíte snímek jsou štítky a počet snímků, které chcete zachovat.
```
./azure_hana_backup.pl lhanad01 customer 20
```
V předchozím příkladu popisek snímku je _zákazníka_ a počet snímků s tímto štítkem pro zachování _20_. Jak můžete reagovat na využívání místa na disku, můžete snížit počet uložené snímky. Snadný způsob, jak snížit počet snímků, je pro spuštění skriptu s parametrem poslední nastavena na 5:
```
./azure_hana_backup.pl lhanad01 customer 5
```
V důsledku spuštění skriptu se toto nastavení, počet snímků, včetně nový snímek úložiště je _5_.

 >[!NOTE]
 > Tento skript snižuje počet snímků, pouze pokud je více než jednu hodinu stará nejnovější předchozí snímek. Skript nedojde k odstranění snímků, které jsou menší než hodinu stará.

Tato omezení se vztahují k zotavení po havárii volitelné funkce nabízené.

Pokud již nechcete udržovat sadu snímky s tuto předponu, můžete spustit skript s _0_ jako číslo uchování odebrat všechny snímky odpovídající tuto předponu. Odebrání všechny snímky však může ovlivnit možnosti zotavení po havárii.

### <a name="recovering-to-the-most-recent-hana-snapshot"></a>Obnovení na poslední snímek HANA

V případě, že dojde rozbalovací produkční scénář, procesem obnovení ze snímku úložiště lze inicializovat jako zákazník incidentu s SAP HANA na Azure Service Management. Neočekávané scénář může být vysokou naléhavost věci, pokud byla data odstraněna v produkční systému a je jediný způsob, jak načíst data obnovit produkční databázi.

Obnovení bodu v čase na druhé straně může být nízkou naléhavost a plánované předem dní. Toto obnovení s SAP HANA můžete naplánovat na Azure Service Management místo vyvolání problém s vysokou prioritou. Například je může být plánování Zkuste upgrade softwaru SAP použitím nový balíček vylepšení a pak je nutné vrátit zpět na snímek, který představuje stav před upgradem EHP.

Před vydáním žádost, budete muset provést určitou přípravu. SAP HANA týmu Azure Service Management můžete žádost zpracovat a poskytují obnovené svazky. Potom můžete se rozhodnout obnovit databázi HANA podle snímky. Tady je postup přípravy pro žádost:

>[!NOTE]
>Uživatelské rozhraní se můžou lišit od následující snímky obrazovky, v závislosti na verzi SAP HANA, který používáte.

1. Rozhodněte, které snímek pro obnovení. Pokud jste se nedostanete budou obnoveny pouze hana nebo datový svazek.

2. Vypněte instanci HANA.

 ![Vypnout instanci HANA](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Odpojte objemy dat v každém uzlu HANA databáze. Obnovení snímku se nezdaří, pokud nejsou datové svazky.

 ![Odpojte svazky dat v každém uzlu databáze HANA](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Otevřete žádost o podporu Azure dáte pokyn, aby obnovení snímku konkrétní.

 - Během obnovení: SAP HANA na Azure Service Management může vás k účasti na konferenci zajistit, že je ztratili žádná data.

 - Po obnovení: SAP HANA na Azure Service Management vás upozorní, jakmile byla obnovena snímku úložiště.

5. Po dokončení procesu obnovení je znovu připojte všechny datové svazky.

 ![Znovu připojte všechny datové svazky](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Možnosti obnovení v rámci SAP HANA Studio, vyberte, pokud nepocházejí automaticky po opětovném připojení k databázi HANA přes SAP HANA Studio. Následující příklad ukazuje obnovení poslední snímek HANA. Jeden snímek HANA vloží snímek úložiště a pokud obnovujete na poslední snímek úložiště, musí být poslední HANA snímku. (Pokud obnovujete do starší úložiště snímků, budete muset najít HANA snímku na základě doby pořízení snímku úložiště.)

 ![Vyberte možnosti obnovení studia SAP HANA](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Vyberte **obnovit databázi do konkrétních dat zálohování nebo úložiště snímků**.

 ![Okno "Zadejte typ obnovení"](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Vyberte **zadejte zálohování bez katalogu**.

 !["Zadejte umístění zálohy" okna](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. V **cílový typ** seznamu, vyberte **snímku**.

 ![Okno "Zadejte zálohu k obnovení"](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Klikněte na tlačítko **Dokončit** zahájíte proces obnovení.

 ![Klikněte na tlačítko "Dokončit" zahájíte proces obnovení](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. Databázi HANA je obnovit a obnovit HANA snímek, který je součástí úložiště snímku.

 ![Databáze HANA je obnovit a obnovit snímek HANA](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a>Obnovení na nejnovější stav

Následující proces obnoví HANA snímek, který je součástí úložiště snímku. Obnoví pak zálohování transakčního protokolu nejnovější stavu databázi před obnovením snímku úložiště.

>[!IMPORTANT]
>Než budete pokračovat, ujistěte se, že máte úplný a souvislý řetězec zálohy protokolu transakcí. Aktuální stav databáze nelze obnovit bez tyto zálohy.

1. Proveďte kroky 1 – 6 v předchozím postupu v "Obnovení poslední snímek HANA."

2. Vyberte **obnovit databázi do stavu nejnovější**.

 ![Vyberte možnost "Obnovit databázi do stavu poslední"](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Zadejte umístění poslední zálohy protokolu HANA. Umístění musí obsahovat všechny HANA zálohování transakčního protokolu ze snímku HANA nejnovější stav.

 ![Zadejte umístění poslední zálohy protokolu HANA](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Vyberte zálohu jako základ, ze kterého chcete obnovit databázi. V našem příkladu je to HANA snímek, který je zahrnutý ve snímku úložiště. (Jen jeden snímek je uveden na následujícím snímku obrazovky).

 ![Vyberte zálohu jako základ, ze kterého chcete obnovit databázi](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Vymazat **použití rozdílové zálohy** zaškrtávací políčko, pokud nejsou k dispozici rozdílů mezi časem HANA snímku a nejnovější stav.

 ![Zrušte zaškrtnutí políčka "Použití rozdílové zálohy", pokud neexistuje žádné rozdíly](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Na obrazovce Přehled klikněte na tlačítko **Dokončit** spustíte proces obnovení.

 ![Klikněte na tlačítko "Dokončit" na obrazovce Přehled](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a>Obnovení do jiného bodu v čase
Obnovit do bodu v čase mezi HANA snímku (zahrnutá ve snímku úložiště) a ten, který je novější než obnovení bodu v čase HANA snímku, postupujte takto:

1. Ujistěte se, že máte všechny zálohování transakčního protokolu ze snímku HANA na dobu, kterou chcete obnovit.
2. Začněte postup v části "Obnovení poslední stavu."
3. V kroku 2 postupu, v **zadejte typ obnovení** vyberte **obnovit databázi do následující bodu v čase**, určete bod v čase a potom proveďte kroky 3 až 6.

## <a name="monitoring-the-execution-of-snapshots"></a>Monitorování provádění snímky

Musíte monitorovat úložiště snímků. Skript, který provede snímku úložiště zapíše výstup do souboru a pak ji uloží je do stejného umístění jako tyto skripty. Samostatný soubor je napsán pro každý snímek. Výstup každého souboru zřetelně zobrazí v různých fázích, že se snímek skript spustí:

- Hledání svazky, které je třeba pro vytvoření snímku
- Hledání snímky převzaty z těchto svazků
- Odstranění případné existující snímků tak, aby odpovídaly počet snímků, které zadáte
- Vytvoření snímku HANA
- Vytvoření snímku úložiště přes svazky
- Odstraněním snímku HANA
- Přejmenování nejnovější snímku do **.0**

Nejdůležitější část skriptu jsou tyto:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
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
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Zobrazí se od této ukázky jak skript záznamy vytvoření snímku HANA. V případě Škálováním na více systémů je tento proces zahájit v hlavní uzel. Hlavní uzel zahájí synchronní vytvoření snímků na všechny uzly pracovního procesu. Potom pořízení snímku úložiště. Po úspěšné provedení snímků úložiště je odstranění snímku HANA.
