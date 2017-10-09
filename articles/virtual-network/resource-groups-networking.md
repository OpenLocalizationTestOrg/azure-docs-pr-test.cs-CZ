---
title: "aaaNetwork přehled zprostředkovatele prostředků | Microsoft Docs"
description: "Další informace o hello nového poskytovatele síťových prostředků ve službě Správce prostředků Azure"
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
ms.openlocfilehash: 81b8f51fe8ee180d8f7885c6e04eb953904d7be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-resource-provider"></a><span data-ttu-id="2390b-103">Poskytovatel síťových prostředků</span><span class="sxs-lookup"><span data-stu-id="2390b-103">Network Resource Provider</span></span>
<span data-ttu-id="2390b-104">Podpůrný potřeba v dnešních obchodní úspěch, je hello možnost toobuild a spravovat aplikace využívající rozsáhlé sítě agilní, flexibilní, zabezpečení a opakovatelných způsobem.</span><span class="sxs-lookup"><span data-stu-id="2390b-104">An underpinning need in today’s business success, is hello ability toobuild and manage large scale network aware applications in an agile, flexible, secure and repeatable way.</span></span> <span data-ttu-id="2390b-105">Azure Resource Manager umožňuje toocreate můžete takové aplikace jako jedinou kolekci prostředků do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2390b-105">Azure Resource Manager enables you toocreate such applications, as a single collection of resources in resource groups.</span></span> <span data-ttu-id="2390b-106">Tyto prostředky se spravují prostřednictvím různých poskytovatelů prostředků v části správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="2390b-106">Such resources are managed through various resource providers under Resource Manager.</span></span>

<span data-ttu-id="2390b-107">Azure Resource Manager závisí na různých prostředků zprostředkovatelé tooprovide přístup tooyour prostředky.</span><span class="sxs-lookup"><span data-stu-id="2390b-107">Azure Resource Manager relies on different resource providers tooprovide access tooyour resources.</span></span> <span data-ttu-id="2390b-108">Existují tři hlavní prostředků zprostředkovatelé: sítě, úložiště a výpočty.</span><span class="sxs-lookup"><span data-stu-id="2390b-108">There are three main resource providers: Network, Storage and Compute.</span></span> <span data-ttu-id="2390b-109">Tento dokument popisuje hello charakteristiky a výhod hello poskytovatele síťových prostředků, včetně:</span><span class="sxs-lookup"><span data-stu-id="2390b-109">This document discusses hello characteristics and benefits of hello Network Resource Provider, including:</span></span>

