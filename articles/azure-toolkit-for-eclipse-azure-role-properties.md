---
title: "aaaAzure Vlastnosti rolí"
description: "Zjistěte, jak toouse hello Azure Toolkit pro nastavení Azure role tooconfigure Eclipse."
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
ms.openlocfilehash: d111b4b9e4f12e49f38755bf6c9acc1a1de17a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-role-properties"></a>Vlastnosti Azure Role
Různá nastavení konfigurace pro vaši Azure roli můžete nastavit v rámci hello nástrojů Azure pro prostředí Eclipse.

## <a name="configuring-azure-role-properties"></a>Konfigurace vlastností Azure Role
Konfigurace vlastností Role vaše Azure se provádí pomocí hello vlastnost dialogová okna pro své role pracovního procesu. Otevřete hello kontextovou nabídku hello role v prohlížeči projektu panelu a vyberte hello na Eclipse **Azure** dílčí nabídky. (Pokud nevidíte hello role v prohlížeči projektu Eclipse hello, rozbalte projektu Azure v prohlížeči projektu.)

![][ic789599]

Hello různé vlastnosti, které lze nastavit z hello **vlastnosti** dialogová okna jsou popsané v tomto tématu. Všimněte si, že mnoho vlastností se vyplní automaticky při vytvoření nového projektu nasazení Azure.

Hello následující stránky vlastností jsou k dispozici pro Azure role.

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
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **vlastnosti**, a budete mít velikost virtuálního počítače hello možnost toochange hello a také změnit Hello počet instancí, jak ukazuje následující obrázek hello.

![][ic719499]

> [!NOTE]
> Pouze v systému Windows: Když nastavíte hello počet instancí tooa hodnota větší než 1 a taky nakonfigurovat aplikační server, hello toolkit vám umožní pouze 1 toorun instance role v emulátoru hello, bez ohledu na toto nastavení. Toto je tooavoid port vazby konflikty mezi instancemi hello jiný server (například všechny snažíme toobind tooport 8080) při spuštění v hello stejný počítač. Vaše nastavení počtu požadované instance se zachová, ale probíhá platí jenom v případě, že nasazujete toohello cloudu.
> 
> 

<a name="caching_properties"></a> 

### <a name="caching-properties"></a>Ukládání do mezipaměti vlastnosti
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **ukládání do mezipaměti**. V tomto dialogu můžete povolit pojmenované blízko memcache kompatibilní s mezipamětí, povolení toohelp rychlost až webových aplikací.

![][ic719483]

V rámci hello **ukládání do mezipaměti** stránce vlastností, můžete určit globální nastavení pro hello následující:

* zda je povoleno ukládání do mezipaměti společně umístěné.
* velikost mezipaměti Hello jako procento paměti.
* Hello název účtu úložiště pro ukládání stavu mezipaměti hello, pokud vaše aplikace běží jako cloudová služba, nebo none, pokud nechcete, aby toosave hello mezipaměti stavu. (název účtu úložiště hello se nepoužije, když spustíte aplikaci v emulátoru služby výpočty hello.) Pokud nastavíte název účtu úložiště hello příliš**(automaticky)** (což je výchozí hello), konfiguraci ukládání do mezipaměti, budou automaticky používat hello stejný účet úložiště, jako jeden vyberete v hello hello **publikování tooAzure**dialogové okno.

> [!NOTE]
> Hello **(automaticky)** nastavení nebude mít vliv hello potřeby pouze v případě, že publikujete nasazení pomocí nástrojů Eclipse hello Průvodce publikováním. Pokud místo toho publikování souboru .cspkg hello ručně pomocí externího mechanismu, jako je například hello [portálu pro správu Azure][Azure Management Portal], hello nasazení nebude fungovat správně.
> 
> 

Hello následující dialogové okno se zobrazují vlastnosti hello pro mezipaměť.

![][ic719501]

* **Název:** hello název hello společně umístěné mezipaměti.
* **Číslo portu:** hello toouse číslo portu pro hello cache.
* **Zásady vypršení platnosti:** jednu z následujících hodnot hello, která určuje, kdy vyprší platnost klíč v mezipaměti hello.
  * **Absolutní:** hello klíč vyprší po času hello určeného **minut toolive** je dosaženo.
  * **NeverExpires:** hello klíč nemá čas vypršení platnosti.
  * **SlidingWindow:** hello klíč vyprší, pokud byla nepoužíval pro hello množství času určeného **minut toolive**; pokaždé, když je přístupný, hodiny hello vypršení platnosti se resetuje.
* **Minut toolive:** maximální počet minut, než toolive, memcached klíče subjektu toohello zásad vypršení platnosti.
* **Vysoká dostupnost s replikované zálohování na jinou roli instancí:** Pokud je povoleno, pomáhá poskytovat použití vysoké dostupnosti replikují zálohy u instancí jiné role. Všimněte si, že aspoň dvě instance role musí být platí pro vaše nasazení pro tuto funkci toowork.

tooadd nové mezipaměti, klikněte na tlačítko hello **přidat** tlačítka na hello **ukládání do mezipaměti** stránku vlastností a **konfigurace s názvem mezipaměti** otevře se dialogové okno. Zadejte hodnoty pro hello vlastnosti, které jsou popsané výše.

