---
title: "Funkce služby synchronizace Azure AD Connect a konfigurace | Microsoft Docs"
description: "Popisuje funkce straně služby pro službu synchronizace Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: c2873510c280a2683c235cfdce3d2617c3b665cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-service-features"></a><span data-ttu-id="2d207-103">Funkce služby Azure AD Connect sync</span><span class="sxs-lookup"><span data-stu-id="2d207-103">Azure AD Connect sync service features</span></span>
<span data-ttu-id="2d207-104">Funkce synchronizace služby Azure AD Connect má dvě součásti:</span><span class="sxs-lookup"><span data-stu-id="2d207-104">The synchronization feature of Azure AD Connect has two components:</span></span>

* <span data-ttu-id="2d207-105">Místní součást s názvem **synchronizace Azure AD Connect**, označovaný také **synchronizační modul**.</span><span class="sxs-lookup"><span data-stu-id="2d207-105">The on-premises component named **Azure AD Connect sync**, also called **sync engine**.</span></span>
* <span data-ttu-id="2d207-106">Službu umístěný ve službě Azure AD, také známé jako **synchronizační služba Azure AD Connect**</span><span class="sxs-lookup"><span data-stu-id="2d207-106">The service residing in Azure AD also known as **Azure AD Connect sync service**</span></span>

<span data-ttu-id="2d207-107">Toto téma vysvětluje, jak tyto funkce **služba Azure AD Connect sync** práci a jak můžete nakonfigurovat pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2d207-107">This topic explains how the following features of the **Azure AD Connect sync service** work and how you can configure them using Windows PowerShell.</span></span>

