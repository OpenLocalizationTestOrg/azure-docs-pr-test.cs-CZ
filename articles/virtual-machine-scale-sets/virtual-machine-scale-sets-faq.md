---
title: "Nejčastější dotazy k sadách škálování virtuálních počítačů aaaAzure | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se sady škálování virtuálního počítače."
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: negat
ms.custom: na
ms.openlocfilehash: 0deb9e2bb79f87f17bbf748397b94dc53070cfbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a><span data-ttu-id="68872-103">Nejčastější dotazy k sadách škálování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="68872-103">Azure virtual machine scale sets FAQs</span></span>

<span data-ttu-id="68872-104">Získejte odpovědi nastaví toofrequently kladené dotazy týkající se škálování virtuálních počítačů v Azure.</span><span class="sxs-lookup"><span data-stu-id="68872-104">Get answers toofrequently asked questions about virtual machine scale sets in Azure.</span></span>

## <a name="autoscale"></a><span data-ttu-id="68872-105">Automatické škálování</span><span class="sxs-lookup"><span data-stu-id="68872-105">Autoscale</span></span>

### <a name="what-are-best-practices-for-azure-autoscale"></a><span data-ttu-id="68872-106">Jaké jsou doporučené postupy pro škálování Azure?</span><span class="sxs-lookup"><span data-stu-id="68872-106">What are best practices for Azure Autoscale?</span></span>

