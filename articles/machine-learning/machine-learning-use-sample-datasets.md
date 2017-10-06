---
title: "aaaUse hello ukázkových datových sad v nástroji Machine Learning Studio | Microsoft Docs"
description: "Popis hello datové sady použité v ukázkových modelů, které jsou zahrnuty v nástroji Machine Learning Studio. Pro experimentů můžete použít tyto ukázkových datových sad."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 03a0b844-e8a7-4896-996f-d3c7a0db7a50
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: c7786478db82d40aaf27c37b3947ded5f042dd70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-sample-datasets-in-azure-machine-learning-studio"></a>Použít hello ukázkových datových sad v nástroji Azure Machine Learning Studio
[top]: #machine-learning-sample-datasets

Když vytvoříte nový pracovní prostor v Azure Machine Learning, několik ukázkových datových sad a experimenty jsou zahrnuté ve výchozím nastavení. Mnoho z těchto ukázkových datových sad jsou používány hello vzorové modely v hello [Azure Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/). Ostatní jsou k dispozici jako příklady různých typů dat, obvykle používané pro strojové učení.

Některé tyto datové sady jsou k dispozici v úložišti objektů Blob Azure. Pro tyto datové sady hello následující tabulka poskytuje přímý odkaz. Můžete použít tyto datové sady v experimentů pomocí hello [importovat Data] [ import-data] modulu.

Hello zbytek tyto ukázkových datových sad jsou k dispozici v pracovním prostoru v části **uložit datové sady** v hello modulu palety toohello nalevo od plátna experimentu hello při otevření nebo vytvořte nový experiment v nástroji Machine Learning Studio.
Všechny tyto datové sady můžete použít v vlastní experimentu přetažením tooyour plátno experimentu.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<table>

<tr>
  <th align=left>Název datové sady</th>
  <th align=left>Popis datové sady</th>
</tr>

<tr>
  <td valign=top>Datové sady pro dospělé úplné zjišťování příjem binární klasifikace</td>
  <td valign=top>
Podmnožinu hello roce 1994 za úplné zjišťování databázi, pomocí pracovní dospělí přes hello stáří 16 s indexem upravenou příjem > 100.<p> </p><b>Použití:</b> klasifikovat uživatelé, kteří používají demografie toopredict, zda uživatel mírou více než 50 tisíc a roce.<p> </p><b>Související Research:</b> Kohavi, R., Becker B., (1996). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Kódy letiště datové sady</td>
  <td valign=top>
Kódy letiště USA.<p> </p>Tato datová sada obsahuje jeden řádek pro každý letiště USA, poskytuje hello letiště identifikační číslo a název spolu s hello umístění Město a kraj.
  </td>
</tr>

<tr>
  <td valign=top>Automobilů price data (Raw)</td>
  <td valign=top>
Informace o automobilů podle značka a model, včetně hello ceníku, funkce, jako je počet hello válců a MPG a také pojištění rizikové skóre.<p> </p>Hello rizikové skóre původně souvisí s automaticky ceny a pak upravit pro skutečné rizik ve správě tento proces se označuje jako symboling tooactuaries. Hodnota + 3 označuje, že je automaticky hello rizikové a hodnota -3 je to pravděpodobně bezpečné.<p> </p><b>Použití:</b> předpovědi hello rizikové skóre funkce regrese nebo multivariate klasifikací. <p> </p><b>Související Research:</b> Schlimmer, J.C. (1987). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Kolo pronájem UCI datové sady</td>
  <td valign=top>
Pronájem kolo UCI datovou sadu, která je založená na reálná data z velké Bikeshare společnosti, která udržuje síť kolo pronájem v Washington DC.<p> </p>Hello datová sada obsahuje jeden řádek pro každou hodinu každý den v 2011 a 2012, celkem 17,379 řádků. každou hodinu kolo pronájem Hello rozsah je 1 too977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Obrázek RGB faktury brány</td>
  <td valign=top>
Veřejně dostupné bitové kopie souboru převést tooCSV data.<p> </p>Hello kódu pro převod hello obrázku je uvedený v hello <strong>barvu kvantizační použití clusteringu K-Means</strong> stránku s podrobnostmi modelu.
  </td>
