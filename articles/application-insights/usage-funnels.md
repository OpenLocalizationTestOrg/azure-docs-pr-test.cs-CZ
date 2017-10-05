---
title: "Nálevky Azure Application Insights"
description: "Zjistěte, jak nálevky můžete zjistit, jak jsou zákazníci interakci s vaší aplikací."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 59c4dfafc102b26e3b9873f433065715f4aec9ec
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a><span data-ttu-id="0fb85-103">Zjistit, jak zákazníci používají aplikace s nálevky statistiky aplikace</span><span class="sxs-lookup"><span data-stu-id="0fb85-103">Discover how customers are using your application with the Application Insights Funnels</span></span>

<span data-ttu-id="0fb85-104">Principy zkušeností zákazníků je nesmírně důležité k vaší firmě.</span><span class="sxs-lookup"><span data-stu-id="0fb85-104">Understanding customer experience is of the utmost importance to your business.</span></span> <span data-ttu-id="0fb85-105">Pokud vaše aplikace zahrnuje několik fází, musíte vědět, pokud se většina zákazníků pokročíte prostřednictvím celý proces, nebo pokud jsou jejich ukončení procesu v určitém okamžiku.</span><span class="sxs-lookup"><span data-stu-id="0fb85-105">If your application involves multiple stages, you will need to know if most customers are progressing through the entire process, or if they are ending the process at some point.</span></span> <span data-ttu-id="0fb85-106">Průběh prostřednictvím sérii kroků ve webové aplikaci se označuje jako "trychtýřového grafu".</span><span class="sxs-lookup"><span data-stu-id="0fb85-106">The progression through a series of steps in a web application is known as a "funnel".</span></span> <span data-ttu-id="0fb85-107">Můžete nálevky Statistika aplikaci a získáte přehled o vašich uživatelů a monitorování podrobné převod sazby.</span><span class="sxs-lookup"><span data-stu-id="0fb85-107">You can use the Application Insights Funnels to gain insights into your users and monitor step-by-step conversion rates.</span></span> 

## <a name="get-started-with-the-funnels-blade"></a><span data-ttu-id="0fb85-108">Začínáme s nálevky okna</span><span class="sxs-lookup"><span data-stu-id="0fb85-108">Get started with the Funnels blade</span></span>
<span data-ttu-id="0fb85-109">Nejjednodušší způsob, jak získat informace o nálevky je provedeme příklad.</span><span class="sxs-lookup"><span data-stu-id="0fb85-109">The easiest way to learn about Funnels is to walk though an example.</span></span> <span data-ttu-id="0fb85-110">Následující ilustrace ukazují vlastníci kroky elektronické obchodování bude trvat, než se dozvíte, jak zákazníkům komunikovat s touto webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="0fb85-110">The following illustrations demonstrate the steps owners of an e-commerce business would take to learn how their customers interact with their web application.</span></span>  

### <a name="create-your-funnel"></a><span data-ttu-id="0fb85-111">Vytvoření vaší trychtýřového grafu</span><span class="sxs-lookup"><span data-stu-id="0fb85-111">Create your funnel</span></span>
<span data-ttu-id="0fb85-112">Než vytvoříte vaší trychtýřového grafu, musíte rozhodnout na otázku, kterou chcete odpovědět.</span><span class="sxs-lookup"><span data-stu-id="0fb85-112">Before you create your funnel, you need to decide on the question you want to answer.</span></span> <span data-ttu-id="0fb85-113">Například můžete chtít vědět, jak mnoho zákazníků zobrazení vaší domovské stránce kliknutím na oznámení o inzerovaném programu.</span><span class="sxs-lookup"><span data-stu-id="0fb85-113">For example, you might want to know how many customers viewing your home page click on an advertisement.</span></span> <span data-ttu-id="0fb85-114">V tomto příkladu vlastníci společnosti Fabrikam Fiber zajímat, do jaké procento zákazníci, kteří nákup po přidání položky do jejich nákupní košík během poslední měsíc.</span><span class="sxs-lookup"><span data-stu-id="0fb85-114">In this example, the owners of the Fabrikam Fiber company want to know the percentage of customers who make a purchase after adding items to their shopping cart during the last month.</span></span>

