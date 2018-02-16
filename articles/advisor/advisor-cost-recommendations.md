---
title: "Azure doporučení služby Advisor náklady | Microsoft Docs"
description: "SipiTest - Advisor Azure používání za účelem optimalizace náklady na nasazení Azure."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 3af81ba94d97ea34e4cf92d6e8e96b28cd012a76
ms.sourcegitcommit: 57921d51ea018e6d8aa6676b3793f5e5a6402a63
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="advisor-cost-recommendations"></a><span data-ttu-id="cfff7-103">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="cfff7-103">Advisor Cost recommendations</span></span>
<span data-ttu-id="cfff7-104">To je upravit pro testování HT/MT přepínače.</span><span class="sxs-lookup"><span data-stu-id="cfff7-104">This is edited for testing HT/MT switch.</span></span>
<span data-ttu-id="cfff7-105">Pomáhá Advisor optimalizovat a snížit vaše celkové Azure tráví určením nečinnosti a nedostatečně prostředky.</span><span class="sxs-lookup"><span data-stu-id="cfff7-105">Advisor helps you optimize and reduce your overall Azure spend by identifying idle and underutilized resources.</span></span> <span data-ttu-id="cfff7-106">Můžete získat náklady doporučení z **náklady** na řídicím panelu služby Advisor.</span><span class="sxs-lookup"><span data-stu-id="cfff7-106">You can get cost recommendations from the **Cost** tab on the Advisor dashboard.</span></span>

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a><span data-ttu-id="cfff7-107">Optimalizace tráví virtuálního počítače nebo změnou velikosti vypíná nedostatečně instancí</span><span class="sxs-lookup"><span data-stu-id="cfff7-107">Optimize virtual machine spend by resizing or shutting down underutilized instances</span></span> 
<span data-ttu-id="cfff7-108">I když některé scénáře aplikací může být nízkou míru využívání návrhu, můžete často ušetřit peníze pomocí správy velikost a počet virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cfff7-108">Although certain application scenarios can result in low utilization by design, you can often save money by managing the size and number of your virtual machines.</span></span> <span data-ttu-id="cfff7-109">Advisor monitoruje vaše využití virtuálního počítače po dobu 14 dnů a pak identifikuje nízké využití virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cfff7-109">Advisor monitors your virtual machine usage for 14 days and then identifies low-utilization virtual machines.</span></span> <span data-ttu-id="cfff7-110">Virtuální počítače, jejichž využití procesoru je 5 % nebo méně a využití sítě je 7 MB nebo méně čtyři nebo více dní jsou považovány za nízké využití virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cfff7-110">Virtual machines whose CPU utilization is 5 percent or less and network usage is 7 MB or less for four or more days are considered low-utilization virtual machines.</span></span>

<span data-ttu-id="cfff7-111">Advisor zobrazí odhadované náklady pokračovat ke spuštění virtuálního počítače, takže je možné ho vypnout, nebo jeho velikost.</span><span class="sxs-lookup"><span data-stu-id="cfff7-111">Advisor shows you the estimated cost of continuing to run your virtual machine, so that you can choose to shut it down or resize it.</span></span>

<span data-ttu-id="cfff7-112">Pokud chcete být agresivnější zjistit nedostatečně virtuální počítače, můžete upravit průměrná pravidlo využití procesoru na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="cfff7-112">If you want to be more aggressive at identifying underutilized virtual machines, you can adjust the average CPU utilization rule on a per subscription basis.</span></span>

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a><span data-ttu-id="cfff7-113">Použít nákladově efektivní řešení ke správě výkonnostní cíle více databází SQL</span><span class="sxs-lookup"><span data-stu-id="cfff7-113">Use a cost effective solution to manage performance goals of multiple SQL databases</span></span>
<span data-ttu-id="cfff7-114">Advisor identifikuje instance serveru SQL, které můžete využít možnost vytvoření fondů elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="cfff7-114">Advisor identifies SQL server instances that can benefit from creating elastic database pools.</span></span> <span data-ttu-id="cfff7-115">Fondy elastické databáze poskytují jednoduché a nákladově efektivní řešení pro správu výkonu cílů více databází, které mají různou vzorce používání.</span><span class="sxs-lookup"><span data-stu-id="cfff7-115">Elastic database pools provide a simple, cost-effective solution to manage the performance goals of multiple databases that have varying usage patterns.</span></span> <span data-ttu-id="cfff7-116">Další informace o Azure elastické fondy najdete v tématu [co je Azure Elastickém fondu?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span><span class="sxs-lookup"><span data-stu-id="cfff7-116">For more information about Azure elastic pools, see [What is an Azure Elastic pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span></span>

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a><span data-ttu-id="cfff7-117">Snížení nákladů odstraněním není zřízený okruhy ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="cfff7-117">Reduce costs by eliminating unprovisioned ExpressRoute circuits</span></span>
<span data-ttu-id="cfff7-118">Advisor identifikuje okruhy ExpressRoute, které byly ve stavu zprostředkovatele *není zajišťováno* pro více než jeden měsíc a doporučuje odstraňování okruh, pokud nemáte v plánu poskytnutí okruhu připojení k Zprostředkovatel.</span><span class="sxs-lookup"><span data-stu-id="cfff7-118">Advisor identifies ExpressRoute circuits that have been in the provider status of *Not Provisioned* for more than one month, and recommends deleting the circuit if you aren't planning to provision the circuit with your connectivity provider.</span></span>

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a><span data-ttu-id="cfff7-119">Jak získat přístup k náklady na doporučení v Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="cfff7-119">How to access Cost recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="cfff7-120">Přihlaste se k [portál Azure](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="cfff7-120">Sign in to the [Azure portal](https://portal.azure.com), and then open [Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2.  <span data-ttu-id="cfff7-121">Na řídicím panelu služby Advisor, klikněte na **náklady** kartě.</span><span class="sxs-lookup"><span data-stu-id="cfff7-121">On the Advisor dashboard, click the **Cost** tab.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cfff7-122">Další postup</span><span class="sxs-lookup"><span data-stu-id="cfff7-122">Next steps</span></span>

<span data-ttu-id="cfff7-123">Další informace o doporučení služby Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="cfff7-123">To learn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="cfff7-124">Úvod do služby Advisor</span><span class="sxs-lookup"><span data-stu-id="cfff7-124">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="cfff7-125">Začínáme</span><span class="sxs-lookup"><span data-stu-id="cfff7-125">Get Started</span></span>](advisor-get-started.md)
* <span data-ttu-id="cfff7-126">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="cfff7-126">[Advisor Performance recommendations](advisor-cost-recommendations.md)</span></span>
* <span data-ttu-id="cfff7-127">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="cfff7-127">[Advisor High Availability recommendations](advisor-cost-recommendations.md)</span></span>
* <span data-ttu-id="cfff7-128">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="cfff7-128">[Advisor Security recommendations](advisor-cost-recommendations.md)</span></span>
