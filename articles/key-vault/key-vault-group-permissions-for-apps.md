---
title: "Udělení oprávnění ke mnoho aplikací pro přístup Azure trezoru klíčů | Microsoft Docs"
description: "Zjistěte, jak chcete udělit oprávnění k mnoha aplikace pro přístup k trezoru klíčů"
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
ms.openlocfilehash: f58b633de2e4b5702ff2df9b3722662b09510200
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a><span data-ttu-id="393b0-103">Udělit oprávnění k mnoha aplikace pro přístup k trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="393b0-103">Grant permission to many applications to access a key vault</span></span>

## <a name="q-i-have-several-over-16-applications-that-need-to-access-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a><span data-ttu-id="393b0-104">Otázka: je nutné několik (více než 16) aplikace, které potřebují přístup k trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="393b0-104">Q: I have several (over 16) applications that need to access a key vault.</span></span> <span data-ttu-id="393b0-105">Vzhledem k tomu, že Key Vault umožňuje pouze 16 položky řízení přístupu, jak lze toho dosáhnout?</span><span class="sxs-lookup"><span data-stu-id="393b0-105">Since Key Vault only allows 16 access control entries, how can I achieve that?</span></span>

<span data-ttu-id="393b0-106">Zásada řízení přístupu Key Vault podporuje pouze 16 položky.</span><span class="sxs-lookup"><span data-stu-id="393b0-106">Key Vault access control policy only supports 16 entries.</span></span> <span data-ttu-id="393b0-107">Ale můžete vytvořit skupiny zabezpečení služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="393b0-107">However you can create an Azure Active Directory security group.</span></span> <span data-ttu-id="393b0-108">Přidejte všechny objekty přidružené služby do této skupiny zabezpečení a pak udělují přístup k této skupině zabezpečení Key Vault.</span><span class="sxs-lookup"><span data-stu-id="393b0-108">Add all the associated service principals to this security group and then grant access to this security group to Key Vault.</span></span>

<span data-ttu-id="393b0-109">Tady jsou požadavky:</span><span class="sxs-lookup"><span data-stu-id="393b0-109">Here are the pre-requisites:</span></span>
* <span data-ttu-id="393b0-110">[Instalace modulu Azure Active Directory V2 PowerShell](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span><span class="sxs-lookup"><span data-stu-id="393b0-110">[Install Azure Active Directory V2 PowerShell module](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span></span>
* <span data-ttu-id="393b0-111">[Nainstalujte prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="393b0-111">[Install Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="393b0-112">Pokud chcete spustit následující příkazy, potřebujete oprávnění k vytvoření nebo úpravě skupin v klientovi služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="393b0-112">To run the following commands, you need permissions to create/edit groups in the Azure Active Directory tenant.</span></span> <span data-ttu-id="393b0-113">Pokud nemáte oprávnění, budete muset obraťte se na správce služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="393b0-113">If you don't have permissions, you may need to contact your Azure Active Directory administrator.</span></span>

<span data-ttu-id="393b0-114">Nyní spusťte následující příkazy v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="393b0-114">Now run the following commands in PowerShell.</span></span>

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionToKeys all –PermissionToSecrets all –PermissionToCertificates all 
 
# Of course you can adjust the permissions as required 
```

<span data-ttu-id="393b0-115">Pokud potřebujete udělit jinou sadu oprávnění pro skupinu aplikací, vytvořte samostatnou skupinu zabezpečení služby Azure Active Directory pro tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="393b0-115">If you need to grant a different set of permissions to a group of applications, create a separate Azure Active Directory security group for such applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="393b0-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="393b0-116">Next steps</span></span>

<span data-ttu-id="393b0-117">Další informace o tom, jak [zabezpečit váš trezor klíčů](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="393b0-117">Learn more about how to [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>
