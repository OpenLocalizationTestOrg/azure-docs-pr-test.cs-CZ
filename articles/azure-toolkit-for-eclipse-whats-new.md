---
title: "Co je nového v sadě Azure nástrojů pro Eclipse"
description: "Další informace o nejnovější funkce v sadě nástrojů Azure pro Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 16b066ea-aae7-4c30-9a12-fa0c3711b93e
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: 71c4f2d2298ea54f18e9ed64b246966b470a4ff4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-eclipse"></a>Co je nového v sadě Azure nástrojů pro Eclipse
## <a name="azure-toolkit-for-eclipse-releases"></a>Azure nástrojů pro verze Eclipse
Tento článek obsahuje informace o různých verzích a nejnovější aktualizace nástrojů Azure pro prostředí Eclipse.

> [!NOTE]
> Je také Azure nástrojů pro IntelliJ IDE. Další informace najdete v tématu [nástrojů Azure pro IntelliJ].
> 
> 

### <a name="april-14-2017"></a>14. dubna 2017
Sady nástrojů Azure pro Eclipse – vydání duben 2017 obsahuje následující vylepšení:

* **Vylepšené přihlašovací prostředí Azure**: Sada nástrojů Azure pro Eclipse teď podporuje dvě metody protokolování do účtu Azure: *interaktivní* a *automatizovaná*. Další informace najdete v tématu [Azure přihlášení v pokyny pro Azure nástrojů pro Eclipse].
* **Publikování pomocí Docker kontejnery**: můžete teď publikování webových aplikací jako kontejnery Docker pomocí sady nástrojů Azure pro Eclipse. Další informace najdete v tématu [jak publikovat webovou aplikaci jako kontejner Docker pomocí sady nástrojů pro Azure pro Eclipse].
* **Správa účtů úložiště**: Sada nástrojů Azure pro Eclipse nyní podporuje správu účty úložiště ze zobrazení Průzkumníka Azure. Další informace najdete v tématu [Správa účtů úložiště pomocí Průzkumníka Azure pro Eclipse].
* **Virtual Machine Management**: Sada nástrojů Azure pro Eclipse teď podporuje správu virtuálních počítačů ze zobrazení Průzkumníka Azure. Další informace najdete v tématu [Správa virtuálních počítačů pomocí Průzkumníka Azure pro Eclipse].
* **Odebrání vzdálené ladění podporu**. Vzdálené ladění webových aplikací Java v Azure App Service je odebraná ze sady nástrojů Azure pro Eclipse; To je nezbytný k řešení některých problémů, které zákazníků měla zaznamenat při pomocí sady nástrojů.

### <a name="august-26-2016"></a>26 srpna 2016
Sada nástrojů Azure pro Eclipse – verze srpna 2016 obsahuje následující vylepšení:

* **Vlastní JDK distribuce**. Sady nástrojů Azure pro prostředí Eclipse nyní podporuje zadání a nasazení je libovolný JDK verze vaší webové aplikace Azure kontejneru:
  * Kromě JDKs poskytovaný platformou Azure můžete také z široký výběr zulština OpenJDK verze k dispozici v Azure Azul systémy.
  * Můžete také zadat distribuční JDK Pokud uložíte jako soubor ZIP do účtu úložiště.
* **Vylepšení Průzkumník Azure**:
  * Podpora pro správu virtuálního počítače pomocí nového modelu Resource Manager Azure: můžete zobrazit seznam, vytvářet a odstraňovat virtuální počítače založené na správci prostředků aniž byste museli opustit rozhraní IDE.
  * Podpora pro správu účtu úložiště blob pomocí Azure Resource Manager, které doplňuje stávající funkce pro správu "klasické" účty úložiště.
* **Ovladač Microsoft JDBC 6.0 pro SQL Server**. Tato aktualizace obsahuje nejnovější ovladač JDBC pro Microsoft SQL Server (verze 6.0), což je nyní zahrnuty jako knihovnu, kterou můžete snadno přidat do vašich projektů Java, a tím nahrazení starší verze.

### <a name="june-29-2016"></a>29. června 2016
Sady nástrojů Azure pro Eclipse –. června 2016 verze obsahuje následující vylepšení:

* **Požadavek Java 8**. Sada nástrojů Azure pro Eclipse nyní vyžaduje Java 8, i když tento požadavek je pouze pro sady nástrojů – aplikace můžete nadále používat všechny verze jazyka Java, které jsou podporovány službou Azure.
* **Podporu pro nejnovější JDKs Java**. Nejnovější verze Java JDKs jsou nyní podporovány sady nástrojů Azure pro prostředí Eclipse.
* **Podpora pro Azure SDK v2.9.1**. Nejnovější verzi sady Azure SDK je nyní minimální nezbytný předpoklad pro sady nástrojů Azure pro prostředí Eclipse.
* **Integrované ukázky**. Sada nástrojů Azure pro Eclipse teď nabízí několik ukázkových aplikací, což vývojářům začít pracovat.
* **Integrace nástroje HDInsight**. Nástroje HDInsight Azure nyní jsou dodávané společně s sady nástrojů Azure pro prostředí Eclipse. Další informace najdete v tématu [modulu plug-in nástroje HDInsight pro Eclipse].
* **Vzdálené ladění webových aplikací Java**. Sada nástrojů Azure pro Eclipse teď podporuje vzdálené ladění webových aplikací Java v Azure App Service.
* **Podpora pro verze vzhled Luna Eclipse.** Nové minimální požadovaná verze prostředí Eclipse IDE je vzhled Luna.

### <a name="april-12-2016"></a>12. dubna 2016
Sady nástrojů Azure pro Eclipse –. dubna 2016 verze obsahuje následující vylepšení:

* **Podpora pro Azure SDK v2.9.0**. Nejnovější verzi sady Azure SDK je nyní minimální nezbytný předpoklad pro sady nástrojů Azure pro prostředí Eclipse.
* **Různé použitelnost, odezvy a vylepšení výkonu související s webové aplikace Azure podporu**. Několik optimalizací výkonu v jak sadu nástrojů komunikuje s Azure výsledek v rychlejšího uživatelského rozhraní.
* **Možnost odstranit kontejner existující webové aplikace v Azure z v prostředí Eclipse**. Sada nástrojů Azure pro Eclipse teď umožňuje odstranit existující kontejner webů Azure bez opuštění Eclipse.

