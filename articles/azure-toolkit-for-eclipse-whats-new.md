---
title: "aaaWhat je nového v hello nástrojů Azure pro Eclipse"
description: "Další informace o hello nejnovější funkce v hello nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: d74eacfb75447a3d659a0c2dc2e247ae6e3ce1b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-hello-azure-toolkit-for-eclipse"></a>Co je nového v hello nástrojů Azure pro Eclipse
## <a name="azure-toolkit-for-eclipse-releases"></a>Azure nástrojů pro verze Eclipse
Tento článek obsahuje informace o hello různých verzí a nejnovější aktualizace toohello nástrojů Azure pro prostředí Eclipse.

> [!NOTE]
> Je také Azure nástrojů pro hello IntelliJ IDE. Další informace najdete v tématu [nástrojů Azure pro IntelliJ].
> 
> 

### <a name="april-14-2017"></a>14. dubna 2017
Hello nástrojů Azure pro Eclipse – vydání duben 2017 zahrnuje hello následující vylepšení:

* **Vylepšené přihlašovací prostředí Azure**: hello Azure Toolkit pro Eclipse nyní podporuje dvě metody protokolování do účtu Azure: *interaktivní* a *automatizovaná*. Další informace najdete v tématu [Azure přihlášení v pokyny pro hello nástrojů Azure pro Eclipse].
* **Publikování pomocí Docker kontejnery**: můžete teď publikování webových aplikací jako kontejnery Docker pomocí sady nástrojů Azure pro Eclipse. Další informace najdete v tématu [jak hello toopublish webové aplikace jako kontejner Docker pomocí nástrojů Azure pro Eclipse].
* **Správa účtů úložiště**: hello Azure Toolkit pro Eclipse nyní podporuje správu účtů úložiště z hello Průzkumník Azure. Další informace najdete v tématu [Správa účtů úložiště pomocí hello Průzkumník Azure pro Eclipse].
* **Virtual Machine Management**: hello Azure Toolkit pro Eclipse nyní podporuje správu virtuálních počítačů z hello Průzkumník Azure. Další informace najdete v tématu [Správa virtuálních počítačů pomocí hello Průzkumník Azure pro Eclipse].
* **Odebrání vzdálené ladění podporu**. Vzdálené ladění webových aplikací Java v Azure App Service byl odebrán z hello nástrojů Azure pro Eclipse; to bylo nezbytné tooresolve některé problémy, které zákazníci byly zaznamenat při použití hello toolkit.

### <a name="august-26-2016"></a>26 srpna 2016
Hello nástrojů Azure pro Eclipse – srpna 2016 verze zahrnuje hello následující vylepšení:

* **Vlastní JDK distribuce**. Hello nástrojů Azure pro prostředí Eclipse nyní podporuje zadání a nasazení kontejner Azure WebApp služby libovolný JDK verze tooyour:
  * Kromě toho toohello JDKs poskytovaný platformou Azure, můžete také z široký výběr zulština OpenJDK verze k dispozici v Azure Azul systémy.
  * Můžete také zadat distribuční JDK odešlete jako účet úložiště tooyour souboru ZIP.
* **Vylepšení toohello Azure Průzkumník**:
  * Podpora pro správu virtuálního počítače pomocí nového modelu Resource Manager Azure: můžete zobrazit seznam, vytvářet a odstraňovat virtuální počítače založené na správci prostředků aniž byste museli opustit hello IDE.
  * Podpora pro správu účtu úložiště blob pomocí Azure Resource Manager, které doplňuje hello stávající funkce pro správu "klasické" účty úložiště.
* **Ovladač Microsoft JDBC 6.0 pro SQL Server**. Tato aktualizace obsahuje nejnovější ovladač JDBC hello pro Microsoft SQL Server (verze 6.0), který je nyní zahrnuty jako knihovny, můžete snadno přidat tooyour Java projekty, a tím nahrazení hello starší verze.

### <a name="june-29-2016"></a>29. června 2016
Hello nástrojů Azure pro Eclipse –. června 2016 verze zahrnuje hello následující vylepšení:

* **Požadavek Java 8**. Hello nástrojů Azure pro prostředí Eclipse nyní vyžaduje Java 8, i když tento požadavek je pouze pro hello toolkit – vaše aplikace může pokračovat toouse všechny verze jazyka Java, které jsou podporovány službou Azure.
* **Podpora pro hello nejnovější Java JDKs**. nejnovější verze Hello hello Java JDKs jsou nyní podporovány hello nástrojů Azure pro Eclipse.
* **Podpora pro Azure SDK v2.9.1**. nejnovější verzi sady Azure SDK hello Hello je nyní hello minimální nezbytný předpoklad pro hello nástrojů Azure pro prostředí Eclipse.
* **Integrované ukázky**. Hello nástrojů Azure pro prostředí Eclipse teď nabízí několik ukázkových aplikací, které toohelp vývojáři mohli začít.
* **Integrace nástroje HDInsight**. Nástroje HDInsight Azure nyní jsou dodávané společně s hello nástrojů Azure pro prostředí Eclipse. Další informace najdete v tématu [modulu plug-in nástroje HDInsight pro Eclipse].
* **Vzdálené ladění webových aplikací Java**. Hello nástrojů Azure pro prostředí Eclipse teď podporuje vzdálené ladění webových aplikací Java v Azure App Service.
* **Podpora pro verzi Eclipse vzhled Luna hello.** Hello minimální požadované Eclipse IDE je nová verze vzhled Luna.

### <a name="april-12-2016"></a>12. dubna 2016
Hello nástrojů Azure pro Eclipse –. dubna 2016 verze zahrnuje hello následující vylepšení:

* **Podpora pro Azure SDK v2.9.0**. nejnovější verzi sady Azure SDK hello Hello je nyní hello minimální nezbytný předpoklad pro hello nástrojů Azure pro prostředí Eclipse.
* **Různé použitelnost, odezvy a vylepšení výkonu související podpora webové aplikace tooAzure**. Několik optimalizací výkonu v jak hello Toolkit komunikuje s Azure výsledek v rychlejšího uživatelského rozhraní.
* **Možnost toodelete kontejner existující webové aplikace v Azure z v prostředí Eclipse**. Hello nástrojů Azure pro prostředí Eclipse teď umožňuje toodelete existující kontejner webů Azure bez opuštění Eclipse.

### <a name="march-7-2016"></a>7. března 2016
Hello nástrojů Azure pro Eclipse –. března 2016 verze zahrnuje hello následující vylepšení:

* **Podpora pro rychlé nasazení lightweight aplikací Java**. Hello nástrojů Azure pro Eclipse teď podporuje hello rychlého nasazení lightweight aplikací v jazyce Java do Azure webové aplikace kontejnery, takže teď nasazení aplikací v jazyce Java přijímá sekund místo minut.
* **Podpora pro správu webové aplikace pomocí zobrazení Průzkumníka Azure hello**. Hello Azure Průzkumník v hello nástrojů se teď podporuje pro výpis, spuštění a zastavení Azure Web Apps.
* **Aktualizovat Tomcat a Jetty, OpenJDK zulština distribuce**. Hello nástrojů Azure pro prostředí Eclipse poskytuje podporu aktualizovaných verzí Tomcat a Jetty zulština OpenJDK pro nasazení Java do cloudové služby Azure.

### <a name="january-4-2016"></a>4 leden 2016
Hello nástrojů Azure pro Eclipse – leden 2016 verze zahrnuje hello následující vylepšení:

* **Podpora pro hello zulština OpenJDK aktualizace**. Další informace najdete v tématu hello [Azul systémy webové stránky pro hello zulština OpenJDK].
* **Aktualizovat distribuce Tomcat a Jetty**. distribuce Hello Jetty a Tomcat, které jsou k dispozici na Microsoft Azure pro použití s hello Azure Toolkit pro Eclipse byly aktualizovány.
* **Parita mezi Eclipse a IntelliJ sadách pro Azure funkce**. Hello nástrojů Azure pro Eclipse a hello [nástrojů Azure pro IntelliJ] teď podporují hello stejnou sadu funkcí.

### <a name="september-1-2015"></a>1. září 2015
Hello nástrojů Azure pro Eclipse – září 2015 verze zahrnuje hello následující vylepšení:

* **Podpora pro hello zulština OpenJDK aktualizace**. Další informace najdete v tématu hello [Azul systémy webové stránky pro hello zulština OpenJDK].
* **Aktualizovat distribuce Tomcat a Jetty**. distribuce Hello Jetty a Tomcat, které jsou k dispozici na Microsoft Azure pro použití s hello Azure Toolkit pro Eclipse byly aktualizovány. (Tyto distribuce vývojáři toocreate rychlý vývoj a testování projektů s hello nástrojů Azure pro prostředí Eclipse.
* **Podpora pro automaticky aktualizované odkazy Tomcat a Jetty**. V přidání toohello konkrétních verzí Tomcat a Jetty které jsou k dispozici v Azure, můžete odkazovat vývojáři nyní hello odkazované tooas distribuční "nejnovější (automatické aktualizaci)", který bude automaticky aktualizovat toohello nejnovější distribuce každou hlavní verzi Jetty nebo Tomcat hello příštím instance role jsou recyklovány. (Recyklace probíhá automaticky, ale vývojáři můžou aktivovat ručně recyklaci prostřednictvím hello portálu Azure.) Tato nová funkce znamená, že vývojáři nemají tooredeploy jejich aplikace toobe možné toohave aktualizovat jejich serverového softwaru. (
* Tato funkce je aktuálně určena pouze pro účely vývoj a testování nebo jiných zvláště důležitých aplikací a se nedoporučuje pro produkční prostředí.)
* **Azure Průzkumník pro objekty BLOB, fronty a tabulky v úložišti Azure**. To umožňuje vývojáři tooperform sadu běžné úlohy s jejich úložiště artefaktů přímo z hello Eclipse IDE. Příklad: odstraňování, nahrávání nebo stahování objekty BLOB.

### <a name="august-1-2015"></a>1. srpna 2015
Hello nástrojů Azure pro Eclipse – srpen 2015 verze zahrnuje hello následující vylepšení:

* **Application Insights instrumentace správy klíčů**. Tato aktualizace umožňuje tooacquire, vytvářet a spravovat klíče instrumentace Application Insights přímo z hello Eclipse IDE.
* **Ovladač Microsoft JDBC 4.1 pro SQL Server**. Tato aktualizace zahrnuje podporu pro nejnovější ovladač JDBC hello Microsoft SQL Server.
* **2.7 verzi hello Azure SDK**. Tento poslední aktualizace toohello Azure SDK je nové nezbytný předpoklad pro hello Toolkit při instalaci v systému Windows hello. (Všimněte si, že to není nutné v operačních systémech jiný systém než Windows).
* **Podpora pro aktualizaci hello v7 zulština OpenJDK**. Další informace najdete v tématu hello [Azul systémy webové stránky pro hello zulština OpenJDK].

### <a name="may-1-2015"></a>1 květen 2015
Hello nástrojů Azure pro Eclipse – květen 2015 verze zahrnuje hello následující vylepšení:

* **Vylepšené výběr serveru uživatelského rozhraní**. Tato verze zjednodušuje použití hello sady nástrojů hello na jiný systém než Windows operační systémy.
* **Podpora pro projekty Maven**. Tato verze podporuje projekty Maven jako aplikace, které hello toolkit můžete nasadit tooAzure a nakonfigurovat Application Insights.
* **2.6 verzi hello Azure SDK**. Tento poslední aktualizace toohello Azure SDK je nové nezbytný předpoklad pro hello Toolkit při instalaci v systému Windows hello. (Všimněte si, že to není nutné v operačních systémech jiný systém než Windows).
* **Nasazení upgradu místo opakované publikování**. Pokud znovu projekt nasazení při předchozí verze hello je již živá, hello toolkit teď používá Azure nasazení upgradu funkce místo vypínání hello předchozích nasazení a opětovné publikování od začátku, stejně jako ve hello posledních. To umožňuje vaše cloudové služby toorun bez přerušení, kdykoli je to možné, pomáhá dosažení vysoké dostupnosti i během aktualizace a urychluje hello procesu publikování znovu.
* **Podpora pro v8 hello nejnovější zulština OpenJDK: - aktualizovat 40**. Další informace najdete v tématu hello [Azul systémy webové stránky pro hello zulština OpenJDK].

### <a name="march-9-2015"></a>9. března 2015
Hello nástrojů Azure pro Eclipse – března 2015 verze zahrnuje hello následující vylepšení:

* **Podpora pro Mac, Ubuntu a dalších typů Linux**. Tato verze hello nástrojů Azure pro prostředí Eclipse přidává podporu pro Mac OS a několik platforem Unix, takže vývojáři můžete nainstalovat hello toolkit toocreate, konfigurace a publikování Java projekty tooAzure cloudové služby (PaaS) z Eclipse běžící na operačních systémech jiné než Windows.

> [!NOTE]
> Tato funkce je ve verzi preview a není doporučeno pro použití v produkčním prostředí. Neexistuje žádná podpora zákaznickou smlouvu o úrovni služeb (SLA), ale všechny zpětná vazba je vezme v úvahu a podporovat.
> 
> 

* **Nový modul plug-in Application Insights**. Vývojáři jsou teď dokáže tooconfigure telemetrii automatické serveru pomocí Application Insights na Azure.
* **Automatizace nasazení na základě ant příkazového řádku**. Tato funkce umožňuje vývojářům tooautomate hello publikování novějších verzích jejich nasazení pomocí Ant mimo prostředí Eclipse. Předem generovaného skriptu se automaticky nakonfiguruje pro projekt po hello při prvním nasazení z prostředí Eclipse a následné nasazení můžete použít hello skriptu toofully automatizovat nasazení prostřednictvím pouze hello příkazového řádku.
* **Tomcat a Jetty dostupnosti v Azure pro jednodušší a rychlejší nasazení**. Vývojáři nyní můžete odkazovat různé Tomcat a Jetty verze, které jsou k dispozici v Azure přímo místo toho, že tooupload účty tootheir Java serveru (nebo prostřednictvím hello Toolkit), takže nejsou bez nutnosti tooupload serveru Java pro rychlý a Začínáme scénáře.
* **Metoda zástupce pro publikování Java webové aplikace tooAzure cloudové služby**. tooreduce hello křivku pro jednoduché vývojová a testovací scénáře, vývojáři mohou nyní publikovat aplikací Java tooAzure více přímo. Místo nutnosti toogo prostřednictvím hello procesu vytváření a konfigurace projektu nasazení Azure, bude aplikace nasadit s výchozí instanci v8: Tomcat a zulština JVM (OpenJDK).

### <a name="january-30-2015"></a>30 leden 2015
Hello nástrojů Azure pro Eclipse – leden 2015 verze zahrnuje hello následující vylepšení:

* **Podpora jádra Liberty serveru aplikace IBM® WebSphere®**. Tato verze přidává hello jádra Liberty IBM WebSphere aplikace serveru toohello seznamu podporované aplikační servery, ze které hello nástrojů je možné toodeploy tooAzure. Nejnovější přidání rozšíří hello aktuální seznam aplikační servery, které jsou podporovány &quot;out box&quot; podle hello nástrojů, které již součástí různých verzí Tomcat, Jetty, JBoss a GlassFish.
* **Zahrnutí Application Insights SDK**. Tato knihovna nově vydané klientského rozhraní API (v0.9.0) je teď součástí hello balíček pro knihovny Azure Libraries for Java.
* **Aktualizoval balíček pro knihovny Azure Libraries for Java**. Tato aktualizace zahrnuje knihovny Azure pro Java v0.7.0 a rozhraní API klienta úložiště v2.0.0 i hello nově vydané v0.9.0 Application Insights SDK.

### <a name="november-12-2014"></a>12. listopadu 2014
Hello nástrojů Azure pro Eclipse – verze v listopadu 2014 zahrnuje hello následující vylepšení:

* **Podpora pro Azure SDK, 2.5**. Tento poslední aktualizace toohello Azure SDK je hello nové nezbytný předpoklad pro hello Toolkit.
* **Podpora pro aktualizovanou verzi hello zulština OpenJDK v1.8, v1.7 a v1.6 balíčky**. Další informace najdete v tématu hello [Azul systémy webové stránky pro hello zulština OpenJDK].
* **Podpora pro hello nové standardní D velikosti pro cloudové služby**, který nabízí vyšší výkon a další paměťové prostředky. Další informace najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure].

