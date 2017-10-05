---
title: "Připojit virtuální počítače Azure k analýze protokolů | Microsoft Docs"
description: "Pro systém Windows a Linux virtuální počítače běžící v Azure je doporučeným způsobem shromažďovat protokoly a metriky instalace rozšíření virtuálního počítače Azure Log Analytics. Instalace rozšíření virtuálního počítače analýzy protokolů na virtuálních počítačích Azure můžete portál Azure nebo PowerShell."
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
ms.openlocfilehash: cdae291b546fef4d7fdb8b067c8e4f4c9708d43f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-azure-virtual-machines-to-log-analytics-with-a-log-analytics-agent"></a><span data-ttu-id="c8eec-104">Připojit virtuální počítače Azure k analýze protokolů s agentem analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="c8eec-104">Connect Azure virtual machines to Log Analytics with a Log Analytics agent</span></span>

<span data-ttu-id="c8eec-105">Doporučené metody pro shromažďování protokolů a metriky pro počítače s Windows a Linux, je instalace agenta analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="c8eec-105">For Windows and Linux computers, the recommended method for collecting logs and metrics is by installing the Log Analytics agent.</span></span>

<span data-ttu-id="c8eec-106">Nejjednodušší způsob, jak nainstalovat agenta analýzy protokolů na virtuálních počítačích Azure je prostřednictvím rozšíření protokolu analýzy virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8eec-106">The easiest way to install the Log Analytics agent on Azure virtual machines is through the Log Analytics VM Extension.</span></span>  <span data-ttu-id="c8eec-107">Pomocí rozšíření zjednodušuje proces instalace a automaticky nakonfiguruje agenta k odesílání dat do pracovního prostoru analýzy protokolů, který určíte.</span><span class="sxs-lookup"><span data-stu-id="c8eec-107">Using the extension simplifies the installation process and automatically configures the agent to send data to the Log Analytics workspace that you specify.</span></span> <span data-ttu-id="c8eec-108">Agent je také automaticky aktualizovány, zajistíte, že máte nejnovější funkce a opravy.</span><span class="sxs-lookup"><span data-stu-id="c8eec-108">The agent is also upgraded automatically, ensuring that you have the latest features and fixes.</span></span>

<span data-ttu-id="c8eec-109">Pro virtuální počítače s Windows, povolíte *agenta Microsoft Monitoring Agent* rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8eec-109">For Windows virtual machines, you enable the *Microsoft Monitoring Agent* virtual machine extension.</span></span>
<span data-ttu-id="c8eec-110">Pro virtuální počítače s Linuxem, povolíte *OMS agenta pro Linux* rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8eec-110">For Linux virtual machines, you enable the *OMS Agent For Linux* virtual machine extension.</span></span>

