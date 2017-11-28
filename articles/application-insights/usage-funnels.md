---
title: "aaaAzure nálevky Application Insights"
description: "Zjistěte, jak můžete pomocí nálevky toodiscover jak zákazníci komunikují s vaší aplikací."
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
ms.openlocfilehash: 2a6125cf596570cfaee30bb3ff757916e90d7676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="discover-how-customers-are-using-your-application-with-hello-application-insights-funnels"></a><span data-ttu-id="1950b-103">Zjistit, jak zákazníci používají aplikace s hello nálevky Application Insights</span><span class="sxs-lookup"><span data-stu-id="1950b-103">Discover how customers are using your application with hello Application Insights Funnels</span></span>

<span data-ttu-id="1950b-104">Principy zkušeností zákazníků je hello velký důraz tooyour firmy.</span><span class="sxs-lookup"><span data-stu-id="1950b-104">Understanding customer experience is of hello utmost importance tooyour business.</span></span> <span data-ttu-id="1950b-105">Pokud vaše aplikace zahrnuje několik fází, tooknow je nutné, pokud se většina zákazníků pokročíte prostřednictvím hello celý proces, nebo pokud jsou jejich ukončení procesu hello v určitém okamžiku.</span><span class="sxs-lookup"><span data-stu-id="1950b-105">If your application involves multiple stages, you will need tooknow if most customers are progressing through hello entire process, or if they are ending hello process at some point.</span></span> <span data-ttu-id="1950b-106">rozšiřování Hello prostřednictvím sérii kroků ve webové aplikaci se označuje jako "trychtýřového grafu".</span><span class="sxs-lookup"><span data-stu-id="1950b-106">hello progression through a series of steps in a web application is known as a "funnel".</span></span> <span data-ttu-id="1950b-107">Můžete použít hello Application Insights nálevky toogain přehledy o vašich uživatelů a monitorování podrobné převod sazby.</span><span class="sxs-lookup"><span data-stu-id="1950b-107">You can use hello Application Insights Funnels toogain insights into your users and monitor step-by-step conversion rates.</span></span> 

## <a name="get-started-with-hello-funnels-blade"></a><span data-ttu-id="1950b-108">Začínáme s okno nálevky hello</span><span class="sxs-lookup"><span data-stu-id="1950b-108">Get started with hello Funnels blade</span></span>
<span data-ttu-id="1950b-109">Hello nejjednodušší způsob, jak toolearn o nálevky je toowalk, když příklad.</span><span class="sxs-lookup"><span data-stu-id="1950b-109">hello easiest way toolearn about Funnels is toowalk though an example.</span></span> <span data-ttu-id="1950b-110">Hello následující obrázky ukazují hello vlastníci kroky elektronické obchodování bude trvat, než se toolearn jak zákazníkům komunikovat s touto webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="1950b-110">hello following illustrations demonstrate hello steps owners of an e-commerce business would take toolearn how their customers interact with their web application.</span></span>  

### <a name="create-your-funnel"></a><span data-ttu-id="1950b-111">Vytvoření vaší trychtýřového grafu</span><span class="sxs-lookup"><span data-stu-id="1950b-111">Create your funnel</span></span>
<span data-ttu-id="1950b-112">Než vytvoříte vaší trychtýřového grafu, je třeba toodecide na otázku hello chcete tooanswer.</span><span class="sxs-lookup"><span data-stu-id="1950b-112">Before you create your funnel, you need toodecide on hello question you want tooanswer.</span></span> <span data-ttu-id="1950b-113">Například můžete tooknow jak mnoho zákazníků zobrazení vaší domovské stránce kliknutím na oznámení o inzerovaném programu.</span><span class="sxs-lookup"><span data-stu-id="1950b-113">For example, you might want tooknow how many customers viewing your home page click on an advertisement.</span></span> <span data-ttu-id="1950b-114">V tomto příkladu má hello vlastníci hello společnosti Fabrikam Fiber tooknow hello podíl zákazníci, kteří nákup po přidání položky tootheir nákupní košík během hello poslední měsíc.</span><span class="sxs-lookup"><span data-stu-id="1950b-114">In this example, hello owners of hello Fabrikam Fiber company want tooknow hello percentage of customers who make a purchase after adding items tootheir shopping cart during hello last month.</span></span>