### <a name="october-17-2014"></a>17 říjen 2014
Hello nástrojů Azure pro Eclipse – říjen 2014 verze zahrnuje hello následující vylepšení:

* **Vylepšení výkonu v hello publikovat tooCloud scénářích**. Načítání informace o předplatném je mnohem rychlejší, když uživatelé mají více předplatných a účtů úložiště.
* **Podpora pro aktualizovanou verzi balíčku v1.8 zulština OpenJDK hello**. Další informace najdete v tématu hello [Azul systémy webové stránky pro hello zulština OpenJDK].
* **Podpora pro starší verze 3. stran JDKs místo začne**. Zastaralý JDK balíčky se nebude zobrazovat v rozevírací nabídce hello pro nové projekty nasazení. Existující projekty odkazující na zastaralý JDK balíčky bude pokračovat se může toodo tak dobu hello probíhá, ale doporučujeme tooupgrade takové projekty toorely na hello nejnovější.
* **Aktualizovanou verzi hello balíček pro knihovny Azure pro Java klientské rozhraní API knihovny**. Další informace najdete v tématu hello [API klienta služby Microsoft Azure].
* **Opravy chyb.** Tato verze obsahuje několik ostatní opravy chyb, které jsou založené na uživatele sestavy a testování.

### <a name="august-5-2014"></a>5 srpen 2014
Hello nástrojů Azure pro Eclipse – srpen 2014 verze obsahuje následující vylepšení hello

* **Podpora pro Azure SDK 2.4.** Starší verze hello nástrojů Eclipse nebude fungovat s touto sadou SDK nově vydané.
* **Aktualizované verze hello zulština OpenJDK v1.6, 1.7 a v1.8 balíčky.** Další informace najdete v tématu hello [Azul systémy webové stránky pro hello zulština OpenJDK].
* **Aktualizovanou verzi hello balíček pro knihovny Azure pro Javu klientské rozhraní API knihovny.** Další informace najdete v tématu hello [API klienta služby Microsoft Azure].
* **Podporu pro nejnovější publikovat nastavení formátu souboru.** Byla přidána podpora pro verze 2.0 hello formát souboru nastavení publikování.
* **Architektury změny za hello publikovat tooCloud funkce.** Hello nástrojů má nyní API klienta služby Microsoft Azure pro jazyk Java pomocí hello nově vydané pro jeho podporu publikovat do cloudu.
* **Opravy chyb.** Tato verze obsahuje několik-uživatel si vyžádal opravy chyb.

### <a name="june-12-2014"></a>12. června 2014
Hello nástrojů Azure pro Eclipse – verze června 2014 je dílčí servisní aktualizace, která poskytuje hello následující vylepšení:

