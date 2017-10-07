---
title: "virtuální počítače Azure tooLog aaaConnect Analytics | Microsoft Docs"
description: "Pro systém Windows a Linux virtuální počítače běžící v Azure, hello nedoporučuje způsob, jak shromažďovat protokoly a metriky, je instalace rozšíření virtuálního počítače Azure Log Analytics hello. Můžete použít hello portál Azure nebo PowerShell tooinstall hello analýzy protokolů rozšíření virtuálního počítače na virtuálních počítačích Azure."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: ca39e586-a6af-42fe-862e-80978a58d9b1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: richrund
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac96c242d03ed3a22ca96368e5a8cc53f9a993db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-azure-virtual-machines-toolog-analytics-with-a-log-analytics-agent"></a>Připojit virtuální počítače Azure tooLog Analytics s agentem analýzy protokolů

Pro počítače s Windows a Linux hello doporučená metoda pro shromažďování protokolů a metriky, je instalace agenta analýzy protokolů hello.

Hello nejjednodušší způsob, jak tooinstall hello analýzy protokolů agent na virtuálních počítačích Azure je prostřednictvím hello rozšíření virtuálního počítače pro analýzu protokolu.  Pomocí rozšíření hello zjednodušuje proces instalace hello a automaticky nakonfiguruje hello agenta toosend data toohello pracovní prostor analýzy protokolů, které zadáte. Hello agent je také automaticky aktualizovány, zajistíte, že máte hello nejnovější funkce a opravy.

Pro virtuální počítače s Windows, povolíte hello *agenta Microsoft Monitoring Agent* rozšíření virtuálního počítače.
Pro virtuální počítače s Linuxem, povolíte hello *OMS agenta pro Linux* rozšíření virtuálního počítače.

