---
title: "aaaAutoscale Škálovací sady virtuálních počítačů Linux | Microsoft Docs"
description: "Nastavení automatického škálování pro Linux sadu škálování virtuálního počítače pomocí rozhraní příkazového řádku Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 4352b94ec2973c37bc5616e3be25bd0c9442632b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a>Automatické škálování počítače se systémem Linux v škálovací sadu virtuálních počítačů
Sady škálování virtuálního počítače můžete snadno můžete toodeploy a spravovat identickými virtuálními počítači jako sada. Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření. Další, najdete v části toolearn [Přehled sady škálování virtuálních počítačů](virtual-machine-scale-sets-overview.md).

Tento kurz ukazuje, jak nastavit toocreate škálování virtuálních počítačů Linux pomocí nejnovější verze Ubuntu Linux hello. Hello kurz také ukazuje, jak nastavit tooautomatically škálování hello počítačů v hello. Můžete vytvořit hello škálování a vytvořit škálování vytvořením šablonu Azure Resource Manager a nasazení pomocí rozhraní příkazového řádku Azure. Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md). toolearn Další informace o automatické škálování sad škálování, najdete v části [automatické škálování a sadách škálování virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).

V tomto kurzu nasadíte následující hello prostředků a rozšíření:

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

Před zahájením práce s hello kroky v tomto kurzu [nainstalovat hello rozhraní příkazového řádku Azure](../cli-install-nodejs.md).

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a>Krok 1: Vytvoření skupiny prostředků a účet úložiště

