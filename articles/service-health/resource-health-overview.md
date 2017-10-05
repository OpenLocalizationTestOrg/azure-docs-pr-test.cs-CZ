---
title: "Přehled Azure Resource health | Microsoft Docs"
description: "Přehled o stavu prostředků Azure"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/01/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: 040d58a81a9b41fe660e4276d698bf884f90bb6c
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="azure-resource-health-overview"></a><span data-ttu-id="12955-103">Přehled stavu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="12955-103">Azure resource health overview</span></span>
 
<span data-ttu-id="12955-104">Resource Health pomáhá při diagnostice a získání podpory v případě, že potíže s Azure ovlivňují vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="12955-104">Resource health helps you diagnose and get support when an Azure issue impacts your resources.</span></span> <span data-ttu-id="12955-105">Informuje o aktuálním a dřívějším stavu prostředků a pomáhá zmírnit problémy.</span><span class="sxs-lookup"><span data-stu-id="12955-105">It informs you about the current and past health of your resources and helps you mitigate issues.</span></span> <span data-ttu-id="12955-106">Resource Health poskytuje technickou podporu, když potřebujete pomoc při potížích se službami Azure.</span><span class="sxs-lookup"><span data-stu-id="12955-106">Resource health provides technical support when you need help with Azure service issues.</span></span>

