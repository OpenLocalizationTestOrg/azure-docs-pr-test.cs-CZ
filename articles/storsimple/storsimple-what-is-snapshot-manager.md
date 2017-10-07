---
title: aaaWhat je StorSimple Snapshot Manager? | Dokumentace Microsoftu
description: "Popisuje hello Snapshot Manager zařízení StorSimple, jeho architektura a jejich funkce."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6094c31e-e2d9-4592-8a15-76bdcf60a754
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: v-sharos
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e79ce7b7e1970ac862038af2a0e67065b6fb6eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-toostorsimple-snapshot-manager"></a>Úvod tooStorSimple Snapshot Manager

## <a name="overview"></a>Přehled
Snapshot Manager zařízení StorSimple je modul snap-in konzoly Microsoft Management Console (MMC), který zjednodušuje ochrany dat a správu záloh v prostředí Microsoft Azure StorSimple. S StorSimple Snapshot Manager můžete spravovat Microsoft Azure StorSimple data v datovém centru hello a v cloudu hello jako řešení jedné integrované úložiště, proto kvůli zjednodušení zálohování procesů a snížení nákladů.

Tento přehled zavádí hello Snapshot Manager zařízení StorSimple, popisuje její funkce a vysvětluje, jeho role v Microsoft Azure StorSimple. 

Přehled hello celý Microsoft Azure StorSimple systému, včetně hello zařízení StorSimple, služby StorSimple Manager, Snapshot Manager zařízení StorSimple a StorSimple adaptéru pro službu SharePoint, naleznete v části [řady StorSimple 8000: hybridní cloud řešení úložiště](storsimple-overview.md). 

> [!NOTE]
> * Nelze použít StorSimple Snapshot Manager toomanage Microsoft Azure StorSimple virtuální pole (také označované jako StorSimple místní virtuální zařízení).
> * Pokud máte v plánu tooinstall StorSimple Update 2 v zařízení StorSimple, že toodownload hello nejnovější verzi služby StorSimple Snapshot Manager a nainstalujte ji **před instalací StorSimple Update 2**. Hello nejnovější verzi služby StorSimple Snapshot Manager je zpětně kompatibilní a spolupracuje s všechny vydané verze Microsoft Azure StorSimple. Pokud používáte hello předchozí verze služby StorSimple Snapshot Manager, budete potřebovat tooupdate it (není nutné toouninstall hello předchozí verzi před instalací nové verze hello).
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>Účel Snapshot Manager zařízení StorSimple a architektura
StorSimple Snapshot Manager poskytuje centrální konzolu, které můžete použít toocreate konzistentní, v okamžiku záložní kopie místní a cloudové data. Například můžete použít konzolu hello:

* Konfigurace, zálohovat a odstraňte svazky.
* Konfigurace svazku skupiny tooensure zálohovaných dat je konzistentní s aplikací.
* Spravovat zásady zálohování tak, aby se data zálohují na předem určeného plánu.
* Vytvořte místních a cloudových snímků, které můžou být uložené v cloudu hello a použít pro obnovení po havárii.

Hello StorSimple Snapshot Manager načte seznam hello aplikace zaregistrované s poskytovatelem služby VSS hello na hostiteli hello. Potom zálohování konzistentní s aplikací toocreate, zkontroluje svazky hello používá jiná aplikace a navrhne tooconfigure skupiny svazku. StorSimple Snapshot Manager používá tyto svazku skupiny toogenerate záložní kopie, které jsou konzistentní s aplikací. (Konzistence aplikací existuje, když všechny související soubory a databáze jsou synchronizovány a představují hello true stav aplikace hello v určitém bodě v čase.) 

Zálohování StorSimple Snapshot Manager formu hello přírůstkové snímky, které zaznamenat pouze hello změny od poslední zálohy hello. V důsledku toho zálohy vyžadují menší úložiště umožňuje vytvořit a a rychle obnovit. StorSimple Snapshot Manager používá tooensure hello Windows Stínová kopie svazku Service (VSS), zaznamenat snímky konzistentní s aplikací data. (Další informace, přejděte toohello integrace s části služby Stínová kopie svazku systému Windows.) S StorSimple Snapshot Manager můžete vytvořit plánů zálohování nebo provést okamžitou zálohování podle potřeby. Pokud potřebujete toorestore data ze zálohy, umožňuje Snapshot Manager zařízení StorSimple, vyberte z katalogu místní nebo cloudové snímky. Azure StorSimple obnoví pouze hello data, která je potřeba, protože je to potřeba, což zabraňuje zpoždění v dostupnost dat během operace obnovení).

