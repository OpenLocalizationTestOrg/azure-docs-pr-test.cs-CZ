---
title: "Připojení ke cloudové službě k vlastní řadiče domény | Microsoft Docs"
description: "Zjistěte, jak připojit vaše role web nebo worker k vlastní doméně AD pomocí prostředí PowerShell a rozšíření AD domény"
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
ms.openlocfilehash: 17f6918371678ac849198bff4e3b3eea8678c660
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a><span data-ttu-id="33eef-103">Připojení k řadiči domény AD hostované v Azure vlastní role služeb v cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="33eef-103">Connecting Azure Cloud Services Roles to a custom AD Domain Controller hosted in Azure</span></span>
<span data-ttu-id="33eef-104">Nejprve nastavíme virtuální síť (VNet) v Azure.</span><span class="sxs-lookup"><span data-stu-id="33eef-104">We will first set up a Virtual Network (VNet) in Azure.</span></span> <span data-ttu-id="33eef-105">Potom přidáme Active Directory řadiče domény (hostované na virtuální počítač Azure) k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="33eef-105">We will then add an Active Directory Domain Controller (hosted on an Azure Virtual Machine) to the VNet.</span></span> <span data-ttu-id="33eef-106">V dalším kroku jsme se přidat existující role cloudové služby na předem vytvořené virtuální síť a potom jejich připojení k řadiči domény.</span><span class="sxs-lookup"><span data-stu-id="33eef-106">Next, we will add existing cloud service roles to the pre-created VNet, then connect them to the Domain Controller.</span></span>

<span data-ttu-id="33eef-107">Než začneme, několik věcí mějte na paměti, které:</span><span class="sxs-lookup"><span data-stu-id="33eef-107">Before we get started, couple of things to keep in mind:</span></span>

