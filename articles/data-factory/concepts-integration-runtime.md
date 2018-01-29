---
title: "Prostředí Integration Runtime v Azure Data Factory | Dokumentace Microsoftu"
description: "Seznamte se s prostředím Integration Runtime v Azure Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/22/2018
ms.author: shlo
ms.openlocfilehash: 5285cddcb64e44cb61c99cc18f8a66d60cd1c0fc
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="integration-runtime-in-azure-data-factory"></a>Prostředí Integration Runtime v Azure Data Factory
Prostředí Integration Runtime (IR) je výpočetní infrastruktura, kterou Azure Data Factory používá k poskytování následujících funkcí integrace dat v různých síťových prostředích:

- **Přesuny dat**: přesouvání dat mezi úložišti dat ve veřejné síti a úložišti dat v privátní síti (místní nebo virtuální privátní síť). Poskytuje podporu vestavěných konektorů, převodu formátu, mapování sloupců a výkonného a škálovatelného přenosu dat.
- **Odesílání aktivit**: odesílání a monitorování aktivit transformace spuštěných v různých výpočetních službách, jako je Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server a další.
- **Spouštění balíčků služby SSIS**: nativní spouštění balíčků služby SQL Server Integration Services (SSIS) ve spravovaném výpočetním prostředí Azure.


> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-introduction.md).

Aktivita ve službě Data Factory určuje akci, která se má provést. Propojená služba určuje cílové úložiště dat nebo výpočetní službu. Prostředí Integration Runtime představuje spojení mezi aktivitou a propojenými službami.  Na toto prostředí se odkazuje z propojené služby. Poskytuje výpočetní prostředí, ve kterém se příslušná aktivita buď spustí, nebo se z něj odešle.  Tímto způsobem se dá aktivita co nejefektivněji provést v oblasti, která je nejblíž cílovému úložišti dat nebo výpočetní službě, a zároveň vyhovět potřebám zabezpečení a dodržování předpisů.

## <a name="integration-runtime-types"></a>Typy prostředí Integration Runtime
Služba Data Factory nabízí tři typy prostředí Integration Runtime. Z nich si můžete vybrat ten, který nejlépe vyhovuje hledaným funkcím integrace dat a potřebám síťového prostředí.  Jedná se o tyto tři typy:

- Azure
- V místním prostředí
- Azure-SSIS

Následující tabulka obsahuje informace o podpoře funkcí a sítí pro každý typ prostředí Integration Runtime:

Typ prostředí IR | Veřejná síť | Privátní síť
------- | -------------- | ---------------
Azure | Přesuny dat<br/>Odesílání aktivit | &nbsp;
V místním prostředí | Přesuny dat<br/>Odesílání aktivit | Přesuny dat<br/>Odesílání aktivit
Azure-SSIS | Spouštění balíčků služby SSIS | Spouštění balíčků služby SSIS

Následující diagram znázorňuje, jak se dají různá prostředí Integration Runtime používat v kombinaci, aby nabízela bohaté funkce integrace dat a podporu sítí:

![Různé typy prostředí Integration Runtime](media\concepts-integration-runtime\different-integration-runtimes.png)


## <a name="azure-integration-runtime"></a>Prostředí Azure Integration Runtime
Prostředí Azure Integration Runtime nabízí tyto funkce:

- Spouštění aktivity kopírování mezi úložišti dat v cloudu.
- Odesílání následujících aktivit transformace ve veřejné síti: aktivita Hive služby HDInsight, aktivita Pig služby HDInsight, aktivita MapReduce služby HDInsight, aktivita Spark služby HDInsight, aktivita streamování služby HDInsight, aktivita provedení dávky služby Machine Learning, aktivity aktualizace prostředku služby Machine Learning, aktivita uložené procedury, aktivita U-SQL služby Data Lake Analytics, vlastní aktivita rozhraní .Net, webová aktivita, aktivita vyhledávání a aktivita načtení metadat.

