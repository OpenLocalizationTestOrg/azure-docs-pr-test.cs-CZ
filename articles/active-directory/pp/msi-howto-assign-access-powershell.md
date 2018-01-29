---
title: "Tom, jak přiřadit MSI přístup k prostředku Azure, pomocí prostředí PowerShell"
description: "Podrobné pokyny pro přiřazení MSI na jeden prostředek, přístup k jiný prostředek, pomocí prostředí PowerShell."
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 8b3b000b4dc708e50c7e96fe04646837c96a555a
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>Přiřadit identita spravované služby (MSI) přístup k prostředku pomocí prostředí PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Po konfiguraci prostředek služby Azure s MSI, můžete zajistit přístup MSI pro jiný prostředek, stejně jako libovolný zaregistrovaný objekt zabezpečení. Tento příklad ukazuje, jak poskytnout přístup MSI Azure virtuálního počítače k účtu úložiště Azure, pomocí prostředí PowerShell.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Také nainstalovat [prostředí Azure PowerShell verze 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1) Pokud jste tak ještě neučinili.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Používat funkci RBAC přiřazení přístupu MSI pro jiný prostředek

Po povolení MSI na prostředek Azure [například virtuální počítač Azure](msi-qs-configure-powershell-windows-vm.md):

1. Přihlaste se k Azure pomocí `Login-AzureRmAccount` rutiny. Používáte účet, který je přidružen k předplatnému Azure, pod kterým jste nakonfigurovali soubor MSI:

   ```powershell
   Login-AzureRmAccount
   ```
2. V tomto příkladu nabízíme virtuálního počítače Azure přístup k účtu úložiště. Nejprve používáme [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) získat objekt služby pro virtuální počítač s názvem "Můjvp", který byl vytvořen, když jsme povolené MSI. Potom používáme [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) Pokud chcete poskytnout virtuální počítač "Čtečky" přístup k účtu úložiště s názvem "myStorageAcct":

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="troubleshooting"></a>Řešení potíží

Pokud soubor MSI pro prostředek nezobrazuje v seznamu dostupných identit, ověřte, aby byla správně povolená soubor MSI. V našem případě jsme se vrátit k virtuálnímu počítači Azure v [portál Azure](https://portal.azure.com) a:

- Podívejte se na stránce "Konfigurace" a zkontrolujte MSI, povoleno = "Yes".
- Podívejte se na stránce "Rozšíření" a zkontrolujte příponou MSI úspěšně nasazena.

Pokud je buď nesprávný, musíte znovu nasaďte MSI v prostředku znovu, nebo vyřešit potíže s selhání nasazení.

## <a name="related-content"></a>Související obsah

- Přehled MSI najdete v tématu [identita spravované služby přehled](msi-overview.md).
- Pokud chcete povolit MSI ve virtuálním počítači Azure, najdete v části [konfigurace Azure virtuálního počítače spravované služby Identity (MSI) pomocí prostředí PowerShell](msi-qs-configure-powershell-windows-vm.md).

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.