* **Podpora pro hello zulština OpenJDK balíček v1.8.** Další informace najdete v tématu hello [Azul systémy webové stránky pro hello zulština OpenJDK].
* **Aktualizované verze hello zulština OpenJDK v1.6 a 1.7 balíčků.** Další informace najdete v tématu hello [Azul systémy webové stránky pro hello zulština OpenJDK].
* **Aktualizovanou verzi hello balíček pro knihovny Azure pro Javu klientské rozhraní API knihovny.** Další informace najdete v tématu hello [API klienta služby Microsoft Azure].
* **Opravy chyb.** Tato verze obsahuje několik-uživatel si vyžádal opravy chyb.

### <a name="april-4-2014"></a>4. dubna 2014
Hello modul plug-in Azure pro Eclipse – vydání duben 2014 vydala. Toto je aktualizace doplňujícími hello verzi hello Azure SDK 2.3, který je nezbytný předpoklad a stáhnou automaticky při instalaci modulu plug-in hello. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od hello náhled únor 2014:

* **Podpora pro verze hello Azure SDK 2.3.** Hello modul plug-in Azure pro Eclipse – vydání duben 2014 vyžaduje Azure SDK 2.3. Při použití nového modulu plug-in webu hello, pokud jste již nemají Azure SDK 2.3, budete mít výzvami tooallow jeho instalace. Nepoužívejte Azure SDK 2.3 s dřívějšími verzemi nástroje hello modulu plug-in.
* **Upgrade aplikací bez dokončení balíčku nasazení.** Při nasazování aplikací Java, které jsou součástí projektu, modul plug-in hello nyní automaticky odesílá je do vaší vybraný účet úložiště, aby mohli aktualizovat a bez nutnosti recyklovat hello role instance toodeploy hello nejnovější aplikace bits toorebuild a znovu ho zaveďte hello celý balíček.
* **Tomcat 8 je nyní server známou aplikací.** Pokud vyberete instalační adresář Tomcat 8 na počítači v hello **Server** kartě hello **projekt nasazení Azure** dialogu hello modul plug-in bude nyní automaticky zjišťovat a být schopný toodeploy Tomcat 8 automatizovaně, podobně jako toohello starších verzí Tomcat již v seznamu hello.
* **Aktualizace balíčků Azul zulština OpenJDK: 47 aktualizací v1.6 a v1.7 aktualizací 51.** Efektivní u této verze, aktualizace balíčku v7 otevřete JDK zulština Azul systému 51 je k dispozici. Otevřete JDK zulština v6 balíčky spustit i, který je k dispozici od aktualizace 47. Tyto aktualizace jsou kromě toohello dříve k dispozici otevřete JDK zulština v7 aktualizace balíčku 45, aktualizací 40 nebo 25.
* **Podpora pro velikosti A8 a A9 virtuální počítač Microsoft Azure.** Teď můžete nasadit cloudové služby toohello vysoké paměti A8 a A9 virtuálního počítače velikosti. Další informace o těchto velikosti virtuálních počítačů najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure].
* **Automatické přesměrování z tooHTTPS HTTP pro protokol SSL povolen role.** Když cloudové služby obsahuje pouze HTTPS rolí, pokud požadavek uživatele hello určuje HTTP, přesměruje automaticky tooHTTPS. Neexistuje žádné toocreate potřeba samostatné role toohandle hello HTTP žádosti.
* **Expresní emulátor používá pro místní emulace.** Hello emulátoru Express Azure se teď používá jako emulátor hello při ladění aplikace místně.
* **Azure má byla přejmenované jako Microsoft Azure.** Obrazovky uživatelského rozhraní nyní projeví, že Azure má byla přejmenované a už názvem Azure.

### <a name="february-6-2014"></a>6. února 2014
Hello modul plug-in Azure pro Eclipse – únor 2014 Preview vydala. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od hello října 2013 Preview:

* **Podpora pro snižování zátěže protokolu SSL.** Zabezpečené snižování zátěže Sockets Layer (SSL) přidaná jako funkci, díky tomu můžete povolit tooeasily, které podporují protokol Secure HTTPS (Hypertext Transfer) ve vašem nasazení Java v Azure, aniž by bylo nutné tooconfigure SSL v aplikační server Java. To je zvlášť důležité v spřažení relace nebo ověřit scénáře komunikace. Například při použití hello služby Řízení přístupu (ACS) filtrovat, který je již nepodporuje hello toolkit. Další informace najdete v tématu [snižování zátěže protokolu SSL] a [jak tooUse snižování zátěže protokolu SSL].
* **GlassFish 4 je nyní server známou aplikací.** Pokud vyberete instalační adresář GlassFish 4 na počítači v hello **Server** kartě hello **projekt nasazení Azure** dialogu hello modul plug-in bude nyní automaticky zjišťovat a být schopný toodeploy GlassFish OSE 4 automatizovaně, podobně jako verze toohello GlassFish OSE 3 již v seznamu hello.
* **Aktualizace balíčku Azul zulština OpenJDK 45.** Efektivní v této verzi, Azul systému zulština (balíček v7 otevřete JDK) aktualizace 45 je nyní k dispozici. Toto je kromě toohello dříve k dispozici aktualizace 40 a aktualizovat 25.
* **Podpora pro 'auto' pro privátní porty koncových bodů.** Můžete nastavit privátní port tooautomatic pro vstupních koncových bodů a vnitřních koncových bodů toolet Azure automaticky přiřadit toothat koncový bod portu. Dříve může přiřadit pouze konkrétní číslo portu.
* **Podpora pro přizpůsobení hello certifikátu název (CN) v hello podepsaný certifikát vytvoření uživatelského rozhraní.** Dříve hello, který byl použit stejný název pevně pro všechny nové certifikáty; Teď můžete zadat vlastní certifikát název toohelp rozlišit mezi více certifikátů v hello portál Azure používá pro jiné účely.
* **Panel nástrojů Azure:** hello Azure nástrojů má aktualizované s hello následující změny: 
  * ![][ic710876]Tato ikona se přidal pro hello **nový projekt nasazení Azure**.
  * ![][ic710877]Tato ikona se přidal jako dialogové okno Vytvoření zástupce toohello certifikát podepsaný svým držitelem.
* **Podpora pro virtuální počítač A5 Azure velikost.** Teď můžete nasadit cloudové služby toohello vysoké paměti velikost A5 virtuálního počítače. Další informace o této velikosti virtuálního počítače najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure].
* **Podpora pro Microsoft Windows Server 2012 R2.** Windows Server 2012 R2 můžete nyní vybrat jako hello cloudu operační systém.

