---
title: "aaaMonitor zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak toouse hello toomonitor vstupně-výstupní výkon služby StorSimple Manager, využití kapacity, propustnost sítě a výkonu zařízení."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: bd4f7704-4f6f-47d0-927a-b1c91eabc453
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: c1f614a7f52728650bfadb3335435b8b5a17e6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomonitor-your-storsimple-device"></a>Použít toomonitor služby StorSimple Manager hello zařízení StorSimple
## <a name="overview"></a>Přehled
V rámci vašeho řešení StorSimple můžete použít hello StorSimple Manager service toomonitor konkrétní zařízení. Můžete vytvořit vlastní grafy na základě vstupně-výstupní výkon, využití kapacity, propustnost sítě a metriky výkonu zařízení. 

hello tooview monitorování informace pro konkrétní zařízení, v hello portál Azure classic, vyberte hello služby StorSimple Manager. Klikněte na tlačítko hello **monitorování** kartě a pak vyberte ze seznamu hello zařízení. Hello **monitorování** stránka obsahuje následující informace hello.

## <a name="io-performance"></a>Vstupně-výstupní výkon
**Vstupně-výstupní výkon** sleduje metriky související toohello počet čtení a zápisu operace mezi serverem iSCSI buď hello iniciátor rozhraní na serveru hostitele hello a hello zařízení nebo zařízení hello a hello cloudu. Tento výkon lze měřit pro konkrétní svazku, kontejner svazků konkrétní nebo všechny kontejnery svazků.

Graf Hello níže znázorňuje hello vstupně-výstupních operací pro hello iniciátor tooyour zařízení pro všechny svazky hello pro produkční zařízení. Hello metriky vykreslí čtení a zápisu bajtů za sekundu, čtení a vstupně-výstupní operace za sekundu, zápisu a čtení a zápisu latence.

