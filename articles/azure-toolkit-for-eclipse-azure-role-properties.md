---
title: Vlastnosti Azure Role
description: "Další informace o použití sady nástrojů Azure pro prostředí Eclipse ke konfiguraci nastavení Azure role."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5c0ec412-5702-465a-8f47-87a8ce99a267
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: cd734c64ba6d1394cb261bace92dee9dd579dd08
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-role-properties"></a>Vlastnosti Azure Role
Různá nastavení konfigurace pro vaši Azure roli můžete nastavit v rámci sady nástrojů Azure pro prostředí Eclipse.

## <a name="configuring-azure-role-properties"></a>Konfigurace vlastností Azure Role
Konfigurace vlastností Role vaše Azure se provádí pomocí vlastnosti dialogová okna pro své role pracovního procesu. Otevřete v podokně Průzkumník projektů Eclipse na místní nabídku pro roli a vyberte **Azure** dílčí nabídky. (Pokud nevidíte roli v prohlížeči projektu Eclipse, rozbalte projektu Azure v prohlížeči projektu.)

![][ic789599]

Různé vlastnosti, které lze nastavit z **vlastnosti** dialogová okna jsou popsané v tomto tématu. Všimněte si, že mnoho vlastností se vyplní automaticky při vytvoření nového projektu nasazení Azure.

Následující stránky vlastností jsou k dispozici pro Azure role.

