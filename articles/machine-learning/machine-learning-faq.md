---
title: "aaaAzure Machine Learning nejčastější dotazy (FAQ) | Microsoft Docs"
description: "Představení služby Azure Machine Learning: časté otázky k fakturaci, schopnostem a omezením cloudové služby pro efektivní prediktivní modelování"
keywords: "úvod ke strojovému učení, prediktivní modelování, co je strojové učení"
services: machine-learning
documentationcenter: 
author: garyericson
manager: paulettm
editor: cgronlun
ms.assetid: a4a32a06-dbed-4727-a857-c10da774ce66
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/02/2017
ms.author: garye
ms.openlocfilehash: 3af84451dde064c3c9520ee520b541128b1eef92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-frequently-asked-questions-billing-capabilities-limitations-and-support"></a>Nejčastější dotazy ke službě Azure Machine Learning: fakturace, možnosti, omezení a podpora
Zde jsou některé nejčastější dotazy (a příslušné odpovědi) týkající se cloudové služby Azure Machine Learning, která slouží k vývoji prediktivních modelů a zprovozňování řešení prostřednictvím webových služeb. Tyto nejčastější dotazy poskytují dotazy o tom, jak toouse hello service, která zahrnuje hello fakturační model, schopností, omezení a podpory.

**Máte dotaz, který tady nemůžete najít?**

