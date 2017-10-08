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
# <a name="passing-credentials-toohello-azure-dsc-extension-handler"></a>Předání přihlašovacích údajů toohello Azure DSC rozšíření obslužné rutiny
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Tento článek se zabývá hello konfigurace požadovaného stavu rozšíření pro Azure. Přehled hello DSC rozšíření obslužné rutiny najdete na [Úvod toohello konfigurace požadovaného stavu Azure rozšíření obslužné rutiny](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="passing-in-credentials"></a>Předávání přihlašovací údaje
Jako součást procesu konfigurace hello, budete pravděpodobně tooset uživatelských účtů, získat přístup ke službám nebo instalaci programu v kontextu uživatele. toodo tyto věci, třeba tooprovide přihlašovací údaje. 

DSC umožňuje parametrizované konfigurace, ve kterých jsou přihlašovací údaje předané do konfigurace hello a bezpečně uloženo na soubory MOF. Hello obslužná rutina rozšíření Azure zjednodušuje správu přihlašovacích údajů prostřednictvím automatické správy certifikátů. 

Vezměte v úvahu následující DSC konfigurační skript, který vytvoří místní uživatelský účet pomocí zadaného hesla hello hello:

*user_configuration.ps1*

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

Je důležité tooinclude *uzlu localhost* jako součást konfigurace hello. Pokud tento příkaz chybí, hello následující kroky nefungují jako obslužná rutina rozšíření hello konkrétně hledá hello uzlu localhost příkaz. Je také důležité přiřazení typu tooinclude hello *[PsCredential]*podle konkrétního typu aktivuje hello rozšíření tooencrypt hello pověření. 

Publikujte tento skript tooblob úložiště:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Nastavit rozšíření Azure DSC hello a zadejte přihlašovací údaje hello:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Jak jsou zabezpečené přihlašovací údaje
Spuštění tohoto kódu vyzve k zadání pověření. Jakmile je poskytována, uloží se do paměti stručně. Když je publikována s `Set-AzureVmDscExtension` rutiny ho přenášená přes HTTPS toohello virtuálního počítače, kde Azure ukládá zašifrovaná na disku, certifikátem hello místní počítač. Pak je stručně dešifrovaný v paměti a znovu zašifrovat toopass ho tooDSC.

Toto chování se liší od [pomocí zabezpečené konfigurace bez popisovače rozšíření hello](https://msdn.microsoft.com/powershell/dsc/securemof). Hello prostředí Azure poskytuje konfigurační data tootransmit způsob, jak bezpečně prostřednictvím certifikátů. Pokud používáte rozšíření hello DSC obslužné rutiny, je bez nutnosti tooprovide $CertificatePath nebo $CertificateID nebo $Thumbprint položku v ConfigurationData.

## <a name="next-steps"></a>Další kroky
Další informace o hello Azure DSC rozšíření obslužné rutiny, najdete v části [Úvod toohello konfigurace požadovaného stavu Azure rozšíření obslužné rutiny](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Zkontrolujte hello [šablony Azure Resource Manageru pro rozšíření hello DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Další informace o DSC Powershellu [navštivte centru dokumentace prostředí PowerShell hello](https://msdn.microsoft.com/powershell/dsc/overview). 

toofind další funkce, které můžete spravovat pomocí prostředí PowerShell DSC [procházet galerii prostředí PowerShell hello](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další prostředky DSC.

