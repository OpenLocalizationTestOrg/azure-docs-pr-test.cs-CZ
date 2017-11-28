---
title: "komplexní hodnoty aaaPass mezi šablony Azure | Microsoft Docs"
description: "Zobrazí doporučená přístupy k pomocí šablony Azure Resource Manager a propojená data o stavu tooshare komplexních objektů."
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
ms.openlocfilehash: 72df1dee351446cea6ce15269e6db288b1f1db79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="share-state-tooand-from-azure-resource-manager-templates"></a><span data-ttu-id="28287-103">Sdílená složka stavu tooand ze šablon Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="28287-103">Share state tooand from Azure Resource Manager templates</span></span>
<span data-ttu-id="28287-104">Toto téma ukazuje osvědčené postupy pro správu a sdílení stavu v rámci šablony.</span><span class="sxs-lookup"><span data-stu-id="28287-104">This topic shows best practices for managing and sharing state within templates.</span></span> <span data-ttu-id="28287-105">Hello parametry a proměnné, které jsou uvedené v tomto tématu jsou příklady typu hello objektů můžete definovat tooconveniently uspořádání požadavků vašeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="28287-105">hello parameters and variables shown in this topic are examples of hello type of objects you can define tooconveniently organize your deployment requirements.</span></span> <span data-ttu-id="28287-106">V těchto příkladech můžete implementovat vlastní objekty s hodnoty vlastností, které dávají smysl pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="28287-106">From these examples, you can implement your own objects with property values that make sense for your environment.</span></span>

