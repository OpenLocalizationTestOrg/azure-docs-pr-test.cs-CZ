---
title: "výkon hello aaaTesting cloudové služby | Microsoft Docs"
description: "Testování výkonu hello cloudové služby pomocí sady Visual Studio profiler hello"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7a5501aa-f92c-457c-af9b-92ea50914e24
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 98bd775e6ffcf948e737c5ec26399c81f4770fe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="testing-hello-performance-of-a-cloud-service"></a>Testování výkonu hello cloudové služby
## <a name="overview"></a>Přehled
Výkon hello cloudové služby můžete otestovat v hello následující způsoby:

* Použití Azure Diagnostics toocollect informace o žádosti a připojení a statistiky tooreview lokality, které ukazují, jak služba hello provede z hlediska zákazníka. tooget začít s, najdete v části [konfigurace diagnostiky pro Azure Cloud Services a virtuálních počítačů](http://go.microsoft.com/fwlink/p/?LinkId=623009).
* Použijte hello Visual Studio profiler tooget hloubkovou analýzu hello výpočetní aspektů jak hello služba běží. Podle popisu v tomto tématu, můžete použít hello profileru toomeasure výkonu služby běží v Azure. Informace o tom, jak toouse hello profileru toomeasure výkonu jako služba běží místně v emulátoru výpočetní najdete v tématu [hello testování výkonu služby Azure Cloud Service místně v hello výpočetní pomocí emulátoru hello Visual Studio Profiler ](http://go.microsoft.com/fwlink/p/?LinkId=262845).

## <a name="choosing-a-performance-testing-method"></a>Výběr výkonu testování – metoda
### <a name="use-azure-diagnostics-toocollect"></a>Použijte toocollect Azure Diagnostics:
* Statistika na webové stránky nebo služby, například požadavky a připojení.
* Statistika na rolí, například jak často se restartuje roli.
* Informace o využití paměti, jako je například hello procento času, který hello trvá systém uvolňování paměti nebo hello paměti celkové sada spuštěné roli.

### <a name="use-hello-visual-studio-profiler-to"></a>Pomocí sady Visual Studio profiler hello na:
* Určete, které funkce trvat hello většinu času.
* Měří jak dlouho trvá jednotlivých součástí výpočetně náročné program.
* Porovnejte výkon podrobné sestavy pro dvě verze služby.
* Přidělení paměti podrobněji než hello úroveň přidělení paměti jednotlivých analyzujte.
* Analýza problémů souběžnosti v kódu s více vlákny.

Při použití hello profileru spuštění cloudové služby místně nebo v Azure můžete shromáždit data.

### <a name="collect-profiling-data-locally-to"></a>Shromážděte data profilování místně na:
* Testování výkonu hello součástí cloudové služby, jako je například hello spuštění konkrétního pracovního role, která nevyžaduje realistické simulované zatížení.
* Testování výkonu hello cloudové služby v izolaci za kontrolovaných podmínek.
* Testování výkonu hello cloudové služby předtím, než ho nasadit tooAzure.
* Testování výkonu hello cloudové služby soukromě, bez narušení hello existující nasazení.
* Testování výkonu hello hello služby bez nabíhání poplatků za pro spuštění v Azure.

### <a name="collect-profiling-data-in-azure-to"></a>Data profilování v Azure a shromážděte:
* Testování výkonu hello cloudové služby v simulovaném ani skutečné zatížení.
* Pomocí metody instrumentace hello shromažďování dat o profilování, jak toto téma popisuje později.
* Testování výkonu hello hello služby v hello stejné prostředí jako spuštění hello služby v produkčním prostředí.

Obvykle simulovat zatížení tootest cloudové služby v rámci normální nebo vystavila zátěži podmínky.

## <a name="profiling-a-cloud-service-in-azure"></a>Profilace cloudové služby v Azure
Když publikujete cloudové služby ze sady Visual Studio, můžete profil hello služby a zadejte hello profilace nastavení, které umožňují hello informace, které chcete. Pro každou instanci role spuštění profilování relace. Další informace o tom, jak toopublish služby ze sady Visual Studio, najdete v části [publikování tooan Cloudová služba Azure ze sady Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

toounderstand Další informace o profilace výkonu v sadě Visual Studio, najdete v části [tooPerformance Průvodce začátečníka profilování](https://msdn.microsoft.com/library/azure/ms182372.aspx) a [Analýza výkonu aplikace pomocí nástrojů pro profilaci](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

> [!NOTE]
> Můžete povolit IntelliTrace nebo profilace při publikování cloudové služby. Nelze povolit obě.
> 
> 

### <a name="profiler-collection-methods"></a>Kolekce metod profileru
Můžete použít jinou kolekci metody pro profilování, založené na problémy s výkonem:

* **Vzorkování procesoru** -tato metoda shromažďuje statistiky aplikace, které jsou užitečné pro počáteční analýzy problémy s využitím procesoru. Vzorkování procesoru je hello navrhované metody pro spouštění většina vyšetřování výkonu. Je nízký dopad na hello aplikace, která jsou profilace při shromažďování dat vzorkování procesoru.
* **Instrumentace** -tato metoda shromažďuje podrobné časování data, která je užitečný pro přesně zacílené analýzy a k analýze problémy s výkonem vstupu a výstupu. Metoda instrumentace Hello zaznamenává každou položku, ukončení a volání funkce hello funkcí v modulu během profilace spustit. Tato metoda je užitečná pro shromažďování časování podrobné informace o části kódu a porozumění vlivu hello vstupních a výstupních operací na výkon aplikace. Tato metoda je zakázána pro počítač s operačním systémem 32-bit. Tato možnost je dostupná pouze při spuštění hello cloudové služby v Azure, ne místně v emulátoru služby výpočty hello.
* **Přidělení paměti .NET** -tato metoda shromažďuje údaje o přidělení paměti rozhraní .NET Framework pomocí metoda profilování se vzorkováním hello. Hello shromážděná data zahrnují hello počet a velikost přidělené objektů.
* **Concurrency** -tato metoda shromažďuje data kolizí prostředku a proces a přístup z více vláken provádění data, která jsou užitečné při analýze aplikace Vícevláknová a více procesy. metoda souběžného zpracování Hello shromažďuje data pro každou jednotlivou událost, která blokuje provádění kódu, například když vlákno čeká uzamčena přístup tooan aplikace prostředků toobe vydání. Tato metoda je užitečná pro analýzu vícevláknové aplikace.
* Můžete také povolit **profilace interakce vrstvy**, který poskytuje další informace o časy spuštění hello ADO.NET synchronní volání v funkce víceúrovňových aplikací, které komunikují s jedním nebo více databáze. Můžete shromáždit dat interakce vrstev s žádným z metod profilace hello. Další informace o vytváření profilů interakce vrstvy najdete v tématu [zobrazení interakcí vrstev](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Profilování nastavení konfigurace.
Následující obrázek ukazuje, jak Hello tooconfigure nastavení profilování z dialogové okno publikování aplikaci Azure hello.

![Konfigurace nastavení pro profilaci](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

> [!NOTE]
> tooenable hello **povolit profilace** zaškrtávací políčko, musí mít hello profileru nainstalována na místním počítači hello, že používáte toopublish cloudové služby. Ve výchozím nastavení je hello profileru nainstaluje při instalaci sady Visual Studio.
> 
> 

### <a name="tooconfigure-profiling-settings"></a>tooconfigure profilace nastavení
1. V Průzkumníku řešení otevřete hello místní nabídky pro Azure projekt a zvolte **publikovat**. Podrobné pokyny o tom, najdete v části toopublish Cloudová služba, [publikování cloudu pomocí služby hello nástroje Azure](http://go.microsoft.com/fwlink/p?LinkId=623012).
2. V hello **publikování aplikaci Azure** dialogové okno, pokusit hello **Upřesnit nastavení** kartě.
3. tooenable profilování, vyberte hello **povolit profilace** zaškrtávací políčko.
4. tooconfigure nastavení profilování zvolte hello **nastavení** hypertextový odkaz. Zobrazí se dialogové okno nastavení profilace Hello.
5. Z hello **jaké metody profilace by vám jako toouse** možnost tlačítka, vyberte typ hello profilace, je nutné.
6. toocollect hello profilace sledováním interakce vrstev data, vyberte hello **povolit profilace interakce vrstvy** zaškrtávací políčko.
7. nastavení hello toosave, zvolte hello **OK** tlačítko.
   
    Při publikování této aplikace, tato nastavení jsou použité toocreate hello profilace relace pro každou roli.

## <a name="viewing-profiling-reports"></a>Zobrazení sestavy profilaci
Relace profilování se vytvoří pro každou instanci role v rámci cloudové služby. tooview vaše profilace sestavy každé relace ze sady Visual Studio, můžete zobrazit okno Průzkumníka serveru hello a potom vyberte hello Azure výpočetní uzel tooselect instance role. Potom můžete zobrazit hello sestava profilace, jak ukazuje následující obrázek hello.

![Zobrazit sestavy profilaci z Azure](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="tooview-profiling-reports"></a>tooview sestavy profilaci
1. tooview hello Průzkumníka serveru okno v sadě Visual Studio na hello nabídky panelu zvolte Zobrazení Průzkumníka serveru.
2. Zvolte hello Azure výpočetní uzel a zvolte hello nasazení Azure uzel pro hello cloudové služby, které jste vybrali tooprofile při publikování ze sady Visual Studio.
3. profilace tooview sestav pro instance, zvolte roli hello v hello služby, otevřete hello místní nabídku pro konkrétní instanci a pak **zobrazit sestavu profilace**.
   
    Hello sestavy, soubor .vsp je nyní stáhne z Azure a stav hello hello stažení se zobrazí na hello protokol činnosti Azure. Po dokončení stahování hello hello sestava profilace se zobrazí na kartě v editoru hello pro sadu Visual Studio s názvem <Role name>  *<Instance Number>*  <identifier>.vsp. Zobrazí se souhrn dat pro sestavu hello.
4. toodisplay různá zobrazení hello sestavy, v seznamu hello aktuální zobrazení, vyberte typ hello zobrazení, které chcete. Další informace najdete v tématu [zobrazeních sestav nástrojů pro profilaci](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Další kroky
[Ladění cloudové služby](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Publikování tooan Cloudová služba Azure ze sady Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

