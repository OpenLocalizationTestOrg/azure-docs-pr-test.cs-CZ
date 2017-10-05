---
title: "Předání přihlašovacích údajů do Azure pomocí DSC | Microsoft Docs"
description: "Přehled o bezpečně předávání přihlašovací údaje pro virtuální počítače Azure pomocí konfigurace požadovaného stavu prostředí PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: acd768c0219ec23c0453a65c575faf5213d9c616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a><span data-ttu-id="0d69b-103">Předání přihlašovacích údajů k obslužné rutině rozšíření Azure DSC</span><span class="sxs-lookup"><span data-stu-id="0d69b-103">Passing credentials to the Azure DSC extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="0d69b-104">Tento článek se týká konfigurace požadovaného stavu rozšíření pro Azure.</span><span class="sxs-lookup"><span data-stu-id="0d69b-104">This article covers the Desired State Configuration extension for Azure.</span></span> <span data-ttu-id="0d69b-105">Přehled obslužná rutina rozšíření DSC naleznete na adrese [Úvod do rozšíření obslužné rutiny konfigurace požadovaného stavu Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0d69b-105">An overview of the DSC extension handler can be found at [Introduction to the Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="passing-in-credentials"></a><span data-ttu-id="0d69b-106">Předávání přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="0d69b-106">Passing in credentials</span></span>
<span data-ttu-id="0d69b-107">Jako součást procesu konfigurace můžete potřebovat nastavit uživatelské účty, získat přístup ke službám, nebo instalaci programu v kontextu uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d69b-107">As a part of the configuration process, you may need to set up user accounts, access services, or install a program in a user context.</span></span> <span data-ttu-id="0d69b-108">Provádět tyto kroky, budete muset zadat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0d69b-108">To do these things, you need to provide credentials.</span></span> 

<span data-ttu-id="0d69b-109">DSC umožňuje parametrizované konfigurace, ve kterých jsou přihlašovací údaje předané do konfigurace a bezpečně uloženo na soubory MOF.</span><span class="sxs-lookup"><span data-stu-id="0d69b-109">DSC allows for parameterized configurations in which credentials are passed into the configuration and securely stored in MOF files.</span></span> <span data-ttu-id="0d69b-110">Obslužná rutina rozšíření Azure zjednodušuje správu přihlašovacích údajů prostřednictvím automatické správy certifikátů.</span><span class="sxs-lookup"><span data-stu-id="0d69b-110">The Azure Extension Handler simplifies credential management by providing automatic management of certificates.</span></span> 

<span data-ttu-id="0d69b-111">Vezměte v úvahu následující skript konfigurace DSC, který vytvoří místní uživatelský účet pomocí zadaného hesla:</span><span class="sxs-lookup"><span data-stu-id="0d69b-111">Consider the following DSC configuration script that creates a local user account with the specified password:</span></span>

<span data-ttu-id="0d69b-112">*user_configuration.ps1*</span><span class="sxs-lookup"><span data-stu-id="0d69b-112">*user_configuration.ps1*</span></span>

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

<span data-ttu-id="0d69b-113">Je důležité zahrnout *uzlu localhost* jako součást konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0d69b-113">It is important to include *node localhost* as part of the configuration.</span></span> <span data-ttu-id="0d69b-114">Pokud tento příkaz chybí, následující kroky nefungují jako obslužná rutina rozšíření konkrétně hledá příkaz localhost uzlu.</span><span class="sxs-lookup"><span data-stu-id="0d69b-114">If this statement is missing, the following steps do not work as the extension handler specifically looks for the node localhost statement.</span></span> <span data-ttu-id="0d69b-115">Je také důležité zahrnout typecast *[PsCredential]*podle konkrétního typu aktivuje rozšíření pro šifrování přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="0d69b-115">It is also important to include the typecast *[PsCredential]*, as this specific type triggers the extension to encrypt the credential.</span></span> 

<span data-ttu-id="0d69b-116">Tento skript publikujte do úložiště objektů blob:</span><span class="sxs-lookup"><span data-stu-id="0d69b-116">Publish this script to blob storage:</span></span>

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

<span data-ttu-id="0d69b-117">Nastavit rozšíření Azure DSC a zadejte přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="0d69b-117">Set the Azure DSC extension and provide the credential:</span></span>

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a><span data-ttu-id="0d69b-118">Jak jsou zabezpečené přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="0d69b-118">How credentials are secured</span></span>
<span data-ttu-id="0d69b-119">Spuštění tohoto kódu vyzve k zadání pověření.</span><span class="sxs-lookup"><span data-stu-id="0d69b-119">Running this code prompts for a credential.</span></span> <span data-ttu-id="0d69b-120">Jakmile je poskytována, uloží se do paměti stručně.</span><span class="sxs-lookup"><span data-stu-id="0d69b-120">Once it is provided, it is stored in memory briefly.</span></span> <span data-ttu-id="0d69b-121">Když je publikována s `Set-AzureVmDscExtension` rutiny ho se přenesou pomocí protokolu HTTPS k virtuálnímu počítači, kde Azure ukládá zašifrovaná na disku, certifikátem místní počítač.</span><span class="sxs-lookup"><span data-stu-id="0d69b-121">When it is published with `Set-AzureVmDscExtension` cmdlet, it is transmitted over HTTPS to the VM, where Azure stores it encrypted on disk, using the local VM certificate.</span></span> <span data-ttu-id="0d69b-122">Pak je stručně dešifrovat v paměti a znovu zašifrována mají být předány DSC.</span><span class="sxs-lookup"><span data-stu-id="0d69b-122">Then it is briefly decrypted in memory and re-encrypted to pass it to DSC.</span></span>

<span data-ttu-id="0d69b-123">Toto chování se liší od [pomocí zabezpečené konfigurace bez obslužná rutina rozšíření](https://msdn.microsoft.com/powershell/dsc/securemof).</span><span class="sxs-lookup"><span data-stu-id="0d69b-123">This behavior is different than [using secure configurations without the extension handler](https://msdn.microsoft.com/powershell/dsc/securemof).</span></span> <span data-ttu-id="0d69b-124">Prostředí Azure poskytuje způsob, jak přenášet data konfigurace bezpečně prostřednictvím certifikátů.</span><span class="sxs-lookup"><span data-stu-id="0d69b-124">The Azure environment gives a way to transmit configuration data securely via certificates.</span></span> <span data-ttu-id="0d69b-125">Pokud používáte obslužná rutina rozšíření DSC, není nutné poskytnout $CertificatePath nebo $CertificateID nebo $Thumbprint položku v ConfigurationData.</span><span class="sxs-lookup"><span data-stu-id="0d69b-125">When using the DSC extension handler, there is no need to provide $CertificatePath or a $CertificateID / $Thumbprint entry in ConfigurationData.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d69b-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d69b-126">Next steps</span></span>
<span data-ttu-id="0d69b-127">Další informace o obslužná rutina rozšíření Azure DSC najdete v tématu [Úvod do rozšíření obslužné rutiny konfigurace požadovaného stavu Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0d69b-127">For more information on the Azure DSC extension handler, see [Introduction to the Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="0d69b-128">Zkontrolujte [šablony Azure Resource Manageru pro rozšíření DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0d69b-128">Examine the [Azure Resource Manager template for the DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="0d69b-129">Další informace o DSC Powershellu [přejděte do centra dokumentace k prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="0d69b-129">For more information about PowerShell DSC, [visit the PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="0d69b-130">Najít další funkce, které můžete spravovat pomocí prostředí PowerShell DSC [procházet galerii prostředí PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další prostředky DSC.</span><span class="sxs-lookup"><span data-stu-id="0d69b-130">To find additional functionality you can manage with PowerShell DSC, [browse the PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

