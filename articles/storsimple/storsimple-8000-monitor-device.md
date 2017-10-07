---
title: "aaaMonitor zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak služba toouse hello Správce zařízení StorSimple toomonitor využití vstupně-výstupní výkon a využití kapacity."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 092dab8dd301c50fc12316b4031a8d1b34fab876
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomonitor-your-storsimple-device"></a>Použít toomonitor služby StorSimple Manager zařízení hello zařízení StorSimple
## <a name="overview"></a>Přehled
V rámci vašeho řešení StorSimple můžete použít hello Správce zařízení StorSimple služby toomonitor konkrétní zařízení. Můžete vytvořit vlastní grafy na základě vstupně-výstupní výkon, využití kapacity, propustnost sítě a metriky výkonu zařízení a připnout těchto toohello řídicího panelu. Další informace, přejděte příliš[přizpůsobit řídicího panelu portálu](/articles/azure-portal/azure-portal-dashboards.md).

tooview hello informací o monitorování pro určité zařízení, v hello portál Azure, vyberte službu StorSimple Manager zařízení hello. Hello seznam zařízení, vyberte zařízení a potom přejděte příliš**monitorování**. Můžete se podívat, hello **kapacity**, **využití**, a **výkonu** grafy pro vybrané zařízení hello.

## <a name="capacity"></a>Kapacita
**Kapacita** sleduje hello zřízené místo a hello zbývající místo na hello zařízení. Hello zbývající kapacity se následně zobrazí jako místně vázaný nebo vrstvené.

Hello zřízený a zbývající kapacita je další členěné podle vrstvené a místně vázaných svazků. Pro každý svazek hello zřídit kapacitu a hello zbývající kapacity na hello zařízení se zobrazí.

![Kapacita v/v](./media/storsimple-8000-monitor-device/device-capacity.png)



## <a name="usage"></a>Využití
**Využití** sleduje metriky související toohello množství prostoru úložiště dat, který je používán hello svazky, kontejnery svazků nebo zařízení. Můžete vytvořit sestavy na základě využití kapacity hello primárního úložiště, cloudového úložiště nebo úložiště zařízení. Využití kapacity lze měřit na konkrétním svazku, kontejner svazků konkrétní nebo všechny kontejnery svazků.
Ve výchozím nastavení je uvedená hello využití za posledních 24 hodin. Můžete upravit hello grafu toochange hello trvání, přes které hello využití údajně výběrem ze:
* Za posledních 24 hodin
* Počet uplynulých 7 dní
* Posledních 30 dnů
* Posledních 90 dnů
* Za poslední rok


Hello primární, cloud a místní úložiště používá lze popsat následujícím způsobem:

### <a name="primary-storage-usage"></a>Využití primárního úložiště
Tyto grafy zobrazují hello množství dat zapsaných tooStorSimple svazky předtím, než je s odstraněním duplicitních dat a komprimované hello data. Můžete zobrazit hello primárního úložiště používané všechny svazky v kontejneru svazků nebo pro samostatný svazek. Hello používá primárního úložiště je rozdělena další primární vrstvené úložiště primární a použitá místně vázaný úložiště používá.

Hello následující grafech hello primárního úložiště používá pro zařízení StorSimple před a po pořízení snímku cloudu. Protože se právě objem dat, cloudový snímek neměli měnit hello primárního úložiště. Jak vidíte, hello graf znázorňuje žádné rozdíly v úložišti pro vrstvené nebo místně vázaný primární hello je použit v důsledku pořizování snímku cloudu. Hello cloudový snímek zahájená přibližně 23:50:00 na daném zařízení.

![Využití kapacity primární po cloudový snímek](./media/storsimple-8000-monitor-device/device-primary-storage-after-cloudsnapshot.png)

Pokud teď spustit vstupně-výstupních operací na zařízení StorSimple tooyour hello hostování připojené, zobrazí se o zvýšení v primární vrstveného úložiště nebo primární místně připnutý úložiště používané v závislosti na svazků, které (víceúrovňová nebo místně vázaný) napíšete hello data. Tady jsou hello primárního úložiště využití grafy pro zařízení StorSimple. Na tomto zařízení hostitele zařízení StorSimple hello spuštění obsluhující zapíše na přibližně 2:30 pm na vrstvený svazek v zařízení hello. Můžete zobrazit ve špičce hello v hello zápisu bajtů/s odpovídající toohello vstupně-výstupních operací na hostiteli hello spuštěná.

