---
title: "aaaMicrosoft Azure StorSimple Data Manager uživatelského rozhraní | Microsoft Docs"
description: "Popisuje, jak toouse služby StorSimple Data Manager uživatelského rozhraní (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: b0ee12b3e495400b54e48eb1a98c68b1af2e5f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-using-hello-storsimple-data-manager-service-ui-private-preview"></a>Spravovat pomocí služby StorSimple Manager dat hello uživatelského rozhraní (soukromém náhledu).

Tento článek vysvětluje, jak můžete použít hello transformaci dat tooperform uživatelského rozhraní správce StorSimple dat na data uložená na řadu zařízení StorSimple 8000 hello. Hello Transformovaná data můžete pak být spotřebovávána jinými službami Azure, například Azure Media Services, Azure HDInsight, Azure Machine Learning a Azure Search. 


## <a name="use-storsimple-data-transformation"></a>Transformace dat StorSimple použít

Hello Data Manager zařízení StorSimple je hello prostředků v rámci kterého transformaci dat se dá vytvořit instance. Hello transformaci dat služby umožňuje přesun dat z vašeho zařízení StorSimple místní zařízení tooblobs v úložišti Azure. Proto v pracovním postupu potřebujete toospecify hello podrobnosti o StorSimple zařízení a hello dat, které chcete účet úložiště toohello toomove zájmu.

### <a name="create-a-storsimple-data-manager-service"></a>Vytvoření služby StorSimple Manager dat

Proveďte následující kroky toocreate služby StorSimple Manager dat hello.

1. toocreate služby StorSimple Manager dat, přejděte příliš[https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)

2. Klikněte na tlačítko hello  **+**  ikonu a vyhledávání pro StorSimple Data Manager. Klikněte na tlačítko služby StorSimple Manager dat a pak klikněte na **vytvořit**.

