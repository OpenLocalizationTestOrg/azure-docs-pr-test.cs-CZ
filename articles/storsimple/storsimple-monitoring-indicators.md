---
title: "monitorování indikátory aaaStorSimple | Microsoft Docs"
description: "Popisuje hello svítivé diody (LED) a zvukových výstrahy použít toomonitor hello stav zařízení StorSimple hello."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 59dee7b9-ca6d-4fd9-96e6-a0071e8d248e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: e690b8f4727272f5fbb8886a594a046f794a1380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-monitoring-indicators-toomanage-your-device"></a>Použití vašeho zařízení StorSimple monitorování toomanage ukazatele
## <a name="overview"></a>Přehled
Zařízení StorSimple zahrnuje svítivé diody (LED) a výstrahy, které můžete použít toomonitor hello moduly a celkového stavu zařízení StorSimple hello. Hello monitorování indikátory naleznete na hello hardwarové součásti primární skříň hello zařízení a skříň EBOD hello. Hello ukazatele monitorování může být LED nebo zvukové výstrahy.

Existují tři DIODU stavů, použít tooindicate hello stav modulu: zelenou přerušované zelená toored oranžová, nebo žlutou red.  

* Zelená LED představují provozní stav v pořádku.  
* Blikající zelená toored žlutou LED představují hello přítomnost nekritické podmínky, které mohou vyžadovat zásah uživatele.  
* Red žlutou LED znamenat, že existuje v rámci modulu hello kritickou chybu.  

Hello zbývající část tohoto článku popisuje hello různých monitorování kláves, jejich umístění na zařízení StorSimple hello, stav zařízení hello podle hello DIODU stavy a všech přidružených zvukové výstrahy.

## <a name="front-panel-indicator-leds"></a>Přední panel indikátor LED
přední panel Hello, také známé jako hello *operations panely* nebo *ops panely*, zobrazí hello agregovaný stav všech modulů hello systému hello. přední panel Hello je stejná na hello StorSimple primární i hello EBOD skříň a je zobrazený dole.  

   ![Zařízení předního panelu][1]

přední panel Hello obsahuje následující indikátory hello:  

