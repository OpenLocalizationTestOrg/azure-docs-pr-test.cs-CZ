---
title: "aaaLoad data do Azure SQL Data Warehouse – Data Factory | Microsoft Docs"
description: "V tomto kurzu načte data do Azure SQL Data Warehouse pomocí Azure Data Factory a používá databázi systému SQL Server jako zdroj dat hello."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: loading
ms.date: 02/08/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 471871d3ee00ab34cc84bb63fbd13a323d14c2b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a><span data-ttu-id="55991-103">Načtení dat do SQL Data Warehouse pomocí služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="55991-103">Load data into SQL Data Warehouse with Data Factory</span></span>

<span data-ttu-id="55991-104">Můžete použít Azure Data Factory tooload data do Azure SQL Data Warehouse z jakéhokoli z hello [podporovanými úložišti dat zdroj](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="55991-104">You can use Azure Data Factory tooload data into Azure SQL Data Warehouse from any of hello [supported source data stores](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="55991-105">Například můžete načtení dat z Azure SQL database nebo databáze Oracle do SQL data warehouse pomocí služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="55991-105">For example, you can load data from an Azure SQL database or an Oracle database into a SQL data warehouse by using Data Factory.</span></span> <span data-ttu-id="55991-106">Kurzu v tomto článku se dozvíte, jak tooload dat z SQL serveru místní databáze do SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="55991-106">Tutorial in this article shows you how tooload data from an on-premises SQL Server database into a SQL data warehouse.</span></span>

<span data-ttu-id="55991-107">**Odhad času**: v tomto kurzu, trvá asi 10 až 15 minut toocomplete po splnění předpokladů hello.</span><span class="sxs-lookup"><span data-stu-id="55991-107">**Time estimate**: This tutorial takes about 10-15 minutes toocomplete once hello prerequisites are met.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55991-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="55991-108">Prerequisites</span></span>

- <span data-ttu-id="55991-109">Je nutné **databáze systému SQL Server** s tabulkami, které obsahují hello data toobe zkopírovali toohello SQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="55991-109">You need a **SQL Server database** with tables that contain hello data toobe copied over toohello SQL data warehouse.</span></span>  

- <span data-ttu-id="55991-110">Je třeba online **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="55991-110">You need an online **SQL Data Warehouse**.</span></span> <span data-ttu-id="55991-111">Pokud již datový sklad nemáte, zjistěte, jak příliš[vytvoření Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="55991-111">If you do not already have a data warehouse, learn how too[Create an Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span></span>

- <span data-ttu-id="55991-112">Budete potřebovat **účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="55991-112">You need an **Azure Storage Account**.</span></span> <span data-ttu-id="55991-113">Pokud již nemáte účet úložiště, zjistěte, jak příliš[vytvořit účet úložiště](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="55991-113">If you do not already have a storage account, learn how too[Create a storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="55991-114">Pro nejlepší výkon, vyhledejte účet úložiště hello a hello datový sklad v hello stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="55991-114">For best performance, locate hello storage account and hello data warehouse in hello same Azure region.</span></span>

## <a name="configure-a-data-factory"></a><span data-ttu-id="55991-115">Konfigurace služby data factory</span><span class="sxs-lookup"><span data-stu-id="55991-115">Configure a data factory</span></span>
1. <span data-ttu-id="55991-116">Přihlaste se toohello [portál Azure][].</span><span class="sxs-lookup"><span data-stu-id="55991-116">Log in toohello [Azure portal][].</span></span>
2. <span data-ttu-id="55991-117">Vyhledejte datového skladu a klikněte na tlačítko tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="55991-117">Locate your data warehouse and click tooopen it.</span></span>
3. <span data-ttu-id="55991-118">V hlavním okně hello, klikněte na **načítání dat** > **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="55991-118">In hello main blade, click **Load Data** > **Azure Data Factory**.</span></span>

    ![Spustí Průvodce načítání dat](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. <span data-ttu-id="55991-120">Pokud nemáte objekt pro vytváření dat ve vašem předplatném Azure, uvidíte **nový objekt pro vytváření dat** dialogové okno na samostatné kartě hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="55991-120">If you do not have a data factory in your Azure subscription, you see a **New Data Factory** dialog box in a separate tab of hello browser.</span></span> <span data-ttu-id="55991-121">Vyplňte hello požadované informace a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="55991-121">Fill in hello requested information, and click **Create**.</span></span> <span data-ttu-id="55991-122">Po vytvoření objektu pro vytváření dat hello hello **nový objekt pro vytváření dat** dialog se zavře a zobrazí hello **vyberte objekt pro vytváření dat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="55991-122">After hello data factory is created, hello **New Data Factory** dialog box closes, and you see hello **Select Data Factory** dialog box.</span></span>

    <span data-ttu-id="55991-123">Pokud máte jeden nebo více datové továrny už v hello předplatné Azure, uvidíte hello **vyberte objekt pro vytváření dat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="55991-123">If you have one or more data factories already in hello Azure subscription, you see hello **Select Data Factory** dialog box.</span></span> <span data-ttu-id="55991-124">V tomto dialogovém můžete vybrat existující datovou továrnu nebo klikněte na tlačítko **vytvořit nový objekt pro vytváření dat** toocreate novou.</span><span class="sxs-lookup"><span data-stu-id="55991-124">In this dialog box, you can either select an existing data factory or click **Create new data factory** toocreate a new one.</span></span>

    ![Konfigurace objektu pro vytváření dat](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. <span data-ttu-id="55991-126">V hello **vyberte objekt pro vytváření dat** dialogové okno, hello **načíst data** ve výchozím nastavení je vybraná možnost.</span><span class="sxs-lookup"><span data-stu-id="55991-126">In hello **Select Data Factory** dialog box, hello **Load data** option is selected by default.</span></span> <span data-ttu-id="55991-127">Klikněte na tlačítko **Další** toostart vytváření dat načítání úloh.</span><span class="sxs-lookup"><span data-stu-id="55991-127">Click **Next** toostart creating a data loading task.</span></span>

## <a name="configure-hello-data-factory-properties"></a><span data-ttu-id="55991-128">Konfigurovat vlastnosti objektu pro vytváření dat hello</span><span class="sxs-lookup"><span data-stu-id="55991-128">Configure hello data factory properties</span></span>
<span data-ttu-id="55991-129">Teď, když jste vytvořili objekt pro vytváření dat, hello dalším krokem je tooconfigure hello data načítání plánu.</span><span class="sxs-lookup"><span data-stu-id="55991-129">Now that you have created a data factory, hello next step is tooconfigure hello data loading schedule.</span></span>

1. <span data-ttu-id="55991-130">Pro **název úlohy**, zadejte **DWLoadData fromSQLServer**.</span><span class="sxs-lookup"><span data-stu-id="55991-130">For **Task name**, enter **DWLoadData-fromSQLServer**.</span></span>
2. <span data-ttu-id="55991-131">Použít výchozí hello **spustit jednou nyní** možnost, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="55991-131">Use hello default **Run once now** option, click **Next**.</span></span>

    ![Nakonfigurujte plán zatížení](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-hello-source-data-store-and-gateway"></a><span data-ttu-id="55991-133">Konfigurace hello zdrojového úložiště dat a brány</span><span class="sxs-lookup"><span data-stu-id="55991-133">Configure hello source data store and gateway</span></span>
<span data-ttu-id="55991-134">Objekt pro vytváření dat se teď říct o databázi systému SQL Server místní hello, ze kterého chcete tooload data.</span><span class="sxs-lookup"><span data-stu-id="55991-134">Now you tell Data Factory about hello on-premises SQL Server database from which you want tooload data.</span></span>

1. <span data-ttu-id="55991-135">Zvolte **systému SQL Server** z hello podporované zdroje dat ukládání katalogu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="55991-135">Choose **SQL Server** from hello supported source data store catalog, and click **Next**.</span></span>

    ![Vyberte zdroj systému SQL Server](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. <span data-ttu-id="55991-137">A **databáze systému SQL Server místní hello zadejte** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="55991-137">A **Specify hello on-premises SQL Server database** dialog appears.</span></span> <span data-ttu-id="55991-138">nejprve Hello **název připojení** je automaticky vyplněno pole.</span><span class="sxs-lookup"><span data-stu-id="55991-138">hello first  **Connection name** field is auto filled in.</span></span> <span data-ttu-id="55991-139">druhé pole Hello požádá o hello název hello **brány**.</span><span class="sxs-lookup"><span data-stu-id="55991-139">hello second field asks for hello name of hello **Gateway**.</span></span> <span data-ttu-id="55991-140">Pokud používáte existující datovou továrnu, která již má bránu, můžete znovu použít bránu hello vyberete v rozevíracím seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="55991-140">If you are using an existing data factory that already has a gateway, you can reuse hello gateway by selecting it from hello drop-down list.</span></span> <span data-ttu-id="55991-141">Klikněte na tlačítko hello **vytvořit bránu** toocreate Brána pro správu dat na odkaz.</span><span class="sxs-lookup"><span data-stu-id="55991-141">Click hello **Create Gateway** link toocreate a Data Management Gateway.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="55991-142">Pokud hello zdrojového úložiště dat je místně nebo na virtuálním počítači Azure IaaS, brána pro správu dat je požadovaná.</span><span class="sxs-lookup"><span data-stu-id="55991-142">If hello source data store is on-premises or in an Azure IaaS virtual machine, a Data Management Gateway is required.</span></span> <span data-ttu-id="55991-143">Brána je v relaci 1-1 pomocí služby data factory.</span><span class="sxs-lookup"><span data-stu-id="55991-143">A gateway has a 1-1 relationship with a data factory.</span></span> <span data-ttu-id="55991-144">Nedá se použít z jiného objektu pro vytváření dat, ale mohou být využívána načítání úlohy s v hello více dat stejné služby data factory.</span><span class="sxs-lookup"><span data-stu-id="55991-144">It cannot be used from another data factory, but it can be used by multiple data loading tasks with in hello same data factory.</span></span> <span data-ttu-id="55991-145">Při spuštění úlohy načítání dat, může být bránu použité tooconnect toomultiple datová úložiště.</span><span class="sxs-lookup"><span data-stu-id="55991-145">A gateway can be used tooconnect toomultiple data stores when running data loading tasks.</span></span>
    >
    > <span data-ttu-id="55991-146">Podrobné informace o bráně hello najdete v tématu [Brána pro správu dat](../data-factory/data-factory-data-management-gateway.md) článku.</span><span class="sxs-lookup"><span data-stu-id="55991-146">For detailed information about hello gateway, see [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md) article.</span></span>

3. <span data-ttu-id="55991-147">A **vytvořit bránu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="55991-147">A **Create Gateway** dialog box appears.</span></span> <span data-ttu-id="55991-148">Název zadejte **GatewayForDWLoading**a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="55991-148">For Name, enter **GatewayForDWLoading**, and click **Create**.</span></span>

4. <span data-ttu-id="55991-149">A **konfigurace brány** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="55991-149">A **Configure Gateway** dialog box appears.</span></span> <span data-ttu-id="55991-150">Klikněte na tlačítko **spusťte Expresní instalace na tomto počítači** tooautomatically stáhnout, nainstalovat a zaregistrovat brány pro správu dat na počítači pro aktuální.</span><span class="sxs-lookup"><span data-stu-id="55991-150">Click **Launch express setup on this computer** tooautomatically download, install, and register Data Management Gateway on your current machine.</span></span> <span data-ttu-id="55991-151">průběh Hello je zobrazen v místním okně.</span><span class="sxs-lookup"><span data-stu-id="55991-151">hello progress is shown in a pop-up window.</span></span> <span data-ttu-id="55991-152">Pokud počítač hello se nemůže připojit toohello úložiště dat, můžete ručně [stáhnout a nainstalovat bránu hello](https://www.microsoft.com/download/details.aspx?id=39717) na počítači, který může připojit toohello data ukládat a potom pomocí klíče tooregister hello.</span><span class="sxs-lookup"><span data-stu-id="55991-152">If hello machine cannot connect toohello data store, you can manually [download and install hello gateway](https://www.microsoft.com/download/details.aspx?id=39717) on a machine that can connect toohello data store, and then use hello key tooregister.</span></span>
    > [!NOTE]
    > <span data-ttu-id="55991-153">Expresní instalace Hello nativně pracuje s Microsoft Edge a prohlížeče Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="55991-153">hello express setup works natively with Microsoft Edge and Internet Explorer.</span></span> <span data-ttu-id="55991-154">Pokud používáte Google Chrome, nejprve nainstalujte rozšíření hello ClickOnce z internetového obchodu Chrome.</span><span class="sxs-lookup"><span data-stu-id="55991-154">If you are using Google Chrome, first install hello ClickOnce extension from Chrome web store.</span></span>

    ![Spusťte instalační program Express](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. <span data-ttu-id="55991-156">Počkejte toocomplete instalační program brány hello.</span><span class="sxs-lookup"><span data-stu-id="55991-156">Wait for hello gateway setup toocomplete.</span></span> <span data-ttu-id="55991-157">Jakmile hello brány se úspěšně zaregistroval a je online, hello automaticky otevírané okno se zavře a hello nové brány se zobrazí v poli brána hello.</span><span class="sxs-lookup"><span data-stu-id="55991-157">Once hello gateway is successfully registered and is online, hello pop-up window closes and hello new gateway appears in hello gateway field.</span></span> <span data-ttu-id="55991-158">Potom vyplňte hello rest požadovaná pole následujícím způsobem, pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="55991-158">Then fill in hello rest required fields as follows, then click **Next**.</span></span>
    - <span data-ttu-id="55991-159">**Název serveru**: název hello místní systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="55991-159">**Server name**: Name of hello on-premises SQL Server.</span></span>
    - <span data-ttu-id="55991-160">**Název databáze**: databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="55991-160">**Database name**: SQL Server database.</span></span>
    - <span data-ttu-id="55991-161">**Přihlašovací údaje šifrování**: použijte výchozí hello "ve webovém prohlížeči".</span><span class="sxs-lookup"><span data-stu-id="55991-161">**Credential encryption**: Use hello default "By web browser".</span></span>
    - <span data-ttu-id="55991-162">**Typ ověřování**: Zvolte hello typ ověřování, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="55991-162">**Authentication type**: Choose hello type of authentication you are using.</span></span>
    - <span data-ttu-id="55991-163">**Uživatelské jméno** a **heslo**: Zadejte hello uživatelské jméno a heslo pro uživatele, který má oprávnění toocopy hello data.</span><span class="sxs-lookup"><span data-stu-id="55991-163">**User name** and **password**: Enter hello user name and password for a user who has permission toocopy hello data.</span></span>

    ![Spusťte instalační program Express](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. <span data-ttu-id="55991-165">dalším krokem Hello je toochoose hello tabulky z datových toocopy hello.</span><span class="sxs-lookup"><span data-stu-id="55991-165">hello next step is toochoose hello tables from which toocopy hello data.</span></span> <span data-ttu-id="55991-166">Hello tabulky můžete filtrovat pomocí klíčových slov.</span><span class="sxs-lookup"><span data-stu-id="55991-166">You can filter hello tables by using keywords.</span></span> <span data-ttu-id="55991-167">A můžete zobrazit náhled hello dat a tabulka schématu dolním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="55991-167">And you can preview hello data and table schema in hello bottom panel.</span></span> <span data-ttu-id="55991-168">Po dokončení váš výběr, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="55991-168">After you finish your selection, click **Next**.</span></span>

    ![Výběr tabulek](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-hello-destination-your-sql-data-warehouse"></a><span data-ttu-id="55991-170">Nakonfigurujte cíl hello, datový sklad SQL</span><span class="sxs-lookup"><span data-stu-id="55991-170">Configure hello destination, your SQL Data Warehouse</span></span>

<span data-ttu-id="55991-171">Objekt pro vytváření dat se teď říct o informace o cíli hello.</span><span class="sxs-lookup"><span data-stu-id="55991-171">Now you tell Data Factory about hello destination information.</span></span>

1. <span data-ttu-id="55991-172">Informace o připojení SQL Data Warehouse se vyplní automaticky.</span><span class="sxs-lookup"><span data-stu-id="55991-172">Your SQL Data Warehouse connection information is filled in automatically.</span></span> <span data-ttu-id="55991-173">Zadejte heslo hello hello uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="55991-173">Enter hello password for hello user name.</span></span> <span data-ttu-id="55991-174">a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="55991-174">and click **Next**.</span></span>

    ![Konfigurace cílového](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. <span data-ttu-id="55991-176">Zobrazí se mapování inteligentního tabulky, která mapuje zdrojové tabulky toodestination podle názvů tabulek.</span><span class="sxs-lookup"><span data-stu-id="55991-176">An intelligent table mapping appears that maps source toodestination tables based on table names.</span></span> <span data-ttu-id="55991-177">Pokud tabulka hello neexistuje v cílovém umístění hello, ve výchozím nastavení ADF vytvoří s hello stejným názvem (platí tooSQL serveru nebo Azure SQL Database jako zdroj).</span><span class="sxs-lookup"><span data-stu-id="55991-177">If hello table does not exist in hello destination, by default ADF will create one with hello same name (this applies tooSQL Server or Azure SQL Database as source).</span></span> <span data-ttu-id="55991-178">Můžete také toomap tooan existující tabulce.</span><span class="sxs-lookup"><span data-stu-id="55991-178">You can also choose toomap tooan existing table.</span></span> <span data-ttu-id="55991-179">Zkontrolujte a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="55991-179">Review and click **Next**.</span></span>

    ![Mapování tabulek](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. <span data-ttu-id="55991-181">Zkontrolujte mapování hello schématu a vyhledejte chyby nebo upozornění.</span><span class="sxs-lookup"><span data-stu-id="55991-181">Review hello schema mapping and look for error or warning messages.</span></span> <span data-ttu-id="55991-182">Inteligentní mapování je založena na název sloupce.</span><span class="sxs-lookup"><span data-stu-id="55991-182">Intelligent mapping is based on column name.</span></span> <span data-ttu-id="55991-183">Pokud konverzi nepodporovaný datový typ mezi zdrojovým a cílovým sloupcem hello se zobrazí chybová zpráva spolu s odpovídající tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="55991-183">If there is an unsupported data type conversion between hello source and destination column, you see an error message alongside hello corresponding table.</span></span> <span data-ttu-id="55991-184">Pokud se rozhodnete toolet Data Factory automaticky vytvářet tabulky hello, převod typu správné dat může dojít v případě potřeby toofix hello nekompatibilita mezi zdrojovým a cílovým úložišti.</span><span class="sxs-lookup"><span data-stu-id="55991-184">If you choose toolet Data Factory auto create hello tables, proper data type conversion may happen if needed toofix hello incompatibility between source and destination stores.</span></span>

    ![Schéma mapování](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. <span data-ttu-id="55991-186">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="55991-186">Click **Next**.</span></span>

## <a name="configure-hello-performance-settings"></a><span data-ttu-id="55991-187">Konfigurovat nastavení výkonu hello</span><span class="sxs-lookup"><span data-stu-id="55991-187">Configure hello performance settings</span></span>
<span data-ttu-id="55991-188">V konfiguracích hello výkonu, nakonfigurujte účet úložiště Azure používají pro pracovní hello data předtím, než se načte do SQL Data Warehouse pomocí performantly [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span><span class="sxs-lookup"><span data-stu-id="55991-188">In hello Performance configurations, you configure an Azure storage account used for staging hello data before it loads into SQL Data Warehouse performantly using [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span></span> <span data-ttu-id="55991-189">Po dokončení kopírování hello hello průběžných dat v úložišti se automaticky vyčistí.</span><span class="sxs-lookup"><span data-stu-id="55991-189">After hello copy is done, hello interim data in storage will be cleaned up automatically.</span></span>

<span data-ttu-id="55991-190">Vyberte stávající účet úložiště Azure a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="55991-190">Select an existing Azure Storage account, and click **Next**.</span></span>

![Konfigurovat pracovní objektů blob](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-hello-pipeline"></a><span data-ttu-id="55991-192">Zkontrolujte souhrnné informace a nasaďte kanál hello</span><span class="sxs-lookup"><span data-stu-id="55991-192">Review summary information and deploy hello pipeline</span></span>

<span data-ttu-id="55991-193">Zkontrolujte konfiguraci hello a klikněte na tlačítko **Dokončit** tlačítko toodeploy hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="55991-193">Review hello configuration and click **Finish** button toodeploy hello pipeline.</span></span>

![Nasazení služby data factory](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a><span data-ttu-id="55991-195">Probíhá načítání dat monitorování</span><span class="sxs-lookup"><span data-stu-id="55991-195">Monitor data loading progress</span></span>

<span data-ttu-id="55991-196">Zobrazí se průběh nasazení hello a má za následek hello **nasazení** stránky.</span><span class="sxs-lookup"><span data-stu-id="55991-196">You can see hello deployment progress and results in hello **Deployment** page.</span></span>

1. <span data-ttu-id="55991-197">Po dokončení nasazení hello kliknutím na odkaz hello, která uvádí, že **kliknutím sem kanál kopírování toomonitor** toomonitor data průběhu načítání.</span><span class="sxs-lookup"><span data-stu-id="55991-197">Once hello deployment is done, click hello link that says **Click here toomonitor copy pipeline** toomonitor data loading progress.</span></span>

    ![Monitorování kanálu](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. <span data-ttu-id="55991-199">nově vytvořený Hello **DWLoadData fromSQLServer** kanálu načítání dat se automaticky vybere ze hello levé **Průzkumníka prostředků**.</span><span class="sxs-lookup"><span data-stu-id="55991-199">hello newly created **DWLoadData-fromSQLServer** data loading pipeline is auto selected from hello left-hand **Resource Explorer**.</span></span>

    ![Zobrazení kanálu](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. <span data-ttu-id="55991-201">Klikněte na tlačítko do kanálu hello uprostřed hello panely toosee hello podrobný stav pro každou tabulku, která mapuje tooan aktivity.</span><span class="sxs-lookup"><span data-stu-id="55991-201">Click into hello pipeline in hello middle panel toosee hello detailed status for each table that maps tooan Activity.</span></span>

    ![Zobrazení tabulky aktivity](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. <span data-ttu-id="55991-203">Klikněte na další na aktivity a zobrazí data hello načítání podrobností o v pravém panelu hello včetně velikost dat, řádky, propustnost, atd.</span><span class="sxs-lookup"><span data-stu-id="55991-203">Further click into an activity and you see hello data loading details in hello right panel including data size, rows, throughput, etc.</span></span>

    ![Zobrazit podrobnosti o aktivitě tabulky](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. <span data-ttu-id="55991-205">toolaunch toto monitorování zobrazit později, přejděte tooyour SQL Data Warehouse, klikněte na tlačítko **načítání dat > Azure Data Factory**, vyberete továrnu a zvolit **monitorování existující načítání úlohy**.</span><span class="sxs-lookup"><span data-stu-id="55991-205">toolaunch this monitoring view later, go tooyour SQL Data Warehouse, click **Load Data > Azure Data Factory**, select your factory, and choose **Monitor existing loading tasks**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="55991-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55991-206">Next steps</span></span>

<span data-ttu-id="55991-207">toomigrate tooSQL vaše databáze datového skladu, najdete v části [Přehled migrace](sql-data-warehouse-overview-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="55991-207">toomigrate your database tooSQL Data Warehouse, see [Migration overview](sql-data-warehouse-overview-migrate.md).</span></span>

<span data-ttu-id="55991-208">toolearn informace o Azure Data Factory a možnosti přesun dat najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="55991-208">toolearn more about Azure Data Factory and its data movement capabilities, see hello following articles:</span></span>

- [<span data-ttu-id="55991-209">Úvod tooAzure objekt pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="55991-209">Introduction tooAzure Data Factory</span></span>](../data-factory/data-factory-introduction.md)
- [<span data-ttu-id="55991-210">Přesun dat pomocí aktivity kopírování</span><span class="sxs-lookup"><span data-stu-id="55991-210">Move data by using Copy Activity</span></span>](../data-factory/data-factory-data-movement-activities.md)
- [<span data-ttu-id="55991-211">Přesun dat tooand z Azure SQL Data Warehouse pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="55991-211">Move data tooand from Azure SQL Data Warehouse using Azure Data Factory</span></span>](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

<span data-ttu-id="55991-212">tooexplore vaše data v SQL Data Warehouse, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="55991-212">tooexplore your data in SQL Data Warehouse, see hello following articles:</span></span>

- [<span data-ttu-id="55991-213">Připojení k sadě Visual Studio a rozšíření SSDT tooSQL datového skladu</span><span class="sxs-lookup"><span data-stu-id="55991-213">Connect tooSQL Data Warehouse with Visual Studio and SSDT</span></span>](sql-data-warehouse-query-visual-studio.md)
- <span data-ttu-id="55991-214">[Vizuální data s Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="55991-214">[Visual data with Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span></span>

<!-- Azure references -->
[portál Azure]: https://portal.azure.com