<span data-ttu-id="68872-107">Osvědčené postupy pro škálování, najdete v části [osvědčené postupy pro automatické škálování virtuálních počítačů](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span><span class="sxs-lookup"><span data-stu-id="68872-107">For best practices for Autoscale, see [Best practices for autoscaling virtual machines](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span></span>

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a><span data-ttu-id="68872-108">Kde metriky názvy pro automatické škálování, který používá metriky na hostiteli</span><span class="sxs-lookup"><span data-stu-id="68872-108">Where do I find metric names for autoscaling that uses host-based metrics?</span></span>

<span data-ttu-id="68872-109">Metriky názvy pro automatické škálování, který používá metriky na hostiteli, najdete v části [podporované metriky s Azure monitorování](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span><span class="sxs-lookup"><span data-stu-id="68872-109">For metric names for autoscaling that uses host-based metrics, see [Supported metrics with Azure Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span></span>

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a><span data-ttu-id="68872-110">Existují jakékoli příklady automatické škálování podle o délce téma a fronty Azure Service Bus?</span><span class="sxs-lookup"><span data-stu-id="68872-110">Are there any examples of autoscaling based on an Azure Service Bus topic and queue length?</span></span>

<span data-ttu-id="68872-111">Ano.</span><span class="sxs-lookup"><span data-stu-id="68872-111">Yes.</span></span> <span data-ttu-id="68872-112">Příklady automatické škálování podle o délce téma a fronty Azure Service Bus najdete v tématu [běžné metriky automatického škálování Azure monitorování](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span><span class="sxs-lookup"><span data-stu-id="68872-112">For examples of autoscaling based on an Azure Service Bus topic and queue length, see [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span></span>

<span data-ttu-id="68872-113">Pro fronty Service Bus použijte následující JSON hello:</span><span class="sxs-lookup"><span data-stu-id="68872-113">For a Service Bus queue, use hello following JSON:</span></span>

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

<span data-ttu-id="68872-114">Pro frontu úložiště použijte následující JSON hello:</span><span class="sxs-lookup"><span data-stu-id="68872-114">For a storage queue, use hello following JSON:</span></span>

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

<span data-ttu-id="68872-115">Nahraďte ukázkové hodnoty prostředku identifikátory Uniform Resource (Identifier).</span><span class="sxs-lookup"><span data-stu-id="68872-115">Replace example values with your resource Uniform Resource Identifiers (URIs).</span></span>


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a><span data-ttu-id="68872-116">Měli I škálování pomocí metriky na hostiteli nebo rozšíření diagnostiky?</span><span class="sxs-lookup"><span data-stu-id="68872-116">Should I autoscale by using host-based metrics or a diagnostics extension?</span></span>

<span data-ttu-id="68872-117">Nastavení automatického škálování můžete vytvořit na metriky úrovni hostitele virtuálních počítačů toouse nebo metriky na základě operačního systému hosta.</span><span class="sxs-lookup"><span data-stu-id="68872-117">You can create an autoscale setting on a VM toouse host-level metrics or guest OS-based metrics.</span></span>

<span data-ttu-id="68872-118">Seznam podporovaných metriky, naleznete v části [běžné metriky automatického škálování Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span><span class="sxs-lookup"><span data-stu-id="68872-118">For a list of supported metrics, see [Azure Monitor autoscaling common metrics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span></span> 

<span data-ttu-id="68872-119">Úplný příklad pro sady škálování virtuálního počítače, naleznete v tématu [pokročilé škálování konfigurace pomocí šablony Resource Manageru pro sady škálování virtuálního počítače](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span><span class="sxs-lookup"><span data-stu-id="68872-119">For a full sample for virtual machine scale sets, see [Advanced autoscale configuration by using Resource Manager templates for virtual machine scale sets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span></span> 

<span data-ttu-id="68872-120">Ukázka Hello používá metrika procesoru hello úrovni hostitele a metriky počet zpráv.</span><span class="sxs-lookup"><span data-stu-id="68872-120">hello sample uses hello host-level CPU metric and a message count metric.</span></span>



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a><span data-ttu-id="68872-121">Jak nastavit pravidla výstrah na škálovací sadu virtuálních počítačů?</span><span class="sxs-lookup"><span data-stu-id="68872-121">How do I set alert rules on a virtual machine scale set?</span></span>

<span data-ttu-id="68872-122">Výstrahy můžete vytvořit na metriky pro sady škálování virtuálního počítače pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="68872-122">You can create alerts on metrics for virtual machine scale sets via PowerShell or Azure CLI.</span></span> <span data-ttu-id="68872-123">Další informace najdete v tématu [Azure PowerShell monitorování rychlý start ukázky](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) a [Azure monitorování napříč platformami CLI rychlý start ukázky](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span><span class="sxs-lookup"><span data-stu-id="68872-123">For more information, see [Azure Monitor PowerShell quick start samples](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) and [Azure Monitor cross-platform CLI quick start samples](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span></span>

<span data-ttu-id="68872-124">Hello TargetResourceId škálovací sadu virtuálních počítačů hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="68872-124">hello TargetResourceId of hello virtual machine scale set looks like this:</span></span> 

<span data-ttu-id="68872-125">/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.COMPUTE/virtualMachineScaleSets/yourvmssname</span><span class="sxs-lookup"><span data-stu-id="68872-125">/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname</span></span>

<span data-ttu-id="68872-126">Všechny čítače výkonu virtuálních počítačů můžete vybrat jako metriky tooset hello upozornění.</span><span class="sxs-lookup"><span data-stu-id="68872-126">You can choose any VM performance counter as hello metric tooset an alert for.</span></span> <span data-ttu-id="68872-127">Další informace najdete v tématu [metriky hostovaného operačního systému pro virtuální počítače na bázi správce prostředků Windows](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) a [metriky hostovaného operačního systému pro virtuální počítače s Linuxem](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) v hello [běžné metriky automatického škálování Azure monitorování](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/)článku.</span><span class="sxs-lookup"><span data-stu-id="68872-127">For more information, see [Guest OS metrics for Resource Manager-based Windows VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) and [Guest OS metrics for Linux VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) in hello [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) article.</span></span>

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a><span data-ttu-id="68872-128">Jak mám nastavit automatické škálování podle škálování virtuálních počítačů, nastavit pomocí prostředí PowerShell?</span><span class="sxs-lookup"><span data-stu-id="68872-128">How do I set up autoscale on a virtual machine scale set by using PowerShell?</span></span>

<span data-ttu-id="68872-129">tooset až automatické škálování podle škálování virtuálních počítačů, nastavit pomocí prostředí PowerShell, najdete v příspěvku blogu hello [jak nastavit automatické škálování tooan tooadd škálování virtuálního počítače Azure](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="68872-129">tooset up autoscale on a virtual machine scale set by using PowerShell, see hello blog post [How tooadd autoscale tooan Azure virtual machine scale set](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span></span>




## <a name="certificates"></a><span data-ttu-id="68872-130">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="68872-130">Certificates</span></span>

### <a name="how-do-i-securely-ship-a-certificate-toohello-vm-how-do-i-provision-a-virtual-machine-scale-set-toorun-a-website-where-hello-ssl-for-hello-website-is-shipped-securely-from-a-certificate-configuration-hello-common-certificate-rotation-operation-would-be-almost-hello-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-toodo-this"></a><span data-ttu-id="68872-131">Jak můžu bezpečně dodávat certifikát toohello virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="68872-131">How do I securely ship a certificate toohello VM?</span></span> <span data-ttu-id="68872-132">Jak můžu zřídit toorun sady škálování virtuálního počítače web hello SSL pro web hello je dodán bezpečně z konfigurace certifikátu?</span><span class="sxs-lookup"><span data-stu-id="68872-132">How do I provision a virtual machine scale set toorun a website where hello SSL for hello website is shipped securely from a certificate configuration?</span></span> <span data-ttu-id="68872-133">(hello běžné operace certifikátu otočení by být téměř hello stejné jako operace aktualizace konfigurace.) Máte příklad toodo to?</span><span class="sxs-lookup"><span data-stu-id="68872-133">(hello common certificate rotation operation would be almost hello same as a configuration update operation.) Do you have an example of how toodo this?</span></span> 

<span data-ttu-id="68872-134">toosecurely dodávat certifikát toohello virtuálních počítačů, může nainstalovat certifikát zákazníka přímo do úložiště certifikátů systému Windows z trezoru klíčů hello zákazníka.</span><span class="sxs-lookup"><span data-stu-id="68872-134">toosecurely ship a certificate toohello VM, you can install a customer certificate directly into a Windows certificate store from hello customer's key vault.</span></span>

<span data-ttu-id="68872-135">Použijte následující JSON hello:</span><span class="sxs-lookup"><span data-stu-id="68872-135">Use hello following JSON:</span></span>

```json
"secrets": [
    {
        "sourceVault": {
            "id": "/subscriptions/{subscriptionid}/resourceGroups/myrg1/providers/Microsoft.KeyVault/vaults/mykeyvault1"
        },
        "vaultCertificates": [
            {
                "certificateUrl": "https://mykeyvault1.vault.azure.net/secrets/{secretname}/{secret-version}",
                "certificateStore": "certificateStoreName"
            }
        ]
    }
]
```

<span data-ttu-id="68872-136">Kód Hello podporuje Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="68872-136">hello code supports Windows and Linux.</span></span>

<span data-ttu-id="68872-137">Další informace najdete v tématu [vytvoření nebo aktualizace škálovací sady virtuálních počítačů](https://msdn.microsoft.com/library/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="68872-137">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/mt589035.aspx).</span></span>


### <a name="example-of-self-signed-certificate"></a><span data-ttu-id="68872-138">Příklad certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="68872-138">Example of Self-signed certificate</span></span>

1.  <span data-ttu-id="68872-139">Vytvořte certifikát podepsaný svým držitelem v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="68872-139">Create a self-signed certificate in a key vault.</span></span>

    <span data-ttu-id="68872-140">Použijte následující příkazy prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="68872-140">Use hello following PowerShell commands:</span></span>

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    <span data-ttu-id="68872-141">Tento příkaz poskytuje hello vstup pro šablonu Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="68872-141">This command gives you hello input for hello Azure Resource Manager template.</span></span>

    <span data-ttu-id="68872-142">Příklad jak toocreate certifikát podepsaný svým držitelem v trezoru klíčů, najdete v části [scénáře zabezpečení clusteru Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span><span class="sxs-lookup"><span data-stu-id="68872-142">For an example of how toocreate a self-signed certificate in a key vault, see [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span></span>

2.  <span data-ttu-id="68872-143">Změnit hello šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="68872-143">Change hello Resource Manager template.</span></span>

    <span data-ttu-id="68872-144">Přidejte tuto vlastnost příliš**virtualMachineProfile**, jako součást hello škálovací sady virtuálních počítačů prostředků:</span><span class="sxs-lookup"><span data-stu-id="68872-144">Add this property too**virtualMachineProfile**, as part of hello virtual machine scale set resource:</span></span>

    ```json 
    "osProfile": {
        "computerNamePrefix": "[variables('namingInfix')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "secrets": [
            {
                "sourceVault": {
                    "id": "[resourceId('KeyVault', 'Microsoft.KeyVault/vaults', 'MikhegnVault')]"
                },
                "vaultCertificates": [
                    {
                        "certificateUrl": "https://mikhegnvault.vault.azure.net:443/secrets/VMSSCert/20709ca8faee4abb84bc6f4611b088a4",
                        "certificateStore": "My"
                    }
                ]
            }
        ]
    }
    ```
  

### <a name="can-i-specify-an-ssh-key-pair-toouse-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a><span data-ttu-id="68872-145">Můžete zadat toouse pár klíčů SSH pro ověřování SSH s měřítkem virtuální počítač Linux nastavte ze šablony Resource Manageru?</span><span class="sxs-lookup"><span data-stu-id="68872-145">Can I specify an SSH key pair toouse for SSH authentication with a Linux virtual machine scale set from a Resource Manager template?</span></span>  

<span data-ttu-id="68872-146">Ano.</span><span class="sxs-lookup"><span data-stu-id="68872-146">Yes.</span></span> <span data-ttu-id="68872-147">Hello REST API pro **osProfile** je podobné toohello standardní virtuálních počítačů REST API.</span><span class="sxs-lookup"><span data-stu-id="68872-147">hello REST API for **osProfile** is similar toohello standard VM REST API.</span></span> 

<span data-ttu-id="68872-148">Zahrnout **osProfile** v šabloně:</span><span class="sxs-lookup"><span data-stu-id="68872-148">Include **osProfile** in your template:</span></span>

```json 
"osProfile": {
    "computerName": "[variables('vmName')]",
    "adminUsername": "[parameters('adminUserName')]",
    "linuxConfiguration": {
        "disablePasswordAuthentication": "true",
        "ssh": {
            "publicKeys": [
                {
                    "path": "[variables('sshKeyPath')]",
                    "keyData": "[parameters('sshKeyData')]"
                }
            ]
        }
    }
}
```
 
<span data-ttu-id="68872-149">Tento blok JSON se používá v [hello 101-vm-sshkey Githubu úvodní šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="68872-149">This JSON block is used in [hello 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>
 
<span data-ttu-id="68872-150">Hello profil operačního systému se také používá při [hello grelayhost.json Githubu rychlý start šablony](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span><span class="sxs-lookup"><span data-stu-id="68872-150">hello OS profile also is used in [hello grelayhost.json GitHub quick start template](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span></span>

<span data-ttu-id="68872-151">Další informace najdete v tématu [vytvoření nebo aktualizace škálovací sady virtuálních počítačů](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span><span class="sxs-lookup"><span data-stu-id="68872-151">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span></span>
  

### <a name="how-do-i-remove-deprecated-certificates"></a><span data-ttu-id="68872-152">Jak odstranit zastaralé certifikáty?</span><span class="sxs-lookup"><span data-stu-id="68872-152">How do I remove deprecated certificates?</span></span> 

<span data-ttu-id="68872-153">tooremove zastaralé certifikáty, odeberte hello původní certifikát ze seznamu certifikátů hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="68872-153">tooremove deprecated certificates, remove hello old certificate from hello vault certificates list.</span></span> <span data-ttu-id="68872-154">Ponechte všechny certifikáty hello chcete tooremain ve vašem počítači v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="68872-154">Leave all hello certificates that you want tooremain on your computer in hello list.</span></span> <span data-ttu-id="68872-155">Hello certifikát nebude odstraněn ze všech virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-155">This does not remove hello certificate from all your VMs.</span></span> <span data-ttu-id="68872-156">Také nepřidá hello certifikát toonew virtuálních počítačů, které jsou vytvořené v sadě škálování virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="68872-156">It also does not add hello certificate toonew VMs that are created in hello virtual machine scale set.</span></span> 

<span data-ttu-id="68872-157">tooremove hello certifikát z existujících virtuálních počítačů, napsat vlastní skript rozšíření toomanually odebrat hello certifikáty z úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="68872-157">tooremove hello certificate from existing VMs, write a custom script extension toomanually remove hello certificates from your certificate store.</span></span>
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-hello-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-toostore-hello-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a><span data-ttu-id="68872-158">Jak I vložit existující veřejný klíč SSH do hello virtuálního počítače škálování sadu SSH vrstvy při zřizování?</span><span class="sxs-lookup"><span data-stu-id="68872-158">How do I inject an existing SSH public key into hello virtual machine scale set SSH layer during provisioning?</span></span> <span data-ttu-id="68872-159">Chcete toostore hello SSH veřejné hodnoty klíče v Azure Key Vault a potom je použít v šabloně moje šablona Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="68872-159">I want toostore hello SSH public key values in Azure Key Vault, and then use them in my Resource Manager template.</span></span>

<span data-ttu-id="68872-160">Pokud zadáte hello virtuální počítače pouze pomocí veřejného klíče SSH, nepotřebujete tooput hello veřejné klíče v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="68872-160">If you are providing hello VMs only with a public SSH key, you don't need tooput hello public keys in Key Vault.</span></span> <span data-ttu-id="68872-161">Veřejné klíče nejsou tajné.</span><span class="sxs-lookup"><span data-stu-id="68872-161">Public keys are not secret.</span></span>
 
<span data-ttu-id="68872-162">Veřejné klíče SSH ve formátu prostého textu můžete zadat při vytváření virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="68872-162">You can provide SSH public keys in plain text when you create a Linux VM:</span></span>

```json
"linuxConfiguration": {
    "ssh": {
        "publicKeys": [
            {
                "path": "path",
                "keyData": "publickey"
            }
        ]
    }
```
 
<span data-ttu-id="68872-163">Název elementu linuxConfiguration</span><span class="sxs-lookup"><span data-stu-id="68872-163">linuxConfiguration element name</span></span> | <span data-ttu-id="68872-164">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="68872-164">Required</span></span> | <span data-ttu-id="68872-165">Typ</span><span class="sxs-lookup"><span data-stu-id="68872-165">Type</span></span> | <span data-ttu-id="68872-166">Popis</span><span class="sxs-lookup"><span data-stu-id="68872-166">Description</span></span>
--- | --- | --- | --- |  ---
<span data-ttu-id="68872-167">SSH</span><span class="sxs-lookup"><span data-stu-id="68872-167">ssh</span></span> | <span data-ttu-id="68872-168">Ne</span><span class="sxs-lookup"><span data-stu-id="68872-168">No</span></span> | <span data-ttu-id="68872-169">Kolekce</span><span class="sxs-lookup"><span data-stu-id="68872-169">Collection</span></span> | <span data-ttu-id="68872-170">Určuje hello klíče konfigurace SSH pro operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="68872-170">Specifies hello SSH key configuration for a Linux OS</span></span>
<span data-ttu-id="68872-171">Cesta</span><span class="sxs-lookup"><span data-stu-id="68872-171">path</span></span> | <span data-ttu-id="68872-172">Ano</span><span class="sxs-lookup"><span data-stu-id="68872-172">Yes</span></span> | <span data-ttu-id="68872-173">Řetězec</span><span class="sxs-lookup"><span data-stu-id="68872-173">String</span></span> | <span data-ttu-id="68872-174">Určuje, kde klíče SSH hello nebo certifikát by měl být umístěn cesta k souboru hello Linux</span><span class="sxs-lookup"><span data-stu-id="68872-174">Specifies hello Linux file path where hello SSH keys or certificate should be located</span></span>
<span data-ttu-id="68872-175">data klíče</span><span class="sxs-lookup"><span data-stu-id="68872-175">keyData</span></span> | <span data-ttu-id="68872-176">Ano</span><span class="sxs-lookup"><span data-stu-id="68872-176">Yes</span></span> | <span data-ttu-id="68872-177">Řetězec</span><span class="sxs-lookup"><span data-stu-id="68872-177">String</span></span> | <span data-ttu-id="68872-178">Určuje kódování base64 veřejný klíč SSH</span><span class="sxs-lookup"><span data-stu-id="68872-178">Specifies a base64-encoded SSH public key</span></span>

<span data-ttu-id="68872-179">Příklad, naleznete v části [hello 101-vm-sshkey Githubu úvodní šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="68872-179">For an example, see [hello 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-hello-same-key-vault-i-see-hello-following-message"></a><span data-ttu-id="68872-180">Při spuštění `Update-AzureRmVmss` po přidání více než jeden certifikát z hello stejný klíč trezoru, vidím hello následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="68872-180">When I run `Update-AzureRmVmss` after adding more than one certificate from hello same key vault, I see hello following message:</span></span>
 
><span data-ttu-id="68872-181">Update-AzureRmVmss: Tajný klíč seznam obsahuje opakované instance /subscriptions/ < my-subscription-id > / resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, což je zakázaná.</span><span class="sxs-lookup"><span data-stu-id="68872-181">Update-AzureRmVmss: List secret contains repeated instances of /subscriptions/<my-subscription-id>/resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, which is disallowed.</span></span>
 
<span data-ttu-id="68872-182">K tomu může dojít, pokud se pokusíte toore-přidat hello stejné místo použití nového certifikátu trezoru pro existující trezor zdroj hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="68872-182">This can happen if you try toore-add hello same vault instead of using a new vault certificate for hello existing source vault.</span></span> <span data-ttu-id="68872-183">Hello `Add-AzureRmVmssSecret` příkaz nefunguje správně při přidávání dalších tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="68872-183">hello `Add-AzureRmVmssSecret` command does not work correctly if you are adding additional secrets.</span></span>
 
<span data-ttu-id="68872-184">tooadd další tajné klíče z hello stejné trezoru klíčů, seznam $vmss.properties.osProfile.secrets[0].vaultCertificates hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="68872-184">tooadd more secrets from hello same key vault, update hello $vmss.properties.osProfile.secrets[0].vaultCertificates list.</span></span>
 
<span data-ttu-id="68872-185">Hello očekávané vstupní struktuře, najdete v části [vytvoření nebo aktualizaci virtuálního počítače nastavit](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="68872-185">For hello expected input structure, see [Create or update a virtual machine set](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span></span>
 
<span data-ttu-id="68872-186">V hello virtuálního počítače škálování sadu objekt, který je v trezoru klíčů hello najít hello tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="68872-186">Find hello secret in hello virtual machine scale set object that is in hello key vault.</span></span> <span data-ttu-id="68872-187">Pak přidejte seznamu toohello odkaz (hello adresy URL a název tajný úložiště hello) certifikát přidružený k trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="68872-187">Then, add your certificate reference (hello URL and hello secret store name) toohello list associated with hello vault.</span></span>

> [!NOTE] 
> <span data-ttu-id="68872-188">V současné době nelze odebrat certifikáty z virtuálních počítačů pomocí hello rozhraní API sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68872-188">Currently, you cannot remove certificates from VMs by using hello virtual machine scale set API.</span></span>
>

<span data-ttu-id="68872-189">Nové virtuální počítače nebudou mít hello starý certifikát.</span><span class="sxs-lookup"><span data-stu-id="68872-189">New VMs will not have hello old certificate.</span></span> <span data-ttu-id="68872-190">Virtuální počítače, které jsou už nasazené, které mají certifikát hello však bude mít hello starý certifikát.</span><span class="sxs-lookup"><span data-stu-id="68872-190">However, VMs that have hello certificate and which are already deployed will have hello old certificate.</span></span>
 
### <a name="can-i-push-certificates-toohello-virtual-machine-scale-set-without-providing-hello-password-when-hello-certificate-is-in-hello-secret-store"></a><span data-ttu-id="68872-191">Můžete posouvat škálování virtuálních počítačů toohello certifikáty nastavit bez nutnosti hello hesla, je-li hello certifikát v úložišti tajný hello?</span><span class="sxs-lookup"><span data-stu-id="68872-191">Can I push certificates toohello virtual machine scale set without providing hello password, when hello certificate is in hello secret store?</span></span>

<span data-ttu-id="68872-192">Není nutné toohard kód hesla ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="68872-192">You do not need toohard-code passwords in scripts.</span></span> <span data-ttu-id="68872-193">Můžete dynamicky načíst hesla s oprávněními hello použijete skript nasazení toorun hello.</span><span class="sxs-lookup"><span data-stu-id="68872-193">You can dynamically retrieve passwords with hello permissions you use toorun hello deployment script.</span></span> <span data-ttu-id="68872-194">Pokud máte skript, který certifikát se přesouvají z trezoru klíčů hello tajný úložiště, hello tajný úložiště `get certificate` příkaz také výstupy hello heslo souboru PFX hello.</span><span class="sxs-lookup"><span data-stu-id="68872-194">If you have a script that moves a certificate from hello secret store key vault, hello secret store `get certificate` command also outputs hello password of hello .pfx file.</span></span>
 
### <a name="how-does-hello-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-hello-sourcevault-value-when-i-have-toospecify-hello-absolute-uri-for-a-certificate-by-using-hello-certificateurl-property"></a><span data-ttu-id="68872-195">Jak nastavit vlastnost tajné klíče hello virtualMachineProfile.osProfile pro škálování virtuálních počítačů pracovní?</span><span class="sxs-lookup"><span data-stu-id="68872-195">How does hello Secrets property of virtualMachineProfile.osProfile for a virtual machine scale set work?</span></span> <span data-ttu-id="68872-196">Proč musím hello sourceVault hodnotu, pokud toospecify hello absolutní identifikátor URI pro certifikát pomocí vlastnosti certificateUrl hello?</span><span class="sxs-lookup"><span data-stu-id="68872-196">Why do I need hello sourceVault value when I have toospecify hello absolute URI for a certificate by using hello certificateUrl property?</span></span> 

<span data-ttu-id="68872-197">Odkaz certifikát Vzdálená správa systému Windows (WinRM) se musí nacházet v hello tajných klíčů vlastnost hello profil operačního systému.</span><span class="sxs-lookup"><span data-stu-id="68872-197">A Windows Remote Management (WinRM) certificate reference must be present in hello Secrets property of hello OS profile.</span></span> 

<span data-ttu-id="68872-198">účelem Hello označující trezoru zdroj hello je tooenforce přístup ovládacího prvku seznam (ACL) zásad, které existují v modelu Azure Cloud Service uživatele.</span><span class="sxs-lookup"><span data-stu-id="68872-198">hello purpose of indicating hello source vault is tooenforce access control list (ACL) policies that exist in a user's Azure Cloud Service model.</span></span> <span data-ttu-id="68872-199">Pokud není zadán hello zdroj trezoru, uživatelé, kteří nemají oprávnění toodeploy nebo přístup tajné klíče tooa trezoru klíčů by mít toothrough výpočetní prostředek zprostředkovatele (CRP).</span><span class="sxs-lookup"><span data-stu-id="68872-199">If hello source vault isn't specified, users who do not have permissions toodeploy or access secrets tooa key vault would be able toothrough a Compute Resource Provider (CRP).</span></span> <span data-ttu-id="68872-200">Seznamy ACL existují i pro prostředky, které nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="68872-200">ACLs exist even for resources that do not exist.</span></span>

<span data-ttu-id="68872-201">Pokud zadáte ID trezoru nesprávný zdrojový ale adresa URL platná trezoru klíčů, dojde k chybě při dotazování hello operaci.</span><span class="sxs-lookup"><span data-stu-id="68872-201">If you provide an incorrect source vault ID but a valid key vault URL, an error is reported when you poll hello operation.</span></span>
 
### <a name="if-i-add-secrets-tooan-existing-virtual-machine-scale-set-are-hello-secrets-injected-into-existing-vms-or-only-into-new-ones"></a><span data-ttu-id="68872-202">Pokud lze přidat existující škálovací sadu virtuálních počítačů tooan tajné klíče, jsou tajné klíče hello vložit do existujících virtuálních počítačů, nebo pouze do nové?</span><span class="sxs-lookup"><span data-stu-id="68872-202">If I add secrets tooan existing virtual machine scale set, are hello secrets injected into existing VMs, or only into new ones?</span></span> 

<span data-ttu-id="68872-203">Certifikáty jsou přidány tooall virtuální počítače, i těch, které jsou již existující.</span><span class="sxs-lookup"><span data-stu-id="68872-203">Certificates are added tooall your VMs, even preexisting ones.</span></span> <span data-ttu-id="68872-204">Pokud vaše škálovací sady virtuálních počítačů upgradePolicy vlastnost se nastaví příliš**ruční**, hello certifikát bude přidán toohello virtuálních počítačů, když provádíte ruční aktualizace na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-204">If your virtual machine scale set upgradePolicy property is set too**manual**, hello certificate is added toohello VM when you perform a manual update on hello VM.</span></span>
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a><span data-ttu-id="68872-205">Kde umístit certifikáty pro virtuální počítače s Linuxem?</span><span class="sxs-lookup"><span data-stu-id="68872-205">Where do I put certificates for Linux VMs?</span></span>

<span data-ttu-id="68872-206">jak zjistit, toodeploy certifikáty pro virtuální počítače s Linuxem, toolearn [nasadit certifikáty tooVMs z trezoru klíčů spravované zákazníkem](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="68872-206">toolearn how toodeploy certificates for Linux VMs, see [Deploy certificates tooVMs from a customer-managed key vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span></span>
  
### <a name="how-do-i-add-a-new-vault-certificate-tooa-new-certificate-object"></a><span data-ttu-id="68872-207">Jak přidat nový trezor certifikát tooa nový certifikát objekt?</span><span class="sxs-lookup"><span data-stu-id="68872-207">How do I add a new vault certificate tooa new certificate object?</span></span>

<span data-ttu-id="68872-208">tooadd trezoru certifikát tooan existující tajný klíč, viz následující příklad PowerShell text hello.</span><span class="sxs-lookup"><span data-stu-id="68872-208">tooadd a vault certificate tooan existing secret, see hello following PowerShell example.</span></span> <span data-ttu-id="68872-209">Použijte pouze jeden objekt tajný.</span><span class="sxs-lookup"><span data-stu-id="68872-209">Use only one secret object.</span></span>
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-toocertificates-if-you-reimage-a-vm"></a><span data-ttu-id="68872-210">Co se stane toocertificates Pokud obnovení z Image virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="68872-210">What happens toocertificates if you reimage a VM?</span></span>

<span data-ttu-id="68872-211">Pokud se obnovení z Image virtuálního počítače, certifikáty se odstraní.</span><span class="sxs-lookup"><span data-stu-id="68872-211">If you reimage a VM, certificates are deleted.</span></span> <span data-ttu-id="68872-212">Obnovování odstranění hello celý disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="68872-212">Reimaging deletes hello entire OS disk.</span></span> 
 
### <a name="what-happens-if-you-delete-a-certificate-from-hello-key-vault"></a><span data-ttu-id="68872-213">Co se stane, když odstranit certifikát z trezoru klíčů hello?</span><span class="sxs-lookup"><span data-stu-id="68872-213">What happens if you delete a certificate from hello key vault?</span></span>

<span data-ttu-id="68872-214">Pokud z trezoru klíčů hello se odstraní hello tajný klíč, a pak spusťte `stop deallocate` pro všechny virtuální počítače a potom je znovu spusťte, dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="68872-214">If hello secret is deleted from hello key vault, and then you run `stop deallocate` for all your VMs and then start them again, you will encounter a failure.</span></span> <span data-ttu-id="68872-215">dojde k chybě Hello, protože hello CRP vyžaduje tooretrieve hello tajné klíče z trezoru klíčů hello, ale ji nelze.</span><span class="sxs-lookup"><span data-stu-id="68872-215">hello failure occurs because hello CRP needs tooretrieve hello secrets from hello key vault, but it cannot.</span></span> <span data-ttu-id="68872-216">V tomto scénáři můžete odstranit z modelu sady škálování virtuálního počítače hello hello certifikáty.</span><span class="sxs-lookup"><span data-stu-id="68872-216">In this scenario, you can delete hello certificates from hello virtual machine scale set model.</span></span> 

<span data-ttu-id="68872-217">součást CRP Hello není zachována tajné klíče zákazníků.</span><span class="sxs-lookup"><span data-stu-id="68872-217">hello CRP component does not persist customer secrets.</span></span> <span data-ttu-id="68872-218">Pokud spustíte `stop deallocate` pro všechny virtuální počítače ve škálovací sadu virtuálních počítačů hello hello mezipaměť je Odstraněná.</span><span class="sxs-lookup"><span data-stu-id="68872-218">If you run `stop deallocate` for all VMs in hello virtual machine scale set, hello cache is deleted.</span></span> <span data-ttu-id="68872-219">V tomto scénáři se načítají tajné klíče z trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="68872-219">In this scenario, secrets are retrieved from hello key vault.</span></span>

<span data-ttu-id="68872-220">Tento problém není dojde při škálování, protože není v mezipaměti kopii hello tajný klíč v Azure Service Fabric (v modelu hello fabric jednoho klienta).</span><span class="sxs-lookup"><span data-stu-id="68872-220">You don't encounter this problem when scaling out because there is a cached copy of hello secret in Azure Service Fabric (in hello single-fabric tenant model).</span></span>
 
### <a name="why-do-i-have-toospecify-hello-exact-location-for-hello-certificate-url-httpsname-of-hello-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a><span data-ttu-id="68872-221">Proč musím toospecify hello přesné umístění pro adresa URL certifikátu hello (https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>), jak je uvedeno v [scénáře zabezpečení clusteru Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span><span class="sxs-lookup"><span data-stu-id="68872-221">Why do I have toospecify hello exact location for hello certificate URL (https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>), as indicated in [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span></span>
 
<span data-ttu-id="68872-222">Hello dokumentace Azure Key Vault stavy tohoto rozhraní API REST získání tajného klíče by měla vrátit hello nejnovější verzi hello tajný klíč, pokud není určená verze hello hello.</span><span class="sxs-lookup"><span data-stu-id="68872-222">hello Azure Key Vault documentation states that hello Get Secret REST API should return hello latest version of hello secret if hello version is not specified.</span></span>
 
<span data-ttu-id="68872-223">Metoda</span><span class="sxs-lookup"><span data-stu-id="68872-223">Method</span></span> | <span data-ttu-id="68872-224">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="68872-224">URL</span></span>
--- | ---
<span data-ttu-id="68872-225">GET</span><span class="sxs-lookup"><span data-stu-id="68872-225">GET</span></span> | <span data-ttu-id="68872-226">https://mykeyvault.Vault.Azure.NET/secrets/ {tajný klíč name} / {tajný klíč version}? api-version = {api-version}</span><span class="sxs-lookup"><span data-stu-id="68872-226">https://mykeyvault.vault.azure.net/secrets/{secret-name}/{secret-version}?api-version={api-version}</span></span>

<span data-ttu-id="68872-227">Nahraďte {*tajný klíč název*} s názvem hello a nahraďte {*tajný klíč verze*} verzí hello tajného klíče hello chcete tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="68872-227">Replace {*secret-name*} with hello name, and replace {*secret-version*} with hello version of hello secret you want tooretrieve.</span></span> <span data-ttu-id="68872-228">verzi tajného klíče Hello může být vyloučeny.</span><span class="sxs-lookup"><span data-stu-id="68872-228">hello secret version might be excluded.</span></span> <span data-ttu-id="68872-229">V takovém případě se načte aktuální verze hello.</span><span class="sxs-lookup"><span data-stu-id="68872-229">In that case, hello current version is retrieved.</span></span>
  
### <a name="why-do-i-have-toospecify-hello-certificate-version-when-i-use-key-vault"></a><span data-ttu-id="68872-230">Proč se při použití Key Vault mají toospecify hello certifikátu verze?</span><span class="sxs-lookup"><span data-stu-id="68872-230">Why do I have toospecify hello certificate version when I use Key Vault?</span></span>

<span data-ttu-id="68872-231">účelem Hello hello Key Vault požadavek toospecify hello certifikátu verze je toomake vymazat toohello uživatele co certifikát je nasazený na jejich virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="68872-231">hello purpose of hello Key Vault requirement toospecify hello certificate version is toomake it clear toohello user what certificate is deployed on their VMs.</span></span>

<span data-ttu-id="68872-232">Pokud vytvoříte virtuální počítač a pak aktualizujte váš tajný klíč v trezoru klíčů hello, není nový certifikát hello stažené tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-232">If you create a VM and then update your secret in hello key vault, hello new certificate is not downloaded tooyour VMs.</span></span> <span data-ttu-id="68872-233">Ale virtuální počítače se tooreference a nové virtuální počítače získat hello nový sdílený tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="68872-233">But your VMs appear tooreference it, and new VMs get hello new secret.</span></span> <span data-ttu-id="68872-234">tooavoid to jsou požadované tooreference verzi tajného klíče.</span><span class="sxs-lookup"><span data-stu-id="68872-234">tooavoid this, you are required tooreference a secret version.</span></span>

### <a name="my-team-works-with-several-certificates-that-are-distributed-toous-as-cer-public-keys-what-is-hello-recommended-approach-for-deploying-these-certificates-tooa-virtual-machine-scale-set"></a><span data-ttu-id="68872-235">Můj tým pracuje s několik certifikátů, které jsou distribuovány toous jako .cer veřejných klíčů.</span><span class="sxs-lookup"><span data-stu-id="68872-235">My team works with several certificates that are distributed toous as .cer public keys.</span></span> <span data-ttu-id="68872-236">Co je hello doporučenému přístupu pro nasazení škálovací sadu virtuálních počítačů tooa tyto certifikáty?</span><span class="sxs-lookup"><span data-stu-id="68872-236">What is hello recommended approach for deploying these certificates tooa virtual machine scale set?</span></span>

<span data-ttu-id="68872-237">toodeploy .cer veřejné klíče tooa škálovací sadu virtuálních počítačů, můžete generovat soubor .pfx, který obsahuje pouze soubory s příponou CER.</span><span class="sxs-lookup"><span data-stu-id="68872-237">toodeploy .cer public keys tooa virtual machine scale set, you can generate a .pfx file that contains only .cer files.</span></span> <span data-ttu-id="68872-238">toodo tuto, použijte `X509ContentType = Pfx`.</span><span class="sxs-lookup"><span data-stu-id="68872-238">toodo this, use `X509ContentType = Pfx`.</span></span> <span data-ttu-id="68872-239">Například načíst soubor .cer hello jako objekt x509Certificate2 v C# nebo prostředí PowerShell a potom volejte metodu hello.</span><span class="sxs-lookup"><span data-stu-id="68872-239">For example, load hello .cer file as an x509Certificate2 object in C# or PowerShell, and then call hello method.</span></span> 

<span data-ttu-id="68872-240">Další informace najdete v tématu [X509Certificate.Export – metoda (X509ContentType, řetězec)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span><span class="sxs-lookup"><span data-stu-id="68872-240">For more information, see [X509Certificate.Export Method (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span></span>

### <a name="i-do-not-see-an-option-for-users-toopass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a><span data-ttu-id="68872-241">Možnost uživatelé toopass v certifikátech nezobrazí jako řetězce formátu base64.</span><span class="sxs-lookup"><span data-stu-id="68872-241">I do not see an option for users toopass in certificates as base64 strings.</span></span> <span data-ttu-id="68872-242">Tato možnost k dispozici většina jiných poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="68872-242">Most other resource providers have this option.</span></span>

<span data-ttu-id="68872-243">tooemulate předávání v certifikátu jako řetězec base64 můžete rozbalit hello nejnovější verzí adresy URL v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="68872-243">tooemulate passing in a certificate as a base64 string, you can extract hello latest versioned URL in a Resource Manager template.</span></span> <span data-ttu-id="68872-244">Zahrnout hello následující vlastnosti JSON v šabloně Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="68872-244">Include hello following JSON property in your Resource Manager template:</span></span>

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-toowrap-certificates-in-json-objects-in-key-vaults"></a><span data-ttu-id="68872-245">V objektech JSON v trezorů klíčů jsou toowrap certifikáty?</span><span class="sxs-lookup"><span data-stu-id="68872-245">Do I have toowrap certificates in JSON objects in key vaults?</span></span>

<span data-ttu-id="68872-246">Sady škálování virtuálního počítače a virtuální počítače musí být uzavřen certifikáty do objektů JSON.</span><span class="sxs-lookup"><span data-stu-id="68872-246">In virtual machine scale sets and VMs, certificates must be wrapped in JSON objects.</span></span> 

<span data-ttu-id="68872-247">Také podporujeme hello typ obsahu application/x-pkcs12.</span><span class="sxs-lookup"><span data-stu-id="68872-247">We also support hello content type application/x-pkcs12.</span></span> <span data-ttu-id="68872-248">Pokyny k používání application/x-pkcs12 najdete v tématu [certifikáty PFX v Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="68872-248">For instructions on using application/x-pkcs12, see [PFX certificates in Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span></span>
 
<span data-ttu-id="68872-249">Aktuálně nepodporujeme souborů s příponou CER.</span><span class="sxs-lookup"><span data-stu-id="68872-249">We currently do not support .cer files.</span></span> <span data-ttu-id="68872-250">soubory s příponou CER toouse, je exportovat do kontejnerů .pfx.</span><span class="sxs-lookup"><span data-stu-id="68872-250">toouse .cer files, export them into .pfx containers.</span></span>



## <a name="compliance"></a><span data-ttu-id="68872-251">Dodržování předpisů</span><span class="sxs-lookup"><span data-stu-id="68872-251">Compliance</span></span>

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a><span data-ttu-id="68872-252">Jsou kompatibilní se standardem PCI sadách škálování virtuálních počítačů?</span><span class="sxs-lookup"><span data-stu-id="68872-252">Are virtual machine scale sets PCI-compliant?</span></span>

<span data-ttu-id="68872-253">Sady škálování virtuálního počítače jsou dynamické vrstvu rozhraní API nad hello CRP.</span><span class="sxs-lookup"><span data-stu-id="68872-253">Virtual machine scale sets are a thin API layer on top of hello CRP.</span></span> <span data-ttu-id="68872-254">Obě komponenty jsou součástí hello výpočetní platforma v hello Azure stromu služby.</span><span class="sxs-lookup"><span data-stu-id="68872-254">Both components are part of hello compute platform in hello Azure service tree.</span></span>

<span data-ttu-id="68872-255">Z hlediska dodržování předpisů sady škálování virtuálního počítače jsou základní součástí hello výpočetní platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="68872-255">From a compliance perspective, virtual machine scale sets are a fundamental part of hello Azure compute platform.</span></span> <span data-ttu-id="68872-256">Tým, nástroje, procesy, metody nasazení, kontrolních mechanismů pro zabezpečení, za běhu (JIT) kompilace, monitorování, výstrahy a tak dále, sdílejí s hello CRP sám sebe.</span><span class="sxs-lookup"><span data-stu-id="68872-256">They share a team, tools, processes, deployment methodology, security controls, just-in-time (JIT) compilation, monitoring, alerting, and so on, with hello CRP itself.</span></span> <span data-ttu-id="68872-257">Sady škálování virtuálního počítače jsou odvětví platebních karet (PCI)-kompatibilní, protože hello CRP je součástí hello aktuální ověření PCI Data zabezpečení DSS (Standard).</span><span class="sxs-lookup"><span data-stu-id="68872-257">Virtual machine scale sets are Payment Card Industry (PCI)-compliant because hello CRP is part of hello current PCI Data Security Standard (DSS) attestation.</span></span>

<span data-ttu-id="68872-258">Další informace najdete v tématu [hello Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span><span class="sxs-lookup"><span data-stu-id="68872-258">For more information, see [hello Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span></span>






## <a name="extensions"></a><span data-ttu-id="68872-259">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="68872-259">Extensions</span></span>

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a><span data-ttu-id="68872-260">Jak lze odstranit rozšíření sady škálování virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="68872-260">How do I delete a virtual machine scale set extension?</span></span>

<span data-ttu-id="68872-261">toodelete škálování virtuálního počítače nastavit rozšíření, hello použijte následující příklad PowerShell:</span><span class="sxs-lookup"><span data-stu-id="68872-261">toodelete a virtual machine scale set extension, use hello following PowerShell example:</span></span>

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
<span data-ttu-id="68872-262">Můžete najít hodnotu Název_rozšíření hello v `$vmss`.</span><span class="sxs-lookup"><span data-stu-id="68872-262">You can find hello extensionName value in `$vmss`.</span></span>
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a><span data-ttu-id="68872-263">Je k dispozici, že že škálovací sady virtuálních počítačů příklad šablony, který se integruje s Operations Management Suite?</span><span class="sxs-lookup"><span data-stu-id="68872-263">Is there a virtual machine scale set template example that integrates with Operations Management Suite?</span></span>

<span data-ttu-id="68872-264">Pro škálování virtuálních počítačů, nastavte příklad šablony, který se integruje s Operations Management Suite, najdete v části hello druhém příkladu v [nasazení clusteru Azure Service Fabric a povolte monitorování pomocí analýzy protokolů](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span><span class="sxs-lookup"><span data-stu-id="68872-264">For a virtual machine scale set template example that integrates with Operations Management Suite, see hello second example in [Deploy an Azure Service Fabric cluster and enable monitoring by using Log Analytics](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span></span>
   
### <a name="extensions-seem-toorun-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-toofail-what-can-i-do-toofix-this"></a><span data-ttu-id="68872-265">Rozšíření zdá se, že toorun paralelně na sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68872-265">Extensions seem toorun in parallel on virtual machine scale sets.</span></span> <span data-ttu-id="68872-266">To způsobí, že moje toofail rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="68872-266">This causes my custom script extension toofail.</span></span> <span data-ttu-id="68872-267">Jak toofix to?</span><span class="sxs-lookup"><span data-stu-id="68872-267">What can I do toofix this?</span></span>

<span data-ttu-id="68872-268">toolearn o rozšíření sekvencování v sady škálování virtuálního počítače, najdete v části [rozšíření sekvencování v sady škálování virtuálního počítače Azure](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="68872-268">toolearn about extension sequencing in virtual machine scale sets, see [Extension sequencing in Azure virtual machine scale sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span></span>
 
 
### <a name="how-do-i-reset-hello-password-for-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="68872-269">Jak I reset hello heslo pro virtuální počítače v mé škálovací sadu virtuálních počítačů?</span><span class="sxs-lookup"><span data-stu-id="68872-269">How do I reset hello password for VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="68872-270">tooreset hello heslo pro virtuální počítače ve vaší škálování virtuálních počítačů nastavení, použijte rozšíření virtuálního počítače přístup.</span><span class="sxs-lookup"><span data-stu-id="68872-270">tooreset hello password for VMs in your virtual machine scale set, use VM access extensions.</span></span> 

<span data-ttu-id="68872-271">Použijte následující příklad PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="68872-271">Use hello following PowerShell example:</span></span>

```powershell
$vmssName = "myvmss"
$vmssResourceGroup = "myvmssrg"
$publicConfig = @{"UserName" = "newuser"}
$privateConfig = @{"Password" = "********"}
 
$extName = "VMAccessAgent"
$publisher = "Microsoft.Compute"
$vmss = Get-AzureRmVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
$vmss = Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
Update-AzureRmVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName -VirtualMachineScaleSet $vmss
```
 
 
### <a name="how-do-i-add-an-extension-tooall-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="68872-272">Jak přidat rozšíření tooall virtuální počítače v mé škálovací sadu virtuálních počítačů?</span><span class="sxs-lookup"><span data-stu-id="68872-272">How do I add an extension tooall VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="68872-273">Pokud zásady aktualizace je nastaven příliš**automatické**, opětovného nasazení šablony hello novými vlastnostmi rozšíření hello aktualizuje všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="68872-273">If update policy is set too**automatic**, redeploying hello template with hello new extension properties updates all VMs.</span></span>

<span data-ttu-id="68872-274">Pokud zásady aktualizace je nastaven příliš**ruční**, nejdřív aktualizovat hello rozšíření a potom ručně aktualizovat všechny instance ve virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-274">If update policy is set too**manual**, first update hello extension, and then manually update all instances in your VMs.</span></span>

  
### <a name="if-hello-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-hello-vms-not-match-hello-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-hello-scripts-that-are-currently-configured-on-hello-virtual-machine-scale-set-executed-or-are-hello-scripts-that-were-configured-when-hello-vm-was-first-created-used"></a><span data-ttu-id="68872-275">Pokud jsou aktualizovány hello rozšíření spojená se existující škálovací sadu virtuálních počítačů jsou existující virtuální počítače vliv?</span><span class="sxs-lookup"><span data-stu-id="68872-275">If hello extensions associated with an existing virtual machine scale set are updated, are existing VMs affected?</span></span> <span data-ttu-id="68872-276">(To znamená, budou virtuální počítače hello *není* modelu sady škálování virtuálního počítače hello shodu?) Nebo se budou ignorovat?</span><span class="sxs-lookup"><span data-stu-id="68872-276">(That is, will hello VMs *not* match hello virtual machine scale set model?) Or are they ignored?</span></span> <span data-ttu-id="68872-277">Pokud je existující počítač zacelené služby nebo obnovit z Image, jsou hello skripty, které jsou aktuálně nakonfigurované na škálovací sadu virtuálních počítačů hello provést, nebo se hello skripty, které byly nakonfigurovány při použití hello, ke které byl virtuální počítač nejprve vytvořen?</span><span class="sxs-lookup"><span data-stu-id="68872-277">When an existing machine is service-healed or reimaged, are hello scripts that are currently configured on hello virtual machine scale set executed, or are hello scripts that were configured when hello VM was first created used?</span></span>

<span data-ttu-id="68872-278">Pokud definice rozšíření hello v škálování virtuálních počítačů hello nastavený dojde k aktualizaci modelu a hello upgradePolicy je nastavena příliš**automatické**, aktualizuje hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-278">If hello extension definition in hello virtual machine scale set model is updated and hello upgradePolicy property is set too**automatic**, it updates hello VMs.</span></span> <span data-ttu-id="68872-279">Pokud je vlastnost upgradePolicy hello nastavena příliš**ruční**, rozšíření jsou označeny jako neodpovídá modelu hello.</span><span class="sxs-lookup"><span data-stu-id="68872-279">If hello upgradePolicy property is set too**manual**, extensions are flagged as not matching hello model.</span></span> 

<span data-ttu-id="68872-280">Pokud existující virtuální počítač zacelené služby, zobrazí se jako restartování a hello rozšíření nejsou znovu spusťte.</span><span class="sxs-lookup"><span data-stu-id="68872-280">If an existing VM is service-healed, it appears as a reboot, and hello extensions are not rerun.</span></span> <span data-ttu-id="68872-281">Pokud ho je obnovit z Image, je třeba nahraďte hello zdrojové bitové kopie disku hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="68872-281">If it is reimaged, it's like replacing hello OS drive with hello source image.</span></span> <span data-ttu-id="68872-282">Všechny specializace z nejnovější modelu hello, jako je například rozšíření, jsou spuštěny.</span><span class="sxs-lookup"><span data-stu-id="68872-282">Any specialization from hello latest model, such as extensions, are run.</span></span>
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-tooan-azure-ad-domain"></a><span data-ttu-id="68872-283">Jak připojit k doméně tooan Azure AD sady škálování virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="68872-283">How do I join a virtual machine scale set tooan Azure AD domain?</span></span>

<span data-ttu-id="68872-284">toojoin domény Azure Active Directory (Azure AD) tooan sady škálování virtuálního počítače, můžete definovat rozšíření.</span><span class="sxs-lookup"><span data-stu-id="68872-284">toojoin a virtual machine scale set tooan Azure Active Directory (Azure AD) domain, you can define an extension.</span></span> 

<span data-ttu-id="68872-285">toodefine rozšíření, použijte vlastnost JsonADDomainExtension hello:</span><span class="sxs-lookup"><span data-stu-id="68872-285">toodefine an extension, use hello JsonADDomainExtension property:</span></span>

```json
"extensionProfile": {
    "extensions": [
        {
            "name": "joindomain",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "JsonADDomainExtension",
                "typeHandlerVersion": "1.3",
                "settings": {
                    "Name": "[parameters('domainName')]",
                    "OUPath": "[variables('ouPath')]",
                    "User": "[variables('domainAndUsername')]",
                    "Restart": "true",
                    "Options": "[variables('domainJoinOptions')]"
                },
                "protectedsettings": {
                    "Password": "[parameters('domainJoinPassword')]"
                }
            }
        }
    ]
}
```
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-tooinstall-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a><span data-ttu-id="68872-286">Moje rozšíření sady škálování virtuálního počítače se pokouší tooinstall něco, co vyžaduje restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="68872-286">My virtual machine scale set extension is trying tooinstall something that requires a reboot.</span></span> <span data-ttu-id="68872-287">Například "commandToExecute": "powershell.exe - ExecutionPolicy Unrestricted Install-WindowsFeature – název služby FS-Resource-Manager – IncludeManagementTools"</span><span class="sxs-lookup"><span data-stu-id="68872-287">For example, "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted Install-WindowsFeature –Name FS-Resource-Manager –IncludeManagementTools"</span></span>

<span data-ttu-id="68872-288">Pokud vaše rozšíření sady škálování virtuálního počítače se pokouší tooinstall něco, co vyžaduje restartování počítače, můžete použít rozšíření Azure Automation konfigurace požadovaného stavu (DSC automatizace) hello.</span><span class="sxs-lookup"><span data-stu-id="68872-288">If your virtual machine scale set extension is trying tooinstall something that requires a reboot, you can use hello Azure Automation Desired State Configuration (Automation DSC) extension.</span></span> <span data-ttu-id="68872-289">Pokud hello operační systém je Windows Server 2012 R2, Azure vrátí v instalačním programu hello Windows Management Framework (WMF) 5.0 restartování počítače a poté pokračuje v konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="68872-289">If hello operating system is Windows Server 2012 R2, Azure pulls in hello Windows Management Framework (WMF) 5.0 setup, reboots, and then continues with hello configuration.</span></span> 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a><span data-ttu-id="68872-290">Jak zapnout antimalwarových v mé škálovací sadu virtuálních počítačů?</span><span class="sxs-lookup"><span data-stu-id="68872-290">How do I turn on antimalware in my virtual machine scale set?</span></span>

<span data-ttu-id="68872-291">tooturn na antimalware na vaše škálování virtuálních počítačů nastavení, použijte následující příklad PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="68872-291">tooturn on antimalware on your virtual machine scale set, use hello following PowerShell example:</span></span>

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve hello most recent version number of hello extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-tooexecute-a-custom-script-thats-hosted-in-a-private-storage-account-hello-script-runs-successfully-when-hello-storage-is-public-but-when-i-try-toouse-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a><span data-ttu-id="68872-292">Potřebuji tooexecute vlastní skript, který je hostován v účtu privátní úložiště.</span><span class="sxs-lookup"><span data-stu-id="68872-292">I need tooexecute a custom script that's hosted in a private storage account.</span></span> <span data-ttu-id="68872-293">Hello skript spustí úspěšně hello úložiště je veřejný, ale když se pokouším toouse sdíleného přístupového podpisu (SAS), se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="68872-293">hello script runs successfully when hello storage is public, but when I try toouse a Shared Access Signature (SAS), it fails.</span></span> <span data-ttu-id="68872-294">Zobrazí se tato zpráva: "Chybějící povinné parametry pro platný podpis sdíleného přístupu".</span><span class="sxs-lookup"><span data-stu-id="68872-294">This message is displayed: “Missing mandatory parameters for valid Shared Access Signature”.</span></span> <span data-ttu-id="68872-295">Odkaz + SAS funguje bez problémů z místní v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="68872-295">Link+SAS works fine from my local browser.</span></span>

<span data-ttu-id="68872-296">tooexecute vlastní skript, který je hostován v účet privátní úložiště nastavit chráněné hello klíč účtu úložiště a s názvem.</span><span class="sxs-lookup"><span data-stu-id="68872-296">tooexecute a custom script that's hosted in a private storage account, set up protected settings with hello storage account key and name.</span></span> <span data-ttu-id="68872-297">Další informace najdete v tématu [vlastní skript rozšíření pro Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span><span class="sxs-lookup"><span data-stu-id="68872-297">For more information, see [Custom Script Extension for Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span></span>







## <a name="networking"></a><span data-ttu-id="68872-298">Sítě</span><span class="sxs-lookup"><span data-stu-id="68872-298">Networking</span></span>
 
### <a name="is-it-possible-tooassign-a-network-security-group-nsg-tooa-scale-set-so-that-it-will-apply-tooall-hello-vm-nics-in-hello-set"></a><span data-ttu-id="68872-299">Je možné tooassign nastavit stupnici tooa skupina zabezpečení sítě (NSG), takže bude se vztahovat tooall hello síťové adaptéry virtuálních počítačů v sadě hello?</span><span class="sxs-lookup"><span data-stu-id="68872-299">Is it possible tooassign a Network Security Group (NSG) tooa scale set, so that it will apply tooall hello VM NICs in hello set?</span></span>

<span data-ttu-id="68872-300">Ano.</span><span class="sxs-lookup"><span data-stu-id="68872-300">Yes.</span></span> <span data-ttu-id="68872-301">Skupina zabezpečení sítě je možné použít přímo sady škálování tooa odkazem v části Networkinterfaceconfiguration hello hello profilu sítě.</span><span class="sxs-lookup"><span data-stu-id="68872-301">A Network Security Group can be applied directly tooa scale set by referencing it in hello networkInterfaceConfigurations section of hello network profile.</span></span> <span data-ttu-id="68872-302">Příklad:</span><span class="sxs-lookup"><span data-stu-id="68872-302">Example:</span></span>

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                 }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-hello-same-subscription-and-same-region"></a><span data-ttu-id="68872-303">Jakým způsobem prohození virtuálních IP adres pro sadách škálování virtuálních počítačů v hello stejné předplatné a stejné oblasti?</span><span class="sxs-lookup"><span data-stu-id="68872-303">How do I do a VIP swap for virtual machine scale sets in hello same subscription and same region?</span></span>

<span data-ttu-id="68872-304">Pokud máte dva virtuální počítače, nastaví se nástroj pro vyrovnávání zatížení Azure front-end a jsou v hello stejném předplatném, oblasti, můžete zrušit přidělení hello veřejné IP adresy z každé z nich a přiřadit toohello jiné.</span><span class="sxs-lookup"><span data-stu-id="68872-304">If you have two virtual machine scale sets with Azure Load Balancer front-ends, and they are in hello same subscription and region, you could deallocate hello public IP addresses from each one, and assign toohello other.</span></span> <span data-ttu-id="68872-305">V tématu [prohození virtuálních IP adres: modrá zelená nasazení ve službě Správce prostředků Azure](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) třeba.</span><span class="sxs-lookup"><span data-stu-id="68872-305">See [VIP Swap: Blue-green deployment in Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) for example.</span></span> <span data-ttu-id="68872-306">To neznamená zpoždění, i když jako hello jsou prostředky navrácena přidělené na úrovni sítě hello.</span><span class="sxs-lookup"><span data-stu-id="68872-306">This does imply a delay though as hello resources are deallocated/allocated at hello network level.</span></span> <span data-ttu-id="68872-307">Rychlejší možnost je toouse Azure Application Gateway se dvěma back-endové fondy a pravidel směrování.</span><span class="sxs-lookup"><span data-stu-id="68872-307">A faster option is toouse Azure Application Gateway with two backend pools, and a routing rule.</span></span> <span data-ttu-id="68872-308">Alternativně může hostování vaší aplikace s [služby Azure App service](https://azure.microsoft.com/en-us/services/app-service/) která poskytuje podporu pro rychlé přepínání mezi sloty pracovní a provozní.</span><span class="sxs-lookup"><span data-stu-id="68872-308">Alternatively, you could host your application with [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) which provides support for fast switching between staging and production slots.</span></span>
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-toouse-for-static-private-ip-address-allocation"></a><span data-ttu-id="68872-309">Jak určit rozsah privátní IP adresy toouse pro statického přidělování privátní IP adresu?</span><span class="sxs-lookup"><span data-stu-id="68872-309">How do I specify a range of private IP addresses toouse for static private IP address allocation?</span></span>

<span data-ttu-id="68872-310">IP adresy jsou vybrány z podsítě, který určíte.</span><span class="sxs-lookup"><span data-stu-id="68872-310">IP addresses are selected from a subnet that you specify.</span></span> 

<span data-ttu-id="68872-311">Metoda přidělení Hello adres IP sady škálování virtuálního počítače je vždy "dynamická", ale který neznamená, že tyto IP adresy můžou změnit.</span><span class="sxs-lookup"><span data-stu-id="68872-311">hello allocation method of virtual machine scale set IP addresses is always “dynamic,” but that doesn't mean that these IP addresses can change.</span></span> <span data-ttu-id="68872-312">V tomto případě "dynamická" pouze znamená, nezadávejte IP adresu hello v požadavku PUT.</span><span class="sxs-lookup"><span data-stu-id="68872-312">In this case, "dynamic" only means that you do not specify hello IP address in a PUT request.</span></span> <span data-ttu-id="68872-313">Zadejte hello statické nastavit pomocí hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="68872-313">Specify hello static set by using hello subnet.</span></span> 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-tooan-existing-azure-virtual-network"></a><span data-ttu-id="68872-314">Nasazení virtuální počítač škálování sadu tooan existující virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="68872-314">How do I deploy a virtual machine scale set tooan existing Azure virtual network?</span></span> 

<span data-ttu-id="68872-315">toodeploy škálování virtuálního počítače nastavit tooan existující virtuální síť Azure, najdete v části [nasadit virtuální počítač škálování sadu tooan existující virtuální síť](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="68872-315">toodeploy a virtual machine scale set tooan existing Azure virtual network, see [Deploy a virtual machine scale set tooan existing virtual network](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span></span> 

### <a name="how-do-i-add-hello-ip-address-of-hello-first-vm-in-a-virtual-machine-scale-set-toohello-output-of-a-template"></a><span data-ttu-id="68872-316">Jak přidat hello IP adresu z hello první virtuální počítač v škálování virtuálního počítače nastavit toohello výstup šablony?</span><span class="sxs-lookup"><span data-stu-id="68872-316">How do I add hello IP address of hello first VM in a virtual machine scale set toohello output of a template?</span></span>

<span data-ttu-id="68872-317">tooadd hello IP adresu hello první virtuální počítač v virtuálních počítačů škálování sadu toohello výstupu šablony, najdete v části [ARM: privátních IP adres získat VMSS](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span><span class="sxs-lookup"><span data-stu-id="68872-317">tooadd hello IP address of hello first VM in a virtual machine scale set toohello output of a template, see [ARM: Get VMSS's private IPs](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span></span>

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a><span data-ttu-id="68872-318">Můžete použít sady škálování pomocí Accelerated sítě?</span><span class="sxs-lookup"><span data-stu-id="68872-318">Can I use scale sets with Accelerated Networking?</span></span>

<span data-ttu-id="68872-319">Ano.</span><span class="sxs-lookup"><span data-stu-id="68872-319">Yes.</span></span> <span data-ttu-id="68872-320">toouse accelerated sítě, nastavení enableAcceleratedNetworking tootrue ve vaší škálovací sady Networkinterfaceconfiguration.</span><span class="sxs-lookup"><span data-stu-id="68872-320">toouse accelerated networking, set enableAcceleratedNetworking tootrue in your scale set's networkInterfaceConfigurations settings.</span></span> <span data-ttu-id="68872-321">Například</span><span class="sxs-lookup"><span data-stu-id="68872-321">E.g.</span></span>
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
        "name": "niconfig1",
        "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
                ]
            }
            }
        ]
        }
    }
    ]
}
```

### <a name="how-can-i-configure-hello-dns-servers-used-by-a-scale-set"></a><span data-ttu-id="68872-322">Konfigurování serverů DNS hello používá škálovací sada</span><span class="sxs-lookup"><span data-stu-id="68872-322">How can I configure hello DNS servers used by a scale set?</span></span>

<span data-ttu-id="68872-323">toocreate sady škálování virtuálního počítače pomocí vlastní konfigurace DNS, přidejte dnsSettings JSON paketu toohello škálování nastavit Networkinterfaceconfiguration části.</span><span class="sxs-lookup"><span data-stu-id="68872-323">toocreate a VM scale set with a custom DNS configuration, add a dnsSettings JSON packet toohello scale set networkInterfaceConfigurations section.</span></span> <span data-ttu-id="68872-324">Příklad:</span><span class="sxs-lookup"><span data-stu-id="68872-324">Example:</span></span>
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-tooassign-a-public-ip-address-tooeach-vm"></a><span data-ttu-id="68872-325">Konfigurování škálování sadu tooassign tooeach veřejnou adresu IP virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="68872-325">How can I configure a scale set tooassign a public IP address tooeach VM?</span></span>

<span data-ttu-id="68872-326">toocreate sady škálování virtuálního počítače, přiřadí tooeach veřejnou adresu IP virtuálního počítače, zkontrolujte, zda je 2017-03-30 hello rozhraní API verze hello Microsoft.Compute/virtualMAchineScaleSets prostředků a přidejte _publicipaddressconfiguration_ paketu JSON sady škálování toohello část konfigurace IP adresy.</span><span class="sxs-lookup"><span data-stu-id="68872-326">toocreate a VM scale set that assigns a public IP address tooeach VM, make sure hello API version of hello Microsoft.Compute/virtualMAchineScaleSets resource is 2017-03-30, and add a _publicipaddressconfiguration_ JSON packet toohello scale set ipConfigurations section.</span></span> <span data-ttu-id="68872-327">Příklad:</span><span class="sxs-lookup"><span data-stu-id="68872-327">Example:</span></span>

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-toowork-with-multiple-application-gateways"></a><span data-ttu-id="68872-328">Můžete nakonfigurovat sadu toowork škálování s více bran aplikace?</span><span class="sxs-lookup"><span data-stu-id="68872-328">Can I configure a scale set toowork with multiple Application Gateways?</span></span>

<span data-ttu-id="68872-329">Ano.</span><span class="sxs-lookup"><span data-stu-id="68872-329">Yes.</span></span> <span data-ttu-id="68872-330">Můžete přidat hello id prostředku je pro více Application Gateway back-end adresy fondy toohello _applicationGatewayBackendAddressPools_ seznamu v hello _konfigurace IP adresy_ části škálovací sady profil sítě.</span><span class="sxs-lookup"><span data-stu-id="68872-330">You can add hello resource id's for multiple Application Gateway backend address pools toohello _applicationGatewayBackendAddressPools_ list in hello _ipConfigurations_ section of your scale set network profile.</span></span>

## <a name="scale"></a><span data-ttu-id="68872-331">Měřítko</span><span class="sxs-lookup"><span data-stu-id="68872-331">Scale</span></span>

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a><span data-ttu-id="68872-332">V případě, co by vytvořit škálování virtuálního počítače s méně než dva virtuální počítače?</span><span class="sxs-lookup"><span data-stu-id="68872-332">In what case would I create a virtual machine scale set with fewer than two VMs?</span></span>

<span data-ttu-id="68872-333">Jedním z důvodů toocreate škálovací sady virtuálních počítačů s méně než dva virtuální počítače se nastavuje toouse hello elastické vlastnosti škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-333">One reason toocreate a virtual machine scale set with fewer than two VMs would be toouse hello elastic properties of a virtual machine scale set.</span></span> <span data-ttu-id="68872-334">Například můžete nasadit škálovací sadu virtuálních počítačů s nulový počet virtuálních počítačů toodefine infrastruktury bez placení virtuálního počítače s náklady.</span><span class="sxs-lookup"><span data-stu-id="68872-334">For example, you could deploy a virtual machine scale set with zero VMs toodefine your infrastructure without paying VM running costs.</span></span> <span data-ttu-id="68872-335">Potom po připravené toodeploy virtuální počítače zvýšit hello "kapacitu" hello virtuálního počítače škálování sadu toohello produkční počet instancí.</span><span class="sxs-lookup"><span data-stu-id="68872-335">Then, when you are ready toodeploy VMs, increase hello “capacity” of hello virtual machine scale set toohello production instance count.</span></span>

<span data-ttu-id="68872-336">Dalším důvodem, které můžete vytvořit škálování virtuálních počítačů s méně než dva virtuální počítače je, pokud máte obavy, méně s dostupností než při použití s virtuálními počítači diskrétní sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="68872-336">Another reason you might create a virtual machine scale set with fewer than two VMs is if you're concerned less with availability than in using an availability set with discrete VMs.</span></span> <span data-ttu-id="68872-337">Sady škálování virtuálního počítače zadejte způsob toowork s poskytujících blíže neurčené výpočetní jednotky, které jsou zastupitelných.</span><span class="sxs-lookup"><span data-stu-id="68872-337">Virtual machine scale sets give you a way toowork with undifferentiated compute units that are fungible.</span></span> <span data-ttu-id="68872-338">Toto sjednocení je klíčovým rozdílem pro sady škálování virtuálního počítače a skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="68872-338">This uniformity is a key differentiator for virtual machine scale sets versus availability sets.</span></span> <span data-ttu-id="68872-339">Mnoho Bezstavová zatížení do not track jednotlivé jednotky.</span><span class="sxs-lookup"><span data-stu-id="68872-339">Many stateless workloads do not track individual units.</span></span> <span data-ttu-id="68872-340">Pokud hodnota klesne hello zatížení, můžete snížit tooone výpočetní jednotka a pak škálovat toomany při zvyšuje zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="68872-340">If hello workload drops, you can scale down tooone compute unit, and then scale up toomany when hello workload increases.</span></span>

### <a name="how-do-i-change-hello-number-of-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="68872-341">Změna hello počet virtuálních počítačů sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="68872-341">How do I change hello number of VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="68872-342">toochange hello počet virtuální počítače ve škálovací sadě virtuálních počítačů najdete v části [změnit počet instancí hello škálovací sadu virtuálních počítačů](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="68872-342">toochange hello number of VMs in a virtual machine scale set, see [Change hello instance count of a virtual machine scale set](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span></span>

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a><span data-ttu-id="68872-343">Jak definuje vlastní výstrahy pro když se dosáhne určité prahové hodnoty?</span><span class="sxs-lookup"><span data-stu-id="68872-343">How do I define custom alerts for when certain thresholds are reached?</span></span>

<span data-ttu-id="68872-344">Máte určitou volnost v tom, jak zpracovat výstrahy pro zadané prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="68872-344">You have some flexibility in how you handle alerts for specified thresholds.</span></span> <span data-ttu-id="68872-345">Například můžete definovat vlastní webhooky.</span><span class="sxs-lookup"><span data-stu-id="68872-345">For example, you can define customized webhooks.</span></span> <span data-ttu-id="68872-346">Následující ukázka webhooku Hello je z šablony Resource Manageru:</span><span class="sxs-lookup"><span data-stu-id="68872-346">hello following webhook example is from a Resource Manager template:</span></span>

```json
{
    "type": "Microsoft.Insights/autoscaleSettings",
    "apiVersion": "[variables('insightsApi')]",
    "name": "autoscale",
    "location": "[parameters('resourceLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
    ],
    "properties": {
        "name": "autoscale",
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
        "enabled": true,
        "notifications": [
            {
                "operation": "Scale",
                "email": {
                    "sendToSubscriptionAdministrator": true,
                    "sendToSubscriptionCoAdministrators": true,
                    "customEmails": [
                        "youremail@address.com"
                    ]
                },
                "webhooks": [
                    {
                        "serviceUri": "https://events.pagerduty.com/integration/0b75b57246814149b4d87fa6e1273687/enqueue",
                        "properties": {
                            "key1": "custommetric",
                            "key2": "scalevmss"
                        }
                    }
                ]
            }
        ],
```

<span data-ttu-id="68872-347">V tomto příkladu výstrahu přejde tooPagerduty.com, když je dosaženo prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="68872-347">In this example, an alert goes tooPagerduty.com when a threshold is reached.</span></span>



## <a name="patching-and-operations"></a><span data-ttu-id="68872-348">Operace a oprav</span><span class="sxs-lookup"><span data-stu-id="68872-348">Patching and operations</span></span>

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a><span data-ttu-id="68872-349">Vytvoření sad v existující skupinu prostředků škálování</span><span class="sxs-lookup"><span data-stu-id="68872-349">How do I create a scale set in an existing resource group?</span></span>

<span data-ttu-id="68872-350">Vytváření sad škálování v existující prostředek skupiny ještě není možné z hello portálu Azure, ale můžete zadat existující skupinu prostředků, když nasazení škálování nastavit z šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="68872-350">Creating scale sets in an existing resource group is not yet possible from hello Azure portal, but you can specify an existing resource group when deploying a scale set from an Azure Resource Manager template.</span></span> <span data-ttu-id="68872-351">Můžete také zadat existující skupinu prostředků, při vytváření škálování nastavit pomocí prostředí Azure PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="68872-351">You can also specify an existing resource group when creating a scale set using Azure PowerShell or CLI.</span></span>

### <a name="can-we-move-a-scale-set-tooanother-resource-group"></a><span data-ttu-id="68872-352">Jsme, že škálování nastavit skupinu prostředků tooanother můžete přesunout?</span><span class="sxs-lookup"><span data-stu-id="68872-352">Can we move a scale set tooanother resource group?</span></span>

<span data-ttu-id="68872-353">Ano, můžete přesunout škálování sadu prostředků tooa nové předplatné nebo skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="68872-353">Yes, you can move scale set resources tooa new subscription or resource group.</span></span>

### <a name="how-tooi-update-my-virtual-machine-scale-set-tooa-new-image-how-do-i-manage-patching"></a><span data-ttu-id="68872-354">Jak aktualizovat tooI Moje škálovací sady virtuálních počítačů tooa novou bitovou kopii?</span><span class="sxs-lookup"><span data-stu-id="68872-354">How tooI update my virtual machine scale set tooa new image?</span></span> <span data-ttu-id="68872-355">Jak spravovat, opravy?</span><span class="sxs-lookup"><span data-stu-id="68872-355">How do I manage patching?</span></span>

<span data-ttu-id="68872-356">tooupdate vaše škálovací sady virtuálních počítačů tooa novou bitovou kopii a opravy chyb toomanage, najdete v části [upgradovat škálovací sadu virtuálních počítačů](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span><span class="sxs-lookup"><span data-stu-id="68872-356">tooupdate your virtual machine scale set tooa new image, and toomanage patching, see [Upgrade a virtual machine scale set](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span></span>

### <a name="can-i-use-hello-reimage-operation-tooreset-a-vm-without-changing-hello-image-that-is-i-want-reset-a-vm-toofactory-settings-rather-than-tooa-new-image"></a><span data-ttu-id="68872-357">Můžete použít tooreset operace obnovení z Image hello virtuálních počítačů beze změny hello image?</span><span class="sxs-lookup"><span data-stu-id="68872-357">Can I use hello reimage operation tooreset a VM without changing hello image?</span></span> <span data-ttu-id="68872-358">(To znamená, chci resetování nastavení virtuálních počítačů toofactory místo tooa novou bitovou kopii.)</span><span class="sxs-lookup"><span data-stu-id="68872-358">(That is, I want reset a VM toofactory settings rather than tooa new image.)</span></span>

<span data-ttu-id="68872-359">Ano, aniž byste museli měnit hello image můžete tooreset operace obnovení z Image hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68872-359">Yes, you can use hello reimage operation tooreset a VM without changing hello image.</span></span> <span data-ttu-id="68872-360">Ale pokud vaše škálovací sady virtuálních počítačů odkazuje image platformy s `version = latest`, virtuální počítač můžete aktualizovat tooa při volání bitové kopie operačního systému novější `reimage`.</span><span class="sxs-lookup"><span data-stu-id="68872-360">However, if your virtual machine scale set references a platform image with `version = latest`, your VM can update tooa later OS image when you call `reimage`.</span></span>

<span data-ttu-id="68872-361">Další informace najdete v tématu [spravovat všechny virtuální počítače ve škálovací sadu virtuálních počítačů](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span><span class="sxs-lookup"><span data-stu-id="68872-361">For more information, see [Manage all VMs in a virtual machine scale set](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="68872-362">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="68872-362">Troubleshooting</span></span>

### <a name="how-do-i-turn-on-boot-diagnostics"></a><span data-ttu-id="68872-363">Jak zapnout na Diagnostika spouštění?</span><span class="sxs-lookup"><span data-stu-id="68872-363">How do I turn on boot diagnostics?</span></span>

<span data-ttu-id="68872-364">tooturn na Diagnostika spouštění, nejdřív vytvořte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="68872-364">tooturn on boot diagnostics, first, create a storage account.</span></span> <span data-ttu-id="68872-365">Pak, přesuňte tento blok JSON sady škálování virtuálního počítače **virtualMachineProfile**a aktualizovat sadu škálování virtuálního počítače hello:</span><span class="sxs-lookup"><span data-stu-id="68872-365">Then, put this JSON block in your virtual machine scale set **virtualMachineProfile**, and update hello virtual machine scale set:</span></span>

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

<span data-ttu-id="68872-366">Při vytváření nového virtuálního počítače hello InstanceView vlastnost hello virtuální počítač zobrazuje podrobnosti hello hello – snímek obrazovky a tak dále.</span><span class="sxs-lookup"><span data-stu-id="68872-366">When a new VM is created, hello InstanceView property of hello VM shows hello details for hello screenshot, and so on.</span></span> <span data-ttu-id="68872-367">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="68872-367">Here's an example:</span></span>
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a><span data-ttu-id="68872-368">Vlastnosti virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="68872-368">Virtual machine properties</span></span>

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-hello-fault-domain-for-each-of-hello-100-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="68872-369">Jak získat informace o vlastnosti pro každý virtuální počítač bez volání více?</span><span class="sxs-lookup"><span data-stu-id="68872-369">How do I get property information for each VM without making multiple calls?</span></span> <span data-ttu-id="68872-370">Například jak by získat doména selhání hello každý hello 100 virtuálních počítačů v mé škálovací sadu virtuálních počítačů?</span><span class="sxs-lookup"><span data-stu-id="68872-370">For example, how would I get hello fault domain for each of hello 100 VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="68872-371">informace o vlastnosti tooget pro každý virtuální počítač bez volání více, můžete volat `ListVMInstanceViews` pomocí rozhraní REST API `GET` na hello následující identifikátor URI prostředku:</span><span class="sxs-lookup"><span data-stu-id="68872-371">tooget property information for each VM without making multiple calls, you can call `ListVMInstanceViews` by doing a REST API `GET` on hello following resource URI:</span></span>

<span data-ttu-id="68872-372">/subscriptions/ < ID_ODBĚRU > /resourceGroups/ < resource_group_name > /providers/Microsoft.Compute/virtualMachineScaleSets/ < scaleset_name > / virtualMachines? $expand = instanceView & $select = instanceView</span><span class="sxs-lookup"><span data-stu-id="68872-372">/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Compute/virtualMachineScaleSets/<scaleset_name>/virtualMachines?$expand=instanceView&$select=instanceView</span></span>

### <a name="can-i-pass-different-extension-arguments-toodifferent-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="68872-373">Může I předání jiné rozšíření argumentů toodifferent virtuální počítače ve škálovací sadě virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="68872-373">Can I pass different extension arguments toodifferent VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="68872-374">Ne, nelze předat jiné rozšíření argumenty toodifferent virtuální počítače ve škálovací sadě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-374">No, you cannot pass different extension arguments toodifferent VMs in a virtual machine scale set.</span></span> <span data-ttu-id="68872-375">Rozšíření však může fungovat podle hello jedinečné vlastnosti hello virtuální počítač běží na, například jako na název počítače hello.</span><span class="sxs-lookup"><span data-stu-id="68872-375">However, extensions can act based on hello unique properties of hello VM they are running on, such as on hello machine name.</span></span> <span data-ttu-id="68872-376">Rozšíření také můžete dotazovat instanci metadata na http://169.254.169.254 tooget Další informace o hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-376">Extensions also can query instance metadata on http://169.254.169.254 tooget more information about hello VM.</span></span>

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a><span data-ttu-id="68872-377">Proč se mezery mezi Moje identifikátory virtuálních počítačů a názvy počítačů virtuálních počítačů sady škálování virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="68872-377">Why are there gaps between my virtual machine scale set VM machine names and VM IDs?</span></span> <span data-ttu-id="68872-378">Příklad: 0, 1, 3...</span><span class="sxs-lookup"><span data-stu-id="68872-378">For example: 0, 1, 3...</span></span>

<span data-ttu-id="68872-379">Protože vaše škálovací sady virtuálních počítačů jsou mezery mezi názvy počítačů virtuálních počítačů sady škálování virtuálního počítače a virtuálního počítače ID **overprovision** je nastavena výchozí hodnota toohello **true**.</span><span class="sxs-lookup"><span data-stu-id="68872-379">There are gaps between your virtual machine scale set VM machine names and VM IDs because your virtual machine scale set **overprovision** property is set toohello default value of **true**.</span></span> <span data-ttu-id="68872-380">Pokud předimenzování je nastaven příliš**true**, než požadovaná vytvářejí další virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="68872-380">If overprovisioning is set too**true**, more VMs than requested are created.</span></span> <span data-ttu-id="68872-381">Navíc za virtuální počítače, budou odstraněna.</span><span class="sxs-lookup"><span data-stu-id="68872-381">Extra VMs are then deleted.</span></span> <span data-ttu-id="68872-382">V takovém případě získat nasazení vyšší spolehlivost ale na náklady hello souvislý pojmenování a souvislý překlad adres (NAT) pravidla.</span><span class="sxs-lookup"><span data-stu-id="68872-382">In this case, you gain increased deployment reliability, but at hello expense of contiguous naming and contiguous Network Address Translation (NAT) rules.</span></span> 

<span data-ttu-id="68872-383">Tuto vlastnost lze nastavit příliš**false**.</span><span class="sxs-lookup"><span data-stu-id="68872-383">You can set this property too**false**.</span></span> <span data-ttu-id="68872-384">Pro malé virtuálního počítače sady škálování to nemá vliv výrazně spolehlivost nasazení.</span><span class="sxs-lookup"><span data-stu-id="68872-384">For small virtual machine scale sets, this doesn't significantly affect deployment reliability.</span></span>

### <a name="what-is-hello-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-hello-vm-when-should-i-choose-one-over-hello-other"></a><span data-ttu-id="68872-385">Co je hello rozdíl mezi odstranění virtuálního počítače ve škálovací sadu virtuálních počítačů a rušení přidělení hello virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="68872-385">What is hello difference between deleting a VM in a virtual machine scale set and deallocating hello VM?</span></span> <span data-ttu-id="68872-386">Pokud by měl vybrat některého hello další?</span><span class="sxs-lookup"><span data-stu-id="68872-386">When should I choose one over hello other?</span></span>

<span data-ttu-id="68872-387">Hello hlavní rozdíl mezi odstranění virtuálního počítače ve škálovací sadu virtuálních počítačů a rušení přidělení hello virtuálních počítačů je, že `deallocate` nedojde k odstranění hello virtuální pevné disky (VHD).</span><span class="sxs-lookup"><span data-stu-id="68872-387">hello main difference between deleting a VM in a virtual machine scale set and deallocating hello VM is that `deallocate` doesn’t delete hello virtual hard disks (VHDs).</span></span> <span data-ttu-id="68872-388">Jsou náklady na úložiště přidružené k spuštění `stop deallocate`.</span><span class="sxs-lookup"><span data-stu-id="68872-388">There are storage costs associated with running `stop deallocate`.</span></span> <span data-ttu-id="68872-389">Může použít jednu nebo druhou pro jednu z následujících důvodů hello hello:</span><span class="sxs-lookup"><span data-stu-id="68872-389">You might use one or hello other for one of hello following reasons:</span></span>

- <span data-ttu-id="68872-390">Chcete toostop platícího výpočetní náklady, ale chcete tookeep hello disku stav hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-390">You want toostop paying compute costs, but you want tookeep hello disk state of hello VMs.</span></span>
- <span data-ttu-id="68872-391">Chcete toostart sadu virtuálních počítačů rychleji, než může škálovat škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68872-391">You want toostart a set of VMs more quickly than you could scale out a virtual machine scale set.</span></span>
  - <span data-ttu-id="68872-392">Související toothis scénáři jste mohli vytvořit vlastní modul škálování a chcete rychlejší škálování začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="68872-392">Related toothis scenario, you might have created your own autoscale engine and want a faster end-to-end scale.</span></span>
- <span data-ttu-id="68872-393">Máte škálovací sadu virtuálních počítačů, nerovnoměrně distribuované domén selhání a aktualizace domény.</span><span class="sxs-lookup"><span data-stu-id="68872-393">You have a virtual machine scale set that is unevenly distributed across fault domains or update domains.</span></span> <span data-ttu-id="68872-394">Může to být proto selektivně odstranit virtuální počítače, nebo proto, že virtuální počítače byly odstraněny po předimenzování.</span><span class="sxs-lookup"><span data-stu-id="68872-394">This might be because you selectively deleted VMs, or because VMs were deleted after overprovisioning.</span></span> <span data-ttu-id="68872-395">Spuštění `stop deallocate` následuje `start` na virtuálním počítači hello rozděluje rovnoměrně sad škálování virtuálních počítačů hello na domén selhání nebo doménách aktualizace.</span><span class="sxs-lookup"><span data-stu-id="68872-395">Running `stop deallocate` followed by `start` on hello virtual machine scale set evenly distributes hello VMs across fault domains or update domains.</span></span>

