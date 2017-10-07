---
title: "plánování pro aplikace Service Fabric aaaCapacity | Microsoft Docs"
description: "Popisuje, jak tooidentify hello počet výpočetních uzlů, které jsou potřebné pro aplikace Service Fabric"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: markfuss
editor: 
ms.assetid: 9fa47be0-50a2-4a51-84a5-20992af94bea
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 44a69e9d8ec5efcc43122dc42e8f923ef37378f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="capacity-planning-for-service-fabric-applications"></a>Plánování kapacity pro aplikace Service Fabric
Tento dokument se naučíte, jak tooestimate hello množství prostředků (procesory, paměť RAM, disku úložiště) je nutné toorun aplikace Azure Service Fabric. Je běžné, že vaše požadavky toochange prostředků v čase. Několik prostředků se obvykle vyžadují, jako je vývoj/testování vaší služby a pak vyžadují více prostředků, jak můžete přejít do produkčního prostředí a aplikace zvětšování popularita. Při návrhu vaší aplikace, pečlivě promyslete hello dlouhodobé požadavky a rozhodování, které umožňují vaší služby tooscale toomeet vysoké poptávka.

 Když vytvoříte cluster Service Fabric, je rozhodnout, jaké druhy virtuální počítače (VM) se skládá hello clusteru. Každý virtuální počítač se dodává s omezenou dobu prostředky v podobě hello procesorů (jader a rychlost), šířku pásma sítě, paměť RAM a diskového úložiště. S růstem vaší služby v čase, můžete upgradovat tooVMs, které nabízejí větší prostředky nebo přidejte další tooyour cluster virtuálních počítačů. hello toodo pozdější, můžete musí architektury služby původně, ho můžete využít výhod nové virtuální počítače, které se přidají dynamicky toohello clusteru.

Některé služby spravovat málo toono data v hello vlastních virtuálních počítačů. Plánování pro tyto služby by měla soustředit především na výkon, což znamená, výběr hello kapacity proto vhodné procesorů (jader a rychlost) hello virtuálních počítačů. Kromě toho byste měli zvážit šířky pásma sítě, včetně jak často dochází k síťové přenosy a kolik dat je přenosu. Pokud vaše služba potřebuje tooperform a taky zvýší využití služby, můžete přidat další toohello cluster virtuálních počítačů a požadavků sítě hello Vyrovnávání zatížení v rámci všech virtuálních počítačů hello.

Pro služby, které spravovat velké objemy dat na hello virtuálních počítačů měli primárně na velikosti zaměřit plánování kapacity. Proto měli pečlivě zvážit hello kapacitu paměti RAM a disku úložiště Virtuálního počítače hello. systém správy Hello virtuální paměti v systému Windows umožňuje vypadat RAM tooapplication kód místa na disku. Kromě toho poskytuje modulu runtime Service Fabric hello inteligentní stránkování zachovat data pouze aktivní v paměti a přesunutí toodisk pomaleji přístupná data hello. Aplikace můžou proto používat více paměti, než je fyzicky k dispozici na hello virtuálních počítačů. S víc paměti RAM zvyšuje výkon, jednoduše, vzhledem k tomu, že hello virtuálních počítačů můžete ponechat další diskového úložiště v paměti RAM. Hello vyberete virtuální počítač by měl mít data hello dostatečně velké na to toostore disku, které chcete na hello virtuálních počítačů. Podobně hello virtuální počítač by měl mít dostatek paměti RAM tooprovide vám hello výkonu, které očekáváte. Pokud časem naroste data vaší služby, můžete přidat více virtuálních počítačů toohello clusteru a oddíl hello dat napříč všechny virtuální počítače hello.

## <a name="determine-how-many-nodes-you-need"></a>Určit, kolik uzly, budete potřebovat
Vytváření oddílů služby vám umožní tooscale dat služby. Další informace o vytváření oddílů, najdete v části [dělení Service Fabric](service-fabric-concepts-partitioning.md). Každý oddíl se musí vejít do jednoho virtuálního počítače, ale více oddílů (malé) mohou být umístěny na jeden virtuální počítač. Ano s více oddíly malé vám dává větší flexibilitu, než má několik větší oddíly. kompromis Hello je, že spousta oddíly zvyšuje režii Service Fabric a napříč oddíly nelze provést zpracovaných operací. Je zde také další potenciální síťový provoz Pokud kódu služby často potřebuje tooaccess kusy data, která za provozu v různých oddílů. Při navrhování vaší služby, měli pečlivě zvažte tyto výhody a nevýhody tooarrive na efektivní strategie dělení.

