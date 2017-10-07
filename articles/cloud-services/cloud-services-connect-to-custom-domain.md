---
title: "aaaConnect tooa Cloudová služba vlastní řadiče domény | Microsoft Docs"
description: "Zjistěte, jak tooconnect váš web nebo worker role tooa vlastní domény AD pomocí prostředí PowerShell a rozšíření domény AD"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1e2d7c87-d254-4e7a-a832-67f84411ec95
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 9540190ccf17c03e55159c6c68429eee29e0a558
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-azure-cloud-services-roles-tooa-custom-ad-domain-controller-hosted-in-azure"></a><span data-ttu-id="fdc30-103">Připojení tooa vlastní role služeb v cloudu Azure, které řadič domény AD hostované v Azure</span><span class="sxs-lookup"><span data-stu-id="fdc30-103">Connecting Azure Cloud Services Roles tooa custom AD Domain Controller hosted in Azure</span></span>
<span data-ttu-id="fdc30-104">Nejprve nastavíme virtuální síť (VNet) v Azure.</span><span class="sxs-lookup"><span data-stu-id="fdc30-104">We will first set up a Virtual Network (VNet) in Azure.</span></span> <span data-ttu-id="fdc30-105">Potom přidáme řadič domény Active Directory (hostované na virtuální počítač Azure) toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fdc30-105">We will then add an Active Directory Domain Controller (hosted on an Azure Virtual Machine) toohello VNet.</span></span> <span data-ttu-id="fdc30-106">V dalším kroku jsme se přidejte existující cloudové služby role toohello předem vytvořené virtuální síť a pak připojte je toohello řadič domény.</span><span class="sxs-lookup"><span data-stu-id="fdc30-106">Next, we will add existing cloud service roles toohello pre-created VNet, then connect them toohello Domain Controller.</span></span>

<span data-ttu-id="fdc30-107">Než začneme, několik z věcí tookeep nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="fdc30-107">Before we get started, couple of things tookeep in mind:</span></span>

