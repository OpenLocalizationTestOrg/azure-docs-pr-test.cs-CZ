---
title: "aaaAdd uživatele tooyour kolekci Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak uživatelé tooyour tooadd kolekci Azure Remoteappu"
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
ms.openlocfilehash: 0ae88e04c8bfc2ed55dc963945ed7e9ff687b603
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-a-user-tooyour-azure-remoteapp-collection"></a><span data-ttu-id="bf400-103">Jak tooadd uživatele tooyour kolekci Azure Remoteappu</span><span class="sxs-lookup"><span data-stu-id="bf400-103">How tooadd a user tooyour Azure RemoteApp collection</span></span>
> [!IMPORTANT]
> <span data-ttu-id="bf400-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="bf400-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="bf400-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bf400-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="bf400-106">Než uživatelé můžete zobrazit a používat aplikace v Azure Remoteappu, musíte je přístup ke kolekci tooyour toogrant.</span><span class="sxs-lookup"><span data-stu-id="bf400-106">Before your users can see and use your apps in Azure RemoteApp, you have toogrant them access tooyour collection.</span></span> <span data-ttu-id="bf400-107">Toto je část snadno hello: na hello **přístup uživatelů** , zadejte informace o účtu hello hello uživatele a pak klikněte hello zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="bf400-107">This is hello easy part: On hello **User Access** tab, enter hello account information for hello user, and then click hello check mark.</span></span>

<span data-ttu-id="bf400-108">Jaké informace o účtu potřebujete?</span><span class="sxs-lookup"><span data-stu-id="bf400-108">What account information do you need?</span></span> <span data-ttu-id="bf400-109">To závisí na typu hello kolekce jste vytvořili (Cloudová nebo hybridní) a zda používáte Office 365 ProPlus v dané kolekci.</span><span class="sxs-lookup"><span data-stu-id="bf400-109">That depends on hello type of collection you created (cloud or hybrid) and whether you are using Office 365 ProPlus in that collection.</span></span>

## <a name="supported-user-identities"></a><span data-ttu-id="bf400-110">Podporované uživatelských identit</span><span class="sxs-lookup"><span data-stu-id="bf400-110">Supported user identities</span></span>
<span data-ttu-id="bf400-111">typy jinou kolekci Hello (cloud a hybridní) podporují, pomocí identity jiného uživatele pro přístup k tooapplications.</span><span class="sxs-lookup"><span data-stu-id="bf400-111">hello different collection types (cloud vs. hybrid) support using different user identities for access tooapplications.</span></span>  

<span data-ttu-id="bf400-112">Pro hybridní kolekci vzdálené aplikace RemoteApp budete potřebovat tooset až infrastrukturu domény služby Active Directory místní a tenanta služby Azure Active Directory s integrace adresáře (a volitelně jednotné přihlašování).</span><span class="sxs-lookup"><span data-stu-id="bf400-112">For a hybrid collection of RemoteApp, you need tooset up an Active Directory domain infrastructure on premises and an Azure Active Directory tenant with Directory Integration (and optionally single sign-on).</span></span> <span data-ttu-id="bf400-113">Kromě toho musíte toocreate některé objekty služby Active Directory v hello místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="bf400-113">Additionally, you need toocreate some Active Directory objects in hello on-premises directory.</span></span>  

<span data-ttu-id="bf400-114">Pro cloudové kolekce vzdálené aplikace RemoteApp každý uživatel, který má Azure Active Directory podporu identity lze udělit práva uživatele přístupu tooRemoteApp tooinclude Accounts Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bf400-114">For a cloud collection of RemoteApp, any user that has Azure Active Directory support identities can be granted user access tooRemoteApp tooinclude Microsoft Accounts.</span></span>  <span data-ttu-id="bf400-115">Viz následující tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="bf400-115">See hello table below.</span></span>

<span data-ttu-id="bf400-116">Office 365 se uživatelů Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bf400-116">Office 365 users are Azure Active Directory users.</span></span> <span data-ttu-id="bf400-117">Pokud budou mít hybridní Azure Active Directory, adresář synchronizoval účty, se můžete udělit přístup uživatelů v hybridním nasazení vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bf400-117">If they have Azure Active Directory hybrid, Directory synchronized accounts, they can be granted user access in a RemoteApp hybrid deployment.</span></span>   

<span data-ttu-id="bf400-118">Tato tabulka slouží jako Stručná referenční příručka, pro který je podporován identity v kolekci a jaké jsou požadavky služby Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="bf400-118">You can use this table as a quick reference for which identity is supported in your collection and what hello Active Directory requirements are.</span></span>

