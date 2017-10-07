---
title: "aaaStorSimple místně připojené svazky – nejčastější dotazy | Microsoft Docs"
description: "Poskytuje odpovědi toofrequently zobrazí výzva, že dotazy týkající se StorSimple místně připojené svazky."
services: storsimple
documentationcenter: NA
author: manuaery
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2017
ms.author: manuaery
ms.openlocfilehash: a3a6557ca15e7e1947b45dcfd005640103c09591
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple místně připojené svazky: Nejčastější dotazy (FAQ)
## <a name="overview"></a>Přehled
Následující Hello jsou otázky a odpovědi, může být při vytváření svazek StorSimple místně vázaný převést tooa místně vázaný svazek vrstvený svazek (a naopak), nebo zálohování a obnovení místně vázaný svazek.

Otázky a odpovědi jsou uspořádány do následujících kategorií hello

* Vytváření místně vázaný svazek
* Zálohování místně vázaný
* Převádění tooa místně vázaný svazek vrstvený svazek
* Obnovení místně vázaný svazek
* Při přechodu místně vázaný svazek

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Dotazy týkající se vytváření místně vázaný svazek
**Otázka:** Jaká je maximální velikost hello místně vázaný svazek, který můžete vytvořit na zařízeních řady 8000 hello?

**A** na zařízení se systémem StorSimple 8000 řady aktualizace 3.0, můžete zřídit místně vázaných svazků až too8.5 TB a vrstvené svazky až too200 TB na zařízení 8100 hello. Větší zařízení 8600 hello můžete zřídit místně vázaných svazků až too22.5 TB a vrstvené svazky až too500 TB.

**Otázka:** Došlo k inovaci nedávno Moje tooUpdate zařízení 8100 3.0 a při toocreate místně vázaný svazek se maximální velikost dostupné hello je pouze 6 TB a není šířka 8,5 TB. Proč nelze vytvořit svazek šířka 8,5 TB?

**A** Pokud vaše zařízení používá aktualizace 3.0, můžete zřizovat místně vázané svazky až too8.5 TB nebo vrstvené svazky až too200 TB na hello 8100 zařízení. Pokud vaše zařízení už zřízeny vrstvené svazky, hello místa pro vytvoření místně vázaný svazek bude úměrně nižší než tento maximální limit. Například, pokud již byly zajištěny přibližně 106 TB vrstvené svazky na vašem zařízení 8100 (což je polovinu hello víceúrovňová kapacity), pak odpovídajícím způsobem omezená too4 TB (maximální velikost místního svazku, který můžete vytvořit pro zařízení hello 8100 hello zhruba polovinu hello maximální místně připnuli kapacita svazku).

Některé volné místo v zařízení hello je použité toohost hello pracovní sady vrstvených svazků, a proto se snižuje hello volné místo pro vytvoření místně vázaný svazek, pokud zařízení hello zřízeny vrstvené svazky. Naopak vytváření místně vázaný svazek úměrně snižuje hello volné místo pro vrstvené svazky. Následující tabulky Hello shrnuje dostupné vrstvené kapacity hello na zařízeních hello 8100 a 8600 při vytváření místně vázaných svazků.

#### <a name="update-30"></a>Aktualizace 3.0 
| Zřízená kapacita místně vázaných svazků | Dostupná kapacita toobe zřízené pro vrstvené svazky - 8100 | Dostupná kapacita toobe zřízené pro vrstvené svazky - 8600 |
| --- | --- | --- |
| 0 |200 TB |500 TB |
| 1 TB |176.5 TB |477.8 TB |
| 4 TB |105.9 TB |411.1 TB |
| 8.5 TB |0 TB |311.1 TB |
| 10 TB |Není k dispozici |277.8 TB |
| 15 TB |Není k dispozici |166.7 TB |
| 22,5 TB |Není k dispozici |0 TB |

**Otázka:** Proč vytváření místně vázaných svazků je dlouhotrvající operace?