</tr>

<tr>
  <td valign=top>Data odběru krve</td>
  <td valign=top>
Podmnožinu dat z databáze dárce krve hello hello transfuzním Service Center Hsin Chu města, Tchaj-wan.<p> </p>Dárce data zahrnují hello měsíců od poslední odběru) a četnost nebo hello celkový počet odběrů, doba od poslední odběru a množství krve věnován.<p> </p><b>Použití:</b> hello cílem je toopredict prostřednictvím klasifikace, zda hello dárce věnován krve v březnu 2007, kde 1 znamená dárce během období hello cíl a 0 bez dárce. <p> </p><b>Související Research:</b> Já, systémem, (2008). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače <p> </p>Já, I-vývoj: Cheng, Yang, král-Jang a si toho, jde Tao "zjišťování znalostní báze na modelu do režimu omezené Funkčnosti pomocí Bernoulliho pořadí" Expert systémy s aplikacemi, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Kniha recenze z Amazon</td>
  <td valign=top>
Recenze knih v Amazon, provedenou univerzity Velká Británie výzkumných pracovníků z webu amazon.com hello (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">postojích</a>). Hello research dokument, najdete v části "biografie, Bollywood, výložníku polí a Blenders: přizpůsobení domény pro klasifikaci postojích" Jan Blitzer, označit Dredze a Fernando Pereira; Přidružení výpočetní Linguistics (ACL), 2007.<p> </p>původní datové sady Hello má 975 tisíc recenze s pořadím 1, 2, 3, 4 nebo 5. Hello recenze napsaných v angličtině, jsou z hello časové období 1997-2007. Tato datová sada byl vzorkovat nižší too10K recenze.
  </td>
</tr>

<tr>
  <td valign=top>Data rakoviny mateřské</td>
  <td valign=top>
Jeden ze tří související rakoviny datové sady poskytované hello Institute radiology, který se zobrazí často v machine learning dokumentace. Kombinuje diagnostické informace a funkce z laboratorní analýzy přibližně 300 tkáňových vzorků.<p> </p><b>Použití:</b> klasifikace hello typ rakoviny, na základě 9 atributů, z nichž některé jsou lineární a některé jsou kategorií. <p> </p><b>Související Research:</b> Wohlberg, čísel, ulici, W.N. a Mangasarian, O.L. (1995). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Funkce rakoviny mateřské <td valign=top>
Hello datová sada obsahuje informace o 102 tisíc podezřelé oblasti (kandidáty) X-ray bitových kopií, každý popsaného 117 funkce. Funkce Hello jsou proprietární a jejich význam není zjištěné při hello datovou sadu creators (Siemens zdravotní péče). 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Informace o rakovině mateřské</td>
  <td valign=top>
Hello datová sada obsahuje další informace pro každou podezřelou oblast X-ray bitové kopie. Každý příklad poskytuje informace (například štítku, pacienta ID, souřadnice oprava relativní toohello celého obrázku) o hello příslušné číslo řádku v datové sadě funkcí rakoviny mateřské hello. Každý pacienta má počet příklady. Pro pacientů, kteří mají rakoviny je několik příkladů kladné a některé jsou záporné. Všechny příklady jsou pro pacientů, kteří nemají rakoviny, záporné. Datová sada Hello má 102 tisíc příklady. tendenční datovou sadu Hello 0,6 % hello body je kladné, hello rest záporné. Datová sada Hello byla provedena dostupné Siemens zdravotní péče.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>Popisky Appetency CRM sdílet</td>
  <td valign=top>
Štítky z hello KDD Cup 2009 zákazníka vztah předpovědi výzvy (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>Popisky změn CRM sdílet</td>
  <td valign=top>
Štítky z hello KDD Cup 2009 zákazníka vztah předpovědi výzvy (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>Datová sada CRM sdílet</td>
  <td valign=top>
Tato data pocházejí z hello KDD Cup 2009 zákazníka vztah předpovědi výzvy (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>Hello datová sada obsahuje 50 tisíc zákazníků z hello francouzština telekomunikace společnosti oranžová. Každý zákazník má 230 anonymizovaná funkce 190, které jsou číselné a 40 jsou kategorií. Funkce Hello jsou velmi zhuštěných.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>Popisky Upselling CRM sdílet</td>
  <td valign=top>
Štítky z hello KDD Cup 2009 zákazníka vztah předpovědi výzvy (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Data účinnosti regrese energie</td>
  <td valign=top>
Kolekce simulované energie profily založené na 12 jiné budovy tvarů. Hello budovy jsou rozlišené pomocí 8 funkce, jako je použitém oblasti hello použitém oblasti distribuce a orientace.<p> </p><b>Použití:</b> použít regrese nebo klasifikaci toopredict hello efektivity hodnocení na základě jako jednu ze dvou skutečných hodnot odpovědi. Pro více třída klasifikace se zaokrouhlí hello odpovědi proměnné toohello nejbližší celé číslo. <p> </p><b>Související Research:</b> Xifara A. & Tsanas A. (2012). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Letu zpozdí dat</td>
  <td valign=top>
Osobní letu na běhu výkonu dat získaných z hello kolekce dat TranStats hello USA Ministerstvo dopravy (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">na čas</a>).<p> </p>Datová sada Hello popisuje hello časové období duben – říjen 2013. Před nahráním tooAzure Machine Learning Studio, datová sada hello zpracování následujícím způsobem:<ul><li>Datová sada Hello byl filtrované toocover pouze hello 70 nejvytíženější letištích v hello kontinentální USA</li><li>Zrušené lety měla označené jako zpožděn víc než 15 minut</li><li>Odkloněných lety byly vyfiltrovány.</li><li>Hello následující sloupce nebyly vybrány: rok, měsíc, DayofMonth, DayOfWeek, operátora, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, zrušeno</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Letu na časový výkon (Raw)</td>
  <td valign=top>
Záznamy letadle letu doručení a odchylky v USA z října 2011.<p> </p><b>Použití:</b> předpovědi zpoždění letů. <p> </p><b>Související Research:</b> z USA oddělení Transport <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Data o lesních požárech</td>
  <td valign=top>
Obsahuje data, počasí, například teploty a vlhkosti indexy a rychlosti větru z oblasti severovýchodním Portugalsku, v kombinaci s záznamy aktivuje se v doménové struktuře.<p> </p><b>Použití:</b> Toto je úloha obtížné regrese, kde je cílem hello toopredict hello zapsaný oblasti aktivuje se v doménové struktuře. <p> </p><b>Související Research:</b> Cortez, P. & Morais A. (2008). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače <p> </p>[Cortez a Morais 2007] P. Cortez a A. Morais. Postup dolování dat tooPredict lesních požárech pomocí meteorologických Data. V J. Neves, M. F. Santos a Edit.: Machado J., nové trendy v umělé inteligence jednání hello 13. EPIA 2007 - portugalština konference na umělé Intelligence prosinec, 523-Guimarães, Portugalsko s. 512, 2007. APPIA, ISBN 13 978-989-95618-0-9. K dispozici na: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Němčina platební karty UCI datové sady</td>
  <td valign=top>
Hello UCI Statlog (němčina platební karty) datovou sadu (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + němčina + platební + Data</a>), pomocí souboru german.data hello.<p> </p>Datová sada Hello klasifikuje osoby, popsaného sadu atributů, jako nízkou nebo vysokou platební rizika. Každý příklad představuje osoby. Existují 20 funkcí číselné i kategorií a binární popisek (hello úvěrové riziko hodnota). Vysoká platební riziko položky mají popisek = 2, nízkou úvěrové riziko položky mají popisek = 1. misclassifying příklad nízké riziko tak vysoké náklady Hello je 1, zatímco hello náklady misclassifying vysoce rizikové příklad jako nízkou je 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>Názvy filmů IMDB</td>
  <td valign=top>
Hello datová sada obsahuje informace o filmy, které byly hodnocení v Twitter tweetů: IMDB ID film, název film, genre a produkční roku. V datové sadě hello existují filmy 17 kB. Datová sada Hello byla zavedena v dokumentu hello "S. Dooms, T. De Pessemier a L. Martens. MovieTweetings: film hodnocení datovou sadu se shromažďují ze služby Twitter. Dílny na Crowdsourcingový a lidské výpočtů systémů doporučené CrowdRec na RecSys 2013."
  </td>
</tr>

<tr>
  <td valign=top>Data Iris dva – třída</td>
  <td valign=top>
Toto je možná hello nejznámější databáze toobe najít v hello vzor rozpoznávání dokumentace. Datová sada Hello je poměrně malý, obsahující 50 příklady každý Okvětní lístek měření z tři typy iris.<p> </p><b>Použití:</b> předpovědi hello iris typu z hello měření.  <p> </p><b>Související Research:</b> Fisherovy R.A. (1988). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Film Tweetů</td>
  <td valign=top>
Hello datová sada rozšířenou verzi hello film Tweetings datovou sadu. Datová sada Hello má 170K hodnocení filmy, které se extrahují z dobře strukturovaných tweetů na Twitteru. Každá instance představuje tweet a je řazené kolekce členů: ID uživatele, IMDB film ID, hodnocení, časové razítko, počet oblíbených položek pro tento tweet a počet retweets tento tweet. Datová sada Hello byla k dispozici A. uvedená, S. Dooms, B. Loni a D. Tikk pro doporučené systémy výzvy 2014.
  </td>
</tr>

<tr>
  <td valign=top>Data o spotřebě paliv u různých automobilů</td>
  <td valign=top>
Tato datová sada mírně upravenou verzi datové sady hello poskytované hello knihovně StatLib univerzity Carnegie Mellon. Datová sada Hello byl použit v hello 1983 American statistické budeme přidružení.<p> </p>Hello data uvádí spotřeby paliva u různých automobilů v miles za spotřeby, společně s informacemi o takové hello počtu válců, modul posouvání, koňských sil, celkový počet váhy a akcelerace.<p> </p><b>Použití:</b> předpovědi paliva na základě 3 s více hodnotami diskrétních atributů a 5 souvislé atributy. <p> </p><b>Související Research:</b> StatLib, Carnegie TruSecure, (1993). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače </td>
</tr>

<tr>
  <td valign=top>Datová sada Pima indiánů Diabetes binární klasifikace</td>
  <td valign=top>
Podmnožinu dat z databáze National Institute Diabetes a trávícího a ledviny nákaz hello. Datová sada Hello se filtrované toofocus na ženského pacienty Indie dědictví Pima. Hello data zahrnují lékařské data, jako jsou glukosy a inulinový úrovně, jakož i lifestyle faktory.<p> </p><b>Použití:</b> předpovědět, zda text hello subjektu má diabetes (binární klasifikace). <p> </p><b>Související Research:</b> Sigillito, V. (1990). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače </td>
</tr>

<tr>
  <td valign=top>Data zákazníků restaurace</td>
  <td valign=top>
Sada metadata o zákazníků, včetně demografické údaje a předvolby.<p> </p><b>Použití:</b> použít tuto datovou sadu, v kombinaci s hello další dvě restaurace datové sady, tootrain a testování doporučené systému. <p> </p><b>Související Research:</b> Bache, K. a Lichman, M. (2013). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače.
  </td>
</tr>

<tr>
  <td valign=top>Data funkce restaurace</td>
  <td valign=top>
Sada metadata o restaurace a jejich funkce, jako je například typ jídlo, nabízet stylu a umístění.<p> </p><b>Použití:</b> použít tuto datovou sadu, v kombinaci s hello další dvě restaurace datové sady, tootrain a testování doporučené systému. <p> </p><b>Související Research:</b> Bache, K. a Lichman, M. (2013). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače.
  </td>
</tr>

<tr>
  <td valign=top>Restaurace hodnocení</td>
  <td valign=top>
Obsahuje hodnocení poskytují uživatelům toorestaurants na škále od 0 too2.<p> </p><b>Použití:</b> použít tuto datovou sadu, v kombinaci s hello další dvě restaurace datové sady, tootrain a testování doporučené systému. <p> </p><b>Související Research:</b> Bache, K. a Lichman, M. (2013). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače.
  </td>
</tr>

<tr>
  <td valign=top>Ocelové datovou sadu Annealing více – třída</td>
  <td valign=top>
Tato datová sada obsahuje řadu záznamy ze oceli žíhání zkušební verze se hello fyzické atributy (šířka, tloušťka, typ (smyčka, list atd.) hello výsledná oceli typy.<p> </p><b>Použití:</b> dva atributy číselné třídy; tvrdosti nebo sílu předpovědi. Může také analyzovat korelací mezi atributy.<p> </p>Ocelové tříd podle sady standard, definované SAE a jiných organizací. Hledáte konkrétní "třída" (proměnné třídy hello) a chcete toounderstand hello hodnoty. <p> </p><b>Související Research:</b> šterlinků, D. & Buntine, dokončeno (NA). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity kalifornské, školní informace a vědecké účely počítače <p> </p>Třídy užitečné Průvodce toosteel naleznete zde: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Telescope dat</td>
  <td valign=top>
Záznamy vysoké energii gama částice shluky společně s hluku na pozadí, obě simulated pomocí typu Monte Carlo procesu.<p> </p>Hello záměr hello simulace byla tooimprove hello přesnost na základě základů důsledky Cherenkov gama dalekohledy, pomocí statistické metody toodifferentiate mezi požadované hello signál (Cherenkov záření sprchy) a (hadronic hluku na pozadí Sprchy iniciovaná cosmic paprsky ve hello horní prostředí).<p> </p>Hello data nebyla předem zpracovaných toocreate podlouhlého clusteru s dlouhou osy hello je zaměřen na hello fotoaparát center. Vlastnosti Hello tento třemi tečkami (často říká Hillas parametry) jsou mezi parametry hello bitové kopie, které lze použít pro diskriminace.<p> </p><b>Použití:</b> předpovědět, zda obrázek sprcha reprezentuje šumu signál nebo na pozadí.<p> </p><b>Poznámky:</b> jednoduché klasifikace přesnost není smysl pro tato data od klasifikace pozadí události, jako je signál zhoršení než klasifikace událost signálu jako pozadí. Pro porovnání různých třídění je třeba použít hello ROC grafu. Hello pravděpodobnosti přijetí události pozadí jako signálu musí být níže jednu z následujících prahové hodnoty hello: 0,01, 0,02, hodnotu 0,05, 0,1 nebo 0,2.<p> </p>Mějte také na paměti, že je podcenila hello počet událostí pozadí (pro hadronic sprchy h), zatímco v reálného měření hello h nebo šumu třída reprezentuje hello většiny událostí. <p> </p><b>Související Research:</b> Bock, R.K. (1995). UCI strojového učení úložiště <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, certifikační Autorita: Univerzity z kalifornské, školní informace </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Datová sada počasí</td>
  <td valign=top>
Hodinové hodnoty na základě krajině počasí s NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 too201310">sloučit data z 201304 too201310</a>).<p> </p>data o počasí Hello popisuje připomínkám z letiště počasí stanice, přičemž zahrnuje hello časové období duben – říjen 2013. Před nahráním tooAzure Machine Learning Studio, datová sada hello zpracování následujícím způsobem:<ul><li>ID stanice počasí byly namapované toocorresponding letiště ID</li><li>Stanice počasí nejsou přidružené 70 letiště nejvytíženější hello byly vyfiltrovány.</li><li>sloupce s datem Hello byl rozdělen do samostatných sloupců rok, měsíc a den</li><li>Hello následující sloupce nebyly vybrány: AirportID, rok, měsíc, den, čas, časové pásmo, SkyCondition, viditelnost, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, Rychlost, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, výškoměru</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Web Wikipedia SP 500 datové sady</td>
  <td valign=top>
Data je odvozený od Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) založené na články, každý S & P 500 společnosti, uložené jako XML data.<p> </p>Před nahráním tooAzure Machine Learning Studio, datová sada hello zpracování následujícím způsobem:<ul><li>Extrahování obsahu text pro každou konkrétní společnosti</li><li>Odebrat wiki formátování</li><li>Odeberte jiné než alfanumerické znaky</li><li>Převést všechny toolowercase textu</li><li>Kategorie známé společnosti byly přidány.</li></ul><p> </p>Všimněte si, že pro některé společnosti článek nelze nalézt, takže hello počet záznamů je menší než 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
Hello datová sada obsahuje zákaznická data a údaje o jejich odpovědi tooa přímé poštovní kampaně. Každý řádek představuje zákazníka. Hello datová sada obsahuje 9 funkce o demografie uživatelů a po chování a 3 popisek sloupce (navštěvovat, převodu a tráví).  Najdete je na binární sloupec, který označuje, že je zákazník navštívené po hello marketingovou kampaň, převod označuje, že zákazník něco zakoupili a náklady na hello množství, které byl stráven.  Datová sada Hello byla provedena dostupné kevina Hillstrom pro MineThatData e-mailu Analytics a Data Mining výzvu.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Funkce testovací příklady v datové sadě zprávy RCV1 V2 Reuters hello. Datová sada Hello má články s příspěvky 781 tisíc spolu s jejich ID (první sloupec datovou sadu hello). Každý článek tokenizovaného stopworded a nasadí. Datová sada Hello byla provedena dostupné David. D. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Funkce školení příklady v datové sadě zprávy RCV1 V2 Reuters hello. Datová sada Hello má články s příspěvky 23 tisíc spolu s jejich ID (první sloupec datovou sadu hello). Každý článek tokenizovaného stopworded a nasadí. Datová sada Hello byla provedena dostupné David. D. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
Datové sady z hello KDD Cup 1999 znalostní báze zjišťování a nástrojů soutěže dolování dat (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>Datová sada Hello byla stažena a uložená v úložišti objektů Blob v Azure (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) a zahrnuje jak trénování a testování datové sady. Datová sada školení Hello má přibližně 126 tisíc řádků a 43 sloupců, včetně popisků hello. Tři sloupce jsou součástí hello popisek informace a 40 sloupce, který se skládá z funkce číselný a řetězec, kategorií, jsou k dispozici pro trénování modelu hello. Hello testovací data mají přibližně 22,5 K testování příklady hello stejné 43 sloupce jako hello Cvičná data.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1 v2.topics.qrels.csv</a></td>
  <td valign=top>
Téma přiřazení pro články s příspěvky v datové sadě zprávy RCV1 V2 Reuters hello. Aktuální článek lze přiřadit tooseveral témata. Každý řádek Hello formát je "&lt;název tématu&gt; &lt;id dokumentu&gt; 1". Hello datová sada obsahuje 2.6M tématu přiřazení. Datová sada Hello byla provedena dostupné David. D. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Tato data pocházejí z hello výzvy vyhodnocení výkonu KDD Cup 2010 Student (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">vyhodnocení výkonu student</a>). Hello používaná data je hello Algebra_2008_2009 trénovací sady (Stamper, J., Niculescu-Mizil, S. A. Ritter, Gordon, G.J. & Koedinger, k. r. (2010). Algebra I 2008-2009. Výzvy datové sady z KDD Cup 2010 výukových Data Mining výzvu. Najít v <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> nebo <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>Datová sada Hello byla stažena a uložená v úložišti objektů Blob v Azure (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) a obsahuje soubory protokolů z student soukromé vyučování/doučování systému. Hello zadaná funkce zahrnují ID problému a jeho stručný popis student ID, časové razítko, a kolik se pokusí provést před řešení problému hello v hello student hello pravým způsobem. původní datové sady Hello má záznamy 8,9 M; Tato datová sada byl vzorkovat nižší toohello prvních 100 tisíc řádků. Datová sada Hello má 23 tabulátorem sloupce různé typy: číselné literály, kategorií a časové razítko.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