1. <span data-ttu-id="33eef-108">Tento kurz používá prostředí PowerShell, proto se ujistěte, že máte Azure PowerShell nainstalované a připravené na vynucování.</span><span class="sxs-lookup"><span data-stu-id="33eef-108">This tutorial uses PowerShell, so make sure you have Azure PowerShell installed and ready to go.</span></span> <span data-ttu-id="33eef-109">Chcete-li získat nápovědu pro nastavení prostředí Azure PowerShell, přečtěte si téma [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="33eef-109">To get help with setting up Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="33eef-110">Vaše řadiče domény AD a Role Web nebo Worker instance musí být ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="33eef-110">Your AD Domain Controller and Web/Worker Role instances need to be in the VNet.</span></span>

<span data-ttu-id="33eef-111">Postupujte podle podrobných pokynů a pokud narazíte na potíže, nechte nás komentář na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="33eef-111">Follow this step-by-step guide and if you run into any issues, leave us a comment at the end of the article.</span></span> <span data-ttu-id="33eef-112">Někdo se vrátit k vám (Ano, jsme přečíst komentáře).</span><span class="sxs-lookup"><span data-stu-id="33eef-112">Someone will get back to you (yes, we do read comments).</span></span>

<span data-ttu-id="33eef-113">Musí být sítě, který je odkazován cloudové služby **klasickou virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="33eef-113">The network that is referenced by the cloud service must be a **classic virtual network**.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="33eef-114">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="33eef-114">Create a Virtual Network</span></span>
<span data-ttu-id="33eef-115">Můžete vytvořit virtuální síť v Azure pomocí portálu Azure nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33eef-115">You can create a Virtual Network in Azure using the Azure portal or PowerShell.</span></span> <span data-ttu-id="33eef-116">V tomto kurzu budeme používat prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33eef-116">For this tutorial, we will use PowerShell.</span></span> <span data-ttu-id="33eef-117">Vytvoření virtuální sítě pomocí portálu Azure, naleznete v části [vytvořit virtuální síť](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="33eef-117">To create a Virtual Network using the Azure portal, see [Create Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>

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

## <a name="create-a-virtual-machine"></a><span data-ttu-id="33eef-118">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="33eef-118">Create a Virtual Machine</span></span>
<span data-ttu-id="33eef-119">Po dokončení nastavení virtuální sítě, musíte vytvořit řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="33eef-119">Once you have completed setting up the Virtual Network, you will need to create an AD Domain Controller.</span></span> <span data-ttu-id="33eef-120">V tomto kurzu budeme zadávat řadič domény služby AD na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="33eef-120">For this tutorial, we will be setting up an AD Domain Controller on an Azure Virtual Machine.</span></span>

<span data-ttu-id="33eef-121">K tomu, vytvoření virtuálního počítače pomocí prostředí PowerShell pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="33eef-121">To do this, create a virtual machine through PowerShell using the following commands:</span></span>

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

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a><span data-ttu-id="33eef-122">Povýšení na řadič domény virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="33eef-122">Promote your Virtual Machine to a Domain Controller</span></span>
<span data-ttu-id="33eef-123">Konfigurace virtuálního počítače jako řadič domény AD, musíte se přihlaste k virtuálnímu počítači a jeho konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="33eef-123">To configure the Virtual Machine as an AD Domain Controller, you will need to log in to the VM and configure it.</span></span>

<span data-ttu-id="33eef-124">Přihlásit se k virtuálnímu počítači, můžete si stáhnout soubor RDP pomocí prostředí PowerShell, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="33eef-124">To log in to the VM, you can get the RDP file through PowerShell, use the following commands:</span></span>

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

<span data-ttu-id="33eef-125">Po přihlášení k virtuálnímu počítači nastavit virtuální počítač jako řadič domény služby AD podle podrobný průvodce [jak nastavit zákazníkovi řadič domény AD](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="33eef-125">Once you are signed in to the VM, set up your Virtual Machine as an AD Domain Controller by following the step-by-step guide on [How to set up your customer AD Domain Controller](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).</span></span>

## <a name="add-your-cloud-service-to-the-virtual-network"></a><span data-ttu-id="33eef-126">Přidat cloudové služby na virtuální síť</span><span class="sxs-lookup"><span data-stu-id="33eef-126">Add your Cloud Service to the Virtual Network</span></span>
<span data-ttu-id="33eef-127">Dále musíte přidat do nové sítě VNet nasazením cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="33eef-127">Next, you need to add your cloud service deployment to the new VNet.</span></span> <span data-ttu-id="33eef-128">K tomuto účelu upravte přidáním v příslušných částech k vaší cscfg pomocí sady Visual Studio nebo editoru podle své volby cscfg vaše cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="33eef-128">To do this, modify your cloud service cscfg by adding the relevant sections to your cscfg using Visual Studio or the editor of your choice.</span></span>

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

<span data-ttu-id="33eef-129">V dalším sestavení projektu cloudové služby a nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="33eef-129">Next build your cloud services project and deploy it to Azure.</span></span> <span data-ttu-id="33eef-130">Chcete-li získat nápovědu k nasazení vašeho balíčku cloudové služby Azure, přečtěte si téma [postup vytvoření a nasazení cloudové služby](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)</span><span class="sxs-lookup"><span data-stu-id="33eef-130">To get help with deploying your cloud services package to Azure, see [How to Create and Deploy a Cloud Service](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)</span></span>

## <a name="connect-your-webworker-roles-to-the-domain"></a><span data-ttu-id="33eef-131">Váš web/role pracovního procesu se připojte k doméně</span><span class="sxs-lookup"><span data-stu-id="33eef-131">Connect your web/worker roles to the domain</span></span>
<span data-ttu-id="33eef-132">Jakmile se projekt cloudové služby je nasazen na Azure, připojte k vlastní doméně AD pomocí rozšíření domény AD instance role.</span><span class="sxs-lookup"><span data-stu-id="33eef-132">Once your cloud service project is deployed on Azure, connect your role instances to the custom AD domain using the AD Domain Extension.</span></span> <span data-ttu-id="33eef-133">Pokud chcete přidat rozšíření AD domény do existující nasazení cloudové služby a připojit se k vlastní doméně, spusťte následující příkazy v prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="33eef-133">To add the AD Domain Extension to your existing cloud services deployment and join the custom domain, execute the following commands in PowerShell:</span></span>

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

<span data-ttu-id="33eef-134">A je to.</span><span class="sxs-lookup"><span data-stu-id="33eef-134">And that's it.</span></span>

<span data-ttu-id="33eef-135">Cloudové služby by měl být připojený k řadiči domény vlastní.</span><span class="sxs-lookup"><span data-stu-id="33eef-135">Your cloud services should be joined to your custom domain controller.</span></span> <span data-ttu-id="33eef-136">Pokud vás zajímají další informace o různých možnostech dostupných pro postup konfigurace rozšíření domény AD, použijte nápovědu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33eef-136">If you would like to learn more about the different options available for how to configure AD Domain Extension, use the PowerShell help.</span></span> <span data-ttu-id="33eef-137">Pár příkladů postupujte podle kroků:</span><span class="sxs-lookup"><span data-stu-id="33eef-137">A couple of examples follow:</span></span>

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