1. <span data-ttu-id="fdc30-108">Tento kurz používá prostředí PowerShell, takže si máte Azure PowerShell, které jsou nainstalované a připravené toogo.</span><span class="sxs-lookup"><span data-stu-id="fdc30-108">This tutorial uses PowerShell, so make sure you have Azure PowerShell installed and ready toogo.</span></span> <span data-ttu-id="fdc30-109">v tématu nápovědy tooget s nastavení prostředí Azure PowerShell [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fdc30-109">tooget help with setting up Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="fdc30-110">Vaše řadiče domény AD a Role Web nebo Worker instance potřebovat toobe v hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fdc30-110">Your AD Domain Controller and Web/Worker Role instances need toobe in hello VNet.</span></span>

<span data-ttu-id="fdc30-111">Postupujte podle podrobných pokynů a pokud narazíte na potíže, nechte nás komentář na konci hello tohoto článku hello.</span><span class="sxs-lookup"><span data-stu-id="fdc30-111">Follow this step-by-step guide and if you run into any issues, leave us a comment at hello end of hello article.</span></span> <span data-ttu-id="fdc30-112">Někdo se vrátit tooyou (Ano, jsme přečíst komentáře).</span><span class="sxs-lookup"><span data-stu-id="fdc30-112">Someone will get back tooyou (yes, we do read comments).</span></span>

<span data-ttu-id="fdc30-113">musí být Hello sítě, který je odkazován hello cloudové služby **klasickou virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="fdc30-113">hello network that is referenced by hello cloud service must be a **classic virtual network**.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="fdc30-114">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="fdc30-114">Create a Virtual Network</span></span>
<span data-ttu-id="fdc30-115">Můžete vytvořit virtuální síť v Azure pomocí hello portál Azure nebo PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fdc30-115">You can create a Virtual Network in Azure using hello Azure portal or PowerShell.</span></span> <span data-ttu-id="fdc30-116">V tomto kurzu budeme používat prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fdc30-116">For this tutorial, we will use PowerShell.</span></span> <span data-ttu-id="fdc30-117">hello toocreate virtuální sítě pomocí portálu Azure najdete [vytvořit virtuální síť](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="fdc30-117">toocreate a Virtual Network using hello Azure portal, see [Create Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="fdc30-118">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fdc30-118">Create a Virtual Machine</span></span>
<span data-ttu-id="fdc30-119">Po dokončení nastavení hello virtuální sítě, budete potřebovat toocreate řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="fdc30-119">Once you have completed setting up hello Virtual Network, you will need toocreate an AD Domain Controller.</span></span> <span data-ttu-id="fdc30-120">V tomto kurzu budeme zadávat řadič domény služby AD na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="fdc30-120">For this tutorial, we will be setting up an AD Domain Controller on an Azure Virtual Machine.</span></span>

<span data-ttu-id="fdc30-121">toodo, vytvoření virtuálního počítače pomocí prostředí PowerShell pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="fdc30-121">toodo this, create a virtual machine through PowerShell using hello following commands:</span></span>

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it toohello Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-tooa-domain-controller"></a><span data-ttu-id="fdc30-122">Zvýšení úrovně tooa vašeho virtuálního počítače řadiče domény</span><span class="sxs-lookup"><span data-stu-id="fdc30-122">Promote your Virtual Machine tooa Domain Controller</span></span>
<span data-ttu-id="fdc30-123">tooconfigure hello virtuální počítač jako řadič domény služby AD, bude nutné toolog v toohello virtuálních počítačů a nakonfigurujte ji.</span><span class="sxs-lookup"><span data-stu-id="fdc30-123">tooconfigure hello Virtual Machine as an AD Domain Controller, you will need toolog in toohello VM and configure it.</span></span>

<span data-ttu-id="fdc30-124">toolog v toohello virtuálních počítačů, můžete získat soubor RDP hello pomocí prostředí PowerShell, hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="fdc30-124">toolog in toohello VM, you can get hello RDP file through PowerShell, use hello following commands:</span></span>

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

<span data-ttu-id="fdc30-125">Po přihlášení toohello virtuálního počítače nastavit virtuální počítač jako řadič domény služby AD tím následující hello podrobný Průvodce na [jak tooset zákazníka řadič domény AD](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="fdc30-125">Once you are signed in toohello VM, set up your Virtual Machine as an AD Domain Controller by following hello step-by-step guide on [How tooset up your customer AD Domain Controller](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).</span></span>

## <a name="add-your-cloud-service-toohello-virtual-network"></a><span data-ttu-id="fdc30-126">Přidat vaše cloudové služby toohello virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="fdc30-126">Add your Cloud Service toohello Virtual Network</span></span>
<span data-ttu-id="fdc30-127">Dále je nutné tooadd vaše cloudové služby nasazení toohello nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fdc30-127">Next, you need tooadd your cloud service deployment toohello new VNet.</span></span> <span data-ttu-id="fdc30-128">toodo, vaše cloudové služby cscfg upravit přidáním hello příslušné části tooyour cscfg pomocí sady Visual Studio nebo hello editoru podle své volby.</span><span class="sxs-lookup"><span data-stu-id="fdc30-128">toodo this, modify your cloud service cscfg by adding hello relevant sections tooyour cscfg using Visual Studio or hello editor of your choice.</span></span>

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

<span data-ttu-id="fdc30-129">Další sestavení projektu cloudové služby a nasaďte ji tooAzure.</span><span class="sxs-lookup"><span data-stu-id="fdc30-129">Next build your cloud services project and deploy it tooAzure.</span></span> <span data-ttu-id="fdc30-130">tooget nápovědu k nasazení vaší tooAzure balíček cloud services najdete v části [jak tooCreate a nasazení cloudové služby](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)</span><span class="sxs-lookup"><span data-stu-id="fdc30-130">tooget help with deploying your cloud services package tooAzure, see [How tooCreate and Deploy a Cloud Service](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)</span></span>

## <a name="connect-your-webworker-roles-toohello-domain"></a><span data-ttu-id="fdc30-131">Připojit vaše doména toohello role web nebo worker</span><span class="sxs-lookup"><span data-stu-id="fdc30-131">Connect your web/worker roles toohello domain</span></span>
<span data-ttu-id="fdc30-132">Jakmile se projekt cloudové služby je nasazen na Azure, připojte doménu toohello vlastní AD instancí role pomocí hello rozšíření domény AD.</span><span class="sxs-lookup"><span data-stu-id="fdc30-132">Once your cloud service project is deployed on Azure, connect your role instances toohello custom AD domain using hello AD Domain Extension.</span></span> <span data-ttu-id="fdc30-133">tooadd hello rozšíření domény AD tooyour existující cloudové služby nasazení a připojení k hello vlastní doménu, spusťte následující příkazy v prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="fdc30-133">tooadd hello AD Domain Extension tooyour existing cloud services deployment and join hello custom domain, execute hello following commands in PowerShell:</span></span>

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension toohello cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

<span data-ttu-id="fdc30-134">A je to.</span><span class="sxs-lookup"><span data-stu-id="fdc30-134">And that's it.</span></span>

<span data-ttu-id="fdc30-135">Cloudové služby by měl být připojený k tooyour řadič vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="fdc30-135">Your cloud services should be joined tooyour custom domain controller.</span></span> <span data-ttu-id="fdc30-136">Pokud vás zajímají další informace o hello různé možnosti, které jsou k dispozici pro toolearn jak pomoci tooconfigure rozšíření domény AD, použijte hello prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fdc30-136">If you would like toolearn more about hello different options available for how tooconfigure AD Domain Extension, use hello PowerShell help.</span></span> <span data-ttu-id="fdc30-137">Pár příkladů postupujte podle kroků:</span><span class="sxs-lookup"><span data-stu-id="fdc30-137">A couple of examples follow:</span></span>

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