* [Vlastnosti virtuálního počítače](#virtual_machine_properties)
* [Ukládání do mezipaměti vlastnosti](#caching_properties)
* [Vlastnosti certifikáty](#certificates_properties)
* [Vlastnosti součásti](#components_properties)
<!-- * [Debugging properties](#debugging_properties) -->
* [Vlastnosti koncové body](#endpoints_properties)
* [Vlastnosti proměnné prostředí](#environment_variables_properties)
* [Vyrovnávání zatížení nebo vlastnosti relace spřažení (známé jako "trvalé relace")](#session_affinity_properties)
* [Místní úložiště vlastností](#local_storage_properties)
* [Vlastnosti konfigurace serveru](#server_configuration_properties)
* [Vlastnosti snižování zátěže protokolu SSL](#ssl_offloading_properties)

<a name="virtual_machine_properties"></a>

### <a name="virtual-machine-properties"></a>Vlastnosti virtuálního počítače
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **vlastnosti**, a budete mít možnost změnit velikost virtuálního počítače a také změnit číslo instancí, jak je znázorněno na následujícím obrázku.

![][ic719499]

> [!NOTE]
> Pouze v systému Windows: Když nastavíte počtu instancí na hodnotu větší než 1 a taky nakonfigurovat aplikační server, sady nástrojů vám umožní pouze 1 instancí role spustit v emulátoru, bez ohledu na toto nastavení. Toto je konfliktům port vazbu mezi instancemi jiný server (například všechny pokusu vytvořit vazbu na port 8080) při jejich spuštění na stejném počítači. Vaše nastavení počtu požadované instance se zachová, ale přestane platit pouze v případě, že můžete nasadit do cloudu.
> 
> 

<a name="caching_properties"></a> 

### <a name="caching-properties"></a>Ukládání do mezipaměti vlastnosti
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **ukládání do mezipaměti**. V tomto dialogu můžete povolit pojmenované blízko memcache kompatibilní s mezipamětí, což umožňuje urychlit webových aplikací.

![][ic719483]

V rámci **ukládání do mezipaměti** stránce vlastností, můžete určit globální nastavení pro následující:

* zda je povoleno ukládání do mezipaměti společně umístěné.
* velikost mezipaměti jako procento paměti.
* název účtu úložiště pro ukládání stavu mezipaměti, když vaše aplikace běží jako cloudová služba, nebo none, pokud nechcete uložit stav mezipaměti. (Název účtu úložiště se nepoužívá při spuštění aplikace v emulátoru služby výpočty v.) Pokud nastavíte název účtu úložiště na **(automaticky)** (což je výchozí nastavení), konfiguraci ukládání do mezipaměti bude automaticky použít stejný účet úložiště jako ta, vyberte v **publikovat do Azure** dialogové okno.

> [!NOTE]
> **(Automaticky)** nastavení budou mít požadovaný účinek pouze v případě, že publikujete nasazení pomocí sady nástrojů Eclipse Průvodce publikováním. Pokud místo toho můžete publikovat soubor .cspkg ručně pomocí externího mechanismu, jako [portálu pro správu Azure][Azure Management Portal], nasazení nebude fungovat správně.
> 
> 

Následující dialogové okno se zobrazí vlastnosti mezipaměti.

![][ic719501]

* **Název:** název umístit do stejného umístění mezipaměti.
* **Číslo portu:** číslo portu pro mezipaměť používat.
* **Zásady vypršení platnosti:** jednu z následujících hodnot, která určuje, kdy vyprší platnost klíč v mezipaměti.
  * **Absolutní:** klíč vyprší po uplynutí určeného podle **minut za provozu** je dosaženo.
  * **NeverExpires:** klíč nemá čas vypršení platnosti.
  * **SlidingWindow:** klíč vyprší, pokud byla nepoužíval pro množství času určeného **minut za provozu**; pokaždé, když je přístupný, hodiny vypršení platnosti se resetuje.
* **Minut za provozu:** maximální počet minut pro klíč memcached TTL předmětem zásad vypršení platnosti.
* **Vysoká dostupnost s replikované zálohování na jinou roli instancí:** Pokud je povoleno, pomáhá poskytovat použití vysoké dostupnosti replikují zálohy u instancí jiné role. Všimněte si, že aspoň dvě instance role musí být platí pro vaše nasazení pro tuto funkci používat.

Chcete-li přidat novou mezipaměť, klikněte na tlačítko **přidat** tlačítka na **ukládání do mezipaměti** stránku vlastností a **konfigurace s názvem mezipaměti** otevře se dialogové okno. Zadejte hodnoty pro vlastnosti, které jsou popsané výše.

Chcete-li upravit pojmenované mezipaměti, vyberte mezipaměti a klikněte na **upravit** tlačítka na **ukládání do mezipaměti** stránka vlastností. Umožňuje upravit vlastnosti mezipaměti se otevře dialogové okno. Stiskněte klávesu **OK** uložit hodnot mezipaměti.

Pokud chcete odstranit mezipaměť, vyberte mezipaměti a klikněte na tlačítko **odebrat** tlačítka na **ukládání do mezipaměti** stránku vlastností a pak klikněte na **Ano** potvrďte odstranění.

Další informace o tom, jak používat ukládání do mezipaměti najdete v tématu [jak používat ukládání do mezipaměti Co-located][How to Use Co-located Caching].

<a name="certificates_properties"></a> 

### <a name="certificates-properties"></a>Vlastnosti certifikáty
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **certifikáty**.

![][ic710964]

V tomto dialogu můžete přidat nebo odebrat certifikáty odkazuje projektu Eclipse. Všimněte si, že nejsou automaticky uloženy uvnitř žádné Java keystore certifikáty tady a proto nejsou automaticky k dispozici pro jakékoli použití z v rámci aplikace Java. Jsou pouze registrované v Azure tak, aby může být zavedené do systému Windows certifikát úložiště na virtuálních počítačích s nasazením a následně použít jiný software Windows. V současné době pouze funkce sady nástrojů, která používá certifikáty odkazuje tímto způsobem v **certifikáty** dialogové okno je [snižování zátěže protokolu SSL][SSL Offloading], kvůli jeho závislá na Internetové informační služby (IIS) a požádat o směrování aplikace (ARR), které vyžadují vhodný certifikát, který se má zpřístupnit tímto způsobem.

Když nasadíte projekt do Azure pomocí Průvodce publikování, zobrazí se výzva tak, aby odkazoval na soubory Personal Information Exchange (PFX) odpovídající tyto certifikáty, spolu s jejich hesla, aby bylo možné automaticky jejich nahrávání do služby Azure , ale jenom v případě, že nebyly odeslány existuje dříve.

<a name="components_properties"></a> 

### <a name="components-properties"></a>Vlastnosti součásti
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **součásti**. V rámci toto dialogové okno máte možnost přidat, upravit, nebo odebrat součásti role, a také změnit pořadí, ve kterém jsou zpracovány.

![][ic719502]

Funkce komponenty umožňuje přidejte závislosti do projektu nasazení Azure, jako jsou projekty aplikací Java, speciální soubory a příkazy spustitelný soubor příkazového řádku, které jsou vyžadovány vaše nasazení.

Pro každou součást můžete zadat:

* Krok mají být provedeny, při importu komponentu do projektu nasazení Azure, když je vytvořen.
* Krok při nasazení této součásti v cloudu Azure.

> [!NOTE]
> Při zadávání součást soubory nebo příkazové řádky, mějte na paměti, že vaše nasazení bude publikována do virtuálního počítače s Windows, takže své kroky musí být platná pro operačního systému Windows. 
> 
> 

Součásti mít následující vlastnosti:

* **Import:** metoda, která určuje, jak součást bude naimportován do projektu při sestavení projektu. To může být jeden z následujících hodnot:
  * **kopírování:** komponentu zkopírován z místní cestu určenou položkou **z** vlastnost do role **approot** adresáře.
  * **Vymazat:** součást je v jazyce Java enterprise archivu (Vymazat) importovat z projektu aplikace organizace v místní cestě určeného **z** vlastnost. (To je zjištěno automaticky pomocí sady nástrojů závislosti na povaze projektu v tomto umístění).
  * **JAR:** součásti Java archiv (JAR) a musí být importována z projektu Java v místní cestě určeného **z** vlastnost. (To je zjištěno automaticky pomocí sady nástrojů závislosti na povaze projektu v tomto umístění).
  * **žádné:** import komponentu nebyla provedena žádná akce. Tento krok platí, když se předpokládá, že součást již je součástí role **approot** adresář, nebo pokud není součást jenom spustitelný soubor příkazového řádku příkaz tak, jak je uvedeno v **jako** Vlastnost při **nasadit** je metoda **exec**.
  * **WAR:** komponentu je webový archiv aplikace Java (WAR) a musí být importována z Dynamic Web Project v místní cestě určeného **z** vlastnost. (To je zjištěno automaticky pomocí sady nástrojů závislosti na povaze projektu v tomto umístění).
  * **ZIP:** komponentu je soubor zip a importovaných pomocí pomocí formátu ZIP zadaný adresář nebo soubor určený touto **z** vlastnost.
* **Z:** cestu ke zdroji na místním počítači do složky nebo souboru, který představuje položky pro import do vašeho nasazení. Proměnné prostředí systému Windows lze použít v této vlastnosti. Všechny součásti importovatelné bude naimportován do role **approot** directory při sestavení projektu.
  
    Všimněte si, že máte možnost nasazení součásti ze stahování při nasazení v cloudu (ne emulátoru služby výpočty). Zobrazit související informace níže o přidání komponenty.    
* **Jako:** název souboru, pod kterým komponentu bude naimportován do role **approot** adresář a nakonec nasazené v cloudu Azure. Tuto vlastnost nezadáte zachovat název, stejně jako na místní počítač. (Pro spustitelný soubor komponenty, který je těchto jehož **nasadit** je nastavena metoda **exec**, může to být příkazu libovolného příkazového řádku systému Windows.)
  
  > [!IMPORTANT]
  > Pokud použijete místo znaků pro tuto hodnotu, se budou zpracovávat odlišně v závislosti na metodě nasazení. Pokud je metoda nasazení **exec**, mezery, bude vyhodnocen jako oddělovače argumentů příkazového řádku a nikoli jako součást názvu souboru. Pro všechny ostatní nasazování metod, mezery, bude vyhodnocen jako součást názvu souboru.
  > 
  > 
* **Nasaďte:** metoda, která určuje akce použitá ke komponentě při spuštění nasazení. To může být jeden z následujících hodnot:
  
  * **kopírování:** komponentu je zkopírován do cílové určeného **k** vlastnost.
  * **Exec:** komponentu je provést v rámci cestu určenou položkou spustitelného souboru příkazu příkazového řádku systému Windows **k** vlastnost v době nasazení spustí.
  * **žádné:** žádná akce se použije pro komponentu při spuštění nasazení.
  * **ZIP:** komponentu je rozbalené na cílovou cestu určeného **k** vlastnost. Tato metoda je k dispozici pouze tehdy, když **Import** vlastnost je **zip**.
* **Na:** cílovou cestu na virtuálním počítači, kde bude nasazen komponentu. Proměnné prostředí systému Windows lze použít v této vlastnosti a cesty k souborům jsou vzhledem k **approot**.

Chcete-li přidat novou součást, klikněte na tlačítko **přidat** tlačítka na **součásti** stránku vlastností a **součást Role Azure** otevře se dialogové okno. Zadejte hodnoty pro vlastnosti, které jsou popsané výše. 

Následující ukazuje příklad pro přidání nové komponenty WAR.

![][ic719503]

Při nasazení do cloudu (ne emulátoru služby výpočty v), pokud chcete nasadit součásti ke stažení, zajistěte, aby **v cloudu, místo v balíčku, včetně nasazení z** je zaškrtnuté. Pokud chcete stáhnout z vašeho účtu úložiště Azure, vyberte účet úložiště z **účet úložiště** rozevíracího seznamu (můžete kliknout na **účty** odkaz změnit, co je v seznamu), který bude částečně vyplňte **URL** pole a pak zadejte zbývající část adresy URL. Pokud nechcete používat úložiště Azure, vyberte **(none)** z **účet úložiště** rozevíracího seznamu seznamu a zadejte adresu URL příslušné součásti v **URL** pole. Určete jednu z následujících metod:

* **kopírování:** komponentu stažení je zkopírován do cílové určeného **do adresáře** cesta.
* **stejné:** stejnou metodu používá pro **nasazení ze stažení** jako u **nasazení z balíčku**.
* **ZIP:** komponentu stažení je rozbalené na cílovou cestu určeného **do adresáře** cesta.

Chcete-li upravit komponentu, vyberte součást a klikněte na **upravit** tlačítka na **součásti** stránku vlastností. Umožňuje upravit vlastnosti komponenty se otevře dialogové okno. Stiskněte klávesu **OK** uložit součást hodnoty.

Při odstranění komponenty, vyberte součást a klikněte na tlačítko **odebrat** tlačítka na **součásti** stránku vlastností a pak klikněte na tlačítko **Ano** potvrďte odstranění.

Součásti jsou zpracovány v uvedeném pořadí. Použití **nahoru** a **přesunout dolů** tlačítka pořadí.

> [!NOTE]
> Funkce Konfigurace serveru závisí na součástech a. Tyto součásti nelze odebrat nebo upravit bez odebrání odpovídající konfigurace na serveru. Zobrazí se výzva o, při pokusu provést změny na tyto složky.
> 
> 

<!-- <a name="debugging_properties"></a> -->

<!-- ### Debugging properties -->
<!-- Open the context menu for the role in Eclipse's Project Explorer pane, click **Azure**, and then click **Debugging**. Within this dialog, you have the ability to enable or disable remote debugging, as well as create debug configurations, as shown in the following image. -->

<!-- ![][ic719504] -->

<!-- For related information about debugging, see [Debugging Azure Applications in Eclipse][Debugging Azure Applications in Eclipse]. -->

<a name="endpoints_properties"></a> 

### <a name="endpoints-properties"></a>Vlastnosti koncové body
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **koncové body**. V rámci toto dialogové okno máte možnost vytvořit koncový bod, jakož i upravit nebo odebrat koncový bod, jak je znázorněno na následujícím obrázku.

![][ic719505]

Chcete-li přidat koncový bod, klikněte na tlačítko **přidat** tlačítka na **koncové body** stránku vlastností a **přidat koncový bod** otevře se dialogové okno.

![][ic710897]

Zadejte název pro koncový bod, vyberte typ (buď **vstup**, **interní**, nebo **InstanceInput**) a zadejte veřejné a privátní port. Stiskněte klávesu **OK** uložit nové hodnoty koncového bodu.

V závislosti na typu koncový bod může použít rozsahy portů takto:

* Pro instance vstupní koncový bod, může být veřejný port rozsah portů (například **2000 2010**) a privátní port je pevnou hodnotu.
* Pro vnitřní koncový bod se nepoužívá veřejný port a privátní port může být rozsah, nebo levém prázdné nebo ho nastavte na hvězdičku udávajících ho bude automaticky nastavena Azure.
* Pro vstupní koncové body veřejný port lze pouze na pevnou hodnotu a privátní port může být pevná hodnota nebo levém prázdný, nebo nastavte hvězdičku se indikovat, že je automaticky nastaven na Azure.

Pokud chcete používat jediný port číslo místo rozsah, ponechte textové pole pro konec rozsahu prázdný.

Pro porty, které jsou nastaveny na automatické, pokud je potřeba určit port, který se používá ve skutečnosti během doby běhu, může aplikace pomocí služby modulu Runtime rozhraní API služby Azure, která je popsána v [com.microsoft.windowsazure.serviceruntime balíček souhrn ][com.microsoft.windowsazure.serviceruntime package summary].

<!-- To see how instance input endpoints can be used to help with debugging a multi-instance deployment, see [Debugging a specific role instance in a multi-instance deployment][Debugging a specific role instance in a multi-instance deployment]. -->

Chcete-li upravit koncový bod, vyberte koncový bod a klikněte na tlačítko **upravit** tlačítko v **koncové body** stránka vlastností. Umožňuje upravit název koncového bodu, typ a veřejné a privátní porty, se otevře dialogové okno. Stiskněte klávesu **OK** uložit upravené koncový bod hodnoty.

Pokud chcete odstranit koncový bod, vyberte koncového bodu a klikněte na tlačítko **odebrat** tlačítka na **koncové body** stránku vlastností a pak klikněte na tlačítko **Ano** potvrďte odstranění.

Aby bylo možné správně nakonfigurovat některé funkce (například ukládání do mezipaměti, spřažení relace nebo SSL snižování zátěže) povolen uživatelem v roli, sady nástrojů automaticky nakonfigurovat speciální koncových bodů, které se objeví spolu s uživatelem definované koncové body. Sada nástrojů, nemůže uživatel úpravy nebo odstranění například automaticky vygenerovat koncových bodů tak dlouho, dokud je povolena funkce přidružené.

<a name="environment_variables_properties"></a> 

### <a name="environment-variables-properties"></a>Vlastnosti proměnné prostředí
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **proměnné prostředí**. V rámci toto dialogové okno máte možnost vytvořit proměnné prostředí, a také změnit nebo odebrat proměnné prostředí, jak je znázorněno na následujícím obrázku.

![][ic719506]

Proměnné prostředí jsou k dispozici pro spuštění skriptu při spuštění role.

> [!NOTE]
> Při zadávání proměnných prostředí, mějte na paměti, že vaše nasazení bude publikována do virtuálního počítače s Windows, takže proměnných prostředí musí být platná pro operačního systému Windows.
> 
> 

Jako příklad proměnné prostředí, který je k dispozici po spuštění role, vytvořte novou proměnnou prostředí kliknutím **přidat** tlačítko. Následující příklad zobrazuje proměnnou prostředí s názvem **MyRoleVersion** právě vytvořili a má přiřazenu hodnotu **1.0**.

![][ic659268]

V rámci jsp kódu, můžete zobrazit hodnotu používanou `System.getenv` metoda:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Při spuštění aplikace, což vede k tento výstup:

![][ic552233]

Chcete-li upravit proměnné prostředí, vyberte proměnnou prostředí a klikněte na **upravit** tlačítko v **proměnné prostředí** stránka vlastností. Umožňuje upravit vlastnosti proměnné prostředí se otevře dialogové okno. Stiskněte klávesu **OK** uložit hodnoty proměnných prostředí.

Pokud chcete odstranit proměnnou prostředí, vyberte proměnnou prostředí a klikněte na tlačítko **odebrat** tlačítka na **proměnné prostředí** stránku vlastností a pak klikněte na **Ano** k Potvrďte odstranění.

Chcete-li správně nakonfigurovat některé funkce (například konfigurace serveru, vzdáleném ladění nebo místní úložiště) povolen uživatelem v roli, mohou automaticky sady nástrojů nakonfigurovat speciální proměnné, které se objeví spolu s uživatelem definované proměnné prostředí. Sada nástrojů, nemůže uživatel úpravy nebo odstranění například automaticky vygenerovat proměnné prostředí tak dlouho, dokud je přidružená funkce povolena.

<a name="session_affinity_properties"></a> 

### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Vyrovnávání zatížení nebo vlastnosti relace spřažení (známé jako "trvalé relace")
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **Vyrovnávání zatížení**. V rámci toto dialogové okno máte možnost povolit nebo zakázat spřažení relace, jak je znázorněno na následujícím obrázku.

![][ic719492]

Související informace najdete v tématu [spřažení relace][Session Affinity]. Mějte také na paměti tato funkce chování v kontextu snižování zátěže protokolu SSL, jak je popsáno v [snižování zátěže protokolu SSL][SSL Offloading].

<a name="local_storage_properties"></a> 

### <a name="local-storage-properties"></a>Místní úložiště vlastností
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **místní úložiště**. V rámci toto dialogové okno máte možnost vytvořit, upravit nebo odebrat dočasné místní úložiště pro virtuální počítač, který je spuštění aplikace. Konkrétní hodnoty lze nastavit pro velikost místního úložiště, a také, zda obsah se zachová, i když roli dojde k recyklování, jak je znázorněno na následujícím obrázku.

![][ic719508]

Volitelně můžete zadat proměnné prostředí, která odpovídá do místního úložiště.

Ve výchozím nastavení, vše, co nasadíte do Azure je umístěn (a rozbalené) v **approot** složky role instance. Zatímco většina jednoduché nasazení se vejde existuje i po rozbalení, místo přidělené **approot** adresář je omezený a dobře definovaný (méně než 1 GB je možné logicky pravidlem). Proto, aby Azure přiděluje dostatek místa na disku pro rozsáhlejší nasazení, které nemusí vyhovovat v **approot** složky, měli byste nastavit místní úložiště prostředků pomocí **místní úložiště** dialogové okno. Snadný způsob, jak to udělat, najdete v části [nasazení velkých nasazeních][Deploying Large Deployments].

Prostředků úložiště můžete snadno odkazovat z spuštění skriptů (například vaše **startup.cmd**) pomocí proměnné prostředí automaticky pomocí sady nástrojů Eclipse přidruženého k prostředku, jak je znázorněno  **Místní úložiště** dialogové okno. Tuto proměnnou prostředí bude obsahovat úplnou cestu místní prostředek, který jste nakonfigurovali v době, kdy se spustí spouštěcího skriptu. 

Chcete-li upravit prostředek Místní úložiště, vyberte prostředek Místní úložiště a klikněte na **upravit** tlačítka na **místní úložiště** stránku vlastností. Umožňuje upravit vlastnosti prostředku Místní úložiště se otevře dialogové okno. Stiskněte klávesu **OK** uložit hodnoty prostředků místní úložiště.

Pokud chcete odstranit prostředek Místní úložiště, vyberte prostředek Místní úložiště a klikněte na tlačítko **odebrat** tlačítka na **místní úložiště** stránku vlastností a pak klikněte na tlačítko **Ano** potvrďte odstranění.

<a name="server_configuration_properties"></a> 

### <a name="server-configuration-properties"></a>Vlastnosti konfigurace serveru
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **konfigurace serveru**. V tomto dialogu můžete přidat, odebrat a upravit je JDK a Java aplikační server, které používá vaše nasazení, a také přidat nebo odebrat aplikace (například soubory WAR, JAR nebo vymazat) používané vaším nasazením.

### <a name="jdk-configuration"></a>Konfigurace JDK
Tento dialog umožňuje zadat JDK balíček k použití pro vaše nasazení. Pokud používáte Eclipse v systému Windows, můžete zadat balíček JDK místně použije při spuštění v Azure emulátoru a máte možnost nasadit místní instalaci do Azure. Na jiný systém než Windows operační systémy nastavení JDK emulátoru se nedá použít a lokálně nainstalované JDK nelze nasadit, protože to není kompatibilní se systémem Windows. Nicméně bez ohledu na operační systém, který používáte, vždy můžete mezi 3. stran JDK balíčky nasazení do Azure nebo z umístění stahování alternativní bod v vlastní balíček JDK kompatibilní se systémem Windows.

Následuje příklad, jak můžete zadat JDK v systému Windows:

![][ic780647]

Pokud používáte Eclipse v systému Windows, můžete zadat JDK pro použití s emulátoru služby výpočty v; Uděláte to tak, ujistěte se **použít sadu JDK z tuto cestu k souboru pro místní testování** se změnami **emulátoru nasazení** části. Potom můžete Určete místní cesta k vaší JDK; můžete vyhledat jiný JDK Pokud nevyberete automaticky je ta, kterou chcete použít. Máte také možnost nasadit vaší JDK k službě Azure cloud; to pokud chcete udělat, vyberte **nasazení Moje místní JDK (automatické – nahrání do cloudového úložiště)** možnost **cloudové nasazení** části.

Poznámka: na jiný systém než Windows operační systémy **emulátoru nasazení** nastavení a **nasazení Moje místní JDK** možnost nejsou k dispozici. Následující příklad ilustruje, zadáním JDK na Macu nebo jiných podporovaný operační systém jiný systém než Windows:

![][ic789643]

Bez ohledu na operační systém na, máte následující dva **cloudové nasazení** možnosti pro zdroj a typ vašeho JDK balíčku:

* **Nasazení 3. stran JDK balíčku k dispozici v Azure** 
* **Nasazení z vlastní stahování** 

Pokud používáte **nasazení 3. stran JDK balíčku k dispozici prostřednictvím Azure** možnost:

1. Zaškrtněte políčko s názvem **nasazení 3. stran JDK balíčku k dispozici prostřednictvím Azure**.
2. V rozevíracím seznamu vyberte 3. balíček JDK strany, který je k dispozici v Azure.
3. Vaše **JDK** karta bude vypadat podobně jako následující v systému Windows: ![][ic780648] a bude vypadat podobně jako následující v systému Mac OS nebo jiné podporované operační systémy jiný systém než Windows:![][ic789643]
4. Klikněte na tlačítko **OK** a uložte změny.
5. Po zobrazení výzvy přijměte licenční smlouvu od 3. stran JDK balíček poskytovatele, přečtěte si licenční podmínky. Za předpokladu, že souhlasíte s podmínkami, klikněte na tlačítko **Ano** zavřete **přijmout licenční smlouvu** dialogové okno.
    Všimněte si, že základní logiku pro položky, které se zobrazí v rozevíracím seznamu pro **nasazení 3. stran JDK balíčku k dispozici prostřednictvím Azure** možnost lze přizpůsobit. Lze přizpůsobit položky, v **JDK** dialogové okno, klikněte **přizpůsobit** odkaz. To se zavře **JDK** stránku vlastností a otevřete **componentsets.xml** souboru v prostředí Eclipse, který poté můžete upravit podle potřeby. Dokumentace pro **componentsets.xml** je součástí **componentsets.xml** samotném souboru.

Pokud používáte **nasazení JDK z vlastní stahování** možnost:

1. Vytvořte ZIP adresáře instalace JDK zajišťující, že uzel adresáře je podřízeným strukturu ZIP a ne její obsah. Poznamenejte si název adresáře, jak je budete potřebovat později a mějte na paměti, které tato instalace JDK se nasadí do virtuálního počítače s Windows.
2. Nahrání souboru ZIP do účtu úložiště Azure jako objekt blob. Můžete tak učinit pomocí externě dostupný nástroj pro odesílání objekty BLOB do úložiště Azure. Doporučujeme používat privátní objektů blob. Poznamenejte si adresu URL objektu blob ZIP obsahu.
3. Zaškrtněte políčko s názvem **nasazení JDK z vlastní stahování**.
    Pokud chcete stáhnout z vašeho účtu úložiště Azure, vyberte účet úložiště z **účet úložiště** rozevíracího seznamu (můžete kliknout na **účty** odkaz změnit, co je v seznamu), který bude částečně vyplňte **URL** pole a pak zadejte zbývající část adresy URL. Pokud nechcete používat úložiště Azure, vyberte **(none)** z **účet úložiště** rozevíracího seznamu seznamu a zadejte adresu URL pro stahování JDK v **URL** pole. Pokud používáte Azure storage, názvy objektů blob v adrese URL musí být malé.
4. Ujistěte se, že **JAVA_HOME** textbox odkazuje na název správném adresáři. Ve výchozím nastavení se bude odkazovat stejný název adresáře JDK jako hodnota, kterou jste zvolili pro místní použití. Ale pokud adresáři obsažené v souboru ZIP má jiný název (například kvůli použití jiné verze), aktualizujte název adresáře v **JAVA_HOME** textbox odpovídajícím způsobem, protože toto nastavení se použije v cloudu (není v emulátor služby výpočty).
5. Klikněte na tlačítko **OK** a uložte změny.

A je to. Teď když vytvoříte pro cloud, si všimnete budou mnohem menší velikost balíčku, procesu sestavení by měl obvykle trvá méně času a sám při publikování do cloudu nasazení také provést méně času. Všimněte si, že **nasazení Moje místní JDK (automatické – nahrání do cloudového úložiště)** nebo **nasazení JDK z vlastní stahování** jsou možnosti platí pouze, když je aplikace nasazená v cloudu. To mít žádný vliv na vaše uživatelské prostředí emulátoru výpočtů; místní verze součástí se stále použije při nasazování do emulátoru služby výpočty v. 

### <a name="server-configuration"></a>Konfigurace serveru
Následuje příklad, jak můžete zadat aplikační server.

![][ic796926]

Ověřte, zda **nasadit server tohoto typu** zaškrtávací políčko je vybraná a pak vyberte typ aplikačního serveru, kterou chcete použít.

Pro zadání serveru chcete použít pro nasazení cloudu, můžete využít výhod z následujících možností:

1. **Nasazení k dispozici v Azure 3. stran serveru** – to platí hlavně v scénáře vývoje/testování kde nasazení efektivitu a jednoduchost, je prioritu a server nevyžaduje vlastní konfiguraci. Nebo pokud chcete použít jednu z těchto serverů jako výchozí bod, ale zahrnete přizpůsobení příslušného serveru kroky v logika spuštění vašeho nasazení.
2. **Nasazení z vlastní stahování** – to je obzvláště použít v produkčních scénářích, když máte speciálně připravené a nakonfigurovaný server, který chcete použít v cloudu.
3. **Moje instalace místní server nasazení** – to platí hlavně v Pokud už je místní server instalace konfigurovány pro vaše použití. Pokud zvolíte tuto možnost, musíte zadat cestu k místní server **místní server cesta** textového pole níže.

Pokud používáte **nasazení k dispozici v Azure 3. stran serveru** možnost:

1. Zaškrtněte políčko s názvem **nasazení k dispozici v Azure 3. stran serveru**.
2. Z rozevírací nabídky vyberte požadovanou serverového softwaru pro použití s nasazením v cloudu. Poznámka: Pokud jste již zadali typ serveru pro použití dříve, bude omezený výběr pouze server cloudu, který je ve stejné rodiny jako tento typ serveru. Ale pokud jste nezvolili typ serveru, můžete vybrat ze všech serverů, které jsou aktuálně dostupné v Azure a typ serveru bude automaticky vybrána pro vás.
3. Klikněte na tlačítko **OK** a uložte změny.

Pokud se používá **nasadit z vlastní stahování** možnost:

1. Ujistěte se, že jste již vybrali typ serveru podle předchozích kroků. Tato hodnota informuje o plugin jak nasadit server z vlastní stahování, jako musí být ze stejné rodiny jako typ vybraný server.
2. Zaškrtněte políčko s názvem **nasadit z vlastní stahování**.
    Pokud chcete stáhnout z vašeho účtu úložiště Azure, vyberte účet úložiště z **účet úložiště** rozevíracího seznamu (můžete kliknout na **účty** odkaz změnit, co je v seznamu), který bude částečně vyplňte **URL** pole a potom vyplňte zbývající část adresy URL na váš server stáhnout ZIP (při použití služby Azure storage, názvy objektů blob v adrese URL musí být malá písmena). Pokud nechcete používat úložiště Azure, vyberte **(none)** z **účet úložiště** rozevíracího seznamu seznamu a zadejte adresu URL serveru stáhnout ZIP v **URL** pole. Souboru ZIP obsahuje podřízenou složku představující instalační adresář serveru vaší aplikace. Například pokud použijete zip Apache Tomcat 7.0.35, v souboru zip by podřízenou složku představující instalační adresář, jako například **apache tomcat-7.0.35**. 
3. Zadejte hodnotu pro proměnnou prostředí domovský adresář. Použije výchozí hodnotu použitou pro místní aplikačního serveru, pokud existuje, ale můžete zadat jinou hodnotu, pokud váš server cloudové aplikace se liší od místní aplikačního serveru. Musíte ale ujistěte se, zda je server aplikace cloudu stejné rodiny jako server dříve vybraný typ.
    Pokud aktualizujete vaší cloudové aplikace server zip v budoucnosti, můžete ručně změnit nastavení domovský adresář, nebo jej znovu shodovat s vaší místní nastavení (Pokud jste změnili příliš místního aplikačního serveru).
4. Klikněte na tlačítko **OK** a uložte změny.

Základní logiku, pro které položky se zobrazí ve **Server** kartě **konfigurace serveru** stránka vlastností lze přizpůsobit. Toto je pokročilá funkce, které byste mohli potřebovat, pokud vaše potřeby rozšířit nad výchozí hodnoty, nebo pokud chcete přidat další servery. Chcete-li přizpůsobit logiku, v **Server** dialogové okno, klikněte na tlačítko **přizpůsobit** odkaz. To se zavře **konfigurace serveru** stránku vlastností a otevřete **componentsets.xml** souboru v prostředí Eclipse, který poté můžete upravit podle potřeby rozšířit serveru konfigurace šablony. Dokumentace pro **componentsets.xml** je součástí **componentsets.xml** samotném souboru.

Pokud používáte **nasazení Moje místní server (automatické – nahrání do cloudového úložiště)** možnost:

1. Zaškrtněte políčko s názvem **nasazení Moje místní server (automatické – nahrání do cloudového úložiště)**.
2. Pomocí **účet úložiště** rozevíracího seznamu vyberte **(automaticky)**. Pokud zadáte **(automaticky)** zde nástrojů Eclipse použije stejný účet úložiště pro váš server jako ta, kterou vyberete pro vaše nasazení v **publikovat do Azure** dialogové okno.
3. Klikněte na tlačítko **OK** a uložte změny.

Vyberte cestu instalace serveru ve vašem počítači v **místní server cesta** textového pole, pokud platí některá z následujících podmínek:

* Chcete nasazení otestovat v emulátoru (platí pouze pro Windows).
* Chcete nasazení lokálně nainstalované serveru do cloudu.
* Chcete použít vlastní server stahování vlastní aplikaci v cloudu, ve kterém případu, ujistěte se také **nasazení Moje místní server (automatické – nahrání do cloudového úložiště)** možnost výše.

Pokud žádná z předchozích možností použít pro vaši situaci, nastavení místní server je volitelné.

### <a name="applications-configuration"></a>Konfigurace aplikace
Následuje příklad, jak můžete zadat aplikace.

![][ic719512]

Klikněte na tlačítko **přidat** přidat jiné aplikace, nebo **odebrat** postup odebrání aplikace. Pro účely účinnost, pokud chcete používat ke stažení pro zdroj aplikace při nasazování do cloudu, použijte [vlastnosti součásti](#components_properties) zadejte adresu URL účtu úložiště, atd. 

Počínaje verzí. dubna 2014, vaše aplikace se automaticky nahraje do stejného účtu úložiště (v části **eclipsedeploy** kontejneru) jako ten, vybraný pro vaše nasazení. Logika spuštění nasazení obsahuje krok, který nejprve stáhne tyto aplikace z daného účtu úložiště. To znamená, že může upgrade aplikace ve vašem nasazení bez nutnosti znovu sestavili a nasadili celý balíček ručně odesláním novější verze aplikace přímo do tohoto účtu úložiště (například pomocí portálu Azure), nahraďte soubory WAR původně odeslaný existuje sada nástrojů. Potom právě zahajte recyklaci všech těchto instancí rolí pomocí portálu pro správu Azure, je znovu, nebo pomocí nástroje příkazového řádku. (Spuštění v rámci sady nástrojů Eclipse recyklace přímo z rolí se aktuálně nepodporuje.)

### <a name="notes-about-server-configuration"></a>Poznámky k nástroji Konfigurace serveru
Změny provedené prostřednictvím **konfigurace serveru** stránka vlastností se projeví v `<component>` prvky package.xml souboru.

Při použití **automaticky odeslat...**  nebo **nasazení ze stažení...**  jsou možnosti pro sadu JDK nebo aplikační server a můžete vytvářet pro cloudu (ne emulátoru služby výpočty v) a jste připojení k síti, může dojít k vytvoření zprávy, jako je například následující výstup konzoly podle Tvůrce Ant ověřuje dostupnost ke stažení:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Pokud jste vybrali **nasazení ze stažení...**  možnost, se může zobrazit následující upozornění, ale bude pokračovat sestavení:

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Toto upozornění je jediné není ověřená dostupnosti ke stažení. Takže pokud nasazení je z nějakého důvodu selže v cloudu, zkontrolujte, pokud se vám zobrazila upozornění.

Pokud chcete zakázat stahování ověření (například, pokud si myslíte, že ho zbytečně zpomaluje sestavení), nastavte `verifydownloads` atribut `false` v `<windowsazurepackage>` element package.xml: 

`<windowsazurepackage verifydownloads="false" ...>` 

Pokud jste vybrali **automaticky odeslat...**  možnost, pak v okně konzoly se zobrazí sestavení reporting průběh nahrávání každých 5 sekund, kdykoli je nezbytné odeslání zprávy.

<a name="ssl_offloading_properties"></a> 

### <a name="ssl-offloading-properties"></a>Vlastnosti snižování zátěže protokolu SSL
Klikněte na Otevřít kontextové nabídky pro roli v podokně Průzkumník projektů Eclipse na **Azure**a potom klikněte na **snižování zátěže protokolu SSL**. 

![][ic719481]

V tomto dialogu můžete povolit snižování zátěže protokolu SSL, což umožňuje snadno povolit podporu protokolu Secure HTTPS (Hypertext Transfer) ve vašem nasazení Java v Azure, aniž by bylo potřeba konfigurace protokolu SSL na vašem serveru aplikace Java. Další informace najdete v tématu [snižování zátěže protokolu SSL] [ SSL Offloading] a [postup snižování zátěže protokolu SSL použití][How to Use SSL Offloading].

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse]

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Vlastnosti projektu Azure][Azure Project Properties]

[Seznam účtů úložiště Azure][Azure Storage Account List]

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Project Properties]: http://go.microsoft.com/fwlink/?LinkID=699524
[Azure Storage Account List]: http://go.microsoft.com/fwlink/?LinkID=699528
[com.microsoft.windowsazure.serviceruntime package summary]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Debugging a specific role instance in a multi-instance deployment]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Debugging Azure Applications in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[Deploying Large Deployments]: http://go.microsoft.com/fwlink/?LinkID=699536
[How to Use Co-located Caching]: http://go.microsoft.com/fwlink/?LinkID=699542
[How to Use SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->