3. Pokud je vaše předplatné povolená pro vytvoření této služby, uvidíte hello následující okno.

    ![Vytvořte prostředek StorSimple Data správců](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. Zadejte vstupy hello a klikněte na **vytvořit**. Hello zadané umístění musí být hello jeden zaštiťující účty úložiště a služby StorSimple Manager. V současné době jsou podporovány pouze oblasti západní USA a západní Evropa. Proto vaší služby StorSimple Manager, služba Data Manager a hello přidružené účtu úložiště musí být v oblastech hello předchozí podporována. Trvá o minutu toocreate hello služby.

### <a name="create-a-data-transformation-job-definition"></a>Vytvoření definice úlohy transformace dat

V rámci služby StorSimple Manager dat je nutné toocreate definice úlohy transformace data. Definice úlohy Určuje podrobnosti hello data, která vás zajímá Přesun do účtu úložiště v nativním formátu hello. 

Proveďte následující kroky toocreate novou definici úlohy transformace dat hello.

1.  Přejděte toohello službu, kterou jste vytvořili. Klikněte na tlačítko **+ úlohy definice**.

    ![Klikněte na tlačítko + definice úlohy](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. Otevře okno Definice úlohy nové Hello. Pojmenujte svou definici úlohy a klikněte na tlačítko **zdroj**. V hello **zdroj dat konfigurace** okno, zadejte hello podrobnosti o zařízení StorSimple a hello požadovaná data.

    ![Vytvořit definici úlohy](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. Vzhledem k tomu, že toto je nová služba Data Manager, jsou nakonfigurovány žádné datové úložiště. Klikněte na tlačítko tooadd vaše StorSimple Manager jako úložiště dat, **přidat nový** v hello rozevírací úložiště dat a pak klikněte na **úložiště dat přidat**.

4. Zvolte **řady StorSimple 8000 Manager** jako úložiště hello zadejte a zadejte vlastnosti hello vaše **StorSimple Manager**. Pro hello **Id prostředku** pole, je třeba číslo hello tooenter před hello **:** v hello registračního klíče služby StorSimple manager.

    ![Vytvoření zdroje dat](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  Klikněte na tlačítko **OK** po dokončení. To umožňuje ušetřit úložiště dat a tato StorSimple Manager lze opětovně použít v jiné definice úlohy bez opětovného zadávání tyto parametry. Jak dlouho trvá několik sekund, po kliknutí na tlačítko **OK** pro hello tooshow StorSimple Manager nahoru v rozevírací nabídce hello.

6.  V hello **zdroj dat konfigurace** okno, zadejte název zařízení hello a hello název svazku, která obsahuje vaše data týkající se.

7.  V hello **filtru** pododdílu, zadejte hello kořenový adresář, který obsahuje vaše data týkající se (v tomto poli by měla začínat znakem `\`). Můžete také přidat všechny souboru filtry.

8.  Služba transformaci dat Hello funguje na hello data, která se instaluje se toohello Azure prostřednictvím snímky. Při spuštění této úlohy můžete zvolit tootake zálohu pokaždé, když tato úloha se spouští (toowork na nejnovější data) nebo toouse hello poslední existující zálohy v cloudu hello (Pokud pracujete na některé Archivovaná data).

    ![Podrobnosti nové zdroje dat](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. V dalším kroku nastavení cílového hello potřebovat toobe nakonfigurované. Existují 2 typy podporované cíle – účty Azure Storage a účty služby Azure Media Services. Vyberte účty úložiště tooput soubory do objektů BLOB v daném účtu. Výběr účtu media services tooput souborů do prostředky v daném účtu. Znovu potřebujeme tooadd úložiště. V rozevírací nabídce hello, vyberte **přidat nový** a potom **nakonfigurovat nastavení**.

    ![Vytvoření jímku dat](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. Tady můžete vybrat hello typ úložiště, které chcete tooadd a hello další parametry související s úložištěm hello. V obou případech se vytvoří frontu úložiště při spuštění úlohy hello. Tato fronta je naplňována zprávami o transformovaných objektech blob, jakmile jsou připravené. Hello název této fronty je hello stejný jako název hello hello definice úlohy. Pokud vyberete **Media Services** jako hello typ úložiště, pak můžete také zadat přihlašovací údaje účtu úložiště kde je vytvářena fronta hello.

    ![Nová data jímky podrobnosti](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. Po přidání hello úložiště dat, (která má několik sekund), zjistí hello úložišti v rozevírací nabídce hello v hello **název cílového účtu**.  Zvolte hello cíl, který potřebujete.

12. Klikněte na tlačítko **OK** definice úlohy toocreate hello. Vaše definice úlohy je teď nastavený. Můžete použít tuto definici úlohy několikrát prostřednictvím hello uživatelského rozhraní.

    ![Přidat nové definice úlohy](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-hello-job-definition"></a>Spustit definice úlohy hello

Kdykoli budete potřebovat toomove data z účtu úložiště toohello StorSimple, které jste zadali v definici úlohy hello, budete potřebovat tooinvoke ho. V každém vyvolání úlohy hello Změna parametrů hello je určitou volnost. Hello kroky jsou následující:

1. Vyberte služby StorSimple Manager dat a pak použijte příliš**monitorování**. Klikněte na tlačítko **spustit nyní**.

    ![Definice úlohy aktivační události](./media/storsimple-data-manager-ui/run-now.png)

2. Zvolte hello definice úlohy, které chcete toorun. Klikněte na tlačítko **spustit nastavení** toomodify všechna nastavení, které můžete chtít toochange pro tuto úlohu spustit.

    ![Nastavení úloh spustit](./media/storsimple-data-manager-ui/run-settings.png)

3. Klikněte na tlačítko **OK** a pak klikněte na **spustit** toolaunch úlohu. toomonitor této úlohy, přejděte toohello **úlohy** stránky vaše Data Manager zařízení StorSimple.

    ![Seznam úloh a stav](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. V přidání toomonitoring v hello **úlohy** okno, můžete také poslouchat na fronty hello úložiště, kde se zpráva přidá pokaždé, když je přesunut do souboru z účtu úložiště toohello StorSimple.


## <a name="next-steps"></a>Další kroky

[Pomocí sady .NET SDK toolaunch StorSimple Manager dat úloh](storsimple-data-manager-dotnet-jobs.md).