### <a name="march-7-2016"></a>7. března 2016
Sada nástrojů Azure pro Eclipse – verze března 2016 obsahuje následující vylepšení:

* **Podpora pro rychlé nasazení lightweight aplikací Java**. Sady nástrojů Azure pro prostředí Eclipse teď podporuje rychlého nasazení lightweight aplikací v jazyce Java do Azure webové aplikace kontejnery, takže teď nasazení aplikací v jazyce Java přijímá sekund místo minut.
* **Podpora pro správu webové aplikace pomocí Azure Průzkumník**. Průzkumník Azure v sadě nástrojů se teď podporuje pro výpis, spuštění a zastavení Azure Web Apps.
* **Aktualizovat Tomcat a Jetty, OpenJDK zulština distribuce**. Sada nástrojů Azure pro Eclipse poskytuje podporu aktualizovaných verzí Tomcat a Jetty zulština OpenJDK pro nasazení Java do cloudové služby Azure.

### <a name="january-4-2016"></a>4 leden 2016
Sada nástrojů Azure pro Eclipse – leden 2016 verze obsahuje následující vylepšení:

* **Podpora pro aktualizace zulština OpenJDK**. Další informace najdete v tématu [Azul systémy webové stránky pro OpenJDK zulština].
* **Aktualizovat distribuce Tomcat a Jetty**. Distribuce Jetty a Tomcat, které jsou k dispozici na Microsoft Azure pro použití s Azure Toolkit pro Eclipse byly aktualizovány.
* **Parita mezi Eclipse a IntelliJ sadách pro Azure funkce**. Sada nástrojů Azure pro Eclipse a [nástrojů Azure pro IntelliJ] teď podporují stejnou sadu funkcí.

### <a name="september-1-2015"></a>1. září 2015
Sada nástrojů Azure pro Eclipse – září 2015 release obsahuje následující vylepšení:

* **Podpora pro aktualizace zulština OpenJDK**. Další informace najdete v tématu [Azul systémy webové stránky pro OpenJDK zulština].
* **Aktualizovat distribuce Tomcat a Jetty**. Distribuce Jetty a Tomcat, které jsou k dispozici na Microsoft Azure pro použití s Azure Toolkit pro Eclipse byly aktualizovány. (Tyto distribuce umožňuje vývojářům vytvářet rychlý vývoj a testování projektů pomocí sady nástrojů Azure pro prostředí Eclipse.
* **Podpora pro automaticky aktualizované odkazy Tomcat a Jetty**. Kromě konkrétních verzí Tomcat a Jetty, které jsou k dispozici v Azure, můžete odkazovat vývojáři nyní označuje jako distribuční "nejnovější (automatické aktualizaci)", který bude automaticky aktualizovat na nejnovější distribuci každou hlavní verzi Jetty nebo Tomcat při příštím instance role jsou recyklovány. (Recyklace probíhá automaticky, ale vývojáři můžou aktivovat ručně recyklaci prostřednictvím portálu Azure.) Tato nová funkce znamená, že vývojáři, není nutné znovu nasadit aplikaci, která bude mít možnost získat přehledné jejich serverového softwaru aktualizovat. (
* Tato funkce je aktuálně určena pouze pro účely vývoj a testování nebo jiných zvláště důležitých aplikací a se nedoporučuje pro produkční prostředí.)
* **Azure Průzkumník pro objekty BLOB, fronty a tabulky v úložišti Azure**. To umožňuje vývojářům provádět sadu běžné úlohy s jejich úložiště artefaktů přímo z integrovaného vývojového prostředí Eclipse. Příklad: odstraňování, nahrávání nebo stahování objekty BLOB.

### <a name="august-1-2015"></a>1. srpna 2015
Sada nástrojů Azure pro Eclipse – srpen 2015 release obsahuje následující vylepšení:

* **Application Insights instrumentace správy klíčů**. Tato aktualizace umožňuje získat, vytvořit a spravovat klíče instrumentace Application Insights přímo z integrovaného vývojového prostředí Eclipse.
* **Ovladač Microsoft JDBC 4.1 pro SQL Server**. Tato aktualizace zahrnuje podporu pro nejnovější ovladač JDBC pro Microsoft SQL Server.
* **2.7 verzi sady Azure SDK**. Tato nejnovější aktualizace na sadu SDK Azure je nové nezbytné sady Toolkit při instalaci v systému Windows. (Všimněte si, že to není nutné v operačních systémech jiný systém než Windows).
* **Podpora pro aktualizaci v7 zulština OpenJDK**. Další informace najdete v tématu [Azul systémy webové stránky pro OpenJDK zulština].

### <a name="may-1-2015"></a>1 květen 2015
Sada nástrojů Azure pro Eclipse – květen 2015 release obsahuje následující vylepšení:

* **Vylepšené výběr serveru uživatelského rozhraní**. Tato verze zjednodušuje použití sady nástrojů v operačních systémech jiný systém než Windows.
* **Podpora pro projekty Maven**. Tato verze podporuje projekty Maven jako aplikace, které sady nástrojů můžete nasadit do Azure a konfigurace Application Insights.
* **2.6 verzi sady Azure SDK**. Tato nejnovější aktualizace na sadu SDK Azure je nové nezbytné sady Toolkit při instalaci v systému Windows. (Všimněte si, že to není nutné v operačních systémech jiný systém než Windows).
* **Nasazení upgradu místo opakované publikování**. Pokud znovu projekt nasazení při předchozí verze je již živá, sady nástrojů teď používá Azure nasazení upgradu funkce místo vypínání předchozích nasazení a opětovné publikování od začátku, stejně jako v minulosti. To umožňuje cloudové služby běžet bez přerušení, kdykoli je to možné, pomáhá dosažení vysoké dostupnosti i během aktualizace a urychlí proces opětovné publikování.
* **Podporu pro nejnovější v8: OpenJDK zulština – aktualizovat 40**. Další informace najdete v tématu [Azul systémy webové stránky pro OpenJDK zulština].

### <a name="march-9-2015"></a>9. března 2015
Sady nástrojů Azure pro Eclipse – března 2015 verze obsahuje následující vylepšení:

* **Podpora pro Mac, Ubuntu a dalších typů Linux**. Tato verze sady nástrojů Azure pro Eclipse přidává podporu pro Mac OS a několik platforem Unix, takže vývojáři můžete nainstalovat sadu nástrojů k vytváření, konfiguraci a publikovat projekty Java do Azure Cloud Services (PaaS) z Eclipse běžící na operačních systémech než Windows.

> [!NOTE]
> Tato funkce je ve verzi preview a není doporučeno pro použití v produkčním prostředí. Neexistuje žádná podpora zákaznickou smlouvu o úrovni služeb (SLA), ale všechny zpětná vazba je vezme v úvahu a podporovat.
> 
> 

* **Nový modul plug-in Application Insights**. Vývojáři jsou teď moct konfigurovat automatické serveru telemetrie pomocí Application Insights na platformě Azure.
* **Automatizace nasazení na základě ant příkazového řádku**. Tato funkce umožňuje vývojářům automatizovat publikování novějších verzích jejich nasazení pomocí Ant mimo prostředí Eclipse. Předem generovaného skriptu se automaticky nakonfiguruje pro projekt po prvním nasazení z prostředí Eclipse, a následné nasazení můžete použít skript pro úplnou automatizaci nasazení pomocí příkazového řádku.
* **Tomcat a Jetty dostupnosti v Azure pro jednodušší a rychlejší nasazení**. Vývojáři nyní můžete odkazovat různých verzí Tomcat a Jetty, které jsou k dispozici v Azure přímo místo toho, že nahrát Java serveru k účtu (nebo prostřednictvím sady Toolkit), takže není nutné odeslat serveru Java pro rychlý a Začínáme scénáře.
* **Metoda zástupce pro publikování webové aplikace v jazyce Java do cloudové služby Azure**. Chcete-li redukovat křivku pro jednoduché vývojová a testovací scénáře, můžete vývojáři nyní publikovat aplikací Java více přímo do Azure. Místo nutnosti přejít provede procesem vytvoření a konfigurace projektu nasazení Azure, bude aplikace nasadit s výchozí instanci v8: Tomcat a zulština JVM (OpenJDK).

### <a name="january-30-2015"></a>30 leden 2015
Sada nástrojů Azure pro Eclipse – leden 2015 release obsahuje následující vylepšení:

* **Podpora jádra Liberty serveru aplikace IBM® WebSphere®**. Tato verze přidává Liberty jádra IBM WebSphere aplikace serveru do seznamu podporované aplikační servery, ze kterých je sada nástrojů lze nasadit do Azure. Nejnovější přidání rozšíří aktuální seznam aplikační servery, které jsou podporovány &quot;out box&quot; pomocí sady nástrojů, které již součástí různých verzí Tomcat, Jetty, JBoss a GlassFish.
* **Zahrnutí Application Insights SDK**. Tato knihovna nově vydané klientského rozhraní API (v0.9.0) je nyní součástí balíčku pro knihovny Azure Libraries for Java.
* **Aktualizoval balíček pro knihovny Azure Libraries for Java**. Tato aktualizace zahrnuje knihovny Azure pro Java v0.7.0 a rozhraní API klienta úložiště v2.0.0 i v0.9.0 nově vydané Application Insights SDK.

### <a name="november-12-2014"></a>12. listopadu 2014
Sada nástrojů Azure pro Eclipse – verze v listopadu 2014 obsahuje následující vylepšení:

* **Podpora pro Azure SDK, 2.5**. Tato nejnovější aktualizace na sadu SDK Azure je nové nezbytné pro sadu nástrojů.
* **Podpora pro aktualizovanou verzi zulština OpenJDK v1.8, v1.7 a v1.6 balíčky**. Další informace najdete v tématu [Azul systémy webové stránky pro OpenJDK zulština].
* **Podpora pro nové standardní D velikosti pro cloudové služby**, který nabízí vyšší výkon a další paměťové prostředky. Další informace najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure].