Předpokládejme, že aplikace má jeden stavové služby, která má velikost úložiště očekáváte, že toogrow tooDB_Size GB v roce. Jste ochotni tooadd další aplikace (a oddíly) jako prostředí růstu nad rámec tohoto roku.  Hello replikace faktor (RF), která určuje hello počet replik pro vaše služba ovlivňuje hello celkový DB_Size. Hello celkový DB_Size napříč všechny repliky je hello násobí hodnotou DB_Size Multi-Factor replikace.  Node_Size představuje místa na disku hello/RAM na uzel chcete toouse pro vaši službu. Pro zajištění nejlepšího výkonu by měl hello DB_Size začlenit do paměti mezi hello clusteru a Node_Size, který je kolem hello RAM hello by měl být zvolen virtuální počítač. Přidělí Node_Size, která je větší než hello kapacitu paměti RAM, se spoléhat na hello stránkování poskytované modulu runtime Service Fabric hello. Výkon vašich proto nemusí být optimální, pokud celého datového považuje za toobe aktivní (od pak hello dat je stránkovaného vstup/výstup). Pro mnoho služeb, kde je aktivní pouze část hello dat, je však cenově výhodnější.

Hello počet uzlů, které jsou potřebné pro maximální výkon můžete vypočítat následujícím způsobem:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Účet pro růst
Můžete chtít toocompute hello počet uzlů podle hello DB_Size, které očekáváte, že vaše služba toogrow, kromě toohello DB_Size, které začne s. Potom růst hello počet uzlů s růstem vaší služby, aby nejsou předimenzování hello počet uzlů. Ale hello počet oddílů by měla být založena na hello počet uzlů, které jsou potřebné, pokud používáte služby v maximální růstu.

Ho je dobré toohave některé další počítače, které jsou k dispozici kdykoli tak, aby bylo možné zpracovat všechny neočekávané špičky nebo selhání, (například pokud několika virtuálními počítači přejděte).  Při hello velmi kapacity by měl být určen pomocí vaší očekávané špičky, je výchozí bod tooreserve několik navíc za virtuální počítače (5-10 procent navíc).

Hello předchozí předpokládá jednu stavové služby. Pokud máte více než jeden stavové služby, je třeba tooadd hello DB_Size přidružené hello jiných služeb do rovnice hello. Alternativně můžete vypočítat hello počet uzlů samostatně pro každou stavové služby.  Vaše služba může mít repliky nebo oddíly, které nejsou spárovány. Uvědomte si, že oddíly mohou mít i více dat než jiné. Další informace o vytváření oddílů, najdete v části [dělení článek o osvědčených postupech](service-fabric-concepts-partitioning.md). Hello předchozí rovnice je však oddíl a repliky lhostejné, protože Service Fabric zajistí, že hello repliky jsou rozprostřeny mezi uzly hello optimalizovaným způsobem.

## <a name="use-a-spreadsheet-for-cost-calculation"></a>Používání tabulky pro výpočet nákladů
Nyní ve vzorci hello přidejme některé reálná čísla. [Příklad tabulky](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) ukazuje, jak tooplan hello kapacita pro aplikaci, která obsahuje tři typy datových objektů. Pro každý objekt jsme Přibližná jeho velikost a kolik objekty že Očekáváme, že toohave. Můžeme také vybrat kolik repliky chceme každého typu objektu. v tabulce Hello vypočtena hello celkovou velikost paměti toobe uložené v clusteru hello.

Potom jsme zadejte velikost virtuálního počítače a měsíční náklady. Podle hello velikost virtuálního počítače, hello tabulky informuje, že hello minimální počet oddílů, je nutné použít toosplit podle toophysically vaše data na uzlech hello. Může si přejí větší počet oddílů tooaccommodate, musí aplikace konkrétní výpočetní a síťové přenosy. tabulky Hello zobrazuje hello počet oddílů, které budete spravovat objekty profilu uživatele hello zvýšilo z jednoho toosix.

Nyní založená na všechny tyto informace, hello tabulky zobrazuje může fyzicky získat všechna data hello s oddíly hello potřeby a repliky na 26 uzly clusteru. Však tento cluster by hustě balí, proto je vhodné některé selhání uzlu tooaccommodate další uzly a upgrady. tabulky Hello také ukazuje, že s více než 57 uzly poskytuje žádná další hodnota, protože bude mít prázdný uzly. Znovu můžete chtít toogo výše 57 uzly přesto selhání uzlu tooaccommodate a upgrady. Toomatch hello tabulky, můžete upravit specifické potřeby vaší aplikace.   

![Tabulky pro výpočet nákladů][Image1]

## <a name="next-steps"></a>Další kroky
Podívejte se na [dělení Service Fabric služby] [ 10] toolearn informace o vytváření oddílů služby.

<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-concepts-partitioning.md
