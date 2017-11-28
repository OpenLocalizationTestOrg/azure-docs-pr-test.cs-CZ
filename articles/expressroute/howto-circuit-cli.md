---
title: "Vytvoření a úprava okruhu Azure ExpressRoute: rozhraní příkazového řádku | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate, zřizovat, ověřte, aktualizovat, odstranit a zrušit jejich zřízení okruhu ExpressRoute pomocí rozhraní příkazového řádku."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 396e325658a59afadb209bb525cbb59ac775ae6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a><span data-ttu-id="5193f-103">Vytvoření a úprava okruhu ExpressRoute pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5193f-103">Create and modify an ExpressRoute circuit using CLI</span></span>


<span data-ttu-id="5193f-104">Tento článek popisuje, jak hello toocreate okruhu Azure ExpressRoute pomocí rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="5193f-104">This article describes how toocreate an Azure ExpressRoute circuit by using hello Command Line Interface (CLI).</span></span> <span data-ttu-id="5193f-105">Tento článek také ukazuje, jak toocheck hello stav, aktualizovat, nebo odstranit a zrušit jejich zřízení okruh.</span><span class="sxs-lookup"><span data-stu-id="5193f-105">This article also shows you how toocheck hello status, update, or delete and deprovision a circuit.</span></span> <span data-ttu-id="5193f-106">Pokud chcete toouse toowork jinou metodu s okruhy ExpressRoute, můžete vybrat hello článek z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="5193f-106">If you want toouse a different method toowork with ExpressRoute circuits, you can select hello article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5193f-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5193f-107">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="5193f-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5193f-108">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="5193f-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5193f-109">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="5193f-110">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="5193f-110">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="5193f-111">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="5193f-111">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a><span data-ttu-id="5193f-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="5193f-112">Before you begin</span></span>