### <a name="october-17-2014"></a>17 říjen 2014
Sada nástrojů Azure pro Eclipse – říjen 2014 verze obsahuje následující vylepšení:

* **Vylepšení výkonu v publikování do cloudu scénáře**. Načítání informace o předplatném je mnohem rychlejší, když uživatelé mají více předplatných a účtů úložiště.
* **Podpora pro aktualizovanou verzi balíčku v1.8 zulština OpenJDK**. Další informace najdete v tématu [Azul systémy webové stránky pro OpenJDK zulština].
* **Podpora pro starší verze 3. stran JDKs místo začne**. Zastaralý JDK balíčky se nebude zobrazovat v rozevírací nabídce pro nové projekty nasazení. Existující projekty odkazující zastaralý JDK balíčky bude pokračovat můžete provést po dobu probíhá, ale doporučuje se upgradovat takové projekty moct spolehnout na nejnovější.
* **Aktualizovaná verze balíčku pro knihovny Azure pro Java klientské rozhraní API knihovny**. Další informace najdete v tématu [API klienta služby Microsoft Azure].
* **Opravy chyb.** Tato verze obsahuje několik ostatní opravy chyb, které jsou založené na uživatele sestavy a testování.

### <a name="august-5-2014"></a>5 srpen 2014
Sady nástrojů Azure pro Eclipse – srpen 2014 verze obsahuje následující vylepšení

* **Podpora pro Azure SDK 2.4.** Starší verze sady nástrojů Eclipse nebude fungovat s touto sadou SDK nově vydané.
* **Aktualizované verze zulština OpenJDK v1.6, 1.7 a v1.8 balíčky.** Další informace najdete v tématu [Azul systémy webové stránky pro OpenJDK zulština].
* **Aktualizovaná verze balíčku pro knihovny Azure pro Javu klientské rozhraní API knihovny.** Další informace najdete v tématu [API klienta služby Microsoft Azure].
* **Podporu pro nejnovější publikovat nastavení formátu souboru.** Byla přidána podpora pro verze 2.0 ve formátu souboru nastavení publikování.
* **Architektury změny za publikování cloudové funkce.** Sada nástrojů teď používá nově vydané klienta API služby Microsoft Azure pro jazyk Java pro jeho podporu publikovat do cloudu.
* **Opravy chyb.** Tato verze obsahuje několik-uživatel si vyžádal opravy chyb.

### <a name="june-12-2014"></a>12. června 2014
Sada nástrojů Azure pro Eclipse – verze června 2014 je dílčí servisní aktualizace, která poskytuje následující vylepšení:

* **Podpora pro v1.8 balíček zulština OpenJDK.** Další informace najdete v tématu [Azul systémy webové stránky pro OpenJDK zulština].
* **Aktualizované verze v1.6 zulština OpenJDK a 1.7 balíčků.** Další informace najdete v tématu [Azul systémy webové stránky pro OpenJDK zulština].
* **Aktualizovaná verze balíčku pro knihovny Azure pro Javu klientské rozhraní API knihovny.** Další informace najdete v tématu [API klienta služby Microsoft Azure].
* **Opravy chyb.** Tato verze obsahuje několik-uživatel si vyžádal opravy chyb.

