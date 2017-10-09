---
title: "aaaBrowsing a Správa prostředků úložiště pomocí Průzkumníka serveru | Microsoft Docs"
description: "Procházení a Správa prostředků úložiště pomocí Průzkumníka serveru"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 658dc064-4a4e-414b-ae5a-a977a34c930d
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/24/2017
ms.author: kraigb
ms.openlocfilehash: f5b456b812f2ad8103c50538d532a57397bcccbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Procházení a Správa prostředků úložiště pomocí Průzkumníka serveru
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Přehled
Pokud jste nainstalovali nástroje hello Azure pro sadu Microsoft Visual Studio, můžete zobrazit blob, fronty a tabulky dat z účtů úložiště Azure. uzel Hello Azure Storage v Průzkumníku serveru zobrazuje data, která jsou v účtu emulátor místního úložiště a jiné účty úložiště Azure.

Zvolte tooview Průzkumníka serveru v sadě Visual Studio na řádku nabídek hello **zobrazení**, **Průzkumníka serveru**. uzel úložiště Hello zobrazuje všechny účty úložiště hello, které existují v rámci každé Azure předplatného nebo certifikát, který jste připojení k. Pokud váš účet úložiště se nezobrazí, můžete ho přidat podle pokynů hello [dál v tomto tématu](#add-storage-accounts-by-using-server-explorer).

Počínaje Azure SDK 2.7, můžete také použít novou tooview Průzkumník cloudu hello a správě prostředků Azure. V tématu [Správa prostředků Azure pomocí Průzkumníku cloudu](vs-azure-tools-resources-managing-with-cloud-explorer.md) Další informace.

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Zobrazit a spravovat prostředky úložiště v sadě Visual Studio
V Průzkumníku serveru automaticky zobrazí seznam objektů BLOB, fronty a tabulky ve vašem účtu emulátoru úložiště. účet emulátor úložiště Hello je uveden v Průzkumníku serveru pod uzlem hello úložiště jako hello **vývoj** uzlu.

prostředky toosee hello emulátoru účtu úložiště, rozbalte položku hello **vývoj** uzlu. Pokud emulátor úložiště hello nebyla spuštěna při rozšiřování hello **vývoj** uzlu, bude automaticky spuštěn. Může to trvat několik sekund. Toowork můžete pokračovat v jiných oblastech sady Visual Studio, zatímco emulátor úložiště hello spustí.

tooview prostředky v účtu úložiště, rozbalte uzel účtu úložiště hello v Průzkumníku serveru. Zobrazí se následující dílčí uzly Hello:

* Objekty blob
* Fronty
* Tabulky

## <a name="work-with-blob-resources"></a>Pracovat s prostředky objektů Blob
uzel objekty BLOB Hello zobrazí seznam kontejnery pro hello vybraný účet úložiště. Kontejnery objektů BLOB obsahovat soubory objektů blob a tyto objekty BLOB můžete uspořádat do složky a podsložky. V tématu [jak toouse úložiště Blob z .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md) Další informace.

### <a name="toocreate-a-blob-container"></a>toocreate kontejner objektů blob
1. Otevřete hello místní nabídku pro hello **objekty BLOB** uzel a potom zvolte **vytvořit kontejner objektů Blob**.
2. V hello **vytvořit kontejner objektů Blob** dialogovém okně zadejte název hello hello nový kontejner.  
3. Stiskněte klávesu **ENTER** na klávesnici, nebo klikněte na tlačítko nebo klepněte na mimo hello název pole toosave hello kontejner objektů blob.
   
   > [!NOTE]
   > název kontejneru objektu blob Hello musí začínat řetězcem číslici (0-9) nebo malé písmeno (a – ž).
   > 
   > 

### <a name="toodelete-a-blob-container"></a>toodelete kontejner objektů blob
* Otevřete hello místní nabídku pro kontejner objektů blob hello chcete tooremove a potom zvolte **odstranit**.

### <a name="toodisplay-a-list-of-hello-items-contained-in-a-blob-container"></a>toodisplay seznam položek hello obsažené v kontejneru objektů blob
* V seznamu hello otevřete hello místní nabídku pro název kontejneru objektu blob a potom zvolte **otevřete**.
  
    Při zobrazení hello obsah kontejneru objektů blob, se zobrazí na kartě známé jako zobrazení kontejneru objektů blob hello.
  
    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)
  
    Můžete provádět následující operace na objekty BLOB pomocí tlačítka hello v pravém horním rohu hello zobrazení kontejneru objektů blob hello hello:
  
  * Zadejte hodnotu filtru a použijte ho
  * Obnovte hello seznam objektů BLOB v kontejneru hello
  * Nahrání souboru
  * Odstranění objektu blob
    
    > [!NOTE]
    > Odstranění souboru z kontejneru objektů blob nedojde k odstranění hello podkladový soubor; jej pouze odeberete z kontejneru objektů blob hello.
    > 
    > 
  * Otevřete objektu blob
  * Uložit místní počítač toohello objektů blob

