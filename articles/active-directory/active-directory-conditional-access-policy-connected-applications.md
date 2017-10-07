---
title: "zásady podmíněného přístupu na základě zařízení služby Azure Active Directory aaaConfigure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory na základě zařízení podmíněný přístup, zásady."
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
ms.openlocfilehash: cc5923b8ccee4335442c17ef63b2ee6f098e104e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a><span data-ttu-id="ebfc8-103">Nakonfigurujte zásady podmíněného přístupu na základě zařízení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ebfc8-103">Configure Azure Active Directory device-based conditional access policies</span></span>

<span data-ttu-id="ebfc8-104">S [podmíněného přístupu Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), můžete upřesnit jak oprávněným uživatelům můžete přístup k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-104">With [Azure Active Directory (Azure AD) conditional access](active-directory-conditional-access-azure-portal.md), you can fine-tune how authorized users can access your resources.</span></span> <span data-ttu-id="ebfc8-105">Například můžete omezit hello přístup toocertain prostředky tootrusted zařízení.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-105">For example, you limit hello access toocertain resources tootrusted devices.</span></span> <span data-ttu-id="ebfc8-106">Zásady podmíněného přístupu, která vyžaduje důvěryhodné zařízení je také označován jako zásady podmíněného přístupu podle zařízení.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-106">A conditional access policy that requires a trusted device is also known as device-based conditional access policy.</span></span>

<span data-ttu-id="ebfc8-107">Toto téma vám poskytne informace o tom, jak tooconfigure na zařízení podmíněný přístup, zásady pro aplikace Azure AD připojené.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-107">This topic provides you with information on how tooconfigure device-based conditional access policies for Azure AD-connected applications.</span></span> 


## <a name="before-you-begin"></a><span data-ttu-id="ebfc8-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ebfc8-108">Before you begin</span></span>

<span data-ttu-id="ebfc8-109">Podmíněný přístup využívající zařízení ties **podmíněného přístupu Azure AD** a **správy zařízení služby Azure AD společně**.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-109">Device-based conditional access ties **Azure AD conditional access** and **Azure AD device management together**.</span></span> <span data-ttu-id="ebfc8-110">Pokud nejste obeznámeni s jedním z těchto oblastí ještě, přečtěte si následující témata, nejprve hello:</span><span class="sxs-lookup"><span data-stu-id="ebfc8-110">If you are not familiar with one of these areas yet, you should read hello following topics, first:</span></span>