![Výkon, pokud je spuštěn vstupně-výstupní operace zřízeny vrstvené svazky](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

Pokud si prohlédnete hello primární vrstveného úložiště používá, který prošel nahoru, zatímco hello primární místně vázaný využití zůstává beze změny, protože neexistují žádné zápisy zpracovat svazky toohello místně vázaný na hello zařízení.

![Využití kapacity primární při vstupně-výstupní operace jsou spuštěny na vrstvené svazky](./media/storsimple-8000-monitor-device/device-primary-storage-io-from-initiator.png)

Pokud používáte aktualizací 3 nebo vyšší, lze rozčlenit využití kapacity primárního úložiště hello svazku, všechny svazky, všechny vrstvené svazky a všechny místně vázaných svazků jak je uvedeno níže. Rozdělení podle všechny místně vázaných svazků vám umožní tooquickly zjistit, kolik místní vrstvy hello se používá.

![Využití primární kapacity pro všechny vrstvené svazky](./media/storsimple-8000-monitor-device/monitor-usage3.png)

![Využití primární kapacity pro všechny místně vázaných svazků](./media/storsimple-8000-monitor-device/monitor-usage4.png)

Můžete dál klikněte na každý ze svazků hello v seznamu hello a zobrazit odpovídající využití hello.

![Využití primární kapacity pro všechny místně vázaných svazků](./media/storsimple-8000-monitor-device/device-primary-storage-usage-by-volume.png)

### <a name="cloud-storage-usage"></a>Využití úložiště cloudu
Tyto grafy zobrazují hello množství používá úložiště v cloudu. Tato data se odstraňují a komprimované. Toto množství zahrnuje cloudových snímků, které může obsahovat data, která se nereflektují v jakýkoli svazek, primární a je uložen pro účely starší verze nebo požadované uchování. Můžete porovnat hello primární a cloudové úložiště spotřeba obrázky tooget představu o snížení data hello hodnocení, i když hello počet nesmí být přesné.

Hello následující grafech hello cloudu využití úložiště na zařízení StorSimple při pořízení snímku cloudu.

* Hello cloudový snímek zahájená přibližně 11:50 am na daném zařízení a můžete uvidíte, že před hello cloudový snímek, se žádné úložiště v cloudu používat. 
* Jednou cloudový snímek hello dokončit využití úložiště cloudu hello snímek až rovný 0,89 GB. 
* Během provádění hello cloudový snímek, je zde i odpovídající ve špičce hello vstupně-výstupní operace z toocloud zařízení.

    ![Využití úložiště cloudu před cloudový snímek](./media/storsimple-8000-monitor-device/device-cloud-storage-before-cloudsnapshot.png)

    ![Využití úložiště cloudu po cloudový snímek](./media/storsimple-8000-monitor-device/device-cloud-storage-after-cloudsnapshot.png)

    ![Vstupně-výstupní operace z toocloud zařízení během cloudový snímek](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)


### <a name="local-storage-usage"></a>Využití místní úložiště
Tyto grafy zobrazují celkové využití hello hello zařízení, který může mít více než použití primárního úložiště, protože obsahuje lineární vrstvy SSD hello. Tato úroveň obsahuje množství dat, která také existuje na hello zařízení na jiných úrovních. Hello kapacity lineární vrstvy SSD hello je procházení tak, aby při nová data se dodává, stará data hello vrstvy HDD přesunutý toohello (kdy je s odstraněním duplicitních dat a komprimované) a následně toohello cloudu.

V průběhu času vrstvené primárního úložiště, které použité a místní úložiště používá pravděpodobně zvýší společně dokud hello dat začíná toobe toohello cloudu. V tomto okamžiku tooplateau pravděpodobně zahájíte hello používá místní úložiště, ale hello primárního úložiště používá zvýší při další data jsou zapsána.

Hello následující grafech hello primárního úložiště používá se pro zařízení StorSimple při pořízení snímku cloudu. Hello cloudový snímek zahájená 11:50 am a místní úložiště hello spuštění snížení v daném čase. použít místní úložiště Hello snížil z 1.445 GB too1.09 GB. To označuje, že s největší pravděpodobností hello nekomprimovaných dat ve vrstvě SSD lineární hello bylo odstranění duplicit, komprimované a přesunout do vrstvy HDD hello. Všimněte si, že pokud hello zařízení už má velké množství dat v hello SSD i vrstvy HDD, nemusíte vidět toto snížení. V tomto příkladu hello zařízení má malou část data.

![Využití místní úložiště po cloudový snímek](./media/storsimple-8000-monitor-device/device-local-storage-after-cloudsnapshot.png)

## <a name="performance"></a>Výkon
**Výkon** sleduje metriky související toohello počet čtení a zápisu operace mezi serverem iSCSI buď hello iniciátor rozhraní na serveru hostitele hello a hello zařízení nebo zařízení hello a hello cloudu. Tento výkon lze měřit pro konkrétní svazku, kontejner svazků konkrétní nebo všechny kontejnery svazků. Výkon zahrnuje taky využití procesoru a propustnost sítě pro hello různých síťových rozhraní na vašem zařízení.

### <a name="io-performance-for-initiator-toodevice"></a>Vstupně-výstupní výkon pro toodevice iniciátor
Graf Hello níže znázorňuje hello vstupně-výstupních operací pro hello iniciátor tooyour zařízení pro všechny svazky hello pro produkční zařízení. metriky Hello vykreslí jsou čtení a zápisu bajtů za sekundu. Můžete také grafu pro čtení, zápisu a nezpracovaných vstupně-výstupní operace, nebo čtení a zápisu latenci.

![Výkon vstupně-výstupní operace pro toodevice iniciátor](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

### <a name="io-performance-for-device-toocloud"></a>Vstupně-výstupní výkon pro toocloud zařízení
Pro hello stejného zařízení, hello vstupně-výstupní operace jsou zobrazeny pro hello data z cloudu toohello hello zařízení pro všechny hello kontejnery svazků. Na tomto zařízení hello data jsou pouze ve vrstvě lineární hello a nic se uniknout toohello cloudu. Neprobíhají žádné operace čtení a zápis, ke kterým dochází z cloudu toohello zařízení. Proto jsou hello vrcholů v grafu hello v intervalu 5 minut, který odpovídá frekvence toohello, kdy se kontroluje prezenčního signálu hello mezi hello zařízením a službou hello.

Pro hello stejného zařízení, cloudový snímek nebyla provedena pro svazek data začínající na 11:50 am. Výsledkem dat odesílaných z cloudu toohello hello zařízení. Zápisy byly poskytovány toohello cloudu na této hodnotě duration. Hello vstupně-výstupní operace graf zobrazuje ve špičce hello zápisu bajtů/s odpovídající toohello čas při pořízení snímku hello.

![Vstupně-výstupní operace z toocloud zařízení během cloudový snímek](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)

### <a name="network-throughput-for-device-network-interfaces"></a>Propustnost sítě pro zařízení Síťová rozhraní
**Propustnost sítě** sleduje metriky související toohello množství dat přenesených od hello iSCSI initiator síťových rozhraní hello hostitelský server a hello zařízení a mezi hello zařízení a hello cloudu. Tato metrika pro každou síťová rozhraní iSCSI hello můžete sledovat na vašem zařízení.

Hello následující grafy zobrazují hello propustnost sítě pro hello Data 0, 1 1 GbE sítě na vašem zařízení, která byla obou povolenou podporu cloudu (výchozí) a podporou iSCSI. Na tomto zařízení na června 14 v přibližně 21: 00 byl vrstvené data do cloudu hello (žádný cloud, které snímky byly provedeny na čas, které se tootiering body hello mechanismus toomove hello data do cloudu hello), což vedlo vstupně-výstupní operace, které jsou obsluhovány toohello cloudu. Neexistuje odpovídající ve špičce v grafu propustnost sítě hello, hello současně a většina síťových přenosů hello je odchozí toohello cloudu.

![Propustnost sítě pro Data 0](./media/storsimple-8000-monitor-device/device-network-throughput-data0.png)

Pokud podíváme na graf propustnost rozhraní hello Data 1, jiné 1gbe síťové rozhraní, která byla pouze iSCSI povolený, pak se prakticky žádné síťové přenosy v této hodnotě duration.

![Propustnost sítě pro Data 1](./media/storsimple-8000-monitor-device/device-network-throughput-data1.png)


## <a name="cpu-utilization-for-device"></a>Využití procesoru pro zařízení
**Využití procesoru** sleduje metriky související toohello procesoru použity na zařízení. Hello následující graf znázorňuje statistik využití procesoru hello pro zařízení v provozním prostředí.

![Využití procesoru pro zařízení](./media/storsimple-8000-monitor-device/device-cpu-utilization.png)



## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[hello řídicí panel zařízení služby StorSimple Manager zařízení](storsimple-device-dashboard.md).
* Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-manager-service-administration.md).