Vyberte hello mezipaměti toomodify pojmenované mezipaměti a klikněte na tlačítko hello **upravit** tlačítka na hello **ukládání do mezipaměti** stránku vlastností. Povolení jste toomodify hello vlastnosti mezipaměti se otevře dialogové okno. Stiskněte klávesu **OK** toosave hello mezipaměti hodnoty.

Vyberte hello mezipaměti toodelete mezipaměti a klikněte na tlačítko hello **odebrat** tlačítka na hello **ukládání do mezipaměti** stránku vlastností a pak klikněte na tlačítko **Ano** tooconfirm hello odstranění.

Další informace o tom, najdete v části ukládání do mezipaměti, toouse [jak společně umístěné tooUse ukládání do mezipaměti][How tooUse Co-located Caching].

<a name="certificates_properties"></a> 

### <a name="certificates-properties"></a>Vlastnosti certifikáty
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **certifikáty**.

![][ic710964]

V tomto dialogu můžete přidat nebo odebrat certifikáty odkazuje projektu Eclipse. Upozorňujeme, že certifikáty hello tady nejsou automaticky uloženy uvnitř žádné Java keystore a proto nejsou automaticky k dispozici pro jakékoli použití z v rámci aplikace Java. Jsou pouze registrované v Azure tak, aby může být zavedené do hello Windows certifikát úložiště na virtuálních počítačích hello s nasazením a následně použít jiný software Windows. V současné době hello pouze funkce hello nástrojů, která používá certifikáty hello odkazuje tímto způsobem v hello **certifikáty** dialogové okno je [snižování zátěže protokolu SSL][SSL Offloading], kvůli tooits závislá na Internetové informační služby (IIS) a požádat o směrování aplikace (ARR), které vyžadují hello toobe vhodný certifikát k dispozici tímto způsobem.

Když nasadíte tooAzure váš projekt pomocí Průvodce publikování hello, bude výzvami toopoint v hello Personal Information Exchange (PFX) soubory odpovídající toothese certifikáty, spolu s jejich hesla, aby tooautomatically odešlete toohello Služba Azure, ale pouze v případě, že nebyly odeslány existuje dříve.

<a name="components_properties"></a> 

### <a name="components-properties"></a>Vlastnosti součásti
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **součásti**. V tomto dialogu můžete mít možnost tooadd hello, upravit, nebo odebrat součásti hello role, jakož i změnit hello pořadí, ve kterém jsou zpracovány.

![][ic719502]

funkce součásti Hello umožňuje tooadd závislosti tooyour Azure projekt nasazení, jako jsou projekty aplikací Java, speciální soubory a příkazy spustitelný soubor příkazového řádku, které jsou vyžadovány vaše nasazení.

Pro každou součást můžete zadat:

* toobe krok Hello prováděné při importu hello součást do projektu nasazení Azure, když je vytvořen.
* Při nasazení této součásti v cloudu Azure hello toobe krok Hello.

> [!NOTE]
> Při zadávání součást soubory nebo příkazové řádky, mějte na paměti, že vaše nasazení bude publikované tooa virtuálního počítače s Windows, tak své kroky musí být platná pro operačního systému Windows. 
> 
> 

Součásti mají hello následující vlastnosti:

* **Import:** metoda, která určuje, jak součást hello bude naimportován do projektu hello při sestavení projektu hello. To může být jeden z hello následující hodnoty:
  * **kopírování:** součást hello se zkopíruje z hello místní cestu určenou položkou hello **z** vlastnost do hello role **approot** adresáře.
  * **Vymazat:** hello součást je v jazyce Java enterprise archivu (Vymazat) importovat z projektu aplikace organizace v místní cestě hello určeného hello **z** vlastnost. (To je zjištěno automaticky pomocí nástrojů hello závislosti na povaze hello hello projektu v tomto umístění).
  * **JAR:** hello součásti Java archiv (JAR) a musí být importována z projektu Java v místní cestě hello určeného hello **z** vlastnost. (To je zjištěno automaticky pomocí nástrojů hello závislosti na povaze hello hello projektu v tomto umístění).
  * **žádné:** tooimport hello komponenta nebyla provedena žádná akce. Tuto možnost lze použít při považován za součást hello tooalready nacházet v roli hello **approot** adresář, nebo pokud není součást hello jenom spustitelný soubor příkazového řádku příkaz tak, jako zadaný v hello **jako**při hello **nasadit** je metoda **exec**.
  * **WAR:** hello je webový archiv aplikace Java (WAR) a je naimportovaná ze Dynamic Web Project v místní cestě hello určeného hello **z** vlastnost. (To je zjištěno automaticky pomocí nástrojů hello závislosti na povaze hello hello projektu v tomto umístění).
  * **ZIP:** hello je soubor zip a importovaných pomocí pomocí formátu ZIP hello adresář nebo soubor určený touto hello **z** vlastnost.
