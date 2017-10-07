---
title: "aaaConcurrency a úlohy správy v SQL Data Warehouse | Microsoft Docs"
description: "Porozumět souběžnosti a úlohy správy v Azure SQL Data Warehouse pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 08/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: 7f7e77aa687760252aed16573b609817ed9111c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Souběžnost a úlohy správy v SQL Data Warehouse
předvídatelný výkon toodeliver ve velkém měřítku, Microsoft Azure SQL Data Warehouse umožňuje řídit souběžnosti úrovně a přidělení prostředků jako paměti a procesoru stanovení priorit. Tento článek představuje koncepty toohello souběžnosti a úlohy správy, která vysvětluje, jak oba implementována a jak je můžete řídit v datovém skladu. Úlohy správy datového skladu SQL je určený toohelp podporu prostředí s více uživateli. Rozhraní není určeno pro více klientů úlohy.

## <a name="concurrency-limits"></a>Omezení souběžnosti
SQL Data Warehouse umožňuje až too1 024 souběžných připojení. Všechna 1 024 připojení mohou souběžně odesílat dotazy. Toooptimize propustnost, SQL Data Warehouse může však fronty tooensure některé dotazy, že každý dotaz obdrží grant minimální paměti. Služba Řízení front dojde v době provedení dotazu. Řízení front dotazy při může zvýšit souběžnost dosaženo omezení SQL Data Warehouse celková propustnost tím zajistí, že aktivní dotazy získat přístup toocritically potřeba paměťových prostředků.  

Omezení souběžnosti se řídí dvěma konceptů: *souběžných dotazů* a *souběžnosti sloty*. Pro tooexecute dotaz musí provést v rámci omezení souběžnosti hello dotazu a hello souběžnosti slotu přidělení.

