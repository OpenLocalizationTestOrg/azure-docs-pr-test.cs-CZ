---
title: "Vysoká dostupnost a zotavení po havárii systému SAP HANA v Azure (velké instance) | Microsoft Docs"
description: "Vytvoření vysoké dostupnosti a plán pro zotavení po havárii SAP HANA v Azure (velké instance)"
services: virtual-machines-linux
documentationcenter: 
author: saghorpa
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/31/2017
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 09aa98a35fa8286828a99c49a33a80d5938afe3a
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>Velké instance SAP HANA vysoké dostupnosti a zotavení po havárii v Azure 

Vysoká dostupnost a zotavení po havárii (DR) jsou důležité aspekty běží vaše důležité SAP HANA na serveru Azure (velké instance). Je důležité pro práci s SAP, vaše systémový Integrátor nebo Microsoft správně architektury a implementovat správné vysokou dostupnost a zotavení po havárii strategie. Také je potřeba vzít v úvahu plánovaného bodu obnovení (RPO) a cíli času obnovení, které jsou specifické pro vaše prostředí.

Společnost Microsoft podporuje některé funkce vysoké dostupnosti SAP HANA velké instancemi HANA. Tyto funkce patří:

- **Replikace úložiště**: schopnost systému úložiště replikovat všechna data do jiné Instance velké HANA razítko v jiné oblasti Azure. SAP HANA funguje nezávisle na tuto metodu.
- **Replikace systému HANA**: replikace všech dat v SAP HANA na samostatném systému SAP HANA. Plánovanou dobu obnovení je minimalizován prostřednictvím replikace dat v pravidelných intervalech. SAP HANA podporuje asynchronní, synchronní režimy v paměti a synchronní. Doporučuje se synchronním režimu jenom pro SAP HANA systémy, které jsou ve stejném datovém centru nebo méně než 100 km od sebe. V aktuální návrhu HANA velké instance razítka replikaci systému HANA slouží pouze pro vysokou dostupnost. V současné době replikace systému HANA vyžaduje součást reverznímu proxy serveru třetích stran pro zotavení po havárii konfigurace do jiné oblasti Azure. 
- **Automatické převzetí služeb při selhání hostitele**: místní selhání obnovení řešení pro SAP HANA používat jako alternativu k replikaci HANA systému. Pokud hlavní uzel nebude k dispozici, můžete nakonfigurovat jeden nebo více uzlů SAP HANA pohotovostní v režimu Škálováním na více systémů, SAP HANA automaticky převezme do pohotovostního uzlu.

SAP HANA v Azure (velké instance) je k dispozici v dvou oblastech Azure, které se týkají tři různé geopolitické oblasti (USA, Austrálie a Evropě). Že hostitele HANA velké Instance razítka připojeni k okruhy samostatné vyhrazenou síť, které používají se pro replikaci úložiště snímků pro zotavení po havárii metody dvou různých oblastech. Ve výchozím nastavení není zřídit replikace. Je nastavený pro zákazníky, kteří seřazené funkce zotavení po havárii. Replikace úložiště je závislá na využití úložiště snímků pro velké instance HANA. Není možné vybrat jako oblasti zotavení po Havárii, která je v jiné oblasti geopolitické oblasti Azure. 

V následující tabulce jsou aktuálně podporované metody vysokou dostupnost a zotavení po havárii a kombinace:

| Scénář v HANA velké instancí | Možnost vysoké dostupnosti | Možnost zotavení po havárii | Komentáře |
| --- | --- | --- | --- |
| Jeden uzel | Není k dispozici. | Instalační program vyhrazené zotavení po Havárii.<br /> Instalační program Multipurpose zotavení po Havárii. | |
| Automatické převzetí služeb při selhání hostitele: N + m<br /> včetně 1 + 1 | Chcete-li to možné s v úsporném režimu trvá aktivní roli.<br /> HANA řídí přepínač role. | Instalační program vyhrazené zotavení po Havárii.<br /> Instalační program Multipurpose zotavení po Havárii.<br /> Zotavení po Havárii synchronizace pomocí replikace úložiště. | Sady svazků HANA jsou připojené ke všem uzlům (n + m).<br /> Zotavení po Havárii lokalita musí mít stejný počet uzlů. |
| Replikace systému HANA | Chcete-li to možné primární nebo sekundární instalačního programu.<br /> Sekundární přesune do primární role v případě převzetí služeb při selhání.<br /> Převzetí služeb při selhání řízení HANA systému replikace a operačního systému. | Instalační program vyhrazené zotavení po Havárii.<br /> Instalační program Multipurpose zotavení po Havárii.<br /> Zotavení po Havárii synchronizace pomocí replikace úložiště.<br /> Zotavení po Havárii pomocí replikace systému HANA ještě není možné bez komponenty jiných výrobců. | Samostatnou sadu diskové svazky jsou připojené do každého uzlu.<br /> Pouze diskové svazky sekundární replika v produkční lokality replikovaly do umístění, zotavení po Havárii.<br /> Na webu zotavení po Havárii je požadován jednu sadu svazky. | 

Instalace s vyhrazenou zotavení po Havárii je, kde není jednotka HANA velké Instance v lokalitě zotavení po Havárii používaný ke spuštění jakékoli úlohy nebo testovacím systému. Jednotka je pasivní a nasazuje pouze v případě, že je provést převzetí služeb po havárii. Ale toto není upřednostňovaný volba pro mnoho zákazníků.

Víceúčelové instalace zotavení po Havárii je, kde je spuštěna Instance HANA velké jednotky na webu zotavení po Havárii mimo produkční zatížení. V případě havárie vypnutí systému mimo produkční, připojit replikované úložiště (Další) svazek sady a spusťte instance HANA produkční. Většina zákazníkům, kteří používají funkci zotavení po havárii HANA velké Instance pomocí této konfigurace. 


Další informace o vysoké dostupnosti SAP HANA najdete v těchto článcích SAP: 

