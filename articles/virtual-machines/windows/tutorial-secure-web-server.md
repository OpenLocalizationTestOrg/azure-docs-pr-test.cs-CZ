---
title: "aaaSecure služby IIS pomocí protokolu SSL certifikáty v Azure | Microsoft Docs"
description: "Zjistěte, jak toosecure hello IIS webový server s certifikáty protokolu SSL na virtuální počítač s Windows v Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 7a9e0ce07be2f55095fdb5347b64faf5caa4f7e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="65209-103">Zabezpečení webového serveru IIS s certifikáty protokolu SSL na virtuální počítač Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="65209-103">Secure IIS web server with SSL certificates on a Windows virtual machine in Azure</span></span>
<span data-ttu-id="65209-104">toosecure webových serverů, může být certifikát později SSL (Secure Sockets) používá tooencrypt webový provoz.</span><span class="sxs-lookup"><span data-stu-id="65209-104">toosecure web servers, a Secure Sockets Later (SSL) certificate can be used tooencrypt web traffic.</span></span> <span data-ttu-id="65209-105">Tyto certifikáty SSL můžou být uložená v Azure Key Vault a povolit zabezpečená nasazení certifikátů tooWindows virtuálních počítačů (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="65209-105">These SSL certificates can be stored in Azure Key Vault, and allow secure deployments of certificates tooWindows virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="65209-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="65209-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65209-107">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="65209-107">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="65209-108">Generovat nebo nahrát certifikát toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="65209-108">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="65209-109">Vytvoření virtuálního počítače a instalaci webového serveru IIS hello</span><span class="sxs-lookup"><span data-stu-id="65209-109">Create a VM and install hello IIS web server</span></span>
> * <span data-ttu-id="65209-110">Vložit hello certifikát do hello virtuálních počítačů a konfiguraci služby IIS s vazbou SSL</span><span class="sxs-lookup"><span data-stu-id="65209-110">Inject hello certificate into hello VM and configure IIS with an SSL binding</span></span>

<span data-ttu-id="65209-111">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="65209-111">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="65209-112">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="65209-112">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="65209-113">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="65209-113">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="overview"></a><span data-ttu-id="65209-114">Přehled</span><span class="sxs-lookup"><span data-stu-id="65209-114">Overview</span></span>
<span data-ttu-id="65209-115">Azure Key Vault chrání kryptografické klíče a tajné klíče, tyto certifikáty a hesla.</span><span class="sxs-lookup"><span data-stu-id="65209-115">Azure Key Vault safeguards cryptographic keys and secrets, such certificates or passwords.</span></span> <span data-ttu-id="65209-116">Key Vault pomáhá zjednodušit proces správy certifikátů hello a umožní vám toomaintain kontrolu nad klíči, které přístup těchto certifikátů.</span><span class="sxs-lookup"><span data-stu-id="65209-116">Key Vault helps streamline hello certificate management process and enables you toomaintain control of keys that access those certificates.</span></span> <span data-ttu-id="65209-117">Můžete vytvořit certifikát podepsaný svým držitelem v Key Vault nebo nahrát certifikát existující, důvěryhodné, který už vlastníte.</span><span class="sxs-lookup"><span data-stu-id="65209-117">You can create a self-signed certificate inside Key Vault, or upload an existing, trusted certificate that you already own.</span></span>

<span data-ttu-id="65209-118">Místo použití vlastní image virtuálního počítače, který zahrnuje certifikáty zaručená v, vložit certifikáty do spuštěného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="65209-118">Rather than using a custom VM image that includes certificates baked-in, you inject certificates into a running VM.</span></span> <span data-ttu-id="65209-119">Tento proces zajišťuje instalaci hello nejaktuálnější certifikáty na webovém serveru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="65209-119">This process ensures that hello most up-to-date certificates are installed on a web server during deployment.</span></span> <span data-ttu-id="65209-120">Je-li obnovit nebo nahradit certifikát, také nemáte toocreate novou vlastní bitovou kopii virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="65209-120">If you renew or replace a certificate, you don't also have toocreate a new custom VM image.</span></span> <span data-ttu-id="65209-121">Hello nejnovější certifikáty jsou automaticky vložit, jako je vytváření dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="65209-121">hello latest certificates are automatically injected as you create additional VMs.</span></span> <span data-ttu-id="65209-122">Během celého procesu hello nikdy certifikáty hello nechte hello platformy Azure nebo jsou viditelné ve skriptu, historie příkazového řádku nebo šablony.</span><span class="sxs-lookup"><span data-stu-id="65209-122">During hello whole process, hello certificates never leave hello Azure platform or are exposed in a script, command-line history, or template.</span></span>


## <a name="create-an-azure-key-vault"></a><span data-ttu-id="65209-123">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="65209-123">Create an Azure Key Vault</span></span>
<span data-ttu-id="65209-124">Před vytvořením Key Vault a certifikáty, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="65209-124">Before you can create a Key Vault and certificates, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="65209-125">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupSecureWeb* v hello *východní USA* umístění:</span><span class="sxs-lookup"><span data-stu-id="65209-125">hello following example creates a resource group named *myResourceGroupSecureWeb* in hello *East US* location:</span></span>

```powershell
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

<span data-ttu-id="65209-126">Dále vytvořte Key Vault s [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault).</span><span class="sxs-lookup"><span data-stu-id="65209-126">Next, create a Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault).</span></span> <span data-ttu-id="65209-127">Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena.</span><span class="sxs-lookup"><span data-stu-id="65209-127">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="65209-128">Nahraďte `<mykeyvault>` v hello následující příklad s svůj vlastní jedinečný název pro Key Vault:</span><span class="sxs-lookup"><span data-stu-id="65209-128">Replace `<mykeyvault>` in hello following example with your own unique Key Vault name:</span></span>

```powershell
$keyvaultName="<mykeyvault>"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a><span data-ttu-id="65209-129">Vygenerování certifikátu a uložit v Key Vault</span><span class="sxs-lookup"><span data-stu-id="65209-129">Generate a certificate and store in Key Vault</span></span>
<span data-ttu-id="65209-130">Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [Import AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate).</span><span class="sxs-lookup"><span data-stu-id="65209-130">For production use, you should import a valid certificate signed by trusted provider with [Import-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate).</span></span> <span data-ttu-id="65209-131">V tomto kurzu hello následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [přidat AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) že hello používá výchozí zásady certifikátů z [ Nové AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy).</span><span class="sxs-lookup"><span data-stu-id="65209-131">For this tutorial, hello following example shows how you can generate a self-signed certificate with [Add-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) that uses hello default certificate policy from [New-AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy).</span></span> 

```powershell
$policy = New-AzureKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzureKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a><span data-ttu-id="65209-132">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="65209-132">Create a virtual machine</span></span>
<span data-ttu-id="65209-133">Sada správce uživatelské jméno a heslo pro virtuální počítač s hello [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="65209-133">Set an administrator username and password for hello VM with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="65209-134">Nyní můžete vytvořit virtuální počítač s hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="65209-134">Now you can create hello VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="65209-135">Hello následující příklad vytvoří hello požadované součásti virtuální sítě, konfiguraci hello operačního systému a poté vytvoří virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="65209-135">hello following example creates hello required virtual network components, hello OS configuration, and then creates a VM named *myVM*:</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myVnet" `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleRDP"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 443
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleWWW"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name "myNic" `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2" | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName "myVM" -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2016-Datacenter" -Version "latest" | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

# Use hello Custom Script Extension tooinstall IIS
Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

<span data-ttu-id="65209-136">Jak dlouho trvá několik minut, než toobe hello virtuální počítač vytvořit.</span><span class="sxs-lookup"><span data-stu-id="65209-136">It takes a few minutes for hello VM toobe created.</span></span> <span data-ttu-id="65209-137">Hello poslední krok používá hello rozšíření vlastních skriptů Azure tooinstall hello webového serveru IIS s [Set-AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).</span><span class="sxs-lookup"><span data-stu-id="65209-137">hello last step uses hello Azure Custom Script Extension tooinstall hello IIS web server with [Set-AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).</span></span>


## <a name="add-a-certificate-toovm-from-key-vault"></a><span data-ttu-id="65209-138">Přidat certifikát tooVM z Key Vault</span><span class="sxs-lookup"><span data-stu-id="65209-138">Add a certificate tooVM from Key Vault</span></span>
<span data-ttu-id="65209-139">certifikát hello tooadd z tooa Key Vault virtuálních počítačů, získat hello ID vašeho certifikátu s [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret).</span><span class="sxs-lookup"><span data-stu-id="65209-139">tooadd hello certificate from Key Vault tooa VM, obtain hello ID of your certificate with [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret).</span></span> <span data-ttu-id="65209-140">Přidat certifikát toohello hello virtuálních počítačů s [přidat AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):</span><span class="sxs-lookup"><span data-stu-id="65209-140">Add hello certificate toohello VM with [Add-AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):</span></span>

```powershell
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-toouse-hello-certificate"></a><span data-ttu-id="65209-141">Konfigurace služby IIS toouse hello certifikátu</span><span class="sxs-lookup"><span data-stu-id="65209-141">Configure IIS toouse hello certificate</span></span>
<span data-ttu-id="65209-142">Použití hello rozšíření vlastních skriptů znovu s [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) konfigurace služby IIS tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="65209-142">Use hello Custom Script Extension again with [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooupdate hello IIS configuration.</span></span> <span data-ttu-id="65209-143">Tato aktualizace použije certifikát hello vložili z tooIIS Key Vault a nakonfiguruje hello webové vazby:</span><span class="sxs-lookup"><span data-stu-id="65209-143">This update applies hello certificate injected from Key Vault tooIIS and configures hello web binding:</span></span>

```powershell
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-hello-secure-web-app"></a><span data-ttu-id="65209-144">Testování hello zabezpečení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="65209-144">Test hello secure web app</span></span>
<span data-ttu-id="65209-145">Získat hello veřejnou IP adresu vašeho virtuálního počítače s [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="65209-145">Obtain hello public IP address of your VM with [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="65209-146">Hello následující příklad získá hello IP adresu pro `myPublicIP` vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="65209-146">hello following example obtains hello IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIP" | select "IpAddress"
```

<span data-ttu-id="65209-147">Nyní můžete otevřít webový prohlížeč a zadejte `https://<myPublicIP>` v panelu Adresa hello.</span><span class="sxs-lookup"><span data-stu-id="65209-147">Now you can open a web browser and enter `https://<myPublicIP>` in hello address bar.</span></span> <span data-ttu-id="65209-148">Upozornění zabezpečení hello tooaccept Pokud použijete certifikát podepsaný svým držitelem, vyberte **podrobnosti** a potom **přejděte na webové stránce toohello**:</span><span class="sxs-lookup"><span data-stu-id="65209-148">tooaccept hello security warning if you used a self-signed certificate, select **Details** and then **Go on toohello webpage**:</span></span>

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-secure-web-server/browser-warning.png)

<span data-ttu-id="65209-150">Zabezpečené webu služby IIS se následně zobrazí jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="65209-150">Your secured IIS website is then displayed as in hello following example:</span></span>

![Web služby IIS spuštěná zabezpečené zobrazení](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a><span data-ttu-id="65209-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65209-152">Next steps</span></span>

<span data-ttu-id="65209-153">V tomto kurzu zabezpečená certifikátem SSL uložené v Azure Key Vault webový server služby IIS.</span><span class="sxs-lookup"><span data-stu-id="65209-153">In this tutorial, you secured an IIS web server with an SSL certificate stored in Azure Key Vault.</span></span> <span data-ttu-id="65209-154">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="65209-154">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65209-155">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="65209-155">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="65209-156">Generovat nebo nahrát certifikát toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="65209-156">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="65209-157">Vytvoření virtuálního počítače a instalaci webového serveru IIS hello</span><span class="sxs-lookup"><span data-stu-id="65209-157">Create a VM and install hello IIS web server</span></span>
> * <span data-ttu-id="65209-158">Vložit hello certifikát do hello virtuálních počítačů a konfiguraci služby IIS s vazbou SSL</span><span class="sxs-lookup"><span data-stu-id="65209-158">Inject hello certificate into hello VM and configure IIS with an SSL binding</span></span>

<span data-ttu-id="65209-159">Použijte tento odkaz toosee předem vytvořené ukázky skript virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="65209-159">Follow this link toosee pre-built virtual machine script samples.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="65209-160">Ukázky skriptu Windows virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="65209-160">Windows virtual machine script samples</span></span>](./powershell-samples.md)