* <span data-ttu-id="2390b-110">**Metadata** – můžete přidat informace tooresources pomocí značek.</span><span class="sxs-lookup"><span data-stu-id="2390b-110">**Metadata** – you can add information tooresources using tags.</span></span> <span data-ttu-id="2390b-111">Tyto značky mohou být použité tootrack využití prostředků v rámci skupiny prostředků a předplatná.</span><span class="sxs-lookup"><span data-stu-id="2390b-111">These tags can be used tootrack resource utilization across resource groups and subscriptions.</span></span>
* <span data-ttu-id="2390b-112">**Větší kontrolu nad síti** – síťové prostředky jsou volně vázány a je mohli řídit podrobnější způsobem.</span><span class="sxs-lookup"><span data-stu-id="2390b-112">**Greater control of your network** - network resources are loosely coupled and you can control them in a more granular fashion.</span></span> <span data-ttu-id="2390b-113">To znamená, že máte větší flexibilitu při správě síťových prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="2390b-113">This means you have more flexibility in managing hello networking resources.</span></span>
* <span data-ttu-id="2390b-114">**Rychlejší konfigurace** -vzhledem k tomu, že jsou volně vázány síťovým prostředkům, můžete vytvořit a orchestraci síťové prostředky paralelně.</span><span class="sxs-lookup"><span data-stu-id="2390b-114">**Faster configuration** - because network resources are loosely coupled, you can create and orchestrate network resources in parallel.</span></span> <span data-ttu-id="2390b-115">To má výrazně snížit čas konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2390b-115">This has drastically reduced configuration time.</span></span>
* <span data-ttu-id="2390b-116">**Řízení přístupu na základě role** -RBAC poskytuje výchozí role, s konkrétní bezpečnostní obor v přidání tooallowing hello vytvoření vlastních rolí zabezpečení správy.</span><span class="sxs-lookup"><span data-stu-id="2390b-116">**Role Based Access Control** - RBAC provides default roles, with specific security scope, in addition tooallowing hello creation of custom roles for secure management.</span></span>
* <span data-ttu-id="2390b-117">**Jednodušší správu a nasazení** -je snazší toodeploy a spravovat aplikace, protože můžete můžete vytvořit celý zásobník aplikací jako jedinou kolekci prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="2390b-117">**Easier management and deployment** - it’s easier toodeploy and manage applications since you can can create an entire application stack as a single collection of resources in a resource group.</span></span> <span data-ttu-id="2390b-118">A rychlejší toodeploy, protože můžete nasadit jednoduše poskytnutím datové části JSON šablony.</span><span class="sxs-lookup"><span data-stu-id="2390b-118">And faster toodeploy, since you can deploy by simply providing a template JSON payload.</span></span>
* <span data-ttu-id="2390b-119">**Rychlé přizpůsobení** – můžete použít vlastní opakovatelných a rychlé tooenable deklarativní stylu šablony nasazení.</span><span class="sxs-lookup"><span data-stu-id="2390b-119">**Rapid customization** - you can use declarative-style templates tooenable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="2390b-120">**Přizpůsobení opakovatelných** – můžete použít vlastní opakovatelných a rychlé tooenable deklarativní stylu šablony nasazení.</span><span class="sxs-lookup"><span data-stu-id="2390b-120">**Repeatable customization** - you can use declarative-style templates tooenable repeatable and rapid customization of deployments.</span></span>
* <span data-ttu-id="2390b-121">**Rozhraní pro správu** -pomocí některé z následujících rozhraní toomanage hello vaše prostředky:</span><span class="sxs-lookup"><span data-stu-id="2390b-121">**Management interfaces** - you can use any of hello following interfaces toomanage your resources:</span></span>
  * <span data-ttu-id="2390b-122">REST API založené na</span><span class="sxs-lookup"><span data-stu-id="2390b-122">REST based API</span></span>
  * <span data-ttu-id="2390b-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2390b-123">PowerShell</span></span>
  * <span data-ttu-id="2390b-124">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="2390b-124">.NET SDK</span></span>
  * <span data-ttu-id="2390b-125">Node.JS SDK</span><span class="sxs-lookup"><span data-stu-id="2390b-125">Node.JS SDK</span></span>
  * <span data-ttu-id="2390b-126">Java SDK</span><span class="sxs-lookup"><span data-stu-id="2390b-126">Java SDK</span></span>
  * <span data-ttu-id="2390b-127">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2390b-127">Azure CLI</span></span>
  * <span data-ttu-id="2390b-128">Portál Preview</span><span class="sxs-lookup"><span data-stu-id="2390b-128">Preview Portal</span></span>
  * <span data-ttu-id="2390b-129">Jazyk šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2390b-129">Resource Manager template language</span></span>

## <a name="network-resources"></a><span data-ttu-id="2390b-130">Síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="2390b-130">Network resources</span></span>
<span data-ttu-id="2390b-131">Teď můžete spravovat síťovým prostředkům nezávisle, místo nutnosti je spravovat prostřednictvím jednoho výpočetních prostředků (virtuální počítač).</span><span class="sxs-lookup"><span data-stu-id="2390b-131">You can now manage network resources independently, instead of having them all managed through a single compute resource (a virtual machine).</span></span> <span data-ttu-id="2390b-132">Tím se zajistí vyšší stupeň pružnost a přizpůsobivost v skládání komplexní a velké škálování infrastruktury ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="2390b-132">This ensures a higher degree of flexibility and agility in composing a complex and large scale infrastructure in a resource group.</span></span>

