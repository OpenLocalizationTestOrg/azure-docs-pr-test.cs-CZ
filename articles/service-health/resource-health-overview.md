---
title: "Přehled stavu prostředků aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: f06153864090487829f717dc3e8972c78a4a58af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-overview"></a><span data-ttu-id="19a29-103">Přehled stavu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="19a29-103">Azure resource health overview</span></span>
 
<span data-ttu-id="19a29-104">Resource Health pomáhá při diagnostice a získání podpory v případě, že potíže s Azure ovlivňují vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="19a29-104">Resource health helps you diagnose and get support when an Azure issue impacts your resources.</span></span> <span data-ttu-id="19a29-105">To informující o hello aktuální a starší stav svých prostředků a pomáhá zmírnit problémy.</span><span class="sxs-lookup"><span data-stu-id="19a29-105">It informs you about hello current and past health of your resources and helps you mitigate issues.</span></span> <span data-ttu-id="19a29-106">Resource Health poskytuje technickou podporu, když potřebujete pomoc při potížích se službami Azure.</span><span class="sxs-lookup"><span data-stu-id="19a29-106">Resource health provides technical support when you need help with Azure service issues.</span></span>

<span data-ttu-id="19a29-107">Zatímco [stavu Azure](https://status.azure.com) informující o problémů služby, které ovlivňují širokou škálu Azure zákazníků, stav prostředku vám poskytne přizpůsobené řídicí panel hello stav svých prostředků.</span><span class="sxs-lookup"><span data-stu-id="19a29-107">Whereas [Azure Status](https://status.azure.com) informs you about service issues that affect a broad set of Azure customers, resource health provides you with a personalized dashboard of hello health of your resources.</span></span> <span data-ttu-id="19a29-108">Stav prostředku se dozvíte, všechny časy hello vaše prostředky byly k dispozici v hello po splatnosti tooAzure problémů služby.</span><span class="sxs-lookup"><span data-stu-id="19a29-108">Resource health shows you all hello times your resources were unavailable in hello past due tooAzure service issues.</span></span> <span data-ttu-id="19a29-109">To umožňuje snadno můžete toounderstand Pokud porušení SLA.</span><span class="sxs-lookup"><span data-stu-id="19a29-109">This makes it simple for you toounderstand if an SLA was violated.</span></span> 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a><span data-ttu-id="19a29-110">Co se považuje za zdroj a jak funguje stav prostředku rozhodne, zda prostředek je v pořádku, či nikoliv?</span><span class="sxs-lookup"><span data-stu-id="19a29-110">What is considered a resource and how does resource health decides if a resource is healthy or not?</span></span>
<span data-ttu-id="19a29-111">Prostředek se o instanci typu prostředku, které nabízí služby Azure pomocí Azure Resource Manager, například: virtuálního počítače, webovou aplikaci nebo v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="19a29-111">A resource is an instance of a resource type offered by an Azure service through Azure Resource Manager, for example: a virtual machine, a web app, or a SQL database.</span></span>

<span data-ttu-id="19a29-112">Stav prostředku spoléhá na signály vysílaných tooassess hello různé služby Azure, pokud prostředek je v pořádku, či nikoli.</span><span class="sxs-lookup"><span data-stu-id="19a29-112">Resource health relies on signals emitted by hello different Azure services tooassess if a resource is healthy or not.</span></span> <span data-ttu-id="19a29-113">Pokud prostředek není v pořádku, analyzuje stav prostředku hello zdroj dalších informací toodetermine hello problém.</span><span class="sxs-lookup"><span data-stu-id="19a29-113">If a resource is unhealthy, resource health analyzes additional information toodetermine hello source of hello problem.</span></span> <span data-ttu-id="19a29-114">Také identifikuje akce, které společnost Microsoft provádí toofix hello problém nebo jaké akce může trvat tooaddress hello způsobí, že hello problému.</span><span class="sxs-lookup"><span data-stu-id="19a29-114">It also identifies actions Microsoft is taking toofix hello issue or what actions you can take tooaddress hello cause of hello problem.</span></span> 

<span data-ttu-id="19a29-115">Zkontrolujte hello úplný seznam typů prostředků a stavu kontroluje [stavu prostředků Azure](resource-health-checks-resource-types.md) další podrobnosti o tom, jak se hodnotí stavu.</span><span class="sxs-lookup"><span data-stu-id="19a29-115">Review hello full list of resource types and health checks in [Azure resource health](resource-health-checks-resource-types.md) for additional details on how health is assessed.</span></span>

## <a name="health-status-provided-by-resource-health"></a><span data-ttu-id="19a29-116">Stav poskytované stav prostředků</span><span class="sxs-lookup"><span data-stu-id="19a29-116">Health status provided by resource health</span></span>
<span data-ttu-id="19a29-117">Hello stav prostředku je jedním z hello následující stavy:</span><span class="sxs-lookup"><span data-stu-id="19a29-117">hello health of a resource is one of hello following statuses:</span></span>

### <a name="available"></a><span data-ttu-id="19a29-118">Dostupné</span><span class="sxs-lookup"><span data-stu-id="19a29-118">Available</span></span>
<span data-ttu-id="19a29-119">Služba Hello nezjistila všechny události, které mají vliv hello stavu prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="19a29-119">hello service has not detected any events impacting hello health of hello resource.</span></span> <span data-ttu-id="19a29-120">V případech, kde hello prostředků se zotavil z neplánované výpadky během hello posledních 24 hodin, zobrazí se hello **nedávno obnovena** oznámení.</span><span class="sxs-lookup"><span data-stu-id="19a29-120">In cases where hello resource has recovered from unplanned downtime during hello last 24 hours you will see hello **recently recovered** notification.</span></span>

![Prostředek stavu k dispozici virtuálního počítače](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a><span data-ttu-id="19a29-122">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="19a29-122">Unavailable</span></span>
<span data-ttu-id="19a29-123">Hello služba zjistila probíhající platformy nebo události bez platformy hello stav hello prostředků, které mají vliv.</span><span class="sxs-lookup"><span data-stu-id="19a29-123">hello service has detected an ongoing platform or non-platform event impacting hello health of hello resource.</span></span>

#### <a name="platform-events"></a><span data-ttu-id="19a29-124">Události platformy</span><span class="sxs-lookup"><span data-stu-id="19a29-124">Platform events</span></span>
<span data-ttu-id="19a29-125">Tyto události jsou aktivovány více součástí hello infrastrukturu Azure a jsou oba plánované akce, jako je plánované údržby a neočekávané incidenty jako restartování neplánované hostitele.</span><span class="sxs-lookup"><span data-stu-id="19a29-125">These events are triggered by multiple components of hello Azure infrastructure and include both scheduled actions like planned maintenance and unexpected incidents like an unplanned host reboot.</span></span>

<span data-ttu-id="19a29-126">Stav prostředku poskytuje další podrobnosti pro hello událost, proces obnovení hello a umožní vám toocontact podpory, i když nebudou mít aktivní podpory smlouvy společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="19a29-126">Resource health provides additional details on hello event, hello recovery process and enables you toocontact support even if you don't have an active Microsoft support agreement.</span></span>

![Prostředek stavu není k dispozici virtuální počítač z důvodu tooplatform událostí](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a><span data-ttu-id="19a29-128">Události jiné platformy</span><span class="sxs-lookup"><span data-stu-id="19a29-128">Non-Platform events</span></span>
<span data-ttu-id="19a29-129">Tyto události jsou aktivovány akce prováděné uživateli, například zastavení virtuálního počítače nebo dosažení maximálního počtu připojení tooa Redis Cache hello.</span><span class="sxs-lookup"><span data-stu-id="19a29-129">These events are triggered by actions taken by users, for example stopping a virtual machine or reaching hello maximum number of connections tooa Redis Cache.</span></span>

![Prostředek stavu není k dispozici virtuální počítač z důvodu události toonon platformy](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a><span data-ttu-id="19a29-131">Neznámý</span><span class="sxs-lookup"><span data-stu-id="19a29-131">Unknown</span></span>
<span data-ttu-id="19a29-132">Tento stav označuje, že stav prostředku nedostal informace o tento prostředek pro více než 10 minut.</span><span class="sxs-lookup"><span data-stu-id="19a29-132">This health status indicates that resource health has not received information about this resource for more than 10 minutes.</span></span> <span data-ttu-id="19a29-133">Přestože tento stav není spolehlivý údaj o hello stav hello prostředku, je bod důležitých dat ve hello procesu odstraňování potíží:</span><span class="sxs-lookup"><span data-stu-id="19a29-133">While this status is not a definitive indication of hello state of hello resource, it is an important data point in hello troubleshooting process:</span></span>
* <span data-ttu-id="19a29-134">Pokud prostředek hello běží jako očekávané hello stav prostředku hello zaktualizuje tooAvailable po několika minutách.</span><span class="sxs-lookup"><span data-stu-id="19a29-134">If hello resource is running as expected hello status of hello resource will update tooAvailable after a few minutes.</span></span>
* <span data-ttu-id="19a29-135">Pokud dochází k potížím s hello prostředků, hello Neznámý stav může naznačují, že je událost v platformě hello dopad hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="19a29-135">If you are experiencing problems with hello resource, hello Unknown health status may suggest hello resource is impacted by an event in hello platform.</span></span>

![Stav prostředků, které se neznámý virtuálního počítače](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a><span data-ttu-id="19a29-137">Nesprávný stav sestavy</span><span class="sxs-lookup"><span data-stu-id="19a29-137">Report an incorrect status</span></span>
<span data-ttu-id="19a29-138">Pokud v libovolném okamžiku budete mít dojem, aktuální stav hello je nesprávný, vám může dejte nám vědět kliknutím **sestavy nesprávný stav**.</span><span class="sxs-lookup"><span data-stu-id="19a29-138">If at any point you believe hello current health status is incorrect, you can let us know by clicking **Report incorrect health status**.</span></span> <span data-ttu-id="19a29-139">V případech, kde je postiženo Azure problém doporučujeme vám toocontact podpory z okna stavu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="19a29-139">In cases where you are impacted by an Azure problem, we encourage you toocontact support from hello resource health blade.</span></span> 

![Stav prostředku nesprávný stav sestavy](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a><span data-ttu-id="19a29-141">Historické informace</span><span class="sxs-lookup"><span data-stu-id="19a29-141">Historical Information</span></span>
<span data-ttu-id="19a29-142">Dostupné až too14 dnů od data historie stavu klepnutím na **zobrazit historii** v okně stavu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="19a29-142">You can access up too14 days of historical health data by clicking **View History** in hello Resource health blade.</span></span> 

![Stav prostředku historie sestavy](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a><span data-ttu-id="19a29-144">Začínáme</span><span class="sxs-lookup"><span data-stu-id="19a29-144">Getting started</span></span>
<span data-ttu-id="19a29-145">tooopen Resource health jeden prostředek</span><span class="sxs-lookup"><span data-stu-id="19a29-145">tooopen Resource health for one resource</span></span>
1.  <span data-ttu-id="19a29-146">Přihlaste se do hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="19a29-146">Sign in into hello Azure portal.</span></span>
2.  <span data-ttu-id="19a29-147">Přejděte tooyour prostředků.</span><span class="sxs-lookup"><span data-stu-id="19a29-147">Navigate tooyour resource.</span></span>
3.  <span data-ttu-id="19a29-148">V nabídce prostředků hello nachází na levé straně hello, klikněte na tlačítko **stav prostředku**.</span><span class="sxs-lookup"><span data-stu-id="19a29-148">In hello resource menu located in hello left-hand side, click **Resource health**.</span></span>

![Otevřete v okně prostředků stav prostředků](./media/resource-health-overview/from-resource-blade.png)

<span data-ttu-id="19a29-150">Stav prostředku můžete taky přejít kliknutím **další služby**a zadáte **stav prostředku** ve filtru textové pole tooopen hello **Nápověda a podpora** okno.</span><span class="sxs-lookup"><span data-stu-id="19a29-150">You can also access resource health by clicking **More services**, and typing **resource health** in filter text box tooopen hello **Help + Support** blade.</span></span> <span data-ttu-id="19a29-151">Nakonec klikněte na tlačítko [ **stav prostředku**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span><span class="sxs-lookup"><span data-stu-id="19a29-151">Finally click [**Resource health**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span></span>

![Otevřete stav prostředku z další služby](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a><span data-ttu-id="19a29-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19a29-153">Next steps</span></span>

<span data-ttu-id="19a29-154">Podívejte se na tyto prostředky toolearn Další informace o stavu prostředků:</span><span class="sxs-lookup"><span data-stu-id="19a29-154">Check out these resources toolearn more about resource health:</span></span>
-  [<span data-ttu-id="19a29-155">Typy prostředků a stav kontrol ve stavu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="19a29-155">Resource types and health checks in Azure resource health</span></span>](resource-health-checks-resource-types.md)
-  [<span data-ttu-id="19a29-156">Nejčastější dotazy týkající se stavu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="19a29-156">Frequently asked questions about Azure resource health</span></span>](resource-health-faq.md)