<span data-ttu-id="28287-107">Toto téma je součástí větší dokument White Paper.</span><span class="sxs-lookup"><span data-stu-id="28287-107">This topic is part of a larger whitepaper.</span></span> <span data-ttu-id="28287-108">Stáhnout tooread hello úplné papír [World Třída prostředků Manager šablony požadavky a osvědčené postupy](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span><span class="sxs-lookup"><span data-stu-id="28287-108">tooread hello full paper, download [World Class Resource Manager Templates Considerations and Proven Practices](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).</span></span>

## <a name="provide-standard-configuration-settings"></a><span data-ttu-id="28287-109">Zadejte standardní konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="28287-109">Provide standard configuration settings</span></span>
<span data-ttu-id="28287-110">Místo nabízí šablonu, která poskytuje celkový flexibilitu a obrovském množství variant, je běžné vzor tooprovide výběr známé konfigurace.</span><span class="sxs-lookup"><span data-stu-id="28287-110">Rather than offer a template that provides total flexibility and countless variations, a common pattern is tooprovide a selection of known configurations.</span></span> <span data-ttu-id="28287-111">V důsledku toho mohou uživatelé vybrat standardní velikosti tričko například izolovaného prostoru, malé, střední a velké.</span><span class="sxs-lookup"><span data-stu-id="28287-111">In effect, users can select standard t-shirt sizes such as sandbox, small, medium, and large.</span></span> <span data-ttu-id="28287-112">Další příklady tričko velikosti jsou nabídky produktů, jako je například community edition nebo enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="28287-112">Other examples of t-shirt sizes are product offerings, such as community edition or enterprise edition.</span></span> <span data-ttu-id="28287-113">V jiných případech může být určeného pro konkrétní úlohy konfigurace technologii – například mapy snížit nebo žádné sql.</span><span class="sxs-lookup"><span data-stu-id="28287-113">In other cases, it may be workload-specific configurations of a technology – such as map reduce or no sql.</span></span>

<span data-ttu-id="28287-114">S komplexních objektů můžete vytvářet proměnné, které obsahují kolekcí dat, někdy označovanou jako "kontejnery vlastností" a použít tuto deklaraci data toodrive hello prostředků ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="28287-114">With complex objects, you can create variables that contain collections of data, sometimes known as "property bags" and use that data toodrive hello resource declaration in your template.</span></span> <span data-ttu-id="28287-115">Tento přístup poskytuje dobrý, známé konfigurace různou velikost předem konfigurovaných pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="28287-115">This approach provides good, known configurations of varying sizes that are preconfigured for customers.</span></span> <span data-ttu-id="28287-116">Bez známé konfigurace, musí uživatelé hello šablony určit velikost clusteru na své vlastní, faktorem omezení prostředků platformy a provádět matematické tooidentify hello výsledná oddíly účty úložiště a dalším prostředkům (z důvodu toocluster velikost a omezení zdrojů).</span><span class="sxs-lookup"><span data-stu-id="28287-116">Without known configurations, users of hello template must determine cluster sizing on their own, factor in platform resource constraints, and do math tooidentify hello resulting partitioning of storage accounts and other resources (due toocluster size and resource constraints).</span></span> <span data-ttu-id="28287-117">Kromě toho toomaking lepší prostředí pro zákazníka hello několik známé konfigurace jsou jednodušší toosupport a mohou vám pomoci poskytovat vyšší úroveň hustotu.</span><span class="sxs-lookup"><span data-stu-id="28287-117">In addition toomaking a better experience for hello customer, a few known configurations are easier toosupport and can help you deliver a higher level of density.</span></span>

<span data-ttu-id="28287-118">Následující příklad ukazuje, jak Hello toodefine proměnné, které obsahují pro představující kolekce dat na komplexní objekty.</span><span class="sxs-lookup"><span data-stu-id="28287-118">hello following example shows how toodefine variables that contain complex objects for representing collections of data.</span></span> <span data-ttu-id="28287-119">kolekce Hello definovat hodnoty, které se používají pro velikost virtuálního počítače, nastavení sítě, nastavení operačního systému a nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="28287-119">hello collections define values that are used for virtual machine size, network settings, operating system settings and availability settings.</span></span>

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

<span data-ttu-id="28287-120">Všimněte si, že hello **tshirtSize** zřetězí velikost trička hello zadaný pomocí parametru (**malé**, **střední**, **velké**) toohello text **tshirtSize**.</span><span class="sxs-lookup"><span data-stu-id="28287-120">Notice that hello **tshirtSize** variable concatenates hello t-shirt size you provided through a parameter (**Small**, **Medium**, **Large**) toohello text **tshirtSize**.</span></span> <span data-ttu-id="28287-121">Tato proměnná proměnné tooretrieve hello přidružené komplexní objekt se používá pro tato velikost tričko.</span><span class="sxs-lookup"><span data-stu-id="28287-121">You use this variable tooretrieve hello associated complex object variable for that t-shirt size.</span></span>

<span data-ttu-id="28287-122">Pak můžete odkazovat tyto proměnné později v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="28287-122">You can then reference these variables later in hello template.</span></span> <span data-ttu-id="28287-123">zjednodušuje hello syntaxi šablony a umožňuje snadno toounderstand kontextu, Hello možnost tooreference s názvem proměnné a jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="28287-123">hello ability tooreference named-variables and their properties simplifies hello template syntax, and makes it easy toounderstand context.</span></span> <span data-ttu-id="28287-124">Následující ukázka Hello definuje toodeploy prostředků pomocí objektů hello uvedený výše tooset hodnoty.</span><span class="sxs-lookup"><span data-stu-id="28287-124">hello following example defines a resource toodeploy by using hello objects shown previously tooset values.</span></span> <span data-ttu-id="28287-125">Například je hello velikost virtuálního počítače nastavit načtením hello hodnotu `variables('tshirtSize').vmSize` při hello hodnotu pro velikost disku hello se načítají z `variables('tshirtSize').diskSize`.</span><span class="sxs-lookup"><span data-stu-id="28287-125">For example, hello VM size is set by retrieving hello value for `variables('tshirtSize').vmSize` while hello value for hello disk size is retrieved from `variables('tshirtSize').diskSize`.</span></span> <span data-ttu-id="28287-126">Kromě toho hello URI propojené šablony je nastavený s hodnotou hello `variables('tshirtSize').vmTemplate`.</span><span class="sxs-lookup"><span data-stu-id="28287-126">In addition, hello URI for a linked template is set with hello value for `variables('tshirtSize').vmTemplate`.</span></span>

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

## <a name="pass-state-tooa-template"></a><span data-ttu-id="28287-127">Předat stavu tooa šablony</span><span class="sxs-lookup"><span data-stu-id="28287-127">Pass state tooa template</span></span>
<span data-ttu-id="28287-128">Sdílíte stavu do šablony prostřednictvím parametrů, které poskytují přímo během nasazení.</span><span class="sxs-lookup"><span data-stu-id="28287-128">You share state into a template through parameters that you provide directly during deployment.</span></span>

<span data-ttu-id="28287-129">Hello následující parametry tabulky seznamy běžně používané v šablonách.</span><span class="sxs-lookup"><span data-stu-id="28287-129">hello following table lists commonly used parameters in templates.</span></span>

| <span data-ttu-id="28287-130">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="28287-130">Name</span></span> | <span data-ttu-id="28287-131">Hodnota</span><span class="sxs-lookup"><span data-stu-id="28287-131">Value</span></span> | <span data-ttu-id="28287-132">Popis</span><span class="sxs-lookup"><span data-stu-id="28287-132">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="28287-133">location</span><span class="sxs-lookup"><span data-stu-id="28287-133">location</span></span> |<span data-ttu-id="28287-134">Řetězec z omezené seznam oblastí Azure</span><span class="sxs-lookup"><span data-stu-id="28287-134">String from a constrained list of Azure regions</span></span> |<span data-ttu-id="28287-135">Hello umístění, kde jsou nasazené prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="28287-135">hello location where hello resources are deployed.</span></span> |
| <span data-ttu-id="28287-136">storageAccountNamePrefix</span><span class="sxs-lookup"><span data-stu-id="28287-136">storageAccountNamePrefix</span></span> |<span data-ttu-id="28287-137">Řetězec</span><span class="sxs-lookup"><span data-stu-id="28287-137">String</span></span> |<span data-ttu-id="28287-138">Jedinečný název DNS pro hello účet úložiště, kde jsou umístěné disky hello Virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="28287-138">Unique DNS name for hello Storage Account where hello VM's disks are placed</span></span> |
| <span data-ttu-id="28287-139">domainName</span><span class="sxs-lookup"><span data-stu-id="28287-139">domainName</span></span> |<span data-ttu-id="28287-140">Řetězec</span><span class="sxs-lookup"><span data-stu-id="28287-140">String</span></span> |<span data-ttu-id="28287-141">Název domény hello veřejně přístupná jumpbox virtuálních počítačů ve formátu hello: **{domainName}. { umístění}.cloudapp.com** například: **mydomainname.westus.cloudapp.azure.com**</span><span class="sxs-lookup"><span data-stu-id="28287-141">Domain name of hello publicly accessible jumpbox VM in hello format: **{domainName}.{location}.cloudapp.com** For example: **mydomainname.westus.cloudapp.azure.com**</span></span> |
| <span data-ttu-id="28287-142">adminUsername</span><span class="sxs-lookup"><span data-stu-id="28287-142">adminUsername</span></span> |<span data-ttu-id="28287-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="28287-143">String</span></span> |<span data-ttu-id="28287-144">Uživatelské jméno pro hello virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="28287-144">Username for hello VMs</span></span> |
| <span data-ttu-id="28287-145">adminPassword</span><span class="sxs-lookup"><span data-stu-id="28287-145">adminPassword</span></span> |<span data-ttu-id="28287-146">Řetězec</span><span class="sxs-lookup"><span data-stu-id="28287-146">String</span></span> |<span data-ttu-id="28287-147">Heslo pro hello virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="28287-147">Password for hello VMs</span></span> |
| <span data-ttu-id="28287-148">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="28287-148">tshirtSize</span></span> |<span data-ttu-id="28287-149">Řetězec, ze seznamu omezené nabízí tričko velikosti</span><span class="sxs-lookup"><span data-stu-id="28287-149">String from a constrained list of offered t-shirt sizes</span></span> |<span data-ttu-id="28287-150">Hello s názvem tooprovision velikost jednotky škálování.</span><span class="sxs-lookup"><span data-stu-id="28287-150">hello named scale unit size tooprovision.</span></span> <span data-ttu-id="28287-151">Například "Malá", "střední", "Velké"</span><span class="sxs-lookup"><span data-stu-id="28287-151">For example, "Small", "Medium", "Large"</span></span> |
| <span data-ttu-id="28287-152">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="28287-152">virtualNetworkName</span></span> |<span data-ttu-id="28287-153">Řetězec</span><span class="sxs-lookup"><span data-stu-id="28287-153">String</span></span> |<span data-ttu-id="28287-154">Název hello virtuální síť, která hello příjemce chce toouse.</span><span class="sxs-lookup"><span data-stu-id="28287-154">Name of hello virtual network that hello consumer wants toouse.</span></span> |
| <span data-ttu-id="28287-155">enableJumpbox</span><span class="sxs-lookup"><span data-stu-id="28287-155">enableJumpbox</span></span> |<span data-ttu-id="28287-156">Řetězec omezené seznamu (povoleno nebo zakázáno)</span><span class="sxs-lookup"><span data-stu-id="28287-156">String from a constrained list (enabled/disabled)</span></span> |<span data-ttu-id="28287-157">Parametr, který identifikuje zda tooenable jumpbox hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="28287-157">Parameter that identifies whether tooenable a jumpbox for hello environment.</span></span> <span data-ttu-id="28287-158">Hodnoty: "povoleno", "Zakázat"</span><span class="sxs-lookup"><span data-stu-id="28287-158">Values: "enabled", "disabled"</span></span> |

<span data-ttu-id="28287-159">Hello **tshirtSize** parametr použitý v předchozí části hello je definován jako:</span><span class="sxs-lookup"><span data-stu-id="28287-159">hello **tshirtSize** parameter used in hello previous section is defined as:</span></span>

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
          "Description": "T-shirt size of hello MongoDB deployment"
        }
      }
    }


