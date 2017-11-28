---
title: "Služba synchronizace aaaAzure AD Connect funkcí a konfigurace | Microsoft Docs"
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
ms.openlocfilehash: 7ad05c45bb6b5fd5deaa3466e2936b19d3d9eabb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-features"></a><span data-ttu-id="5f61a-103">Funkce služby Azure AD Connect sync</span><span class="sxs-lookup"><span data-stu-id="5f61a-103">Azure AD Connect sync service features</span></span>
<span data-ttu-id="5f61a-104">Funkce Hello synchronizace služby Azure AD Connect má dvě součásti:</span><span class="sxs-lookup"><span data-stu-id="5f61a-104">hello synchronization feature of Azure AD Connect has two components:</span></span>

* <span data-ttu-id="5f61a-105">Hello místní součást s názvem **synchronizace Azure AD Connect**, označovaný také **synchronizační modul**.</span><span class="sxs-lookup"><span data-stu-id="5f61a-105">hello on-premises component named **Azure AD Connect sync**, also called **sync engine**.</span></span>
* <span data-ttu-id="5f61a-106">Služba Hello umístěný ve službě Azure AD, také známé jako **synchronizační služba Azure AD Connect**</span><span class="sxs-lookup"><span data-stu-id="5f61a-106">hello service residing in Azure AD also known as **Azure AD Connect sync service**</span></span>

<span data-ttu-id="5f61a-107">Toto téma vysvětluje, jak hello následující funkce z hello **služba Azure AD Connect sync** práci a jak můžete nakonfigurovat pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5f61a-107">This topic explains how hello following features of hello **Azure AD Connect sync service** work and how you can configure them using Windows PowerShell.</span></span>

