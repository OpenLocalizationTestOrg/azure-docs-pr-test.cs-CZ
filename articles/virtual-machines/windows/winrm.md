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
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Nastavení přístupu WinRM pro virtuální počítače ve službě Správce prostředků Azure
## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM v sadě vs Azure Service Management Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

* Přehled hello Azure Resource Manager, najdete v tématu to [článku](../../azure-resource-manager/resource-group-overview.md)
* Rozdíly mezi Azure Service Management a Azure Resource Manager, najdete v tématu to [článku](../../resource-manager-deployment-model.md)

Hello klíčovým rozdílem v nastavení konfigurace služby WinRM mezi dvěma zásobníky hello je, jak získá hello certifikát nainstalovaný v hello virtuálních počítačů. V zásobníku hello Azure Resource Manager jsou certifikáty hello modelován jako prostředky, které spravuje hello zprostředkovatele prostředku trezoru klíčů. Proto hello uživatel potřebuje tooprovide svůj vlastní certifikát a nahrajte ho tooa Key Vault před jeho použitím ve virtuálním počítači.

Tady jsou kroky hello potřebujete tootake tooset si virtuální počítač s připojením služby WinRM

1. Vytvoření trezoru klíčů
2. Vytvořit certifikát podepsaný svým držitelem
3. Nahrát váš certifikát podepsaný svým držitelem tooKey trezoru
4. Získat adresu URL hello pro certifikátu podepsaného svým držitelem v hello Key Vault
5. Odkazovat na adresu URL vašeho certifikáty podepsané svým držitelem při vytváření virtuálního počítače

## <a name="step-1-create-a-key-vault"></a>Krok 1: Vytvoření trezoru klíčů
Můžete použít hello níže příkaz toocreate hello Key Vault

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Krok 2: Vytvořte certifikát podepsaný svým držitelem
Můžete vytvořit certifikát podepsaný svým držitelem pomocí tohoto skriptu prostředí PowerShell

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter hello certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-toohello-key-vault"></a>Krok 3: Nahrajte váš certifikát podepsaný svým držitelem toohello Key Vault
Před nahráním hello certifikát toohello Key Vault vytvořili v kroku 1, musí tooconverted do formátu hello pochopili zprostředkovatele prostředků Microsoft.Compute. Hello níže uvedený skript prostředí PowerShell vám umožní, že můžete udělat

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

## <a name="step-4-get-hello-url-for-your-self-signed-certificate-in-hello-key-vault"></a>Krok 4: Získat adresu URL hello pro certifikátu podepsaného svým držitelem v hello Key Vault
zprostředkovateli prostředků Microsoft.Compute Hello musí tajný klíč toohello adresu URL uvnitř hello Key Vault při zřizování hello virtuálních počítačů. To umožňuje zprostředkovatele prostředků hello Microsoft.Compute toodownload hello tajný klíč a vytvořit ekvivalentní certifikát hello na hello virtuálních počítačů.

> [!NOTE]
> Adresa URL Hello tajného klíče hello musí tooinclude hello verzi také. Adresu URL příklad vypadá níže https://contosovault.vault.azure.net:443 nebo tajných klíčů nebo contososecret/01h9db0df2cd4300a20ence585a6s7ve
> 
> 

#### <a name="templates"></a>Šablony
V šabloně hello pomocí hello níže uvedeného kódu můžete získat URL toohello odkazu hello

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell
Můžete získat tuto adresu URL pomocí hello následující příkaz prostředí PowerShell

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Krok 5: Odkazovat na adresu URL vašeho certifikáty podepsané svým držitelem při vytváření virtuálního počítače
#### <a name="azure-resource-manager-templates"></a>Šablony Azure Resource Manageru
Při vytváření virtuálního počítače prostřednictvím šablony, získá certifikát hello odkazovaná v hello tajné klíče a oddílu winRM hello jak je uvedeno níže:

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

Vzorové šablony pro hello výše je zde uveden v [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Zdrojový kód pro tuto šablonu lze najít na [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-toohello-vm"></a>Krok 6: Připojení toohello virtuálních počítačů
Před připojením toohello virtuálních počítačů musíte toomake se, že váš počítač je nakonfigurován pro vzdálenou správu přes WinRM. Spusťte prostředí PowerShell jako správce a spusťte hello níže příkaz toomake se, že jste nastavili.

    Enable-PSRemoting -Force

> [!NOTE]
> Může být nutné toomake se, že hello Služba WinRM je spuštěná, pokud hello výše nefunguje. Můžete to, že pomocí`Get-Service WinRM`
> 
> 

Po dokončení instalace hello se můžete připojit toohello virtuálních počítačů pomocí hello pod příkaz

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