### <a name="toocreate-a-folder-or-subfolder-in-a-blob-container"></a>toocreate složku nebo podsložku v kontejneru objektů blob
1. Vyberte kontejner objektů blob hello v Průzkumníku cloudu. V okně kontejner hello zvolte hello **nahrát objekt Blob** tlačítko.
   
    ![Nahrání souboru do složky, objektů blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)
2. V hello **nahrát nový soubor** dialogovém okně vyberte hello **Procházet** tlačítko toospecify hello souboru chcete tooupload a potom zadejte název složky v hello **složky (volitelné)** pole .
   
    Můžete přidat podsložky v kontejneru složky podle hello stejný postup. Pokud nezadáte název složky, hello soubor bude nahrán toohello nejvyšší úrovní hello kontejner objektů blob. soubor Hello se zobrazí v zadané složce hello v kontejneru hello.
   
    ![Přidá složka tooa kontejner objektů blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)
3. Dvakrát klikněte na složku hello nebo stiskněte klávesu ENTER toosee hello obsah složky hello. Pokud jste ve složce hello kontejneru, můžete přejít zpět toohello kořenovém kontejneru hello výběrem hello **otevřete nadřazený adresář** (šipka) nahoru tlačítko.

### <a name="toodelete-a-container-folder"></a>toodelete složku kontejneru
* Odstranit všechny hello soubory ve složce hello
  
  > [!NOTE]
  > Protože složky v kontejnerech objektů blob jsou virtuální složky, nelze vytvořit prázdnou složku, ani je možné odstranit složku toodelete jeho obsah souboru. Máte toodelete hello celý obsah hello toodelete složka.
  > 
  > 

### <a name="toofilter-blobs-in-a-container"></a>toofilter objekty BLOB v kontejneru
Můžete filtrovat hello objekty BLOB, které se zobrazují tak, že zadáte předponu běžné.

Například pokud zadáte předponu hello `hello` v hello filtru textového pole a zvolte hello **Execute** (**!**) tlačítko se zobrazí pouze objekty BLOB, které začínají "hello".

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

> [!NOTE]
> pole filtru Hello je malá a velká písmena a nepodporuje filtrování se zástupnými znaky. Objekty BLOB můžete filtrovat pouze podle předpony. Předpona Hello může zahrnovat oddělovač, pokud používáte objekty BLOB tooorganize oddělovač v hierarchii virtuální. Například filtrování hello předponu HelloFabric nebo vrátí všechny objekty BLOB počínaje tento řetězec.
> 
> 

### <a name="toodownload-blob-data"></a>toodownload data objektů blob
* V **Průzkumník cloudu**, otevřete hello místní nabídku pro jeden nebo více objektů BLOB a potom zvolte **otevřete**, nebo vyberte název objektu blob hello a potom vyberte hello **otevřete** tlačítko nebo dvakrát kliknout na Název objektu blob Hello.
  
    Hello průběh stahování objekt blob se zobrazí v hello **protokol činnosti Azure** okno.
  
    Hello blob se otevře v editoru hello výchozí pro tento typ souboru. Pokud typ souboru hello rozpoznává hello operační systém, hello soubor se otevře v lokálně nainstalované aplikace; jinak se zobrazí výzva toochoose aplikace, která je vhodná pro typ souboru hello objektu hello blob. Hello místního souboru, který se vytvoří, když si stáhnete objekt blob je označena jako jen pro čtení.
  
    Data objektu BLOB je v místní mezipaměti a kontrolovat hello čas poslední změny objektů blob v hello služby objektů Blob. Pokud od poslední stažení objektů blob hello aktualizován, budou staženy znovu; hello blob, jinak budou načteny z místního disku hello. Ve výchozím nastavení je objekt blob stažené tooa dočasný adresář. toodownload objekty BLOB tooa konkrétního adresáře, otevřete hello místní nabídku pro hello vybrané názvy objektů blob a zvolte **uložit jako**. Při ukládání objektu blob tímto způsobem, není otevřený soubor blob hello a hello místního souboru je vytvořena s atributy pro čtení a zápis.