**Odpověď:** Místně vázaných svazků jsou tlustě zřízený. místo toocreate na hello místních vrstvách hello zařízení, může být některá data z vrstvené svazky posunuta toohello cloudu během procesu zřizování hello. A vzhledem k tomu, že to závisí na velikosti svazku hello se zřídí hello stávající data v zařízení a hello dostupnou šířku pásma toohello cloudu, hello hello doba trvání toocreate místní svazek, který může být několik hodin.

**Otázka:** Jak dlouho trvá toocreate místně vázaný svazek?

**Odpověď:** Protože jsou místně vázaných svazků tlustě zřízený, může některá existující data z vrstvené svazky poslat toohello cloudu během procesu zřizování hello. Proto hello toocreate doba, kterou místně vázaný svazek závisí na několika faktory, včetně hello velikost svazku hello, hello dat ve vašem zařízení a hello dostupnou šířku pásma. V nově instalovaném zařízení, které má žádné svazky hello čas toocreate místně vázaný svazek je přibližně 10 minut za terabajt data. Vytváření místních svazků však může trvat několik hodin, které jsou založeny na faktorech hello vysvětlené dřív na zařízení, která je používána.

**Otázka:** Chci toocreate místně vázaný svazek. Existují veškeré nejlepší postupy, které je potřeba toobe vědět?

**Odpověď:** Místně vázaných svazků jsou vhodné pro úlohy, které vyžadují místní záruky dat po celou dobu a jsou citlivá toocloud latenci. Při určování využití místní svazky v žádném z vašich zatížení, uvědomte hello následující:

* Místně vázaných svazků jsou tlustě zřízený a vytváření místních svazků ovlivní hello volné místo pro vrstvené svazky. Proto doporučujeme začít s menším svazky a škálovat jako požadavek zvyšuje vašeho úložiště.
* Zřizování místních svazků je dlouhotrvající operace, které mohou zahrnovat nabízet existující data z cloudu toohello vrstvené svazky. V důsledku toho může docházet v těchto svazcích snížený výkon.
* Zajištění místní svazků je časově náročná operace. Skutečný čas Hello související se situací, závisí na několika faktorech: hello velikost svazku hello se zřídí, data na zařízení a dostupnou šířku pásma. Pokud nebyla zálohována existující svazky toohello cloudu, je vytvoření svazku pomalejší. Doporučujeme, že provedete cloudových snímků existující svazky než zřídíte místní svazek.
* Můžete převést existující svazky toolocally připnutý vrstvené svazky a tento převod zahrnuje zřizování místa v zařízení hello hello výsledná místně vázaný svazek (v přidání toobringing dolů vrstvené data, pokud existuje, z cloudu hello). Znovu toto je dlouhotrvající operace, které závisí na faktorech, které jsme probrali výše. Doporučujeme zálohovat stávající předchozí tooconversion svazky jako hello proces bude i pomalejší, pokud nejsou existující svazky zálohovat. Zařízení může také dojít k omezení výkonu během tohoto procesu.