## <a name="pass-state-toolinked-templates"></a><span data-ttu-id="28287-160">Předat stavu toolinked šablony</span><span class="sxs-lookup"><span data-stu-id="28287-160">Pass state toolinked templates</span></span>
<span data-ttu-id="28287-161">Při připojování toolinked šablony, můžete často použít kombinaci statických a vygeneruje proměnné.</span><span class="sxs-lookup"><span data-stu-id="28287-161">When connecting toolinked templates, you often use a mix of static and generated variables.</span></span>

### <a name="static-variables"></a><span data-ttu-id="28287-162">Statické proměnné</span><span class="sxs-lookup"><span data-stu-id="28287-162">Static variables</span></span>
<span data-ttu-id="28287-163">Statické proměnné jsou často používané tooprovide základní hodnoty, například adresy URL, které se používají v rámci šablony.</span><span class="sxs-lookup"><span data-stu-id="28287-163">Static variables are often used tooprovide base values, such as URLs, that are used throughout a template.</span></span>

<span data-ttu-id="28287-164">V následující výňatek ze šablony, hello `templateBaseUrl` určuje hello kořenový adresář pro šablonu hello v Githubu.</span><span class="sxs-lookup"><span data-stu-id="28287-164">In hello following template excerpt, `templateBaseUrl` specifies hello root location for hello template in GitHub.</span></span> <span data-ttu-id="28287-165">Další řádek Hello vytvoří novou proměnnou `sharedTemplateUrl` který zřetězí hello základní adresu URL s hello známý název šablony hello sdílených prostředků.</span><span class="sxs-lookup"><span data-stu-id="28287-165">hello next line builds a new variable `sharedTemplateUrl` that concatenates hello base URL with hello known name of hello shared resources template.</span></span> <span data-ttu-id="28287-166">Níže daného řádku proměnnou komplexní objekt je použité toostore velikost trička, kde základní adresu URL hello zřetězených toohello označuje umístění konfigurace šablon a uloženy v hello `vmTemplate` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="28287-166">Below that line, a complex object variable is used toostore a t-shirt size, where hello base URL is concatenated toohello known configuration template location and stored in hello `vmTemplate` property.</span></span>

