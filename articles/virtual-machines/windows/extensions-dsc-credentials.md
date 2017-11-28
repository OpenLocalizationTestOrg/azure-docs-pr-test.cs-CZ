---
title: "aaaPassing pověření tooAzure pomocí DSC | Microsoft Docs"
description: "Přehled o bezpečně předávání pověření tooAzure virtuálních počítačů pomocí konfigurace požadovaného stavu prostředí PowerShell"
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
ms.openlocfilehash: 306ecd3fd481f49a0beca5052fc7531a52999330
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="passing-credentials-toohello-azure-dsc-extension-handler"></a><span data-ttu-id="6a0df-103">Předání přihlašovacích údajů toohello Azure DSC rozšíření obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="6a0df-103">Passing credentials toohello Azure DSC extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="6a0df-104">Tento článek se zabývá hello konfigurace požadovaného stavu rozšíření pro Azure.</span><span class="sxs-lookup"><span data-stu-id="6a0df-104">This article covers hello Desired State Configuration extension for Azure.</span></span> <span data-ttu-id="6a0df-105">Přehled hello DSC rozšíření obslužné rutiny najdete na [Úvod toohello konfigurace požadovaného stavu Azure rozšíření obslužné rutiny](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a0df-105">An overview of hello DSC extension handler can be found at [Introduction toohello Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="passing-in-credentials"></a><span data-ttu-id="6a0df-106">Předávání přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="6a0df-106">Passing in credentials</span></span>
<span data-ttu-id="6a0df-107">Jako součást procesu konfigurace hello, budete pravděpodobně tooset uživatelských účtů, získat přístup ke službám nebo instalaci programu v kontextu uživatele.</span><span class="sxs-lookup"><span data-stu-id="6a0df-107">As a part of hello configuration process, you may need tooset up user accounts, access services, or install a program in a user context.</span></span> <span data-ttu-id="6a0df-108">toodo tyto věci, třeba tooprovide přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="6a0df-108">toodo these things, you need tooprovide credentials.</span></span> 

<span data-ttu-id="6a0df-109">DSC umožňuje parametrizované konfigurace, ve kterých jsou přihlašovací údaje předané do konfigurace hello a bezpečně uloženo na soubory MOF.</span><span class="sxs-lookup"><span data-stu-id="6a0df-109">DSC allows for parameterized configurations in which credentials are passed into hello configuration and securely stored in MOF files.</span></span> <span data-ttu-id="6a0df-110">Hello obslužná rutina rozšíření Azure zjednodušuje správu přihlašovacích údajů prostřednictvím automatické správy certifikátů.</span><span class="sxs-lookup"><span data-stu-id="6a0df-110">hello Azure Extension Handler simplifies credential management by providing automatic management of certificates.</span></span> 

<span data-ttu-id="6a0df-111">Vezměte v úvahu následující DSC konfigurační skript, který vytvoří místní uživatelský účet pomocí zadaného hesla hello hello:</span><span class="sxs-lookup"><span data-stu-id="6a0df-111">Consider hello following DSC configuration script that creates a local user account with hello specified password:</span></span>

<span data-ttu-id="6a0df-112">*user_configuration.ps1*</span><span class="sxs-lookup"><span data-stu-id="6a0df-112">*user_configuration.ps1*</span></span>

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

<span data-ttu-id="6a0df-113">Je důležité tooinclude *uzlu localhost* jako součást konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="6a0df-113">It is important tooinclude *node localhost* as part of hello configuration.</span></span> <span data-ttu-id="6a0df-114">Pokud tento příkaz chybí, hello následující kroky nefungují jako obslužná rutina rozšíření hello konkrétně hledá hello uzlu localhost příkaz.</span><span class="sxs-lookup"><span data-stu-id="6a0df-114">If this statement is missing, hello following steps do not work as hello extension handler specifically looks for hello node localhost statement.</span></span> <span data-ttu-id="6a0df-115">Je také důležité přiřazení typu tooinclude hello *[PsCredential]*podle konkrétního typu aktivuje hello rozšíření tooencrypt hello pověření.</span><span class="sxs-lookup"><span data-stu-id="6a0df-115">It is also important tooinclude hello typecast *[PsCredential]*, as this specific type triggers hello extension tooencrypt hello credential.</span></span> 

<span data-ttu-id="6a0df-116">Publikujte tento skript tooblob úložiště:</span><span class="sxs-lookup"><span data-stu-id="6a0df-116">Publish this script tooblob storage:</span></span>

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

<span data-ttu-id="6a0df-117">Nastavit rozšíření Azure DSC hello a zadejte přihlašovací údaje hello:</span><span class="sxs-lookup"><span data-stu-id="6a0df-117">Set hello Azure DSC extension and provide hello credential:</span></span>

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a><span data-ttu-id="6a0df-118">Jak jsou zabezpečené přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="6a0df-118">How credentials are secured</span></span>
<span data-ttu-id="6a0df-119">Spuštění tohoto kódu vyzve k zadání pověření.</span><span class="sxs-lookup"><span data-stu-id="6a0df-119">Running this code prompts for a credential.</span></span> <span data-ttu-id="6a0df-120">Jakmile je poskytována, uloží se do paměti stručně.</span><span class="sxs-lookup"><span data-stu-id="6a0df-120">Once it is provided, it is stored in memory briefly.</span></span> <span data-ttu-id="6a0df-121">Když je publikována s `Set-AzureVmDscExtension` rutiny ho přenášená přes HTTPS toohello virtuálního počítače, kde Azure ukládá zašifrovaná na disku, certifikátem hello místní počítač.</span><span class="sxs-lookup"><span data-stu-id="6a0df-121">When it is published with `Set-AzureVmDscExtension` cmdlet, it is transmitted over HTTPS toohello VM, where Azure stores it encrypted on disk, using hello local VM certificate.</span></span> <span data-ttu-id="6a0df-122">Pak je stručně dešifrovaný v paměti a znovu zašifrovat toopass ho tooDSC.</span><span class="sxs-lookup"><span data-stu-id="6a0df-122">Then it is briefly decrypted in memory and re-encrypted toopass it tooDSC.</span></span>

<span data-ttu-id="6a0df-123">Toto chování se liší od [pomocí zabezpečené konfigurace bez popisovače rozšíření hello](https://msdn.microsoft.com/powershell/dsc/securemof).</span><span class="sxs-lookup"><span data-stu-id="6a0df-123">This behavior is different than [using secure configurations without hello extension handler](https://msdn.microsoft.com/powershell/dsc/securemof).</span></span> <span data-ttu-id="6a0df-124">Hello prostředí Azure poskytuje konfigurační data tootransmit způsob, jak bezpečně prostřednictvím certifikátů.</span><span class="sxs-lookup"><span data-stu-id="6a0df-124">hello Azure environment gives a way tootransmit configuration data securely via certificates.</span></span> <span data-ttu-id="6a0df-125">Pokud používáte rozšíření hello DSC obslužné rutiny, je bez nutnosti tooprovide $CertificatePath nebo $CertificateID nebo $Thumbprint položku v ConfigurationData.</span><span class="sxs-lookup"><span data-stu-id="6a0df-125">When using hello DSC extension handler, there is no need tooprovide $CertificatePath or a $CertificateID / $Thumbprint entry in ConfigurationData.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a0df-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a0df-126">Next steps</span></span>
<span data-ttu-id="6a0df-127">Další informace o hello Azure DSC rozšíření obslužné rutiny, najdete v části [Úvod toohello konfigurace požadovaného stavu Azure rozšíření obslužné rutiny](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a0df-127">For more information on hello Azure DSC extension handler, see [Introduction toohello Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="6a0df-128">Zkontrolujte hello [šablony Azure Resource Manageru pro rozšíření hello DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a0df-128">Examine hello [Azure Resource Manager template for hello DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="6a0df-129">Další informace o DSC Powershellu [navštivte centru dokumentace prostředí PowerShell hello](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="6a0df-129">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="6a0df-130">toofind další funkce, které můžete spravovat pomocí prostředí PowerShell DSC [procházet galerii prostředí PowerShell hello](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další prostředky DSC.</span><span class="sxs-lookup"><span data-stu-id="6a0df-130">toofind additional functionality you can manage with PowerShell DSC, [browse hello PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

