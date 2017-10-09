---
title: "aaaService správce prostředků infrastruktury Cluster - spřažení | Microsoft Docs"
description: "Přehled konfigurace spřažení pro služby Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 678073e1-d08d-46c4-a811-826e70aba6c4
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7dc9b6d9c18d9d615d39cff7de9d7cba1c040474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfigurace a použití spřažení služby v Service Fabric
Spřažení je ovládací prvek, který je k dispozici především toohelp snadné hello přechod větší monolitický aplikace do cloudu a mikroslužeb world hello. Také slouží jako optimalizace pro zlepšení výkonu hello služeb, i když to může mít vedlejší účinky.

Řekněme, že jste přináší větší aplikace, nebo ten, který právě nebyl navržen s mikroslužeb v paměti, tooService prostředků infrastruktury (nebo všechny distribuovaném prostředí). Tento typ přechodu je běžné. Můžete začít zrušení celá aplikace hello do prostředí hello, balení a ujistěte se, že je plynulý chod. Potom spusťte jej rozdělit do různých menší služeb, že všechny komunikovat tooeach jiné.

Nakonec můžete zjistit, že aplikace hello dochází k problémům. Hello problémů obvykle spadají do jedné z těchto kategorií:

1. Některé součásti X hello monolitický aplikace měla nedokumentovanými závislost na součásti Y a právě převedena tyto součásti na samostatné služby. Vzhledem k tomu, že tyto služby běží v rozdílných uzlech v clusteru hello, jsou poškozená.
2. Tyto součásti komunikovat přes (místní pojmenované kanály | sdílené paměti | soubory na disku) a skutečně potřebují toobe možné toowrite tooa sdílet místní prostředek z důvodů výkonu hned teď. Že pevné závislostí získá později odebral, možná.
3. Všechno, co je v pořádku, ale ukazuje se, že tyto dvě součásti jsou ve skutečnosti chatty a výkonu citlivé. Při jejich přesunu do samostatné služby celkového výkonu aplikace tanked nebo latenci vyšší. V důsledku toho hello celkové aplikace není splnění očekávání.

V těchto případech jsme nechcete, aby toolose naše refaktoringu pracovní a nechcete, aby toogo back toohello monolitu. poslední podmínka Hello může být i žádoucí jako prostý optimalizace. Ale dokud jsme můžete změnit návrh hello součásti toowork přirozeně jako služby (nebo dokud jsme může pomoct vyřešit očekávaný výkon hello nějakým způsobem) vytvoříme tooneed některé smysl polohu.

Jaké toodo? Dobře mohou zkuste zapnete spřažení.

## <a name="how-tooconfigure-affinity"></a>Jak tooconfigure spřažení
tooset až spřažení, můžete definovat relaci spřažení mezi dva různé služby. Si můžete představit spřažení jako "ukazující" jedné služby v jiné a informacemi o tom, "tuto službu spustit pouze kde běží služba." Někdy tooaffinity označujeme jako relaci nadřazený podřízený (kde je bod hello podřízené v nadřazené hello). Spřažení zajišťuje, že hello replik nebo instancí jedné služby budou umístěny v hello stejné uzly jako jiné služby.

```csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

> [!NOTE]
> Služba podřízené jenom účastnit vztahu jeden spřažení. Pokud byste chtěli hello podřízené toobe spřažené tootwo nadřazené služby najednou máte několik možností:
> - Reverse hello relace (mají parentService1 a parentService2, přejděte na aktuální službu podřízené hello), nebo
> - Určete jeden z nadřazené položky hello jako rozbočovač podle konvence a mít všechny služby, přejděte na tuto službu. 
>
> Hello výsledná chování umístění v clusteru hello by měl být hello stejné.
>

## <a name="different-affinity-options"></a>Možnosti různých spřažení
Spřažení je reprezentována prostřednictvím jedním z několika korelace schémata a má dva různé režimy. Hello nejběžnější režim spřažení nazýváme NonAlignedAffinity. V NonAlignedAffinity, hello replik nebo instancí hello různé služby jsou umístěny na hello stejné uzly. Hello jiných režim je AlignedAffinity. Zarovnaný spřažení je užitečný jenom v případě stavové služby. Konfigurace dvou stateful spřažení toohave zarovnán služeb zajišťuje, že základní barvy hello těchto služeb jsou uvedeny na hello stejným uzlům, jako každé jiné. Také způsobuje, že každý pár sekundárních databází pro tyto služby toobe umístit na hello stejné uzly. Je také možné tooconfigure NonAlignedAffinity (přestože méně častých) pro stavové služby. Pro NonAlignedAffinity hello hello dvě stavové služby běžet na různých repliky hello stejným uzlům, ale jejich základní barvy, může dojít v různých uzlech.

<center>
![Režimy spřažení a jejich důsledky][Image1]
</center>

### <a name="best-effort-desired-state"></a>Nejlepší stav úsilí potřeby
Relaci spřažení je nejlepší úsilí. Neposkytuje hello stejné záruky společné umístění nebo spolehlivost, která spuštěná v hello nemá stejné spustitelného souboru procesu. Hello služby v relaci spřažení se podstatně liší entit, které můžou selhat a přesunování nezávisle. Relaci spřažení může také zalomit, i když jsou tyto konce dočasné. Například omezení kapacity může znamenat, že jenom některé z hello služby objekty v relaci spřažení hello vejde na daný uzel. V těchto případech i když je vztah spřažení na místě, se nedá vynutit kvůli toohello jiná omezení. Pokud je možné toodo tak, porušení hello je automaticky opraven později.

### <a name="chains-vs-stars"></a>Řetězy oproti hvězdičkami
Dnes hello správce prostředků clusteru není možné toomodel řetězy spřažení relací. Co to znamená, který je služba, která je podřízená v jednom vztahu spřažení nemůže být nadřízený prvek v jiné relace spřažení. Pokud chcete tento typ vztahu toomodel, máte efektivně toomodel jej jako hvězdu, nikoli řetězec. toomove z řetězu tooa hvězdu, nejspodnějších podřízené hello místo toho by nadřazeným prvkem toohello první podřízený objekt Nadřazený objekt. V závislosti na hello uspořádání vašich služeb může mít toodo několikrát. Pokud není k dispozici žádná fyzická nadřazená služba, bude pravděpodobně toocreate jeden, který slouží jako zástupný znak. V závislosti na požadavcích, můžete také toolook do [skupin aplikací](service-fabric-cluster-resource-manager-application-groups.md).

<center>
![Řetězy vs. Hvězdičky v hello kontextu spřažení relace][Image2]
</center>

Jiné toonote věc o spřažení relace dnes je, že jsou směrovou. To znamená, že daného pravidla spřažení hello pouze vynucuje podřazených hello umístěn nadřazené hello. Nezajišťuje se, že tento nadřazený hello je umístěn v podřízené hello. Je také důležité toonote, který hello vztah spřažení nelze vynikající nebo okamžitě vynutit, protože různé služby s jinou životní cykly a může selhat a přesunout nezávisle. Řekněme například, že hello nadřazený uzel tooanother najednou převezme, protože ho došlo k chybě. Hello správce prostředků clusteru a převzetí služeb při selhání Manager popisovač hello převzetí služeb při selhání nejprve vzhledem k tomu, že zachování hello služby, konzistentní a k dispozici je priorita hello. Po dokončení převzetí služeb při selhání hello vztah spřažení hello je poškozený, ale hello správce prostředků clusteru předpokládá, že všechno, co je vhodná, dokud se oznámení podřazených hello není umístěný hello nadřazený. Tyto řazení kontrol se provádí pravidelně. Další informace o tom, jak hello správce prostředků clusteru vyhodnotí omezení je k dispozici v [v tomto článku](service-fabric-cluster-resource-manager-management-integration.md#constraint-types), a [tato](service-fabric-cluster-resource-manager-balancing.md) komunikuje informace o tom, jak tooconfigure hello cadence, na kterém jsou tato omezení vyhodnocení.   


### <a name="partitioning-support"></a>Vytváření oddílů podpory
Hello poslední věcí toonotice o spřažení je, že spřažení relace nejsou podporované hello nadřazené kde je rozdělena na oddíly. Nakonec může podporovány oddílů nadřazené služby, ale ještě dnes není povoleno.

## <a name="next-steps"></a>Další kroky
- Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)
- toolimit služeb tooa malého počítačů nebo totožný hello zatížení služeb, použijte [skupin aplikací](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png