<span data-ttu-id="28287-167">Hello výhody tohoto přístupu je, že pokud se změní umístění šablon hello pouze toochange hello statické proměnné na jednom místě, které vyhovují v rámci hello propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="28287-167">hello benefit of this approach is that if hello template location changes, you only need toochange hello static variable in one place, which passes it throughout hello linked templates.</span></span>

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

### <a name="generated-variables"></a><span data-ttu-id="28287-168">Vygenerovaný proměnné</span><span class="sxs-lookup"><span data-stu-id="28287-168">Generated variables</span></span>
<span data-ttu-id="28287-169">Několik proměnné přidání toostatic proměnné, jsou generována dynamicky.</span><span class="sxs-lookup"><span data-stu-id="28287-169">In addition toostatic variables, several variables are generated dynamically.</span></span> <span data-ttu-id="28287-170">Tato část popisuje některé běžné typy hello generovaného proměnných.</span><span class="sxs-lookup"><span data-stu-id="28287-170">This section identifies some of hello common types of generated variables.</span></span>

#### <a name="tshirtsize"></a><span data-ttu-id="28287-171">tshirtSize</span><span class="sxs-lookup"><span data-stu-id="28287-171">tshirtSize</span></span>
<span data-ttu-id="28287-172">Jste obeznámeni s Tato proměnná generovaný z výše uvedených příkladech hello.</span><span class="sxs-lookup"><span data-stu-id="28287-172">You are familiar with this generated variable from hello examples above.</span></span>

