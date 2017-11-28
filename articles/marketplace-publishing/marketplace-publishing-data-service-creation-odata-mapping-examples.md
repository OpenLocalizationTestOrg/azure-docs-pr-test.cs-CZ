---
title: "aaaGuide toocreating datové služby pro hello Marketplace | Microsoft Docs"
description: "Podrobné pokyny, jak toocreate, certifikovat a nasadit službu Data pro zakoupit na hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 148f8638-ee80-4100-8d63-5afa4167ca1b
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 8917a43959834d15f70866297f98d24bb83e217f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="examples-of-mapping-an-existing-web-service-tooodata-through-csdls"></a><span data-ttu-id="4e7ef-103">Příklady mapování existující webové služby tooOData prostřednictvím CSDLs</span><span class="sxs-lookup"><span data-stu-id="4e7ef-103">Examples of mapping an existing web service tooOData through CSDLs</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4e7ef-104">**V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.**</span><span class="sxs-lookup"><span data-stu-id="4e7ef-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="4e7ef-105">Pokud máte obchodní aplikace SaaS chcete toopublish na AppSource najdete další informace [zde](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="4e7ef-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="4e7ef-106">Pokud máte IaaS aplikace nebo služba vývojáře by jako toopublish na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="4e7ef-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

## <a name="example-functionimport-for-raw-data-returned-using-post"></a><span data-ttu-id="4e7ef-107">Příklad: FunctionImport pro vrátila pomocí "POST" data "Raw"</span><span class="sxs-lookup"><span data-stu-id="4e7ef-107">Example: FunctionImport for "Raw" data returned using "POST"</span></span>
<span data-ttu-id="4e7ef-108">Použít novou podřízenou položkou toocreate POST nezpracovaná data a vrátí svůj server definované URL(location) nebo tooupdate část hello podřízené na hello server definována adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-108">Use POST Raw data toocreate a new subordinate and return its server defined URL(location) or tooupdate part of hello subordinate at hello server defined URL.</span></span>  <span data-ttu-id="4e7ef-109">Kde podřízená hello je datový proud, tj. nestrukturovaných, např.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-109">Where hello subordinate is a stream, i.e. unstructured, ex.</span></span> <span data-ttu-id="4e7ef-110">textový soubor.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-110">a text file.</span></span>  <span data-ttu-id="4e7ef-111">Pozor POST v není idempotent bez umístění.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-111">Beware POST in not idempotent without a location.</span></span>

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="AddUsageEvent" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Add usage event (data acquisition)</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-delete"></a><span data-ttu-id="4e7ef-112">Příklad: FunctionImport pomocí "Odstranit"</span><span class="sxs-lookup"><span data-stu-id="4e7ef-112">Example: FunctionImport using "DELETE"</span></span>
<span data-ttu-id="4e7ef-113">Použijte tooremove odstranění zadaného identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-113">Use DELETE tooremove a specified URI.</span></span>

        <EntitySet Name="DeleteUsageFileEntitySet" EntityType="MyOffer.DeleteUsageFileEntity" />
        <FunctionImport Name="DeleteUsageFile" EntitySet="DeleteUsageFileEntitySet" ReturnType="Collection(MyOffer.DeleteUsageFileEntity)"  d:AllowedHttpMethods="DELETE" d:EncodeParameterValues="true” d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643" >
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Delete usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="DeleteUsageFileEntity" d:Map="//boolean">
        <Property Name="boolean" Type="String" Nullable="true" d:Map="./boolean" />
        </EntityType>

## <a name="example-functionimport-using-post"></a><span data-ttu-id="4e7ef-114">Příklad: FunctionImport pomocí "POST"</span><span class="sxs-lookup"><span data-stu-id="4e7ef-114">Example: FunctionImport using "POST"</span></span>
<span data-ttu-id="4e7ef-115">Použít novou podřízenou položkou toocreate POST nezpracovaná data a vrátí svůj server definované URL(location) nebo tooupdate část hello podřízené na hello server definována adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-115">Use POST Raw data toocreate a new subordinate and return its server defined URL(location) or tooupdate part of hello subordinate at hello server defined URL.</span></span>  <span data-ttu-id="4e7ef-116">Kde podřízená hello je struktura.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-116">Where hello subordinate is a structure.</span></span> <span data-ttu-id="4e7ef-117">Mějte na paměti, POST není idempotent bez umístění.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-117">Beware POST is not idempotent without a location.</span></span>

        <EntitySet Name="CreateANewModelEntitySet2" EntityType=" MyOffer.CreateANewModelEntity2" />
        <FunctionImport Name="CreateModel" EntitySet="CreateANewModelEntitySet2" ReturnType="Collection(MyOffer.CreateANewModelEntity2)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Create A New Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-put"></a><span data-ttu-id="4e7ef-118">Příklad: FunctionImport pomocí "PUT"</span><span class="sxs-lookup"><span data-stu-id="4e7ef-118">Example: FunctionImport using "PUT"</span></span>
<span data-ttu-id="4e7ef-119">Pomocí PUT toocreate novou podřízenou položkou nebo tooupdate hello celý podřízená na server definované adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-119">Use PUT toocreate a new subordinate or tooupdate hello entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="4e7ef-120">Kde podřízená hello je struktura, PUT je idempotent, takže více výskytů bude mít za následek hello stejné stav, jednofaktorovému</span><span class="sxs-lookup"><span data-stu-id="4e7ef-120">Where hello subordinate is a structure, PUT is idempotent so multiple occurrences will result in hello same state, i.e</span></span> <span data-ttu-id="4e7ef-121">x = 5.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-121">x=5.</span></span>  <span data-ttu-id="4e7ef-122">PUT, musí být použit s hello celého obsahu z hello zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-122">Put should be used with hello full content of hello specified resource.</span></span>

        <EntitySet Name="UpdateAnExistingModelEntitySet" EntityType="MyOffer.UpdateAnExistingModelEntity" />
        <FunctionImport Name="UpdateModel" EntitySet="UpdateAnExistingModelEntitySet" ReturnType="Collection(MyOffer.UpdateAnExistingModelEntity)" d:EncodeParameterValues="true" d:AllowedHttpMethods="PUT" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Update an Existing Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="UpdateAnExistingModelEntity" d:Map="//string">
        <Property Name="string"     Type="String" Nullable="true" d:Map="./string" />
        </EntityType>


## <a name="example-functionimport-for-raw-data-returned-using-put"></a><span data-ttu-id="4e7ef-123">Příklad: FunctionImport pro vrátila pomocí "PUT" data "Raw"</span><span class="sxs-lookup"><span data-stu-id="4e7ef-123">Example: FunctionImport for "Raw" data returned using "PUT"</span></span>
<span data-ttu-id="4e7ef-124">Pomocí PUT nezpracovaná data toocreate novou podřízenou položkou nebo celý podřízený hello tooupdate na adrese URL definován server.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-124">Use PUT Raw data toocreate a new subordinate or tooupdate hello entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="4e7ef-125">Kde podřízená hello je datový proud, tj. nestrukturovaných, např.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-125">Where hello subordinate is a stream, i.e. unstructured, ex.</span></span> <span data-ttu-id="4e7ef-126">textový soubor.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-126">a text file.</span></span>  <span data-ttu-id="4e7ef-127">PUT je idempotent tak více výskytů bude mít za následek hello stejné stav, jednofaktorovému</span><span class="sxs-lookup"><span data-stu-id="4e7ef-127">PUT is idempotent so multiple occurrences will result in hello same state, i.e</span></span> <span data-ttu-id="4e7ef-128">x = 5.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-128">x=5.</span></span>  <span data-ttu-id="4e7ef-129">PUT, musí být použit s hello celého obsahu z hello zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-129">Put should be used with hello full content of hello specified resource.</span></span>

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="CancelBuild” ReturnType="Raw(text/plain)" d:AllowedHttpMethods="PUT" d:EncodeParameterValues="true" d:BaseUri=” http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Cancel Build</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>


## <a name="example-functionimport-for-raw-data-returned-using-get"></a><span data-ttu-id="4e7ef-130">Příklad: FunctionImport pro vrátila pomocí "GET" data "Raw"</span><span class="sxs-lookup"><span data-stu-id="4e7ef-130">Example: FunctionImport for "Raw" data returned using "GET"</span></span>
<span data-ttu-id="4e7ef-131">Použijte získat nezpracovaná data tooreturn podřízená, který nestrukturovaných, tj. text.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-131">Use GET Raw data tooreturn a subordinate that is unstructured, i.e. text.</span></span>

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="GetModelUsageFile" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="GET" d:BaseUri="https://cmla.cloudapp.net/api2/model/builder/build?buildId={buildId}&amp;apiVersion={apiVersion}">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Download A Models Usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Accept" d:Value="application/xml,application/xhtml+xml,text/html;" />
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-for-paging-through-returned-data"></a><span data-ttu-id="4e7ef-132">Příklad: FunctionImport pro "Stránkování" prostřednictvím vrácená data</span><span class="sxs-lookup"><span data-stu-id="4e7ef-132">Example: FunctionImport for "Paging" through returned data</span></span>
<span data-ttu-id="4e7ef-133">Implementace RESTful stránkování prostřednictvím svá data pomocí GET.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-133">Use implement RESTful paging through your data with GET.</span></span>  <span data-ttu-id="4e7ef-134">Výchozí stránkování nastavena too100 řádek na stránce data.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-134">Default paging is set too100 row per page of data.</span></span>

        <EntitySet Name=”CropEntitySet" EntityType="MyOffer.CropEntity" />
        <FunctionImport    Name="GetCropReport" EntitySet="CropEntitySet” ReturnType="Collection(MyOffer.CropEntity)" d:EmitSelfLink="false" d:EncodeParameterValues="true" d:Paging="SkipTake" d:MaxPageSize="100" d:BaseUri="http://api.mydata.org/Crop? report={report}&amp;series={series}&amp;start={$skip}&amp;size=100">
        <Parameter Name="report" Type="Int32" Mode="In" Nullable="false" d:SampleValues="4"  d:enum="1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19"  />
        <Parameter Name="series"    Type="String"    Mode="In" Nullable="false" d:SampleValues="FARM" />
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="text/xml;charset=UTF-8" />
        </d:Headers>
        <d:Namespaces>
        <d:Namespace d:Prefix="diffgr" d:Uri="urn:schemas-microsoft-com:xml-diffgram-v1" />
        </d:Namespaces>
        </FunctionImport>

## <a name="see-also"></a><span data-ttu-id="4e7ef-135">Viz také</span><span class="sxs-lookup"><span data-stu-id="4e7ef-135">See Also</span></span>
* <span data-ttu-id="4e7ef-136">Pokud vás zajímá Principy hello celý proces mapování OData a účel, přečtěte si tento článek [mapování dat služby OData](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definice struktury a pokynů.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-136">If you are interested in understanding hello overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definitions, structures, and instructions.</span></span>
* <span data-ttu-id="4e7ef-137">Pokud vás zajímá učení a pochopení hello konkrétní uzlů a jejich parametrů, přečtěte si tento článek [datové služby OData mapování uzly](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) pro definice a vysvětlení, příklady a kontext případů použití.</span><span class="sxs-lookup"><span data-stu-id="4e7ef-137">If you are interested in learning and understanding hello specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="4e7ef-138">tooreturn toohello předepsané cestu pro publikování dat služby toohello Azure Marketplace, přečtěte si tento článek [Průvodce publikování dat služby](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="4e7ef-138">tooreturn toohello prescribed path for publishing a Data Service toohello Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