- <span data-ttu-id="ebfc8-111">**[Podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  – Toto téma obsahuje vám koncepční přehled podmíněného přístupu a hello související terminologie.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-111">**[Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)** - This topic provides you with a conceptual overview of conditional access and hello related terminology.</span></span>

- <span data-ttu-id="ebfc8-112">**[Úvod toodevice správy ve službě Azure Active Directory](device-management-introduction.md)**  – Toto téma poskytuje přehled o hello různé možnosti máte tooconnect zařízení s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-112">**[Introduction toodevice management in Azure Active Directory](device-management-introduction.md)** - This topic gives you an overview of hello various options you have tooconnect devices with Azure AD.</span></span> 


## <a name="trusted-devices"></a><span data-ttu-id="ebfc8-113">Důvěryhodná zařízení</span><span class="sxs-lookup"><span data-stu-id="ebfc8-113">Trusted devices</span></span>

<span data-ttu-id="ebfc8-114">První mobilní, cloudové první světě Azure Active Directory umožňuje toodevices přihlašování, aplikacím a službám odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-114">In a mobile-first, cloud-first world, Azure Active Directory enables single sign-on toodevices, apps, and services from anywhere.</span></span> <span data-ttu-id="ebfc8-115">U některých prostředků ve vašem prostředí, udělení přístupu toohello ti správní uživatelé nemusí být dostatečně dobrý.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-115">For certain resources in your environment, granting access toohello right users might not be good enough.</span></span> <span data-ttu-id="ebfc8-116">Kromě toho toohello ti správní uživatelé, můžete také potřebovat toobe důvěryhodné zařízení používá tooaccess prostředku.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-116">In addition toohello right users, you might also require a trusted device toobe used tooaccess a resource.</span></span> <span data-ttu-id="ebfc8-117">Ve vašem prostředí, můžete definovat, co je důvěryhodné zařízení na základě hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="ebfc8-117">In your environment, you can define what a trusted device is based on hello following components:</span></span>

- <span data-ttu-id="ebfc8-118">Hello [platformy zařízení](active-directory-conditional-access-azure-portal.md#device-platforms) na zařízení</span><span class="sxs-lookup"><span data-stu-id="ebfc8-118">hello [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) on a device</span></span>
- <span data-ttu-id="ebfc8-119">Jestli je zařízení kompatibilní</span><span class="sxs-lookup"><span data-stu-id="ebfc8-119">Whether a device is compliant</span></span>
- <span data-ttu-id="ebfc8-120">Jestli je zařízení připojené k doméně</span><span class="sxs-lookup"><span data-stu-id="ebfc8-120">Whether a device is domain-joined</span></span> 

<span data-ttu-id="ebfc8-121">Hello [platformy zařízení](active-directory-conditional-access-azure-portal.md#device-platforms) je charakterizovaná hello operační systém, který běží na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-121">hello [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) is characterized by hello operating system that is running on your device.</span></span> <span data-ttu-id="ebfc8-122">V zásadách podmíněného přístupu na základě zařízení můžete omezit přístup toocertain prostředky toospecific zařízení platformy.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-122">In your device-based conditional access policy, you can limit access toocertain resources toospecific device platforms.</span></span>



<span data-ttu-id="ebfc8-123">V zásadách podmíněného přístupu podle zařízení můžete vyžadovat toobe důvěryhodných zařízení označit jako kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-123">In a device-based conditional access policy, you can require trusted devices toobe marked as compliant.</span></span>

![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/24.png)

<span data-ttu-id="ebfc8-125">Zařízení může být označen jako kompatibilní v adresáři hello podle:</span><span class="sxs-lookup"><span data-stu-id="ebfc8-125">Devices can be marked as compliant in hello directory by:</span></span>

- <span data-ttu-id="ebfc8-126">Intune</span><span class="sxs-lookup"><span data-stu-id="ebfc8-126">Intune</span></span> 
- <span data-ttu-id="ebfc8-127">Systém správy mobilních zařízení třetích stran, který se integruje se službou Azure AD</span><span class="sxs-lookup"><span data-stu-id="ebfc8-127">A third-party mobile device management system that integrates with Azure AD</span></span>  

<span data-ttu-id="ebfc8-128">Jenom zařízení, které jsou připojené tooAzure AD může být označen jako kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-128">Only devices that are connected tooAzure AD can be marked as compliant.</span></span> <span data-ttu-id="ebfc8-129">tooconnect zařízení tooAzure služby Active Directory, máte následující možnosti hello:</span><span class="sxs-lookup"><span data-stu-id="ebfc8-129">tooconnect a device tooAzure Active Directory, you have hello following options:</span></span> 

- <span data-ttu-id="ebfc8-130">Zaregistrovat Azure AD</span><span class="sxs-lookup"><span data-stu-id="ebfc8-130">Azure AD registered</span></span>
- <span data-ttu-id="ebfc8-131">Připojené k Azure AD</span><span class="sxs-lookup"><span data-stu-id="ebfc8-131">Azure AD joined</span></span>
- <span data-ttu-id="ebfc8-132">Hybridní připojený k Azure AD</span><span class="sxs-lookup"><span data-stu-id="ebfc8-132">Hybrid Azure AD joined</span></span>

    ![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/26.png)

<span data-ttu-id="ebfc8-134">Pokud budete mít místní službě Active Directory (AD) nároky, můžete zvážit zařízení, které nejsou připojené tooAzure AD ale připojený k tooyour AD toobe důvěryhodné.</span><span class="sxs-lookup"><span data-stu-id="ebfc8-134">If you have an on-premises Active Directory (AD) footprint, you might consider devices that are not connected tooAzure AD but joined tooyour AD toobe trusted.</span></span>

![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a><span data-ttu-id="ebfc8-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebfc8-136">Next steps</span></span>

<span data-ttu-id="ebfc8-137">Před konfigurací zásad podmíněného přístupu na základě zařízení ve vašem prostředí, měli byste podniknout podívejte se na hello [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="ebfc8-137">Before configuring a device-based conditional access policy in your environment, you should take a look at hello [best practices for conditional access in Azure Active Directory](active-directory-conditional-access-best-practices.md).</span></span>

