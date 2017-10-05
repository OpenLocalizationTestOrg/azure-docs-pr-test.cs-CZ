---
title: "Procházení a Správa prostředků úložiště pomocí Průzkumníka serveru | Microsoft Docs"
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
ms.openlocfilehash: 43ab501c69c0c1e3271dbfcf08e5342a3507ab82
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Procházení a Správa prostředků úložiště pomocí Průzkumníka serveru
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Přehled
Pokud jste nainstalovali nástroje Azure pro sadu Microsoft Visual Studio, můžete zobrazit blob, fronty a tabulky dat z účtů úložiště Azure. V uzlu úložiště Azure v Průzkumníku serveru zobrazuje data, která jsou v účtu emulátor místního úložiště a jiné účty úložiště Azure.

Chcete-li zobrazit v Průzkumníku serveru v sadě Visual Studio na řádku nabídek, zvolte **zobrazení**, **Průzkumníka serveru**. V uzlu úložiště zobrazuje všechny účty úložiště, které existují v rámci každé Azure předplatného nebo certifikát, který jste připojení k. Pokud váš účet úložiště se nezobrazí, můžete ho přidat podle těchto pokynů [dál v tomto tématu](#add-storage-accounts-by-using-server-explorer).

Počínaje Azure SDK 2.7, můžete také pomocí nové Průzkumníka cloudu k zobrazení a správě prostředků Azure. V tématu [Správa prostředků Azure pomocí Průzkumníku cloudu](vs-azure-tools-resources-managing-with-cloud-explorer.md) Další informace.

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Zobrazit a spravovat prostředky úložiště v sadě Visual Studio
V Průzkumníku serveru automaticky zobrazí seznam objektů BLOB, fronty a tabulky ve vašem účtu emulátoru úložiště. Účet emulátor úložiště je uveden v Průzkumníku serveru pod uzlem úložiště jako **vývoj** uzlu.

Chcete-li zobrazit zdroje účet emulátor úložiště, rozbalte **vývoj** uzlu. Pokud při rozšiřování nebyla spuštěna emulátor úložiště **vývoj** uzlu, bude automaticky spuštěn. Může to trvat několik sekund. Můžete pokračovat v práci v jiných oblastech sady Visual Studio, při spuštění emulátor úložiště.

Chcete-li zobrazit prostředky v účtu úložiště, rozbalte uzel účet úložiště v Průzkumníku serveru. Zobrazí se následující dílčí uzly:

* Objekty blob
* Fronty
* Tabulky

## <a name="work-with-blob-resources"></a>Pracovat s prostředky objektů Blob
Objekty BLOB uzlu zobrazí seznam kontejnery pro vybraný účet úložiště. Kontejnery objektů BLOB obsahovat soubory objektů blob a tyto objekty BLOB můžete uspořádat do složky a podsložky. V tématu [postup používání úložiště Blob z .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md) Další informace.

### <a name="to-create-a-blob-container"></a>Chcete-li vytvořit kontejner objektů blob
1. Otevřete místní nabídku pro **objekty BLOB** uzel a potom zvolte **vytvořit kontejner objektů Blob**.
2. V **vytvořit kontejner objektů Blob** dialogovém okně zadejte název nového kontejneru.  
3. Stiskněte klávesu **ENTER** na klávesnici, nebo klikněte na tlačítko nebo klepněte na mimo pole název kontejneru objektů blob uložit.
   
   > [!NOTE]
   > Název kontejneru objektu blob musí začínat řetězcem číslici (0-9) nebo malé písmeno (a – ž).
   > 
   > 

### <a name="to-delete-a-blob-container"></a>Chcete-li odstranit kontejner objektů blob
* Otevřete místní nabídku pro kontejner objektů blob, které chcete odebrat a pak zvolte **odstranit**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Chcete-li zobrazit seznam položek obsažených v kontejneru objektů blob
* Otevřete místní nabídku pro název kontejneru objektů blob v seznamu a potom zvolte **otevřete**.
  
    Při zobrazení obsahu kontejneru objektů blob, se zobrazí na kartě známé jako zobrazení kontejneru objektů blob.
  
    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)
  
    Pomocí tlačítek v pravém horním rohu zobrazení kontejneru objektů blob můžete provádět následující operace na objekty BLOB:
  
  * Zadejte hodnotu filtru a použijte ho
  * Aktualizujte seznam objektů BLOB v kontejneru
  * Nahrání souboru
  * Odstranění objektu blob
    
    > [!NOTE]
    > Odstranění souboru z kontejneru objektů blob nedojde k odstranění podkladový soubor; jej pouze odeberete z kontejneru objektů blob.
    > 
    > 
  * Otevřete objektu blob
  * Uložit do objektu blob do místního počítače

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Chcete-li vytvořit složku nebo podsložku v kontejneru objektů blob
1. Vyberte kontejner objektů blob v Průzkumníku cloudu. V okně kontejner zvolte **nahrát objekt Blob** tlačítko.
   
    ![Nahrání souboru do složky, objektů blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)
