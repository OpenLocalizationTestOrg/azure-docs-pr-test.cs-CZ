---
title: "Konfigurace sítě režimy služby Azure Service Fabric kontejneru | Microsoft Docs"
description: "Zjistěte, jak nastavit různé režimy sítě, které podporuje Azure Service Fabric."
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
ms.openlocfilehash: f792f9604a5d6e62551ed92c1049d6e2b4216417
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="service-fabric-container-networking-modes"></a><span data-ttu-id="3fe7c-103">Režimy sítě kontejneru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3fe7c-103">Service Fabric container networking modes</span></span>

<span data-ttu-id="3fe7c-104">Je výchozí sítě režimu nenabízí cluster Service Fabric pro služby kontejneru `nat` sítě režimu.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-104">The default networking mode offered in the Service Fabric cluster for container services is the `nat` networking mode.</span></span> <span data-ttu-id="3fe7c-105">Pomocí `nat` sítě režimu s více než jedna služba kontejnery naslouchání stejný port výsledkem chyby nasazení.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-105">With the `nat` networking mode, having more than one containers service listening to the same port results in deployment errors.</span></span> <span data-ttu-id="3fe7c-106">Spuštění několik služeb této naslouchání na stejném portu, Service Fabric podporuje `open` sítě režimu (verze 5.7 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="3fe7c-106">For running several services that listen on the same port, Service Fabric supports the `open` networking mode (version 5.7 or higher).</span></span> <span data-ttu-id="3fe7c-107">Pomocí `open` sítě režimu, každou službu kontejneru získá dynamicky přiřazenou IP adresu interně povolení více služeb tak, aby naslouchala na stejný port.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-107">With the `open` networking mode, each container service gets a dynamically assigned IP address internally allowing multiple services to listen to the same port.</span></span>   

<span data-ttu-id="3fe7c-108">Proto s jednoho typu služby s koncový bod statické definované v service manifest, nové služby může vytvořit a odstraněn bez chyby nasazení pomocí `open` sítě režimu.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-108">Thus, with a single service type with a static endpoint defined in the service manifest, new services may be created and deleted without deployment errors using the `open` networking mode.</span></span> <span data-ttu-id="3fe7c-109">Podobně stejné můžete použít jednu `docker-compose.yml` souboru s mapováním statický port pro vytváření více služeb.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-109">Similarly, one can use the same `docker-compose.yml` file with static port mappings for creating multiple services.</span></span>

<span data-ttu-id="3fe7c-110">Použít dynamicky přiřazené IP ke zjištění služby není vhodné od změny IP adresy při restartování služby nebo přesune do jiného uzlu.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-110">Using the dynamically assigned IP to discover services is not advisable since the IP address changes when the service restarts or moves to another node.</span></span> <span data-ttu-id="3fe7c-111">Použít pouze **Service Fabric Naming Service** nebo **služba DNS** pro zjišťování služby.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-111">Only use the **Service Fabric Naming Service**  or the **DNS Service** for service discovery.</span></span> 


> [!WARNING]
> <span data-ttu-id="3fe7c-112">Na virtuální sítě v Azure jsou povoleny pouze celkem 4096 IP adresy.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-112">Only a total of 4096 IPs are allowed per vNET in Azure.</span></span> <span data-ttu-id="3fe7c-113">Proto součet počet uzlů a počet instancí kontejneru služby (s `open` sítě) nesmí být delší než 4096 v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-113">Thus, the sum of the number of nodes and the number of container service instances (with `open` networking) cannot exceed 4096 within a vNET.</span></span> <span data-ttu-id="3fe7c-114">U scénářů s vysokou hustotou `nat` se doporučuje sítě režimu.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-114">For such high-density scenarios, the `nat` networking mode is recommended.</span></span>
>

## <a name="setting-up-open-networking-mode"></a><span data-ttu-id="3fe7c-115">Nastavení sítě režim otevření</span><span class="sxs-lookup"><span data-stu-id="3fe7c-115">Setting up open networking mode</span></span>

1. <span data-ttu-id="3fe7c-116">Nastavení šablony Azure Resource Manageru povolením služby DNS a IP zprostředkovatele v rámci `fabricSettings`.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-116">Set up the Azure Resource Manager template by enabling DNS Service and the IP Provider under `fabricSettings`.</span></span> 

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

2. <span data-ttu-id="3fe7c-117">Nastavte části profil sítě a povolit více IP adres nakonfigurovat na každém uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-117">Set up the network profile section to allow multiple IP addresses to be configured on each node of the cluster.</span></span> <span data-ttu-id="3fe7c-118">Následující příklad nastaví pět IP adres na uzel (proto vám může mít pět instancí služby naslouchání na portu na každém uzlu) pro cluster s podporou Windows Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-118">The following example sets up five IP addresses per node (thus you can have five service instances listening to the port on each node) for a Windows Service Fabric cluster.</span></span>

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

    <span data-ttu-id="3fe7c-119">Pro Linux clusterů se přidá další konfiguraci veřejných IP adres umožňuje odchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-119">For Linux clusters, an additional public IP configuration is added to allow outbound connectivity.</span></span> <span data-ttu-id="3fe7c-120">Následující fragment kódu nastaví pět IP adres na uzel pro cluster s podporou Linux:</span><span class="sxs-lookup"><span data-stu-id="3fe7c-120">The following snippet sets up five IP addresses per node for a Linux cluster:</span></span>

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

3. <span data-ttu-id="3fe7c-121">Pro clustery Windows nastavte pravidlo NSG otevření portů UDP/53 pro virtuální síť s následujícími hodnotami:</span><span class="sxs-lookup"><span data-stu-id="3fe7c-121">For Windows clusters only, set up an NSG rule opening up port UDP/53 for the vNET with the following values:</span></span>

   | <span data-ttu-id="3fe7c-122">Priorita</span><span class="sxs-lookup"><span data-stu-id="3fe7c-122">Priority</span></span> |    <span data-ttu-id="3fe7c-123">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="3fe7c-123">Name</span></span>    |    <span data-ttu-id="3fe7c-124">Zdroj</span><span class="sxs-lookup"><span data-stu-id="3fe7c-124">Source</span></span>      |  <span data-ttu-id="3fe7c-125">Cíl</span><span class="sxs-lookup"><span data-stu-id="3fe7c-125">Destination</span></span>   |   <span data-ttu-id="3fe7c-126">Služba</span><span class="sxs-lookup"><span data-stu-id="3fe7c-126">Service</span></span>    | <span data-ttu-id="3fe7c-127">Akce</span><span class="sxs-lookup"><span data-stu-id="3fe7c-127">Action</span></span> |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     <span data-ttu-id="3fe7c-128">2000</span><span class="sxs-lookup"><span data-stu-id="3fe7c-128">2000</span></span> | <span data-ttu-id="3fe7c-129">Custom_Dns</span><span class="sxs-lookup"><span data-stu-id="3fe7c-129">Custom_Dns</span></span> | <span data-ttu-id="3fe7c-130">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="3fe7c-130">VirtualNetwork</span></span> | <span data-ttu-id="3fe7c-131">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="3fe7c-131">VirtualNetwork</span></span> | <span data-ttu-id="3fe7c-132">DNS (UDP/53)</span><span class="sxs-lookup"><span data-stu-id="3fe7c-132">DNS (UDP/53)</span></span> | <span data-ttu-id="3fe7c-133">Povolit</span><span class="sxs-lookup"><span data-stu-id="3fe7c-133">Allow</span></span>  |


4. <span data-ttu-id="3fe7c-134">Zadejte režim sítě v manifest aplikace pro každou službu `<NetworkConfig NetworkType="open">`.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-134">Specify the networking mode in the app manifest for each service `<NetworkConfig NetworkType="open">`.</span></span>  <span data-ttu-id="3fe7c-135">Režim `open` výsledky v rámci služby získávání vyhrazenou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-135">The mode `open` results in the service getting a dedicated IP address.</span></span> <span data-ttu-id="3fe7c-136">Pokud není zadán režim, nastaví se jako výchozí základní `nat` režimu.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-136">If a mode isn't specified, it defaults to the basic `nat` mode.</span></span> <span data-ttu-id="3fe7c-137">Proto v následujícím příkladu manifestu `NodeContainerServicePackage1` a `NodeContainerServicePackage2` můžete každý naslouchání ke stejnému portu (obě služby jsou naslouchá na `Endpoint1`).</span><span class="sxs-lookup"><span data-stu-id="3fe7c-137">Thus, in the following manifest example, `NodeContainerServicePackage1` and `NodeContainerServicePackage2` can each listen to the same port (both services are listening on `Endpoint1`).</span></span>

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
<span data-ttu-id="3fe7c-138">Můžete kombinovat a párovat síťové různé režimy ve službách v rámci aplikace pro Windows cluster.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-138">You can mix and match different networking modes across services within an application for a Windows cluster.</span></span> <span data-ttu-id="3fe7c-139">Proto může mít některé služby `open` režimu a některé na `nat` sítě režimu.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-139">Thus, you can have some services on `open` mode and some on `nat` networking mode.</span></span> <span data-ttu-id="3fe7c-140">Když je nakonfigurované služby `nat`, naslouchá na portu musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-140">When a service is configured with `nat`, the port it is listening to must be unique.</span></span> <span data-ttu-id="3fe7c-141">Kombinování sítě režimy pro různé služby se nepodporuje v clusterech Linux.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-141">Mixing networking modes for different services isn't supported on Linux clusters.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="3fe7c-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3fe7c-142">Next steps</span></span>
<span data-ttu-id="3fe7c-143">V tomto článku jste se dozvěděli o sítě režimy, které nabízí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3fe7c-143">In this article, you learned about networking modes offered by Service Fabric.</span></span>  

* [<span data-ttu-id="3fe7c-144">Model aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3fe7c-144">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="3fe7c-145">Prostředky manifestu služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3fe7c-145">Service Fabric service manifest resources</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="3fe7c-146">Nasazení kontejneru systému Windows pro Service Fabric na Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="3fe7c-146">Deploy a Windows container to Service Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="3fe7c-147">Nasadit kontejner Docker do Service Fabric v systému Linux</span><span class="sxs-lookup"><span data-stu-id="3fe7c-147">Deploy a Docker container to Service Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
