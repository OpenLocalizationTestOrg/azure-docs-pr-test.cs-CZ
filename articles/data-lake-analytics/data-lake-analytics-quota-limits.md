---
title: "aaaAzure Data Lake Analytics kvótami | Microsoft Docs"
description: "Zjistěte, jak tooadjust a zvyšte kvótu omezení v Azure Data Lake Analytics (ADLA) účty."
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: omidm
ms.openlocfilehash: 875c4d00e0c57414031e50754495c02162bdca48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a><span data-ttu-id="602e4-104">Maximální kvóty Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="602e4-104">Azure Data Lake Analytics Quota Limits</span></span>

<span data-ttu-id="602e4-105">Zjistěte, jak tooadjust a zvyšte kvótu omezení v Azure Data Lake Analytics (ADLA) účty.</span><span class="sxs-lookup"><span data-stu-id="602e4-105">Learn how tooadjust and increase quota limits in Azure Data Lake Analytics (ADLA) accounts.</span></span> <span data-ttu-id="602e4-106">Znalost těchto mezních hodnot vám mohou pomoci porozumět chování vaší úlohy U-SQL.</span><span class="sxs-lookup"><span data-stu-id="602e4-106">Knowing these limits may help you understand your U-SQL job behavior.</span></span> <span data-ttu-id="602e4-107">Kvótami všechny jsou logicky, takže může zvýšit maximální limit hello oslovit toous.</span><span class="sxs-lookup"><span data-stu-id="602e4-107">All quota limits are soft, so you can increase hello maximum limits by reaching out toous.</span></span>

## <a name="azure-subscriptions-limits"></a><span data-ttu-id="602e4-108">Omezení předplatných Azure</span><span class="sxs-lookup"><span data-stu-id="602e4-108">Azure subscriptions limits</span></span>

