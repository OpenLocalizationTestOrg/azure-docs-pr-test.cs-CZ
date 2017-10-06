---
title: "aaaAzure SDK pro .NET poznámky k verzi 2.6"
description: "Poznámky k verzi sady Azure SDK pro .NET 2.6"
services: app-service/web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: b45853d5-a2b8-4962-a22d-579cb36ae14c
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: ac613cf20da4f731fab6f35ccbf6dbeaf18c0ec4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-26-release-notes"></a>Poznámky k verzi sady Azure SDK pro .NET 2.6
Tento dokument obsahuje hello poznámky k verzi pro hello Azure SDK pro .NET 2.6 verzi. 

S Azure SDK 2.6 můžete vyvíjet cílení na rozhraní .NET 4.5.2 nebo .NET 4.6 za předpokladu, že je ručně nainstalovat hello cílové rozhraní .NET Framework na hello cloudové služby Role aplikace cloudové služby (PaaS). V tématu [instalace rozhraní .NET v roli služby Cloud](http://go.microsoft.com/fwlink/?LinkID=309796).

## <a name="service-bus-updates"></a>Aktualizace služby Service Bus
* Centra událostí: 
  
  * Teď umožňuje řídit cílové přístup při odesílání událostí podle vystavení další vydavatele koncový bod pro službu Event Hubs.
  * Zvýšení stability a zlepšování přidat funkce tooEvent rozbočovače.
  * Přidání podpory protokolu Amqp přes protokol WebSocket pro zasílání zpráv a Event Hubs.

## <a name="hdinsight-tools-for-visual-studio-updates"></a>Aktualizace nástroje HDInsight pro Visual Studio
* **Vylepšení IntelliSense**: návrh vzdálených metadat
  
    Nástroje HDInsight pro Visual Studio teď podporuje získávání vzdálených metadat při úpravách skriptu Hive. Například můžete zadat **vyberte * FROM** a zobrazí se všechny názvy tabulek hello. Taky názvy sloupců hello se zobrazí po zadání tabulku.
* **Podporu emulátoru HDInsight**
  
    Nástroje HDInsight pro Visual Studio – podpora připojení tooHDInsight emulátoru, takže skripty Hive může vyvíjet místně bez zavedení žádné náklady, teď spustit pak tyto skripty proti clusterům HDInsight. 
  
    Další informace najdete v části příliš[této příručky](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).
* **Nástroje HDInsight pro Visual Studio podporují pro obecné clusterů systému Hadoop** (Preview)
  
    Nástroje HDInsight pro Visual Studio teď podporují obecné clusterů systému Hadoop, abyste je mohli používat nástroje HDInsight pro Visual Studio toodo hello následující:
  
  * Připojte tooyour cluster 
  * Zápis dotazů Hive s rozšířenou podporu technologie IntelliSense nebo automatické dokončování 
  * Zobrazte všechny úlohy hello v clusteru s intuitivní uživatelské rozhraní. 
    
    Další informace najdete v části příliš[této příručky](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

## <a name="in-role-cache-updates"></a>Aktualizace mezipaměti v roli
* **Mezipaměť hostovaná v instanci Role** byl aktualizovaný toouse **Microsoft Azure Storage SDK** verze 4.3. Dosud hello **mezipaměť hostovaná v instanci Role** používal úložiště Azure SDK 1.7 verze.
  
    Zákazníci pomocí Azure SDK 2.5 nebo níže by měl aktualizovat tooAzure SDK 2.6 a přesunout toohello novou verzi sady Azure SDK úložiště. 
  
    V této verzi čas Azure Storage je 2011-08-18 naplánované toobe odebrat 1 srpna 2016. Všechny migrace mezipaměť hostovaná v instanci Role z Azure SDK 2.5 nebo pod too2.6 musí být úplný té doby. Další informace o vyřazení hello verze 2011-08-18 Azure Storage najdete v tématu [aktualizace verze odebrání úložiště služby Microsoft Azure: rozšíření too2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

> [!IMPORTANT]
> Hello jsme se uvedení 30. listopadu 2016, vyřazení služby mezipaměti spravované Azure a Azure v roli Cache. Doporučujeme migraci tooAzure Redis Cache během přípravy na tento vyřazení. Další informace o kalendářních dat a migrace pokyny najdete v tématu [Azure Cache které nabídky je pro mě nejlepší?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)
> 
> 

## <a name="azure-app-service-tools"></a>Nástroje služby Azure App Service
Hello následující položky byly aktualizovány v hello Azure SDK 2.6 verzi.

* Azure publikování rozšířeného aplikace API Azure tooinclude jako cíl nasazení.
* Zřizování uživatelů tooenable funkce s vytvoření aplikace API a zřizování funkce API Apps.
* Server Explorer změnit tooreflect nový uzel App Service, webové, mobilní a API aplikace, které jsou seskupené podle skupiny prostředků.
* Přidat klienta aplikace Azure API gesto přidaný toomost C# projekty, které vám umožní automatické generování povoleno Swagger rozhraní API aplikace běžící v rámci předplatného Azure uživatele.
* Pouze jsou k dispozici v sadě Visual Studio 2013 nástrojů aplikace API a uzly služby App Service v Průzkumníku serveru. 

## <a name="azure-resource-manager-tools-updates"></a>Aktualizace nástroje Azure Resource Manager
nástroje Správce prostředků Azure Hello byly tooinclude aktualizované šablony pro virtuální počítače, sítě a úložiště. úpravy prostředí Hello JSON byl aktualizovaný tooinclude nové zobrazení osnovy pro šablony a hello možnost tooedit hello použití fragmentů kódu JSON. Šablony nasazení ze sady Visual Studio pomocí skriptu prostředí PowerShell, který je součástí projektu hello, takže veškeré změny provedené toohello skriptu se použije v sadě Visual Studio.

## <a name="diagnostics-improvements-for-cloud-services"></a>Vylepšení diagnostiky pro cloudové služby
Azure SDK 2.6 zobrazí zpět podporu pro shromažďování protokolů diagnostiky v hello emulátoru služby výpočty Azure a jejich přenos toodevelopment úložiště. Všechny diagnostické protokoly (včetně protokolů událostí trasování pro protokoly systému Windows (ETW), čítače výkonu, infrastruktury protokoly událostí systému windows a protokolování trasování aplikace) vygenerované při hello aplikace běží v emulátoru hello může být přenášená toodevelopment tooverify úložiště, který vaše protokolování diagnostiky pracuje na místním počítači. 

v souboru (.csdef) služby hello, takže je jednodušší účty úložiště v různých prostředích toouse různých diagnostiky nyní lze zadat Hello účet úložiště diagnostiky. Existují některé významné rozdíly mezi jak hello připojovací řetězec fungovala Azure SDK 2.4 a Azure SDK 2.6. Další informace o jak toouse hello úložiště diagnostiky připojovací řetězec a jak se má dopad na vašich projektů zjistit [konfigurace diagnostiky pro Azure Cloud Services](http://go.microsoft.com/fwlink/?LinkID=532784).

## <a name="breaking-changes"></a>Nejnovější změny
### <a name="azure-resource-manager-tools"></a>Nástroje Azure Resource Manager
* Hello **projekty nasazení cloudu** projektu typu, které jsou k dispozici v hello Azure SDK 2.5 byl přejmenován příliš**skupiny prostředků Azure**.
* **Projekty nasazení cloudu** typ projektů vytvořených v hello Azure SDK 2.5 je možné použít ve verzi 2.6 ale nasazení hello šablony ze sady Visual Studio dojde k chybě. Ale nasazení s hello skript prostředí PowerShell budou i nadále fungovat stejně jako dříve.  Informace o tom toouse **projekty nasazení cloudu** ve verzi 2.6 načíst tato [post](http://go.microsoft.com/fwlink/?LinkID=534086).

## <a name="known-issues"></a>Známé problémy
* Shromažďování protokolů diagnostiky v emulátoru hello vyžaduje 64bitový operační systém. Diagnostické protokoly nebudou shromažďovány, pokud se používá na 32bitové verzi operačního systému. Toto nastavení neovlivní žádné další funkce emulátor. 
* Azure SDK 2.6 vydané 4/29/2015 měl dva problémy: 
  
  * Univerzální aplikace nelze načíst v sadě Visual Studio 2015, pokud Azure SDK 2.6 byl nainstalován na počítači hello.
  * Ladění projektu cloudové služby skončí s chybou v sadě Visual Studio 2013 a Visual Studio 2015, kde Visual Studio přestane reagovat a dojde k chybě při zobrazení dialogového okna s uvítací zprávu "Konfigurace diagnostiky pro emulátor".
    
    Aktualizaci tooAzure SDK 2.6 byla vydána 18 5. 2015. Hello aktualizované verze je 2.6.30508.1601; obsahuje opravy pro dva problémy popsané výše. Můžete identifikovat hello sestavení hello SDK z ovládací panely -> programy a funkce -> nástroje Microsoft Azure pro Microsoft Visual Studio 2013 – v 2.6. sloupec verze Hello se zobrazí číslo sestavení hello.
    
    Pokud se stále setkávají hello výše problémy, nainstalujte hello nejnovější verzi hello Azure 2.6 SDK pro [VS 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) nebo [VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).

## <a name="see-also"></a>Viz také
[Podporu a informace o vyřazení pro hello Azure SDK pro .NET a rozhraní API](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