### <a name="tooupload-blobs"></a>objekty BLOB tooupload
* Zvolte hello **nahrát objekt Blob** když je otevřen pro zobrazení v zobrazení kontejneru objektů blob hello hello kontejneru.
  
    Můžete si vybrat jednu nebo více souborů tooupload a mohou odesílat soubory libovolného typu. Hello **protokol činnosti Azure** ukazuje hello průběh nahrávání hello. Další informace o tom, najdete v části toowork s data objektů blob, [jak toouse hello služby úložiště objektů Blob Azure v rozhraní .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="tooview-logs-transferred-tooblobs"></a>protokoly tooview přenést tooblobs
* Pokud používáte Azure Diagnostics toolog data z aplikace Azure a jste přenesli účet úložiště tooyour protokoly, uvidíte kontejnery, které byly vytvořeny v Azure pro tyto protokoly. Zobrazení tyto protokoly v Průzkumníku serveru se snadný způsob tooidentify problémy s aplikací, zejména v případě, že byla nasazená tooAzure. Další informace o Azure Diagnostics najdete v tématu [shromažďovat protokolování dat pomocí Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="tooget-hello-url-for-a-blob"></a>Adresa URL hello tooget pro objekt blob
* Otevřete nabídku zástupce hello blob a potom zvolte **kopie URL**.

### <a name="tooedit-a-blob"></a>tooedit objektu blob
* Vyberte hello blob a potom zvolte hello **otevřete Blob** tlačítko.
  
    Hello soubor stažený tooa dočasné umístění a otevřít na místním počítači hello. Objekt blob hello musíte nahrát znovu po provedení změn.

## <a name="work-with-queue-resources"></a>Pracovat s prostředky fronty
Fronty služby úložiště, které jsou hostované v účtu úložiště Azure a můžete je použít tooallow vaše cloudové služby role toocommunicate mezi sebou a s jinými službami zobrazí zpráva o předání mechanismus. Hello fronty můžete přistupovat prostřednictvím kódu programu prostřednictvím cloudové služby a přes webové služby pro externí klienty. Můžete také přistupovat hello frontě přímo pomocí Průzkumníka serveru v sadě Visual Studio.

Při vývoji Cloudová služba, která používá fronty, můžete má toouse Visual Studio toocreate front a pracovat s nimi interaktivně, když jste sami vyvinuli a otestujte svůj kód.

V Průzkumníku serveru můžete zobrazit hello fronty v účtu úložiště, vytvoření a odstranění front, otevřete tooview fronty jeho zprávy a přidejte tooa fronty zpráv. Při otevření fronty pro zobrazení hello jednotlivé zprávy lze zobrazit a můžete provádět následující akce ve frontě hello pomocí tlačítka hello v levém horním rohu hello hello:

* Aktualizujte zobrazení hello hello fronty
* Přidat toohello fronty zpráv
* Dequeue – zpráva nejhornější hello
* Vymazat hello celý fronty

Hello následující obrázek znázorňuje fronty, který obsahuje dvě zprávy.

![Zobrazení fronty](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Další informace o úložišti služeb fronty naleznete v tématu [postupy: použití hello fronty úložiště služby](http://go.microsoft.com/fwlink/?LinkID=264702). Informace o hello webové služby úložiště služeb fronty naleznete v tématu [koncepty služby front](http://go.microsoft.com/fwlink/?LinkId=264788). Informace o tom, jak toosend zprávy tooa úložiště služby fronty pomocí sady Visual Studio najdete v tématu [tooa odesílat zprávy fronty úložiště služby](https://msdn.microsoft.com/library/azure/jj649344.aspx).

> [!NOTE]
> Fronty úložiště služby se liší od fronty služby service bus. Další informace o fronty služby service bus najdete v části fronty služby Service Bus, témata a odběry.
> 
> 

## <a name="work-with-table-resources"></a>Pracovat s prostředky tabulky
Hello služby Azure Table storage ukládá velkých objemů strukturovaná data. Hello služby je úložištěm dat typu NoSQL, které přijímá ověřená volání z uvnitř i vně hello cloudu Azure. Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.

### <a name="toocreate-a-table"></a>toocreate tabulku
1. V Průzkumníku cloudu vyberte hello **tabulky** uzlu úložiště hello účet a potom vyberte **Create Table**.
2. V hello **Create Table** dialogovém okně zadejte název tabulky hello.

### <a name="tooview-table-data"></a>data tabulky tooview
1. V Průzkumníku cloudu, otevřete hello **Azure** uzel a pak otevřete hello **úložiště** uzlu.
2. Uzel účtu úložiště otevřete hello zájem a pak otevřete hello **tabulky** uzlu toosee seznam tabulek pro účet úložiště hello.
3. Otevřete hello místní nabídky pro tabulku a zvolte **zobrazení tabulky**.
   
    ![Tabulky Azure v Průzkumníku řešení](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

Tabulka Hello je seřazená podle entity (zobrazené v řádcích) a vlastnosti (zobrazené ve sloupcích). Například hello následující obrázek ukazuje entit, které jsou uvedené v hello **návrháře tabulky**:

### <a name="tooedit-table-data"></a>data tabulky tooedit
1. V hello **návrháře tabulky**, otevřete hello místní nabídku pro entitu (jeden řádek) nebo vlastnost (jedné buňky) a potom zvolte **upravit**.
   
    ![Přidat nebo upravit entitu tabulky](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)
   
    Entity v jediné tabulce nejsou požadované toohave hello stejná sada vlastností (sloupce). Mějte na paměti hello následující omezení na prohlížení a úpravy dat v tabulce.
   
   * Nelze zobrazit nebo upravit binární data (byte[]) typu, ale můžete ho uložit v tabulce.
   * Nelze upravit hello **PartitionKey** nebo **RowKey** hodnoty, protože úložiště table v Azure nepodporuje tuto operaci.
   * Nelze vytvořit vlastnost s názvem časového razítka, Azure Storage services použijte vlastnost s tímto názvem.
   * Pokud zadáte hodnotu DateTime, musí odpovídat formátu, který je vhodné toohello místní a jazykové nastavení počítače (například MM/DD/RRRR HH: mm: [AM | PM] pro USA Angličtina).

### <a name="tooadd-entities"></a>tooadd entity
1. V hello **návrháře tabulky**, zvolte hello **Přidat entitu** tlačítko.
   
    ![Přidání Entity](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)
2. V hello **Přidat entitu** dialogovém okně zadejte hodnoty hello hello **PartitionKey** a **RowKey** vlastnosti.
   
    ![Entity dialogové okno Přidat](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)
   
    Zadejte hodnoty hello pečlivě, protože po zavřete dialogové okno hello Pokud hello entitu odstranit a znovu přidat, nelze je změnit.

### <a name="toofilter-entities"></a>toofilter entity
Můžete přizpůsobit hello sady entit, které se zobrazují v tabulce, pokud používáte Tvůrce dotazů hello.

1. Tvůrce dotazů hello tooopen, otevřít tabulku pro zobrazení.
2. Zvolte tlačítko hello Tvůrce dotazů na panelu nástrojů zobrazení tabulky hello.
   
    Hello **Tvůrce dotazů** zobrazí se dialogové okno. Hello následující obrázek ukazuje dotaz, který sestavuje v Tvůrce dotazů hello.
   
    ![Tvůrce dotazů](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
3. Po dokončení vytváření hello dotazu, zavřete dialogové okno hello. Výsledný formulář textu Hello hello dotazu se zobrazí v textovém poli jako filtr součásti WCF Data Services.
4. toorun hello dotazu, zvolte ikonu zeleným trojúhelníkem hello.
   
    Můžete také filtrovat data entity, který se zobrazí v hello **návrháře tabulky** Pokud zadejte řetězec filtru datových služeb WCF přímo v poli filtru hello. Tento druh řetězec je podobné tooa SQL klauzule WHERE, ale je odeslána toohello serveru jako požadavek HTTP. Informace o tom, jak filtrovat tooconstruct řetězce najdete v tématu [vytváření řetězců filtru pro hello návrháře tabulky](https://msdn.microsoft.com/library/azure/ff683669.aspx).
   
    Hello následující obrázek ukazuje příklad řetězec platný filtru:
   
    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Aktualizovat data úložiště
Když v Průzkumníku serveru připojí tooor získá data z účtu úložiště, může trvat až minutu tooa pro operaci toocomplete hello. Pokud se nemůže připojit, může operace hello vypršení časového limitu. Když jsou načtena data, můžete pokračovat v toowork v dalších částí sady Visual Studio. operace toocancel hello, pokud to trvá příliš dlouho, zvolte hello **zrušit aktualizaci** tlačítka na panelu nástrojů hello Průzkumníka serveru.

#### <a name="toorefresh-blob-container-data"></a>toorefresh data kontejneru objektů blob
* Vyberte hello **objekty BLOB** uzlu pod **úložiště** a zvolte hello **aktualizovat** tlačítka na panelu nástrojů hello Průzkumníka serveru.
* toorefresh hello seznamu objektů BLOB, který se zobrazí, zvolte hello **Execute** tlačítko.

#### <a name="toorefresh-table-data"></a>data tabulky toorefresh
* Vyberte hello **tabulky** uzlu pod **úložiště** a zvolte hello **aktualizovat** tlačítko.
* toorefresh seznamu hello entit, který se zobrazí v hello **návrháře tabulky**, zvolte hello **Execute** na hello tlačítko **návrháře tabulky**.

#### <a name="toorefresh-queue-data"></a>toorefresh fronty dat
* Vyberte hello **fronty** uzel a potom zvolte hello **aktualizovat** tlačítko.

#### <a name="toorefresh-all-items-in-a-storage-account"></a>toorefresh všechny položky v účtu úložiště
* Zvolte název účtu hello a pak hello **aktualizovat** tlačítka na panelu nástrojů hello pro Průzkumníka serveru.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Přidejte účty úložiště pomocí Průzkumníka serveru
Existují dva způsoby účty úložiště tooadd pomocí Průzkumníka serveru. Ve vašem předplatném Azure můžete vytvořit nový účet úložiště, nebo můžete připojit existující účet úložiště.

#### <a name="toocreate-a-new-storage-account-by-using-server-explorer"></a>toocreate nový účet úložiště pomocí Průzkumníka serveru
1. V Průzkumníku serveru otevřete hello místní nabídku pro uzel hello úložiště a pak zvolte Vytvořit účet úložiště.
   
    ![Vytvořit nový účet úložiště Azure](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)
2. Vyberte nebo zadejte následující informace pro nový účet úložiště hello v hello hello **vytvořit účet úložiště** dialogové okno.
   
   * Chcete účet úložiště hello tooadd toowhich předplatného Azure Hello.
   * Chcete použít pro nový účet úložiště hello toouse název Hello.
   * Hello oblast nebo skupinu vztahů (například západní USA nebo jihovýchodní Asie).
   * Hello typ replikace pro účet úložiště hello, jako je například geograficky redundantní chcete toouse.
3. Zvolte **Vytvořit**.
   
    Hello nový účet úložiště se zobrazí v hello **úložiště** seznamu v Průzkumníku řešení.

#### <a name="tooattach-an-existing-storage-account-by-using-server-explorer"></a>tooattach stávající účet úložiště pomocí Průzkumníka serveru
1. V Průzkumníku serveru otevřete hello místní nabídky pro uzel hello úložiště Azure a pak zvolte **připojit externí úložiště**.
   
    ![Přidání stávající účet úložiště](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
2. Vyberte nebo zadejte následující informace pro nový účet úložiště hello v hello hello **vytvořit účet úložiště** dialogové okno.
   
   * Název Hello hello stávající účet úložiště, že který má tooattach. Můžete zadat název nebo vyberte ze seznamu hello.
   * Hello klíč pro hello vybraný účet úložiště. Tato hodnota se většinou poskytuje pro vás, když vyberete účet úložiště. Pokud chcete klíč účtu úložiště hello tooremember Visual Studio, vyberte hello zapamatovat účet klíčové pole.
   * Hello protokol toouse tooconnect toohello účet úložiště, jako je například HTTP, HTTPS nebo vlastní koncový bod. V tématu [jak řetězce připojení tooConfigure](https://msdn.microsoft.com/library/azure/ee758697.aspx) Další informace o vlastní koncové body.

### <a name="tooview-hello-secondary-endpoints"></a>sekundární koncové body tooview hello
* Pokud jste vytvořili účet úložiště pomocí hello **přístup pro čtení geograficky redundantní** možnost replikace, můžete zobrazit jeho sekundární koncové body. Otevřete hello místní nabídku pro hello název účtu a potom zvolte **vlastnosti**.
  
    ![Koncové body sekundární úložiště](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="tooremove-a-storage-account-from-server-explorer"></a>tooremove účet úložiště z Průzkumníka serveru
* V Průzkumníku serveru otevřete hello místní nabídku pro hello název účtu a potom zvolte **odstranit**. Pokud odstraníte účet úložiště, odeberou se také žádné uložené informace o klíči pro tohoto účtu.
  
  > [!NOTE]
  > Pokud odstraníte účet úložiště z Průzkumníka serveru, nemá vliv účtu úložiště nebo všechna data, která obsahuje; Odstraní pouze hello odkaz z Průzkumníka serveru. toopermanently odstranit účet úložiště, použijte hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
  > 
  > 

## <a name="next-steps"></a>Další kroky
toolearn informace o používání služby Azure storage, naleznete v tématu [přístup ke službám úložiště Azure hello](https://msdn.microsoft.com/library/azure/ee405490.aspx).

