---
title: "aaaUse případ - odběratele profilace"
description: "Zjistěte, jak je Azure Data Factory používané toocreate řízené daty pracovního postupu (kanál) tooprofile herní zákazníků."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: e07d55cf-8051-4203-9966-bdfa1035104b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 47f5e77242366c80cce2a2db65e3c696505b3e1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-case---customer-profiling"></a>Příklad použití – profilace zákazníka
Azure Data Factory je jedním z mnoha služeb používaných tooimplement hello Cortana Intelligence Suite akcelerátorů řešení.  Další informace o Cortana Intelligence najdete v článku [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics). V tomto dokumentu jsme popisují toohelp případu jednoduché použití začít pracovat s pochopení, jak Azure Data Factory můžete řešení běžných potíží analytics.

## <a name="scenario"></a>Scénář
Contoso je herní společnost, která vytvoří hry pro různé platformy: her konzoly, dlaně zařízení a osobních počítačů (počítače). Jako přehrávače hraní her, velkého objemu dat protokolu se vytvářejí, zda text hello sleduje předvolby hello uživatele, herní styl a vzorce používání.  V kombinaci s demografické údaje, místní a data produktu Contoso můžou provádět analýzy tooguide ve hře a o tom, jak tooenhance přehrávače prostředí a cílení pro upgrade je uzavře. 

Cílem společnosti Contoso je tooidentify příležitostí až zákazník nebo cross zákazník na základě historie herní hello jeho přehrávačů a přidejte zajímavé funkce toodrive obchodní růstu a poskytují lepší toocustomers prostředí. Pro tento případ použití používáme herní společnosti jako příklad firmy. Hello společnost chce toooptimize jeho hry na základě chování hráčů. Tyto zásady použít obchodní tooany, který chce tooengage svým zákazníkům kolem jeho zboží a služeb a zajištění lepších možností svým zákazníkům.

V tomto řešení Contoso chce tooevaluate hello efektivita marketingové kampaně, které se nedávno spuštěna. Jsme začínat hello nezpracovaná herní protokoly, proces a zlepšit komunikaci oddělení je s daty zeměpisnou polohu, připojení s inzerování referenční data a nakonec je zkopírujte do Azure SQL Database tooanalyze hello kampaně dopad.

## <a name="deploy-solution"></a>Nasazení řešení.
Všechny budete muset tooaccess a je vyzkoušet tento případ použití jednoduchého [předplatného Azure](https://azure.microsoft.com/pricing/free-trial/), [účtu Azure Blob storage](../storage/common/storage-create-storage-account.md#create-a-storage-account)a [Azure SQL Database](../sql-database/sql-database-get-started.md). Nasazení hello odběratele profilace kanálu z hello **ukázkové kanály** dlaždici na domovskou stránku hello svojí datové továrny.

1. Objekt pro vytváření dat vytvořit nebo otevřít existující datovou továrnu. V tématu [kopírování dat z úložiště objektů Blob tooSQL databázi pomocí služby Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro kroky toocreate služby data factory.
2. V hello **DATA FACTORY** hello data Factory klikněte na hello **ukázkové kanály** dlaždici.

    ![Ukázka kanály dlaždice](./media/data-factory-samples/SamplePipelinesTile.png)
3. V hello **ukázkové kanály** okně klikněte na tlačítko hello **odběratele profilace** , které chcete toodeploy.

    ![Okno ukázku kanálů](./media/data-factory-samples/SampleTile.png)
4. Zadejte nastavení konfigurace pro ukázku hello. Například váš název účtu úložiště Azure a klíč, název serveru Azure SQL, databáze, ID uživatele a heslo.

    ![Ukázka okna](./media/data-factory-samples/SampleBlade.png)
5. Jakmile jste hotovi s určujícím hello konfigurační nastavení, klikněte na tlačítko **vytvořit** toocreate nebo nasazení hello ukázka kanálů a propojené služby nebo tabulek používané hello kanály.
6. Hello stav nasazení se zobrazí na dlaždici ukázka hello dříve jste klikli na hello **ukázkové kanály** okno.

    ![Stav nasazení](./media/data-factory-samples/DeploymentStatus.png)
7. Až se zobrazí hello **nasazení bylo úspěšné** zprávu na dlaždici hello hello ukázce zavřít hello **ukázkové kanály** okno.  
8. Na **DATA FACTORY** okně uvidíte propojených služeb, datových sad a kanálů přidají tooyour data factory.  

    ![Okno Objekt pro vytváření dat](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="solution-overview"></a>Přehled řešení
Tento případ použití jednoduchého slouží jako příklad jak můžete použít Azure Data Factory tooingest, Příprava, transformace, analyzovat a publikovat data.

![Ucelený pracovní postup](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Tento obrázek znázorňuje, jak hello datových kanálů po zobrazí v hello portál Azure byla nasazena.

1. Hello **PartitionGameLogsPipeline** čte hello nezpracovaná herní události z úložiště objektů blob a vytvoří oddíly na základě rok, měsíc a den.
2. Hello **EnrichGameLogsPipeline** připojí oddílů herní události u referenčních dat geografické kódu a vylepšuje hello data tím mapování IP adresy toohello odpovídající zeměpisných míst.
3. Hello **AnalyzeMarketingCampaignPipeline** kanálu používá hello obohacená známým data a procesy s hello inzerování toocreate hello finální výstup dat obsahující efektivitu marketingu kampaně.

V tomto příkladu je objekt pro vytváření dat použité tooorchestrate aktivity, které zkopírujte vstupních dat, transformace a zpracování dat hello a výstup hello poslední datovou tooan Azure SQL Database.  Také můžete vizualizovat hello síti datových kanálů, spravovat a monitorovat jejich stav z hello uživatelského rozhraní.

## <a name="benefits"></a>Výhody
Optimalizace analýzy profilu uživatele a zarovnání s obchodními cíli, herní společnosti je možné tooquickly shromažďování vzorce a analyzovat hello účinnosti jeho marketingových kampaní.

