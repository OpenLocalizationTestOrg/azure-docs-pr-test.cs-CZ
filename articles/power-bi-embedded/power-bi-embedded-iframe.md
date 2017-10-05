---
title: "Jak používat Power BI Embedded se zbytkem | Microsoft Docs"
description: "Naučte se používat se zbytkem Power BI Embedded "
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
ms.openlocfilehash: 31624b9d15772a4f08cf013ac713b3aa636acfca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-power-bi-embedded-with-rest"></a><span data-ttu-id="3b977-103">Jak používat Power BI Embedded službou REST</span><span class="sxs-lookup"><span data-stu-id="3b977-103">How to use Power BI Embedded with REST</span></span>

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a><span data-ttu-id="3b977-104">Power BI Embedded: Co je a co je pro</span><span class="sxs-lookup"><span data-stu-id="3b977-104">Power BI Embedded: What it is and what it's for</span></span>

<span data-ttu-id="3b977-105">Přehled Power BI Embedded je popsán v oficiálním [Power BI Embedded lokality](https://azure.microsoft.com/services/power-bi-embedded/), ale podívejme se rychlé předtím, než se nám získat na podrobné informace o používání s REST.</span><span class="sxs-lookup"><span data-stu-id="3b977-105">An overview of Power BI Embedded is described in the official [Power BI Embedded site](https://azure.microsoft.com/services/power-bi-embedded/), but let's take a quick look before we get into the details about using it with REST.</span></span>

<span data-ttu-id="3b977-106">Je to úplně jednoduché, skutečně.</span><span class="sxs-lookup"><span data-stu-id="3b977-106">It's quite simple, really.</span></span> <span data-ttu-id="3b977-107">můžete chtít použít vizualizace dynamická data [Power BI](https://powerbi.microsoft.com) ve vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3b977-107">you may want to use the dynamic data visualizations of [Power BI](https://powerbi.microsoft.com) in your own application.</span></span>

<span data-ttu-id="3b977-108">Většina vlastní aplikace potřebují k poskytování dat pro vlastní zákazníky a nemusí uživatelé ve vlastní organizaci.</span><span class="sxs-lookup"><span data-stu-id="3b977-108">Most custom applications need to deliver the data for their own customers, not necessarily users in their own organization.</span></span> <span data-ttu-id="3b977-109">Například pokud jste doručování některé služby pro firmy A a B společnosti, uživatelům ve společnosti A by měl viděli pouze data pro své vlastní společnosti A. To znamená je potřeba více klientů pro doručení.</span><span class="sxs-lookup"><span data-stu-id="3b977-109">For example, if you're delivering some service for both company A and company B, users in company A should only see data for their own company A. That is, the multi-tenancy is needed for the delivery.</span></span>

<span data-ttu-id="3b977-110">Vlastní aplikace může také nabízí vlastní metody ověřování, jako je například ověřování formulářů, základní ověřování, atd...</span><span class="sxs-lookup"><span data-stu-id="3b977-110">The custom application might also be offering its own authentication methods such as forms auth, basic auth, etc..</span></span> <span data-ttu-id="3b977-111">Potom vnoření řešení musí spolupracovat s tento existující metody ověřování bezpečně.</span><span class="sxs-lookup"><span data-stu-id="3b977-111">Then, the embedding solution must collaborate with this existing authentication methods safely.</span></span> <span data-ttu-id="3b977-112">Je také nutné uživatelům umožnit používat tyto aplikace ISV bez další nákupu nebo licencování předplatného Power BI.</span><span class="sxs-lookup"><span data-stu-id="3b977-112">It's also necessary for users to be able to use those ISV applications without the extra purchase or licensing of a Power BI subscription.</span></span>

 <span data-ttu-id="3b977-113">**Power BI Embedded** je určená pro přesněji tyto druhy scénářů.</span><span class="sxs-lookup"><span data-stu-id="3b977-113">**Power BI Embedded** is designed for precisely these kinds of scenarios.</span></span> <span data-ttu-id="3b977-114">Ano teď, když máme tento rychlý úvod stranou, se můžeme dát do některé podrobnosti</span><span class="sxs-lookup"><span data-stu-id="3b977-114">So, now that we have that quick introduction out of the way, let's get into some details</span></span>

<span data-ttu-id="3b977-115">Můžete použít .NET \(C#) nebo Node.js SDK, snadno vytvářet aplikace s Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="3b977-115">You can use the .NET \(C#) or Node.js SDK, to easily build your application with Power BI Embedded.</span></span> <span data-ttu-id="3b977-116">Ale v tomto článku vám objasníme o toku HTTP \(včetně ověřovacího) z Power BI bez sady SDK.</span><span class="sxs-lookup"><span data-stu-id="3b977-116">But, in this article, we'll explain about HTTP flow \(incl. AuthN) of Power BI without SDKs.</span></span> <span data-ttu-id="3b977-117">Principy tento tok, můžete vytvořit aplikaci **s žádný programovací jazyk**, a porozumíte hluboko podstatu Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="3b977-117">Understanding this flow, you can build your application **with any programming language**, and you can understand deeply the essence of Power BI Embedded.</span></span>

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a><span data-ttu-id="3b977-118">Kolekce pracovních prostorů vytvořit Power BI a přístupový klíč get \(zřizování)</span><span class="sxs-lookup"><span data-stu-id="3b977-118">Create Power BI workspace collection, and get access key \(Provisioning)</span></span>

<span data-ttu-id="3b977-119">Power BI Embedded je jedním ze služby Azure.</span><span class="sxs-lookup"><span data-stu-id="3b977-119">Power BI Embedded is one of the Azure services.</span></span> <span data-ttu-id="3b977-120">Pouze ISV, který používá portál Azure je účtovat poplatky za použití \(za každou hodinu uživatelské relace), a uživatel, který si prohlíží sestavy není účtován nebo dokonce předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="3b977-120">Only the ISV who uses Azure Portal is charged for usage fees \(per hourly user session), and the user who views the report isn't charged or even require an Azure subscription.</span></span>
<span data-ttu-id="3b977-121">Před zahájením náš vývoj aplikací, jsme musí vytvořit **kolekce pracovních prostorů Power BI** pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b977-121">Before starting our application development, we must create the **Power BI workspace collection** by using Azure Portal.</span></span>

<span data-ttu-id="3b977-122">Každý prostoru Power BI Embedded je pracovním prostoru pro každý zákazníků (klientů), a přidáme mnoho pracovní prostory v každé kolekci pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="3b977-122">Each workspace of Power BI Embedded is the workspace for each customer (tenant), and we can add many workspaces in each workspace collection.</span></span> <span data-ttu-id="3b977-123">Stejné přístupový klíč se používá v každé kolekci pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="3b977-123">The same access key is used in each workspace collection.</span></span> <span data-ttu-id="3b977-124">V platit, kolekce pracovních prostorů je hranicí zabezpečení pro Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="3b977-124">In-effect, the workspace collection is the security boundary for Power BI Embedded.</span></span>

![](media/power-bi-embedded-iframe/create-workspace.png)

<span data-ttu-id="3b977-125">Když jsme vytvoření kolekce pracovních prostorů zkopírujte přístupový klíč z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b977-125">When we finish creating the workspace collection, copy the access key from Azure Portal.</span></span>

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> <span data-ttu-id="3b977-126">Také můžeme zřídit kolekce pracovních prostorů a získat přístupový klíč přes REST API.</span><span class="sxs-lookup"><span data-stu-id="3b977-126">We can also provision the workspace collection and get access key via REST API.</span></span> <span data-ttu-id="3b977-127">Další informace najdete v tématu [rozhraní API pro Power BI prostředků zprostředkovatele](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b977-127">To learn more, see [Power BI Resource Provider APIs](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span></span>

## <a name="create-pbix-file-with-power-bi-desktop"></a><span data-ttu-id="3b977-128">Vytvoření souboru .pbix s Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="3b977-128">Create .pbix file with Power BI Desktop</span></span>

<span data-ttu-id="3b977-129">V dalším kroku jsme musíte vytvořit datové připojení a sestavy, které má být vložen.</span><span class="sxs-lookup"><span data-stu-id="3b977-129">Next, we must create the data connection and reports to be embedded.</span></span>
<span data-ttu-id="3b977-130">Pro tuto úlohu neexistuje žádné programování nebo kódu.</span><span class="sxs-lookup"><span data-stu-id="3b977-130">For this task, there’s no programming or code.</span></span> <span data-ttu-id="3b977-131">Právě jsme pomocí Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="3b977-131">We just use Power BI Desktop.</span></span>
<span data-ttu-id="3b977-132">V tomto článku jsme nebude projít informace o tom, jak používat Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="3b977-132">In this article, we won't go through the details about how to use Power BI Desktop.</span></span> <span data-ttu-id="3b977-133">Pokud budete potřebovat některé zde, přečtěte [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="3b977-133">If you need some help here, see [Getting started with Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span></span> <span data-ttu-id="3b977-134">Pro náš příklad právě použijeme [prodejní analýzy ukázka](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span><span class="sxs-lookup"><span data-stu-id="3b977-134">For our example, we'll just use the [Retail Analysis Sample](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span></span>

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a><span data-ttu-id="3b977-135">Vytvořit pracovní prostor Power BI</span><span class="sxs-lookup"><span data-stu-id="3b977-135">Create a Power BI workspace</span></span>

<span data-ttu-id="3b977-136">Teď, když zajišťování vše na jednom, můžeme začít vytváření pracovního prostoru zákazníka v kolekci pracovních prostorů přes rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="3b977-136">Now that the provisioning is all done, let’s get started creating a customer’s workspace in the workspace collection via REST APIs.</span></span> <span data-ttu-id="3b977-137">Následující HTTP POST požadavku (REST) vytváří nový pracovní prostor v našem existující kolekci pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="3b977-137">The following HTTP POST Request (REST) is creating the new workspace in our existing workspace collection.</span></span> <span data-ttu-id="3b977-138">Toto je [POST prostoru API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b977-138">This is the [POST Workspace API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span> <span data-ttu-id="3b977-139">V našem příkladu je název kolekce prostoru **mypbiapp**.</span><span class="sxs-lookup"><span data-stu-id="3b977-139">In our example, the workspace collection name is **mypbiapp**.</span></span> <span data-ttu-id="3b977-140">Právě nastavujeme přístupový klíč, které jsme dříve zkopírovat, jako **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="3b977-140">We just set the access key, which we previously copied, as **AppKey**.</span></span> <span data-ttu-id="3b977-141">Je velmi jednoduché ověřování!</span><span class="sxs-lookup"><span data-stu-id="3b977-141">It’s very simple authentication!</span></span>

<span data-ttu-id="3b977-142">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="3b977-142">**HTTP Request**</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="3b977-143">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="3b977-143">**HTTP Response**</span></span>

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

<span data-ttu-id="3b977-144">Vrácený **workspaceId** se používá pro následující následných volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3b977-144">The returned **workspaceId** is used for the following subsequent API calls.</span></span> <span data-ttu-id="3b977-145">Aplikace musí zachovat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3b977-145">Our application must retain this value.</span></span>

## <a name="import-pbix-file-into-the-workspace"></a><span data-ttu-id="3b977-146">Import souboru .pbix do pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="3b977-146">Import .pbix file into the workspace</span></span>

<span data-ttu-id="3b977-147">Každé sestavy v pracovním prostoru odpovídá do jednoho souboru Power BI Desktop s datovou sadu \(včetně nastavení zdroje dat).</span><span class="sxs-lookup"><span data-stu-id="3b977-147">Each report in a workspace corresponds to a single Power BI Desktop file with a dataset \(including datasource settings).</span></span> <span data-ttu-id="3b977-148">Naše souboru .pbix jsme můžete importovat do pracovního prostoru, jak je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="3b977-148">We can import our .pbix file to the workspace as shown in the code below.</span></span> <span data-ttu-id="3b977-149">Jak vidíte, jsme nahrát binárního souboru .pbix souboru MIME multipart pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b977-149">As you can see, we can upload the binary of .pbix file using MIME multipart in http.</span></span>

<span data-ttu-id="3b977-150">Identifikátor uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** je ID pracovního prostoru a parametr dotazu **datasetDisplayName** je název datové sady, které chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="3b977-150">The uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** is the workspaceId, and query parameter **datasetDisplayName** is the dataset name to create.</span></span> <span data-ttu-id="3b977-151">Vytvořenou datovou sadu obsahuje všechna data související s artefakty v .pbix souboru, například jako importovaných dat, má ukazatel na zdroji dat atd...</span><span class="sxs-lookup"><span data-stu-id="3b977-151">The created dataset holds all data related artifacts in .pbix file such as imported data, the pointer to the data source, etc..</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

<span data-ttu-id="3b977-152">Může nějakou dobu spuštění této úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="3b977-152">This import task might run for a while.</span></span> <span data-ttu-id="3b977-153">Po dokončení můžete pokládat naší aplikaci pomocí id importu stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="3b977-153">When complete, our application can ask the task status using import id.</span></span> <span data-ttu-id="3b977-154">V našem příkladu je id importu **4eec64dd-533b-47c3-a72c-6508ad854659**.</span><span class="sxs-lookup"><span data-stu-id="3b977-154">In our example, the import id is **4eec64dd-533b-47c3-a72c-6508ad854659**.</span></span>

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

<span data-ttu-id="3b977-155">Toto je žádostí stavu pomocí tohoto id importu:</span><span class="sxs-lookup"><span data-stu-id="3b977-155">The following is asking status using this import id:</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="3b977-156">Pokud úloha není kompletní, může být odpověď HTTP takto:</span><span class="sxs-lookup"><span data-stu-id="3b977-156">If the task isn't complete, the HTTP response could be like this:</span></span>

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

<span data-ttu-id="3b977-157">Pokud úloha skončí, může být odpověď HTTP další takto:</span><span class="sxs-lookup"><span data-stu-id="3b977-157">If the task is complete, the HTTP response could be more like this:</span></span>

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

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a><span data-ttu-id="3b977-158">Připojení zdroje dat \(a víceklientský dat)</span><span class="sxs-lookup"><span data-stu-id="3b977-158">Data source connectivity \(and multi-tenancy of data)</span></span>

<span data-ttu-id="3b977-159">Při téměř všechny artefaktů v souboru .pbix jsou importovány do našich pracovního prostoru, přihlašovací údaje pro zdroje dat nejsou.</span><span class="sxs-lookup"><span data-stu-id="3b977-159">While almost all of the artifacts in .pbix file are imported into our workspace, the  credentials for data sources are not.</span></span> <span data-ttu-id="3b977-160">V důsledku toho, při použití **režimu DirectQuery**, vloženou sestavu nelze zobrazit správně.</span><span class="sxs-lookup"><span data-stu-id="3b977-160">As a result, when using **DirectQuery mode**, the embedded report cannot be shown correctly.</span></span> <span data-ttu-id="3b977-161">Ale při použití **režimu Import**, jsme můžete zobrazit sestavy pomocí stávající naimportovaná data.</span><span class="sxs-lookup"><span data-stu-id="3b977-161">But, when using **Import mode**, we can view the report using the existing imported data.</span></span> <span data-ttu-id="3b977-162">V takovém případě jsme musí nastavit přihlašovací údaje, pomocí následujících kroků prostřednictvím volání REST.</span><span class="sxs-lookup"><span data-stu-id="3b977-162">In such a case, we must set the credential using the following steps via REST calls.</span></span>

<span data-ttu-id="3b977-163">Nejprve se nám musí získat zdroj dat brány.</span><span class="sxs-lookup"><span data-stu-id="3b977-163">First, we must get the gateway datasource.</span></span> <span data-ttu-id="3b977-164">Víme, datová sada **id** je dříve vrácený id.</span><span class="sxs-lookup"><span data-stu-id="3b977-164">We know the dataset **id** is the previously returned id.</span></span>

<span data-ttu-id="3b977-165">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="3b977-165">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="3b977-166">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="3b977-166">**HTTP Response**</span></span>

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

<span data-ttu-id="3b977-167">Pomocí id vrácený brány a id zdroje dat \(najdete v předchozí **gatewayId** a **id** v vrácených výsledků), jsme následujícím způsobem změnit pověření pro tohoto zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="3b977-167">Using the returned gateway id and datasource id \(see the previous **gatewayId** and **id** in the returned result), we can change the credential of this datasource as follows:</span></span>

<span data-ttu-id="3b977-168">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="3b977-168">**HTTP Request**</span></span>

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

<span data-ttu-id="3b977-169">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="3b977-169">**HTTP Response**</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

<span data-ttu-id="3b977-170">V produkčním prostředí jsme také můžete nastavit různé připojovací řetězec pro každý pracovní prostor pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="3b977-170">In production, we can also set the different connection string for each workspace using REST API.</span></span> <span data-ttu-id="3b977-171">\(tj, jsme lze jednotlivé databáze pro každý zákazníky.)</span><span class="sxs-lookup"><span data-stu-id="3b977-171">\(i.e, we can separate the database for each customers.)</span></span>

<span data-ttu-id="3b977-172">Následující mění řetězec připojení zdroje dat přes REST.</span><span class="sxs-lookup"><span data-stu-id="3b977-172">The following is changing the connection string of datasource via REST.</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

<span data-ttu-id="3b977-173">Používáme zabezpečení na úrovni řádků v Power BI Embedded a jsme můžete oddělit data pro každého uživatele v jednu sestavu.</span><span class="sxs-lookup"><span data-stu-id="3b977-173">Or, we can use Row Level Security in Power BI Embedded and we can separate the data for each users in one report.</span></span> <span data-ttu-id="3b977-174">As a result, jsme můžete zřídit každou zprávu zákazníků s stejné .pbix \(uživatelského rozhraní, atd.) a různé datové zdroje.</span><span class="sxs-lookup"><span data-stu-id="3b977-174">As a result, we can provision each customer report with same .pbix \(UI, etc.) and different data sources.</span></span>

> [!NOTE]
> <span data-ttu-id="3b977-175">Pokud používáte **režimu Import** místo **režimu DirectQuery**, neexistuje žádný způsob, jak aktualizovat modely prostřednictvím rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3b977-175">If you’re using **Import mode** instead of **DirectQuery mode**, there’s no way to refresh models via API.</span></span> <span data-ttu-id="3b977-176">A v Power BI Embedded ještě není podporovaný místní zdroje dat prostřednictvím brány Power BI.</span><span class="sxs-lookup"><span data-stu-id="3b977-176">And, on-premises datasources through Power BI gateway isn't yet supported in Power BI Embedded.</span></span> <span data-ttu-id="3b977-177">Však budete Opravdu chcete dohlížet na [Power BI blog](https://powerbi.microsoft.com/blog/) pro co je nového a co Připravujeme v budoucnosti uvolní.</span><span class="sxs-lookup"><span data-stu-id="3b977-177">However, you'll really want to keep an eye on the [Power BI blog](https://powerbi.microsoft.com/blog/) for what's new and what's coming in future releases.</span></span>

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a><span data-ttu-id="3b977-178">Ověřování a hostování (vnoření) sestavách v naší webové stránky</span><span class="sxs-lookup"><span data-stu-id="3b977-178">Authentication and hosting (embedding) reports in our web page</span></span>

<span data-ttu-id="3b977-179">V předchozích REST API, můžeme použít přístupový klíč **AppKey** sám sebe jako hlavičku autorizace.</span><span class="sxs-lookup"><span data-stu-id="3b977-179">In the previous REST API, we can use the access key **AppKey** itself as the authorization header.</span></span> <span data-ttu-id="3b977-180">Protože tyto volání může být zpracována na straně back-end serveru, je bezpečné.</span><span class="sxs-lookup"><span data-stu-id="3b977-180">Because these calls can be handled on the backend server side, it's safe.</span></span>

<span data-ttu-id="3b977-181">Ale po tuto sestavu vložit naše webové stránce, tento druh informací o zabezpečení by být zpracována pomocí jazyka JavaScript \(front-endu).</span><span class="sxs-lookup"><span data-stu-id="3b977-181">But, when we embed the report in our web page, this kind of security information would be handled using JavaScript \(frontend).</span></span> <span data-ttu-id="3b977-182">Pak musí být zabezpečená hodnota hlavičky ověřování.</span><span class="sxs-lookup"><span data-stu-id="3b977-182">Then the authorization header value must be secured.</span></span> <span data-ttu-id="3b977-183">Pokud uživatel se zlými úmysly nebo škodlivý kód je zjištěno naše přístupový klíč, můžou volat jakékoli operace pomocí tohoto klíče.</span><span class="sxs-lookup"><span data-stu-id="3b977-183">If our access key is discovered by a malicious user or malicious code, they can call any operations using this key.</span></span>

<span data-ttu-id="3b977-184">Pokud tuto sestavu vložit naše webové stránce, jsme místo přístupový klíč musíte použít počítaný token **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="3b977-184">When we embed the report in our web page, we must use the computed token instead of access key **AppKey**.</span></span> <span data-ttu-id="3b977-185">Aplikace musí vytvořit OAuth webových tokenů Json \(JWT) který se skládá z deklarací identity a počítaný digitální podpis.</span><span class="sxs-lookup"><span data-stu-id="3b977-185">Our application must create the OAuth Json Web Token \(JWT) which consists of the claims and the computed digital signature.</span></span> <span data-ttu-id="3b977-186">Jak je uvedeno níže, je tento token JWT OAuth tokeny řetězec s kódováním oddělené tečkou.</span><span class="sxs-lookup"><span data-stu-id="3b977-186">As illustrated below, this OAuth JWT is dot-delimited encoded string tokens.</span></span>

![](media/power-bi-embedded-iframe/oauth-jwt.png)

<span data-ttu-id="3b977-187">Nejdřív jsme musíte připravit vstupní hodnotu, která je podepsána později.</span><span class="sxs-lookup"><span data-stu-id="3b977-187">First, we must prepare the input value, which is signed later.</span></span> <span data-ttu-id="3b977-188">Tato hodnota je řetězec ve formátu base64 kódovaná adresou url (rfc4648) následujícím kódu json, a ty jsou odděleny tečkou \(.) znaků.</span><span class="sxs-lookup"><span data-stu-id="3b977-188">This value is the base64 url encoded (rfc4648) string of the following json, and these are delimited by the dot \(.) character.</span></span> <span data-ttu-id="3b977-189">Později vysvětlíme, jak získat id sestavy.</span><span class="sxs-lookup"><span data-stu-id="3b977-189">Later, we'll explain how to get the report id.</span></span>

> [!NOTE]
> <span data-ttu-id="3b977-190">Pokud nám chcete používat zabezpečení na úrovni řádků (RLS) s nástrojem Power BI Embedded, jsme musíte zadat také **uživatelské jméno** a **role** v deklaracích.</span><span class="sxs-lookup"><span data-stu-id="3b977-190">If we want to use Row Level Security (RLS) with Power BI Embedded, we must also specify **username** and **roles** in the claims.</span></span>

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

<span data-ttu-id="3b977-191">Dál musíte vytvořit řetězec s kódováním base64 z HMAC \(podpis) s algoritmem SHA256.</span><span class="sxs-lookup"><span data-stu-id="3b977-191">Next, we must create the base64 encoded string of HMAC \(the signature) with SHA256 algorithm.</span></span> <span data-ttu-id="3b977-192">Tato podepsaný vstupní hodnota je předchozích řetězec.</span><span class="sxs-lookup"><span data-stu-id="3b977-192">This signed input value is the previous string.</span></span>

<span data-ttu-id="3b977-193">Poslední, jsme musíte kombinovat vstupní řetězec na hodnotu a podpis pomocí období \(.) znaků.</span><span class="sxs-lookup"><span data-stu-id="3b977-193">Last, we must combine the input value and signature string using period \(.) character.</span></span> <span data-ttu-id="3b977-194">Dokončené řetězec je tokenu aplikace pro vložení sestavy.</span><span class="sxs-lookup"><span data-stu-id="3b977-194">The completed string is the app token for the report embedding.</span></span> <span data-ttu-id="3b977-195">I v případě, že uživatel se zlými úmysly zjišťováno tokenu aplikace, nemají přístup původní přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="3b977-195">Even if the app token is discovered by a malicious user, they cannot get the original access key.</span></span> <span data-ttu-id="3b977-196">Tento token aplikace rychle vyprší.</span><span class="sxs-lookup"><span data-stu-id="3b977-196">This app token will expire quickly.</span></span>

<span data-ttu-id="3b977-197">Tady je příklad PHP pro tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="3b977-197">Here's a PHP example for these steps:</span></span>

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

// 4. show result (which is the apptoken)
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

## <a name="finally-embed-the-report-into-the-web-page"></a><span data-ttu-id="3b977-198">Nakonec tuto sestavu vložte do webové stránky</span><span class="sxs-lookup"><span data-stu-id="3b977-198">Finally, embed the report into the web page</span></span>

<span data-ttu-id="3b977-199">Pro vkládání naše sestavy, nám musí získat adresu url vložení a sestava **id** pomocí následující rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="3b977-199">For embedding our report, we must get the embed url and report **id** using the following REST API.</span></span>

<span data-ttu-id="3b977-200">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="3b977-200">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="3b977-201">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="3b977-201">**HTTP Response**</span></span>

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

<span data-ttu-id="3b977-202">V našem webovou aplikaci pomocí předchozího tokenu aplikace jsme můžete tuto sestavu vložit.</span><span class="sxs-lookup"><span data-stu-id="3b977-202">We can embed the report in our web app using the previous app token.</span></span>
<span data-ttu-id="3b977-203">Pokud se podíváme na Další ukázkový kód, dřív používaná část je stejný jako předchozí příklad.</span><span class="sxs-lookup"><span data-stu-id="3b977-203">If we look at the next sample code, the former part is the same as the previous example.</span></span> <span data-ttu-id="3b977-204">V pozdější části, tento příklad ukazuje **embedUrl** \(viz předchozí výsledek) v elementu iframe a je publikování tokenu aplikace do iframe.</span><span class="sxs-lookup"><span data-stu-id="3b977-204">In the latter part, this sample shows the **embedUrl** \(see the previous result) in the iframe, and is posting the app token into the iframe.</span></span>

> [!NOTE]
> <span data-ttu-id="3b977-205">Budete muset změnit hodnotu id sestavy na vlastním.</span><span class="sxs-lookup"><span data-stu-id="3b977-205">You'll need to change the report id value to one of your own.</span></span> <span data-ttu-id="3b977-206">Navíc způsobena chybou v našem systému správy obsahu, značku iframe v ukázce kódu je pro čtení oznámena.</span><span class="sxs-lookup"><span data-stu-id="3b977-206">Also, due to a bug in our content management system, the iframe tag in the code sample is read literally.</span></span> <span data-ttu-id="3b977-207">Odeberte vyloučením nejvyššího a nejnižšího text ze značky, pokud je kopírujete a vkládáte ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="3b977-207">Remove the capped text from the tag if you copy and paste this sample code.</span></span>

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

<span data-ttu-id="3b977-208">A tady je naše výsledek:</span><span class="sxs-lookup"><span data-stu-id="3b977-208">And here's our result:</span></span>

![](media/power-bi-embedded-iframe/view-report.png)

<span data-ttu-id="3b977-209">V tomto okamžiku Power BI Embedded pouze zobrazí sestavu v iframe.</span><span class="sxs-lookup"><span data-stu-id="3b977-209">At this time, Power BI Embedded only shows the report in the iframe.</span></span> <span data-ttu-id="3b977-210">Ale dohlížet na [Power BI Blog](https://powerbi.microsoft.com/blog/).</span><span class="sxs-lookup"><span data-stu-id="3b977-210">But, keep an eye on the [Power BI Blog](https://powerbi.microsoft.com/blog/).</span></span> <span data-ttu-id="3b977-211">Budoucí vylepšení použít nové na straně klienta rozhraní API, která bude dejte nám odeslat informace do iframe a také získat informace o.</span><span class="sxs-lookup"><span data-stu-id="3b977-211">Future improvements could use new client side APIs that will let us send information into the iframe as well as get information out.</span></span> <span data-ttu-id="3b977-212">Zajímavé věcem!</span><span class="sxs-lookup"><span data-stu-id="3b977-212">Exciting stuff!</span></span>

## <a name="see-also"></a><span data-ttu-id="3b977-213">Viz také</span><span class="sxs-lookup"><span data-stu-id="3b977-213">See also</span></span>
* [<span data-ttu-id="3b977-214">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="3b977-214">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)

<span data-ttu-id="3b977-215">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="3b977-215">More questions?</span></span> [<span data-ttu-id="3b977-216">Vyzkoušejte komunitu Power BI</span><span class="sxs-lookup"><span data-stu-id="3b977-216">Try the Power BI Community</span></span>](http://community.powerbi.com/)