### <a name="network-environment"></a>Síťové prostředí
Prostředí Azure Integration Runtime podporuje připojení k úložištím dat a výpočetním službám ve veřejné síti s veřejně přístupnými koncovými body. Pro prostředí Azure Virtual Network používejte prostředí Integration Runtime v místním prostředí.

### <a name="compute-resource-and-scaling"></a>Výpočetní prostředky a škálování
Prostředí Azure Integration Runtime poskytuje v rámci Azure plně spravované výpočetní prostředí bez serveru.  Nemusíte si dělat starosti se zřizováním infrastruktury, instalací softwaru, opravami ani škálováním kapacity.  Kromě toho platíte jenom po dobu, kdy prostředí opravdu používáte.

Prostředí Azure Integration Runtime poskytuje nativní výpočetní prostředí pro zabezpečené, spolehlivé a vysoce výkonné přesuny dat mezi cloudovými úložišti dat.  Můžete nastavit počet jednotek přesunu dat, který se má použít v aktivitě kopírování, a velikost výpočetního prostředí Azure IR se elasticky vertikálně navýší, aniž byste museli velikost prostředí Azure Integration Runtime explicitně upravovat.

Odesílání aktivit je nenáročná operace, která zajišťuje směrování aktivity do cílové výpočetní služby, takže pro tento scénář není nutné vertikálně navyšovat kapacitu výpočetního prostředí.

Informace o vytváření a konfiguraci prostředí najdete v návodu k vytvoření a konfiguraci prostředí Azure IR mezi příručkami s postupy. 

## <a name="self-hosted-integration-runtime"></a>Prostředí Integration Runtime v místním prostředí
Prostředí IR v místním prostředí nabízí tyto funkce:

- Spouštění aktivity kopírování mezi úložišti dat v cloudu a úložištěm dat v privátní síti.
- Odesílání následujících aktivit transformace ve výpočetních prostředcích v místní virtuální síti nebo virtuální síti Azure: aktivita Hive služby HDInsight (BYOC), aktivita Pig služby HDInsight (BYOC), aktivita MapReduce služby HDInsight (BYOC), aktivita Spark služby HDInsight (BYOC), aktivita streamování služby HDInsight (BYOC), aktivita provedení dávky služby Machine Learning, aktivity aktualizace prostředku služby Machine Learning, aktivita uložené procedury, aktivita U-SQL služby Data Lake Analytics, vlastní aktivita rozhraní .Net, aktivita vyhledávání a aktivita načtení metadat.