1. Tlačítko pro ztlumení
2. Indikátor Power DIODU (zelený/red žlutou)
3. Indikátor selhání modulu VEDLA (na red oranžová/OFF)
4. Logické chyby indikátor VEDLA (na red oranžová/OFF
5. Zobrazení ID jednotky  

Hello hlavní rozdíl mezi přední panel hello LED hello zařízení a pro hello EBOD skříň je hello **systému jednotky identifikační číslo** zobrazený na hello DIODU zobrazení. jednotka výchozí Hello ID zobrazí na zařízení hello je **00**, kdežto ID jednotky výchozí hello zobrazí na hello EBOD skříň **01**. To vám umožní tooquickly rozlišit mezi hello zařízení a hello EBOD skříň po zapnutí zařízení hello. Pokud vaše zařízení je vypnutý, použijte hello informace uvedené v [zapnout nové zařízení](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) toodifferentiate hello zařízení z hello EBOD skříň.  

## <a name="front-panel-led-status"></a>Přední panel Indikátor stavu
Pomocí následující tabulky tooidentify hello stav indikován hello LED na hello přední panel pro hello zařízení nebo hello EBOD skříň hello.  

| Napájení systému | Selhání modulu | Logické chyby | Výstrahy | Status |
| --- | --- | --- | --- | --- |
| Red oranžová |OFF |OFF |Není k dispozici |Ztratili napájení ze sítě, operační na zálohování napájení nebo napájení ze sítě na a hello řadiče moduly byly odebrány. |
| Zelená |ON |ON |Není k dispozici |Test stavu OPS panely zapnutí (5s) |
| Zelená |OFF |OFF |Není k dispozici |Zapnutí všechny funkce funkční |
| Zelená |ON |Není k dispozici |Selhání PCM LED selhání ventilátor LED |Všechny chyby PCM ventilátor odolnost, nad nebo pod teploty |
| Zelená |ON |Není k dispozici |Vstupně-výstupních operací modulu LED |Všechny řadiče selhání modulu |
| Zelená |ON |Není k dispozici |Není k dispozici |Skříň logiku selhání |
| Zelená |Flash |Není k dispozici |V modulu řadiče VEDLA stav modulu. Selhání PCM LED selhání ventilátor LED |Typ modulu neznámé řadiče nainstalovaná, I2C sběrnice selhání, Chyba konfigurace řadiče modulu produktu důležitých dat (VPD) |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Napájení chladicí modulu (PCM) indikátor LED
Napájení chladicí modulu (PCM) indikátor LED naleznete na zadní hello primární skříň nebo EBOD skříň na každý modul PCM hello. Toto téma popisuje, jak toouse hello následující LED toomonitor hello stav zařízení StorSimple.  

* PCM LED pro primární skříň hello
* PCM LED pro hello EBOD skříň

## <a name="pcm-leds-for-hello-primary-enclosure"></a>PCM LED pro primární skříň hello
zařízení StorSimple Hello má modul 764W PCM s další stav baterie. Hello následující obrázek znázorňuje hello DIODU panel pro hello zařízení.  

   ![PCM LED na primární skříň hello][2]

Indikátor legendy:

1. Výpadku napájení ze sítě
2. Ventilátor selhání
3. Selhání stav baterie.
4. PCM OK
5. Řadič domény selhání
6. Dobrý stav baterie  

Stav Hello hello PCM uvedené na hello VEDLA panelu. panel PCM DIODU Hello zařízení má šest LED. Čtyři tyto LED zobrazí stav hello hello napájení a ventilátor hello. zbývající dvě LED Hello označují stav hello hello zálohování baterie modulu v hello PCM. Můžete použít následující tabulky toodetermine hello stav hello PCM hello.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>Indikátor PCM LED pro napájení a ventilátor
| Status | PCM OK (zelený) | Selhání AC (oranžové) | Ventilátor služeb při selhání (oranžové) | Řadič domény selhání (oranžové) |
| --- | --- | --- | --- | --- |
| Napájení (tooenclosure) žádné ze sítě |OFF |OFF |OFF |OFF |
| Napájení (pouze tento PCM) žádné ze sítě |OFF |ON |OFF |ON |
| AC prezentovat PCM ON - OK |ON |OFF |OFF |OFF |
| Selhání PCM (ventilátor selhání) |OFF |OFF |ON |Není k dispozici |
| Selhání PCM (přes amp přes napětí přes aktuální) |OFF |ON |ON |ON |
| PCM (ventilátor mimo toleranci) |ON |OFF |OFF |ON |
| Pohotovostní režim |Blikání |OFF |OFF |OFF |
| Stažení firmwaru PCM |OFF |Blikání |Blikání |Blikání |

### <a name="pcm-indicator-leds-for-hello-backup-battery"></a>Indikátor PCM LED pro zálohování baterie hello
| Status | Baterie dobrý (zelený) | Stav baterie. chyby (oranžová) |
| --- | --- | --- |
| Nejsou k dispozici baterie |OFF |OFF |
| Baterie přítomen a účtovat |ON |OFF |
| Baterie poplatků nebo Údržba na vyřízení |Blikání |OFF |
| Baterie "soft" selhání (obnovitelné) |OFF |Blikání |
| "Pevné" selhání baterie (neobnovitelná) |OFF |ON |
| S odstraněnými stav baterie. |Blikání |OFF |

## <a name="pcm-leds-for-hello-ebod-enclosure"></a>PCM LED pro hello EBOD skříň
Hello EBOD skříň má 580W PCM a žádná další baterie. Hello PCM panel pro hello EBOD skříň má kláves pouze pro hello napájení a ventilátor hello. Hello následující obrázek znázorňuje tyto LED.

   ![PCM LED na hello EBOD skříň][3] 

Můžete použít následující tabulky toodetermine hello stav hello PCM hello.  

| Status | PCM OK (zelený) | Selhání AC (oranžové) | Ventilátor služeb při selhání (oranžové) | Řadič domény selhání (oranžové) |
| --- | --- | --- | --- | --- |
| Napájení (tooenclosure) žádné ze sítě |OFF |OFF |OFF |OFF |
| Napájení (pouze tento PCM) žádné ze sítě |OFF |ON |OFF |ON |
| AC prezentovat PCM ON – OK |ON |OFF |OFF |OFF |
| Selhání PCM (ventilátor selhání) |OFF |OFF |ON |X |
| Selhání PCM (přes amp přes napětí přes aktuální |OFF |ON |ON |ON |
| PCM (ventilátor mimo toleranci) |ON |OFF |OFF |ON |
| Pohotovostní modelu |Blikání |OFF |OFF |OFF |
| Stažení firmwaru PCM |OFF |Blikání |Blikání |Blikání |

## <a name="controller-module-indicator-leds"></a>Řadič modulu indikátor LED
zařízení StorSimple Hello obsahuje LED pro primární řadič hello a hello EBOD řadiče moduly.   

### <a name="monitoring-leds-for-hello-primary-controller"></a>Monitorování LED pro primární řadič hello
Hello následujícím obrázku vám pomůže identifikovat hello LED na primárním řadiči hello. (Všechny součásti hello jsou uvedené tooaid v orientaci).  

   ![Monitorování LED - primární řadič][4]

Použití hello následující tabulka toodetermine, zda modul řadiče hello pracuje správně.  

### <a name="controller-indicator-leds"></a>Řadič indikátor LED
| INDIKÁTOR | Popis |
| --- | --- |
| ID DIODU (modrá) |Označuje, že se se identifikuje tento modul hello. Pokud hello blue DIODU bliká na běžícího řadiče, pak hello řadič je hello aktivní a hello jiného je hello pohotovostní řadič. Další informace najdete v tématu [řadič active identifikace hello na vašem zařízení](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Selhání DIODU (oranžová) |Označuje chybu v kontroleru hello. |
| Indikátor LED OK (zelený) |Konstantní zelená značí, že tento kontroler hello je OK. Blikající zelená značí řadič VPD Chyba konfigurace. |
| Aktivita SAS LED (zelený) |Konstantní zelená značí, připojení se žádné aktuální aktivity. Blikající zelená značí, že připojení hello má probíhající aktivity. |
| Stav Ethernet LED |Pravé straně označuje odkaz nebo síťové aktivity: active (blikající zelená) (konstantní zelený) odkaz síťové aktivity. Levé straně určuje rychlost sítě: (žlutý) 1000 Mb/s (zelený) 100 Mb/s a (OFF) 10 Mb/s. V závislosti na modelu součást hello může tento lehký blink i v případě, že není povoleno hello síťové rozhraní. |
| LED POST |Určuje spouštěcí průběh hello po zapnutí hello řadiče. Pokud zařízení StorSimple hello selže tooboot, tento DIODU pomůže identifikovat bod hello v hello proces spouštění, kdy došlo k selhání hello Microsoft Support. |

> [!IMPORTANT]
> Pokud je lit hello selhání DIODU, došlo k potížím s hello řadiče modul, který může vyřešit restartováním řadiče hello. Pokud se restartování řadiče hello tento problém nevyřeší, obraťte se na Microsoft Support.  
> 
> 

### <a name="monitoring-leds-for-hello-ebod-ebod-enclosure"></a>Monitorování LED hello EBOD (EBOD skříň)
Každý z hello 6 Gb/s SAS EBOD řadiče má LED, který indikuje její stav, jak ukazuje následující obrázek hello.  

  ![Monitorování LED - EBOD skříň][5]

Pomocí následující tabulky toodetermine, zda modul řadiče EBOD hello pracuje normálně hello.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD řadiče modulu indikátor LED
| Status | Vstupně-výstupních operací modulu OK (zelený) | Selhání modulu vstupně-výstupní operace (oranžová) | Aktivitu port hostitele (zelený) |
| --- | --- | --- | --- |
| Modul řadiče OK |ON |OFF |- |
| Selhání modulu řadiče |OFF |ON |- |
| Port připojení externího hostitele |- |- |OFF |
| Port připojení externího hostitele – žádná aktivita |- |- |ON |
| Připojení k externím hostiteli port - aktivity |- |- |Blikání |
| Chyba modulu metadat řadiče |Blikání |- |- |

## <a name="disk-drive-indicator-leds-for-hello-primary-enclosure-and-ebod-enclosure"></a>Disková jednotka indikátor LED pro primární skříň hello a EBOD skříň
zařízení StorSimple Hello má diskových jednotek, které jsou umístěné v primární skříň hello i hello EBOD skříň. Každé diskové jednotce obsahuje monitorování kláves, jak je popsáno v této části. 

Pro hello diskové jednotky, hello disku stav je indikován zelená DIODU a červená oranžová DIODU připojena hello před každou jednotku poskytovatel modulu. Hello následující obrázek znázorňuje tyto LED.

  ![LED diskovou jednotku][6]

Pomocí následující tabulky toodetermine hello stav každého disku, který naopak ovlivňuje hello celkové předního panelu Indikátor stavu hello.  

### <a name="disk-drive-indicator-leds-for-hello-ebod-enclosure"></a>Disková jednotka indikátor LED pro hello EBOD skříň
| Status | Aktivita DIODU OK (zelený) | Selhání DIODU (červená oranžová) | Související ops panely DIODU |
| --- | --- | --- | --- |
| Žádné jednotky nainstalována |OFF |OFF |Žádný |
| Jednotka nainstalovaný a funkční |Blikající zapnout nebo vypnout s aktivitou |X |Žádný |
| Sada identity zařízení SCSI skříň služby (SES) |ON |Blikající 1 sekunda na/1 sekundu vypnuto |Žádný |
| SES zařízení selhání bitová sada |ON |ON |Logické chyby (červený) |
| Selhání okruh řízení spotřeby |OFF |ON |Selhání modulu (červený) |

## <a name="audible-alarms"></a>Zvukové výstrahy
Zařízení StorSimple obsahuje zvukové výstrahy související s primární skříň hello a skříň EBOD hello. Zvukové poplašné se nachází na hello přední panel (také označované jako hello ops panely) i skříně. zvukové poplašné Hello označuje, když podmínky selhání nachází. hello výstrahy se budou aktivovat Hello následující podmínky:  

* Ventilátor chyb nebo selhání
* Napětí mimo rozsah.
* Nad nebo pod teploty podmínky
* Teplotní přetečení
* Selhání systému
* Logické chyby
* Selhání napájení napájení
* Odebrání napájení chlazení modulu (PCM)  

Hello následující tabulka popisuje hello různé poplašný stavy.  

### <a name="alarm-states"></a>Stavy výstrahy
| Stav výstrahy | Akce | Akce s ztlumení stisknutí tlačítka |
| --- | --- | --- |
| S0 |Normálním režimu: tichou |Zvukový signál dvakrát |
| S1 |Režim selhání: 1 sekunda na/1 sekundu vypnuto |Přechod tooS2 nebo S3 (viz poznámky) |
| S2 |Připomenout režim: přerušované zvukový signál |Žádný |
| S3 |Muted režim: tichou |Žádný |
| S4 |Kritické chyby režimu: průběžné výstrahy |Není k dispozici: ztlumení není aktivní |

> [!NOTE]
> * Ve stavu výstrahy S1 Pokud není stisknete ztlumení během 2 minut, stav hello automaticky přejde tooS2 nebo S3.  
> * Stavy alarmů S1 tooS4 vrátí tooS0 po podmínky selhání hello se vymaže.  
> * Kritické chyby stavu S4 lze zadat z jiných stavu.  


Zvukové poplašné hello můžete ztlumení stisknutím hello ztlumení tlačítka v panelech ops hello. Automatické ztlumení bude po dvou minutách dojít, pokud není vypínač zvuku hello provozovat ručně. Pokud je ztlumen hello alarmů, bude pokračovat toosound s tooindicate krátké přerušované signálů, který stále existuje problém. Hello alarmů bude tichou, když jsou všechny problémy hello odstraněny.

Hello následující tabulka popisuje hello různé poplašný podmínky.

### <a name="alarm-conditions"></a>Podmínky výstrahy
| Status | Závažnost | Výstrahy | OPS panelu DIODU |
| --- | --- | --- | --- |
| Výstraha PCM – výpadku napájení řadiče domény z jedné PCM |Selhání – bez ztráty redundance |S1 |Selhání modulu |
| Výstraha PCM – výpadku napájení řadiče domény z jedné PCM |Selhání – ztráta redundance |S1 |Selhání modulu |
| Ventilátor selhání PCM |Selhání – ztráta redundance |S1 |Selhání modulu |
| SBB modulu bylo zjištěno selhání PCM |Selhání |S1 |Selhání modulu |
| Odebrat PCM |Chyba konfigurace. |Žádný |Selhání modulu |
| Chyba konfigurace skříň |Selhání – kritická |S1 |Selhání modulu |
| Teplotní nízkou upozornění |Upozornění |S1 |Selhání modulu |
| Vysoké teploty upozornění |Upozornění |S1 |Selhání modulu |
| Přes teplotní alarm |Selhání – kritická |S1 |Selhání modulu |
| Selhání sběrnice I2C |Selhání – ztráta redundance |S1 |Selhání modulu |
| OPS panelu došlo k chybě komunikace (I2C) |Selhání – kritická |S1 |Selhání modulu |
| Chyba řadiče |Selhání – kritická |S1 |Selhání modulu |
| Selhání modulu SBB rozhraní |Selhání – kritická |S1 |Selhání modulu |
| Selhání modulu SBB rozhraní – bez funkční moduly zbývající |Selhání – kritická |S4 |Selhání modulu |
| Odebere modul SBB rozhraní |Upozornění |Žádný |Selhání modulu |
| Jednotka selhání řízení spotřeby |Upozornění – bez výpadku napájení jednotky |S1 |Selhání modulu |
| Jednotka selhání řízení spotřeby |Selhání – kritická; výpadku napájení jednotky |S1 |Selhání modulu |
| Jednotka byla odebrána. |Upozornění |Žádný |Selhání modulu |
| Nedostatečné napájení, které jsou k dispozici |Upozornění |None |Selhání modulu |

## <a name="next-steps"></a>Další kroky
Další informace o [StorSimple hardwarové součásti a stav](storsimple-8000-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png


