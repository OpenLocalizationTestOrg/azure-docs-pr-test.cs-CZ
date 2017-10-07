---
title: "aaaGet začít s Azure Search v Javě | Microsoft Docs"
description: "Jak toobuild hostovaného cloudu vyhledat aplikaci v Azure pomocí programovacího jazyka Java."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 8b4df3c9-3ae5-4e3a-b4bb-74b516a91c8e
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 07/14/2016
ms.author: evboyle
ms.openlocfilehash: 5476a2103f3b60fe6ec78ff3d3fdba9fcff55c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-java"></a>Začínáme s Azure Search v Javě
> [!div class="op_single_selector"]
> * [Azure Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Zjistěte, jak toobuild vlastní Java Hledat aplikace, která používá službu Azure Search své zkušenosti vyhledávání. Tento kurz používá hello [rozhraní REST API služby Azure Search](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objekty a operace, na které se použijí v tomto cvičení.

toorun této ukázky musíte mít službu Azure Search, které se můžete zaregistrovat si v hello [portálu Azure](https://portal.azure.com). V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) podrobné pokyny.

Můžeme použít hello následující software toobuild a testování tohoto příkladu:

* [Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar) Být jisti verzi EE toodownload hello. Jeden z kroků ověření hello vyžaduje funkci, která se nachází pouze v této edici.
* [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-hello-data"></a>O datech hello
Tato ukázková aplikace používá data z hello [Spojených států Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), která jsou filtrovaná na velikost datové sady hello státu Rhode Island tooreduce hello. Použijeme tato data toobuild vyhledávací aplikaci, která najde významné budovy, například nemocnice a školy, a geologické prvky, jako jsou datové proudy, jezera a vrcholy.

V této aplikaci hello **SearchServlet.java** program sestaví a zatížením hello index pomocí [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstrukce, načítání hello filtrovat sadu dat USGS z veřejné databáze Azure SQL Database. Předdefinované přihlašovací údaje a připojení informace toohello online zdroji dat jsou uvedené v kódu programu hello. Z hlediska přístupu k datům není potřeba žádná další konfigurace.

> [!NOTE]
> Jsme použili filtr na tuto datovou sadu toostay pod hello 10 000 dokumentů limit hello volné cenová úroveň. Pokud používáte úroveň standard hello, toto omezení se nevztahuje a můžete upravit tento kód toouse větší datovou sadu. Podrobnosti týkající se kapacity u jednotlivých cenových úrovní najdete v tématu [Omezení](search-limits-quotas-capacity.md).
> 
> 

## <a name="about-hello-program-files"></a>O souborech programu hello
Hello následující seznam popisuje hello soubory, které jsou relevantní toothis ukázka.

* Search.jsp: Poskytuje uživatelské rozhraní hello
* SearchServlet.java: Poskytuje metody (podobně jako tooa řadič v MVC)
* SearchServiceClient.java: Zpracovává požadavky HTTP.
* SearchServiceHelper.java: Pomocná třída, která poskytuje statické metody.
* Document.Java: Poskytuje datový model hello
* config.Properties: Nastavuje adresu URL služby Search hello a klíč api-key
* Pom.xml: Závislost Maven

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a>Nalezení názvu služby hello a klíč api-key služby Azure Search
Všechna volání rozhraní REST API Azure Search vyžaduje, aby vám poskytla hello adresa URL služby a klíč rozhraní api. 

1. Přihlaste se toohello [portálu Azure](https://portal.azure.com).
2. Hello přechod panelu klikněte na **služby vyhledávání** toolist všechny hello služby Azure Search zřízených pro předplatné.
3. Vyberte službu hello chcete toouse.
4. Na hello panelu služby uvidíte dlaždice se základními informacemi a ikonu hello klíče pro přístup ke klíčům správce hello.
   
      ![][3]
5. Zkopírujte adresu URL hello služby a klíč správce. Budete je potřebovat později, až je budete přidávat toohello **config.properties** souboru.

## <a name="download-hello-sample-files"></a>Stažení ukázkových souborů hello
1. Přejděte příliš[AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) na Githubu.
2. Klikněte na tlačítko **stáhnout ZIP**, uložte toodisk soubor .zip hello a pak extrahujte všechny soubory hello obsahuje. Zvažte extrahování hello souborů do vašeho pracovního prostoru toomake Java snazší projekt toofind hello později.
3. Hello ukázkové soubory jsou jen pro čtení. Klikněte pravým tlačítkem na složku vlastnosti a zrušte hello atribut jen pro čtení.

Všechny následné úpravy souborů a spouštěné příkazy se budou provádět na souborech v této složce.  

## <a name="import-project"></a>Import projektu
1. V prostředí Eclipse zvolte **Soubor** > **Import** > **Obecné** > **Existující projekty do pracovního prostoru**.
   
    ![][4]
2. V **vybrat kořenový adresář**, procházet toohello složky obsahující ukázkové soubory. Vyberte složku hello, která obsahuje složku .project hello. Hello projekt by se měla objevit v hello **projekty** seznamu vybranou položku.
   
    ![][12]
3. Klikněte na **Dokončit**.
4. Použití **Project Exploreru** tooview a úpravy souborů hello. Pokud již není otevřený, klikněte na tlačítko **okno** > **zobrazit zobrazení** > **Project Exploreru** nebo pomocí zástupce tooopen hello ho.

## <a name="configure-hello-service-url-and-api-key"></a>Konfigurace hello adresa URL služby a klíč api-key
1. V **Project Exploreru**, dvakrát klikněte na **config.properties** nastavení konfigurace hello tooedit obsahující hello název serveru a klíč api-key.
2. Odkazovat toohello postup uvedený dříve v tomto článku, kde jste našli hello adresa URL služby a klíč api-key v hello [portálu Azure](https://portal.azure.com), tooget hello hodnoty, které nyní zadáte do **config.properties**.
3. V **config.properties**, nahraďte "Api Key" hello api-key pro vaši službu. V dalším kroku název služby (hello první komponenta adresy URL http://servicename.search.windows.net hello) nahradí "název služby" v hello stejného souboru.
   
    ![][5]

## <a name="configure-hello-project-build-and-runtime-environments"></a>Konfigurovat hello projektu, buildu a běhového prostředí
1. V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na projekt hello > **vlastnosti** > **omezující vlastnosti projektu**.
2. Vyberte **Dynamic Web Module**, **Java** a **JavaScript**.
   
    ![][6]
3. Klikněte na **Použít**.
4. Vyberte **Okno** > **Předvolby** > **Server** > **Běhová prostředí** > **Přidat**.
5. Apache rozbalte a vyberte server Apache Tomcat hello, které jste dříve nainstalovali verzi hello. V našem systému jsme nainstalovali verzi 8.
   
    ![][7]
6. Na další stránku hello zadejte instalační adresář Tomcat hello. V počítači s Windows to bude pravděpodobně C:\Program Files\Apache Software Foundation\Tomcat *verze*.
7. Klikněte na **Dokončit**.
8. Vyberte **Okno** > **Předvolby** > **Java** > **Nainstalovaná prostředí JRE** > **Přidat**.
9. V okně **Přidat prostředí JRE**, vyberte **Standardní virtuální počítač**.
10. Klikněte na **Další**.
11. V definici prostředí JRE v kořenovém adresáři JRE klikněte na **Adresář**.
12. Přejděte příliš**Program Files** > **Java** a vyberte hello JDK jste dříve nainstalovali. Je důležité tooselect hello JDK jako prostředí JRE hello.
13. V okně nainstalovaná prostředí JRE, zvolte hello **JDK**. Vaše nastavení by měla vypadat podobně jako toohello následující snímek obrazovky.
    
    ![][9]
14. Volitelně vyberte **okno** > **webový prohlížeč** > **Internet Explorer** aplikace hello tooopen v okně externího prohlížeče. Použití externího prohlížeče poskytuje lepší uživatelské prostředí webové aplikace.
    
    ![][8]

Teď jste dokončili úlohy konfigurace hello. V dalším kroku sestavíte a spusťte projekt hello.

## <a name="build-hello-project"></a>Sestavení projektu hello
1. V prohlížeči projektu klikněte pravým tlačítkem na název projektu hello a zvolte **spustit jako** > **sestavení Maven...**  tooconfigure hello projektu.
   
    ![][10]
2. V okně Upravit konfiguraci v části Cíle zadejte „clean install“ a pak klikněte na **Spustit**.

Stavové zprávy se výstup okna konzoly toohello. Měli byste vidět naznačuje projektu hello ÚSPĚŠNOSTI sestavení vytvořené bez chyb.

## <a name="run-hello-app"></a>Spuštění aplikace hello
V tomto posledním kroku spustíte hello aplikace v prostředí runtime místní server.

Pokud jste v prostředí Eclipse ještě neurčili běhové prostředí serveru, budete potřebovat toodo nyní.

1. V Prohlížeči projektu rozbalte položku **WebContent**.
2. Klikněte pravým tlačítkem na **Search.jsp** > **Spustit jako** > **Spustit na serveru**. Vyberte server Apache Tomcat hello a pak klikněte na tlačítko **spustit**.

> [!TIP]
> Pokud jste použili toostore prostoru jiné než výchozí projekt, budete potřebovat toomodify **konfigurace spustit** toopoint toohello projektu umístění tooavoid chybě při spuštění serveru. V Prohlížeči projektu klikněte pravým tlačítkem na **Search.jsp** > **Spustit jako** > **Konfigurace spuštění**. Vyberte server Apache Tomcat hello. Klikněte na **Argumenty**. Klikněte na tlačítko **prostoru** nebo **systém souborů** tooset hello složku obsahující projekt hello.
> 
> 

Při spuštění aplikace hello, měli byste vidět okno prohlížeče, poskytuje vyhledávací pole pro zadání termínů.

Počkejte přibližně jednu minutu před kliknutím na tlačítko **vyhledávání** toogive hello služby čas toocreate a zatížení hello index. Pokud se zobrazí chyba HTTP 404, bude třeba jen toowait trochu déle, než to zkusíte znovu.

## <a name="search-on-usgs-data"></a>Hledání v datech USGS
Hello sada dat USGS obsahuje záznamy, které jsou relevantní toohello státu Rhode Island. Pokud kliknete na tlačítko **vyhledávání** na prázdného vyhledávacího pole, obdržíte prvních 50 položek hello, což je výchozí hello.

Zadat hledaný termín získáte hello vyhledávacího webu něco toogo na. Zkuste zadat místní název. "Roger Williams" byla hello prvním guvernérem státu Rhode Island. Je po něm pojmenovaná celá řada parků, budov a škol.

![][11]

Může taky zkusit kterýkoli z těchto výrazů:

* Pawtucket
* Pembroke
* goose +cape

## <a name="next-steps"></a>Další kroky
Toto je hello první kurz služby Azure Search založený na Javě a hello sadu dat USGS. V čase budete rozšiřujeme tento kurz toodemonstrate dalších vyhledávacích funkcí, které můžete chtít toouse ve vlastních řešeních.

Pokud již máte základní ve službě Azure Search, můžete tuto ukázku jako odrazový můstek pro další experimentování, případně rozšířit hello [stránky hledání](search-pagination-page-layout.md), nebo implementaci [Fasetové navigace](search-faceted-navigation.md). Můžete taky zdokonalit stránku výsledků hledání hello přidáním počtů a dávkování dokumenty tak, aby hello výsledky procházet po stránkách.

Nové tooAzure hledání? Doporučujeme vyzkoušet ostatní kurzy toodevelop představu o co můžete vytvořit. Navštivte naše [stránky dokumentace, která](https://azure.microsoft.com/documentation/services/search/) toofind více prostředků. Můžete také zobrazit hello odkazy v našem [seznamu videí a kurzů](search-video-demo-tutorial-list.md) tooaccess Další informace.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