### <a name="october-22-2013"></a>22 říjen 2013
Hello modul plug-in Azure pro Eclipse – říjen 2013 Preview vydala. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od hello ze září 2013 Preview:

* **Podpora pro verze hello Azure SDK 2.2.** Hello modul plug-in Azure pro Eclipse – říjen 2013 Preview podporuje Azure SDK 2.2. modul plug-in Hello budou i nadále fungovat s Azure SDK 2.1 a bude automaticky nainstalovat Azure SDK 2.2, pokud již nemáte alespoň Azure SDK 2.1 nainstalovaný.
* **Aktualizace balíčku Azul zulština OpenJDK 40.** Jako oznámila pro hello září 2013 Preview, modul plug-in hello teď umožňuje pomocí zadané třetích stran JDK přímo v Azure, aniž by bylo nutné tooupload vlastní JDK. V hello října 2013 verze systému Azul zulština (balíček v7 otevřete JDK) 40 je k dispozici aktualizace; To je v přidání toohello původně publikované aktualizovat 25.
* **Odkaz na nasazení cloudu v hello protokol aktivit.** V rámci hello protokol činnosti Azure, když vaše nasazení je ve stavu **publikováno**, můžete kliknout na **publikováno** od je nyní nasazení tooyour odkaz; nasazení pak se otevřou v prohlížeči. (hello stav **publikováno** byl dříve označené **systémem**.)
* **Výběru OS cílů, které jsou k dispozici na publikování čas.** Hello **publikování tooAzure** dialogové okno obsahuje nové pole **cíl OS**, cílového operačního systému který poskytuje více zjistitelný způsob, jak můžete tooset.
* **Auto přepište předchozí nasazení.** Hello **publikování tooAzure** dialogové okno obsahuje nové zaškrtávací políčko, **přepsat předchozí nasazení**. Pokud zaškrtnete toto políčko, pokud je publikována nové nasazení automaticky se přepíšou hello předchozích nasazení; nebude mít &quot;409 – konflikt&quot; problémy při publikování toohello stejné místo bez první zrušení publikování hello předchozí nasazení.
* **Jetty 9 je nyní server známou aplikací.** Pokud vyberete instalační adresář Jetty 9 na počítači v hello **Server** kartě hello **projekt nasazení Azure** dialogu hello modul plug-in bude nyní automaticky zjišťovat a být schopný toodeploy Jetty 9 v Automatické způsobem, podobně jako toohello starší verze Jetty již v seznamu hello.
* **Přidání role z místní nabídky projektu hello.** Hello **Azure** místní nabídky projektu nyní obsahuje novou položku nabídky, **přidat roli**, který nabízí rychlejší a další zjistitelný způsob, jak můžete tooadd nové role tooyour projektu Azure.
* **Aktualizaci toohello balíčku pro hello knihovny Azure pro knihovna Java.** To je založené na verzi 0.4.6 hello [API klienta služby Microsoft Azure].

### <a name="september-25-2013"></a>25 ze září 2013
Hello modul plug-in Azure pro Eclipse – září 2013 Preview vydala. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od hello srpen 2013 Preview:

* **Možnost toodeploy hello Azul zulština OpenJDK balíčku k dispozici v Azure.** Byla přidána nová možnost, při zadávání hello JDK toouse s nasazením Azure. Použití této možnosti můžete nasadit balíček JDK třetích stran přímo na hello cloudu Azure, bez nutnosti tooupload vlastní. Systémy Azul poskytuje hello nejprve takový balíček s názvem zulština, podle hello OpenJDK, které teď můžou být nasazené pomocí této možnosti.
* **Aktualizaci toohello balíčku pro hello knihovny Azure pro knihovna Java.** To je založené na verzi 0.4.5 hello [API klienta služby Microsoft Azure].

### <a name="august-1-2013"></a>1. srpna 2013
Hello modul plug-in Azure pro Eclipse – srpen 2013 Preview vydala. Toto je aktualizace doplňujícími hello verzi hello Azure SDK 2.1, který je nezbytný předpoklad a stáhnou automaticky při instalaci modulu plug-in hello. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od hello července 2013 Preview:

* **Odebrání možnosti tooinclude hello místní JDK a místní aplikační server jako součást balíčku pro nasazení hello.** Stahování hello JDK a aplikační server, z cloudového úložiště během nasazení hello je vhodnější tooembedding těchto součástí v hello balíček od stahování hello položky výsledky menší časů velikost a rychlejší nasazení balíčku nasazení a jednodušší údržby. V důsledku toho byly odebrány hello možnosti tooinclude hello JDK a aplikační server, v balíčku pro nasazení hello. Existující projekty, které byly nakonfigurované tooinclude hello místní JDK a místní aplikační server jako součást balíčku pro nasazení hello bude automaticky převedený tooauto – nahrání hello JDK a aplikace serveru toocloud úložiště.
* **Podpora pro verze hello Azure SDK 2.1.** Hello modul plug-in Azure pro Eclipse – srpen 2013 Preview vyžaduje Azure SDK 2.1. Nepoužívejte hello srpen 2013 preview s dřívějšími verzemi nástroje hello Azure SDK a nepoužívejte Azure SDK 2.1 s dřívějšími verzemi nástroje hello modul plug-in Azure pro Eclipse.
* **Podpora pro verzi Eclipse Kepler hello.** Související toothis hello nové minimální požadovaná verze Eclipse IDE je indigově modré. modul plug-in Azure pro prostředí Eclipse Hello je už oficiálně otestovali na Helios.

### <a name="july-3-2013"></a>3. července 2013
Hello modul plug-in Azure pro Eclipse – červenec 2013 Preview vydala. Tato aktualizace zahrnuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od hello může 2013 Preview:

* **Možnost toocreate nový účet úložiště.** A **nový** tlačítko přidala toohello **přidat účet úložiště** dialogové okno. To vám umožní toocreate účet úložiště v rámci hello Eclipse modul plug-in, aniž by bylo nutné toolog v toohello portálu pro správu Azure. (Předplatnému Azure toouse již musí mít tuto funkci.) Další informace o vytvoření nového účtu úložiště najdete v tématu [toocreate nový účet úložiště].
* **Nové &quot;(automaticky)&quot; možnost pro účet úložiště používané pro automatické nasazení JDK a serveru a pro ukládání do mezipaměti.** Při použití hello **automaticky odeslat** možnost pro hello JDK a aplikační server, teď můžete zadat **(automaticky)** pro hello adresy URL a úložiště účet toouse při nahrávání hello JDK a aplikační server nebo pokud používáte Azure ukládání do mezipaměti. Potom tyto funkce budou automaticky používat hello stejný účet úložiště, jako ten, který jste vybrali v hello hello **publikování tooAzure** dialogové okno. Hello [vytvoření aplikace Hello World služby Azure v prostředí Eclipse] kurz byl aktualizovaný toouse hello nové **(automaticky)** možnost.
* **Možnost tooset koncové body služby Azure.** Zadejte koncové body hello služby, které určují, že zda je aplikace nasazená tooand spravuje hello globální platformy Azure, Azure provozována společností 21Vianet v Číně, nebo soukromé platformy Azure. Další informace najdete v tématu [koncové body služby Azure].
* **Nasazení ve velkých organizacích, můžete zadat místní úložiště prostředků.** V hello událost, která vaše nasazení je příliš velký toobe obsažené ve složce approot výchozí hello, teď můžete zadat místní úložiště prostředků jako cíl hello nasazení pro vaše JDK a aplikačního serveru. Další informace najdete v tématu [nasazení velkých nasazeních].
* **Podpora pro velikosti A6 a virtuální počítač A7 Azure.** Teď můžete nasadit virtuální počítač A7 velikost a cloudové služby toohello vysoké paměti A6. Další informace o těchto velikosti najdete v tématu [virtuálního počítače a Cloud velikost služeb pro Azure].
* **Aktualizaci toohello balíčku pro hello knihovny Azure pro knihovna Java.** To je založené na verzi 0.4.4 hello [API klienta služby Microsoft Azure].

### <a name="may-1-2013"></a>1. května 2013
Hello Azure modul plug-in pro Eclipse – může 2013 Preview vydala. Toto je hlavní aktualizace doplňujícími hello verzi hello Azure SDK 2.0, který je nezbytný předpoklad a stáhnou automaticky při instalaci modulu plug-in hello. Tato verze obsahuje nové funkce, oprav chyb a vylepšení některé použitelnost řízené zpětnou vazbu od hello února 2013 Preview:

* **Automatické odesílání hello JDK a aplikační server a nasazení z úložiště Azure.** Nová možnost, která automaticky nahrává hello vybrané JDK a aplikační server, v případě potřeby, tooa zadaný účet úložiště Azure a nasadí tyto komponenty z ní místo vkládání v balíčku pro nasazení hello nebo nutnosti hello uživatele nahrávání pak ručně. Tato funkce běžně požadovaných může výrazně zvýšit hello snadné nasazení hello JDK a součásti serveru, zejména pro nové uživatele. Návod, který používá tyto možnosti, najdete v části [vytvoření aplikace Hello World služby Azure v prostředí Eclipse].
* **Centralizované sledování účet úložiště a možnost tooreference účty úložiště snadno (prostřednictvím ovládacího prvku rozevíracího seznamu).** To platí toomultiple funkce, které jsou závislé na úložiště, například JDK a nasazení součásti serveru a ukládání do mezipaměti. Další informace najdete v tématu [seznam účtů úložiště Azure].
* **Zjednodušené nastavení vzdáleného přístupu v Průvodci tooCloud publikovat hello.** Stačí toodo je zadejte uživatelské jméno a heslo tooenable vzdáleného přístupu nebo nechte ho prázdný tookeep vzdálený přístup zakázán.
* **Aktualizaci toohello balíčku pro hello knihovny Azure pro knihovna Java.** To je založené na verzi 0.4.2 hello [API klienta služby Microsoft Azure].
* **Podpora pro trvalé relace v systému Windows Server 2012.** Trvalé relace dříve, fungoval pouze pro Windows Server 2008 R2, teď i cloudu spřažení relace podporu cílů operačního systému.
* **Vylepšení výkonu nahrávání balíčku.** I když hello JDK a aplikační server, jsou součástí balíčku pro nasazení hello, hello nahrávání část procesu nasazení hello může být přibližně dvakrát tak rychlý jako porovnání tooprevious verze.

### <a name="february-8-2013"></a>8. února 2013
Hello modul plug-in Azure pro Eclipse – únor 2013 Preview vydala. To je menší aktualizace včetně oprav chyb, vylepšení řízené zpětnou vazbu použitelnost a některé nové funkce od hello náhled listopad 2012:

* Podpora pro nasazení JDKs, aplikační servery a libovolné další součásti z veřejných nebo privátních Azure blob úložiště stahování místo včetně v balíčku pro nasazení hello při nasazování toohello cloudu.
* Možnost toochange hello pořadí součásti uživatelské role jsou zpracování, prostřednictvím hello přidání **nahoru** a **přesunout dolů** tlačítka v hello **součásti** část hello **vlastnosti Role Azure**.
* Aktualizaci toohello **balíček pro hello knihovny Azure Libraries for Java** knihovny, na základě verze 0.4.0 hello [API klienta služby Microsoft Azure].

### <a name="november-5-2012"></a>5 listopad 2012
Hello modul plug-in Azure pro Eclipse – listopad 2012 Preview vydala. Toto je hlavní aktualizaci, která zahrnuje několik nových funkcí, stejně jako opravy dalších chyb a vylepšení použitelnost řízené zpětnou vazbu od hello září 2012 verzi Preview:

* Podpora pro Microsoft Windows Server 2012 jako hello cloudu operační systém.
* Podpora pro Azure společně umístěné ukládání do mezipaměti podporu pro klienty memcached.
* Zahrnutí knihovny klienta Apache Qpid JMS hello využít zasílání zpráv na základě Azure AMQP.
* Lepší **nový projekt** průvodce s novou stránku na konci hello, která poskytuje uživatelům možnost tooquickly hello povolit několik běžných klíčové funkce v jejich projektu: trvalé relace, ukládání do mezipaměti a vzdálené ladění.
* Automatické snížení too1 instancí role při spuštění v emulátoru služby výpočty hello, tooavoid port vazby konflikty mezi instancemi serveru.

### <a name="september-28-2012"></a>28 září 2012
Hello modul plug-in Azure pro Eclipse – září 2012 Preview vydala. Tato aktualizace služby zahrnuje několik dalších opravy chyb od hello srpen 2012 náhled a také některé řízené zpětnou vazbu použitelnost vylepšení stávajících funkcí:

* Podpora pro Microsoft Windows 8 a Microsoft Windows Server 2012 jako hello vývoj operační systém, řešení problémů, které dříve modulu plug-in hello bránily správně funguje na těchto operačních systémech.
* Vylepšená podpora pro zadávání rozsahů portů koncový bod.
* Opravy chyb souvisejících s toofile cesty obsahující mezery.
* Role kontextové nabídky vylepšení pro nastavení konfigurace specifických toorole rychlejší přístup.
* Malé obecnější v hello **publikování toocloud** průvodce a řadu dalších opravy chyb.