* **Z:** zdrojovou cestu v místním počítači toohello složky nebo souboru, který představuje hello položky tooimport tooyour nasazení. Proměnné prostředí systému Windows lze použít v této vlastnosti. Všechny součásti importovatelné bude naimportován do hello role **approot** directory při sestavení projektu hello.
  
    Všimněte si, máte možnost toodeploy hello součásti ke stažení při nasazování toohello cloudu (ne hello výpočetní emulátor). Zobrazit související informace níže o přidání komponenty.    
* **Jako:** název souboru, pod které hello součásti bude naimportován do hello role **approot** adresář a nakonec nasazené v cloudu Azure hello. Nechte tato vlastnost prázdná tookeep hello hello název stejný jako jeho je v místním počítači hello. (Pro spustitelný soubor komponenty, který je těchto jehož **nasadit** je nastavena metoda příliš**exec**, může to být příkazu libovolného příkazového řádku systému Windows.)
  
  > [!IMPORTANT]
  > Pokud použijete místo znaků pro tuto hodnotu, se budou zpracovávat odlišně v závislosti na hello nasazení metoda. Pokud metoda nasazení hello **exec**, mezery, bude vyhodnocen jako oddělovače argumentů příkazového řádku a nikoli jako součást hello název souboru. Pro všechny ostatní nasazování metod, mezery, bude vyhodnocen jako součást hello název souboru.
  > 
  > 
* **Nasaďte:** metoda, která určuje hello akce použijí toohello součást při spuštění hello nasazení. To může být jeden z hello následující hodnoty:
  
  * **kopírování:** součást hello je zkopírovaný toohello cílová cesta určeného hello **k** vlastnost.
  * **Exec:** součást hello je spustit v kontextu hello hello cestu určenou položkou hello spustitelného souboru příkazu příkazového řádku Windows **k** vlastnost během hello hello nasazení spustí.
  * **žádné:** při spuštění hello nasazení není žádná akce použité toohello součásti.
  * **ZIP:** součást hello je cílová cesta rozbalené toohello určeného hello **k** vlastnost. Tato metoda je k dispozici, pouze pokud hello **Import** vlastnost je **zip**.
* **Na:** cílovou cestu, kde hello součástí nasadí hello virtuálního počítače. Proměnné prostředí systému Windows lze použít v této vlastnosti a jsou cesty k souboru relativní příliš**approot**.

tooadd nové komponenty, klikněte na tlačítko hello **přidat** tlačítka na hello **součásti** stránku vlastností a **součást Role Azure** otevře se dialogové okno. Zadejte hodnoty pro hello vlastnosti, které jsou popsané výše. 

Následující Hello ukazuje příklad pro přidání nové komponenty WAR.

![][ic719503]

Při nasazení cloudu toohello (ne hello emulátoru služby výpočty), pokud chcete, aby toodeploy hello součásti ke stažení, zajistěte, aby **v cloudu, místo v balíčku hello, včetně nasazení z** je zaškrtnuté. Pokud chcete toodownload z vašeho účtu úložiště Azure, vyberte účet úložiště hello z hello **účet úložiště** rozevíracího seznamu (můžete kliknout na hello **účty** odkaz toomodify, co je v seznamu hello), které budou částečně vyplňte hello **URL** pole a pak zadejte zbývající část adresy URL hello hello. Pokud nechcete, aby toouse úložiště Azure, vyberte **(none)** z hello **účet úložiště** rozevíracího seznamu seznamu a zadejte hello URL tooyour součásti v hello **URL** pole. Určete jednu z následujících metod hello:

* **kopírování:** hello stažení součást je zkopírovaný toohello cílová cesta určeného hello **tooDirectory** cesta.
* **stejné:** hello stejnou metodu používá pro **nasazení ze stažení** jako u **nasazení z balíčku**.
* **ZIP:** hello stažení součást je cílová cesta rozbalené toohello určeného hello **tooDirectory** cesta.

toomodify hello součásti, vyberte součást a klikněte na tlačítko hello **upravit** tlačítka na hello **součásti** stránku vlastností. Povolení jste toomodify hello vlastnosti komponenty se otevře dialogové okno. Stiskněte klávesu **OK** toosave hello součást hodnoty.

toodelete hello součásti, vyberte součást a klikněte na tlačítko hello **odebrat** tlačítka na hello **součásti** stránku vlastností a pak klikněte na tlačítko **Ano** tooconfirm hello odstranění.

Součásti jsou zpracovány v uvedeném pořadí hello. Použití hello **nahoru** a **přesunout dolů** tlačítka tooarrange hello pořadí.

> [!NOTE]
> funkce Konfigurace serveru Hello spoléhá na součástech a. Tyto součásti nelze odebrat nebo upravit bez odebrání hello odpovídající konfiguraci serveru. Zobrazí se výzva o, při pokusu o toomake změny toosuch součásti.
> 
> 

<!-- <a name="debugging_properties"></a> -->

<!-- ### Debugging properties -->
<!-- Open hello context menu for hello role in Eclipse's Project Explorer pane, click **Azure**, and then click **Debugging**. Within this dialog, you have hello ability tooenable or disable remote debugging, as well as create debug configurations, as shown in hello following image. -->

<!-- ![][ic719504] -->

<!-- For related information about debugging, see [Debugging Azure Applications in Eclipse][Debugging Azure Applications in Eclipse]. -->