<span data-ttu-id="12955-107">Zatímco [stavu Azure](https://status.azure.com) informující o problémů služby, které ovlivňují širokou škálu Azure zákazníků, stav prostředku vám poskytne přizpůsobené řídicí panel stavu prostředků.</span><span class="sxs-lookup"><span data-stu-id="12955-107">Whereas [Azure Status](https://status.azure.com) informs you about service issues that affect a broad set of Azure customers, resource health provides you with a personalized dashboard of the health of your resources.</span></span> <span data-ttu-id="12955-108">Stav prostředků ukazuje všechny časy, které prostředky byly v minulosti kvůli problémům s služby Azure k dispozici.</span><span class="sxs-lookup"><span data-stu-id="12955-108">Resource health shows you all the times your resources were unavailable in the past due to Azure service issues.</span></span> <span data-ttu-id="12955-109">Zjednodušuje porozumět, pokud byl došlo k porušení SLA.</span><span class="sxs-lookup"><span data-stu-id="12955-109">This makes it simple for you to understand if an SLA was violated.</span></span> 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a><span data-ttu-id="12955-110">Co se považuje za zdroj a jak funguje stav prostředku rozhodne, zda prostředek je v pořádku, či nikoliv?</span><span class="sxs-lookup"><span data-stu-id="12955-110">What is considered a resource and how does resource health decides if a resource is healthy or not?</span></span>
<span data-ttu-id="12955-111">Prostředek se o instanci typu prostředku, které nabízí služby Azure pomocí Azure Resource Manager, například: virtuálního počítače, webovou aplikaci nebo v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="12955-111">A resource is an instance of a resource type offered by an Azure service through Azure Resource Manager, for example: a virtual machine, a web app, or a SQL database.</span></span>

<span data-ttu-id="12955-112">Stav prostředku spoléhá na signály vysílaných různých služeb Azure k vyhodnocení, pokud prostředek je v pořádku, či nikoli.</span><span class="sxs-lookup"><span data-stu-id="12955-112">Resource health relies on signals emitted by the different Azure services to assess if a resource is healthy or not.</span></span> <span data-ttu-id="12955-113">Pokud prostředek není v pořádku, analyzuje stav prostředku dodatečné informace k určení příčiny problému.</span><span class="sxs-lookup"><span data-stu-id="12955-113">If a resource is unhealthy, resource health analyzes additional information to determine the source of the problem.</span></span> <span data-ttu-id="12955-114">Také určuje opatření Microsoft trvá vyřešit problém, nebo jaké akce může trvat vyřešit příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="12955-114">It also identifies actions Microsoft is taking to fix the issue or what actions you can take to address the cause of the problem.</span></span> 

<span data-ttu-id="12955-115">Úplný seznam typů prostředků a stavu zkontroluje [stavu prostředků Azure](resource-health-checks-resource-types.md) další podrobnosti o tom, jak se hodnotí stavu.</span><span class="sxs-lookup"><span data-stu-id="12955-115">Review the full list of resource types and health checks in [Azure resource health](resource-health-checks-resource-types.md) for additional details on how health is assessed.</span></span>

## <a name="health-status-provided-by-resource-health"></a><span data-ttu-id="12955-116">Stav poskytované stav prostředků</span><span class="sxs-lookup"><span data-stu-id="12955-116">Health status provided by resource health</span></span>
<span data-ttu-id="12955-117">Stav prostředku je jedním z následujících stavů:</span><span class="sxs-lookup"><span data-stu-id="12955-117">The health of a resource is one of the following statuses:</span></span>

### <a name="available"></a><span data-ttu-id="12955-118">Dostupné</span><span class="sxs-lookup"><span data-stu-id="12955-118">Available</span></span>
<span data-ttu-id="12955-119">Služba zjistila všechny události, které mají vliv stav prostředku.</span><span class="sxs-lookup"><span data-stu-id="12955-119">The service has not detected any events impacting the health of the resource.</span></span> <span data-ttu-id="12955-120">V případech, kdy prostředek se obnovila neplánované výpadky během posledních 24 hodin se zobrazí **nedávno obnovena** oznámení.</span><span class="sxs-lookup"><span data-stu-id="12955-120">In cases where the resource has recovered from unplanned downtime during the last 24 hours you will see the **recently recovered** notification.</span></span>

![Prostředek stavu k dispozici virtuálního počítače](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a><span data-ttu-id="12955-122">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="12955-122">Unavailable</span></span>
<span data-ttu-id="12955-123">Služba zjistila probíhající platformy nebo jiné platformy událost stavu prostředku, které mají vliv.</span><span class="sxs-lookup"><span data-stu-id="12955-123">The service has detected an ongoing platform or non-platform event impacting the health of the resource.</span></span>

#### <a name="platform-events"></a><span data-ttu-id="12955-124">Události platformy</span><span class="sxs-lookup"><span data-stu-id="12955-124">Platform events</span></span>
<span data-ttu-id="12955-125">Tyto události jsou aktivovány více součástí infrastruktury Azure a jsou oba plánované akce, jako je plánované údržby a neočekávané incidenty jako restartování neplánované hostitele.</span><span class="sxs-lookup"><span data-stu-id="12955-125">These events are triggered by multiple components of the Azure infrastructure and include both scheduled actions like planned maintenance and unexpected incidents like an unplanned host reboot.</span></span>

<span data-ttu-id="12955-126">Stav prostředku poskytuje další podrobnosti o události procesu obnovení a můžete se obrátit na podporu i v případě, že nemáte active podpory smlouvy společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="12955-126">Resource health provides additional details on the event, the recovery process and enables you to contact support even if you don't have an active Microsoft support agreement.</span></span>

![Prostředek stavu není k dispozici virtuální počítač z důvodu události platformy](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a><span data-ttu-id="12955-128">Události jiné platformy</span><span class="sxs-lookup"><span data-stu-id="12955-128">Non-Platform events</span></span>
<span data-ttu-id="12955-129">Tyto události jsou aktivovány akce prováděné uživateli, například zastavení virtuálního počítače nebo dosažení maximálního počtu připojení k Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="12955-129">These events are triggered by actions taken by users, for example stopping a virtual machine or reaching the maximum number of connections to a Redis Cache.</span></span>

![Prostředek stavu není k dispozici virtuální počítač z důvodu události jiné platformy](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a><span data-ttu-id="12955-131">Neznámý</span><span class="sxs-lookup"><span data-stu-id="12955-131">Unknown</span></span>
<span data-ttu-id="12955-132">Tento stav označuje, že stav prostředku nedostal informace o tento prostředek pro více než 10 minut.</span><span class="sxs-lookup"><span data-stu-id="12955-132">This health status indicates that resource health has not received information about this resource for more than 10 minutes.</span></span> <span data-ttu-id="12955-133">Přestože tento stav není spolehlivý Indikace stavu prostředku, je bod důležitých dat ve proces řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="12955-133">While this status is not a definitive indication of the state of the resource, it is an important data point in the troubleshooting process:</span></span>
* <span data-ttu-id="12955-134">Pokud prostředek běží podle očekávání stav prostředku se aktualizuje a k dispozici po několika minutách.</span><span class="sxs-lookup"><span data-stu-id="12955-134">If the resource is running as expected the status of the resource will update to Available after a few minutes.</span></span>
* <span data-ttu-id="12955-135">Pokud dochází k potížím s hledaným prostředkem, neznámý stav může naznačovat, že je prostředek dopad na událost samotné platformy.</span><span class="sxs-lookup"><span data-stu-id="12955-135">If you are experiencing problems with the resource, the Unknown health status may suggest the resource is impacted by an event in the platform.</span></span>

![Stav prostředků, které se neznámý virtuálního počítače](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a><span data-ttu-id="12955-137">Nesprávný stav sestavy</span><span class="sxs-lookup"><span data-stu-id="12955-137">Report an incorrect status</span></span>
<span data-ttu-id="12955-138">Pokud v libovolném okamžiku budete mít dojem, aktuální stav je nesprávný, vám může dejte nám vědět kliknutím **sestavy nesprávný stav**.</span><span class="sxs-lookup"><span data-stu-id="12955-138">If at any point you believe the current health status is incorrect, you can let us know by clicking **Report incorrect health status**.</span></span> <span data-ttu-id="12955-139">V případech, kde je postiženo Azure problém doporučujeme kontaktovat podporu v okně prostředků stavu.</span><span class="sxs-lookup"><span data-stu-id="12955-139">In cases where you are impacted by an Azure problem, we encourage you to contact support from the resource health blade.</span></span> 

![Stav prostředku nesprávný stav sestavy](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a><span data-ttu-id="12955-141">Historické informace</span><span class="sxs-lookup"><span data-stu-id="12955-141">Historical Information</span></span>
<span data-ttu-id="12955-142">Dostanete až na 14 dní data historie stavu kliknutím **zobrazit historii** v okně prostředků stavu.</span><span class="sxs-lookup"><span data-stu-id="12955-142">You can access up to 14 days of historical health data by clicking **View History** in the Resource health blade.</span></span> 

![Stav prostředku historie sestavy](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a><span data-ttu-id="12955-144">Začínáme</span><span class="sxs-lookup"><span data-stu-id="12955-144">Getting started</span></span>
<span data-ttu-id="12955-145">Chcete-li otevřít Resource health jeden prostředek</span><span class="sxs-lookup"><span data-stu-id="12955-145">To open Resource health for one resource</span></span>
1.  <span data-ttu-id="12955-146">Přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="12955-146">Sign in into the Azure portal.</span></span>
2.  <span data-ttu-id="12955-147">Přejděte do prostředku.</span><span class="sxs-lookup"><span data-stu-id="12955-147">Navigate to your resource.</span></span>
3.  <span data-ttu-id="12955-148">V nabídce prostředků se nachází na levé straně klikněte na tlačítko **stav prostředku**.</span><span class="sxs-lookup"><span data-stu-id="12955-148">In the resource menu located in the left-hand side, click **Resource health**.</span></span>

![Otevřete v okně prostředků stav prostředků](./media/resource-health-overview/from-resource-blade.png)

<span data-ttu-id="12955-150">Stav prostředku můžete taky přejít kliknutím **další služby**a zadáte **stav prostředku** do textového pole filtru otevřete **Nápověda a podpora** okno.</span><span class="sxs-lookup"><span data-stu-id="12955-150">You can also access resource health by clicking **More services**, and typing **resource health** in filter text box to open the **Help + Support** blade.</span></span> <span data-ttu-id="12955-151">Nakonec klikněte na tlačítko [ **stav prostředku**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span><span class="sxs-lookup"><span data-stu-id="12955-151">Finally click [**Resource health**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span></span>

![Otevřete stav prostředku z další služby](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a><span data-ttu-id="12955-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12955-153">Next steps</span></span>

<span data-ttu-id="12955-154">Podívejte se na tyto prostředky Další informace o stavu prostředků:</span><span class="sxs-lookup"><span data-stu-id="12955-154">Check out these resources to learn more about resource health:</span></span>
-  [<span data-ttu-id="12955-155">Typy prostředků a stav kontrol ve stavu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="12955-155">Resource types and health checks in Azure resource health</span></span>](resource-health-checks-resource-types.md)
-  [<span data-ttu-id="12955-156">Nejčastější dotazy týkající se stavu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="12955-156">Frequently asked questions about Azure resource health</span></span>](resource-health-faq.md)



