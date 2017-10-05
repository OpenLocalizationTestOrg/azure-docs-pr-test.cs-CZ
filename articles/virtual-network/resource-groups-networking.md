---
title: "Přehled zprostředkovatele prostředků sítě | Microsoft Docs"
description: "Další informace o nové poskytovatele síťových prostředků ve službě Správce prostředků Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 79bf09da-4809-45cb-8d21-705616ef24dc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 2428c707ddeed281fddd1e57bc5574603f0b9b1c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="network-resource-provider"></a><span data-ttu-id="578ef-103">Poskytovatel síťových prostředků</span><span class="sxs-lookup"><span data-stu-id="578ef-103">Network Resource Provider</span></span>
<span data-ttu-id="578ef-104">Podpůrný potřeba v dnešních obchodní úspěch, je možnost vytvářet a spravovat aplikace využívající rozsáhlé sítě způsobem agilní, flexibilní, zabezpečení a opakovatelných.</span><span class="sxs-lookup"><span data-stu-id="578ef-104">An underpinning need in today’s business success, is the ability to build and manage large scale network aware applications in an agile, flexible, secure and repeatable way.</span></span> <span data-ttu-id="578ef-105">Azure Resource Manager umožňuje vytvářet takové aplikace jako jedinou kolekci prostředků do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="578ef-105">Azure Resource Manager enables you to create such applications, as a single collection of resources in resource groups.</span></span> <span data-ttu-id="578ef-106">Tyto prostředky se spravují prostřednictvím různých poskytovatelů prostředků v části správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="578ef-106">Such resources are managed through various resource providers under Resource Manager.</span></span>

<span data-ttu-id="578ef-107">Azure Resource Manager spoléhá na poskytovatele různých prostředků k poskytování přístupu k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="578ef-107">Azure Resource Manager relies on different resource providers to provide access to your resources.</span></span> <span data-ttu-id="578ef-108">Existují tři hlavní prostředků zprostředkovatelé: sítě, úložiště a výpočty.</span><span class="sxs-lookup"><span data-stu-id="578ef-108">There are three main resource providers: Network, Storage and Compute.</span></span> <span data-ttu-id="578ef-109">Tento dokument popisuje vlastnosti a výhody poskytovatele síťových prostředků, včetně:</span><span class="sxs-lookup"><span data-stu-id="578ef-109">This document discusses the characteristics and benefits of the Network Resource Provider, including:</span></span>