<span data-ttu-id="2390b-133">Koncepční zobrazení nasazení ukázkové zahrnující víceúrovňových aplikací je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="2390b-133">A conceptual view of a sample deployment involving a multi-tiered application is presented below.</span></span> <span data-ttu-id="2390b-134">Každý prostředek, který se zobrazí, jako jsou síťové adaptéry, veřejné IP adresy a virtuálních počítačů, lze spravovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="2390b-134">Each resource you see, such as NICs, public IP addresses, and VMs, can be managed independently.</span></span>

![Model prostředků sítě](./media/resource-groups-networking/Figure2.png)

<span data-ttu-id="2390b-136">Každý prostředek obsahuje společnou sadu vlastností a jejich jednotlivé vlastnost nastavit.</span><span class="sxs-lookup"><span data-stu-id="2390b-136">Every resource contains a common set of properties, and their individual property set.</span></span> <span data-ttu-id="2390b-137">běžné vlastnosti Hello jsou:</span><span class="sxs-lookup"><span data-stu-id="2390b-137">hello common properties are:</span></span>

| <span data-ttu-id="2390b-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2390b-138">Property</span></span> | <span data-ttu-id="2390b-139">Popis</span><span class="sxs-lookup"><span data-stu-id="2390b-139">Description</span></span> | <span data-ttu-id="2390b-140">Ukázkové hodnoty</span><span class="sxs-lookup"><span data-stu-id="2390b-140">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2390b-141">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="2390b-141">**name**</span></span> |<span data-ttu-id="2390b-142">Název prostředku jedinečné.</span><span class="sxs-lookup"><span data-stu-id="2390b-142">Unique resource name.</span></span> <span data-ttu-id="2390b-143">Každý typ prostředku má svou vlastní omezení pro pojmenovávání.</span><span class="sxs-lookup"><span data-stu-id="2390b-143">Each resource type has its own naming restrictions.</span></span> |<span data-ttu-id="2390b-144">NIC01 PIP01, VM01,</span><span class="sxs-lookup"><span data-stu-id="2390b-144">PIP01, VM01, NIC01</span></span> |
| <span data-ttu-id="2390b-145">**location**</span><span class="sxs-lookup"><span data-stu-id="2390b-145">**location**</span></span> |<span data-ttu-id="2390b-146">Oblast Azure, ve které hello nachází prostředků</span><span class="sxs-lookup"><span data-stu-id="2390b-146">Azure region in which hello resource resides</span></span> |<span data-ttu-id="2390b-147">westus eastus</span><span class="sxs-lookup"><span data-stu-id="2390b-147">westus, eastus</span></span> |
| <span data-ttu-id="2390b-148">**ID**</span><span class="sxs-lookup"><span data-stu-id="2390b-148">**id**</span></span> |<span data-ttu-id="2390b-149">Jedinečný identifikátor URI na základě identifikace</span><span class="sxs-lookup"><span data-stu-id="2390b-149">Unique URI based identification</span></span> |<span data-ttu-id="2390b-150">/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span><span class="sxs-lookup"><span data-stu-id="2390b-150">/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP</span></span> |

<span data-ttu-id="2390b-151">Můžete zkontrolovat hello jednotlivé vlastnosti prostředků v následujících částech hello.</span><span class="sxs-lookup"><span data-stu-id="2390b-151">You can check hello individual properties of resources in hello sections below.</span></span>

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

## <a name="management-interfaces"></a><span data-ttu-id="2390b-152">Rozhraní pro správu</span><span class="sxs-lookup"><span data-stu-id="2390b-152">Management interfaces</span></span>
<span data-ttu-id="2390b-153">Můžete spravovat pomocí různých rozhraní prostředků Azure sítě.</span><span class="sxs-lookup"><span data-stu-id="2390b-153">You can manage your Azure networking resources using different interfaces.</span></span> <span data-ttu-id="2390b-154">V tomto dokumentu se zaměříme na kabely o těchto rozhraní: rozhraní API REST a šablony.</span><span class="sxs-lookup"><span data-stu-id="2390b-154">In this document we will focus on tow of those interfaces: REST API, and templates.</span></span>

