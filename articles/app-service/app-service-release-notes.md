---
title: "Poznámky k verzi sady SDK pro .NET 2.5.1 aaaAzure"
description: "Poznámky k verzi sady Azure SDK pro .NET 2.5.1"
services: app-service
documentationcenter: .net,nodejs,java
author: Juliako
manager: erikre
editor: 
ms.assetid: 8d3d815f-bb58-447e-8ff0-f9b9603c7b00
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/10/2016
ms.author: juliako
ms.openlocfilehash: 5ee7688617c966baa409045881c172bbbc55ff63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-251-release-notes"></a>Poznámky k verzi sady Azure SDK pro .NET 2.5.1
Tento dokument obsahuje hello poznámky k verzi pro hello Azure SDK pro .NET 2.5.1 verzi. 

## <a name="azure-sdk-for-net-251-release-notes"></a>Poznámky k verzi sady Azure SDK pro .NET 2.5.1
Následující Hello jsou nové funkce a aktualizace v hello Azure SDK for .NET 2.5.1.

* Nové features\scenarios související příliš**Web Tools Extensions**. 
  
  * Weby Azure byl přejmenován tooAzure služby App Service. Další informace najdete v tématu [Azure App Service a stávající služby Azure](../app-service-web/app-service-changes-existing-services.md).
  * Byla přidána podpora Azure API Apps (Preview) tak, aby zákazníci můžete publikovat projekty ASP.NET jako aplikace API a pak použít hello Přidat > gesto klienta aplikace Azure API v C# projekty toogenerate kód na základě hello struktura hello nasazené aplikace API. 
  * uzel Hello weby v Průzkumníku serveru se už nepoužívá místo hello Azure App Service uzlu, který obsahuje podporu pro prostředek skupiny založené seskupení aplikace Azure API, mobilní aplikace a webové aplikace.
  * Byla přidána podpora Azure Mobile Apps (Preview) tak, aby zákazníci vytvářet nové projekty Mobile Apps, přidejte řadiče Mobile Apps, publikovat projekty hello a vzdáleně ladit aplikace.
  * Přidat > gesto klienta aplikace API Azure nyní podporuje místní soubory JSON pro Swagger, takže mohou vývojáři webového rozhraní API NuGets třetích stran, jako je Swashbuckle toogenerate Swagger nebo ho ručně vytvořit. Tímto způsobem klienta mohou vývojáři hello generování kódu funkce tooconsume žádný koncový bod Swagger v projektů C#. 
  * Webová aplikace a aplikace API, kterou je publikování dialogová okna Rozšířené toosupport hello portálu Azure koncept seskupení prostředků a výběr nebo vytvoření skupiny prostředků Azure a plány služby App Service jsou reprezentované v hello webové aplikace a aplikace API zřizování dialogové okno Nový. 
  * Uzlů Azure Průzkumníka serveru aplikace API poskytují toohello odkazy, které aplikace API přímý odkaz v hello portálu Azure a také další funkce, například vysílání datového proudu protokolu a vzdálené ladění.
    
    Známé problémy a aktuální omezení v Azure SDK .NET 2.5.1 [to](app-service-release-notes.md#known_issues_2_5_1) části níže.
* Nové features\scenarios související příliš**nástroje HDInsight** v sadě Visual Studio, které jsou povolené v této verzi. 
  
  * Místní ověření hive skriptů. Pokud nejsou žádné chyby ve vašem skriptu, klikněte na tlačítko hello ověření skriptu tlačítka na panelu nástrojů toosee hello. 
  * Vylepšení ladění úloh Hive. Nyní můžete ladit úlohy Hive díky přístupu k protokoly Yarn v sadě Visual Studio. Pokud vaše aplikace má problémy s výkonem, bude na odstranění příčin protokoly YARN poskytují užitečné informace...
  * (Verze public Preview) Automatické dokončování – klíčové slovo a IntelliSense podpora Hive. toohelp vytvářet skripty Hive, nástroje HDInsight pro Visual Studio přidat automatické dokončování – klíčové slovo a podporu technologie IntelliSense pro Hive.
  * Podpora Storm. Teď můžete používat nástroje HDInsight pro Visual Studio toodevelop Storm topologie/funkcích Spouts nebo funkce Bolts v jazyce C#. Potom můžete odeslat hello vyvinuté cluster Storm tooa topologie a zobrazit stav topologie/bolt/spout hello. Můžete použít systémové protokoly a zákazník protokoly tootroubleshoot vaše Storm topologie nebo funkce Bolts nebo funkcích Spouts. Můžete také použít existující prostředky JAVA v Storm v HDInsight.
    
    Další informace najdete v tématu [Začínáme pomocí nástrojů Hadoop HDInsight pro Visual Studio](../hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

## <a id="known_issues_2_5_1"></a>Azure SDK pro .NET 2.5.1 – známé problémy a omezení
* Aplikace API Azure se zobrazí jako cíl nasazení pro mobilní aplikace. Webové aplikace by měl být hello pouze cíl pro Mobile Apps, dokud další verze. 
* Zřizování aplikace API Azure může způsobit úspěšné, ale občas selhat tooupdate hello průběh v okně aktivita služby Azure App Service hello. Alternativní řešení není toocheck stav hello nové Azure API App v hello portálu Azure. 
* Soubor > Nový Projekt > aplikace API > F5 prostředí výsledkem chyby protokolu HTTP, protože neexistuje žádný default/index.html. Alternativní řešení je toomanually procházet toohello/api/hodnoty URL. 
* Ikony v Průzkumníku serveru zobrazí přerušovaně plochou. Restartování VS řeší tento problém. 
* Pokud je vyvolána výjimka při webovou aplikaci nebo aplikaci API zřizování (například chyb překročil kvótu nebo duplicitní název aplikace API Azure brány), zobrazit chyby hello některé nezpracovaný text JSON. 
* Při kontrole Application Insights v okamžiku vytvoření projektu přerušované vytváření projektu problémy.
* V některých případech hello vygenerovat kód klienta aplikace Azure API chybí obory názvů, potřebují toobe ručně zahrnout (nebo automaticky importovány pomocí sady Visual Studio upozornění) pro toocompile kódu. 
* Projekty pro mobilní aplikace by měla být tooweb publikované aplikace, ale je třeba vybrat, kterou jste vytvořili jako mobilní aplikace v portálu Azure hello vzhledem k tomu, že projekty mobilní aplikace vyžadují databázi lokality. 
* Hello úvodní stránky pro Mobile Apps používá hello termín "Mobilní služba" místo "mobilní aplikace" 
* Vytvoření projektu mobilní aplikace může trvat minutu toocreate tooa. 
* Během aplikace API zřizování (v některých případech) chybu zpátky ze hello rozhraní API služby Azure odrážející, nepodařilo se správně, nastavit oprávnění hello při hello aplikace API správně zřízený a je připravený k použití. Můžete ručně nastavit oprávnění pomocí hello portálu Azure.
* Application Insights nepodporuje šablony aplikace API a mobilní aplikace.
* Projekty aplikace API nelze použít ve spojení s projekty cloudových služeb.
* Šablony projektu aplikace API jsou dostupné jenom v C#.
* Spotřeba aplikace API prostřednictvím hello "Přidat klienta aplikace Azure API" Kontextová nabídka je podporována pouze v jazyce C#.