<span data-ttu-id="5f61a-108">Tato nastavení jsou konfigurována pomocí hello [Azure Active Directory modul pro prostředí Windows PowerShell](http://aka.ms/aadposh).</span><span class="sxs-lookup"><span data-stu-id="5f61a-108">These settings are configured by hello [Azure Active Directory Module for Windows PowerShell](http://aka.ms/aadposh).</span></span> <span data-ttu-id="5f61a-109">Stáhněte a nainstalujte samostatně z Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="5f61a-109">Download and install it separately from Azure AD Connect.</span></span> <span data-ttu-id="5f61a-110">Hello rutiny popsané v tomto tématu se zavedly v hello [verze března 2016 (sestavení 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span><span class="sxs-lookup"><span data-stu-id="5f61a-110">hello cmdlets documented in this topic were introduced in hello [2016 March release (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span></span> <span data-ttu-id="5f61a-111">Pokud nemáte hello rutiny popsané v tomto tématu nebo nevedou hello stejné způsobit a pak zajistěte, aby spuštěním hello nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="5f61a-111">If you do not have hello cmdlets documented in this topic or they do not produce hello same result, then make sure you run hello latest version.</span></span>

<span data-ttu-id="5f61a-112">Konfigurace hello toosee v adresáři služby Azure AD, spusťte `Get-MsolDirSyncFeatures`.</span><span class="sxs-lookup"><span data-stu-id="5f61a-112">toosee hello configuration in your Azure AD directory, run `Get-MsolDirSyncFeatures`.</span></span>  
<span data-ttu-id="5f61a-113">![Get-MsolDirSyncFeatures výsledek](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span><span class="sxs-lookup"><span data-stu-id="5f61a-113">![Get-MsolDirSyncFeatures result](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span></span>

<span data-ttu-id="5f61a-114">Mnoho z těchto nastavení můžete změnit jenom přes Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="5f61a-114">Many of these settings can only be changed by Azure AD Connect.</span></span>

<span data-ttu-id="5f61a-115">Hello následující nastavení se dá nakonfigurovat `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="5f61a-115">hello following settings can be configured by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="5f61a-116">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="5f61a-116">DirSyncFeature</span></span> | <span data-ttu-id="5f61a-117">Komentář</span><span class="sxs-lookup"><span data-stu-id="5f61a-117">Comment</span></span> |
| --- | --- |
| [<span data-ttu-id="5f61a-118">EnableSoftMatchOnUpn</span><span class="sxs-lookup"><span data-stu-id="5f61a-118">EnableSoftMatchOnUpn</span></span>](#userprincipalname-soft-match) |<span data-ttu-id="5f61a-119">Umožňuje toojoin objekty v userPrincipalName přidání tooprimary SMTP adresu.</span><span class="sxs-lookup"><span data-stu-id="5f61a-119">Allows objects toojoin on userPrincipalName in addition tooprimary SMTP address.</span></span> |
| [<span data-ttu-id="5f61a-120">SynchronizeUpnForManagedUsers</span><span class="sxs-lookup"><span data-stu-id="5f61a-120">SynchronizeUpnForManagedUsers</span></span>](#synchronize-userprincipalname-updates) |<span data-ttu-id="5f61a-121">Umožňuje hello synchronizační modul tooupdate hello atribut userPrincipalName spravované nebo licencovaných uživatelů (nefederovaných).</span><span class="sxs-lookup"><span data-stu-id="5f61a-121">Allows hello sync engine tooupdate hello userPrincipalName attribute for managed/licensed (non-federated) users.</span></span> |

<span data-ttu-id="5f61a-122">Poté, co povolíte funkci nelze zakázat znovu.</span><span class="sxs-lookup"><span data-stu-id="5f61a-122">After you have enabled a feature, it cannot be disabled again.</span></span>

> [!NOTE]
> <span data-ttu-id="5f61a-123">Z 24 srpna 2016 hello funkce *duplicitní atribut odolnosti* je ve výchozím nastavení povolena pro nové Azure AD adresáře.</span><span class="sxs-lookup"><span data-stu-id="5f61a-123">From August 24, 2016 hello feature *Duplicate attribute resiliency* is enabled by default for new Azure AD directories.</span></span> <span data-ttu-id="5f61a-124">Tato funkce bude také nasazuje a zapnuta adresáře vytvořené před tímto datem.</span><span class="sxs-lookup"><span data-stu-id="5f61a-124">This feature will also be rolled out and enabled on directories created before this date.</span></span> <span data-ttu-id="5f61a-125">Obdržíte e-mail s oznámením při adresáře je o tooget povolení této funkce.</span><span class="sxs-lookup"><span data-stu-id="5f61a-125">You will receive an email notification when your directory is about tooget this feature enabled.</span></span>
> 
> 

<span data-ttu-id="5f61a-126">Hello následující nastavení jsou nakonfigurovány přes Azure AD Connect a nelze změnit pomocí `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="5f61a-126">hello following settings are configured by Azure AD Connect and cannot be modified by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="5f61a-127">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="5f61a-127">DirSyncFeature</span></span> | <span data-ttu-id="5f61a-128">Komentář</span><span class="sxs-lookup"><span data-stu-id="5f61a-128">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="5f61a-129">DeviceWriteback</span><span class="sxs-lookup"><span data-stu-id="5f61a-129">DeviceWriteback</span></span> |[<span data-ttu-id="5f61a-130">Azure AD Connect: Povolení zpětného zápisu zařízení</span><span class="sxs-lookup"><span data-stu-id="5f61a-130">Azure AD Connect: Enabling device writeback</span></span>](active-directory-aadconnect-feature-device-writeback.md) |
| <span data-ttu-id="5f61a-131">DirectoryExtensions</span><span class="sxs-lookup"><span data-stu-id="5f61a-131">DirectoryExtensions</span></span> |[<span data-ttu-id="5f61a-132">Synchronizace Azure AD Connect: rozšíření adresáře</span><span class="sxs-lookup"><span data-stu-id="5f61a-132">Azure AD Connect sync: Directory extensions</span></span>](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [<span data-ttu-id="5f61a-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span><span class="sxs-lookup"><span data-stu-id="5f61a-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span></span>](#duplicate-attribute-resiliency) |<span data-ttu-id="5f61a-134">Umožňuje atributu toobe do karantény, pokud je duplicitní jiný objekt, nikoli celý objekt hello selhání při exportu.</span><span class="sxs-lookup"><span data-stu-id="5f61a-134">Allows an attribute toobe quarantined when it is a duplicate of another object rather than failing hello entire object during export.</span></span> |
| <span data-ttu-id="5f61a-135">PasswordSync</span><span class="sxs-lookup"><span data-stu-id="5f61a-135">PasswordSync</span></span> |[<span data-ttu-id="5f61a-136">Implementace synchronizace hesel s synchronizace Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="5f61a-136">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md) |
| <span data-ttu-id="5f61a-137">UnifiedGroupWriteback</span><span class="sxs-lookup"><span data-stu-id="5f61a-137">UnifiedGroupWriteback</span></span> |[<span data-ttu-id="5f61a-138">Ve verzi Preview: Zpětný zápis skupin</span><span class="sxs-lookup"><span data-stu-id="5f61a-138">Preview: Group writeback</span></span>](active-directory-aadconnect-feature-preview.md#group-writeback) |
| <span data-ttu-id="5f61a-139">UserWriteback</span><span class="sxs-lookup"><span data-stu-id="5f61a-139">UserWriteback</span></span> |<span data-ttu-id="5f61a-140">Není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="5f61a-140">Not currently supported.</span></span> |

## <a name="duplicate-attribute-resiliency"></a><span data-ttu-id="5f61a-141">Odolnost proti chybám duplicitní atribut</span><span class="sxs-lookup"><span data-stu-id="5f61a-141">Duplicate attribute resiliency</span></span>
<span data-ttu-id="5f61a-142">Místo selhání tooprovision objekty s duplicitní názvy UPN nebo proxyAddresses, duplicitní atribut hello "v karanténě" a je mu přiřazená dočasnou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="5f61a-142">Instead of failing tooprovision objects with duplicate UPNs / proxyAddresses, hello duplicated attribute is “quarantined” and a temporary value is assigned.</span></span> <span data-ttu-id="5f61a-143">Po vyřešení konfliktu hello hello dočasných UPN je změnit toohello správnou hodnotu automaticky.</span><span class="sxs-lookup"><span data-stu-id="5f61a-143">When hello conflict is resolved, hello temporary UPN is changed toohello proper value automatically.</span></span> <span data-ttu-id="5f61a-144">Další podrobnosti najdete v tématu [odolnosti Identity synchronizace a duplicitní atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="5f61a-144">For more details, see [Identity synchronization and duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="userprincipalname-soft-match"></a><span data-ttu-id="5f61a-145">UserPrincipalName logicky shody</span><span class="sxs-lookup"><span data-stu-id="5f61a-145">UserPrincipalName soft match</span></span>
<span data-ttu-id="5f61a-146">Pokud je tato funkce povolená, konfigurace soft-match je povolena pro hlavní název uživatele v přidání toohello [primární adresa SMTP](https://support.microsoft.com/kb/2641663), který je vždycky povolená.</span><span class="sxs-lookup"><span data-stu-id="5f61a-146">When this feature is enabled, soft-match is enabled for UPN in addition toohello [primary SMTP address](https://support.microsoft.com/kb/2641663), which is always enabled.</span></span> <span data-ttu-id="5f61a-147">Konfigurace soft-match je použité toomatch stávající cloudu uživatele ve službě Azure AD s místními uživateli.</span><span class="sxs-lookup"><span data-stu-id="5f61a-147">Soft-match is used toomatch existing cloud users in Azure AD with on-premises users.</span></span>

<span data-ttu-id="5f61a-148">Pokud potřebujete toomatch místní účty AD s existující účty vytvořené v cloudu hello a nepoužíváte Exchange Online, bude tato funkce je užitečná.</span><span class="sxs-lookup"><span data-stu-id="5f61a-148">If you need toomatch on-premises AD accounts with existing accounts created in hello cloud and you are not using Exchange Online, then this feature is useful.</span></span> <span data-ttu-id="5f61a-149">V tomto scénáři obecně nemáte atribut důvod tooset hello SMTP v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="5f61a-149">In this scenario, you generally don’t have a reason tooset hello SMTP attribute in hello cloud.</span></span>

<span data-ttu-id="5f61a-150">Tato funkce je na ve výchozím nastavení pro nově vytvořený adresáře Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f61a-150">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="5f61a-151">Se zobrazí, pokud je tato funkce povolená pro vás spuštěním:</span><span class="sxs-lookup"><span data-stu-id="5f61a-151">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

<span data-ttu-id="5f61a-152">Pokud je tato funkce není povoleno pro váš adresář Azure AD, můžete ji povolit spuštěním:</span><span class="sxs-lookup"><span data-stu-id="5f61a-152">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a><span data-ttu-id="5f61a-153">Synchronizovat aktualizace userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="5f61a-153">Synchronize userPrincipalName updates</span></span>
<span data-ttu-id="5f61a-154">V minulosti atribut UserPrincipalName toohello aktualizace pomocí služby synchronizace hello z místního je zablokovaný, pokud jsou splněny obě tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="5f61a-154">Historically, updates toohello UserPrincipalName attribute using hello sync service from on-premises has been blocked, unless both of these conditions are true:</span></span>

* <span data-ttu-id="5f61a-155">uživatel Hello je spravovaný (nefederovaných).</span><span class="sxs-lookup"><span data-stu-id="5f61a-155">hello user is managed (non-federated).</span></span>
* <span data-ttu-id="5f61a-156">Hello uživateli nebyla přiřazena licence.</span><span class="sxs-lookup"><span data-stu-id="5f61a-156">hello user has not been assigned a license.</span></span>

<span data-ttu-id="5f61a-157">Další podrobnosti najdete v tématu [uživatelského jména v Office 365, Azure nebo Intune se neshodují. hello místní UPN nebo alternativním přihlašovacím ID](https://support.microsoft.com/kb/2523192).</span><span class="sxs-lookup"><span data-stu-id="5f61a-157">For more details, see [User names in Office 365, Azure, or Intune don't match hello on-premises UPN or alternate login ID](https://support.microsoft.com/kb/2523192).</span></span>

<span data-ttu-id="5f61a-158">Povolení této funkce umožňuje hello synchronizační modul tooupdate hello userPrincipalName, pokud je změněné místní a používat synchronizaci hesla. Pokud používáte federace, tato funkce není podporována.</span><span class="sxs-lookup"><span data-stu-id="5f61a-158">Enabling this feature allows hello sync engine tooupdate hello userPrincipalName when it is changed on-premises and you use password sync. If you use federation, this feature is not supported.</span></span>

<span data-ttu-id="5f61a-159">Tato funkce je na ve výchozím nastavení pro nově vytvořený adresáře Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f61a-159">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="5f61a-160">Se zobrazí, pokud je tato funkce povolená pro vás spuštěním:</span><span class="sxs-lookup"><span data-stu-id="5f61a-160">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

<span data-ttu-id="5f61a-161">Pokud je tato funkce není povoleno pro váš adresář Azure AD, můžete ji povolit spuštěním:</span><span class="sxs-lookup"><span data-stu-id="5f61a-161">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

<span data-ttu-id="5f61a-162">Po povolení této funkce, zůstanou existující hodnoty userPrincipalName jako-je.</span><span class="sxs-lookup"><span data-stu-id="5f61a-162">After enabling this feature, existing userPrincipalName values will remain as-is.</span></span> <span data-ttu-id="5f61a-163">Při příští změně hello userPrincipalName atribut místního hello normální rozdílová synchronizace na uživatele se aktualizuje hello UPN.</span><span class="sxs-lookup"><span data-stu-id="5f61a-163">On next change of hello userPrincipalName attribute on-premises, hello normal delta sync on users will update hello UPN.</span></span>  

## <a name="see-also"></a><span data-ttu-id="5f61a-164">Viz také</span><span class="sxs-lookup"><span data-stu-id="5f61a-164">See also</span></span>
* [<span data-ttu-id="5f61a-165">Synchronizace služby Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="5f61a-165">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="5f61a-166">[Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="5f61a-166">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