| <span data-ttu-id="bf400-119">Uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="bf400-119">User accounts</span></span> | <span data-ttu-id="bf400-120">Cloud</span><span class="sxs-lookup"><span data-stu-id="bf400-120">Cloud</span></span> | <span data-ttu-id="bf400-121">Hybridní</span><span class="sxs-lookup"><span data-stu-id="bf400-121">Hybrid</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bf400-122">Účet Microsoft</span><span class="sxs-lookup"><span data-stu-id="bf400-122">Microsoft Account</span></span> |<span data-ttu-id="bf400-123">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-123">Yes</span></span> |<span data-ttu-id="bf400-124">Ne</span><span class="sxs-lookup"><span data-stu-id="bf400-124">No</span></span> |
| <span data-ttu-id="bf400-125">Azure Active Directory (Azure AD)</span><span class="sxs-lookup"><span data-stu-id="bf400-125">Azure Active Directory (Azure AD)</span></span> | | |
| <span data-ttu-id="bf400-126">Jenom Azure AD cloud</span><span class="sxs-lookup"><span data-stu-id="bf400-126">Azure AD cloud only</span></span> |<span data-ttu-id="bf400-127">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-127">Yes</span></span> |<span data-ttu-id="bf400-128">Ne</span><span class="sxs-lookup"><span data-stu-id="bf400-128">No</span></span> |
| <span data-ttu-id="bf400-129">ADsync se synchronizací hesla</span><span class="sxs-lookup"><span data-stu-id="bf400-129">ADsync with password sync</span></span> |<span data-ttu-id="bf400-130">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-130">Yes</span></span> |<span data-ttu-id="bf400-131">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-131">Yes</span></span> |
| <span data-ttu-id="bf400-132">ADsync bez synchronizace hesla</span><span class="sxs-lookup"><span data-stu-id="bf400-132">ADsync without password sync</span></span> |<span data-ttu-id="bf400-133">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-133">Yes</span></span> |<span data-ttu-id="bf400-134">Ne</span><span class="sxs-lookup"><span data-stu-id="bf400-134">No</span></span> |
| <span data-ttu-id="bf400-135">ADsync se službou AD FS</span><span class="sxs-lookup"><span data-stu-id="bf400-135">ADsync with AD FS</span></span> |<span data-ttu-id="bf400-136">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-136">Yes</span></span> |<span data-ttu-id="bf400-137">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-137">Yes</span></span> |
| <span data-ttu-id="bf400-138">[3. stran Azure podporován zprostředkovatelů identity](https://msdn.microsoft.com/library/azure/jj679342.aspx) (třeba příkaz Ping)</span><span class="sxs-lookup"><span data-stu-id="bf400-138">[3rd-party Azure supported identity providers](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (example Ping)</span></span> |<span data-ttu-id="bf400-139">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-139">Yes</span></span> |<span data-ttu-id="bf400-140">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-140">Yes</span></span> |
| <span data-ttu-id="bf400-141">Multi-factor Authentication</span><span class="sxs-lookup"><span data-stu-id="bf400-141">Multi-Factor Authentication</span></span> |<span data-ttu-id="bf400-142">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-142">Yes</span></span> |<span data-ttu-id="bf400-143">Ano</span><span class="sxs-lookup"><span data-stu-id="bf400-143">Yes</span></span> |

<span data-ttu-id="bf400-144">Podívejte se na [informace](remoteapp-ad.md) týkající se konfigurace služby Active Directory pro vzdálenou aplikaci RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bf400-144">Check out [more information](remoteapp-ad.md) about configuring Active Directory for RemoteApp.</span></span>

> [!NOTE]
> <span data-ttu-id="bf400-145">Uživatelé Azure Active Directory Hello musí pocházet z hello klienta, který je spojen s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="bf400-145">hello Azure Active Directory users must be from hello tenant that's associated with your subscription.</span></span> <span data-ttu-id="bf400-146">(Můžete zobrazit a změnit předplatné hello **nastavení** kartě hello portálu.</span><span class="sxs-lookup"><span data-stu-id="bf400-146">(You can view and modify your subscription on hello **Settings** tab in hello portal.</span></span> <span data-ttu-id="bf400-147">V tématu [klienta Azure Active Directory hello změnu používaného Remoteappem](remoteapp-changetenant.md) Další informace.)</span><span class="sxs-lookup"><span data-stu-id="bf400-147">See [Change hello Azure Active Directory tenant used by RemoteApp](remoteapp-changetenant.md) for more information.)</span></span>
> 
> 

## <a name="office-365-proplus-user-account-information"></a><span data-ttu-id="bf400-148">Informace o účtu Office 365 ProPlus uživatele</span><span class="sxs-lookup"><span data-stu-id="bf400-148">Office 365 ProPlus user account information</span></span>
<span data-ttu-id="bf400-149">Pokud používáte image šablony Office 365 ProPlus hello v kolekci *nebo* Pokud jste vytvořili vlastní obrázek, který používá Office 365, jsou povoleny pouze tooadd Azure Active Directory uživatelů, kteří mají předplatná Office 365 pro hello výchozí doménu vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="bf400-149">If you are using hello Office 365 ProPlus template image in your collection *or* if you created a custom image that uses Office 365, you are only allowed tooadd Azure Active Directory users that have Office 365 subscriptions for hello default domain of your subscription.</span></span> <span data-ttu-id="bf400-150">V tématu [pomocí Office 365 s Azure Remoteappem](remoteapp-o365.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bf400-150">See [Using Office 365 with Azure RemoteApp](remoteapp-o365.md) for more information.</span></span>