1. **Přihlaste se tooMicrosoft Azure**  
Ve svém rozhraní příkazového řádku (Bash, terminálu, příkazového řádku), přepínače režimu tooResource Manager a potom [přihlásit pomocí id váš pracovní nebo školní](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Postupujte podle výzvy hello k tooyour prostředí interaktivní přihlašovací účet Azure.

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > Pokud máte pracovní nebo školní ID a není povoleno dvoufaktorové ověřování, použijte `azure login -u` s ID toolog hello v bez interaktivní relace. Pokud nemáte pracovní nebo školní ID, můžete [vytvořit pracovní nebo školní id z vašeho osobního účtu Microsoft](../active-directory/active-directory-users-create-azure-portal.md).
    
2. **Vytvořte skupinu prostředků**  
Všechny prostředky musí být nasazené tooa skupinu prostředků. V tomto kurzu, název skupiny prostředků hello **vmsstest1**.
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. **Nasazení účtu úložiště do nové skupiny prostředků hello**  
Tento účet úložiště se uloží hello šablony. Vytvořit účet úložiště s názvem **vmsstestsa**.
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-hello-template"></a>Krok 2: Vytvoření šablony hello
Šablonu Azure Resource Manager umožňuje vám toodeploy a spravovat prostředky Azure společně s použitím popis JSON hello prostředků a parametry přidružené nasazení.

1. Ve svém oblíbeném editoru vytvořte soubor hello VMSSTemplate.json a přidejte hello počáteční JSON struktura toosupport hello šablony.

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
   * Název účtu úložiště hello se uloží hello šablony.
   * Hello počet instancí virtuálních počítačů tooinitially vytvořit v sadě škálování hello.
   * Název a heslo účtu správce hello hello virtuálními počítači.
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
    "wadlogs": "<WadCfg><DiagnosticMonitorConfiguration>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor\\PercentProcessorTime\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU percentage guest OS\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```

   * Názvy DNS, které jsou používány hello síťových rozhraní.
   * Hello IP adresy názvů a předpony pro hello virtuální sítě a podsítě.
   * Hello názvy a identifikátory hello virtuální sítě, zatížení vyrovnávání a síťových rozhraní.
   * Názvy účtů úložiště pro hello účty přidružené k hello počítače ve škálovací sadě hello.
   * Nastavení pro hello rozšíření diagnostiky, který je nainstalován na hello virtuálních počítačů. Další informace o hello rozšíření diagnostiky najdete v tématu [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

4. Přidáte prostředek účet úložiště hello pod hello prostředky nadřazeného elementu, zda jste přidali toohello šablony. Tato šablona používá hello toocreate smyčky doporučená pět účty úložiště, kde jsou uloženy hello disky operačního systému a diagnostických dat. Tuto sadu účtů může podporovat až too100 virtuální počítače ve škálovací sadě, což je maximální aktuální hello. Každý účet úložiště je s názvem se písmeno označení, která byla definována v kombinaci s příponou hello, který zadáte v hello parametry šablony hello proměnné hello.
   
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
                "id": "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
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
              "backendPort": 22
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
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "14.04.4-LTS",
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
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4],'.blob.core.windows.net/vmss')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "Canonical",
              "offer": "UbuntuServer",
              "sku": "14.04.4-LTS",
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
                "name":"LinuxDiagnostic",
                "properties": {
                  "publisher":"Microsoft.OSTCExtensions",
                  "type":"LinuxDiagnostic",
                  "typeHandlerVersion":"2.1",
                  "autoUpgradeMinorVersion":false,
                  "settings": {
                    "xmlCfg":"[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount":"[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName":"[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey":"[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint":"https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. Přidejte hello autoscaleSettings prostředek, který definuje, jak upraví podle využití procesoru na počítačích hello sady hello hello škálovací sadu.

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
                  "metricName": "\\Processor\\PercentProcessorTime",
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
    Tato hodnota je hello stejné jako hello čítačů výkonu, které jsme definovali hello wadperfcounter proměnné. Pomocí této proměnné, hello rozšíření diagnostiky shromažďuje hello **Processor\PercentProcessorTime** čítače.
    
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
    Tato hodnota se aktivuje hello akce škálování. V této šabloně se přidávají počítače sad po více než 50 % hello průměrné využití procesoru mezi počítači v sadě hello toohello škálování.
    
    * **směr**  
    Tato hodnota určuje hello akce, která se provede, když je dosaženo hello prahovou hodnotu. Hello možné hodnoty jsou zvýšení nebo snížení. V této šabloně hello počet virtuálních počítačů v sadě škálování hello se zvyšuje, když prahové hodnoty hello je více než 50 % hello definované časové okno.

    * **Typ**  
    Tato hodnota je hello typ akce, který má vzniknout a musí být nastaven tooChangeCount.
    
    * **Hodnota**  
    Tato hodnota je hello počet virtuálních počítačů, které jsou přidány nebo odebrány z hello škálovací sadu. Tato hodnota musí být 1 nebo vyšší. Hello výchozí hodnota je 1. V této šabloně hello počet počítačů v hello škálování nastavit zvýší o 1 při splnění hello prahovou hodnotu.

    * **cooldown**  
    Tato hodnota je hello množství toowait času od poslední akce škálování hello předtím, než dojde k hello další akce. Tato hodnota musí být mezi minutu a jeden týden.

12. Uložte soubor šablony hello.    

## <a name="step-3-upload-hello-template-toostorage"></a>Krok 3: Nahrát na server hello šablony toostorage
lze uložit šablonu Hello tak dlouho, dokud znáte název hello a primární klíč účtu úložiště hello, kterou jste vytvořili v kroku 1.

1. Ve svém rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) spuštěním těchto příkazů proměnné prostředí hello tooset potřeby tooaccess hello účet úložiště:

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    Hello klíč můžete získat kliknutím na ikonu klíče hello při zobrazení prostředků účtu úložiště hello hello portálu Azure. Při použití příkazového řádku Windows, zadejte **nastavit** místo exportu.

2. Vytvoření kontejneru hello uložení šablony hello.
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. Nahrajte hello šablony souboru toohello nový kontejner.
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-hello-template"></a>Krok 4: Nasazení šablony hello
Teď, když jste vytvořili hello šablony, můžete začít nasazovat hello prostředky. Použijte tento příkaz toostart hello proces:

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

Po stisknutí klávesy zadejte, jsou výzvami tooprovide hodnoty pro proměnné hello, které jste přiřadili. Zadejte tyto hodnoty:

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

Pro všechny prostředky toosuccessfully hello nasadit má trvat přibližně 15 minut.

> [!NOTE]
> Můžete také použít portál hello možnost toodeploy hello prostředky. Použít tento odkaz: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>


## <a name="step-5-monitor-resources"></a>Krok 5: Sledování prostředků
Můžete získat některé informace o sady škálování virtuálního počítače pomocí těchto metod:

* Hello portál Azure – nyní můžete získat omezené množství informací pomocí portálu hello.

* Hello [Průzkumníka prostředků Azure](https://resources.azure.com/) – tento nástroj je nejlepší pro zkoumání hello aktuální stav škálovací sady hello. Postupujte podle této cestě a měli byste vidět sadu zobrazení instance hello hello měřítka, kterou jste vytvořili:
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* Rozhraní příkazového řádku Azure - použít tento příkaz tooget některé informace:

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* Připojte toohello jumpbox virtuálního počítače stejně, jako by všechny ostatní počítače a pak můžete vzdáleně přistupovat hello virtuálních počítačů v hello škálování sadu toomonitor jednotlivých procesů.

> [!NOTE]
> Dokončení rozhraní REST API k získání informací o sady škálování najdete v [sadách škálování virtuálního počítače](https://msdn.microsoft.com/library/mt589023.aspx).

## <a name="step-6-remove-hello-resources"></a>Krok 6: Odebrat prostředky hello
Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, je vždy prostředky toodelete osvědčených postupů, které už nejsou potřeba. Nepotřebujete toodelete každého prostředku nezávisle na skupinu prostředků. Odstraněním skupiny prostředků hello a všechny její prostředky se automaticky odstraní.

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a>Další kroky
* Najít příklady Azure monitorování monitorování funkcí v [Azure monitorování napříč platformami CLI rychlý start ukázky](../monitoring-and-diagnostics/insights-cli-samples.md)
* Další informace o funkcích oznámení v [použití automatického škálování akce toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
* Zjistěte, jak příliš[protokoly auditu použití toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
* Podívejte se na hello [škálování ukázkovou aplikaci na Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) šablonu, která nastavuje Python nebo bottle aplikace tooexercise hello automatického škálování funkce sadách škálování virtuálního počítače.

