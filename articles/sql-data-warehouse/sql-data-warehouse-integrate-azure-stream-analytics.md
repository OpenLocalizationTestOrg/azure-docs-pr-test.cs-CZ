---
title: aaaUse Azure Stream Analytics s SQL Data Warehouse | Microsoft Docs
description: "Tipy pro používání Azure Stream Analytics s datovým skladem SQL Azure pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 8aeb2247-20c5-4a29-b327-30a8ce09dfdc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1278197a6764864124fd92fc672de00b83ec343f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a><span data-ttu-id="119e9-103">Použití služby Azure Stream Analytics s SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="119e9-103">Use Azure Stream Analytics with SQL Data Warehouse</span></span>
<span data-ttu-id="119e9-104">Azure Stream Analytics je plně spravovaná služba poskytuje nízkou latencí, vysoce dostupných, škálovatelných komplexní zpracování událostí přes streamování dat v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="119e9-104">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable complex event processing over streaming data in hello cloud.</span></span> <span data-ttu-id="119e9-105">Dozvíte se základy hello načtením [tooAzure Úvod Stream Analytics][Introduction tooAzure Stream Analytics].</span><span class="sxs-lookup"><span data-stu-id="119e9-105">You can learn hello basics by reading [Introduction tooAzure Stream Analytics][Introduction tooAzure Stream Analytics].</span></span> <span data-ttu-id="119e9-106">Potom dozvíte, jak toocreate řešení začátku do konce pomocí služby Stream Analytics podle hello [začít používat Azure Stream Analytics] [ Get started using Azure Stream Analytics] kurzu.</span><span class="sxs-lookup"><span data-stu-id="119e9-106">You can then learn how toocreate an end-to-end solution with Stream Analytics by following hello [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>

<span data-ttu-id="119e9-107">V tomto článku se dozvíte, jak toouse Azure SQL Data Warehouse databáze jako výstupní jímku pro datový proud Analytics úlohy.</span><span class="sxs-lookup"><span data-stu-id="119e9-107">In this article, you will learn how toouse your Azure SQL Data Warehouse database as an output sink for your Steam Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="119e9-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="119e9-108">Prerequisites</span></span>
<span data-ttu-id="119e9-109">Nejprve spustit prostřednictvím hello proveďte kroky v hello [začít používat Azure Stream Analytics] [ Get started using Azure Stream Analytics] kurzu.</span><span class="sxs-lookup"><span data-stu-id="119e9-109">First, run through hello following steps in hello [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>  

1. <span data-ttu-id="119e9-110">Vytvoření centra událostí vstup</span><span class="sxs-lookup"><span data-stu-id="119e9-110">Create an Event Hub input</span></span>
2. <span data-ttu-id="119e9-111">Nakonfigurujte a spusťte aplikaci generátor událostí</span><span class="sxs-lookup"><span data-stu-id="119e9-111">Configure and start event generator application</span></span>
3. <span data-ttu-id="119e9-112">Zřídit úloha Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="119e9-112">Provision a Stream Analytics job</span></span>
4. <span data-ttu-id="119e9-113">Zadejte vstup úlohy a dotaz</span><span class="sxs-lookup"><span data-stu-id="119e9-113">Specify job input and query</span></span>

<span data-ttu-id="119e9-114">Pak vytvořte databázi Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="119e9-114">Then, create an Azure SQL Data Warehouse database</span></span>

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a><span data-ttu-id="119e9-115">Zadejte výstup úlohy: databáze Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="119e9-115">Specify job output: Azure SQL Data Warehouse database</span></span>
### <a name="step-1"></a><span data-ttu-id="119e9-116">Krok 1</span><span class="sxs-lookup"><span data-stu-id="119e9-116">Step 1</span></span>
<span data-ttu-id="119e9-117">V úloze Stream Analytics klikněte na tlačítko **výstup** shora hello hello stránky a pak klikněte na tlačítko **přidat výstup**.</span><span class="sxs-lookup"><span data-stu-id="119e9-117">In your Stream Analytics job click **OUTPUT** from hello top of hello page, and then click **ADD OUTPUT**.</span></span>

### <a name="step-2"></a><span data-ttu-id="119e9-118">Krok 2</span><span class="sxs-lookup"><span data-stu-id="119e9-118">Step 2</span></span>
<span data-ttu-id="119e9-119">Vyberte databázi SQL a klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="119e9-119">Select SQL Database and click next.</span></span>

![][add-output]

### <a name="step-3"></a><span data-ttu-id="119e9-120">Krok 3</span><span class="sxs-lookup"><span data-stu-id="119e9-120">Step 3</span></span>
<span data-ttu-id="119e9-121">Zadejte následující hodnoty na další stránku hello hello:</span><span class="sxs-lookup"><span data-stu-id="119e9-121">Enter hello following values on hello next page:</span></span>

* <span data-ttu-id="119e9-122">*Výstup Alias*: Zadejte popisný název pro tento výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="119e9-122">*Output Alias*: Enter a friendly name for this job output.</span></span>
* <span data-ttu-id="119e9-123">*Předplatné*:</span><span class="sxs-lookup"><span data-stu-id="119e9-123">*Subscription*:</span></span>
  * <span data-ttu-id="119e9-124">Pokud je databáze SQL Data Warehouse v hello stejnému předplatnému jako hello úlohy služby Stream Analytics, vyberte použít databázi SQL z aktuálního předplatného.</span><span class="sxs-lookup"><span data-stu-id="119e9-124">If your SQL Data Warehouse database is in hello same subscription as hello Stream Analytics job, select Use SQL Database from Current Subscription.</span></span>
  * <span data-ttu-id="119e9-125">Pokud vaše databáze je v jiném předplatném, vyberte použít SQL Database z jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="119e9-125">If your database is in a different subscription, select Use SQL Database from Another Subscription.</span></span>
* <span data-ttu-id="119e9-126">*Databáze*: Zadejte název hello cílové databáze.</span><span class="sxs-lookup"><span data-stu-id="119e9-126">*Database*: Specify hello name of a destination database.</span></span>
* <span data-ttu-id="119e9-127">*Název serveru*: Zadejte název serveru hello hello databáze, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="119e9-127">*Server Name*: Specify hello server name for hello database you just specified.</span></span> <span data-ttu-id="119e9-128">Tímto způsobem můžete portál Azure Classic toofind hello.</span><span class="sxs-lookup"><span data-stu-id="119e9-128">You can use hello Azure Classic Portal toofind this.</span></span>

![][server-name]

* <span data-ttu-id="119e9-129">*Uživatelské jméno*: Zadejte hello uživatelské jméno účtu, který má oprávnění k zápisu pro databázi hello.</span><span class="sxs-lookup"><span data-stu-id="119e9-129">*User Name*: Specify hello user name of an account that has write permissions for hello database.</span></span>
* <span data-ttu-id="119e9-130">*Heslo*: Zadejte heslo hello hello zadaný uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="119e9-130">*Password*: Provide hello password for hello specified user account.</span></span>
* <span data-ttu-id="119e9-131">*Tabulka*: Zadejte název hello hello cílové tabulky v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="119e9-131">*Table*: Specify hello name of hello target table in hello database.</span></span>

![][add-database]

### <a name="step-4"></a><span data-ttu-id="119e9-132">Krok 4</span><span class="sxs-lookup"><span data-stu-id="119e9-132">Step 4</span></span>
<span data-ttu-id="119e9-133">Tento výstup úlohy a tooverify Stream Analytics můžete úspěšně připojit toohello databáze, klikněte na tlačítko tooadd tlačítko Kontrola hello.</span><span class="sxs-lookup"><span data-stu-id="119e9-133">Click hello check button tooadd this job output and tooverify that Stream Analytics can successfully connect toohello database.</span></span>

![][test-connection]

<span data-ttu-id="119e9-134">Pokud je databáze toohello připojení hello úspěšná, zobrazí se oznámení v dolním hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="119e9-134">When hello connection toohello database succeeds, you will see a notification at hello bottom of hello portal.</span></span> <span data-ttu-id="119e9-135">Test připojení můžete kliknutím na hello dolní tootest hello připojení toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="119e9-135">You can click Test Connection at hello bottom tootest hello connection toohello database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="119e9-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="119e9-136">Next steps</span></span>
<span data-ttu-id="119e9-137">Přehled integrace najdete v tématu [Přehled integrace SQL Data Warehouse][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="119e9-137">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>

<span data-ttu-id="119e9-138">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="119e9-138">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction tooAzure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
