---
title: "Tým Azure Data vědecké účely proces cyklu | Microsoft Docs"
description: "Kroky potřebné k provedení projekty vědecké účely data."
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
ms.openlocfilehash: 9b8ef4f1165a89fa6ed1b64b44d58bb45f08f232
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="team-data-science-process-lifecycle"></a>Životní cyklus Team proces vědecké účely dat

Proces pro vědecké účely Data Team (TDSP) poskytuje doporučené životního cyklu, který můžete použít pro struktury projekty vědecké účely data. Životní cyklus popisuje kroky, od začátku do konce, projekty obvykle postupujte při jejich spuštění. Pokud použijete jiný vědecké účely životního cyklu data, jako [OSTRÉ DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) nebo vaše organizace vlastní proces, můžete dál používat TDSP založený na úlohách s tyto vývojové životní cykly. 

Byla vytvořena tohoto životního cyklu pro projekty vědecké účely dat, které jsou určeny k dodávají jako součást inteligentní aplikace. Tyto aplikace nasadit machine learning nebo umělé intelligence modely pro prediktivní analýzy. Projekty vědecké účely nahodilého dat a ad hoc analytics projekty mohou také těžit z pomocí tohoto procesu. Ale v takovém případě nemusí být některé kroky popsané potřeba.    

Tady je vizuální reprezentace **procesu vědecké účely Team datového cyklu**. 

![TDSP cyklu](./media/data-science-process-overview/tdsp-lifecycle.png) 

TDSP životní cyklus se skládá z pěti hlavních fází, které jsou spouštěny interaktivně. Mezi ně patří:

* **Pochopení obchodních**
* **Získávání dat a principy**
* **Modelování**
* **Nasazení**
* **Přijetí zákazníka**

Pro každou fázi jsme zadejte následující informace:

* **Cíle**: specifické cíle.
* **Jak to udělat**: konkrétní úlohy uvedené a pokyny, které jsou k dispozici na jejich dokončení.
* **Artefakty**: dodávky a podporu místo nich.


## <a name="1-business-understanding"></a>1. Principy podniku

### <a name="goals"></a>Cíle
* **Klíče proměnné** jsou zadány, která bude sloužit jako, které jsou **modelu cíle** a jehož související metriky používají zjistili úspěšnost pro projekt.
* Odpovídajícího **zdroje dat** se identifikovanou že firmy má přístup k nebo je potřeba získat.

### <a name="how-to-do-it"></a>Jak to udělat
Existují dvě hlavní úlohy v této fázi řešit: 

* **Definování cílů**: práce s vašich zákazníků a dalších zúčastněných osob, pochopit a identifikovat obchodních problémů. Formulovali otázky, které definují obchodních cílů a který může cílit na datové vědě techniky.
* **Identifikaci zdroje dat**: najít relevantní data, která vám pomůže odpovědět na otázky, které definují cíle projektu.

#### <a name="11-define-objectives"></a>1.1 definování cílů

1. Ústředním cílem tohoto kroku je identifikace klíč **obchodní proměnné** vyžadující analýza předpovědět. Tyto proměnné jsou označovány jako **modelu cíle** a metriky s nimi spojených se používají k určení úspěšnosti projektu. Dva příklady takových cíle jsou prognózy prodeje nebo pravděpodobnost pořadí se podvodné.

