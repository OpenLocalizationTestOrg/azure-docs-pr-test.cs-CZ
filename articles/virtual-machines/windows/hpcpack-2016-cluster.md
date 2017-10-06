---
title: aaaHPC Pack 2016 clusteru v Azure | Microsoft Docs
description: "Zjistěte, jak toodeploy HPC Pack 2016 cluster v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3dde6a68-e4a6-4054-8b67-d6a90fdc5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/15/2016
ms.author: danlep
ms.openlocfilehash: 9e21ec70c822a783474b96da77ce925940279abf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a>Nasazení clusteru HPC Pack 2016 v Azure

Postupujte podle kroků hello v toodeploy Tento článek [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) clusteru ve virtuálních počítačích Azure. HPC Pack je společnosti Microsoft volné HPC řešení založen na technologiích Microsoft Azure a Windows Server a podporuje úlohy HPC široký rozsah.

Použijte jednu z hello [šablon Azure Resource Manageru](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 clusteru. Máte několik možností topologie clusteru s různý počet hlavních uzlech clusteru a se buď Linux nebo Windows výpočetních uzlů.

## <a name="prerequisites"></a>Požadavky

### <a name="pfx-certificate"></a>Certifikát PFX

Cluster s podporou Microsoft HPC Pack 2016 vyžaduje Personal Information Exchange (PFX) certifikát toosecure hello komunikaci mezi uzly HPC hello. certifikát Hello musí splňovat následující požadavky hello:

* Musí mít privátní klíč umožňuje výměnu klíčů
* Použití klíče zahrnuje digitální podpis a šifrování klíče
* Rozšířené použití klíče obsahuje ověření klienta a ověření serveru

Pokud již nemáte certifikát, který splňuje tyto požadavky, můžete požádat, hello certifikát od certifikační autority. Alternativně můžete použít následující příkazy toogenerate hello certifikát podepsaný svým držitelem podle hello operační systém, na kterém spusťte příkaz hello a exportovat certifikát formátu PFX hello s privátním klíčem hello.

* **Pro Windows 10 nebo Windows Server 2016**, spusťte předdefinovaný hello **New-SelfSignedCertificate** rutiny prostředí PowerShell následujícím způsobem:

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* **Pro operační systémy starší než Windows 10 nebo Windows Server 2016**, stáhněte si hello [certifikát podepsaný svým držitelem generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) z hello Microsoft Script Center. Extrahujte obsah a spusťte následující příkazy v příkazovém řádku prostředí PowerShell hello:

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

### <a name="upload-certificate-tooan-azure-key-vault"></a>Nahrajte certifikát tooan Azure trezoru klíčů

Před nasazením clusteru HPC hello, nahrajte certifikát tooan hello [Azure trezoru klíčů](../../key-vault/index.md) jako tajný a záznamů hello následující informace pro použití během nasazování hello: **název trezoru**,  **Skupina prostředků trezoru**, **adresa URL certifikátu**, a **kryptografický otisk certifikátu**.

Následuje ukázka prostředí PowerShell skriptu tooupload hello certifikát. Další informace o nahrávání certifikátů tooan Azure trezoru klíčů najdete v tématu [Začínáme s Azure Key Vault](../../key-vault/key-vault-get-started.md).

```powershell
#Give hello following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate hello pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode hello JSON object
$pfxContentBytes = Get-Content $PfxFile -Encoding Byte
$pfxContentEncoded = [System.Convert]::ToBase64String($pfxContentBytes)
$jsonObject = @"
{
"data": "$pfxContentEncoded",
"dataType": "pfx",
"password": "$Password"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
#Create an Azure key vault and upload hello certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"hello following Information will be used in hello deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a>Podporované topologie

Vyberte jednu z hello [šablon Azure Resource Manageru](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 clusteru. Následují základní architektury topologií tři podporované clusteru. Topologie vysoké dostupnosti zahrnují více hlavních uzlech clusteru.

1. Vysoká dostupnost clusteru s doménou služby Active Directory

    ![Clusteru HA v doméně AD](./media/hpcpack-2016-cluster/haad.png)



2. Vysoká dostupnost clusteru bez domény služby Active Directory

    ![Clusteru HA bez domény služby AD](./media/hpcpack-2016-cluster/hanoad.png)

3. Cluster s jeden hlavní uzel

   ![Cluster s jeden hlavní uzel](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a>Nasazení clusteru

toocreate hello cluster, vyberte šablonu a klikněte na tlačítko **nasazení tooAzure**. V hello [portál Azure](https://portal.azure.com), zadejte parametry šablony hello, jak je popsáno v hello následující kroky. Každá šablona vytvoří všechny prostředky Azure potřebné pro infrastruktura clusteru HPC hello. Prostředky zahrnují virtuální síť Azure, veřejné IP adresy, Vyrovnávání zatížení (pouze pro vysokou dostupnost clusteru), síťová rozhraní, skupiny dostupnosti, účty úložiště a virtuálních počítačů.

### <a name="step-1-select-hello-subscription-location-and-resource-group"></a>Krok 1: Vyberte předplatné hello, umístění a skupiny prostředků

Hello **předplatné** a hello **umístění** musí být stejná, kterou jste zadali, když jste nahráli vašeho certifikátu PFX (viz požadavky). Doporučujeme, abyste vytvořili **skupiny prostředků** hello nasazení.

### <a name="step-2-specify-hello-parameter-settings"></a>Krok 2: Zadejte nastavení parametrů hello

Zadejte nebo upravte hodnoty pro parametry šablony hello. Klikněte na tlačítko hello ikonu další tooeach parametr pro informace nápovědy. Viz také pokyny, hello [dostupných velikostí virtuálních počítačů](sizes.md).

Zadejte hodnoty hello jste si poznamenali v hello předpoklady pro hello následující parametry: **název trezoru**, **skupiny prostředků trezoru**, **adresa URL certifikátu**a **Kryptografický otisk certifikátu**.

### <a name="step-3-review-legal-terms-and-create"></a>Krok 3. Přečíst si právní podmínky a vytvořte
Klikněte na tlačítko **přečíst si právní podmínky** tooreview hello podmínky. Pokud souhlasíte, klikněte na tlačítko **nákupu**a potom klikněte na **vytvořit** toostart hello nasazení.

## <a name="connect-toohello-cluster"></a>Připojte toohello cluster
1. Po nasazení clusteru HPC Pack hello přejděte toohello [portál Azure](https://portal.azure.com). Klikněte na tlačítko **skupiny prostředků**a skupině prostředků hello najít, na které hello clusteru nasazená. Můžete najít hello hlavního uzlu virtuálních počítačů.

    ![Hlavních uzlech clusteru hello portálu](./media/hpcpack-2016-cluster/clusterhns.png)

2. Klikněte na jeden hlavní uzel (v clusteru s podporou vysokou dostupností, klikněte na některé z hlavních uzlech hello). V **Essentials**, můžete vyhledat hello veřejnou IP adresu nebo úplný název DNS clusteru hello.

    ![Nastavení připojení clusteru](./media/hpcpack-2016-cluster/clusterconnect.png)

3. Klikněte na tlačítko **připojit** toolog na tooany hello pomocí vzdálené plochy s svoje uživatelské jméno zadané správce hlavních uzlech. Pokud jste nasadili cluster hello v doméně služby Active Directory, hello uživatelské jméno má podobu hello <privateDomainName> \<adminUsername > (například hpc.local\hpcadmin).

## <a name="next-steps"></a>Další kroky
* Odeslání úlohy tooyour clusteru. V tématu [odeslání úlohy tooHPC clusteru služby HPC Pack v Azure](hpcpack-cluster-submit-jobs.md) a [spravovat cluster služby HPC Pack 2016 v Azure pomocí Azure Active Directory](hpcpack-cluster-active-directory.md).

