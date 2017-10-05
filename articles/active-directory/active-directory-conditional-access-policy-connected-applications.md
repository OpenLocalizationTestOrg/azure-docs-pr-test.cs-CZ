---
title: "Nakonfigurujte zásady podmíněného přístupu na základě zařízení služby Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat zásady podmíněného přístupu na základě zařízení služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: a26c40351c6b982fd90acb4bf06220ef3f79f399
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a><span data-ttu-id="fade8-103">Nakonfigurujte zásady podmíněného přístupu na základě zařízení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fade8-103">Configure Azure Active Directory device-based conditional access policies</span></span>

<span data-ttu-id="fade8-104">S [podmíněného přístupu Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), můžete upřesnit jak oprávněným uživatelům můžete přístup k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="fade8-104">With [Azure Active Directory (Azure AD) conditional access](active-directory-conditional-access-azure-portal.md), you can fine-tune how authorized users can access your resources.</span></span> <span data-ttu-id="fade8-105">Například můžete omezit přístup k určitým prostředkům důvěryhodná zařízení.</span><span class="sxs-lookup"><span data-stu-id="fade8-105">For example, you limit the access to certain resources to trusted devices.</span></span> <span data-ttu-id="fade8-106">Zásady podmíněného přístupu, která vyžaduje důvěryhodné zařízení je také označován jako zásady podmíněného přístupu podle zařízení.</span><span class="sxs-lookup"><span data-stu-id="fade8-106">A conditional access policy that requires a trusted device is also known as device-based conditional access policy.</span></span>

<span data-ttu-id="fade8-107">Toto téma poskytuje informace o tom, jak nakonfigurovat zásady podmíněného přístupu na základě zařízení pro aplikace Azure AD připojen.</span><span class="sxs-lookup"><span data-stu-id="fade8-107">This topic provides you with information on how to configure device-based conditional access policies for Azure AD-connected applications.</span></span> 


## <a name="before-you-begin"></a><span data-ttu-id="fade8-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="fade8-108">Before you begin</span></span>

<span data-ttu-id="fade8-109">Podmíněný přístup využívající zařízení ties **podmíněného přístupu Azure AD** a **správy zařízení služby Azure AD společně**.</span><span class="sxs-lookup"><span data-stu-id="fade8-109">Device-based conditional access ties **Azure AD conditional access** and **Azure AD device management together**.</span></span> <span data-ttu-id="fade8-110">Pokud nejste obeznámeni s jedním z těchto oblastí ještě, měli byste si přečíst v následujících tématech, nejdřív:</span><span class="sxs-lookup"><span data-stu-id="fade8-110">If you are not familiar with one of these areas yet, you should read the following topics, first:</span></span>

