---
title: "aaaData případ použití objektu pro vytváření - doporučení produktů"
description: "Další informace o případ použití implementovaná pomocí Azure Data Factory spolu s jinými službami."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6f1523c7-46c3-4b8d-9ed6-b847ae5ec4ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: d7912965fe4762d64e8ca3c28381ea6187f36631
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-case---product-recommendations"></a>Příklad použití – doporučení produktu
Azure Data Factory je jedním z mnoha služeb používaných tooimplement hello Cortana Intelligence Suite akcelerátorů řešení.  V tématu [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics) stránku Podrobnosti o této sady. V tomto dokumentu jsme popisují běžné případ použití, který Azure uživatelé už vyřeší a implementovaná pomocí Azure Data Factory a dalším službám součásti Cortana Intelligence.

## <a name="scenario"></a>Scénář
Prodejci běžně má tooentice jejich produkty toopurchase zákazníkům prezentací s produkty, které jsou pravděpodobně toobe zajímají a proto pravděpodobně toobuy. tooaccomplish, prodejci potřebovat toocustomize online možnosti pro své uživatele pomocí přizpůsobené produktu doporučení pro tento konkrétní uživatel. Tato přizpůsobené doporučení jsou toobe provedené na základě jejich aktuálním a historickém nákupní chování data, informace o produktu, nově zavedené značek a produktu a k zákaznickým segmentace data.  Kromě toho můžete poskytují doporučení produktů hello uživatele na základě analýzy chování celkové využití z jejich uživatelé kombinaci.

Cílem těchto prodejců Hello je toooptimize pro uživatele klikněte na tlačítko Prodej převody a vám vyšší výnosy prodeje.  Se zavedením kontextové, na základě chování produktu doporučení na základě zájmů zákazníka a akce dosáhnout tento převod. Pro tento případ použití používáme prodejci jako příklad firmám, které chcete toooptimize zákazníkům. Tyto zásady však použít obchodní tooany, který chce tooengage svým zákazníkům kolem jeho zboží a služeb a zajištění lepších možností nákupní svým zákazníkům s doporučeními přizpůsobené produktu.

## <a name="challenges"></a>Problémy
Existuje mnoho problémů s že vzhled prodejci při tooimplement tento typ případ použití. 

Nejdřív musíte data různých tvarů a obrazců konzumaci z více zdrojů dat, jak místně a v cloudu hello. Tato data zahrnují data produktu, data o chování historických zákazníka a data uživatele jako hello uživatel prochází hello online prodejní lokality. 

Doporučení produktů druhou, přizpůsobené musí být přiměřené a přesně vypočítat a předpovědět. Kromě toho tooproduct, značky a chování a prohlížeč data zákazníků, prodejci také potřebovat tooinclude názory zákazníků na posledních toofactor nákupy v hello stanovení hello nejlepší doporučení produktů pro uživatele hello. 

Doporučení hello třetí, musí být okamžitě dodávky toohello uživatele tooprovide plynulé procházení a nákup prostředí a poskytovat doporučení hello nejnovější a relevantní. 

Nakonec prodejců potřebovat toomeasure hello účinnosti jejich přístup pomocí funkce sledování celkového až prodává cross prodává klikněte na převod prodeje úspěchy a upravit tootheir budoucí doporučení.

## <a name="solution-overview"></a>Přehled řešení
Tento případ použití příklad je vyřešeno a implementované skutečných Azure uživatele pomocí Azure Data Factory a dalším službám součásti Cortana Intelligence, včetně [HDInsight](https://azure.microsoft.com/services/hdinsight/) a [Power BI](https://powerbi.microsoft.com/).

online prodejce Hello používá úložiště objektů Blob v Azure, místní SQL server, databázi SQL Azure a relační datové Tržiště jako jejich možnosti ukládání dat v rámci pracovního postupu hello.  úložiště objektů blob Hello obsahuje informace o zákazníkovi, data o chování zákazníků a data informace o produktu. informace o produktu Hello, že data zahrnují informace o produktu značky a katalog produktů uložené místně v SQL data warehouse. 

Všechna data hello je kombinaci a předány do produktu doporučení systému toodeliver přizpůsobené doporučení na základě zájmů zákazníka a akcí, zatímco hello uživatel prochází produktů v katalogu hello na webu hello. Zákazníci Hello také zobrazit produkty, které se vztahují toohello produkt, který se prohlížení podle celkové vzorce používání webu, které nejsou tooany související s jedním uživatelem.

![diagram případu použití](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabajty nezpracovaná webové soubory protokolu jsou generovány denně z webu hello online prodejce jako částečně strukturovaných soubory. Hello nezpracovaná webové soubory protokolů a informace o katalogu hello zákazníka a produkt je pravidelně konzumována do Azure Blob storage pomocí služby Data Factory přesun globálně nasazené dat jako službu. Hello nezpracované soubory protokolu pro hello den jsou rozdělena na oddíly (ve rok a měsíc) v úložišti objektů blob pro dlouhodobé uložení.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) je použité toopartition hello nezpracovaných log souborů v hello blob hello požity protokoly úložiště a procesů ve velkém měřítku použití Hive a Pig skriptů. Hello oddílů webové protokoly, data se pak zpracovaná tooextract hello potřeby vstupy pro machine learning doporučení systému toogenerate hello přizpůsobené produktu doporučení.

Hello doporučení systém používá pro machine learning hello v tomto příkladu je otevřeným zdrojem strojového učení platformy doporučení z [Apache Mahout](http://mahout.apache.org/).  Všechny [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) nebo vlastní modelu může být použité toohello scénář.  Hello Mahout model je použité toopredict hello podobnosti mezi položky na webu hello na základě způsobů celkové využití a toogenerate hello přizpůsobené doporučení na základě hello jednotlivé uživatele.

Sady výsledků dotazu hello doporučení přizpůsobené produktu je nakonec přesunutý tooa relační datové tržiště pro použití webu prodejce hello.  Sada výsledků dotazu Hello může také přistupovat přímo z úložiště objektů blob jiná aplikace nebo přesunout tooadditional úložiště pro ostatní příjemce a případy použití.

## <a name="benefits"></a>Výhody
Optimalizace jejich strategie doporučení produktu a zarovnání s obchodními cíli, splní hello řešení hello online prodejce prodeje a marketingu cíle. Kromě toho by byly možné toooperationalize a spravovat pracovní postup doporučení produktu hello efektivní, spolehlivou a cenově způsobem. Hello přístup usnadnit jejich tooupdate jejich modelu a systém doladit jeho účinnost podle hello měr prodeje úspěchů klikněte na převod. Pomocí Azure Data Factory by byly možné tooabandon jejich správa prostředků časově náročná a nákladná ruční cloudu a správu prostředků cloudu tooon vyžádání přesunout. Proto by byly možné toosave čas, peníze a snížit jejich nasazení toosolution čas. Zobrazení rodokmenu dat a provozu služby stavu se stalo snadno toovisualize a Poradce při potížích s hello intuitivní monitorování služby Data Factory a z hello portál Azure k dispozici uživatelské rozhraní pro správu. Své řešení lze nyní naplánováno a spravovat tak, aby spolehlivě vytváří a doručit toousers dokončení dat a dat a zpracování závislosti jsou automaticky spravovány bez lidského zásahu.

Díky této přizpůsobené nákupní rozhraní hello online prodejce vytvořit více produktivní, nestačí, aby zákazník prostředí a proto zvýšit prodeje a celková spokojenost zákazníků.

