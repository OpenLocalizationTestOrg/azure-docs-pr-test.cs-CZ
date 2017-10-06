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
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a><span data-ttu-id="4e32a-103">Nasazení clusteru HPC Pack 2016 v Azure</span><span class="sxs-lookup"><span data-stu-id="4e32a-103">Deploy an HPC Pack 2016 cluster in Azure</span></span>

<span data-ttu-id="4e32a-104">Postupujte podle kroků hello v toodeploy Tento článek [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) clusteru ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="4e32a-104">Follow hello steps in this article toodeploy a [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) cluster in Azure virtual machines.</span></span> <span data-ttu-id="4e32a-105">HPC Pack je společnosti Microsoft volné HPC řešení založen na technologiích Microsoft Azure a Windows Server a podporuje úlohy HPC široký rozsah.</span><span class="sxs-lookup"><span data-stu-id="4e32a-105">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies and supports a wide range of HPC workloads.</span></span>

<span data-ttu-id="4e32a-106">Použijte jednu z hello [šablon Azure Resource Manageru](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e32a-106">Use one of hello [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 cluster.</span></span> <span data-ttu-id="4e32a-107">Máte několik možností topologie clusteru s různý počet hlavních uzlech clusteru a se buď Linux nebo Windows výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="4e32a-107">You have several choices of cluster topology with different numbers of cluster head nodes, and with either Linux or Windows compute nodes.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e32a-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4e32a-108">Prerequisites</span></span>

### <a name="pfx-certificate"></a><span data-ttu-id="4e32a-109">Certifikát PFX</span><span class="sxs-lookup"><span data-stu-id="4e32a-109">PFX certificate</span></span>

<span data-ttu-id="4e32a-110">Cluster s podporou Microsoft HPC Pack 2016 vyžaduje Personal Information Exchange (PFX) certifikát toosecure hello komunikaci mezi uzly HPC hello.</span><span class="sxs-lookup"><span data-stu-id="4e32a-110">A Microsoft HPC Pack 2016 cluster requires a Personal Information Exchange (PFX) certificate toosecure hello communication between hello HPC nodes.</span></span> <span data-ttu-id="4e32a-111">certifikát Hello musí splňovat následující požadavky hello:</span><span class="sxs-lookup"><span data-stu-id="4e32a-111">hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="4e32a-112">Musí mít privátní klíč umožňuje výměnu klíčů</span><span class="sxs-lookup"><span data-stu-id="4e32a-112">It must have a private key capable of key exchange</span></span>
* <span data-ttu-id="4e32a-113">Použití klíče zahrnuje digitální podpis a šifrování klíče</span><span class="sxs-lookup"><span data-stu-id="4e32a-113">Key usage includes Digital Signature and Key Encipherment</span></span>
* <span data-ttu-id="4e32a-114">Rozšířené použití klíče obsahuje ověření klienta a ověření serveru</span><span class="sxs-lookup"><span data-stu-id="4e32a-114">Enhanced key usage includes Client Authentication and Server Authentication</span></span>

<span data-ttu-id="4e32a-115">Pokud již nemáte certifikát, který splňuje tyto požadavky, můžete požádat, hello certifikát od certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="4e32a-115">If you don’t already have a certificate that meets these requirements, you can request hello certificate from a certification authority.</span></span> <span data-ttu-id="4e32a-116">Alternativně můžete použít následující příkazy toogenerate hello certifikát podepsaný svým držitelem podle hello operační systém, na kterém spusťte příkaz hello a exportovat certifikát formátu PFX hello s privátním klíčem hello.</span><span class="sxs-lookup"><span data-stu-id="4e32a-116">Alternatively, you can use hello following commands toogenerate hello self-signed certificate based on hello operating system on which you run hello command, and export hello PFX format certificate with private key.</span></span>

* <span data-ttu-id="4e32a-117">**Pro Windows 10 nebo Windows Server 2016**, spusťte předdefinovaný hello **New-SelfSignedCertificate** rutiny prostředí PowerShell následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e32a-117">**For Windows 10 or Windows Server 2016**, run hello built-in **New-SelfSignedCertificate** PowerShell cmdlet as follows:</span></span>

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* <span data-ttu-id="4e32a-118">**Pro operační systémy starší než Windows 10 nebo Windows Server 2016**, stáhněte si hello [certifikát podepsaný svým držitelem generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) z hello Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="4e32a-118">**For operating systems earlier than Windows 10 or Windows Server 2016**, download hello [self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from hello Microsoft Script Center.</span></span> <span data-ttu-id="4e32a-119">Extrahujte obsah a spusťte následující příkazy v příkazovém řádku prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="4e32a-119">Extract its contents and run hello following commands at a PowerShell prompt:</span></span>

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

### <a name="upload-certificate-tooan-azure-key-vault"></a><span data-ttu-id="4e32a-120">Nahrajte certifikát tooan Azure trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="4e32a-120">Upload certificate tooan Azure key vault</span></span>

<span data-ttu-id="4e32a-121">Před nasazením clusteru HPC hello, nahrajte certifikát tooan hello [Azure trezoru klíčů](../../key-vault/index.md) jako tajný a záznamů hello následující informace pro použití během nasazování hello: **název trezoru**,  **Skupina prostředků trezoru**, **adresa URL certifikátu**, a **kryptografický otisk certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="4e32a-121">Before deploying hello HPC cluster, upload hello certificate tooan [Azure key vault](../../key-vault/index.md) as a secret, and record hello following information for use during hello deployment: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

<span data-ttu-id="4e32a-122">Následuje ukázka prostředí PowerShell skriptu tooupload hello certifikát.</span><span class="sxs-lookup"><span data-stu-id="4e32a-122">A sample PowerShell script tooupload hello certificate follows.</span></span> <span data-ttu-id="4e32a-123">Další informace o nahrávání certifikátů tooan Azure trezoru klíčů najdete v tématu [Začínáme s Azure Key Vault](../../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4e32a-123">For more information about uploading a certificate tooan Azure key vault, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md).</span></span>

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


## <a name="supported-topologies"></a><span data-ttu-id="4e32a-124">Podporované topologie</span><span class="sxs-lookup"><span data-stu-id="4e32a-124">Supported topologies</span></span>

<span data-ttu-id="4e32a-125">Vyberte jednu z hello [šablon Azure Resource Manageru](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e32a-125">Choose one of hello [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 cluster.</span></span> <span data-ttu-id="4e32a-126">Následují základní architektury topologií tři podporované clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e32a-126">Following are high-level architectures of three supported cluster topologies.</span></span> <span data-ttu-id="4e32a-127">Topologie vysoké dostupnosti zahrnují více hlavních uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e32a-127">High-availability topologies include multiple cluster head nodes.</span></span>

1. <span data-ttu-id="4e32a-128">Vysoká dostupnost clusteru s doménou služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="4e32a-128">High-availability cluster with Active Directory domain</span></span>

    ![Clusteru HA v doméně AD](./media/hpcpack-2016-cluster/haad.png)



2. <span data-ttu-id="4e32a-130">Vysoká dostupnost clusteru bez domény služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="4e32a-130">High-availability cluster without Active Directory domain</span></span>

    ![Clusteru HA bez domény služby AD](./media/hpcpack-2016-cluster/hanoad.png)

3. <span data-ttu-id="4e32a-132">Cluster s jeden hlavní uzel</span><span class="sxs-lookup"><span data-stu-id="4e32a-132">Cluster with a single head node</span></span>

   ![Cluster s jeden hlavní uzel](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a><span data-ttu-id="4e32a-134">Nasazení clusteru</span><span class="sxs-lookup"><span data-stu-id="4e32a-134">Deploy a cluster</span></span>

<span data-ttu-id="4e32a-135">toocreate hello cluster, vyberte šablonu a klikněte na tlačítko **nasazení tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="4e32a-135">toocreate hello cluster, choose a template and click **Deploy tooAzure**.</span></span> <span data-ttu-id="4e32a-136">V hello [portál Azure](https://portal.azure.com), zadejte parametry šablony hello, jak je popsáno v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="4e32a-136">In hello [Azure portal](https://portal.azure.com), specify parameters for hello template as described in hello following steps.</span></span> <span data-ttu-id="4e32a-137">Každá šablona vytvoří všechny prostředky Azure potřebné pro infrastruktura clusteru HPC hello.</span><span class="sxs-lookup"><span data-stu-id="4e32a-137">Each template creates all Azure resources required for hello HPC cluster infrastructure.</span></span> <span data-ttu-id="4e32a-138">Prostředky zahrnují virtuální síť Azure, veřejné IP adresy, Vyrovnávání zatížení (pouze pro vysokou dostupnost clusteru), síťová rozhraní, skupiny dostupnosti, účty úložiště a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4e32a-138">Resources include an Azure virtual network, public IP address, load balancer (only for a high-availability cluster), network interfaces, availability sets, storage accounts, and virtual machines.</span></span>

### <a name="step-1-select-hello-subscription-location-and-resource-group"></a><span data-ttu-id="4e32a-139">Krok 1: Vyberte předplatné hello, umístění a skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="4e32a-139">Step 1: Select hello subscription, location, and resource group</span></span>

<span data-ttu-id="4e32a-140">Hello **předplatné** a hello **umístění** musí být stejná, kterou jste zadali, když jste nahráli vašeho certifikátu PFX (viz požadavky).</span><span class="sxs-lookup"><span data-stu-id="4e32a-140">hello **Subscription** and hello **Location** must be same that you specified when you uploaded your PFX certificate (see Prerequisites).</span></span> <span data-ttu-id="4e32a-141">Doporučujeme, abyste vytvořili **skupiny prostředků** hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e32a-141">We recommend that you create a **Resource group** for hello deployment.</span></span>

### <a name="step-2-specify-hello-parameter-settings"></a><span data-ttu-id="4e32a-142">Krok 2: Zadejte nastavení parametrů hello</span><span class="sxs-lookup"><span data-stu-id="4e32a-142">Step 2: Specify hello parameter settings</span></span>

<span data-ttu-id="4e32a-143">Zadejte nebo upravte hodnoty pro parametry šablony hello.</span><span class="sxs-lookup"><span data-stu-id="4e32a-143">Enter or modify values for hello template parameters.</span></span> <span data-ttu-id="4e32a-144">Klikněte na tlačítko hello ikonu další tooeach parametr pro informace nápovědy.</span><span class="sxs-lookup"><span data-stu-id="4e32a-144">Click hello icon next tooeach parameter for help information.</span></span> <span data-ttu-id="4e32a-145">Viz také pokyny, hello [dostupných velikostí virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="4e32a-145">Also see hello guidance for [available VM sizes](sizes.md).</span></span>

<span data-ttu-id="4e32a-146">Zadejte hodnoty hello jste si poznamenali v hello předpoklady pro hello následující parametry: **název trezoru**, **skupiny prostředků trezoru**, **adresa URL certifikátu**a **Kryptografický otisk certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="4e32a-146">Specify hello values you recorded in hello Prerequisites for hello following parameters: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

### <a name="step-3-review-legal-terms-and-create"></a><span data-ttu-id="4e32a-147">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="4e32a-147">Step 3.</span></span> <span data-ttu-id="4e32a-148">Přečíst si právní podmínky a vytvořte</span><span class="sxs-lookup"><span data-stu-id="4e32a-148">Review legal terms and create</span></span>
<span data-ttu-id="4e32a-149">Klikněte na tlačítko **přečíst si právní podmínky** tooreview hello podmínky.</span><span class="sxs-lookup"><span data-stu-id="4e32a-149">Click **Review legal terms** tooreview hello terms.</span></span> <span data-ttu-id="4e32a-150">Pokud souhlasíte, klikněte na tlačítko **nákupu**a potom klikněte na **vytvořit** toostart hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e32a-150">If you agree, click **Purchase**, and then click **Create** toostart hello deployment.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="4e32a-151">Připojte toohello cluster</span><span class="sxs-lookup"><span data-stu-id="4e32a-151">Connect toohello cluster</span></span>
1. <span data-ttu-id="4e32a-152">Po nasazení clusteru HPC Pack hello přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e32a-152">After hello HPC Pack cluster is deployed, go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4e32a-153">Klikněte na tlačítko **skupiny prostředků**a skupině prostředků hello najít, na které hello clusteru nasazená.</span><span class="sxs-lookup"><span data-stu-id="4e32a-153">Click **Resource groups**, and find hello resource group in which hello cluster was deployed.</span></span> <span data-ttu-id="4e32a-154">Můžete najít hello hlavního uzlu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4e32a-154">You can find hello head node virtual machines.</span></span>

    ![Hlavních uzlech clusteru hello portálu](./media/hpcpack-2016-cluster/clusterhns.png)

2. <span data-ttu-id="4e32a-156">Klikněte na jeden hlavní uzel (v clusteru s podporou vysokou dostupností, klikněte na některé z hlavních uzlech hello).</span><span class="sxs-lookup"><span data-stu-id="4e32a-156">Click one head node (in a high-availability cluster, click any of hello head nodes).</span></span> <span data-ttu-id="4e32a-157">V **Essentials**, můžete vyhledat hello veřejnou IP adresu nebo úplný název DNS clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4e32a-157">In **Essentials**, you can find hello public IP address or full DNS name of hello cluster.</span></span>

    ![Nastavení připojení clusteru](./media/hpcpack-2016-cluster/clusterconnect.png)

3. <span data-ttu-id="4e32a-159">Klikněte na tlačítko **připojit** toolog na tooany hello pomocí vzdálené plochy s svoje uživatelské jméno zadané správce hlavních uzlech.</span><span class="sxs-lookup"><span data-stu-id="4e32a-159">Click **Connect** toolog on tooany of hello head nodes using Remote Desktop with your specified administrator user name.</span></span> <span data-ttu-id="4e32a-160">Pokud jste nasadili cluster hello v doméně služby Active Directory, hello uživatelské jméno má podobu hello <privateDomainName> \<adminUsername > (například hpc.local\hpcadmin).</span><span class="sxs-lookup"><span data-stu-id="4e32a-160">If hello cluster you deployed is in an Active Directory Domain, hello user name is of hello form <privateDomainName>\<adminUsername> (for example, hpc.local\hpcadmin).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e32a-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e32a-161">Next steps</span></span>
* <span data-ttu-id="4e32a-162">Odeslání úlohy tooyour clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e32a-162">Submit jobs tooyour cluster.</span></span> <span data-ttu-id="4e32a-163">V tématu [odeslání úlohy tooHPC clusteru služby HPC Pack v Azure](hpcpack-cluster-submit-jobs.md) a [spravovat cluster služby HPC Pack 2016 v Azure pomocí Azure Active Directory](hpcpack-cluster-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="4e32a-163">See [Submit jobs tooHPC an HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md) and [Manage an HPC Pack 2016 cluster in Azure using Azure Active Directory](hpcpack-cluster-active-directory.md).</span></span>

