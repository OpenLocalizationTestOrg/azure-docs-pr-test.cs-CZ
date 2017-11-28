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
# <a name="connect-azure-virtual-machines-toolog-analytics-with-a-log-analytics-agent"></a><span data-ttu-id="599c4-104">Připojit virtuální počítače Azure tooLog Analytics s agentem analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="599c4-104">Connect Azure virtual machines tooLog Analytics with a Log Analytics agent</span></span>

<span data-ttu-id="599c4-105">Pro počítače s Windows a Linux hello doporučená metoda pro shromažďování protokolů a metriky, je instalace agenta analýzy protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="599c4-105">For Windows and Linux computers, hello recommended method for collecting logs and metrics is by installing hello Log Analytics agent.</span></span>

<span data-ttu-id="599c4-106">Hello nejjednodušší způsob, jak tooinstall hello analýzy protokolů agent na virtuálních počítačích Azure je prostřednictvím hello rozšíření virtuálního počítače pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="599c4-106">hello easiest way tooinstall hello Log Analytics agent on Azure virtual machines is through hello Log Analytics VM Extension.</span></span>  <span data-ttu-id="599c4-107">Pomocí rozšíření hello zjednodušuje proces instalace hello a automaticky nakonfiguruje hello agenta toosend data toohello pracovní prostor analýzy protokolů, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="599c4-107">Using hello extension simplifies hello installation process and automatically configures hello agent toosend data toohello Log Analytics workspace that you specify.</span></span> <span data-ttu-id="599c4-108">Hello agent je také automaticky aktualizovány, zajistíte, že máte hello nejnovější funkce a opravy.</span><span class="sxs-lookup"><span data-stu-id="599c4-108">hello agent is also upgraded automatically, ensuring that you have hello latest features and fixes.</span></span>

<span data-ttu-id="599c4-109">Pro virtuální počítače s Windows, povolíte hello *agenta Microsoft Monitoring Agent* rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="599c4-109">For Windows virtual machines, you enable hello *Microsoft Monitoring Agent* virtual machine extension.</span></span>
<span data-ttu-id="599c4-110">Pro virtuální počítače s Linuxem, povolíte hello *OMS agenta pro Linux* rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="599c4-110">For Linux virtual machines, you enable hello *OMS Agent For Linux* virtual machine extension.</span></span>

