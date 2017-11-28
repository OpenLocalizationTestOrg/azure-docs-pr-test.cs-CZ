---
title: Power BI Embedded se zbytkem aaaHow toouse | Microsoft Docs
description: "Zjistěte, jak Power BI Embedded se zbytkem toouse "
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 8bcef780-cca0-4f30-9a9b-9daa1a7ae865
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 02/06/2017
ms.author: asaxton
ms.openlocfilehash: 98057724e60ba868f9c93de8c50383569eb8852d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-power-bi-embedded-with-rest"></a><span data-ttu-id="ef365-103">Jak toouse Power BI Embedded s REST</span><span class="sxs-lookup"><span data-stu-id="ef365-103">How toouse Power BI Embedded with REST</span></span>

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a><span data-ttu-id="ef365-104">Power BI Embedded: Co je a co je pro</span><span class="sxs-lookup"><span data-stu-id="ef365-104">Power BI Embedded: What it is and what it's for</span></span>

<span data-ttu-id="ef365-105">Přehled Power BI Embedded je popsaná v oficiální hello [Power BI Embedded lokality](https://azure.microsoft.com/services/power-bi-embedded/), ale podívejme se rychlé předtím, než se nám získat do hello podrobnosti o použití s REST.</span><span class="sxs-lookup"><span data-stu-id="ef365-105">An overview of Power BI Embedded is described in hello official [Power BI Embedded site](https://azure.microsoft.com/services/power-bi-embedded/), but let's take a quick look before we get into hello details about using it with REST.</span></span>

<span data-ttu-id="ef365-106">Je to úplně jednoduché, skutečně.</span><span class="sxs-lookup"><span data-stu-id="ef365-106">It's quite simple, really.</span></span> <span data-ttu-id="ef365-107">může být vhodné vizualizace dynamická data hello toouse [Power BI](https://powerbi.microsoft.com) ve vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef365-107">you may want toouse hello dynamic data visualizations of [Power BI](https://powerbi.microsoft.com) in your own application.</span></span>

<span data-ttu-id="ef365-108">Většina vlastních aplikací musí toodeliver hello data pro vlastní zákazníky, nikoli nutně uživatele ve vlastní organizaci.</span><span class="sxs-lookup"><span data-stu-id="ef365-108">Most custom applications need toodeliver hello data for their own customers, not necessarily users in their own organization.</span></span> <span data-ttu-id="ef365-109">Například pokud jste doručování některé služby pro firmy A a B společnosti, uživatelům ve společnosti A by měl viděli pouze data pro své vlastní společnosti A. To znamená je potřeba hello víceklientský pro doručení hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-109">For example, if you're delivering some service for both company A and company B, users in company A should only see data for their own company A. That is, hello multi-tenancy is needed for hello delivery.</span></span>

<span data-ttu-id="ef365-110">vlastní aplikace Hello může také nabízí vlastní metody ověřování, jako je například ověřování formulářů, základní ověřování, atd...</span><span class="sxs-lookup"><span data-stu-id="ef365-110">hello custom application might also be offering its own authentication methods such as forms auth, basic auth, etc..</span></span> <span data-ttu-id="ef365-111">Potom hello vložení řešení musí spolupracovat s tento existující metody ověřování bezpečně.</span><span class="sxs-lookup"><span data-stu-id="ef365-111">Then, hello embedding solution must collaborate with this existing authentication methods safely.</span></span> <span data-ttu-id="ef365-112">Je také nezbytné pro možnost toouse toobe uživatelé tyto aplikace ISV bez hello navíc nákupu nebo licencování předplatného Power BI.</span><span class="sxs-lookup"><span data-stu-id="ef365-112">It's also necessary for users toobe able toouse those ISV applications without hello extra purchase or licensing of a Power BI subscription.</span></span>

 <span data-ttu-id="ef365-113">**Power BI Embedded** je určená pro přesněji tyto druhy scénářů.</span><span class="sxs-lookup"><span data-stu-id="ef365-113">**Power BI Embedded** is designed for precisely these kinds of scenarios.</span></span> <span data-ttu-id="ef365-114">Ano teď, když máme tento rychlý úvod mimo hello způsobem, se můžeme dát do některé podrobnosti</span><span class="sxs-lookup"><span data-stu-id="ef365-114">So, now that we have that quick introduction out of hello way, let's get into some details</span></span>

<span data-ttu-id="ef365-115">Můžete použít hello .NET \(C#) nebo Node.js SDK, tooeasily sestavení vaší aplikace pomocí Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="ef365-115">You can use hello .NET \(C#) or Node.js SDK, tooeasily build your application with Power BI Embedded.</span></span> <span data-ttu-id="ef365-116">Ale v tomto článku vám objasníme o toku HTTP \(včetně ověřovacího) z Power BI bez sady SDK.</span><span class="sxs-lookup"><span data-stu-id="ef365-116">But, in this article, we'll explain about HTTP flow \(incl. AuthN) of Power BI without SDKs.</span></span> <span data-ttu-id="ef365-117">Principy tento tok, můžete vytvořit aplikaci **s žádný programovací jazyk**, a porozumíte hluboko hello podstatu Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="ef365-117">Understanding this flow, you can build your application **with any programming language**, and you can understand deeply hello essence of Power BI Embedded.</span></span>

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a><span data-ttu-id="ef365-118">Kolekce pracovních prostorů vytvořit Power BI a přístupový klíč get \(zřizování)</span><span class="sxs-lookup"><span data-stu-id="ef365-118">Create Power BI workspace collection, and get access key \(Provisioning)</span></span>

<span data-ttu-id="ef365-119">Power BI Embedded je jedním z hello služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ef365-119">Power BI Embedded is one of hello Azure services.</span></span> <span data-ttu-id="ef365-120">Pouze hello ISV, který používá portál Azure je účtovat poplatky za použití \(za každou hodinu uživatelské relace), a hello uživatele, který sestavu hello zobrazení není účtovat nebo dokonce vyžadovat předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ef365-120">Only hello ISV who uses Azure Portal is charged for usage fees \(per hourly user session), and hello user who views hello report isn't charged or even require an Azure subscription.</span></span>
<span data-ttu-id="ef365-121">Před zahájením náš vývoj aplikací, musí vytvoříme hello **kolekce pracovních prostorů Power BI** pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ef365-121">Before starting our application development, we must create hello **Power BI workspace collection** by using Azure Portal.</span></span>

<span data-ttu-id="ef365-122">Každý prostoru Power BI Embedded je hello prostoru pro každý zákazníků (klientů), a přidáme mnoho pracovní prostory v každé kolekci pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="ef365-122">Each workspace of Power BI Embedded is hello workspace for each customer (tenant), and we can add many workspaces in each workspace collection.</span></span> <span data-ttu-id="ef365-123">Hello stejné přístupový klíč se používá v každé kolekci pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="ef365-123">hello same access key is used in each workspace collection.</span></span> <span data-ttu-id="ef365-124">V platit, kolekce pracovních prostorů hello je hello hranice zabezpečení pro Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="ef365-124">In-effect, hello workspace collection is hello security boundary for Power BI Embedded.</span></span>

![](media/power-bi-embedded-iframe/create-workspace.png)

<span data-ttu-id="ef365-125">Když jsme vytvoření kolekce pracovních prostorů hello zkopírujte hello přístupový klíč z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ef365-125">When we finish creating hello workspace collection, copy hello access key from Azure Portal.</span></span>

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> <span data-ttu-id="ef365-126">Také můžeme zřídit hello kolekce pracovních prostorů a získat přístupový klíč přes REST API.</span><span class="sxs-lookup"><span data-stu-id="ef365-126">We can also provision hello workspace collection and get access key via REST API.</span></span> <span data-ttu-id="ef365-127">Další, najdete v části toolearn [rozhraní API pro Power BI prostředků zprostředkovatele](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef365-127">toolearn more, see [Power BI Resource Provider APIs](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span></span>

## <a name="create-pbix-file-with-power-bi-desktop"></a><span data-ttu-id="ef365-128">Vytvoření souboru .pbix s Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="ef365-128">Create .pbix file with Power BI Desktop</span></span>

<span data-ttu-id="ef365-129">V dalším kroku musí vytvoříme hello datové připojení a vložených toobe sestavy.</span><span class="sxs-lookup"><span data-stu-id="ef365-129">Next, we must create hello data connection and reports toobe embedded.</span></span>
<span data-ttu-id="ef365-130">Pro tuto úlohu neexistuje žádné programování nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="ef365-130">For this task, there’s no programming or code.</span></span> <span data-ttu-id="ef365-131">Právě jsme pomocí Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="ef365-131">We just use Power BI Desktop.</span></span>
<span data-ttu-id="ef365-132">V tomto článku jsme nebude projít hello podrobnosti o způsobu toouse Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="ef365-132">In this article, we won't go through hello details about how toouse Power BI Desktop.</span></span> <span data-ttu-id="ef365-133">Pokud budete potřebovat některé zde, přečtěte [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="ef365-133">If you need some help here, see [Getting started with Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span></span> <span data-ttu-id="ef365-134">Pro náš příklad právě použijeme hello [prodejní analýzy ukázka](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span><span class="sxs-lookup"><span data-stu-id="ef365-134">For our example, we'll just use hello [Retail Analysis Sample](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span></span>

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a><span data-ttu-id="ef365-135">Vytvořit pracovní prostor Power BI</span><span class="sxs-lookup"><span data-stu-id="ef365-135">Create a Power BI workspace</span></span>

<span data-ttu-id="ef365-136">Teď, když zřizování hello vše na jednom, můžeme začít vytváření pracovního prostoru zákazníka v kolekci pracovních prostorů hello přes rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="ef365-136">Now that hello provisioning is all done, let’s get started creating a customer’s workspace in hello workspace collection via REST APIs.</span></span> <span data-ttu-id="ef365-137">Hello následující HTTP POST požadavku (REST) vytváří nový pracovní prostor hello v našem existující kolekci pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="ef365-137">hello following HTTP POST Request (REST) is creating hello new workspace in our existing workspace collection.</span></span> <span data-ttu-id="ef365-138">Toto je hello [POST prostoru API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef365-138">This is hello [POST Workspace API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span> <span data-ttu-id="ef365-139">V našem příkladu se název kolekce prostoru hello **mypbiapp**.</span><span class="sxs-lookup"><span data-stu-id="ef365-139">In our example, hello workspace collection name is **mypbiapp**.</span></span> <span data-ttu-id="ef365-140">Právě nastavujeme hello přístupový klíč, které jsme dříve zkopírovat, jako **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="ef365-140">We just set hello access key, which we previously copied, as **AppKey**.</span></span> <span data-ttu-id="ef365-141">Je velmi jednoduché ověřování!</span><span class="sxs-lookup"><span data-stu-id="ef365-141">It’s very simple authentication!</span></span>

<span data-ttu-id="ef365-142">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="ef365-142">**HTTP Request**</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="ef365-143">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="ef365-143">**HTTP Response**</span></span>

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

<span data-ttu-id="ef365-144">Hello vrátil **workspaceId** se používá pro hello následující následných volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ef365-144">hello returned **workspaceId** is used for hello following subsequent API calls.</span></span> <span data-ttu-id="ef365-145">Aplikace musí zachovat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ef365-145">Our application must retain this value.</span></span>

## <a name="import-pbix-file-into-hello-workspace"></a><span data-ttu-id="ef365-146">Import souboru .pbix do pracovního prostoru hello</span><span class="sxs-lookup"><span data-stu-id="ef365-146">Import .pbix file into hello workspace</span></span>

<span data-ttu-id="ef365-147">Každé sestavy v pracovním prostoru odpovídá tooa jednoho souboru Power BI Desktop s datovou sadu \(včetně nastavení zdroje dat).</span><span class="sxs-lookup"><span data-stu-id="ef365-147">Each report in a workspace corresponds tooa single Power BI Desktop file with a dataset \(including datasource settings).</span></span> <span data-ttu-id="ef365-148">Jsme můžete importovat naše .pbix souboru toohello prostoru jak je znázorněno v následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-148">We can import our .pbix file toohello workspace as shown in hello code below.</span></span> <span data-ttu-id="ef365-149">Jak vidíte, jsme nahrát hello binární souboru .pbix MIME multipart pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef365-149">As you can see, we can upload hello binary of .pbix file using MIME multipart in http.</span></span>

<span data-ttu-id="ef365-150">Hello uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** hello ID pracovního prostoru, a parametr dotazu **datasetDisplayName** je toocreate název datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-150">hello uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** is hello workspaceId, and query parameter **datasetDisplayName** is hello dataset name toocreate.</span></span> <span data-ttu-id="ef365-151">Hello vytvořit datová sada obsahuje všechna data související s artefakty v souboru .pbix, například importovaných dat hello zdroj dat toohello ukazatel atd...</span><span class="sxs-lookup"><span data-stu-id="ef365-151">hello created dataset holds all data related artifacts in .pbix file such as imported data, hello pointer toohello data source, etc..</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{hello content (binary) of .pbix file}
--A300testx--
```

<span data-ttu-id="ef365-152">Může nějakou dobu spuštění této úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="ef365-152">This import task might run for a while.</span></span> <span data-ttu-id="ef365-153">Po dokončení můžete pokládat naše aplikace hello stav úlohy pomocí id importu. V našem příkladu je id importu hello **4eec64dd-533b-47c3-a72c-6508ad854659**.</span><span class="sxs-lookup"><span data-stu-id="ef365-153">When complete, our application can ask hello task status using import id. In our example, hello import id is **4eec64dd-533b-47c3-a72c-6508ad854659**.</span></span>

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

<span data-ttu-id="ef365-154">Následující Hello je žádostí stavu pomocí tohoto id importu:</span><span class="sxs-lookup"><span data-stu-id="ef365-154">hello following is asking status using this import id:</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="ef365-155">Pokud úloha hello není kompletní, hello odpovědi HTTP může být například takto:</span><span class="sxs-lookup"><span data-stu-id="ef365-155">If hello task isn't complete, hello HTTP response could be like this:</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

<span data-ttu-id="ef365-156">Pokud hello úkol je dokončen, může být hello odpověď HTTP další takto:</span><span class="sxs-lookup"><span data-stu-id="ef365-156">If hello task is complete, hello HTTP response could be more like this:</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a><span data-ttu-id="ef365-157">Připojení zdroje dat \(a víceklientský dat)</span><span class="sxs-lookup"><span data-stu-id="ef365-157">Data source connectivity \(and multi-tenancy of data)</span></span>

<span data-ttu-id="ef365-158">Při téměř všechny hello artefaktů v souboru .pbix jsou importovány do našich pracovního prostoru, hello přihlašovací údaje pro zdroje dat nejsou.</span><span class="sxs-lookup"><span data-stu-id="ef365-158">While almost all of hello artifacts in .pbix file are imported into our workspace, hello  credentials for data sources are not.</span></span> <span data-ttu-id="ef365-159">V důsledku toho, při použití **režimu DirectQuery**, hello vloženou sestavu nelze zobrazit správně.</span><span class="sxs-lookup"><span data-stu-id="ef365-159">As a result, when using **DirectQuery mode**, hello embedded report cannot be shown correctly.</span></span> <span data-ttu-id="ef365-160">Ale při použití **režimu Import**, jsme můžete zobrazit sestavy hello pomocí stávající importovaných dat hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-160">But, when using **Import mode**, we can view hello report using hello existing imported data.</span></span> <span data-ttu-id="ef365-161">V takovém případě musí nastaví hello pověření pomocí následujících kroků prostřednictvím volání REST hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-161">In such a case, we must set hello credential using hello following steps via REST calls.</span></span>

<span data-ttu-id="ef365-162">Nejprve se nám musí získat hello gateway datasource.</span><span class="sxs-lookup"><span data-stu-id="ef365-162">First, we must get hello gateway datasource.</span></span> <span data-ttu-id="ef365-163">Víme, datová sada hello **id** je hello předtím vrátila id.</span><span class="sxs-lookup"><span data-stu-id="ef365-163">We know hello dataset **id** is hello previously returned id.</span></span>

<span data-ttu-id="ef365-164">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="ef365-164">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="ef365-165">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="ef365-165">**HTTP Response**</span></span>

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

<span data-ttu-id="ef365-166">Pomocí hello vrátil id brány a id zdroje dat \(najdete v části hello předchozí **gatewayId** a **id** v hello vrátí výsledek), jsme následujícím způsobem změnit pověření hello tohoto zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="ef365-166">Using hello returned gateway id and datasource id \(see hello previous **gatewayId** and **id** in hello returned result), we can change hello credential of this datasource as follows:</span></span>

<span data-ttu-id="ef365-167">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="ef365-167">**HTTP Request**</span></span>

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

<span data-ttu-id="ef365-168">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="ef365-168">**HTTP Response**</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

<span data-ttu-id="ef365-169">V produkčním prostředí jsme můžete také nastavit hello různých připojovací řetězec pro každý pracovní prostor pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="ef365-169">In production, we can also set hello different connection string for each workspace using REST API.</span></span> <span data-ttu-id="ef365-170">\(tj, jsme můžete oddělit hello databáze pro každý zákazníky.)</span><span class="sxs-lookup"><span data-stu-id="ef365-170">\(i.e, we can separate hello database for each customers.)</span></span>

<span data-ttu-id="ef365-171">Následující Hello mění hello připojovací řetězec zdroje dat přes REST.</span><span class="sxs-lookup"><span data-stu-id="ef365-171">hello following is changing hello connection string of datasource via REST.</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

<span data-ttu-id="ef365-172">Používáme zabezpečení na úrovni řádků v Power BI Embedded a jsme můžete oddělit hello dat pro každého uživatele v jednu sestavu.</span><span class="sxs-lookup"><span data-stu-id="ef365-172">Or, we can use Row Level Security in Power BI Embedded and we can separate hello data for each users in one report.</span></span> <span data-ttu-id="ef365-173">As a result, jsme můžete zřídit každou zprávu zákazníků s stejné .pbix \(uživatelského rozhraní, atd.) a různé datové zdroje.</span><span class="sxs-lookup"><span data-stu-id="ef365-173">As a result, we can provision each customer report with same .pbix \(UI, etc.) and different data sources.</span></span>

> [!NOTE]
> <span data-ttu-id="ef365-174">Pokud používáte **režimu Import** místo **režimu DirectQuery**, neexistuje žádný způsob, jak toorefresh modely prostřednictvím rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ef365-174">If you’re using **Import mode** instead of **DirectQuery mode**, there’s no way toorefresh models via API.</span></span> <span data-ttu-id="ef365-175">A v Power BI Embedded ještě není podporovaný místní zdroje dat prostřednictvím brány Power BI.</span><span class="sxs-lookup"><span data-stu-id="ef365-175">And, on-premises datasources through Power BI gateway isn't yet supported in Power BI Embedded.</span></span> <span data-ttu-id="ef365-176">Však budete opravdu chtějí tookeep přehled o hello [Power BI blog](https://powerbi.microsoft.com/blog/) pro co je nového a co Připravujeme v budoucnosti uvolní.</span><span class="sxs-lookup"><span data-stu-id="ef365-176">However, you'll really want tookeep an eye on hello [Power BI blog](https://powerbi.microsoft.com/blog/) for what's new and what's coming in future releases.</span></span>

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a><span data-ttu-id="ef365-177">Ověřování a hostování (vnoření) sestavách v naší webové stránky</span><span class="sxs-lookup"><span data-stu-id="ef365-177">Authentication and hosting (embedding) reports in our web page</span></span>

<span data-ttu-id="ef365-178">V hello předchozí REST API, můžeme použít hello přístupový klíč **AppKey** sám sebe jako hello autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="ef365-178">In hello previous REST API, we can use hello access key **AppKey** itself as hello authorization header.</span></span> <span data-ttu-id="ef365-179">Protože tyto volání může být zpracována na straně serveru back-end hello, je bezpečné.</span><span class="sxs-lookup"><span data-stu-id="ef365-179">Because these calls can be handled on hello backend server side, it's safe.</span></span>

<span data-ttu-id="ef365-180">Ale po jsme vložení sestavy hello naše webové stránce, tento druh informací o zabezpečení by být zpracována pomocí jazyka JavaScript \(front-endu).</span><span class="sxs-lookup"><span data-stu-id="ef365-180">But, when we embed hello report in our web page, this kind of security information would be handled using JavaScript \(frontend).</span></span> <span data-ttu-id="ef365-181">Pak musí být zabezpečená hello hodnota hlavičky ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef365-181">Then hello authorization header value must be secured.</span></span> <span data-ttu-id="ef365-182">Pokud uživatel se zlými úmysly nebo škodlivý kód je zjištěno naše přístupový klíč, můžou volat jakékoli operace pomocí tohoto klíče.</span><span class="sxs-lookup"><span data-stu-id="ef365-182">If our access key is discovered by a malicious user or malicious code, they can call any operations using this key.</span></span>

<span data-ttu-id="ef365-183">Když jsme vložení sestavy hello naše webové stránce, musí používáme počítaný token hello místo přístupový klíč **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="ef365-183">When we embed hello report in our web page, we must use hello computed token instead of access key **AppKey**.</span></span> <span data-ttu-id="ef365-184">Naše aplikace musíte vytvořit hello OAuth webových tokenů Json \(JWT) který se skládá z hello deklarace identity a hello počítaný digitální podpis.</span><span class="sxs-lookup"><span data-stu-id="ef365-184">Our application must create hello OAuth Json Web Token \(JWT) which consists of hello claims and hello computed digital signature.</span></span> <span data-ttu-id="ef365-185">Jak je uvedeno níže, je tento token JWT OAuth tokeny řetězec s kódováním oddělené tečkou.</span><span class="sxs-lookup"><span data-stu-id="ef365-185">As illustrated below, this OAuth JWT is dot-delimited encoded string tokens.</span></span>

![](media/power-bi-embedded-iframe/oauth-jwt.png)

<span data-ttu-id="ef365-186">Nejdřív jsme musíte připravit hello vstupní hodnoty, který je podepsaný později.</span><span class="sxs-lookup"><span data-stu-id="ef365-186">First, we must prepare hello input value, which is signed later.</span></span> <span data-ttu-id="ef365-187">Tato hodnota je řetězec kódovaná adresou url (rfc4648) base64 hello hello následující json a ty jsou odděleny tečkou hello \(.) znaků.</span><span class="sxs-lookup"><span data-stu-id="ef365-187">This value is hello base64 url encoded (rfc4648) string of hello following json, and these are delimited by hello dot \(.) character.</span></span> <span data-ttu-id="ef365-188">Později vysvětlíme, jak tooget hello id sestavy.</span><span class="sxs-lookup"><span data-stu-id="ef365-188">Later, we'll explain how tooget hello report id.</span></span>

> [!NOTE]
> <span data-ttu-id="ef365-189">Pokud chceme toouse zabezpečení úrovni řádků (RLS) s Power BI Embedded, jsme musíte zadat také **uživatelské jméno** a **role** v deklaracích identity hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-189">If we want toouse Row Level Security (RLS) with Power BI Embedded, we must also specify **username** and **roles** in hello claims.</span></span>

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

<span data-ttu-id="ef365-190">V dalším kroku musí vytvoříme hello řetězec s kódováním base64 z HMAC \(hello podpis) s algoritmem SHA256.</span><span class="sxs-lookup"><span data-stu-id="ef365-190">Next, we must create hello base64 encoded string of HMAC \(hello signature) with SHA256 algorithm.</span></span> <span data-ttu-id="ef365-191">Tato podepsaný vstupní hodnota je řetězec předchozí hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-191">This signed input value is hello previous string.</span></span>

<span data-ttu-id="ef365-192">Poslední, jsme musíte kombinovat hello vstupní hodnoty a řetězec podpis pomocí období \(.) znaků.</span><span class="sxs-lookup"><span data-stu-id="ef365-192">Last, we must combine hello input value and signature string using period \(.) character.</span></span> <span data-ttu-id="ef365-193">řetězec Hello dokončit je hello tokenu aplikace pro vložení sestavy hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-193">hello completed string is hello app token for hello report embedding.</span></span> <span data-ttu-id="ef365-194">I v případě, že uživatel se zlými úmysly zjišťováno tokenu aplikace hello, nemají přístup hello původní přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="ef365-194">Even if hello app token is discovered by a malicious user, they cannot get hello original access key.</span></span> <span data-ttu-id="ef365-195">Tento token aplikace rychle vyprší.</span><span class="sxs-lookup"><span data-stu-id="ef365-195">This app token will expire quickly.</span></span>

<span data-ttu-id="ef365-196">Tady je příklad PHP pro tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="ef365-196">Here's a PHP example for these steps:</span></span>

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is hello apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-hello-report-into-hello-web-page"></a><span data-ttu-id="ef365-197">Nakonec vložení sestavy hello do hello webové stránky</span><span class="sxs-lookup"><span data-stu-id="ef365-197">Finally, embed hello report into hello web page</span></span>

<span data-ttu-id="ef365-198">Pro vkládání naše sestavy, nám musí získat hello vložte adresu url a sestava **id** pomocí hello následující rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="ef365-198">For embedding our report, we must get hello embed url and report **id** using hello following REST API.</span></span>

<span data-ttu-id="ef365-199">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="ef365-199">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="ef365-200">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="ef365-200">**HTTP Response**</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

<span data-ttu-id="ef365-201">V našem webovou aplikaci pomocí tokenu aplikace předchozí hello jsme můžete vložení sestavy hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-201">We can embed hello report in our web app using hello previous app token.</span></span>
<span data-ttu-id="ef365-202">Pokud se podíváme na hello Další ukázkový kód, dřív používaná část hello je hello stejný jako předchozí příklad hello.</span><span class="sxs-lookup"><span data-stu-id="ef365-202">If we look at hello next sample code, hello former part is hello same as hello previous example.</span></span> <span data-ttu-id="ef365-203">V pozdější části hello, tento příklad ukazuje hello **embedUrl** \(viz předchozí výsledek hello) v hello iframe a je publikování tokenu aplikace hello do hello iframe.</span><span class="sxs-lookup"><span data-stu-id="ef365-203">In hello latter part, this sample shows hello **embedUrl** \(see hello previous result) in hello iframe, and is posting hello app token into hello iframe.</span></span>

> [!NOTE]
> <span data-ttu-id="ef365-204">Budete potřebovat toochange hello sestavy id hodnota tooone vlastní.</span><span class="sxs-lookup"><span data-stu-id="ef365-204">You'll need toochange hello report id value tooone of your own.</span></span> <span data-ttu-id="ef365-205">Navíc z důvodu chyb tooa v našem systému správy obsahu, značku iframe hello v ukázce kódu hello je pro čtení oznámena.</span><span class="sxs-lookup"><span data-stu-id="ef365-205">Also, due tooa bug in our content management system, hello iframe tag in hello code sample is read literally.</span></span> <span data-ttu-id="ef365-206">Odeberte textu hello omezená ze hello značky, pokud je kopírujete a vkládáte ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="ef365-206">Remove hello capped text from hello tag if you copy and paste this sample code.</span></span>

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

<span data-ttu-id="ef365-207">A tady je naše výsledek:</span><span class="sxs-lookup"><span data-stu-id="ef365-207">And here's our result:</span></span>

![](media/power-bi-embedded-iframe/view-report.png)

<span data-ttu-id="ef365-208">V tomto okamžiku Power BI Embedded pouze zobrazuje hello sestavu v hello iframe.</span><span class="sxs-lookup"><span data-stu-id="ef365-208">At this time, Power BI Embedded only shows hello report in hello iframe.</span></span> <span data-ttu-id="ef365-209">Ale dohlížet na hello [Power BI Blog](https://powerbi.microsoft.com/blog/).</span><span class="sxs-lookup"><span data-stu-id="ef365-209">But, keep an eye on hello [Power BI Blog](https://powerbi.microsoft.com/blog/).</span></span> <span data-ttu-id="ef365-210">Budoucí vylepšení použít nové na straně klienta rozhraní API, která bude dejte nám odeslat informace do hello iframe a také získat informace o. Zajímavé věcem!</span><span class="sxs-lookup"><span data-stu-id="ef365-210">Future improvements could use new client side APIs that will let us send information into hello iframe as well as get information out. Exciting stuff!</span></span>

## <a name="see-also"></a><span data-ttu-id="ef365-211">Viz také</span><span class="sxs-lookup"><span data-stu-id="ef365-211">See also</span></span>
* [<span data-ttu-id="ef365-212">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="ef365-212">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)

<span data-ttu-id="ef365-213">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="ef365-213">More questions?</span></span> [<span data-ttu-id="ef365-214">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="ef365-214">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