#### <a name="networksettings"></a><span data-ttu-id="28287-173">networkSettings</span><span class="sxs-lookup"><span data-stu-id="28287-173">networkSettings</span></span>
<span data-ttu-id="28287-174">V kapacitu, schopností nebo oboru řešení začátku do konce šablony hello propojených šablon obvykle vytvořit prostředky, které existují v síti.</span><span class="sxs-lookup"><span data-stu-id="28287-174">In a capacity, capability, or end-to-end scoped solution template, hello linked templates typically create resources that exist on a network.</span></span> <span data-ttu-id="28287-175">Jeden Nejjednodušším způsobem je toouse nastavení sítě toostore komplexní objekt a předat je toolinked šablony.</span><span class="sxs-lookup"><span data-stu-id="28287-175">One straightforward approach is toouse a complex object toostore network settings and pass them toolinked templates.</span></span>

<span data-ttu-id="28287-176">Níže najdete příklad komunikaci nastavení sítě.</span><span class="sxs-lookup"><span data-stu-id="28287-176">An example of communicating network settings can be seen below.</span></span>

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

#### <a name="availabilitysettings"></a><span data-ttu-id="28287-177">availabilitySettings</span><span class="sxs-lookup"><span data-stu-id="28287-177">availabilitySettings</span></span>
<span data-ttu-id="28287-178">Prostředky vytvořené v propojených šablon jsou často umístěny v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="28287-178">Resources created in linked templates are often placed in an availability set.</span></span> <span data-ttu-id="28287-179">V následujícím příkladu hello název sady dostupnosti hello je zadána a také hello doména selhání a aktualizace domény počet toouse.</span><span class="sxs-lookup"><span data-stu-id="28287-179">In hello following example, hello availability set name is specified and also hello fault domain and update domain count toouse.</span></span>

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

<span data-ttu-id="28287-180">Pokud potřebujete více skupiny dostupnosti (například jeden pro hlavní uzly) a druhý pro datové uzly, můžete použít název jako předpony, zadejte několik skupin dostupnosti nebo postupujte podle modelu hello uvedené výše pro vytvoření proměnné pro konkrétní velikost trička.</span><span class="sxs-lookup"><span data-stu-id="28287-180">If you need multiple availability sets (for example, one for master nodes and another for data nodes), you can use a name as a prefix, specify multiple availability sets, or follow hello model shown earlier for creating a variable for a specific t-shirt size.</span></span>

#### <a name="storagesettings"></a><span data-ttu-id="28287-181">storageSettings</span><span class="sxs-lookup"><span data-stu-id="28287-181">storageSettings</span></span>
<span data-ttu-id="28287-182">Podrobnosti o úložiště jsou často sdílet s propojených šablon.</span><span class="sxs-lookup"><span data-stu-id="28287-182">Storage details are often shared with linked templates.</span></span> <span data-ttu-id="28287-183">V následujícím příkladu hello *storageSettings* objekt poskytuje podrobnosti o hello názvy účtů a kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="28287-183">In hello example below, a *storageSettings* object provides details about hello storage account and container names.</span></span>

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a><span data-ttu-id="28287-184">osSettings</span><span class="sxs-lookup"><span data-stu-id="28287-184">osSettings</span></span>
<span data-ttu-id="28287-185">S propojených šablon může být nutné typy uzlů toopass operačního systému nastavení toovarious typů jiné známé konfigurace.</span><span class="sxs-lookup"><span data-stu-id="28287-185">With linked templates, you may need toopass operating system settings toovarious nodes types across different known configuration types.</span></span> <span data-ttu-id="28287-186">Komplexní objekt operačního systému informace snadný způsob, toostore a sdílené složky a také umožňuje snazší toosupport více možností operačního systému pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="28287-186">A complex object is an easy way toostore and share operating system information and also makes it easier toosupport multiple operating system choices for deployment.</span></span>

