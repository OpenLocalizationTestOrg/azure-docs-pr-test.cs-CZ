---
title: "aaaHow toomonitor cloudové služby | Microsoft Docs"
description: "Zjistěte, jak toomonitor cloudových služeb pomocí hello portál Azure classic."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 5c48d2fb-b8ea-420f-80df-7aebe2b66b1b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2015
ms.author: adegeo
ms.openlocfilehash: ee98c56e0b98b85d75a5c1d796800069c4f06d20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-cloud-services"></a>Jak tooMonitor cloudových služeb
[!INCLUDE [disclaimer](../../includes/disclaimer.md)]

Můžete sledovat `key` metrika výkonu pro vaše cloudové služby v hello portál Azure classic. Můžete nastavit hello monitorování toominimal úrovně a podrobné pro každou roli služby a můžete přizpůsobit monitorování zobrazí hello. Podrobné monitorování data se ukládají v účtu úložiště, které je přístupné mimo portál hello. 

Zobrazení monitorování v hello portál Azure classic jsou vysoce konfigurovatelné. Můžete vybrat metriky hello chcete toomonitor v seznamu hello metriky na hello **monitorování** stránky a vy můžete zvolit které tooplot metriky v grafech metriky na hello **monitorování** stránku a hello řídicího panelu. 

## <a name="concepts"></a>Koncepty
Ve výchozím nastavení se minimální monitorování poskytuje pro novou cloudovou službu pomocí čítače výkonu získané z hello hostitelský operační systém pro instance rolí hello (virtuální počítače). minimální metriky Hello jsou omezené tooCPU procento, Data v, Data se, propustnost čtení disku a zápis propustnost disku. Podle konfigurace, podrobného sledování, může přijímat další metriky na základě dat o výkonu v rámci hello virtuálních počítačů (instance rolí). Podrobné metriky Hello povolit blíže analyzovat problémy, ke kterým došlo během operací aplikace.

Ve výchozím nastavení se data čítače výkonu z instancí role vzorků a přenést z hello role instance v intervalech 3 minut. Pokud povolíte podrobné monitorování, hello hrubý výkon čítač agregovaná data pro každou instanci role a napříč instancemi role pro každou roli v intervalech 5 minut, 1 hodina a 12 hodin. Hello agregovaná data se vyprazdňují po 10 dní.

Po povolení podrobného monitorování hello agregovat data monitorování je ukládat do tabulek ve vašem účtu úložiště. tooenable podrobné sledování pro roli, je nutné nakonfigurovat diagnostiky připojovací řetězec, který se propojuje účet úložiště toohello. Jiným účtům úložiště můžete použít pro různé role.

Povolení podrobného monitorování zvyšuje náklady na úložiště související toodata úložiště, přenos dat a transakce úložiště. Minimální monitorování nevyžaduje žádný účet úložiště. Hello data pro hello metriky, které jsou zveřejněné na minimální úroveň monitorování hello nejsou uložené v účtu úložiště, i když nastavíte hello monitorování úrovně tooverbose.

## <a name="how-to-configure-monitoring-for-cloud-services"></a>Postupy: Konfigurace monitorování pro cloudové služby
Použijte následující postupy tooconfigure verbose nebo minimální monitorování v hello portál Azure classic hello. 