<span data-ttu-id="2d207-108">Tato nastavení jsou konfigurována pomocí [Azure Active Directory modul pro prostředí Windows PowerShell](http://aka.ms/aadposh).</span><span class="sxs-lookup"><span data-stu-id="2d207-108">These settings are configured by the [Azure Active Directory Module for Windows PowerShell](http://aka.ms/aadposh).</span></span> <span data-ttu-id="2d207-109">Stáhněte a nainstalujte samostatně z Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="2d207-109">Download and install it separately from Azure AD Connect.</span></span> <span data-ttu-id="2d207-110">Rutiny popsané v tomto tématu se zavedly v [verze března 2016 (sestavení 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span><span class="sxs-lookup"><span data-stu-id="2d207-110">The cmdlets documented in this topic were introduced in the [2016 March release (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span></span> <span data-ttu-id="2d207-111">Pokud nemáte rutiny popsané v tomto tématu nebo nevedou stejný výsledek, zkontrolujte, že spustíte na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="2d207-111">If you do not have the cmdlets documented in this topic or they do not produce the same result, then make sure you run the latest version.</span></span>

<span data-ttu-id="2d207-112">Chcete-li zobrazit konfigurace v adresáři služby Azure AD, spusťte `Get-MsolDirSyncFeatures`.</span><span class="sxs-lookup"><span data-stu-id="2d207-112">To see the configuration in your Azure AD directory, run `Get-MsolDirSyncFeatures`.</span></span>  
<span data-ttu-id="2d207-113">![Get-MsolDirSyncFeatures výsledek](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span><span class="sxs-lookup"><span data-stu-id="2d207-113">![Get-MsolDirSyncFeatures result](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span></span>

<span data-ttu-id="2d207-114">Mnoho z těchto nastavení můžete změnit jenom přes Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="2d207-114">Many of these settings can only be changed by Azure AD Connect.</span></span>

<span data-ttu-id="2d207-115">Následující nastavení se dá nakonfigurovat pomocí `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="2d207-115">The following settings can be configured by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="2d207-116">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="2d207-116">DirSyncFeature</span></span> | <span data-ttu-id="2d207-117">Komentář</span><span class="sxs-lookup"><span data-stu-id="2d207-117">Comment</span></span> |
| --- | --- |
| [<span data-ttu-id="2d207-118">EnableSoftMatchOnUpn</span><span class="sxs-lookup"><span data-stu-id="2d207-118">EnableSoftMatchOnUpn</span></span>](#userprincipalname-soft-match) |<span data-ttu-id="2d207-119">Umožňuje objekty, které chcete připojit na userPrincipalName kromě primární adresa SMTP.</span><span class="sxs-lookup"><span data-stu-id="2d207-119">Allows objects to join on userPrincipalName in addition to primary SMTP address.</span></span> |
| [<span data-ttu-id="2d207-120">SynchronizeUpnForManagedUsers</span><span class="sxs-lookup"><span data-stu-id="2d207-120">SynchronizeUpnForManagedUsers</span></span>](#synchronize-userprincipalname-updates) |<span data-ttu-id="2d207-121">Umožňuje synchronizační modul aktualizovat atribut userPrincipalName pro spravované nebo licencovaných uživatelů (nefederovaných).</span><span class="sxs-lookup"><span data-stu-id="2d207-121">Allows the sync engine to update the userPrincipalName attribute for managed/licensed (non-federated) users.</span></span> |

<span data-ttu-id="2d207-122">Poté, co povolíte funkci nelze zakázat znovu.</span><span class="sxs-lookup"><span data-stu-id="2d207-122">After you have enabled a feature, it cannot be disabled again.</span></span>

> [!NOTE]
> <span data-ttu-id="2d207-123">Z 24 srpna 2016 funkci *duplicitní atribut odolnosti* je ve výchozím nastavení povolena pro nové Azure AD adresáře.</span><span class="sxs-lookup"><span data-stu-id="2d207-123">From August 24, 2016 the feature *Duplicate attribute resiliency* is enabled by default for new Azure AD directories.</span></span> <span data-ttu-id="2d207-124">Tato funkce bude také nasazuje a zapnuta adresáře vytvořené před tímto datem.</span><span class="sxs-lookup"><span data-stu-id="2d207-124">This feature will also be rolled out and enabled on directories created before this date.</span></span> <span data-ttu-id="2d207-125">Obdržíte e-mail s oznámením při adresáře je získat povolení této funkce.</span><span class="sxs-lookup"><span data-stu-id="2d207-125">You will receive an email notification when your directory is about to get this feature enabled.</span></span>
> 
> 

<span data-ttu-id="2d207-126">Následující nastavení jsou nakonfigurované přes Azure AD Connect a nelze změnit pomocí `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="2d207-126">The following settings are configured by Azure AD Connect and cannot be modified by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="2d207-127">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="2d207-127">DirSyncFeature</span></span> | <span data-ttu-id="2d207-128">Komentář</span><span class="sxs-lookup"><span data-stu-id="2d207-128">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="2d207-129">DeviceWriteback</span><span class="sxs-lookup"><span data-stu-id="2d207-129">DeviceWriteback</span></span> |[<span data-ttu-id="2d207-130">Azure AD Connect: Povolení zpětného zápisu zařízení</span><span class="sxs-lookup"><span data-stu-id="2d207-130">Azure AD Connect: Enabling device writeback</span></span>](active-directory-aadconnect-feature-device-writeback.md) |
| <span data-ttu-id="2d207-131">DirectoryExtensions</span><span class="sxs-lookup"><span data-stu-id="2d207-131">DirectoryExtensions</span></span> |[<span data-ttu-id="2d207-132">Synchronizace Azure AD Connect: rozšíření adresáře</span><span class="sxs-lookup"><span data-stu-id="2d207-132">Azure AD Connect sync: Directory extensions</span></span>](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [<span data-ttu-id="2d207-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span><span class="sxs-lookup"><span data-stu-id="2d207-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span></span>](#duplicate-attribute-resiliency) |<span data-ttu-id="2d207-134">Umožňuje atributu umístí do karantény, pokud je duplicitní jiného objektu, nikoli celý objekt selhání během exportu.</span><span class="sxs-lookup"><span data-stu-id="2d207-134">Allows an attribute to be quarantined when it is a duplicate of another object rather than failing the entire object during export.</span></span> |
| <span data-ttu-id="2d207-135">PasswordSync</span><span class="sxs-lookup"><span data-stu-id="2d207-135">PasswordSync</span></span> |[<span data-ttu-id="2d207-136">Implementace synchronizace hesel s synchronizace Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="2d207-136">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md) |
| <span data-ttu-id="2d207-137">UnifiedGroupWriteback</span><span class="sxs-lookup"><span data-stu-id="2d207-137">UnifiedGroupWriteback</span></span> |[<span data-ttu-id="2d207-138">Ve verzi Preview: Zpětný zápis skupin</span><span class="sxs-lookup"><span data-stu-id="2d207-138">Preview: Group writeback</span></span>](active-directory-aadconnect-feature-preview.md#group-writeback) |
| <span data-ttu-id="2d207-139">UserWriteback</span><span class="sxs-lookup"><span data-stu-id="2d207-139">UserWriteback</span></span> |<span data-ttu-id="2d207-140">Není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="2d207-140">Not currently supported.</span></span> |

## <a name="duplicate-attribute-resiliency"></a><span data-ttu-id="2d207-141">Odolnost proti chybám duplicitní atribut</span><span class="sxs-lookup"><span data-stu-id="2d207-141">Duplicate attribute resiliency</span></span>
<span data-ttu-id="2d207-142">Místo selhání zřídit objekty s duplicitní názvy UPN nebo proxyAddresses, duplicitní atribut "v karanténě" a je mu přiřazená dočasnou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="2d207-142">Instead of failing to provision objects with duplicate UPNs / proxyAddresses, the duplicated attribute is “quarantined” and a temporary value is assigned.</span></span> <span data-ttu-id="2d207-143">Po vyřešení konfliktu dočasné UPN se automaticky změní na správnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2d207-143">When the conflict is resolved, the temporary UPN is changed to the proper value automatically.</span></span> <span data-ttu-id="2d207-144">Další podrobnosti najdete v tématu [odolnosti Identity synchronizace a duplicitní atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="2d207-144">For more details, see [Identity synchronization and duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="userprincipalname-soft-match"></a><span data-ttu-id="2d207-145">UserPrincipalName logicky shody</span><span class="sxs-lookup"><span data-stu-id="2d207-145">UserPrincipalName soft match</span></span>
<span data-ttu-id="2d207-146">Pokud je tato funkce povolena, konfigurace soft-match je povolený pro UPN kromě [primární adresa SMTP](https://support.microsoft.com/kb/2641663), který je vždycky povolená.</span><span class="sxs-lookup"><span data-stu-id="2d207-146">When this feature is enabled, soft-match is enabled for UPN in addition to the [primary SMTP address](https://support.microsoft.com/kb/2641663), which is always enabled.</span></span> <span data-ttu-id="2d207-147">Konfigurace soft-match slouží k vyhledání existující cloudu uživatelů ve službě Azure AD s místních uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2d207-147">Soft-match is used to match existing cloud users in Azure AD with on-premises users.</span></span>

<span data-ttu-id="2d207-148">Pokud potřebujete shodu místní účty AD s existující účty vytvořené v cloudu a nepoužíváte Exchange Online, bude tato funkce je užitečná.</span><span class="sxs-lookup"><span data-stu-id="2d207-148">If you need to match on-premises AD accounts with existing accounts created in the cloud and you are not using Exchange Online, then this feature is useful.</span></span> <span data-ttu-id="2d207-149">V tomto scénáři obecně nemáte důvod k nastavení SMTP atributu v cloudu.</span><span class="sxs-lookup"><span data-stu-id="2d207-149">In this scenario, you generally don’t have a reason to set the SMTP attribute in the cloud.</span></span>

<span data-ttu-id="2d207-150">Tato funkce je na ve výchozím nastavení pro nově vytvořený adresáře Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d207-150">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="2d207-151">Se zobrazí, pokud je tato funkce povolená pro vás spuštěním:</span><span class="sxs-lookup"><span data-stu-id="2d207-151">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

<span data-ttu-id="2d207-152">Pokud je tato funkce není povoleno pro váš adresář Azure AD, můžete ji povolit spuštěním:</span><span class="sxs-lookup"><span data-stu-id="2d207-152">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a><span data-ttu-id="2d207-153">Synchronizovat aktualizace userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="2d207-153">Synchronize userPrincipalName updates</span></span>
<span data-ttu-id="2d207-154">V minulosti aktualizace pomocí služby synchronizace z místního atribut UserPrincipalName je zablokovaný, pokud jsou splněny obě tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="2d207-154">Historically, updates to the UserPrincipalName attribute using the sync service from on-premises has been blocked, unless both of these conditions are true:</span></span>

* <span data-ttu-id="2d207-155">Uživatel je spravovaný (nefederovaných).</span><span class="sxs-lookup"><span data-stu-id="2d207-155">The user is managed (non-federated).</span></span>
* <span data-ttu-id="2d207-156">Uživateli nebyla přiřazena licence.</span><span class="sxs-lookup"><span data-stu-id="2d207-156">The user has not been assigned a license.</span></span>

<span data-ttu-id="2d207-157">Další podrobnosti najdete v tématu [uživatelská jména v Office 365, Azure nebo Intune se neshodují místní UPN nebo alternativním přihlašovacím ID](https://support.microsoft.com/kb/2523192).</span><span class="sxs-lookup"><span data-stu-id="2d207-157">For more details, see [User names in Office 365, Azure, or Intune don't match the on-premises UPN or alternate login ID](https://support.microsoft.com/kb/2523192).</span></span>

<span data-ttu-id="2d207-158">Povolení této funkce umožňuje synchronizační modul aktualizovat userPrincipalName, pokud je změněné místní a používat synchronizaci hesla.</span><span class="sxs-lookup"><span data-stu-id="2d207-158">Enabling this feature allows the sync engine to update the userPrincipalName when it is changed on-premises and you use password sync.</span></span> <span data-ttu-id="2d207-159">Pokud používáte federace, tato funkce není podporována.</span><span class="sxs-lookup"><span data-stu-id="2d207-159">If you use federation, this feature is not supported.</span></span>

<span data-ttu-id="2d207-160">Tato funkce je na ve výchozím nastavení pro nově vytvořený adresáře Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d207-160">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="2d207-161">Se zobrazí, pokud je tato funkce povolená pro vás spuštěním:</span><span class="sxs-lookup"><span data-stu-id="2d207-161">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

<span data-ttu-id="2d207-162">Pokud je tato funkce není povoleno pro váš adresář Azure AD, můžete ji povolit spuštěním:</span><span class="sxs-lookup"><span data-stu-id="2d207-162">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

<span data-ttu-id="2d207-163">Po povolení této funkce, zůstanou existující hodnoty userPrincipalName jako-je.</span><span class="sxs-lookup"><span data-stu-id="2d207-163">After enabling this feature, existing userPrincipalName values will remain as-is.</span></span> <span data-ttu-id="2d207-164">Při další změně userPrincipalName atribut místní normální rozdílová synchronizace na uživatele se aktualizuje název UPN.</span><span class="sxs-lookup"><span data-stu-id="2d207-164">On next change of the userPrincipalName attribute on-premises, the normal delta sync on users will update the UPN.</span></span>  

## <a name="see-also"></a><span data-ttu-id="2d207-165">Viz také</span><span class="sxs-lookup"><span data-stu-id="2d207-165">See also</span></span>
* [<span data-ttu-id="2d207-166">Synchronizace služby Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="2d207-166">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="2d207-167">[Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="2d207-167">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