<span data-ttu-id="28287-187">Hello následující příklad ukazuje objekt pro *osSettings*:</span><span class="sxs-lookup"><span data-stu-id="28287-187">hello following example shows an object for *osSettings*:</span></span>

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a><span data-ttu-id="28287-188">machineSettings</span><span class="sxs-lookup"><span data-stu-id="28287-188">machineSettings</span></span>
<span data-ttu-id="28287-189">Proměnnou generovaného *machineSettings* je komplexní objekt obsahující směs základní proměnných pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="28287-189">A generated variable, *machineSettings* is a complex object containing a mix of core variables for creating a VM.</span></span> <span data-ttu-id="28287-190">proměnné Hello zahrnují správce uživatelské jméno a heslo, předpona pro názvy virtuálních počítačů hello a odkaz na bitovou kopii operačního systému.</span><span class="sxs-lookup"><span data-stu-id="28287-190">hello variables include administrator user name and password, a prefix for hello VM names, and an operating system image reference.</span></span>

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

<span data-ttu-id="28287-191">Všimněte si, že *osImageReference* načte hello hodnoty z hello *osSettings* proměnná definovaná v šabloně hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="28287-191">Note that *osImageReference* retrieves hello values from hello *osSettings* variable defined in hello main template.</span></span> <span data-ttu-id="28287-192">To znamená, že můžete snadno změnit hello operačního systému pro virtuální počítač – zcela nebo v závislosti na hello předvoleb šablony příjemce.</span><span class="sxs-lookup"><span data-stu-id="28287-192">That means you can easily change hello operating system for a VM—entirely or based on hello preference of a template consumer.</span></span>

#### <a name="vmscripts"></a><span data-ttu-id="28287-193">vmScripts</span><span class="sxs-lookup"><span data-stu-id="28287-193">vmScripts</span></span>
<span data-ttu-id="28287-194">Hello *vmScripts* objektu obsahuje podrobnosti o toodownload hello skripty a spustit v instanci virtuálního počítače, včetně odkazů a vnější.</span><span class="sxs-lookup"><span data-stu-id="28287-194">hello *vmScripts* object contains details about hello scripts toodownload and execute on a VM instance, including outside and inside references.</span></span> <span data-ttu-id="28287-195">Mimo zahrnovat odkazy hello infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="28287-195">Outside references include hello infrastructure.</span></span>
<span data-ttu-id="28287-196">Odkazy uvnitř zahrnují nainstalovaný hello nainstalovaný software a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="28287-196">Inside references include hello installed software installed and configuration.</span></span>

<span data-ttu-id="28287-197">Použít hello *scriptsToDownload* vlastnost toolist hello skriptů toodownload toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="28287-197">You use hello *scriptsToDownload* property toolist hello scripts toodownload toohello VM.</span></span> <span data-ttu-id="28287-198">Tento objekt obsahuje taky odkazy na řádku toocommand argumenty pro různé typy akcí.</span><span class="sxs-lookup"><span data-stu-id="28287-198">This object also contains references toocommand-line arguments for different types of actions.</span></span> <span data-ttu-id="28287-199">Mezi ně patří provádění hello výchozí instalace pro všechny jednotlivé uzly, instalaci, která spustí po všechny uzly, jsou nasazené a další skripty, které mohou být konkrétní tooa zadané šablony.</span><span class="sxs-lookup"><span data-stu-id="28287-199">These actions include executing hello default installation for each individual node, an installation that runs after all nodes are deployed, and any additional scripts that may be specific tooa given template.</span></span>

<span data-ttu-id="28287-200">V tomto příkladu je z šablony použité toodeploy MongoDB, které vyžaduje arbiter toodeliver vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="28287-200">This example is from a template used toodeploy MongoDB, which requires an arbiter toodeliver high availability.</span></span> <span data-ttu-id="28287-201">Hello *arbiterNodeInstallCommand* byla přidána příliš*vmScripts* tooinstall hello arbiter.</span><span class="sxs-lookup"><span data-stu-id="28287-201">hello *arbiterNodeInstallCommand* has been added too*vmScripts* tooinstall hello arbiter.</span></span>