### <a name="before-you-begin"></a>Než začnete
* Vytvoření *classic* hello toostore účet úložiště dat monitorování. Jiným účtům úložiště můžete použít pro různé role. Další informace najdete v tématu [jak toocreate účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* Povolte Azure Diagnostics pro role cloudové služby. V tématu [konfigurace diagnostiky pro cloudové služby](cloud-services-dotnet-diagnostics.md).

Ujistěte se, zda je v konfiguraci Role hello hello diagnostiky připojovací řetězec. Nelze povolit, podrobného sledování, dokud povolíte Azure Diagnostics a zahrnují diagnostiky připojovací řetězec v konfiguraci Role hello.   

> [!NOTE]
> Projektech zacílených na Azure SDK 2.5 automaticky nezahrnuli hello diagnostiky připojovací řetězec v šabloně projektu hello. Pro tyto projekty, je třeba toomanually přidání hello diagnostiky připojovací řetězec toohello Role konfigurace.
> 
> 

**toomanually Přidání diagnostiky připojovací řetězec tooRole konfigurace**

1. Otevřete hello projekt cloudové služby v sadě Visual Studio
2. Dvakrát klikněte na hello **Role** tooopen hello Návrhář roli a vyberte hello **nastavení** karta
3. Vyhledejte nastavení s názvem **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Pokud toto nastavení není zadán, klikněte na hello **přidat nastavení** tooadd tlačítko ji toohello konfigurace a změňte hello typ pro hello nové nastavení příliš**ConnectionString**
5. Nastavit hodnotu hello hello řetězec připojení kliknutím na hello **...**  tlačítko. Tím se otevře dialogové okno umožní vám tooselect účet úložiště.
   
    ![Nastavení sady Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="toochange-hello-monitoring-level-tooverbose-or-minimal"></a>hello toochange monitorování úrovni tooverbose nebo minimální
1. V hello [portál Azure classic](https://manage.windowsazure.com/), otevřete hello **konfigurace** stránky pro nasazení služby hello cloudu.
2. V **úroveň**, klikněte na tlačítko **podrobné** nebo **minimální**. 
3. Klikněte na **Uložit**.

Po zapnutí podrobného monitorování, měli byste začít zobrazuje hello dat v hello portál Azure classic monitorování v rámci hello hodinu.

data čítače Hello hrubý výkon a agregovaná data monitorování jsou uloženy v účtu úložiště hello v tabulkách kvalifikovaný hello ID nasazení pro role hello. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Postupy: přijímat výstrahy metrik, které cloudové služby
Může přijímat výstrahy založené na cloudové služby monitorování metriky. Na hello **Management Services** stránku hello portál Azure classic, můžete vytvořit pravidlo tootrigger výstrahu při hello metrika zvolíte dosáhne hodnotu, která zadáte. Můžete také odeslat v případě, že hello výstrahy e-mailu toohave. Další informace najdete v tématu [postupy: příjem oznámení výstrah a spravovat pravidla výstrah v Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-toohello-metrics-table"></a>Postupy: Přidání tabulky metriky toohello metriky
1. V hello [portál Azure classic](http://manage.windowsazure.com/), otevřete hello **monitorování** stránku hello cloudové služby.
   
    Ve výchozím nastavení hello metriky tabulce podmnožinu hello dostupné metriky. Hello obrázku hello výchozí podrobné metriky pro cloudovou službu, která je omezená toohello čítač výkonu paměť\počet MB k dispozici, s daty agregován na úrovni role hello. Použití **přidat metriky** tooselect další agregaci a metriky úrovní role toomonitor v hello portál Azure classic.
   
    ![Podrobné zobrazení](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
2. Tabulka tooadd metriky toohello metriky:
   
   1. Klikněte na tlačítko **přidat metriky** tooopen **zvolte metriky**, vidíte níže.
      
       první dostupná metrika Hello je tooshow rozšířené možnosti, které jsou k dispozici. Pro jednotlivé metriky možnost top hello zobrazuje agregovaná data monitorování u všech rolí. Kromě toho můžete jednotlivých rolí toodisplay data.
      
       ![Přidat metriky](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)
   2. toodisplay tooselect metriky
      
      * Klikněte na tlačítko hello šipka dolů podle hello metriky tooexpand hello možnosti monitorování.
      * Vyberte hello políčko u každé monitorování možnost, kterou chcete toodisplay.
        
        Můžete zobrazit si too50 metriky v tabulce metriky hello.
        
        > [!TIP]
        > Podrobné monitorování hello metriky seznam může obsahovat desítek metriky. toodisplay scrollbar, najeďte myší hello pravé straně dialogového okna hello. toofilter hello seznamu, klikněte na ikonu hledání hello a zadejte text do pole hledání text hello, jak je uvedeno níže.
        > 
        > 
        
        ![Přidat metriky vyhledávání](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)
3. Po dokončení výběru metriky, klikněte na tlačítko OK (zaškrtnutí).
   
    Hello vybrané metriky se přidají toohello metriky tabulky, jak je uvedeno níže.
   
    ![monitorování metriky](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)
4. toodelete metrika z hello metriky tabulky, klikněte na metriky tooselect hello a pak klikněte na tlačítko **odstranit metrika**. (Jenom zobrazí **odstranit metrika** Pokud máte vybrané metriky.)

### <a name="tooadd-custom-metrics-toohello-metrics-table"></a>tooadd vlastní metriky toohello metriky tabulky
Hello **podrobné** monitorování úroveň obsahuje seznam výchozích metrik, které můžete monitorovat na portálu hello. Kromě toho toothese můžete sledovat všechny vlastní metriky nebo čítače výkonu, které jsou definované aplikace prostřednictvím portálu hello.

Hello následující kroky předpokládají, že jste zapnuli **podrobné** monitorování úroveň a nakonfigurovali vaší aplikace toocollect a přenos vlastní čítače výkonu. 

toodisplay hello vlastní čítače výkonu v hello portál, je nutné konfiguraci hello tooupdate v kontejneru ovládacího prvku wad:

1. Otevřete hello kontejneru ovládacího prvku wad blob ve vašem účtu úložiště diagnostiky. Můžete použít Visual Studio nebo jakékoli jiné toodo Průzkumníka úložiště to.
   
    ![Průzkumníka serveru Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)
2. Přejděte cesta blobu hello pomocí vzoru hello **DeploymentId/RoleName/RoleInstance** toofind hello konfigurace pro instanci role. 
   
    ![Storage Explorer sady Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Stáhnout hello konfigurační soubor pro instanci role a aktualizovat ji tooinclude všechny vlastní čítače výkonu. Například toomonitor *zápis disku bajtů/s* pro hello *jednotka C* přidejte následující hello pod **PerformanceCounters\Subscriptions** uzlu
   
    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Uložte změny hello a nahrávání hello konfigurační soubor back toohello hello stejné umístění přepsal existující soubor v objektu blob hello.
5. Přepnutí režimu tooVerbose v hello Azure konfigurace portálu classic. Pokud jste již v režimu s komentářem budete mít tooverbose tootoggle toominimal a zpět.
6. Čítač výkonu vlastní Hello bude nyní k dispozici v hello **přidat metriky** dialogové okno. 

## <a name="how-to-customize-hello-metrics-chart"></a>Postupy: přizpůsobení hello metriky grafu
1. V tabulce hello metriky vyberte si too6 tooplot metriky v grafu metriky hello. tooselect metriky, zaškrtněte políčko hello na jeho levé straně. tooremove metrika z hello metriky grafu, zrušte zaškrtnutí jejího políčka v tabulce metriky hello.
   
    Při výběru metriky v tabulce hello metriky, se přidají hello metriky toohello metriky grafu. Na úzké zobrazení **n Další** rozevírací seznam obsahuje metriky hlavičky, které nebudou vyhovovat hello zobrazení.
2. tooswitch mezi zobrazením relativní hodnoty (konečná hodnota jenom pro jednotlivé metriky) a absolutní hodnoty (osy Y zobrazit), vyberte relativní nebo absolutní horní hello hello grafu.
   
    ![Relativní nebo absolutní](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)
3. toochange hello čas rozsah hello metriky grafu se zobrazuje, vyberte 1 hod., 24 hodin nebo 7 dní v horní části hello hello grafu.
   
    ![Období zobrazení monitorování](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)
   
    V grafu metriky hello řídicí panel metoda hello obrátíte metrik, které se liší. Je k dispozici standardní sadu metriky a přidávání nebo odebírání výběrem metriky záhlaví hello metriky.

### <a name="toocustomize-hello-metrics-chart-on-hello-dashboard"></a>toocustomize hello metriky graf na řídicí panel hello
1. Otevřete řídicí panel hello hello cloudové služby.
2. Přidat nebo odebrat z grafu hello metriky:
   
   * tooplot nové hello metriky, vyberte zaškrtávací políčko pro metriku hello v hlavičkách hello grafu. Na úzké zobrazení, klikněte na tlačítko hello podle šipka dolů  ***n* ?? metriky** tooplot metriky hello záhlaví oblast grafu nelze zobrazit.
   * toodelete metrika, který se vykreslí v grafu hello, hello zrušte zaškrtnutí políčka podle jeho záhlaví.
   
3. Přepínání mezi **relativní** a **absolutní** zobrazí.
4. Vyberte 1 hod., 24 hodin nebo data toodisplay 7 dní.

## <a name="how-to-access-verbose-monitoring-data-outside-hello-azure-classic-portal"></a>Postupy: přístup k podrobné monitorování data mimo hello portál Azure classic
Podrobné sledování dat je ukládat do tabulek v účtech úložiště hello, které zadáte pro každou roli. Pro každé nasazení cloudové služby vytvoří se pro roli hello šesti tabulky. Dvě tabulky se vytváří pro každý (5 minut, 1 hodina a 12 hodin). Některé z těchto tabulek ukládá agregace na úrovni role; Hello jiných agregací tabulky úložiště pro instance rolí. 

názvy tabulek Hello mít hello následující formát:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Kde:

* *deploymentID* je hello identifikátor GUID přiřazený nasazení toohello cloudové služby
* *aggregation_interval* = 5 M, 1 H nebo 12 H
* agregace na úrovni role = R
* agregace pro instance rolí = RI

Například hello následující tabulky by uložit podrobné data monitorování agregován v intervalu 1 hodin:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for hello role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
