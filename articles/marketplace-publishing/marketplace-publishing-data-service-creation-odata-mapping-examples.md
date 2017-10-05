---
title: "Příručka o vytváření datové služby pro Marketplace | Microsoft Docs"
description: "Podrobné pokyny o tom, jak vytvořit, certifikovat a datové služby pro nasazení zakoupit na webu Azure Marketplace."
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
ms.openlocfilehash: 2ab624941fc385f14b62bb5d743927f157955845
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="examples-of-mapping-an-existing-web-service-to-odata-through-csdls"></a><span data-ttu-id="fd032-103">Příklady mapování existující webovou službu na OData prostřednictvím CSDLs</span><span class="sxs-lookup"><span data-stu-id="fd032-103">Examples of mapping an existing web service to OData through CSDLs</span></span>
> [!IMPORTANT]
> <span data-ttu-id="fd032-104">**V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.**</span><span class="sxs-lookup"><span data-stu-id="fd032-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="fd032-105">Pokud máte SaaS obchodní aplikace, který chcete publikovat na AppSource můžete najít další informace [zde](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="fd032-105">If you have a SaaS business application you would like to publish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="fd032-106">Pokud máte IaaS aplikace nebo služby vývojáře, které chcete publikovat na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="fd032-106">If you have an IaaS applications or developer service you would like to publish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

## <a name="example-functionimport-for-raw-data-returned-using-post"></a><span data-ttu-id="fd032-107">Příklad: FunctionImport pro vrátila pomocí "POST" data "Raw"</span><span class="sxs-lookup"><span data-stu-id="fd032-107">Example: FunctionImport for "Raw" data returned using "POST"</span></span>
<span data-ttu-id="fd032-108">Vytvořit novou podřízenou položkou a vrátí jeho server definován URL(location) nebo pro část podřízená na serveru aktualizovat definovali adresu URL pomocí POST nezpracovaná data.</span><span class="sxs-lookup"><span data-stu-id="fd032-108">Use POST Raw data to create a new subordinate and return its server defined URL(location) or to update part of the subordinate at the server defined URL.</span></span>  <span data-ttu-id="fd032-109">Kde podřízená je datový proud, tj.</span><span class="sxs-lookup"><span data-stu-id="fd032-109">Where the subordinate is a stream, i.e.</span></span> <span data-ttu-id="fd032-110">nestrukturovaných, např.</span><span class="sxs-lookup"><span data-stu-id="fd032-110">unstructured, ex.</span></span> <span data-ttu-id="fd032-111">textový soubor.</span><span class="sxs-lookup"><span data-stu-id="fd032-111">a text file.</span></span>  <span data-ttu-id="fd032-112">Pozor POST v není idempotent bez umístění.</span><span class="sxs-lookup"><span data-stu-id="fd032-112">Beware POST in not idempotent without a location.</span></span>

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

## <a name="example-functionimport-using-delete"></a><span data-ttu-id="fd032-113">Příklad: FunctionImport pomocí "Odstranit"</span><span class="sxs-lookup"><span data-stu-id="fd032-113">Example: FunctionImport using "DELETE"</span></span>
<span data-ttu-id="fd032-114">ODSTRANĚNÍ použijte k odebrání zadaného identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="fd032-114">Use DELETE to remove a specified URI.</span></span>

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

## <a name="example-functionimport-using-post"></a><span data-ttu-id="fd032-115">Příklad: FunctionImport pomocí "POST"</span><span class="sxs-lookup"><span data-stu-id="fd032-115">Example: FunctionImport using "POST"</span></span>
<span data-ttu-id="fd032-116">Vytvořit novou podřízenou položkou a vrátí jeho server definován URL(location) nebo pro část podřízená na serveru aktualizovat definovali adresu URL pomocí POST nezpracovaná data.</span><span class="sxs-lookup"><span data-stu-id="fd032-116">Use POST Raw data to create a new subordinate and return its server defined URL(location) or to update part of the subordinate at the server defined URL.</span></span>  <span data-ttu-id="fd032-117">Kde podřízená je struktura.</span><span class="sxs-lookup"><span data-stu-id="fd032-117">Where the subordinate is a structure.</span></span> <span data-ttu-id="fd032-118">Mějte na paměti, POST není idempotent bez umístění.</span><span class="sxs-lookup"><span data-stu-id="fd032-118">Beware POST is not idempotent without a location.</span></span>

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

