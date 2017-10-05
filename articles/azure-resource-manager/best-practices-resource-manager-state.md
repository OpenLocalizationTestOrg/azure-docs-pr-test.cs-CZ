---
title: "Předejte komplexní hodnoty mezi šablony Azure | Microsoft Docs"
description: "Zobrazí doporučená přístupy k používání komplexních objektů ke sdílení dat o stavu pomocí šablon Azure Resource Manager a propojených šablon."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: 23cc4321159a87b61c177b11381646af8bd9eb35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="share-state-to-and-from-azure-resource-manager-templates"></a><span data-ttu-id="69fc1-103">Sdílená složka stavu do a z šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="69fc1-103">Share state to and from Azure Resource Manager templates</span></span>
<span data-ttu-id="69fc1-104">Toto téma ukazuje osvědčené postupy pro správu a sdílení stavu v rámci šablony.</span><span class="sxs-lookup"><span data-stu-id="69fc1-104">This topic shows best practices for managing and sharing state within templates.</span></span> <span data-ttu-id="69fc1-105">Parametry a proměnné, které jsou uvedené v tomto tématu jsou příklady typu objektů můžete definovat pohodlně uspořádání požadavků vašeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="69fc1-105">The parameters and variables shown in this topic are examples of the type of objects you can define to conveniently organize your deployment requirements.</span></span> <span data-ttu-id="69fc1-106">V těchto příkladech můžete implementovat vlastní objekty s hodnoty vlastností, které dávají smysl pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="69fc1-106">From these examples, you can implement your own objects with property values that make sense for your environment.</span></span>