2. V **nahrát nový soubor** dialogovém okně vyberte **Procházet** tlačítko zadat soubor, který chcete odeslat a pak zadejte název složky v **složky (volitelné)** pole.
   
    Pomocí stejného postupu můžete přidat podsložky ve složkách kontejneru. Pokud nezadáte název složky, soubor se nahraje do nejvyšší úrovně kontejneru objektů blob. Soubor se zobrazí ve složce zadané v kontejneru.
   
    ![Složky přidat do kontejneru objektů blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)
3. Poklikejte na složku nebo stiskněte klávesu ENTER a zobrazit obsah složky. Pokud jste ve složce kontejneru, můžete přejít zpět na kořenovém kontejneru výběrem **otevřete nadřazený adresář** (šipka) nahoru tlačítko.

### <a name="to-delete-a-container-folder"></a>Odstranění složky kontejneru
* Odstranit všechny soubory ve složce
  
  > [!NOTE]
  > Protože složky v kontejnerech objektů blob jsou virtuální složky, nelze vytvořit prázdnou složku ani odstranit složku k odstranění jeho obsah souboru. Je třeba odstranit celý obsah složky se odstranit složku.
  > 
  > 

### <a name="to-filter-blobs-in-a-container"></a>Chcete-li filtrovat objekty BLOB v kontejneru
Můžete filtrovat objekty BLOB, které se zobrazují tak, že zadáte předponu běžné.

Například pokud zadáte předponu `hello` v textu filtru pole a zvolte **Execute** (**!**) tlačítko se zobrazí pouze objekty BLOB, které začínají "hello".

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

> [!NOTE]
> Pole filtru je malá a velká písmena a nepodporuje filtrování se zástupnými znaky. Objekty BLOB můžete filtrovat pouze podle předpony. Předpona, která může zahrnovat oddělovač, pokud používáte oddělovač pro uspořádání objektů BLOB v hierarchii virtuální. Například filtrování na předponě HelloFabric / vrátí všechny objekty BLOB počínaje tento řetězec.
> 
> 

### <a name="to-download-blob-data"></a>Chcete-li stáhnout data objektů blob
* V **Průzkumník cloudu**, otevřete místní nabídku pro jeden nebo více objektů BLOB a potom zvolte **otevřete**, nebo vyberte název objektu blob a potom vyberte **otevřete** tlačítko nebo dvakrát kliknout na název objektu blob.
  
    Zobrazí se průběh stahování objektů blob v **protokol činnosti Azure** okno.
  
    Objekt blob se otevře v editoru výchozí pro tento typ souboru. Pokud operační systém rozpozná typ souboru, soubor se otevře v lokálně nainstalované aplikace; jinak budete vyzváni k výběru aplikace, která je vhodná pro typ souboru objektu blob. Místní soubor, který se vytvoří, když si stáhnete objekt blob je určen jen pro čtení.
  
    Data objektu BLOB je v místní mezipaměti a kontrolovat čas poslední změny objektu blob ve službě Blob. Pokud od poslední stažení byl aktualizován objekt blob, budou staženy znovu; Objekt blob, jinak budou načteny z místního disku. Ve výchozím nastavení je stáhne objekt blob do dočasného adresáře. Pokud chcete stáhnout objekty BLOB do určitého adresáře, otevřete místní nabídku pro názvy vybraných objektů blob a zvolte **uložit jako**. Při ukládání objektu blob tímto způsobem, není otevřený soubor blob a místního souboru je vytvořena s atributy pro čtení a zápis.