* <span data-ttu-id="578ef-110">**Metadata** – budete moct přidat informace k prostředkům pomocí značek.</span><span class="sxs-lookup"><span data-stu-id="578ef-110">**Metadata** – you can add information to resources using tags.</span></span> <span data-ttu-id="578ef-111">Tyto značky lze použít ke sledování využití prostředků mezi skupinami prostředků a předplatná.</span><span class="sxs-lookup"><span data-stu-id="578ef-111">These tags can be used to track resource utilization across resource groups and subscriptions.</span></span>
* <span data-ttu-id="578ef-112">**Větší kontrolu nad síti** – síťové prostředky jsou volně vázány a je mohli řídit podrobnější způsobem.</span><span class="sxs-lookup"><span data-stu-id="578ef-112">**Greater control of your network** - network resources are loosely coupled and you can control them in a more granular fashion.</span></span> <span data-ttu-id="578ef-113">To znamená, že máte větší flexibilitu při správě síťových prostředků.</span><span class="sxs-lookup"><span data-stu-id="578ef-113">This means you have more flexibility in managing the networking resources.</span></span>
* <span data-ttu-id="578ef-114">**Rychlejší konfigurace** -vzhledem k tomu, že jsou volně vázány síťovým prostředkům, můžete vytvořit a orchestraci síťové prostředky paralelně.</span><span class="sxs-lookup"><span data-stu-id="578ef-114">**Faster configuration** - because network resources are loosely coupled, you can create and orchestrate network resources in parallel.</span></span> <span data-ttu-id="578ef-115">To má výrazně snížit čas konfigurace.</span><span class="sxs-lookup"><span data-stu-id="578ef-115">This has drastically reduced configuration time.</span></span>
* <span data-ttu-id="578ef-116">**Řízení přístupu na základě role** -RBAC poskytuje výchozí role, s konkrétní bezpečnostní oboru, kromě toho, že vytvoření vlastních rolí zabezpečení správy.</span><span class="sxs-lookup"><span data-stu-id="578ef-116">**Role Based Access Control** - RBAC provides default roles, with specific security scope, in addition to allowing the creation of custom roles for secure management.</span></span>
* <span data-ttu-id="578ef-117">**Jednodušší správu a nasazení** -je jednodušší nasazení a Správa aplikací, protože můžete můžete vytvořit celý zásobník aplikací jako jedinou kolekci prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="578ef-117">**Easier management and deployment** - it’s easier to deploy and manage applications since you can can create an entire application stack as a single collection of resources in a resource group.</span></span> <span data-ttu-id="578ef-118">A rychlejší chcete nasadit, protože můžete nasadit jednoduše poskytnutím datové části JSON šablony.</span><span class="sxs-lookup"><span data-stu-id="578ef-118">And faster to deploy, since you can deploy by simply providing a template JSON payload.</span></span>
* <span data-ttu-id="578ef-119">**Rychlé přizpůsobení** -deklarativní stylu šablony můžete použít za účelem přizpůsobení opakovatelných a rychlé nasazení.</span><span class="sxs-lookup"><span data-stu-id="578ef-119">**Rapid customization** - you can use declarative-style templates to enable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="578ef-120">**Přizpůsobení opakovatelných** -deklarativní stylu šablony můžete použít za účelem přizpůsobení opakovatelných a rychlé nasazení.</span><span class="sxs-lookup"><span data-stu-id="578ef-120">**Repeatable customization** - you can use declarative-style templates to enable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="578ef-121">**Rozhraní pro správu** -k správě prostředků můžete použít některou z následujících rozhraní:</span><span class="sxs-lookup"><span data-stu-id="578ef-121">**Management interfaces** - you can use any of the following interfaces to manage your resources:</span></span>
  * <span data-ttu-id="578ef-122">REST API založené na</span><span class="sxs-lookup"><span data-stu-id="578ef-122">REST based API</span></span>
  * <span data-ttu-id="578ef-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="578ef-123">PowerShell</span></span>
  * <span data-ttu-id="578ef-124">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="578ef-124">.NET SDK</span></span>
  * <span data-ttu-id="578ef-125">Node.JS SDK</span><span class="sxs-lookup"><span data-stu-id="578ef-125">Node.JS SDK</span></span>
  * <span data-ttu-id="578ef-126">Java SDK</span><span class="sxs-lookup"><span data-stu-id="578ef-126">Java SDK</span></span>
  * <span data-ttu-id="578ef-127">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="578ef-127">Azure CLI</span></span>
  * <span data-ttu-id="578ef-128">Portál Preview</span><span class="sxs-lookup"><span data-stu-id="578ef-128">Preview Portal</span></span>
  * <span data-ttu-id="578ef-129">Jazyk šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="578ef-129">Resource Manager template language</span></span>

## <a name="network-resources"></a><span data-ttu-id="578ef-130">Síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="578ef-130">Network resources</span></span>
<span data-ttu-id="578ef-131">Teď můžete spravovat síťovým prostředkům nezávisle, místo nutnosti je spravovat prostřednictvím jednoho výpočetních prostředků (virtuální počítač).</span><span class="sxs-lookup"><span data-stu-id="578ef-131">You can now manage network resources independently, instead of having them all managed through a single compute resource (a virtual machine).</span></span> <span data-ttu-id="578ef-132">Tím se zajistí vyšší stupeň pružnost a přizpůsobivost v skládání komplexní a velké škálování infrastruktury ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="578ef-132">This ensures a higher degree of flexibility and agility in composing a complex and large scale infrastructure in a resource group.</span></span>

