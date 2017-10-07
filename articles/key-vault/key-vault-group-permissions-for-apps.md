---
title: "aaaGrant oprávnění toomany aplikace tooaccess klíče trezoru služby Azure | Microsoft Docs"
description: "Zjistěte, jak toogrant oprávnění toomany aplikace tooaccess klíč trezoru"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: 5258149f939856f91b3848fc50399e58e5894f0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="grant-permission-toomany-applications-tooaccess-a-key-vault"></a>Udělení oprávnění toomany aplikace tooaccess trezoru klíčů

## <a name="q-i-have-several-over-16-applications-that-need-tooaccess-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a>Otázka: je nutné několik (více než 16) aplikace, které potřebují tooaccess trezoru klíčů. Vzhledem k tomu, že Key Vault umožňuje pouze 16 položky řízení přístupu, jak lze toho dosáhnout?

Zásada řízení přístupu Key Vault podporuje pouze 16 položky. Ale můžete vytvořit skupiny zabezpečení služby Azure Active Directory. Přidejte všechny hello přidružené skupiny zabezpečení toothis objekty služby, pak udělují přístup toothis zabezpečení skupiny tooKey trezoru.

Tady jsou požadavky hello:
* [Instalace modulu Azure Active Directory V2 PowerShell](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).
* [Nainstalujte prostředí Azure PowerShell](/powershell/azure/overview).
* toorun hello následující příkazy, potřebujete oprávnění toocreate či upravit skupiny v hello klienta Azure Active Directory. Pokud nemáte oprávnění, musíte toocontact správce Azure Active Directory.

Nyní spusťte následující příkazy v prostředí PowerShell hello.

```powershell
# Connect tooAzure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members toothis group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members toothis group, in this fashion. 
 
# Set hello Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionToKeys all –PermissionToSecrets all –PermissionToCertificates all 
 
# Of course you can adjust hello permissions as required 
```

Pokud potřebujete toogrant jinou sadu oprávnění tooa skupinu aplikací, vytvořte samostatnou skupinu zabezpečení služby Azure Active Directory pro tyto aplikace.

## <a name="next-steps"></a>Další kroky

Další informace o příliš[zabezpečit váš trezor klíčů](key-vault-secure-your-key-vault.md).
