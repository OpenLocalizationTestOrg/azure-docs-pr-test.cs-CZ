---
title: "aaaAutoscale sady škálování virtuálního počítače Windows | Microsoft Docs"
description: "Nastavení automatického škálování pro Windows sadu škálování virtuálního počítače pomocí Azure PowerShell"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 67cf1c5063ceba4fc076dc270090defdbddbcfe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automatické škálování počítače ve škálovací sadě virtuálních počítačů
Sady škálování virtuálního počítače můžete snadno můžete toodeploy a spravovat identickými virtuálními počítači jako sada. Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření. Další informace o sadách škálování najdete v tématu [sadách škálování virtuálního počítače](virtual-machine-scale-sets-overview.md).

Tento kurz ukazuje, jak nastavit toocreate škálovací sadu virtuálních počítačích s Windows a automaticky škálování hello počítačů v hello. Můžete vytvořit hello škálování a vytvořit škálování vytvořením šablonu Azure Resource Manager a nasazení pomocí prostředí Azure PowerShell. Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md). toolearn Další informace o automatické škálování sad škálování, najdete v části [automatické škálování a sadách škálování virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).

V tomto článku, můžete nasadit následující hello prostředků a rozšíření:

* Microsoft.Storage/storageAccounts.
* Microsoft.Network/virtualNetworks
* Microsoft.Network/publicIPAddresses
* Microsoft.Network/loadBalancers
* Microsoft.Network/networkInterfaces
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.Insights.VMDiagnosticsSettings
* Microsoft.Insights/autoscaleSettings

Další informace o prostředky Resource Manageru najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="step-1-install-azure-powershell"></a>Krok1: Nainstalování prostředí Azure PowerShell
V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooAzure.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Krok 2: Vytvoření skupiny prostředků a účet úložiště
1. **Vytvořte skupinu prostředků** – všechny prostředky musí být nasazené tooa skupinu prostředků. Použití [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) toocreate skupinu prostředků s názvem **vmsstestrg1**.
2. **Vytvoření účtu úložiště** – tento účet úložiště se uloží hello šablony. Použití [nový AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) toocreate s názvem účtu úložiště **vmsstestsa**.

## <a name="step-3-create-hello-template"></a>Krok 3: Vytvoření šablony hello
Šablonu Azure Resource Manager umožňuje vám toodeploy a spravovat prostředky Azure společně s použitím popis JSON hello prostředků a parametry přidružené nasazení.

1. Ve svém oblíbeném editoru vytvořte soubor hello C:\VMSSTemplate.json a přidejte hello počáteční JSON struktura toosupport hello šablony.

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. Parametry nejsou vždy vyžaduje, ale poskytují způsob, jak tooinput hodnoty při nasazení šablony hello. Přidejte tyto parametry v části hello parametry nadřazený element, zda jste přidali toohello šablony.

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * Název hello samostatný virtuální počítač, který je použité tooaccess hello počítače ve škálovací sadě hello.
    * Hello název účtu úložiště hello se uloží hello šablony.
    * Hello počet virtuálních počítačů tooinitially vytvořit v sadě škálování hello.
    * Hello jméno a heslo účtu správce hello hello virtuálními počítači.
    * Předpona názvu pro hello prostředky, které jsou vytvořené toosupport hello škálování nastavit.