![Výkon vstupně-výstupní operace z toodevice iniciátor](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Pro hello stejného zařízení, hello vstupně-výstupní operace jsou zobrazeny pro hello data z cloudu toohello hello zařízení pro všechny hello kontejnery svazků. Na tomto zařízení hello data jsou pouze ve vrstvě lineární hello a nic se uniknout toohello cloudu. Neprobíhají žádné operace čtení a zápis, ke kterým dochází z cloudu toohello zařízení. Proto jsou hello vrcholů v grafu hello v intervalu 5 minut, který odpovídá frekvence toohello, kdy se kontroluje prezenčního signálu hello mezi hello zařízením a službou hello. 

![Výkon vstupně-výstupní operace z toocloud zařízení](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)

Pro hello stejného zařízení, cloudový snímek nebyla provedena pro svazek data začínající na 2:00 pm. Výsledkem dat odesílaných z cloudu toohello hello zařízení. Čtení zápisy byly zpracovat toohello cloudu v této hodnotě duration. Hello vstupně-výstupní operace graf znázorňuje ve špičce v hello různé metriky odpovídající toohello čas při pořízení snímku hello. 

![Výkon vstupně-výstupní operace pro zařízení toocloud po cloudový snímek](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)

## <a name="capacity-utilization"></a>Využití kapacity
**Využití kapacity** sleduje metriky související toohello množství prostoru úložiště dat, který je používán hello svazky, kontejnery svazků nebo zařízení. Můžete vytvořit sestavy na základě využití kapacity hello primárního úložiště, cloudového úložiště nebo úložiště zařízení. Využití kapacity lze měřit na konkrétním svazku, kontejner svazků konkrétní nebo všechny kontejnery svazků.

Hello primární, cloud a kapacitu úložiště zařízení lze popsat následujícím způsobem:

### <a name="primary-storage-capacity-utilization"></a>Využití kapacity primárního úložiště
Tyto grafy zobrazují hello množství dat zapsaných tooStorSimple svazky předtím, než je s odstraněním duplicitních dat a komprimované hello data. Využití úložiště primární hello můžete zobrazit všechny svazky nebo pro samostatný svazek.

Při zobrazení grafů využití hello primárního úložiště svazku kapacity pro všechny svazky a všechny jednotlivé svazky hello a součet hello primární data v obou těchto případech, může být Neshoda mezi hello dvou čísel. Celkový počet primární data ze všech svazků Hello nemusí dohromady celkový součet toohello hello primární data hello jednotlivé svazky. Důvodem mohou být tooone hello následující:

* **Snímek dat, které jsou zahrnuté pro všechny svazky**: Toto chování je vidět jenom v případě, že používáte verzi starší než Update 3. Hello primární data zobrazená pro všechny svazky hello je součtem hello hello primární data pro jednotlivé svazky a data snímku hello. Hello primární data zobrazená pro daný svazek odpovídá tooonly hello množství dat, které jsou přiděleny na svazku hello (a nesmí obsahovat data snímku svazku odpovídající hello).
  
    To lze také vysvětlit hello následující rovnice:
  
    *Primární data (všech svazků) = součet (primární data (svazku i) + velikost dat snímku (svazku i))*
  
    *WHERE, primární data (svazku i) = velikost primárního datového přidělené toovolume i*
  
    Pokud hello snímky jsou odstraněny prostřednictvím služby hello, odstranění hello se asynchronně provádí v pozadí hello. To může trvat nějakou dobu hello svazku data velikost toobe aktualizovat po odstranění snímku hello. 
  
    Pokud verzi Update 3 nebo novější, data snímku hello není zahrnutý v datech hello svazku. A primární využití hello se vypočítává takto:
  
    * Primární data (všech svazků) = součet (primární dat (svazku i)
  
    *WHERE, primární data (svazku i) = velikost primárního datového přidělené toovolume i*
* **Svazky s monitorováním zakázáno součástí všech svazků**: Pokud máte svazky na vašem zařízení, pro které monitorování je vypnutý, hello dat pro tyto svazky jednotlivých monitorování nebudete mít k dispozici v grafech hello. Ale hello data pro všechny svazky v grafu hello obsahuje hello svazky, pro které monitorování je vypnutý. 
* **Odstranit svazky s za provozu přidružených záloh, které jsou zahrnuté pro všechny svazky**: svazky obsahující data snímku se odstraní, ale stále existují hello přidružené snímky a pak mohou se zobrazit neshody.
* **Odstranit svazky, které jsou zahrnuté pro všechny svazky**: V některých případech může existovat starých svazků, i když tyto byly odstraněny. není vidět Hello důsledky odstranění a hello zařízení může zobrazovat nižší dostupné kapacity. Toocontact Microsoft Support tooremove musíte tyto svazky.

Hello následující grafech využití kapacity úložiště primární hello zařízení StorSimple před a po pořízení snímku cloudu. Protože se právě objem dat, cloudový snímek neměli měnit hello primárního úložiště. Jak vidíte, hello graf znázorňuje žádný rozdíl ve využití hello primární kapacity v důsledku pořizování snímku cloudu. Hello cloudový snímek zahájená přibližně 2:00 pm na daném zařízení.

![Využití kapacity primární před cloudový snímek](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Využití kapacity primární po cloudový snímek](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Pokud používáte Update 2 nebo vyšší, lze rozčlenit využití kapacity primárního úložiště hello svazku, všechny svazky, všechny vrstvené svazky a všechny místně vázaných svazků jak je uvedeno níže. Rozdělení podle všechny místně vázaných svazků vám umožní tooquickly zjistit, kolik místní vrstvy hello se používá.

![Využití primární kapacity pro všechny místně vázaných svazků](./media/storsimple-monitor-device/localvolumes.png)

### <a name="cloud-storage-capacity-utilization"></a>Využití kapacity úložiště cloudu
Tyto grafy zobrazují hello množství používá úložiště v cloudu. Tato data se odstraňují a komprimované. Toto množství zahrnuje cloudových snímků, které může obsahovat data, která se nereflektují v jakýkoli svazek, primární a je uložen pro účely starší verze nebo požadované uchování. Můžete porovnat hello primární a cloudové úložiště spotřeba obrázky tooget představu o snížení data hello hodnocení, i když hello počet nesmí být přesné. Hello následující grafech hello cloudu využití kapacity úložiště zařízení StorSimple před a po pořízení snímku cloudu. Hello cloudový snímek zahájená přibližně 2:00 pm na daném zařízení, uvidíte využití kapacity cloudu hello snímek v hello stejný čas, zvýšit z too4.04 5.73 MB, GB.

![Využití kapacity cloudu před cloudový snímek](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Využití kapacity cloudu po cloudový snímek](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)

### <a name="device-storage-capacity-utilization"></a>Využití kapacity úložiště zařízení
Tyto grafy zobrazují celkové využití hello hello zařízení, který může mít více než využití primárního úložiště, protože obsahuje lineární vrstvy SSD hello. Tato úroveň obsahuje množství dat, která také existuje na hello zařízení na jiných úrovních. Hello kapacity lineární vrstvy SSD hello je procházení tak, aby při nová data se dodává, stará data hello vrstvy HDD přesunutý toohello (kdy je s odstraněním duplicitních dat a komprimované) a následně toohello cloudu.

V průběhu času využití primární kapacitu a využití kapacity zařízení pravděpodobně zvýší společně dokud hello dat začíná toobe vrstvené toohello cloudu. V tomto bodě využití kapacity zařízení hello pravděpodobně zahájíte tooplateau, ale jako další data se zapisují, zvýší se využití kapacity primární hello.

Hello následující grafech využití kapacity úložiště primární hello zařízení StorSimple před a po pořízení snímku cloudu. Hello cloudový snímek zahájená 14:00:00 a využití kapacity zařízení hello spuštění snížení v daném čase. využití kapacity úložiště zařízení Hello snížil z 11.58 GB too7.48 GB. To označuje, že s největší pravděpodobností hello nekomprimovaných dat ve vrstvě SSD lineární hello bylo odstranění duplicit, komprimované a přesunout do vrstvy HDD hello. Všimněte si, že pokud hello zařízení už má velké množství dat v hello SSD i vrstvy HDD, nemusíte vidět toto snížení. V tomto příkladu hello zařízení má malou část data.

![Využití kapacity zařízení před cloudový snímek](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Využití kapacity zařízení po cloudový snímek](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)

## <a name="network-throughput"></a>Propustnost sítě
**Propustnost sítě** sleduje metriky související toohello množství dat přenesených od hello iSCSI initiator síťových rozhraní hello hostitelský server a hello zařízení a mezi hello zařízení a hello cloudu. Tato metrika pro každou síťová rozhraní iSCSI hello můžete sledovat na vašem zařízení.

Hello následující grafy zobrazují hello propustnost sítě pro hello Data 0 a Data 4 síťových rozhraní 1 GbE i na vašem zařízení. V tomto případě Data 0 byl povolenou podporu cloudu zatímco 4 dat byla iSCSI povolený. Se zobrazí oba hello příchozí a odchozí přenosy pro zařízení StorSimple hello. Hello ploché řádku v grafu hello od 15:24:00 je díky toohello skutečnost, že jsme shromažďování dat pouze každých 5 minut a třeba ji ignorovat. 

![Propustnost sítě pro Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Propustnost sítě pro Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)

## <a name="device-performance"></a>Výkon zařízení
**Výkon zařízení** sleduje metriky související toohello výkon vašeho zařízení. Hello následující graf znázorňuje statistik využití procesoru hello pro zařízení v provozním prostředí.

![Využití procesoru pro zařízení](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[hello řídicí panel zařízení služby StorSimple Manager](storsimple-device-dashboard.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