### <a name="to-upload-blobs"></a>Nahrát objektů BLOB
* Vyberte **nahrát objekt Blob** když je otevřen pro zobrazení v zobrazení kontejneru objektů blob kontejneru.
  
    Můžete vybrat jeden nebo více souborů pro nahrání a můžete nahrát souborů všech typů. **Protokol činnosti Azure** zobrazující průběh nahrávání. Další informace o tom, jak pracovat s daty objektu blob najdete v tématu [jak používat službu Azure Blob Storage v rozhraní .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Pokud chcete zobrazit protokoly přenést na objekty BLOB
* Pokud používáte Azure Diagnostics protokolování dat z aplikace Azure a protokoly přenesly na váš účet úložiště, uvidíte kontejnery, které byly vytvořeny v Azure pro tyto protokoly. Tyto protokoly zobrazení v Průzkumníku serveru je snadný způsob, jak identifikovat problémy s aplikací, zvlášť pokud je nasazený do Azure. Další informace o Azure Diagnostics najdete v tématu [shromažďovat protokolování dat pomocí Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Získat adresu URL pro objekt blob
* Otevřete nabídku zástupce objektu blob a potom zvolte **kopie URL**.

### <a name="to-edit-a-blob"></a>Chcete-li upravit objekt blob
* Vyberte objekt blob a potom zvolte **otevřete Blob** tlačítko.
  
    Soubor se stáhne do dočasného umístění a otevřít v místním počítači. Po provedení změn musíte znovu nahrát objekt blob.

## <a name="work-with-queue-resources"></a>Pracovat s prostředky fronty
Fronty služby úložiště, které jsou hostované v účtu úložiště Azure a můžete je povolit vaše cloudové služby rolí pro komunikaci mezi sebou a s jinými službami zobrazí zpráva o předání mechanismus. Prostřednictvím cloudové služby a přes webové služby pro externí klienty můžete programově přistupovat ke frontě. Přímo pomocí Průzkumníka serveru v sadě Visual Studio můžete také přistupovat ke frontě.

Při vývoji Cloudová služba, která používá fronty, můžete chtít použít Visual Studio k vytvoření fronty a pracovat s nimi interaktivně, když jste sami vyvinuli a otestujte svůj kód.

V Průzkumníku serveru můžete zobrazit fronty v účtu úložiště, vytvoření a odstranění front, otevření fronty k zobrazení jeho zpráv a taky přidat zprávy do fronty. Při otevření fronty pro zobrazení, můžete zobrazit jednotlivé zprávy a můžete provádět následující akce pro frontu pomocí tlačítka v levém horním rohu:

* Aktualizujte zobrazení fronty
* Přidat zprávu do fronty
* Dequeue – nejhornější zpráv
* Vymazání celého fronty

Následující obrázek znázorňuje fronty, který obsahuje dvě zprávy.

![Zobrazení fronty](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Další informace o úložišti služeb fronty naleznete v tématu [postupy: používání služby úložiště fronty](http://go.microsoft.com/fwlink/?LinkID=264702). Informace o webové službě úložiště služeb fronty naleznete v tématu [koncepty služby front](http://go.microsoft.com/fwlink/?LinkId=264788). Informace o tom, jak odesílat zprávy do fronty služby úložiště pomocí sady Visual Studio najdete v tématu [odeslání zprávy do fronty služby úložiště](https://msdn.microsoft.com/library/azure/jj649344.aspx).

> [!NOTE]
> Fronty úložiště služby se liší od fronty služby service bus. Další informace o fronty služby service bus najdete v části fronty služby Service Bus, témata a odběry.
> 
> 

## <a name="work-with-table-resources"></a>Pracovat s prostředky tabulky
Služba Azure Table Storage ukládá velké objemy strukturovaných dat. Služba je úložištěm dat typu NoSQL, které přijímá ověřená volání z cloudu Azure i z prostředí mimo něj. Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.

### <a name="to-create-a-table"></a>K vytvoření tabulky
1. V Průzkumníku cloudu, vyberte **tabulky** uzlu úložiště účet a potom vyberte **Create Table**.
2. V **Create Table** dialogovém okně zadejte název tabulky.

### <a name="to-view-table-data"></a>Chcete-li zobrazit data tabulky
1. V Průzkumníku cloudu, otevřete **Azure** uzel a potom otevřete **úložiště** uzlu.
2. Otevřete uzlu účet úložiště, který se zajímá a pak otevřete **tabulky** uzlu zobrazíte seznam tabulek pro účet úložiště.
3. Otevřete místní nabídku pro tabulku a zvolte **zobrazení tabulky**.
   
    ![Tabulky Azure v Průzkumníku řešení](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

Tabulka je seřazená podle entity (zobrazené v řádcích) a vlastnosti (zobrazené ve sloupcích). Například následující obrázek znázorňuje entit, které jsou uvedené v **návrháře tabulky**:

### <a name="to-edit-table-data"></a>Chcete-li upravit data tabulky
1. V **návrháře tabulky**, otevřete místní nabídku pro entitu (jeden řádek) nebo vlastnost (jedné buňky) a potom zvolte **upravit**.
   
    ![Přidat nebo upravit entitu tabulky](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)
   
    Entity v jediné tabulce nejsou potřeba mít stejnou sadu vlastností (sloupce). Mějte na paměti následující omezení pro prohlížení a úpravy dat v tabulce.
   
   * Nelze zobrazit nebo upravit binární data (byte[]) typu, ale můžete ho uložit v tabulce.
   * Nelze upravit **PartitionKey** nebo **RowKey** hodnoty, protože úložiště table v Azure nepodporuje tuto operaci.
   * Nelze vytvořit vlastnost s názvem časového razítka, Azure Storage services použijte vlastnost s tímto názvem.
   * Pokud zadáte hodnotu DateTime, musí odpovídat formátu, který je vhodný pro místní a jazykové nastavení počítače (například MM/DD/RRRR HH: mm: [AM | PM] pro USA Angličtina).

### <a name="to-add-entities"></a>Přidání entity
1. V **návrháře tabulky**, vyberte **Přidat entitu** tlačítko.
   
    ![Přidání Entity](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)
2. V **Přidat entitu** dialogovém okně zadejte hodnoty **PartitionKey** a **RowKey** vlastnosti.
   
    ![Entity dialogové okno Přidat](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)
   
    Zadejte hodnoty pečlivě, protože po zavřete dialogové okno, pokud odstranit entitu a znovu přidat, nelze je změnit.

### <a name="to-filter-entities"></a>Chcete-li filtrovat entity
Můžete přizpůsobit sadu entit, které se zobrazují v tabulce, pokud používáte Tvůrce dotazů.

1. Chcete-li otevřít Tvůrce dotazů, otevřete tabulku pro zobrazení.
2. Zvolte tlačítko Tvůrce dotazů na panelu nástrojů zobrazení tabulky.
   
    **Tvůrce dotazů** zobrazí se dialogové okno. Následující obrázek znázorňuje dotaz, který sestavuje v Tvůrce dotazů.
   
    ![Tvůrce dotazů](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
3. Po dokončení sestavení dotazu, zavřete dialogové okno. Výsledný formulář text dotazu se zobrazí v textovém poli jako filtr součásti WCF Data Services.
4. Pokud chcete spustit dotaz, zvolte ikonu zeleným trojúhelníkem.
   
    Můžete také filtrovat data entity, který se zobrazí v **návrháře tabulky** Pokud zadejte řetězec filtru datových služeb WCF přímo do pole filtru. Tento druh řetězec je podobná SQL klauzule WHERE, ale je odeslána na server jako požadavek HTTP. Informace o tom, jak vytvořit filtr řetězců najdete v tématu [vytváření řetězců filtru pro návrháře tabulky](https://msdn.microsoft.com/library/azure/ff683669.aspx).
   
    Následující obrázek znázorňuje příklad řetězec platný filtru:
   
    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Aktualizovat data úložiště
Když v Průzkumníku serveru se připojí k nebo získá data z účtu úložiště, to může trvat až několik minut na dokončení operace. Pokud se nemůže připojit, může operaci vypršení časového limitu. Když jsou načtena data, můžete pokračovat v práci v dalších částí sady Visual Studio. Chcete-li zrušit operaci, pokud to trvá příliš dlouho, zvolte **zrušit aktualizaci** tlačítka na panelu nástrojů Průzkumníka serveru.

#### <a name="to-refresh-blob-container-data"></a>Chcete-li aktualizovat data kontejneru objektů blob
* Vyberte **objekty BLOB** uzlu pod **úložiště** a zvolte **aktualizovat** tlačítka na panelu nástrojů Průzkumníka serveru.
* Chcete-li aktualizovat seznam objektů BLOB, který se zobrazí, zvolte **Execute** tlačítko.

#### <a name="to-refresh-table-data"></a>Aktualizace dat v tabulce
* Vyberte **tabulky** uzlu pod **úložiště** a zvolte **aktualizovat** tlačítko.
* Chcete-li aktualizovat seznam sad entit, které se zobrazí v **návrháře tabulky**, vyberte **Execute** tlačítko **návrháře tabulky**.

#### <a name="to-refresh-queue-data"></a>Aktualizovat data fronty
* Vyberte **fronty** uzel a potom zvolte **aktualizovat** tlačítko.

#### <a name="to-refresh-all-items-in-a-storage-account"></a>Chcete-li aktualizovat všechny položky v účtu úložiště
* Zvolte název účtu a potom **aktualizovat** tlačítka na panelu nástrojů pro Průzkumníka serveru.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Přidejte účty úložiště pomocí Průzkumníka serveru
Existují dva způsoby, jak přidat účty úložiště pomocí Průzkumníka serveru. Ve vašem předplatném Azure můžete vytvořit nový účet úložiště, nebo můžete připojit existující účet úložiště.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Chcete-li vytvořit nový účet úložiště pomocí Průzkumníka serveru
1. V Průzkumníku serveru otevřete místní nabídku pro uzel úložiště a pak zvolte Vytvořit účet úložiště.
   
    ![Vytvořit nový účet úložiště Azure](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)
2. Vyberte nebo zadejte následující informace pro nový účet úložiště **vytvořit účet úložiště** dialogové okno.
   
   * Předplatné Azure, ke které chcete přidat účet úložiště.
   * Název, který chcete použít pro nový účet úložiště.
   * Oblast nebo skupinu vztahů (například západní USA nebo jihovýchodní Asie).
   * Typ replikace, kterou chcete použít pro účet úložiště, jako je například geograficky redundantní.
3. Zvolte **Vytvořit**.
   
    Nový účet úložiště zobrazí v **úložiště** seznamu v Průzkumníku řešení.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Připojit stávající účet úložiště pomocí Průzkumníka serveru
1. V Průzkumníku serveru otevřete místní nabídku pro uzel úložiště Azure a pak zvolte **připojit externí úložiště**.
   
    ![Přidání stávající účet úložiště](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
2. Vyberte nebo zadejte následující informace pro nový účet úložiště **vytvořit účet úložiště** dialogové okno.
   
   * Název existující účet úložiště, který chcete připojit. Můžete zadat název nebo vyberte ze seznamu.
   * Klíč pro vybraný účet úložiště. Tato hodnota se většinou poskytuje pro vás, když vyberete účet úložiště. Pokud chcete Visual Studio pamatovat klíč účtu úložiště, vyberte pole klíče účtu zapamatovat.
   * Protokol pro připojení k účtu úložiště, jako je například HTTP, HTTPS nebo vlastní koncový bod. V tématu [postup nakonfigurování připojovacích řetězců](https://msdn.microsoft.com/library/azure/ee758697.aspx) Další informace o vlastní koncové body.

### <a name="to-view-the-secondary-endpoints"></a>Chcete-li zobrazit sekundární koncové body
* Pokud jste vytvořili účet úložiště pomocí **přístup pro čtení geograficky redundantní** možnost replikace, můžete zobrazit jeho sekundární koncové body. Otevřete místní nabídku pro název účtu a potom zvolte **vlastnosti**.
  
    ![Koncové body sekundární úložiště](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Chcete odstranit účet úložiště z Průzkumníka serveru
* V Průzkumníku serveru otevřete místní nabídku pro název účtu a potom zvolte **odstranit**. Pokud odstraníte účet úložiště, odeberou se také žádné uložené informace o klíči pro tohoto účtu.
  
  > [!NOTE]
  > Pokud odstraníte účet úložiště z Průzkumníka serveru, nemá vliv účtu úložiště nebo všechna data, která obsahuje; Odstraní pouze odkaz z Průzkumníka serveru. Pokud chcete trvale odstranit účet úložiště, použijte [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
  > 
  > 

## <a name="next-steps"></a>Další kroky
Další informace o použití služby Azure storage najdete v tématu [přístup ke službám úložiště Azure](https://msdn.microsoft.com/library/azure/ee405490.aspx).

