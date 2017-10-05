---
title: "Vytvoření služby Azure Search na portálu | Microsoft Docs"
description: "Zřídit služby Azure Search na portálu."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: 58f4eab190e40e16ed261c165ffdfc8155eeb434
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-search-service-in-the-portal"></a><span data-ttu-id="32a9e-103">Vytvoření služby Azure Search v portálu.</span><span class="sxs-lookup"><span data-stu-id="32a9e-103">Create an Azure Search service in the portal</span></span>

<span data-ttu-id="32a9e-104">Tento článek vysvětluje, jak vytvořit nebo zřídit služby Azure Search na portálu.</span><span class="sxs-lookup"><span data-stu-id="32a9e-104">This article explains how to create or provision an Azure Search service in the portal.</span></span> <span data-ttu-id="32a9e-105">Prostředí PowerShell pokyny najdete v tématu [spravovat Azure Search pomocí prostředí PowerShell](search-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="32a9e-105">For PowerShell instructions, see [Manage Azure Search with PowerShell](search-manage-powershell.md).</span></span>

## <a name="subscribe-free-or-paid"></a><span data-ttu-id="32a9e-106">Přihlášení k odběru (volné nebo placené)</span><span class="sxs-lookup"><span data-stu-id="32a9e-106">Subscribe (free or paid)</span></span>

<span data-ttu-id="32a9e-107">[Otevřít bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) a použijte bezplatný kredit k vyzkoušení placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="32a9e-107">[Open a free Azure account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) and use free credits to try out paid Azure services.</span></span> <span data-ttu-id="32a9e-108">Až kredity vyčerpáte, si účet nechat a dál používat bezplatné služby Azure, jako jsou weby.</span><span class="sxs-lookup"><span data-stu-id="32a9e-108">After credits are used up, keep the account and continue to use free Azure services, such as Websites.</span></span> <span data-ttu-id="32a9e-109">Platební karty se nikdy účtovat Pokud explicitně změnit nastavení a požádejte o nestrhne.</span><span class="sxs-lookup"><span data-stu-id="32a9e-109">Your credit card is never charged unless you explicitly change your settings and ask to be charged.</span></span>

<span data-ttu-id="32a9e-110">Alternativně [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="32a9e-110">Alternatively, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span> <span data-ttu-id="32a9e-111">Předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="32a9e-111">An MSDN subscription gives you credits every month you can use for paid Azure services.</span></span> 

## <a name="find-azure-search"></a><span data-ttu-id="32a9e-112">Najít vyhledávání systému Azure</span><span class="sxs-lookup"><span data-stu-id="32a9e-112">Find Azure Search</span></span>
1. <span data-ttu-id="32a9e-113">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="32a9e-113">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="32a9e-114">Kliknutím na znaménko plus ("+") v levém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="32a9e-114">Click the plus sign ("+") in the top left corner.</span></span>
3. <span data-ttu-id="32a9e-115">Vyberte **Web + mobilní** > **služba Azure Search**.</span><span class="sxs-lookup"><span data-stu-id="32a9e-115">Select **Web + Mobile** > **Azure Search**.</span></span>

![](./media/search-create-service-portal/find-search2.png)

## <a name="name-the-service-and-url-endpoint"></a><span data-ttu-id="32a9e-116">Název služby a koncový bod adresy URL</span><span class="sxs-lookup"><span data-stu-id="32a9e-116">Name the service and URL endpoint</span></span>

<span data-ttu-id="32a9e-117">Název služby je součástí adresy URL koncového bodu, vůči které jsou vydány volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="32a9e-117">A service name is part of the URL endpoint against which API calls are issued.</span></span> <span data-ttu-id="32a9e-118">Zadejte název vaší služby v **URL** pole.</span><span class="sxs-lookup"><span data-stu-id="32a9e-118">Type your service name in the **URL** field.</span></span> 

<span data-ttu-id="32a9e-119">Požadavky na název služby:</span><span class="sxs-lookup"><span data-stu-id="32a9e-119">Service name requirements:</span></span>
   * <span data-ttu-id="32a9e-120">2 až 60 znaků.</span><span class="sxs-lookup"><span data-stu-id="32a9e-120">2 and 60 characters in length</span></span>
   * <span data-ttu-id="32a9e-121">malá písmena, číslice nebo spojovníky ("-")</span><span class="sxs-lookup"><span data-stu-id="32a9e-121">lowercase letters, digits, or dashes ("-")</span></span>
   * <span data-ttu-id="32a9e-122">žádné pomlčka ("-") jako první 2 znaky nebo poslední jeden znak</span><span class="sxs-lookup"><span data-stu-id="32a9e-122">no dash ("-") as the first 2 characters or last single character</span></span>
   * <span data-ttu-id="32a9e-123">žádné po sobě jdoucí pomlčky ("--")</span><span class="sxs-lookup"><span data-stu-id="32a9e-123">no consecutive dashes ("--")</span></span>

## <a name="select-a-subscription"></a><span data-ttu-id="32a9e-124">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="32a9e-124">Select a subscription</span></span>
<span data-ttu-id="32a9e-125">Pokud máte více než jedno předplatné, vyberte ten, který má také služby úložiště dat nebo souboru.</span><span class="sxs-lookup"><span data-stu-id="32a9e-125">If you have more than one subscription, choose one that also has data or file storage services.</span></span> <span data-ttu-id="32a9e-126">Vyhledávání systému Azure můžete automaticky rozpoznat úložiště Azure Table a objektů Blob, databáze SQL a Azure Cosmos DB pro indexování prostřednictvím *indexery*, ale pouze pro služby ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="32a9e-126">Azure Search can auto-detect Azure Table and Blob storage, SQL Database, and Azure Cosmos DB for indexing via *indexers*, but only for services in the same subscription.</span></span>

## <a name="select-a-resource-group"></a><span data-ttu-id="32a9e-127">Vybrat skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="32a9e-127">Select a resource group</span></span>
<span data-ttu-id="32a9e-128">Skupina prostředků je kolekce služeb Azure a prostředky, použít společně.</span><span class="sxs-lookup"><span data-stu-id="32a9e-128">A resource group is a collection of Azure services and resources used together.</span></span> <span data-ttu-id="32a9e-129">Pokud používáte Azure Search při indexování databázi SQL, pak obě služby by měl být například součástí stejné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="32a9e-129">For example, if you are using Azure Search to index a SQL database, then both services should be part of the same resource group.</span></span>

> [!TIP]
> <span data-ttu-id="32a9e-130">Odstranění skupiny prostředků se odstraní taky služby v něm.</span><span class="sxs-lookup"><span data-stu-id="32a9e-130">Deleting a resource group also deletes the services within it.</span></span> <span data-ttu-id="32a9e-131">Pro projekty prototypu využívá více služeb třeba umisťovat všechny ve stejné skupině prostředků usnadňuje čištění po dokončení projektu.</span><span class="sxs-lookup"><span data-stu-id="32a9e-131">For prototype projects utilizing multiple services, putting all of them in the same resource group makes cleanup easier after the project is over.</span></span> 

## <a name="select-a-hosting-location"></a><span data-ttu-id="32a9e-132">Vyberte umístění pro hostování</span><span class="sxs-lookup"><span data-stu-id="32a9e-132">Select a hosting location</span></span> 
<span data-ttu-id="32a9e-133">Jako služba Azure může být hostovaný Azure Search v datových centrech po celém světě.</span><span class="sxs-lookup"><span data-stu-id="32a9e-133">As an Azure service, Azure Search can be hosted in datacenters around the world.</span></span> <span data-ttu-id="32a9e-134">Všimněte si, že [ceny se může lišit](https://azure.microsoft.com/pricing/details/search/) zeměpisných údajů.</span><span class="sxs-lookup"><span data-stu-id="32a9e-134">Note that [prices can differ](https://azure.microsoft.com/pricing/details/search/) by geography.</span></span>

## <a name="select-a-pricing-tier-sku"></a><span data-ttu-id="32a9e-135">Vyberte cenovou úroveň (SKU)</span><span class="sxs-lookup"><span data-stu-id="32a9e-135">Select a pricing tier (SKU)</span></span>
<span data-ttu-id="32a9e-136">[Služba Azure Search je aktuálně k dispozici v několika cenových úrovní](https://azure.microsoft.com/pricing/details/search/): volné, Basic nebo Standard.</span><span class="sxs-lookup"><span data-stu-id="32a9e-136">[Azure Search is currently offered in multiple pricing tiers](https://azure.microsoft.com/pricing/details/search/): Free, Basic, or Standard.</span></span> <span data-ttu-id="32a9e-137">Každá úroveň má svou vlastní [kapacity a omezení](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="32a9e-137">Each tier has its own [capacity and limits](search-limits-quotas-capacity.md).</span></span> <span data-ttu-id="32a9e-138">V tématu [zvolte cenovou úroveň nebo SKU](search-sku-tier.md) pokyny.</span><span class="sxs-lookup"><span data-stu-id="32a9e-138">See [Choose a pricing tier or SKU](search-sku-tier.md) for guidance.</span></span>

<span data-ttu-id="32a9e-139">V tomto návodu jsme zvolili na plán úrovně Standard pro naši službu.</span><span class="sxs-lookup"><span data-stu-id="32a9e-139">In this walkthrough, we have chosen the Standard tier for our service.</span></span>

## <a name="create-your-service"></a><span data-ttu-id="32a9e-140">Vytvoření služby</span><span class="sxs-lookup"><span data-stu-id="32a9e-140">Create your service</span></span>

<span data-ttu-id="32a9e-141">Nezapomeňte připnout služby pro snadný přístup k řídicímu panelu při každém přihlášení.</span><span class="sxs-lookup"><span data-stu-id="32a9e-141">Remember to pin your service to the dashboard for easy access whenever you sign in.</span></span>

![](./media/search-create-service-portal/new-service2.png)

## <a name="scale-your-service"></a><span data-ttu-id="32a9e-142">Škálování služby</span><span class="sxs-lookup"><span data-stu-id="32a9e-142">Scale your service</span></span>
<span data-ttu-id="32a9e-143">To může trvat několik minut pro vytvoření služby (15 minut nebo déle v závislosti na vrstvě).</span><span class="sxs-lookup"><span data-stu-id="32a9e-143">It can take a few minutes to create a service (15 minutes or more depending on the tier).</span></span> <span data-ttu-id="32a9e-144">Po zřízení služby, je možné škálovat tak, aby vyhovovala vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="32a9e-144">After your service is provisioned, you can scale it to meet your needs.</span></span> <span data-ttu-id="32a9e-145">Protože jste zvolili na plán úrovně Standard pro služby Azure Search, můžete škálovat služby v dvěma rozměry: repliky a oddíly.</span><span class="sxs-lookup"><span data-stu-id="32a9e-145">Because you chose the Standard tier for your Azure Search service, you can scale your service in two dimensions: replicas and partitions.</span></span> <span data-ttu-id="32a9e-146">Jste nastavili základní vrstvě, můžete použít pouze repliky.</span><span class="sxs-lookup"><span data-stu-id="32a9e-146">Had you chosen the Basic tier, you can only add replicas.</span></span> <span data-ttu-id="32a9e-147">Pokud jste zřídili bezplatnou službou, škálování není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="32a9e-147">If you provisioned the free service, scale is not available.</span></span>

<span data-ttu-id="32a9e-148">***Oddíly*** povolit služby k ukládání a hledání v další dokumenty.</span><span class="sxs-lookup"><span data-stu-id="32a9e-148">***Partitions*** allow your service to store and search through more documents.</span></span>

<span data-ttu-id="32a9e-149">***Repliky*** povolit služby pro zpracování vyšší zatížení vyhledávací dotazy.</span><span class="sxs-lookup"><span data-stu-id="32a9e-149">***Replicas*** allow your service to handle a higher load of search queries.</span></span>

> [!Important]
> <span data-ttu-id="32a9e-150">Služba musí mít [2 repliky smlouva SLA jen pro čtení a 3 repliky pro čtení/zápisu SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span><span class="sxs-lookup"><span data-stu-id="32a9e-150">A service must have [2 replicas for read-only SLA and 3 replicas for read/write SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span></span>

1. <span data-ttu-id="32a9e-151">Přejděte do vaší okna služby search na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="32a9e-151">Go to your search service blade in the Azure portal.</span></span>
2. <span data-ttu-id="32a9e-152">V levém navigačním podokně vyberte **nastavení** > **škálování**.</span><span class="sxs-lookup"><span data-stu-id="32a9e-152">In the left-navigation pane, select **Settings** > **Scale**.</span></span>
3. <span data-ttu-id="32a9e-153">Pomocí slidebar přidejte repliky nebo oddíly.</span><span class="sxs-lookup"><span data-stu-id="32a9e-153">Use the slidebar to add Replicas or Partitions.</span></span>

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> <span data-ttu-id="32a9e-154">Každá úroveň má jiný [omezení](search-limits-quotas-capacity.md) na celkový počet jednotek vyhledávání povolené v jedné službě (repliky * oddíly = celkový počet jednotek vyhledávání).</span><span class="sxs-lookup"><span data-stu-id="32a9e-154">Each tier has different [limits](search-limits-quotas-capacity.md) on the total number of Search Units allowed in a single service (Replicas * Partitions = Total Search Units).</span></span>

## <a name="when-to-add-a-second-service"></a><span data-ttu-id="32a9e-155">Pokud chcete přidat druhý službu</span><span class="sxs-lookup"><span data-stu-id="32a9e-155">When to add a second service</span></span>

<span data-ttu-id="32a9e-156">Většina zákazníků použít pouze jednu službu zřídí v vrstvy, která poskytuje [pravým vyrovnávání prostředků](search-sku-tier.md).</span><span class="sxs-lookup"><span data-stu-id="32a9e-156">A large majority of customers use just one service provisioned at a tier that provides the [right balance of resources](search-sku-tier.md).</span></span> <span data-ttu-id="32a9e-157">Jedna služba může být hostitelem více indexů, podléhají [maximální limit vrstvy vyberete](search-capacity-planning.md), s každou indexem izolované z jiné.</span><span class="sxs-lookup"><span data-stu-id="32a9e-157">One service can host multiple indexes, subject to the [maximum limits of the tier you select](search-capacity-planning.md), with each index isolated from another.</span></span> <span data-ttu-id="32a9e-158">Ve službě Azure Search požadavků můžete pouze přesměrováni na jeden index, což minimalizuje načítání náhodnému nebo záměrnému dat z jiných indexy v stejnou službu.</span><span class="sxs-lookup"><span data-stu-id="32a9e-158">In Azure Search, requests can only be directed to one index, minimizing the chance of accidental or intentional data retrieval from other indexes in the same service.</span></span>

<span data-ttu-id="32a9e-159">I když většina zákazníků používat jenom jedna služba, redundance služby může být nutné v případě provozních požadavků zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="32a9e-159">Although most customers use just one service, service redundancy might be necessary if operational requirements include the following:</span></span>

+ <span data-ttu-id="32a9e-160">Zotavení po havárii (data center výpadek).</span><span class="sxs-lookup"><span data-stu-id="32a9e-160">Disaster recovery (data center outage).</span></span> <span data-ttu-id="32a9e-161">Služba Azure Search neposkytuje rychlých převzetí služeb při selhání v případě výpadku.</span><span class="sxs-lookup"><span data-stu-id="32a9e-161">Azure Search does not provide instant failover in the event of an outage.</span></span> <span data-ttu-id="32a9e-162">Doporučení a pokyny najdete v tématu [služby správy](search-manage.md).</span><span class="sxs-lookup"><span data-stu-id="32a9e-162">For recommendations and guidance, see [Service administration](search-manage.md).</span></span>
+ <span data-ttu-id="32a9e-163">Vaše šetření víceklientský modelování má zjistíte, že další služby optimální návrhu.</span><span class="sxs-lookup"><span data-stu-id="32a9e-163">Your investigation of multi-tenancy modeling has determined that additional services is the optimal design.</span></span> <span data-ttu-id="32a9e-164">Další informace najdete v tématu [návrhu víceklientský](search-modeling-multitenant-saas-applications.md).</span><span class="sxs-lookup"><span data-stu-id="32a9e-164">For more information, see [Design for multi-tenancy](search-modeling-multitenant-saas-applications.md).</span></span>
+ <span data-ttu-id="32a9e-165">Pro globální nasazené aplikace může vyžadovat instanci Azure Search v několika oblastech pro zajištění minimální latence mezinárodní provoz vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="32a9e-165">For globally deployed applications, you might require an instance of Azure Search in multiple regions to minimize latency of your application’s international traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="32a9e-166">Ve službě Azure Search nelze oddělit indexování a dotazování zatížení; Proto by nikdy vytvořit více služeb pro oddělenou úlohy.</span><span class="sxs-lookup"><span data-stu-id="32a9e-166">In Azure Search, you cannot segregate indexing and querying workloads; thus, you would never create multiple services for segregated workloads.</span></span> <span data-ttu-id="32a9e-167">Index je vždy dotaz na službu ve kterém it vytvořil (můžete v jedné službě vytvoření indexu a zkopírujte ho do jiného.).</span><span class="sxs-lookup"><span data-stu-id="32a9e-167">An index is always queried on the service in which it was created (you cannot create an index in one service and copy it to another).</span></span>
>

<span data-ttu-id="32a9e-168">Druhý služby není nutné pro zajištění vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="32a9e-168">A second service is not required for high availability.</span></span> <span data-ttu-id="32a9e-169">Vysoká dostupnost pro dotazy se dosáhne, když používáte 2 nebo více replik v rámci stejné služby.</span><span class="sxs-lookup"><span data-stu-id="32a9e-169">High availability for queries is achieved when you use 2 or more replicas in the same service.</span></span> <span data-ttu-id="32a9e-170">Aktualizace repliky jsou sekvenční, což znamená, že je alespoň jeden funkční při aktualizaci služby se nasazuje.</span><span class="sxs-lookup"><span data-stu-id="32a9e-170">Replica updates are sequential, which means at least one is operational when a service update is rolled out.</span></span> <span data-ttu-id="32a9e-171">Další informace o provozu najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span><span class="sxs-lookup"><span data-stu-id="32a9e-171">For more information about uptime, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="32a9e-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32a9e-172">Next steps</span></span>
<span data-ttu-id="32a9e-173">Po zřízení služby Azure Search, budete chtít [definujte index](search-what-is-an-index.md) vám umožní nahrát a hledání vaše data.</span><span class="sxs-lookup"><span data-stu-id="32a9e-173">After provisioning an Azure Search service, you are ready to [define an index](search-what-is-an-index.md) so you can upload and search your data.</span></span>

<span data-ttu-id="32a9e-174">Přístup ke službě z kódu nebo skriptu, zadejte adresu URL (*název služby*. search.windows.net) a klíč.</span><span class="sxs-lookup"><span data-stu-id="32a9e-174">To access the service from code or script, provide the URL (*service-name*.search.windows.net) and a key.</span></span> <span data-ttu-id="32a9e-175">Klíče správce poskytují úplný přístup; klíče dotazu udělují přístup jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="32a9e-175">Admin keys grant full access; query keys grant read-only access.</span></span> <span data-ttu-id="32a9e-176">V tématu [jak používat Azure Search v rozhraní .NET](search-howto-dotnet-sdk.md) začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="32a9e-176">See [How to use Azure Search in .NET](search-howto-dotnet-sdk.md) to get started.</span></span>

<span data-ttu-id="32a9e-177">V tématu [sestavení a dotazování indexu první](search-get-started-portal.md) najdete rychlý kurz založené na portálu.</span><span class="sxs-lookup"><span data-stu-id="32a9e-177">See [Build and query your first index](search-get-started-portal.md) for a quick portal-based tutorial.</span></span>

