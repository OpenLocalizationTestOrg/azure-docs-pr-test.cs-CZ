---
title: "Načtení dat do Azure SQL Data Warehouse – Data Factory | Microsoft Docs"
description: "V tomto kurzu načte data do Azure SQL Data Warehouse pomocí Azure Data Factory a používá jako zdroj dat databázi systému SQL Server."
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
ms.openlocfilehash: 12a35213e07ff16bdc1c27be106792bcc032ac80
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a><span data-ttu-id="1ab28-103">Načtení dat do SQL Data Warehouse pomocí služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="1ab28-103">Load data into SQL Data Warehouse with Data Factory</span></span>

<span data-ttu-id="1ab28-104">Azure Data Factory můžete použít k načtení dat do Azure SQL Data Warehouse z jakéhokoli z [podporovanými úložišti dat zdroj](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="1ab28-104">You can use Azure Data Factory to load data into Azure SQL Data Warehouse from any of the [supported source data stores](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="1ab28-105">Například můžete načtení dat z Azure SQL database nebo databáze Oracle do SQL data warehouse pomocí služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1ab28-105">For example, you can load data from an Azure SQL database or an Oracle database into a SQL data warehouse by using Data Factory.</span></span> <span data-ttu-id="1ab28-106">Kurz v tomto článku se dozvíte, jak načíst data z databáze SQL serveru místně do SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="1ab28-106">Tutorial in this article shows you how to load data from an on-premises SQL Server database into a SQL data warehouse.</span></span>

<span data-ttu-id="1ab28-107">**Odhad času**: v tomto kurzu dokončení po splnění předpokladů trvá asi 10 až 15 minut.</span><span class="sxs-lookup"><span data-stu-id="1ab28-107">**Time estimate**: This tutorial takes about 10-15 minutes to complete once the prerequisites are met.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ab28-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1ab28-108">Prerequisites</span></span>

- <span data-ttu-id="1ab28-109">Je nutné **databáze systému SQL Server** s tabulkami, které obsahují data, která mají být zkopírovali do SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="1ab28-109">You need a **SQL Server database** with tables that contain the data to be copied over to the SQL data warehouse.</span></span>  

- <span data-ttu-id="1ab28-110">Je třeba online **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-110">You need an online **SQL Data Warehouse**.</span></span> <span data-ttu-id="1ab28-111">Pokud již datový sklad nemáte, přečtěte si postup [vytvoření Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="1ab28-111">If you do not already have a data warehouse, learn how to [Create an Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span></span>

- <span data-ttu-id="1ab28-112">Budete potřebovat **účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-112">You need an **Azure Storage Account**.</span></span> <span data-ttu-id="1ab28-113">Pokud již nemáte účet úložiště, přečtěte si postup [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="1ab28-113">If you do not already have a storage account, learn how to [Create a storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="1ab28-114">Pro nejlepší výkon vyhledejte účet úložiště a datového skladu ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="1ab28-114">For best performance, locate the storage account and the data warehouse in the same Azure region.</span></span>

## <a name="configure-a-data-factory"></a><span data-ttu-id="1ab28-115">Konfigurace služby data factory</span><span class="sxs-lookup"><span data-stu-id="1ab28-115">Configure a data factory</span></span>
1. <span data-ttu-id="1ab28-116">Přihlaste se k portálu [Azure Portal][].</span><span class="sxs-lookup"><span data-stu-id="1ab28-116">Log in to the [Azure portal][].</span></span>
2. <span data-ttu-id="1ab28-117">Vyhledejte datového skladu a kliknutím ho otevřete.</span><span class="sxs-lookup"><span data-stu-id="1ab28-117">Locate your data warehouse and click to open it.</span></span>
3. <span data-ttu-id="1ab28-118">V hlavním okně klikněte na **načítání dat** > **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-118">In the main blade, click **Load Data** > **Azure Data Factory**.</span></span>

    ![Spustí Průvodce načítání dat](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. <span data-ttu-id="1ab28-120">Pokud nemáte objekt pro vytváření dat ve vašem předplatném Azure, uvidíte **nový objekt pro vytváření dat** dialogové okno na samostatné kartě prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ab28-120">If you do not have a data factory in your Azure subscription, you see a **New Data Factory** dialog box in a separate tab of the browser.</span></span> <span data-ttu-id="1ab28-121">Zadejte požadované informace a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-121">Fill in the requested information, and click **Create**.</span></span> <span data-ttu-id="1ab28-122">Po vytvoření objektu pro vytváření dat **nový objekt pro vytváření dat** dialog se zavře a zobrazí **vyberte objekt pro vytváření dat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ab28-122">After the data factory is created, the **New Data Factory** dialog box closes, and you see the **Select Data Factory** dialog box.</span></span>

    <span data-ttu-id="1ab28-123">Pokud máte jeden nebo více datové továrny už v rámci předplatného Azure, uvidíte **vyberte objekt pro vytváření dat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ab28-123">If you have one or more data factories already in the Azure subscription, you see the **Select Data Factory** dialog box.</span></span> <span data-ttu-id="1ab28-124">V tomto dialogovém můžete vybrat existující datovou továrnu nebo klikněte na tlačítko **vytvořit nový objekt pro vytváření dat** vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="1ab28-124">In this dialog box, you can either select an existing data factory or click **Create new data factory** to create a new one.</span></span>

    ![Konfigurace objektu pro vytváření dat](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. <span data-ttu-id="1ab28-126">V **vyberte objekt pro vytváření dat** dialogové okno, **načíst data** ve výchozím nastavení je vybraná možnost.</span><span class="sxs-lookup"><span data-stu-id="1ab28-126">In the **Select Data Factory** dialog box, the **Load data** option is selected by default.</span></span> <span data-ttu-id="1ab28-127">Klikněte na tlačítko **Další** chcete začít vytvářet dat načítání úloh.</span><span class="sxs-lookup"><span data-stu-id="1ab28-127">Click **Next** to start creating a data loading task.</span></span>

## <a name="configure-the-data-factory-properties"></a><span data-ttu-id="1ab28-128">Konfigurovat vlastnosti objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="1ab28-128">Configure the data factory properties</span></span>
<span data-ttu-id="1ab28-129">Teď, když jste vytvořili objekt pro vytváření dat, dalším krokem je konfigurace dat načítání plánu.</span><span class="sxs-lookup"><span data-stu-id="1ab28-129">Now that you have created a data factory, the next step is to configure the data loading schedule.</span></span>

1. <span data-ttu-id="1ab28-130">Pro **název úlohy**, zadejte **DWLoadData fromSQLServer**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-130">For **Task name**, enter **DWLoadData-fromSQLServer**.</span></span>
2. <span data-ttu-id="1ab28-131">Použít výchozí **spustit jednou nyní** možnost, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-131">Use the default **Run once now** option, click **Next**.</span></span>

    ![Nakonfigurujte plán zatížení](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-the-source-data-store-and-gateway"></a><span data-ttu-id="1ab28-133">Konfigurace zdrojového úložiště dat a brány</span><span class="sxs-lookup"><span data-stu-id="1ab28-133">Configure the source data store and gateway</span></span>
<span data-ttu-id="1ab28-134">Objekt pro vytváření dat se teď říct o databázi systému SQL Server na místě, ze kterého chcete načíst data.</span><span class="sxs-lookup"><span data-stu-id="1ab28-134">Now you tell Data Factory about the on-premises SQL Server database from which you want to load data.</span></span>

1. <span data-ttu-id="1ab28-135">Zvolte **systému SQL Server** z podporované zdrojové dat ukládání katalogu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-135">Choose **SQL Server** from the supported source data store catalog, and click **Next**.</span></span>

    ![Vyberte zdroj systému SQL Server](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. <span data-ttu-id="1ab28-137">A **zadejte místní databázi systému SQL Server** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ab28-137">A **Specify the on-premises SQL Server database** dialog appears.</span></span> <span data-ttu-id="1ab28-138">První **název připojení** je automaticky vyplněno pole.</span><span class="sxs-lookup"><span data-stu-id="1ab28-138">The first  **Connection name** field is auto filled in.</span></span> <span data-ttu-id="1ab28-139">Druhé pole požádá o název **brány**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-139">The second field asks for the name of the **Gateway**.</span></span> <span data-ttu-id="1ab28-140">Pokud používáte existující datovou továrnu, která již má bránu, můžete znovu použít bránu vyberete v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="1ab28-140">If you are using an existing data factory that already has a gateway, you can reuse the gateway by selecting it from the drop-down list.</span></span> <span data-ttu-id="1ab28-141">Klikněte **vytvořit bránu** odkaz pro vytvoření Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="1ab28-141">Click the **Create Gateway** link to create a Data Management Gateway.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="1ab28-142">Pokud zdrojového úložiště dat je místně nebo na virtuálním počítači Azure IaaS, brána pro správu dat je požadovaná.</span><span class="sxs-lookup"><span data-stu-id="1ab28-142">If the source data store is on-premises or in an Azure IaaS virtual machine, a Data Management Gateway is required.</span></span> <span data-ttu-id="1ab28-143">Brána je v relaci 1-1 pomocí služby data factory.</span><span class="sxs-lookup"><span data-stu-id="1ab28-143">A gateway has a 1-1 relationship with a data factory.</span></span> <span data-ttu-id="1ab28-144">Nedá se použít z jiného objektu pro vytváření dat, ale mohou být využívána dat více načítání úlohy s ve stejném objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="1ab28-144">It cannot be used from another data factory, but it can be used by multiple data loading tasks with in the same data factory.</span></span> <span data-ttu-id="1ab28-145">Brána slouží k připojení k několika úložišť dat při spuštění úlohy načítání dat.</span><span class="sxs-lookup"><span data-stu-id="1ab28-145">A gateway can be used to connect to multiple data stores when running data loading tasks.</span></span>
    >
    > <span data-ttu-id="1ab28-146">Podrobné informace o bráně najdete v tématu [Brána pro správu dat](../data-factory/data-factory-data-management-gateway.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1ab28-146">For detailed information about the gateway, see [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md) article.</span></span>

3. <span data-ttu-id="1ab28-147">A **vytvořit bránu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ab28-147">A **Create Gateway** dialog box appears.</span></span> <span data-ttu-id="1ab28-148">Název zadejte **GatewayForDWLoading**a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-148">For Name, enter **GatewayForDWLoading**, and click **Create**.</span></span>

4. <span data-ttu-id="1ab28-149">A **konfigurace brány** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ab28-149">A **Configure Gateway** dialog box appears.</span></span> <span data-ttu-id="1ab28-150">Klikněte na tlačítko **spusťte Expresní instalace na tomto počítači** automaticky stáhnout, nainstalovat a zaregistrovat brány pro správu dat na počítači aktuální.</span><span class="sxs-lookup"><span data-stu-id="1ab28-150">Click **Launch express setup on this computer** to automatically download, install, and register Data Management Gateway on your current machine.</span></span> <span data-ttu-id="1ab28-151">Průběh se zobrazí v místním okně.</span><span class="sxs-lookup"><span data-stu-id="1ab28-151">The progress is shown in a pop-up window.</span></span> <span data-ttu-id="1ab28-152">Pokud se počítač nemůže připojit k úložišti dat, můžete ručně [stáhnout a nainstalovat bránu](https://www.microsoft.com/download/details.aspx?id=39717) na počítači, který může připojit k datům uložit a potom pomocí klíče k registraci.</span><span class="sxs-lookup"><span data-stu-id="1ab28-152">If the machine cannot connect to the data store, you can manually [download and install the gateway](https://www.microsoft.com/download/details.aspx?id=39717) on a machine that can connect to the data store, and then use the key to register.</span></span>
    > [!NOTE]
    > <span data-ttu-id="1ab28-153">Expresní instalace nativně pracuje s Microsoft Edge a prohlížeče Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="1ab28-153">The express setup works natively with Microsoft Edge and Internet Explorer.</span></span> <span data-ttu-id="1ab28-154">Pokud používáte Google Chrome, nejprve nainstalujte rozšíření ClickOnce z internetového obchodu Chrome.</span><span class="sxs-lookup"><span data-stu-id="1ab28-154">If you are using Google Chrome, first install the ClickOnce extension from Chrome web store.</span></span>

    ![Spusťte instalační program Express](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. <span data-ttu-id="1ab28-156">Počkejte, než instalační program brány k dokončení.</span><span class="sxs-lookup"><span data-stu-id="1ab28-156">Wait for the gateway setup to complete.</span></span> <span data-ttu-id="1ab28-157">Jakmile se brána je úspěšně zaregistrovaný a je online, místní okno se zavře a nové bráně se zobrazí v poli brána.</span><span class="sxs-lookup"><span data-stu-id="1ab28-157">Once the gateway is successfully registered and is online, the pop-up window closes and the new gateway appears in the gateway field.</span></span> <span data-ttu-id="1ab28-158">Potom vyplňte zbývající požadovaná pole následujícím způsobem, pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-158">Then fill in the rest required fields as follows, then click **Next**.</span></span>
    - <span data-ttu-id="1ab28-159">**Název serveru**: název místního serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="1ab28-159">**Server name**: Name of the on-premises SQL Server.</span></span>
    - <span data-ttu-id="1ab28-160">**Název databáze**: databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1ab28-160">**Database name**: SQL Server database.</span></span>
    - <span data-ttu-id="1ab28-161">**Přihlašovací údaje šifrování**: použijte výchozí "ve webovém prohlížeči".</span><span class="sxs-lookup"><span data-stu-id="1ab28-161">**Credential encryption**: Use the default "By web browser".</span></span>
    - <span data-ttu-id="1ab28-162">**Typ ověřování**: Zvolte typ ověřování, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="1ab28-162">**Authentication type**: Choose the type of authentication you are using.</span></span>
    - <span data-ttu-id="1ab28-163">**Uživatelské jméno** a **heslo**: Zadejte uživatelské jméno a heslo pro uživatele, který má oprávnění ke zkopírování dat.</span><span class="sxs-lookup"><span data-stu-id="1ab28-163">**User name** and **password**: Enter the user name and password for a user who has permission to copy the data.</span></span>

    ![Spusťte instalační program Express](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. <span data-ttu-id="1ab28-165">Dalším krokem je vybrat tabulky, ze kterého chcete zkopírovat data.</span><span class="sxs-lookup"><span data-stu-id="1ab28-165">The next step is to choose the tables from which to copy the data.</span></span> <span data-ttu-id="1ab28-166">Tabulky můžete filtrovat pomocí klíčových slov.</span><span class="sxs-lookup"><span data-stu-id="1ab28-166">You can filter the tables by using keywords.</span></span> <span data-ttu-id="1ab28-167">A můžete zobrazit náhled schéma dat a tabulkou v dolním panelu.</span><span class="sxs-lookup"><span data-stu-id="1ab28-167">And you can preview the data and table schema in the bottom panel.</span></span> <span data-ttu-id="1ab28-168">Po dokončení váš výběr, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-168">After you finish your selection, click **Next**.</span></span>

    ![Výběr tabulek](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-the-destination-your-sql-data-warehouse"></a><span data-ttu-id="1ab28-170">Nakonfigurujte cíl, datový sklad SQL</span><span class="sxs-lookup"><span data-stu-id="1ab28-170">Configure the destination, your SQL Data Warehouse</span></span>

<span data-ttu-id="1ab28-171">Objekt pro vytváření dat se teď říct o informace o cíli.</span><span class="sxs-lookup"><span data-stu-id="1ab28-171">Now you tell Data Factory about the destination information.</span></span>

1. <span data-ttu-id="1ab28-172">Informace o připojení SQL Data Warehouse se vyplní automaticky.</span><span class="sxs-lookup"><span data-stu-id="1ab28-172">Your SQL Data Warehouse connection information is filled in automatically.</span></span> <span data-ttu-id="1ab28-173">Zadejte heslo pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="1ab28-173">Enter the password for the user name.</span></span> <span data-ttu-id="1ab28-174">a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-174">and click **Next**.</span></span>

    ![Konfigurace cílového](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. <span data-ttu-id="1ab28-176">Mapování u inteligentního tabulky se zobrazí, že mapy zdrojové do cílové tabulky podle názvů tabulek.</span><span class="sxs-lookup"><span data-stu-id="1ab28-176">An intelligent table mapping appears that maps source to destination tables based on table names.</span></span> <span data-ttu-id="1ab28-177">Pokud tabulka neexistuje v cílovém umístění, ve výchozím nastavení ADF vytvoří jednu se stejným názvem (to platí pro SQL Server nebo Azure SQL Database jako zdroj).</span><span class="sxs-lookup"><span data-stu-id="1ab28-177">If the table does not exist in the destination, by default ADF will create one with the same name (this applies to SQL Server or Azure SQL Database as source).</span></span> <span data-ttu-id="1ab28-178">Můžete také mapovat k existující tabulce.</span><span class="sxs-lookup"><span data-stu-id="1ab28-178">You can also choose to map to an existing table.</span></span> <span data-ttu-id="1ab28-179">Zkontrolujte a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-179">Review and click **Next**.</span></span>

    ![Mapování tabulek](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. <span data-ttu-id="1ab28-181">Zkontrolujte mapování schématu a vyhledejte chyby nebo upozornění.</span><span class="sxs-lookup"><span data-stu-id="1ab28-181">Review the schema mapping and look for error or warning messages.</span></span> <span data-ttu-id="1ab28-182">Inteligentní mapování je založena na název sloupce.</span><span class="sxs-lookup"><span data-stu-id="1ab28-182">Intelligent mapping is based on column name.</span></span> <span data-ttu-id="1ab28-183">Pokud konverzi nepodporovaný datový typ mezi zdrojovým a cílovým sloupcem se zobrazí chybová zpráva spolu s odpovídající tabulky.</span><span class="sxs-lookup"><span data-stu-id="1ab28-183">If there is an unsupported data type conversion between the source and destination column, you see an error message alongside the corresponding table.</span></span> <span data-ttu-id="1ab28-184">Pokud si zvolíte umožníte Data Factory automaticky vytvářet tabulky, převod typu správné dat může dojít, pokud je nutná k vyřešení nekompatibilita mezi zdrojovým a cílovým úložišti.</span><span class="sxs-lookup"><span data-stu-id="1ab28-184">If you choose to let Data Factory auto create the tables, proper data type conversion may happen if needed to fix the incompatibility between source and destination stores.</span></span>

    ![Schéma mapování](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. <span data-ttu-id="1ab28-186">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-186">Click **Next**.</span></span>

## <a name="configure-the-performance-settings"></a><span data-ttu-id="1ab28-187">Konfigurovat nastavení výkonu</span><span class="sxs-lookup"><span data-stu-id="1ab28-187">Configure the performance settings</span></span>
<span data-ttu-id="1ab28-188">V konfiguraci výkonu nakonfigurujete účet úložiště Azure používají pro pracovní data předtím, než se načte do SQL Data Warehouse pomocí performantly [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span><span class="sxs-lookup"><span data-stu-id="1ab28-188">In the Performance configurations, you configure an Azure storage account used for staging the data before it loads into SQL Data Warehouse performantly using [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span></span> <span data-ttu-id="1ab28-189">Po dokončení kopírování průběžných dat v úložišti se automaticky vyčistí.</span><span class="sxs-lookup"><span data-stu-id="1ab28-189">After the copy is done, the interim data in storage will be cleaned up automatically.</span></span>

<span data-ttu-id="1ab28-190">Vyberte stávající účet úložiště Azure a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-190">Select an existing Azure Storage account, and click **Next**.</span></span>

![Konfigurovat pracovní objektů blob](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-the-pipeline"></a><span data-ttu-id="1ab28-192">Zkontrolujte souhrnné informace a nasaďte kanál</span><span class="sxs-lookup"><span data-stu-id="1ab28-192">Review summary information and deploy the pipeline</span></span>

<span data-ttu-id="1ab28-193">Zkontrolujte konfiguraci a klikněte na tlačítko **Dokončit** tlačítko nasaďte kanál.</span><span class="sxs-lookup"><span data-stu-id="1ab28-193">Review the configuration and click **Finish** button to deploy the pipeline.</span></span>

![Nasazení služby data factory](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a><span data-ttu-id="1ab28-195">Probíhá načítání dat monitorování</span><span class="sxs-lookup"><span data-stu-id="1ab28-195">Monitor data loading progress</span></span>

<span data-ttu-id="1ab28-196">Zobrazí se průběh nasazení a výsledkem **nasazení** stránky.</span><span class="sxs-lookup"><span data-stu-id="1ab28-196">You can see the deployment progress and results in the **Deployment** page.</span></span>

1. <span data-ttu-id="1ab28-197">Po dokončení nasazení klikněte na odkaz, která uvádí, že **kliknutím sem monitorování kopie kanálu** sledovat data průběhu načítání.</span><span class="sxs-lookup"><span data-stu-id="1ab28-197">Once the deployment is done, click the link that says **Click here to monitor copy pipeline** to monitor data loading progress.</span></span>

    ![Monitorování kanálu](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. <span data-ttu-id="1ab28-199">Nově vytvořený **DWLoadData fromSQLServer** kanálu načítání dat se automaticky vybere ze levé straně **Průzkumníka prostředků**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-199">The newly created **DWLoadData-fromSQLServer** data loading pipeline is auto selected from the left-hand **Resource Explorer**.</span></span>

    ![Zobrazení kanálu](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. <span data-ttu-id="1ab28-201">Klikněte do kanálu v panelu střední zobrazit podrobný stav pro každou tabulku, která se mapuje na aktivitu.</span><span class="sxs-lookup"><span data-stu-id="1ab28-201">Click into the pipeline in the middle panel to see the detailed status for each table that maps to an Activity.</span></span>

    ![Zobrazení tabulky aktivity](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. <span data-ttu-id="1ab28-203">Dále klikněte na aktivity a zobrazit data načítání podrobností o v pravém panelu včetně velikost dat, řádky, propustnost, atd.</span><span class="sxs-lookup"><span data-stu-id="1ab28-203">Further click into an activity and you see the data loading details in the right panel including data size, rows, throughput, etc.</span></span>

    ![Zobrazit podrobnosti o aktivitě tabulky](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. <span data-ttu-id="1ab28-205">Chcete-li spustit tento zobrazení monitorování později, přejděte do SQL Data Warehouse, klikněte na tlačítko **načítání dat > Azure Data Factory**, vyberete továrnu a zvolit **monitorování existující načítání úlohy**.</span><span class="sxs-lookup"><span data-stu-id="1ab28-205">To launch this monitoring view later, go to your SQL Data Warehouse, click **Load Data > Azure Data Factory**, select your factory, and choose **Monitor existing loading tasks**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ab28-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ab28-206">Next steps</span></span>

<span data-ttu-id="1ab28-207">Migrace databáze do SQL Data Warehouse, najdete v části [Přehled migrace](sql-data-warehouse-overview-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="1ab28-207">To migrate your database to SQL Data Warehouse, see [Migration overview](sql-data-warehouse-overview-migrate.md).</span></span>

<span data-ttu-id="1ab28-208">Další informace o Azure Data Factory a jeho funkce pro přesun dat, naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="1ab28-208">To learn more about Azure Data Factory and its data movement capabilities, see the following articles:</span></span>

- [<span data-ttu-id="1ab28-209">Úvod do Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="1ab28-209">Introduction to Azure Data Factory</span></span>](../data-factory/data-factory-introduction.md)
- [<span data-ttu-id="1ab28-210">Přesun dat pomocí aktivity kopírování</span><span class="sxs-lookup"><span data-stu-id="1ab28-210">Move data by using Copy Activity</span></span>](../data-factory/data-factory-data-movement-activities.md)
- [<span data-ttu-id="1ab28-211">Přesun dat do a z Azure SQL Data Warehouse pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="1ab28-211">Move data to and from Azure SQL Data Warehouse using Azure Data Factory</span></span>](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

<span data-ttu-id="1ab28-212">Chcete-li prozkoumat vaše data v SQL Data Warehouse, najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="1ab28-212">To explore your data in SQL Data Warehouse, see the following articles:</span></span>

- [<span data-ttu-id="1ab28-213">Připojení k SQL Data Warehouse pomocí sady Visual Studio a rozšíření SSDT</span><span class="sxs-lookup"><span data-stu-id="1ab28-213">Connect to SQL Data Warehouse with Visual Studio and SSDT</span></span>](sql-data-warehouse-query-visual-studio.md)
- <span data-ttu-id="1ab28-214">[Vizuální data s Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="1ab28-214">[Visual data with Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span></span>

<!-- Azure references -->
[Azure Portal]: https://portal.azure.com