* Počet souběžných dotazů jsou hello dotazy na spuštění v hello stejný čas. SQL Data Warehouse podporuje až too32 souběžných dotazů hello větší velikostí DWU.
* Sloty souběžnosti mají při přidělování podle DWU. Každý 100 DWU poskytuje 4 sloty souběžnosti. Například od DW100 přiděluje 4 sloty souběžnosti a DW1000 přiděluje 40. Každý dotaz využívá jednu nebo více souběžnosti sloty, závisí na hello [Třída prostředků](#resource-classes) hello dotazu. Dotazy na spouštění v Třída prostředků smallrc hello zabrat jeden slot souběžnosti. Dotazy spuštěnými v vyšší Třída prostředků využívat další sloty souběžnosti.

Hello následující tabulka popisuje hello limity pro souběžné dotazy a sloty souběžnosti v hello různých velikostí DWU.

### <a name="concurrency-limits"></a>Omezení souběžnosti
| DWU | Maximální počet souběžných dotazů | Přidělené sloty souběžnosti |
|:--- |:---:|:---:|
| OD DW100 |4 |4 |
| DW200 |8 |8 |
| DW300 |12 |12 |
| DW400 |16 |16 |
| DW500 |20 |20 |
| DW600 |24 |24 |
| DW1000 |32 |40 |
| DW1200 |32 |48 |
| DW1500 |32 |60 |
| DW2000 |32 |80 |
| DW3000 |32 |120 |
| DW6000 |32 |240 |

Když je splněna jedna z těchto prahových hodnot, nové dotazy jsou zařazeny do fronty a jsou prováděny na základě ven first-in.  Dokončení dotazy a počet hello dotazy a sloty klesne pod hello omezení, jsou vydávány ve frontě dotazů. 

> [!NOTE]  
> *Vyberte* dotazy na spuštění výhradně na zobrazení dynamické správy (zobrazení dynamické správy) nebo zobrazení katalogu se neřídí žádné omezení souběžnosti hello. Můžete monitorovat hello systém bez ohledu na počet hello dotazy na spuštění v něm.
> 
> 

## <a name="resource-classes"></a>Třídy prostředků
Prostředek třídy pomáhá řídit přidělování paměti a cyklů procesoru zadané tooa dotazu. Můžete přiřadit dva typy prostředků třídy tooa uživatele v podobě hello rolí databáze. Hello dva typy prostředků třídy jsou následující:
1. Dynamické třídy prostředků (**smallrc, mediumrc, largerc, xlargerc**) přidělit proměnné množství paměti v závislosti na hello aktuální DWU. To znamená, že při škálování tooa větší DWU, vaše dotazy automaticky získat více paměti. 
2. Statické třídy prostředků (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) přidělit hello stejné množství paměti, bez ohledu na to hello aktuální DWU (za předpokladu, že hello DWU sám sebe má dostatek paměti). To znamená, že na větší počet Dwu, můžete spouštět další dotazy v každé třídě prostředků současně.

Uživatelé v **smallrc** a **staticrc10** mají menší množství paměti a můžete využít výhod vyšší souběžnosti. Naproti tomu uživatelům přiřazena příliš**xlargerc** nebo **staticrc80** mají velké množství paměti, a proto méně jejich dotazů můžou běžet souběžně.

Ve výchozím nastavení, každý uživatel je členem hello Třída prostředků se malé, **smallrc**. Hello postup `sp_addrolemember` je používá třída prostředků hello tooincrease, a `sp_droprolemember` se používá třída prostředků toodecrease hello. Například tento příkaz by zvýšit hello prostředků třídu loaduser příliš**largerc**:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a>Dotazy, které nerespektují třídy prostředků

Existuje několik typů dotazy, které nejsou nijak přínosné větší přidělení paměti. systém Hello ignoruje jejich přidělení Třída prostředků a vždy spusťte tyto dotazy v třídě malé prostředků hello místo. Pokud tyto dotazy spustit vždy v třída hello malé prostředků, mohou spouštět při souběžnosti sloty jsou přetížena a jejich nebude využívat další slotů, než je potřeba. V tématu [výjimky třídy prostředků](#query-exceptions-to-concurrency-limits) Další informace.

## <a name="details-on-resource-class-assignment"></a>Informace o přiřazení třídy prostředků


Několik další podrobnosti o Třída prostředků:

* *Příkaz ALTER role* oprávnění jsou nutná Třída prostředků hello toochange uživatele.
* I když můžete přidat uživatele tooone nebo více prostředků třídy vyšší hello, dynamické prostředků třídy mají přednost před třídy statické prostředků. To znamená pokud uživatel přiřazený tooboth **mediumrc**(dynamické) a **staticrc80**(statické), **mediumrc** je hello Třída prostředků, která je dodržení.
 * Když se uživatel je přiřazený toomore než jeden prostředek třídy v na konkrétní třídy typ prostředku (více než jeden dynamický prostředků nebo více než jeden statický prostředků třída), je dodržení hello nejvyšší Třída prostředků. To znamená pokud uživatel přiřazený tooboth mediumrc a largerc, je dodržení Třída prostředků vyšší hello (largerc). A pokud uživatel přiřazený tooboth **staticrc20** a **statirc80**, **staticrc80** je přijmout.
* Třída prostředků Hello hello systému administrativní uživatele nelze změnit.

Podrobný příklad najdete v tématu [příklad třídy prostředků uživatele změna](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Přidělení paměti
Existují výhody a nevýhody tooincreasing Třída prostředků uživatele. Zvýšení Třída prostředků pro uživatele, dává přístup toomore paměti, což může znamenat, že dotazy spouštět rychleji jejich dotazů.  Vyšší třídy prostředků však také snížit hello počet souběžných dotazů, které můžou běžet. Toto je hello kompromis mezi přidělování velké objemy paměti tooa jeden dotaz nebo povolením jiné dotazy, které je také třeba přidělení paměti, toorun současně. Pokud jeden uživatel vysoké přidělení paměti pro dotaz, ostatní uživatelé nebudou mít přístup toothat stejné paměti toorun dotazu.

Hello následující tabulka mapuje hello paměti přidělené tooeach distribuční třídou DWU a prostředků.

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a>Přidělení paměti za distribuci pro dynamické prostředků třídy (MB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| OD DW100 |100 |100 |200 |400 |
| DW200 |100 |200 |400 |800 |
| DW300 |100 |200 |400 |800 |
| DW400 |100 |400 |800 |1,600 |
| DW500 |100 |400 |800 |1,600 |
| DW600 |100 |400 |800 |1,600 |
| DW1000 |100 |800 |1,600 |3,200 |
| DW1200 |100 |800 |1,600 |3,200 |
| DW1500 |100 |800 |1,600 |3,200 |
| DW2000 |100 |1,600 |3,200 |6,400 |
| DW3000 |100 |1,600 |3,200 |6,400 |
| DW6000 |100 |3,200 |6,400 |12,800 |

Hello následující tabulka mapuje hello paměti přidělené tooeach distribuční DWU a třída statické prostředků. Všimněte si, že hello vyšší prostředků třídy mají jejich paměti snížit toohonor hello globální DWU omezení.

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a>Přidělení paměti za distribuci pro statické prostředků třídy (MB)
| DWU | staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| OD DW100 |100 |200 |400 |400 |400 |400 |400 |400 |
| DW200 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW300 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW400 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW500 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW600 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW1000 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW1200 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW1500 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW2000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |6,400 |
| DW3000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |6,400 |
| DW6000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |12,800 |

Z hello předcházející tabulce, můžete uvidíte, že dotaz spuštěný na DW2000 v hello **xlargerc** Třída prostředků by měla mít přístup too6, 400 MB paměti v každém z hello 60 distribuované databáze.  V SQL Data Warehouse jsou 60 distribuce. Proto by měl toocalculate hello celkové paměti přidělení pro dotaz v třídě daný prostředek, hello nad hodnotu vynásobeny 60.

### <a name="memory-allocations-system-wide-gb"></a>Paměť přidělení systémové (GB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| OD DW100 |6 |6 |12 |23 |
| DW200 |6 |12 |23 |47 |
| DW300 |6 |12 |23 |47 |
| DW400 |6 |23 |47 |94 |
| DW500 |6 |23 |47 |94 |
| DW600 |6 |23 |47 |94 |
| DW1000 |6 |47 |94 |188 |
| DW1200 |6 |47 |94 |188 |
| DW1500 |6 |47 |94 |188 |
| DW2000 |6 |94 |188 |375 |
| DW3000 |6 |94 |188 |375 |
| DW6000 |6 |188 |375 |750 |

Z této tabulky přidělených systémové paměti, uvidíte, že dotaz spuštěný na DW2000 ve třídě prostředků xlargerc hello je přidělen celkem 375 GB paměti (6 400 MB * 60 distribuce / 1 024 tooconvert tooGB) přes hello celého SQL Data Warehouse.

Hello stejný výpočet platí toostatic třídy prostředků.
 
## <a name="concurrency-slot-consumption"></a>Spotřeba slotu souběžnosti  
SQL Data Warehouse uděluje více paměti tooqueries spuštěné v vyšší třídy prostředků. Velikost paměti je pevné prostředků.  Proto hello více paměti přidělené na dotaz, hello, které můžete provést méně souběžných dotazů. Hello následující tabulka opětovně uvádí, že všechny hello předchozí konceptů v rámci jednoho zobrazení, která zobrazuje počet hello sloty souběžnosti podle DWU a hello sloty spotřebovávají každá třída prostředků.  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a>Přidělení a využití souběžnosti přihrádky pro dynamické prostředků třídy  
| DWU | Maximální počet souběžných dotazů | Přidělené sloty souběžnosti | Sloty používané smallrc | Sloty používané mediumrc | Sloty používané largerc | Sloty používané xlargerc |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| OD DW100 |4 |4 |1 |1 |2 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |
| DW400 |16 |16 |1 |4 |8 |16 |
| DW500 |20 |20 |1 |4 |8 |16 |
| DW600 |24 |24 |1 |4 |8 |16 |
| DW1000 |32 |40 |1 |8 |16 |32 |
| DW1200 |32 |48 |1 |8 |16 |32 |
| DW1500 |32 |60 |1 |8 |16 |32 |
| DW2000 |32 |80 |1 |16 |32 |64 |
| DW3000 |32 |120 |1 |16 |32 |64 |
| DW6000 |32 |240 |1 |32 |64 |128 |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a>Přidělení a využití souběžnosti přihrádky pro statické prostředků třídy  
| DWU | Maximální počet souběžných dotazů | Přidělené sloty souběžnosti |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| OD DW100 |4 |4 |1 |2 |4 |4 |4 |4 |4 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW400 |16 |16 |1 |2 |4 |8 |16 |16 |16 |16 |
| DW500 | 20| 20| 1| 2| 4| 8| 16| 16| 16| 16|
| DW600 | 24| 24| 1| 2| 4| 8| 16| 16| 16| 16|
| DW1000 | 32| 40| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1200 | 32| 48| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1500 | 32| 60| 1| 2| 4| 8| 16| 32| 32| 32|
| DW2000 | 32| 80| 1| 2| 4| 8| 16| 32| 64| 64|
| DW3000 | 32| 120| 1| 2| 4| 8| 16| 32| 64| 64|
| DW6000 | 32| 240| 1| 2| 4| 8| 16| 32| 64| 128|

Z těchto tabulek zobrazí se systémem SQL Data Warehouse jako DW1000 přiděluje maximálně 32 souběžných dotazů a celkem 40 sloty souběžnosti. Pokud všichni uživatelé používají v smallrc, 32 souběžných dotazů by být povolena, protože každý dotaz by používat 1 slot souběžnosti. Pokud všechny uživatele DW1000 byly spuštěny v mediumrc, by se každý dotaz přidělit 800 MB za distribuci pro přidělení celkové paměti 47 GB na jeden dotaz a souběžnosti by uživatelé omezené too5 (40 sloty souběžnosti 8 přihrádek na mediumrc uživatele /).

## <a name="selecting-proper-resource-class"></a>Výběr třídy odpovídající prostředek  
Doporučeným postupem je třída prostředků toopermanently přiřazovat uživatele tooa nemusíte měnit jejich třídy prostředků. Můžete například vytvořit tabulky columnstore tooclustered zatížení kvalitnější indexy při přidělení více paměti. tooensure, objemy paměti toohigher přístup, vytvořte uživatele speciálně pro načítání dat a trvale přiřadit tento uživatel tooa vyšší Třída prostředků.
Existuje několik osvědčených postupů toofollow sem. Jak je uvedeno nahoře, SQL DW podporuje dva druhy třída typy prostředků: prostředků statické třídy a třídy dynamického prostředku.
### <a name="loading-best-practices"></a>Načítání osvědčené postupy
1.  Pokud hello očekávání zatížení s regulární množství dat, třída statické prostředků je vhodné použít. Později při rozšiřování prostředků tooget další výpočetní výkon, hello datového skladu bude mít toorun více souběžných dotazuje out-of-the-box, jako uživatel zatížení hello nespotřebovává více paměti.
2.  Pokud hello očekávání větší zatížení v některých případech, třída dynamické prostředků je vhodné použít. Později, při rozšiřování prostředků tooget další výpočetní výkon, hello zatížení uživatel dostane další paměť out-of-the-box, proto hello zatížení tooperform umožňuje rychlejší.

Hello objemy paměti vyžadované tooprocess efektivně závisí na povaze hello hello tabulka načíst a hello objemu zpracovaných dat.. Načítání dat do tabulek KÚS vyžaduje například, že některé paměti toolet KÚS rowgroups dosáhnout optimality. Další podrobnosti najdete v tématu indexy Columnstore hello - data načítání pokyny.

Jako osvědčený postup doporučujeme vám toouse alespoň 200MB paměti pro zatížení.

### <a name="querying-best-practices"></a>Dotazování osvědčené postupy
Dotazy mají různé požadavky v závislosti na jejich složitost. Zvýšení paměti na jeden dotaz nebo zvýšení hello souběžnosti jsou obě platné způsoby tooaugment celkovou propustnost v závislosti na potřebách hello dotazu.
1.  Pokud hello očekávání regulární a složité dotazy (například toogenerate denní nebo týdenní sestavy) a není nutné tootake výhod souběžnosti, třída dynamické prostředků je vhodné použít. Pokud má systém hello tooprocess další data, vertikálním navýšení kapacity hello datového skladu se proto automaticky pro další paměť toohello uživatele spuštění dotazu hello.
2.  Pokud hello očekávání vzory proměnnou nebo křivka souběžnosti (pro instanci Pokud hello databáze je dotazován prostřednictvím webového uživatelského rozhraní široce dostupné), třída statické prostředků je vhodné použít. Později, při rozšiřování prostředků toodata skladu, hello uživatel přidružený ke třídě hello statické prostředků bude automaticky se možné toorun více souběžných dotazů.

Výběr správné paměti grant podle potřeby hello tohoto dotazu je netriviální, protože závisí na mnoha faktorech, například hello množství dat získaných, hello povahu hello schémata tabulek a různé spojení, výběru a predikáty skupiny. Z hlediska obecné přidělení více paměti umožní dotazy toocomplete rychlejší, ale by snížení hello celkové souběžnosti. Pokud souběžnosti není problém, přepsání přidělování paměti není poškodit. toofine tune propustnost, při různých typů prostředků třídy mohou být vyžadovány.

Můžete použít následující hello uložené procedury toofigure out souběžnosti a paměť grant za Třída prostředků na daný objekt SLO a hello nejbližší nejlepší prostředků třídu pro operace náročné na prostředky KÚS paměti na bez oddílů tabulky KÚS na třídy daného prostředku:

#### <a name="description"></a>Popis:  
Tady je hello účel tuto uloženou proceduru:  
1. Obrázek uživatele toohelp out souběžnosti a paměť grant za Třída prostředků v daném objektu SLO. Uživatel potřebuje tooprovide hodnotu NULL pro schématu a název tabulky pro tento jak je znázorněno v následujícím příkladu hello.  
2. toohelp uživatele zjistěte hello nejbližší nejlepší Třída prostředků pro hello paměti intensed KÚS operací (zatížení, kopie tabulku znovu sestavit index, atd.) na jiný dělenou tabulku KÚS na třídu daného prostředku. Hello uložená procedura používá pro tento toofind schématu tabulky se grant hello požadovanou paměť.

#### <a name="dependencies--restrictions"></a>Závislosti & omezení:
- Tato uložená procedura není požadavek na paměť navrženou toocalculate rozdělena na oddíly KÚS tabulky.    
- Tato uložená procedura neberou požadavek na paměť v úvahu hello vyberte součást funkce CTAS/INSERT-výběr a předpokládá se toobe jednoduché vyberte.
- Tato uložená procedura používá dočasnou tabulku, takže toto lze použít v relaci hello kde byl vytvořen tento uložené procedury.    
- Tato uložená procedura závisí na aktuální nabídky hello (např. konfiguraci hardwaru, konfigurace DMS) a pokud se některé této změny pak tato uložená procedura nebude fungovat správně.  
- Tato uložená procedura závisí na existující nabízený souběžnosti limit a pokud se změní, pak tato uložená procedura nebude fungovat správně.  
- Tato uložená procedura závisí na stávající nabídky Třída prostředků a který změní-li se pak to uložené procedura zamýšlenému není pracovní správně.  

>  [!NOTE]  
>  Pokud se nezobrazují výstupu po spuštění uložené procedury s parametry zadané, pak může být dvěma případy. <br />1. Buď parametr datového skladu obsahuje neplatnou hodnotu SLO <br />2. NEBO pokud byl poskytnut název tabulky neexistují žádné odpovídající Třída prostředků pro operaci KÚS. <br />Například na od DW100, nejvyšší přidělení paměti k dispozici je 400MB a pokud je schéma tabulky široké dostatek toocross hello požadavek 400MB.
      
#### <a name="usage-example"></a>Příklad použití:
Syntaxe:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU:Buď zadejte parametr tooextract hodnotu NULL hello aktuální DWU z hello databázi datového skladu nebo zadejte jakýkoli podporovaný DWU hello tvar 'od DW100.
2. @SCHEMA_NAME:Zadejte název schématu tabulky hello
3. @TABLE_NAME:Zadejte název tabulky zájmu hello

Příklady provádění této uložené procedury:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

By bylo možné vytvořit tabulky1 použít v hello výše příklady, jak je uvedeno níže:  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-hello-stored-procedure-definition"></a>Zde je definice hello uložené procedury:
```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(   @DWU VARCHAR(7),
      @SCHEMA_NAME VARCHAR(128),
       @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for hello current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need toosupply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) toohold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping tootheir corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="query-importance"></a>Význam dotazu
SQL Data Warehouse implementuje třídy prostředků pomocí skupiny úloh. Existují celkem osm zatížení skupin, které řídí chování hello třídy prostředků hello napříč hello různých velikostí DWU. Pro všechny DWU SQL Data Warehouse používá jenom čtyři hello osm skupiny úloh. To dává smysl, protože každé skupiny úlohy je přiřazen tooone čtyři třídy prostředků: smallrc, mediumrc, largerc, nebo xlargerc. Hello význam pochopení skupiny úloh hello je, že některé z těchto skupin úloh jsou nastaveny toohigher *důležitosti*. Důležitost se používá pro procesoru plánování. Dotazy spustit s vysokou důležitostí získají třikrát více cyklů procesoru než ty, které se střední důležitostí. Proto souběžnosti slotu mapování taky určit Priorita procesoru. Pokud je dotaz spotřebovává 16 nebo víc sloty, běží jako vysokou důležitostí.

Hello následující tabulka uvádí hello důležitosti mapování pro každou skupinu úloh.

### <a name="workload-group-mappings-tooconcurrency-slots-and-importance"></a>Zatížení skupiny mapování tooconcurrency sloty a důležitostí
| Skupiny úloh | Mapování slotu souběžnosti | MB / distribuce | Mapování důležitostí |
|:--- |:---:|:---:|:--- |
| SloDWGroupC00 |1 |100 |Střednědobé používání |
| SloDWGroupC01 |2 |200 |Střednědobé používání |
| SloDWGroupC02 |4 |400 |Střednědobé používání |
| SloDWGroupC03 |8 |800 |Střednědobé používání |
| SloDWGroupC04 |16 |1,600 |Vysoký |
| SloDWGroupC05 |32 |3,200 |Vysoký |
| SloDWGroupC06 |64 |6,400 |Vysoký |
| SloDWGroupC07 |128 |12,800 |Vysoký |

Z hello **přidělení a využití souběžnosti přihrádky** grafu, zjistíte, že DW500 používá 1, 4, 8 nebo 16 souběžnosti sloty pro smallrc, mediumrc, largerc a xlargerc, v uvedeném pořadí. Tyto hodnoty lze vyhledat v hello předcházející důležitosti hello toofind grafu pro každou třídu prostředků.

### <a name="dw500-mapping-of-resource-classes-tooimportance"></a>Mapování DW500 tooimportance třídy prostředků
| Třída prostředků | Skupina úlohy | Použít sloty souběžnosti | MB / distribuce | Význam |
|:--- |:--- |:---:|:---:|:--- |
| smallrc |SloDWGroupC00 |1 |100 |Střednědobé používání |
| mediumrc |SloDWGroupC02 |4 |400 |Střednědobé používání |
| largerc |SloDWGroupC03 |8 |800 |Střednědobé používání |
| xlargerc |SloDWGroupC04 |16 |1,600 |Vysoký |
| staticrc10 |SloDWGroupC00 |1 |100 |Střednědobé používání |
| staticrc20 |SloDWGroupC01 |2 |200 |Střednědobé používání |
| staticrc30 |SloDWGroupC02 |4 |400 |Střednědobé používání |
| staticrc40 |SloDWGroupC03 |8 |800 |Střednědobé používání |
| staticrc50 |SloDWGroupC03 |16 |1,600 |Vysoký |
| staticrc60 |SloDWGroupC03 |16 |1,600 |Vysoký |
| staticrc70 |SloDWGroupC03 |16 |1,600 |Vysoký |
| staticrc80 |SloDWGroupC03 |16 |1,600 |Vysoký |

Můžete použít následující toolook dotazu DMV v hello rozdíly v přidělování prostředků paměti podrobně z perspektivy hello správce zdrojů hello nebo tooanalyze aktivní a historického využití skupiny úloh hello při řešení potíží s hello.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                        AS node_type
    ,pn.pdw_node_id                    AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024                AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                    AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                        AS group_max_dop
    ,wg.effective_max_dop                AS group_effective_max_dop
    ,wg.total_request_count                AS group_total_request_count
    ,wg.total_queued_request_count            AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT    pool_name
,        pool_max_mem_MB
,        group_name
,        group_importance
,        (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,        node_name
,        node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,    group_request_max_memory_grant_pcnt
,    group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Dotazy, které respektovat omezení souběžnosti
Třídy prostředků se řídí většina dotazů. Tyto dotazy se musí vejít do hello souběžných dotazů a prahové hodnoty slotu souběžnosti. Uživatel, nelze zvolit tooexclude dotaz z modelu slotu hello souběžnosti.

tooreiterate, hello následující příkazy dodržet prostředků třídy:

* PŘÍKAZ INSERT SELECT
* AKTUALIZACE
* ODSTRANIT
* Vyberte (při dotazování tabulky uživatelů)
* PŘÍKAZ ALTER INDEX OPĚTOVNÉ SESTAVENÍ
* PŘÍKAZ ALTER INDEX REORGANIZE
* PŘÍKAZ ALTER TABLE OPĚTOVNÉ SESTAVENÍ
* VYTVOŘENÍ INDEXU
* VYTVOŘIT CLUSTEROVANÝ INDEX COLUMNSTORE
* VYTVOŘENÍ TABLE AS SELECT (FUNKCE CTAS)
* Načítání dat
* Operace přesunu dat prováděné hello služby přesun dat (DMS)

## <a name="query-exceptions-tooconcurrency-limits"></a>Omezení tooconcurrency výjimky dotazu
Některé dotazy nerespektují hello prostředků třída toowhich hello uživatel je přiřazen. Tyto limity souběžnosti toohello výjimky jsou vytvářeny po hello paměťové prostředky potřebné pro konkrétní příkaz nízká, často vzhledem k tomu, že příkaz hello je operace metadat. cílem Hello tyto výjimky je tooavoid větší přidělení paměti pro dotazy, které je nikdy nebudete potřebovat. V těchto případech přiřadit výchozí hello třída malé prostředků (smallrc) je vždycky použijí bez ohledu na Třída prostředků se skutečné hello toohello uživatele. Například `CREATE LOGIN` bude vždy spuštěn v smallrc. Hello prostředků, které vyžaduje toofulfill této operace jsou velmi nízkou, takže neprovede smysl tooinclude hello dotazu v modelu slotu souběžnosti hello.  Tyto dotazy nejsou také omezena hello 32 uživatele souběžnosti limit, neomezený počet tyto dotazy spustit až toohello omezení relace 1 024 relací.

Následující příkazy Hello nerespektují třídy prostředků:

* Vytvořit nebo VYŘADIT tabulku
* PŘÍKAZ ALTER TABLE... PŘEPÍNAČE, rozdělení nebo sloučit oddíl
* PŘÍKAZ ALTER INDEX ZAKÁZAT
* DROP INDEX
* Vytvoření, aktualizace a použít příkaz DROP STATISTICS
* ZKRÁCENÍ TABULKY
* PŘÍKAZ ALTER AUTORIZACE
* VYTVOŘIT PŘIHLÁŠENÍ
* CREATE, ALTER nebo DROP USER
* CREATE, ALTER nebo VYŘADIT postup
* Vytvořit nebo VYŘADIT zobrazení
* VLOŽENÍ HODNOTY
* Vyberte z systémová zobrazení a zobrazení dynamické správy
* VYSVĚTLUJÍ
* PŘÍKAZ DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <a name="changing-user-resource-class-example"></a>Změnit v příkladu třída prostředků uživatele
1. **Vytvořit přihlášení:** otevřete připojení tooyour **hlavní** databáze na serveru SQL hello hostování vaší databázi SQL Data Warehouse a spusťte následující příkazy hello.
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > Je vhodné toocreate uživatele v hlavní databázi hello pro uživatele Azure SQL Data Warehouse. Vytváření uživatele v předloze umožňuje uživatele toologin, pomocí nástroje, například aplikace SSMS bez zadání názvu databáze.  Umožňuje také jejich toouse hello object explorer tooview všechny databáze na serveru SQL server.  Další informace o vytváření a správě uživatelů najdete v tématu [zabezpečení databáze v SQL Data Warehouse][Secure a database in SQL Data Warehouse].
   > 
   > 
2. **Vytvoření SQL Data Warehouse uživatele:** otevřete připojení toohello **SQL Data Warehouse** databáze a spusťte následující příkaz hello.
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. **Udělení oprávnění:** hello následující příklad uděluje `CONTROL` na hello **SQL Data Warehouse** databáze. `CONTROL`v hello je hello úroveň databáze ekvivalentní role db_owner v systému SQL Server.
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW toonewperson;
    ```
4. **Zvýšit Třída prostředků:** použití hello následující dotaz tooadd roli uživatele tooa vyšší úlohy správy.
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. **Třída prostředků snížit:** použití hello následující dotaz tooremove uživatele z role úlohy správy.
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > Není možné tooremove uživatele z smallrc.
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a>Detekce ve frontě dotazů a jiné zobrazení dynamické správy
Můžete použít hello `sys.dm_pdw_exec_requests` DMV tooidentify dotazy, které jsou čekajících ve frontě souběžnosti. Dotazuje čekání slot souběžnosti bude mít stav **pozastaveno**.

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Role správy úloh lze zobrazit pomocí `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Hello následující dotaz uvádí role, které každý uživatel je přiřazen k.

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse má následující hello počkejte typy:

* **LocalQueriesConcurrencyResourceType**: dotazy, které se nacházejí mimo hello souběžnosti slotu framework. DMV dotazy a systému funkce, jako `SELECT @@VERSION` jsou příklady místní dotazů.
* **UserConcurrencyResourceType**: dotazy, které se nacházejí v hello souběžnosti slotu framework. Dotazy pro koncového uživatele tabulky představují příklady, které byste použili tento typ prostředku.
* **DmsConcurrencyResourceType**: počká vyplývající z operace přesunu dat.
* **BackupConcurrencyResourceType**: Tento čekání označuje, že se záloha databáze. Hello maximální hodnota pro tento typ prostředku je 1. Pokud požadovaly více záloh v hello současně, hello jiné fronty.

Hello `sys.dm_pdw_waits` DMV lze použít toosee prostředky, ke kterým žádost čeká.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,    SESSION_ID()            AS Current_session
,    s.[status]            AS Session_status
,    s.[login_name]
,    s.[query_count]
,    s.[client_id]
,    s.[sql_spid]
,    r.[command]            AS Request_command
,    r.[label]
,    r.[status]            AS Request_status
,    r.[submit_time]
,    r.[start_time]
,    r.[end_compile_time]
,    r.[end_time]
,    DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,    DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,    DATEDIFF(ms,r.[end_compile_time],r.[end_time])        AS Request_execution_time_ms
,    r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID();
```

Hello `sys.dm_pdw_resource_waits` DMV zobrazuje pouze hello prostředků čeká spotřebovávají daný dotaz. Doba čekání prostředků pouze opatření hello času čekáním na prostředky toobe zadat, jak názvem na rozdíl od toosignal čekací dobu, což je čas hello je potřebná pro hello základní SQL servery tooschedule hello dotaz na hello procesoru.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID();
```

Hello `sys.dm_pdw_wait_stats` DMV lze použít pro analýzu Historický trend čeká.

```sql
SELECT    w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Další kroky
Další informace o správě uživatelů a zabezpečení najdete v tématu [zabezpečení databáze v SQL Data Warehouse][Secure a database in SQL Data Warehouse]. Další informace o tom, jak větší třídy prostředků můžete zlepšení kvality indexu columnstore clusteru, najdete v části [znovu sestavit indexy tooimprove segment kvality].

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[znovu sestavit indexy tooimprove segment kvality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
