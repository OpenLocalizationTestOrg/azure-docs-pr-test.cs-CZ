---
title: "Přidání uživatele do kolekce Azure Remoteappu | Microsoft Docs"
description: "Postup přidání uživatelů do kolekcí vzdálené aplikace Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 6b751fd2-2b11-499f-a2eb-12cb47f3129b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 281e74c7941c42d8a3e4351953391229e54ce0a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a><span data-ttu-id="358e1-103">Postup přidání uživatele do kolekcí vzdálené aplikace Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="358e1-103">How to add a user to your Azure RemoteApp collection</span></span>
> [!IMPORTANT]
> <span data-ttu-id="358e1-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="358e1-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="358e1-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="358e1-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="358e1-106">Než uživatelé můžete zobrazit a používat aplikace v Azure Remoteappu, musíte jim udělit přístup k vaší kolekce.</span><span class="sxs-lookup"><span data-stu-id="358e1-106">Before your users can see and use your apps in Azure RemoteApp, you have to grant them access to your collection.</span></span> <span data-ttu-id="358e1-107">Toto je snadno část: na **přístup uživatelů** , zadejte informace o účtu pro uživatele a pak klikněte na symbol zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="358e1-107">This is the easy part: On the **User Access** tab, enter the account information for the user, and then click the check mark.</span></span>

<span data-ttu-id="358e1-108">Jaké informace o účtu potřebujete?</span><span class="sxs-lookup"><span data-stu-id="358e1-108">What account information do you need?</span></span> <span data-ttu-id="358e1-109">To závisí na typu kolekce jste vytvořili (Cloudová nebo hybridní) a zda používáte Office 365 ProPlus v dané kolekci.</span><span class="sxs-lookup"><span data-stu-id="358e1-109">That depends on the type of collection you created (cloud or hybrid) and whether you are using Office 365 ProPlus in that collection.</span></span>

## <a name="supported-user-identities"></a><span data-ttu-id="358e1-110">Podporované uživatelských identit</span><span class="sxs-lookup"><span data-stu-id="358e1-110">Supported user identities</span></span>
<span data-ttu-id="358e1-111">Typy jinou kolekci (cloud a hybridní) podporují, pomocí identity jiného uživatele pro přístup k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="358e1-111">The different collection types (cloud vs. hybrid) support using different user identities for access to applications.</span></span>  

<span data-ttu-id="358e1-112">Pro hybridní kolekci vzdálené aplikace RemoteApp musíte nastavit infrastrukturu domény služby Active Directory místní a tenanta služby Azure Active Directory s integrace adresáře (a volitelně jednotné přihlášení).</span><span class="sxs-lookup"><span data-stu-id="358e1-112">For a hybrid collection of RemoteApp, you need to set up an Active Directory domain infrastructure on premises and an Azure Active Directory tenant with Directory Integration (and optionally single sign-on).</span></span> <span data-ttu-id="358e1-113">Kromě toho budete muset vytvořit některé objekty služby Active Directory v místním adresáři.</span><span class="sxs-lookup"><span data-stu-id="358e1-113">Additionally, you need to create some Active Directory objects in the on-premises directory.</span></span>  

<span data-ttu-id="358e1-114">Pro cloudové kolekce vzdálené aplikace RemoteApp každý uživatel, který má Azure Active Directory podporu identity lze udělit přístup uživatele vzdálené aplikace RemoteApp zahrnout Accounts Microsoft.</span><span class="sxs-lookup"><span data-stu-id="358e1-114">For a cloud collection of RemoteApp, any user that has Azure Active Directory support identities can be granted user access to RemoteApp to include Microsoft Accounts.</span></span>  <span data-ttu-id="358e1-115">Viz následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="358e1-115">See the table below.</span></span>

<span data-ttu-id="358e1-116">Office 365 se uživatelů Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="358e1-116">Office 365 users are Azure Active Directory users.</span></span> <span data-ttu-id="358e1-117">Pokud budou mít hybridní Azure Active Directory, adresář synchronizoval účty, se můžete udělit přístup uživatelů v hybridním nasazení vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="358e1-117">If they have Azure Active Directory hybrid, Directory synchronized accounts, they can be granted user access in a RemoteApp hybrid deployment.</span></span>   

<span data-ttu-id="358e1-118">Tato tabulka slouží jako Stručná referenční příručka, pro který je podporován identity v kolekci a jaké jsou požadavky služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="358e1-118">You can use this table as a quick reference for which identity is supported in your collection and what the Active Directory requirements are.</span></span>