<span data-ttu-id="578ef-133">Koncepční zobrazení nasazení ukázkové zahrnující víceúrovňových aplikací je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="578ef-133">A conceptual view of a sample deployment involving a multi-tiered application is presented below.</span></span> <span data-ttu-id="578ef-134">Každý prostředek, který se zobrazí, jako jsou síťové adaptéry, veřejné IP adresy a virtuálních počítačů, lze spravovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="578ef-134">Each resource you see, such as NICs, public IP addresses, and VMs, can be managed independently.</span></span>

![Model prostředků sítě](./media/resource-groups-networking/Figure2.png)

<span data-ttu-id="578ef-136">Každý prostředek obsahuje společnou sadu vlastností a jejich jednotlivé vlastnost nastavit.</span><span class="sxs-lookup"><span data-stu-id="578ef-136">Every resource contains a common set of properties, and their individual property set.</span></span> <span data-ttu-id="578ef-137">Běžné vlastnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="578ef-137">The common properties are:</span></span>

| <span data-ttu-id="578ef-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="578ef-138">Property</span></span> | <span data-ttu-id="578ef-139">Popis</span><span class="sxs-lookup"><span data-stu-id="578ef-139">Description</span></span> | <span data-ttu-id="578ef-140">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="578ef-140">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="578ef-141">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="578ef-141">**name**</span></span> |<span data-ttu-id="578ef-142">Název prostředku jedinečné.</span><span class="sxs-lookup"><span data-stu-id="578ef-142">Unique resource name.</span></span> <span data-ttu-id="578ef-143">Každý typ prostředku má svou vlastní omezení pro pojmenovávání.</span><span class="sxs-lookup"><span data-stu-id="578ef-143">Each resource type has its own naming restrictions.</span></span> |<span data-ttu-id="578ef-144">NIC01 PIP01, VM01,</span><span class="sxs-lookup"><span data-stu-id="578ef-144">PIP01, VM01, NIC01</span></span> |
| <span data-ttu-id="578ef-145">**location**</span><span class="sxs-lookup"><span data-stu-id="578ef-145">**location**</span></span> |<span data-ttu-id="578ef-146">Oblast Azure, ve kterém se nachází na prostředek</span><span class="sxs-lookup"><span data-stu-id="578ef-146">Azure region in which the resource resides</span></span> |<span data-ttu-id="578ef-147">westus eastus</span><span class="sxs-lookup"><span data-stu-id="578ef-147">westus, eastus</span></span> |
| <span data-ttu-id="578ef-148">**ID**</span><span class="sxs-lookup"><span data-stu-id="578ef-148">**id**</span></span> |<span data-ttu-id="578ef-149">Jedinečný identifikátor URI na základě identifikace</span><span class="sxs-lookup"><span data-stu-id="578ef-149">Unique URI based identification</span></span> |<span data-ttu-id="578ef-150">/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span><span class="sxs-lookup"><span data-stu-id="578ef-150">/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span></span> |

<span data-ttu-id="578ef-151">Zkontrolujte jednotlivé vlastnosti prostředků v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="578ef-151">You can check the individual properties of resources in the sections below.</span></span>