### <a name="rest-api"></a><span data-ttu-id="2390b-155">REST API</span><span class="sxs-lookup"><span data-stu-id="2390b-155">REST API</span></span>
<span data-ttu-id="2390b-156">Jak už bylo zmíněno dříve, je možné spravovat síťovým prostředkům prostřednictvím různých rozhraní, včetně REST API, sady .NET SDK, Node.JS SDK, sady Java SDK, prostředí PowerShell, rozhraní příkazového řádku, portálu Azure a šablony.</span><span class="sxs-lookup"><span data-stu-id="2390b-156">As mentioned earlier, network resources can be managed via a variety of interfaces, including REST API,.NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, Azure Portal and templates.</span></span>

<span data-ttu-id="2390b-157">rozhraní Rest API Hello požadavkům toohello specifikace protokolu HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="2390b-157">hello Rest API’s conform toohello HTTP 1.1 protocol specification.</span></span> <span data-ttu-id="2390b-158">Hello obecné URI struktura hello rozhraní API je uveden níže:</span><span class="sxs-lookup"><span data-stu-id="2390b-158">hello general URI structure of hello API is presented below:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

<span data-ttu-id="2390b-159">A parametry hello do složených závorek představují hello následující prvky:</span><span class="sxs-lookup"><span data-stu-id="2390b-159">And hello parameters in braces represent hello following elements:</span></span>

* <span data-ttu-id="2390b-160">**id předplatného** -svoje id předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="2390b-160">**subscription-id** - your Azure subscription id.</span></span>
* <span data-ttu-id="2390b-161">**– obor názvů zprostředkovatele prostředků** – obor názvů pro používaný zprostředkovatel hello.</span><span class="sxs-lookup"><span data-stu-id="2390b-161">**resource-provider-namespace** - namespace for hello provider being used.</span></span> <span data-ttu-id="2390b-162">Hodnota Hello poskytovatele síťových prostředků hello je *Microsoft.Network*.</span><span class="sxs-lookup"><span data-stu-id="2390b-162">hello value for hello network resource provider is *Microsoft.Network*.</span></span>
* <span data-ttu-id="2390b-163">**Název oblasti** – název oblasti Azure hello</span><span class="sxs-lookup"><span data-stu-id="2390b-163">**region-name** - hello Azure region name</span></span>

<span data-ttu-id="2390b-164">Hello následující metody HTTP jsou podporovány při provádění toohello volání rozhraní REST API:</span><span class="sxs-lookup"><span data-stu-id="2390b-164">hello following HTTP methods are supported when making calls toohello REST API:</span></span>

* <span data-ttu-id="2390b-165">**UVEĎTE** – použité toocreate prostředek daného typu, upravte vlastnosti prostředku nebo změnit přidružení mezi prostředky.</span><span class="sxs-lookup"><span data-stu-id="2390b-165">**PUT** - used toocreate a resource of a given type, modify a resource property or change an association between resources.</span></span>
* <span data-ttu-id="2390b-166">**ZÍSKAT** – používá informace tooretrieve zřízené prostředku.</span><span class="sxs-lookup"><span data-stu-id="2390b-166">**GET** - used tooretrieve information for a provisioned resource.</span></span>
* <span data-ttu-id="2390b-167">**Odstranit** -použít toodelete existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="2390b-167">**DELETE** - used toodelete an existing resource.</span></span>