Azure Machine Learning má fórum na webu MSDN, kde můžete klást otázky o Azure Machine Learning členové komunity vědecké účely hello data. tým Azure Machine Learning Hello monitoruje fórum hello. Přejděte toohello [fóru služby Azure Machine Learning](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toosearch pro odpovědi nebo toopost novou otázku vlastní.

## <a name="general-questions"></a>Obecné otázky
**Co je Azure Machine Learning?**

Azure Machine Learning je plně spravovaná služba, můžete použít toocreate, testování, provozovat a spravovat řešení prediktivní analýzy v cloudu hello. Vystačíte si jen s prohlížečem, přes který se můžete přihlásit, nahrát data a okamžitě začít experimentovat se strojovým učením. Prediktivní modelování podporující přetahování myší, rozsáhlá paleta modulů a knihovna šablon, se kterými je možné hned začít, značně usnadňují a urychlují běžné úkoly strojového učení. Další informace najdete v tématu hello [Přehled služby Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/). Learning toomachine Úvod vysvětlující klíčové terminologie a koncepty, najdete v části [tooAzure Úvod Machine Learning](machine-learning-what-is-machine-learning.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Co je Machine Learning Studio?**

Machine Learning Studio je pracovní prostředí, ke kterému přistupujete pomocí webového prohlížeče. Machine Learning Studio nabízí mnoho modulů v vizuálním kompozičním rozhraním, která vám pomáhá tvořit začátku do konce, vědecké zpracování dat pracovního postupu v hello formě experimentu.

Další informace o nástroji Machine Learning Studio najdete v tématu [Co je Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

**Co je Služba Machine Learning API hello?**

Hello Služba Machine Learning API umožňuje toodeploy prediktivní modely, například ty, které jsou součástí Machine Learning Studio, jako škálovatelné, odolné proti chybám, webové služby. Hello webové služby, které vytvoří služba Machine Learning API hello se rozhraní REST API, které poskytují rozhraní pro komunikaci mezi externími aplikacemi a vašimi modely prediktivní analýzy.

Další informace najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

**Kde jsou uvedené klasické webové služby? Kde jsou uvedené nové webové služby (využívající Azure Resource Manager)?**

Webové služby vytvořené pomocí hello klasického modelu a webové služby pro nasazení vytvořené pomocí modelu nasazení nové Azure Resource Manager hello jsou uvedeny v hello [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/) portálu.

Classic webových služeb jsou také uvedeny v [Machine Learning Studio](http://studio.azureml.net) na hello **webové služby** kartě.

## <a name="azure-machine-learning-questions"></a>Dotazy ke službě Azure Machine Learning
**Co jsou webové služby Azure Machine Learning?**

Webové služby Machine Learning poskytují rozhraní mezi aplikací a modelem Machine Learning pro vyhodnocování pracovních postupů. Externí aplikací můžete použít Azure Machine Learning toocommunicate s vyhodnocování model Machine Learning pracovního postupu v reálném čase. Volání tooa webové službě Machine Learning vrátí předpovědi výsledky tooan externí aplikaci. toomake tooa volání webové služby, předáte klíč rozhraní API, která byla vytvořena, když jste nasadili hello webové služby. Webová služba Machine Learning je založená na architektuře REST, která je u programátorských projektů na webu oblíbenou volbou.

Azure Machine Learning zahrnuje dva typy webových služeb:

* Služba požadavků a odpovědí (záznamy RR): Nízkou latencí, vysoce škálovatelná služba, která poskytuje rozhraní toohello bezstavové modely vytvořená a nasazená pomocí Machine Learning Studio.
* Služba Batch Execution (BES): Asynchronní služba pro vyhodnocování dávek datových záznamů.

Existuje několik způsobů tooconsume hello REST API a přístup hello webové služby. Můžete například napsat aplikaci v jazyce C#, R nebo Python pomocí hello ukázkový kód, který se vygeneruje pro vás, když jste nasadili hello webové služby.

Hello ukázkový kód je k dispozici na:
- Hello spotřebě stránky pro webovou službu hello hello webové služby Azure Machine Learning portálu
- Hello stránce nápovědy k rozhraní API v řídicím panelu hello webových služeb v nástroji Machine Learning Studio

Můžete také použít sešit hello ukázkové aplikace Microsoft Excel, který je pro vás vytvořen a je k dispozici v řídicím panelu hello webových služeb v nástroji Machine Learning Studio.

**Jaké jsou hlavní aktualizace tooAzure hello Machine Learning?**

Hello nejnovější aktualizace, najdete v části [co je nového v Azure Machine Learning](machine-learning-whats-new.md).

## <a name="machine-learning-studio-questions"></a>Otázky k nástroji Machine Learning Studio
### <a name="import-and-export-data-for-machine-learning"></a>Import a export dat pro Machine Learning
**Jaké zdroje dat Machine Learning podporuje?**

Si můžete stáhnout tooa dat Machine Learning Studio experimentu třemi způsoby:

- Nahrání místního souboru jako datové sady
- Použít modul tooimport data z cloudových datových služeb
- Import datové sady uložené z jiného experimentu

Další informace o toolearn podporované formáty souborů najdete v tématu [importu trénovacích dat do nástroje Machine Learning Studio](machine-learning-data-science-import-data.md).

#### <a id="ModuleLimit"></a>Jak velká může hello být datová sada pro mé moduly?
Modulů v Machine Learning Studio podporují datové sady z až too10 GB hustých číselných dat pro běžné případy použití. Pokud modul přijímá více než jeden vstup, hello 10 GB hodnota je hello celkový počet všech vstupních velikostí. Větší datové sady je před ingestováním možné vzorkovat pomocí dotazů Hive nebo Azure SQL Database. Je možné také použít předzpracování metodou Learning by Counts.  

Hello následující typy dat během normalizace příznaků můžete rozšířit toolarger datových sad a jsou omezené tooless než 10 GB:

* Řídké
* Kategorické
* Řetězce
* Binární data

Hello následující moduly jsou omezené toodatasets menší než 10 GB:

* Doporučené moduly
* Modul SMOTE (Synthetic Minority Oversampling Technique)
* Skriptovací moduly: R, Python, SQL
* Moduly, kde výstup hello velikost dat může být větší než velikost vstupních dat, jako je například spojení nebo Hashování
* Křížové ověření, Tune Model Hyperparameters, Ordinal Regression a One-vs-All Multiclass hello počet opakování je velmi velká

#### <a id="UploadLimit"></a>Jaká jsou omezení hello dat nahrát?
Pro datové sady, které jsou větší než několik GB, nahrávat data tooAzure úložiště nebo Azure SQL Database, nebo použití Azure HDInsight nikoli přímo nahrávat z místního souboru.

**Je možné číst data z Amazonu S3?**

Pokud máte malé množství dat a chcete tooexpose prostřednictvím adresy URL protokolu HTTP, pak může použít hello [importovat Data] [ import-data] modulu. Pro větší množství dat, přeneste ji tooAzure úložiště nejprve a pak použijte hello [importovat Data] [ import-data] toobring modulu do experimentu.
<!--

<SEE CLOUD DS PROCESS>
-->

**Je k dispozici integrovaná funkce pro obrazový vstup?**

Dozvíte se o funkci obrazového vstupu v hello [Import obrázků] [ image-reader] odkaz.

### <a name="modules"></a>Moduly
**Hello algoritmus, zdroj dat, formát dat nebo operace transformace dat, kterou hledám není v Azure Machine Learning Studio. Jaké mám možnosti?**

Můžete přejít toohello [fóru pro zpětnou vazbu uživatelů](http://go.microsoft.com/fwlink/?LinkId=404231) toosee funkce požadavky jsme sledování. Vaše žádost tooa hlas přidejte, pokud již požadavek funkci, která hledáte. Pokud hello schopností, který hledáte, neexistuje, vytvořte novou žádost. Na tomto fóru můžete zobrazit stav hello žádosti příliš. Tento seznam pečlivě sledovat jsme často aktualizovat hello stav dostupnosti funkce. Kromě toho můžete hello integrovaná podpora pro R a Python toocreate vlastní transformace v případě potřeby.

**Můžu převést svůj existující kód do nástroje Machine Learning Studio?**

Ano, můžete zahrnout existující kód R nebo Python do nástroje Machine Learning Studio, spusťte v hello stejné experimentovat s inteligentních algoritmů Azure Machine Learning a nasadit hello řešení jako webovou službu pomocí Azure Machine Learning. Další informace najdete v tématech o [rozšíření experimentů pomocí R](machine-learning-extend-your-experiment-with-r.md) a o [spouštění pythonových skriptů strojového učení v nástroji Azure Machine Learning Studio](machine-learning-execute-python-scripts.md).

**Je možné toouse něco jako [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) toodefine modelu?**

Ne, jazyk PMML (Predictive Model Markup Language) se nepodporuje. Můžete použít vlastní R a Python kódu toodefine modul.

**Kolik modulů je možné v experimentu paralelně spustit?**  

Můžete spustit až toofour moduly v experimentu paralelně.

### <a name="data-processing"></a>Zpracování dat
**Je k dispozici data možnost toovisualize (kromě vizualizací R) interaktivně v rámci experimentu hello?**

Klikněte na výstup hello dat hello toovisualize modulu a získat statistiky.

**Při prohlížení náhledu výsledků nebo dat v prohlížeči, je omezený počet hello řádků a sloupců. Proč?**

Protože velké objemy dat mohou být odeslány tooa prohlížeče, velikost dat je omezená tooprevent zpomalení nástroje Machine Learning Studio. toovisualize všechny hello data nebo výsledky, lepší toodownload hello dat a použít Excel nebo jiný nástroj.

### <a name="algorithms"></a>Algoritmy
**Jaké stávající algoritmy podporuje Machine Learning Studio?**

Machine Learning Studio poskytuje nejmodernější algoritmy, například škálovatelné vylepšené rozhodovací stromy, systémy bayesovského rozhodování, hluboké neuronové sítě a rozhodovací džungle vyvinuté v Microsoft Research. Dostupné jsou i škálovatelné open-sourcové balíčky pro strojové učení, třeba Vowpal Wabbit. Machine Learning Studio podporuje algoritmy strojového učení pro binární klasifikaci a klasifikaci s více třídami, regresi a clustering. Viz úplný seznam k hello [modulů Machine Learning][machine-learning-modules].

**Budete automaticky hello správné Machine Learning algoritmus toouse pro svá data navrhnout?**

Ne, ale Machine Learning Studio obsahuje různé způsoby, kterými toocompare hello výsledky jednotlivých algoritmus toodetermine hello správné jednu pro váš problém.

**Máte poradit, výběr jeden algoritmus oproti jinému pro algoritmy hello poskytuje?**

V tématu [jak toochoose algoritmus](machine-learning-algorithm-choice.md).

**Jsou zadaná hello algoritmy napsány v R nebo Pythonu?**

Ne, tyto algoritmy jsou většinou napsány v kompilovaných jazycích tooprovide lepší výkon.

**Jsou nějaké podrobnosti o hello algoritmy poskytuje?**

Hello dokumentace obsahuje některé informace o hello algoritmy a parametry pro ladění jsou popsány toooptimize hello algoritmu pro vaše použití.  

**Podporuje se online učení?**

Ne, v tuto chvíli je podporováno jenom programové přeučení.

**Můžete vizualizovat hello vrstvy modelu Neuronové sítě pomocí integrovaného modulu hello?**

Ne.

**Je možné vytvářet vlastní moduly v jazyce C# nebo v nějakém jiném jazyce?**

V současné době můžete použít pouze vlastních modulů R toocreate nové.

### <a name="r-module"></a>Modul R
**Jaké balíčky R jsou k dispozici v nástroji Machine Learning Studio?**

Machine Learning Studio podporuje více než 400 balíčků CRAN r. dnes, a zde je hello [aktuálního seznamu](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) všechny zahrnuté balíčky. Další informace naleznete v [rozšíření experimentů pomocí R](machine-learning-extend-your-experiment-with-r.md) toolearn jak tooretrieve tento seznam sami. Pokud hello balíček, který chcete v tomto seznamu není, zadejte název hello hello balíčku na hello [fóru pro zpětnou vazbu uživatelů](http://go.microsoft.com/fwlink/?LinkId=404231).

**Je možné toobuild vlastní R modulu?**

Ano, další informace najdete v tématu o [vytváření vlastních modulů R ve službě Azure Machine Learning](machine-learning-custom-r-modules.md).

**Je pro R k dispozici prostředí REPL?**

Ne, je žádné čtení – zkušební verze-tisk-smyčky () prostředí REPL. v hello studio.

### <a name="python-module"></a>Modul Python
**Je možné toobuild vlastní modul Python?**

Není v současné době ale můžete použít jeden nebo více [Execute Python Script] [ python] tooget moduly hello stejného výsledku.

**Je pro Python k dispozici prostředí REPL?**

V nástroji Machine Learning Studio můžete použít hello Jupyter Notebooks. Další informace najdete v tématu [Úvod do aplikace Jupyter Notebooks v Azure Machine Learning Studio](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## <a name="web-service"></a>Webová služba
### <a name="retrain"></a>Přeučování
**Jak je možné programově přeučit modely Azure Machine Learning?**

Použijte hello retraining API. Další informace najdete v tématu o [programovém přeučení modelů Machine Learning](machine-learning-retrain-models-programmatically.md). Ukázkový kód je také dostupná v hello [Microsoft Azure Machine Learning ukázce Přeučování](https://azuremlretrain.codeplex.com/).

### <a name="create"></a>Vytvořit
**Můžete nasadit hello model, místně nebo v aplikaci, která nemá připojení k Internetu?**

Ne.

**Existuje základní latence, kterou je možné očekávat u všech webových služeb?**

V tématu hello [limity předplatného Azure](../azure-subscription-service-limits.md).

### <a name="use"></a>Použití
**Pokud by se měla toorun prediktivní model jako službu Batch Execution a service Request Response?**

Hello služba Request Response (RRS) je nízkou latencí, špičkové webová služba, která je použité tooprovide rozhraní toostateless modelů, které jsou vytvořené a nasazené z prostředí experimentů hello. Hello služba Batch Execution (BES) je služba, která asynchronně skóre dávky datových záznamů. Hello vstup pro BES je jako vstup dat, který používá záznamy o prostředku. Hello hlavní rozdíl je, že BES čte blok záznamů z různých zdrojů, například Azure Blob storage, Azure Table storage, Azure SQL Database, HDInsight (dotaz hive) a zdrojů HTTP. Další informace najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

**Jak aktualizovat hello modelu pro hello nasadit webovou službu?**

tooupdate prediktivního modelu pro již nasazenou službu, upravit a znovu spusťte hello experiment, kterou jste použili tooauthor a uložit hello trained model. Až budete mít novou verzi k dispozici hello trained model, Machine Learning Studio požádá, pokud chcete tooupdate webové služby. Další informace o tom najdete v části tooupdate nasazenou webovou službu [nasazení webové služby Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

Můžete také použít hello rozhraní Retraining API.
Další informace najdete v tématu o [programovém přeučení modelů Machine Learning](machine-learning-retrain-models-programmatically.md). Ukázkový kód je také dostupná v hello [Microsoft Azure Machine Learning ukázce Přeučování](https://azuremlretrain.codeplex.com/).

**Jak je možné sledovat webovou službu nasazenou do produkčního prostředí?**

Po nasazení prediktivního modelu, můžete ho sledovat z hello Azure classic portal (pouze Classic webové služby) nebo webové služby Azure Machine Learning portál hello. Každá nasazená služba má svůj vlastní řídicí panel, kde můžete sledovat informace z monitorování dané služby. Další informace o tom, jak toomanage nasazené webové služby, najdete v části [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md) a [spravovat pracovní prostor služby Azure Machine Learning](machine-learning-manage-workspace.md).

**Je k dispozici na místě, kde jsou zobrazeny hello výstup RRS nebo BES?**

Pro záznamy o prostředcích hello odpověď webové služby je obvykle kde uvidíte hello výsledek. Je také možné zapsat ho tooAzure úložiště objektů Blob. Pro BES je výstup hello podle výchozího nastavení zapisují tooa objektů blob. Je také možné zapsat hello výstup tooa databázi nebo tabulku pomocí hello [Export dat] [ export-data] modulu.

**Webové služby je možné vytvářet jenom z modelů, které byly vytvořené v Machine Learning Studiu?**

Ne, webové služby je možné vytvářet i přímo pomocí Jupyter Notebooks a RStudia.

**Kde najdu informace o chybových kódech?**

Seznam chybových kódů a jejich popisy najdete v tématu o [chybových kódech modulů Machine Learning](https://msdn.microsoft.com/library/azure/dn905910.aspx).

## <a name="scalability"></a>Škálovatelnost
**Co je škálovatelnost hello hello webové služby?**

V současné době hello výchozí koncový bod zřizován s 20 souběžnými požadavky RRS na jeden koncový bod. Je možné škálovat tento too200 souběžných požadavků na jeden koncový bod a je možné škálovat každý too10 webové služby, 000 koncových bodů webové služby, jak je popsáno v [škálování webové služby](machine-learning-scaling-webservice.md). Pro BES každý koncový bod může zpracovat 40 požadavků najednou, požadavky nad těchto 40 požadavků se zařazují do fronty. Požadavky ve frontě spustit automaticky jako hello fronta vyprazdňuje.

**Jsou úlohy R rozdělené mezi uzly?**

Ne.  

**Kolik dat je možné použít k trénování?**

Modulů v Machine Learning Studio podporují datové sady z až too10 GB hustých číselných dat pro běžné případy použití. Pokud modul přijímá více než jeden vstupní, hello celková velikost všech vstupů je 10 GB. Větší datové sady je před ingestováním možné vzorkovat pomocí dotazů Hivu, dotazů Azure SQL Database nebo předzpracováním pomocí modulů [učení na základě počtů][counts].  

Hello následující typy dat během normalizace příznaků můžete rozšířit toolarger datových sad a jsou omezené tooless než 10 GB:

* Řídké
* Kategorické
* Řetězce
* Binární data

Hello následující moduly jsou omezené toodatasets menší než 10 GB:

* Doporučené moduly
* Modul SMOTE (Synthetic Minority Oversampling Technique)
* Skriptovací moduly: R, Python, SQL
* Moduly, kde výstup hello velikost dat může být větší než velikost vstupních dat, jako je například spojení nebo Hashování
* Pro velmi velký počet iterací Cross-Validate, Tune Model Hyperparameters, Ordinal Regression a One-vs-All Multiclass

Pro datové sady, které jsou větší než několik GB, nahrávat data tooAzure úložiště nebo Azure SQL Database, případně použít HDInsight nikoli přímo nahrávat z místního souboru.

**Existují nějaká omezení velikosti vektoru?**

Řádky i sloupce jsou omezení Max Int každý omezené toohello .NET: 2 147 483 647.

**Můžete upravit velikost hello hello virtuálního počítače, který spouští hello webovou službu?**

Ne.  

## <a name="security-and-availability"></a>Zabezpečení a dostupnost
**Kdo má přístup k hello koncový bod http pro webovou službu hello ve výchozím nastavení? Jak omezím přístup toohello koncový bod?**

Po nasazení webové služby je pro tuto službu vytvořen koncový bod. Hello výchozí koncový bod je možné volat pomocí jeho klíče rozhraní API. Můžete přidat, že hello další koncové body s jejich vlastními klíči z hello portál Azure classic nebo programově pomocí rozhraní Web Service Management API. Přístupové klíče jsou potřebné toomake volání toohello webové služby. Další informace najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

**Co se stane, když se můj účet Azure Storage nenajde?**

Machine Learning Studio využívá u datových zprostředkující toosave účet úložiště Azure uživatelem zadané, když ji spustí pracovní postup hello. Tento účet úložiště je k dispozici tooMachine Learning Studio při vytváření pracovního prostoru. Po hello je vytvořen pracovní prostor, pokud účet úložiště hello se odstraní a již nelze nalézt, hello pracovní prostor přestane fungovat a všechny experimenty v tom, že pracovní prostor se nezdaří.

Pokud omylem odstraníte účet úložiště hello, znovu vytvořit účet úložiště hello hello stejný název v hello stejné oblasti jako hello odstranit účet úložiště. Potom nové synchronizace hello přístupový klíč.

**Co se stane, když přístupový klíč účtu úložiště není synchronizovaný?**

Machine Learning Studio využívá u datových zprostředkující toostore účet úložiště Azure uživatelem zadané, když ji spustí pracovní postup hello. Tento účet úložiště je získáte tooMachine Learning Studio, při vytváření pracovního prostoru, a hello přístupové klíče se přiřadí k danému pracovnímu prostoru. Pokud se hello přístupové klíče se změnila po vytvoření pracovního prostoru hello, hello prostoru mít nadále přístup k účtu úložiště hello. Přestane proto fungovat a všechny experimenty v něm selžou.

Pokud jste změnili přístupové klíče účtu úložiště, hello synchronizaci hello přístupové klíče v prostoru hello pomocí portálu Azure classic.  

## <a name="support-and-training"></a>Podpora a školení
**Kde získám školení pro Azure Machine Learning?**

Hello [centru dokumentace Azure Machine Learning na](https://azure.microsoft.com/services/machine-learning/) hostitelem video kurzy a jak tooguides. Tyto podrobné návody zavést hello služby a popisují hello datové vědy životní cyklus importu dat, čištění dat, vytváření prediktivních modelů a jejich nasazení v produkčním prostředí pomocí Azure Machine Learning.

Průběžné přidáme nové podstatným toohello centru pro Machine Learning. Můžete zaslat žádost o další výukové materiály v Centru pro Machine Learning v hello [fóru pro zpětnou vazbu uživatelů](https://windowsazure.uservoice.com/forums/257792-machine-learning).

Školení najdete i v [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning).

**Jak je možné získat podporu pro Azure Machine Learning?**

tooget technická podpora pro Azure Machine Learning, přejděte příliš[podporu Azure](/support/options/)a vyberte **Machine Learning**.

Azure Machine Learning má i fórum komunity na webu MSDN, kde se můžete ptát na věci související se službou Azure Machine Learning. tým Azure Machine Learning Hello monitoruje fórum hello. Přejděte příliš[fóru Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).

## <a name="billing-questions"></a>Dotazy k fakturaci
**Jak se Machine Learning fakturuje?**

Azure Machine Learning má dvě komponenty: Machine Learning Studio a webové služby Machine Learning.

Při hodnocení Machine Learning Studio, můžete úroveň Free fakturace hello. úroveň Free Hello také umožňuje nasazení Classic webová služba, která má omezenou kapacitu.

Pokud se rozhodnete, že Azure Machine Learning vyhovuje vašim potřebám, můžete si zaregistrovat hello úrovně Standard. toosign nahoru, musí mít předplatné Microsoft Azure.

V úrovni Standard hello fakturuje se každý měsíc pro každý pracovní prostor, který definujete v Machine Learning Studio. Při spuštění experimentu v hello studio se vám fakturují za výpočetní prostředky při spuštění experimentu. Při nasazení Classic webové služby, transakce a výpočetní hodiny se účtují na základě platím průběžně hello.

S novými webovými službami (založenými na Resource Manageru) přichází fakturační plány, se kterými lépe odhadnete výsledné náklady. Vrstvený ceny nabízí toocustomers zvýhodněné sazby, které potřebují velké množství kapacity.

Při vytváření plánu potvrzení tooa pevné náklady, která se dodává s zahrnuté množství výpočetních hodin API a rozhraní API transakce. Pokud potřebujete více zahrnuté množství, můžete přidat plán tooyour instance. Pokud budete vyžadovat podstatně víc výpočetního času a transakcí, můžete si vybrat vyšší úroveň, která kromě toho nabízí i výhodnější sazbu.

Až hello zahrnuté množství v existující instance vyčerpáte, další využití výši hello Nadlimitní míry, který je spojen s hello fakturační plán vrstvy.

> [!NOTE]
Zahrnuté množství jsou opětovnému přidělení každých 30 dní, a nepoužívané zahrnuté množství nevracet přes toohello další období.

Další informace o fakturaci a cenách najdete v tématu [Machine Learning – ceny](https://azure.microsoft.com/pricing/details/machine-learning/).

**Nabízí Machine Learning bezplatnou zkušební verzi?**

 Azure Machine Learning nabízí možnost bezplatného předplatného, které je vysvětlené v tématu [Podrobnosti o cenách za Machine Learning](https://azure.microsoft.com/pricing/details/machine-learning/). Machine Learning Studio obsahuje rychlý vyhodnocení osmi hodin zkušební verze, která je k dispozici, když se přihlásíte příliš[Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2).

 Když se navíc zaregistrujete k bezplatné zkušební verzi Azure, získáte možnost vyzkoušet si všechny služby Azure po dobu jednoho měsíce. toolearn Další informace o hello Azure bezplatnou zkušební verzi, navštivte [Azure bezplatné zkušební verze – nejčastější dotazy](https://azure.microsoft.com/pricing/free-trial-faq/).

**Co je transakce?**

Transakcí se rozumí volání rozhraní API, na které odpoví služba Azure Machine Learning. Transakce, které proběhnou po volání služeb Request-Response (RRS) a Batch Execution (BES), se sčítají a potom účtují podle vašeho plánu.

**Lze použít hello zahrnuté množství transakcí v plánu pro transakce RRS a BES?**

Ano, transakce RRS a BES se sčítají dohromady a účtují podle vašeho plánu.

**Co je výpočetní čas rozhraní API?**

Výpočetní hodiny API je jednotka fakturace hello dobu hello že toorun proveďte volání rozhraní API pomocí Machine Learning výpočetní prostředky. Všechna volání se vám pro účely fakturace započítávají dohromady.

**Jak dlouho obvykle trvá volání rozhraní API produkčního prostředí?**

Časy volání rozhraní API produkčního prostředí se může lišit výrazně, obecně od stovky milisekundách tooa několik sekund. Některá volání rozhraní API může vyžadovat minut v závislosti na složitosti hello hello zpracování dat a strojové učení modelu. Hello nejlepší způsob, jak tooestimate rozhraní API produkčního prostředí volání časy je toobenchmark modelu na hello služby Machine Learning.

**Co je výpočetní hodina Studia?**

Výpočetní hodinu Studio je hello fakturace jednotka pro agregovaná doba hello, experimentů v studio použití výpočetní prostředky.

**V nových služeb (Azure Resource Manager) webovou vrstvu vývoje/testování hello pojmy pro?**

Založené na správci prostředků webové služby poskytují více úrovně, které můžete použít tooprovision cenového plánu. Hello cenovou úroveň pro vývoj/testování poskytuje omezené, zahrnuté množství, které vám umožňují tootest experimentu jako webovou službu, aniž by docházelo k náklady. Máte toosee hello příležitost, jak to funguje.

**Platí se úložiště odděleně?**

Machine Learning bezplatnou úroveň Hello nevyžaduje ani povolit samostatné úložiště. Machine Learning standardní vrstvě Hello vyžaduje toohave uživatelé s účtem úložiště Azure. Služba Azure Storage se [fakturuje zvlášť](https://azure.microsoft.com/pricing/details/storage/).

**Podporuje služba Machine Learning vysokou dostupnost?**

Ano. Podrobnosti najdete v tématu [Machine Learning – ceny](https://azure.microsoft.com/en-us/pricing/details/machine-learning/) popis hello smlouvy o úrovni služeb (SLA).

**Jaké výpočetní prostředky konkrétně zpracovávají volání rozhraní API produkčního prostředí?**

Hello služby Machine Learning je víceklientské služby. Skutečné výpočetní prostředky, které se používají na hello back-end se liší a jsou optimalizované pro výkon a předvídatelnost.

### <a name="management-of-new-resource-manager-based-web-services"></a>Správa nových webových služeb (využívajících Resource Manager)
**Co se stane, když odstraním plán?**

plán Hello se odebere ze svého předplatného a fakturuje se poměrné využití.

> [!NOTE]
Plán, který právě používá nějaká webová služba, se nedá odstranit. plán hello toodelete, je nutné buď přiřadit nový plán toohello webové služby nebo odstranit hello webové služby.

**Co je instance plánu?**

Plán instance je jednotka zahrnuté množství, můžete přidat tooyour fakturační plán. Po výběru úrovně obsahuje plán jednu instanci. Pokud potřebujete více zahrnuté množství, můžete přidat instancí hello vybrané úrovně tooyour plán fakturace.

**Kolik instancí plánu je možné přidat?**

Může mít jednu instanci cenová úroveň hello vývoje/testování v odběru.

Úrovně Standard S1, Standard S2 a Standard S3 můžete přidávat podle potřeby.

> [!NOTE]
V závislosti na vaší předpokládaného využití doporučujeme může být cenově efektivnější tooupgrade tooa vrstvy, která má více zahrnuté množství spíše přidat instancí toohello Současná vrstva.

**Co se stane po změně úrovně plánu (po upgradu nebo downgradu)?**

původní plán Hello je odstranit a aktuálního využití hello je účtován na základě poměrné. Pro zbytek hello hello období se vytvoří nový plán s hello úplné zahrnuté množství hello upgradovat nebo snížit úroveň.

> [!NOTE]
Zahrnuté množství se přiděluje na jednotlivá období a nevyužité prostředky se do dalších období nepřevádějí.

**Co se stane, když zvýšit hello instancí v plánu?**

Počty jsou obsaženy na základě poměrné a může trvat 24 hodin toobe efektivní.

**Co se stane po odstranění instance plánu?**

Hello instance je odebrána ze svého předplatného a fakturuje se poměrné využití.

### <a name="sign-up-for-new-resource-manager-based-web-services-plans"></a>Registrace k novým plánům Web Service (které využívající Resource Manager)
**Jak se mám k plánu zaregistrovat?**

Máte dva způsoby toocreate fakturace plány.

Při prvním nasazení webové služby využívající Resource Manager můžete vybrat existující plán nebo vytvořit nový.

Plány, které vytvoříte tímto způsobem jsou ve vašem regionu výchozí a webová služba bude nasazený toothat oblast.

Pokud chcete toodeploy služby tooregions než výchozí oblasti, můžete toodefine fakturace plánu před nasazením služby.

V takovém případě můžete přihlásit toohello webové služby Azure Machine Learning portál a přejděte toohello plány stránku. Odtud můžete plány přidávat, odstraňovat a upravovat.

**Které plán by měl vybrat toostart vypnout s?**

Doporučujeme vám, který bude začínat hello Standard S1 vrstvy a monitorovat vaši službu pro využití. Pokud zjistíte, že vaše zahrnuté množství používáte rychle, můžete přidat instance nebo přesunout tooa vyšší úroveň a získat lepší zvýhodněné sazby. Fakturační plán můžete podle potřeby upravovat kdykoliv během zúčtovacího období.

**Které oblasti jsou k dispozici v nové plány hello?**

Hello nové fakturace plány jsou k dispozici v hello tři produkční oblastech ve kterých podporujeme hello nové webové služby:

* Střed USA – jih
* Západní Evropa
* Jihovýchodní Asie

**Webové služby využívám v několika různých oblastech. Potřebuji plán pro každou oblast?**

Ano. Ceny plánů se podle oblasti liší. Když nasadíte oblast tooanother webové služby, je třeba tooassign it a plán, který je konkrétní toothat oblast. Další informace najdete v tématu [Dostupné produkty v jednotlivých oblastech]( https://azure.microsoft.com/regions/services/).

### <a name="new-web-services-overages"></a>Nové webové služby: nadlimitní využití
**Jak můžu ověřit, jestli webové služby nevyužívám nadlimitně?**

Můžete zobrazit hello použití na všechny plány na stránce plány hello hello webové služby Azure Machine Learning portálu. Přihlaste se toohello portál a potom klikněte na hello **plány** možnost nabídky.

V hello **transakce** a **výpočetní** sloupce tabulky hello, uvidíte hello zahrnuté množství hello plán a procento hello používá.

**Co se stane, když se používá až hello zahrnout počty cenovou úroveň pro vývoj/testování hello?**

Služby, které mají vývoje/testování cenová úroveň přiřazené toothem se zastaví dokud hello další období nebo dokud se jejich přesunutí tooa placené vrstvy.

**Jak se počítají ceny úloh RRS (Request Response) a BES (Batch) v případě klasických webových služeb a nadlimitního využití nových webových služeb (využívajících Resource Manager)?**

Pro zatížení RRS budou se vám účtovat pro každou transakci volání rozhraní API, které vytváříte a pro dobu hello výpočtů, který je spojen s těmito požadavky. Náklady RRS rozhraní API produkčního transakce se počítají jako hello celkový počet volání rozhraní API, které provedete násobí hodnotou hello cena za 1 000 transakcí (účtovány poměrnou částí jednotlivých transakcí). Náklady na hodinu výpočetní rozhraní API produkčního prostředí rozhraní API záznamy o prostředku se počítá jako hello množství času potřebné pro každé toorun volání rozhraní API, násobenou hello celkový počet transakcí rozhraní API, násobenou hello cena za hodinu výpočetní rozhraní API produkčního prostředí.

Například pro Nadlimitní události úrovně Standard S1, 1 000 000 transakcí rozhraní API, které 0,72 sekund každý toorun by způsobilo (1000000 * $0,50 / 1 tisíc transakcí rozhraní API) v 500 USD v cena za transakci rozhraní API produkčního prostředí a (1000000 * 0.72 sekundu * $2 / hr) $400 v výpočetní rozhraní API produkčního prostředí hodiny celkem 900 $.

Pro BES zatížení, budou se vám účtovat v hello stejný způsobem. Však cena za transakci hello API představují hello počet dávkové úlohy, které odešlete a náklady na výpočetní hello představují dobu hello výpočtů, který je spojen s tyto úlohy batch. Náklady BES rozhraní API produkčního transakce se počítají jako hello celkový počet úlohy, odeslané násobí hodnotou hello cena za 1 000 transakcí (účtovány poměrnou částí jednotlivých transakcí). Náklady na rozhraní API BES rozhraní API produkčního prostředí výpočetní hodiny se počítá jako hello množství času potřebné pro každý řádek v vaše úlohy toorun násobí hodnotou hello celkový počet řádků ve vaší úloze násobí hodnotou hello celkový počet úloh násobí hodnotou hello cena za rozhraní API produkčního prostředí výpočetní hodinu. Pokud používáte hello kalkulačky Machine Learning, hello transakce měření představuje hello počet úloh, že máte v plánu toosubmit a pole čas jednotlivé transakce hello představuje hello kombinaci dobu, po kterou je potřeba pro všechny řádky v každé toorun úlohy.

Podívejme se na příklad nadlimitního využití úrovně Standard S1. Předpokládejme, že odešlete 100 úloh za den, z nichž se každá skládá z 500 řádků trvajících 0,72 sekundy. Měsíční náklady za nadlimitní využití potom dosáhnou výše (100 úloh za den = 3 100 úloh za měsíc * 0,50 USD / 1 000 transakcí API) 1,55 USD za transakce API produkčního prostředí a (500 řádků * 0,72 s * 3 100 úloh * 2 USD / hod) 620 USD za výpočetní čas API produkčního prostředí, takže celkem 621,55 USD.

### <a name="azure-machine-learning-classic-web-services"></a>Klasické webové služby Azure Machine Learning
**Je dál možné využívat průběžné platby (Pay As You Go)?**

Ano, klasické webové služby jsou ve službě Azure Machine Learning stále dostupné.  

### <a name="azure-machine-learning-free-and-standard-tier"></a>Azure Machine Learning – úrovně Free a Standard
**Co je součástí Azure Machine Learning bezplatnou úroveň hello?**

Azure Machine Learning bezplatnou úroveň Hello je určený tooprovide Podrobný úvod toohello Azure Machine Learning Studio. Je třeba zapnutý toosign účet Microsoft. Hello úroveň Free zahrnuje volný přístup tooone Azure Machine Learning Studio prostoru za [účtu Microsoft](https://www.microsoft.com/account/default.aspx). V této vrstvě můžete použít až too10 GB úložiště a zprovoznit modely jako pracovní rozhraní API. Úlohy úrovně Free nejsou předmětem smlouvy SLA a jsou určeny jenom pro vývoj a osobní užití. 

Pracovní prostory úroveň Free mít hello následující omezení:

* Úlohy nelze přistupovat k datům připojením tooan na místním serveru, který spouští SQL Server.
* Nejde nasadit nové webové služby využívajících Resource Manager.


**Co je součástí vrstvy Azure Machine Learning standardní hello a plány?**

Azure Machine Learning standardní vrstvě Hello je placené produkční verzi Azure Machine Learning Studio. Hello Azure Machine Learning Studio se fakturuje měsíční poplatek na za základ prostoru za měsíc a poměrné částečné měsíců. Hodiny experimentování se službou Azure Machine Learning Studio se účtují za výpočetní čas aktivního experimentování. Načaté hodiny se fakturují podle průběžného využívání.  

v závislosti na tom, zda je webová služba Classic nebo novou službu web (využívající Resource Manager) se fakturuje Hello služby Azure Machine Learning API.

Hello následující poplatky jsou agregován na pracovní prostor pro vaše předplatné.

* Předplatné pracovního prostoru Machine Learning: hello předplatné pracovního prostoru Machine Learning je měsíční poplatek poskytující pracovního prostoru Machine Learning Studio tooa přístup. předplatné Hello je požadovaná toorun experimenty v hello studio a tooutilize hello produkční rozhraní API.
* Hodiny experimentování se službou ml Studio: Toto monitorování agreguje všechny poplatky za výpočty, které nabíhají spuštěním experimentů v nástroji Machine Learning Studio a volání běžící rozhraní API produkčního v hello pracovní prostředí.
* Přístup k datům připojením tooan na místním serveru, který spouští SQL Server v modely pro vaše školení a vyhodnocování.
* U klasických webových služeb:
  * Výpočetní hodiny v rozhraní API produkčního prostředí: Měří poplatky za výpočetní kapacitu využitou webovými službami spuštěnými v produkčním prostředí.
  * Transakce v rozhraní API produkčního prostředí (v 1000s): Toto monitorování zahrnuje poplatky, které nabíhají za volání tooyour produkční webové služby.

Kromě hello předcházející poplatky, v případě hello založené na správci prostředků webové služby jsou poplatky agregované toohello vybraného plánu:

* Standard S1 nebo S2/S3 API plánování (jednotky): Toto měření představuje typ hello instance, který je vybraný založené na správci prostředků webové služby.
* Standard S1 nebo S2/Nadlimitní výpočetní hodiny API S3: Toto monitorování zahrnuje poplatky za výpočty, které nabíhání založené na správci prostředků webové služby, které spuštění v produkčním prostředí, až vyčerpáte počty hello zahrnuté v existující instance. Další využití Hello je účtován na hello overate kurz, který je spojen s S1 nebo S2/S3 plán vrstvy.
* Standard S1 nebo S2/S3 Nadlimitní události úrovně rozhraní API transakce (v 1,000s): Toto monitorování zahrnuje poplatky, které nabíhají na produkční tooyour volání vyčerpáte založené na správci prostředků webové služby po hello součástí počty existující instance. Hello další využití je výši hello overate kurz spojený s S1 nebo S2/S3 plán vrstvy.
* Zahrnuté množství výpočetních hodin API: S založené na správci prostředků webové služby, představuje toto měření hello zahrnuté množství výpočetních hodin API.
* Zahrnuté množství transakcí API (v 1,000s): na základě s Resource Managerem webové služby, toto měření představuje hello zahrnuté množství transakcí API.

**Jak se ve službě Azure Machine Learning zaregistrovat k úrovni Free?**

Stačí vám jen účet Microsoft. Přejděte příliš[Azure Machine Learning domovské](https://azure.microsoft.com/services/machine-learning/)a potom klikněte na **spustit nyní**. Jen co se přihlásíte pomocí účtu Microsoft, vytvoří se vám pracovní prostor v úrovni Free. Můžete spustit tooexplore a hned vytvořit experimenty Machine Learning.

**Jak se ve službě Azure Machine Learning zaregistrovat k úrovni Standard?**

Přístup tooan předplatného Azure toocreate musí mít první pracovní prostor standardní Machine Learning. Můžete si zaregistrovat 30denní bezplatné zkušební verze předplatného Azure a novější upgradu tooa placené předplatné Azure, nebo si můžete zakoupit placené předplatné Azure přímý. Potom můžete vytvořit pracovní prostor Machine Learning z portálu Microsoft Azure classic hello po získání předplatného toohello přístup. Zobrazení hello [podrobné pokyny](https://azure.microsoft.com/trial/get-started-machine-learning-b/).

Alternativně můžete můžete pozvat pomocí pracovního prostoru standardní Machine Learning prostoru vlastníka tooaccess hello vlastníka.

**Můžete zadat vlastní toouse účtu úložiště objektů Blob v Azure s úroveň Free hello?**

Ne, úroveň Standard hello je ekvivalentní toohello verzi hello Služba Machine Learning, která je k dispozici před hello, které byly zavedeny vrstev.

**Můžete nasadit Moje modelů strojového učení jako rozhraní API v úrovni Free hello?**

Ano, můžete zprovoznit modelů machine learning toostaging rozhraní API služby v rámci úroveň Free hello. tooput hello pracovní službu rozhraní API do produkčního prostředí a získat koncový bod produkční hello operationalized služby, je nutné použít hello úrovně Standard.

**Co je hello rozdíl mezi bezplatné zkušební verzi Azure a Azure Machine Learning bezplatnou úroveň?**

Hello [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/free/) nabízí služby kredity, které můžete použít v tooany Azure po jeden měsíc. Hello Azure Machine Learning bezplatnou úroveň nabízí průběžné přístup konkrétně tooAzure Machine Learning pro úlohy mimo produkční.

**Přesunutí experimentu z hello úroveň Free toohello úrovně Standard**

toocopy experimentů z hello úroveň Free toohello standardní vrstvy:

1. Přihlaste se tooAzure Machine Learning Studio a ujistěte se, že uvidíte hello volného prostoru a hello standardní prostoru v hello výběr pracovního prostoru v horním navigačním panelu hello.
2. Přepnout tooFree prostoru, pokud jste v prostoru standardní hello.
3. V zobrazení seznamu hello experiment vyberte experimentu, že by jako toocopy a pak klikněte na hello **kopie** příkazového tlačítka.
4. Vyberte standardní pracovní prostor hello z hello dialogového okna, které se otevře a potom klikněte na hello **kopie** tlačítko.
   Všechny hello přidružené datové sady, společně s hello experimentu do pracovního prostoru standardní hello se zkopírují trained model, atd.
5. Je třeba toorerun hello experiment a znovu publikovat webovou službu v prostoru standardní hello.

### <a name="studio-workspace"></a>Pracovní prostor Studia
**Fakturují se jednotlivé pracovní prostory zvlášť?**

Poplatky za pracovní prostory se rozepisují do jednotlivých měřených kategorií na jedné faktuře.

**Jaké konkrétní výpočetní prostředky se používají ke spouštění experimentů?**

Hello služby Machine Learning je víceklientské služby. Skutečné výpočetní prostředky, které se používají na hello back-end se liší a jsou optimalizované pro výkon a předvídatelnost.

### <a name="guest-access"></a>Přístup hosta
**Co je přístup hosta tooAzure Machine Learning Studio?**

Přístup hosta je omezený zkušební přístup. Umožňuje vytvářet a spouštět experimenty v Azure Machine Learning Studiu zdarma a bez ověřování. Relace typu Host jsou dočasnou (nelze uložit) a omezený tooeight hodin. Mezi další omezení patří chybějící podpora jazyků R a Python, chybějící rozhraní API přípravného prostředí a omezená velikost datové sady a úložiště. Pro srovnání uživatelé, kteří se rozhodnou toosign pomocí účtu Microsoft mají plný přístup úroveň Free toohello nástroje Machine Learning Studio, je popsáno dříve, který obsahuje trvalé prostoru a komplexnější funkce. toochoose prostředí vaší volné Machine Learning, klikněte na tlačítko **Začínáme** na [https://studio.azureml.net](https://studio.azureml.net)a potom vyberte **uhodnout přístup** nebo Přihlaste se pomocí služby Microsoft účet.

<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
