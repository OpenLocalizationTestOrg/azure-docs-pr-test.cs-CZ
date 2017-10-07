---
title: "technických specifikací aaaStorSimple | Microsoft Docs"
description: "Popisuje hello technické údaje a informace o kompatibilitě zákonných norem pro hello StorSimple hardwarové součásti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 17ebf6b3-0872-4332-ac6e-074cc20a2b8e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: bb73661105dee7f6020a91f8c4f5abd6583023ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="technical-specifications-and-compliance-for-hello-storsimple-device"></a>Technických specifikací a dodržování předpisů pro zařízení StorSimple hello
## <a name="overview"></a>Přehled
Hello hardwarové součásti služby Microsoft Azure StorSimple zařízení splňovat zákonné standardy uvedených v tomto článku a toohello technických specifikací. Hello technických specifikací popisují kapacitu úložiště hello napájení a chlazení moduly (PCMs), diskové jednotky a skříně. informace o kompatibilitě Hello popisuje prvků jako mezinárodní standardy, zabezpečení a emisí a kabeláž.

## <a name="power-and-cooling-module-specifications"></a>Specifikace energii a chlazení modulu
zařízení StorSimple Hello má dva, 100 240 v duální ventilátor, kompatibilní se standardem SBB moduly Power chlazení (PCMs). To poskytuje redundantní napájení konfigurace. Pokud se nezdaří PCM, hello zařízení toooperate normálně pokračuje na hello je nahrazeno jiných PCM do hello selhání modulu.  

Hello EBOD skříň používá 580 W PCM a primární skříň 764 W PCM. Hello následující tabulky obsahují seznam hello technických specifikací přidružené hello PCMs.

| Specifikace | 580 W PCM (EBOD) | 764 W PCM (primární) |
| --- | --- | --- |
| Maximální výstupní výkon |580 W |764 |
| frekvence |50/60 Hz |50/60 Hz |
| Výběr rozsahu napětí |Automatické rozsahu: 90 – 264 V AC, 47/63 Hz |Automatické rozsahu: 90-264 V AC, 47/63 Hz |
| Maximální průvalu aktuální |20 A |20 A |
| Korekce faktor výkonu |> 95 % nominální vstupní napětí |> 95 % nominální vstupní napětí |
| Harmonické |Splňuje EN61000-3-2 |Splňuje EN61000-3-2 |
| Výstup |Pohotovostní napětí 5v @ 2.0 A |Pohotovostní napětí 5v @ 2.7 A |
| + 5 42 A |+ 5 40 A | |
| + 12 V @ 38 A |+ 12 V @ 38 A | |
| Horkou modulární |Ano |Ano |
| Přepínače a LED |Přepínač AC samostatnými a indikátor stavu čtyři LED |Přepínač AC samostatnými a indikátor stavu šesti LED |
| Skříň chlazení |Axiální ventilátory s řízením rychlost proměnné ventilátoru |Axiální ventilátory s řízením rychlost proměnné ventilátoru |

## <a name="power-consumption-statistics"></a>Statistické údaje o spotřebě energie
Hello následující tabulka uvádí hello typické spotřebě energie (skutečné hodnoty se liší od hello publikovaná) pro hello různé modely zařízení StorSimple. 

| Podmínky | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC |
| --- | --- | --- | --- | --- | --- | --- |
|  Pomalé, jednotky ventilátory nečinnosti |1,45 A |0.31 kW |1057.76 KJ/h |3.19 A |0.34 kW |1160.13 KJ/h |
|  Pomalé, jednotky ventilátory, přístup k |1.54 A |0.33 kW |1126.01 KJ/h |3.27 A |0.36 kW |1228.37 KJ/h |
|  Používá technologii dvě PSUs ventilátory rychlé, jednotky nečinnosti, |2.14 A |0.49 kW |1671.95 KJ/h |4,99 A |0.54 kW |1842.56 KJ/h |
|  Ventilátory rychlé, jednotky v nečinnosti, jeden PSU zapnuté jeden nečinnosti |2.05 A |0.48 kW |1637.83 KJ/h |4.58 A |0,50 kW |1706.07 KJ/h |
|  Rychlé, jednotky ventilátory přístup, dvě PSUs zapnuté |2.26 A |0,51 kW |1740.19 KJ/h |4.95 A |0.54 kW |1842.56 KJ/h |
|  Rychlé ventilátory, jednotky, které přistupují k, jeden PSU používá technologii jeden nečinnosti |2.14 A |0.49 kW |1671.95 KJ/h |4.81 A |0.53 kW |1808.44 KJ/h |