| <span data-ttu-id="358e1-119">Uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="358e1-119">User accounts</span></span> | <span data-ttu-id="358e1-120">Cloud</span><span class="sxs-lookup"><span data-stu-id="358e1-120">Cloud</span></span> | <span data-ttu-id="358e1-121">Hybridní</span><span class="sxs-lookup"><span data-stu-id="358e1-121">Hybrid</span></span> |
| --- | --- | --- |
| <span data-ttu-id="358e1-122">Účet Microsoft</span><span class="sxs-lookup"><span data-stu-id="358e1-122">Microsoft Account</span></span> |<span data-ttu-id="358e1-123">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-123">Yes</span></span> |<span data-ttu-id="358e1-124">Ne</span><span class="sxs-lookup"><span data-stu-id="358e1-124">No</span></span> |
| <span data-ttu-id="358e1-125">Azure Active Directory (Azure AD)</span><span class="sxs-lookup"><span data-stu-id="358e1-125">Azure Active Directory (Azure AD)</span></span> | | |
| <span data-ttu-id="358e1-126">Jenom Azure AD cloud</span><span class="sxs-lookup"><span data-stu-id="358e1-126">Azure AD cloud only</span></span> |<span data-ttu-id="358e1-127">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-127">Yes</span></span> |<span data-ttu-id="358e1-128">Ne</span><span class="sxs-lookup"><span data-stu-id="358e1-128">No</span></span> |
| <span data-ttu-id="358e1-129">ADsync se synchronizací hesla</span><span class="sxs-lookup"><span data-stu-id="358e1-129">ADsync with password sync</span></span> |<span data-ttu-id="358e1-130">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-130">Yes</span></span> |<span data-ttu-id="358e1-131">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-131">Yes</span></span> |
| <span data-ttu-id="358e1-132">ADsync bez synchronizace hesla</span><span class="sxs-lookup"><span data-stu-id="358e1-132">ADsync without password sync</span></span> |<span data-ttu-id="358e1-133">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-133">Yes</span></span> |<span data-ttu-id="358e1-134">Ne</span><span class="sxs-lookup"><span data-stu-id="358e1-134">No</span></span> |
| <span data-ttu-id="358e1-135">ADsync se službou AD FS</span><span class="sxs-lookup"><span data-stu-id="358e1-135">ADsync with AD FS</span></span> |<span data-ttu-id="358e1-136">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-136">Yes</span></span> |<span data-ttu-id="358e1-137">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-137">Yes</span></span> |
| <span data-ttu-id="358e1-138">[3. stran Azure podporován zprostředkovatelů identity](https://msdn.microsoft.com/library/azure/jj679342.aspx) (třeba příkaz Ping)</span><span class="sxs-lookup"><span data-stu-id="358e1-138">[3rd-party Azure supported identity providers](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (example Ping)</span></span> |<span data-ttu-id="358e1-139">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-139">Yes</span></span> |<span data-ttu-id="358e1-140">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-140">Yes</span></span> |
| <span data-ttu-id="358e1-141">Multi-factor Authentication</span><span class="sxs-lookup"><span data-stu-id="358e1-141">Multi-Factor Authentication</span></span> |<span data-ttu-id="358e1-142">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-142">Yes</span></span> |<span data-ttu-id="358e1-143">Ano</span><span class="sxs-lookup"><span data-stu-id="358e1-143">Yes</span></span> |

<span data-ttu-id="358e1-144">Podívejte se na [informace](remoteapp-ad.md) týkající se konfigurace služby Active Directory pro vzdálenou aplikaci RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="358e1-144">Check out [more information](remoteapp-ad.md) about configuring Active Directory for RemoteApp.</span></span>

> [!NOTE]
> <span data-ttu-id="358e1-145">Uživatelé Azure Active Directory musí pocházet z klienta, který je spojen s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="358e1-145">The Azure Active Directory users must be from the tenant that's associated with your subscription.</span></span> <span data-ttu-id="358e1-146">(Předplatné můžete zobrazit a upravit na kartě **Nastavení** v portálu.</span><span class="sxs-lookup"><span data-stu-id="358e1-146">(You can view and modify your subscription on the **Settings** tab in the portal.</span></span> <span data-ttu-id="358e1-147">Další informace najdete v části [Změna klienta Azure Active Directory používaného RemoteAppem](remoteapp-changetenant.md).)</span><span class="sxs-lookup"><span data-stu-id="358e1-147">See [Change the Azure Active Directory tenant used by RemoteApp](remoteapp-changetenant.md) for more information.)</span></span>
> 
> 

## <a name="office-365-proplus-user-account-information"></a><span data-ttu-id="358e1-148">Informace o účtu Office 365 ProPlus uživatele</span><span class="sxs-lookup"><span data-stu-id="358e1-148">Office 365 ProPlus user account information</span></span>
<span data-ttu-id="358e1-149">Pokud používáte image šablony Office 365 ProPlus v kolekci *nebo* Pokud jste vytvořili vlastní obrázek, který používá Office 365, které jsou povoleny pouze přidat uživatele Azure Active Directory, které mají předplatná Office 365 pro výchozí doménu vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="358e1-149">If you are using the Office 365 ProPlus template image in your collection *or* if you created a custom image that uses Office 365, you are only allowed to add Azure Active Directory users that have Office 365 subscriptions for the default domain of your subscription.</span></span> <span data-ttu-id="358e1-150">V tématu [pomocí Office 365 s Azure Remoteappem](remoteapp-o365.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="358e1-150">See [Using Office 365 with Azure RemoteApp](remoteapp-o365.md) for more information.</span></span>

