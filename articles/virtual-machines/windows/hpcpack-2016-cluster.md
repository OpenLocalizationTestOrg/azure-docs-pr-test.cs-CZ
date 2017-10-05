---
title: Cluster HPC Pack 2016 v Azure | Microsoft Docs
description: "Postup nasazení clusteru HPC Pack 2016 v Azure"
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
ms.openlocfilehash: 88d1f4e29f38ba1a6bef57c2da43bee205575eee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a><span data-ttu-id="d8887-103">Nasazení clusteru HPC Pack 2016 v Azure</span><span class="sxs-lookup"><span data-stu-id="d8887-103">Deploy an HPC Pack 2016 cluster in Azure</span></span>

<span data-ttu-id="d8887-104">Postupujte podle kroků v tomto článku k nasazení [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) clusteru ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="d8887-104">Follow the steps in this article to deploy a [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) cluster in Azure virtual machines.</span></span> <span data-ttu-id="d8887-105">HPC Pack je společnosti Microsoft volné HPC řešení založen na technologiích Microsoft Azure a Windows Server a podporuje úlohy HPC široký rozsah.</span><span class="sxs-lookup"><span data-stu-id="d8887-105">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies and supports a wide range of HPC workloads.</span></span>

<span data-ttu-id="d8887-106">Použijte jednu z [šablon Azure Resource Manageru](https://github.com/MsHpcPack/HPCPack2016) k nasazení clusteru HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="d8887-106">Use one of the [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) to deploy the HPC Pack 2016 cluster.</span></span> <span data-ttu-id="d8887-107">Máte několik možností topologie clusteru s různý počet hlavních uzlech clusteru a se buď Linux nebo Windows výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="d8887-107">You have several choices of cluster topology with different numbers of cluster head nodes, and with either Linux or Windows compute nodes.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8887-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d8887-108">Prerequisites</span></span>

### <a name="pfx-certificate"></a><span data-ttu-id="d8887-109">Certifikát PFX</span><span class="sxs-lookup"><span data-stu-id="d8887-109">PFX certificate</span></span>

<span data-ttu-id="d8887-110">Cluster s podporou Microsoft HPC Pack 2016 vyžaduje certifikát Personal Information Exchange (PFX) k zabezpečení komunikace mezi uzly HPC.</span><span class="sxs-lookup"><span data-stu-id="d8887-110">A Microsoft HPC Pack 2016 cluster requires a Personal Information Exchange (PFX) certificate to secure the communication between the HPC nodes.</span></span> <span data-ttu-id="d8887-111">Certifikát musí splňovat následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="d8887-111">The certificate must meet the following requirements:</span></span>

* <span data-ttu-id="d8887-112">Musí mít privátní klíč umožňuje výměnu klíčů</span><span class="sxs-lookup"><span data-stu-id="d8887-112">It must have a private key capable of key exchange</span></span>
* <span data-ttu-id="d8887-113">Použití klíče zahrnuje digitální podpis a šifrování klíče</span><span class="sxs-lookup"><span data-stu-id="d8887-113">Key usage includes Digital Signature and Key Encipherment</span></span>
* <span data-ttu-id="d8887-114">Rozšířené použití klíče obsahuje ověření klienta a ověření serveru</span><span class="sxs-lookup"><span data-stu-id="d8887-114">Enhanced key usage includes Client Authentication and Server Authentication</span></span>

<span data-ttu-id="d8887-115">Pokud již nemáte certifikát, který splňuje tyto požadavky, můžete požádat o certifikát od certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="d8887-115">If you don’t already have a certificate that meets these requirements, you can request the certificate from a certification authority.</span></span> <span data-ttu-id="d8887-116">Alternativně můžete použít následující příkazy ke generování certifikát podepsaný svým držitelem, který je založený na operačním systému, na kterém jste příkaz spustili a exportujte certifikát formátu PFX s privátním klíčem.</span><span class="sxs-lookup"><span data-stu-id="d8887-116">Alternatively, you can use the following commands to generate the self-signed certificate based on the operating system on which you run the command, and export the PFX format certificate with private key.</span></span>

* <span data-ttu-id="d8887-117">**Pro Windows 10 nebo Windows Server 2016**, spusťte předdefinovaný **New-SelfSignedCertificate** rutiny prostředí PowerShell následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d8887-117">**For Windows 10 or Windows Server 2016**, run the built-in **New-SelfSignedCertificate** PowerShell cmdlet as follows:</span></span>

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* <span data-ttu-id="d8887-118">**Pro operační systémy starší než Windows 10 nebo Windows Server 2016**, stáhněte si [certifikát podepsaný svým držitelem generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) z webu Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="d8887-118">**For operating systems earlier than Windows 10 or Windows Server 2016**, download the [self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from the Microsoft Script Center.</span></span> <span data-ttu-id="d8887-119">Rozbalte jeho obsah a spusťte následující příkazy v příkazovém řádku prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d8887-119">Extract its contents and run the following commands at a PowerShell prompt:</span></span>

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

### <a name="upload-certificate-to-an-azure-key-vault"></a><span data-ttu-id="d8887-120">Nahrajte certifikát do trezoru služby Azure klíče</span><span class="sxs-lookup"><span data-stu-id="d8887-120">Upload certificate to an Azure key vault</span></span>

<span data-ttu-id="d8887-121">Před nasazením clusteru HPC, nahrajte certifikát, který chcete [Azure trezoru klíčů](../../key-vault/index.md) jako tajný klíč a záznam následující informace pro použití při nasazení: **název trezoru**, **skupiny prostředků trezoru**, **adresa URL certifikátu**, a **kryptografický otisk certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="d8887-121">Before deploying the HPC cluster, upload the certificate to an [Azure key vault](../../key-vault/index.md) as a secret, and record the following information for use during the deployment: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

<span data-ttu-id="d8887-122">Následuje ukázkový skript PowerShell, na kterou odešlete certifikát.</span><span class="sxs-lookup"><span data-stu-id="d8887-122">A sample PowerShell script to upload the certificate follows.</span></span> <span data-ttu-id="d8887-123">Další informace o certifikátu se nahrávají na klíče trezoru služby Azure najdete v tématu [Začínáme s Azure Key Vault](../../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d8887-123">For more information about uploading a certificate to an Azure key vault, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md).</span></span>

```powershell
#Give the following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate the pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode the JSON object
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
#Create an Azure key vault and upload the certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"The following Information will be used in the deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a><span data-ttu-id="d8887-124">Podporované topologie</span><span class="sxs-lookup"><span data-stu-id="d8887-124">Supported topologies</span></span>

<span data-ttu-id="d8887-125">Vyberte jednu z [šablon Azure Resource Manageru](https://github.com/MsHpcPack/HPCPack2016) k nasazení clusteru HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="d8887-125">Choose one of the [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) to deploy the HPC Pack 2016 cluster.</span></span> <span data-ttu-id="d8887-126">Následují základní architektury topologií tři podporované clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8887-126">Following are high-level architectures of three supported cluster topologies.</span></span> <span data-ttu-id="d8887-127">Topologie vysoké dostupnosti zahrnují více hlavních uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8887-127">High-availability topologies include multiple cluster head nodes.</span></span>

1. <span data-ttu-id="d8887-128">Vysoká dostupnost clusteru s doménou služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="d8887-128">High-availability cluster with Active Directory domain</span></span>

    ![Clusteru HA v doméně AD](./media/hpcpack-2016-cluster/haad.png)



2. <span data-ttu-id="d8887-130">Vysoká dostupnost clusteru bez domény služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="d8887-130">High-availability cluster without Active Directory domain</span></span>

    ![Clusteru HA bez domény služby AD](./media/hpcpack-2016-cluster/hanoad.png)

3. <span data-ttu-id="d8887-132">Cluster s jeden hlavní uzel</span><span class="sxs-lookup"><span data-stu-id="d8887-132">Cluster with a single head node</span></span>

   ![Cluster s jeden hlavní uzel](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a><span data-ttu-id="d8887-134">Nasazení clusteru</span><span class="sxs-lookup"><span data-stu-id="d8887-134">Deploy a cluster</span></span>

<span data-ttu-id="d8887-135">Pokud chcete vytvořit cluster, vyberte šablonu a klikněte na tlačítko **nasadit do Azure**.</span><span class="sxs-lookup"><span data-stu-id="d8887-135">To create the cluster, choose a template and click **Deploy to Azure**.</span></span> <span data-ttu-id="d8887-136">V [portál Azure](https://portal.azure.com), zadejte parametry šablony, jak je popsáno v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="d8887-136">In the [Azure portal](https://portal.azure.com), specify parameters for the template as described in the following steps.</span></span> <span data-ttu-id="d8887-137">Každá šablona vytvoří všechny prostředky Azure potřebné pro infrastruktura clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="d8887-137">Each template creates all Azure resources required for the HPC cluster infrastructure.</span></span> <span data-ttu-id="d8887-138">Prostředky zahrnují virtuální síť Azure, veřejné IP adresy, Vyrovnávání zatížení (pouze pro vysokou dostupnost clusteru), síťová rozhraní, skupiny dostupnosti, účty úložiště a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d8887-138">Resources include an Azure virtual network, public IP address, load balancer (only for a high-availability cluster), network interfaces, availability sets, storage accounts, and virtual machines.</span></span>

### <a name="step-1-select-the-subscription-location-and-resource-group"></a><span data-ttu-id="d8887-139">Krok 1: Vyberte předplatné, umístění a skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="d8887-139">Step 1: Select the subscription, location, and resource group</span></span>

<span data-ttu-id="d8887-140">**Předplatné** a **umístění** musí být stejná, kterou jste zadali, když jste nahráli vašeho certifikátu PFX (viz požadavky).</span><span class="sxs-lookup"><span data-stu-id="d8887-140">The **Subscription** and the **Location** must be same that you specified when you uploaded your PFX certificate (see Prerequisites).</span></span> <span data-ttu-id="d8887-141">Doporučujeme, abyste vytvořili **skupiny prostředků** pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="d8887-141">We recommend that you create a **Resource group** for the deployment.</span></span>

### <a name="step-2-specify-the-parameter-settings"></a><span data-ttu-id="d8887-142">Krok 2: Zadejte nastavení parametru</span><span class="sxs-lookup"><span data-stu-id="d8887-142">Step 2: Specify the parameter settings</span></span>

<span data-ttu-id="d8887-143">Zadejte nebo upravte hodnoty pro parametry šablony.</span><span class="sxs-lookup"><span data-stu-id="d8887-143">Enter or modify values for the template parameters.</span></span> <span data-ttu-id="d8887-144">Klikněte na ikonu vedle jednotlivých parametrů pro informace nápovědy.</span><span class="sxs-lookup"><span data-stu-id="d8887-144">Click the icon next to each parameter for help information.</span></span> <span data-ttu-id="d8887-145">Viz také pokyny pro [dostupných velikostí virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="d8887-145">Also see the guidance for [available VM sizes](sizes.md).</span></span>

<span data-ttu-id="d8887-146">Zadejte hodnoty, které jste si poznamenali v požadavky pro tyto parametry: **název trezoru**, **skupiny prostředků trezoru**, **adresa URL certifikátu**, a **kryptografický otisk certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="d8887-146">Specify the values you recorded in the Prerequisites for the following parameters: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

### <a name="step-3-review-legal-terms-and-create"></a><span data-ttu-id="d8887-147">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="d8887-147">Step 3.</span></span> <span data-ttu-id="d8887-148">Přečíst si právní podmínky a vytvořte</span><span class="sxs-lookup"><span data-stu-id="d8887-148">Review legal terms and create</span></span>
<span data-ttu-id="d8887-149">Klikněte na tlačítko **přečíst si právní podmínky** k přečtení podmínek.</span><span class="sxs-lookup"><span data-stu-id="d8887-149">Click **Review legal terms** to review the terms.</span></span> <span data-ttu-id="d8887-150">Pokud souhlasíte, klikněte na tlačítko **nákupu**a potom klikněte na **vytvořit** ke spuštění nasazení.</span><span class="sxs-lookup"><span data-stu-id="d8887-150">If you agree, click **Purchase**, and then click **Create** to start the deployment.</span></span>

## <a name="connect-to-the-cluster"></a><span data-ttu-id="d8887-151">Připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="d8887-151">Connect to the cluster</span></span>
1. <span data-ttu-id="d8887-152">Po nasazení clusteru HPC Pack, přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d8887-152">After the HPC Pack cluster is deployed, go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="d8887-153">Klikněte na tlačítko **skupiny prostředků**a najít skupinu prostředků, ve kterém byl nasazen clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8887-153">Click **Resource groups**, and find the resource group in which the cluster was deployed.</span></span> <span data-ttu-id="d8887-154">Můžete získat virtuální počítače hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="d8887-154">You can find the head node virtual machines.</span></span>

    ![Head uzly clusteru na portálu](./media/hpcpack-2016-cluster/clusterhns.png)

2. <span data-ttu-id="d8887-156">Klikněte na jeden hlavní uzel (v clusteru s podporou vysokou dostupností, klikněte na některé z hlavních uzlech).</span><span class="sxs-lookup"><span data-stu-id="d8887-156">Click one head node (in a high-availability cluster, click any of the head nodes).</span></span> <span data-ttu-id="d8887-157">V **Essentials**, můžete vyhledat veřejnou IP adresu nebo úplný název DNS clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8887-157">In **Essentials**, you can find the public IP address or full DNS name of the cluster.</span></span>

    ![Nastavení připojení clusteru](./media/hpcpack-2016-cluster/clusterconnect.png)

3. <span data-ttu-id="d8887-159">Klikněte na tlačítko **Connect** chcete přihlásit k některé z hlavních uzlech pomocí vzdálené plochy s svoje uživatelské jméno zadané správce.</span><span class="sxs-lookup"><span data-stu-id="d8887-159">Click **Connect** to log on to any of the head nodes using Remote Desktop with your specified administrator user name.</span></span> <span data-ttu-id="d8887-160">Pokud jste nasadili cluster nachází v doméně služby Active Directory, je uživatelské jméno ve tvaru <privateDomainName> \<adminUsername > (například hpc.local\hpcadmin).</span><span class="sxs-lookup"><span data-stu-id="d8887-160">If the cluster you deployed is in an Active Directory Domain, the user name is of the form <privateDomainName>\<adminUsername> (for example, hpc.local\hpcadmin).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8887-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8887-161">Next steps</span></span>
* <span data-ttu-id="d8887-162">Odesílání úloh do clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8887-162">Submit jobs to your cluster.</span></span> <span data-ttu-id="d8887-163">V tématu [odeslání úlohy HPC HPC Pack clusteru v Azure](hpcpack-cluster-submit-jobs.md) a [spravovat cluster služby HPC Pack 2016 v Azure pomocí Azure Active Directory](hpcpack-cluster-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="d8887-163">See [Submit jobs to HPC an HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md) and [Manage an HPC Pack 2016 cluster in Azure using Azure Active Directory](hpcpack-cluster-active-directory.md).</span></span>