<span data-ttu-id="c8eec-111">Další informace o [rozšíření virtuálního počítače Azure](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [agenta systému Linux](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c8eec-111">Learn more about [Azure virtual machine extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and the [Linux agent](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="c8eec-112">Pokud používáte kolekce založené na agentovi pro data protokolu, je nutné nakonfigurovat [zdroje dat v analýzy protokolů](log-analytics-data-sources.md) k určení protokoly a metriky, které chcete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="c8eec-112">When you use agent-based collection for log data, you must configure [data sources in Log Analytics](log-analytics-data-sources.md) to specify the logs and metrics that you want to collect.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c8eec-113">Pokud nakonfigurujete analýzy protokolů pro data protokolu index pomocí [Azure diagnostics](log-analytics-azure-storage.md)a konfigurace agenta shromažďování stejné protokoly a protokoly se shromažďují dvakrát.</span><span class="sxs-lookup"><span data-stu-id="c8eec-113">If you configure Log Analytics to index log data by using [Azure diagnostics](log-analytics-azure-storage.md), and you configure the agent to collect the same logs, then the logs are collected twice.</span></span> <span data-ttu-id="c8eec-114">Budou se vám účtovat pro obě datové zdroje.</span><span class="sxs-lookup"><span data-stu-id="c8eec-114">You are charged for both data sources.</span></span> <span data-ttu-id="c8eec-115">Pokud máte nainstalovaného agenta, shromažďování dat protokolu pomocí jenom agenta – není konfigurace analýzy protokolů pro shromažďování dat protokolu z Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="c8eec-115">If you have the agent installed, then collect log data by using only the agent - don't configure Log Analytics to collect log data from Azure diagnostics.</span></span>
>
>

<span data-ttu-id="c8eec-116">Existují tři způsoby snadno povolit rozšíření virtuálního počítače analýzy protokolů:</span><span class="sxs-lookup"><span data-stu-id="c8eec-116">There are three easy ways to enable the Log Analytics virtual machine extension:</span></span>

* <span data-ttu-id="c8eec-117">Pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c8eec-117">By using the Azure portal</span></span>
* <span data-ttu-id="c8eec-118">Pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8eec-118">By using Azure PowerShell</span></span>
* <span data-ttu-id="c8eec-119">Pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c8eec-119">By using an Azure Resource Manager template</span></span>

## <a name="enable-the-vm-extension-in-the-azure-portal"></a><span data-ttu-id="c8eec-120">Povolit rozšíření virtuálního počítače na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c8eec-120">Enable the VM extension in the Azure portal</span></span>
<span data-ttu-id="c8eec-121">Můžete nainstalovat agenta pro analýzy protokolů a připojit virtuální počítač Azure, který běží na pomocí [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c8eec-121">You can install the agent for Log Analytics and connect the Azure virtual machine that it runs on by using the [Azure portal](https://portal.azure.com).</span></span>

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a><span data-ttu-id="c8eec-122">Instalace agenta analýzy protokolů a připojte virtuální počítač do pracovního prostoru analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="c8eec-122">To install the Log Analytics agent and connect the virtual machine to a Log Analytics workspace</span></span>
1. <span data-ttu-id="c8eec-123">Přihlaste se k webu [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c8eec-123">Sign into the [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="c8eec-124">Vyberte **Procházet** na levé straně portálu, a potom přejděte na **analýzy protokolů (OMS)** a vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="c8eec-124">Select **Browse** on the left side of the portal, and then go to **Log Analytics (OMS)** and select it.</span></span>
3. <span data-ttu-id="c8eec-125">V seznamu analýzy protokolů pracovních prostorů vyberte ten, který chcete používat s virtuálním Počítačem Azure.</span><span class="sxs-lookup"><span data-stu-id="c8eec-125">In your list of Log Analytics workspaces, select the one that you want to use with the Azure VM.</span></span>  
   ![Pracovní prostory OMS](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. <span data-ttu-id="c8eec-127">V části **protokolu správy analytics**, vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="c8eec-127">Under **Log analytics management**, select **Virtual machines**.</span></span>  
   <span data-ttu-id="c8eec-128">![Virtual Machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span><span class="sxs-lookup"><span data-stu-id="c8eec-128">![Virtual machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span></span>
5. <span data-ttu-id="c8eec-129">V seznamu **virtuální počítače**, vyberte virtuální počítač, na kterém chcete nainstalovat agenta.</span><span class="sxs-lookup"><span data-stu-id="c8eec-129">In the list of **Virtual machines**, select the virtual machine on which you want to install the agent.</span></span> <span data-ttu-id="c8eec-130">**Stav připojení OMS** pro virtuální počítač naznačuje, že je **Nepřipojeno**.</span><span class="sxs-lookup"><span data-stu-id="c8eec-130">The **OMS connection status** for the VM indicates that it is **Not connected**.</span></span>  
   <span data-ttu-id="c8eec-131">![Virtuální počítač není připojen.](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span><span class="sxs-lookup"><span data-stu-id="c8eec-131">![VM not connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span></span>
6. <span data-ttu-id="c8eec-132">V podrobnostech pro virtuální počítač, vyberte **Connect**.</span><span class="sxs-lookup"><span data-stu-id="c8eec-132">In the details for your virtual machine, select **Connect**.</span></span> <span data-ttu-id="c8eec-133">Agent je automaticky nainstalovat a nakonfigurovat pro pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="c8eec-133">The agent is automatically installed and configured for your Log Analytics workspace.</span></span> <span data-ttu-id="c8eec-134">Tento proces trvá několik minut, během které doby je stav připojení OMS *připojení...*</span><span class="sxs-lookup"><span data-stu-id="c8eec-134">This process takes a few minutes, during which time the OMS Connection status is *Connecting...*</span></span>  
   <span data-ttu-id="c8eec-135">![Připojení virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span><span class="sxs-lookup"><span data-stu-id="c8eec-135">![Connect VM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span></span>
7. <span data-ttu-id="c8eec-136">Po dokončení instalace a připojit agenta, **OMS připojení** stav bude aktualizován zobrazíte **tento pracovní prostor**.</span><span class="sxs-lookup"><span data-stu-id="c8eec-136">After you install and connect the agent, the **OMS connection** status will be updated to show **This workspace**.</span></span>  
   <span data-ttu-id="c8eec-137">![Připojení](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span><span class="sxs-lookup"><span data-stu-id="c8eec-137">![Connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span></span>

## <a name="enable-the-vm-extension-using-powershell"></a><span data-ttu-id="c8eec-138">Povolit rozšíření virtuálního počítače pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8eec-138">Enable the VM extension using PowerShell</span></span>
<span data-ttu-id="c8eec-139">Když nakonfigurujete virtuální počítač pomocí prostředí PowerShell, budete muset zadat **workspaceId** a **workspaceKey**.</span><span class="sxs-lookup"><span data-stu-id="c8eec-139">When you configure your virtual machine by using PowerShell, you need to provide the **workspaceId** and **workspaceKey**.</span></span> <span data-ttu-id="c8eec-140">Názvy vlastností, které ve vaší konfiguraci json jsou **malá a velká písmena**.</span><span class="sxs-lookup"><span data-stu-id="c8eec-140">The property names in your json configuration are **case-sensitive**.</span></span>

<span data-ttu-id="c8eec-141">Můžete najít Id a klíč na **nastavení** stránky portálu OMS nebo pomocí prostředí PowerShell, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="c8eec-141">You can find the Id and key on the **Settings** page of the OMS portal, or by using PowerShell as shown in the preceding example.</span></span>

![ID pracovního prostoru a primární klíč](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

<span data-ttu-id="c8eec-143">Existují jiné příkazy pro virtuální počítače Azure classic a Resource Manager virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c8eec-143">There are different commands for Azure classic virtual machines and Resource Manager virtual machines.</span></span> <span data-ttu-id="c8eec-144">Následují příklady pro classic i Resource Manager virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c8eec-144">Following are examples for both classic and Resource Manager virtual machines.</span></span>

<span data-ttu-id="c8eec-145">Klasické virtuální počítače použijte následující příklad PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c8eec-145">For classic virtual machines, use the following PowerShell example:</span></span>

```PowerShell
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

<span data-ttu-id="c8eec-146">Pro virtuální počítače s Linuxem Resource Manager pomocí rozhraní příkazového řádku následující</span><span class="sxs-lookup"><span data-stu-id="c8eec-146">For Resource Manager Linux VMs using the following CLI</span></span>
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

<span data-ttu-id="c8eec-147">Pro virtuální počítače správce prostředků použijte následující příklad PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c8eec-147">For Resource Manager virtual machines, use the following PowerShell example:</span></span>

```PowerShell
Login-AzureRMAccount
Select-AzureRMSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```


## <a name="deploy-the-vm-extension-using-a-template"></a><span data-ttu-id="c8eec-148">Nasazení rozšíření virtuálního počítače pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="c8eec-148">Deploy the VM extension using a template</span></span>
<span data-ttu-id="c8eec-149">Pomocí Azure Resource Manager můžete vytvořit šablonu (ve formátu JSON), která definuje nasazení a konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8eec-149">By using Azure Resource Manager, you can create a template (in JSON format) that defines the deployment and configuration of your application.</span></span> <span data-ttu-id="c8eec-150">Tato šablona se označuje jako šablona Resource Manageru a nabízí deklarativní způsob, jak definovat nasazení.</span><span class="sxs-lookup"><span data-stu-id="c8eec-150">This template is known as a Resource Manager template and provides a declarative way to define deployment.</span></span> <span data-ttu-id="c8eec-151">Pomocí šablony můžete opakovaně nasazení aplikace v průběhu životního cyklu aplikace a mít jistotu, že se prostředky nasadí v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="c8eec-151">By using a template, you can repeatedly deploy your application throughout the app lifecycle and have confidence that your resources are being deployed in a consistent state.</span></span>

<span data-ttu-id="c8eec-152">Zahrnutím agenta analýzy protokolů v rámci šablony Resource Manageru můžete zajistit, že každý virtuální počítač je předem nakonfigurovaná tak, aby odesílaly pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="c8eec-152">By including the Log Analytics agent as part of your Resource Manager template, you can ensure that each virtual machine is pre-configured to report to your Log Analytics workspace.</span></span>

<span data-ttu-id="c8eec-153">Další informace o šablonách Resource Manager najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c8eec-153">For more information about Resource Manager templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="c8eec-154">Tady je příklad šablony Resource Manageru, který se používá pro nasazení virtuálního počítače se systémem Windows s příponou agenta Microsoft Monitoring Agent nainstalována.</span><span class="sxs-lookup"><span data-stu-id="c8eec-154">Following is an example of a Resource Manager template that's used for deploying a virtual machine that's running Windows with the Microsoft Monitoring Agent extension installed.</span></span> <span data-ttu-id="c8eec-155">Tato šablona je šablonu typické virtuálního počítače s těmito přídavky:</span><span class="sxs-lookup"><span data-stu-id="c8eec-155">This template is a typical virtual machine template, with the following additions:</span></span>

* <span data-ttu-id="c8eec-156">ID pracovního prostoru a workspaceName parametry</span><span class="sxs-lookup"><span data-stu-id="c8eec-156">workspaceId and workspaceName parameters</span></span>
* <span data-ttu-id="c8eec-157">Microsoft.EnterpriseCloud.Monitoring oddílu rozšíření prostředků</span><span class="sxs-lookup"><span data-stu-id="c8eec-157">Microsoft.EnterpriseCloud.Monitoring resource extension section</span></span>
* <span data-ttu-id="c8eec-158">Výstupy k vyhledání ID pracovního prostoru a workspaceSharedKey</span><span class="sxs-lookup"><span data-stu-id="c8eec-158">Outputs to look up the workspaceId and workspaceSharedKey</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
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
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
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

<span data-ttu-id="c8eec-159">Šablonu můžete nasadit pomocí následujícího příkazu Powershellu:</span><span class="sxs-lookup"><span data-stu-id="c8eec-159">You can deploy a template by using the following PowerShell command:</span></span>

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-the-log-analytics-vm-extension"></a><span data-ttu-id="c8eec-160">Řešení potíží s rozšíření virtuálního počítače analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="c8eec-160">Troubleshooting the Log Analytics VM extension</span></span>
<span data-ttu-id="c8eec-161">Obvykle zobrazí zprávu po věcí nefungují z portálu Azure nebo Azure powershell.</span><span class="sxs-lookup"><span data-stu-id="c8eec-161">Usually you receive a message when things don't work, from either Azure portal or Azure powershell.</span></span>

1. <span data-ttu-id="c8eec-162">Přihlaste se k webu [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c8eec-162">Sign into the [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="c8eec-163">Najít virtuálního počítače a otevřete tak podrobnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8eec-163">Find the VM and open VM details.</span></span>
3. <span data-ttu-id="c8eec-164">Klikněte na tlačítko **rozšíření** ke kontrole, pokud je povolené rozšíření OMS, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="c8eec-164">Click **Extensions** to check if OMS extension is enabled or not.</span></span>

   ![Zobrazení rozšíření virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. <span data-ttu-id="c8eec-166">Klikněte *MicrosoftMonitoringAgent*(Windows) nebo *OmsAgentForLinux*rozšíření a zobrazení podrobností (Linux).</span><span class="sxs-lookup"><span data-stu-id="c8eec-166">Click the *MicrosoftMonitoringAgent*(Windows) or *OmsAgentForLinux*(Linux) extension and view details.</span></span> 

   ![Podrobnosti o rozšíření virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a><span data-ttu-id="c8eec-168">Řešení potíží virtuální počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="c8eec-168">Troubleshooting Windows Virtual Machines</span></span>
<span data-ttu-id="c8eec-169">Pokud *agenta Microsoft Monitoring Agent* není instalaci rozšíření agenta virtuálního počítače nebo vytváření sestav, můžete provést následující kroky k odstranění problému.</span><span class="sxs-lookup"><span data-stu-id="c8eec-169">If the *Microsoft Monitoring Agent* VM agent extension is not installing or reporting, you can perform the following steps to troubleshoot the issue.</span></span>

1. <span data-ttu-id="c8eec-170">Zkontrolujte, zda je nainstalován agent virtuálního počítače Azure a že fungují správně pomocí kroků v [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span><span class="sxs-lookup"><span data-stu-id="c8eec-170">Check if the Azure VM agent is installed and working correctly by using the steps in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span></span>
   * <span data-ttu-id="c8eec-171">Můžete také zkontrolovat soubor protokolu agenta virtuálního počítače`C:\WindowsAzure\logs\WaAppAgent.log`</span><span class="sxs-lookup"><span data-stu-id="c8eec-171">You can also review the VM agent log file `C:\WindowsAzure\logs\WaAppAgent.log`</span></span>
   * <span data-ttu-id="c8eec-172">Pokud v protokolu neexistuje, není nainstalován agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8eec-172">If the log does not exist, the VM agent is not installed.</span></span>
     * [<span data-ttu-id="c8eec-173">Nainstalujte agenta virtuálního počítače Azure na klasické virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c8eec-173">Install the Azure VM Agent on classic VMs</span></span>](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. <span data-ttu-id="c8eec-174">Potvrďte, že je spuštěn úkol prezenčního signálu agenta Microsoft Monitoring Agent rozšíření, pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="c8eec-174">Confirm the Microsoft Monitoring Agent extension heartbeat task is running using the following steps:</span></span>
   * <span data-ttu-id="c8eec-175">Přihlaste se k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="c8eec-175">Log in to the virtual machine</span></span>
   * <span data-ttu-id="c8eec-176">Otevřete Plánovač úloh a najít `update_azureoperationalinsight_agent_heartbeat` úloh</span><span class="sxs-lookup"><span data-stu-id="c8eec-176">Open task scheduler and find the `update_azureoperationalinsight_agent_heartbeat` task</span></span>
   * <span data-ttu-id="c8eec-177">Potvrďte úloha je povolená a běží každou minutu</span><span class="sxs-lookup"><span data-stu-id="c8eec-177">Confirm the task is enabled and is running every one minute</span></span>
   * <span data-ttu-id="c8eec-178">Zkontrolujte v souboru protokolu prezenčního signálu`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span><span class="sxs-lookup"><span data-stu-id="c8eec-178">Check the heartbeat logfile in `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span></span>
3. <span data-ttu-id="c8eec-179">Zkontrolujte soubory protokolů rozšíření Microsoft Monitoring Agent virtuálního počítače v`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span><span class="sxs-lookup"><span data-stu-id="c8eec-179">Review the Microsoft Monitoring Agent VM extension log files in `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span></span>
4. <span data-ttu-id="c8eec-180">Zajistěte, aby byl že virtuální počítač můžete spouštět skripty prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8eec-180">Ensure the virtual machine can run PowerShell scripts</span></span>
5. <span data-ttu-id="c8eec-181">Ujistěte se, že oprávnění C:\Windows\temp nezměnily</span><span class="sxs-lookup"><span data-stu-id="c8eec-181">Ensure permissions on C:\Windows\temp haven’t been changed</span></span>
6. <span data-ttu-id="c8eec-182">Zobrazit stav služby Microsoft Monitoring Agent zadáním následujícího příkazu v okně prostředí PowerShell se zvýšenými oprávněními na virtuálním počítači`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span><span class="sxs-lookup"><span data-stu-id="c8eec-182">View the status of the Microsoft Monitoring Agent by typing the following command in an elevated PowerShell window on the virtual machine `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span></span>
7. <span data-ttu-id="c8eec-183">Zkontrolujte soubory protokolu instalace agenta Microsoft Monitoring Agent v`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span><span class="sxs-lookup"><span data-stu-id="c8eec-183">Review the Microsoft Monitoring Agent setup log files in `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span></span>

<span data-ttu-id="c8eec-184">Další informace najdete v tématu [řešení potíží s rozšířeními Windows](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c8eec-184">For more information, see [troubleshooting Windows extensions](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="troubleshooting-linux-virtual-machines"></a><span data-ttu-id="c8eec-185">Řešení potíží virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c8eec-185">Troubleshooting Linux Virtual Machines</span></span>
<span data-ttu-id="c8eec-186">Pokud *OMS agenta pro Linux* není instalaci rozšíření agenta virtuálního počítače nebo vytváření sestav, můžete provést následující kroky k odstranění problému.</span><span class="sxs-lookup"><span data-stu-id="c8eec-186">If the *OMS Agent for Linux* VM agent extension is not installing or reporting, you can perform the following steps to troubleshoot the issue.</span></span>

1. <span data-ttu-id="c8eec-187">Pokud je stav rozšíření *neznámé* zkontrolujte, zda je nainstalován agent virtuálního počítače Azure a že fungují správně, a to prohlédnutím souboru protokolu agenta virtuálního počítače`/var/log/waagent.log`</span><span class="sxs-lookup"><span data-stu-id="c8eec-187">If the extension status is *Unknown* check if the Azure VM agent is installed and working correctly by reviewing the VM agent log file `/var/log/waagent.log`</span></span>
   * <span data-ttu-id="c8eec-188">Pokud v protokolu neexistuje, není nainstalován agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8eec-188">If the log does not exist, the VM agent is not installed.</span></span>
   * [<span data-ttu-id="c8eec-189">Nainstalujte agenta virtuálního počítače Azure na virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c8eec-189">Install the Azure VM Agent on Linux VMs</span></span>](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. <span data-ttu-id="c8eec-190">Pro ostatní stavy není v pořádku, zkontrolujte agenta OMS pro rozšíření virtuálního počítače s Linuxem soubory protokolů `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` a`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span><span class="sxs-lookup"><span data-stu-id="c8eec-190">For other unhealthy statuses, review the OMS Agent for Linux VM extension logs files in `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` and `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span></span>
3. <span data-ttu-id="c8eec-191">Pokud je v pořádku stav rozšíření, ale není odesílání dat zkontrolujte OMS agenta pro Linux soubory protokolu v`/var/opt/microsoft/omsagent/log/omsagent.log`</span><span class="sxs-lookup"><span data-stu-id="c8eec-191">If the extension status is healthy, but data is not being uploaded review the OMS Agent for Linux log files in `/var/opt/microsoft/omsagent/log/omsagent.log`</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8eec-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8eec-192">Next steps</span></span>
* <span data-ttu-id="c8eec-193">Konfigurace [zdroje dat v analýzy protokolů](log-analytics-data-sources.md) k určení ke shromažďování metrik a protokolování.</span><span class="sxs-lookup"><span data-stu-id="c8eec-193">Configure [data sources in Log Analytics](log-analytics-data-sources.md) to specify the logs and metrics to collect.</span></span>
* <span data-ttu-id="c8eec-194">Chcete-li shromažďovat data z virtuálních počítačů [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="c8eec-194">To gather data from virtual machines [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
* <span data-ttu-id="c8eec-195">[Shromažďování dat pomocí Azure Diagnostics](log-analytics-azure-storage.md) další zdroje, které jsou spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="c8eec-195">[Collect data by using Azure Diagnostics](log-analytics-azure-storage.md) for other resources that are running in Azure.</span></span>

<span data-ttu-id="c8eec-196">Pro počítače, které nejsou v Azure můžete nainstalovat agenta analýzy protokolů pomocí metody, které jsou popsané v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="c8eec-196">For computers that are not in Azure, you can install the Log Analytics agent by using the methods that are described in the following articles:</span></span>

* [<span data-ttu-id="c8eec-197">Připojení počítače se systémem Windows k analýze protokolů</span><span class="sxs-lookup"><span data-stu-id="c8eec-197">Connect Windows computers to Log Analytics</span></span>](log-analytics-windows-agents.md)
* [<span data-ttu-id="c8eec-198">Připojení počítačů se systémem Linux k analýze protokolů</span><span class="sxs-lookup"><span data-stu-id="c8eec-198">Connect Linux computers to Log Analytics</span></span>](log-analytics-linux-agents.md)