<span data-ttu-id="602e4-109">**Maximální počet ADLA účty podle předplatného:** 5</span><span class="sxs-lookup"><span data-stu-id="602e4-109">**Maximum number of ADLA accounts per subscription:**  5</span></span>

 <span data-ttu-id="602e4-110">Toto je maximální počet ADLA účty, které můžete vytvořit na jedno předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="602e4-110">This is hello maximum number of ADLA accounts you can create per subscription.</span></span> <span data-ttu-id="602e4-111">Pokud se pokusíte toocreate šestý ADLA účet, bude dojde k chybě "Bylo dosaženo maximální počet hello účty Data Lake Analytics, povolené (5) v oblasti pod názvem odběru".</span><span class="sxs-lookup"><span data-stu-id="602e4-111">If you try toocreate a sixth ADLA account, you will get an error "You have reached hello maximum number of Data Lake Analytics accounts allowed (5) in region under subscription name".</span></span> <span data-ttu-id="602e4-112">V takovém případě Odstraňte nepoužívané účty ADLA nebo oslovení toous podle [otevřením lístku podpory](#increase-maximum-quota-limits).</span><span class="sxs-lookup"><span data-stu-id="602e4-112">In this case, either delete any unused ADLA accounts, or reach out toous by [opening a support ticket](#increase-maximum-quota-limits).</span></span>

## <a name="adla-account-limits"></a><span data-ttu-id="602e4-113">Limity účtu ADLA</span><span class="sxs-lookup"><span data-stu-id="602e4-113">ADLA account limits</span></span>

<span data-ttu-id="602e4-114">**Maximální počet jednotek Analytics (Austrálie) na účet:** 250</span><span class="sxs-lookup"><span data-stu-id="602e4-114">**Maximum number of Analytics Units (AUs) per account:** 250</span></span>

<span data-ttu-id="602e4-115">Toto je maximální počet hello Austrálie, které můžou běžet současně ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="602e4-115">This is hello maximum number of AUs that can run concurrently in your account.</span></span> <span data-ttu-id="602e4-116">Pokud váš celkový počet spuštění Austrálie pro všechny úlohy překračuje tento limit, novější úlohy se zařadí do fronty automaticky.</span><span class="sxs-lookup"><span data-stu-id="602e4-116">If your total running AUs across all jobs exceeds this limit, newer jobs are queued automatically.</span></span> <span data-ttu-id="602e4-117">Například:</span><span class="sxs-lookup"><span data-stu-id="602e4-117">For example:</span></span>

* <span data-ttu-id="602e4-118">Pokud máte pouze jednu úlohu s 250 Austrálie při odesílání druhý úlohy se budou čekat ve frontě úloh hello hello první úloha dokončena.</span><span class="sxs-lookup"><span data-stu-id="602e4-118">If you have only one job running with 250 AUs, when you submit a second job it will wait in hello job queue until hello first job completes.</span></span>
* <span data-ttu-id="602e4-119">Pokud již máte pět spuštěné úlohy a každý používá 50 Austrálie při odesílání šesté úlohu, která potřebuje 20 Austrálie čeká ve frontě úloh hello, dokud jsou 20 Austrálie k dispozici.</span><span class="sxs-lookup"><span data-stu-id="602e4-119">If you already have five jobs running and each is using 50 AUs, when you submit a sixth job that needs 20 AUs it waits in hello job queue until there are 20 AUs available.</span></span>

<span data-ttu-id="602e4-120">**Maximální počet souběžných úloh U-SQL na účet:** 20</span><span class="sxs-lookup"><span data-stu-id="602e4-120">**Maximum number of concurrent U-SQL jobs per account:** 20</span></span>

<span data-ttu-id="602e4-121">Toto je maximální počet úloh, které můžou běžet současně ve vašem účtu hello.</span><span class="sxs-lookup"><span data-stu-id="602e4-121">This is hello maximum number of jobs that can run concurrently in your account.</span></span> <span data-ttu-id="602e4-122">Vyšší než tato hodnota novější úlohy se zařadí do fronty automaticky.</span><span class="sxs-lookup"><span data-stu-id="602e4-122">Above this value, newer jobs are queued automatically.</span></span>

## <a name="adjust-adla-quota-limits-per-account"></a><span data-ttu-id="602e4-123">Upravit ADLA kvótami každý účet</span><span class="sxs-lookup"><span data-stu-id="602e4-123">Adjust ADLA quota limits per account</span></span>

1. <span data-ttu-id="602e4-124">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="602e4-124">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="602e4-125">Zvolte existující účet ADLA.</span><span class="sxs-lookup"><span data-stu-id="602e4-125">Choose an existing ADLA account.</span></span>
3. <span data-ttu-id="602e4-126">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="602e4-126">Click **Properties**.</span></span>
4. <span data-ttu-id="602e4-127">Upravit **paralelismus** a **souběžných úloh** toosuit vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="602e4-127">Adjust **Parallelism** and **Concurrent Jobs** toosuit your needs.</span></span>

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a><span data-ttu-id="602e4-129">Zvýšit maximální kvóty</span><span class="sxs-lookup"><span data-stu-id="602e4-129">Increase maximum quota limits</span></span>

1. <span data-ttu-id="602e4-130">Otevřete žádost o podporu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="602e4-130">Open a support request in Azure Portal.</span></span>

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. <span data-ttu-id="602e4-133">Vyberte typ problému hello **kvóty**.</span><span class="sxs-lookup"><span data-stu-id="602e4-133">Select hello issue type **Quota**.</span></span>
3. <span data-ttu-id="602e4-134">Vyberte vaše **předplatné** (ujistěte se, není "zkušební" předplatné).</span><span class="sxs-lookup"><span data-stu-id="602e4-134">Select your **Subscription** (make sure it is not a "trial" subscription).</span></span>
4. <span data-ttu-id="602e4-135">Vyberte typ kvóty **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="602e4-135">Select quota type **Data Lake Analytics**.</span></span>

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. <span data-ttu-id="602e4-137">V okně problém hello popisují požadovanou zvyšte limit s **podrobnosti** z Proč potřebujete tuto kapacitu navíc.</span><span class="sxs-lookup"><span data-stu-id="602e4-137">In hello problem blade, explain your requested increase limit with **Details** of why you need this extra capacity.</span></span>

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. <span data-ttu-id="602e4-139">Ověřte kontaktní informace a vytvořte žádost o podporu hello.</span><span class="sxs-lookup"><span data-stu-id="602e4-139">Verify your contact information and create hello support request.</span></span>

<span data-ttu-id="602e4-140">Společnost Microsoft nezkontroluje vaši žádost a pokusí tooaccommodate vaše podnikání co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="602e4-140">Microsoft reviews your request and tries tooaccommodate your business needs as soon as possible.</span></span>

## <a name="next-steps"></a><span data-ttu-id="602e4-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="602e4-141">Next steps</span></span>

* [<span data-ttu-id="602e4-142">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="602e4-142">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="602e4-143">Správa Azure Data Lake Analytics pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="602e4-143">Manage Azure Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-manage-use-powershell.md)
* [<span data-ttu-id="602e4-144">Monitorování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="602e4-144">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