### <a name="august-28-2012"></a>28 srpen 2012
Hello modul plug-in Azure pro Eclipse – srpen 2012 Preview vydala. Tato aktualizace služby zahrnuje další opravy chyb od hello červenec 2012 náhled a také několik řízené zpětnou vazbu použitelnost vylepšení stávajících funkcí:

* V dialogovém okně filtru služeb řízení přístupu Azure hello:
  * **Možnost tooembed hello podpisový certifikát** v souboru WAR vaší aplikace, toosimplify cloudové nasazení.
  * **Možnost toocreate certifikát podepsaný svým držitelem** v rámci hello ACS filtrovat uživatelského rozhraní. Další informace o hello filtru služeb řízení přístupu Azure najdete v tématu [jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse].
* V rámci hello – Průvodce projektem Azure nasazení (platí také stránka vlastností konfigurace serveru toohello role):
  * **Automatické zjišťování hello JDK umístění** ve vašem počítači (který můžete přepsat v případě potřeby).
  * **Automatické zjišťování serveru typu hello** když vyberete instalační adresář serveru aplikace hello.

### <a name="july-15-2012"></a>15. července 2012
Hello modul plug-in Azure pro Eclipse – červenec 2012 Preview, které řeší celou řadu hello nejvyšší prioritou chyby najít a/nebo hlášené uživatelé po hello verze června 2012, vydala. Toto je pouze aktualizaci služby, jsou obsaženy žádné nové funkce.

### <a name="june-7-2012"></a>7. června 2012
Modul plug-in Azure pro Eclipse – červen 2012 CTP vydala. Nové funkce patří:

* **Průvodce vytvořením projektu nasazení Azure:** umožňuje vám tooselect JDK, aplikační server Java a aplikací v jazyce Java přímo v Průvodci vylepšené hello uživatelského rozhraní. Součástí hello seznam toochoose konfigurace serveru se na pole z jsou Tomcat 6, Tomcat 7, GlassFish OSE 3, Jetty 7, Jetty 8, JBoss 6 a 7 JBoss (samostatně). Kromě toho můžete přizpůsobit hello seznam konfigurace serveru. Toto vylepšení uživatelského rozhraní je alternativní toodragging a odstranit komprimovaných souborů a překopírování spouštěcí skripty, které se dříve hello hlavní přístup. Dané metody stále funguje bez problémů, ale pravděpodobně použije jenom pro pokročilejší scénáře.
* **Stránky vlastností konfigurace role serveru:** vám umožní tooeasily přepínač hello JDKs, Java aplikačních serverů a aplikací přidružených k nasazení po vytvoření projektu hello. Další informace najdete v tématu [vlastnosti konfigurace serveru].
* **&quot;Publikování toocloud&quot; průvodce:** poskytuje snadný způsob toodeploy vašeho projektu tooAzure přímo z prostředí Eclipse, automatizace hello dříve ruční těžký zrušení načítání přihlašovacích údajů, přihlášení toohello portálu pro správu Azure, nahrávání balíčku, atd. Příklad nasazení vašeho projektu tooAzure toodirectly, naleznete v části [vytvoření aplikace Hello World služby Azure v prostředí Eclipse].
* **Panel nástrojů Azure:** nástrojů Azure je nyní k dispozici v prostředí Eclipse, který obsahuje tlačítka, který vyvolat hello následující funkce:
  * ![][ic710879]**Spustit v emulátoru Azure**: spouští projekt v emulátoru hello.
  * ![][ic710880]**Resetovat emulátoru Azure**: resetování hello emulátor.
  * ![][ic710881]**Sestavení balíček Cloud pro Azure**: zkompiluje vašeho balíčku pro nasazení.
  * ![][ic710876]**Nový projekt nasazení Azure**: vytvoří nový projekt nasazení Azure.
  * ![][ic710882]**Publikování tooAzure cloudu**: publikuje tooAzure vašeho projektu.
  * ![][ic710883]**Zrušit publikování**: odstraní vaše nasazení.
  * Mnoho z těchto tlačítek panelu nástrojů Azure se používají v [vytvoření aplikace Hello World služby Azure v prostředí Eclipse].
* **Knihovny Azure Libraries for Java:** nyní k dispozici jako součást hello jeden balíček pro knihovny Azure pro knihovna Java v prostředí Eclipse, doplňujícími instalace modulu plug-in hello a obsahuje všechny hello potřebné závislosti. Stačí přidat jeden toohello knihovně odkazů v projektu jazyka Java a nepotřebujete toodownload nic samostatně. Další informace najdete v tématu [hello instalace nástrojů Azure pro Eclipse].
* **Microsoft JDBC 4.0 ovladač pro systém SQL Server k dispozici během instalace modulu plug-in:** během instalace nového modulu plug-in webu hello, lze nainstalovat nejnovější verzi hello hello ovladač JDBC Microsoft pro systém SQL Server.
* **Azure přístup k řízení filtr služby k dispozici během instalace modulu plug-in:** tuto novou součást, obsaženy jako knihovny prostředí Eclipse v hello nástrojů umožňuje Java webové aplikace tooseamlessly využít výhody ze služby pro řízení přístupu Azure (ACS) ověřování pomocí různých zprostředkovatelů identity, jako je Google, Live.com a Yahoo!. Nebudete potřebovat logiku ověřování toowrite sami, jenom pár možností konfigurace a nechat hello filtru provést hello velkou zrušení povolení toosign uživatelů pomocí služby ACS. Právě můžete soustředit na zápis hello kód, který poskytuje uživatelům přístup tooresources na základě jejich identity, jako vrácený tooyour aplikace hello filtru uvnitř hello objekt žádosti. Kurz týkající se použití hello ACS filtrovat, najdete v části [jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse].
* **Automatické zjišťování hello Azure SDK 1.7 požadovaných součástí:** když vytvoříte nový projekt nasazení Azure, Azure SDK 1.7 budou automaticky staženy Pokud již není nainstalována.
* **Instance koncových bodů:** přístup koncový bod umožňuje přímé port pro komunikaci s zatížení vyrovnáváním instancí rolí. Přidáním koncových bodů instance prostřednictvím koncových bodů hello uživatelského rozhraní, k dispozici prostřednictvím hello [koncové body vlastnosti] stránky. To pomáhá povolit vzdálené ladění a diagnostiky JMX pro konkrétní výpočetní instance spuštěné v cloudu hello ve scénářích s více – instance nasazení. 
* **Součásti uživatelského rozhraní:** usnadňuje tooset Pokročilí uživatelé si závislosti projektu mezi jednotlivé role Azure v projektu hello a dalších externím prostředkům, jako jsou projekty aplikací Java; také umožňuje snadno toodescribe jejich nasazení logiky . Další informace najdete v tématu [vlastnosti součásti].
* **Automatický upgrade předchozích verzích projekty:** při otevření pracovního prostoru, který má projektu Azure vytvořené pomocí předchozí verze modulu plug-in hello staré projekty hello se zobrazí v prostředí Eclipse jako zavřené, protože předchozí verze projekty nejsou kompatibilní s novou verzí hello. Když zkusíte tooopen jednoho z těchto staré projektů, spustí se Průvodce upgradem. Pokud souhlasíte s toohello upgradu nový projekt, **_Upgraded** připojením toohello název, bude vytvořena a automaticky aktualizovat toowork s novou verzí hello. Nový projekt hello můžete přejmenovat, podle potřeby. Jako součást upgradu hello původní projekt se nezmění (a zůstane uzavřený).