### <a name="april-4-2014"></a>4. dubna 2014
Modul plug-in Azure pro Eclipse – vydání duben 2014 vydala. Jedná se o aktualizaci doplňujícími verzi 2.3 SDK Azure, který je nezbytný předpoklad a budou stahovat automaticky při instalaci modulu plug-in. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od února 2014 verzi Preview:

* **Podpora pro Azure SDK 2.3 verzi.** Modul plug-in Azure pro Eclipse – vydání duben 2014 vyžaduje Azure SDK 2.3. Při použití nových modulů plug-in, pokud již nemáte Azure SDK 2.3, vyzve k umožnit jeho instalaci. Nepoužívejte Azure SDK 2.3 s dřívějšími verzemi nástroje modul plug-in.
* **Upgrade aplikací bez dokončení balíčku nasazení.** Při nasazování aplikací Java, které jsou součástí projektu, modul plug-in teď automaticky odesílá je do vašeho vybraný účet úložiště, aby mohli aktualizovat a recyklovat instance rolí k nasazení nejnovější aplikace bits bez nutnosti znovu sestavit a znovu nasaďte celý balíček.
* **Tomcat 8 je nyní server známou aplikací.** Pokud vyberete instalační adresář Tomcat 8 na počítači v **Server** kartě **projekt nasazení Azure** dialogové okno, modul plug-in bude nyní automaticky zjišťovat a nebude možné instalovat Tomcat 8 v automatizované, podobným způsobem starších verzí Tomcat již v seznamu.
* **Aktualizace balíčků Azul zulština OpenJDK: 47 aktualizací v1.6 a v1.7 aktualizací 51.** Efektivní u této verze, aktualizace balíčku v7 otevřete JDK zulština Azul systému 51 je k dispozici. Otevřete JDK zulština v6 balíčky spustit i, který je k dispozici od aktualizace 47. Tyto aktualizace jsou kromě dříve k dispozici balíčku v7 otevřete JDK zulština aktualizace 45, 40 aktualizace a aktualizace 25.
* **Podpora pro velikosti A8 a A9 virtuální počítač Microsoft Azure.** Teď můžete nasadit cloudové služby na velkého množství paměti A8 a A9 virtuálního počítače velikosti. Další informace o těchto velikosti virtuálních počítačů najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure].
* **Automatické přesměrování z HTTP do HTTPS pro protokol SSL povolen role.** Cloudové služby obsahuje pouze HTTPS rolí, pokud žádost o uživatele HTTP, bude automaticky přesměruje na HTTPS. Není nutné vytvořit samostatnou roli pro zpracování požadavků HTTP.
* **Expresní emulátor používá pro místní emulace.** Emulátor Express Azure se teď používá jako emulátor při ladění aplikace místně.
* **Azure má byla přejmenované jako Microsoft Azure.** Obrazovky uživatelského rozhraní nyní projeví, že Azure má byla přejmenované a už názvem Azure.

### <a name="february-6-2014"></a>6. února 2014
Tento modul plug-in Azure pro Eclipse – únor 2014 Preview vydala. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od října 2013 Preview:

* **Podpora pro snižování zátěže protokolu SSL.** Zabezpečené snižování zátěže Sockets Layer (SSL) přidaná jako funkci, což umožňuje snadno povolit podporu protokolu Secure HTTPS (Hypertext Transfer) ve vašem nasazení Java v Azure, aniž by bylo potřeba konfigurace protokolu SSL v aplikační server Java. To je zvlášť důležité v spřažení relace nebo ověřit scénáře komunikace. Například při použití filtru služby Řízení přístupu (ACS), který již není podporován pomocí sady nástrojů. Další informace najdete v tématu [snižování zátěže protokolu SSL] a [postup snižování zátěže protokolu SSL použití].
* **GlassFish 4 je nyní server známou aplikací.** Pokud vyberete instalační adresář GlassFish 4 na počítači v **Server** kartě **projekt nasazení Azure** dialogové okno, modul plug-in bude nyní automaticky zjišťovat a nebude možné instalovat GlassFish 4 OSE automatizovaně, podobně jako verze GlassFish OSE 3 již v seznamu.
* **Aktualizace balíčku Azul zulština OpenJDK 45.** Efektivní v této verzi, Azul systému zulština (balíček v7 otevřete JDK) aktualizace 45 je nyní k dispozici. Toto je kromě dříve k dispozici aktualizace 40 a aktualizace 25.
* **Podpora pro 'auto' pro privátní porty koncových bodů.** Privátní port můžete nastavit na hodnotu automaticky. pro vstupních koncových bodů a vnitřních koncových bodů chcete, aby Azure automaticky přiřadit port do tohoto koncového bodu. Dříve může přiřadit pouze konkrétní číslo portu.
* **Podpora pro přizpůsobení název certifikátu (CN) k vytvoření certifikátu podepsaného svým držitelem uživatelského rozhraní.** Dříve byl použit stejný název pevně pro všechny nové certifikáty; Nyní můžete určit vlastní certifikát název, který pomůže rozlišit mezi více certifikátů na portálu Azure, použít pro jiné účely.
* **Panel nástrojů Azure:** nástrojů Azure má aktualizované s následujícími změnami: 
  * ![][ic710876]Tato ikona byla přidána **nový projekt nasazení Azure**.
  * ![][ic710877]Tato ikona se přidal jako zástupce do dialogového okna Vytvoření certifikátu podepsaného svým držitelem.
* **Podpora pro virtuální počítač A5 Azure velikost.** Teď můžete nasadit cloudové služby na velkého množství paměti velikost A5 virtuálního počítače. Další informace o této velikosti virtuálního počítače najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure].
* **Podpora pro Microsoft Windows Server 2012 R2.** Windows Server 2012 R2 teď můžete vybrat jako operační systém cloudu.

### <a name="october-22-2013"></a>22 říjen 2013
Tento modul plug-in Azure pro Eclipse – říjen 2013 Preview vydala. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od září 2013 Preview:

* **Podpora pro verze Azure SDK 2.2.** Modul plug-in Azure pro Eclipse – říjen 2013 Preview podporuje Azure SDK 2.2. Tento modul plug-in budou i nadále fungovat s Azure SDK 2.1 a bude automaticky nainstalovat Azure SDK 2.2, pokud již nemáte alespoň Azure SDK 2.1 nainstalovaný.
* **Aktualizace balíčku Azul zulština OpenJDK 40.** Jako oznámeno pro ze září 2013 Preview, povoluje modulů plug-in teď pomocí zadané třetích stran JDK přímo v Azure, aniž by bylo potřeba nahrát vlastní JDK. V říjnu 2013 vydání systému Azul zulština (balíček v7 otevřete JDK) 40 je k dispozici aktualizace; Toto je kromě původně publikované aktualizace 25.
* **Odkaz na nasazení cloudu v protokolu aktivit.** V rámci protokol činnosti Azure, když vaše nasazení je ve stavu **publikováno**, můžete kliknout na **publikováno** od teď je odkaz na vaše nasazení; nasazení pak se otevřou v prohlížeči. (Stav **publikováno** byl dříve označené **systémem**.)
* **Výběru OS cílů, které jsou k dispozici na publikování čas.** **Publikovat do Azure** dialogové okno obsahuje nové pole **cíl OS**, který poskytuje více zjistitelný způsob, jak vám umožní nastavit cílového operačního systému.
* **Auto přepište předchozí nasazení.** **Publikovat do Azure** dialogové okno obsahuje nové zaškrtávací políčko, **přepsat předchozí nasazení**. Pokud zaškrtnete toto políčko, pokud je publikována nové nasazení automaticky se přepíšou předchozích nasazení; nebude mít &quot;409 – konflikt&quot; problémy při publikování do stejného umístění, bez první při zrušení publikování předchozí nasazení.
* **Jetty 9 je nyní server známou aplikací.** Pokud vyberete instalační adresář Jetty 9 na počítači v **Server** kartě **projekt nasazení Azure** dialogové okno, modul plug-in bude nyní automaticky zjišťovat a nebude možné instalovat Jetty 9 v automatizované, podobným způsobem starší verze Jetty již v seznamu.
* **Přidání role z místní nabídky projektu.** **Azure** místní nabídky projektu nyní obsahuje novou položku nabídky, **přidat roli**, která nabízí rychlejší a další zjistitelný způsob, jak můžete přidat nové role do projektu Azure.
* **Aktualizace balíčku pro knihovny Azure Libraries for Java knihovny.** To je založené na verzi 0.4.6 [API klienta služby Microsoft Azure].

### <a name="september-25-2013"></a>25 ze září 2013
Tento modul plug-in Azure pro Eclipse – září 2013 Preview vydala. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od srpna 2013 Preview:

* **Schopnost nasadit balíček Azul zulština OpenJDK dostupné v Azure.** Byla přidána nová možnost, při zadávání JDK pro použití s nasazením Azure. Použití této možnosti můžete nasadit balíček JDK třetích stran přímo v cloudu Azure, aniž by museli nahrát vlastní. Systémy Azul poskytuje první například balíček volané zulština, podle OpenJDK, které teď můžou být nasazené pomocí této možnosti.
* **Aktualizace balíčku pro knihovny Azure Libraries for Java knihovny.** To je založené na verzi 0.4.5 [API klienta služby Microsoft Azure].

### <a name="august-1-2013"></a>1. srpna 2013
Tento modul plug-in Azure pro Eclipse – srpen 2013 Preview vydala. Jedná se o aktualizaci doplňujícími verzi sady Azure SDK 2.1, který je nezbytný předpoklad a budou stahovat automaticky při instalaci modulu plug-in. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od července 2013 Preview:

* **Odebrání z možností, které zahrnují místní JDK a místní aplikační server jako součást balíčku pro nasazení.** Stahování sadu JDK a aplikační server ze cloudového úložiště během nasazení je vhodnější vložení těchto součástí v balíčku, protože stahování položky má za následek menší časy velikost a rychlejší nasazení balíčku nasazení a jednodušší údržby. V důsledku toho možnosti zahrnují sadu JDK a aplikační server v balíčku pro nasazení se odebraly. Existující projekty, které byly nakonfigurovány jako součást balíčku pro nasazení budou automaticky převedeny na automatické – nahrání sadu JDK zahrnuje místní JDK a místní aplikační server a server aplikace do cloudového úložiště.
* **Podpora pro tuto verzi Azure SDK 2.1.** Tento modul plug-in Azure pro Eclipse – srpen 2013 Preview vyžaduje Azure SDK 2.1. Nepoužívejte srpen 2013 preview s předchozími verzemi sady Azure SDK a nepoužívejte Azure SDK 2.1 s dřívějšími verzemi nástroje modul plug-in Azure pro Eclipse.
* **Podpora pro tuto verzi Eclipse Kepler.** Související s tím, nové minimální požadovaná verze prostředí Eclipse IDE je indigově modré. Modul plug-in Azure pro Eclipse je už oficiálně otestovali na Helios.

### <a name="july-3-2013"></a>3. července 2013
Tento modul plug-in Azure pro Eclipse – červenec 2013 Preview vydala. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od může 2013 Preview:

* **Umožňuje vytvořit nový účet úložiště.** A **nový** tlačítko byl přidán do **přidat účet úložiště** dialogové okno. Umožňuje vytvořit účet úložiště v rámci modulu plug-in Eclipse, aniž by bylo nutné se přihlásit k portálu pro správu Azure. (Musíte už mít předplatné Azure k použití této funkce.) Další informace o vytvoření nového účtu úložiště najdete v tématu [k vytvoření nového účtu úložiště].
* **Nové &quot;(automaticky)&quot; možnost pro účet úložiště používané pro automatické nasazení JDK a serveru a pro ukládání do mezipaměti.** Při použití **automaticky odeslat** možnost pro sadu JDK a aplikační server, můžete teď určit **(automaticky)** pro adresu URL a úložiště účtu pro použití při nahrávání sadu JDK a aplikační server, nebo když pomocí ukládání do mezipaměti Azure. Potom budou tyto funkce automaticky používat stejný účet úložiště jako ten, který jste vybrali v **publikovat do Azure** dialogové okno. [vytvoření aplikace Hello World služby Azure v prostředí Eclipse] kurz byl aktualizovaný, aby používal novou **(automaticky)** možnost.
* **Možnost nastavit koncové body služby Azure.** Zadejte koncové body služby, které určují, že jestli vaše aplikace je nasazená a spravuje globální platformy Azure, Azure provozována společností 21Vianet v Číně, nebo soukromé platformy Azure. Další informace najdete v tématu [koncové body služby Azure].
* **Nasazení ve velkých organizacích, můžete zadat místní úložiště prostředků.** V případě, že vaše nasazení je příliš velký být obsažené ve složce approot výchozí, teď můžete zadat místní úložiště prostředků jako cíl nasazení pro vaše JDK a aplikačního serveru. Další informace najdete v tématu [nasazení velkých nasazeních].
* **Podpora pro velikosti A6 a virtuální počítač A7 Azure.** Teď můžete nasadit cloudové služby A6 vysoké paměti a velikosti virtuální počítač A7. Další informace o těchto velikosti najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure].
* **Aktualizace balíčku pro knihovny Azure Libraries for Java knihovny.** To je založené na verzi 0.4.4 [API klienta služby Microsoft Azure].

