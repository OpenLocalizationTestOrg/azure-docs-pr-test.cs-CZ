---
title: "Nastavit přístup WinRM pro virtuální počítač Azure | Microsoft Docs"
description: "Instalační program přístup WinRM pro použití s virtuálního počítače Azure vytvořené v modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9718e85b-d360-4621-90b8-0b0b84a21208
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/16/2016
ms.author: kasing
ms.openlocfilehash: 2d6533462400bc1d93d0d3b0227769784e2658a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="85c77-103">Nastavení přístupu WinRM pro virtuální počítače ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="85c77-103">Setting up WinRM access for Virtual Machines in Azure Resource Manager</span></span>
## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a><span data-ttu-id="85c77-104">WinRM v sadě vs Azure Service Management Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="85c77-104">WinRM in Azure Service Management vs Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

* <span data-ttu-id="85c77-105">Přehled Azure Resource Manager, najdete v tématu to [článku](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="85c77-105">For an overview of the Azure Resource Manager, please see this [article](../../azure-resource-manager/resource-group-overview.md)</span></span>
* <span data-ttu-id="85c77-106">Rozdíly mezi Azure Service Management a Azure Resource Manager, najdete v tématu to [článku](../../resource-manager-deployment-model.md)</span><span class="sxs-lookup"><span data-stu-id="85c77-106">For differences between Azure Service Management and Azure Resource Manager, please see this [article](../../resource-manager-deployment-model.md)</span></span>

<span data-ttu-id="85c77-107">V nastavení konfigurace služby WinRM mezi dvěma zásobníky klíčovým rozdílem je, jak získá certifikát nainstalovaný na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="85c77-107">The key difference in setting up WinRM configuration between the two stacks is how the certificate gets installed on the VM.</span></span> <span data-ttu-id="85c77-108">V zásobníku Azure Resource Manager jsou certifikáty modelován jako prostředky, které spravuje zprostředkovatele prostředku trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="85c77-108">In the Azure Resource Manager stack, the certificates are modeled as resources managed by the Key Vault Resource Provider.</span></span> <span data-ttu-id="85c77-109">Proto uživatel musí zadat své vlastní certifikát a odešlete ji do trezoru klíč před jeho použitím ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="85c77-109">Therefore, the user needs to provide their own certificate and upload it to a Key Vault before using it in a VM.</span></span>

<span data-ttu-id="85c77-110">Tady jsou kroky, které je třeba provést nastavit virtuální počítač s připojením služby WinRM</span><span class="sxs-lookup"><span data-stu-id="85c77-110">Here are the steps you need to take to set up a VM with WinRM connectivity</span></span>

1. <span data-ttu-id="85c77-111">Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="85c77-111">Create a Key Vault</span></span>
2. <span data-ttu-id="85c77-112">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="85c77-112">Create a self-signed certificate</span></span>
3. <span data-ttu-id="85c77-113">Nahrání certifikátu podepsaného svým držitelem pro Key Vault</span><span class="sxs-lookup"><span data-stu-id="85c77-113">Upload your self-signed certificate to Key Vault</span></span>
4. <span data-ttu-id="85c77-114">Získat adresu URL pro certifikátu podepsaného svým držitelem v Key Vault</span><span class="sxs-lookup"><span data-stu-id="85c77-114">Get the URL for your self-signed certificate in the Key Vault</span></span>
5. <span data-ttu-id="85c77-115">Odkazovat na adresu URL vašeho certifikáty podepsané svým držitelem při vytváření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="85c77-115">Reference your self-signed certificates URL while creating a VM</span></span>

## <a name="step-1-create-a-key-vault"></a><span data-ttu-id="85c77-116">Krok 1: Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="85c77-116">Step 1: Create a Key Vault</span></span>
<span data-ttu-id="85c77-117">Můžete použít následující příkaz pro vytvoření Key Vault</span><span class="sxs-lookup"><span data-stu-id="85c77-117">You can use the below command to create the Key Vault</span></span>

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a><span data-ttu-id="85c77-118">Krok 2: Vytvořte certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="85c77-118">Step 2: Create a self-signed certificate</span></span>
<span data-ttu-id="85c77-119">Můžete vytvořit certifikát podepsaný svým držitelem pomocí tohoto skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="85c77-119">You can create a self-signed certificate using this PowerShell script</span></span>

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a><span data-ttu-id="85c77-120">Krok 3: Odeslání certifikátu podepsaného svým držitelem do služby Key Vault</span><span class="sxs-lookup"><span data-stu-id="85c77-120">Step 3: Upload your self-signed certificate to the Key Vault</span></span>
<span data-ttu-id="85c77-121">Než odešlete certifikát do služby Key Vault vytvořili v kroku 1, je nutné převést do formátu, který pochopili zprostředkovatele prostředků Microsoft.Compute.</span><span class="sxs-lookup"><span data-stu-id="85c77-121">Before uploading the certificate to the Key Vault created in step 1, it needs to converted into a format the Microsoft.Compute resource provider will understand.</span></span> <span data-ttu-id="85c77-122">Níže prostředí PowerShell vám umožní skriptu, můžete udělat</span><span class="sxs-lookup"><span data-stu-id="85c77-122">The below PowerShell script will allow you do that</span></span>

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a><span data-ttu-id="85c77-123">Krok 4: Získáte adresu URL pro certifikátu podepsaného svým držitelem v Key Vault</span><span class="sxs-lookup"><span data-stu-id="85c77-123">Step 4: Get the URL for your self-signed certificate in the Key Vault</span></span>
<span data-ttu-id="85c77-124">Zprostředkovateli prostředků Microsoft.Compute. adresu URL tajný klíč v Key Vault musí při zřizování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="85c77-124">The Microsoft.Compute resource provider needs a URL to the secret inside the Key Vault while provisioning the VM.</span></span> <span data-ttu-id="85c77-125">To umožňuje zprostředkovatele prostředků Microsoft.Compute ke stažení tajný klíč a vytvořit ekvivalentní certifikát ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="85c77-125">This enables the Microsoft.Compute resource provider to download the secret and create the equivalent certificate on the VM.</span></span>

> [!NOTE]
> <span data-ttu-id="85c77-126">Adresa URL tajný klíč musí obsahovat verzi také.</span><span class="sxs-lookup"><span data-stu-id="85c77-126">The URL of the secret needs to include the version as well.</span></span> <span data-ttu-id="85c77-127">Adresu URL příklad vypadá níže https://contosovault.vault.azure.net:443 nebo tajných klíčů nebo contososecret/01h9db0df2cd4300a20ence585a6s7ve</span><span class="sxs-lookup"><span data-stu-id="85c77-127">An example URL looks like below https://contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve</span></span>
> 
> 

#### <a name="templates"></a><span data-ttu-id="85c77-128">Šablony</span><span class="sxs-lookup"><span data-stu-id="85c77-128">Templates</span></span>
<span data-ttu-id="85c77-129">Odkaz na adresu URL pomocí šablony můžete získat níže uvedeného kódu</span><span class="sxs-lookup"><span data-stu-id="85c77-129">You can get the link to the URL in the template using the below code</span></span>

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a><span data-ttu-id="85c77-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="85c77-130">PowerShell</span></span>
<span data-ttu-id="85c77-131">Můžete získat pomocí této adresy URL následující příkaz prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="85c77-131">You can get this URL using the below PowerShell command</span></span>

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a><span data-ttu-id="85c77-132">Krok 5: Odkazovat na adresu URL vašeho certifikáty podepsané svým držitelem při vytváření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="85c77-132">Step 5: Reference your self-signed certificates URL while creating a VM</span></span>
#### <a name="azure-resource-manager-templates"></a><span data-ttu-id="85c77-133">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="85c77-133">Azure Resource Manager Templates</span></span>
<span data-ttu-id="85c77-134">Při vytváření virtuálního počítače prostřednictvím šablony, získá certifikát odkazuje v části tajných klíčů a v části winRM, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="85c77-134">While creating a VM through templates, the certificate gets referenced in the secrets section and the winRM section as below:</span></span>

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

<span data-ttu-id="85c77-135">Vzorové šablony pro výše je zde uveden v [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="85c77-135">A sample template for the above can be found here at [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span></span>

<span data-ttu-id="85c77-136">Zdrojový kód pro tuto šablonu lze najít na [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="85c77-136">Source code for this template can be found on [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span></span>

#### <a name="powershell"></a><span data-ttu-id="85c77-137">PowerShell</span><span class="sxs-lookup"><span data-stu-id="85c77-137">PowerShell</span></span>
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a><span data-ttu-id="85c77-138">Krok 6: Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="85c77-138">Step 6: Connecting to the VM</span></span>
<span data-ttu-id="85c77-139">Před připojením k virtuálnímu počítači, budete muset zajistěte, aby váš počítač je nakonfigurován pro vzdálenou správu přes WinRM.</span><span class="sxs-lookup"><span data-stu-id="85c77-139">Before you can connect to the VM you'll need to make sure your machine is configured for WinRM remote management.</span></span> <span data-ttu-id="85c77-140">Spusťte prostředí PowerShell jako správce a spusťte následující příkaz, abyste měli jistotu, že nastavíte.</span><span class="sxs-lookup"><span data-stu-id="85c77-140">Start PowerShell as an administrator and execute the below command to make sure you're set up.</span></span>

    Enable-PSRemoting -Force

> [!NOTE]
> <span data-ttu-id="85c77-141">Možná budete muset Ujistěte se, že Služba WinRM je spuštěná, pokud výše nefunguje.</span><span class="sxs-lookup"><span data-stu-id="85c77-141">You might need to make sure the WinRM service is running if the above does not work.</span></span> <span data-ttu-id="85c77-142">Můžete to, že pomocí`Get-Service WinRM`</span><span class="sxs-lookup"><span data-stu-id="85c77-142">You can do that using `Get-Service WinRM`</span></span>
> 
> 

<span data-ttu-id="85c77-143">Po dokončení instalace se můžete připojit k virtuální počítač pomocí pod příkaz</span><span class="sxs-lookup"><span data-stu-id="85c77-143">Once the setup is done, you can connect to the VM using the below command</span></span>

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
