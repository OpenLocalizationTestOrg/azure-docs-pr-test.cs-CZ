---
title: "aaaUse prohlížeč úlohy a zobrazení úloh pro úlohy Azure Data Lake Analytics | Microsoft Docs"
description: "Zjistěte, jak toouse úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bdf27b4d-6f58-4093-ab83-4fa3a99b5650
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: jgao
ms.openlocfilehash: c45e618426808349ca380b1bcfaefd4c947ce7ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-job-browser-and-job-view-for-azure-data-lake-analytics-jobs"></a>Pomocí prohlížeče úlohy a zobrazení úloh pro úlohy Azure Data lake Analytics
Hello archivy služby Azure Data Lake Analytics odeslání úloh v [úložiště dotazů](#query-store). V tomto článku se dozvíte, jak toouse úlohy prohlížeče a zobrazení úloh v nástroje Azure Data Lake pro Visual Studio toofind hello historie úlohy informace. 

Ve výchozím nastavení hello služby Data Lake Analytics archivy hello úlohy po dobu 30 dnů. dobu platnosti Hello lze konfigurovat v hello portálu Azure tak, že nakonfigurujete zásady vypršení platnosti hello přizpůsobit. Nebudete moct tooaccess informace o úlohách hello po vypršení platnosti. 

## <a name="prerequisites"></a>Požadavky
V tématu [nástroje Data Lake pro Visual Studio požadavky](data-lake-analytics-data-lake-tools-get-started.md#prerequisites).

## <a name="open-hello-job-browser"></a>Otevřete hello úlohy prohlížeče
Přístup hello úlohy prohlížeče prostřednictvím **Průzkumníka serveru > Azure > Data Lake Analytics > úlohy** v sadě Visual Studio.  Pomocí hello úlohy prohlížeče, získat přístup k úložišti dotazů hello účtu Data Lake Analytics. Úlohy prohlížeče zobrazí úložiště dotazů na hello vlevo, informací o základní úloh a zobrazení úloh na pravé zobrazující hello podrobné informace o úlohách.

## <a name="job-view"></a>Zobrazení úloh
Úlohy zobrazení ukazuje hello podrobnosti úlohy. tooopen úlohu, můžete dvakrát klikněte na úlohu v hello úlohy prohlížeče nebo otevřít z nabídky Data Lake hello klepnutím zobrazení úloh. Měli byste vidět, zobrazí se dialogové okno naplněný hello úlohy URL.

![Nástroje data Lake Visual Studio úlohy prohlížeče](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view.png)

Obsahuje zobrazení úloh:

* Souhrn úlohy
  
    Aktualizujte hello zobrazení úloh toosee hello nejnovější informace o spuštěných úloh.
  
  * Stav úlohy (grafu):
    
      Stav úlohy popisuje fáze úlohy hello:
    
      ![Fáze stav úlohy Azure Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-phases.png)
    
    * Příprava: Nahrajte váš skript toohello cloudu, kompilace a optimalizace pomocí služby kompilace hello skriptu hello.
    * Zařazených do fronty: Úlohy jsou ve frontě syrovátky, že čekají na dostatek prostředků nebo hello úlohy překročí maximální počet souběžných úloh hello na omezení účtu. nastavení priority Hello určuje pořadí hello zařazených do fronty úloh - hello nižší číslo hello, vyšší prioritu hello hello.
    * Spuštění: hello úloha je ve skutečnosti spuštěná v účtu Data Lake Analytics.
    * Dokončení: je dokončení úlohy hello (například dokončení hello souboru).
      
      Úloha Hello může selhat v každé fázi. Například chyby při kompilaci v hello Příprava fáze, vypršení časového limitu ve fázi hello zařazeno do fronty a provádění chyby ve fázi spuštěná hello atd.
  * Základní informace
    
      informace o základní úlohy Hello zobrazuje v dolní části hello hello Souhrn úlohy panelu.
    
      ![Fáze stav úlohy Azure Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-info.png)
    
    * Výsledek úlohy: Byla úspěšná nebo neúspěšná. Úloha Hello může selhat v každé fázi.
    * Celková doba trvání: Horní času hodin (doba trvání) mezi odesláním čas a koncový čas.
    * Celková doba výpočetní: hello součet při každém spuštění vrchol, můžete zvážit, jestli že ho jako hello čas této hello úlohy se spouštějí v pouze jednoho vrcholu. TooTotal vrcholy toofind naleznete další informace o vrchol.
    * Odeslat/počáteční/koncový čas: hello čas, kdy hello služby Data Lake Analytics přijme úlohy odeslání nebo spustí toorun hello úlohy nebo ukončí úlohu hello úspěšně nebo ne.
    * Kompilace nebo zařazeno do fronty nebo spuštění: Skutečný čas strávený fázi hello příprava nebo zařazeno do fronty nebo spuštěna.
    * Účet: účet Data Lake Analytics hello používaný ke spuštění úlohy hello.
    * Autor: hello uživatel, který odeslal hello úlohy, může být skutečný člověk účet nebo účet system.
    * Priorita: hello Priorita hello úlohy. Hello nižší hello číslo, vyšší prioritu hello hello. Ovlivňuje pouze hello pořadí úloh hello ve frontě hello. Nastavení s vyšší prioritou není mají prioritu před spuštěné úlohy.
    * Paralelismus: hello požadovaný maximální počet souběžných Azure Data Lake Analytics jednotek (ADLAUs), neboli vrcholy. V současné době jednoho vrcholu je rovna tooone virtuálních počítačů s dva virtuální jádra a 6 GB paměti RAM, i když může být upgradována v budoucnosti Data Lake Analytics aktualizací.
    * Zbývající bajty: Bajtů, které je třeba toobe zpracovat až do dokončení úlohy hello.
    * Číst/zapsaných bajtů: bajtů, které byly číst nebo zapisovat od spuštění úlohy hello.
    * Celkový počet vrcholy: hello úlohy je rozdělena na mnoha pracovními, každou část práce se nazývá vrchol. Tato hodnota popisuje, kolik pracovními hello úlohy se skládá z. Můžete zvážit vrchol jako základní proces jednotku, neboli Azure Data Lake Analytics jednotky (ADLAU), a vrcholy se může spouštět ve stupně paralelního zpracování. 
    * Byla dokončena nebo spuštěná nebo se nezdařilo: hello počet vrcholy dokončit nebo spuštěná nebo se nezdařilo. Vrcholy můžou selhat v důsledku selhání kódu a systému tooboth uživatele, ale hello systému opakované pokusy byly neúspěšné vrcholy automaticky několikrát. Pokud po opakovaném pokusu se stále nezdařila vrchol hello, hello celou úlohy se nezdaří.
* Graf úlohy
  
    Skript U-SQL představuje hello logiku transformace dat toooutput vstupní data. skript Hello kompiluje a optimalizovaný na fázi Příprava hello tooa fyzické spuštění plánu. Graf úlohy je tooshow hello fyzické spuštění plánu.  Hello následující diagram znázorňuje proces hello:
  
    ![Fáze stav úlohy Azure Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-logical-to-physical-plan.png)
  
    Úloha je rozdělena na mnoha pracovními. Každý část práce se nazývá vrchol. Hello vrcholy jsou seskupeny jako Super vrchol (neboli stage) a vizualizována jako graf úlohy. malá zelená fáze Hello v grafu úlohy hello zobrazit hello fázích.
  
    Všechny vrcholy ve fázi provádí hello stejný druh pracovat různá hello stejná data. Například pokud máte soubor s jedním datovým TB a používají stovky vrcholy čtení z něj, každý z nich je čtení bloku. Tyto vrcholy jsou seskupené v hello stejné fáze a to stejné fungovat na různých částí stejné vstupní soubor.
  
  * <a name="state-information"></a>Fáze informace
    
      V konkrétní fázi jsou uvedeny některé čísla v hello štítek.
    
      ![Fáze graf úlohy Azure Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage.png)
    
    * SV1 Extrahování: název hello fáze, s názvem metodou operace číslo a hello.
    * 84 vrcholy: Celkový počet vrcholy hello v této fázi. Obrázek Hello Určuje, kolik pracovními je rozdělený v této fázi.
    * 12.90 s/vrchol: hello vrchol průměrný čas provádění pro tuto fázi. Tato hodnota je vypočítána pomocí SUM (každých čas provádění vrchol) / (celková vrchol počet). To znamená, pokud může přiřadit všechny vrcholy hello spustit v paralelismus, celou fáze hello je dokončit 12.90 s. Také znamená, pokud všechny hello práce v této fázi se provádí sériově, hello náklady by měly být #vertices * Průměrný čas.
    * 850,895 řádků zapsaných: Celkový počet řádků, které jsou napsané v této fázi.
    * R/w: množství dat pro čtení nebo Written v této fázi v bajtech.
    * Barvy: Barvy se používají v hello fáze tooindicate různých vrchol stavu.
      
      * Zelená značí, že je úspěšně vrchol hello.
      * Oranžová označuje, že se pokus o vrchol hello. Hello opakovat vrchol byl selhal, ale je automaticky opakovat a úspěšně hello systém a hello celkové fáze byla úspěšně dokončena. Pokud hello vrchol opakovat, ale stále se nezdařilo, změní barvu hello red a hello celou úlohy se nezdařilo.
      * Červená značí selhání, což znamená, že určité vrchol obsahovala byla několikrát opakovat hello systému, ale stále se nezdařilo. Tento scénář způsobí, že toofail hello celé úlohy.
      * Modré znamená určité vrchol je spuštěna.
      * Prázdné označuje hello vrchol čeká. vrchol Hello může být čekání toobe naplánované až ADLAU bude k dispozici, nebo ho může být čekání na vstup od jeho vstupních dat nemusí být připravené.
      
      Další informace naleznete pro fázi hello podržením ukazatele myši podle jednoho stavu:
      
      ![Graf fáze podrobnosti úlohy Azure Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage-details.png)
  * Vrcholy: Popisuje podrobnosti vrcholy hello, například kolik vrcholy celkem, kolik vrcholy byly dokončeny, jsou jejich se nezdařilo nebo je stále spuštěná nebo čekání atd.
  * Číst data mezi nebo uvnitř pod: soubory a data jsou uloženy v několika pracovními stanicemi soustředěnými kolem v systému souborů DFS. Hodnota Hello tady popisuje, kolik dat byl přečten v hello stejné pod nebo křížové pod.
  * Celkový čas výpočetní: hello součet provádění každé vrchol ve fázi hello, můžete zvážit ho jako čas hello by byly třeba pokud všechny fungovat ve fázi hello se spouštějí v pouze jednoho vrcholu.
  * Data a řádky zapsána/čtení: Určuje, kolik byly číst nebo zapisovat data nebo řádky, nebo musí toobe číst.
  * Vrchol číst selhání: Popisuje, kolik vrcholy došlo k selhání při čtení dat.
  * Vrchol duplicitní zahodí: Pokud vrchol běží příliš pomalu, hello systému může naplánovat více vrcholy toorun hello stejnou část práce. Reductant vrcholy se zahodí, po jednu hello vrcholy úspěšně dokončit. Vrchol duplicitní zahodí záznamy hello počet vrcholy, které jsou zahozeny jako duplicity vztahující se k ve fázi hello.
  * Vrchol odvolání: hello vrchol bylo úspěšné, ale získat znovu později spustit z důvodu toosome důvodů. Například pokud podřízené vrchol ztratí zprostředkující vstupních dat, bude dotaz toorerun nadřazeného vrchol hello.
  * Vrchol plán spuštěních: hello celkovou dobu, kterou bylo naplánováno hello vrcholy.
  * Průměr/minimální/maximální vrchol data načtená: hello minimální/průměrný nebo maximální počet všechny vrcholy číst data.
  * Doba trvání: hello skutečný čas úsek trvá, je nutné tooload profil toosee tuto hodnotu.
  * Přehrávání úloh
    
      Spustí úlohy data Lake Analytics a archivy hello vrcholy systémem informace hello úloh, jako je například hello vrcholy jsou při spuštění, zastavení se nezdařilo a jak jsou opakovat, atd. Všechny informace hello je automaticky přihlášeni hello úložiště dotazů a uložen v jeho profil úlohy. Si můžete stáhnout hello profil úlohy prostřednictvím "Profil zatížení" v zobrazení úloh a hello přehrávání úloh lze zobrazit po stažení hello profil úlohy.
    
      Přehrávání úloh je vizualizaci epitome co se stalo v clusteru hello. Pomáhá sledovat průběh provádění úlohy a vizuálně detekovat kritická místa v velmi krátké době (méně než 30s obvykle) a anomálie výkonu.
  * Úloha Heat mapa zobrazení 
    
      Heat mapa úlohy lze vybrat pomocí rozevíracího seznamu hello zobrazení v grafu úlohy. 
    
      ![Azure Data Lake Analytics úlohy graf haldy zobrazení mapy](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-display.png)
    
      Zobrazuje hello vstupně-výstupních operací, čas a propustnost heat mapa úlohy, pomocí kterého můžete najít kde hello úlohy tráví většinu času hello, nebo zda úlohu úlohu hranic vstupně-výstupních operací a tak dále.
    
      ![Příklad haldy mapy úlohy graf Azure Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-example.png)
    
    * Průběh: hello průběh provádění úlohy, přečtěte si informace v [Příprava informace](#stage-information).
    * Číst nebo zapisovat data: hello heat mapa celkový dat číst nebo zapisovat v jednotlivých fázích.
    * Výpočetní čas: hello heat mapa Sum (každých čas provádění vrchol), můžete zvážit to jak dlouho bude trvat, pokud všechnu práci ve fázi hello proveden s pouze 1 vrchol.
    * Průměrná doba provádění na jeden uzel: hello heat mapa Sum (každých čas provádění vrchol) / (vrchol číslo). Což znamená, pokud může přiřadit všechny vrcholy hello spustit v paralelismus, celou fáze hello bude provedeno v tento časový rámec.
    * Propustnost vstupu a výstupu: hello heat mapa propustnosti vstupu a výstupu u každé fáze, můžete potvrdit, pokud vaše úlohy vázané úlohu vstupně-výstupní operace přes tento.
* Operace s metadaty
  
    Můžete provádět některé operace metadata ve vašem skriptu U-SQL, jako například vytvoření databáze, drop table atd. Tyto operace jsou uvedené v operace metadat po kompilace. Může najít kontrolní výrazy, vytvořte entity, místo pro entity.
  
    ![Operace v Azure Data Lake Analytics úlohy zobrazení metadat](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-metadata-operations.png)
* Historie stavů
  
    Hello historie stavů jsou také vizualizována ve Souhrn úlohy, ale můžete získat další informace v tomto poli. Můžete najít hello podrobné informace, například když je připravená hello úlohy, ve frontě, spuštění, byl ukončen. Taky můžete najít kolikrát byl zkompilován hello úlohy (hello CcsAttempts: 1), když ve skutečnosti je hello úlohy odeslat toohello clusteru (hello podrobností: odeslání úlohy toocluster) atd.
  
    ![Azure Data Lake Analytics úlohy zobrazení historie stavů](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-state-history.png)
* Diagnostika
  
    Nástroj Hello automaticky diagnostikuje provádění úlohy. Pokud jsou některé chyby nebo problémy s výkonem v úlohách, budou dostávat upozornění. Všimněte si, je nutné, aby toodownload tooget úplné informace o profilu sem. 
  
    ![Azure Data Lake Analytics úlohy zobrazení diagnostiky](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-diagnostics.png)
  
  * Upozornění: Výstraha se zde zobrazuje s upozornění kompilátoru. Můžete kliknout na odkaz toohave "x problém(y)" podrobnosti po hello výstrahy.
  * Vrchol spustit příliš dlouhá: Pokud libovolný bod uchycení používá mimo dobu (například 5 hodin), problémy se nachází tady.
  * Využití prostředků: Pokud jste přidělili další nebo není dostatek paralelismus, než je nutné, problémy se nachází tady. Můžete také podrobnosti klikněte na tlačítko toosee využití prostředků a provádět citlivostních scénářů toofind lepší přidělení prostředků (Další podrobnosti najdete v této příručce).
  * Zkontrolujte paměť: Pokud libovolný bod uchycení používá více než 5 GB paměti, problémy se nachází tady. Provádění úlohy může získat ukončená systému, pokud používá víc paměti než omezení systému.

## <a name="job-detail"></a>Podrobnosti úlohy
Podrobnosti úlohy zobrazuje hello podrobné informace hello úlohy, včetně skriptu, prostředky a zobrazení provádění vrcholů.

![Podrobnosti úlohy Azure Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-details.png)

* Skript
  
    Hello skript U-SQL hello úlohy jsou uložené v úložišti dotazů hello. Můžete zobrazit hello původní skript U-SQL a odešlete ji znovu, v případě potřeby.
* Zdroje
  
    Můžete najít hello úlohy kompilace výstupy uložené v úložišti dotazů hello prostřednictvím prostředků. Například můžete najít "algebra.xml", což je použité tooshow hello graf úlohy, hello sestavení, které jste zaregistrovali, zde atd.
* Zobrazení provádění vrcholů
  
    Podrobnosti o provádění zobrazuje vrcholy. Hello úlohy profil archivy každých protokolu spuštění vrchol, jako je například celková data číst nebo zapisovat, modul runtime, stav, atd. Prostřednictvím tohoto zobrazení můžete získat další informace o tom, jak úloha běžela. Další informace najdete v tématu [použití hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

## <a name="next-steps"></a>Další kroky
* toolog diagnostické informace, najdete v části [přístup k protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* zobrazení provádění vrcholů toouse, najdete v části [použití hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)