[!INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[!INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[!INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[!INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[!INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[!INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[!INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[!INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[!INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[!INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a><span data-ttu-id="578ef-152">Rozhraní pro správu</span><span class="sxs-lookup"><span data-stu-id="578ef-152">Management interfaces</span></span>
<span data-ttu-id="578ef-153">Můžete spravovat pomocí různých rozhraní prostředků Azure sítě.</span><span class="sxs-lookup"><span data-stu-id="578ef-153">You can manage your Azure networking resources using different interfaces.</span></span> <span data-ttu-id="578ef-154">V tomto dokumentu se zaměříme na kabely o těchto rozhraní: rozhraní API REST a šablony.</span><span class="sxs-lookup"><span data-stu-id="578ef-154">In this document we will focus on tow of those interfaces: REST API, and templates.</span></span>

### <a name="rest-api"></a><span data-ttu-id="578ef-155">REST API</span><span class="sxs-lookup"><span data-stu-id="578ef-155">REST API</span></span>
<span data-ttu-id="578ef-156">Jak už bylo zmíněno dříve, je možné spravovat síťovým prostředkům prostřednictvím různých rozhraní, včetně REST API, sady .NET SDK, Node.JS SDK, sady Java SDK, prostředí PowerShell, rozhraní příkazového řádku, portálu Azure a šablony.</span><span class="sxs-lookup"><span data-stu-id="578ef-156">As mentioned earlier, network resources can be managed via a variety of interfaces, including REST API,.NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, Azure Portal and templates.</span></span>

<span data-ttu-id="578ef-157">Rozhraní Rest API odpovídají specifikaci protokol HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="578ef-157">The Rest API’s conform to the HTTP 1.1 protocol specification.</span></span> <span data-ttu-id="578ef-158">Obecná struktura URI rozhraní API je uveden níže:</span><span class="sxs-lookup"><span data-stu-id="578ef-158">The general URI structure of the API is presented below:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

<span data-ttu-id="578ef-159">A parametrů do složených závorek představují následující prvky:</span><span class="sxs-lookup"><span data-stu-id="578ef-159">And the parameters in braces represent the following elements:</span></span>

* <span data-ttu-id="578ef-160">**id předplatného** -svoje id předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="578ef-160">**subscription-id** - your Azure subscription id.</span></span>
* <span data-ttu-id="578ef-161">**– obor názvů zprostředkovatele prostředků** – obor názvů pro použitý zprostředkovatel.</span><span class="sxs-lookup"><span data-stu-id="578ef-161">**resource-provider-namespace** - namespace for the provider being used.</span></span> <span data-ttu-id="578ef-162">Hodnota pro poskytovatele síťových prostředků je *Microsoft.Network*.</span><span class="sxs-lookup"><span data-stu-id="578ef-162">THe value for the network resource provider is *Microsoft.Network*.</span></span>
* <span data-ttu-id="578ef-163">**Název oblasti** – název oblasti Azure</span><span class="sxs-lookup"><span data-stu-id="578ef-163">**region-name** - the Azure region name</span></span>

<span data-ttu-id="578ef-164">Následující metody HTTP jsou podporovány při volání rozhraní REST API:</span><span class="sxs-lookup"><span data-stu-id="578ef-164">The following HTTP methods are supported when making calls to the REST API:</span></span>

* <span data-ttu-id="578ef-165">**UVEĎTE** – použité k vytvoření prostředku daného typu, upravte vlastnosti prostředku nebo změnit přidružení mezi prostředky.</span><span class="sxs-lookup"><span data-stu-id="578ef-165">**PUT** - used to create a resource of a given type, modify a resource property or change an association between resources.</span></span>
* <span data-ttu-id="578ef-166">**ZÍSKAT** – používané k načtení informací pro zřízené prostředek.</span><span class="sxs-lookup"><span data-stu-id="578ef-166">**GET** - used to retrieve information for a provisioned resource.</span></span>
* <span data-ttu-id="578ef-167">**Odstranit** – používané k odstranění existujícího prostředku.</span><span class="sxs-lookup"><span data-stu-id="578ef-167">**DELETE** - used to delete an existing resource.</span></span>

<span data-ttu-id="578ef-168">Požadavků a odpovědí požadavkům formátu datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="578ef-168">Both the request and response conform to a JSON payload format.</span></span> <span data-ttu-id="578ef-169">Další podrobnosti najdete v tématu [rozhraní API pro správu prostředků Azure](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span><span class="sxs-lookup"><span data-stu-id="578ef-169">For more details, see [Azure Resource Management APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span></span>

### <a name="resource-manager-template-language"></a><span data-ttu-id="578ef-170">Jazyk šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="578ef-170">Resource Manager template language</span></span>
<span data-ttu-id="578ef-171">Kromě správy prostředků imperativní (přes rozhraní API nebo sady SDK), můžete taky deklarativní programovací styl vytvářet a spravovat prostředky sítě pomocí jazyka šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="578ef-171">In addition to managing resources imperatively (via APIs or SDK), you can also use a declarative programming style to build and manage network resources by using the Resource Manager Template Language.</span></span>

<span data-ttu-id="578ef-172">Ukázka reprezentace šablony najdete níže –</span><span class="sxs-lookup"><span data-stu-id="578ef-172">A sample representation of a template is provided below –</span></span>

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

<span data-ttu-id="578ef-173">Šablona je primárně popis JSON prostředky a instance hodnoty vložit prostřednictvím parametrů.</span><span class="sxs-lookup"><span data-stu-id="578ef-173">The template is primarily a JSON description of the resources and the instance values injected via parameters.</span></span> <span data-ttu-id="578ef-174">Následující příklad slouží k vytvoření virtuální sítě s 2 podsítě.</span><span class="sxs-lookup"><span data-stu-id="578ef-174">The example below can be used to create a virtual network with 2 subnets.</span></span>

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

<span data-ttu-id="578ef-175">Máte možnost ručně poskytnout hodnoty parametrů při použití šablony, nebo můžete použít soubor s parametry.</span><span class="sxs-lookup"><span data-stu-id="578ef-175">You have the option of providing the parameter values manually when using a template, or you can use a parameter file.</span></span> <span data-ttu-id="578ef-176">Následující příklad ukazuje sadu možných hodnot parametrů pro použití s výše uvedené šablony:</span><span class="sxs-lookup"><span data-stu-id="578ef-176">The example below shows a possible set of parameter values to be used with the template above:</span></span>

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


<span data-ttu-id="578ef-177">Hlavní výhody použití šablony jsou:</span><span class="sxs-lookup"><span data-stu-id="578ef-177">The main advantages of using templates are:</span></span>

* <span data-ttu-id="578ef-178">Můžete vytvořit komplexní infrastruktury ve skupině prostředků v deklarativní stylu.</span><span class="sxs-lookup"><span data-stu-id="578ef-178">You can build a complex infrastructure in a resource group in a declarative style.</span></span> <span data-ttu-id="578ef-179">Pomocí Správce prostředků se zpracovává orchestration vytváření prostředků, včetně správy závislostí.</span><span class="sxs-lookup"><span data-stu-id="578ef-179">The orchestration of creating the resources, including dependency management, is handled by Resource Manager.</span></span>
* <span data-ttu-id="578ef-180">Infrastruktura vytvořením opakovatelných způsobem v různých oblastech a v oblasti jednoduše změnou parametry.</span><span class="sxs-lookup"><span data-stu-id="578ef-180">The infrastructure can be created in a repeatable way across various regions and within a region by simply changing parameters.</span></span>
* <span data-ttu-id="578ef-181">Styl deklarativní vede ke kratší dobu realizace v vytváření šablony a nasazení infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="578ef-181">The declarative style leads to shorter lead time in building the templates and rolling out the infrastructure.</span></span>

<span data-ttu-id="578ef-182">Ukázka šablony, najdete v části [šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="578ef-182">For sample templates, see [Azure quickstart templates](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="578ef-183">Další informace o jazyce šablony Resource Manageru najdete v tématu [jazyk šablony Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="578ef-183">For more information on the Resource Manager Template Language, see [Azure Resource Manager Template Language](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="578ef-184">Výše uvedené ukázkové šablona používá virtuální síť a podsíť prostředky.</span><span class="sxs-lookup"><span data-stu-id="578ef-184">The sample template above uses the virtual network and subnet resources.</span></span> <span data-ttu-id="578ef-185">Existují jiným síťovým prostředkům, které můžete použít jak je uvedeno dále:</span><span class="sxs-lookup"><span data-stu-id="578ef-185">There are other network resources you can use as listed below:</span></span>

### <a name="using-a-template"></a><span data-ttu-id="578ef-186">Pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="578ef-186">Using a template</span></span>
<span data-ttu-id="578ef-187">Služby můžete nasadit do Azure ze šablony pomocí prostředí PowerShell, AzureCLI, nebo provedením klikněte na nasadit z Githubu.</span><span class="sxs-lookup"><span data-stu-id="578ef-187">You can deploy services to Azure from a template by using PowerShell, AzureCLI, or by performing a click to deploy from GitHub.</span></span> <span data-ttu-id="578ef-188">Pokud chcete nasadit služby ze šablony v Githubu, spusťte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="578ef-188">To deploy services from a template in GitHub, execute the following steps:</span></span>

1. <span data-ttu-id="578ef-189">Otevřete soubor template3 z Githubu.</span><span class="sxs-lookup"><span data-stu-id="578ef-189">Open the template3 file from GitHub.</span></span> <span data-ttu-id="578ef-190">Jako příklad, otevřete [virtuální síť se dvěma podsítěmi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="578ef-190">As an example, open [Virtual network with two subnets](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span></span>
2. <span data-ttu-id="578ef-191">Klikněte na **nasadit do Azure**a pak Přihlaste se k portálu Azure pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="578ef-191">Click on **Deploy to Azure**, and then sign in on to the Azure portal with your credentials.</span></span>
3. <span data-ttu-id="578ef-192">Zkontrolujte šablonu a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="578ef-192">Verify the template, and then click **Save**.</span></span>
4. <span data-ttu-id="578ef-193">Klikněte na tlačítko **upravit parametry** a vyberte umístění, jako například *západní USA*, pro virtuální sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="578ef-193">Click **Edit parameters** and select a location, such as *West US*, for the vnet and subnets.</span></span>
5. <span data-ttu-id="578ef-194">V případě potřeby změnit **ADDRESSPREFIX** a **SUBNETPREFIX** parametry a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="578ef-194">If necessary, change the **ADDRESSPREFIX** and **SUBNETPREFIX** parameters, and then click **OK**.</span></span>
6. <span data-ttu-id="578ef-195">Klikněte na tlačítko **vyberte skupinu prostředků** a potom klikněte na skupinu prostředků, které chcete přidat virtuální sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="578ef-195">Click **Select a resource group** and then click on the resource group you want to add the vnet and subnets to.</span></span> <span data-ttu-id="578ef-196">Alternativně můžete vytvořit novou skupinu prostředků kliknutím **nebo vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="578ef-196">Alternatively, you can create a new resource group by clicking **Or create new**.</span></span>
7. <span data-ttu-id="578ef-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="578ef-197">Click **Create**.</span></span> <span data-ttu-id="578ef-198">Všimněte si dlaždice zobrazení **nasazení zřizování šablony**.</span><span class="sxs-lookup"><span data-stu-id="578ef-198">Notice the tile displaying **Provisioning Template deployment**.</span></span> <span data-ttu-id="578ef-199">Po nasazení se provádí, zobrazí podobnou obrazovce níže.</span><span class="sxs-lookup"><span data-stu-id="578ef-199">Once the deployment is done, you will see a screen similar to one below.</span></span>

![Ukázka nasazení šablony](./media/resource-groups-networking/Figure6.png)

## <a name="next-steps"></a><span data-ttu-id="578ef-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="578ef-201">Next steps</span></span>
[<span data-ttu-id="578ef-202">Jazyk šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="578ef-202">Azure Resource Manager Template Language</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)

[<span data-ttu-id="578ef-203">Azure sítě – běžně používané šablony</span><span class="sxs-lookup"><span data-stu-id="578ef-203">Azure Networking – commonly used templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

[<span data-ttu-id="578ef-204">Azure Resource Manager oproti nasazení classic</span><span class="sxs-lookup"><span data-stu-id="578ef-204">Azure Resource Manager vs. classic deployment</span></span>](../azure-resource-manager/resource-manager-deployment-model.md)

[<span data-ttu-id="578ef-205">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="578ef-205">Azure Resource Manager Overview</span></span>](../azure-resource-manager/resource-group-overview.md)

