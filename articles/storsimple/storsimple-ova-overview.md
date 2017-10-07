---
title: "Přehled Azure StorSimple virtuální pole aaaMicrosoft | Microsoft Docs"
description: "Popisuje hello pole virtuální zařízení StorSimple, o řešení integrované úložiště, který spravuje úlohy úložiště mezi virtuální pole místního a cloudového úložiště Microsoft Azure."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/09/2016
ms.author: alkohli
ms.openlocfilehash: 8978e074142940748857150cc93b37272349d53d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-storsimple-virtual-array"></a>Úvod toohello pole virtuální zařízení StorSimple
## <a name="overview"></a>Přehled
Hello Microsoft Azure StorSimple virtuální pole je úložiště integrované řešení, které spravuje úlohy úložiště mezi místní virtuální pole v hypervisoru a cloudového úložiště Microsoft Azure. pole virtuálním Hello je efektivní, nákladově efektivní a snadno spravovaného souborového serveru nebo řešení serveru iSCSI, který eliminuje řadu hello problémy a náklady spojené s enterprise úložiště a ochranu dat. pole virtuálním Hello je obzvláště vhodné pro vzdálené office/pobočkách.

Toto téma obsahuje přehled virtuální pole hello – Zde jsou některé další prostředky:

* Osvědčené postupy, najdete v části [osvědčené postupy pole virtuální zařízení StorSimple](storsimple-ova-best-practices.md).
* Přehled hello StorSimple 8000 řady zařízení přejděte příliš[řady StorSimple 8000: hybridní cloudové řešení](storsimple-overview.md). 
* Informace o zařízení řady StorSimple 5000/7000 hello přejděte příliš[StorSimple Online nápovědy](http://onlinehelp.storsimple.com/).

pole virtuálním Hello podporuje hello iSCSI nebo zpráva bloku protokol Server (SMB). Je kompatibilní s vaší stávající infrastruktury hypervisoru a poskytuje vrstvení toohello cloud, zálohování, rychlé obnovení, obnovení na úrovni položek a funkce pro obnovení po havárii.

Hello následující tabulka shrnuje důležité funkce hello hello pole virtuální zařízení StorSimple.

| Funkce | StorSimple Virtual Array |
| --- | --- |
| Požadavky na instalaci |Používá infrastrukturu virtualizaci (Hyper-V nebo VMware) |
| Dostupnost |Jeden uzel |
| Celková kapacita (včetně cloudu) |Až too64 TB použitelné kapacity na virtuální pole |
| Místní kapacity |Použitelné kapacity too6.4 TB 390 GB na virtuální pole (třeba tooprovision 500 GB too8 TB místa na disku) |
| Nativní protokoly |iSCSI nebo SMB |
| Plánovaná doba obnovení (RTO) |iSCSI: méně než 2 minuty bez ohledu na velikost |
| Cíl bodu obnovení (RPO) |Denní zálohy a zálohy na vyžádání |
| Vrstvení úložiště |Používá vytápění toodetermine mapování, jaká data by měla být rozvrstvena příchozí nebo odchozí |
| Podpora |Infrastruktury virtualizace podporuje hello dodavatele |
| Výkon |Se liší v závislosti na základní infrastruktury |
| Data nastavení mobilních zařízení |Můžete obnovit toohello stejné zařízení nebo obnovení na úrovni položek (souborový server) |
| Vrstvy úložiště |Úložiště na místním hypervisoru a cloud |
| Velikost sdílené složky |Vrstvený: až too20 TB; místně vázaný: až too2 TB |
| Velikost svazku |Vrstvený: too5 TB 500 GB; místně vázaný: 50 GB too500 GB |
| Velikost svazku |Vrstvený: až too5 TB; místně vázaný: až too500 GB |
| Snímky |Konzistentní havárií |
| Obnovení na úrovni položek |Ano; Uživatelé mohou obnovit ze sdílených složek |

## <a name="why-use-storsimple"></a>Proč používat StorSimple?
StorSimple připojí uživatelé a servery úložiště tooAzure v minutách, bez úprav aplikace.

Hello následující tabulka popisuje některé hello klíčové výhody této hello pole virtuální zařízení StorSimple řešení poskytuje.

| Funkce | Výhoda |
| --- | --- |
| Transparentní integrace |pole virtuálním Hello podporuje hello iSCSI nebo protokol SMB hello. Hello přesun dat mezi místní vrstvy hello a vrstvy cloudu hello je snadné a transparentní toohello uživatelem. |
| Náklady na menší úložiště |StorSimple můžete zřizovat dostatečná místní úložiště toomeet aktuální požadavky pro aktivní data hello nejvíce použít. Když je úložiště potřebovat růst, StorSimple vrstev pomaleji přístupná data do nákladově efektivní cloudového úložiště. Hello dat je s odstraněním duplicitních dat a komprimované před odesláním toohello cloudu toofurther snížit požadavky na úložiště a náklady. |
| Jednodušší správu úložišť |StorSimple poskytuje centralizovanou správu v cloudu hello pomocí Správce zařízení StorSimple toomanage více zařízení. |
| Zotavení po havárii vylepšené a dodržování předpisů |StorSimple usnadňuje rychlejší zotavení po havárii tím, že hello metadata okamžitě obnovení a obnovení dat hello podle potřeby. To znamená, že můžete pokračovat normální provozní podmínky s minimálním dopadem. |
| Data nastavení mobilních zařízení |Data, která vrstvené toohello cloudu lze přistupovat z jiných lokalitách pro účely obnovení a migrace. Všimněte si, že můžete obnovit data pouze toohello původní virtuální pole. Můžete však použít po havárii obnovení funkce toorestore hello celý virtuální pole tooanother virtuální pole. |

## <a name="storsimple-workload-summary"></a>Souhrn úloh StorSimple

Souhrnné informace o podporovaných úlohách StorSimple v následující tabulce.

|Scénář     |Úloha     |Podporuje se      |Omezení               |
|-------------|-------------|---------------|---------------------------|
|ROBO spolupráce |Sdílení souborů     |Ano      |V tématu [maximální limit pro souborový server](storsimple-ova-limits.md).<br></br>V tématu [požadavky na systém pro podporované verze SMB](storsimple-ova-system-requirements.md).| Všechny verze     |

## <a name="workflows"></a>Pracovní postupy
Hello pole virtuální zařízení StorSimple je obzvláště vhodný pro hello následující pracovních postupů:

* [Správu cloudového úložiště](#cloud-based-storage-management)
* [Zálohování nezávislých na umístění](#location-independent-backup)
* [Data protection a zotavení po havárii](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>správu cloudového úložiště
Je možné použít službu StorSimple Manager zařízení hello spuštěn v hello Azure portálu toomanage data uložená na několika zařízeních a na více místech. To je zvlášť užitečný ve scénářích, distribuované větev. Všimněte si, že musíte vytvořit samostatné instance hello Správce zařízení StorSimple služby toomanage virtuální pole a fyzického zařízení StorSimple. Všimněte si také, že tento virtuální pole hello teď používá nový portál Azure hello místo hello portál Azure classic.

![správu cloudového úložiště](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Zálohování nezávislých na umístění
S polem virtuálním hello zadejte cloudových snímků nezávislých na umístění, v okamžiku kopie svazku nebo sdílené složky. Cloudové snímky jsou ve výchozím nastavení povolené a nejde zakázat. Všechny svazky a sdílené složky jsou zálohování nahoru v hello stejný čas prostřednictvím jednoho denní zásady zálohování, a můžete provést další ad-hoc zálohy, kdykoli je to nezbytné.

### <a name="data-protection-and-disaster-recovery"></a>Data protection a zotavení po havárii
pole virtuálním Hello podporuje následující scénáře obnovení dat ochrany a po havárii hello:

* **Svazek nebo sdílenou složku obnovení** – použijte hello obnovení jako nový pracovní postup toorecover svazku nebo sdílené složky. Použijte tento postup toorecover hello celý svazek nebo sdílenou složku.
* **Obnovení na úrovni položek** – sdílené složky záloh toorecent povolit zjednodušené přístup. Je možné snadno obnovit soubor z speciální *.backup* dostupnou složku v cloudu hello. Tato možnost obnovení je řízené uživatele a není nutný žádný zásah správce.
* **Zotavení po havárii** – použijte všechny svazky nebo sdílené složky tooa nové virtuální pole hello toorecover schopnosti převzetí služeb při selhání. Vytvořit nový virtuální pole hello a zaregistrovat ji pomocí hello služby StorSimple Manager zařízení a pak převzít hello původní virtuální pole. nové virtuální pole Hello pak převezme hello zřízené prostředky. 

## <a name="storsimple-virtual-array-components"></a>Součásti pole virtuální zařízení StorSimple
pole virtuálním Hello zahrnuje hello následující součásti:

* [Virtuální pole](#virtual-array) – zařízením hybridní cloudové úložiště založené na virtuálním počítači zřízenou ve virtualizovaném prostředí nebo hypervisoru.  
* [Služba StorSimple Manager zařízení](#storsimple-device-manager-service) – na rozšíření hello portálu Azure, který vám umožní spravovat jedno nebo více zařízení StorSimple z jedné webové rozhraní, které je přístupné z různých míst na zeměkouli. Můžete použít toocreate služby StorSimple Manager zařízení hello a spravovat služby, zobrazit a spravovat zařízení a výstrahy a spravovat svazky, sdílené složky a existující snímky.
* [Místní webové uživatelské rozhraní](#local-web-user-interface) – uživatelské rozhraní založené na webu, které je použité tooconfigure hello zařízení tak, aby můžete připojit toohello místní sítě a potom proveďte registraci hello zařízení pomocí služby StorSimple Manager zařízení hello. 
* [Rozhraní příkazového řádku](#command-line-interface) – rozhraní Windows PowerShell, abyste používali toostart relaci podpory na pole virtuálním hello.
  Hello následující části popisují každou z těchto součástí podrobněji a popisují, jak řešení hello uspořádá data, přidělí úložiště a usnadňuje správu úložiště a ochranu dat.

### <a name="virtual-array"></a>Virtuální pole
pole virtuálním Hello je řešení jeden uzel úložiště, které poskytuje primárního úložiště, spravuje komunikace s cloudovým úložištěm a pomáhá tooensure hello zabezpečení a důvěrnost všechna data uložená na zařízení hello.

pole virtuálním Hello je k dispozici v jednom modelu, který je k dispozici ke stažení. pole virtuálním Hello má 6.4 TB na zařízení hello (s základní požadavek úložiště o velikosti 8 TB) a 64 TB včetně maximální kapacitu cloudového úložiště. 

pole virtuálním Hello má hello následující funkce:

* Je nákladově efektivní. Je díky použití existující infrastruktury virtualizace a dá se nasadit na vaše stávající technologie Hyper-V nebo VMware hypervisoru.
* To se nachází v datovém centru hello a může být nakonfigurován jako server se službou iSCSI nebo souborový server. 
* Je integrovaný s cloudem hello.
* Zálohy se ukládají do hello cloudu, což může usnadnit zotavení po havárii a zjednodušit obnovení na úrovni položek (ILR). 
* Aktualizace toohello virtuální pole, můžete použít stejně, jako by aplikovat tooa fyzického zařízení.

> [!NOTE]
> Virtuální pole nelze rozšířit. Proto je důležité tooprovision odpovídající úložiště při vytváření virtuální pole hello. 
> 
> 

### <a name="storsimple-device-manager-service"></a>Služba StorSimple Manager zařízení
Microsoft Azure StorSimple poskytuje webové uživatelské rozhraní, hello služby StorSimple Manager zařízení, která umožňuje toocentrally můžete spravovat úložiště StorSimple. Můžete použít hello Správce zařízení StorSimple služby tooperform hello následující úlohy:

* Několik polí virtuální zařízení StorSimple spravujte z jedné služby. 
* Konfigurovat a spravovat nastavení zabezpečení pro pole virtuální zařízení StorSimple. (Šifrování v cloudu hello je závislá na rozhraní API Microsoft Azure.)
* Nakonfigurujte přihlašovací údaje účtu úložiště a vlastnosti.
* Konfigurovat a spravovat svazky nebo sdílené složky.
* Zálohování a obnovení dat na svazky nebo sdílené složky.
* Sledování výkonu.
* Zkontrolujte nastavení systému a identifikovat možné problémy.

Použijete hello Správce zařízení StorSimple služby tooperform každodenní správu virtuální pole.

Další informace, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-virtual-array-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Místní webové uživatelské rozhraní
pole virtuálním Hello zahrnuje uživatelského rozhraní založené na webu, který se používá pro jednorázovou konfiguraci a registraci zařízení hello s hello služby StorSimple Manager zařízení. Můžete můžete použít tooshut dolů a restartovat virtuální pole hello, spuštění diagnostických testů, aktualizace softwaru, změnit heslo správce zařízení hello, zobrazit protokoly systému a obraťte se na Microsoft Support toofile žádosti o službu. 

Informace o používání hello webové uživatelské rozhraní, přejděte příliš[použití hello webové uživatelské rozhraní tooadminister pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Rozhraní příkazového řádku
rozhraní Windows PowerShell Hello zahrnuté umožňuje tooinitiate relaci podporu s Microsoft Support tak, aby se vám může pomoct vyřešit problémy, které můžete narazit na vaše virtuální pole.

## <a name="storage-management-technologies"></a>Technologie správy úložiště
Kromě toho toohello virtuální pole a další součásti hello StorSimple řešení používá hello následující data tooimportant softwaru technologie tooprovide rychlý přístup, snížení spotřeby úložiště a chránit data uložená na vaše virtuální pole:

* [Automatické úložiště vrstvení](#automatic-storage-tiering) 
* [Místně vázaný sdílených složek a svazků](#locally-pinned-shares-and-volumes)
* [Odstranění duplicitních dat a komprese dat vrstvené nebo zálohovat toohello cloudu](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
* [Zálohy plánované a na vyžádání](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatické úložiště vrstvení
pole virtuálním Hello používá nová data vrstvení toomanage uložené mechanismus mezi hello virtuální pole a hello cloudu. Existují pouze dvě úrovně: Azure a místní virtuální pole hello cloudového úložiště. Hello pole virtuální zařízení StorSimple automaticky uspořádá data do hello vrstvami podle toho, heat mapa, který sleduje aktuální využití, stáří a vztahy tooother data. Data, která je většina aktivní (nejprodávanějších) uložené lokálně, při nižší aktivní i neaktivní data jsou automaticky migrovat toohello cloudu. (Všechny zálohy jsou uložené v cloudu hello). StorSimple upraví a změní uspořádání dat a změnit přiřazení úložiště jako vzorce používání. Některé informace mohou být například míň aktivní v čase. Jakmile je k progresivnímu míň aktivní, je vrstvené out toohello cloudu. Pokud tato data zase aktivní, je vrstvené v toohello pole úložiště.

Data pro konkrétní vrstvené složky nebo svazku záruku, že místo svůj vlastní místní vrstvy. (přibližně 10 % hello celkové zřízené místo pro tuto sdílenou složku nebo svazek). Když to snižuje hello úložiště k dispozici na hello virtuální pole pro tuto sdílenou složku nebo svazek, zajišťuje, že vrstvení pro jednu sdílenou složku nebo svazku se nevztahuje hello vrstvení potřebám jiné sdílené složky nebo svazky. Velmi zaneprázdněny zatížení v jedné sdílené složky nebo svazku, proto nemůže vynutit všechny ostatní cloudové toohello úlohy. 

![Automatické úložiště vrstvení](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> Můžete zadat jako místně vázaný svazek, v takovém případě hello data zůstane v poli virtuálním hello a nikdy vrstvené toohello cloudu. Další informace, přejděte příliš[místně vázaný sdílených složek a svazků](#locally-pinned-shares-and-volumes).
> 
> 

### <a name="locally-pinned-shares-and-volumes"></a>Místně vázaný sdílených složek a svazků
Můžete vytvořit odpovídající složkami a svazky jako místně vázaný. Tato možnost zajistí, že data požadovaná kritické aplikace zůstává v poli virtuálním hello a se nikdy vrstvené toohello cloudu. Místně vázaný složkami a svazky mají hello následující funkce: 

* Nejsou subjektu toocloud latenci nebo problémy s připojením.
* Přesto využívat výhod StorSimple zálohování a po havárii obnovení funkce cloudu.

Můžete obnovit místně vázaný sdílené složky nebo svazku jako vrstvené nebo vrstvené sdílené složky nebo svazku jako místně připojené. 

Další informace o místně vázaných svazků, přejděte příliš[použít hello Správce zařízení StorSimple služby toomanage svazky](storsimple-virtual-array-manage-volumes.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-toohello-cloud"></a>Odstranění duplicitních dat a komprese dat vrstvené nebo zálohovat toohello cloudu
StorSimple používá odstraňování duplicitních dat a dat komprese toofurther snížit požadavky na úložiště v cloudu hello. Odstranění duplicitních dat snižuje hello celkové množství dat uložených odstraněním redundance v sadě dat hello uložené. Informace o změní, StorSimple ignoruje data hello beze změny a zachycení pouze hello změny. Kromě toho StorSimple snižuje hello množství uložených dat určením a odebrání duplicitní informace. 

> [!NOTE]
> Data uložená na pole virtuálním hello není s odstraněním duplicitních dat nebo komprimován. Všechny odstranění duplicitních dat a komprese dojde k těsně před hello data se odesílají toohello cloudu.
> 
> 

### <a name="scheduled-and-on-demand-backups"></a>Zálohy plánované a na vyžádání
Funkce ochrany dat StorSimple umožňují toocreate zálohy na vyžádání. Výchozí plán zálohování také zajišťuje, že data zálohovat denně. Ve formuláři hello přírůstkové snímků, které jsou uloženy v cloudu hello jsou provedeny zálohy. Snímky, které záznam pouze hello změny od poslední zálohy hello, můžete vytvořit a rychle obnovit. Tyto snímky může být důležité ve scénářích zotavení po havárii, protože nahradit sekundární úložných systémů (například zálohování na pásku) a povolit tak toorestore data tooyour datacenter nebo tooalternate lokality v případě potřeby.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak příliš[Příprava portálu virtuální pole hello](storsimple-virtual-array-deploy1-portal-prep.md).

