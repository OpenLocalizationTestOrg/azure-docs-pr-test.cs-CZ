---
title: "aaaAutomatically škálování výpočetních uzlů ve fondu Azure Batch | Microsoft Docs"
description: "Povolit automatické škálování na toodynamically fondu cloudu upravit hello počet výpočetních uzlů ve fondu hello."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: multiple
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6d1e0c5d8e0e56e15a4d3588150f2466a689f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Vytvořit automatické škálování vzorec škálování výpočetních uzlů ve fondu služby Batch

Azure Batch může automaticky škálovat fondů na základě parametrů, které definujete. Automatické škálování, uzly tooa fondu Batch dynamicky přidá jako zvýšení požadavky úloh a odebere výpočetní uzly, jak se snížit. Čas a peníze můžete uložit tak, že automaticky upraví hello počet výpočetních uzlů, které používá vaše aplikace Batch. 

Povolit automatické škálování ve fondu výpočetních uzlů tím, že přidružíte ho *vzorec škálování* , které definujete. Hello služba Batch používá hello škálování vzorce toodetermine hello počet výpočetních uzlů, které jsou potřebné tooexecute úlohy. Výpočetní uzly může být vyhrazený uzly nebo [nízkou prioritu uzly](batch-low-pri-vms.md). Batch odpovídá tooservice metriky data, která se shromažďují pravidelně. Na základě těchto dat metriky Batch upraví hello počet výpočetních uzlů ve fondu hello založenou na vzorec a v konfigurovatelných intervalech.

Můžete povolit automatické škálování buď při vytváření fondu, nebo na existující fond. Můžete také změnit existující vzorec ve fondu, který je nakonfigurován pro automatické škálování. Batch umožňuje tooevaluate běží vaše vzorce před přiřazením je stav hello toopools a toomonitor automatické škálování.

Tento článek popisuje hello různými entitami, které tvoří vaše vzorce automatického škálování, včetně proměnné, operátory, operace a funkce. Probereme jak tooobtain různé výpočetní prostředek a úloha metriky v rámci dávky. Můžete použít tyto metriky tooadjust váš fond počet uzlů na základě prostředků využití a úlohy stavu. Potom popisují, jak tooconstruct vzorec a povolit automatické škálování ve fondu pomocí obou hello Batch REST a rozhraní API technologie .NET. Dokončíme nakonec, se několik vzorci příklad.