> [!NOTE] 
> Prostředí Integration Runtime v místním prostředí umožňuje podporu úložišť dat, která vyžadují vlastní ovladač, jako je SAP Hana, MySQL atd.  Další informace najdete v tabulce [podporovaných úložišť dat](copy-activity-overview.md#supported-data-stores-and-formats).

### <a name="network-environment"></a>Síťové prostředí
Pokud chcete v prostředí privátní sítě, které nenabízí přímý přístup z prostředí veřejného cloudu, provádět zabezpečenou integraci dat, můžete nainstalovat prostředí IR do místního prostředí za podnikovou bránu firewall nebo v rámci virtuální privátní sítě.  Prostředí Integration Runtime v místním prostředí navazuje jenom odchozí připojení HTTP do otevřeného internetu.

### <a name="compute-resource-and-scaling"></a>Výpočetní prostředky a škálování
Prostředí IR v místním prostředí je potřeba nainstalovat do místního počítače nebo virtuálního počítače v rámci privátní sítě. V současné době podporujeme spouštění prostředí IR v místním prostředí jenom v operačním systému Windows.  

Za účelem vysoké dostupnosti a škálovatelnosti můžete horizontálně navýšit kapacitu prostředí IR v místním prostředí tak, že logickou instanci přidružíte k víc místním počítačům v režimu aktivní-aktivní.  Další informace najdete v článku o vytváření a konfiguraci místního prostředí IR mezi příručkami s postupy.

## <a name="azure-ssis-integration-runtime"></a>Prostředí Azure-SSIS Integration Runtime
Pokud chcete navýšit a přesunout stávající úlohy služby SSIS, můžete vytvořit prostředí Azure SSIS IR pro nativní spouštění balíčků služby SSIS.

### <a name="network-environment"></a>Síťové prostředí
Prostředí Azure-SSIS IR se dá zřídit ve veřejné síti nebo privátní síti.  Připojením prostředí Azure-SSIS IR k virtuální síti připojené k vaší místní síti se dá získat podpora přístupu k místním datům.  

### <a name="compute-resource-and-scaling"></a>Výpočetní prostředky a škálování
Prostředí Azure-SSIS IR je plně spravovaný cluster virtuálních počítačů Azure vyhrazených ke spouštění balíčků služby SSIS. Můžete použít vlastní server Azure SQL Database nebo spravované instance (privátní verze Preview) k hostování katalogu projektů/balíčků služby SSIS (SSISDB), který k němu bude připojený. Můžete vertikálně navýšit výkon výpočetního prostředí tím, že určíte velikost uzlu a pak určíte počet uzlů v clusteru. Prostředí Azure-SSIS Integration Runtime můžete podle libosti zastavovat a spouštět, takže můžete mít pod kontrolou související náklady.

Další informace najdete v článku o vytváření a konfiguraci prostředí Azure-SSIS IR mezi příručkami s postupy.  Po vytvoření můžete existující balíčky služby SSIS nasazovat a spravovat s minimem změn pomocí známých nástrojů, jako jsou SQL Server Data Tools (SSDT) a SQL Server Management Studio (SSMS), stejně jako kdybyste službu SSIS používali místně.

Další informace o modulu runtime Azure-SSIS najdete v následujících článcích: 

- [Kurz: Nasazení balíčků SSIS do Azure](tutorial-deploy-ssis-packages-azure.md) Tento článek obsahuje podrobné pokyny pro vytvoření Azure-SSIS IR a využívá databázi Azure SQL k hostování katalogu SSIS. 
- [Postup: Vytvoření prostředí Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md) Tento článek dál navazuje na tento kurz a obsahuje pokyny k použití spravované instance SQL Azure (privátní verze Preview) a připojení IR k virtuální síti. 
- [Monitorování Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime). Tento článek ukazuje, jak načíst informace o Azure-SSIS IR a popisy stavů ve vrácených informacích. 
- [Správa Azure-SSIS IR](manage-azure-ssis-integration-runtime.md). Tento článek ukazuje, jak zastavit, spustit nebo odebrat Azure-SSIS IR. Ukazuje také postup horizontálního navýšení kapacity Azure-SSIS IR přidáním více uzlů. 
- [Připojení Azure-SSIS IR k virtuální síti](join-azure-ssis-integration-runtime-virtual-network.md) Tento článek obsahuje koncepční informace o připojení Azure-SSIS IR k virtuální síti Azure (VNet). Poskytuje také kroky pro využití webu Azure Portal ke konfiguraci virtuální sítě, aby se k ní prostředí Azure-SSIS IR mohlo připojit. 

## <a name="determining-which-ir-to-use"></a>Určení toho, které prostředí IR používat
Každá aktivita transformace má cílovou výpočetní propojenou službu, která odkazuje na určité prostředí Integration Runtime. Z této instance prostředí Integration Runtime se aktivita transformace odesílá.

V případě aktivity kopírování jsou zapotřebí služby propojené se zdrojem a jímkou, které určují směr toku dat. Pomocí následující logiky se dá určit, která instance prostředí Integration Runtime se ke kopírování používá: 

- **Kopírování mezi dvěma cloudovými zdroji dat**: Pokud prostředí Azure IR používají služby propojené se zdrojem i jímkou, použije se k provedení aktivity kopírování prostředí Integration Runtime používané službou propojenou s jímkou.
- **Kopírování mezi cloudovým zdrojem dat a zdrojem dat v privátní síti**: Pokud služba propojená se zdrojem nebo jímkou odkazuje na prostředí IR v místním prostředí, aktivita kopírování se provádí v prostředí Integration Runtime v místním prostředí.
- **Kopírování mezi dvěma zdroji dat v privátní síti**: Služba propojená se zdrojem i jímkou musí odkazovat na stejnou instanci prostředí Integration Runtime a k provedení aktivity kopírování se použije toto prostředí Integration Runtime.

Následující diagram znázorňuje dvě ukázkové aktivity kopírování:

- V případě aktivity kopírování 1 je zdrojem propojená služba SQL Server odkazující na prostředí IR v místním prostředí A a její jímkou je propojená služba Azure Storage odkazující na prostředí Azure IR B. Když se spustí aktivita kopírování, provede se v prostředí IR v místním prostředí A.
- V případě aktivity kopírování 2 je zdrojem propojená služba Azure SQL Database odkazující na prostředí Azure IR C a její jímkou je propojená služba Azure Storage odkazující na prostředí Azure IR B. Když se spustí aktivita kopírování, provede se v prostředí Azure IR B, protože toto prostředí Integration Runtime používá propojená služba jímky.

![Které prostředí IR použít](media/concepts-integration-runtime/which-integration-runtime-to-use.png)

## <a name="integration-runtime-location"></a>Umístění prostředí Integration Runtime
Do umístění služby Data Factory se ukládají metadata datové továrny a inicializují se z něj spouštění kanálu. V současné době je dostupná podpora těchto umístění služby Data Factory: Východní USA, Východní USA 2 a Západní Evropa. Objekt služby Data Factory nicméně může přistupovat k úložištím dat a výpočetním službám v jiných oblastech Azure za účelem přesouvání dat mezi úložišti dat nebo zpracování dat pomocí výpočetních služeb. Toto chování probíhá prostřednictvím prostředí IR dostupného globálně ve více regionech, aby se zajistilo dodržování předpisů pro data, efektivita a nižší náklady na odchozí přenosy v síti.

Umístění prostředí IR určuje umístění výpočetního prostředí back-end, tedy v podstatě umístění, kde se provádí přesun dat, odesílání aktivit a spouštění balíčku služby SSIS. Umístění prostředí IR se může lišit od umístění, do kterého patří datová továrna. Následující diagram znázorňuje nastavení umístění služby Data Factory a jejích prostředí Integration Runtime:

![Umístění prostředí Integration Runtime](media/concepts-integration-runtime/integration-runtime-location.png)

### <a name="azure-ir"></a>Prostředí Azure IR
Služba Data Factory používá k přesunu dat prostředí Azure IR v oblasti, která má nejblíže k jímce ve stejném zeměpisném regionu. Informace o mapování najdete v této tabulce:

Zeměpisný region úložiště dat jímky | Umístění úložiště dat jímky | Umístění používané prostředím Azure Integration Runtime
-------------------------------| ----------------| ------------------
Spojené státy | Východ USA | Východ USA
&nbsp; | Východní USA 2 | Východní USA 2
&nbsp; | Střed USA | Střed USA
&nbsp; | Střed USA – sever | Střed USA – sever
&nbsp; | Střed USA – jih | Střed USA – jih
&nbsp; | Západní střed USA | Západní střed USA
&nbsp; | Západní USA | Západní USA
&nbsp; | Západní USA 2 | Západní USA 2
Kanada | Východní Kanada | Střední Kanada
&nbsp; | Střední Kanada | Střední Kanada
Brazílie | Brazílie – jih | Brazílie – jih
Evropa | Severní Evropa | Severní Evropa
&nbsp; | Západní Evropa | Západní Evropa
Spojené království | Spojené království – západ | Spojené království – jih
&nbsp; | Spojené království – jih | Spojené království – jih
Asie a Tichomoří | Jihovýchodní Asie | Jihovýchodní Asie
&nbsp; | Východní Asie | Jihovýchodní Asie
Austrálie | Austrálie – východ | Austrálie – východ
&nbsp; | Austrálie – jihovýchod | Austrálie – jihovýchod
Japonsko | Japonsko – východ | Japonsko – východ
&nbsp; | Japonsko – západ | Japonsko – východ
Korea | Korea – střed | Korea – střed
&nbsp; | Korea – jih | Korea – střed
Indie | Střed Indie | Střed Indie
&nbsp; | Indie – západ | Střed Indie
&nbsp; | Indie – jih | Střed Indie

Můžete také nastavit automatický překlad umístění prostředí Azure IR, což znamená, že služba Data Factory se pokusí automaticky zjistit nejlepší umístění na základě definice propojené služby.

> [!NOTE] 
> Pokud se oblast cílového úložiště dat v nenachází v seznamu nebo se nedá zjistit, aktivita se z důvodu dodržování předpisů nezdaří, místo aby prošla přes alternativní oblast. V takovém případě explicitně uveďte alternativní umístění, které se má ke kopírování použít.
 
Následující obrázek znázorňuje příklad efektivního umístění, pokud je u umístění prostředí Azure IR nastavený automatický překlad. Při provádění aktivity kopírování se zjišťuje umístění cíle dat, v tomto případě je to Japonsko – západ.  Podle tabulky se k provedení vlastního kopírování dat použije prostředí Azure IR v oblasti Japonsko – východ. Pokud se stejné prostředí IR použije k připojení ke službě HDInsight pro aktivitu Spark, proběhne odeslání aplikace Spark z umístění služby Data Factory, v tomto případě USA – východ, a vlastní provedení aplikace Spark proběhne v umístění serveru HDInsight. 

![Efektivní umístění](media/concepts-integration-runtime/effective-location.png)

### <a name="self-hosted-ir"></a>Prostředí IR v místním prostředí
Prostředí IR v místním prostředí má logickou registraci ve službě Data Factory a výpočetní prostředí používané k podpoře jeho funkcí poskytujete vy. Proto pro prostředí IR v místním prostředí neexistuje žádná explicitní vlastnost umístění. 

Pokud se prostředí IR v místním prostředí používá k provádění přesunu dat, extrahuje data ze zdroje a zapisuje je do cíle.

### <a name="azure-ssis-ir"></a>Prostředí Azure-SSIS IR
Pro vysoký výkon pracovních postupů extrakce, transformace a načítání (ETL) je velmi důležitý výběr správného umístění prostředí Azure-SSIS IR.  Pro verzi Preview jsou na úvod dostupná dvě umístění (USA – východ a Severní Evropa).

- Umístění vaší služby SSIS-Azure IR nemusí být stejné jako umístění datové továrny, ale mělo by odpovídat umístění vašeho vlastního serveru Azure SQL Database / spravované instance (privátní verze Preview), na kterém chcete hostovat službu SSISDB. Díky tomu může prostředí Azure-SSIS Integration Runtime snadno získat přístup ke službě SSISDB bez nadměrných přenosů dat mezi různými umístěními.
- Pokud nemáte stávající server Azure SQL Database / spravované instance (privátní verze Preview) pro hostování služby SSISDB, ale máte místní zdroje/cíle dat, měli byste vytvořit nový server Azure SQL Database / spravované instance (privátní verze Preview) ve stejném umístění jako virtuální síť připojená k vaší místní síti.  Můžete tak vytvořit prostředí Azure-SSIS IR pomocí nového serveru Azure SQL Database / spravované instance (privátní verze Preview) a připojit se k této virtuální síti ve stejném umístění, čímž účinně omezíte přesuny dat mezi různými umístěními.
- Pokud se umístění existujícího serveru Azure SQL Database / spravované instance (privátní verze Preview), který je hostitelem služby SSISDB, neshoduje s umístěním virtuální sítě připojené k vaší místní síti, nejdřív vytvořte prostředí Azure-SSIS IR pomocí existujícího serveru Azure SQL Database / spravované instance (privátní verze Preview), připojte se k jiné virtuální síti ve stejném umístění a potom nakonfigurujte připojení virtuální sítě k virtuální síti mezi různými umístěními.


## <a name="next-steps"></a>Další kroky
Viz následující články:

- [Vytvoření prostředí Integration Runtime v místním prostředí](create-self-hosted-integration-runtime.md)
- [Vytvoření prostředí Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md). Tento článek dál navazuje na tento kurz a obsahuje pokyny k použití spravované instance SQL Azure (privátní verze Preview) a připojení IR k virtuální síti. 