## <a name="disk-drive-specifications"></a>Specifikace diskovou jednotku
Zařízení StorSimple podporuje až too12 3,5 formuláře Multi-Factor Serial Attached (SCSI SAS) disky. Hello skutečné jednotky se může jednat o kombinaci disků SSD (Solid-State Drive) nebo pevných disků (HDD), v závislosti na konfiguraci produktu hello. Hello 12 disku sloty jsou umístěné v konfiguraci 3 ve 4 před hello skříň. Hello EBOD skříň umožňuje další úložiště pro jiné 12 diskové jednotky. Toto jsou vždy pevné disky.  

## <a name="storage-specifications"></a>Specifikace úložiště
zařízení StorSimple Hello mít směs pevných disků a jednotek SSD pro obě hello 8100 a 8600. Hello celkové kapacity použitelné pro hello 8100 a 8600 se přibližně 15 TB a 38 TB. Následující tabulka Hello dokumenty hello podrobnosti o SSD a HDD, kapacity cloudu v kontextu hello hello kapacity řešení StorSimple.

| Model zařízení / kapacity | 8100 | 8600 |
| --- | --- | --- |
| Počet pevných disků (HDD) |8 |19 |
| Počet disků SSD (Solid-State Drive) |4 |5 |
| Kapacity jednoho HDD |4 TB |4 TB |
| Jeden kapacity SSD |400 GB |800 GB |
| Rezervní kapacity |4 TB |4 TB |
| Použitelné kapacita HDD |14 TB |36 TB |
| Použitelné kapacity SSD |800 GB |2 TB |
| Celkový počet použitelné kapacity * |~ 15 TB |~ 38 TB |
| Řešení maximální kapacity (včetně cloudu) |200 TB |500 TB |

<sup>* </sup>- *Celková kapacita použitelné Hello zahrnuje kapacity hello k dispozici pro data, metadat a vyrovnávací paměti.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Skříň dimenze a specifikace váhy
Následující seznam tabulek Hello hello různé specifikace skříň pro dimenze a váhy.  

### <a name="enclosure-dimensions"></a>Skříň dimenze
Hello následující tabulka uvádí hello rozměry hello skříň v milimetrech a palců.

| Skříň | Milimetry | Palců |
| --- | --- | --- |
| Výška |87.9 |3.46 |
| Šířka napříč příruby připojení |483 |19.02 |
| Šířka napříč textu skříň |443 |17.44 |
| Hloubka z front připojení příruby tooextremity těla skříň |577 |22.72 |
| Hloubka operacích panelu toofurthest okraje skříň |630.5 |24.82 |
| Hloubka z připojení příruby toofurthest okraje skříň |603 |23.74 |

### <a name="enclosure-weight"></a>Váha skříň
V závislosti na konfiguraci hello skříni úplným načtením primární můžete naváží z 21 too33 kg a vyžaduje dvě osoby toohandle ho. 

| Skříň | Váha |
| --- | --- |
| Maximální váhy (závisí na konfiguraci hello) |30 kg – 33 kg |
| Prázdný (namontováno žádné jednotky) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Specifikace prostředí skříň
Tato část uvádí hello specifikace související toohello skříň prostředí. Hello teploty, vlhkosti, výšku, rázu, vibrace, orientaci, zabezpečení a elektromagnetické kompatibility (EMC) jsou zahrnuty v této kategorii.  