<a name="endpoints_properties"></a> 

### <a name="endpoints-properties"></a>Vlastnosti koncové body
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **koncové body**. V rámci toto dialogové okno máte možnost toocreate hello koncový bod, a také upravit nebo odstranit koncový bod, jak ukazuje následující obrázek hello.

![][ic719505]

tooadd na koncový bod, klikněte na tlačítko hello **přidat** tlačítka na hello **koncové body** stránku vlastností a **přidat koncový bod** otevře se dialogové okno.

![][ic710897]

Zadejte název pro koncový bod hello, vyberte typ hello (buď **vstup**, **interní**, nebo **InstanceInput**) a zadejte hello veřejné a privátní port. Stiskněte klávesu **OK** toosave hello nové hodnoty koncového bodu.

V závislosti na typu hello koncového bodu může použít rozsahy portů takto:

* Pro instance vstupní koncový bod, může být veřejný port hello rozsah portů (například **2000 2010**) a privátní port hello je pevnou hodnotu.
* Pro vnitřní koncový bod se nepoužívá hello veřejný port a privátní port hello může být v rozsahu, nebo být prázdná nebo sadu tooan hvězdičky tooindicate, je automaticky nastavena pomocí Azure.
* Pro vstupní koncové body veřejný port hello lze pouze na pevnou hodnotu a privátní port hello může být pevná hodnota, nebo být prázdná nebo sadu tooan hvězdičky tooindicate, je automaticky nastavena pomocí Azure.

Pokud chcete toouse jediný port číslo místo rozsah, ponechejte hello textového pole pro hello konec rozsahu hello prázdné.

Pro porty, které jsou tooautomatic sady, pokud potřebujete toodetermine port, který se používá ve skutečnosti během doby běhu, vaše aplikace může použít hello rozhraní API služby modulu Runtime Azure, která je popsána v hello [com.microsoft.windowsazure.serviceruntime balíčku Souhrn][com.microsoft.windowsazure.serviceruntime package summary].

<!-- toosee how instance input endpoints can be used toohelp with debugging a multi-instance deployment, see [Debugging a specific role instance in a multi-instance deployment][Debugging a specific role instance in a multi-instance deployment]. -->

toomodify na koncový bod, vyberte hello koncového bodu a klikněte na hello **upravit** tlačítka na hello **koncové body** stránku vlastností. Umožní vám toomodify hello koncový bod název, typ a veřejné a privátní porty, se otevře dialogové okno. Stiskněte klávesu **OK** toosave hello upravit hodnoty koncového bodu.

toodelete na koncový bod, vyberte hello koncového bodu a klikněte na hello **odebrat** tlačítka na hello **koncové body** stránku vlastností a pak klikněte na tlačítko **Ano** tooconfirm hello odstranění.

V pořadí tooproperly konfigurace, některé funkce hello (například ukládání do mezipaměti, spřažení relace nebo SSL snižování zátěže) povolen uživatelem hello v roli, hello toolkit může automaticky nakonfigurovat speciální koncových bodů, které se objeví spolu s uživatelem definované koncové body. Hello toolkit zabrání uživateli hello úpravy nebo odstranění takové automaticky generované koncových bodů tak dlouho, dokud hello související funkce povolena.

<a name="environment_variables_properties"></a> 

### <a name="environment-variables-properties"></a>Vlastnosti proměnné prostředí
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **proměnné prostředí**. V tomto dialogu máte možnost toocreate hello proměnné prostředí, a také upravit nebo odebrat proměnné prostředí, jak ukazuje následující obrázek hello.

![][ic719506]

Proměnné prostředí jsou k dispozici tooyour spouštěcí skript při spuštění hello role.

> [!NOTE]
> Při zadávání proměnných prostředí, mějte na paměti, že nasazení bude publikované tooa virtuálního počítače s Windows, tak proměnných prostředí musí být platná pro operačního systému Windows.
> 
> 

Jako příklad proměnné prostředí, který je k dispozici, když se spustí hello role, vytvořte novou proměnnou prostředí kliknutím hello **přidat** tlačítko. Hello následující ukazuje proměnnou prostředí s názvem **MyRoleVersion** je vytvořit a přiřadit hodnotu hello **1.0**.

![][ic659268]

V rámci jsp kódu, můžete zobrazit hodnotu hello pomocí hello `System.getenv` metoda:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Při spuštění aplikace, což vede k tento výstup:

![][ic552233]

Vyberte proměnné prostředí hello toomodify proměnné prostředí a klikněte na tlačítko hello **upravit** tlačítka na hello **proměnné prostředí** stránku vlastností. Povolení jste toomodify hello vlastnosti proměnné prostředí se otevře dialogové okno. Stiskněte klávesu **OK** toosave hello hodnot proměnných prostředí.

Vyberte proměnné prostředí hello toodelete proměnné prostředí a klikněte na tlačítko hello **odebrat** tlačítka na hello **proměnné prostředí** stránku vlastností a pak klikněte na tlačítko **Ano**tooconfirm hello odstranění.

