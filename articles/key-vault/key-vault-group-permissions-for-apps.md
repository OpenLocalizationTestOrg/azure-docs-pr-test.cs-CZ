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
# <a name="grant-permission-toomany-applications-tooaccess-a-key-vault"></a><span data-ttu-id="3b3a5-103">Udělení oprávnění toomany aplikace tooaccess trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="3b3a5-103">Grant permission toomany applications tooaccess a key vault</span></span>

## <a name="q-i-have-several-over-16-applications-that-need-tooaccess-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a><span data-ttu-id="3b3a5-104">Otázka: je nutné několik (více než 16) aplikace, které potřebují tooaccess trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3b3a5-104">Q: I have several (over 16) applications that need tooaccess a key vault.</span></span> <span data-ttu-id="3b3a5-105">Vzhledem k tomu, že Key Vault umožňuje pouze 16 položky řízení přístupu, jak lze toho dosáhnout?</span><span class="sxs-lookup"><span data-stu-id="3b3a5-105">Since Key Vault only allows 16 access control entries, how can I achieve that?</span></span>

<span data-ttu-id="3b3a5-106">Zásada řízení přístupu Key Vault podporuje pouze 16 položky.</span><span class="sxs-lookup"><span data-stu-id="3b3a5-106">Key Vault access control policy only supports 16 entries.</span></span> <span data-ttu-id="3b3a5-107">Ale můžete vytvořit skupiny zabezpečení služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3b3a5-107">However you can create an Azure Active Directory security group.</span></span> <span data-ttu-id="3b3a5-108">Přidejte všechny hello přidružené skupiny zabezpečení toothis objekty služby, pak udělují přístup toothis zabezpečení skupiny tooKey trezoru.</span><span class="sxs-lookup"><span data-stu-id="3b3a5-108">Add all hello associated service principals toothis security group and then grant access toothis security group tooKey Vault.</span></span>

<span data-ttu-id="3b3a5-109">Tady jsou požadavky hello:</span><span class="sxs-lookup"><span data-stu-id="3b3a5-109">Here are hello pre-requisites:</span></span>
* <span data-ttu-id="3b3a5-110">[Instalace modulu Azure Active Directory V2 PowerShell](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span><span class="sxs-lookup"><span data-stu-id="3b3a5-110">[Install Azure Active Directory V2 PowerShell module](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span></span>
* <span data-ttu-id="3b3a5-111">[Nainstalujte prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3b3a5-111">[Install Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="3b3a5-112">toorun hello následující příkazy, potřebujete oprávnění toocreate či upravit skupiny v hello klienta Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3b3a5-112">toorun hello following commands, you need permissions toocreate/edit groups in hello Azure Active Directory tenant.</span></span> <span data-ttu-id="3b3a5-113">Pokud nemáte oprávnění, musíte toocontact správce Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3b3a5-113">If you don't have permissions, you may need toocontact your Azure Active Directory administrator.</span></span>

<span data-ttu-id="3b3a5-114">Nyní spusťte následující příkazy v prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="3b3a5-114">Now run hello following commands in PowerShell.</span></span>

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

<span data-ttu-id="3b3a5-115">Pokud potřebujete toogrant jinou sadu oprávnění tooa skupinu aplikací, vytvořte samostatnou skupinu zabezpečení služby Azure Active Directory pro tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b3a5-115">If you need toogrant a different set of permissions tooa group of applications, create a separate Azure Active Directory security group for such applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b3a5-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b3a5-116">Next steps</span></span>

<span data-ttu-id="3b3a5-117">Další informace o příliš[zabezpečit váš trezor klíčů](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="3b3a5-117">Learn more about how too[Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>
