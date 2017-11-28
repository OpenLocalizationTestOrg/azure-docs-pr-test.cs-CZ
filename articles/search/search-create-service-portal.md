---
title: "aaaCreate služby Azure Search na portálu hello | Microsoft Docs"
description: "Zřídit služby Azure Search na portálu hello."
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
ms.openlocfilehash: f1c7197a1467e32bd4b360b165c9059e6bb0e496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-service-in-hello-portal"></a><span data-ttu-id="e0abb-103">Vytvoření služby Azure Search na portálu hello</span><span class="sxs-lookup"><span data-stu-id="e0abb-103">Create an Azure Search service in hello portal</span></span>

<span data-ttu-id="e0abb-104">Tento článek vysvětluje, jak toocreate nebo zřízení Azure Search, které se služby portálu hello.</span><span class="sxs-lookup"><span data-stu-id="e0abb-104">This article explains how toocreate or provision an Azure Search service in hello portal.</span></span> <span data-ttu-id="e0abb-105">Prostředí PowerShell pokyny najdete v tématu [spravovat Azure Search pomocí prostředí PowerShell](search-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e0abb-105">For PowerShell instructions, see [Manage Azure Search with PowerShell](search-manage-powershell.md).</span></span>

## <a name="subscribe-free-or-paid"></a><span data-ttu-id="e0abb-106">Přihlášení k odběru (volné nebo placené)</span><span class="sxs-lookup"><span data-stu-id="e0abb-106">Subscribe (free or paid)</span></span>

<span data-ttu-id="e0abb-107">[Otevřít bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) a používat bezplatné kredity tootry na placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="e0abb-107">[Open a free Azure account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) and use free credits tootry out paid Azure services.</span></span> <span data-ttu-id="e0abb-108">Až kredity vyčerpáte, ponechte hello účet a pokračovat toouse bezplatné služby Azure, třeba weby.</span><span class="sxs-lookup"><span data-stu-id="e0abb-108">After credits are used up, keep hello account and continue toouse free Azure services, such as Websites.</span></span> <span data-ttu-id="e0abb-109">Platební karty se nikdy účtovat Pokud explicitně změnit nastavení a požádejte toobe účtovat.</span><span class="sxs-lookup"><span data-stu-id="e0abb-109">Your credit card is never charged unless you explicitly change your settings and ask toobe charged.</span></span>

<span data-ttu-id="e0abb-110">Alternativně [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="e0abb-110">Alternatively, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span> <span data-ttu-id="e0abb-111">Předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="e0abb-111">An MSDN subscription gives you credits every month you can use for paid Azure services.</span></span> 

## <a name="find-azure-search"></a><span data-ttu-id="e0abb-112">Najít vyhledávání systému Azure</span><span class="sxs-lookup"><span data-stu-id="e0abb-112">Find Azure Search</span></span>
1. <span data-ttu-id="e0abb-113">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e0abb-113">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e0abb-114">V horní části hello levého horního rohu klikněte na tlačítko hello plus přihlašovací ("+").</span><span class="sxs-lookup"><span data-stu-id="e0abb-114">Click hello plus sign ("+") in hello top left corner.</span></span>
3. <span data-ttu-id="e0abb-115">Vyberte **Web + mobilní** > **služba Azure Search**.</span><span class="sxs-lookup"><span data-stu-id="e0abb-115">Select **Web + Mobile** > **Azure Search**.</span></span>

![](./media/search-create-service-portal/find-search2.png)

## <a name="name-hello-service-and-url-endpoint"></a><span data-ttu-id="e0abb-116">Název hello služby a adresu URL koncového bodu</span><span class="sxs-lookup"><span data-stu-id="e0abb-116">Name hello service and URL endpoint</span></span>

<span data-ttu-id="e0abb-117">Název služby je součástí hello URL koncového bodu, vůči které jsou vydány volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e0abb-117">A service name is part of hello URL endpoint against which API calls are issued.</span></span> <span data-ttu-id="e0abb-118">Zadejte název vaší služby hello **URL** pole.</span><span class="sxs-lookup"><span data-stu-id="e0abb-118">Type your service name in hello **URL** field.</span></span> 

<span data-ttu-id="e0abb-119">Požadavky na název služby:</span><span class="sxs-lookup"><span data-stu-id="e0abb-119">Service name requirements:</span></span>
   * <span data-ttu-id="e0abb-120">2 až 60 znaků.</span><span class="sxs-lookup"><span data-stu-id="e0abb-120">2 and 60 characters in length</span></span>
   * <span data-ttu-id="e0abb-121">malá písmena, číslice nebo spojovníky ("-")</span><span class="sxs-lookup"><span data-stu-id="e0abb-121">lowercase letters, digits, or dashes ("-")</span></span>
   * <span data-ttu-id="e0abb-122">žádné pomlčka ("-") jako hello nejprve 2 znaky nebo poslední jeden znak</span><span class="sxs-lookup"><span data-stu-id="e0abb-122">no dash ("-") as hello first 2 characters or last single character</span></span>
   * <span data-ttu-id="e0abb-123">žádné po sobě jdoucí pomlčky ("--")</span><span class="sxs-lookup"><span data-stu-id="e0abb-123">no consecutive dashes ("--")</span></span>

## <a name="select-a-subscription"></a><span data-ttu-id="e0abb-124">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="e0abb-124">Select a subscription</span></span>
<span data-ttu-id="e0abb-125">Pokud máte více než jedno předplatné, vyberte ten, který má také služby úložiště dat nebo souboru.</span><span class="sxs-lookup"><span data-stu-id="e0abb-125">If you have more than one subscription, choose one that also has data or file storage services.</span></span> <span data-ttu-id="e0abb-126">Vyhledávání systému Azure můžete automaticky rozpoznat úložiště Azure Table a objektů Blob, databáze SQL a Azure Cosmos DB pro indexování prostřednictvím *indexery*, ale pouze pro služby v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="e0abb-126">Azure Search can auto-detect Azure Table and Blob storage, SQL Database, and Azure Cosmos DB for indexing via *indexers*, but only for services in hello same subscription.</span></span>

## <a name="select-a-resource-group"></a><span data-ttu-id="e0abb-127">Vybrat skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="e0abb-127">Select a resource group</span></span>
<span data-ttu-id="e0abb-128">Skupina prostředků je kolekce služeb Azure a prostředky, použít společně.</span><span class="sxs-lookup"><span data-stu-id="e0abb-128">A resource group is a collection of Azure services and resources used together.</span></span> <span data-ttu-id="e0abb-129">Například pokud používáte Azure Search tooindex databázi SQL, pak obě služby by měl být součástí hello stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e0abb-129">For example, if you are using Azure Search tooindex a SQL database, then both services should be part of hello same resource group.</span></span>

> [!TIP]
> <span data-ttu-id="e0abb-130">Odstranění skupiny prostředků, odstraní se také hello služby v něm.</span><span class="sxs-lookup"><span data-stu-id="e0abb-130">Deleting a resource group also deletes hello services within it.</span></span> <span data-ttu-id="e0abb-131">Pro projekty prototypu využívá více služeb, všechny z nich uvedení v hello stejnou skupinu prostředků usnadňuje čištění po hello projektu.</span><span class="sxs-lookup"><span data-stu-id="e0abb-131">For prototype projects utilizing multiple services, putting all of them in hello same resource group makes cleanup easier after hello project is over.</span></span> 

## <a name="select-a-hosting-location"></a><span data-ttu-id="e0abb-132">Vyberte umístění pro hostování</span><span class="sxs-lookup"><span data-stu-id="e0abb-132">Select a hosting location</span></span> 
<span data-ttu-id="e0abb-133">Jako služba Azure může být hostovaný Azure Search v datových centrech kolem hello, world.</span><span class="sxs-lookup"><span data-stu-id="e0abb-133">As an Azure service, Azure Search can be hosted in datacenters around hello world.</span></span> <span data-ttu-id="e0abb-134">Všimněte si, že [ceny se může lišit](https://azure.microsoft.com/pricing/details/search/) zeměpisných údajů.</span><span class="sxs-lookup"><span data-stu-id="e0abb-134">Note that [prices can differ](https://azure.microsoft.com/pricing/details/search/) by geography.</span></span>

## <a name="select-a-pricing-tier-sku"></a><span data-ttu-id="e0abb-135">Vyberte cenovou úroveň (SKU)</span><span class="sxs-lookup"><span data-stu-id="e0abb-135">Select a pricing tier (SKU)</span></span>
<span data-ttu-id="e0abb-136">[Služba Azure Search je aktuálně k dispozici v několika cenových úrovní](https://azure.microsoft.com/pricing/details/search/): volné, Basic nebo Standard.</span><span class="sxs-lookup"><span data-stu-id="e0abb-136">[Azure Search is currently offered in multiple pricing tiers](https://azure.microsoft.com/pricing/details/search/): Free, Basic, or Standard.</span></span> <span data-ttu-id="e0abb-137">Každá úroveň má svou vlastní [kapacity a omezení](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="e0abb-137">Each tier has its own [capacity and limits](search-limits-quotas-capacity.md).</span></span> <span data-ttu-id="e0abb-138">V tématu [zvolte cenovou úroveň nebo SKU](search-sku-tier.md) pokyny.</span><span class="sxs-lookup"><span data-stu-id="e0abb-138">See [Choose a pricing tier or SKU](search-sku-tier.md) for guidance.</span></span>

<span data-ttu-id="e0abb-139">V tomto návodu jsme zvolili hello úrovně Standard pro naši službu.</span><span class="sxs-lookup"><span data-stu-id="e0abb-139">In this walkthrough, we have chosen hello Standard tier for our service.</span></span>

## <a name="create-your-service"></a><span data-ttu-id="e0abb-140">Vytvoření služby</span><span class="sxs-lookup"><span data-stu-id="e0abb-140">Create your service</span></span>

<span data-ttu-id="e0abb-141">Mějte na paměti toopin řídicího panelu služby toohello pro snadný přístup při každém přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e0abb-141">Remember toopin your service toohello dashboard for easy access whenever you sign in.</span></span>

![](./media/search-create-service-portal/new-service2.png)

## <a name="scale-your-service"></a><span data-ttu-id="e0abb-142">Škálování služby</span><span class="sxs-lookup"><span data-stu-id="e0abb-142">Scale your service</span></span>
<span data-ttu-id="e0abb-143">Může trvat několik minut toocreate služby (15 minut nebo déle v závislosti na úrovni hello).</span><span class="sxs-lookup"><span data-stu-id="e0abb-143">It can take a few minutes toocreate a service (15 minutes or more depending on hello tier).</span></span> <span data-ttu-id="e0abb-144">Po zřízení služby, je možné škálovat ho toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="e0abb-144">After your service is provisioned, you can scale it toomeet your needs.</span></span> <span data-ttu-id="e0abb-145">Protože jste zvolili hello úrovně Standard pro služby Azure Search, můžete škálovat služby v dvěma rozměry: repliky a oddíly.</span><span class="sxs-lookup"><span data-stu-id="e0abb-145">Because you chose hello Standard tier for your Azure Search service, you can scale your service in two dimensions: replicas and partitions.</span></span> <span data-ttu-id="e0abb-146">Jste nastavili hello základní vrstvy, můžete použít pouze repliky.</span><span class="sxs-lookup"><span data-stu-id="e0abb-146">Had you chosen hello Basic tier, you can only add replicas.</span></span> <span data-ttu-id="e0abb-147">Pokud jste zřídili hello bezplatná služba, škálování není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e0abb-147">If you provisioned hello free service, scale is not available.</span></span>

<span data-ttu-id="e0abb-148">***Oddíly*** povolit služby toostore a vyhledávání pomocí více dokumentů.</span><span class="sxs-lookup"><span data-stu-id="e0abb-148">***Partitions*** allow your service toostore and search through more documents.</span></span>

<span data-ttu-id="e0abb-149">***Repliky*** povolit vaší služby toohandle vyšší zatížení vyhledávací dotazy.</span><span class="sxs-lookup"><span data-stu-id="e0abb-149">***Replicas*** allow your service toohandle a higher load of search queries.</span></span>

> [!Important]
> <span data-ttu-id="e0abb-150">Služba musí mít [2 repliky smlouva SLA jen pro čtení a 3 repliky pro čtení/zápisu SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span><span class="sxs-lookup"><span data-stu-id="e0abb-150">A service must have [2 replicas for read-only SLA and 3 replicas for read/write SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span></span>

1. <span data-ttu-id="e0abb-151">Přejděte okno služby vyhledávání tooyour v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e0abb-151">Go tooyour search service blade in hello Azure portal.</span></span>
2. <span data-ttu-id="e0abb-152">V levém navigačním podokně hello vyberte **nastavení** > **škálování**.</span><span class="sxs-lookup"><span data-stu-id="e0abb-152">In hello left-navigation pane, select **Settings** > **Scale**.</span></span>
3. <span data-ttu-id="e0abb-153">Použijte hello slidebar tooadd repliky nebo oddíly.</span><span class="sxs-lookup"><span data-stu-id="e0abb-153">Use hello slidebar tooadd Replicas or Partitions.</span></span>

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> <span data-ttu-id="e0abb-154">Každá úroveň má jiný [omezení](search-limits-quotas-capacity.md) na celkový počet jednotek vyhledávání povolené v jedné službě hello (repliky * oddíly = celkový počet jednotek vyhledávání).</span><span class="sxs-lookup"><span data-stu-id="e0abb-154">Each tier has different [limits](search-limits-quotas-capacity.md) on hello total number of Search Units allowed in a single service (Replicas * Partitions = Total Search Units).</span></span>

## <a name="when-tooadd-a-second-service"></a><span data-ttu-id="e0abb-155">Když tooadd druhý služby</span><span class="sxs-lookup"><span data-stu-id="e0abb-155">When tooadd a second service</span></span>

<span data-ttu-id="e0abb-156">Většina zákazníků použít pouze jednu službu zřídí v vrstvy, která poskytuje hello [pravým vyrovnávání prostředků](search-sku-tier.md).</span><span class="sxs-lookup"><span data-stu-id="e0abb-156">A large majority of customers use just one service provisioned at a tier that provides hello [right balance of resources](search-sku-tier.md).</span></span> <span data-ttu-id="e0abb-157">Jedna služba může být hostitelem více indexů, předmět toohello [maximální limit hello vrstvy vyberete](search-capacity-planning.md), s každou indexem izolované z jiné.</span><span class="sxs-lookup"><span data-stu-id="e0abb-157">One service can host multiple indexes, subject toohello [maximum limits of hello tier you select](search-capacity-planning.md), with each index isolated from another.</span></span> <span data-ttu-id="e0abb-158">Ve službě Azure Search, můžete požadavky jen bude směrovanou tooone index, minimalizovat riziko hello načtení náhodnému nebo záměrnému dat z jiných indexy v hello stejné služby.</span><span class="sxs-lookup"><span data-stu-id="e0abb-158">In Azure Search, requests can only be directed tooone index, minimizing hello chance of accidental or intentional data retrieval from other indexes in hello same service.</span></span>

<span data-ttu-id="e0abb-159">I když většina zákazníků používat jenom jedna služba, může být nutné v případě provozních požadavků zahrnují následující hello redundance služby:</span><span class="sxs-lookup"><span data-stu-id="e0abb-159">Although most customers use just one service, service redundancy might be necessary if operational requirements include hello following:</span></span>

+ <span data-ttu-id="e0abb-160">Zotavení po havárii (data center výpadek).</span><span class="sxs-lookup"><span data-stu-id="e0abb-160">Disaster recovery (data center outage).</span></span> <span data-ttu-id="e0abb-161">Služba Azure Search neposkytuje rychlých převzetí služeb při selhání v případě hello výpadku.</span><span class="sxs-lookup"><span data-stu-id="e0abb-161">Azure Search does not provide instant failover in hello event of an outage.</span></span> <span data-ttu-id="e0abb-162">Doporučení a pokyny najdete v tématu [služby správy](search-manage.md).</span><span class="sxs-lookup"><span data-stu-id="e0abb-162">For recommendations and guidance, see [Service administration](search-manage.md).</span></span>
+ <span data-ttu-id="e0abb-163">Vaše šetření víceklientský modelování má zjistíte, že další služby hello optimální návrhu.</span><span class="sxs-lookup"><span data-stu-id="e0abb-163">Your investigation of multi-tenancy modeling has determined that additional services is hello optimal design.</span></span> <span data-ttu-id="e0abb-164">Další informace najdete v tématu [návrhu víceklientský](search-modeling-multitenant-saas-applications.md).</span><span class="sxs-lookup"><span data-stu-id="e0abb-164">For more information, see [Design for multi-tenancy](search-modeling-multitenant-saas-applications.md).</span></span>
+ <span data-ttu-id="e0abb-165">Pro globální nasazené aplikace může vyžadovat instanci Azure Search v několika latence oblasti toominimize mezinárodní provoz vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e0abb-165">For globally deployed applications, you might require an instance of Azure Search in multiple regions toominimize latency of your application’s international traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="e0abb-166">Ve službě Azure Search nelze oddělit indexování a dotazování zatížení; Proto by nikdy vytvořit více služeb pro oddělenou úlohy.</span><span class="sxs-lookup"><span data-stu-id="e0abb-166">In Azure Search, you cannot segregate indexing and querying workloads; thus, you would never create multiple services for segregated workloads.</span></span> <span data-ttu-id="e0abb-167">Index je vždy dotazován na hello služby byl vytvořen ve kterém it (nelze vytvořit index v jedné službě a zkopírujte jej tooanother).</span><span class="sxs-lookup"><span data-stu-id="e0abb-167">An index is always queried on hello service in which it was created (you cannot create an index in one service and copy it tooanother).</span></span>
>

<span data-ttu-id="e0abb-168">Druhý služby není nutné pro zajištění vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="e0abb-168">A second service is not required for high availability.</span></span> <span data-ttu-id="e0abb-169">Vysoká dostupnost pro dotazy se dosáhne, když používáte 2 nebo více replik v hello stejnou službu.</span><span class="sxs-lookup"><span data-stu-id="e0abb-169">High availability for queries is achieved when you use 2 or more replicas in hello same service.</span></span> <span data-ttu-id="e0abb-170">Aktualizace repliky jsou sekvenční, což znamená, že je alespoň jeden funkční při aktualizaci služby se nasazuje. Další informace o provozu najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span><span class="sxs-lookup"><span data-stu-id="e0abb-170">Replica updates are sequential, which means at least one is operational when a service update is rolled out. For more information about uptime, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0abb-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0abb-171">Next steps</span></span>
<span data-ttu-id="e0abb-172">Po zřízení služby Azure Search, jste připraveni příliš[definujte index](search-what-is-an-index.md) vám umožní nahrát a hledání vaše data.</span><span class="sxs-lookup"><span data-stu-id="e0abb-172">After provisioning an Azure Search service, you are ready too[define an index](search-what-is-an-index.md) so you can upload and search your data.</span></span>

<span data-ttu-id="e0abb-173">tooaccess hello služby z kódu nebo skriptu, zadejte adresu URL hello (*název služby*. search.windows.net) a klíč.</span><span class="sxs-lookup"><span data-stu-id="e0abb-173">tooaccess hello service from code or script, provide hello URL (*service-name*.search.windows.net) and a key.</span></span> <span data-ttu-id="e0abb-174">Klíče správce poskytují úplný přístup; klíče dotazu udělují přístup jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="e0abb-174">Admin keys grant full access; query keys grant read-only access.</span></span> <span data-ttu-id="e0abb-175">V tématu [jak toouse Azure hledání v rozhraní .NET](search-howto-dotnet-sdk.md) tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="e0abb-175">See [How toouse Azure Search in .NET](search-howto-dotnet-sdk.md) tooget started.</span></span>

<span data-ttu-id="e0abb-176">V tématu [sestavení a dotazování indexu první](search-get-started-portal.md) najdete rychlý kurz založené na portálu.</span><span class="sxs-lookup"><span data-stu-id="e0abb-176">See [Build and query your first index](search-get-started-portal.md) for a quick portal-based tutorial.</span></span>