## <a name="example-functionimport-using-put"></a><span data-ttu-id="fd032-119">Příklad: FunctionImport pomocí "PUT"</span><span class="sxs-lookup"><span data-stu-id="fd032-119">Example: FunctionImport using "PUT"</span></span>
<span data-ttu-id="fd032-120">Pomocí PUT, chcete-li vytvořit novou podřízenou položkou nebo aktualizovat celý podřízená na adrese URL definován server.</span><span class="sxs-lookup"><span data-stu-id="fd032-120">Use PUT to create a new subordinate or to update the entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="fd032-121">Tam, kde podřízená je struktura, PUT je idempotent, takže více výskytů bude mít za následek stejného stavu, jednofaktorovému</span><span class="sxs-lookup"><span data-stu-id="fd032-121">Where the subordinate is a structure, PUT is idempotent so multiple occurrences will result in the same state, i.e</span></span> <span data-ttu-id="fd032-122">x = 5.</span><span class="sxs-lookup"><span data-stu-id="fd032-122">x=5.</span></span>  <span data-ttu-id="fd032-123">PUT, musí být použit s celý obsah zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="fd032-123">Put should be used with the full content of the specified resource.</span></span>

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


## <a name="example-functionimport-for-raw-data-returned-using-put"></a><span data-ttu-id="fd032-124">Příklad: FunctionImport pro vrátila pomocí "PUT" data "Raw"</span><span class="sxs-lookup"><span data-stu-id="fd032-124">Example: FunctionImport for "Raw" data returned using "PUT"</span></span>
<span data-ttu-id="fd032-125">Pomocí PUT nezpracovaná data, chcete-li vytvořit novou podřízenou položkou nebo aktualizovat celý podřízená na adrese URL definován server.</span><span class="sxs-lookup"><span data-stu-id="fd032-125">Use PUT Raw data to create a new subordinate or to update the entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="fd032-126">Kde podřízená je datový proud, tj.</span><span class="sxs-lookup"><span data-stu-id="fd032-126">Where the subordinate is a stream, i.e.</span></span> <span data-ttu-id="fd032-127">nestrukturovaných, např.</span><span class="sxs-lookup"><span data-stu-id="fd032-127">unstructured, ex.</span></span> <span data-ttu-id="fd032-128">textový soubor.</span><span class="sxs-lookup"><span data-stu-id="fd032-128">a text file.</span></span>  <span data-ttu-id="fd032-129">PUT je idempotent tak více výskytů bude mít za následek stejného stavu, jednofaktorovému</span><span class="sxs-lookup"><span data-stu-id="fd032-129">PUT is idempotent so multiple occurrences will result in the same state, i.e</span></span> <span data-ttu-id="fd032-130">x = 5.</span><span class="sxs-lookup"><span data-stu-id="fd032-130">x=5.</span></span>  <span data-ttu-id="fd032-131">PUT, musí být použit s celý obsah zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="fd032-131">Put should be used with the full content of the specified resource.</span></span>

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


## <a name="example-functionimport-for-raw-data-returned-using-get"></a><span data-ttu-id="fd032-132">Příklad: FunctionImport pro vrátila pomocí "GET" data "Raw"</span><span class="sxs-lookup"><span data-stu-id="fd032-132">Example: FunctionImport for "Raw" data returned using "GET"</span></span>
<span data-ttu-id="fd032-133">Použijte získat nezpracovaná data vrátit podřízená, který nestrukturovaných, tj. text.</span><span class="sxs-lookup"><span data-stu-id="fd032-133">Use GET Raw data to return a subordinate that is unstructured, i.e. text.</span></span>

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

## <a name="example-functionimport-for-paging-through-returned-data"></a><span data-ttu-id="fd032-134">Příklad: FunctionImport pro "Stránkování" prostřednictvím vrácená data</span><span class="sxs-lookup"><span data-stu-id="fd032-134">Example: FunctionImport for "Paging" through returned data</span></span>
<span data-ttu-id="fd032-135">Implementace RESTful stránkování prostřednictvím svá data pomocí GET.</span><span class="sxs-lookup"><span data-stu-id="fd032-135">Use implement RESTful paging through your data with GET.</span></span>  <span data-ttu-id="fd032-136">Výchozí stránkování nastavena na 100 řádek na stránce data.</span><span class="sxs-lookup"><span data-stu-id="fd032-136">Default paging is set to 100 row per page of data.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="fd032-137">Viz také</span><span class="sxs-lookup"><span data-stu-id="fd032-137">See Also</span></span>
* <span data-ttu-id="fd032-138">Pokud vás zajímá porozumět celkový proces mapování OData a účel, přečtěte si tento článek [mapování dat služby OData](marketplace-publishing-data-service-creation-odata-mapping.md) ke kontrole definice struktury a pokynů.</span><span class="sxs-lookup"><span data-stu-id="fd032-138">If you are interested in understanding the overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) to review definitions, structures, and instructions.</span></span>
* <span data-ttu-id="fd032-139">Pokud vás zajímá učení a seznámit se s konkrétním uzlům a jejich parametrů, přečtěte si tento článek [datové služby OData mapování uzly](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) pro definice a vysvětlení, příklady a kontext případů použití.</span><span class="sxs-lookup"><span data-stu-id="fd032-139">If you are interested in learning and understanding the specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="fd032-140">Pokud chcete vrátit do předepsaných cestu pro publikování datové služby v Azure Marketplace, přečtěte si tento článek [Průvodce publikování dat služby](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="fd032-140">To return to the prescribed path for publishing a Data Service to the Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