<span data-ttu-id="28287-202">část proměnné Hello je, kde najít hello proměnné, které definují hello určitý text tooexecute hello skriptu s hello správné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="28287-202">hello variables section is where you find hello variables that define hello specific text tooexecute hello script with hello proper values.</span></span>

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a><span data-ttu-id="28287-203">Návratový stav ze šablony</span><span class="sxs-lookup"><span data-stu-id="28287-203">Return state from a template</span></span>
<span data-ttu-id="28287-204">Nejenže můžete předat data do šablony, můžete také sdílení dat toohello zpětné volání šablony.</span><span class="sxs-lookup"><span data-stu-id="28287-204">Not only can you pass data into a template, you can also share data back toohello calling template.</span></span> <span data-ttu-id="28287-205">V hello **výstupy** část propojené šablony, můžete zadat dvojice klíč/hodnota, které mohou být spotřebovávána hello zdrojové šabloně.</span><span class="sxs-lookup"><span data-stu-id="28287-205">In hello **outputs** section of a linked template, you can provide key/value pairs that can be consumed by hello source template.</span></span>

<span data-ttu-id="28287-206">Hello následující příklad ukazuje, jak toopass hello vygenerovaných šablonu propojené privátní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="28287-206">hello following example shows how toopass hello private IP address generated in a linked template.</span></span>

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

<span data-ttu-id="28287-207">V rámci hello hlavní šablony můžete tato data s hello následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="28287-207">Within hello main template, you can use that data with hello following syntax:</span></span>

    "[reference('master-node').outputs.masterip.value]"

<span data-ttu-id="28287-208">Můžete použít tento výraz v části výstupy hello nebo oddílu prostředků hello hello hlavní šablony.</span><span class="sxs-lookup"><span data-stu-id="28287-208">You can use this expression in either hello outputs section or hello resources section of hello main template.</span></span> <span data-ttu-id="28287-209">V části hello proměnné nelze použít hello výraz, protože závisí na stav modulu runtime hello.</span><span class="sxs-lookup"><span data-stu-id="28287-209">You cannot use hello expression in hello variables section because it relies on hello runtime state.</span></span> <span data-ttu-id="28287-210">tooreturn tuto hodnotu z hello hlavní šablony, použijte:</span><span class="sxs-lookup"><span data-stu-id="28287-210">tooreturn this value from hello main template, use:</span></span>

    "outputs": {
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

<span data-ttu-id="28287-211">Příklad použití hello výstupy část propojené šablony tooreturn datových disků pro virtuální počítač, najdete v části [vytváření více datových disků pro virtuální počítač](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="28287-211">For an example of using hello outputs section of a linked template tooreturn data disks for a virtual machine, see [Creating multiple data disks for a Virtual Machine](resource-group-create-multiple.md).</span></span>

## <a name="define-authentication-settings-for-virtual-machine"></a><span data-ttu-id="28287-212">Zadejte nastavení ověřování pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="28287-212">Define authentication settings for virtual machine</span></span>
<span data-ttu-id="28287-213">Můžete použít hello stejného vzoru zobrazovaného dříve pro nastavení ověřování hello toospecify pro konfiguraci nastavení pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="28287-213">You can use hello same pattern shown previously for configuration settings toospecify hello authentication settings for a virtual machine.</span></span> <span data-ttu-id="28287-214">Vytvoření parametru pro předávání v hello typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="28287-214">You create a parameter for passing in hello type of authentication.</span></span>

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

<span data-ttu-id="28287-215">Přidáte proměnných pro hello různá ověřovací typy a proměnné toostore, jaký typ se používá pro toto nasazení na základě hello hodnoty parametru hello.</span><span class="sxs-lookup"><span data-stu-id="28287-215">You add variables for hello different authentication types, and a variable toostore which type is used for this deployment based on hello value of hello parameter.</span></span>

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

<span data-ttu-id="28287-216">Při definování hello virtuálního počítače, nastavíte hello **osProfile** toohello proměnné, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="28287-216">When defining hello virtual machine, you set hello **osProfile** toohello variable you created.</span></span>

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a><span data-ttu-id="28287-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28287-217">Next steps</span></span>
* <span data-ttu-id="28287-218">toolearn o části hello hello šablony, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="28287-218">toolearn about hello sections of hello template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="28287-219">toosee hello funkce, které jsou k dispozici v rámci šablon, najdete v části [funkce šablon Azure Resource Manager](resource-group-template-functions.md)</span><span class="sxs-lookup"><span data-stu-id="28287-219">toosee hello functions that are available within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md)</span></span>