### <a name="temperature-and-humidity"></a>Teploty a vlhkosti
| Skříň | O teplotě rozsahu | Vedlejším relativní vlhkost | Maximální živé žárovky |
| --- | --- | --- | --- |
| Provozní |TEPLOTĚ 5-35° C (41° F - 95° F) |20 – 80 % jiný sloučíte- |28° C (82° F) |
| Nefunkční |-TEPLOTĚ 40-70° C (40° F - 158° F) |100 % 5 % - bez sloučíte |29° C (84° F) |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Proud vzduchu, výšku, rázu, vibrace, orientaci, zabezpečení a EMC
| Skříň | Provozní specifikace |
| --- | --- |
| Proud vzduchu |Systém vzduchu je přední toorear. Systém musí provozovat s Nízkotlaké zadní výfukového instalaci. Back přetížení vytvořené dveře do racku a překážek nesmí překročit 5 pascalech (0,5 mm horních měřidla). |
| Výška, provozní |-30 měřidla too3045 měřidla (too10 nohou-100, 000 nohou) s maximální provozní teplota zrušte hodnocení o 5 ° C výše 7000 nohou. |
| Výška, nefunkční |-305 až měřidla too12, 192 měřidla (-1,000 nohou too40 000 nohou) |
| Proudem, provozní |5g 10 ms, půlvlnný sinus |
| Proudem, nefunkční |30g 10 ms, půlvlnný sinus |
| Vibrace provozní |0.21g RMS 5 500 Hz náhodných |
| Vibrace nefunkční |1.04g RMS 2 – 200 Hz náhodných |
| Vibrace, přemístění |3g 2 – 200 Hz sinus |
| Orientace a připojení |19" namontovat do racku (2 EIA jednotky) |
| Které rack |toofit minimální 700 mm (31.50 palců) hloubka stojany kompatibilní s IEC 297 |
| Zabezpečení a schválení |CE a UL EN 61000-3, IEC 61000-3, UL 61000 – 3 |
| EMC |EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Mezinárodní standardy dodržování předpisů
Microsoft Azure StorSimple zařízení splňuje následující mezinárodní standardy hello:  

* CE - EN 60950-1  
* Označení CB sestavy tooIEC 60950-1  
* UL a cUL tooUL 60950-1  

## <a name="safety-compliance"></a>Dodržování předpisů zabezpečení
Zařízení s Microsoft Azure StorSimple splňuje následující bezpečnostní hodnocení hello:  

* Schválení typ produktu System: UL, cUL CE  
* Dodržování předpisů zabezpečení: UL 60950, IEC 60950, EN 60950  

## <a name="emc-compliance"></a>Dodržování předpisů EMC
Zařízení s Microsoft Azure StorSimple splňuje hello následující EMC hodnocení.  

### <a name="emissions"></a>Emisí
Hello zařízení kompatibilní se standardem EMC emisí provedeny a vyzářený úrovně.  

* Provedeny emisí omezení úrovní: část 47 CFR 15B – třída A EN55022 třídy A CISPR třída A  
* Vyzářený emisí omezení úrovní: část 47 CFR 15B – třída A EN55022 třídy A CISPR třída A   

### <a name="harmonics-and-flicker"></a>Harmonické a blikání
Hello zařízení v souladu s EN61000-3-2 nebo 3.  

### <a name="immunity-limit-levels"></a>Omezení úrovně odolnosti
Hello zařízení v souladu s EN55024.  

## <a name="ac-power-cord-compliance"></a>AC power kabel dodržování předpisů
Hello moduly a hello dokončení sestavení kabel power musí splňovat hello standardy, které jsou vhodné pro hello země, ve které hello je používán zařízení a musí mít bezpečnostní schválení, které jsou přípustné v dané země. Hello následující tabulky obsahují seznam standardy hello USA a Evropě.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC napájecích kabelů – USA (musí být NRTL uvedené)
| Komponenta | Specifikace |
| --- | --- |
| Kabel typu |SV nebo SVT 18 minimální AWG, 3 vodičem, maximální délka 2.0 měřidla |
| Moduly |Moduly zemnicím typ přílohy 5-15P NEMA hodnocení 120 V 10 A; nebo IEC 320 C14, 250 V, 10 A |
| Soket |IEC 320 C-13 250 V, 10 A |

### <a name="ac-power-cords---europe"></a>AC napájecích kabelů - Evropa
| Komponenta | Specifikace |
| --- | --- |
| Kabel typu |Harmonizované, H05. VVF 3G1.0 |
| Soket |IEC 320 C-13 250 V, 10 A |

## <a name="supported-network-cables"></a>Podporované síťové kabely
Hello 10 GbE síťová rozhraní DATA 2 a DATA 3, najdete v části toohello [seznam podporovaných síťových kabelů a moduly](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Další kroky
Jste nyní připraven toodeploy zařízení StorSimple ve vašem datovém centru. Další informace najdete v tématu [nasazení místního zařízení](storsimple-deployment-walkthrough-u2.md).  