Další informace o [rozšíření virtuálního počítače Azure](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a hello [agenta systému Linux](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Pokud používáte kolekce založené na agentovi pro data protokolu, je nutné nakonfigurovat [zdroje dat v analýzy protokolů](log-analytics-data-sources.md) toospecify hello protokoly a metriky, které chcete toocollect.

> [!IMPORTANT]
> Pokud nakonfigurujete data protokolu tooindex analýzy protokolů pomocí [Azure diagnostics](log-analytics-azure-storage.md), a nakonfigurujte hello agenta toocollect hello stejné protokoly, pak hello protokoly se shromažďují dvakrát. Budou se vám účtovat pro obě datové zdroje. Pokud máte nainstalován agent hello, shromažďování dat protokolu pomocí pouze hello agenta – není konfigurace dat protokolu toocollect analýzy protokolů z Azure diagnostics.
>
>

Existují tři způsoby snadné rozšíření virtuálního počítače analýzy protokolů tooenable hello:

* Pomocí hello portálu Azure
* Pomocí prostředí Azure PowerShell
* Pomocí šablony Azure Resource Manager

## <a name="enable-hello-vm-extension-in-hello-azure-portal"></a>Povolit hello rozšíření virtuálního počítače v hello portálu Azure
Můžete nainstalovat agenta hello pro analýzy protokolů a připojit hello Azure virtuální počítač, který běží na pomocí hello [portál Azure](https://portal.azure.com).

### <a name="tooinstall-hello-log-analytics-agent-and-connect-hello-virtual-machine-tooa-log-analytics-workspace"></a>tooinstall hello agenta analýzy protokolů a připojte hello pracovní prostor analýzy protokolů tooa virtuálního počítače
1. Přihlaste se k hello [portál Azure](http://portal.azure.com).
2. Vyberte **Procházet** na hello levé straně hello portálu a pak přejděte příliš**analýzy protokolů (OMS)** a vyberte ho.
3. V seznamu analýzy protokolů pracovních prostorů vyberte hello jeden, které chcete toouse s hello virtuálního počítače Azure.  
   ![Pracovní prostory OMS](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. V části **protokolu správy analytics**, vyberte **virtuální počítače**.  
   ![Virtual Machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5. V seznamu hello **virtuální počítače**, vyberte hello virtuální počítač, na kterém chcete tooinstall hello agenta. Hello **stav připojení OMS** hello virtuálního počítače označuje, že IT oddělení je **Nepřipojeno**.  
   ![Virtuální počítač není připojen.](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6. V hello podrobnosti pro virtuální počítač, vyberte **Connect**. je automaticky nainstalován a nakonfigurován pro pracovní prostor analýzy protokolů Hello agent. Tento proces trvá několik minut, během které doby hello stav připojení OMS je *připojení...*  
   ![Připojení virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7. Po instalaci a připojit agenta hello hello **OMS připojení** stav bude aktualizované tooshow **tento pracovní prostor**.  
   ![Připojení](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)

## <a name="enable-hello-vm-extension-using-powershell"></a>Povolit hello rozšíření virtuálního počítače pomocí prostředí PowerShell
Když nakonfigurujete virtuální počítač pomocí prostředí PowerShell, je nutné tooprovide hello **workspaceId** a **workspaceKey**. názvy vlastností Hello ve vaší konfiguraci json jsou **malá a velká písmena**.

Můžete najít hello Id a klíč na hello **nastavení** stránku hello OMS portálu nebo pomocí prostředí PowerShell, jak je uvedeno v předchozím příkladu hello.

![ID pracovního prostoru a primární klíč](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

Existují jiné příkazy pro virtuální počítače Azure classic a Resource Manager virtuální počítače. Následují příklady pro classic i Resource Manager virtuálních počítačů.

Klasické virtuální počítače použijte následující příklad PowerShell hello:

```PowerShell
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment hello following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment hello following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Pro virtuální počítače s Linuxem Resource Manager pomocí rozhraní příkazového řádku následující hello
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

Pro virtuální počítače správce prostředků použijte následující příklad PowerShell hello:

```PowerShell
Login-AzureRMAccount
Select-AzureRMSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable toofind OMS Workspace $workspaceName. Do you need toorun Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment hello following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment hello following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```


## <a name="deploy-hello-vm-extension-using-a-template"></a>Nasazení rozšíření hello virtuálního počítače pomocí šablony
Pomocí Azure Resource Manager můžete vytvořit šablonu (ve formátu JSON), která definuje hello nasazení a konfiguraci vaší aplikace. Tato šablona se označuje jako šablony Resource Manageru a nabízí deklarativní způsob toodefine nasazení. Pomocí šablony můžete opakovaně nasazení aplikace v průběhu životního cyklu aplikace hello a mít jistotu, že se prostředky nasadí v konzistentním stavu.

Zahrnutím hello analýzy protokolů agenta v rámci šablony Resource Manageru můžete zajistit, že každý virtuální počítač je předem nakonfigurovaný tooreport pracovní prostor analýzy protokolů tooyour.

Další informace o šablonách Resource Manager najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

Tady je příklad šablony Resource Manageru, který se používá pro nasazení virtuálního počítače se systémem Windows s hello rozšíření agenta Microsoft Monitoring Agent nainstalována. Tato šablona je šablonu typické virtuálního počítače s hello následující dodatky:

* ID pracovního prostoru a workspaceName parametry
* Microsoft.EnterpriseCloud.Monitoring oddílu rozšíření prostředků
* Výstupy toolook hello workspaceId a workspaceSharedKey

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for hello Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for hello Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for hello Public IP. Must be lowercase. It should match with hello following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMS workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "hello Windows version for hello VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
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
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Šablonu můžete nasadit pomocí hello následující příkaz prostředí PowerShell:

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-hello-log-analytics-vm-extension"></a>Řešení potíží s rozšíření virtuálního počítače Analytics protokolu hello
Obvykle zobrazí zprávu po věcí nefungují z portálu Azure nebo Azure powershell.

1. Přihlaste se k hello [portál Azure](http://portal.azure.com).
2. Najde hello virtuálního počítače a otevřete tak podrobnosti virtuálního počítače.
3. Klikněte na tlačítko **rozšíření** toocheck, pokud je povolené rozšíření OMS, nebo ne.

   ![Zobrazení rozšíření virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. Klikněte na tlačítko hello *MicrosoftMonitoringAgent*(Windows) nebo *OmsAgentForLinux*rozšíření a zobrazení podrobností (Linux). 

   ![Podrobnosti o rozšíření virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a>Řešení potíží virtuální počítače s Windows
Pokud hello *agenta Microsoft Monitoring Agent* není instalaci rozšíření agenta virtuálního počítače nebo vytváření sestav, můžete provést následující kroky tootroubleshoot hello problém hello.

1. Zkontrolujte, zda je agent virtuálního počítače Azure hello nainstalovaný a funkční správně pomocí hello kroky [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * Můžete také zkontrolovat soubor protokolu agenta virtuálního počítače hello`C:\WindowsAzure\logs\WaAppAgent.log`
   * Pokud hello protokolu neexistuje, není nainstalován agent virtuálního počítače hello.
     * [Nainstalujte agenta virtuálního počítače Azure hello na klasické virtuální počítače](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. Potvrďte hello Microsoft Monitoring Agent je spuštěn úkol prezenčního signálu rozšíření pomocí hello následující kroky:
   * Přihlaste se toohello virtuálního počítače
   * Otevřete Plánovač úloh a najde hello `update_azureoperationalinsight_agent_heartbeat` úloh
   * Potvrďte hello úloh je povolená a běží každou minutu
   * Zkontrolujte v souboru protokolu hello prezenčního signálu`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Zkontrolujte soubory protokolu rozšíření virtuálního počítače agenta monitorování Microsoft hello ve`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Ujistěte se, že hello virtuálního počítače můžete spouštět skripty prostředí PowerShell
5. Ujistěte se, že oprávnění C:\Windows\temp nezměnily
6. Zobrazit stav hello hello agenta Microsoft Monitoring Agent tak, že zadáte následující příkaz v okně Powershellu se zvýšenými oprávněními na virtuálním počítači hello hello`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Zkontrolujte soubory protokolu instalace agenta Microsoft Monitoring Agent hello ve`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Další informace najdete v tématu [řešení potíží s rozšířeními Windows](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="troubleshooting-linux-virtual-machines"></a>Řešení potíží virtuální počítače s Linuxem
Pokud hello *OMS agenta pro Linux* není instalaci rozšíření agenta virtuálního počítače nebo vytváření sestav, můžete provést následující kroky tootroubleshoot hello problém hello.

1. Pokud je stav rozšíření hello *neznámé* zkontrolujte, zda je nainstalován agent virtuálního počítače Azure hello a funguje správně, a to prohlédnutím souboru protokolu agenta virtuálního počítače hello`/var/log/waagent.log`
   * Pokud hello protokolu neexistuje, není nainstalován agent virtuálního počítače hello.
   * [Nainstalujte na virtuální počítače s Linuxem hello agenta virtuálního počítače Azure](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. Pro ostatní stavy není v pořádku, zkontrolujte hello agenta OMS pro rozšíření virtuálního počítače s Linuxem protokolové soubory `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` a`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Pokud stav rozšíření hello je v pořádku, ale není odesílání dat zkontrolujte hello OMS agenta pro Linux soubory protokolu`/var/opt/microsoft/omsagent/log/omsagent.log`

## <a name="next-steps"></a>Další kroky
* Konfigurace [zdroje dat v analýzy protokolů](log-analytics-data-sources.md) toospecify hello protokoly a metriky toocollect.
* toogather data z virtuálních počítačů [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).
* [Shromažďování dat pomocí Azure Diagnostics](log-analytics-azure-storage.md) další zdroje, které jsou spuštěné v Azure.

Pro počítače, které nejsou v Azure můžete nainstalovat agenta analýzy protokolů hello pomocí hello metod, které jsou popsány v hello následující články:

* [Připojení Windows počítače tooLog Analytics](log-analytics-windows-agents.md)
* [Připojení počítače tooLog Linux Analytics](log-analytics-linux-agents.md)