V pořadí tooproperly konfigurace, některé funkce hello (například konfigurace serveru, vzdáleném ladění nebo místní úložiště) povolen uživatelem hello na roli, hello toolkit může automaticky konfigurovat speciální proměnné, které se objeví spolu s proměnné uživatelské prostředí. Sada nástrojů Hello zabrání uživateli hello úpravy nebo odstranění těchto proměnných prostředí automaticky generované tak dlouho, dokud hello související funkce povolena.

<a name="session_affinity_properties"></a> 

### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Vyrovnávání zatížení nebo vlastnosti relace spřažení (známé jako "trvalé relace")
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **Vyrovnávání zatížení**. V rámci toto dialogové okno máte možnost tooenable hello nebo zakažte spřažení relace, jak ukazuje následující obrázek hello.

![][ic719492]

Související informace najdete v tématu [spřažení relace][Session Affinity]. Mějte také na paměti tato funkce chování v kontextu hello snižování zátěže protokolu SSL, jak je popsáno v [snižování zátěže protokolu SSL][SSL Offloading].

<a name="local_storage_properties"></a> 

### <a name="local-storage-properties"></a>Místní úložiště vlastností
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **místní úložiště**. V tomto dialogu mít možnost toocreate hello, upravit nebo odebrat dočasné místní úložiště pro hello virtuální počítač, který je spuštění aplikace. Konkrétní hodnoty lze nastavit pro velikost hello hello místní úložiště, stejně jako jestli hello obsah se zachová, i když hello role dojde k recyklování, jak ukazuje následující obrázek hello.

![][ic719508]

Volitelně můžete zadat proměnné prostředí, která odpovídá toohello místní úložiště.

Ve výchozím nastavení, vše, co nasadíte do Azure je umístěn (a rozbalené) v hello **approot** složky hello role instance. Zatímco většina jednoduché nasazení se vejde existuje i po rozbalení hello místo přidělené hello **approot** adresář je omezený a dobře definovaný (méně než 1 GB je možné logicky pravidlem). Proto tooensure Azure přiděluje dostatek místa na disku pro rozsáhlejší nasazení, které nemusí vyhovovat v hello **approot** složky, měli byste nastavit prostředek Místní úložiště pomocí hello **místní úložiště** Dialogové okno. Pro toodo snadný způsob, najdete v tématu [nasazení velkých nasazeních][Deploying Large Deployments].

Hello úložiště prostředků můžete snadno odkazovat z spuštění skriptů (například vaše **startup.cmd**) pomocí proměnné prostředí hello automaticky pomocí nástrojů Eclipse hello přidružené hello prostředků, jak je znázorněno v hello  **Místní úložiště** dialogové okno. Tuto proměnnou prostředí bude obsahovat úplnou cestu hello hello místní prostředek, kterou jste nakonfigurovali v době hello spouštěcí skript se spustí. 

Vyberte prostředek Místní úložiště hello toomodify prostředku Místní úložiště a klikněte na tlačítko hello **upravit** tlačítka na hello **místní úložiště** stránku vlastností. Zobrazí se dialogové okno se otevře povolení jste toomodify hello místní úložiště vlastností prostředku. Stiskněte klávesu **OK** toosave hello místní úložiště prostředků hodnoty.

Vyberte prostředek Místní úložiště hello toodelete prostředku Místní úložiště a klikněte na tlačítko hello **odebrat** tlačítka na hello **místní úložiště** stránku vlastností a pak klikněte na tlačítko **Ano** odstranění tooconfirm hello.

<a name="server_configuration_properties"></a> 

### <a name="server-configuration-properties"></a>Vlastnosti konfigurace serveru
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **konfigurace serveru**. V rámci toto dialogové okno máte možnost hello tooadd, odebrat a upravit hello JDK a aplikačního serveru Java, které používá vaše nasazení, a také přidat nebo odebrat aplikace hello (například soubory WAR, JAR nebo vymazat) používá vaše nasazení.

### <a name="jdk-configuration"></a>Konfigurace JDK
Tento dialog umožňuje toospecify hello JDK balíček toouse pro vaše nasazení. Pokud používáte Eclipse v systému Windows, můžete zadat hello JDK balíček toouse místně při spuštění v hello Azure emulátoru a máte možnost toodeploy hello této tooAzure místní instalace. Na jiný systém než Windows operační systémy, hello emulátoru JDK nastavení se nedá použít, a nemůžete nasadit hello místně nainstalován JDK, protože to není kompatibilní se systémem Windows. Ale, bez ohledu na to hello operačního systému, který používáte, vždy můžete mezi hello 3. stran JDK balíčky toodeploy tooAzure nebo bodu vlastní balíček JDK kompatibilní se systémem Windows z alternativního stažení umístění.

Hello následuje příklad, jak můžete zadat JDK v systému Windows:

![][ic780647]

Pokud používáte Eclipse v systému Windows, můžete zadat JDK toouse s hello výpočetní emulátor; toodo Zajistěte proto, **použití hello JDK z tuto cestu k souboru pro místní testování** se změnami hello **emulátoru nasazení** části. Zadejte místní cestu tooyour hello JDK; můžete procházet toodifferent JDK, pokud hello jeden toouse není vybrána automaticky. Máte také možnost toodeploy hello vaší JDK tooyour cloudové služby Azure; toodo tedy vyberte hello **nasazení Moje místní JDK (automatické odesílání toocloud úložiště)** možnost v hello **cloudové nasazení** části.

