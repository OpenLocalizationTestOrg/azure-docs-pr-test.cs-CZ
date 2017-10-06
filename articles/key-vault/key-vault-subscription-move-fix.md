---
title: "aaaChange hello trezoru klíčů ID klienta po přesunutí předplatné | Microsoft Docs"
description: "Zjistěte, jak přesunout tooswitch hello ID klienta pro trezor klíčů po předplatné tooa různých klienta"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 46d7bc21-fa79-49e4-8c84-032eef1d813e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: 4d0607208c61c57959439d2d0bd8feade4141fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a><span data-ttu-id="dccb1-103">Změna ID tenanta trezoru klíčů po přesunu předplatného</span><span class="sxs-lookup"><span data-stu-id="dccb1-103">Change a key vault tenant ID after a subscription move</span></span>
### <a name="q-my-subscription-was-moved-from-tenant-a-tootenant-b-how-do-i-change-hello-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a><span data-ttu-id="dccb1-104">Otázka: Moje předplatné byl přesunut z klienta A tootenant B. Jak změnit hello ID klienta pro moje existující trezor klíčů a nastavte správné seznamy ACL pro objekty v klientovi B?</span><span class="sxs-lookup"><span data-stu-id="dccb1-104">Q: My subscription was moved from tenant A tootenant B. How do I change hello tenant ID for my existing key vault and set correct ACLs for principals in tenant B?</span></span>
<span data-ttu-id="dccb1-105">Při vytváření nového trezoru klíčů v odběru, je ID klienta Azure Active Directory automaticky vázanou toohello výchozí pro toto předplatné.</span><span class="sxs-lookup"><span data-stu-id="dccb1-105">When you create a new key vault in a subscription, it is automatically tied toohello default Azure Active Directory tenant ID for that subscription.</span></span> <span data-ttu-id="dccb1-106">Všechny položky zásad přístupu jsou také vázanou toothis ID klienta.</span><span class="sxs-lookup"><span data-stu-id="dccb1-106">All access policy entries are also tied toothis tenant ID.</span></span> <span data-ttu-id="dccb1-107">Při přesunutí vašeho předplatného Azure z klienta tootenant B, vaše existující klíč, které jsou nedostupné podle trezory hello objekty (uživatelé a aplikace) v klientovi B. toofix tento problém, budete muset:</span><span class="sxs-lookup"><span data-stu-id="dccb1-107">When you move your Azure subscription from tenant A tootenant B, your existing key vaults are inaccessible by hello principals (users and applications) in tenant B. toofix this issue, you need to:</span></span>

* <span data-ttu-id="dccb1-108">Změňte ID klienta hello přidružené všechny existující trezorů klíčů ve toto předplatné tootenant B.</span><span class="sxs-lookup"><span data-stu-id="dccb1-108">Change hello tenant ID associated with all existing key vaults in this subscription tootenant B.</span></span>
* <span data-ttu-id="dccb1-109">Odeberte všechny stávající položky zásad přístupu.</span><span class="sxs-lookup"><span data-stu-id="dccb1-109">Remove all existing access policy entries.</span></span>
* <span data-ttu-id="dccb1-110">Přidejte nové položky zásad přístupu, které jsou přidružené k tenantu B.</span><span class="sxs-lookup"><span data-stu-id="dccb1-110">Add new access policy entries that are associated with tenant B.</span></span>

<span data-ttu-id="dccb1-111">Například pokud máte 'myvault' pro trezor klíčů v odběru, který byl přesunut z klienta A tootenant B, zde je jak toochange hello ID klienta pro tento trezor klíčů a odeberte starý zásady přístupu.</span><span class="sxs-lookup"><span data-stu-id="dccb1-111">For example, if you have key vault 'myvault' in a subscription that has been moved from tenant A tootenant B, here's how toochange hello tenant ID for this key vault and remove old access policies.</span></span>

<pre>
$Select-AzureRmSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

<span data-ttu-id="dccb1-112">Protože se tento trezor klienta A před hello přesunutí, hello původní hodnotu **$vault. Properties.TenantId** je klienta A chvíli **(Get-AzureRmContext). Tenant.TenantId** je klienta B.</span><span class="sxs-lookup"><span data-stu-id="dccb1-112">Because this vault was in tenant A before hello move, hello original value of **$vault.Properties.TenantId** is tenant A, while **(Get-AzureRmContext).Tenant.TenantId** is tenant B.</span></span>

<span data-ttu-id="dccb1-113">Teď, když je přidružen ID klienta správné hello trezoru a jsou odebrány staré položky zásady přístupu, nastavení přístupu nové zásady položky s [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).</span><span class="sxs-lookup"><span data-stu-id="dccb1-113">Now that your vault is associated with hello correct tenant ID and old access policy entries are removed, set new access policy entries with [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dccb1-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dccb1-114">Next steps</span></span>
<span data-ttu-id="dccb1-115">Pokud máte dotazy týkající se Azure Key Vault, navštivte hello [Azure Key Vault fóra](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).</span><span class="sxs-lookup"><span data-stu-id="dccb1-115">If you have questions about Azure Key Vault, visit hello [Azure Key Vault Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).</span></span>