<span data-ttu-id="1950b-115">Tady jsou kroky hello jejich trvat toocreate jejich trychtýřového grafu.</span><span class="sxs-lookup"><span data-stu-id="1950b-115">Here are hello steps they take toocreate their funnel.</span></span>

1. <span data-ttu-id="1950b-116">Klikněte na tlačítko Nový hello v okně nálevky hello.</span><span class="sxs-lookup"><span data-stu-id="1950b-116">Click hello New button on hello Funnels blade.</span></span>
1. <span data-ttu-id="1950b-117">Vyberte časové rozmezí hello "Poslední měsíc" z hello **časový rozsah** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1950b-117">Select hello time range of "Last month" from hello **Time Range** drop-down.</span></span> 
1. <span data-ttu-id="1950b-118">Vyberte hello **stránky produktu** událost z hello **kroku 1** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1950b-118">Select hello **Product page** event from hello **Step 1** drop-down list.</span></span> 
1. <span data-ttu-id="1950b-119">Vyberte hello **přidat tooshopping košíku** událost z hello **kroku 2** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1950b-119">Select hello **Add tooshopping cart** event from hello **Step 2** drop-down list.</span></span>
1. <span data-ttu-id="1950b-120">Vyberte hello **kliknutím na nákup** událost z hello **krok 3** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1950b-120">Select hello **Click purchase** event from hello **Step 3** drop-down list.</span></span>
1. <span data-ttu-id="1950b-121">Přidat trychtýřového grafu s názvem toohello a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1950b-121">Add a name toohello funnel and click **Save**.</span></span>

<span data-ttu-id="1950b-122">Hello následující obrázek ukazuje, že generuje okno nálevky hello dat hello.</span><span class="sxs-lookup"><span data-stu-id="1950b-122">hello following illustration demonstrates hello data hello Funnels blade generates.</span></span> <span data-ttu-id="1950b-123">Z hello sem můžete zobrazit vlastníci společnosti Fabrikam, během posledního týdne hello, 22.7 % zákazníků, kteří přidáni tootheir položky nákupního košíku dokončené hello nákupu.</span><span class="sxs-lookup"><span data-stu-id="1950b-123">From here hello Fabrikam owners can see that during hello last week, 22.7% of their customers who added an item tootheir shopping cart completed hello purchase.</span></span> <span data-ttu-id="1950b-124">Můžete také zobrazit, 1 % hello zákazníků před hostujících stránku hello produktů a 20 % zákazníkům odhlášení po dokončení jejich nákup kliknutí na oznámení o inzerovaném programu.</span><span class="sxs-lookup"><span data-stu-id="1950b-124">They can also see that 1% of hello customers clicked an advertisement before visiting hello product page, and 20% of their customers signed out after completing their purchase.</span></span>


![Okno nálevky s daty](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a><span data-ttu-id="1950b-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1950b-126">Next steps</span></span>
  * [<span data-ttu-id="1950b-127">Přehled využití</span><span class="sxs-lookup"><span data-stu-id="1950b-127">Usage overview</span></span>](app-insights-usage-overview.md)
  * [<span data-ttu-id="1950b-128">Uživatelů, relací a události</span><span class="sxs-lookup"><span data-stu-id="1950b-128">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
  * [<span data-ttu-id="1950b-129">Uchování</span><span class="sxs-lookup"><span data-stu-id="1950b-129">Retention</span></span>](app-insights-usage-retention.md)
  * [<span data-ttu-id="1950b-130">Workbooks</span><span class="sxs-lookup"><span data-stu-id="1950b-130">Workbooks</span></span>](app-insights-usage-workbooks.md)
  * [<span data-ttu-id="1950b-131">Přidat uživatelský kontext</span><span class="sxs-lookup"><span data-stu-id="1950b-131">Add user context</span></span>](app-insights-usage-send-user-context.md)