Další informace o příliš[vytvořit místně vázaný svazek](storsimple-8000-manage-volumes-u2.md#add-a-volume)

**Otázka:** Můžete vytvořit více místně vázaných svazků na hello současně?

**Odpověď:** Ano, ale všechny úlohy vytváření a rozšíření místně vázaný svazek se provádějí postupně.

Tlustě zřízený místně vázaných svazků to vyžaduje vytvoření volné místo na hello zařízení (která může mít za následek existující data ze vrstvené svazky toobe nabídnutých toohello cloudu během procesu zřizování hello). Proto pokud právě probíhá zřizování úlohy, jiné místní svazek vytvoření úlohy se zařadí do fronty až do dokončení této úlohy.

Podobně pokud je rozbalována existující místní svazek nebo vrstvený svazek, který je převáděn tooa místně připnutý svazek a potom hello vytvoření nové místně vázaný svazek je zařadit do fronty až do dokončení předchozí úlohy hello. Rozšiřování hello velikost místně vázaný svazek zahrnuje rozšíření hello hello existující místní místa pro tento svazek. Převod ze svazku vrstvené toolocally připnutý také zahrnuje vytvoření hello volné místo pro hello výsledná místně vázaný svazek. V obou těchto operací, vytvoření nebo rozšíření volné místo s dlouhým běží úlohy.

Tyto úlohy můžete zobrazit v hello **úlohy** okno hello služby StorSimple Manager zařízení. Hello úlohu, která se aktivně zpracovává se průběžně aktualizovat tooreflect hello průběh zřizování místa. Hello zbývající místně vázaný svazek úlohy jsou označeny jako spuštěná, ale jejich průběhu je zastaven a proces a jsou zachyceny v hello pořadí, v jakém že byly zařazeny do fronty.

**Otázka:** Uživatel odstranil místně vázaný svazek. Proč nezobrazuje, že hello uvolnit místo promítnuta dostupné místo hello při toocreate nový svazek?

**Odpověď:** Pokud odstraníte místně vázaný svazek, hello místa na disku pro nové svazky nemusí být okamžitě aktualizován. Hello služby StorSimple Manager zařízení aktualizuje hello volné místo dostupné přibližně za hodinu. Doporučujeme že čekat na jednu hodinu, než se pokusíte toocreate hello nový svazek.

**Otázka:** Jsou podporovány místně vázaných svazků na hello cloudu zařízení?

**Odpověď:** Místně vázaných svazků nepodporuje hello cloudu zařízení (8010 a 8020 zařízení dříve odkazované tooas hello virtuálního zařízení StorSimple).

**Otázka:** Můžete použít toocreate rutin prostředí Azure PowerShell hello a spravovat místně vázaných svazků?

**Odpověď:** Ne, nelze vytvořit místně vázaných svazků prostřednictvím rutin prostředí Azure PowerShell (víceúrovňová jakýkoli svazek, který vytvoříte pomocí prostředí Azure PowerShell). Také doporučujeme nepoužívat toomodify rutin prostředí Azure PowerShell hello všechny vlastnosti místně vázaný svazek, jak bude mít hello nežádoucí vliv na změny tootiered typ svazku hello.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Dotazy týkající se zálohování místně vázaný svazek
**Otázka:** Jsou místní snímky místně vázaných svazků, které jsou podporovány?

**Odpověď:** Ano, můžete využít místní snímky místně vázaných svazků. Ale důrazně doporučujeme, aby vám pravidelně zálohovat místně vázaných svazků s tooensure snímky cloudu, která data jsou chráněna v případě havárie hello.

Upozorňujeme, že místní snímky místně vázaných svazků můžete také vrstvy se toohello cloudu a se nezaručuje, že toostay v místní vrstvy hello hello zařízení.

**Otázka:** Existují žádné pokyny pro správu místních snímků místně vázaných svazků?

**Odpověď:** Časté místní snímky spolu s vysokou míru změn dat v hello místně vázaný svazek může způsobit, že volné místo na toobe zařízení hello rychle využívat a mít za následek data z vrstvené svazky se instaluje toohello cloudu. Doporučujeme proto, že můžete minimalizovat počet hello místní snímky.

**Otázka:** Zobrazila se výstraha s oznámením, že moje místní snímky místně vázaných svazků může zrušena. Když tomu může dojít?

**Odpověď:** Časté místní snímky spolu s vysokou míru změn dat v hello místně vázaný svazek může dojít volné místo na toobe zařízení hello rychle využívat. Pokud používáte výraznou hello místních vrstvách hello zařízení, výpadku rozšířené cloudu může způsobit hello zařízení maximálnímu zaplnění a příchozí zápisy toohello svazku může způsobit zneplatnění hello snímků (protože žádné místo existuje tooupdate hello snímky toorefer toohello starší bloky dat, která byla přepsání). V takové situaci hello bude pokračovat zápisy toohello svazku toobe zpracování, ale místní snímky hello může být neplatný. Neexistuje žádný dopad tooyour existující cloudových snímků.

Výstraha Hello je toonotify můžete tento tato situace nastat a ujistěte se, že adresa hello stejné včas, a to prohlédnutím místní snímky plány tootake méně časté místních snímků nebo odstraňování starší místních snímků, které jsou již vyžaduje se.

Pokud jsou zneplatněny hello místních snímků, obdržíte výstrahu informace, upozornění, že se zrušila platnost hello místních snímků pro konkrétní zásady zálohování hello spolu s hello seznam časová razítka hello místní snímků, které došlo ke zrušení platnosti. Tyto snímky bude automaticky odstraněna a už nebude možné tooview je v hello **zálohování katalogů** okno v hello portálu Azure.

## <a name="questions-about-converting-a-tiered-volume-tooa-locally-pinned-volume"></a>Otázky týkající se převodu tooa místně vázaný svazek vrstvený svazek
**Otázka:** Při převodu tooa místně vázaný svazek vrstvený svazek I mě sledování některé pomalost na hello zařízení. Proč je této situaci?

**Odpověď:** proces převodu Hello zahrnuje dva kroky:

1. Zřizování místa v zařízení hello hello brzy na--převést místně vázaný svazek.
2. Stahování žádné vrstvené dat z hello cloudu tooensure místní záruky.

Oba tyto kroky jsou dlouhé spuštění operace, které jsou závislé na hello velikost svazku hello převáděné dat na zařízení hello a dostupnou šířku pásma. Jako některá data z vrstvené svazky může jako součást procesu zřizování hello distribuována toohello cloudu, zařízení může dojít k omezení výkonu během této doby. Kromě toho může být pomalejší procesu převodu hello pokud:

* Existující svazky ještě nebyly zálohovány toohello cloudu; Proto doporučujeme zálohování se předchozí tooinitiating svazky převod.
* Byly použity zásady omezení šířky pásma, což by mohlo omezit hello dostupnou šířku pásma toohello cloudu; Doporučujeme proto, že máte vyhrazený 40 MB/s nebo další cloud toohello připojení.
* proces převodu Hello může trvat několik hodin z důvodu toohello několik faktorů vysvětlené dřív; Proto doporučujeme, aby při provádění této operace v době bez vrcholů nebo na tooavoid víkendu hello dopad na koncové příjemci.

Další informace o příliš[převést tooa místně vázaný svazek vrstvený svazek](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)

**Otázka:** Můžete zrušit operaci převodu svazku hello?

**Odpověď:** Ne, nelze hello operaci převodu hello Storno jednou inicioval. Jak je popsáno v předchozí otázce hello, je potřeba upozornit z hello možným problémům s výkonem, může dojít během procesu hello a použijte osvědčené postupy hello uvedených výše, pokud máte v plánu vaší převod.

**Otázka:** Pokud se nezdaří operace převodu hello co se stane toomy svazek?

**Odpověď:** Převod svazku může selhat z důvodu toocloud problémy s připojením. Hello zařízení se může zastavit nakonec procesu převodu hello po určitém počtu neúspěšných pokusů o toobring dolů vrstvené data z cloudu hello. V takové situaci typ svazku hello bude pokračovat toobe hello zdrojového svazku typu předchozí tooconversion, a:

* Kritická výstraha bude vyvolané toonotify o selhání převodu svazku hello. Další informace o [výstrahy související toolocally připnutý svazky](storsimple-8000-manage-alerts.md#locally-pinned-volume-alerts)
* Pokud převádíte vrstvené tooa místně vázaný svazek, svazek hello bude pokračovat tooexhibit vlastnosti vrstvený svazek jako dat může být stále umístěny v cloudu hello. Doporučujeme vyřešit problémy s připojením k hello a opakujte operaci převodu hello.
* Podobně při převodu z místně vázaný tooa vrstvené svazek selže, i když hello svazku budou označeny jako místně vázaný svazek, se bude fungovat jako vrstvený svazek (protože data může přesahovat toohello cloudu). Nicméně bude pokračovat toooccupy prostor v místních vrstvách hello hello zařízení. Tento prostor nebudete mít k dispozici pro ostatní místně vázaných svazků. Doporučujeme, potom opakujte tuto operaci tooensure, dokončení převodu svazku hello a hello volné místo na hello zařízení se nedá uvolnit.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Otázky týkající se obnovení místně vázaný svazek
**Otázka:** Jsou místně vázaných svazků okamžitě obnovit?

**Odpověď:** Ano, jsou okamžitě obnovit místně vázaných svazků. Co nejrychleji hello informace metadat pro svazek hello pocházejí z cloudu hello jako součást operace obnovení hello, hello svazek je uvést do režimu online a je přístupný pomocí hello hostitele. Místní záruky pro hello svazek, že data nebudou přítomen, dokud se všechna data hello byl stažen z hello cloudu a může dojít k však snižuje výkon v těchto svazcích hello dobu obnovení hello.

**Otázka:** Jak dlouho trvá toorestore místně vázaný svazek?

**Odpověď:** Místně vázaných svazků jsou obnoveny okamžitě a také hello svazku metadata informace získává z hello cloudu, zatímco stále hello svazku data toobe staženy hello pozadí uvést do režimu online. Této druhé části operace obnovení hello – získávání zpět hello místní záruky pro hello objemu dat – je dlouhotrvající operace a může trvat několik hodin pro všechna data toobe hello provedené místní znovu. Hello doba toocomplete hello stejné závisí na několika faktorech, například hello velikost svazku hello obnovena a hello dostupnou šířku pásma. Pokud byl odstraněn hello původního svazku, který je obnovena, další čas, budou provedeny toocreate hello volné místo na hello zařízení v rámci operace obnovení hello.

**Otázka:** Potřebuji toorestore mé existující místně připnutý tooan starší snímky svazků (provedeny, když byla vrstvené hello svazek). Hello svazku se obnoví jako v tomto případě vrstvené?

**Odpověď:** Ne, budou obnoveny hello svazek jako místně vázaný svazek. I když hello snímku data toohello čas, když byl vrstvené hello svazku, při obnovení existující svazky StorSimple hello typ svazku na disku hello vždy používá, protože aktuálně existuje.

**Otázka:** I my místně vázaný svazek rozšířené nedávno, ale teď potřebuji toorestore hello data tooa čas, kdy bylo menší velikost svazku hello. Změní velikost obnovení hello aktuální svazku a bude nutné tooextend hello velikost svazku hello po dokončení obnovení hello?

**Odpověď:** Ano, obnovení hello změní velikost svazku hello a budete potřebovat tooextend hello velikost svazku hello po dokončení obnovení hello.

**Otázka:** Můžete změnit typ hello svazku během obnovení?

**A.**Ne, nemůžete změnit typ svazku hello během obnovení.

* Jako typ hello uložené v hello snímku se obnoví svazky, které byly odstraněny.
* Existující svazky jsou obnoveny podle jejich aktuálního typu, bez ohledu na typ hello uložené v hello snímku (viz. toohello předchozí dva dotazy).

**Otázka:** Potřebuji toorestore Moje místně vázaný svazek, ale importovali správný bod v čas snímku. Můžete zrušit hello aktuální operace obnovení?

**Odpověď:** Ano, můžete zrušit probíhající operace obnovení. Hello stav svazku hello bude vrácena zpět stav toohello při spuštění hello hello obnovení. Všechny zápisů, které byly provedeny toohello svazku při obnovení hello probíhala však budou ztraceny.

**Otázka:** Můžu spustit operaci obnovení na jeden z mých místně vázaných svazků a zobrazují snímku v mé nevyřízených položek katalogu, která I není recollect vytváření. To k čemu slouží?

**Odpověď:** Toto je hello dočasné snímek, který je vytvořen operace obnovení předchozí toohello a používá se pro vrácení zpět v případě obnovení hello je zrušená nebo selže. Neodstraňujte tento snímek; jej bude automaticky odstraněna po dokončení obnovení hello. Toto chování může dojít, pokud vaše úlohy obnovení má pouze místně vázaný svazky nebo kombinaci místně vázaný a vrstvené svazky. Pokud úloha obnovení hello obsahuje pouze vrstvené svazky, toto chování nedojde.

**Otázka:** Může klonovat místně vázaný svazek?

**Odpověď:** Ano, můžete. Hello místně vázaný svazek bude klonovat jako vrstvený svazek, ale ve výchozím nastavení. Další informace o příliš[klonovat místně vázaný svazek](storsimple-8000-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Dotazy týkající se selhání přes místně vázaný svazek
**Otázka:** Potřebuji toofail přes Moje zařízení tooanother fyzického zařízení. Bude Moje místně vázaných svazků se přes místně vázaný nebo vrstvené?

**Odpověď:** Hello místně vázaný svazky jsou převzetí služeb při selhání jako místně vázaný Pokud hello cílové zařízení běží aktualizace řady StorSimple 8000 3 nebo vyšší.

Další informace o [převzetí služeb při selhání a zotavení po Havárii z místně vázaný svazky mezi verzemi](storsimple-8000-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**Otázka:** Místně vázaných svazků okamžitě obnoveny během zotavení po havárii (DR)?

**Odpověď:** Ano, místně vázané svazky jsou obnoveny okamžitě během převzetí služeb při selhání. Jakmile hello informace metadat pro svazek hello pocházejí z cloudu hello v rámci operace převzetí služeb při selhání hello, hello svazku na hello cílové zařízení do režimu online a je přístupný hello hostitele. Mezitím hello svazku dat budou i nadále toodownload hello pozadí a může dojít k snížený výkon v těchto svazcích pro dobu trvání hello hello převzetí služeb při selhání.

**Otázka:** Najdete v části hello převzetí služeb při selhání úloha nebyla dokončena, jak můžete sledovat průběh hello místně vázaný svazek, který je obnovena na hello cílové zařízení?

**Odpověď:** Během operace převzetí služeb při selhání, hello převzetí služeb při selhání úloha je označena jako dokončené po všech svazků hello sady hello převzetí služeb při selhání byla okamžitě obnovena a uvést do režimu online na hello cílové zařízení. To zahrnuje všechny místně vázaných svazků, které může mít byla při selhání; ale místní záruky hello dat bude dostupný pouze pokud stáhl všechny hello data pro svazek hello. Můžete sledovat tento postup pro každý místně vázaný svazek, který byl selhal nepřevezme monitorování hello odpovídající obnovení úlohy, které jsou vytvořené jako součást hello převzetí služeb při selhání. Tyto úlohy obnovení jednotlivých pouze vytvoří místně vázaných svazků.

**Otázka:** Můžete změnit typ hello svazku během převzetí služeb při selhání?

**Odpověď:** Ne, nemůže změnit typ svazku hello během převzetí služeb při selhání. Pokud se nedaří přes tooanother fyzické zařízení, které běží řady StorSimple 8000 update 3, hello svazky jsou převzetí služeb při selhání na základě typu svazku hello uložené v hello snímku.

**Otázka:** Může I převzetí služeb při selhání kontejner svazků s místně vázaných svazků toohello cloudu zařízení?

**Odpověď:** Ano, můžete. svazky Hello místně vázaný převezme služby při selhání jako vrstvené svazky. Další informace o [převzetí služeb při selhání a zotavení po Havárii z místně vázaný svazky mezi verzemi](storsimple-8000-device-failover-disaster-recovery.md#common-considerations-for-device-failover)

