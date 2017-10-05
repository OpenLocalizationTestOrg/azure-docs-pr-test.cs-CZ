---
title: Zaregistrovat v Azure Data Catalog dat z Data Lake Store | Microsoft Docs
description: Zaregistrovat v Azure Data Catalog dat z Data Lake Store
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 41f7ca0d638479b013e77cb949a71c566bc66aa4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a><span data-ttu-id="0212e-103">Zaregistrovat v Azure Data Catalog dat z Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="0212e-103">Register data from Data Lake Store in Azure Data Catalog</span></span>
<span data-ttu-id="0212e-104">V tomto článku se dozvíte, jak integrovat Azure Data Lake Store s Azure Data Catalog zjistitelnost vaše data v rámci organizace integrací s katalogem Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="0212e-104">In this article you will learn how to integrate Azure Data Lake Store with Azure Data Catalog to make your data discoverable within an organization by integrating it with Data Catalog.</span></span> <span data-ttu-id="0212e-105">Další informace o vytváření katalogu dat najdete v tématu [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="0212e-105">For more information on cataloging data, see [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md).</span></span> <span data-ttu-id="0212e-106">Chcete-li pochopit scénáře, ve které můžete použít Data Catalog, přečtěte si téma [běžné scénáře Azure Data Catalog](../data-catalog/data-catalog-common-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="0212e-106">To understand scenarios in which you can use Data Catalog, see [Azure Data Catalog common scenarios](../data-catalog/data-catalog-common-scenarios.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0212e-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0212e-107">Prerequisites</span></span>
<span data-ttu-id="0212e-108">Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="0212e-108">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="0212e-109">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="0212e-109">**An Azure subscription**.</span></span> <span data-ttu-id="0212e-110">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0212e-110">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="0212e-111">**Aktivujte předplatné Azure** pro verzi Public Preview Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="0212e-111">**Enable your Azure subscription** for Data Lake Store Public Preview.</span></span> <span data-ttu-id="0212e-112">Viz [pokyny](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0212e-112">See [instructions](data-lake-store-get-started-portal.md).</span></span>
* <span data-ttu-id="0212e-113">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="0212e-113">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="0212e-114">Postupujte podle pokynů v tématu [Začínáme s Azure Data Lake Store s použitím webu Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0212e-114">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="0212e-115">V tomto kurzu, dejte nám vytvořit účet Data Lake Store s názvem **datacatalogstore**.</span><span class="sxs-lookup"><span data-stu-id="0212e-115">For this tutorial, let us create a Data Lake Store account called **datacatalogstore**.</span></span>

    <span data-ttu-id="0212e-116">Po vytvoření účtu tím, že nahrajete ukázková datové sady do ní.</span><span class="sxs-lookup"><span data-stu-id="0212e-116">Once you have created the account, upload a sample data set to it.</span></span> <span data-ttu-id="0212e-117">V tomto kurzu, dejte nám odeslat všechny soubory .csv v **AmbulanceData** složku [úložiště Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/).</span><span class="sxs-lookup"><span data-stu-id="0212e-117">For this tutorial, let us upload all the .csv files under the **AmbulanceData** folder in the [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/).</span></span> <span data-ttu-id="0212e-118">Můžete používat různé klienty, jako třeba [Azure Storage Explorer](http://storageexplorer.com/), odesílat data do kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0212e-118">You can use various clients, such as [Azure Storage Explorer](http://storageexplorer.com/), to upload data to a blob container.</span></span>
* <span data-ttu-id="0212e-119">**Azure Data Catalog**.</span><span class="sxs-lookup"><span data-stu-id="0212e-119">**Azure Data Catalog**.</span></span> <span data-ttu-id="0212e-120">Vaše organizace již musí mít Azure Data Catalog pro vaši organizaci vytvořený.</span><span class="sxs-lookup"><span data-stu-id="0212e-120">Your organization must already have an Azure Data Catalog created for your organization.</span></span> <span data-ttu-id="0212e-121">Pro každou organizaci, je povolen pouze jeden katalog.</span><span class="sxs-lookup"><span data-stu-id="0212e-121">Only one catalog is allowed for each organization.</span></span>

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a><span data-ttu-id="0212e-122">Registrace Data Lake Store jako zdroj pro katalog Data Catalog</span><span class="sxs-lookup"><span data-stu-id="0212e-122">Register Data Lake Store as a source for Data Catalog</span></span>

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. <span data-ttu-id="0212e-123">Přejděte na `https://azure.microsoft.com/services/data-catalog`a klikněte na tlačítko **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="0212e-123">Go to `https://azure.microsoft.com/services/data-catalog`, and click **Get started**.</span></span>
2. <span data-ttu-id="0212e-124">Přihlaste se k portálu Azure Data Catalog a klikněte na tlačítko **publikovat data**.</span><span class="sxs-lookup"><span data-stu-id="0212e-124">Log into the Azure Data Catalog portal, and click **Publish data**.</span></span>

    <span data-ttu-id="0212e-125">![Registrace zdroje dat](./media/data-lake-store-with-data-catalog/register-data-source.png "registrace zdroje dat")</span><span class="sxs-lookup"><span data-stu-id="0212e-125">![Register a data source](./media/data-lake-store-with-data-catalog/register-data-source.png "Register a data source")</span></span>
3. <span data-ttu-id="0212e-126">Na další stránce klikněte na tlačítko **spustit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="0212e-126">On the next page, click **Launch Application**.</span></span> <span data-ttu-id="0212e-127">To se stažení souboru manifestu aplikace ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0212e-127">This will download the application manifest file on your computer.</span></span> <span data-ttu-id="0212e-128">Poklikejte na soubor manifestu a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0212e-128">Double-click the manifest file to start the application.</span></span>
4. <span data-ttu-id="0212e-129">Na úvodní stránce klikněte na tlačítko **přihlášení**a zadejte svá pověření.</span><span class="sxs-lookup"><span data-stu-id="0212e-129">On the Welcome page, click **Sign in**, and enter your credentials.</span></span>

    <span data-ttu-id="0212e-130">![Úvodní obrazovka](./media/data-lake-store-with-data-catalog/welcome.screen.png "úvodní obrazovce")</span><span class="sxs-lookup"><span data-stu-id="0212e-130">![Welcome screen](./media/data-lake-store-with-data-catalog/welcome.screen.png "Welcome screen")</span></span>
5. <span data-ttu-id="0212e-131">Na stránce vyberte zdroj dat vyberte **Azure Data Lake**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0212e-131">On the Select a Data Source page, select **Azure Data Lake**, and then click **Next**.</span></span>

    <span data-ttu-id="0212e-132">![Vyberte zdroj dat](./media/data-lake-store-with-data-catalog/select-source.png "vyberte zdroj dat")</span><span class="sxs-lookup"><span data-stu-id="0212e-132">![Select data source](./media/data-lake-store-with-data-catalog/select-source.png "Select data source")</span></span>
6. <span data-ttu-id="0212e-133">Na další stránce zadejte název účtu Data Lake Store, který chcete zaregistrovat v katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="0212e-133">On the next page, provide the Data Lake Store account name that you want to register in Data Catalog.</span></span> <span data-ttu-id="0212e-134">Nechte ostatní možnosti jako výchozí a pak klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="0212e-134">Leave the other options as default and then click **Connect**.</span></span>

    <span data-ttu-id="0212e-135">![Připojení ke zdroji dat](./media/data-lake-store-with-data-catalog/connect-to-source.png "připojit ke zdroji dat")</span><span class="sxs-lookup"><span data-stu-id="0212e-135">![Connect to data source](./media/data-lake-store-with-data-catalog/connect-to-source.png "Connect to data source")</span></span>
7. <span data-ttu-id="0212e-136">Na další stránku, je možné rozdělit do následující segmenty.</span><span class="sxs-lookup"><span data-stu-id="0212e-136">The next page can be divided into the following segments.</span></span>

    <span data-ttu-id="0212e-137">a.</span><span class="sxs-lookup"><span data-stu-id="0212e-137">a.</span></span> <span data-ttu-id="0212e-138">**Hierarchii serverů** pole představuje struktura složek účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="0212e-138">The **Server Hierarchy** box represents the Data Lake Store account folder structure.</span></span> <span data-ttu-id="0212e-139">**$Root** reprezentuje kořen účet Data Lake Store, a **AmbulanceData** představuje složku vytvořit v kořenové složce účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="0212e-139">**$Root** represents the Data Lake Store account root, and **AmbulanceData** represents the folder created in the root of the Data Lake Store account.</span></span>

    <span data-ttu-id="0212e-140">b.</span><span class="sxs-lookup"><span data-stu-id="0212e-140">b.</span></span> <span data-ttu-id="0212e-141">**Dostupné objekty** pole obsahuje seznam souborů a složek **AmbulanceData** složky.</span><span class="sxs-lookup"><span data-stu-id="0212e-141">The **Available objects** box lists the files and folders under the **AmbulanceData** folder.</span></span>

    <span data-ttu-id="0212e-142">c.</span><span class="sxs-lookup"><span data-stu-id="0212e-142">c.</span></span> <span data-ttu-id="0212e-143">**Objekty, které chcete být registrovaný pole** obsahuje soubory a složky, které chcete zaregistrovat v Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="0212e-143">**Objects to be registered box** lists the files and folders that you want to register in Azure Data Catalog.</span></span>

    <span data-ttu-id="0212e-144">![Zobrazit datové struktury](./media/data-lake-store-with-data-catalog/view-data-structure.png "zobrazit datové struktury")</span><span class="sxs-lookup"><span data-stu-id="0212e-144">![View data structure](./media/data-lake-store-with-data-catalog/view-data-structure.png "View data structure")</span></span>
8. <span data-ttu-id="0212e-145">V tomto kurzu byste měli zaregistrovat všechny soubory v adresáři.</span><span class="sxs-lookup"><span data-stu-id="0212e-145">For this tutorial, you should register all the files in the directory.</span></span> <span data-ttu-id="0212e-146">Kliknutím (![přesun objektů](./media/data-lake-store-with-data-catalog/move-objects.png "přesun objektů")) přesuňte všechny soubory pro **objekty k registraci** pole.</span><span class="sxs-lookup"><span data-stu-id="0212e-146">For that, click the (![move objects](./media/data-lake-store-with-data-catalog/move-objects.png "Move objects")) button to move all the files to **Objects to be registered** box.</span></span>

    <span data-ttu-id="0212e-147">Vzhledem k tomu, že data budou zaregistrovány ve katalog dat pro celou organizaci, je doporučená přístup přidat některé metadata, která později můžete rychle vyhledat potřebná data.</span><span class="sxs-lookup"><span data-stu-id="0212e-147">Because the data will be registered in an organization-wide data catalog, it is a recommened approach to add some metadata which you can later use to quickly locate the data.</span></span> <span data-ttu-id="0212e-148">Například můžete přidat e-mailovou adresu pro vlastník dat (například jeden, který odesílá data) nebo přidat značku identifikovat data.</span><span class="sxs-lookup"><span data-stu-id="0212e-148">For example, you can add an e-mail address for the data owner (for example, one who is uploading the data) or add a tag to identify the data.</span></span> <span data-ttu-id="0212e-149">Snímek obrazovky níže ukazuje značku přidáme k datům.</span><span class="sxs-lookup"><span data-stu-id="0212e-149">The screen capture below shows a tag that we add to the data.</span></span>

    <span data-ttu-id="0212e-150">![Zobrazit datové struktury](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "zobrazit datové struktury")</span><span class="sxs-lookup"><span data-stu-id="0212e-150">![View data structure](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "View data structure")</span></span>

    <span data-ttu-id="0212e-151">Klikněte na tlačítko **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="0212e-151">Click **Register**.</span></span>
9. <span data-ttu-id="0212e-152">Následující snímek obrazovky označuje, že data se úspěšně zaregistrován v katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="0212e-152">The following screen capture denotes that the data is successfully registered in the Data Catalog.</span></span>

    <span data-ttu-id="0212e-153">![Registrace je dokončena](./media/data-lake-store-with-data-catalog/registration-complete.png "zobrazit datové struktury")</span><span class="sxs-lookup"><span data-stu-id="0212e-153">![Registration complete](./media/data-lake-store-with-data-catalog/registration-complete.png "View data structure")</span></span>
10. <span data-ttu-id="0212e-154">Klikněte na tlačítko **zobrazit portál** přejít zpět na portál pro katalog Data Catalog a ověřte, že jste registrované datové teď přístup z portálu.</span><span class="sxs-lookup"><span data-stu-id="0212e-154">Click **View Portal** to go back to the Data Catalog portal and verify that you can now access the registered data from the portal.</span></span> <span data-ttu-id="0212e-155">Pokud chcete hledat data, můžete použít značky, které jste použili při registraci data.</span><span class="sxs-lookup"><span data-stu-id="0212e-155">To search the data, you can use the tag you used while registering the data.</span></span>

     <span data-ttu-id="0212e-156">![Vyhledávání dat v katalogu](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "vyhledávání dat v katalogu")</span><span class="sxs-lookup"><span data-stu-id="0212e-156">![Search data in catalog](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Search data in catalog")</span></span>
11. <span data-ttu-id="0212e-157">Teď můžete dělat operace, jako je přidání poznámky a dokumentaci k datům.</span><span class="sxs-lookup"><span data-stu-id="0212e-157">You can now perform operations like adding annotations and documentation to the data.</span></span> <span data-ttu-id="0212e-158">Další informace najdete v následujících tématech.</span><span class="sxs-lookup"><span data-stu-id="0212e-158">For more information, see the following links.</span></span>

    * [<span data-ttu-id="0212e-159">Přidání poznámek ke zdrojům dat v katalogu Data Catalog</span><span class="sxs-lookup"><span data-stu-id="0212e-159">Annotate data sources in Data Catalog</span></span>](../data-catalog/data-catalog-how-to-annotate.md)
    * [<span data-ttu-id="0212e-160">Zdroje dat dokumentu v katalogu Data Catalog</span><span class="sxs-lookup"><span data-stu-id="0212e-160">Document data sources in Data Catalog</span></span>](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a><span data-ttu-id="0212e-161">Viz také</span><span class="sxs-lookup"><span data-stu-id="0212e-161">See also</span></span>
* [<span data-ttu-id="0212e-162">Přidání poznámek ke zdrojům dat v katalogu Data Catalog</span><span class="sxs-lookup"><span data-stu-id="0212e-162">Annotate data sources in Data Catalog</span></span>](../data-catalog/data-catalog-how-to-annotate.md)
* [<span data-ttu-id="0212e-163">Zdroje dat dokumentu v katalogu Data Catalog</span><span class="sxs-lookup"><span data-stu-id="0212e-163">Document data sources in Data Catalog</span></span>](../data-catalog/data-catalog-how-to-documentation.md)
* [<span data-ttu-id="0212e-164">Integrace s jinými službami Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="0212e-164">Integrate Data Lake Store with other Azure services</span></span>](data-lake-store-integrate-with-other-services.md)