<span data-ttu-id="599c4-111">Další informace o [rozšíření virtuálního počítače Azure](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a hello [agenta systému Linux](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="599c4-111">Learn more about [Azure virtual machine extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and hello [Linux agent](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="599c4-112">Pokud používáte kolekce založené na agentovi pro data protokolu, je nutné nakonfigurovat [zdroje dat v analýzy protokolů](log-analytics-data-sources.md) toospecify hello protokoly a metriky, které chcete toocollect.</span><span class="sxs-lookup"><span data-stu-id="599c4-112">When you use agent-based collection for log data, you must configure [data sources in Log Analytics](log-analytics-data-sources.md) toospecify hello logs and metrics that you want toocollect.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="599c4-113">Pokud nakonfigurujete data protokolu tooindex analýzy protokolů pomocí [Azure diagnostics](log-analytics-azure-storage.md), a nakonfigurujte hello agenta toocollect hello stejné protokoly, pak hello protokoly se shromažďují dvakrát.</span><span class="sxs-lookup"><span data-stu-id="599c4-113">If you configure Log Analytics tooindex log data by using [Azure diagnostics](log-analytics-azure-storage.md), and you configure hello agent toocollect hello same logs, then hello logs are collected twice.</span></span> <span data-ttu-id="599c4-114">Budou se vám účtovat pro obě datové zdroje.</span><span class="sxs-lookup"><span data-stu-id="599c4-114">You are charged for both data sources.</span></span> <span data-ttu-id="599c4-115">Pokud máte nainstalován agent hello, shromažďování dat protokolu pomocí pouze hello agenta – není konfigurace dat protokolu toocollect analýzy protokolů z Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="599c4-115">If you have hello agent installed, then collect log data by using only hello agent - don't configure Log Analytics toocollect log data from Azure diagnostics.</span></span>
>
>

<span data-ttu-id="599c4-116">Existují tři způsoby snadné rozšíření virtuálního počítače analýzy protokolů tooenable hello:</span><span class="sxs-lookup"><span data-stu-id="599c4-116">There are three easy ways tooenable hello Log Analytics virtual machine extension:</span></span>

* <span data-ttu-id="599c4-117">Pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="599c4-117">By using hello Azure portal</span></span>
* <span data-ttu-id="599c4-118">Pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="599c4-118">By using Azure PowerShell</span></span>
* <span data-ttu-id="599c4-119">Pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="599c4-119">By using an Azure Resource Manager template</span></span>

## <a name="enable-hello-vm-extension-in-hello-azure-portal"></a><span data-ttu-id="599c4-120">Povolit hello rozšíření virtuálního počítače v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="599c4-120">Enable hello VM extension in hello Azure portal</span></span>
<span data-ttu-id="599c4-121">Můžete nainstalovat agenta hello pro analýzy protokolů a připojit hello Azure virtuální počítač, který běží na pomocí hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="599c4-121">You can install hello agent for Log Analytics and connect hello Azure virtual machine that it runs on by using hello [Azure portal](https://portal.azure.com).</span></span>

### <a name="tooinstall-hello-log-analytics-agent-and-connect-hello-virtual-machine-tooa-log-analytics-workspace"></a><span data-ttu-id="599c4-122">tooinstall hello agenta analýzy protokolů a připojte hello pracovní prostor analýzy protokolů tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="599c4-122">tooinstall hello Log Analytics agent and connect hello virtual machine tooa Log Analytics workspace</span></span>
1. <span data-ttu-id="599c4-123">Přihlaste se k hello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="599c4-123">Sign into hello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="599c4-124">Vyberte **Procházet** na hello levé straně hello portálu a pak přejděte příliš**analýzy protokolů (OMS)** a vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="599c4-124">Select **Browse** on hello left side of hello portal, and then go too**Log Analytics (OMS)** and select it.</span></span>
3. <span data-ttu-id="599c4-125">V seznamu analýzy protokolů pracovních prostorů vyberte hello jeden, které chcete toouse s hello virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="599c4-125">In your list of Log Analytics workspaces, select hello one that you want toouse with hello Azure VM.</span></span>  
   ![Pracovní prostory OMS](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. <span data-ttu-id="599c4-127">V části **protokolu správy analytics**, vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="599c4-127">Under **Log analytics management**, select **Virtual machines**.</span></span>  
   <span data-ttu-id="599c4-128">![Virtual Machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span><span class="sxs-lookup"><span data-stu-id="599c4-128">![Virtual machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span></span>
5. <span data-ttu-id="599c4-129">V seznamu hello **virtuální počítače**, vyberte hello virtuální počítač, na kterém chcete tooinstall hello agenta.</span><span class="sxs-lookup"><span data-stu-id="599c4-129">In hello list of **Virtual machines**, select hello virtual machine on which you want tooinstall hello agent.</span></span> <span data-ttu-id="599c4-130">Hello **stav připojení OMS** hello virtuálního počítače označuje, že IT oddělení je **Nepřipojeno**.</span><span class="sxs-lookup"><span data-stu-id="599c4-130">hello **OMS connection status** for hello VM indicates that it is **Not connected**.</span></span>  
   <span data-ttu-id="599c4-131">![Virtuální počítač není připojen.](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span><span class="sxs-lookup"><span data-stu-id="599c4-131">![VM not connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span></span>
6. <span data-ttu-id="599c4-132">V hello podrobnosti pro virtuální počítač, vyberte **Connect**.</span><span class="sxs-lookup"><span data-stu-id="599c4-132">In hello details for your virtual machine, select **Connect**.</span></span> <span data-ttu-id="599c4-133">je automaticky nainstalován a nakonfigurován pro pracovní prostor analýzy protokolů Hello agent.</span><span class="sxs-lookup"><span data-stu-id="599c4-133">hello agent is automatically installed and configured for your Log Analytics workspace.</span></span> <span data-ttu-id="599c4-134">Tento proces trvá několik minut, během které doby hello stav připojení OMS je *připojení...*</span><span class="sxs-lookup"><span data-stu-id="599c4-134">This process takes a few minutes, during which time hello OMS Connection status is *Connecting...*</span></span>  
   <span data-ttu-id="599c4-135">![Připojení virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span><span class="sxs-lookup"><span data-stu-id="599c4-135">![Connect VM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span></span>
7. <span data-ttu-id="599c4-136">Po instalaci a připojit agenta hello hello **OMS připojení** stav bude aktualizované tooshow **tento pracovní prostor**.</span><span class="sxs-lookup"><span data-stu-id="599c4-136">After you install and connect hello agent, hello **OMS connection** status will be updated tooshow **This workspace**.</span></span>  
   <span data-ttu-id="599c4-137">![Připojení](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span><span class="sxs-lookup"><span data-stu-id="599c4-137">![Connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span></span>

## <a name="enable-hello-vm-extension-using-powershell"></a><span data-ttu-id="599c4-138">Povolit hello rozšíření virtuálního počítače pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="599c4-138">Enable hello VM extension using PowerShell</span></span>
<span data-ttu-id="599c4-139">Když nakonfigurujete virtuální počítač pomocí prostředí PowerShell, je nutné tooprovide hello **workspaceId** a **workspaceKey**.</span><span class="sxs-lookup"><span data-stu-id="599c4-139">When you configure your virtual machine by using PowerShell, you need tooprovide hello **workspaceId** and **workspaceKey**.</span></span> <span data-ttu-id="599c4-140">názvy vlastností Hello ve vaší konfiguraci json jsou **malá a velká písmena**.</span><span class="sxs-lookup"><span data-stu-id="599c4-140">hello property names in your json configuration are **case-sensitive**.</span></span>

<span data-ttu-id="599c4-141">Můžete najít hello Id a klíč na hello **nastavení** stránku hello OMS portálu nebo pomocí prostředí PowerShell, jak je uvedeno v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="599c4-141">You can find hello Id and key on hello **Settings** page of hello OMS portal, or by using PowerShell as shown in hello preceding example.</span></span>

![ID pracovního prostoru a primární klíč](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

<span data-ttu-id="599c4-143">Existují jiné příkazy pro virtuální počítače Azure classic a Resource Manager virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="599c4-143">There are different commands for Azure classic virtual machines and Resource Manager virtual machines.</span></span> <span data-ttu-id="599c4-144">Následují příklady pro classic i Resource Manager virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="599c4-144">Following are examples for both classic and Resource Manager virtual machines.</span></span>

<span data-ttu-id="599c4-145">Klasické virtuální počítače použijte následující příklad PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="599c4-145">For classic virtual machines, use hello following PowerShell example:</span></span>

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

<span data-ttu-id="599c4-146">Pro virtuální počítače s Linuxem Resource Manager pomocí rozhraní příkazového řádku následující hello</span><span class="sxs-lookup"><span data-stu-id="599c4-146">For Resource Manager Linux VMs using hello following CLI</span></span>
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

<span data-ttu-id="599c4-147">Pro virtuální počítače správce prostředků použijte následující příklad PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="599c4-147">For Resource Manager virtual machines, use hello following PowerShell example:</span></span>

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


## <a name="deploy-hello-vm-extension-using-a-template"></a><span data-ttu-id="599c4-148">Nasazení rozšíření hello virtuálního počítače pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="599c4-148">Deploy hello VM extension using a template</span></span>
<span data-ttu-id="599c4-149">Pomocí Azure Resource Manager můžete vytvořit šablonu (ve formátu JSON), která definuje hello nasazení a konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="599c4-149">By using Azure Resource Manager, you can create a template (in JSON format) that defines hello deployment and configuration of your application.</span></span> <span data-ttu-id="599c4-150">Tato šablona se označuje jako šablony Resource Manageru a nabízí deklarativní způsob toodefine nasazení.</span><span class="sxs-lookup"><span data-stu-id="599c4-150">This template is known as a Resource Manager template and provides a declarative way toodefine deployment.</span></span> <span data-ttu-id="599c4-151">Pomocí šablony můžete opakovaně nasazení aplikace v průběhu životního cyklu aplikace hello a mít jistotu, že se prostředky nasadí v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="599c4-151">By using a template, you can repeatedly deploy your application throughout hello app lifecycle and have confidence that your resources are being deployed in a consistent state.</span></span>

<span data-ttu-id="599c4-152">Zahrnutím hello analýzy protokolů agenta v rámci šablony Resource Manageru můžete zajistit, že každý virtuální počítač je předem nakonfigurovaný tooreport pracovní prostor analýzy protokolů tooyour.</span><span class="sxs-lookup"><span data-stu-id="599c4-152">By including hello Log Analytics agent as part of your Resource Manager template, you can ensure that each virtual machine is pre-configured tooreport tooyour Log Analytics workspace.</span></span>

<span data-ttu-id="599c4-153">Další informace o šablonách Resource Manager najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="599c4-153">For more information about Resource Manager templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="599c4-154">Tady je příklad šablony Resource Manageru, který se používá pro nasazení virtuálního počítače se systémem Windows s hello rozšíření agenta Microsoft Monitoring Agent nainstalována.</span><span class="sxs-lookup"><span data-stu-id="599c4-154">Following is an example of a Resource Manager template that's used for deploying a virtual machine that's running Windows with hello Microsoft Monitoring Agent extension installed.</span></span> <span data-ttu-id="599c4-155">Tato šablona je šablonu typické virtuálního počítače s hello následující dodatky:</span><span class="sxs-lookup"><span data-stu-id="599c4-155">This template is a typical virtual machine template, with hello following additions:</span></span>

* <span data-ttu-id="599c4-156">ID pracovního prostoru a workspaceName parametry</span><span class="sxs-lookup"><span data-stu-id="599c4-156">workspaceId and workspaceName parameters</span></span>
* <span data-ttu-id="599c4-157">Microsoft.EnterpriseCloud.Monitoring oddílu rozšíření prostředků</span><span class="sxs-lookup"><span data-stu-id="599c4-157">Microsoft.EnterpriseCloud.Monitoring resource extension section</span></span>
* <span data-ttu-id="599c4-158">Výstupy toolook hello workspaceId a workspaceSharedKey</span><span class="sxs-lookup"><span data-stu-id="599c4-158">Outputs toolook up hello workspaceId and workspaceSharedKey</span></span>

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

<span data-ttu-id="599c4-159">Šablonu můžete nasadit pomocí hello následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="599c4-159">You can deploy a template by using hello following PowerShell command:</span></span>

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-hello-log-analytics-vm-extension"></a><span data-ttu-id="599c4-160">Řešení potíží s rozšíření virtuálního počítače Analytics protokolu hello</span><span class="sxs-lookup"><span data-stu-id="599c4-160">Troubleshooting hello Log Analytics VM extension</span></span>
<span data-ttu-id="599c4-161">Obvykle zobrazí zprávu po věcí nefungují z portálu Azure nebo Azure powershell.</span><span class="sxs-lookup"><span data-stu-id="599c4-161">Usually you receive a message when things don't work, from either Azure portal or Azure powershell.</span></span>

1. <span data-ttu-id="599c4-162">Přihlaste se k hello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="599c4-162">Sign into hello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="599c4-163">Najde hello virtuálního počítače a otevřete tak podrobnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="599c4-163">Find hello VM and open VM details.</span></span>
3. <span data-ttu-id="599c4-164">Klikněte na tlačítko **rozšíření** toocheck, pokud je povolené rozšíření OMS, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="599c4-164">Click **Extensions** toocheck if OMS extension is enabled or not.</span></span>

   ![Zobrazení rozšíření virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. <span data-ttu-id="599c4-166">Klikněte na tlačítko hello *MicrosoftMonitoringAgent*(Windows) nebo *OmsAgentForLinux*rozšíření a zobrazení podrobností (Linux).</span><span class="sxs-lookup"><span data-stu-id="599c4-166">Click hello *MicrosoftMonitoringAgent*(Windows) or *OmsAgentForLinux*(Linux) extension and view details.</span></span> 

   ![Podrobnosti o rozšíření virtuálního počítače](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a><span data-ttu-id="599c4-168">Řešení potíží virtuální počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="599c4-168">Troubleshooting Windows Virtual Machines</span></span>
<span data-ttu-id="599c4-169">Pokud hello *agenta Microsoft Monitoring Agent* není instalaci rozšíření agenta virtuálního počítače nebo vytváření sestav, můžete provést následující kroky tootroubleshoot hello problém hello.</span><span class="sxs-lookup"><span data-stu-id="599c4-169">If hello *Microsoft Monitoring Agent* VM agent extension is not installing or reporting, you can perform hello following steps tootroubleshoot hello issue.</span></span>

1. <span data-ttu-id="599c4-170">Zkontrolujte, zda je agent virtuálního počítače Azure hello nainstalovaný a funkční správně pomocí hello kroky [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span><span class="sxs-lookup"><span data-stu-id="599c4-170">Check if hello Azure VM agent is installed and working correctly by using hello steps in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span></span>
   * <span data-ttu-id="599c4-171">Můžete také zkontrolovat soubor protokolu agenta virtuálního počítače hello`C:\WindowsAzure\logs\WaAppAgent.log`</span><span class="sxs-lookup"><span data-stu-id="599c4-171">You can also review hello VM agent log file `C:\WindowsAzure\logs\WaAppAgent.log`</span></span>
   * <span data-ttu-id="599c4-172">Pokud hello protokolu neexistuje, není nainstalován agent virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="599c4-172">If hello log does not exist, hello VM agent is not installed.</span></span>
     * [<span data-ttu-id="599c4-173">Nainstalujte agenta virtuálního počítače Azure hello na klasické virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="599c4-173">Install hello Azure VM Agent on classic VMs</span></span>](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. <span data-ttu-id="599c4-174">Potvrďte hello Microsoft Monitoring Agent je spuštěn úkol prezenčního signálu rozšíření pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="599c4-174">Confirm hello Microsoft Monitoring Agent extension heartbeat task is running using hello following steps:</span></span>
   * <span data-ttu-id="599c4-175">Přihlaste se toohello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="599c4-175">Log in toohello virtual machine</span></span>
   * <span data-ttu-id="599c4-176">Otevřete Plánovač úloh a najde hello `update_azureoperationalinsight_agent_heartbeat` úloh</span><span class="sxs-lookup"><span data-stu-id="599c4-176">Open task scheduler and find hello `update_azureoperationalinsight_agent_heartbeat` task</span></span>
   * <span data-ttu-id="599c4-177">Potvrďte hello úloh je povolená a běží každou minutu</span><span class="sxs-lookup"><span data-stu-id="599c4-177">Confirm hello task is enabled and is running every one minute</span></span>
   * <span data-ttu-id="599c4-178">Zkontrolujte v souboru protokolu hello prezenčního signálu`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span><span class="sxs-lookup"><span data-stu-id="599c4-178">Check hello heartbeat logfile in `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span></span>
3. <span data-ttu-id="599c4-179">Zkontrolujte soubory protokolu rozšíření virtuálního počítače agenta monitorování Microsoft hello ve`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span><span class="sxs-lookup"><span data-stu-id="599c4-179">Review hello Microsoft Monitoring Agent VM extension log files in `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span></span>
4. <span data-ttu-id="599c4-180">Ujistěte se, že hello virtuálního počítače můžete spouštět skripty prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="599c4-180">Ensure hello virtual machine can run PowerShell scripts</span></span>
5. <span data-ttu-id="599c4-181">Ujistěte se, že oprávnění C:\Windows\temp nezměnily</span><span class="sxs-lookup"><span data-stu-id="599c4-181">Ensure permissions on C:\Windows\temp haven’t been changed</span></span>
6. <span data-ttu-id="599c4-182">Zobrazit stav hello hello agenta Microsoft Monitoring Agent tak, že zadáte následující příkaz v okně Powershellu se zvýšenými oprávněními na virtuálním počítači hello hello`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span><span class="sxs-lookup"><span data-stu-id="599c4-182">View hello status of hello Microsoft Monitoring Agent by typing hello following command in an elevated PowerShell window on hello virtual machine `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span></span>
7. <span data-ttu-id="599c4-183">Zkontrolujte soubory protokolu instalace agenta Microsoft Monitoring Agent hello ve`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span><span class="sxs-lookup"><span data-stu-id="599c4-183">Review hello Microsoft Monitoring Agent setup log files in `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span></span>

<span data-ttu-id="599c4-184">Další informace najdete v tématu [řešení potíží s rozšířeními Windows](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="599c4-184">For more information, see [troubleshooting Windows extensions](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="troubleshooting-linux-virtual-machines"></a><span data-ttu-id="599c4-185">Řešení potíží virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="599c4-185">Troubleshooting Linux Virtual Machines</span></span>
<span data-ttu-id="599c4-186">Pokud hello *OMS agenta pro Linux* není instalaci rozšíření agenta virtuálního počítače nebo vytváření sestav, můžete provést následující kroky tootroubleshoot hello problém hello.</span><span class="sxs-lookup"><span data-stu-id="599c4-186">If hello *OMS Agent for Linux* VM agent extension is not installing or reporting, you can perform hello following steps tootroubleshoot hello issue.</span></span>

1. <span data-ttu-id="599c4-187">Pokud je stav rozšíření hello *neznámé* zkontrolujte, zda je nainstalován agent virtuálního počítače Azure hello a funguje správně, a to prohlédnutím souboru protokolu agenta virtuálního počítače hello`/var/log/waagent.log`</span><span class="sxs-lookup"><span data-stu-id="599c4-187">If hello extension status is *Unknown* check if hello Azure VM agent is installed and working correctly by reviewing hello VM agent log file `/var/log/waagent.log`</span></span>
   * <span data-ttu-id="599c4-188">Pokud hello protokolu neexistuje, není nainstalován agent virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="599c4-188">If hello log does not exist, hello VM agent is not installed.</span></span>
   * [<span data-ttu-id="599c4-189">Nainstalujte na virtuální počítače s Linuxem hello agenta virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="599c4-189">Install hello Azure VM Agent on Linux VMs</span></span>](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. <span data-ttu-id="599c4-190">Pro ostatní stavy není v pořádku, zkontrolujte hello agenta OMS pro rozšíření virtuálního počítače s Linuxem protokolové soubory `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` a`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span><span class="sxs-lookup"><span data-stu-id="599c4-190">For other unhealthy statuses, review hello OMS Agent for Linux VM extension logs files in `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` and `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span></span>
3. <span data-ttu-id="599c4-191">Pokud stav rozšíření hello je v pořádku, ale není odesílání dat zkontrolujte hello OMS agenta pro Linux soubory protokolu`/var/opt/microsoft/omsagent/log/omsagent.log`</span><span class="sxs-lookup"><span data-stu-id="599c4-191">If hello extension status is healthy, but data is not being uploaded review hello OMS Agent for Linux log files in `/var/opt/microsoft/omsagent/log/omsagent.log`</span></span>

## <a name="next-steps"></a><span data-ttu-id="599c4-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="599c4-192">Next steps</span></span>
* <span data-ttu-id="599c4-193">Konfigurace [zdroje dat v analýzy protokolů](log-analytics-data-sources.md) toospecify hello protokoly a metriky toocollect.</span><span class="sxs-lookup"><span data-stu-id="599c4-193">Configure [data sources in Log Analytics](log-analytics-data-sources.md) toospecify hello logs and metrics toocollect.</span></span>
* <span data-ttu-id="599c4-194">toogather data z virtuálních počítačů [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="599c4-194">toogather data from virtual machines [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
* <span data-ttu-id="599c4-195">[Shromažďování dat pomocí Azure Diagnostics](log-analytics-azure-storage.md) další zdroje, které jsou spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="599c4-195">[Collect data by using Azure Diagnostics](log-analytics-azure-storage.md) for other resources that are running in Azure.</span></span>

<span data-ttu-id="599c4-196">Pro počítače, které nejsou v Azure můžete nainstalovat agenta analýzy protokolů hello pomocí hello metod, které jsou popsány v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="599c4-196">For computers that are not in Azure, you can install hello Log Analytics agent by using hello methods that are described in hello following articles:</span></span>

* [<span data-ttu-id="599c4-197">Připojení Windows počítače tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="599c4-197">Connect Windows computers tooLog Analytics</span></span>](log-analytics-windows-agents.md)
* [<span data-ttu-id="599c4-198">Připojení počítače tooLog Linux Analytics</span><span class="sxs-lookup"><span data-stu-id="599c4-198">Connect Linux computers tooLog Analytics</span></span>](log-analytics-linux-agents.md)
