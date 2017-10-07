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
# <a name="connecting-azure-cloud-services-roles-tooa-custom-ad-domain-controller-hosted-in-azure"></a>Připojení tooa vlastní role služeb v cloudu Azure, které řadič domény AD hostované v Azure
Nejprve nastavíme virtuální síť (VNet) v Azure. Potom přidáme řadič domény Active Directory (hostované na virtuální počítač Azure) toohello virtuální sítě. V dalším kroku jsme se přidejte existující cloudové služby role toohello předem vytvořené virtuální síť a pak připojte je toohello řadič domény.

Než začneme, několik z věcí tookeep nezapomeňte:

1. Tento kurz používá prostředí PowerShell, takže si máte Azure PowerShell, které jsou nainstalované a připravené toogo. v tématu nápovědy tooget s nastavení prostředí Azure PowerShell [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
2. Vaše řadiče domény AD a Role Web nebo Worker instance potřebovat toobe v hello virtuální sítě.

Postupujte podle podrobných pokynů a pokud narazíte na potíže, nechte nás komentář na konci hello tohoto článku hello. Někdo se vrátit tooyou (Ano, jsme přečíst komentáře).

musí být Hello sítě, který je odkazován hello cloudové služby **klasickou virtuální síť**.

## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě
Můžete vytvořit virtuální síť v Azure pomocí hello portál Azure nebo PowerShell. V tomto kurzu budeme používat prostředí PowerShell. hello toocreate virtuální sítě pomocí portálu Azure najdete [vytvořit virtuální síť](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

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

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače
Po dokončení nastavení hello virtuální sítě, budete potřebovat toocreate řadič domény AD. V tomto kurzu budeme zadávat řadič domény služby AD na virtuální počítač Azure.

toodo, vytvoření virtuálního počítače pomocí prostředí PowerShell pomocí hello následující příkazy:

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

## <a name="promote-your-virtual-machine-tooa-domain-controller"></a>Zvýšení úrovně tooa vašeho virtuálního počítače řadiče domény
tooconfigure hello virtuální počítač jako řadič domény služby AD, bude nutné toolog v toohello virtuálních počítačů a nakonfigurujte ji.

toolog v toohello virtuálních počítačů, můžete získat soubor RDP hello pomocí prostředí PowerShell, hello použijte následující příkazy:

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Po přihlášení toohello virtuálního počítače nastavit virtuální počítač jako řadič domény služby AD tím následující hello podrobný Průvodce na [jak tooset zákazníka řadič domény AD](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-toohello-virtual-network"></a>Přidat vaše cloudové služby toohello virtuální sítě
Dále je nutné tooadd vaše cloudové služby nasazení toohello nové virtuální sítě. toodo, vaše cloudové služby cscfg upravit přidáním hello příslušné části tooyour cscfg pomocí sady Visual Studio nebo hello editoru podle své volby.

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

Další sestavení projektu cloudové služby a nasaďte ji tooAzure. tooget nápovědu k nasazení vaší tooAzure balíček cloud services najdete v části [jak tooCreate a nasazení cloudové služby](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)

## <a name="connect-your-webworker-roles-toohello-domain"></a>Připojit vaše doména toohello role web nebo worker
Jakmile se projekt cloudové služby je nasazen na Azure, připojte doménu toohello vlastní AD instancí role pomocí hello rozšíření domény AD. tooadd hello rozšíření domény AD tooyour existující cloudové služby nasazení a připojení k hello vlastní doménu, spusťte následující příkazy v prostředí PowerShell hello:

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

A je to.

Cloudové služby by měl být připojený k tooyour řadič vlastní domény. Pokud vás zajímají další informace o hello různé možnosti, které jsou k dispozici pro toolearn jak pomoci tooconfigure rozšíření domény AD, použijte hello prostředí PowerShell. Pár příkladů postupujte podle kroků:

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
