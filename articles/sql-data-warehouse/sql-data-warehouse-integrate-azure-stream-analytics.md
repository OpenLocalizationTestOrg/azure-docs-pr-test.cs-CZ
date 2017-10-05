---
title: "Použití služby Azure Stream Analytics s SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 14783f0464764a11d7f03a5db1c2d63728a4cb50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a><span data-ttu-id="37c7d-103">Použití služby Azure Stream Analytics s SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="37c7d-103">Use Azure Stream Analytics with SQL Data Warehouse</span></span>
<span data-ttu-id="37c7d-104">Azure Stream Analytics je plně spravovaná služba poskytuje nízkou latencí, vysoce dostupných, škálovatelných komplexní zpracování událostí přes streamování dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="37c7d-104">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable complex event processing over streaming data in the cloud.</span></span> <span data-ttu-id="37c7d-105">Seznámíte se základy načtením [Úvod do služby Azure Stream Analytics][Introduction to Azure Stream Analytics].</span><span class="sxs-lookup"><span data-stu-id="37c7d-105">You can learn the basics by reading [Introduction to Azure Stream Analytics][Introduction to Azure Stream Analytics].</span></span> <span data-ttu-id="37c7d-106">Můžete pak zjistíte, jak vytvořit-komplexní řešení pomocí služby Stream Analytics podle [začít používat Azure Stream Analytics] [ Get started using Azure Stream Analytics] kurzu.</span><span class="sxs-lookup"><span data-stu-id="37c7d-106">You can then learn how to create an end-to-end solution with Stream Analytics by following the [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>

<span data-ttu-id="37c7d-107">V tomto článku se dozvíte, jak používat databázi Azure SQL Data Warehouse jako výstupní jímku pro datový proud Analytics úlohy.</span><span class="sxs-lookup"><span data-stu-id="37c7d-107">In this article, you will learn how to use your Azure SQL Data Warehouse database as an output sink for your Steam Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37c7d-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="37c7d-108">Prerequisites</span></span>
<span data-ttu-id="37c7d-109">Nejprve spusťte pomocí následujících kroků v [začít používat Azure Stream Analytics] [ Get started using Azure Stream Analytics] kurzu.</span><span class="sxs-lookup"><span data-stu-id="37c7d-109">First, run through the following steps in the [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>  

1. <span data-ttu-id="37c7d-110">Vytvoření centra událostí vstup</span><span class="sxs-lookup"><span data-stu-id="37c7d-110">Create an Event Hub input</span></span>
2. <span data-ttu-id="37c7d-111">Nakonfigurujte a spusťte aplikaci generátor událostí</span><span class="sxs-lookup"><span data-stu-id="37c7d-111">Configure and start event generator application</span></span>
3. <span data-ttu-id="37c7d-112">Zřídit úloha Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="37c7d-112">Provision a Stream Analytics job</span></span>
4. <span data-ttu-id="37c7d-113">Zadejte vstup úlohy a dotaz</span><span class="sxs-lookup"><span data-stu-id="37c7d-113">Specify job input and query</span></span>

<span data-ttu-id="37c7d-114">Pak vytvořte databázi Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="37c7d-114">Then, create an Azure SQL Data Warehouse database</span></span>

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a><span data-ttu-id="37c7d-115">Zadejte výstup úlohy: databáze Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="37c7d-115">Specify job output: Azure SQL Data Warehouse database</span></span>
### <a name="step-1"></a><span data-ttu-id="37c7d-116">Krok 1</span><span class="sxs-lookup"><span data-stu-id="37c7d-116">Step 1</span></span>
<span data-ttu-id="37c7d-117">V úloze Stream Analytics klikněte na tlačítko **výstup** z horní části stránky a pak klikněte na tlačítko **přidat výstup**.</span><span class="sxs-lookup"><span data-stu-id="37c7d-117">In your Stream Analytics job click **OUTPUT** from the top of the page, and then click **ADD OUTPUT**.</span></span>

### <a name="step-2"></a><span data-ttu-id="37c7d-118">Krok 2</span><span class="sxs-lookup"><span data-stu-id="37c7d-118">Step 2</span></span>
<span data-ttu-id="37c7d-119">Vyberte databázi SQL a klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="37c7d-119">Select SQL Database and click next.</span></span>

![][add-output]

### <a name="step-3"></a><span data-ttu-id="37c7d-120">Krok 3</span><span class="sxs-lookup"><span data-stu-id="37c7d-120">Step 3</span></span>
<span data-ttu-id="37c7d-121">Na další stránce zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="37c7d-121">Enter the following values on the next page:</span></span>

* <span data-ttu-id="37c7d-122">*Výstup Alias*: Zadejte popisný název pro tento výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="37c7d-122">*Output Alias*: Enter a friendly name for this job output.</span></span>
* <span data-ttu-id="37c7d-123">*Předplatné*:</span><span class="sxs-lookup"><span data-stu-id="37c7d-123">*Subscription*:</span></span>
  * <span data-ttu-id="37c7d-124">Pokud vaše databáze SQL Data Warehouse je ve stejném předplatném jako úlohu služby Stream Analytics, vyberte použít databázi SQL z aktuálního předplatného.</span><span class="sxs-lookup"><span data-stu-id="37c7d-124">If your SQL Data Warehouse database is in the same subscription as the Stream Analytics job, select Use SQL Database from Current Subscription.</span></span>
  * <span data-ttu-id="37c7d-125">Pokud vaše databáze je v jiném předplatném, vyberte použít SQL Database z jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="37c7d-125">If your database is in a different subscription, select Use SQL Database from Another Subscription.</span></span>
* <span data-ttu-id="37c7d-126">*Databáze*: Zadejte název cílové databáze.</span><span class="sxs-lookup"><span data-stu-id="37c7d-126">*Database*: Specify the name of a destination database.</span></span>
* <span data-ttu-id="37c7d-127">*Název serveru*: Zadejte název serveru pro databázi, který jste právě určili.</span><span class="sxs-lookup"><span data-stu-id="37c7d-127">*Server Name*: Specify the server name for the database you just specified.</span></span> <span data-ttu-id="37c7d-128">Portál Azure Classic můžete použít k vyhledání to.</span><span class="sxs-lookup"><span data-stu-id="37c7d-128">You can use the Azure Classic Portal to find this.</span></span>

![][server-name]

* <span data-ttu-id="37c7d-129">*Uživatelské jméno*: Zadejte uživatelské jméno účtu, který má oprávnění k zápisu pro databázi.</span><span class="sxs-lookup"><span data-stu-id="37c7d-129">*User Name*: Specify the user name of an account that has write permissions for the database.</span></span>
* <span data-ttu-id="37c7d-130">*Heslo*: Zadejte heslo pro zadaný uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="37c7d-130">*Password*: Provide the password for the specified user account.</span></span>
* <span data-ttu-id="37c7d-131">*Tabulka*: Zadejte název cílové tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="37c7d-131">*Table*: Specify the name of the target table in the database.</span></span>

![][add-database]

### <a name="step-4"></a><span data-ttu-id="37c7d-132">Krok 4</span><span class="sxs-lookup"><span data-stu-id="37c7d-132">Step 4</span></span>
<span data-ttu-id="37c7d-133">Klikněte na tlačítko zaškrtnutí chcete přidat tento výstup úlohy a ověřte, zda Stream Analytics můžete úspěšně připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="37c7d-133">Click the check button to add this job output and to verify that Stream Analytics can successfully connect to the database.</span></span>

![][test-connection]

<span data-ttu-id="37c7d-134">Pokud je připojení k databázi úspěšná, zobrazí se oznámení v dolní části portálu.</span><span class="sxs-lookup"><span data-stu-id="37c7d-134">When the connection to the database succeeds, you will see a notification at the bottom of the portal.</span></span> <span data-ttu-id="37c7d-135">Kliknutí na tlačítko Testovat připojení v dolní části, abyste otestovali připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="37c7d-135">You can click Test Connection at the bottom to test the connection to the database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37c7d-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37c7d-136">Next steps</span></span>
<span data-ttu-id="37c7d-137">Přehled integrace najdete v tématu [Přehled integrace SQL Data Warehouse][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="37c7d-137">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>

<span data-ttu-id="37c7d-138">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="37c7d-138">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction to Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