### <a name="may-1-2013"></a>1. května 2013
Modul plug-in Azure pro Eclipse – může 2013 Preview vydala. Toto je hlavní aktualizaci doplňujícími verzi Azure SDK 2.0, který je nezbytný předpoklad a budou stahovat automaticky při instalaci modulu plug-in. Tato verze obsahuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od února 2013 Preview:

* **Automatické odesílání JDK a aplikační server a nasazení z úložiště Azure.** Nová možnost, který automaticky odešle vybraný JDK a aplikační server, v případě potřeby na účet zadaný úložiště Azure a nasadí tyto komponenty z tohoto umístění, místo vkládání v balíčku pro nasazení nebo nutnosti nahrávání uživatele pak ručně. Tato funkce běžně požadovaných může výrazně zvýšit snadné nasazení součásti JDK a serveru, zejména pro nové uživatele. Návod, který používá tyto možnosti, najdete v části [vytvoření aplikace Hello World služby Azure v prostředí Eclipse].
* **Centralizované sledování účtu úložiště a schopnost odkaz účty úložiště další snadno (prostřednictvím ovládacího prvku rozevíracího seznamu).** To platí pro více funkcí, které jsou závislé na úložiště, například JDK a nasazení součásti serveru a ukládání do mezipaměti. Další informace najdete v tématu [seznam účtů úložiště Azure].
* **Zjednodušené nastavení vzdáleného přístupu v publikovat na Průvodce cloudu.** Všechny, které musíte udělat je zadejte uživatelské jméno a heslo k povolení vzdáleného přístupu nebo necháte prázdné, aby vzdálený přístup zakázán.
* **Aktualizace balíčku pro knihovny Azure Libraries for Java knihovny.** To je založené na verzi 0.4.2 [API klienta služby Microsoft Azure].
* **Podpora pro trvalé relace v systému Windows Server 2012.** Trvalé relace dříve, fungoval pouze pro Windows Server 2008 R2, teď i cloudu spřažení relace podporu cílů operačního systému.
* **Vylepšení výkonu nahrávání balíčku.** I když sadu JDK a aplikačního serveru jsou součástí balíčku pro nasazení, nahrávání část procesu nasazení může být přibližně dvakrát tak rychlé porovnání s předchozími verzemi.

### <a name="february-8-2013"></a>8. února 2013
Tento modul plug-in Azure pro Eclipse – únor 2013 Preview vydala. To je menší aktualizace včetně oprav chyb, vylepšení řízené zpětnou vazbu použitelnost a některé nové funkce vzhledem k tomu, že náhled listopad 2012:

* Podpora pro nasazení JDKs, aplikační servery a libovolné další součásti z veřejných nebo privátních Azure blob úložiště stahování místo včetně v balíčku pro nasazení při nasazení v cloudu.
* Umožňuje změnit pořadí, ve kterém jsou zpracovány součásti uživatelské role, po přidání **nahoru** a **přesunout dolů** tlačítka v **součásti** části z **vlastnosti Azure Role**.
* Aktualizace **balíček pro knihovny Azure Libraries for Java** knihovny, na základě verze 0.4.0 [API klienta služby Microsoft Azure].

### <a name="november-5-2012"></a>5 listopad 2012
Tento modul plug-in Azure pro Eclipse – listopad 2012 Preview vydala. Toto je hlavní aktualizaci, která zahrnuje několik nových funkcí a také další opravy chyb a vylepšení použitelnost řízené zpětnou vazbu od září 2012 náhled:

* Podpora pro Microsoft Windows Server 2012 jako operační systém cloudu.
* Podpora pro Azure společně umístěné ukládání do mezipaměti podporu pro klienty memcached.
* Zahrnutí knihovny klienta Apache Qpid JMS využít zasílání zpráv na základě Azure AMQP.
* Lepší **nový projekt** průvodce s novou stránku na element end, která poskytuje uživatelům možnost rychle povolit na několik běžných klíčové funkce v jejich projektu: trvalé relace, ukládání do mezipaměti a vzdálené ladění.
* Automatické snížení instancí role 1 při spuštění v emulátoru služby výpočty v, aby nedocházelo ke konfliktům vazby port mezi instancemi serveru.

### <a name="september-28-2012"></a>28 září 2012
Tento modul plug-in Azure pro Eclipse – září 2012 Preview vydala. Tato aktualizace služby zahrnuje několik dalších opravy chyb od srpen 2012 náhled a také některé řízené zpětnou vazbu použitelnost vylepšení stávajících funkcí:

* Podpora pro Microsoft Windows 8 a Microsoft Windows Server 2012 jako operační systém vývoj řešení problémů, které dříve modul plug-in bránily správně funguje na těchto operačních systémech.
* Vylepšená podpora pro zadávání rozsahů portů koncový bod.
* Opravy chyb související s cesty k souboru obsahující mezery.
* Role kontextové nabídky vylepšení pro rychlejší přístup k nastavení konfigurace specifické role.
* Malé obecnější v **publikovat do cloudu** průvodce a řadu dalších opravy chyb.

### <a name="august-28-2012"></a>28 srpen 2012
Tento modul plug-in Azure pro Eclipse – srpen 2012 Preview vydala. Tato aktualizace služby zahrnuje další opravy chyb od červenec 2012 náhled a také několik řízené zpětnou vazbu použitelnost vylepšení stávajících funkcí:

* V dialogovém okně filtru služeb řízení přístupu Azure:
  * **Možnost vložení podpisový certifikát** v souboru WAR vaší aplikace, aby se zjednodušila nasazení cloudu.
  * **Možnost vytvořit certifikát podepsaný svým držitelem** v rámci služby ACS filtrovat uživatelského rozhraní. Další informace o filtru služeb řízení přístupu Azure najdete v tématu [postup ověření webového uživatele s Azure přístup k řízení služby pomocí Eclipse].
* V rámci Průvodce projektem Azure nasazení (platí také pro stránku vlastností role konfigurace serveru):
  * **Automatické zjišťování umístění JDK** ve vašem počítači (který můžete přepsat v případě potřeby).
  * **Automatické zjišťování serveru typu** když vyberete instalační adresář serveru aplikace.

### <a name="july-15-2012"></a>15. července 2012
Tento modul plug-in Azure pro Eclipse – červenec 2012 Preview, které řeší celou řadu nejvyšší prioritou chyby najít a/nebo hlášené uživatelé po vydání června 2012, vydala. Toto je pouze aktualizaci služby, jsou obsaženy žádné nové funkce.

### <a name="june-7-2012"></a>7. června 2012
Modul plug-in Azure pro Eclipse – červen 2012 CTP vydala. Nové funkce patří:

* **Průvodce vytvořením projektu nasazení Azure:** umožňuje vybrat JDK, aplikační server Java a aplikací v jazyce Java přímo v Průvodci vylepšené uživatelského rozhraní. Součástí konfigurace serveru se na poli můžete vybrat ze seznamu jsou Tomcat 6, Tomcat 7, GlassFish OSE 3, Jetty 7, Jetty 8, JBoss 6 a 7 JBoss (samostatně). Kromě toho můžete přizpůsobit seznam konfigurace serveru. Tomuto vylepšení uživatelského rozhraní je alternativa k přetahování a vyřazení komprimovaných souborů a překopírování spouštěcí skripty, která byla předtím hlavní přístup. Dané metody stále funguje bez problémů, ale pravděpodobně použije jenom pro pokročilejší scénáře.
* **Stránky vlastností konfigurace role serveru:** umožňuje snadno přepínat JDKs, Java aplikačních serverů a aplikací přidružených k nasazení po vytvoření projektu. Další informace najdete v tématu [vlastnosti konfigurace serveru].
* **&quot;Publikování do cloudu&quot; průvodce:** poskytuje snadný způsob, jak nasazení projektu do Azure přímo z prostředí Eclipse automatizaci dříve ruční těžký zrušení načítání přihlašovacích údajů, přihlašování k portálu pro správu Azure, odesílání balíček, atd. Příklad toho, jak přímo nasazení projektu do Azure, naleznete v části [vytvoření aplikace Hello World služby Azure v prostředí Eclipse].
* **Panel nástrojů Azure:** nástrojů Azure je nyní k dispozici v prostředí Eclipse, který obsahuje tlačítka, který vyvolat následující funkce:
  * ![][ic710879]**Spustit v emulátoru Azure**: spouští projekt v emulátoru.
  * ![][ic710880]**Resetovat emulátoru Azure**: Resetuje emulátor.
  * ![][ic710881]**Sestavení balíček Cloud pro Azure**: zkompiluje vašeho balíčku pro nasazení.
  * ![][ic710876]**Nový projekt nasazení Azure**: vytvoří nový projekt nasazení Azure.
  * ![][ic710882]**Publikovat do cloudu Azure**: publikuje projekt do Azure.
  * ![][ic710883]**Zrušit publikování**: odstraní vaše nasazení.
  * Mnoho z těchto tlačítek panelu nástrojů Azure se používají v [vytvoření aplikace Hello World služby Azure v prostředí Eclipse].
* **Knihovny Azure Libraries for Java:** nyní k dispozici v rámci jednoho balíčku pro knihovny Azure pro knihovna Java v prostředí Eclipse, doplňujícími instalace modulu plug-in a obsahuje všechny potřebné závislosti. Stačí přidat jeden odkaz na knihovnu v projektu jazyka Java a vy nemusíte nic stáhnout samostatně. Další informace najdete v tématu [instalaci sady nástrojů Azure pro Eclipse].
* **Microsoft JDBC 4.0 ovladač pro systém SQL Server k dispozici během instalace modulu plug-in:** během instalace nového modulu plug-in, můžete nainstalovat nejnovější verzi ovladač JDBC Microsoft pro systém SQL Server.
* **Azure přístup k řízení filtr služby k dispozici během instalace modulu plug-in:** tuto novou součást, obsaženy jako knihovny prostředí Eclipse v sadě nástrojů umožňuje webové aplikace v jazyce Java bezproblémově využívat výhody ze služby pro řízení přístupu Azure (ACS) ověřování pomocí různých zprostředkovatelů identity, jako je Google, Live.com a Yahoo!. Nebudete muset zapisovat logiku ověřování sami, jenom pár možností konfigurace a nechat filtr provést lifting těžký povolení uživatelům přihlášení pomocí služby ACS. Můžete se zaměřit pouze na psaní kódu, který umožňuje uživatelům přístup k prostředkům v závislosti na jejich identitu, jak ho vrátila do vaší aplikace filtr uvnitř objekt žádosti. Kurz použití filtru služby ACS, najdete v části [postup ověření webového uživatele s Azure přístup k řízení služby pomocí Eclipse].
* **Automatické zjišťování požadovaných součástí Azure SDK 1.7:** když vytvoříte nový projekt nasazení Azure, Azure SDK 1.7 budou automaticky staženy Pokud již není nainstalována.
* **Instance koncových bodů:** přístup koncový bod umožňuje přímé port pro komunikaci s zatížení vyrovnáváním instancí rolí. Přidáním koncových bodů instance prostřednictvím koncové body k dispozici prostřednictvím uživatelského rozhraní [koncové body vlastnosti] stránky. To pomáhá povolit vzdálené ladění a diagnostiky JMX pro konkrétní výpočetní běží v cloudu ve scénářích s více instancemi-instance nasazení. 
* **Součásti uživatelského rozhraní:** je jednodušší pro zkušené uživatele nastavit závislosti projektu mezi jednotlivé role Azure v projektu a dalších externím prostředkům, jako jsou projekty aplikací Java; také umožňuje snadno popsat logiku jejich nasazení. Další informace najdete v tématu [vlastnosti součásti].
* **Automatický upgrade předchozích verzích projekty:** otevřete pracovní prostor, který má projektu Azure vytvořené pomocí předchozí verze modulu plug-in, staré projekty bude zobrazen v prostředí Eclipse jako zavřené, protože předchozí verze projekty nejsou kompatibilní s novou verzí. Pokud se pokusíte otevřít některou z těchto staré projekty, spustí se Průvodce upgradem. Pokud jste upgrade, nový projekt, souhlasím s nimi **_Upgraded** připojeným k názvu, bude vytvořen a automaticky aktualizovat, aby fungoval s novou verzí. Nový projekt můžete přejmenovat, podle potřeby. Jako součást upgradu původní projekt se nezmění (a zůstane uzavřený).