3. Proměnné lze použít v hodnotách toospecify šablony, které se může často mění, nebo hodnoty, které je třeba toobe vytvořena z kombinace hodnot parametrů. Přidejte tyto proměnné v části hello proměnné nadřazený element, zda jste přidali toohello šablony.

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
      "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```
   
    * Názvy DNS, které jsou používány hello síťových rozhraní.

        * Hello IP adresy názvů a předpony pro hello virtuální sítě a podsítě.
        * Hello názvy a identifikátory hello virtuální sítě, zatížení vyrovnávání a síťových rozhraní.
        * Názvy účtů úložiště pro hello účty přidružené k hello počítače ve škálovací sadě hello.
        * Nastavení pro hello rozšíření diagnostiky, který je nainstalován na hello virtuálních počítačů. Další informace o hello rozšíření diagnostiky najdete v tématu [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

4. Přidáte prostředek účet úložiště hello pod hello prostředky nadřazeného elementu, zda jste přidali toohello šablony. Tato šablona používá hello toocreate smyčky doporučená pět účty úložiště, kde jsou uloženy hello disky operačního systému a diagnostických dat. Tuto sadu účtů může podporovat až too100 virtuální počítače ve škálovací sadě, což je maximální aktuální hello. Každý účet úložiště je s názvem se písmeno označení, která byla definována v kombinaci s hello předponu, která zadáte v hello parametry šablony hello proměnné hello.

    ```json
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
      "apiVersion": "2015-06-15",
      "copy": {
        "name": "storageLoop",
        "count": 5
      },
      "location": "[resourceGroup().location]",
      "properties": { "accountType": "Standard_LRS" }
    },
    ```

5. Přidáte prostředek hello virtuální sítě. Další informace najdete v tématu [poskytovatele síťových prostředků](../virtual-network/resource-groups-networking.md).

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. Přidáte hello veřejnou IP adresu prostředky, které jsou používány hello zatížení vyrovnávání a síťové rozhraní.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. Přidejte hello prostředek pro vyrovnávání zatížení, který se používá škálovací sada hello. Další informace najdete v tématu [podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-arm.md).

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 3389
            }
          }
        ]
      }
    },
    ```

8. Přidejte hello síťového rozhraní prostředku, který je používán hello samostatný virtuální počítač. Protože počítače ve škálovací sadě nejsou přístupné přes veřejnou IP adresu, samostatný virtuální počítač se vytvoří v hello stejné virtuální sítě počítače hello tooremotely přístup.

    ```json  
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. Přidejte hello samostatný virtuální počítač do stejné sítě jako hello škálovací sadu hello.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2012-R2-Datacenter",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"        
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. Přidat sady škálování virtuálního počítače hello prostředků a zadejte hello rozšíření diagnostiky, který je nainstalován na všechny virtuální počítače ve škálovací sadě hello. Mnoho hello nastavení pro tento prostředek je podobný s hello prostředek virtuálního počítače. Hlavní rozdíly Hello jsou hello kapacity element, který určuje hello počet virtuálních počítačů v sadě škálování hello a upgradePolicy, která určuje, jak jsou provedeny aktualizace toovirtual počítače. Hello škálovací sadu vytvořen až všechny účty úložiště hello jsou vytvořeny jako zadaný hello dependsOn element.

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Insights.VMDiagnosticsSettings",
                "properties": {
                  "publisher": "Microsoft.Azure.Diagnostics",
                  "type": "IaaSDiagnostics",
                  "typeHandlerVersion": "1.5",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint": "https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. Přidejte hello autoscaleSettings prostředek, který definuje, jak upraví podle využití procesoru hello na počítačích hello v sadě škálování hello hello škálovací sadu.

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor(_Total)\\% Processor Time",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```

    V tomto kurzu jsou důležité tyto hodnoty:
    
    * **metricName**  
    Tato hodnota je hello stejné jako hello čítačů výkonu, které jsme definovali hello wadperfcounter proměnné. Pomocí této proměnné, hello rozšíření diagnostiky shromažďuje hello **procesor(_celkem)\% času procesoru** čítače.
    
    * **metricResourceUri**  
    Tato hodnota je identifikátor prostředku hello škálovací sadu virtuálních počítačů hello.
    
    * **časovými úseky**  
    Tato hodnota je hello členitost hello metriky, které jsou shromážděny. V této šabloně je nastavit tooone minutu.
    
    * **statistiky**  
    Tato hodnota určuje, jak hello metriky jsou kombinované tooaccommodate hello automatického škálování akce. Hello možné hodnoty jsou: průměr, minimum, maximum. V této šabloně se shromažďují hello průměrné celkové využití procesoru hello virtuálních počítačů.
    
    * **Hodnota timeWindow**  
    Tato hodnota je rozsah hello času, ve kterém se shromažďují instance data. Musí být mezi 5 minutami a 12 hodin.
    
    * **Agregace času**  
    jeho hodnota určuje, jak by měla být kombinovány hello data, která se shromažďují v čase. Hello výchozí hodnota je průměr. Hello možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet.
    
    * **operátor**  
    Tato hodnota je hello operátor, který je použité toocompare hello metriky dat a hello prahovou hodnotu. Hello možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    
    * **Prahová hodnota**  
    Tato hodnota je hello hodnotu, která aktivuje hello akce škálování. V této šabloně se přidávají počítače sad po více než 50 % hello průměrné využití procesoru mezi počítači v sadě hello toohello škálování.
    
    * **směr**  
    Tato hodnota určuje hello akce, která se provede, když je dosaženo hello prahovou hodnotu. Hello možné hodnoty jsou zvýšení nebo snížení. V této šabloně hello počet virtuálních počítačů v sadě škálování hello se zvyšuje, když prahové hodnoty hello je více než 50 % hello definované časové okno.
    
    * **Typ**  
    Tato hodnota je hello typ akce, který má vzniknout a musí být nastaven tooChangeCount.
    
    * **Hodnota**  
    Tato hodnota je hello počet virtuálních počítačů, které jsou přidány nebo odebrány z hello škálovací sadu. Tato hodnota musí být 1 nebo vyšší. Hello výchozí hodnota je 1. V této šabloně hello počet počítačů v hello škálování nastavit zvýší o 1 při splnění hello prahovou hodnotu.
    
    * **cooldown**  
    Tato hodnota je hello množství toowait času od poslední akce škálování hello předtím, než dojde k hello další akce. Tato hodnota musí být mezi minutu a jeden týden.

12. Uložte soubor šablony hello.    

## <a name="step-4-upload-hello-template-toostorage"></a>Krok 4: Nahrání toostorage šablony hello
lze uložit šablonu Hello tak dlouho, dokud znáte název hello a primární klíč účtu úložiště hello, kterou jste vytvořili v kroku 1.

1. V okně prostředí Microsoft Azure PowerShell hello nastavte proměnné, která určuje název hello hello účtu úložiště, který jste vytvořili v kroku 1.
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. Nastavte proměnné, která určuje hello primární klíč účtu úložiště hello.
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   Tento klíč můžete získat kliknutím na ikonu klíče hello při zobrazení prostředků účtu úložiště hello hello portálu Azure.
3. Vytvořte hello úložiště účet kontext objekt, který je použité toovalidate operací s účtem úložiště hello.
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. Vytvoření kontejneru hello uložení šablony hello.

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. Nahrajte hello šablony souboru toohello nový kontejner.

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-hello-template"></a>Krok 5: Nasazení šablony hello
Teď, když jste vytvořili hello šablony, můžete začít nasazovat hello prostředky. Použijte tento příkaz toostart hello proces:

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

Po stisknutí klávesy zadejte, jsou výzvami tooprovide hodnoty pro proměnné hello, které jste přiřadili. Zadejte tyto hodnoty:

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

Pro všechny prostředky toosuccessfully hello nasadit má trvat přibližně 15 minut.

> [!NOTE]
> Můžete také použít portál hello možnost toodeploy hello prostředky. Použít tento odkaz: "https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>"
> 
> 

## <a name="step-6-monitor-resources"></a>Krok 6: Sledování prostředků
Můžete získat některé informace o sady škálování virtuálního počítače pomocí těchto metod:

* Hello portál Azure – nyní můžete získat omezené množství informací pomocí portálu hello.
* Hello [Průzkumníka prostředků Azure](https://resources.azure.com/) – tento nástroj je nejlepší pro zkoumání hello aktuální stav škálovací sady hello. Postupujte podle této cestě a měli byste vidět sadu zobrazení instance hello hello měřítka, kterou jste vytvořili:
  
    odběry > {vaše předplatné} > Skupinyprostředků > vmsstestrg1 > poskytovatelé > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtuálních počítačů

* Prostředí Azure PowerShell - použít tento příkaz tooget některé informace:

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  Nebo
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* Připojte toohello samostatný virtuální počítač stejně, jako by všechny ostatní počítače a pak můžete vzdáleně přistupovat hello virtuálních počítačů v hello škálování sadu toomonitor jednotlivých procesů.

> [!NOTE]
> Dokončení rozhraní REST API k získání informací o sady škálování najdete v [sadách škálování virtuálního počítače](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-hello-resources"></a>Krok 7: Odebrání hello prostředky
Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, je vždy prostředky toodelete osvědčených postupů, které už nejsou potřeba. Nepotřebujete toodelete každého prostředku nezávisle na skupinu prostředků. Odstraněním skupiny prostředků hello a všechny její prostředky se automaticky odstraní.

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

Pokud chcete tookeep vaší skupiny prostředků, můžete odstranit pouze sad škálování hello.

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a>Další kroky
* Spravovat hello sadě škálování, který jste právě vytvořili pomocí hello informace v [spravovat virtuální počítače ve Škálovací sadě virtuálního počítače](virtual-machine-scale-sets-windows-manage.md).
* Další informace o vertikálním škálování najdete v tématu [Vertikální automatické škálování se škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-vertical-scale-reprovision.md).
* Najít příklady Azure monitorování monitorování funkcí v [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md)
* Další informace o funkcích oznámení v [použití automatického škálování akce toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
* Zjistěte, jak příliš[protokoly auditu použití toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)