- <span data-ttu-id="fade8-111">**[Podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  – Toto téma poskytuje koncepční přehled podmíněného přístupu a souvisejících technologiím.</span><span class="sxs-lookup"><span data-stu-id="fade8-111">**[Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)** - This topic provides you with a conceptual overview of conditional access and the related terminology.</span></span>

- <span data-ttu-id="fade8-112">**[Úvod do správy zařízení v Azure Active Directory](device-management-introduction.md)**  – Toto téma poskytuje přehled různých možností budete muset připojit zařízení s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fade8-112">**[Introduction to device management in Azure Active Directory](device-management-introduction.md)** - This topic gives you an overview of the various options you have to connect devices with Azure AD.</span></span> 


## <a name="trusted-devices"></a><span data-ttu-id="fade8-113">Důvěryhodná zařízení</span><span class="sxs-lookup"><span data-stu-id="fade8-113">Trusted devices</span></span>

<span data-ttu-id="fade8-114">První mobilní, cloudové první světě Azure Active Directory umožňuje jednotné přihlašování k zařízení, aplikacím a službám odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="fade8-114">In a mobile-first, cloud-first world, Azure Active Directory enables single sign-on to devices, apps, and services from anywhere.</span></span> <span data-ttu-id="fade8-115">Pro některé prostředky ve vašem prostředí, udělení přístupu ti správní uživatelé nemusí být dostatečně funkční.</span><span class="sxs-lookup"><span data-stu-id="fade8-115">For certain resources in your environment, granting access to the right users might not be good enough.</span></span> <span data-ttu-id="fade8-116">Kromě ti správní uživatelé může také vyžadovat důvěryhodné zařízení, který se má použít pro přístup k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="fade8-116">In addition to the right users, you might also require a trusted device to be used to access a resource.</span></span> <span data-ttu-id="fade8-117">Ve vašem prostředí, můžete definovat, co je důvěryhodné zařízení na základě následující součásti:</span><span class="sxs-lookup"><span data-stu-id="fade8-117">In your environment, you can define what a trusted device is based on the following components:</span></span>

- <span data-ttu-id="fade8-118">[Platformy zařízení](active-directory-conditional-access-azure-portal.md#device-platforms) na zařízení</span><span class="sxs-lookup"><span data-stu-id="fade8-118">The [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) on a device</span></span>
- <span data-ttu-id="fade8-119">Jestli je zařízení kompatibilní</span><span class="sxs-lookup"><span data-stu-id="fade8-119">Whether a device is compliant</span></span>
- <span data-ttu-id="fade8-120">Jestli je zařízení připojené k doméně</span><span class="sxs-lookup"><span data-stu-id="fade8-120">Whether a device is domain-joined</span></span> 

<span data-ttu-id="fade8-121">[Platformy zařízení](active-directory-conditional-access-azure-portal.md#device-platforms) je charakterizovaná operačního systému, který běží na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="fade8-121">The [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) is characterized by the operating system that is running on your device.</span></span> <span data-ttu-id="fade8-122">V zásadách podmíněného přístupu na základě zařízení můžete omezit přístup k určitým prostředkům specifické platformy zařízení.</span><span class="sxs-lookup"><span data-stu-id="fade8-122">In your device-based conditional access policy, you can limit access to certain resources to specific device platforms.</span></span>



<span data-ttu-id="fade8-123">V zásadách podmíněného přístupu podle zařízení můžete vyžadovat důvěryhodných zařízení označen jako kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="fade8-123">In a device-based conditional access policy, you can require trusted devices to be marked as compliant.</span></span>

![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/24.png)

<span data-ttu-id="fade8-125">Zařízení může být označen jako kompatibilní v adresáři podle:</span><span class="sxs-lookup"><span data-stu-id="fade8-125">Devices can be marked as compliant in the directory by:</span></span>

- <span data-ttu-id="fade8-126">Intune</span><span class="sxs-lookup"><span data-stu-id="fade8-126">Intune</span></span> 
- <span data-ttu-id="fade8-127">Systém správy mobilních zařízení třetích stran, který se integruje se službou Azure AD</span><span class="sxs-lookup"><span data-stu-id="fade8-127">A third-party mobile device management system that integrates with Azure AD</span></span>  

<span data-ttu-id="fade8-128">Jenom zařízení, které jsou připojené ke službě Azure AD může být označen jako kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="fade8-128">Only devices that are connected to Azure AD can be marked as compliant.</span></span> <span data-ttu-id="fade8-129">Pro připojení zařízení k Azure Active Directory, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="fade8-129">To connect a device to Azure Active Directory, you have the following options:</span></span> 

- <span data-ttu-id="fade8-130">Zaregistrovat Azure AD</span><span class="sxs-lookup"><span data-stu-id="fade8-130">Azure AD registered</span></span>
- <span data-ttu-id="fade8-131">Připojené k Azure AD</span><span class="sxs-lookup"><span data-stu-id="fade8-131">Azure AD joined</span></span>
- <span data-ttu-id="fade8-132">Hybridní připojený k Azure AD</span><span class="sxs-lookup"><span data-stu-id="fade8-132">Hybrid Azure AD joined</span></span>

    ![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/26.png)

<span data-ttu-id="fade8-134">Pokud budete mít místní službě Active Directory (AD) nároky, můžete zvážit zařízení, které nejsou připojené ke službě Azure AD ale připojený do služby AD jako důvěryhodný.</span><span class="sxs-lookup"><span data-stu-id="fade8-134">If you have an on-premises Active Directory (AD) footprint, you might consider devices that are not connected to Azure AD but joined to your AD to be trusted.</span></span>

![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a><span data-ttu-id="fade8-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fade8-136">Next steps</span></span>

<span data-ttu-id="fade8-137">Před konfigurací zásad podmíněného přístupu na základě zařízení ve vašem prostředí, měli byste podniknout podívejte se na [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="fade8-137">Before configuring a device-based conditional access policy in your environment, you should take a look at the [best practices for conditional access in Azure Active Directory](active-directory-conditional-access-best-practices.md).</span></span>

