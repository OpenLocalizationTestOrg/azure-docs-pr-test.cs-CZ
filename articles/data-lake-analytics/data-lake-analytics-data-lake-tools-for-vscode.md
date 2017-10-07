---
title: "Nástroje Azure Data Lake: Nástroje použití Azure Data Lake pro Visual Studio Code | Microsoft Docs"
description: "Zjistěte, jak nástrojů toouse Azure Data Lake pro Visual Studio Code toocreate, testovat a spouštět skripty U-SQL. "
Keywords: "VScode, nástroje Azure Data Lake, místní spuštění souboru úložiště místní ladění, místní ladění preview, nahrajte toostorage cesta"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 77771c5d5dae3bfce4ad2df240ea6c6ef848f288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Pomocí nástroje Azure Data Lake pro Visual Studio Code

Zjistěte, jak nástrojů toouse Azure Data Lake pro Visual Studio Code (kód VS) toocreate, testovat a spouštět skripty U-SQL. Hello informace jsou také obsaženy v hello následující video:

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Požadavky

Nástroje data Lake lze nainstalovat na hello platformách podporovaných aplikací VS Code. jsou následující Hello podporované platformy Windows, Linux a systému MacOS. různé platformy Hello mít hello následující požadavky:

- Windows

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Aktualizace verze 8 prostředí Java Runtime Administrátor 77 nebo novější](https://java.com/download/manual.jsp). Přidejte hello java.exe cesta toohello systému prostředí cestu k proměnné. Pokyny ke konfiguraci, najdete v části [jak nastavit nebo změnit systémové proměnné Path hello?]( https://www.java.com/download/help/path.xml). Cesta Hello je podobné tooC:\Program Files\Java\jdk1.8.0_77\jre\bin.
    - [.NET core SDK 1.0.3 nebo modul runtime .NET Core 1.1](https://www.microsoft.com/net/download).
    
- Linux (doporučujeme Ubuntu 14.04 LTS)

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx). tooinstall hello kód, zadejte následující příkaz hello:

              sudo dpkg -i code_<version_number>_amd64.deb

    - [Monofonní 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/). 

        - balíček bázi deb hello tooupdate zdroje, zadejte hello následující příkazy:

                sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
                echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots 4.2.4.4/main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
                sudo apt-get update

        - tooinstall Mono, zadejte následující příkaz hello:

                sudo apt-get install mono-complete

            > [!NOTE] 
            > Mono 4.6 není podporována. Odinstalujte verzi 4.6 zcela, před instalací 4.2.x.  

        - [Aktualizace verze 8 prostředí Java Runtime Administrátor 77 nebo novější](https://java.com/download/manual.jsp). Pokyny k instalaci naleznete v tématu hello [Linux 64-bit pokyny k instalaci pro jazyk Java]( https://java.com/en/download/help/linux_x64_install.xml) stránky.
        - [.NET core SDK 1.0.3 nebo modul runtime .NET Core 1.1](https://www.microsoft.com/net/download).
- MacOS

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Mono 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/). 
    - [Aktualizace verze 8 prostředí Java Runtime Administrátor 77 nebo novější](https://java.com/download/manual.jsp). Pokyny k instalaci naleznete v tématu hello [Linux 64-bit pokyny k instalaci pro jazyk Java](https://java.com/en/download/help/mac_install.xml) stránky.
    - [.NET core SDK 1.0.3 nebo modul runtime .NET Core 1.1](https://www.microsoft.com/net/download).

## <a name="install-data-lake-tools"></a>Nainstalovat nástroje Data Lake

Po instalaci hello požadavky, můžete nainstalovat nástroje Data Lake pro VS Code.

**tooinstall nástrojů Data Lake**

1. Otevřete Visual Studio Code.
2. Vyberte Ctrl + P a potom zadejte hello následující příkaz:
```
ext install usql-vscode-ext
```
Zobrazí se seznam rozšíření kódu v sadě Visual Studio. Jeden z nich je **nástrojů Azure Data Lake**.

3. Vyberte **nainstalovat** další příliš**nástrojů Azure Data Lake**. Za několik sekund, hello **nainstalovat** tlačítko změny příliš**opětovného načtení**.
4. Vyberte **opětovného načtení** tooactivate hello rozšíření.
5. Vyberte **OK** tooconfirm. Zobrazí nástroje Azure Data Lake v hello **rozšíření** podokně.
    ![Nástroje data Lake pro Visual Studio rozšíření kódu podokno](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

## <a name="activate-azure-data-lake-tools"></a>Aktivovat nástrojů Azure Data Lake
Vytvořit nový soubor .usql nebo otevřít existující tooactivate hello příponu souboru .usql. 

## <a name="connect-tooazure"></a>Připojit tooAzure

Předtím, než můžete zkompilování a spuštění skriptů U-SQL v Data Lake Analytics, je nutné připojit tooyour účet Azure.

**tooconnect tooAzure**

1.  Vyberte Ctrl + Shift + P tooopen hello příkaz palety. 
2.  Zadejte **ADL: přihlášení**. Hello přihlašovací informace se zobrazí v hello **výstup** podokně.

    ![Nástroje data Lake pro Visual Studio Code příkaz palety](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![nástroje Data Lake pro Visual Studio Code zařízení přihlašovací informace](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Vyberte Ctrl + klikněte na adresu URL pro přihlášení k hello: https://aka.ms/devicelogin tooopen hello přihlašovací webovou stránku. Zadejte kód hello **G567LX42V** do hello textového pole a pak vyberte **pokračovat**.

   ![Nástroje data Lake pro Visual Studio Code přihlášení vložte kód](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Postupujte podle pokynů toosign hello v z webovou stránku hello. Když jste připojení, název účtu Azure se zobrazí na stavovém řádku hello v levém dolním rohu hello hello **VS Code** okno. 

    > [!NOTE] 
    > Pokud váš účet má dvě úrovně povoleno, doporučujeme použít ověřování phone místo pomocí PIN kódu.

toosign limitu, zadejte příkaz hello **ADL: odhlášení**.

## <a name="list-your-data-lake-analytics-accounts"></a>Seznam svých účtů Data Lake Analytics

tootest hello připojení, get seznam svých účtů Data Lake Analytics.

**toolist hello účtů Data Lake Analytics v rámci vašeho předplatného Azure**

1. Vyberte Ctrl + Shift + P tooopen hello příkaz palety.
2. Zadejte **ADL: seznamu účtů**. účty Hello se zobrazí v hello **výstup** podokně.

## <a name="open-hello-sample-script"></a>Otevřete hello ukázkový skript
Spusťte příkaz palety hello (Ctrl + Shift + P) a zadejte **ADL: Otevřete ukázkový skript**. Otevře další instanci této ukázce. Můžete také upravit, konfigurovat a odeslat skript v této instanci.

## <a name="work-with-u-sql"></a>Práce s jazykem U-SQL

U-SQL souboru nebo složky toowork třeba otevřete pomocí U-SQL.

**tooopen do složky pro váš projekt U-SQL**

1. Visual Studio Code, vyberte hello **soubor** nabídce a potom vyberte **otevřít složku**.
2. Zadejte složku a potom vyberte **vyberte složku**.
3. Vyberte hello **soubor** nabídce a potom vyberte **nový**. Soubor bez názvu-1 se přidá toohello projektu.
4. Zadejte následující kód do souboru hello bez názvu-1 hello:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO “/Output/departments.csv”

    skript Hello vytvoří soubor departments.csv s některé dat obsažených ve složce/Output hello.

5. Uložte soubor hello jako **myUSQL.usql** v hello otevřít složku. Konfigurační soubor adltools_settings.json je taky přidaná toohello projektu.
4. Otevřít a konfigurovat adltools_settings.json s hello následující vlastnosti:

    - Účet: Účet Data Lake Analytics v rámci vašeho předplatného Azure.
    - Databáze: Databáze v rámci vašeho účtu. Výchozí hodnota Hello je **hlavní**.
    - Schémat: Schéma v databázi. Výchozí hodnota Hello je **dbo**.
    - Volitelné nastavení:
        - Priorita: rozsah priority hello je z 1 too1000 s 1 jako hello nejvyšší prioritou. Hello výchozí hodnota je **1000**.
        - Paralelismus: hello paralelismus rozsah je od 1 too150. Hello výchozí hodnota je hello maximální paralelismus v účtu Azure Data Lake Analytics povolen. 
        
        > [!NOTE] 
        > Pokud jsou neplatné nastavení hello, použijí se výchozí hodnoty hello.

    ![Nástroje data Lake pro Visual Studio Code konfiguračního souboru](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

    Výpočetní, účet Data Lake Analytics je potřeba toocompile a spuštění úloh U-SQL. Předtím, než můžete zkompilování a spuštění úloh U-SQL, je nutné nakonfigurovat účet počítače hello.
    
Po uložení konfigurace hello hello účet, databázi a schéma informace se zobrazí na stavovém řádku hello v hello okraje v levém dolním rohu odpovídající soubor .usql hello. 
 
 
Porovnání tooopening soubor, když otevřete složku, kterou můžete:

- Použijte soubor kódu. V režimu jednoho souboru hello nepodporuje kódu.
- Pomocí konfiguračního souboru. Když otevřete složku, hello skripty v hello pracovní složky sdílet jednom konfiguračním souboru.


Hello skript U-SQL zkompiluje vzdáleně přes hello služby Data Lake Analytics. Pokud vydáte hello **zkompilovat** příkaz hello skript U-SQL je odeslán tooyour účet Data Lake Analytics. Visual Studio Code později, obdrží hello kompilace výsledek. Kvůli toohello vzdálené kompilace Visual Studio Code vyžaduje, že uvedete hello informace tooconnect tooyour účtu Data Lake Analytics v konfiguračním souboru hello.

**skript toocompile U-SQL**

1. Vyberte Ctrl + Shift + P tooopen hello příkaz palety. 
2. Zadejte **ADL: kompilace skriptu**. Hello kompilace výsledky se zobrazí v hello **výstup** okno. Můžete také klikněte pravým tlačítkem na soubor skriptu a potom vyberte **ADL: kompilace skriptu** úlohy toocompile U-SQL. Výsledek kompilace Hello se zobrazí v hello **výstup** podokně.
 

**skript toosubmit U-SQL**

1. Vyberte Ctrl + Shift + P tooopen hello příkaz palety. 
2. Zadejte **ADL: odeslat úlohu**.  Můžete také klikněte pravým tlačítkem na soubor skriptu a potom vyberte **ADL: odeslat úlohu**. 

Po odeslání úlohy U-SQL hello odeslání protokolů se objeví v hello **výstup** okno v produktu VS Code. Pokud hello odeslání úspěšné, se zobrazí také adresu URL hello úlohy. Adresa URL hello úlohy lze otevřít v stav v reálném čase úlohy hello tootrack webového prohlížeče.

výstup hello tooenable hello podrobnosti úlohy, nastavte **jobInformationOutputPath** v hello **vs kód hello u-sql_settings.json** souboru.
 
## <a name="use-a-code-behind-file"></a>Použít soubor kódu

Soubor kódu je soubor C# přidružené jednoho skriptu U-SQL. V souboru kódu na pozadí hello můžete definovat skriptu vyhrazené tooUDO, UDA, UDT a UDF. Hello UDO, UDA, UDT a UDF lze přímo ve skriptu hello bez registrace sestavení hello nejdřív. Hello souboru kódu na pozadí uveden hello stejné složce jako jeho partnerského vztahu soubor skriptu U-SQL. Pokud skript hello název xxx.usql, hello kódu je pojmenována jako xxx.usql.cs. Pokud odstraníte ručně hello souboru kódu na pozadí, hello kódu funkce je vypnutá pro jeho přidružené skript U-SQL. Další informace o psaní kódu zákazníka pro skript U-SQL najdete v tématu [zápis a pomocí vlastní kód v U-SQL: funkce definované uživatelem]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).

toosupport kódu, je nutné otevřít pracovní složky. 

**toogenerate souboru kódu na pozadí**

1. Otevřete zdrojový soubor. 
2. Vyberte Ctrl + Shift + P tooopen hello příkaz palety.
3. Zadejte **ADL: vygenerování kódu na pozadí**. Soubor kódu se vytvoří v hello stejné složce. 

Můžete také klikněte pravým tlačítkem na soubor skriptu a potom vyberte **ADL: generování kódu za**. 

toocompile a odeslat skript U-SQL pomocí souboru kódu je hello stejná hodnota jako soubor skriptu hello samostatné U-SQL.

Hello následující dva snímky obrazovky ukazují souboru kódu na pozadí a jeho přidružený soubor skriptu U-SQL:
 
![Nástroje data Lake pro Visual Studio Code kódu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![Soubor skriptu nástrojů data Lake pro Visual Studio Code kódu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

## <a name="use-assemblies"></a>Použití sestavení

Informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).

Nástroje Data Lake tooregister vlastní kód sestavení můžete použít v katalogu Data Lake Analytics hello.

**tooregister sestavení**

Můžete zaregistrovat sestavení hello prostřednictvím hello **ADL: registrace sestavení** nebo **ADL: registrace sestavení prostřednictvím konfigurace** příkazy.

**tooregister prostřednictvím hello ADL: příkaz registrace sestavení**
1.  Vyberte Ctrl + Shift + P tooopen hello příkaz palety.
2.  Zadejte **ADL: registrace sestavení**. 
3.  Zadejte cestu k místní sestavení hello. 
4.  Vyberte účet Data Lake Analytics.
5.  Vyberte databázi.

Výsledky: portál hello je otevřen v prohlížeči a zobrazí procesu registrace sestavení hello.  

Jiné hello tootrigger pohodlný způsob **ADL: registrace sestavení** příkaz je soubor .dll hello tooright klikněte v Průzkumníku souborů. 

**tooregister ale hello ADL: registrace sestavení pomocí příkazu konfigurace**
1.  Vyberte Ctrl + Shift + P tooopen hello příkaz palety.
2.  Zadejte **ADL: registrace sestavení prostřednictvím konfigurace**. 
3.  Zadejte cestu k místní sestavení hello. 
4.  Zobrazí se soubor JSON Hello. Zkontrolujte a v případě potřeby upravit hello sestavení závislosti a parametry prostředků. Pokyny, které se zobrazují v hello **výstup** okno. tooproceed toohello sestavení registrace, uložte soubor JSON hello (Ctrl + S).

![Nástroje data Lake pro Visual Studio Code kódu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- Závislosti sestavení: automatické nástrojů Azure Data Lake jestli hello DLL má všechny závislosti. Po zjištění, zobrazí se Hello závislosti v souboru JSON hello. 
>- Prostředky: Jako součást registrace sestavení hello můžete nahrát vašich prostředků knihovny DLL (například TXT, .png a CSV). 

Jiný způsob tootrigger hello **ADL: registrace sestavení prostřednictvím konfigurace** příkaz je soubor .dll hello tooright klikněte v Průzkumníku souborů. 

Hello následující U-SQL kód ukazuje, jak toocall sestavení. V ukázce hello hello název sestavení je *testování*.

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    too@"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```


## <a name="access-hello-data-lake-analytics-catalog"></a>Katalog Data Lake Analytics hello přístup

Po připojení tooAzure, můžete použít následující kroky tooaccess hello U-SQL katalogu hello.

**tooaccess hello metadata Azure Data Lake Analytics**

1.  Vyberte Ctrl + Shift + P a potom zadejte **ADL: seznamu tabulek**.
2.  Vyberte jeden z účtů Data Lake Analytics hello.
3.  Vyberte jednu z databáze hello Data Lake Analytics.
4.  Vyberte jednu z hello schémat. Zobrazí seznam hello tabulek.

## <a name="view-data-lake-analytics-jobs"></a>Úlohy zobrazení Data Lake Analytics

**tooview úloh Data Lake Analytics**
1.  Otevřete hello příkaz palety (Ctrl + Shift + P) a vyberte **ADL: Zobrazit úlohy**. 
2.  Vyberte Data Lake Analytics nebo místní účet. 
3.  Počkejte hello seznamu úloh pro účet tooappear hello.
4.  Vyberte ze seznamu úloh úlohu, nástrojů Data Lake podrobnosti úlohy hello se otevře v hello portál Azure a zobrazí soubor informací o úloze hello v produktu VS Code.

![Typy objektů nástroje data Lake pro Visual Studio kódu IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Integrace úložiště Azure Data Lake

Můžete použít příkazy související s Azure Data Lake Storage na:
 - Procházejte prostředky Azure Data Lake Storage hello. 
 - Náhled souboru Azure Data Lake Storage hello.  
 - Nahrát hello tooAzure Data Lake Storage souboru přímo v kódu VS. 

### <a name="list-hello-storage-path"></a>Cestu k úložišti hello seznamu 
Můžete vytvořit seznam cestu k úložišti hello prostřednictvím hello příkaz palety nebo klikněte pravým tlačítkem.

**cestu k úložišti hello toolist prostřednictvím hello příkaz palety**

1.  Spusťte příkaz palety hello (Ctrl + Shift + P) a zadejte **ADL: cestu k úložišti seznamu**.

    ![Nástroje data Lake pro Visual Studio Code seznamu cestu k úložišti](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-storage.png)

2.  Vyberte vaše upřednostňovaný způsob pro výpis cestu k úložišti hello. Používá tento průchod **zadejte cestu** jako příklad.

    ![Nástroje data Lake pro Visual Studio Code cestu k úložišti jedním ze způsobů toolist hello](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)

    > [!NOTE]
    >- VS Code zajišťuje hello poslední navštívené cesta každý účet Data Lake Analytics. Například: / tt/ss.
    >- Prohlížeči kořenovou cestu: hello seznamu kořenovou cestu z účtu Data Lake Analytics vybrané nebo místní cestu.
    >- Zadejte cestu: seznam zadanou cestu z účtu Data Lake Analytics vybrané nebo místní cestu.
    
3. Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.

    ![Nástroje data Lake pro Visual Studio Code vyberte více](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4. Vyberte **Další** toolist další účty Data Lake Analytics a potom vyberte účet Data Lake Analytics.

    ![Nástroje data Lake pro Visual Studio Code vyberte účet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

5.  Zadejte cestu k úložišti Azure. Například/výstup.

    ![Nástroje data Lake pro Visual Studio Code zadejte cestu k úložišti](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-path.png)

6.  Výsledky: hello příkaz palety uvádí informace o cestě hello hodnot.

    ![Nástroje data Lake pro Visual Studio Code seznam výsledků cesta úložiště](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-path.png)

Pohodlnější způsob toolist hello relativní cesta je prostřednictvím hello klikněte pravým tlačítkem na místní nabídce.

**Klikněte pravým tlačítkem na cestu k úložišti hello toolist prostřednictvím**

1.  Klikněte pravým tlačítkem na hello cesta řetězec tooselect **cestu k úložišti seznamu**.

       ![Nástroje data Lake pro Visual Studio Code, klikněte pravým tlačítkem na kontextové nabídky](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)

2. Vybraná cesta relativní Hello se zobrazí v palety příkaz hello.

   ![Nástroje data Lake pro Visual Studio Code vybraná relativní cesta.](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-relative-path.png)

3.  Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.

       ![Nástroje data Lake pro Visual Studio Code vyberte účet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4.  Výsledky: hello příkaz palety uvádí hello složek a souborů pro aktuální cestu hello.

       ![Nástroje data Lake pro Visual Studio Code seznam z aktuální cestě hello](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-current.png)

### <a name="preview-hello-storage-file"></a>Soubor úložiště hello Preview
Můžete zobrazit náhled souboru hello úložiště prostřednictvím hello příkaz palety nebo klikněte pravým tlačítkem.

**soubor úložiště hello toopreview prostřednictvím hello příkaz palety**

1.  Spusťte příkaz palety hello (Ctrl + Shift + P) a zadejte **ADL: soubor úložiště Preview**.

       ![Nástroje data Lake pro Visual Studio Code preview úložiště souborů](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-preview.png)

2.  Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.

       ![Nástroje data Lake pro Visual Studio Code seznamu účtu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  Vyberte **Další** toolist další účty Data Lake Analytics a potom vyberte účet Data Lake Analytics.

       ![Nástroje data Lake pro Visual Studio Code vyberte účet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

4.  Zadejte cestu k úložišti Azure nebo souboru. Například /output/SearchLog.txt.

       ![Nástroje data Lake pro Visual Studio Code zadejte cestu k úložišti a souborů](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

5.  Výsledky: hello příkaz palety uvádí informace o cestě hello hodnot.

       ![Nástroje data Lake pro Visual Studio Code náhled souboru výsledků](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

**Klikněte pravým tlačítkem na cestu k úložišti hello toolist prostřednictvím**

1.  Klikněte pravým tlačítkem toopreview souboru, cesta k souboru hello.

   ![Nástroje data Lake pro Visual Studio Code, klikněte pravým tlačítkem na kontextové nabídky](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-preview.png) 

2.  Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.

       ![Nástroje data Lake pro Visual Studio Code vyberte účet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  Výsledky: VS Code zobrazí výsledky náhledu hello hello souboru.

       ![Nástroje data Lake pro Visual Studio Code náhled souboru výsledků](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

### <a name="upload-a-file"></a>Nahrání souboru 

Můžete nahrát soubory tak, že zadáte příkazy hello **ADL: nahrát soubor** nebo **ADL: nahrát soubor prostřednictvím konfigurace**.

**soubory tooupload ale hello ADL: nahrát soubor – příkaz**
1. Vyberte Ctrl + Shift + P tooopen hello příkaz palety nebo klikněte pravým tlačítkem na editor skriptů hello a potom zadejte **nahrát soubor**.
2.  tooupload hello souboru, zadejte místní cestu.

    ![Nástroje data Lake pro Visual Studio Code zadejte místní cestu.](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-input-local-path.png)

3. Vyberte některým z těchto způsobů hello výpis hello úložiště cesty. Používá tento průchod **zadejte cestu** jako příklad.

    ![Nástroje data Lake pro Visual Studio Code seznamu cestu k úložišti](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)
    >[!NOTE]
    >- VS Code zajišťuje hello poslední navštívené cesta každý účet Data Lake Analytics. Například: / tt/ss.
    >- Prohlížeči kořenovou cestu: hello seznamu kořenovou cestu z účtu Data Lake Analytics vybrané nebo místní cestu.
    >- Zadejte cestu: seznam zadanou cestu z účtu Data Lake Analytics vybrané nebo místní cestu.

4. Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.

    ![Nástroje data Lake pro Visual Studio Code, klikněte pravým tlačítkem na úložiště](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

5. Zadejte cestu k úložišti Azure. Například: / výstup.

       ![Data Lake Tools for Visual Studio Code enter storage path](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

6. Najít cestu k úložišti Azure. Vyberte **aktuální složce zvolte**.

    ![Vyberte složku nástroje data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-choose-current-folder.png)

7.  Výsledky: hello **výstup** okně se zobrazí stav nahrávání souboru hello.

       ![Nástroje data Lake pro Visual Studio Code nahrát stav](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)    

**soubory tooupload ale hello ADL: nahrát soubor pomocí příkazu konfigurace**
1.  Vyberte Ctrl + Shift + P tooopen hello příkaz palety nebo klikněte pravým tlačítkem na editor skriptů hello a potom zadejte **nahrát soubor prostřednictvím konfigurace**.
2.  VS Code zobrazí soubor JSON. Můžete zadat cesty k souborům a uložení více souborů v hello stejnou dobu. Pokyny, které se zobrazují v hello **výstup** okno. tooproceed tooupload hello soubor, uložte soubor JSON hello (Ctrl + S).

       ![Cesta k souboru nástroje data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  Výsledky: hello **výstup** okně se zobrazí stav nahrávání souboru hello.

       ![Nástroje data Lake pro Visual Studio Code nahrát stav](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

Jiný způsob tooupload toostorage souboru je prostřednictvím hello klikněte pravým tlačítkem na nabídky na úplná cesta hello souboru nebo relativní cestu hello souboru v editoru skriptů hello. Zadejte cestu k souboru místní hello a potom vyberte účet hello. Hello **výstup** okně se zobrazí stav nahrávání hello. 

### <a name="open-azure-storage-explorer"></a>Otevřete Azure Storage Explorer
Můžete otevřít **Azure Storage Explorer** zadáním příkazu hello **ADL: Otevřete Web Azure Storage Explorer** nebo výběrem hello klikněte pravým tlačítkem na místní nabídce.

**tooopen Azure Storage Explorer**

1. Vyberte Ctrl + Shift + P tooopen hello příkaz palety.
2. Zadejte **otevřete Web Azure Storage Explorer** nebo klikněte pravým tlačítkem na relativní cestu nebo hello úplnou cestu v editoru hello skript a potom vyberte **otevřete Web Azure Storage Explorer**.
3. Vyberte účet Data Lake Analytics.

Nástroje data Lake otevře cestu k úložišti Azure hello v hello portálu Azure. Můžete najít hello cestu a náhled souboru hello z webové hello.

### <a name="local-run-and-local-debug-for-windows-users"></a>Místní spuštění a místní ladění pro systém Windows uživatele
U-SQL místní spuštění testů místní data a ověří váš skript místně, než váš kód je publikovaná tooData Lake Analytics. funkce umožňuje místní ladění Hello je hello toocomplete následující úkoly před váš kód je odeslána tooData Lake Analytics: 
- Ladění vaší C# kódu. 
- Projděte hello kódu. 
- Ověřte váš skript místně.

Pokyny v místním spuštění a místní ladění najdete v tématu [U-SQL místní spuštění a místní ladění s Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="additional-features"></a>Další funkce

Nástroje data Lake pro VS Code podporuje hello následující funkce:

-   Automatické dokončování IntelliSense: návrhy zobrazí v automaticky otevíraná okna kolem položky, jako jsou klíčová slova, metod a proměnné. Různé ikony představují různé typy objektů hello:

    - Datový typ Scala
    - Komplexní datový typ
    - Předdefinované UDT
    - Třídy a .NET collection
    - Výrazy jazyka C#
    - Předdefinované C# UDF, UDO a UDAAGs 
    - Funkce U-SQL
    - U-SQL oddílová funkce
 
    ![Typy objektů nástroje data Lake pro Visual Studio kódu IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   IntelliSense automatického dokončování v Data Lake Analytics metadata: nástrojů Data Lake stáhne hello Data Lake Analytics informace metadat místně. Funkce IntelliSense Hello automaticky naplní objektů, včetně hello databáze, schéma, tabulka, zobrazení, funkce vracející tabulku, postupy a sestavení C#, z metadat hello Data Lake Analytics.
 
    ![Nástroje data Lake pro Visual Studio kódu IntelliSense metadat](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   Chyba značky IntelliSense: nástrojů Data Lake podtrhne hello úpravy chyby U-SQL a C#. 
-   Označuje syntaxe: nástrojů Data Lake pomocí různých barev toodifferentiate položky, jako jsou proměnné, klíčová slova, datový typ a funkce. 

    ![Nástroje data Lake pro Visual Studio Code syntaxe označuje](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Další kroky

- Místní spuštění U-SQL a místní ladění s kódem jazyka Visual Studio najdete v tématu [U-SQL místní spuštění a místní ladění s Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).
- Začínáme informace o Data Lake Analytics najdete v tématu [kurz: Začínáme s Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Informace o nástrojů Data Lake pro Visual Studio najdete v tématu [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).



