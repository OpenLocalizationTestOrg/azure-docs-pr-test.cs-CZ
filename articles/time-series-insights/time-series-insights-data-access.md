---
title: "zásady přístupu aaaData v Azure časové řady přehledy | Microsoft Docs"
description: "V tomto kurzu zjistíte, toomanage zásady přístupu k datům v přehledy časové řady"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: f286d26c8c5c851c523e9384760dc4b10711fa3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="grant-data-access-tooa-time-series-insights-environment-using-azure-portal"></a><span data-ttu-id="1a837-103">Udělení přístupu tooa časové řady Statistika prostředí datového pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1a837-103">Grant data access tooa Time Series Insights environment using Azure portal</span></span>

<span data-ttu-id="1a837-104">V prostředích Time Series Insights jsou dva nezávislé typy zásad přístupu:</span><span class="sxs-lookup"><span data-stu-id="1a837-104">Time Series Insights environments have two independent types of access policies:</span></span>

* <span data-ttu-id="1a837-105">Zásady přístupu ke správě</span><span class="sxs-lookup"><span data-stu-id="1a837-105">Management access policies</span></span>
* <span data-ttu-id="1a837-106">Zásady přístupu k datům</span><span class="sxs-lookup"><span data-stu-id="1a837-106">Data access policies</span></span>

<span data-ttu-id="1a837-107">Oba typy zásad udělují objektům zabezpečení Azure Active Directory (uživatelům a aplikacím) různá oprávnění ke konkrétnímu prostředí.</span><span class="sxs-lookup"><span data-stu-id="1a837-107">Both policies grant Azure Active Directory principals (users and apps) various permissions on a particular environment.</span></span> <span data-ttu-id="1a837-108">Hello objekty (uživatelé a aplikace) musí patřit toohello active directory (nebo "Klientovi Azure") přidružené k předplatnému hello obsahující hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="1a837-108">hello principals (users and apps) must belong toohello active directory (or “Azure tenant”) associated with hello subscription containing hello environment.</span></span>

<span data-ttu-id="1a837-109">Zásady správy přístupu udělit oprávnění související toohello konfiguraci hello prostředí, například</span><span class="sxs-lookup"><span data-stu-id="1a837-109">Management access policies grant permissions related toohello configuration of hello environment, such as</span></span>
*   <span data-ttu-id="1a837-110">Vytváření a odstraňování hello prostředí zdroje událostí referenční datové sady, a</span><span class="sxs-lookup"><span data-stu-id="1a837-110">Creation and deletion of hello environment, event sources, reference data sets, and</span></span>
*   <span data-ttu-id="1a837-111">Správa hello zásady přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="1a837-111">Management of hello data access policies.</span></span>

<span data-ttu-id="1a837-112">Zásady přístupu k datům udělit oprávnění tooissue dotazy na data, pracovat s daty odkaz v prostředí hello a sdílet uložené dotazy a perspektivy přidružený hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="1a837-112">Data access policies grant permissions tooissue data queries, manipulate reference data in hello environment, and share saved queries and perspectives associated with hello environment.</span></span>

<span data-ttu-id="1a837-113">Hello dva druhy zásad povolí jasně oddělené mezi toohello správu přístupu hello prostředí a přístup k datům toohello uvnitř hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="1a837-113">hello two kinds of policies allow clear separation between access toohello management of hello environment and access toohello data inside hello environment.</span></span> <span data-ttu-id="1a837-114">Například je možné toosetup prostředí tak, aby hello vlastníka nebo autora hello prostředí se odebere z přístupu k datům hello.</span><span class="sxs-lookup"><span data-stu-id="1a837-114">For example, it is possible toosetup an environment such that hello owner/creator of hello environment is removed from hello data access.</span></span> <span data-ttu-id="1a837-115">A také uživatelům a službám, které jsou povoleny tooread data z prostředí hello může být poskytnuta žádná konfigurace toohello přístup hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="1a837-115">As well as users and services that are allowed tooread data from hello environment may be granted no access toohello configuration of hello environment.</span></span>

## <a name="grant-data-access"></a><span data-ttu-id="1a837-116">Udělení přístupu k datům</span><span class="sxs-lookup"><span data-stu-id="1a837-116">Grant data access</span></span>
<span data-ttu-id="1a837-117">Hello následující kroky ukazují, jak přístup k datům toogrant pro hlavní název uživatele:</span><span class="sxs-lookup"><span data-stu-id="1a837-117">hello following steps show how toogrant data access for a user principal:</span></span>

