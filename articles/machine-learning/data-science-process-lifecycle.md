---
title: "aaaAzure Team datové vědy procesu životního cyklu | Microsoft Docs"
description: "Kroky potřebné tooexecute projekty vědecké účely data."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev;
ms.openlocfilehash: 58114a1c2d3289d1c4b2781219d0bf9647dbccd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="team-data-science-process-lifecycle"></a>Životní cyklus Team proces vědecké účely dat

Hello tým datové vědy procesu (TDSP) poskytuje doporučené životního cyklu, které můžete použít toostructure projekty vědecké účely data. životní cyklus Hello popisuje hello kroky, z toofinish spuštění, který projekty obvykle postupujte při jejich spuštění. Pokud použijete jiný vědecké účely životního cyklu data, jako [OSTRÉ DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) nebo vaše organizace vlastní proces, můžete dál používat hello založený na úlohách TDSP s tyto vývojové životní cykly. 

Pro datové vědy projekty, které jsou určený tooship jako součást inteligentní aplikace byl navržen tak tohoto životního cyklu. Tyto aplikace nasadit machine learning nebo umělé intelligence modely pro prediktivní analýzy. Projekty vědecké účely nahodilého dat a ad hoc analytics projekty mohou také těžit z pomocí tohoto procesu. Ale v takovém případě nemusí být některé kroky popsané potřeba.    

Tady je vizuální reprezentace hello **procesu vědecké účely Team datového cyklu**. 

![TDSP cyklu](./media/data-science-process-overview/tdsp-lifecycle.png) 

Hello TDSP životní cyklus se skládá z pěti hlavních fází, které jsou spouštěny interaktivně. Mezi ně patří:

* **Pochopení obchodních**
* **Získávání dat a principy**
* **Modelování**
* **Nasazení**
* **Přijetí zákazníka**

Pro každou fázi poskytujeme hello následující informace:

* **Cíle**: hello specifické cíle.
* **Jak toodo ho**: hello konkrétní úlohy uvedené a pokyny, které jsou k dispozici na jejich dokončení.
* **Artefakty**: hello výsledek a hello podpora místo nich.


## <a name="1-business-understanding"></a>1. Principy podniku

### <a name="goals"></a>Cíle
* Hello **klíče proměnné** jsou zadány, které jsou tooserve jako hello **modelu cíle** a jehož související metriky používají určit úspěšný hello hello projektu.
* Hello relevantní **zdroje dat** se označují, že hello firmy má tooobtain potřebám tooor přístup.

### <a name="how-toodo-it"></a>Jak toodo ho
Existují dvě hlavní úlohy v této fázi řešit: 

* **Definování cílů**: práce s vašich zákazníků a dalších zúčastněných toounderstand a identifikovat hello obchodních problémů. Formulovali otázky, které definují hello obchodních cílů a že data vědecké účely techniky, můžete vybrat.
* **Identifikaci zdroje dat**: hello najít relevantní se data, která vám pomůže odpovědět hello otázky, které definují hello cíli projektu hello.

#### <a name="11-define-objectives"></a>1.1 definování cílů

1. Ústředním cílem tohoto kroku je klíč hello tooidentify **obchodní proměnné** že hello analysis potřebuje toopredict. Tyto proměnné, které jsou odkazované tooas hello **modelu cíle** a metriky hello s nimi spojených se použité toodetermine hello úspěch hello projektu. Jsou dva příklady těchto cílů prodeje prognózy nebo hello pravděpodobnost pořadí se podvodné.