Poznámka: Na jiný systém než Windows operační systémy, hello **emulátoru nasazení** nastavení a hello **nasazení Moje místní JDK** možnost nejsou k dispozici. Hello následující příklad ilustruje zadáním JDK na Macu nebo jiné podporovaný operační systém jiný systém než Windows:

![][ic789643]

Bez ohledu na hello operační systém, máte následující dvě hello **cloudové nasazení** možnosti pro zdrojové hello a typ vašeho JDK balíčku:

* **Nasazení 3. stran JDK balíčku k dispozici v Azure** 
* **Nasazení z vlastní stahování** 

Pokud používáte hello **nasazení 3. stran JDK balíčku k dispozici prostřednictvím Azure** možnost:

1. Zaškrtněte políčko hello s názvem **nasazení 3. stran JDK balíčku k dispozici prostřednictvím Azure**.
2. Hello rozevíracím seznamu vyberte hello 3. stran JDK balíček, který je k dispozici v Azure.
3. Vaše **JDK** karta bude vypadat podobně jako následující toohello v systému Windows: ![][ic780648] a bude vypadat podobně jako následující toohello v systému Mac OS nebo jiné podporované operační systémy jiný systém než Windows:![][ic789643]
4. Klikněte na tlačítko **OK** toosave změny.
5. Když výzvami tooaccept hello licenční smlouvy od hello 3. stran JDK balíček zprostředkovatele, přečtěte si podmínky licenční hello. Za předpokladu, že vyjadřujete souhlas s podmínkami hello, klikněte na tlačítko **Ano** tooclose hello **přijmout licenční smlouvu** dialogové okno.
    Všimněte si, že hello základní logiku, pro který položky v seznamu se zobrazí hello rozevíracího seznamu pro hello **nasazení 3. stran JDK balíčku k dispozici prostřednictvím Azure** možnost lze přizpůsobit. položky hello toocustomize, v hello **JDK** dialogové okno, klikněte na tlačítko hello **přizpůsobit** odkaz. To se zavře hello **JDK** stránku vlastností a otevřete hello **componentsets.xml** souboru v prostředí Eclipse, který poté můžete upravit podle potřeby. Dokumentace pro **componentsets.xml** je součástí hello **componentsets.xml** samotném souboru.

Pokud používáte hello **nasazení JDK z vlastní stahování** možnost:

1. Vytvořit ZIP adresáře instalace JDK, zajistit, že hello directory uzel je podřízeným hello hello ZIP struktura a ne její obsah. Povšimněte si hello název adresáře hello, jak je budete potřebovat později a mějte na paměti Toto JDK instalace budou nasazené tooa Windows virtuálního počítače.
2. Nahrajte hello ZIP do účtu úložiště Azure jako objekt blob. To provedete pomocí externě dostupný nástroj pro nahrávání tooAzure úložiště objektů BLOB. Je doporučeno toouse privátní objektů blob. Poznamenejte si adresu URL objektů blob hello hello ZIP obsahu.
3. Zaškrtněte políčko hello s názvem **nasazení JDK z vlastní stahování**.
    Pokud chcete toodownload z vašeho účtu úložiště Azure, vyberte účet úložiště hello z hello **účet úložiště** rozevíracího seznamu (můžete kliknout na hello **účty** odkaz toomodify, co je v seznamu hello), které budou částečně vyplňte hello **URL** pole a pak zadejte zbývající část adresy URL hello hello. Pokud nechcete, aby toouse úložiště Azure, vyberte **(none)** z hello **účet úložiště** rozevíracího seznamu seznamu a zadejte hello URL tooyour JDK stáhnout hello **URL** pole. Pokud používáte Azure storage, názvy objektů blob v adrese URL hello musí být malé.
4. Ujistěte se, že hello **JAVA_HOME** textbox odkazuje název toohello správném adresáři. Ve výchozím nastavení, bude odkazovat hello stejný název adresáře JDK jako hodnota hello jste zvolili pro místní použití. Ale pokud hello directory obsažené v hello ZIP má jiný název (například kvůli toousing jinou verzi), název adresáře hello aktualizace v hello **JAVA_HOME** textbox odpovídajícím způsobem, protože toto nastavení se použije v hello cloudu ( není v hello emulátoru služby výpočty).
5. Klikněte na tlačítko **OK** toosave změny.

A je to. Teď když vytvoříte pro hello cloud, si všimnete budou mnohem menší velikost balíčku hello hello procesu sestavení by měl obvykle trvá méně času a hello nasazení, samotné při publikování cloudu toohello také provést méně času. Všimněte si, že hello **nasazení Moje místní JDK (automatické odesílání toocloud úložiště)** nebo **nasazení JDK z vlastní stahování** jsou možnosti platí pouze, když je aplikace nasazená v cloudu hello. To mít žádný vliv na vaše uživatelské prostředí emulátoru výpočtů; Hello místní verze součástí hello se stále použije, když nasazujete emulátoru služby výpočty toohello. 