<span data-ttu-id="0fb85-115">Tady jsou kroky, které budou chtít vytvořit jejich trychtýřového grafu.</span><span class="sxs-lookup"><span data-stu-id="0fb85-115">Here are the steps they take to create their funnel.</span></span>

1. <span data-ttu-id="0fb85-116">Klikněte na tlačítko Nový v okně nálevky.</span><span class="sxs-lookup"><span data-stu-id="0fb85-116">Click the New button on the Funnels blade.</span></span>
1. <span data-ttu-id="0fb85-117">Vyberte časové rozmezí "Poslední měsíc" z **časový rozsah** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0fb85-117">Select the time range of "Last month" from the **Time Range** drop-down.</span></span> 
1. <span data-ttu-id="0fb85-118">Vyberte **stránky produktu** událost z **kroku 1** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0fb85-118">Select the **Product page** event from the **Step 1** drop-down list.</span></span> 
1. <span data-ttu-id="0fb85-119">Vyberte **přidat nákupního košíku** událost z **kroku 2** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0fb85-119">Select the **Add to shopping cart** event from the **Step 2** drop-down list.</span></span>
1. <span data-ttu-id="0fb85-120">Vyberte **kliknutím na nákup** událost z **krok 3** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0fb85-120">Select the **Click purchase** event from the **Step 3** drop-down list.</span></span>
1. <span data-ttu-id="0fb85-121">Přidat název do trychtýřového grafu a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0fb85-121">Add a name to the funnel and click **Save**.</span></span>

<span data-ttu-id="0fb85-122">Následující obrázek ukazuje, že data v okně nálevky generuje.</span><span class="sxs-lookup"><span data-stu-id="0fb85-122">The following illustration demonstrates the data the Funnels blade generates.</span></span> <span data-ttu-id="0fb85-123">Tady Fabrikam můžete zobrazit vlastníky během posledního týdne dokončit 22.7 % zákazníků, kteří přidat položku do jejich nákupní košík nákupu.</span><span class="sxs-lookup"><span data-stu-id="0fb85-123">From here the Fabrikam owners can see that during the last week, 22.7% of their customers who added an item to their shopping cart completed the purchase.</span></span> <span data-ttu-id="0fb85-124">Můžete také zobrazit, 1 % zákazníkům před hostujících stránky produktu a 20 % zákazníkům odhlášení po dokončení jejich nákup kliknutí na oznámení o inzerovaném programu.</span><span class="sxs-lookup"><span data-stu-id="0fb85-124">They can also see that 1% of the customers clicked an advertisement before visiting the product page, and 20% of their customers signed out after completing their purchase.</span></span>


![Okno nálevky s daty](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a><span data-ttu-id="0fb85-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fb85-126">Next steps</span></span>
  * [<span data-ttu-id="0fb85-127">Přehled využití</span><span class="sxs-lookup"><span data-stu-id="0fb85-127">Usage overview</span></span>](app-insights-usage-overview.md)
  * [<span data-ttu-id="0fb85-128">Uživatelů, relací a události</span><span class="sxs-lookup"><span data-stu-id="0fb85-128">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
  * [<span data-ttu-id="0fb85-129">Uchování</span><span class="sxs-lookup"><span data-stu-id="0fb85-129">Retention</span></span>](app-insights-usage-retention.md)
  * [<span data-ttu-id="0fb85-130">Workbooks</span><span class="sxs-lookup"><span data-stu-id="0fb85-130">Workbooks</span></span>](app-insights-usage-workbooks.md)
  * [<span data-ttu-id="0fb85-131">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="0fb85-131">Add user context</span></span>](app-insights-usage-send-user-context.md)