<span data-ttu-id="2390b-168">Hello požadavku a odpovědi požadavkům tooa Formát datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="2390b-168">Both hello request and response conform tooa JSON payload format.</span></span> <span data-ttu-id="2390b-169">Další podrobnosti najdete v tématu [rozhraní API pro správu prostředků Azure](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span><span class="sxs-lookup"><span data-stu-id="2390b-169">For more details, see [Azure Resource Management APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx).</span></span>

### <a name="resource-manager-template-language"></a><span data-ttu-id="2390b-170">Jazyk šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2390b-170">Resource Manager template language</span></span>
<span data-ttu-id="2390b-171">V přidání toomanaging prostředky imperativní (přes rozhraní API nebo sady SDK) můžete také použít deklarativní programování toobuild styl a spravovat síťovým prostředkům pomocí hello jazyk šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="2390b-171">In addition toomanaging resources imperatively (via APIs or SDK), you can also use a declarative programming style toobuild and manage network resources by using hello Resource Manager Template Language.</span></span>

<span data-ttu-id="2390b-172">Ukázka reprezentace šablony najdete níže –</span><span class="sxs-lookup"><span data-stu-id="2390b-172">A sample representation of a template is provided below –</span></span>

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

<span data-ttu-id="2390b-173">Šablona Hello je primárně popis JSON hello prostředků a hodnoty instance hello vložit prostřednictvím parametrů.</span><span class="sxs-lookup"><span data-stu-id="2390b-173">hello template is primarily a JSON description of hello resources and hello instance values injected via parameters.</span></span> <span data-ttu-id="2390b-174">Následující příklad Hello lze použít toocreate virtuální síť s 2 podsítě.</span><span class="sxs-lookup"><span data-stu-id="2390b-174">hello example below can be used toocreate a virtual network with 2 subnets.</span></span>

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

<span data-ttu-id="2390b-175">Máte možnost hello ručně zadáním hodnot parametru hello při použití šablony, nebo můžete použít soubor s parametry.</span><span class="sxs-lookup"><span data-stu-id="2390b-175">You have hello option of providing hello parameter values manually when using a template, or you can use a parameter file.</span></span> <span data-ttu-id="2390b-176">Následující příklad Hello ukazuje možné sadu toobe hodnoty parametru použít s hello šablony výše:</span><span class="sxs-lookup"><span data-stu-id="2390b-176">hello example below shows a possible set of parameter values toobe used with hello template above:</span></span>

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


<span data-ttu-id="2390b-177">Hello hlavní výhody použití šablony jsou:</span><span class="sxs-lookup"><span data-stu-id="2390b-177">hello main advantages of using templates are:</span></span>

* <span data-ttu-id="2390b-178">Můžete vytvořit komplexní infrastruktury ve skupině prostředků v deklarativní stylu.</span><span class="sxs-lookup"><span data-stu-id="2390b-178">You can build a complex infrastructure in a resource group in a declarative style.</span></span> <span data-ttu-id="2390b-179">Dobrý den orchestraci vytváření hello prostředků, včetně správy závislostí, zpracovává pomocí Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="2390b-179">hello orchestration of creating hello resources, including dependency management, is handled by Resource Manager.</span></span>
* <span data-ttu-id="2390b-180">Hello infrastruktury vytvořením opakovatelných způsobem v různých oblastech a v oblasti jednoduše změnou parametry.</span><span class="sxs-lookup"><span data-stu-id="2390b-180">hello infrastructure can be created in a repeatable way across various regions and within a region by simply changing parameters.</span></span>
* <span data-ttu-id="2390b-181">deklarativní styl Hello vede doba realizace tooshorter ve vytváření šablony hello a zavádí hello infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="2390b-181">hello declarative style leads tooshorter lead time in building hello templates and rolling out hello infrastructure.</span></span>

<span data-ttu-id="2390b-182">Ukázka šablony, najdete v části [šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="2390b-182">For sample templates, see [Azure quickstart templates](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="2390b-183">Další informace o hello jazyk šablony Resource Manageru, najdete v části [jazyk šablony Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2390b-183">For more information on hello Resource Manager Template Language, see [Azure Resource Manager Template Language](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="2390b-184">výše uvedené šablony ukázka Hello používá hello virtuální síť a podsíť prostředky.</span><span class="sxs-lookup"><span data-stu-id="2390b-184">hello sample template above uses hello virtual network and subnet resources.</span></span> <span data-ttu-id="2390b-185">Existují jiným síťovým prostředkům, které můžete použít jak je uvedeno dále:</span><span class="sxs-lookup"><span data-stu-id="2390b-185">There are other network resources you can use as listed below:</span></span>

### <a name="using-a-template"></a><span data-ttu-id="2390b-186">Pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="2390b-186">Using a template</span></span>
<span data-ttu-id="2390b-187">TooAzure služby ze šablony můžete nasadit pomocí prostředí PowerShell, AzureCLI, nebo provedením toodeploy klikněte na z webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="2390b-187">You can deploy services tooAzure from a template by using PowerShell, AzureCLI, or by performing a click toodeploy from GitHub.</span></span> <span data-ttu-id="2390b-188">toodeploy služby ze šablony v Githubu, spusťte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2390b-188">toodeploy services from a template in GitHub, execute hello following steps:</span></span>

1. <span data-ttu-id="2390b-189">Otevřete soubor template3 hello z Githubu.</span><span class="sxs-lookup"><span data-stu-id="2390b-189">Open hello template3 file from GitHub.</span></span> <span data-ttu-id="2390b-190">Jako příklad, otevřete [virtuální síť se dvěma podsítěmi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="2390b-190">As an example, open [Virtual network with two subnets](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).</span></span>
2. <span data-ttu-id="2390b-191">Klikněte na **nasazení tooAzure**a potom se přihlaste na toohello portálu Azure pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="2390b-191">Click on **Deploy tooAzure**, and then sign in on toohello Azure portal with your credentials.</span></span>
3. <span data-ttu-id="2390b-192">Ověření hello šablony a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2390b-192">Verify hello template, and then click **Save**.</span></span>
4. <span data-ttu-id="2390b-193">Klikněte na tlačítko **upravit parametry** a vyberte umístění, jako například *západní USA*pro hello virtuální sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="2390b-193">Click **Edit parameters** and select a location, such as *West US*, for hello vnet and subnets.</span></span>
5. <span data-ttu-id="2390b-194">V případě potřeby změňte hello **ADDRESSPREFIX** a **SUBNETPREFIX** parametry a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2390b-194">If necessary, change hello **ADDRESSPREFIX** and **SUBNETPREFIX** parameters, and then click **OK**.</span></span>
6. <span data-ttu-id="2390b-195">Klikněte na tlačítko **vyberte skupinu prostředků** a potom klikněte na skupinu prostředků hello chcete tooadd hello virtuální sítě a podsítě, které.</span><span class="sxs-lookup"><span data-stu-id="2390b-195">Click **Select a resource group** and then click on hello resource group you want tooadd hello vnet and subnets to.</span></span> <span data-ttu-id="2390b-196">Alternativně můžete vytvořit novou skupinu prostředků kliknutím **nebo vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="2390b-196">Alternatively, you can create a new resource group by clicking **Or create new**.</span></span>
7. <span data-ttu-id="2390b-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2390b-197">Click **Create**.</span></span> <span data-ttu-id="2390b-198">Všimněte si dlaždice zobrazení hello **nasazení zřizování šablony**.</span><span class="sxs-lookup"><span data-stu-id="2390b-198">Notice hello tile displaying **Provisioning Template deployment**.</span></span> <span data-ttu-id="2390b-199">Po dokončení nasazení hello, zobrazí se dole podobné tooone obrazovky.</span><span class="sxs-lookup"><span data-stu-id="2390b-199">Once hello deployment is done, you will see a screen similar tooone below.</span></span>

![Ukázka nasazení šablony](./media/resource-groups-networking/Figure6.png)

## <a name="next-steps"></a><span data-ttu-id="2390b-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2390b-201">Next steps</span></span>
[<span data-ttu-id="2390b-202">Jazyk šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2390b-202">Azure Resource Manager Template Language</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)

[<span data-ttu-id="2390b-203">Azure sítě – běžně používané šablony</span><span class="sxs-lookup"><span data-stu-id="2390b-203">Azure Networking – commonly used templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

[<span data-ttu-id="2390b-204">Azure Resource Manager oproti nasazení classic</span><span class="sxs-lookup"><span data-stu-id="2390b-204">Azure Resource Manager vs. classic deployment</span></span>](../azure-resource-manager/resource-manager-deployment-model.md)

[<span data-ttu-id="2390b-205">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2390b-205">Azure Resource Manager Overview</span></span>](../azure-resource-manager/resource-group-overview.md)