2. Definování **projektu cíle** žádostí a upřesnění "ostré" otázky, které jsou relevantní a konkrétní a jednoznačné. Vědecké zpracování dat je proces, pomocí názvů a čísel k odpovědi na tyto otázky. Další informace o klást otázky sharp najdete v tématu [postup vědecké zpracování dat](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blogu. Vědecké zpracování dat / machine learning se obvykle používá k odpovědi na pět typů otázek:
 
   * Kolik nebo kolik? (regrese)
   * Které kategorie? (klasifikace)
   * Které skupiny? (clustery)
   * Je to divné? (detekce anomálií)
   * Možnosti, kterou by měla být provedena? (doporučení)

    Určete, který tyto dotazy jsou požádat a jak volaného dosahuje svých cílů firmy.

3. Definování **projektu team** zadáním rolích a zodpovědnostech svých členů. Vytvořte plán vysoké úrovně milník, který iterovat jako je zjistit další informace.  

4. **Definování metriky úspěchu**. Příklad: zákazníka změn předpovědi přesnost X % dosáhnout konce tohoto projektu 3 měsíce, aby nabízíme povýšení ke snížení změn. Musí být metriky **INTELIGENTNÍ**: 
   * **S**určitá 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**vázané na editoru ime 

#### <a name="12-identify-data-sources"></a>1.2 identifikaci zdroje dat
Identifikujte zdroje dat, které obsahují známé příklady odpovědi na otázky sharp. Vyhledejte následující data:

* Data, která je **Relevant** na otázku. Máme míry cíl a funkce, které se vztahují k cíli?
* Data, která je **přesně** naše cílových modelu a funkce, které vás zajímají.

Například není neobvyklé, zjistíte, je potřeba stávajících systémů shromažďovat a protokolovat další typy dat potíže vyřešit a dosáhnout cíle projektu. V takovém případě můžete hledat externích zdrojů dat. nebo aktualizovat vaše systémy shromažďovat nová data.

### <a name="artifacts"></a>Artefakty
Zde jsou dodávky v této fázi:

* [**Charterovou dokumentu**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): standardní šablona je součástí definice struktury TDSP projektu. Toto je životností dokumentu, který se aktualizuje v celém projektu jako nové zjišťování jsou vytvářeny a jako obchodní požadavky se změní. Klíč je k iteraci na tento dokument, přidání další podrobnosti, jako průběh procesu zjišťování. Ponechte zákazníka a ostatní účastníci součástí provádění požadovaných změn a zřetelněji sdělují důvody pro změny k nim.  
* [**Zdroje dat**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): Toto je **nezpracovaná zdroje dat** části **definice dat** sestavu, která se nachází v projektu TDSP **sestavu dat** složky. Určuje původní a cílové umístění nezpracovaná data. Ve fázích novější je zadat další podrobnosti, třeba skripty pro přesun dat do prostředí analýzy.  
* [**Slovník dat**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): Tento dokument obsahuje popis dat, která je zadaných klientem. Tyto popisy obsahují informace o schématu (datové typy, informace o ověřovacích pravidel, pokud existuje) a entity-relation diagramy, pokud je k dispozici.


## <a name="2-data-acquisition-and-understanding"></a>2. Získávání a pochopení dat

### <a name="goals"></a>Cíle
* Čistá, vysoce kvalitní sady dat se rozumí jehož vztahy proměnným cíl se nacházejí v prostředí příslušné analýzy připraven k modelu.
* Byl vyvinut architektury řešení datového kanálu aktualizujte a pravidelně stanovení skóre data.

### <a name="how-to-do-it"></a>Jak to udělat
Existují tři hlavní úlohy v této fázi řešit:

* **Zpracování příjmu dat** do cílové analytické prostředí.
* **Data prozkoumat** k určení, pokud kvalitu dat je dostačující odpověď na otázku. 
* **Nastavit datovém kanálu** skóre pro nová nebo pravidelně aktualizovat data.

#### <a name="21-ingest-the-data"></a>2.1 zpracování příjmu dat
Nastavte proces přesouvat data ze zdrojového umístění do cílového umístění, kde operací analýz jako školení a předpovědi se mají provést. Technické podrobnosti a možnosti o tom, jak to provést pomocí různých služeb Azure dat najdete v tématu [načtení dat do úložiště prostředí pro analýzu](machine-learning-data-science-ingest-data.md). 

#### <a name="22-explore-the-data"></a>2.2 data prozkoumat
Předtím, než je cvičení modely, budete muset vyvíjet zvukové povědomí o data. Datové sady reálného jsou často aktivní nebo chybí hodnoty nebo hostitel jiných nesrovnalostí. Shrnutí dat a vizualizaci slouží k auditu kvality vaše data a poskytovat informace potřebné ke zpracování dat, než je připraven pro modelování. Tento proces je často iterativní.

TDSP poskytuje automatizované nástroj [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) k vizualizaci dat a připravit data souhrnné sestavy. Doporučujeme začít s IDEAR nejprve k prozkoumání dat k vývoji počáteční data porozumění interaktivně s žádné kódování a pak psát vlastní kód pro zkoumání dat a vizualizaci. Pokyny k čištění data, najdete v části [úkoly přípravy dat pro vylepšený strojového učení](machine-learning-data-science-prepare-data.md).  

Jakmile budete spokojeni s kvality vyčištěny data, dalším krokem je lépe pochopit vzorů, které jsou obsaženy v datech, které vám pomůžou vybrat a vytvořte odpovídající prediktivního modelu k cílovému. Podívejte se pro důkaz pro jak dobře připojené dat je k cíli, a zda je dostatek dat přejdete na následující dalších kroků modelování. Tento proces je znovu, často iterativní. Budete muset najít nové zdroje dat se přesnější nebo více odpovídajících dat k posílení původně stanovených ve fázi předchozí datovou sadu.  

#### <a name="23-set-up-a-data-pipeline"></a>2.3 nastavit datovém kanálu
Kromě počáteční přijímání a čištění dat obvykle musíte nastavit proces skóre pro nová data nebo pravidelně aktualizuje data v rámci procesu probíhající učení. Tento krok můžete provést nastavením na datovém kanálu nebo pracovního postupu. Tady je [příklad](machine-learning-data-science-move-sql-azure-adf.md) o tom, jak nastavit kanál pomocí [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

Architektura řešení datového kanálu je vyvinuta v této fázi. Kanál také vyvinutý paralelně s těchto fází projektu vědecké účely data. Kanál může být na základě batch nebo streamování nebo real-bránu nebo hybridní v závislosti na obchodních potřeb a omezení do stávajících systémů, do které je integrované řešení. 

### <a name="artifacts"></a>Artefakty
Níže jsou dodávky v této fázi.

* [**Sestava kvality dat**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Tato sestava obsahuje souhrnné informace o data, vztahy mezi každý atribut a cíl, proměnné hodnocení atd. [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) nástroj, který poskytuje jako součást TDSP můžete rychle vygenerovat tuto sestavu na žádné tabulkové datové sady jako soubor CSV nebo relační tabulku. 
* **Architektura řešení**: to může být diagram nebo popis kanál dat použitý ke spuštění vyhodnocování nebo předpovědi na nová data, po který jste vytvořili model. Obsahuje taky kanálu, který má být přeučování modelu na základě na nová data. Dokument je uložen v [projektu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) directory při použití šablony TDSP directory struktura.
* **Kontrolní bod rozhodnutí**: než začnete úplné funkce inženýrství a vytváření modelů, prezentovaný projekt, který má-li určit, zda je očekávána hodnota dostatečná pokračujte usilovat o normalizaci ho. Může být například připraveni pokračovat, třeba shromažďovat další data, nebo zrušte projektu jako data neexistuje odpověď na otázku.


## <a name="3-modeling"></a>3. Modelování

### <a name="goals"></a>Cíle
* Funkce optimální dat pro model machine learning.
* Informativní modelu ML, který nejpřesněji předpovídá cíl.
* Modelu ML, který je vhodný pro produkční prostředí.

### <a name="how-to-do-it"></a>Jak to udělat
Existují tři hlavní úlohy v této fázi řešit:

* **Konstruování**: vytvoření funkce data z původní data, která usnadňují modelu školení.
* **Model školení**: najít model, který odpovídá na otázku nejvíce přesně tak, že porovnáte jejich metriky úspěchu.
* Určí, zda váš model **vhodná pro produkční**.

#### <a name="31-feature-engineering"></a>3.1 funkce inženýrství
Funkce inženýrství zahrnuje zahrnutí, agregace a transformaci nezpracovaná proměnných pro vytvoření funkce použité v analýzy. Pokud chcete, aby aspekty faktory určující model, musíte pochopit, jak funkce jsou vzájemně souvisí a jak jsou algoritmy strojového učení k používání těchto funkcí. Tento krok vyžaduje tvůrčí kombinaci domény znalosti a statistiky získat z kroku zkoumání dat. Toto je vyrovnávání operace hledání a včetně informativní proměnné a snižuje příliš mnoho nesouvisejícími proměnné. Informativní proměnné zlepšení našeho výsledek; nesouvisejícími proměnné zavést jako zbytečný rušivý element do modelu. Také budete muset vygenerovat tyto funkce pro žádná nová data získané při vyhodnocování. Proto generování tyto funkce lze pouze závisí na data, která je k dispozici v době vyhodnocování. Technické pokyny v inženýrství funkce pro používání různých technologií dat Azure najdete v tématu [konstruování v procesu vědecké účely dat](machine-learning-data-science-create-features.md). 

#### <a name="32-model-training"></a>3.2 modelu školení
V závislosti na typu otázku, kterou zkoušíte odpovědí existuje mnoho modelování algoritmy. Pokyny k výběru algoritmy, najdete v části [jak zvolit algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md). I když v tomto článku je napsán pro Azure Machine Learning, je užitečné pro všechny strojového učení projekty pokyny, které poskytuje. 

Proces pro trénování modelu zahrnuje následující kroky: 

* **Rozdělení vstupních dat** náhodně pro modelování do trénovací datové sady a testovací datové sady.
* **Vytvářet modely** pomocí trénovací datové sady.
* **Vyhodnocení** optimalizace parametrů (označované jako parametr oblouku), které se mají za cíl zajistit odpovědi na otázky týkající se data aktuálního přidružené řady konkurenční algoritmy strojového učení spolu různé (učení a testovací datové sady).
* **Určit "doporučené" řešení** odpověď na otázku tak, že porovnáte metriky úspěchu mezi alternativní metody.

> [!NOTE]
> **Zabránilo úniku**: úniku dat může být způsobeno zahrnutí dat z mimo datovou sadu školení, která umožňuje, aby modelu nebo algoritmu strojového učení aby unrealistically dobrý předpovědi. Únikům je běžným důvodem, proč data, která vědců získat nervové, když získají prediktivní výsledky, které se zdají příliš výhodné mít hodnotu true. Tyto závislosti může být obtížné zjistit. Aby se zabránilo úniku často vyžaduje iterace mezi vytváření sadu analýzy dat, vytvoření modelu a vyhodnocení přesnost. 
> 
> 

Poskytujeme [automatizované modelování a vytváření sestav nástroje](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) s TDSP, který se bude moct spustit prostřednictvím více algoritmů a parametr změny k vytvoření základního modelu. Vytváří také směrný plán modelování sestavy shrnutí výkonu kombinace každý model a parametr včetně proměnné význam. Tento proces je iterativní také jako ho můžete drive inženýrství další funkce. 

### <a name="artifacts"></a>Artefakty
Artefakty vytvořeného v této fázi patří:

* [**Funkce sady**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): funkce vytvořených pro modelování jsou popsány v **sady funkcí** části **definice dat** sestavy. Obsahuje odkazy na kód pro generování funkce a popis na tom, jak byl vygenerován funkci.
* [**Model sestavy**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): pro každý model, který se pokus o, standard, se vytváří na základě šablon sestav, která poskytuje podrobnosti o každém pokusu.
* **Kontrolní bod rozhodnutí**: vyhodnocení, zda ji nasadit do produkčního systému provádí dostatečně dobře modelu. Jsou některé klíčové otázky:
  * Model odpovědět na otázku s dostatečnou důvěru zadané testovací data? 
  * Vyzkoušejte všechny přístupy: shromažďovat další data, proveďte další funkce inženýrství nebo experimentovat s jiné algoritmy?


## <a name="4-deployment"></a>4. Nasazení

### <a name="goal"></a>Cílem
* Modely datovém kanálu se nasadí do produkce nebo prostředí produkčního prostředí pro přijetí konečného uživatele. 

### <a name="how-to-do-it"></a>Jak to udělat
Hlavní úloha řešeny v této fázi:

* **Zprovoznit model**: nasadit model a kanál pro produkční nebo prostředí produkčního prostředí pro používání aplikace.

#### <a name="41-operationalize-a-model"></a>4.1 zprovoznit model
Jakmile máte sadu modely, které provádějí dobře, může být operationalized u ostatních aplikací používat. V závislosti na obchodní požadavky jsou vytvářeny předpovědi buď v reálném čase, nebo na základě dávky. Chcete-li být operationalized, mít modely mají být exponovány s otevřené rozhraní API, které je snadno použít z různých aplikací, jako je například online webů, tabulek, řídicí panely a obchodní a back-end aplikace. Příklady operationalization modelu pomocí webové služby Azure Machine Learning najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md). Je také doporučujeme vytvářet telemetrie a sledování do provozního modelu a datovém kanálu nasadit usnadní stav následné systému hlášení a řešení potíží.  

### <a name="artifacts"></a>Artefakty
* Řídicí panel stavu systému stavu a klíč metrik.
* Poslední modelování sestavu s podrobnostmi o nasazení.
* Architektura dokumentu konečné řešení.

## <a name="5-customer-acceptance"></a>5. Přijetí zákazníky

### <a name="goal"></a>Cílem
* **Dokončení dodávky projektu**: Ověřte, jestli kanál, model a jejich nasazení v produkčním prostředí jsou vyhovuje požadavkům cíle zákazníků.

### <a name="how-to-do-it"></a>Jak to udělat
Existují dvě hlavní úlohy v této fázi řešit:

* **Ověřování systému**: potvrďte nasazené modelu a kanálu, byly splněny požadavky zákazníka.
* **Projekt ruční vypnout**: na typ entity, která je k provozování systému v produkčním prostředí.

Zákazník musí ověřit, že systém splňuje jejich potřeb vaší organizace a odpovědi na otázky s přijatelnou přesnost do produkčního prostředí pro nasazení systému použít ve své klientské aplikace. Veškerá dokumentace je dokončen a zkontrolovat. Ruční – vypnuté projektu v entitě zodpovědná za operace byla dokončena. Tato entita může být například IT nebo tým vědecké účely dat zákazníka nebo agenta zákazníka, která je zodpovědná za spuštění systému v produkčním prostředí. 

### <a name="artifacts"></a>Artefakty
Hlavní artefaktů vytvořeného v této konečné fázi je **ukončení sestavy projektu pro zákazníka**. Toto je technické sestavu obsahující všechny podrobnosti o to užitečné fungovat v systému a další informace o projektu. [Ukončení sestavy](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) TDSP, která se dá použít jako je nebo přizpůsobené pro konkrétní klient potřebuje poskytuje šablony. 

## <a name="summary"></a>Souhrn
[Procesu vědecké účely Team datového cyklu](http://aka.ms/datascienceprocess) je modelovaná jako pořadí vstupní kroků, které poskytují pokyny na úlohách, které jsou potřeba k použití prediktivní modely. Tyto modely se dá nasadit v provozním prostředí využít k vytváření inteligentního aplikací. Cílem tohoto životního cyklu proces je nadále přesunout data vědecké účely projektu předat do zrušte zapojení koncového bodu. I když je hodnota true, vědecké zpracování dat je cvičení pro výzkum a zjišťování, schopnost zřetelněji sdělují tyto úlohy váš tým a zákazníky pomocí dobře definované sadě artefaktů, které zaměstnanci standardizovaných šablon mohou pomoci vyhnout neporozumění a zvýšit riziko úspěšné dokončení rozšířené datové vědy projektu.

## <a name="next-steps"></a>Další kroky
Úplné návody začátku do konce, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v [proces vědecké účely dat Team návody](data-science-process-walkthroughs.md) tématu.

