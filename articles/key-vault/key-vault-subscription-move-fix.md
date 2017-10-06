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
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Změna ID tenanta trezoru klíčů po přesunu předplatného
### <a name="q-my-subscription-was-moved-from-tenant-a-tootenant-b-how-do-i-change-hello-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>Otázka: Moje předplatné byl přesunut z klienta A tootenant B. Jak změnit hello ID klienta pro moje existující trezor klíčů a nastavte správné seznamy ACL pro objekty v klientovi B?
Při vytváření nového trezoru klíčů v odběru, je ID klienta Azure Active Directory automaticky vázanou toohello výchozí pro toto předplatné. Všechny položky zásad přístupu jsou také vázanou toothis ID klienta. Při přesunutí vašeho předplatného Azure z klienta tootenant B, vaše existující klíč, které jsou nedostupné podle trezory hello objekty (uživatelé a aplikace) v klientovi B. toofix tento problém, budete muset:

* Změňte ID klienta hello přidružené všechny existující trezorů klíčů ve toto předplatné tootenant B.
* Odeberte všechny stávající položky zásad přístupu.
* Přidejte nové položky zásad přístupu, které jsou přidružené k tenantu B.

Například pokud máte 'myvault' pro trezor klíčů v odběru, který byl přesunut z klienta A tootenant B, zde je jak toochange hello ID klienta pro tento trezor klíčů a odeberte starý zásady přístupu.

<pre>
$Select-AzureRmSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Protože se tento trezor klienta A před hello přesunutí, hello původní hodnotu **$vault. Properties.TenantId** je klienta A chvíli **(Get-AzureRmContext). Tenant.TenantId** je klienta B.

Teď, když je přidružen ID klienta správné hello trezoru a jsou odebrány staré položky zásady přístupu, nastavení přístupu nové zásady položky s [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## <a name="next-steps"></a>Další kroky
Pokud máte dotazy týkající se Azure Key Vault, navštivte hello [Azure Key Vault fóra](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