### <a name="december-10-2011"></a>10. prosince 2011
Modul plug-in Azure pro Eclipse – prosince 2011 CTP vydala. Nové funkce patří:

* **Spřažení relace (&quot;trvalé relace&quot;) podporují:** pomáhá povolit stateful, clusterovaný aplikací v jazyce Java s právě jeden zaškrtávací políčko. Další informace najdete v tématu [spřažení relace].
* **Předem provedené spuštění skriptu ukázky:** pro nejoblíbenější Java servery (Tomcat, Jetty, JBoss, GlassFish), které vám může právě zkopírujte a vložte z adresáře ukázky vašeho projektu do vašeho skriptu spuštění.
* **Emulátor spuštění výstupu v reálném čase:** nyní můžete sledovat provedení všech kroků z vaší spouštěcí skript v okně konzoly vyhrazené, ukazuje, průběh a selhání ve vašem skriptu jako je proveden v Azure.
* **Automatická, šedé – java.exe monitorování:** , vynutí recyklaci role, když se java.exe zastaví, pomocí skriptu lightweight, předem vytvořené automaticky zahrnuty ve vašem nasazení.
* **Vzdálené ladění uživatelské rozhraní konfigurace aplikace v jazyce Java:** umožňuje snadno povolit vzdáleného ladicího programu Eclipse společnosti pro přístup k aplikaci v jazyce Java spuštěné v emulátoru nebo cloudu Azure, abyste mohli jednotlivé kroky a ladit kód v jazyce Java v reálném čase. Další informace najdete v tématu [ladění aplikací Azure v prostředí Eclipse].
* **Konfigurace prostředků místní úložiště uživatelského rozhraní:** tak, že už máte nakonfigurovat místní prostředky tak, že přímo manipulace s XML. Tato funkce také umožňuje přístup k cestě k souboru platná místní prostředek po jeho nasazení pomocí proměnné prostředí, které přímo z vašeho spuštění skriptu, můžete odkazovat. Další informace najdete v tématu [místní úložiště vlastnosti].
* **Konfigurace proměnné prostředí uživatelského rozhraní:** takže již nemusíte nastavení proměnných prostředí pomocí ruční úpravy konfigurace XML. Další informace najdete v tématu [vlastnosti proměnné prostředí].
* **Ovladač JDBC pro SQL Azure:** získá instalovaných pomocí modulu plug-in jako plně integrovaného knihovna Eclipse, umožňuje snazší programování s SQL Azure. 
* **Rychlé kontextové nabídky přístup k uživatelské rozhraní konfigurace role**: právě klikněte pravým tlačítkem na složku role a klikněte na tlačítko **vlastnosti**.
* **Vlastní Azure projekt a role složky ikony:** pro lepší viditelnost a usnadnil navigaci v rámci pracovního prostoru a projekt.

## <a name="see-also"></a>Viz také
Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:

* [Azure nástrojů pro Eclipse]
  * *Co je nového v sadě Azure nástrojů pro Eclipse (v tomto článku)*
  * [instalaci sady nástrojů Azure pro Eclipse]
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]
* [nástrojů Azure pro IntelliJ]
  * [Novinky v sadě Azure Toolkit pro IntelliJ]
  * [Instalace sady Azure Toolkit pro IntelliJ]
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[instalaci sady nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Novinky v sadě Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure přihlášení v pokyny pro Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[jak publikovat webovou aplikaci jako kontejner Docker pomocí sady nástrojů pro Azure pro Eclipse]: ./azure-toolkit-for-eclipse-publish-as-docker-container.md
[Správa účtů úložiště pomocí Průzkumníka Azure pro Eclipse]: ./azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer.md
[Správa virtuálních počítačů pomocí Průzkumníka Azure pro Eclipse]: ./azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer.md

[Středisko pro vývojáře Java]: http://go.microsoft.com/fwlink/?LinkID=699547

[Azul systémy webové stránky pro OpenJDK zulština]: http://go.microsoft.com/fwlink/?LinkId=402457
[koncové body služby Azure]: http://go.microsoft.com/fwlink/?LinkID=699526
[seznam účtů úložiště Azure]: http://go.microsoft.com/fwlink/?LinkID=699528
[vlastnosti součásti]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[vytvoření aplikace Hello World služby Azure v prostředí Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[ladění aplikací Azure v prostředí Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[nasazení velkých nasazeních]: http://go.microsoft.com/fwlink/?LinkID=699536
[koncové body vlastnosti]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[vlastnosti proměnné prostředí]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[modulu plug-in nástroje HDInsight pro Eclipse]: ./hdinsight/hdinsight-apache-spark-eclipse-tool-plugin.md
[postup ověření webového uživatele s Azure přístup k řízení služby pomocí Eclipse]: http://go.microsoft.com/fwlink/?LinkID=264703
[postup snižování zátěže protokolu SSL použití]: http://go.microsoft.com/fwlink/?LinkID=699545
[Instalace sady Azure Toolkit pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[místní úložiště vlastnosti]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[API klienta služby Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=280397
[vlastnosti konfigurace serveru]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[spřažení relace]: http://go.microsoft.com/fwlink/?LinkID=699548
[snižování zátěže protokolu SSL]: http://go.microsoft.com/fwlink/?LinkID=699549
[k vytvoření nového účtu úložiště]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
[virtuálního počítače a Cloud velikost služeb pro Azure]: http://go.microsoft.com/fwlink/?LinkId=466520

<!-- IMG List -->

[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710877]: ./media/azure-toolkit-for-eclipse-whats-new/ic710877.png
[ic710879]: ./media/azure-toolkit-for-eclipse-whats-new/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-whats-new/ic710880.png
[ic710881]: ./media/azure-toolkit-for-eclipse-whats-new/ic710881.png
[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710882]: ./media/azure-toolkit-for-eclipse-whats-new/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-whats-new/ic710883.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh694270.aspx -->