### <a name="server-configuration"></a>Konfigurace serveru
Hello následuje příklad, jak můžete zadat aplikační server.

![][ic796926]

Ověřte, že hello **nasadit server tohoto typu** zaškrtávací políčko je vybraná a pak vyberte typ hello aplikační server, na které má toouse.

Pro zadání toouse serveru pro nasazení cloudu, můžete využít výhod hello následující možnosti:

1. **Nasazení k dispozici v Azure 3. stran serveru** – to platí hlavně v scénáře vývoje/testování kde nasazení efektivitu a jednoduchost, je prioritu a hello server nevyžaduje vlastní konfiguraci. Nebo pokud chcete používat jako výchozí bod hello toouse jednu z těchto serverů, ale zahrnete přizpůsobení příslušného serveru kroky v logika spuštění vašeho nasazení.
2. **Nasazení z vlastní stahování** – to je obzvláště použít v produkčních scénářích, když máte speciálně připravené a konfiguraci serveru, které chcete toouse v cloudu hello.
3. **Moje instalace místní server nasazení** – to platí hlavně v Pokud už je místní server instalace konfigurovány pro vaše použití. Pokud zvolíte tuto možnost, musíte také zadat cestu místní server v hello **místní server cesta** textového pole níže.

Pokud používáte hello **nasazení k dispozici v Azure 3. stran serveru** možnost:

1. Zaškrtněte políčko hello s názvem **nasazení k dispozici v Azure 3. stran serveru**.
2. Hello rozevírací nabídce vyberte hello požadovaného serveru softwaru toouse s nasazením v cloudu hello. Všimněte si, pokud jste již zadali typ serveru toouse dříve, bude omezený toochoosing pouze server cloudu, který je v hello stejná rodina jako tento typ serveru. Ale pokud jste nezvolili typ serveru, můžete vybrat ze všech hello serverů, které jsou aktuálně dostupné v Azure a typ serveru hello bude automaticky vybrána pro vás.
3. Klikněte na tlačítko **OK** toosave změny.

Pokud používáte hello **nasadit z vlastní stahování** možnost:

1. Ujistěte se, že jste již vybrali podle předchozích kroků toohello typ serveru. Tato hodnota informuje modul plug-in hello jak server hello toodeploy z vlastní stahování, protože musí být z hello stejná rodina jako typ vybraný server.
2. Zaškrtněte políčko hello s názvem **nasadit z vlastní stahování**.
    Pokud chcete toodownload z vašeho účtu úložiště Azure, vyberte účet úložiště hello z hello **účet úložiště** rozevíracího seznamu (můžete kliknout na hello **účty** odkaz toomodify, co je v seznamu hello), které budou částečně vyplňte hello **URL** pole a potom vyplňte hello zbývající část adresy URL tooyour serveru hello stáhnout ZIP (při použití služby Azure storage, názvy objektů blob v adrese URL hello musí být malá písmena). Pokud nechcete, aby toouse úložiště Azure, vyberte **(none)** z hello **účet úložiště** rozevíracího seznamu seznamu a zadejte hello URL tooyour serveru stáhnout ZIP v hello **URL** pole. Hello ZIP obsahuje podřízenou složku představující instalační adresář serveru vaší aplikace. Například pokud používáte zip pro Apache Tomcat 7.0.35, v rámci hello zip by hello podřízené složky představující hello instalační adresář, jako například **apache tomcat-7.0.35**. 
3. Zadejte hodnotu proměnné prostředí domovský adresář hello hello. Se nastaví jako výchozí hodnota toohello používaná pro místní aplikačního serveru, pokud existuje, ale můžete zadat jinou hodnotu, pokud váš server cloudové aplikace se liší od místní aplikačního serveru. Ale musíte toomake se, že váš server cloudové aplikace je dobrý den stejná rodina jako typ serveru hello vybrali dříve.
    Pokud aktualizujete vaší cloudové aplikace server zip v hello budoucí, můžete ručně změnit nastavení hello domovský adresář, nebo, toohave znovu odpovídají vaše místní nastavení (Pokud jste změnili příliš místního aplikačního serveru).
4. Klikněte na tlačítko **OK** toosave změny.

Hello základní logiku, pro které položky se zobrazí v hello **Server** kartě hello **konfigurace serveru** stránka vlastností lze přizpůsobit. Toto je pokročilá funkce, které byste mohli potřebovat, pokud vaše potřeby rozšířit nad rámec hello výchozí hodnoty, nebo pokud chcete tooadd jiných serverů. toocustomize hello logiku, hello **Server** dialogové okno, klikněte na tlačítko hello **přizpůsobit** odkaz. To se zavře hello **konfigurace serveru** stránku vlastností a otevřete hello **componentsets.xml** souboru v prostředí Eclipse, který poté můžete upravit jako potřebné tooextend hello serveru konfigurace šablony. Dokumentace pro **componentsets.xml** je součástí hello **componentsets.xml** samotném souboru.

Pokud používáte hello **nasazení Moje místní server (automatické odesílání toocloud úložiště)** možnost:

1. Zaškrtněte políčko hello s názvem **nasazení Moje místní server (automatické odesílání toocloud úložiště)**.
2. Pomocí hello **účet úložiště** rozevíracího seznamu vyberte **(automaticky)**. Pokud zadáte **(automaticky)** zde hello Eclipse toolkit použije hello stejný účet úložiště pro váš server jako hello jeden vyberete pro vaše nasazení v hello **publikování tooAzure** dialogové okno.
3. Klikněte na tlačítko **OK** toosave změny.

Vyberte cestu instalace serveru ve vašem počítači v hello **místní server cesta** textového pole, pokud platí některá z hello následující podmínky:

* Chcete tootest nasazení v emulátoru hello (platí pouze tooWindows).
* Chcete toodeploy cloudu toohello místně nainstalovaný server.
* Má vlastní server stahování vlastní aplikaci v cloudu hello, v takovém případě toouse, ujistěte se také hello **nasazení Moje místní server (automatické odesílání toocloud úložiště)** možnost výše.

Pokud žádná z předchozích možností hello použít tooyour situaci, nastavení místní server hello je volitelné.

### <a name="applications-configuration"></a>Konfigurace aplikace
Hello následuje příklad, jak můžete zadat aplikace.

![][ic719512]

Klikněte na tlačítko **přidat** tooadd jiná aplikace, nebo **odebrat** tooremove aplikace. Pro účely účinnost, pokud chcete toouse stahování zdroje hello aplikace při nasazování toohello cloudu, použijte hello [vlastnosti součásti](#components_properties) toospecify a adresu URL, účet úložiště, atd. 

Počínaje hello vydání duben 2014, vaše aplikace se automaticky nahraje do hello stejný účet úložiště (v části hello **eclipsedeploy** kontejneru) jako hello jeden vybrané pro vaše nasazení. Logika spuštění Hello nasazení obsahuje krok, který nejprve stáhne tyto aplikace z daného účtu úložiště. To znamená, že můžete upgradovat aplikace ve vašem nasazení bez nutnosti toorebuild a znovu nasaďte hello celý balíček, ručně odesláním novější verze aplikace hello přímo do tohoto účtu úložiště (například pomocí hello portál Azure) , nahraďte soubory WAR hello původně odeslaný existuje hello toolkit. Potom právě zahajte hello recyklaci všech těchto instancí rolí pomocí portálu pro správu Azure, je znovu, nebo pomocí nástroje příkazového řádku. (Spuštění role recyklace přímo z v rámci nástrojů Eclipse hello se aktuálně nepodporuje.)

### <a name="notes-about-server-configuration"></a>Poznámky k nástroji Konfigurace serveru
Změny provedené prostřednictvím hello **konfigurace serveru** stránka vlastností se projeví v hello `<component>` prvky hello package.xml souboru.

Při použití hello **automaticky odeslat...**  nebo **nasazení ze stažení...**  jsou možnosti hello JDK nebo aplikační server a můžete vytvářet pro cloud hello (ne hello emulátoru služby výpočty) a jsou připojené toohello sítí, může dojít k vytvoření zprávy, jako je například hello následující výstup hello konzoly, jako hello Ant Tvůrce ověřuje hello stažení dostupnosti:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Pokud jste vybrali hello **nasazení ze stažení...**  možnost, hello následující upozornění se může zobrazit, ale bude pokračovat hello sestavení:

`[windowsazurepackage] warning: Failed tooconfirm blob availability! Make sure hello URL and/or hello access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Toto upozornění je, že hello jediné, hello na stažení dostupnosti nebyla ověřena. Proto pokud z nějakého důvodu selže hello cloudové nasazení, zkontrolujte toosee, pokud jste dostali toto upozornění.

Pokud chcete toodisable hello stažení ověření (např. Pokud si myslíte, že ho zbytečně zpomaluje sestavení hello), nastavte hello `verifydownloads` atribut příliš`false` v hello `<windowsazurepackage>` element package.xml: 

`<windowsazurepackage verifydownloads="false" ...>` 

Pokud jste vybrali hello **automaticky odeslat...**  možnost, pak v okně konzoly hello uvidíte reporting hello průběh nahrávání hello každých 5 sekund, kdykoli je nezbytné odeslání zprávy sestavení.

<a name="ssl_offloading_properties"></a> 

### <a name="ssl-offloading-properties"></a>Vlastnosti snižování zátěže protokolu SSL
V podokně Průzkumník projektů Eclipse na otevřete hello kontextovou nabídku hello role, klikněte na **Azure**a potom klikněte na **snižování zátěže protokolu SSL**. 

![][ic719481]

V tomto dialogu můžete povolit protokol SSL snižování zátěže, což vám povolit tooeasily, které podporují protokol Secure HTTPS (Hypertext Transfer) ve vašem nasazení Java v Azure, aniž byste si tooconfigure SSL v aplikační server Java. Další informace najdete v tématu [snižování zátěže protokolu SSL] [ SSL Offloading] a [jak tooUse snižování zátěže protokolu SSL][How tooUse SSL Offloading].

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse]

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Vlastnosti projektu Azure][Azure Project Properties]

[Seznam účtů úložiště Azure][Azure Storage Account List]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].

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
[How tooUse Co-located Caching]: http://go.microsoft.com/fwlink/?LinkID=699542
[How tooUse SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
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