> [!IMPORTANT]
> Při vytváření účtu Batch, můžete zadat hello [konfigurace účtu](batch-api-basics.md#account), která určuje, zda jsou fondy přidělené v předplatném služby Batch (hello výchozí nastavení) nebo ve vašem předplatném uživatele. Pokud jste vytvořili vašeho účtu Batch s hello výchozí služba Batch konfigurace, váš účet je omezená tooa maximální počet jader, které lze použít ke zpracování. Hello služba Batch škáluje výpočetní uzly pouze až toothat základní omezení. Z tohoto důvodu nemusí hello služba Batch dosáhnout hello cílovým počtem výpočetních uzlů určeného vzorec škálování. V tématu [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md) informace o prohlížení a zvýšení kvóty vašeho účtu.
>
>Pokud jste vytvořili účet s konfigurací hello uživatele předplatného, váš účet sdílí v hello základní kvóta pro předplatné hello. Další informace najdete v tématu [Omezení virtuálních počítačů](../azure-subscription-service-limits.md#virtual-machines-limits) v tématu [Limity, kvóty a omezení předplatného a služeb Azure](../azure-subscription-service-limits.md).
>
>

## <a name="automatic-scaling-formulas"></a>Automatické škálování vzorce
Vzorec automatického škálování je řetězcová hodnota, kterou definujete, který obsahuje jeden nebo více příkazů. Vzorec škálování Hello je přiřazen fondu tooa [autoScaleFormula] [ rest_autoscaleformula] – element (Batch REST) nebo [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] vlastnost (Batch .NET). Hello služba Batch používá vaše vzorce toodetermine hello cílovým počtem výpočetních uzlů ve fondu hello hello další interval zpracování. Hello vzorce řetězec nesmí být delší než 8 KB, může obsahovat až too100 příkazy, které jsou odděleny středníky a může obsahovat konce řádků a komentáře.

Automatické škálování vzorců si můžete představit jako Batch škálování "jazyk." Vzorce příkazy jsou volné formátovaných výrazů, které můžou zahrnovat proměnných definovaných servisu (proměnné definované služba Batch hello) a uživatelem definované proměnné (proměnné, které definujete). Pomocí předdefinovaných typů, operátory a funkce můžou dělat různé operace na tyto hodnoty. Příkaz může například trvat hello následující formulář:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Vzorce obvykle obsahují více příkazů, které provádějí operace na hodnotách, které byly získány v předchozích příkazech. Například nejprve jsme získat hodnotu `variable1`, předejte ji tooa funkce toopopulate `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Zahrňte tyto příkazy vaše vzorce tooarrive škálování na cílovým počtem výpočetních uzlů. Vyhrazené uzlů a uzly nízkou prioritu mít vlastní nastavení cílové tak, aby můžete definovat cíl pro každý typ uzlu. Vzorec škálování může zahrnovat cílovou hodnotu pro vyhrazené uzly a cílovou hodnotu pro uzly nízkou prioritu.

Hello cílový počet uzlů může být vyšší, menší nebo stejná jako aktuální počet uzlů typu ve fondu hello hello hello. Batch vyhodnocuje vzorec škálování fondu v určitých intervalech (viz [automatického škálování intervaly](#automatic-scaling-interval)). Batch upraví hello cílový počet každý typ uzlu v hello fondu toohello číslo, které určuje vzorec škálování v době vyhodnocení hello.

### <a name="sample-autoscale-formula"></a>Vzorec škálování ukázka

Tady je příklad vzorec škálování, který může být upravenou toowork pro většinu scénářů. Hello proměnné `startingNumberOfVMs` a `maxNumberofVMs` v hello může být například vzorec upravenou tooyour potřebám. Tento vzorec škáluje vyhrazené uzly, ale může být upravený tooapply tooscale nízkou prioritu také uzly. 

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

Pomocí tohoto vzorce automatického škálování je původně vytvořen fond hello se jeden virtuální počítač. Hello `$PendingTasks` metrika definuje hello počet úloh, které jsou spuštěné nebo zařazené ve frontě. Vzorec Hello najde hello průměrný počet úkolů čekajících na zpracování v hello posledních 180 sekund a nastaví hello `$TargetDedicatedNodes` proměnná odpovídajícím způsobem. Vzorec Hello zajistí, že hello cílový počet uzlů vyhrazené nikdy nepřesáhne 25 virtuálních počítačů. Nové úkoly jsou předloženy, hello fondu automaticky rozšiřovat. Jako dokončení úkolů virtuální počítače se stanou volné jeden po druhém a vzorec automatického škálování hello zmenšuje hello fondu.

## <a name="variables"></a>Proměnné
Můžete je používat **služby definované** a **uživatelem definované** proměnné ve vzorcích škálování. proměnné definované služby Hello jsou integrované v toohello služby Batch. Některé proměnné definované služby jsou pro čtení a zápis a některé jsou jen pro čtení. Uživatelem definované proměnné jsou proměnné, které definujete. Ve vzorci příklad hello uvedené v předchozí části hello `$TargetDedicatedNodes` a `$PendingTasks` jsou proměnných definovaných servisu. Proměnné `startingNumberOfVMs` a `maxNumberofVMs` jsou uživatelem definované proměnné.

> [!NOTE]
> Proměnné definované služby jsou vždy uvedeny znak dolaru ($). Uživatelem definované proměnné dolaru hello je volitelné.
>
>

Hello následující tabulky ukazují, jak pro čtení a zápis a jen pro čtení proměnné, které jsou definovány hello služby Batch.

Můžete získat a nastavit hodnoty těchto proměnných definovaných servisu hello toomanage hello počet výpočetních uzlů ve fondu:

| Proměnné definované služby pro čtení a zápis | Popis |
| --- | --- |
| $TargetDedicatedNodes |Hello cílový počet vyhrazených výpočetní uzly fondu hello. Hello počet vyhrazených uzlů je zadán jako cíl, protože fond nemusí vždy dosáhnout hello požadovaného počtu uzlů. Například pokud je upraven hello cílový počet vyhrazených uzlů ve zkušební verzi škálování před hello fond dosáhla hello počáteční cíl a potom hello fondu nemusí dosáhnout cílového hello. <br /><br /> Fond na účtu vytvoří s konfigurací služby Batch hello nemusí dosáhnout cíli, pokud cílový hello překročí kvótu uzlu nebo core účtu Batch. Fond na účtu vytvořené pomocí konfigurace odběru uživatele hello nemusí dosáhnout cíli, pokud cílový hello překračuje hello sdílené základní kvóta pro předplatné hello.|
| $TargetLowPriorityNodes |Hello cílový počet nízkou prioritu výpočetní uzly fondu hello. Hello počet uzlů nízkou prioritu je zadán jako cíl, protože fond nemusí vždy dosáhnout hello požadovaného počtu uzlů. Například pokud je upraven hello cílový počet uzlů nízkou prioritu ve zkušební verzi škálování před hello fond dosáhla hello počáteční cíl a potom hello fondu nemusí dosáhnout cílového hello. Fond nemusí také dosáhnout cíli, pokud cílový hello překročí kvótu uzlu nebo core účtu Batch. <br /><br /> Další informace na nízkou prioritu výpočetních uzlů najdete v tématu [použít nízkou prioritu virtuálních počítačů pomocí služby Batch (Preview)](batch-low-pri-vms.md). |
| $NodeDeallocationOption |Hello akce, která nastane, když se odebere z fondu, výpočetních uzlů. Možné hodnoty:<ul><li>**requeue**– okamžitě ukončí úlohy a vloží je zpět ve frontě úloh hello tak, aby se přeplánovat.<li>**Ukončit**– okamžitě ukončí úlohy a odebere je ze fronty úloh hello.<li>**taskcompletion**– čeká aktuálně spuštěných úloh toofinish a pak odebere hello uzel z fondu hello.<li>**retaineddata**– čeká pro všechna hello místní úloh uchovávají data na uzlu toobe hello vyčistit před odebráním hello uzel z fondu hello.</ul> |

Můžete získat hodnotu hello těchto proměnných definovaných servisu toomake úpravy, které jsou založeny na metriky ze služby Batch hello:

| Jen pro čtení služby definované proměnné | Popis |
| --- | --- |
| $CPUPercent |Hello průměrné procento využití procesoru. |
| $WallClockSeconds |Hello počet sekund využívat. |
| $MemoryBytes |Hello průměrný počet MB použít. |
| $DiskBytes |Hello průměrný počet gigabajtů použít na místní disky hello. |
| $DiskReadBytes |Hello počet přečtených bajtů. |
| $DiskWriteBytes |Hello počet zapsaných bajtů. |
| $DiskReadOps |provést Hello počet operací čtení disku. |
| $DiskWriteOps |Hello počet operací zápisu disku provést. |
| $NetworkInBytes |Hello počet přijatých bajtů. |
| $NetworkOutBytes |Hello počet bajtů. |
| $SampleNodeCount |Hello počet výpočetních uzlů. |
| $ActiveTasks |Hello počet úloh, které jsou připravené tooexecute, ale ještě nejsou provádění. počet Hello $ActiveTasks zahrnuje všechny úlohy, které jsou v aktivním stavu hello a jehož závislosti byly splněny. Všechny úlohy, které jsou v aktivním stavu hello, ale nebyly splněny jehož závislosti jsou vyloučeny z hello $ActiveTasks počet.|
| $RunningTasks |Hello počet úloh ve spuštěném stavu. |
| $PendingTasks |Hello součet $ActiveTasks a $RunningTasks. |
| $SucceededTasks |Hello počet úloh, které bylo úspěšně dokončeno. |
| $FailedTasks |Hello počet úloh, které se nezdařilo. |
| $CurrentDedicatedNodes |Hello aktuální počet vyhrazených výpočetních uzlů. |
| $CurrentLowPriorityNodes |Aktuální počet nízkou prioritu Hello výpočetní uzly, včetně všech uzlech, které byly zrušeny. |
| $PreemptedNodeCount | Hello počet uzlů ve fondu hello, které jsou ve stavu, zrušeny. |

> [!TIP]
> Hello jen pro čtení, služby definované proměnné, které jsou uvedené v předchozí tabulce hello jsou *objekty* které tooaccess data související s jednotlivými poskytují různé metody. Další informace najdete v tématu [získat ukázková data](#getsampledata) dále v tomto článku.
>
>

## <a name="types"></a>Typy
Tyto typy jsou podporovány v vzorec:

* Double
* doubleVec
* doubleVecList
* Řetězec
* časové razítko – časové razítko je složené struktura, která obsahuje hello následující členy:

  * Rok
  * měsíc (1-12)
  * den (1-31)
  * den v týdnu (ve formátu hello čísla; příklad: 1 pro pondělí)
  * hodinu (ve 24hodinovém formátu číslo; například 13 znamená 13: 00)
  * minut (00-59)
  * druhý (00-59)
* TimeInterval

  * TimeInterval_Zero
  * TimeInterval_100ns
  * TimeInterval_Microsecond
  * TimeInterval_Millisecond
  * TimeInterval_Second
  * TimeInterval_Minute
  * TimeInterval_Hour
  * TimeInterval_Day
  * TimeInterval_Week
  * TimeInterval_Year

## <a name="operations"></a>Operace
Tyto operace jsou povoleny na hello typy, které jsou uvedeny v předchozí části hello.

| Operace | Podporované operátory | Typ výsledku |
| --- | --- | --- |
| dvojité *operátor* double |+, -, *, / |Double |
| dvojité *operátor* timeinterval |* |TimeInterval |
| doubleVec *operátor* double |+, -, *, / |doubleVec |
| doubleVec *operátor* doubleVec |+, -, *, / |doubleVec |
| TimeInterval *operátor* double |*, / |TimeInterval |
| TimeInterval *operátor* timeinterval |+, - |TimeInterval |
| TimeInterval *operátor* časové razítko |+ |časové razítko |
| časové razítko *operátor* timeinterval |+ |časové razítko |
| časové razítko *operátor* časové razítko |- |TimeInterval |
| *operátor*double |-, ! |Double |
| *operátor*timeinterval |- |TimeInterval |
| dvojité *operátor* double |<, <=, ==, >=, >, != |Double |
| řetězec *operátor* řetězec |<, <=, ==, >=, >, != |Double |
| časové razítko *operátor* časové razítko |<, <=, ==, >=, >, != |Double |
| TimeInterval *operátor* timeinterval |<, <=, ==, >=, >, != |Double |
| dvojité *operátor* double |&&, &#124;&#124; |Double |

Při testování dvojitou s Ternární operátor (`double ? statement1 : statement2`), nenulové hodnoty je **true**, a je nula **false**.

## <a name="functions"></a>Funkce
Tyto předdefinované **funkce** jsou k dispozici pro toouse k definování vzorec automatické škálování.

| Funkce | Návratový typ | Popis |
| --- | --- | --- |
| AVG(doubleVecList) |Double |Vrátí hello průměrnou hodnotu pro všechny hodnoty v hello doubleVecList. |
| Len(doubleVecList) |Double |Vrátí hello délka hello vektor, který je vytvořený z hello doubleVecList. |
| LG(Double) |Double |Vrátí hodnotu hello protokolu základní 2 hello double. |
| LG(doubleVecList) |doubleVec |Vrátí základní 2 hello doubleVecList hello component-wise protokolu. Vec(double) musí být explicitně předán parametru hello. Předpokládá se, jinak hodnota hello dvojité lg(double) verze. |
| ln(Double) |Double |Vrátí hello přirozené protokolu hello double. |
| ln(doubleVecList) |doubleVec |Vrátí základní 2 hello doubleVecList hello component-wise protokolu. Vec(double) musí být explicitně předán parametru hello. Předpokládá se, jinak hodnota hello dvojité lg(double) verze. |
| log(Double) |Double |Vrátí hodnotu hello protokolu základní 10 hello double. |
| log(doubleVecList) |doubleVec |Vrátí hello component-wise protokolu základní 10 hello doubleVecList. Vec(double) musí být explicitně předán jeden dvojité parametru hello. Předpokládá se, jinak hodnota hello dvojité log(double) verze. |
| Max(doubleVecList) |Double |Vrátí hello maximální hodnota v hello doubleVecList. |
| min(doubleVecList) |Double |Vrátí hello minimální hodnota v hello doubleVecList. |
| Norm(doubleVecList) |Double |Vrátí hello dva norm hello vektor, který je vytvořený z hello doubleVecList. |
| percentil (doubleVec v dvojité p) |Double |Vrátí hello percentilu element hello vektoru v. |
| rand() |Double |Vrátí náhodná hodnota mezi 0,0 a 1,0. |
| Range(doubleVecList) |Double |Vrátí hello rozdíl mezi hello minimální a maximální hodnoty v hello doubleVecList. |
| STD(doubleVecList) |Double |Vrátí hello ukázka směrodatnou odchylku hodnot hello v hello doubleVecList. |
| stop() | |Zastaví vyhodnocování výrazu hello automatické škálování. |
| SUM(doubleVecList) |Double |Vrátí hello součet všech součástí hello hello doubleVecList. |
| čas (řetězce data a času = "") |časové razítko |Vrátí hello časového razítka hello aktuální čas, pokud žádné parametry jsou předávány nebo hello časové razítko hello řetězce data a času, je-li předán. Data a času podporované formáty jsou W3C DTF a RFC 1123. |
| Val (doubleVec v dvojité i) |Double |Vrátí hodnotu hello hello element, který je v umístění i u funkce vector v, počáteční index nula. |

Seznam některých hello funkcí, které jsou popsané v předchozí tabulce hello může přijmout jako argument. seznam oddělený čárkami Hello je libovolnou kombinaci *dvojité* a *doubleVec*. Například:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

Hello *doubleVecList* hodnota je převeden tooa jeden *doubleVec* před vyhodnocení. Například pokud `v = [1,2,3]`, pak volání `avg(v)` je ekvivalentní toocalling `avg(1,2,3)`. Volání metody `avg(v, 7)` je ekvivalentní toocalling `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Získat ukázková data
Vzorce automatického škálování fungují na data metriky (Ukázky), která zajišťuje hello služby Batch. Vzorec zvětšováním nebo zmenšováním velikost fondu na základě hello hodnot, které se získá od služby hello. Hello služby-proměnných definovaných popsané dříve jsou objekty, které poskytují různé metody tooaccess dat, který je přidružený tento objekt. Například hello následující výraz ukazuje žádost tooget hello posledních pět minut využití procesoru:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Metoda | Popis |
| --- | --- |
| GetSample() |Hello `GetSample()` metoda vrátí Vektor vzorků dat.<br/><br/>Ukázka je 30 sekund vhodné dat metriky. Jinými slovy ukázky jsou získávány každých 30 sekund. Ale dole uvedených položek, dochází ke zpoždění mezi při shromažďování ukázku a kdy je k dispozici tooa vzorec. Ne všechny ukázky pro dané časové období jako takový, může být dostupné pro vyhodnocení podle vzorce.<ul><li>`doubleVec GetSample(double count)`<br/>Určuje počet hello tooobtain ukázky z hello nejnovější vzorků, které byly shromážděny.<br/><br/>`GetSample(1)`vrací posledního vzorku dostupné hello. Pro metriky `$CPUPercent`, ale to nesmí použít, protože je možné tooknow *při* hello ukázka nebyla shromážděna. Může to být poslední, nebo kvůli problémům s systému, může být mnohem starší. Je lepší v takových případech toouse časový interval, jak je uvedeno níže.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Určuje časový rámec pro shromažďování ukázková data. Volitelně můžete také určuje procento hello vzorků, které musí být k dispozici v hello požadovanou časového rámce.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`Vrátí 20 vzorků v případě všechny ukázky pro hello posledních 10 minut se nacházejí v historii CPUPercent hello. Pokud hello poslední minutu historie nebyl k dispozici, ale pouze 18 ukázky by byla vrácena. V takovém případě:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`selže, protože nejsou k dispozici pouze 90 procent hello ukázky.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`mohla být úspěšná.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Určuje časový rámec pro shromažďování dat, s počáteční čas a koncový čas.<br/><br/>Jak je uvedeno nahoře, dochází ke zpoždění mezi při shromažďování ukázku a kdy je k dispozici tooa vzorec. Vezměte v úvahu tato prodleva při použití hello `GetSample` metoda. V tématu `GetSamplePercent` níže. |
| GetSamplePeriod() |Vrátí hello období vzorků, které byly provedeny v datové sadě historických ukázka. |
| Count() |Vrátí hello celkového počtu vzorků v historii metriky hello. |
| HistoryBeginTime() |Vrátí hello časové razítko hello nejstarší dostupných dat ukázky pro metriku hello. |
| GetSamplePercent() |Vrátí hello procento vzorků, které jsou k dispozici v daném časovém intervalu. Například:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Protože hello `GetSample` metoda selže, pokud procento hello vrátil vzorků, které je menší než hello `samplePercent` zadána, můžete použít hello `GetSamplePercent` metoda toocheck první. Poté můžete provádět alternativní akce když dostatek ukázky jsou přítomny, bez zastavení hello automatické škálování vyhodnocení. |

### <a name="samples-sample-percentage-and-hello-getsample-method"></a>Ukázky, ukázka procento a hello *GetSample()* – metoda
Hello základní provoz od vzorce automatického škálování je tooobtain úlohy a prostředku metriky dat a upravte velikost fondu podle data. Jako takový je důležité toohave vědět, jak vzorce automatického škálování pracovat s daty metrik (Ukázky).

**Ukázky**

Hello služba Batch pravidelně využívá ukázky úlohy a prostředku metriky a zajišťuje je k dispozici tooyour vzorce automatického škálování. Tyto ukázky zaznamenávají každých 30 sekund podle hello služby Batch. Je však obvykle zpoždění mezi při nebyly zaznamenány tyto ukázky a když jsou k dispozici příliš (a mohou být přečteny) vzorcích škálování. Kromě toho kvůli toovarious faktorech, například sítě nebo jiných problémů s infrastrukturou, nemusí být ukázky zaznamenány pro konkrétním intervalu.

**Ukázka procento**

Když `samplePercent` je předán toohello `GetSample()` metoda nebo hello `GetSamplePercent()` metoda je volána, _procent_ odkazuje tooa srovnání hello celkový možný počet vzorků, které jsou zaznamenány hello služby Batch a Hello počet vzorků, které jsou k dispozici tooyour vzorec škálování.

Podívejme se na časový interval 10 minut jako příklad. Protože vzorky se zaznamenávají každých 30 sekund v rámci časový interval 10 minut, bude hello maximální celkový počet vzorků, které se zaznamenávají dávkou 20 vzorků (2 za minutu). Ale kvůli toohello vyplývajících latence hello reporting mechanismus a další problémy v rámci Azure, může existovat pouze 15 vzorků, které jsou k dispozici tooyour vzorec škálování pro čtení. Ano například tuto dobu 10 minut 75 % hello celkový počet vzorků, které zaznamenávají může být k dispozici tooyour vzorec.

**GetSample() a ukázkové rozsahů**

Vaše vzorce automatického škálování jsou probíhající toobe zvětšování a zmenšování fondech &mdash; uzly pro přidání nebo odebrání uzlů. Vzhledem k tomu, že uzly náklady peníze, budete chtít tooensure, který vzorcích používat metodu inteligentního analýzy, která je založená na dostatečného množství dat. Proto doporučujeme použít k analýze trendů typu ve vzorcích. Tento typ zvětšování a zmenší fondech na základě rozsahu shromažďovaných vzorků.

Ano, použít toodo `GetSample(interval look-back start, interval look-back end)` tooreturn vektoru vzorků:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Vyhodnocena hello nad řádkem dávkou vrátí oblast ukázky vektor hodnot. Například:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Jakmile jste získány hello vektor ukázky, pak můžete použít funkce jako `min()`, `max()`, a `avg()` tooderive smysluplný hodnoty z hello shromážděných rozsahu.

Pro dodatečné zabezpečení můžete vynutit vzorců toofail nižší než určité procento ukázka je dostupná pro konkrétní časové období. Pokud vynutíte toofail vzorců, požádejte Batch toocease další vyhodnocení hello vzorec Pokud hello zadané procento vzorků, které není k dispozici. V takovém případě je provedena žádná změna toohello velikost fondu. toospecify požadované procento ukázky pro vyhodnocení toosucceed hello, zadejte ji jako hello třetí parametr příliš`GetSample()`. Zde je zadán požadavek 75 procent ukázky:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Protože ukázka dostupnosti může dojít ke zpoždění, je důležité, tooalways zadejte časový interval s časem zahájení vzhled zpět, která jsou starší než jedna minuta. Jak dlouho trvá přibližně jednu minutu pro ukázky toopropagate prostřednictvím systému hello, takže ukázky v rozsahu hello `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` nemusí být k dispozici. Znovu, můžete použít parametr procento hello `GetSample()` tooforce požadavek procento konkrétní vzorek.

> [!IMPORTANT]
> Jsme **důrazně doporučujeme** , které **se spoléhat *pouze* na `GetSample(1)` ve vzorcích škálování**. Důvodem je, že `GetSample(1)` v podstatě uvádí toohello služby Batch, "Udělení mi hello posledního vzorku máte, bez ohledu na dobu načíst." Vzhledem k tomu, že je pouze jeden vzorku a může být starší ukázkové, nemusí být zástupce hello větší přehled o poslední úlohu nebo stav prostředku. Pokud používáte `GetSample(1)`, ujistěte se, že je součástí větší příkazu a není hello pouze datový bod, který vzorec spoléhá na.
>
>

## <a name="metrics"></a>Metriky
Prostředek a úloha metriky můžete použít, když definujete vzorec. Můžete upravit hello cílový počet vyhrazených uzlů ve fondu hello na základě hello metriky dat, který můžete získat a vyhodnocení. V tématu hello [proměnné](#variables) části výše pro další informace o jednotlivé metriky.

<table>
  <tr>
    <th>Metrika</th>
    <th>Popis</th>
  </tr>
  <tr>
    <td><b>Prostředek</b></td>
    <td><p>Metrika prostředků jsou založené na hello procesoru, šířky pásma hello, využití paměti hello výpočetních uzlů a hello počet uzlů.</p>
        <p> Tyto proměnné definované služby jsou užitečné pro provádění úprav na základě počtu uzlu:</p>
    <p><ul>
            <li>$TargetDedicatedNodes</li>
            <li>$TargetLowPriorityNodes</li>
            <li>$CurrentDedicatedNodes</li>
            <li>$CurrentLowPriorityNodes</li>
            <li>$preemptedNodeCount</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Tyto proměnné definované služby jsou užitečné pro provádění úprav na základě využití prostředků uzlu:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Úkol</b></td>
    <td><p>Metriky úloh jsou na základě hello stavu úkolů, například aktivní, čeká na vyřízení a byla dokončena. Hello následující proměnné definované služby jsou užitečné pro provádění úprav velikost fondu na základě metriky úloh:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Zápis vzorec škálování
Sestavení se vzorec škálování vytvořením příkazy, které používají hello výše součásti a potom kombinace těchto příkazů do dokončení vzorce. V této části vytvoříme vzorec škálování příklad, který může provádět některé reálného škálování rozhodnutí.

Nejdříve definujme hello požadavky pro naše nové vzorec škálování. Vzorec Hello proveďte následující kroky:

1. Zvýšit číslo cílového hello vyhrazené výpočetních uzlů ve fondu, pokud využití CPU je vysoké.
2. Hello cílový počet vyhrazených výpočetních uzlů ve fondu snížit, až bude malé využití procesoru.
3. Vždy omezte maximální počet vyhrazených uzlů too400 hello.

tooincrease hello počet uzlů během vysoké využití procesoru, zadejte příkaz hello, které naplňuje uživatelem definované proměnné (`$totalDedicatedNodes`) s hodnotou, která je 110 procent hello aktuální cílový počet vyhrazených uzlů, ale pouze pokud hello minimální průměrné využití procesoru během hello posledních 10 minut byla vyšší než 70 procent. Jinak použijte hello hodnotu pro hello aktuální počet vyhrazených uzlů.

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

příliš*snížit* hello počet vyhrazených uzlů při nízkém zatížení procesoru, hello další příkaz v našich hello vzorce nastaví stejné `$totalDedicatedNodes` proměnné too90 procent hello aktuální cílový počet vyhrazených uzlů Pokud hello průměrné využití procesoru v hello byla za posledních 60 minut v části 20 procent. Jinak použijte hello aktuální hodnota `$totalDedicatedNodes` , jsme vložené do hello příkaz výše.

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

Nyní omezte hello cílový počet vyhrazených výpočetní uzly tooa maximum 400:

```
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

Tady je hello dokončení vzorec:

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

## <a name="create-an-autoscale-enabled-pool-with-net"></a>Vytvoření fondu povoleno škálování s rozhraním .NET

toocreate fond s automatické škálování povolené v rozhraní .NET, postupujte takto:

1. Vytvoření fondu hello s [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).
2. Sada hello [CloudPool.AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) vlastnost příliš`true`.
3. Sada hello [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) vlastnost s vzorec škálování.
4. (Volitelné) Sada hello [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) vlastnosti (výchozí hodnota je 15 minut).
5. Potvrdit hello fond s [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) nebo [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).

Hello následující fragment kódu vytvoří fond škálování povolené v rozhraní .NET. Hello vzorec škálování fondu nastaví hello cílový počet vyhrazených uzlů too5 v pondělí a 1 na každý druhý den v týdnu hello. Hello [interval automatického škálování](#automatic-scaling-interval) je nastavit too30 minut. V tomto a dalších fragmenty C# v tomto článku hello `myBatchClient` je správně inicializována instanci hello [BatchClient] [ net_batchclient] třídy.

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "small", // single-core, 1.75 GB memory, 225 GB disk
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> Když vytvoříte fond povoleno automatické škálování, nezadávejte hello _targetDedicatedComputeNodes_ parametr nebo hello _targetLowPriorityComputeNodes_ parametr na hello volání příliš **CreatePool**. Místo toho zadejte hello **AutoScaleEnabled** a **AutoScaleFormula** vlastnosti fondu hello. Hello hodnoty pro tyto vlastnosti určují hello cílový počet každý typ uzlu. Navíc toomanually změnit velikost fondu škálování povolené (například s [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]), první **zakázat** automatické škálování na Hello fondu a pak jeho velikost.
>
>

Kromě toho tooBatch .NET, můžete použít některý z dalších hello [SDK služby Batch](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch REST](https://docs.microsoft.com/rest/api/batchservice/), [rutiny prostředí PowerShell Batch](batch-powershell-cmdlets-get-started.md)a hello [Batch CLI](batch-cli-get-started.md)tooconfigure automatické škálování.


### <a name="automatic-scaling-interval"></a>Interval automatického škálování
Ve výchozím nastavení služba Batch hello upraví velikost fondu podle tooits vzorec škálování každých 15 minut. Tento interval je možné konfigurovat pomocí hello následující vlastnosti fondu:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API)

minimální interval Hello je pět minut a maximální hello je 168 hodin. Pokud je zadán interval mimo tento rozsah, hello služba Batch vrátí chybu chybný požadavek (400).

> [!NOTE]
> Automatické škálování není aktuálně určený toorespond toochanges za méně než minutu, ale spíše je určena tooadjust hello velikost fondu postupně po spuštění zatížení.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Povolit automatické škálování na existující fond

Každý Batch SDK poskytuje automatické škálování způsob tooenable. Například:

* [BatchClient.PoolOperations.EnableAutoScaleAsync] [ net_enableautoscaleasync] (Batch .NET)
* [Povolit automatické škálování ve fondu] [ rest_enableautoscale] (REST API)

Když povolíte automatické škálování na existující fond, mějte na paměti hello následující body:

* Pokud automatické škálování je aktuálně zakázáno ve fondu hello při vydání hello požadavek tooenable automatické škálování, je nutné zadat platný škálování vzorec Pokud vydáte požadavek hello. Volitelně můžete zadat interval testování škálování. Pokud nezadáte intervalu, je použita výchozí hodnota hello 15 minut.
* Pokud automatického škálování je nyní povolena na hello fondu, můžete vzorec škálování, intervalu vyhodnocení nebo obojí. Musíte zadat alespoň jednu z těchto vlastností.

  * Pokud zadáte nový vyhodnocení interval škálování, je zastavena hello existující plán vyhodnocení a spuštění nový plán. čas zahájení Hello nový plán je hello čas, na které hello byl vydán požadavek tooenable automatické škálování.
  * Pokud vynecháte interval buď hello škálování vzorec nebo vyhodnocení, pokračuje hello služba Batch toouse hello aktuální hodnota tohoto nastavení.

> [!NOTE]
> Pokud jste zadali hodnoty pro hello *targetDedicatedComputeNodes* nebo *targetLowPriorityComputeNodes* parametry hello **CreatePool** metoda při vytváření hello fond v rozhraní .NET, nebo pro hello porovnatelný z hlediska parametrů v jiném jazyce, pak tyto hodnoty se ignorují vyhodnocena hello vzorce automatického škálování.
>
>

Tento fragment kódu jazyka C# používá hello [Batch .NET] [ net_api] automatické škálování tooenable knihovny na existující fond:

```csharp
// Define hello autoscaling formula. This formula sets hello target number of nodes
// too5 on Mondays, and 1 on every other day of hello week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set hello autoscale formula on hello existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Aktualizace se vzorec škálování

Vzorec hello tooupdate na existující škálování povolené fond, volání hello operace automatické škálování tooenable znovu s novou vzorec hello. Například, pokud je již zapnuta automatické škálování `myexistingpool` po provedení hello následující kód .NET se jeho vzorec škálování nahradí obsah hello `myNewFormula`.

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-hello-autoscale-interval"></a>Interval automatického škálování hello aktualizace

tooupdate hello škálování vyhodnocení interval existujícího povoleno škálování fondu, volání hello operace automatické škálování tooenable znovu s novou interval hello. Například tooset hello škálování vyhodnocení too60 minut určený v intervalu pro fond, který je již povolen škálování v rozhraní .NET:

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Vyhodnocení se vzorec škálování

Vzorce můžete vyhodnotit před každým jejím použitím tooa fondu. Tímto způsobem můžete otestovat hello vzorce toosee jak vyhodnotit jeho příkazy před uvedení hello vzorec do produkčního prostředí.

tooevaluate vzorec škálování, musíte nejdřív povolit automatické škálování na fond hello se platný vzorec. povoleno tootest vzorec na fond, který ještě nemá automatické škálování, použijte hello jednořádkové vzorec `$TargetDedicatedNodes = 0` když poprvé povolíte automatické škálování. Pak použijte jednu z následujících vzorec hello tooevaluate chcete tootest hello:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) nebo [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)

    Tyto metody Batch .NET, vyžadují ID hello existující fond a řetězec obsahující tooevaluate vzorce automatického škálování hello.

* [Vyhodnocení vzorec automatické škálování](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    V této žádosti o volání rozhraní REST API, zadejte ID fondu hello v hello URI a hello vzorec škálování v hello *autoScaleFormula* element textu hello žádost. Hello odpověď operace hello obsahuje všechny informace o chybě, který může být související toohello vzorec.

V tomto [Batch .NET] [ net_api] fragment kódu, vyhodnocení se vzorec škálování. Pokud fond hello nemá povoleno automatické škálování, jsme ji povolit nejdřív.

```csharp
// First obtain a reference tooan existing pool
CloudPool pool = await batchClient.PoolOperations.GetPoolAsync("myExistingPool");

// If autoscaling isn't already enabled on hello pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula tooenable autoscaling on the
    // pool. This formula is valid, but won't resize hello pool:
    await pool.EnableAutoScaleAsync(
        autoscaleFormula: "$TargetDedicatedNodes = {pool.CurrentDedicatedNodes};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScaleAsync calls tooonce every 30 seconds.
    // Because we want tooapply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here tooensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh hello properties of hello pool so that we've got the
    // latest value for AutoScaleEnabled
    await pool.RefreshAsync();
}

// We must ensure that autoscaling is enabled on hello pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // hello formula tooevaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform hello autoscale formula evaluation. Note that this code does not
    // actually apply hello formula toohello pool.
    AutoScaleRun eval =
        await batchClient.PoolOperations.EvaluateAutoScaleAsync(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print hello results of hello AutoScaleRun.
        // This will display hello values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply hello formula toohello pool since it evaluated successfully
        await batchClient.PoolOperations.EnableAutoScaleAsync(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output hello message associated with hello error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Úspěšné vyhodnocení hello vzorec tento fragment kódu ukazuje vytváří výsledky podobně jako:

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Získat informace o spuštění automatického škálování

byl očekáván tooensure, který provádí vzorec jako, doporučujeme vám, že pravidelně kontrolovat hello výsledky spustí hello automatické škálování, které Batch provádí fondu. toodo tedy získat (nebo aktualizujte) toohello odkaz na fond a zkontrolujte vlastnosti hello jeho poslední škálování spustit.

V Batch .NET, hello [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) vlastnost má několik vlastností, které obsahují informace o hello nejnovější automatické škálování spustit provádět ve fondu hello:

* [AutoScaleRun.Timestamp](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [AutoScaleRun.Results](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [AutoScaleRun.Error](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

V hello REST API, hello [získat informace o fondu](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) požadavek vrací informace o hello fondu, který zahrnuje hello nejnovější automatické škálování spouštění informace v hello [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool#bk_autrun) vlastnost.

Hello následující fragment kódu jazyka C# používá hello Batch .NET knihovny tooprint informace o hello poslední automatické škálování, spusťte na fond _myPool_:

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Ukázkový výstup hello předcházející fragment kódu:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicatedNodes=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Příklad vzorce automatického škálování
Podívejme se na několik vzorce, které se zobrazí různé způsoby tooadjust hello množství výpočetních prostředků v rámci fondu.

### <a name="example-1-time-based-adjustment"></a>Příklad 1: Založené na čase úpravy
Předpokládejme, že chcete velikost fondu hello tooadjust na základě hello den v týdnu hello a denní dobu. Tento příklad ukazuje, jak tooincrease nebo snížení hello počet uzlů v hello fond odpovídajícím způsobem.

Vzorec Hello nejdřív získá hello aktuální čas. Pokud je jeden den v týdnu (1-5) a v rámci pracovní dobu (too6 8: 00 PM), velikost fondu cíl hello je nastavit too20 uzly. V opačném nastavil too10 uzlů.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Příklad 2: Úprava založený na úlohách
V tomto příkladu se upraví velikost fondu hello podle hello počet úloh ve frontě hello. Komentáře a zalomení řádků jsou přípustné v řetězcích vzorce.

```csharp
// Get pending tasks for hello past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use hello last sample point,
// otherwise we use hello maximum of last sample point and hello history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM toopending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicatedNodes/2);
// hello pool size is capped at 20, if target VM value is more than that, set it
// too20. This value should be adjusted according tooyour use case.
$TargetDedicatedNodes = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Příklad 3: Monitorování účtů pro paralelní úlohy
Tento příklad upraví hello velikost fondu na základě hello počtu úloh. Tento vzorec zohledňuje také účet hello [MaxTasksPerComputeNode] [ net_maxtasks] hodnotu, která je nastavená pro fond hello. Tento přístup je užitečné v situacích, kde [paralelní provádění úkolů](batch-parallel-node-tasks.md) bylo povoleno na váš fond.

```csharp
// Determine whether 70 percent of hello samples have been recorded in hello past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set hello number of nodes tooadd tooone-fourth hello number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set too4, adjust this number
// for your use case)
$cores = $TargetDedicatedNodes * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicatedNodes + $extraVMs);
// Attempt toogrow hello number of compute nodes toomatch hello number of active
// tasks, with a maximum of 3
$TargetDedicatedNodes = max(0,min($targetVMs,3));
// Keep hello nodes active until hello tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Příklad 4: Nastavení velikosti počáteční fondu
Tento příklad, že zobrazuje fragment kódu pomocí vzorce automatického škálování, který nastaví kódu jazyka C# hello tooa velikost fondu zadaný počet uzlů na počáteční čas období. Pak se upraví hello velikost fondu na základě počtu hello spuštěná a aktivních úloh po hello počáteční čas období uplynul.

Vzorec Hello v hello následující fragment kódu:

* Nastaví počáteční fondu hello velikost toofour uzlů.
* Nepřizpůsobí hello velikost fondu v rámci hello první 10 minut životního cyklu hello fondu.
* Po 10 minutách, získá maximální hodnotu hello hello počet spuštění a aktivní úlohy v rámci hello za posledních 60 minut.
  * Pokud jsou obě hodnoty 0 (což znamená, že žádné úlohy byly spuštěné nebo aktivní v hello posledních 60 minut), je nastavit velikost fondu hello too0.
  * Pokud je buď hodnota větší než nula, nebudou provedeny žádné změny.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicatedNodes = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicatedNodes = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicatedNodes) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Další kroky
* [Maximalizovat využití prostředků Azure Batch výpočetní uzel souběžných úloh](batch-parallel-node-tasks.md) obsahuje podrobnosti o tom, jak můžete spustit více úloh s současně na hello výpočetních uzlů ve fondu. Kromě tooautoscaling, tato funkce může pomoct toolower dobu trvání úlohy pro některé úlohy, ušetřit peníze.
* Pro jiné podpůrná efektivitu Ujistěte se, že vaše dotazy aplikace Batch hello služba Batch v hello většina optimální způsob. V tématu [efektivní dotazování služby Azure Batch hello](batch-efficient-list-queries.md) toolearn jak toolimit hello množství dat, když dotazujete hello stav potenciálně tisíce protne hello přenosová výpočetní uzly nebo úlohy.

[net_api]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[net_batchclient]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient
[net_cloudpool_autoscaleformula]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula
[net_cloudpool_autoscaleevalinterval]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval
[net_enableautoscaleasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.enableautoscaleasync
[net_maxtasks]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.maxtaskspercomputenode
[net_poolops_resizepoolasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.resizepoolasync

[rest_api]: https://docs.microsoft.com/rest/api/batchservice/
[rest_autoscaleformula]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_autoscaleinterval]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_enableautoscale]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