- [SAP HANA vysokou dostupnost dokument White Paper](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [Příručka pro správu SAP HANA](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy Video o replikaci systému SAP HANA](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP podporu Poznámka #1999880 – nejčastější dotazy na replikaci systému SAP HANA](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP podporu Poznámka #2165547 – SAP HANA zpět nahoru a obnovení v rámci prostředí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [Pro Exchange hardwaru minimální nebo nula. výpadků SAP podporu Poznámka #1984882 – pomocí replikace systému SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Aspekty sítě pro zotavení po havárii s instancemi velké HANA

Abyste mohli využívat funkci zotavení po havárii HANA velké instancí, budete muset navrhnout připojení k síti na dvou různých oblastech Azure. Potřebujete připojení okruh Azure ExpressRoute z místního ve vaši hlavní oblast Azure a jiný okruh připojení z místního k vaší oblasti zotavení po havárii. Tato míra popisuje situaci tam, kde došlo k potížím v oblasti Azure, včetně umístění Microsoft Enterprise Edge směrovač (MSEE).

Jako druhá míra se můžete připojit virtuální sítě Azure, která se připojují k SAP HANA v Azure (velké instance) v jedné oblasti s okruhem ExpressRoute, který se připojuje HANA velké instance v jiné oblasti. Pomocí této *křížové připojení*, služby spuštěné v Azure virtuální síť v oblast č. 1, se může připojit k instanci HANA velké jednotky v oblasti č. 2 a naopak. Tato míra řeší případ, kdy jenom jeden MSEE umístění, která se připojuje k vaší místní umístění pomocí Azure přejde do režimu offline.

Odolné konfigurace pro zotavení po havárii je znázorněný na následujícím obrázku:

![Optimální konfigurace pro zotavení po havárii](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-when-you-use-hana-large-instances-storage-replication-for-disaster-recovery"></a>Další požadavky při použití replikace úložiště HANA velké instance pro zotavení po havárii

Další požadavky pro instalaci zotavení po havárii s instancemi velké HANA jsou:

- Musí pořadí SAP HANA na Azure (velké instance) SKU stejnou velikost jako provozním SKU a nasaďte je do oblasti zotavení po havárii. V aktuální zákaznických nasazení tyto instance slouží ke spuštění instancí HANA nevýrobní prostředí. Označujeme je jako *víceúčelových zotavení po Havárii nastavení*.   
- Další úložiště na webu zotavení po Havárii musí pořadí pro každý z vaší SAP HANA na Azure SKU (velké instance), které chcete obnovit v lokalitě pro obnovení po havárii. Nákup dalšího úložiště umožňuje přidělit svazky úložiště. Svazky, které jsou cílem replikace úložiště z provozním oblasti Azure do zotavení po havárii oblast Azure, kterou můžete přidělit.
 

## <a name="backup-and-restore"></a>Zálohování a obnovení

Jedním z nejdůležitějších aspektů operační databáze je z důvodu ochrany zkontrolujte různé závažné události. Příčinu tyto události může být cokoli z přírodní katastrofy chyby jednoduché uživatele.

Zálohování databáze, možnost obnovit do libovolného bodu v čase (například před odstraněním někdo důležitá data), umožňuje obnovení stavu, který je co nejblíže ke byl před přerušení.

Pro dosažení co nejlepších výsledků se musí provádět dva typy zálohování:

- K zálohování databáze: úplná, přírůstkové nebo rozdílové zálohy
- Zálohy protokolu transakcí

Kromě zálohování databáze úplné provést na úrovni aplikace může provést zálohování pomocí snímků úložiště. Úložiště snímků nenahrazují zálohy protokolu transakcí. Zálohy protokolu transakcí zůstat důležité obnovit databázi do určité míry v čase nebo prázdný protokoly z již potvrzené transakce. Úložiště snímků však mohou urychlit obnovení rychle díky dopředné bitové kopie databáze. 

SAP HANA v Azure (velké instance) nabízí dvě možnosti pro zálohování a obnovení:

- Provést sami (DIY). Po můžete vypočítat zkontrolujte, zda že je dostatek místa na disku, proveďte úplné databáze a protokolu zálohování pomocí metody zálohování na disk. Můžete zálohovat buď přímo na svazky připojené jednotky HANA velké Instance nebo do sdílené složky NFS (Network File) nastavit v Azure virtuální počítač (VM). V takovém případě zákazníků nastavení virtuálního počítače s Linuxem v Azure, připojte k virtuálnímu počítači Azure Storage a sdílené složky úložiště prostřednictvím nakonfigurovaný server systému souborů NFS v tohoto virtuálního počítače. Pokud provádíte zálohování pro svazky, které přímo připojit k instanci HANA velké jednotky, budete muset zkopírovat zálohování k účtu úložiště Azure (po nastavení virtuálního počítače Azure, který exportuje sdílených složek NFS, které jsou založeny na Azure Storage). Nebo můžete použít trezor služby Azure backup nebo úložiště Azure uloží málo používaná data. 

   Další možností je použít nástroj ochrany dat třetích stran a uložte zálohy po se kopírují do účtu úložiště Azure. DIY možnost zálohování může být také nutné pro data, která je potřeba uložit na delší dobu pro dodržování předpisů a auditování. Ve všech případech zálohování se zkopírují do sdílené složky NFS reprezentované prostřednictvím virtuálních počítačů a úložiště Azure.

- Použít funkci zálohování a obnovení funkce, která poskytuje základní infrastruktura SAP HANA v Azure (velké instance). Tato možnost splňuje potřeby zálohování a rychlé obnovení. Zbývající část tohoto oddílu řeší funkce zálohování a obnovení, která se nabízí velké instancemi HANA. Tato část také obsahuje vztah zálohování a obnovení má k zotavení po havárii funkce nabízené sítěmi HANA velké instancí.

> [!NOTE]
> Snímek technologie, která se používá základní infrastruktura velké instancí HANA má závislost na snímky SAP HANA. V tomto okamžiku SAP HANA snímky nefungují ve spojení s více klienty kontejnerů víceklientské databázi SAP HANA. Tato metoda zálohování proto nelze použít, když nasazujete více klientů v kontejnerech víceklientské databázi SAP HANA. Pokud jenom jednoho klienta je nasazená, SAP HANA snímky fungují.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Pomocí snímků úložiště SAP HANA v Azure (velké instance)

Infrastrukturu úložiště základní SAP HANA v Azure (velké instance) podporuje úložiště snímků svazků. Zálohování a obnovení svazků se podporuje, s následující aspekty:

- Místo databáze úplné zálohy snímků svazků úložiště přesměrováni na základě časté.
- Při spouštění snímku přes/hana/data, hana nebo protokolu a /hana/shared (zahrnuje /usr/sap) svazky úložiště snímku zahájí SAP HANA snímek před spuštěním snímku úložiště. Tento snímek SAP HANA je bod instalační program pro případné protokolu obnovení po obnovení snímku úložiště.
- Po bodu, kde byl úspěšně proveden snímku úložiště je odstraněn snímek SAP HANA.
- Zálohy protokolu transakcí se často provádějí a jsou uložené ve svazku /hana/logbackups nebo v Azure. Můžete aktivovat /hana/logbackups svazku, který obsahuje zálohy protokolu transakcí, které chcete pořízení snímku samostatně. V takovém případě není nutné provést HANA snímku.
- Pokud je nutné obnovit databázi do určité míry v čase, žádosti o podporu Microsoft Azure (pro produkční výpadek) nebo SAP HANA na Azure Service Management k obnovení snímku určité úložiště. Příkladem je plánované obnovení systému izolovaného prostoru do původního stavu.
- SAP HANA snímek, který je součástí úložiště snímků je bod posunutí pro použití zálohování protokolu transakcí, které byly provedeny a uložené po pořízení snímku úložiště.
- Tyto zálohy protokolu transakcí se provádějí k obnovení databázi zpět do určité míry v čase.

Můžete provést úložiště snímků cílení na tři různé třídy svazky:

- Kombinovaná snímek nad/hana/daty a /hana/shared (zahrnuje/usr/sap). Tento snímek vyžaduje vytvoření snímku SAP HANA jako příprava pro vytvoření snímku úložiště. SAP HANA snímku se ujistěte, že databáze je v konzistentním stavu z hlediska úložiště.
- Samostatné snímek přes/hana/logbackups.
- Oddíl OS (jen pro typ I velké instancí HANA).


### <a name="storage-snapshot-considerations"></a>Aspekty volby úložiště snímků

>[!NOTE]
>Úložiště snímků využívat prostor úložiště, který byl přidělen do Instance HANA velké jednotky. Proto je potřeba zvážit následující aspekty plánování úložiště snímků a kolik úložiště snímků chcete zachovat. 

Konkrétní mechanismů úložiště snímků pro SAP HANA v Azure (velké instance) patří:

- Konkrétní úložiště snímků (v bodě v čase, kdy se provede) využívá malé úložiště.
- Jako obsahu změny dat a obsah SAP HANA data, která soubory změnit na svazek úložiště je potřeba ukládat původní obsah bloku, jakož i změny dat snímku.
- V důsledku toho snímku úložiště nárůstu velikosti. Čím delší snímku existuje, tím větší bude snímku úložiště.
- Další změny, které jsou vytvářeny svazku databáze SAP HANA průběhu životnosti snímku úložiště, tím větší spotřebu prostor úložiště snímků.

SAP HANA v Azure (velké instance) se dodává s pevný svazek velikosti pro svazky protokolu a data SAP HANA. Provádění snímky tyto svazky eats do místo ve svazku. Budete muset určit, kdy se při plánování úložiště snímků. Musíte taky monitorovat využívání místa svazků úložiště, a také spravovat počet snímků, které jsou uloženy. Úložiště snímků můžete zakázat při importu hmotností dat nebo provést jiné významné změny databáze HANA. 


Následující části obsahují informace pro provedení tyto snímky, včetně obecná doporučení:

- I když hardware tolerovat 255 snímků na svazku, důrazně doporučujeme, abyste zůstat dobře pod tuto hodnotu.
- Před provedením úložiště snímků, monitorování a sledování volného místa.
- Nižší počet snímků úložiště podle volného místa. Můžete snížit počet snímků, které můžete zachovat, nebo můžete rozšířit svazky. Další úložiště můžete uspořádat v jednotkách 1 terabajt.
- Během aktivity, například přesun dat do SAP HANA s nástroji pro migraci platformy SAP (R3load) nebo při obnovování databáze SAP HANA ze zálohy zakažte úložiště snímků na /hana/data svazku. 
- Během větší uspořádání SAP HANA tabulek úložiště snímků je nutno, pokud je to možné.
- Úložiště snímků jsou předpokladem pro využívat výhod možností zotavení po havárii SAP HANA v Azure (velké instance).

### <a name="setting-up-storage-snapshots"></a>Nastavení úložiště snímků

Postup nastavení úložiště snímků velké instancemi HANA jsou následující:
1. Ujistěte se, zda Perl je nainstalován na operační systém Linux na HANA velké instance serveru.
2. Upravit/etc/ssh/ssh\_konfigurace přidat řádek _Mac hmac-sha1_.
3. Vytvoření účtu SAP HANA zálohování uživatele na hlavní uzel pro každou instanci SAP HANA, kterou používáte, pokud je k dispozici.
4. Nainstalujte klienta SAP HANA HDB ve všech serverech velké instance SAP HANA.
5. Na prvním serveru SAP HANA velké instancí každé oblasti vytvořte veřejný klíč pro přístup k podkladové infrastruktury úložiště, který řídí vytváření snímku.
6. Zkopírujte skripty a konfigurační soubor z [Githubu](https://github.com/Azure/hana-large-instances-self-service-scripts) umístění **hdbsql** v instalaci SAP HANA.
7. Upravte soubor HANABackupDetails.txt v případě potřeby u specifikace odpovídající zákazníka.

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Krok 1: Instalace klienta SAP HANA HDB

Operační systém Linux nainstalován na SAP HANA v Azure (velké instance) obsahuje složky a skripty potřebné k provedení SAP HANA úložiště snímků pro účely zálohování a zotavení po havárii. Zkontrolujte novější verze v [Githubu](https://github.com/Azure/hana-large-instances-self-service-scripts). Nejnovější verzi z skriptů je 2.1.
Je však za to, že instalace klienta na SAP HANA HDB na jednotkách HANA velké instanci při instalaci SAP HANA. (Microsoft neinstaluje klienta HDB nebo SAP HANA.)

### <a name="step-2-change-the-etcsshsshconfig"></a>Krok 2: Změnit/etc/ssh/ssh\_konfigurace

Změna `/etc/ssh/ssh_config` přidáním _Mac hmac-sha1_ řádek, jak je vidět tady:
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

Pokud chcete povolit přístup k rozhraní úložiště snímků klienta služby HANA velké Instance, potřebujete vytvořit přihlášení prostřednictvím veřejného klíče. Na první SAP HANA na serveru Azure (velké instance) ve vašem klientovi vytvořte veřejný klíč, který se má použít pro přístup k infrastruktury úložiště, takže si můžete vytvořit snímky. Veřejný klíč zajistí, že pro přihlášení k rozhraní úložiště snímku není vyžadováno heslo. Vytvoření veřejný klíč také znamená, že není potřeba spravovat hesla pověření. V systému Linux na SAP HANA velké instance serveru spusťte následující příkaz k vygenerování veřejný klíč:
```
  ssh-keygen –t dsa –b 1024
```
Nové umístění je **_/root/.ssh/id\_dsa.pub**. Nezadávejte vlastní heslo, jinak je nutné zadat heslo při každém přihlášení. Místo toho vyberte **Enter** dvakrát možnosti odeberete požadavek "Zadejte heslo" pro přihlášení.

Zkontrolujte, že veřejný klíč byl opraven podle očekávání změnou složky, které mají **/root/.ssh/** a pak provádění `ls` příkaz. Pokud je klíč přítomen, můžete ji zkopírovat tak, že spustíte následující příkaz:

![Veřejný klíč se zkopíruje spuštěním tohoto příkazu](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

V tomto okamžiku obraťte SAP HANA na Azure Service Management a poskytnout veřejný klíč. Zástupce služeb používá veřejný klíč a zaregistrujte ho v základní infrastrukturu úložiště, která je ubírat pro vašeho klienta HANA velké Instance.

### <a name="step-4-create-an-sap-hana-user-account"></a>Krok 4: Vytvoření účtu uživatele SAP HANA

Zahájíte vytváření snímků SAP HANA musíte vytvořit uživatelský účet v SAP HANA, který můžete použít skripty snímku úložiště. Pro tento účel vytvořte uživatelský účet SAP HANA studia SAP HANA. Tento účet musí mít následující oprávnění: **správce zálohování** a **katalogu čtení**. V tomto příkladu je uživatelské jméno **SCADMIN**. Název uživatelského účtu v HANA Studio vytvořili rozlišuje velká a malá písmena. Je nutné vybrat **ne** pro vyžádání uživatelům změnit heslo pro další přihlášení.

![Vytvoření uživatele v HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Krok 5: Autorizace uživatelský účet SAP HANA

V tomto kroku autorizovat SAP HANA uživatelský účet, který jste vytvořili, takže skripty nemusíte odeslání hesla za běhu. Příkaz SAP HANA `hdbuserstore` umožňuje vytváření SAP HANA uživatelský klíč, který je uložený na jeden nebo více uzlů SAP HANA. Uživatelský klíč umožňuje uživateli přístup SAP HANA bez nutnosti Správa hesel z v rámci procesu skriptování. Skriptovací proces je popsán později.

>[!IMPORTANT]
>Spusťte následující příkaz jako `root`. Skript, jinak hodnota nemůže pracovat správně.

Zadejte `hdbuserstore` příkaz takto:

**Pro instalaci bez MDC HANA**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**Pro instalaci MDC HANA**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

V následujícím příkladu je uživatel **SCADMIN01**, název hostitele je **lhanad01**, a číslo instance se **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Pokud máte SAP HANA škálovatelnou konfiguraci, by měla spravovat, všechny skriptování z jednoho serveru. V tomto příkladu klíč SAP HANA **SCADMIN01** musí být změněna pro každého hostitele tak, aby odráží hostitele, který souvisí se klíč. Změnit účet záloh SAP HANA číslem instance databáze HANA. Klíč musí mít oprávnění správce na hostiteli, který je přiřazen, a zálohování uživatele pro konfigurace Škálováním na více systémů, musíte mít přístupová práva na všechny instance SAP HANA. Za předpokladu, že tři uzly Škálováním na více systémů mít názvy **lhanad01**, **lhanad02**, a **lhanad03**, sekvence příkazů vypadá takto:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Krok 6: Získání snímku skripty, konfigurace snímků a testování v konfiguraci a připojení

Stáhněte si nejnovější verzi skripty z [Githubu](https://github.com/Azure/hana-large-instances-self-service-scripts). Zkopírujte stažený skripty a textový soubor do pracovní adresář pro **hdbsql**. Pro aktuální HANA instalace, tento adresář se jako /hana/shared/D01/exe/linuxx86\_64/hdb. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl 
HANABackupCustomerDetails.txt 
``` 


Tady je účelem jiné skripty a soubory:

- **Azure\_hana\_backup.pl**: naplánovat tento skript s cron provést úložiště snímků na HANA data/log/sdílených svazků, svazek logbackups/hana/nebo operačního systému (na typu I SKU z HANA velké instance).
- **Azure\_hana\_replikace\_status.pl**: Tento skript poskytuje základní podrobnosti kolem stav replikace z pracoviště na web zotavení po havárii. Monitorování skriptu a zajistit, že replikace probíhá, a zobrazuje velikost položek, které jsou právě replikován. Také obsahuje pokyny, pokud replikace trvá příliš dlouho, nebo pokud na odkaz je vypnutý.
- **Azure\_hana\_snímku\_details.pl**: Tento skript obsahuje seznam základní podrobnosti o všechny snímky, na jeden svazek, které existují ve vašem prostředí. Tento skript spustíte na primárním serveru nebo na jednotce serveru v umístění pro obnovení po havárii. Skript obsahuje následující informace podle každý svazek, který obsahuje snímky:
   * Velikost celkový počet snímků na svazku
   * Každý snímek v tomto svazku obsahuje následující podrobnosti: 
      - Název snímku 
      - Doba pro vytvoření 
      - Velikost snímku
      - Frekvence snímků
      - ID zálohování HANA přidružené tento snímek, pokud je to důležité
- **Azure\_hana\_snímku\_delete.pl**: Tento skript odstraní snímku úložiště nebo u sady snímků. Můžete použít ID zálohování SAP HANA jako nachází v nástroji HANA Studio nebo název snímku úložiště. V současné době zálohování ID je vázaný jenom na snímky vytvořené pro HANA data/log/sdílených svazků. Jinak Pokud je zadaný Identifikátor snímku, usiluje všechny snímky, které odpovídají zadané snímku ID.  
- **testHANAConnection.pl**: Tento skript Otestuje připojení k instanci SAP HANA a je nutný k nastavení úložiště snímků.
- **testStorageSnapshotConnection.pl**: Tento skript má dva účely. Nejprve zajišťuje, že velké Instance HANA jednotka, ve které spouští skripty má přístup k přiřazené úložiště virtuálního počítače a rozhraní úložiště snímků instancí HANA velké. Druhým účelem je vytvořit dočasný snímků pro HANA instance, kterou zkoušíte. Tento skript by měl spustit pro všechny instance HANA na serveru, aby zálohování skripty fungovat podle očekávání.
- **removeTestStorageSnapshot.pl**: Tento skript odstraní testovací snímku vytvořeného pomocí skriptu **testStorageSnapshotConnection.pl**. 
- **HANABackupCustomerDetails.txt**: Tento soubor je upravitelnými konfiguračního souboru, který budete muset upravit přizpůsobit konfiguraci SAP HANA.

 
HANABackupCustomerDetails.txt soubor je soubor řízení a konfigurace pro skript, který běží úložiště snímků. Upravte soubor pro účely a instalační program. Byste měli obdržet **název zálohy úložiště** a **úložiště IP adresu** ze SAP HANA na Azure Service Management při nasazení vaší instance. Pořadí, nelze změnit, řazení, nebo mezer všech proměnných v tomto souboru. Skripty, jinak nebudou správně spouštět. Kromě toho obdržíte adresu IP hlavní uzel nebo uzel škálování (Pokud Škálováním na více systémů) od SAP HANA na Azure Service Management. Víte také číslo HANA instance, který jste získali během instalace SAP HANA. Teď je potřeba přidat název zálohy do konfiguračního souboru.

Pro nasazení škálování nebo Škálováním na více systémů konfigurační soubor vypadat jako v následujícím příkladu, poté, co jste vyplnili název úložiště záloh a IP adresu úložiště. Také musíte vyplnit následující data v konfiguračním souboru:
- Jeden uzel nebo IP adresa hlavního uzlu
- Čísla instance HANA
- Název zálohy 
    
```
#Provided by Microsoft Service Management
Storage Backup Name: client1hm3backup
Storage IP Address: 10.240.20.31
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 
Node 1 HANA instance number:
Node 1 HANA userstore Name:
```

>[!NOTE]
>V současné době pouze podrobnosti o 1 uzlu se používají ve skutečné skriptu HANA úložiště snímků. Doporučujeme, abyste otestovali přístup do nebo ze všech uzlů HANA tak, že pokud se hlavní uzel zálohování někdy změní, již zajistíte, že jiného uzlu může trvat jeho místo změnou podrobnosti v uzlu 1.

Po přepnutí všechna konfigurační data do souboru HANABackupCustomerDetails.txt, budete muset zkontrolujte, zda jsou konfigurace správné týkající se HANA instance data. Pomocí tohoto skriptu `testHANAConnection.pl`. Tento skript je nezávislá konfigurace aplikace SAP HANA škálování nebo Škálováním na více systémů.

```
testHANAConnection.pl
```

Pokud máte SAP HANA škálovatelnou konfiguraci, zajistěte, aby hlavní instance HANA přístup k požadované HANA servery a instancí. Neexistují žádné parametry testu skriptu, ale vaše data je nutné přidat do konfiguračního souboru HANABackupCustomerDetails.txt pro skript, který chcete spustit správně. Jenom kódy chyb prostředí příkaz se vrátí, takže není možné pro skript, který chcete chyby, zkontrolujte všechny instance. I tak skript zadejte některé užitečné komentáře můžete znovu zkontrolovat.

Chcete-li spustit skript, zadejte následující příkaz:
```
 ./testHANAConnection.pl
```
Pokud skript úspěšně získá stav instance HANA, zobrazí se zpráva, že HANA připojení bylo úspěšné.


Dalším krokem testovací je zkontrolujte připojení k úložišti na základě dat, které vložíte do konfiguračního souboru HANABackupCustomerDetails.txt, a potom spusťte test snímku. Před spuštěním `azure_hana_backup.pl` skriptu, je třeba spustit tento test. Pokud svazek obsahuje žádné snímky, je možné určit, zda svazek je prázdný nebo pokud dojde selhání SSH k získání podrobných informací o snímek. Z tohoto důvodu se skript spustí dva kroky:

- Ověří, jestli jsou dostupné pro skripty k provedení snímků úložiště virtuálního počítače a rozhraní klienta.
- Vytvoří snímek testu nebo fiktivní, pro každý svazek HANA instance.

Z tohoto důvodu je zahrnuta jako argument HANA instance. Pokud se nezdaří spuštění, není možné zajistit kontrolu chyb pro připojení k úložišti. I v případě, že se nezobrazí žádná chyba kontrola, skript poskytuje užitečné pomocné parametry.

Spuštění skriptu jako:
```
 ./testStorageSnapshotConnection.pl <HANA SID>
```
V dalším kroku skript se pokusí přihlásit do úložiště pomocí veřejný klíč zadaný v předchozích krocích instalační program a nakonfigurovat v souboru HANABackupCustomerDetails.txt daty. Pokud přihlášení je úspěšné, zobrazí se následující obsah:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Pokud při připojování ke konzole úložiště dochází k potížím, výstup vypadá takto:

```
**********************Checking access to Storage**********************
WARNING: Storage check status command 'volume show -type RW -fields volume' failed: 65280
WARNING: Please check the following:
WARNING: Was publickey sent to Microsoft Service Team?
WARNING: If passphrase entered while using tool, publickey must be re-created and passphrase must be left blank for both entries
WARNING: Ensure correct IP address was entered in HANABackupCustomerDetails.txt
WARNING: Ensure correct Storage backup name was entered in HANABackupCustomerDetails.txt
WARNING: Ensure that no modification in format HANABackupCustomerDetails.txt like additional lines, line numbers or spacing
WARNING: ******************Exiting Script*******************************
```

Po úspěšného přihlášení k rozhraní úložiště virtuálních počítačů skript pokračuje fáze #2 a vytvoří snímek testu. Pro konfigurací Škálováním na více systémů tři uzly SAP HANA jsou zde uvedeny výstup:

```
**********************Creating Storage snapshot**********************
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_shared_hm3_t020_vol ...
Snapshot created successfully.
```

Pokud snímek test byl úspěšně proveden pomocí skriptu, můžete pokračovat konfigurace skutečné úložiště snímků. Pokud není úspěšné, prozkoumejte problémy před další postup. Test snímku by měl zůstávají kolem, dokud se provádějí první skutečné snímky.


### <a name="step-7-perform-snapshots"></a>Krok 7: Provedení snímků

Jako přípravné kroky jsou dokončené, můžete začít konfigurovat konfiguraci skutečné úložiště snímků. Skript, který chcete naplánovat funguje s konfigurací škálování zatížení a škálování SAP HANA. Měli byste naplánovat spuštění skriptů prostřednictvím procesu cron. 

Můžete vytvořit tři typy zálohy snímků:
- **HANA**: kombinaci zálohy snímku, ve kterém svazků, které obsahují/hana/data a/hana/sdílené (který obsahuje také /usr/sap) jsou předmětem koordinované snímku. Jeden soubor obnovit je možné z tento snímek.
- **Protokoly**: zálohy snímku svazku logbackups/hana /. Aktivuje se, že žádný snímek HANA spustit tento snímek úložiště. Tento svazek úložiště je svazek, měl by obsahovat zálohy protokolu transakcí SAP HANA. SAP HANA protokolů transakcí zálohy jsou prováděny častěji omezit růst protokolu a zabránit ztrátě dat. Jeden soubor obnovit je možné z tento snímek. Neměli nižší četnost v části pět minut.
- **Spouštěcí**: snímku svazku, který obsahuje počet spouštěcí logické jednotky (LUN) velké instance HANA. Záloha snímku je možné pouze pomocí typu I SKU z HANA velké instancí. Obnovení nelze provést jedním souborem ze snímku svazku, který obsahuje spouštěcí logické jednotky. Pro typ II SKU z HANA velké instance můžete provést na úrovni operačního systému na zálohování a obnovení jednotlivých souborů. Najdete v dokumentu "[jak provést zálohování operačního systému pro typ II SKU](os-backup-type-ii-skus.md)" Další informace.


Syntaxe volání tyto tři různé typy snímků vypadá takto:
```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <HANA SID> manual 30

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <HANA SID> manual 30

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot none manual 30

```

Je třeba zadat následující parametry:

- První parametr charakterizuje typ zálohy snímku. Povolené hodnoty jsou **hana**, **protokoly**, a **spouštěcí**. 
- Druhý parametr je **HANA SID** (jako jsou například HM3) nebo **žádné**. Pokud je zadaná hodnota první parametry **hana** nebo **protokoly**, hodnota tohoto parametru bude **HANA SID** (např. HM3), jinak pro spouštěcí svazek zálohování, hodnota je **žádné**. 
- Třetí parametr není snímek nebo zálohování popisek pro typ snímku. Má dva účely. Jediný účel, můžete je zadat název, abyste věděli, co tyto snímky jsou o. Druhým účelem je pro azure skriptu\_hana\_backup.pl k určení počtu snímků úložiště, které jsou uchovány v rámci tohoto konkrétní popisku. Pokud naplánujete dvě zálohy snímků úložiště stejného typu (jako **hana**), se dvou různých štítky a definujte 30 snímky by měly být udržovány pro každou, že se chystáte končit 60 úložiště snímků svazků vliv. 
- Uchování snímky čtvrtého parametru nepřímo, definuje definováním počet snímků se stejnou předponou snímku (štítek) budou muset zůstat. Tento parametr je důležité pro naplánovaného spuštění prostřednictvím procesu cron. 

V případě škálovatelnou skript neobsahuje některé další kontroly k zajištění, že máte přístup všechny servery HANA. Skript také zkontroluje, jestli všechny instance HANA vrátit příslušný stav instancí, předtím, než se vytvoří snímek SAP HANA. Snímek SAP HANA následuje snímku úložiště.

Provádění skriptu `azure_hana_backup.pl` vytvoří úložiště snímku v těchto tří různých fází:

1. Spustí na snímku SAP HANA
2. Provede snímku úložiště
3. Odebere SAP HANA snímek, který byl vytvořen před spuštěním úložiště snímků

Spustit skript, zavolejte z HDB složku spustitelného souboru, který jste zkopírovali do. 

Doba uchování je možné spravovat pomocí počet snímků, které se odešlou jako parametr při spuštění skriptu (například **30**, uvedené dříve). Ano, množství času, která je předmětem úložiště snímků je funkce dvě věci: dobu provádění a počet snímků odeslána jako parametr při spouštění skriptu. Pokud počet snímků, které jsou zachovány překročí číslo, které jsou pojmenované jako parametr ve volání skriptu, nejstarší snímku úložiště stejný popisku (v našem případě předchozí **ruční**) je odstraněn před provedením nový snímek. Počet dáváte jako poslední parametr volání je číslo můžete použít k řízení počet snímků, které jsou zachovány. S tímto číslem můžete taky řídit, nepřímo, místo na disku využité pro snímky. 

> [!NOTE]
>Jakmile změníte štítek, počítání znovu spustí. To znamená, že je potřeba mít přísné v označování, vaše snímky nebude odstraněn omylem.

### <a name="snapshot-strategies"></a>Strategie snímku
Frekvence snímků pro různé typy závisí na ať už používáte funkci zotavení po havárii HANA velké Instance nebo ne. Funkci zotavení po havárii velké instancí HANA spoléhá na úložiště snímků. Využití úložiště snímků může vyžadovat některé speciální doporučení z hlediska četnost a provádění období úložiště snímků. 

V informace a doporučení, které následují, předpokládáme, že provedete *není* používat velké instancí HANA nabízí funkce zotavení po havárii. Místo toho použijte úložiště snímků jako způsob, jak mít zálohy a poskytnout obnovení bodu v čase za posledních 30 dní. Zadané omezení počtu snímků a adresní prostor, mají zákazníci za následující požadavky:

- Čas obnovení pro obnovení bodu v čase.
- Místo na použít.
- Cíl a plánovanou dobu obnovení pro zotavení po případné havárii bodu obnovení.
- Závěrečné provádění HANA databáze úplné zálohování na disky. Vždy, když zálohu celé databáze proti disky nebo **backint** rozhraní se provádí, provádění úložiště snímků selže. Pokud máte v plánu provést zálohování úplné databáze nad úložiště snímků, ujistěte se, že během této doby je zakázána provádění úložiště snímků.
- Počet snímků na jeden svazek je omezená na 255.


Pro zákazníky, kteří nepoužívají funkci zotavení po havárii HANA velké instancí je méně častá období snímku. V takových případech vidíme zákazníkům provádění kombinované snímky na /hana/data a /hana/shared (zahrnuje /usr/sap) v období 12 hodin nebo 24 hodin a udržují snímky tak, aby pokrývalo celé měsíce. Platí to i s snímky zálohování svazku protokolu. Doba pro provedení zálohy protokolu transakcí SAP HANA na zálohování svazku protokolu se však dojde v 5 minut období 15 minut.

Doporučujeme provést plánované úložiště snímků pomocí procesu cron. Doporučujeme také, které použijete stejný skriptu pro všechny zálohy a zotavení po havárii vyžaduje. Budete muset upravit skript tak, aby odpovídaly různé vstupy požadovaný čas zálohování. Tyto snímky jsou všechny naplánované jinak v procesu cron v závislosti na jejich čas spuštění: hodinové, 12 hodin, denní nebo týdenní. 

Příkladem plán cron v /etc/crontab může vypadat například takto:
```
00 1-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 46
10 00 * * *  ./azure_hana_backup.pl hana HM3 dailyhana 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
22 12 * * *  ./azure_hana_backup.pl log HM3 dailylogback 28
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```
V předchozím příkladu je hodinové kombinované snímků, které pokrývá svazky, které obsahují data/hana/a /hana/shared (zahrnuje/usr/sap) umístění. Tento typ snímku se použije pro obnovení bodu v čase rychlejší v posledních dvou dnů. Kromě toho je denní snímek na tyto svazky. Ano máte dva dny pokrytí podle hodinové snímky plus čtyři týdny pokrytí podle denní snímky. Kromě toho svazek zálohování protokolu transakcí zálohovat jednou denně. Tyto zálohy jsou zachovány i čtyři týdny. Jak vidíte v ve třetím řádku crontab, zálohování transakčního protokolu HANA je naplánován ke spuštění každých pět minut. Spuštění minut cron různých úloh, které provedení úložiště snímků rozkládají tak, aby tyto snímky nebudou provedeny všechny najednou v určitém místě v čase. 

V následujícím příkladu můžete provést kombinovaný snímek, který popisuje svazků, které obsahují/hana/data a/hana/sdílené (včetně/usr/sap) umístění hodinu. Tyto snímky ponechat dva dny. Snímky svazků zálohování protokolu transakcí jsou spouštěny na základě pět minut a jsou uchovávány čtyři hodiny. Jako dříve, zálohování soubor protokolu transakcí HANA je naplánován ke spuštění každých pět minut. Snímku svazku, zálohování protokolu transakcí se provádí pomocí zpoždění dvě minuty po spuštění zálohování protokolu transakcí. V rámci tyto dvě minuty třeba za normálních okolností dokončit zálohování protokolu transakcí SAP HANA. Jako dříve, svazku, který obsahuje spouštěcí LUN zálohovat jednou denně pomocí snímků úložiště a je udržováno čtyři týdny.

```
10 0-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl log HM3 logback 48
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```

Sekvence na předchozí příklad, s výjimkou spouštěcí logické jednotky je znázorněný na následujícím obrázku:

![Vztah mezi zálohy a snímky](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA provede regulární zápisu svazku /hana/log dokumentu potvrzené změny do databáze. V pravidelných intervalech zapíše SAP HANA uloženého bodu do svazku /hana/data. Jak je uvedeno v crontab je provést zálohu transakčního protokolu SAP HANA každých pět minut. Můžete také zjistit, že SAP HANA snímku se spustí každou hodinu v důsledku spuštění snímku kombinované úložiště přes /hana/data a /hana/shared svazky. Po úspěšné HANA snímku se spustí kombinované úložiště snímku. Podle pokynů v crontab, snímku úložiště na svazku /hana/logbackup se spustí každých pět minut, po zálohování protokolu transakcí HANA zhruba dvě minuty.


>[!IMPORTANT]
> Používání úložiště snímků pro SAP HANA zálohy se hodí v situaci, jenom v případě, že snímky jsou prováděny ve spojení s zálohy protokolu transakcí SAP HANA. Tyto zálohy protokolu transakcí musejí mít tak, aby pokrývalo časových období mezi snímky úložiště. 

Pokud jste nastavili závazek uživatelům v okamžiku obnovení 30 dní, postupujte takto:

- Ve výjimečných případech musíte přístup kombinované úložiště snímků over/hana nebo data a /hana/shared to znamená 30 dní.
- Máte zálohy souvislý protokolu transakcí, které se týkají čas mezi jakékoli snímky, kombinované úložiště. Ano nejstarší snímku svazku zálohování protokolu transakcí musí být 30 dní. Nejedná se případě, pokud zkopírujete zálohy protokolu transakcí do jiné složky systému souborů NFS, který je umístěný na úložiště Azure. V takovém případě může vyžádat starší zálohy protokolu transakcí z této sdílené složky NFS.

Abyste mohli využívat výhod úložiště snímků a replikace případné úložiště zálohy protokolu transakcí, musíte změnit umístění, která zapisuje zálohy protokolu transakcí, které chcete SAP HANA. Provedení této změny můžete v HANA Studio. I když SAP HANA zálohuje protokolu úplné segmenty automaticky, musíte zadat intervalu zálohování protokolu být deterministický. To platí hlavně při použití možnosti zotavení po havárii, vzhledem k tomu obvykle chcete provést zálohování protokolu s dobou deterministický. V následujícím případě vzali jsme 15 minut jako interval zálohování protokolů.

![Plán zálohování SAP HANA přihlásí SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Můžete vybrat zálohování, které jsou častější, než každých 15 minut. To se často provádí ve spojení s zotavení po havárii. Někteří zákazníci provést zálohování protokolu transakcí každých pět minut.  

Pokud databázi nikdy byla vytvořena záloha, v posledním kroku je provést zálohu databáze na základě souborů k vytvoření jedné zálohování položku, která musí existovat v rámci katalogu zálohování. SAP HANA nelze zahájit, jinak hodnota zadaného protokolu zálohování.

![Vytvořit zálohu na základě souborů k vytvoření jedné položky zálohování](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Po provedení mít vaše první úspěšné úložiště snímků, můžete také odstranit testovací snímek, který byl proveden v kroku 6. Chcete-li tak učinit, spusťte skript `removeTestStorageSnapshot.pl`:
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Monitorování počtu a velikosti snímků na diskovém svazku

Na svazku konkrétní úložiště můžete sledovat počet snímků a spotřebu úložiště těchto snímků. `ls` Příkaz nezobrazí snímek adresáře nebo soubory. Ale příkaz operační systém Linux `du` obsahuje podrobnosti o těchto snímků úložiště, protože jsou uložené ve stejném svazcích. Příkaz lze použít tyto možnosti:

- `du –sh .snapshot`: Poskytuje celkem všechny snímky v adresáři snímku.
- `du –sh --max-depth=1`: Zobrazí všechny snímky, které jsou uloženy v **.snapshot** složku a velikosti jednotlivých snímků.
- `du –hc`: Poskytuje celkovou velikost používané všechny snímky.

Pomocí těchto příkazů a ujistěte se, že snímky, které jsou provedeny a uloženy nejsou využívá všechny úložiště na svazcích.

>[!NOTE]
>Snímky spouštěcí LUN nejsou viditelné s předchozích příkazů.

### <a name="getting-details-of-snapshots"></a>Získávání podrobností snímků
K získání dalších podrobností na snímky, můžete také použít skript `azure_hana_snapshot_details.pl`. Tento skript můžete spustit v některém umístění, pokud je aktivní server v umístění pro obnovení po havárii. Skript poskytuje následující výstup, členěné podle každý svazek, který obsahuje snímky: 
   * Velikost celkový počet snímků na svazku
   * Každý snímek v tomto svazku obsahuje následující podrobnosti: 
      - Název snímku 
      - Doba pro vytvoření 
      - Velikost snímku
      - Frekvence snímků
      - ID zálohování HANA přidružené tento snímek, pokud je to důležité

Syntaxe provádění skriptu vypadá takto:

```
./azure_hana_snapshot_details.pl 
```

Protože skript se pokusí načíst ID zálohování HANA, musí se připojit k instanci SAP HANA. Toto připojení vyžaduje konfiguračního souboru HANABackupCustomerDetails.txt správně nastavit. Výstup dvě snímků na svazku může vypadat například takto:

```
**********************************************************
****Volume: hana_shared_SAPTSTHDB100_t020_vol       ***********
**********************************************************
Total Snapshot Size:  411.8MB
----------------------------------------------------------
Snapshot:   customer.2016-09-20_1404.0
Create Time:   "Tue Sep 20 18:08:35 2016"
Size:   2.10MB
Frequency:   customer 
HANA Backup ID:   
----------------------------------------------------------
Snapshot:   customer2.2016-09-20_1532.0
Create Time:   "Tue Sep 20 19:36:21 2016"
Size:   2.37MB
Frequency:   customer2
HANA Backup ID:   
```


### <a name="file-level-restore-from-a-storage-snapshot"></a>Obnovení na úrovni souboru ze snímku úložiště
Pro hana typy snímků a protokolů, budete moci přístup k snímkům přímo na svazky v **.snapshot** adresáře. Není podadresáři pro všechny snímky. Nyní byste měli mít zkopírovat každý soubor, který je předmětem snímku ve stavu, který měl v místě snímek z podadresář do skutečné adresářové struktury.

>[!NOTE]
>Obnovení jedním souborem nefunguje pro snímky spouštěcí logické jednotky. **.Snapshot** directory nebude vystavena ve spouštěcí logické jednotky. 


### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Omezení počtu snímků na serveru

Jak je popsáno výše, můžete snížit počet určité popisky snímků, které jsou uloženy. Poslední dva parametry tohoto příkazu zahájíte snímek jsou popisek a počet snímků, které chcete zachovat.

```
./azure_hana_backup.pl hana HM3 hanadaily 30
```

V předchozím příkladu popisek snímku je **zákazníka** a počet snímků s tímto štítkem pro zachování **30**. Jak můžete reagovat na využívání místa na disku, můžete snížit počet uložené snímky. Je snadný způsob, jak snížit počet snímků na 15, například pro spuštění skriptu s poslední parametr nastavit na **15**:

```
./azure_hana_backup.pl hana HM3 hanadaily 15
```

Pokud spustíte skript s tímto nastavením, počet snímků, včetně nový snímek úložiště je 15. 15 posledních snímků jsou zachovány, zatímco 15 starší snímky jsou odstraněny.

 >[!NOTE]
 > Tento skript snižuje počet snímků, pouze v případě, že existují snímky, které jsou více než jednu hodinu stará. Skript nedojde k odstranění snímků, které jsou menší než hodinu stará. Tato omezení se vztahují k zotavení po havárii volitelné funkce nabízené.

Pokud již nechcete udržovat sadu snímky s určitým zálohování popiskem **hanadaily** příklady syntaxe, můžete spustit skript s **0** jako číslo uchovávání informací. Tím se odeberou všechny snímky odpovídající tohoto popisku. Odebrání všechny snímky však může ovlivnit možnosti zotavení po havárii.

Druhá možnost, pokud chcete odstranit konkrétní snímky, je použít skript `azure_hana_snapshot_delete.pl`. Tento skript je určena k odstranění snímků nebo sadu snímky buď pomocí zálohování ID HANA jak se nachází v HANA Studio nebo prostřednictvím samotný název snímku. V současné době zálohování ID pouze svázané s snímky vytvořené pro **hana** typu snímku. Snímek zálohy typu **protokoly** a **spouštěcí** neprovádět SAP HANA snímku. Je proto žádné zálohování ID, která se má najít pro tyto snímky. Pokud je zadán název snímku, hledá všechny snímky v různých svazcích, které odpovídají názvu zadané snímku. Syntaxe volání skriptu je:

```
./azure_hana_snapshot_delete.pl 

```

Spuštění skriptu jako uživatel **kořenové**.

Pokud vyberete snímek, máte možnost odstranit jednotlivých snímků jednotlivě. Nejdřív zadejte svazek, který obsahuje snímek a potom zadejte název snímku. Pokud snímku na tento svazek existuje a je více než jednu hodinu stará, je odstraněn. Názvy svazku a názvy snímků můžete najít spuštěním `azure_hana_snapshot_details` skriptu. 

>[!IMPORTANT]
>Pokud data, která existuje pouze na snímek, který odstraňujete, pak pokud spustíte odstranění, data budou ztracena navždy.

   

### <a name="recovering-to-the-most-recent-hana-snapshot"></a>Obnovení na poslední snímek HANA

Pokud dojde k rozbalovací produkční scénář, procesem obnovení ze snímku úložiště lze inicializovat jako zákazník incidentu s Microsoft Azure Support. Bude stačit vysokou naléhavost, pokud byla data odstraněna v produkční systému a je jediný způsob, jak načíst data obnovit produkční databázi.

Obnovení bodu v čase v jiné situaci, může být nízkou naléhavost a plánované dnů předem. Toto obnovení s SAP HANA můžete naplánovat na Azure Service Management místo vyvolání problém s vysokou prioritou. Například je může plánování k upgradu softwaru SAP použitím nový balíček vylepšení. Pak budete muset obnovit snímek, který představuje stav před upgradem vylepšení balíčku.

Než odešlete požadavek, je nutné připravit. SAP HANA týmu Azure Service Management můžete žádost zpracovat a poskytují obnovené svazky. Potom můžete obnovit databázi HANA podle snímky. Tady je postup přípravy pro žádost:

>[!NOTE]
>Uživatelské rozhraní se můžou lišit od následující snímky obrazovky, v závislosti na verzi SAP HANA, který používáte.

1. Rozhodněte, které snímek pro obnovení. Pouze hana nebo datový svazek je obnoven, pokud dáte pokyn jinak. 

2. Vypněte instanci HANA.

 ![Vypnout instanci HANA](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Odpojte objemy dat v každém uzlu HANA databáze. Pokud datové svazky se pořád připojený k operačního systému, obnovení snímku se nezdaří.
 ![Odpojte svazky dat v každém uzlu databáze HANA](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Otevřete je, požádejte o obnovení snímku konkrétní požadavek podporu Azure.

 - Během obnovení: SAP HANA na Azure Service Management může vás k účasti na konferenci pro zajištění spolupráce, ověření a potvrzení, že se snímek správné úložiště obnoví. 

 - Po obnovení: SAP HANA na Azure Service Management vás upozorní, jakmile byla obnovena snímku úložiště.

5. Po dokončení procesu obnovení je znovu připojte všechny datové svazky.

 ![Znovu připojte všechny datové svazky](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Možnosti obnovení v rámci SAP HANA Studio, vyberte, pokud nepocházejí automaticky po opětovném připojení k databázi HANA přes SAP HANA Studio. Následující příklad ukazuje obnovení poslední snímek HANA. Úložiště snímků Vloží jednu HANA snímku. Pokud obnovíte poslední snímek úložiště, by mělo být nejnovější HANA snímku. (Pokud obnovujete na starší snímku úložiště, budete muset najít HANA snímku na základě doby pořízení snímku úložiště.)

 ![Vyberte možnosti obnovení studia SAP HANA](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Vyberte **obnovit databázi do konkrétních dat zálohování nebo úložiště snímků**.

 ![Zadejte typ obnovení okna](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Vyberte **zadejte zálohování bez katalogu**.

 ![Zadejte umístění zálohy okna](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. V **cílový typ** seznamu, vyberte **snímku**.

 ![Zadejte zálohy obnovit okna](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Vyberte **Dokončit** zahájíte proces obnovení.

 ![Vyberte "Dokončit" zahájíte proces obnovení](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. Databázi HANA je obnovit a obnovit HANA snímek, který je součástí úložiště snímku.

 ![Databáze HANA je obnovit a obnovit snímek HANA](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a>Obnovení na nejnovější stav

Následující proces obnoví HANA snímek, který je součástí úložiště snímku. Obnoví pak zálohy protokolu transakcí nejnovější stavu databázi před obnovením snímku úložiště.

>[!IMPORTANT]
>Než budete pokračovat, ujistěte se, že máte úplný a souvislý řetězec zálohy protokolu transakcí. Aktuální stav databáze nelze obnovit bez tyto zálohy.

1. Dokončit kroky 1 – 6 z [obnovení poslední snímek HANA](#recovering-to-the-most-recent-hana-snapshot).

2. Vyberte **obnovit databázi do stavu nejnovější**.

 ![Vyberte možnost "Obnovit databázi do stavu poslední"](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Zadejte umístění poslední zálohy protokolu HANA. Umístění musí obsahovat všechna HANA transakčního protokolu zálohování ze snímku HANA nejnovější stav.

 ![Zadejte umístění poslední zálohy protokolu HANA](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Vyberte zálohu jako základ, ze kterého chcete obnovit databázi. V našem příkladu HANA snímku na snímku obrazovky je HANA snímek, který je zahrnutý ve snímku úložiště. 

 ![Vyberte zálohu jako základ, ze kterého chcete obnovit databázi](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Vymazat **použití rozdílové zálohy** zaškrtávací políčko, pokud nejsou k dispozici rozdílů mezi časem HANA snímku a nejnovější stav.

 ![Zrušte zaškrtnutí políčka "Použití rozdílové zálohy", pokud neexistuje žádné rozdíly](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Na obrazovce Přehled vyberte **Dokončit** spustíte proces obnovení.

 ![Klikněte na tlačítko "Dokončit" na obrazovce Přehled](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a>Obnovení do jiného bodu v čase
Obnovit do bodu v čase mezi HANA snímku (zahrnutá ve snímku úložiště) a ten, který je novější než obnovení bodu v čase HANA snímku, postupujte takto:

1. Ujistěte se, že máte všechny transakční protokol zálohy ze snímku HANA na dobu, kterou chcete obnovit.
2. Začněte postup uvedený v [obnovení poslední stav](#recovering-to-the-most-recent-state).
3. V kroku 2 postupu, v **zadejte typ obnovení** vyberte **obnovit databázi do následující bodu v čase**a určete bod v čase. Dokončete kroky 3 až 6.

### <a name="monitoring-the-execution-of-snapshots"></a>Monitorování provádění snímky

Jako pomocí snímků úložiště velké instancí HANA, musíte taky monitorovat provádění těchto snímků úložiště. Skript, který provede snímku úložiště zapíše výstup do souboru a pak ji uloží je do stejného umístění jako tyto skripty. Samostatný soubor je napsán pro každý úložiště snímků. Výstup každého souboru zřetelně zobrazí v různých fázích, že se snímek skript spustí:

1. Najít svazky, které je třeba pro vytvoření snímku.
2. Najít snímky převzaty z těchto svazků.
3. Odstraňte případné existující snímky tak, aby odpovídaly počet snímků, které zadáte.
4. Vytvoření snímku SAP HANA.
5. Vytvoří úložiště snímků svazků.
6. Odstraňte snímek SAP HANA.
7. Přejmenujte nejnovější snímku do **.0**.

Nejdůležitější část identifikuje soubor cab skriptu je tuto část:
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
Zobrazí se od této ukázky jak skript záznamy vytvoření snímku HANA. V případě Škálováním na více systémů je tento proces zahájit v hlavní uzel. Hlavní uzel zahájí synchronní vytváření snímků SAP HANA na všechny uzly pracovního procesu. Potom pořízení snímku úložiště. Po úspěšné provedení snímků úložiště je odstranění snímku HANA. Odstranění snímku HANA je zahájena z hlavního uzlu.


## <a name="disaster-recovery-principles"></a>Zásady obnovení po havárii
Velké instancemi HANA nabízíme zotavení po havárii funkcí mezi razítka HANA velké Instance v různých oblastech Azure. Například při nasazování velkých Instance HANA jednotky v oblasti USA – západ Azure, můžete použít instanci HANA velké jednotky v oblasti USA – východ jako jednotky zotavení po havárii. Jak už bylo zmíněno dříve, zotavení po havárii není konfigurován automaticky, protože se vyžaduje, abyste platit pro jiné jednotce HANA velké Instance v oblasti zotavení po Havárii. Instalační program zotavení po havárii funguje pro nastavení škálování, jakož i Škálováním na více systémů. 

Ve scénářích nasazení, pokud pomocí našich zákazníků jednotka v oblasti zotavení po Havárii můžete spustit mimo produkční systémy, které používají nainstalovaná instance HANA. Instance HANA velké jednotky musí být stejné verze SKU jako SKU používá pro produkční účely. Konfigurace disku mezi jednotky serverů v oblasti Azure produkční a oblasti obnovení po havárii vypadá takto:

![Konfigurace nastavení zotavení po Havárii z hlediska disku](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Jak je znázorněno na obrázku přehled, musíte pak pořadí druhou sadu diskové svazky. Cílové diskové svazky mají stejnou velikost jako produkční svazky pro instanci provozní v jednotkách obnovení po havárii. Tyto diskové svazky jsou přidruženy k jednotka HANA velké instanci serveru v lokalitě pro obnovení po havárii. Následující svazky se replikují z oblasti produkční k webu zotavení po Havárii:

- / hana/dat
- / hana/logbackups 
- /Hana/Shared (zahrnuje/usr/sap)

/Hana/log svazek není replikují, protože transakčního protokolu SAP HANA není potřeba takovým způsobem, který je provést obnovení z těchto svazků. 

Základ pro zotavení po havárii funkce nabízené se funkce replikace úložiště nabízí infrastruktura HANA velké Instance. Funkce, která se používá na straně úložiště není nepřetržitý datový proud změny, které se replikují v asynchronním režimu, protože změny dojít ke svazku úložiště. Místo toho je mechanismus, který závisí na skutečnost, že jsou vytvořeny snímky tyto svazky v pravidelných intervalech. Rozdíl mezi již replikované snímku a nový snímek, který ještě nebyla replikována je pak přeneseny na zotavení po havárii lokality do cílové diskové svazky.  Tyto snímky jsou uložené ve svazcích a v případě selhání obnovení po havárii, je nutné obnovit na těchto svazcích.  

První přenos dokončení dat svazku by měl být než množství dat bude menší než rozdílů mezi snímky. V důsledku toho svazky v lokalitě zotavení po Havárii obsahovat každých snímků svazku v produkční lokality provést. Tuto skutečnost umožňuje nakonec tento systém zotavení po Havárii použít k získání starší stav chcete-li obnovit ke ztrátě dat, bez vrácení zpět produkční systému.

V případech, kde používáte replikaci HANA systému jako funkce vysoké dostupnosti v produkční lokality replikují se jenom svazky instance vrstvy 2 (nebo replika). Tato konfigurace může vést ke zpoždění replikace úložiště k webu zotavení po Havárii, pokud udržovat nebo vypnout jednotka serveru sekundární repliky (úroveň 2) nebo instance SAP HANA v této jednotce. 

>[!IMPORTANT]
>Stejně jako u vícevrstvé replikace systému HANA, vypnutí jednotky vrstvy 2 HANA instance nebo server blokuje replikace na lokalitu zotavení po havárii při použití funkce velké Instance HANA zotavení po havárii.


>[!NOTE]
>Funkce replikace úložiště HANA velké Instance je zrcadlení a replikace úložiště snímků. Proto pokud neprovedete úložiště snímků zavedeném ve zálohování část v tomto dokumentu, nemůže existovat žádné replikace na lokalitu zotavení po havárii. Provádění snímku úložiště je předpokladem pro replikaci úložiště do lokality zotavení po havárii.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>Příprava scénáře zotavení po havárii
Předpokládáme, že máte produkční systému, který používá u velkých instancí HANA v produkčním prostředí oblast Azure. Pro následující dokumentaci Předpokládejme, že identifikátor SID tohoto systému HANA je "PRD." Také předpokládáme, že máte testovacím systému na velké instancí HANA spuštěných v zotavení po havárii oblast Azure. Dokumentace předpokládáme, že jeho SID je "TST." Konfigurace, vypadá takto:

![Spuštění instalačního programu zotavení po Havárii](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Pokud instance serveru nebyla byly již uspořádány sadou svazek úložiště, SAP HANA na Azure Service Management připojí další sadu svazky jako cíl pro repliku produkčního k jednotce velké Instance HANA spuštěný TST Instance HANA na. K tomuto účelu budete muset zadat identifikátor SID instance HANA produkční. Poté, co SAP HANA na Azure Service Management potvrdí připojení tyto svazky, musíte tyto svazky na jednotku, velké Instance HANA připojte.

![Dalším krokem instalace zotavení po Havárii](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

Dalším krokem pro vás je k instalaci druhé instance SAP HANA na jednotce HANA velké Instance v oblasti Azure, kde spouštíte instance TST HANA zotavení po havárii. Nově instalovaný instance SAP HANA musí mít stejný identifikátor SID. Uživatelé vytvořili musí mít stejný UID a ID skupiny, která má instance produkční. Pokud instalace byla úspěšná, budete muset:
- Zastavte nově nainstalovaná instance SAP HANA na jednotce velké Instance HANA v oblasti Azure zotavení po havárii.
- Odpojit tyto svazky PRD a obraťte se na Azure Service Management SAP HANA. Svazky nelze zůstanou připojené k jednotce, protože nesmí být dostupný při fungování jako cíl replikace úložiště.  

![Zotavení po Havárii kroku před navázáním replikace](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

Provozní tým chce vytvořit vztah replikace mezi svazky PRD v produkčním prostředí oblast Azure a PRD svazky v oblasti Azure zotavení po havárii.

>[!IMPORTANT]
>Svazek /hana/log nebude replikován, protože není nutné obnovit databázi SAP HANA replikované do konzistentního stavu v lokalitě pro obnovení po havárii.

Dalším krokem, můžete je nastavit nebo upravit plán zálohování snímku úložiště, abyste se dostali na RTO a plánovaný bod obnovení v případě, že po havárii. Chcete-li minimalizovat plánovaného bodu obnovení, nastavte následující intervaly replikace v HANA velké Instance služby:
- Svazky, které jsou předmětem kombinované snímku (snímek typ = **hana**) replikovat každých 15 minut k cílům svazku ekvivalentní úložiště v lokalitě pro obnovení po havárii.
- Svazek zálohování protokolu transakcí (snímek typ = **protokoly**) replikuje každé tři minuty k cílům svazku ekvivalentní úložiště v lokalitě pro obnovení po havárii.

Chcete-li minimalizovat plánovaného bodu obnovení, následující nastavení:
- Provedení **hana** typ úložiště snímků (najdete v části "krok 7: provedení snímků") každých 30 minut na 1 hodinu.
- Proveďte zálohování protokolu transakcí SAP HANA každých 5 minut.
- Provedení **protokoly** zadejte úložiště snímku každých 5 až 15 minut. Pomocí tohoto období interval musí být schopní dosáhnout RPO přibližně 15 25 minut.

Toto nastavení, pořadí zálohování protokolů transakcí, úložiště snímků a replikaci protokolů transakcí HANA zálohování svazku a /hana/data a /hana/shared (zahrnuje/usr/sap) může vypadat například data zobrazená na obrázku:

 ![Vztah mezi snímek zálohy protokolu transakcí a snap zrcadlení na časová osa](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

K dosažení i lepším plánovaným bodem obnovení v případě zotavení po havárii, můžete zkopírovat zálohy protokolu transakcí HANA z SAP HANA v Azure (velké instance) oblasti Azure. K dosažení této další snížení plánovaný bod obnovení, proveďte následující kroky hrubý:

1. Zálohování transakce HANA protokolu možné se často jako /hana/logbackups.
2. Použití rsync zkopírovat zálohy protokolu transakcí do sdílené složky NFS hostované virtuální počítače Azure. Virtuální počítače jsou ve virtuálních sítí Azure, v oblasti Azure produkční a v oblastech zotavení po Havárii. Budete muset připojit obě Azure virtuální sítě k okruhu provozní HANA velké instance připojení k Azure. V tématu grafiky ve [sítě důležité informace pro zotavení po havárii s instancemi velké HANA](#Network-considerations-for-disaster-recovery-with-HANA-Large-Instances) části. 
3. Zajistit zálohy protokolu transakcí v oblasti ve virtuálním počítači připojit k systému souborů NFS, exportovat úložiště.
4. V případě havárie-převzetí služeb při selhání doplňují zálohy protokolu transakcí, které můžete najít na svazku /hana/logbackups s více nedávno provedených v systému souborů NFS zálohování protokolu transakcí sdílet v lokalitě pro obnovení po havárii. 
5. Nyní můžete spustit zálohování protokolu transakcí k obnovení poslední zálohy, který může být uložený přes oblasti zotavení po Havárii.

Jak velký Instance HANA operace potvrzení s instalačním programem vztah replikace a spustit zálohy provádění úložiště snímků, začne replikovat data.

![Zotavení po Havárii kroku před navázáním replikace](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

V průběhu replikace, snímky na svazcích PRD v zotavení po havárii Azure nejsou obnoveny oblasti. Pouze jsou uloženy. Pokud jsou svazky připojené v takový stav, představují stavu, ve kterém můžete odpojit tyto svazky po instalaci instance PRD SAP HANA v jednotce serveru v oblasti Azure zotavení po havárii. Také představují úložiště záloh, které ještě nejsou obnoveny.

V případě selhání také můžete k obnovení na starší snímku úložiště místo nejnovější snímku úložiště.

## <a name="disaster-recovery-failover-procedure"></a>Postup převzetí služeb při selhání zotavení po havárii
Chcete-li nebo muset převzetí služeb při selhání na web zotavení po Havárii, budete muset interakci s SAP HANA týmu operace v Azure. Hrubý kroky procesu dosavadní vypadá takto:

1. Vzhledem k tomu, že používáte mimo produkční instanci HANA na jednotce zotavení po havárii HANA velké instancí, musíte vypnout tuto instanci. Předpokládáme, že bude neaktivní instance produkční HANA předinstalovaným.
2. Ujistěte se, zda jsou spuštěny žádné procesy SAP HANA. Použijte následující příkaz pro tuto kontrolu: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. Výstup by měl zobrazit, můžete **hdbdaemon** procesů v zastaveném stavu a žádné jiné procesy HANA ve stavu spuštěná nebo spuštěna.
3. Určete, které SAP HANA zálohování ID nebo název snímku chcete obnovit lokalitu zotavení po havárii. V případech, skutečné zotavení po havárii tento snímek je obvykle nejnovější snímku. Pokud potřebujete obnovit ke ztrátě dat, vyberte starší snímku.
4. Kontaktujte Azure podporují pomocí žádosti o podporu s vysokou prioritou a požádejte o obnovení tohoto snímku (název a datum snímku) nebo HANA zálohování ID na webu zotavení po Havárii. Výchozí hodnota je, že operace obnovení svazku /hana/data pouze. Pokud chcete mít/hana/logbackups svazky a, musíte výslovně uvádějí, že. *Doporučujeme nejsou, obnovení /hana/shared svazku.* Místo toho, měli byste vybrat konkrétní soubory, jako je global.ini mimo **.snapshot** adresáře a jeho podadresářů po opětovném připojení/hana/sdílený svazek clusteru pro PRD. Na straně operace se chystáte dojít následující kroky:. Replikace z svazek provozního snímků za účelem zotavení po havárii svazků je zastavena. To může již dojít, pokud výpadku v produkčním je z důvodu, že budete potřebovat zotavení po Havárii.
    b. Úložiště snímků název nebo snímku se v průběhu zálohování, které jste zvolili ID je obnovit na svazcích, zotavení po havárii.
    c. Po obnovení je možné připojit k instanci HANA velké jednotky v oblasti zotavení po havárii svazky zotavení po havárii.
5. Připojte zotavení po havárii svazky, které mají jednotku HANA velké Instance v lokalitě pro obnovení po havárii. 
6. Spuštění, pokud spících produkční instance SAP HANA.
7. Pokud jste zvolili pro kopírování protokolů transakcí protokoly zálohování kromě zkrátit čas plánovaný bod obnovení, budete muset do adresáře logbackups nově připojené zotavení po Havárii/hana/sloučit tyto zálohy protokolu transakcí. Nepřepisovat existující zálohy. Jednoduše zkopírujete novější zálohování, které se nezreplikovaly s nejnovější replikace úložiště snímku.
8. Také můžete obnovit jednu soubory ze snímků, které mají být replikovány do svazku /hana/shared/PRD v oblasti Azure zotavení po havárii.

Další pořadí kroků zahrnuje obnovení instance SAP HANA produkční na základě obnovené úložiště snímků a zálohování protokolu transakcí, které jsou k dispozici. Kroky vypadat takto:

1. Změna umístění zálohy na **/hana/logbackups** pomocí SAP HANA Studio.
   ![Změna umístění zálohy pro zotavení po Havárii obnovení](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

2. SAP HANA kontrol prostřednictvím umístění zálohování souborů a navrhne nejnovější zálohy protokolu transakcí k obnovení. Kontrola může trvat několik minut, dokud se na obrazovce, jako se zobrazí následující: ![seznam zálohy protokolu transakcí pro zotavení po Havárii obnovení](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

3. Upravte některé výchozí nastavení:

      - Zrušte **používat rozdílové zálohy**.
      - Vyberte **inicializovat oblasti protokolu**.

   ![Nastavení protokolu oblasti inicializovat](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

4. Vyberte **Finish** (Dokončit).

   ![Dokončení obnovení zotavení po Havárii](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Okno Průběh, jak ukazuje tady by se zobrazit. Mějte na paměti, že v příkladu je zotavení po havárii obnovením konfigurace SAP HANA 3 uzlu Škálováním na více systémů.

![Průběh obnovení](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Pokud se zdá, že obnovení přestane reagovat na **Dokončit** obrazovky a nezobrazuje obrazovka průběhu kontrolu, aby potvrdil, že instance SAP HANA na pracovních uzlech běží. V případě potřeby spusťte ručně instance SAP HANA.


### <a name="failback-from-dr-to-a-production-site"></a>Navrácení služeb po obnovení z zotavení po Havárii pro produkční lokality
Pro produkční lokality se může selhat zpět z zotavení po Havárii. Podívejme se na tento případ, že převzetí služeb při selhání do lokality zotavení po havárii způsobila problémy v produkčním prostředí oblast Azure a ne vaše potřeba obnovit ztracené data. To znamená, že jste už běží vaše úlohy produkční SAP chvíli v lokalitě pro obnovení po havárii. Jak se řeší problémy v produkční lokality, budete chtít navrácení služeb po obnovení provozního webu. Protože dojde ke ztrátě dat, krok zpět do pracoviště zahrnuje několik kroků a úzké spolupráci s SAP HANA týmu operace v Azure. Můžete se rozhodnout pro aktivaci provozní tým spustit synchronizaci zpět na pracoviště po vyřešení problémů.

Pořadí kroků vypadá takto:

1. SAP HANA na Azure provozní tým získá aktivační událost pro synchronizaci svazky úložiště produkční ze svazků úložiště zotavení po havárii, které teď představují provozního stavu. Tento stav Instance HANA velké jednotky v produkční lokality je vypnutý.
2. SAP HANA na Azure provozní tým monitoruje replikace a zajišťuje, že je dosaženo zjištěná před vás informuje o jako zákazník.
3. Vypnout aplikace, které používají provozní HANA Instance v lokalitě pro obnovení po havárii. Poté provedete HANA zálohy protokolu transakcí. Potom můžete zastavit HANA instance spuštěné v instanci HANA velké jednotky v lokalitě pro obnovení po havárii.
4. Po ukončení HANA instance spuštěné v instanci HANA velké jednotky v lokalitě pro obnovení po havárii, provozní tým ručně provede synchronizaci diskové svazky.
5. SAP HANA na Azure provozní tým znovu spustí instanci HANA velké jednotky v pracoviště a předá pro vás. Ujistěte se, zda je instance SAP HANA v době spuštění jednotky HANA velké Instance v ve stavu vypnutí.
6. Stejně jako při přebírání služeb při selhání lokality zotavení po havárii dřív provedete stejný postup obnovení databáze.

### <a name="monitoring-disaster-recovery-replication"></a>Monitorování replikace pro zotavení po havárii

Stav replikace úložiště průběh můžete sledovat spuštěním skriptu `azure_hana_replication_status.pl`. Tento skript musí spustit z jednotky spuštěné v umístění pro obnovení po havárii. Jinak není přejdete fungovat podle očekávání. Skript funguje bez ohledu na to, zda je replikace aktivní. Skript lze spustit pro každou jednotku velké Instance HANA klienta služby v umístění pro obnovení po havárii. Nelze získat podrobnosti o spouštěcí svazek.

Volání skript jako:
```
./replication_status.pl <HANA SID>
```

Výstup je rozdělit, dle svazku do následujících částí:  

- Stav odkazu
- Aktuální aktivity replikace
- Replikovat nejnovější snímku 
- Velikost nejnovější snímku
- Aktuální prodleva mezi snímky – poslední dokončené snímku replikace a nyní  

Stav odkazu zobrazuje jako **Active** Pokud propojení mezi umístění je mimo provoz nebo převzetí služeb při selhání událostí právě probíhá. Aktivity replikace řeší, zda žádná data, je právě replikován nebo nečinné, nebo zda jiné aktivity se právě děje na odkaz. Poslední snímek replikovat objevit pouze jako `snapmirror…`. Velikost poslední snímek se následně zobrazí. Nakonec se zobrazí prodleva. Po dokončení replikace prodleva představuje čas od času naplánované replikaci do. Prodleva může být větší než hodinu pro replikaci dat, zejména v počáteční replikace, i když má spustit replikaci. Časový interval se bude nadále zvýšit, dokud nebude dokončeno probíhající replikace.

Příklad výstupu může vypadat například takto:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```