<span data-ttu-id="69fc1-107">Toto téma je součástí větší dokument White Paper.</span><span class="sxs-lookup"><span data-stu-id="69fc1-107">This topic is part of a larger whitepaper.</span></span> <span data-ttu-id="69fc1-108">Číst úplnou dokumentu, stáhněte si [World Třída prostředků Manager šablony požadavky a osvědčené postupy](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span><span class="sxs-lookup"><span data-stu-id="69fc1-108">To read the full paper, download [World Class Resource Manager Templates Considerations and Proven Practices](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span></span>

## <a name="provide-standard-configuration-settings"></a><span data-ttu-id="69fc1-109">Zadejte standardní konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="69fc1-109">Provide standard configuration settings</span></span>
<span data-ttu-id="69fc1-110">Místo nabízí šablonu, která poskytuje celkový flexibilitu a obrovském množství variant, je běžné vzor poskytnout výběr známé konfigurace.</span><span class="sxs-lookup"><span data-stu-id="69fc1-110">Rather than offer a template that provides total flexibility and countless variations, a common pattern is to provide a selection of known configurations.</span></span> <span data-ttu-id="69fc1-111">V důsledku toho mohou uživatelé vybrat standardní velikosti tričko například izolovaného prostoru, malé, střední a velké.</span><span class="sxs-lookup"><span data-stu-id="69fc1-111">In effect, users can select standard t-shirt sizes such as sandbox, small, medium, and large.</span></span> <span data-ttu-id="69fc1-112">Další příklady tričko velikosti jsou nabídky produktů, jako je například community edition nebo enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="69fc1-112">Other examples of t-shirt sizes are product offerings, such as community edition or enterprise edition.</span></span> <span data-ttu-id="69fc1-113">V jiných případech může být určeného pro konkrétní úlohy konfigurace technologii – například mapy snížit nebo žádné sql.</span><span class="sxs-lookup"><span data-stu-id="69fc1-113">In other cases, it may be workload-specific configurations of a technology – such as map reduce or no sql.</span></span>

<span data-ttu-id="69fc1-114">S komplexních objektů můžete vytvářet proměnné, které obsahují kolekcí dat, někdy označovanou jako "kontejnery vlastností" a tato data použít k řízení deklaraci prostředků ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="69fc1-114">With complex objects, you can create variables that contain collections of data, sometimes known as "property bags" and use that data to drive the resource declaration in your template.</span></span> <span data-ttu-id="69fc1-115">Tento přístup poskytuje dobrý, známé konfigurace různou velikost předem konfigurovaných pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="69fc1-115">This approach provides good, known configurations of varying sizes that are preconfigured for customers.</span></span> <span data-ttu-id="69fc1-116">Bez známé konfigurace musí uživatelé šablony určit cluster pro změnu velikosti na své vlastní, zohlednit v omezení prostředků platformy a provést matematické k identifikaci výsledné oddíly účty úložiště a dalším prostředkům (z důvodu omezení velikosti a prostředek clusteru).</span><span class="sxs-lookup"><span data-stu-id="69fc1-116">Without known configurations, users of the template must determine cluster sizing on their own, factor in platform resource constraints, and do math to identify the resulting partitioning of storage accounts and other resources (due to cluster size and resource constraints).</span></span> <span data-ttu-id="69fc1-117">Kromě vytváření lepší podmínky pro zákazníka, několik známé konfigurace se snadněji podporovat a můžete poskytovat vyšší úroveň hustotou.</span><span class="sxs-lookup"><span data-stu-id="69fc1-117">In addition to making a better experience for the customer, a few known configurations are easier to support and can help you deliver a higher level of density.</span></span>

<span data-ttu-id="69fc1-118">Následující příklad ukazuje, jak k definování proměnné, které obsahují pro představující kolekce dat na komplexní objekty.</span><span class="sxs-lookup"><span data-stu-id="69fc1-118">The following example shows how to define variables that contain complex objects for representing collections of data.</span></span> <span data-ttu-id="69fc1-119">Kolekce definovat hodnoty, které se používají pro velikost virtuálního počítače, nastavení sítě, nastavení operačního systému a nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="69fc1-119">The collections define values that are used for virtual machine size, network settings, operating system settings and availability settings.</span></span>

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

<span data-ttu-id="69fc1-120">Všimněte si, že **tshirtSize** zřetězí velikost trička zadaný pomocí parametru (**malé**, **střední**, **velké**) na text **tshirtSize**.</span><span class="sxs-lookup"><span data-stu-id="69fc1-120">Notice that the **tshirtSize** variable concatenates the t-shirt size you provided through a parameter (**Small**, **Medium**, **Large**) to the text **tshirtSize**.</span></span> <span data-ttu-id="69fc1-121">Pomocí této proměnné načíst proměnnou přidružené komplexní objekt pro tento tričko velikost.</span><span class="sxs-lookup"><span data-stu-id="69fc1-121">You use this variable to retrieve the associated complex object variable for that t-shirt size.</span></span>

<span data-ttu-id="69fc1-122">Pak můžete odkazovat tyto proměnné později v šabloně.</span><span class="sxs-lookup"><span data-stu-id="69fc1-122">You can then reference these variables later in the template.</span></span> <span data-ttu-id="69fc1-123">Možnost k odkazování proměnné s názvem a jejich vlastnosti zjednodušuje syntaxi šablony a umožňuje snadno pochopit kontextu.</span><span class="sxs-lookup"><span data-stu-id="69fc1-123">The ability to reference named-variables and their properties simplifies the template syntax, and makes it easy to understand context.</span></span> <span data-ttu-id="69fc1-124">V následujícím příkladu definuje zdroj nasazení pomocí objekty zobrazené dříve nastavit hodnoty.</span><span class="sxs-lookup"><span data-stu-id="69fc1-124">The following example defines a resource to deploy by using the objects shown previously to set values.</span></span> <span data-ttu-id="69fc1-125">Například je velikost virtuálního počítače nastavit načtením hodnota `variables('tshirtSize').vmSize` při hodnotu pro velikost disku je načtena z `variables('tshirtSize').diskSize`.</span><span class="sxs-lookup"><span data-stu-id="69fc1-125">For example, the VM size is set by retrieving the value for `variables('tshirtSize').vmSize` while the value for the disk size is retrieved from `variables('tshirtSize').diskSize`.</span></span> <span data-ttu-id="69fc1-126">Kromě toho je nastaven identifikátor URI pro šablonu propojené s hodnotou pro `variables('tshirtSize').vmTemplate`.</span><span class="sxs-lookup"><span data-stu-id="69fc1-126">In addition, the URI for a linked template is set with the value for `variables('tshirtSize').vmTemplate`.</span></span>

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a><span data-ttu-id="69fc1-127">Předat stavu do šablony</span><span class="sxs-lookup"><span data-stu-id="69fc1-127">Pass state to a template</span></span>
<span data-ttu-id="69fc1-128">Sdílíte stavu do šablony prostřednictvím parametrů, které poskytují přímo během nasazení.</span><span class="sxs-lookup"><span data-stu-id="69fc1-128">You share state into a template through parameters that you provide directly during deployment.</span></span>

<span data-ttu-id="69fc1-129">Následující tabulka uvádí běžně používané parametry v šablonách.</span><span class="sxs-lookup"><span data-stu-id="69fc1-129">The following table lists commonly used parameters in templates.</span></span>

| <span data-ttu-id="69fc1-130">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="69fc1-130">Name</span></span> | <span data-ttu-id="69fc1-131">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69fc1-131">Value</span></span> | <span data-ttu-id="69fc1-132">Popis</span><span class="sxs-lookup"><span data-stu-id="69fc1-132">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="69fc1-133">location</span><span class="sxs-lookup"><span data-stu-id="69fc1-133">location</span></span> |<span data-ttu-id="69fc1-134">Řetězec z omezené seznam oblastí Azure</span><span class="sxs-lookup"><span data-stu-id="69fc1-134">String from a constrained list of Azure regions</span></span> |<span data-ttu-id="69fc1-135">Umístění, kde jsou nasazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="69fc1-135">The location where the resources are deployed.</span></span> |
| <span data-ttu-id="69fc1-136">storageAccountNamePrefix</span><span class="sxs-lookup"><span data-stu-id="69fc1-136">storageAccountNamePrefix</span></span> |<span data-ttu-id="69fc1-137">Řetězec</span><span class="sxs-lookup"><span data-stu-id="69fc1-137">String</span></span> |<span data-ttu-id="69fc1-138">Jedinečný název DNS pro účet úložiště, kde jsou umístěné disky Virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="69fc1-138">Unique DNS name for the Storage Account where the VM's disks are placed</span></span> |
| <span data-ttu-id="69fc1-139">domainName</span><span class="sxs-lookup"><span data-stu-id="69fc1-139">domainName</span></span> |<span data-ttu-id="69fc1-140">Řetězec</span><span class="sxs-lookup"><span data-stu-id="69fc1-140">String</span></span> |<span data-ttu-id="69fc1-141">Název domény veřejně přístupná jumpbox virtuálních počítačů ve formátu: **{domainName}. { umístění}.cloudapp.com** například: **mydomainname.westus.cloudapp.azure.com**</span><span class="sxs-lookup"><span data-stu-id="69fc1-141">Domain name of the publicly accessible jumpbox VM in the format: **{domainName}.{location}.cloudapp.com** For example: **mydomainname.westus.cloudapp.azure.com**</span></span> |
| <span data-ttu-id="69fc1-142">adminUsername</span><span class="sxs-lookup"><span data-stu-id="69fc1-142">adminUsername</span></span> |<span data-ttu-id="69fc1-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="69fc1-143">String</span></span> |<span data-ttu-id="69fc1-144">Uživatelské jméno pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="69fc1-144">Username for the VMs</span></span> |
| <span data-ttu-id="69fc1-145">adminPassword</span><span class="sxs-lookup"><span data-stu-id="69fc1-145">adminPassword</span></span> |<span data-ttu-id="69fc1-146">Řetězec</span><span class="sxs-lookup"><span data-stu-id="69fc1-146">String</span></span> |<span data-ttu-id="69fc1-147">Heslo pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="69fc1-147">Password for the VMs</span></span> |
| <span data-ttu-id="69fc1-148">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="69fc1-148">tshirtSize</span></span> |<span data-ttu-id="69fc1-149">Řetězec, ze seznamu omezené nabízí tričko velikosti</span><span class="sxs-lookup"><span data-stu-id="69fc1-149">String from a constrained list of offered t-shirt sizes</span></span> |<span data-ttu-id="69fc1-150">Velikost jednotky škálování s názvem zřídit.</span><span class="sxs-lookup"><span data-stu-id="69fc1-150">The named scale unit size to provision.</span></span> <span data-ttu-id="69fc1-151">Například "Malá", "střední", "Velké"</span><span class="sxs-lookup"><span data-stu-id="69fc1-151">For example, "Small", "Medium", "Large"</span></span> |
| <span data-ttu-id="69fc1-152">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="69fc1-152">virtualNetworkName</span></span> |<span data-ttu-id="69fc1-153">Řetězec</span><span class="sxs-lookup"><span data-stu-id="69fc1-153">String</span></span> |<span data-ttu-id="69fc1-154">Název virtuální sítě, které chce příjemce použít.</span><span class="sxs-lookup"><span data-stu-id="69fc1-154">Name of the virtual network that the consumer wants to use.</span></span> |
| <span data-ttu-id="69fc1-155">enableJumpbox</span><span class="sxs-lookup"><span data-stu-id="69fc1-155">enableJumpbox</span></span> |<span data-ttu-id="69fc1-156">Řetězec omezené seznamu (povoleno nebo zakázáno)</span><span class="sxs-lookup"><span data-stu-id="69fc1-156">String from a constrained list (enabled/disabled)</span></span> |<span data-ttu-id="69fc1-157">Parametr, který určuje, jestli se má povolit jumpbox pro prostředí.</span><span class="sxs-lookup"><span data-stu-id="69fc1-157">Parameter that identifies whether to enable a jumpbox for the environment.</span></span> <span data-ttu-id="69fc1-158">Hodnoty: "povoleno", "Zakázat"</span><span class="sxs-lookup"><span data-stu-id="69fc1-158">Values: "enabled", "disabled"</span></span> |

<span data-ttu-id="69fc1-159">**TshirtSize** parametr použitý v předchozí části je definován jako:</span><span class="sxs-lookup"><span data-stu-id="69fc1-159">The **tshirtSize** parameter used in the previous section is defined as:</span></span>

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a><span data-ttu-id="69fc1-160">Stav předat propojených šablon</span><span class="sxs-lookup"><span data-stu-id="69fc1-160">Pass state to linked templates</span></span>
<span data-ttu-id="69fc1-161">Při připojování k propojených šablon, můžete často použít kombinaci statických a vygeneruje proměnné.</span><span class="sxs-lookup"><span data-stu-id="69fc1-161">When connecting to linked templates, you often use a mix of static and generated variables.</span></span>

### <a name="static-variables"></a><span data-ttu-id="69fc1-162">Statické proměnné</span><span class="sxs-lookup"><span data-stu-id="69fc1-162">Static variables</span></span>
<span data-ttu-id="69fc1-163">Statické proměnné se často používá k poskytování základní hodnoty, například adresy URL, které se používají v rámci šablony.</span><span class="sxs-lookup"><span data-stu-id="69fc1-163">Static variables are often used to provide base values, such as URLs, that are used throughout a template.</span></span>

<span data-ttu-id="69fc1-164">V následující výňatek ze šablony `templateBaseUrl` Určuje kořenový adresář pro danou šablonu v Githubu.</span><span class="sxs-lookup"><span data-stu-id="69fc1-164">In the following template excerpt, `templateBaseUrl` specifies the root location for the template in GitHub.</span></span> <span data-ttu-id="69fc1-165">Na další řádek vytvoří novou proměnnou `sharedTemplateUrl` který zřetězí základní adresu URL s známý název šablony sdílených prostředků.</span><span class="sxs-lookup"><span data-stu-id="69fc1-165">The next line builds a new variable `sharedTemplateUrl` that concatenates the base URL with the known name of the shared resources template.</span></span> <span data-ttu-id="69fc1-166">Níže daného řádku proměnnou komplexní objekt se používá k ukládání velikost trička, kde je zřetězen do umístění šablony známé konfigurace a uložené v základní adresu URL `vmTemplate` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="69fc1-166">Below that line, a complex object variable is used to store a t-shirt size, where the base URL is concatenated to the known configuration template location and stored in the `vmTemplate` property.</span></span>

<span data-ttu-id="69fc1-167">Výhodou tohoto přístupu je, pokud se změní umístění šablon, potřebujete jenom statické proměnné na jednom místě, které vyhovují v propojených šablon změnit.</span><span class="sxs-lookup"><span data-stu-id="69fc1-167">The benefit of this approach is that if the template location changes, you only need to change the static variable in one place, which passes it throughout the linked templates.</span></span>

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a><span data-ttu-id="69fc1-168">Vygenerovaný proměnné</span><span class="sxs-lookup"><span data-stu-id="69fc1-168">Generated variables</span></span>
<span data-ttu-id="69fc1-169">Kromě statické proměnné několika proměnných generována dynamicky.</span><span class="sxs-lookup"><span data-stu-id="69fc1-169">In addition to static variables, several variables are generated dynamically.</span></span> <span data-ttu-id="69fc1-170">Tato část popisuje některé běžné typy generovaného proměnných.</span><span class="sxs-lookup"><span data-stu-id="69fc1-170">This section identifies some of the common types of generated variables.</span></span>

#### <a name="tshirtsize"></a><span data-ttu-id="69fc1-171">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="69fc1-171">tshirtSize</span></span>
<span data-ttu-id="69fc1-172">Jste obeznámeni s Tato proměnná generovaný z výše uvedených příkladech.</span><span class="sxs-lookup"><span data-stu-id="69fc1-172">You are familiar with this generated variable from the examples above.</span></span>

#### <a name="networksettings"></a><span data-ttu-id="69fc1-173">networkSettings</span><span class="sxs-lookup"><span data-stu-id="69fc1-173">networkSettings</span></span>
<span data-ttu-id="69fc1-174">V kapacitu, schopností nebo šablona vymezená řešení začátku do konce propojených šablon obvykle vytvořit prostředky, které existují v síti.</span><span class="sxs-lookup"><span data-stu-id="69fc1-174">In a capacity, capability, or end-to-end scoped solution template, the linked templates typically create resources that exist on a network.</span></span> <span data-ttu-id="69fc1-175">Jeden Nejjednodušším způsobem je použití komplexní objekt pro uložení nastavení sítě a předat propojených šablon.</span><span class="sxs-lookup"><span data-stu-id="69fc1-175">One straightforward approach is to use a complex object to store network settings and pass them to linked templates.</span></span>

<span data-ttu-id="69fc1-176">Níže najdete příklad komunikaci nastavení sítě.</span><span class="sxs-lookup"><span data-stu-id="69fc1-176">An example of communicating network settings can be seen below.</span></span>

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a><span data-ttu-id="69fc1-177">availabilitySettings</span><span class="sxs-lookup"><span data-stu-id="69fc1-177">availabilitySettings</span></span>
<span data-ttu-id="69fc1-178">Prostředky vytvořené v propojených šablon jsou často umístěny v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="69fc1-178">Resources created in linked templates are often placed in an availability set.</span></span> <span data-ttu-id="69fc1-179">V následujícím příkladu je zadána název sady dostupnosti a také počet doména selhání a aktualizace domény použít.</span><span class="sxs-lookup"><span data-stu-id="69fc1-179">In the following example, the availability set name is specified and also the fault domain and update domain count to use.</span></span>

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

<span data-ttu-id="69fc1-180">Pokud potřebujete více skupiny dostupnosti (například jeden pro hlavní uzly) a druhý pro datové uzly, můžete použít název jako předpony, zadejte několik skupin dostupnosti nebo postupujte podle modelu uvedené výše pro vytvoření proměnné pro konkrétní velikost trička.</span><span class="sxs-lookup"><span data-stu-id="69fc1-180">If you need multiple availability sets (for example, one for master nodes and another for data nodes), you can use a name as a prefix, specify multiple availability sets, or follow the model shown earlier for creating a variable for a specific t-shirt size.</span></span>

#### <a name="storagesettings"></a><span data-ttu-id="69fc1-181">storageSettings</span><span class="sxs-lookup"><span data-stu-id="69fc1-181">storageSettings</span></span>
<span data-ttu-id="69fc1-182">Podrobnosti o úložiště jsou často sdílet s propojených šablon.</span><span class="sxs-lookup"><span data-stu-id="69fc1-182">Storage details are often shared with linked templates.</span></span> <span data-ttu-id="69fc1-183">V příkladu níže *storageSettings* objektu obsahuje podrobné informace o názvu účtu a kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="69fc1-183">In the example below, a *storageSettings* object provides details about the storage account and container names.</span></span>

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a><span data-ttu-id="69fc1-184">osSettings</span><span class="sxs-lookup"><span data-stu-id="69fc1-184">osSettings</span></span>
<span data-ttu-id="69fc1-185">S propojených šablon musíte předat nastavení operačního systému na různé typy uzlů napříč jinou konfiguraci známé typy.</span><span class="sxs-lookup"><span data-stu-id="69fc1-185">With linked templates, you may need to pass operating system settings to various nodes types across different known configuration types.</span></span> <span data-ttu-id="69fc1-186">Komplexní objekt je snadný způsob, jak ukládat a sdílet informace o operačním systému a také usnadňuje podporují více možností operačního systému pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="69fc1-186">A complex object is an easy way to store and share operating system information and also makes it easier to support multiple operating system choices for deployment.</span></span>

<span data-ttu-id="69fc1-187">Následující příklad ukazuje objekt pro *osSettings*:</span><span class="sxs-lookup"><span data-stu-id="69fc1-187">The following example shows an object for *osSettings*:</span></span>

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a><span data-ttu-id="69fc1-188">machineSettings</span><span class="sxs-lookup"><span data-stu-id="69fc1-188">machineSettings</span></span>
<span data-ttu-id="69fc1-189">Proměnnou generovaného *machineSettings* je komplexní objekt obsahující směs základní proměnných pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="69fc1-189">A generated variable, *machineSettings* is a complex object containing a mix of core variables for creating a VM.</span></span> <span data-ttu-id="69fc1-190">Proměnné zahrnují správce uživatelské jméno a heslo, předpona pro názvy virtuálních počítačů a odkaz na bitovou kopii operačního systému.</span><span class="sxs-lookup"><span data-stu-id="69fc1-190">The variables include administrator user name and password, a prefix for the VM names, and an operating system image reference.</span></span>

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

<span data-ttu-id="69fc1-191">Všimněte si, že *osImageReference* načte hodnoty z *osSettings* proměnná definovaná v šabloně hlavní.</span><span class="sxs-lookup"><span data-stu-id="69fc1-191">Note that *osImageReference* retrieves the values from the *osSettings* variable defined in the main template.</span></span> <span data-ttu-id="69fc1-192">To znamená, že můžete snadno změnit operační systém pro virtuální počítač – zcela nebo v závislosti na předvoleb šablony příjemce.</span><span class="sxs-lookup"><span data-stu-id="69fc1-192">That means you can easily change the operating system for a VM—entirely or based on the preference of a template consumer.</span></span>

#### <a name="vmscripts"></a><span data-ttu-id="69fc1-193">vmScripts</span><span class="sxs-lookup"><span data-stu-id="69fc1-193">vmScripts</span></span>
<span data-ttu-id="69fc1-194">*VmScripts* objekt obsahuje podrobnosti o skripty ke stažení a spuštění na instanci virtuálního počítače, včetně odkazů a vnější.</span><span class="sxs-lookup"><span data-stu-id="69fc1-194">The *vmScripts* object contains details about the scripts to download and execute on a VM instance, including outside and inside references.</span></span> <span data-ttu-id="69fc1-195">Mimo obsahovat odkazy na infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="69fc1-195">Outside references include the infrastructure.</span></span>
<span data-ttu-id="69fc1-196">Odkazy uvnitř zahrnují nainstalovaného softwaru nainstalovaného a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="69fc1-196">Inside references include the installed software installed and configuration.</span></span>

<span data-ttu-id="69fc1-197">Můžete použít *scriptsToDownload* vlastnost seznam skripty ke stažení do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="69fc1-197">You use the *scriptsToDownload* property to list the scripts to download to the VM.</span></span> <span data-ttu-id="69fc1-198">Tento objekt obsahuje taky odkazy na argumenty příkazového řádku pro různé typy akcí.</span><span class="sxs-lookup"><span data-stu-id="69fc1-198">This object also contains references to command-line arguments for different types of actions.</span></span> <span data-ttu-id="69fc1-199">Mezi ně patří provádění výchozí instalace pro všechny jednotlivé uzly, instalaci, která spustí po všechny uzly, jsou nasazené a jakékoli další skripty, které mohou být specifické pro dané šablony.</span><span class="sxs-lookup"><span data-stu-id="69fc1-199">These actions include executing the default installation for each individual node, an installation that runs after all nodes are deployed, and any additional scripts that may be specific to a given template.</span></span>

<span data-ttu-id="69fc1-200">V tomto příkladu je ze šablony použít k nasazení MongoDB, který vyžaduje arbiter zajistit vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="69fc1-200">This example is from a template used to deploy MongoDB, which requires an arbiter to deliver high availability.</span></span> <span data-ttu-id="69fc1-201">*ArbiterNodeInstallCommand* byl přidán do *vmScripts* k instalaci arbiter.</span><span class="sxs-lookup"><span data-stu-id="69fc1-201">The *arbiterNodeInstallCommand* has been added to *vmScripts* to install the arbiter.</span></span>

<span data-ttu-id="69fc1-202">V části proměnné je, kde najít proměnné, které definují konkrétní text pro spuštění skriptu s správné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="69fc1-202">The variables section is where you find the variables that define the specific text to execute the script with the proper values.</span></span>

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a><span data-ttu-id="69fc1-203">Návratový stav ze šablony</span><span class="sxs-lookup"><span data-stu-id="69fc1-203">Return state from a template</span></span>
<span data-ttu-id="69fc1-204">Nejenže můžete předat data do šablony, můžete také sdílet data zpět do volání šablony.</span><span class="sxs-lookup"><span data-stu-id="69fc1-204">Not only can you pass data into a template, you can also share data back to the calling template.</span></span> <span data-ttu-id="69fc1-205">V **výstupy** část propojené šablony, můžete zadat dvojice klíč/hodnota, které mohou být spotřebovávána zdrojovou šablonu.</span><span class="sxs-lookup"><span data-stu-id="69fc1-205">In the **outputs** section of a linked template, you can provide key/value pairs that can be consumed by the source template.</span></span>

<span data-ttu-id="69fc1-206">Následující příklad ukazuje, jak předat privátní IP adresu vygenerovaných propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="69fc1-206">The following example shows how to pass the private IP address generated in a linked template.</span></span>

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

<span data-ttu-id="69fc1-207">V rámci hlavní šablony můžete tato data s následující syntaxí:</span><span class="sxs-lookup"><span data-stu-id="69fc1-207">Within the main template, you can use that data with the following syntax:</span></span>

    "[reference('master-node').outputs.masterip.value]"

<span data-ttu-id="69fc1-208">Můžete použít tento výraz v části výstupy nebo oddílu prostředků hlavní šablony.</span><span class="sxs-lookup"><span data-stu-id="69fc1-208">You can use this expression in either the outputs section or the resources section of the main template.</span></span> <span data-ttu-id="69fc1-209">V sekci proměnných nelze použít ve výrazu, protože závisí na stav modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="69fc1-209">You cannot use the expression in the variables section because it relies on the runtime state.</span></span> <span data-ttu-id="69fc1-210">Vrácená hodnota z hlavní šablony, použijte:</span><span class="sxs-lookup"><span data-stu-id="69fc1-210">To return this value from the main template, use:</span></span>

    "outputs": {
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

<span data-ttu-id="69fc1-211">Příklad použití části výstupy propojené šablony pro vrácení datových disků pro virtuální počítač, naleznete v části [vytváření více datových disků pro virtuální počítač](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="69fc1-211">For an example of using the outputs section of a linked template to return data disks for a virtual machine, see [Creating multiple data disks for a Virtual Machine](resource-group-create-multiple.md).</span></span>

## <a name="define-authentication-settings-for-virtual-machine"></a><span data-ttu-id="69fc1-212">Zadejte nastavení ověřování pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="69fc1-212">Define authentication settings for virtual machine</span></span>
<span data-ttu-id="69fc1-213">Stejného vzoru zobrazovaného dříve nastavení konfigurace můžete použít k určení nastavení ověřování pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="69fc1-213">You can use the same pattern shown previously for configuration settings to specify the authentication settings for a virtual machine.</span></span> <span data-ttu-id="69fc1-214">Vytvoření parametru pro předávání v typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="69fc1-214">You create a parameter for passing in the type of authentication.</span></span>

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

<span data-ttu-id="69fc1-215">Přidejte proměnných pro typy různých ověřování a proměnnou pro uložení jaký typ se používá pro toto nasazení na základě hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="69fc1-215">You add variables for the different authentication types, and a variable to store which type is used for this deployment based on the value of the parameter.</span></span>

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

<span data-ttu-id="69fc1-216">Při definování virtuální počítač, můžete nastavit **osProfile** do proměnné, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="69fc1-216">When defining the virtual machine, you set the **osProfile** to the variable you created.</span></span>

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a><span data-ttu-id="69fc1-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69fc1-217">Next steps</span></span>
* <span data-ttu-id="69fc1-218">Další informace o části šablony najdete v tématu [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="69fc1-218">To learn about the sections of the template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="69fc1-219">Funkce, které jsou k dispozici v rámci šablon najdete v sekci [funkce šablon Azure Resource Manager](resource-group-template-functions.md)</span><span class="sxs-lookup"><span data-stu-id="69fc1-219">To see the functions that are available within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md)</span></span>