2. Definování hello **projektu cíle** žádostí a upřesnění "ostré" otázky, které jsou relevantní a konkrétní a jednoznačné. Vědecké zpracování dat je proces hello pomocí názvů a čísla tooanswer takové dotazy. Další informace o klást otázky sharp najdete v tématu [jak toodo vědecké zpracování dat](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blogu. Vědecké zpracování dat / machine learning je obvykle používanými tooanswer pět typů otázek:
 
   * Kolik nebo kolik? (regrese)
   * Které kategorie? (klasifikace)
   * Které skupiny? (clustery)
   * Je to divné? (detekce anomálií)
   * Možnosti, kterou by měla být provedena? (doporučení)

    Určete, který tyto dotazy jsou požádat a jak volaného dosahuje svých cílů firmy.

3. Definování hello **projektu team** zadáním hello rolích a zodpovědnostech svých členů. Vytvořte plán vysoké úrovně milník, který iterovat jako je zjistit další informace.  

4. **Definování metriky úspěchu**. Příklad: zákazníka změn předpovědi přesnost X % dosáhnout konce hello tohoto projektu 3 měsíce, aby nabízíme povýšení tooreduce změn. musí být Hello metriky **INTELIGENTNÍ**: 
   * **S**určitá 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**vázané na editoru ime 

#### <a name="12-identify-data-sources"></a>1.2 identifikaci zdroje dat
Identifikujte zdroje dat, které obsahují známé příklady odpovědi tooyour sharp otázek. Podívejte se na hello následující data:

* Data, která je **Relevant** toohello otázku. Máme míry hello cíl a funkce, které jsou cílové související toohello?
* Data, která je **přesně** naše modelu cíle a hello funkcí zájmu.

Není, například toofind, že existující systémy potřebovat toocollect a protokolu další typy dat tooaddress hello problém a dosáhnout cíle projektu hello. V takovém případě může má toolook pro externí zdroje dat nebo aktualizovat vaše systémy toocollect nová data.

### <a name="artifacts"></a>Artefakty
Zde jsou hello výsledek v této fázi:

* [**Charterovou dokumentu**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): standardní šablona je součástí hello Definice struktury TDSP projektu. Toto je životností dokument, který je aktualizováno v celé hello projektu, jako jsou vytvářeny nové zjišťování a jako obchodní požadavky se změní. klíč Hello je tooiterate na tento dokument, přidání další podrobnosti, jako probíhá proces zjišťování hello. Udržování hello zákazníka a dalších zúčastněných osob součástí změnami hello a zřetelněji sdělují hello důvody toothem změny hello.  
* [**Zdroje dat**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): Toto je hello **nezpracovaná zdroje dat** části hello **definice dat** sestavu, která se nachází v projektu TDSP hello **sestavu dat**  složky. Určuje hello původní a cílové umístění pro hello nezpracovaná data. Ve fázích novější je zadat další podrobnosti, třeba skripty toomove hello data tooyour analytické prostředí.  
* [**Slovník dat**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): Tento dokument obsahuje popis hello dat, která zajišťuje hello klienta. Tyto popisy obsahují informace o schématu hello (datové typy, informace o ověřovacích pravidel, pokud existuje) a hello entity Vztah diagramy, pokud je k dispozici.


## <a name="2-data-acquisition-and-understanding"></a>2. Získávání a pochopení dat

### <a name="goals"></a>Cíle
* Čistá, vysoce kvalitní sady dat se rozumí jejichž vztahů toohello cílových proměnných jsou umístěny v hello odpovídající analytics prostředí připravené toomodel.
* Architektura řešení hello datového kanálu toorefresh a skóre dat pravidelně vyvinula.

### <a name="how-toodo-it"></a>Jak toodo ho
Existují tři hlavní úlohy v této fázi řešit:

* **Načítání dat hello** do hello cílové analytické prostředí.
* **Prozkoumejte hello data** toodetermine Pokud kvality dat. hello odpovídající tooanswer hello otázku. 
* **Nastavit datovém kanálu** tooscore nové nebo pravidelně aktualizovat data.

#### <a name="21-ingest-hello-data"></a>2.1 ingestovat hello data
Nastavit hello procesu toomove jsou hello data ze zdrojového umístění toohello cílových míst, kde operací analýz jako školení a předpovědi toobe provést. Technické podrobnosti a možností, jak toodo to s různými službami dat Azure, najdete v části [načtení dat do úložiště prostředí pro analýzu](machine-learning-data-science-ingest-data.md). 

#### <a name="22-explore-hello-data"></a>2.2 zkoumat hello data
Než budete cvičení modely, je třeba toodevelop zvukové pochopení dat hello. Datové sady reálného jsou často aktivní nebo chybí hodnoty nebo hostitel jiných nesrovnalostí. Shrnutí dat a vizualizaci můžete být použité tooaudit hello kvality dat. a poskytují informace hello potřeby tooprocess hello data předtím, než je připraven k modelování. Tento proces je často iterativní.

TDSP poskytuje automatizované nástroj [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) toohelp vizualizovat hello data a připravit data souhrnné sestavy. Doporučujeme začít s IDEAR první tooexplore hello data toohelp vyvíjet počáteční data porozumění interaktivně s žádné kódování a pak psát vlastní kód pro zkoumání dat a vizualizaci. Pokyny k vyčištění dat hello, najdete v části [tooprepare dat pro rozšířené machine learning úkolů](machine-learning-data-science-prepare-data.md).  

Jakmile budete spokojeni s kvalitou hello hello vyčištěny dat, je dalším krokem hello toobetter pochopit hello vzorů, které jsou obsaženy v hello data, která vám pomůžou vybrat a vytvořte odpovídající prediktivního modelu k cílovému. Vyhledejte důkaz, jak dobře připojené hello dat je cílem toohello a zda je dostatek dat toomove dál s další kroky modelování hello. Tento proces je znovu, často iterativní. Toofind nové zdroje dat může být nutné přesnější nebo více relevantní data tooaugment hello datové sadě původně stanovených ve fázi předchozí hello.  

#### <a name="23-set-up-a-data-pipeline"></a>2.3 nastavit datovém kanálu
Kromě toho toohello počáteční přijímání a čištění hello dat, obvykle musíte tooset proces tooscore nová data nebo data hello aktualizace pravidelně jako součást procesu probíhající učení. Tento krok můžete provést nastavením na datovém kanálu nebo pracovního postupu. Tady je [příklad](machine-learning-data-science-move-sql-azure-adf.md) jak tooset až kanál pomocí [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

Architektura řešení hello datového kanálu je vyvinuta v této fázi. kanál Hello také vyvinutý paralelně s hello následujících fázích hello datové vědy projektu. Hello kanálu může být na základě batch nebo streamování nebo real-bránu nebo hybridní v závislosti na vaší firmě musí a hello omezení do stávajících systémů, do které je integrované řešení. 

### <a name="artifacts"></a>Artefakty
v této fázi jsou následující Hello hello výsledek.

* [**Sestava kvality dat**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Tato sestava obsahuje souhrnné informace o data, vztahy mezi každý atribut a cíl, proměnné, řazení atd hello [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) nástroj, který poskytuje jako součást TDSP můžete rychle vytvořit Tato sestava pro všechny tabulkové datovou sadu jako soubor CSV nebo relační tabulku. 
* **Architektura řešení**: to může být diagram nebo popis datový kanál používat toorun vyhodnocování nebo předpovědi na nová data, po který jste vytvořili model. Obsahuje taky tooretrain kanálu hello modelu založené na nová data. dokument Hello je uložen v hello [projektu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) directory při použití hello TDSP directory struktura šablony.
* **Kontrolní bod rozhodnutí**: než začnete úplné funkce inženýrství a vytváření modelů, prezentovaný hello projektu toodetermine informaci, jestli byla očekávána hodnota hello dostatečná toocontinue usilovat o normalizaci ho. Může například být připravené tooproceed toocollect potřeba víc dat nebo zrušte hello projektu jako hello data neexistuje tooanswer hello otázku.


## <a name="3-modeling"></a>3. Modelování

### <a name="goals"></a>Cíle
* Funkce optimální dat pro model strojového učení hello.
* Informativní modelu ML, který nejpřesněji předpovídá hello cíl.
* Modelu ML, který je vhodný pro produkční prostředí.

### <a name="how-toodo-it"></a>Jak toodo ho
Existují tři hlavní úlohy v této fázi řešit:

* **Konstruování**: vytvoření funkce dat z hello nezpracovaná data toofacilitate modelu školení.
* **Model školení**: najít hello model tuto otázku hello odpovědi nejvíce přesně tak, že porovnáte jejich metriky úspěchu.
* Určí, zda váš model **vhodná pro produkční**.

#### <a name="31-feature-engineering"></a>3.1 funkce inženýrství
Funkce inženýrství zahrnuje zahrnutí, agregace a transformace nezpracovaná proměnné toocreate hello funkce použité v hello analýzy. Pokud chcete aspekty faktory určující model, budete potřebovat toounderstand jak jsou související tooeach další funkce a algoritmy strojového učení hello jsou toouse tyto funkce. Tento krok vyžaduje tvůrčí kombinaci domény znalosti a statistiky získat z kroku zkoumání dat hello. Toto je vyrovnávání operace hledání a včetně informativní proměnné a snižuje příliš mnoho nesouvisejícími proměnné. Informativní proměnné zlepšení našeho výsledek; nesouvisejícími proměnné zavést jako zbytečný rušivý element do modelu hello. Budete také potřebovat toogenerate tyto funkce pro žádná nová data získané při vyhodnocování. Proto hello generování tyto funkce lze pouze závisí na data, která je k dispozici v době hello vyhodnocování. Technické pokyny v inženýrství funkce pro používání různých technologií dat Azure najdete v tématu [konstruování v hello proces vědecké účely dat](machine-learning-data-science-create-features.md). 

#### <a name="32-model-training"></a>3.2 modelu školení
V závislosti na typu otázku, kterou zkoušíte odpovědí existuje mnoho modelování algoritmy. Pokyny k výběru hello algoritmy, najdete v části [jak toochoose algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md). I když v tomto článku je napsán pro Azure Machine Learning, hello pokyny, které poskytuje je užitečná pro všechny strojového učení projekty. 

proces Hello k trénování modelu zahrnuje hello následující kroky: 

* **Rozdělení hello vstupní data** náhodně pro modelování do trénovací datové sady a testovací datové sady.
* **Vytvářet modely hello** pomocí hello trénovací datové sady.
* **Vyhodnocení** (učení a testovací datové sady) řadu konkurenční algoritmů strojového učení společně s hello různé přidružené vyladění parametry (označované jako parametr oblouku), které se mají za cíl zajistit odpovědi na otázky hello zájmu s hello aktuální data.
* **Určit "doporučené" řešení hello** tooanswer hello dotaz tak, že porovnáte metrika úspěšnosti hello mezi alternativní metody.

> [!NOTE]
> **Zabránilo úniku**: úniku dat může být způsobeno hello zahrnutí dat z mimo datovou sadu hello školení, která umožňuje, aby modelu nebo algoritmu strojového učení toomake unrealistically dobrý předpovědi. Únikům je běžným důvodem, proč získat datových vědců nervové při získají prediktivní výsledky, které pravděpodobně příliš dobrý toobe hodnotu true. Tyto závislosti může být pevný toodetect. tooavoid úniku často vyžaduje iterace mezi vytváření sadu analýzy dat, vytvoření modelu a vyhodnocení přesnost hello. 
> 
> 

Poskytujeme [automatizované modelování a vytváření sestav nástroje](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) s TDSP, který je možné toorun prostřednictvím více algoritmů, a parametr přesune tooproduce model směrného plánu. Vytváří také směrný plán modelování sestavy shrnutí výkonu kombinace každý model a parametr včetně proměnné význam. Tento proces je iterativní také jako ho můžete drive inženýrství další funkce. 

### <a name="artifacts"></a>Artefakty
artefakty Hello vytvořeného v této fázi patří:

* [**Funkce sady**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): funkce hello vytvořených pro modelování hello jsou popsány v hello **sady funkcí** části hello **definice dat** sestavy. Obsahuje ukazatele toohello kód toogenerate hello funkce a popis na tom, jak byl vygenerován hello funkce.
* [**Model sestavy**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): pro každý model, který se pokus o, standard, se vytváří na základě šablon sestav, která poskytuje podrobnosti o každém pokusu.
* **Kontrolní bod rozhodnutí**: vyhodnocení, zda hello model provádí dobře dostatek toodeploy ho tooa produkční systému. Jsou některé klíčové otázky tooask:
  * Zadaný hello modelu odpovědí hello otázku s dostatečnou důvěru hello testovací data? 
  * Vyzkoušejte všechny přístupy: shromažďovat další data, proveďte další funkce inženýrství nebo experimentovat s jiné algoritmy?


## <a name="4-deployment"></a>4. Nasazení

### <a name="goal"></a>Cílem
* Modely se zřetězením příkazů data jsou nasazené tooa produkční nebo prostředí produkčního prostředí pro přijetí konečného uživatele. 

### <a name="how-toodo-it"></a>Jak toodo ho
Úloha hlavní Hello řešeny v této fázi:

* **Zprovoznit hello model**: nasadit tooa provozního modelu a kanálu hello nebo prostředí produkčního prostředí pro používání aplikace.

#### <a name="41-operationalize-a-model"></a>4.1 zprovoznit model
Jakmile máte sadu modely, které provádějí dobře, může být operationalized pro tooconsume jiné aplikace. V závislosti na požadavcích hello firmy jsou vytvářeny předpovědi buď v reálném čase, nebo na základě dávky. toobe operationalized, hello modely mají toobe zveřejněné s otevřené rozhraní API, které je snadno použít z různých aplikací, jako je například online webů, tabulek, řídicí panely a obchodní a back-end aplikace. Příklady operationalization modelu pomocí webové služby Azure Machine Learning najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md). Je také nejlepší telemetrie toobuild postupů a monitorování do hello provozního modelu a hello datového kanálu nasazené toohelp stavem následné systému hlášení a řešení potíží.  

### <a name="artifacts"></a>Artefakty
* Řídicí panel stavu systému stavu a klíč metrik.
* Poslední modelování sestavu s podrobnostmi o nasazení.
* Architektura dokumentu konečné řešení.

## <a name="5-customer-acceptance"></a>5. Přijetí zákazníky

### <a name="goal"></a>Cílem
* **Dokončení dodávky projektu hello**: Ověřte, jestli kanál hello hello modelu a jejich nasazení v produkčním prostředí jsou vyhovuje požadavkům cíle zákazníků.

### <a name="how-toodo-it"></a>Jak toodo ho
Existují dvě hlavní úlohy v této fázi řešit:

* **Ověřování systému**: potvrďte hello nasadit model a kanál byly splněny požadavky zákazníka.
* **Projekt ruční vypnout**: toohello entita, která je toorun hello systému v produkčním prostředí.

Hello zákazníka by měl ověřit, že hello systém splňuje jejich obchodních potřeb a odpovědi hello hello dotazy s přijatelnou přesnost toodeploy hello systému tooproduction pro použití ve své klientské aplikace. Veškerá dokumentace hello je dokončen a zkontrolovat. Ruční – vypnuté hello projektu toohello entity zodpovědná za operace byla dokončena. Tato entita může být například IT nebo tým vědecké účely dat zákazníka nebo agenta hello zákazníka, které je zodpovědná za spuštění hello systému v produkčním prostředí. 

### <a name="artifacts"></a>Artefakty
hlavní artefaktů Hello vytvořeného v této fázi konečné je hello **ukončení sestavy projektu pro zákazníka**. Toto je hello technické sestavy obsahující tuto užitečné toolearn o všech podrobností o hello projektu a používá systém hello. [Ukončení sestavy](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) TDSP, která se dá použít jako je nebo přizpůsobené pro konkrétní klient potřebuje poskytuje šablony. 

## <a name="summary"></a>Souhrn
Hello [procesu vědecké účely Team datového cyklu](http://aka.ms/datascienceprocess) je modelována podle potřeby prediktivní modely toouse pořadí vstupní kroků, které poskytují pokyny v hello úlohy. Tyto modely se dá nasadit v produkční prostředí toobe využít toobuild inteligentní aplikace. cílem Hello tohoto životního cyklu proces je toocontinue toomove data vědecké účely projektu předat do zrušte zapojení koncového bodu. I když je hodnota true, vědecké zpracování dat je úkol Research a zjišťování, je možné tooclearly komunikovat tým tooyour tyto úlohy a zákazníky pomocí dobře definované sadě artefaktů, které zaměstnanci standardizovaných šablon mohou pomoci vyhněte neporozumění a zvýšit pravděpodobnost hello úspěšné dokončení rozšířené datové vědy projektu.

## <a name="next-steps"></a>Další kroky
Úplné návody začátku do konce, které ukazují všechny kroky hello v hello proces **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v hello [proces vědecké účely dat Team návody](data-science-process-walkthroughs.md) tématu.

