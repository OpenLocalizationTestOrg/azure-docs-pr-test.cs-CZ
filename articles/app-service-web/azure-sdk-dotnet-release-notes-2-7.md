---
title: "Poznámky k verzi sady SDK pro .NET 2.7 a .NET 2.7.1 aaaAzure"
description: "Poznámky k verzi sady Azure SDK pro .NET 2.7 a .NET 2.7.1"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: 877d070a-9bd5-49b3-8fac-6bb5f65c3554
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 8ec72b0f18702e6d811f0cbe4790685be7881d96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Poznámky k verzi sady Azure SDK pro .NET 2.7 a .NET 2.7.1
## <a name="overview"></a>Přehled
Tento dokument obsahuje hello poznámky k verzi pro hello Azure SDK pro .NET 2.7 verzi. 

dokument Hello také obsahovat hello poznámky k verzi pro hello Azure SDK pro .NET 2.7.1 verzi.

Azure SDK 2.7 je podporována pouze v sadě Visual Studio 2015 a Visual Studio 2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) je hello poslední nepodporují SDK pro sadu Visual Studio 2012.

Podrobné informace o tomto vydání najdete v tématu [post oznámení Azure SDK 2.7](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) a [post oznámení Azure SDK 2.7.1](http://go.microsoft.com/fwlink/?LinkId=623850).

## <a name="azure-sdk-for-net-27"></a>Azure SDK pro .NET 2.7
### <a name="sign-in-improvements-for-visual-studio-2015"></a>Přihlaste se vylepšení pro Visual Studio 2015
2.7 Azure SDK pro Visual Studio 2015 podporuje hello nové funkce správy identit v sadě Visual Studio 2015.  To zahrnuje podporu pro účty přístupu k Azure prostřednictvím řízení přístupu na základě rolí, poskytovatele cloudových řešení, DreamSpark a dalších typů účet a předplatné.

Hello přihlášení vylepšení součástí Azure SDK 2.7 jsou pouze k dispozici v sadě Visual Studio 2015. Podpora pro Visual Studio 2013 je součástí sady Azure SDK 2.7.1.

### <a name="mobile-sdk"></a>Mobilní SDK
Aktualizovat **Mobile Apps** šablony tooreflect nejnovější [balíček NuGet](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) a instalační program procesu.

### <a name="service-bus"></a>Service Bus
Obecné oprav chyb a vylepšení. Podrobnosti o aktualizacemi a funkcemi, naleznete toohello poznámky k verzi nástroje hello nejnovější [Service Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

### <a name="hdinsight-tools"></a>Nástroje HDInsight
V této verzi hello byly provedeny následující aktualizace. Tyto aktualizace jsou ve verzi preview. Další informace najdete v tématu [tomto blogu](http://go.microsoft.com/fwlink/?LinkId=619108).

* Hive grafy pro Hive na úlohách Tez
* Plná podpora Hive DML IntelliSense
* Podpora šablony pig
* Storm šablon pro služby Azure

#### <a name="breaking-changes"></a>Nejnovější změny
* Původní **Storm** projektu, musí být upgradován při použití této verzi nástroje hello. Další informace najdete v tématu [tomto blogu](http://go.microsoft.com/fwlink/?LinkId=619108).
* Visual Studio Web Express již není podporována. Další informace najdete v tématu [tomto blogu](http://go.microsoft.com/fwlink/?LinkId=619108).

### <a name="azure-app-service-tools"></a>Nástroje služby Azure App Service
V této verzi hello byly provedeny následující aktualizace rozšíření nástrojů tooWeb. Další informace najdete v části [to](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blogu. 

* Podpora pro DreamSpark účty přidat
* Úplné změňte tooAzure provedené toosupport hello nové Azure prostředků rozhraní API pro správu nástroje
* Příliš přidána podpora pro Azure App Service[Průzkumníku cloudu](#cloud_explorer)

#### <a name="known-issues"></a>Známé problémy
Uzly slotu nasazení aplikace Web nezobrazí v uzlu sloty hello v Průzkumníku serveru a uzly podřízené slotu nasazení webové aplikace se nenačtou v Průzkumníku cloudu. Tento problém byl vyřešen a připravit pro další verze sady SDK hello. 

### <a name="cloud_explorer"></a>Průzkumník cloudu pro Visual Studio 2015
Azure SDK 2.7 obsahuje Průzkumník cloudu pro Visual Studio 2015, což vám umožní tooview vašich prostředků Azure, kontrolovat jejich vlastnosti a provádět akce klíče vývojáře z Visual Studia. 

Průzkumník cloudu podporuje hello následující:

* Zobrazení skupiny prostředků a typ prostředku vašich prostředků Azure 
* Hledat prostředky podle názvu (k dispozici v zobrazení typ prostředku)
* Podpora pro odběry a prostředky, které mají na základě řízení přístupu Role (RBAC) použít 
* Integrované panelu akcí, které znázorňuje zaměřené na vývojáře akce tooselected konkrétní prostředky. Příklad: připojení vzdáleného ladicího programu pro virtuální počítače vytvořené pomocí hello zásobníku Resource Manager, zobrazit data diagnostiky pro virtuální počítače atd.
* Integrované panely vlastnosti, které se zobrazují vlastnosti zaměřené na vývojáře běžně potřebný během vývoje/testování 
* Rychlé přepínání toouse hello účet při vytváření výčtu zdroje (použijte příkaz Nastavení na panelu nástrojů) 
* Filtrování odběry toouse při vytváření výčtu zdroje (použijte příkaz Nastavení na panelu nástrojů) 
* Přímé odkazy toohello portál Azure pro správu prostředků a skupin prostředků 

### <a name="azure-resource-manager-tools"></a>Nástroje Azure Resource Manager
Nástroje Správce prostředků Azure Hello byly aktualizované toowork s na základě řízení přístupu Role (RBAC) a nové typy předplatného.  Součástí těchto změn je hello možnost toouse nové účtů úložiště, kromě tooclassic úložiště artefaktů toostore během nasazení.  

Pokud používáte projekt skupiny prostředků Azure z předchozí verze hello SDK s hello SDK 2.7, nový skript nasazení je potřebné toodeploy místo klasického úložiště pomocí nového účtu úložiště.  Zobrazí se výzva před provedením změn tooyour projektu tooadd hello nový skript.  původní skript Hello se přejmenuje a budete potřebovat toomanually provést žádné úpravy toohello nový skript.

### <a name="storage-explorer-tools"></a>Nástroje pro Průzkumníka úložiště
* Podpora pro zobrazení doplňovacích objektů BLOB. Další informace v [tomto příspěvku na blogu](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
* Podpora pro zobrazení účty služby Premium Storage pomocí Průzkumníka serveru. V Průzkumníku serveru se zobrazí jenom objekty BLOB stránky pro účty úložiště premium, protože se jedná o typ hello podporována pouze pro účty úložiště premium.

### <a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data Factory Tools for Visual Studio
Představení **nástrojů Azure Data Factory** pro sadu Visual Studio. Níže jsou hello povolené funkce. V tématu [tomto blogu](http://go.microsoft.com/fwlink/?LinkId=617530) Další informace.

* **Na základě šablony při vytváření**: Vyberte použití použita na základě šablony, šablony pro přesun dat nebo zpracování dat šablony toodeploy integrační řešení začátku do konce dat a začít praktických rychle pomocí služby Data Factory. 
* **Integrace s Průzkumníku řešení pro vytváření a nasazení entit služby Data Factory**: vytvoření a nasazení kanálů a entit v relaci jako projektů sady Visual Studio. 
* **Integrace s zobrazení diagramu pro visual interakce při vytváření**: vizuálně vytváření kanálů a datové sady s podporou z hello zobrazení diagramu. 
* **Integrace s Průzkumníka serveru pro procházení a interakci s již nasazené entity**: toobrowse Průzkumníka serveru hello využívají nenasadí datové továrny a odpovídající entity. Importujte objekt pro vytváření dat nasazené nebo entitu (kanál, propojené služby, datové sady) do projektu. 
* **JSON úpravy pomocí schématu ověření a bohaté intellisense**: efektivní konfigurace a úpravám dokumentů JSON entit služby Data Factory s bohaté funkce intellisense a schéma ověřování 
* **Publikování více prostředí**: publikování toodev vytvořené kanály, testu nebo produkčnímu prostředí tak, že vytvoříte samostatné konfigurační soubory pro každé prostředí.
* **Pig, Hive a .net podporuje zpracování dat na základě**: podpora pro Pig a Hive skripty v projekt Data Factory. Podpora pro odkazování na projekt C# pro správu .net aktivity.

## <a name="azure-sdk-for-net-271"></a>Azure SDK pro .NET 2.7.1
Hello následující část obsahuje aktualizace, které byly zavedeny v hello Azure SDK pro .NET 2.7.1 verzi.

### <a name="hdinsight-tools"></a>Nástroje HDInsight
Podrobné vysvětlení o aktualizacích nástroje HDInsight, najdete v části [tomto blogu](http://go.microsoft.com/fwlink/?LinkId=623831).

* Operátor zobrazení úloh Hive (nová funkce)
  
    toohelp pochopit dotaz Hive lepší, hello zobrazení operátora Hive funkci se přidala. toosee všechny hello operátory uvnitř vrcholu, dvakrát klikněte na vrcholy grafu úlohy hello hello. tooview další podrobnosti o konkrétní operátor, najeďte myší na tento operátor.
* Hive chyby značky (nová funkce)
  
    tooenable, které jste tooview hello gramatika chyby téměř okamžitě hello Hive značky Chyba funkce byla přidána. Navíc byly vylepšeny chybové zprávy a můžete nyní zobrazit podrobné gramatika chyby okamžitě (až do této verze jste měli toosubmit clusteru toohello skriptu Hive a počkejte chvíli před získáním podrobnosti o chybová zpráva).  
* Graf topologie Storm (nová funkce)
  
    Vizualizace je velmi důležité, když chcete toosee, pokud vaše topologie funguje podle očekávání. V této verzi jsme přidali vizualizaci Storm grafy. Můžete vizualizovat hello důležité metriky pro topologii (například barvu, která označuje počasí na určité funkce Bolt "zaneprázdněn" nebo ne). Můžete také dvakrát kliknete hello Bolt/Spout tooview další podrobnosti.
* Podpora pro clusterů HDInsight, které byly vytvořeny v hello portálu Azure (oprava chyb)
  
    Teď můžete použít Visual Studio tooview a odeslání úlohy tooall clusterů vaší HDInsight bez ohledu na to, kde byly vytvořeny hello clusteru.
* Další podporu technologie IntelliSense & rychlejší Hive Metadata načítání (zlepšení)
  
    Přidáním další návrhy uživatelsky přívětivý jsme vylepšili hello IntelliSense. Například tabulky alias můžete nyní také navrženo v technologii IntelliSense, lze snadněji napsat dotaz. Také jsme vylepšili metadata Hivu hello toolist načítání tak bude trvat jenom několik sekund, všechny databáze hello, tabulky a sloupce vaše metaúložiště Hive.

Podrobné vysvětlení o aktualizacích nástroje HDInsight, najdete v části [tomto blogu](http://go.microsoft.com/fwlink/?LinkId=623831).

### <a name="improvements-in-visual-studio-2013"></a>Vylepšení v sadě Visual Studio 2013
* Azure SDK 2.7.1 umožňuje Visual Studio 2013 tooaccess účtů Azure a odběry prostřednictvím řízení přístupu na základě rolí, poskytovatele cloudových řešení a Dreamspark.
* Sada Azure SDK 2.7.1 a hello nové okno nástroje Průzkumník cloudu je nyní dostupná také v sadě Visual Studio 2013.

### <a name="known-issues"></a>Známé problémy
Instalace hello Azure SDK 2.6 nebo 2.7.1 pro Visual Studio Community 2013 na jiný - anglické operačního systému se zobrazí upozornění, které hello angličtina a může být neshoda jiných než anglických prostředky sady Visual Studio. Toto upozornění může bezpečně zrušit. Se vztahuje pouze pokud počítače hello neobsahoval předchozí instalace Visual Studio Community 2013 a instalujete hello SDK na jiný - anglické operačního systému. Hello upozornění se zobrazí po hello jazyková sada platí hello RTM prostředky tooVisual Studio, ale předtím, než se vztahuje aktualizací 4. Zavření hello upozornění vám umožní hello language pack toocontinue a dokončení používá verzi hello aktualizace 4 hello jazykových sad obsahu.

Projekty LightSwitch nejsou s aktualizací v této verzi. Tento problém se vyřeší s hello verzi další sady SDK.

## <a name="also-see"></a>Viz také
[Azure SDK 2.7.1 post oznámení](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 oznámení post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Podporu a informace o vyřazení pro hello Azure SDK pro .NET a rozhraní API](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