* <span data-ttu-id="5193f-113">Než začnete, nainstalujte nejnovější verzi hello hello rozhraní příkazového řádku (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="5193f-113">Before beginning, install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="5193f-114">Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli) a [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5193f-114">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli) and [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>
* <span data-ttu-id="5193f-115">Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5193f-115">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="5193f-116">Vytvořit a zřídit okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5193f-116">Create and provision an ExpressRoute circuit</span></span>

### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a><span data-ttu-id="5193f-117">1. Přihlaste se tooyour účet Azure a vybrat své předplatné</span><span class="sxs-lookup"><span data-stu-id="5193f-117">1. Sign in tooyour Azure account and select your subscription</span></span>

<span data-ttu-id="5193f-118">toobegin konfiguraci přihlášení tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="5193f-118">toobegin your configuration, sign in tooyour Azure account.</span></span> <span data-ttu-id="5193f-119">Následující příklady toohelp, ke kterým se připojujete pomocí hello:</span><span class="sxs-lookup"><span data-stu-id="5193f-119">Use hello following examples toohelp you connect:</span></span>

```azurecli
az login
```

<span data-ttu-id="5193f-120">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-120">Check hello subscriptions for hello account.</span></span>

```azurecli
az account list
```

<span data-ttu-id="5193f-121">Vyberte hello předplatné, pro které chcete toocreate okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5193f-121">Select hello subscription for which you want toocreate an ExpressRoute circuit.</span></span>

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="5193f-122">2. Získat hello seznam podporovaných zprostředkovatelů, umístění a šířek pásma</span><span class="sxs-lookup"><span data-stu-id="5193f-122">2. Get hello list of supported providers, locations, and bandwidths</span></span>

<span data-ttu-id="5193f-123">Než vytvoříte okruh ExpressRoute, musíte hello seznam poskytovatelů podporovaných připojení, umístění a možnosti šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="5193f-123">Before you create an ExpressRoute circuit, you need hello list of supported connectivity providers, locations, and bandwidth options.</span></span> <span data-ttu-id="5193f-124">Hello rozhraní příkazového řádku příkaz 'az sítě express route seznamu, kteří, vrátí tato informace, které budete používat v dalších krocích:</span><span class="sxs-lookup"><span data-stu-id="5193f-124">hello CLI command 'az network express-route list-service-providers' returns this information, which you’ll use in later steps:</span></span>

```azurecli
az network express-route list-service-providers
```

<span data-ttu-id="5193f-125">odpověď Hello je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="5193f-125">hello response is similar toohello following example:</span></span>

```azurecli
[
  {
    "bandwidthsOffered": [
      {
        "offerName": "50Mbps",
        "valueInMbps": 50
      },
      {
        "offerName": "100Mbps",
        "valueInMbps": 100
      },
      {
        "offerName": "200Mbps",
        "valueInMbps": 200
      },
      {
        "offerName": "500Mbps",
        "valueInMbps": 500
      },
      {
        "offerName": "1Gbps",
        "valueInMbps": 1000
      },
      {
        "offerName": "2Gbps",
        "valueInMbps": 2000
      },
      {
        "offerName": "5Gbps",
        "valueInMbps": 5000
      },
      {
        "offerName": "10Gbps",
        "valueInMbps": 10000
      }
    ],
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/expressRouteServiceProviders/",
    "location": null,
    "name": "AARNet",
    "peeringLocations": [
      "Melbourne",
      "Sydney"
    ],
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "tags": null,
    "type": "Microsoft.Network/expressRouteServiceProviders"
  },
```

<span data-ttu-id="5193f-126">Hello odpovědi toosee zkontrolujte, zda je uvedený svého poskytovatele připojení.</span><span class="sxs-lookup"><span data-stu-id="5193f-126">Check hello response toosee if your connectivity provider is listed.</span></span> <span data-ttu-id="5193f-127">Poznamenejte si následující informace, které budete potřebovat při vytvoření okruhu hello:</span><span class="sxs-lookup"><span data-stu-id="5193f-127">Make a note of hello following information, which you will need when you create a circuit:</span></span>

* <span data-ttu-id="5193f-128">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5193f-128">Name</span></span>
* <span data-ttu-id="5193f-129">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="5193f-129">PeeringLocations</span></span>
* <span data-ttu-id="5193f-130">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="5193f-130">BandwidthsOffered</span></span>

<span data-ttu-id="5193f-131">Nyní jste připravené toocreate okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5193f-131">You're now ready toocreate an ExpressRoute circuit.</span></span>

### <a name="3-create-an-expressroute-circuit"></a><span data-ttu-id="5193f-132">3. Vytvoření okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5193f-132">3. Create an ExpressRoute circuit</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5193f-133">Váš okruh ExpressRoute je účtován z hello okamžiku, kdy se objeví klíč služby.</span><span class="sxs-lookup"><span data-stu-id="5193f-133">Your ExpressRoute circuit is billed from hello moment a service key is issued.</span></span> <span data-ttu-id="5193f-134">Tuto operaci proveďte, pokud poskytovatel připojení hello je připraven tooprovision hello okruh.</span><span class="sxs-lookup"><span data-stu-id="5193f-134">Perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

<span data-ttu-id="5193f-135">Pokud ještě nemáte skupinu prostředků, je musíte vytvořit před vytvořením váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5193f-135">If you don't already have a resource group, you must create one before you create your ExpressRoute circuit.</span></span> <span data-ttu-id="5193f-136">Můžete vytvořit skupinu prostředků spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5193f-136">You can create a resource group by running hello following command:</span></span>

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

<span data-ttu-id="5193f-137">Hello následující příklad ukazuje, jak toocreate 200 MB/s ExpressRoute okruhu prostřednictvím Equinix ze Silicon Valley.</span><span class="sxs-lookup"><span data-stu-id="5193f-137">hello following example shows how toocreate a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="5193f-138">Pokud používáte k jinému zprostředkovateli a různá nastavení, dosaďte tyto informace při zkontrolujte vaši žádost.</span><span class="sxs-lookup"><span data-stu-id="5193f-138">If you're using a different provider and different settings, substitute that information when you make your request.</span></span> 

<span data-ttu-id="5193f-139">Ujistěte se, že jste zadali správné úrovně SKU hello a řada SKU:</span><span class="sxs-lookup"><span data-stu-id="5193f-139">Make sure that you specify hello correct SKU tier and SKU family:</span></span>

* <span data-ttu-id="5193f-140">Úroveň SKU Určuje, zda je povolen ExpressRoute standard nebo doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="5193f-140">SKU tier determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="5193f-141">Můžete zadat "Standard" hello tooget standardní SKU nebo 'Premium' pro doplněk premium hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-141">You can specify 'Standard' tooget hello standard SKU or 'Premium' for hello premium add-on.</span></span>
* <span data-ttu-id="5193f-142">Řada SKU Určuje typ fakturace hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-142">SKU family determines hello billing type.</span></span> <span data-ttu-id="5193f-143">Pro plán neomezená data na úrovni můžete zadat 'Metereddata' pro plán měření podle objemu dat a 'Unlimiteddata'.</span><span class="sxs-lookup"><span data-stu-id="5193f-143">You can specify 'Metereddata' for a metered data plan and 'Unlimiteddata' for an unlimited data plan.</span></span> <span data-ttu-id="5193f-144">Můžete změnit hello fakturace typu z 'Metereddata too'Unlimiteddata, ale nelze změnit typ hello z too'Metereddata 'Unlimiteddata' '.</span><span class="sxs-lookup"><span data-stu-id="5193f-144">You can change hello billing type from 'Metereddata' too'Unlimiteddata', but you can't change hello type from 'Unlimiteddata' too'Metereddata'.</span></span>


<span data-ttu-id="5193f-145">Váš okruh ExpressRoute je účtován z hello okamžiku, kdy se objeví klíč služby.</span><span class="sxs-lookup"><span data-stu-id="5193f-145">Your ExpressRoute circuit is billed from hello moment a service key is issued.</span></span> <span data-ttu-id="5193f-146">Následující ukázka Hello je žádost o nový klíč služby:</span><span class="sxs-lookup"><span data-stu-id="5193f-146">hello following example is a request for a new service key:</span></span>

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

<span data-ttu-id="5193f-147">Hello odpovědi obsahuje klíč služby hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-147">hello response contains hello service key.</span></span>

### <a name="4-list-all-expressroute-circuits"></a><span data-ttu-id="5193f-148">4. Zobrazí seznam všech okruhy ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5193f-148">4. List all ExpressRoute circuits</span></span>

<span data-ttu-id="5193f-149">tooget seznam všech hello okruhy ExpressRoute, které jste vytvořili, spusťte příkaz "az sítě express route seznam" hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-149">tooget a list of all hello ExpressRoute circuits that you created, run hello 'az network express-route list' command.</span></span> <span data-ttu-id="5193f-150">Tyto informace kdykoli můžete načíst pomocí tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="5193f-150">You can retrieve this information at any time by using this command.</span></span> <span data-ttu-id="5193f-151">toolist všechny okruhů, zkontrolujte hello volání bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="5193f-151">toolist all circuits, make hello call with no parameters.</span></span>

```azurecli
az network express-route list
```

<span data-ttu-id="5193f-152">Klíč služby, je uvedena ve hello *klíč ServiceKey* pole hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="5193f-152">Your service key is listed in hello *ServiceKey* field of hello response.</span></span>

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

<span data-ttu-id="5193f-153">Podrobný popis všech parametrů hello získáte tak, že spuštěný příkaz hello pomocí hello '-h "parametr.</span><span class="sxs-lookup"><span data-stu-id="5193f-153">You can get detailed descriptions of all hello parameters by running hello command using hello '-h' parameter.</span></span>

```azurecli
az network express-route list -h
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="5193f-154">5. Odeslání poskytovatele připojení klíče tooyour hello služby pro zřizování</span><span class="sxs-lookup"><span data-stu-id="5193f-154">5. Send hello service key tooyour connectivity provider for provisioning</span></span>

<span data-ttu-id="5193f-155">'ServiceProviderProvisioningState' poskytuje informace o hello aktuální stav zřizování na straně hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="5193f-155">'ServiceProviderProvisioningState' provides information about hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="5193f-156">Hello stavu poskytuje stav hello hello straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5193f-156">hello status provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="5193f-157">Další informace najdete v tématu hello [pracovních článku](expressroute-workflows.md#expressroute-circuit-provisioning-states).</span><span class="sxs-lookup"><span data-stu-id="5193f-157">For more information, see hello [Workflows article](expressroute-workflows.md#expressroute-circuit-provisioning-states).</span></span>

<span data-ttu-id="5193f-158">Když vytvoříte nový okruh ExpressRoute, hello okruh je ve hello následující stav:</span><span class="sxs-lookup"><span data-stu-id="5193f-158">When you create a new ExpressRoute circuit, hello circuit is in hello following state:</span></span>

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

<span data-ttu-id="5193f-159">změny toohello okruhu Hello následující stavu, pokud je zprostředkovatel připojení k hello v procesu hello povolení pro vás:</span><span class="sxs-lookup"><span data-stu-id="5193f-159">hello circuit changes toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

<span data-ttu-id="5193f-160">Pro jste toobe možné toouse okruh ExpressRoute musí být v hello následující stav:</span><span class="sxs-lookup"><span data-stu-id="5193f-160">For you toobe able toouse an ExpressRoute circuit, it must be in hello following state:</span></span>

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="5193f-161">6. Pravidelně kontrolovat hello stav a stav hello hello okruh klíče</span><span class="sxs-lookup"><span data-stu-id="5193f-161">6. Periodically check hello status and hello state of hello circuit key</span></span>

<span data-ttu-id="5193f-162">Kontrola stavu hello a stavu hello hello okruh klíče umožňuje vědět, pokud se váš poskytovatel povolil váš okruh.</span><span class="sxs-lookup"><span data-stu-id="5193f-162">Checking hello status and hello state of hello circuit key lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="5193f-163">Po nakonfiguroval hello okruh, se zobrazí 'ServiceProviderProvisioningState' jako 'Zajištěno', jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="5193f-163">After hello circuit has been configured, 'ServiceProviderProvisioningState' appears as 'Provisioned', as shown in hello following example:</span></span>

```azurecli
az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
```

<span data-ttu-id="5193f-164">odpověď Hello je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="5193f-164">hello response is similar toohello following example:</span></span>

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

### <a name="7-create-your-routing-configuration"></a><span data-ttu-id="5193f-165">7. Vytvořte vlastní konfiguraci směrování</span><span class="sxs-lookup"><span data-stu-id="5193f-165">7. Create your routing configuration</span></span>

<span data-ttu-id="5193f-166">Podrobné pokyny najdete v tématu hello [konfigurace směrování pro okruh ExpressRoute](howto-routing-cli.md) článek toocreate a upravit partnerských vztahů pro okruh.</span><span class="sxs-lookup"><span data-stu-id="5193f-166">For step-by-step instructions, see hello [ExpressRoute circuit routing configuration](howto-routing-cli.md) article toocreate and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5193f-167">Tyto pokyny platí pouze toocircuits, které jsou vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby.</span><span class="sxs-lookup"><span data-stu-id="5193f-167">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="5193f-168">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení konfiguruje a spravuje směrování.</span><span class="sxs-lookup"><span data-stu-id="5193f-168">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider configures and manages routing for you.</span></span>
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="5193f-169">8. Propojení virtuální sítě tooan okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5193f-169">8. Link a virtual network tooan ExpressRoute circuit</span></span>

<span data-ttu-id="5193f-170">V dalším kroku propojte virtuální sítě tooyour okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5193f-170">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="5193f-171">Použití hello [propojení virtuální sítě okruhů tooExpressRoute](howto-linkvnet-cli.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5193f-171">Use hello [Linking virtual networks tooExpressRoute circuits](howto-linkvnet-cli.md) article.</span></span>

## <span data-ttu-id="5193f-172"><a name="modify"></a>Úprava okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5193f-172"><a name="modify"></a>Modifying an ExpressRoute circuit</span></span>

<span data-ttu-id="5193f-173">Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5193f-173">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span> <span data-ttu-id="5193f-174">Můžete provést následující změny bez výpadků hello:</span><span class="sxs-lookup"><span data-stu-id="5193f-174">You can make hello following changes with no downtime:</span></span>

* <span data-ttu-id="5193f-175">Můžete povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5193f-175">You can enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="5193f-176">Zadaný na portu hello je dostupná kapacita můžete zvýšit hello šířka pásma okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5193f-176">You can increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="5193f-177">Však hello šířka pásma okruhu přechod na starší verzi není podporován.</span><span class="sxs-lookup"><span data-stu-id="5193f-177">However, downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="5193f-178">Z tooUnlimited dat – měření podle objemu dat, můžete změnit plán měření hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-178">You can change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="5193f-179">Změna však hello měření plán z tooMetered neomezená Data na úrovni, dat není podporováno.</span><span class="sxs-lookup"><span data-stu-id="5193f-179">However, changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="5193f-180">Můžete povolit nebo zakázat *povolit klasické operace*.</span><span class="sxs-lookup"><span data-stu-id="5193f-180">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="5193f-181">Další informace o omezení a omezení, najdete v části hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="5193f-181">For more information on limits and limitations, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

### <a name="tooenable-hello-expressroute-premium-add-on"></a><span data-ttu-id="5193f-182">doplněk ExpressRoute premium tooenable hello</span><span class="sxs-lookup"><span data-stu-id="5193f-182">tooenable hello ExpressRoute premium add-on</span></span>

<span data-ttu-id="5193f-183">Doplněk ExpressRoute premium hello u existujícího okruhu můžete povolit pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5193f-183">You can enable hello ExpressRoute premium add-on for your existing circuit by using hello following command:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

<span data-ttu-id="5193f-184">okruh Hello má nyní hello ExpressRoute premium rozšíření funkce povolené.</span><span class="sxs-lookup"><span data-stu-id="5193f-184">hello circuit now has hello ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="5193f-185">Můžeme začít hned, jak byl úspěšně spuštěn příkaz hello fakturace pro funkci rozšíření hello premium.</span><span class="sxs-lookup"><span data-stu-id="5193f-185">We begin billing you for hello premium add-on capability as soon as hello command has successfully run.</span></span>

### <a name="toodisable-hello-expressroute-premium-add-on"></a><span data-ttu-id="5193f-186">doplněk ExpressRoute premium toodisable hello</span><span class="sxs-lookup"><span data-stu-id="5193f-186">toodisable hello ExpressRoute premium add-on</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5193f-187">Tato operace může selhat, pokud používáte prostředky, které jsou větší než co je povolené standardní okruh hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-187">This operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

<span data-ttu-id="5193f-188">Před zakázáním hello doplněk ExpressRoute premium, pochopit hello následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="5193f-188">Before disabling hello ExpressRoute premium add-on, understand hello following criteria:</span></span>

* <span data-ttu-id="5193f-189">Předtím, než jste se downgradovat z úrovně premium toostandard, musí se ujistěte, že máte méně než 10 virtuální sítě propojené toohello okruh.</span><span class="sxs-lookup"><span data-stu-id="5193f-189">Before you downgrade from premium toostandard, you must make sure that you have fewer than 10 virtual networks linked toohello circuit.</span></span> <span data-ttu-id="5193f-190">Pokud máte více než 10, vaše žádost o aktualizaci nezdaří a budeme vám účtovat za zvýhodněné sazby.</span><span class="sxs-lookup"><span data-stu-id="5193f-190">If you have more than 10, your update request fails, and we bill you at premium rates.</span></span>
* <span data-ttu-id="5193f-191">Je nutné zrušit všechny virtuální sítě v jiných geopolitické oblasti.</span><span class="sxs-lookup"><span data-stu-id="5193f-191">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="5193f-192">Pokud nemáte propojení virtuálních sítí, vaše žádost o aktualizaci nezdaří a budeme vám účtovat za zvýhodněné sazby.</span><span class="sxs-lookup"><span data-stu-id="5193f-192">If you don't unlink all your virtual networks, your update request fails and we bill you at premium rates.</span></span>
* <span data-ttu-id="5193f-193">Směrovací tabulka musí být menší než 4 000 tras pro soukromý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="5193f-193">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="5193f-194">Pokud vaše velikost tabulky trasy je větší než 4 000 tras, zahodí se relace protokolu BGP hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-194">If your route table size is greater than 4,000 routes, hello BGP session drops.</span></span> <span data-ttu-id="5193f-195">opětovně Hello relace nebudou povolena, dokud hello počet předpon inzerovaných je menší než 4 000.</span><span class="sxs-lookup"><span data-stu-id="5193f-195">hello session won't be reenabled until hello number of advertised prefixes is below 4,000.</span></span>

<span data-ttu-id="5193f-196">Doplněk ExpressRoute premium hello u existujícího okruhu hello můžete zakázat pomocí hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="5193f-196">You can disable hello ExpressRoute premium add-on for hello existing circuit by using hello following example:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a><span data-ttu-id="5193f-197">šířku pásma okruhu ExpressRoute tooupdate hello</span><span class="sxs-lookup"><span data-stu-id="5193f-197">tooupdate hello ExpressRoute circuit bandwidth</span></span>

<span data-ttu-id="5193f-198">Možnosti šířky pásma hello podporované pro zprostředkovatele, zkontrolujte hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="5193f-198">For hello supported bandwidth options for your provider, check hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span> <span data-ttu-id="5193f-199">Můžete vybrat libovolnou velikost, která je větší než velikost hello existujícím okruhem.</span><span class="sxs-lookup"><span data-stu-id="5193f-199">You can pick any size greater than hello size of your existing circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5193f-200">Pokud je na existující port hello nedostatečné kapacity, můžete mít okruh ExpressRoute toorecreate hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-200">If there is inadequate capacity on hello existing port, you may have toorecreate hello ExpressRoute circuit.</span></span> <span data-ttu-id="5193f-201">Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat hello okruh.</span><span class="sxs-lookup"><span data-stu-id="5193f-201">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="5193f-202">Nelze zmenšit hello šířku pásma okruhu ExpressRoute bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="5193f-202">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="5193f-203">Přechod na starší verzi šířky pásma vyžaduje jste toodeprovision hello okruh ExpressRoute a pak znova nezajistíte nové okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5193f-203">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit, and then reprovision a new ExpressRoute circuit.</span></span>
>

<span data-ttu-id="5193f-204">Až se rozhodnete hello velikost, které potřebujete, použijte následující příkaz tooresize hello váš okruh:</span><span class="sxs-lookup"><span data-stu-id="5193f-204">After you decide hello size you need, use hello following command tooresize your circuit:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

<span data-ttu-id="5193f-205">Váš okruh je velikost na straně Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="5193f-205">Your circuit is sized up on hello Microsoft side.</span></span> <span data-ttu-id="5193f-206">Dále je třeba kontaktovat vaše připojení poskytovatele tooupdate konfigurace na jejich straně toomatch tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="5193f-206">Next, you must contact your connectivity provider tooupdate configurations on their side toomatch this change.</span></span> <span data-ttu-id="5193f-207">Když provedete toto oznámení, začneme fakturace můžete pro možnost hello aktualizovat šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="5193f-207">After you make this notification, we begin billing you for hello updated bandwidth option.</span></span>

### <a name="toomove-hello-sku-from-metered-toounlimited"></a><span data-ttu-id="5193f-208">toomove hello SKU z monitorovaných toounlimited</span><span class="sxs-lookup"><span data-stu-id="5193f-208">toomove hello SKU from metered toounlimited</span></span>

<span data-ttu-id="5193f-209">Hello SKU okruhu ExpressRoute můžete změnit pomocí hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="5193f-209">You can change hello SKU of an ExpressRoute circuit by using hello following example:</span></span>

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a><span data-ttu-id="5193f-210">toocontrol přístup toohello classic a Resource Manager prostředí</span><span class="sxs-lookup"><span data-stu-id="5193f-210">toocontrol access toohello classic and Resource Manager environments</span></span>

<span data-ttu-id="5193f-211">Přečtěte si pokyny hello v [okruhy ExpressRoute přesunout z modelu nasazení Resource Manager classic toohello hello](expressroute-howto-move-arm.md).</span><span class="sxs-lookup"><span data-stu-id="5193f-211">Review hello instructions in [Move ExpressRoute circuits from hello classic toohello Resource Manager deployment model](expressroute-howto-move-arm.md).</span></span>

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="5193f-212">Zrušení zřízení a odstraňování okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5193f-212">Deprovisioning and deleting an ExpressRoute circuit</span></span>

<span data-ttu-id="5193f-213">toodeprovision a odstranit okruh ExpressRoute, ujistěte se, že rozumíte hello následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="5193f-213">toodeprovision and delete an ExpressRoute circuit, make sure you understand hello following criteria:</span></span>

* <span data-ttu-id="5193f-214">Je nutné zrušit všechny virtuální sítě od hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5193f-214">You must unlink all virtual networks from hello ExpressRoute circuit.</span></span> <span data-ttu-id="5193f-215">Pokud se tato operace nezdaří, zkontrolujte, že toosee, pokud žádné virtuální sítě propojené toohello okruh.</span><span class="sxs-lookup"><span data-stu-id="5193f-215">If this operation fails, check toosee if any virtual networks are linked toohello circuit.</span></span>
* <span data-ttu-id="5193f-216">Pokud je hello ExpressRoute okruhu poskytovatele služeb Stav zřizování **zřizování** nebo **zajištěno**, na jejich straně, musíte pracovat se váš okruh hello toodeprovision zprostředkovatele služby.</span><span class="sxs-lookup"><span data-stu-id="5193f-216">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned**, you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="5193f-217">Pokračovat tooreserve prostředky jsme vám účtovat, dokud poskytovatele služeb hello dokončení zrušení zřízení hello okruhu a upozorní nás.</span><span class="sxs-lookup"><span data-stu-id="5193f-217">We continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="5193f-218">Hello okruhu můžete odstranit, pokud má poskytovatel služeb hello zrušit hello okruh.</span><span class="sxs-lookup"><span data-stu-id="5193f-218">You can delete hello circuit if hello service provider has deprovisioned hello circuit.</span></span> <span data-ttu-id="5193f-219">Když je zrušit okruh, poskytovatele služeb hello Stav zřizování se nastaví příliš**není zajišťováno**.</span><span class="sxs-lookup"><span data-stu-id="5193f-219">When a circuit is deprovisioned, hello service provider provisioning state is set too**Not provisioned**.</span></span> <span data-ttu-id="5193f-220">To zastaví fakturace hello okruh.</span><span class="sxs-lookup"><span data-stu-id="5193f-220">This stops billing for hello circuit.</span></span>

<span data-ttu-id="5193f-221">Váš okruh ExpressRoute můžete odstranit spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5193f-221">You can delete your ExpressRoute circuit by running hello following command:</span></span>

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="5193f-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5193f-222">Next steps</span></span>

<span data-ttu-id="5193f-223">Po vytvoření okruhu, ujistěte se, zda text hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="5193f-223">After you create your circuit, make sure that you do hello following tasks:</span></span>

* [<span data-ttu-id="5193f-224">Vytvoření a úprava směrování pro okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5193f-224">Create and modify routing for your ExpressRoute circuit</span></span>](howto-routing-cli.md)
* [<span data-ttu-id="5193f-225">Propojení vaší virtuální sítě tooyour okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5193f-225">Link your virtual network tooyour ExpressRoute circuit</span></span>](howto-linkvnet-cli.md)