![Architektura Snapshot Manager zařízení StorSimple](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**Architektura Snapshot Manager zařízení StorSimple** 

## <a name="support-for-multiple-volume-types"></a>Podpora pro více typů svazku
Můžete použít hello tooconfigure Snapshot Manager zařízení StorSimple a zálohovat následující typy svazků hello: 

* **Základní svazky** – vytvoření základního svazku je jednoho oddílu na základním disku. 
* **Jednoduché svazky** – jednoduchý svazek je dynamický svazek, který obsahuje místa na disku z jeden dynamický disk. Jednoduchý svazek se skládá z jedné oblasti na disku nebo více oblastí, které jsou spojeny dohromady na hello stejný disk. (Jenom na dynamické disky můžete vytvořit jednoduché svazky.) Jednoduché svazky nejsou odolné proti chybám.
* **Dynamické svazky** – dynamický svazek je svazek vytvořený na dynamický disk. Dynamické disky používají informace o databázi tootrack o svazcích, které jsou obsaženy na dynamické disky v počítači. 
* **Dynamické svazky s zrcadlení** – dynamické svazky s zrcadlení jsou postaveny na architektuře hello RAID 1. S RAID 1 identické data zapsána na dva nebo víc disku, který vytvořil sadu zrcadlených. Požadavek čtení může následně ošetřit každý disk, který obsahuje hello požadovaná data.
* **Sdílených svazcích clusteru** – s sdílených svazcích clusteru (CSV), více uzlů v clusteru s podporou převzetí služeb při selhání můžete současně čtení nebo zápisu toohello stejný disk. Převzetí služeb při selhání z jednoho uzlu tooanother uzlu může dojít, rychle a bez nutnosti změny vlastnictví disku nebo připojení, odpojení a odebrání svazku. 

> [!IMPORTANT]
> Nemíchat sdílené svazky clusteru a bez CSV hello stejné snímku. Kombinování sdílené svazky clusteru a bez CSV snímku se nepodporuje. 
> 
> 

Můžete použít skupiny celý svazek StorSimple Snapshot Manager toorestore nebo klonovat jednotlivé svazky a obnovit jednotlivé soubory.

* [Svazky a svazek skupiny](#volumes-and-volume-groups) 
* [Typy zálohování a zásady zálohování](#backup-types-and-backup-policies) 

Další informace o funkcích Snapshot Manager zařízení StorSimple a jak toouse, najdete v části [StorSimple Snapshot Manager uživatelské rozhraní](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Svazky a svazek skupiny
S StorSimple Snapshot Manager, můžete vytvářet svazky a nakonfigurujete je do skupin svazku. 

StorSimple Snapshot Manager používá svazek skupiny toocreate záložní kopie, které jsou konzistentní s aplikací. Konzistence aplikací existuje, když všechny související soubory a databáze jsou synchronizovány a představují hello true stavu aplikace v určitém bodě v čase. Svazek skupiny (která se také označují jako *konzistence skupin*) tvoří základ hello zálohování nebo obnovení úlohy.

Svazek skupiny nejsou hello stejné jako kontejnery svazků. Kontejner svazků obsahuje jeden nebo více svazků, které sdílejí účet cloudového úložiště a další atributy, jako je například šifrování a šířky pásma. Kontejner jednom svazku může obsahovat až too256 dynamicky zajišťované svazky zařízení StorSimple. Další informace o kontejnery svazků, přejděte příliš[spravovat vaše kontejnery svazků](storsimple-manage-volume-containers.md). Svazek skupiny jsou kolekce svazků, který konfigurujete toofacilitate operace zálohování. Pokud vyberete dva svazky, které patří toodifferent kontejnery svazků, umístěte je v jednom svazku skupiny a pak vytvořit zásady zálohování pro tuto skupinu svazku, každý svazek budou zálohovány v kontejneru hello vhodný svazek pomocí příslušné úložiště hello účet.

> [!NOTE]
> Všechny svazky ve skupině svazku musí pocházet ze zprostředkovatele služeb jeden cloud.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integrace s Windows služby Stínová kopie svazku
StorSimple Snapshot Manager používá hello konzistentní s aplikací data toocapture Windows Stínová kopie svazku Service (VSS). Služby Stínová kopie svazku zajišťuje konzistence aplikací navázat komunikaci s toocoordinate aplikace používající stínovou kopii svazku hello vytváření přírůstkové snímků. VSS zajistí aplikace hello dočasně neaktivní nebo tichém, při pořizování snímků. 

Hello StorSimple Snapshot Manager implementace služby Stínová kopie svazku funguje pro SQL Server a obecné svazky systému souborů NTFS. proces Hello vypadá takto: 

1. Žadatel, který je obvykle Správa dat a řešení ochrany (například StorSimple Snapshot Manager) nebo aplikace pro zálohování, vyvolá služby Stínová kopie svazku a požádá ho toogather informace z hello zapisovače softwaru v cílové aplikace hello.
2. Kontakty služby Stínová kopie svazku hello zapisovače součást tooretrieve popis hello data. Zapisovač Hello vrací hello popis hello toobe data zálohovat. 
3. VSS signály hello zapisovače tooprepare hello aplikace pro zálohování. Zapisovač Hello připraví hello data pro zálohování provedením otevřených transakcí na aktualizace protokoly transakcí a tak dále a poté oznámí VSS.
4. VSS dá pokyn hello zapisovače tootemporarily zastavení hello data aplikace ukládá a ujistěte se, když se vytvoří hello stínové kopie se nezapisují žádná data toohello svazku. Tento krok zajišťuje konzistenci dat a trvá víc než 60 sekund.
5. VSS dá pokyn hello zprostředkovatele toocreate hello stínové kopie. Zprostředkovatele, které může být softwaru nebo hardwaru – na základě, spravovat hello svazky, které jsou aktuálně spuštěné a vytvoření stínové kopie je na vyžádání. Zprostředkovatel Hello vytvoří hello stínové kopie a upozorní služby Stínová kopie svazku, až se dokončí.
6. VSS kontakty hello zapisovače toonotify hello aplikaci, která můžete obnovit vstupně-výstupních operací a také tooconfirm, vstupně-výstupní operace byla úspěšně pozastavena během stínové kopírování vytvoření. 
7. Pokud hello kopírování byla úspěšná, vrátí služby Stínová kopie svazku hello žadatele toohello umístění pro kopírování. 
8. Data byla zapsána při vytvoření stínové kopie hello, hello zálohování bude být nekonzistentní. Služby Stínová kopie svazku odstraní hello stínové kopie a upozorní hello žadatele. Hello žadatele můžete buď automaticky opakovat proces zálohování hello nebo odeslání oznámení hello správce tooretry ji později.

Viz následující obrázek hello.

![Proces služby Stínová kopie svazku](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Proces systému Windows služby Stínová kopie svazku** 

## <a name="backup-types-and-backup-policies"></a>Typy zálohování a zásady zálohování
S StorSimple Snapshot Manager můžete zálohovat data a uložte ho místně a v cloudu hello. StorSimple Snapshot Manager tooback dat můžete použít okamžitě, nebo můžete použít zásady zálohování toocreate plánu automaticky umožňoval vytvářet zálohy pro. Zásady zálohování také povolit toospecify kolik snímky budou zachována. 

### <a name="backup-types"></a>Typy zálohování
Můžete použít následující typy záloh hello toocreate StorSimple Snapshot Manager:

* **Místní snímky** – místní snímky jsou v daném okamžiku kopie svazku data, která jsou uložená na zařízení StorSimple hello. Tento typ zálohy obvykle, můžete vytvořit a rychle obnovit. Stejně jako místní záložní kopii můžete použít místní snímek.
* **Cloudových snímků** – cloudových snímků jsou v daném okamžiku kopie svazku data, která jsou uložená v cloudu hello. Cloudový snímek je ekvivalentní tooa snímku replikují na jiný, odlehlého úložiště systému. Cloudové snímky jsou obzvláště užitečná ve scénářích zotavení po havárii.

### <a name="on-demand-and-scheduled-backups"></a>Zálohování na vyžádání a naplánované
S StorSimple Snapshot Manager můžete zahájit jednorázové zálohování toobe vytvořena ihned, nebo můžete použít zásady zálohování tooschedule, opakování operace zálohování.

Zásady zálohování je sada automatizovaných pravidel, které můžete použít tooschedule pravidelných záloh. Zásady zálohování můžete toodefine hello četnost a parametry pořizování snímků skupinu konkrétní svazku. Zásady toospecify počátku a ukončení platnosti data, časy, četnosti a uchování požadavky, můžete použít pro obě místní a cloudových snímků. Zásada je použita ihned po jeho definujete. 

Můžete použít tooconfigure Snapshot Manager zařízení StorSimple nebo změnit konfiguraci zásady zálohování, kdykoli je to nezbytné. 

Můžete nakonfigurovat následující informace pro každé zásadě zálohování, který vytvoříte hello:

* **Název** – jedinečný název hello hello vybrané zásady zálohování.
* **Typ** – hello typ zásady zálohování; místní snímek nebo cloudový snímek.
* **Svazek skupiny** – hello svazku skupiny toowhich hello vybrané zálohování zásady přiřazeny.
* **Uchování** – počet záložních kopií tooretain hello. Pokud zaškrtnete hello **všechny** pole všechny záložní kopie uchovávají až do dosažení hello maximální počet záložních kopií na každý svazek, na které bodu hello zásad se nezdaří a generovat chybovou zprávu. Alternativně můžete zadat počet tooretain zálohování (od 1 do 64).
* **Datum** – hello datum vytvoření zásady zálohování hello.

Informace o konfiguraci zásady zálohování, přejděte příliš[toocreate použití StorSimple Snapshot Manager a spravovat zásady zálohování](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Úloha zálohování monitorování a Správa
Můžete použít hello toomonitor Snapshot Manager zařízení StorSimple a spravovat nadcházející, plánované a dokončené úlohy zálohování. Kromě toho StorSimple Snapshot Manager poskytuje katalog zálohování too64 byla dokončena. Můžete použít hello katalogu toofind a obnovit svazky nebo jednotlivé soubory. 

Informace o monitorování úlohy zálohování přejděte příliš[tooview použití StorSimple Snapshot Manager a spravovat úlohy zálohování](storsimple-snapshot-manager-manage-backup-jobs.md).

## <a name="next-steps"></a>Další kroky
* Další informace o [pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).
* Stáhněte si [Snapshot Manager zařízení StorSimple](https://www.microsoft.com/download/details.aspx?id=44220).

