---
title: "aaaSet až WinRM přístup pro virtuální počítač Azure | Microsoft Docs"
description: "Instalační program přístup WinRM pro použití s virtuálního počítače Azure vytvořené v modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: 23d1d3a3065cbd8e4036be085c6d835cae36caae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="d376f-103">Nastavení přístupu WinRM pro virtuální počítače ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="d376f-103">Setting up WinRM access for Virtual Machines in Azure Resource Manager</span></span>
## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a><span data-ttu-id="d376f-104">WinRM v sadě vs Azure Service Management Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d376f-104">WinRM in Azure Service Management vs Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

* <span data-ttu-id="d376f-105">Přehled hello Azure Resource Manager, najdete v tématu to [článku](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d376f-105">For an overview of hello Azure Resource Manager, please see this [article](../../azure-resource-manager/resource-group-overview.md)</span></span>
* <span data-ttu-id="d376f-106">Rozdíly mezi Azure Service Management a Azure Resource Manager, najdete v tématu to [článku](../../resource-manager-deployment-model.md)</span><span class="sxs-lookup"><span data-stu-id="d376f-106">For differences between Azure Service Management and Azure Resource Manager, please see this [article](../../resource-manager-deployment-model.md)</span></span>

<span data-ttu-id="d376f-107">Hello klíčovým rozdílem v nastavení konfigurace služby WinRM mezi dvěma zásobníky hello je, jak získá hello certifikát nainstalovaný v hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d376f-107">hello key difference in setting up WinRM configuration between hello two stacks is how hello certificate gets installed on hello VM.</span></span> <span data-ttu-id="d376f-108">V zásobníku hello Azure Resource Manager jsou certifikáty hello modelován jako prostředky, které spravuje hello zprostředkovatele prostředku trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="d376f-108">In hello Azure Resource Manager stack, hello certificates are modeled as resources managed by hello Key Vault Resource Provider.</span></span> <span data-ttu-id="d376f-109">Proto hello uživatel potřebuje tooprovide svůj vlastní certifikát a nahrajte ho tooa Key Vault před jeho použitím ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="d376f-109">Therefore, hello user needs tooprovide their own certificate and upload it tooa Key Vault before using it in a VM.</span></span>

<span data-ttu-id="d376f-110">Tady jsou kroky hello potřebujete tootake tooset si virtuální počítač s připojením služby WinRM</span><span class="sxs-lookup"><span data-stu-id="d376f-110">Here are hello steps you need tootake tooset up a VM with WinRM connectivity</span></span>

1. <span data-ttu-id="d376f-111">Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="d376f-111">Create a Key Vault</span></span>
2. <span data-ttu-id="d376f-112">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="d376f-112">Create a self-signed certificate</span></span>
3. <span data-ttu-id="d376f-113">Nahrát váš certifikát podepsaný svým držitelem tooKey trezoru</span><span class="sxs-lookup"><span data-stu-id="d376f-113">Upload your self-signed certificate tooKey Vault</span></span>
4. <span data-ttu-id="d376f-114">Získat adresu URL hello pro certifikátu podepsaného svým držitelem v hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="d376f-114">Get hello URL for your self-signed certificate in hello Key Vault</span></span>
5. <span data-ttu-id="d376f-115">Odkazovat na adresu URL vašeho certifikáty podepsané svým držitelem při vytváření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d376f-115">Reference your self-signed certificates URL while creating a VM</span></span>

## <a name="step-1-create-a-key-vault"></a><span data-ttu-id="d376f-116">Krok 1: Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="d376f-116">Step 1: Create a Key Vault</span></span>
<span data-ttu-id="d376f-117">Můžete použít hello níže příkaz toocreate hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="d376f-117">You can use hello below command toocreate hello Key Vault</span></span>

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a><span data-ttu-id="d376f-118">Krok 2: Vytvořte certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="d376f-118">Step 2: Create a self-signed certificate</span></span>
<span data-ttu-id="d376f-119">Můžete vytvořit certifikát podepsaný svým držitelem pomocí tohoto skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d376f-119">You can create a self-signed certificate using this PowerShell script</span></span>

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter hello certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-toohello-key-vault"></a><span data-ttu-id="d376f-120">Krok 3: Nahrajte váš certifikát podepsaný svým držitelem toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="d376f-120">Step 3: Upload your self-signed certificate toohello Key Vault</span></span>
<span data-ttu-id="d376f-121">Před nahráním hello certifikát toohello Key Vault vytvořili v kroku 1, musí tooconverted do formátu hello pochopili zprostředkovatele prostředků Microsoft.Compute.</span><span class="sxs-lookup"><span data-stu-id="d376f-121">Before uploading hello certificate toohello Key Vault created in step 1, it needs tooconverted into a format hello Microsoft.Compute resource provider will understand.</span></span> <span data-ttu-id="d376f-122">Hello níže uvedený skript prostředí PowerShell vám umožní, že můžete udělat</span><span class="sxs-lookup"><span data-stu-id="d376f-122">hello below PowerShell script will allow you do that</span></span>

```
$fileName = "<Path toohello .pfx file>"
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

## <a name="step-4-get-hello-url-for-your-self-signed-certificate-in-hello-key-vault"></a><span data-ttu-id="d376f-123">Krok 4: Získat adresu URL hello pro certifikátu podepsaného svým držitelem v hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="d376f-123">Step 4: Get hello URL for your self-signed certificate in hello Key Vault</span></span>
<span data-ttu-id="d376f-124">zprostředkovateli prostředků Microsoft.Compute Hello musí tajný klíč toohello adresu URL uvnitř hello Key Vault při zřizování hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d376f-124">hello Microsoft.Compute resource provider needs a URL toohello secret inside hello Key Vault while provisioning hello VM.</span></span> <span data-ttu-id="d376f-125">To umožňuje zprostředkovatele prostředků hello Microsoft.Compute toodownload hello tajný klíč a vytvořit ekvivalentní certifikát hello na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d376f-125">This enables hello Microsoft.Compute resource provider toodownload hello secret and create hello equivalent certificate on hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="d376f-126">Adresa URL Hello tajného klíče hello musí tooinclude hello verzi také.</span><span class="sxs-lookup"><span data-stu-id="d376f-126">hello URL of hello secret needs tooinclude hello version as well.</span></span> <span data-ttu-id="d376f-127">Adresu URL příklad vypadá níže https://contosovault.vault.azure.net:443 nebo tajných klíčů nebo contososecret/01h9db0df2cd4300a20ence585a6s7ve</span><span class="sxs-lookup"><span data-stu-id="d376f-127">An example URL looks like below https://contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve</span></span>
> 
> 

#### <a name="templates"></a><span data-ttu-id="d376f-128">Šablony</span><span class="sxs-lookup"><span data-stu-id="d376f-128">Templates</span></span>
<span data-ttu-id="d376f-129">V šabloně hello pomocí hello níže uvedeného kódu můžete získat URL toohello odkazu hello</span><span class="sxs-lookup"><span data-stu-id="d376f-129">You can get hello link toohello URL in hello template using hello below code</span></span>

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a><span data-ttu-id="d376f-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d376f-130">PowerShell</span></span>
<span data-ttu-id="d376f-131">Můžete získat tuto adresu URL pomocí hello následující příkaz prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d376f-131">You can get this URL using hello below PowerShell command</span></span>

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a><span data-ttu-id="d376f-132">Krok 5: Odkazovat na adresu URL vašeho certifikáty podepsané svým držitelem při vytváření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d376f-132">Step 5: Reference your self-signed certificates URL while creating a VM</span></span>
#### <a name="azure-resource-manager-templates"></a><span data-ttu-id="d376f-133">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d376f-133">Azure Resource Manager Templates</span></span>
<span data-ttu-id="d376f-134">Při vytváření virtuálního počítače prostřednictvím šablony, získá certifikát hello odkazovaná v hello tajné klíče a oddílu winRM hello jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="d376f-134">While creating a VM through templates, hello certificate gets referenced in hello secrets section and hello winRM section as below:</span></span>

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of hello Key Vault containing hello secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for hello certificate you got in Step 4>",
                  "certificateStore": "<Name of hello certificate store on hello VM>"
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
                  "certificateUrl": "<URL for hello certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

<span data-ttu-id="d376f-135">Vzorové šablony pro hello výše je zde uveden v [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="d376f-135">A sample template for hello above can be found here at [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span></span>

<span data-ttu-id="d376f-136">Zdrojový kód pro tuto šablonu lze najít na [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="d376f-136">Source code for this template can be found on [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span></span>

#### <a name="powershell"></a><span data-ttu-id="d376f-137">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d376f-137">PowerShell</span></span>
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-toohello-vm"></a><span data-ttu-id="d376f-138">Krok 6: Připojení toohello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d376f-138">Step 6: Connecting toohello VM</span></span>
<span data-ttu-id="d376f-139">Před připojením toohello virtuálních počítačů musíte toomake se, že váš počítač je nakonfigurován pro vzdálenou správu přes WinRM.</span><span class="sxs-lookup"><span data-stu-id="d376f-139">Before you can connect toohello VM you'll need toomake sure your machine is configured for WinRM remote management.</span></span> <span data-ttu-id="d376f-140">Spusťte prostředí PowerShell jako správce a spusťte hello níže příkaz toomake se, že jste nastavili.</span><span class="sxs-lookup"><span data-stu-id="d376f-140">Start PowerShell as an administrator and execute hello below command toomake sure you're set up.</span></span>

    Enable-PSRemoting -Force

> [!NOTE]
> <span data-ttu-id="d376f-141">Může být nutné toomake se, že hello Služba WinRM je spuštěná, pokud hello výše nefunguje.</span><span class="sxs-lookup"><span data-stu-id="d376f-141">You might need toomake sure hello WinRM service is running if hello above does not work.</span></span> <span data-ttu-id="d376f-142">Můžete to, že pomocí`Get-Service WinRM`</span><span class="sxs-lookup"><span data-stu-id="d376f-142">You can do that using `Get-Service WinRM`</span></span>
> 
> 

<span data-ttu-id="d376f-143">Po dokončení instalace hello se můžete připojit toohello virtuálních počítačů pomocí hello pod příkaz</span><span class="sxs-lookup"><span data-stu-id="d376f-143">Once hello setup is done, you can connect toohello VM using hello below command</span></span>

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