1.  <span data-ttu-id="1a837-118">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1a837-118">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="1a837-119">V nabídce hello na levé straně hello hello portálu Azure klikněte na tlačítko "Všechny prostředky".</span><span class="sxs-lookup"><span data-stu-id="1a837-119">Click “All resources” in hello menu on hello left side of hello Azure portal.</span></span>
3.  <span data-ttu-id="1a837-120">Vyberte vaše prostředí Time Series Insights.</span><span class="sxs-lookup"><span data-stu-id="1a837-120">Select your Time Series Insights environment.</span></span>

  ![Spravovat hello časové řady Statistika zdroj - prostředí](media/data-access/getstarted-grant-data-access1.png)

4.  <span data-ttu-id="1a837-122">Vyberte Přístup k rovině dat a klikněte na Přidat.</span><span class="sxs-lookup"><span data-stu-id="1a837-122">Select “Data Plane Access”, click “Add”</span></span>

  ![Spravovat zdroje časové řady Statistika hello – přidání](media/data-access/getstarted-grant-data-access2.png)

5.  <span data-ttu-id="1a837-124">Klikněte na Vybrat uživatele.</span><span class="sxs-lookup"><span data-stu-id="1a837-124">Click “Select user”.</span></span>
6.  <span data-ttu-id="1a837-125">Vyhledat a vybrat uživatele hello e-mailem.</span><span class="sxs-lookup"><span data-stu-id="1a837-125">Search and select user by hello email.</span></span>
7.  <span data-ttu-id="1a837-126">Klikněte na Vybrat v okně Vybrat uživatele.</span><span class="sxs-lookup"><span data-stu-id="1a837-126">Click “Select” in “Select User” blade.</span></span>

  ![Spravovat hello časové řady Statistika zdroj - vybrat uživatele](media/data-access/getstarted-grant-data-access3.png)

8.  <span data-ttu-id="1a837-128">Klikněte na Vybrat roli.</span><span class="sxs-lookup"><span data-stu-id="1a837-128">Click “Select role”.</span></span>
9.  <span data-ttu-id="1a837-129">Pokud chcete tooallow uživatele toochange referenční data a sdílet s jinými uživateli hello prostředí uložené dotazy a perspektivy, vyberte "Přispěvatel".</span><span class="sxs-lookup"><span data-stu-id="1a837-129">Select “Contributor” if you want tooallow user toochange reference data and share saved queries and perspectives with other users of hello environment.</span></span> <span data-ttu-id="1a837-130">V opačném případě vyberte "Čtečky" tooallow uživatele dotaz na data v prostředí hello a ukládejte dotazy za osobní (není sdílený) v prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="1a837-130">Otherwise select “Reader” tooallow user query data in hello environment and save personal (not shared) queries in hello environment.</span></span>
10. <span data-ttu-id="1a837-131">V okně "Vybrat roli" hello, klikněte na tlačítko "Ok".</span><span class="sxs-lookup"><span data-stu-id="1a837-131">Click “Ok” in hello “Select Role” blade.</span></span>

  ![Spravovat hello časové řady Statistika zdrojem – vyberte role](media/data-access/getstarted-grant-data-access4.png)

11. <span data-ttu-id="1a837-133">V okně "Vybrat roli uživatele" hello, klikněte na tlačítko "Ok".</span><span class="sxs-lookup"><span data-stu-id="1a837-133">Click “Ok” in hello “Select User Role” blade.</span></span>
12. <span data-ttu-id="1a837-134">Měli byste vidět tohle:</span><span class="sxs-lookup"><span data-stu-id="1a837-134">You should see:</span></span>

  ![Spravovat hello časové řady Statistika zdrojem – výsledky](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a><span data-ttu-id="1a837-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a837-136">Next steps</span></span>

* [<span data-ttu-id="1a837-137">Vytvoření zdroje událostí</span><span class="sxs-lookup"><span data-stu-id="1a837-137">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="1a837-138">[Odesílání událostí](time-series-insights-send-events.md) toohello zdroj události</span><span class="sxs-lookup"><span data-stu-id="1a837-138">[Send events](time-series-insights-send-events.md) toohello event source</span></span>
* <span data-ttu-id="1a837-139">Zobrazení prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="1a837-139">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
