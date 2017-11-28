---
title: "aaaConfigure sítě režimy služby Azure Service Fabric kontejneru | Microsoft Docs"
description: "Zjistěte, jak toosetup hello různé sítě režimy této Azure Service Fabric podporuje."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 5c5dd4c590c7698a947503cbe8ef66ff7d6b503a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-container-networking-modes"></a><span data-ttu-id="5d338-103">Režimy sítě kontejneru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="5d338-103">Service Fabric container networking modes</span></span>

<span data-ttu-id="5d338-104">Hello výchozí sítě režim nenabízí hello Service Fabric clusteru pro kontejner služby je hello `nat` sítě režimu.</span><span class="sxs-lookup"><span data-stu-id="5d338-104">hello default networking mode offered in hello Service Fabric cluster for container services is hello `nat` networking mode.</span></span> <span data-ttu-id="5d338-105">S hello `nat` sítě režimu s více než jeden kontejnery služby naslouchání toohello stejný port má za následek chyby nasazení.</span><span class="sxs-lookup"><span data-stu-id="5d338-105">With hello `nat` networking mode, having more than one containers service listening toohello same port results in deployment errors.</span></span> <span data-ttu-id="5d338-106">Pro spuštění několika služeb, které naslouchat na stejný port, Service Fabric podporuje hello hello `open` sítě režimu (verze 5.7 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="5d338-106">For running several services that listen on hello same port, Service Fabric supports hello `open` networking mode (version 5.7 or higher).</span></span> <span data-ttu-id="5d338-107">S hello `open` sítě režimu, každou službu kontejneru získá dynamicky přiřazenou IP adresu interně povolení více služeb toolisten toohello stejný port.</span><span class="sxs-lookup"><span data-stu-id="5d338-107">With hello `open` networking mode, each container service gets a dynamically assigned IP address internally allowing multiple services toolisten toohello same port.</span></span>   

<span data-ttu-id="5d338-108">Proto s jednoho typu služby s koncový bod statické definované v hello service manifest, nové služby může vytvořit a odstraněn bez chyby nasazení pomocí hello `open` sítě režimu.</span><span class="sxs-lookup"><span data-stu-id="5d338-108">Thus, with a single service type with a static endpoint defined in hello service manifest, new services may be created and deleted without deployment errors using hello `open` networking mode.</span></span> <span data-ttu-id="5d338-109">Podobně můžete použít jednu hello stejné `docker-compose.yml` souboru s mapováním statický port pro vytváření více služeb.</span><span class="sxs-lookup"><span data-stu-id="5d338-109">Similarly, one can use hello same `docker-compose.yml` file with static port mappings for creating multiple services.</span></span>

<span data-ttu-id="5d338-110">Pomocí hello dynamicky přiřadit IP toodiscover služby není vhodné od změny IP adresy hello při restartování služby hello nebo přesune tooanother uzlu.</span><span class="sxs-lookup"><span data-stu-id="5d338-110">Using hello dynamically assigned IP toodiscover services is not advisable since hello IP address changes when hello service restarts or moves tooanother node.</span></span> <span data-ttu-id="5d338-111">Používat pouze hello **Service Fabric Naming Service** nebo hello **služba DNS** pro zjišťování služby.</span><span class="sxs-lookup"><span data-stu-id="5d338-111">Only use hello **Service Fabric Naming Service**  or hello **DNS Service** for service discovery.</span></span> 


> [!WARNING]
> <span data-ttu-id="5d338-112">Na virtuální sítě v Azure jsou povoleny pouze celkem 4096 IP adresy.</span><span class="sxs-lookup"><span data-stu-id="5d338-112">Only a total of 4096 IPs are allowed per vNET in Azure.</span></span> <span data-ttu-id="5d338-113">Proto hello součet hello počet uzlů a hello počet instancí kontejneru služby (s `open` sítě) nesmí být delší než 4096 v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="5d338-113">Thus, hello sum of hello number of nodes and hello number of container service instances (with `open` networking) cannot exceed 4096 within a vNET.</span></span> <span data-ttu-id="5d338-114">U scénářů s vysokou hustotou hello `nat` se doporučuje sítě režimu.</span><span class="sxs-lookup"><span data-stu-id="5d338-114">For such high-density scenarios, hello `nat` networking mode is recommended.</span></span>
>

## <a name="setting-up-open-networking-mode"></a><span data-ttu-id="5d338-115">Nastavení sítě režim otevření</span><span class="sxs-lookup"><span data-stu-id="5d338-115">Setting up open networking mode</span></span>

1. <span data-ttu-id="5d338-116">Nastavit šablony Azure Resource Manageru hello povolením služby DNS a hello Zprostředkovatel IP v části `fabricSettings`.</span><span class="sxs-lookup"><span data-stu-id="5d338-116">Set up hello Azure Resource Manager template by enabling DNS Service and hello IP Provider under `fabricSettings`.</span></span> 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name":  "Trace/Etw", 
                    "parameters": [
                    {
                            "name": "Level",
                            "value": "5"
                    }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```

2. <span data-ttu-id="5d338-117">Nastavte hello síťový profil části tooallow více IP adres toobe nakonfigurované na každém uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5d338-117">Set up hello network profile section tooallow multiple IP addresses toobe configured on each node of hello cluster.</span></span> <span data-ttu-id="5d338-118">Hello následující příklad nastavuje pět IP adresy na jeden uzel (proto vám může mít pět instancí služby naslouchání portu toohello na každém uzlu) pro cluster s podporou Windows Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5d338-118">hello following example sets up five IP addresses per node (thus you can have five service instances listening toohello port on each node) for a Windows Service Fabric cluster.</span></span>

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
    ```

    <span data-ttu-id="5d338-119">Pro Linux clusterů se přidá další konfiguraci veřejných IP adres tooallow odchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="5d338-119">For Linux clusters, an additional public IP configuration is added tooallow outbound connectivity.</span></span> <span data-ttu-id="5d338-120">Hello následující fragment kódu nastavuje pět IP adresy na uzel pro cluster s podporou Linux:</span><span class="sxs-lookup"><span data-stu-id="5d338-120">hello following snippet sets up five IP addresses per node for a Linux cluster:</span></span>

    ```json
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "publicipaddressconfiguration": {
                              "name": "devpub",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 1)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 2)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 3)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 4)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 5)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
    ```

3. <span data-ttu-id="5d338-121">Pro systém Windows, které clustery, nastavit skupiny NSG pravidlo otevření až port UDP/53 pro virtuální síť hello s hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5d338-121">For Windows clusters only, set up an NSG rule opening up port UDP/53 for hello vNET with hello following values:</span></span>

   | <span data-ttu-id="5d338-122">Priorita</span><span class="sxs-lookup"><span data-stu-id="5d338-122">Priority</span></span> |    <span data-ttu-id="5d338-123">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5d338-123">Name</span></span>    |    <span data-ttu-id="5d338-124">Zdroj</span><span class="sxs-lookup"><span data-stu-id="5d338-124">Source</span></span>      |  <span data-ttu-id="5d338-125">Cíl</span><span class="sxs-lookup"><span data-stu-id="5d338-125">Destination</span></span>   |   <span data-ttu-id="5d338-126">Služba</span><span class="sxs-lookup"><span data-stu-id="5d338-126">Service</span></span>    | <span data-ttu-id="5d338-127">Akce</span><span class="sxs-lookup"><span data-stu-id="5d338-127">Action</span></span> |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     <span data-ttu-id="5d338-128">2000</span><span class="sxs-lookup"><span data-stu-id="5d338-128">2000</span></span> | <span data-ttu-id="5d338-129">Custom_Dns</span><span class="sxs-lookup"><span data-stu-id="5d338-129">Custom_Dns</span></span> | <span data-ttu-id="5d338-130">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="5d338-130">VirtualNetwork</span></span> | <span data-ttu-id="5d338-131">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="5d338-131">VirtualNetwork</span></span> | <span data-ttu-id="5d338-132">DNS (UDP/53)</span><span class="sxs-lookup"><span data-stu-id="5d338-132">DNS (UDP/53)</span></span> | <span data-ttu-id="5d338-133">Povolit</span><span class="sxs-lookup"><span data-stu-id="5d338-133">Allow</span></span>  |


4. <span data-ttu-id="5d338-134">Zadejte režim sítě hello v manifestu aplikace hello u každé služby `<NetworkConfig NetworkType="open">`.</span><span class="sxs-lookup"><span data-stu-id="5d338-134">Specify hello networking mode in hello app manifest for each service `<NetworkConfig NetworkType="open">`.</span></span>  <span data-ttu-id="5d338-135">režim Hello `open` výsledkem hello služby získávání vyhrazenou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="5d338-135">hello mode `open` results in hello service getting a dedicated IP address.</span></span> <span data-ttu-id="5d338-136">Pokud není zadán režim, je standardně toohello základní `nat` režimu.</span><span class="sxs-lookup"><span data-stu-id="5d338-136">If a mode isn't specified, it defaults toohello basic `nat` mode.</span></span> <span data-ttu-id="5d338-137">Proto v hello následující manifestu ukázka `NodeContainerServicePackage1` a `NodeContainerServicePackage2` můžete každý naslouchání toohello stejný port (obě služby jsou naslouchá na `Endpoint1`).</span><span class="sxs-lookup"><span data-stu-id="5d338-137">Thus, in hello following manifest example, `NodeContainerServicePackage1` and `NodeContainerServicePackage2` can each listen toohello same port (both services are listening on `Endpoint1`).</span></span>

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="open"/>
           <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="open"/>
            <PortBinding ContainerPort="8910" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```
<span data-ttu-id="5d338-138">Můžete kombinovat a párovat síťové různé režimy ve službách v rámci aplikace pro Windows cluster.</span><span class="sxs-lookup"><span data-stu-id="5d338-138">You can mix and match different networking modes across services within an application for a Windows cluster.</span></span> <span data-ttu-id="5d338-139">Proto může mít některé služby `open` režimu a některé na `nat` sítě režimu.</span><span class="sxs-lookup"><span data-stu-id="5d338-139">Thus, you can have some services on `open` mode and some on `nat` networking mode.</span></span> <span data-ttu-id="5d338-140">Když je nakonfigurované služby `nat`, port hello je naslouchání toomust být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="5d338-140">When a service is configured with `nat`, hello port it is listening toomust be unique.</span></span> <span data-ttu-id="5d338-141">Kombinování sítě režimy pro různé služby se nepodporuje v clusterech Linux.</span><span class="sxs-lookup"><span data-stu-id="5d338-141">Mixing networking modes for different services isn't supported on Linux clusters.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="5d338-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d338-142">Next steps</span></span>
<span data-ttu-id="5d338-143">V tomto článku jste se dozvěděli o sítě režimy, které nabízí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5d338-143">In this article, you learned about networking modes offered by Service Fabric.</span></span>  

* [<span data-ttu-id="5d338-144">Model aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="5d338-144">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="5d338-145">Prostředky manifestu služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="5d338-145">Service Fabric service manifest resources</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="5d338-146">Nasazení tooService kontejneru Windows Fabric na Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="5d338-146">Deploy a Windows container tooService Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="5d338-147">Nasazení tooService kontejner Docker prostředků infrastruktury v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5d338-147">Deploy a Docker container tooService Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