### <a name="december-10-2011"></a>10. prosince 2011
Modul plug-in Azure pro Eclipse – prosince 2011 CTP vydala. Nové funkce patří:

* **Spřažení relace (&quot;trvalé relace&quot;) podporují:** pomáhá povolit stateful, clusterovaný aplikací v jazyce Java s právě jeden zaškrtávací políčko. Další informace najdete v tématu [spřažení relace].
* **Předem provedené spuštění skriptu ukázky:** hello nejoblíbenější Java serverů (Tomcat, Jetty, JBoss, GlassFish), které vám může právě zkopírujte a vložte z adresáře ukázky vašeho projektu do vašeho skriptu spuštění.
* **Emulátor spuštění výstupu v reálném čase:** nyní můžete sledovat hello provedení všech kroků hello z vaší spouštěcí skript v okně konzoly vyhrazené ukazuje hello průběh a selhání ve vašem skriptu jako je proveden v Azure.
* **Automatická, šedé – java.exe monitorování:** , vynutí recyklaci role, když se java.exe zastaví, pomocí skriptu lightweight, předem vytvořené automaticky zahrnuty ve vašem nasazení.
* **Vzdálené ladění uživatelské rozhraní konfigurace aplikace v jazyce Java:** umožňuje tooeasily můžete povolit na Eclipse vzdáleného ladicího programu tooaccess Java aplikace spuštěné v hello emulátoru nebo hello cloudu Azure, abyste mohli jednotlivé kroky a ladit kód v jazyce Java v reálném čase. Další informace najdete v tématu [ladění aplikací Azure v prostředí Eclipse].
* **Konfigurace prostředků místní úložiště uživatelského rozhraní:** , již není místní prostředky tooconfigure manipulací hello XML přímo. Tato funkce také umožňuje tooaccess toohello efektivní cestu k souboru místní prostředek po jeho nasazení pomocí proměnné prostředí, které přímo z vašeho spuštění skriptu, můžete odkazovat. Další informace najdete v tématu [místní úložiště vlastnosti].
* **Konfigurace proměnné prostředí uživatelského rozhraní:** tak, že už máte tooset proměnné prostředí prostřednictvím ruční úpravy hello konfigurace XML. Další informace najdete v tématu [vlastnosti proměnné prostředí].
* **Ovladač JDBC pro SQL Azure:** získá instalovaných pomocí modulu plug-in hello jako plně integrovaného knihovna Eclipse, umožňuje snazší programování s SQL Azure. 
* **Uživatelské rozhraní konfigurace toorole přístup rychlé kontextové nabídky**: právě klikněte pravým tlačítkem na složku hello role a klikněte na tlačítko **vlastnosti**.
* **Vlastní Azure projekt a role složky ikony:** pro lepší viditelnost a usnadnil navigaci v rámci pracovního prostoru a projekt.

## <a name="see-also"></a>Viz také
Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující odkazy:

* [Azure nástrojů pro Eclipse]
  * *Co je nového v hello nástrojů Azure pro Eclipse (v tomto článku)*
  * [hello instalace nástrojů Azure pro Eclipse]
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
  * [Přihlášení v pokyny pro hello nástrojů Azure pro Eclipse]
* [nástrojů Azure pro IntelliJ]
  * [Co je nového v hello nástrojů Azure pro IntelliJ]
  * [Instalace hello Azure Toolkit pro IntelliJ]
  * [Přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java].

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[hello instalace nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Přihlášení v pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's New in hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure přihlášení v pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[jak hello toopublish webové aplikace jako kontejner Docker pomocí nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-publish-as-docker-container.md
[Správa účtů úložiště pomocí hello Průzkumník Azure pro Eclipse]: ./azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer.md
[Správa virtuálních počítačů pomocí hello Průzkumník Azure pro Eclipse]: ./azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer.md

[Azure střediska pro vývojáře Java]: http://go.microsoft.com/fwlink/?LinkID=699547

[Azul systémy webové stránky pro hello zulština OpenJDK]: http://go.microsoft.com/fwlink/?LinkId=402457
[koncové body služby Azure]: http://go.microsoft.com/fwlink/?LinkID=699526
[seznam účtů úložiště Azure]: http://go.microsoft.com/fwlink/?LinkID=699528
[vlastnosti součásti]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[vytvoření aplikace Hello World služby Azure v prostředí Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[ladění aplikací Azure v prostředí Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[nasazení velkých nasazeních]: http://go.microsoft.com/fwlink/?LinkID=699536
[koncové body vlastnosti]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[vlastnosti proměnné prostředí]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[modulu plug-in nástroje HDInsight pro Eclipse]: ./hdinsight/hdinsight-apache-spark-eclipse-tool-plugin.md
[jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse]: http://go.microsoft.com/fwlink/?LinkID=264703
[jak tooUse snižování zátěže protokolu SSL]: http://go.microsoft.com/fwlink/?LinkID=699545
[Instalace hello nástrojů Azure pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[místní úložiště vlastnosti]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[API klienta služby Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=280397
[vlastnosti konfigurace serveru]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[spřažení relace]: http://go.microsoft.com/fwlink/?LinkID=699548
[snižování zátěže protokolu SSL]: http://go.microsoft.com/fwlink/?LinkID=699549
[toocreate nový účet úložiště]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
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
