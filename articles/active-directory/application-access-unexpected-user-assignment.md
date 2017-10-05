---
title: "Postup přiřazení uživatelů k aplikacím | Microsoft Docs"
description: "Pochopit, jak přiřadit uživatele k aplikaci v klientovi"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 916238ba402a2555bac620d7f08e02799d981ae0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-assign-users-to-applications"></a><span data-ttu-id="a12ee-103">Přiřazení uživatelů k aplikacím</span><span class="sxs-lookup"><span data-stu-id="a12ee-103">How to assign users to applications</span></span>

<span data-ttu-id="a12ee-104">Tento článek vám pomůže lépe porozumět jak přiřadit uživatele k aplikaci v klientovi.</span><span class="sxs-lookup"><span data-stu-id="a12ee-104">This article help you to understand how users get assigned to an application in your tenant.</span></span>

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a><span data-ttu-id="a12ee-105">Jak uživatelé přiřazovány aplikace ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="a12ee-105">How do users get assigned to an application in Azure AD?</span></span>

<span data-ttu-id="a12ee-106">Pro uživatele o přístup k aplikaci je třeba nejprve je přiřadit k němu nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="a12ee-106">For a user to access an application, they must first be assigned to it in some way.</span></span> <span data-ttu-id="a12ee-107">Přiřazení můžete provést pomocí správce, delegáta firmy nebo v některých případech uživatel sami.</span><span class="sxs-lookup"><span data-stu-id="a12ee-107">Assignment can be performed by an administrator, a business delegate, or sometimes, the user themselves.</span></span> <span data-ttu-id="a12ee-108">Níže popisuje způsoby, které uživatelé můžou přiřazovány aplikace:</span><span class="sxs-lookup"><span data-stu-id="a12ee-108">Below describes the ways users can get assigned to applications:</span></span>

1.  <span data-ttu-id="a12ee-109">Správce [přiřadí uživatele](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) přímo do aplikace</span><span class="sxs-lookup"><span data-stu-id="a12ee-109">An administrator [assigns a user](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) to the application directly</span></span>

2.  <span data-ttu-id="a12ee-110">Správce [přiřadí skupinu](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) , uživatel je členem do aplikace, včetně:</span><span class="sxs-lookup"><span data-stu-id="a12ee-110">An administrator [assigns a group](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) that the user is a member of to the application, including:</span></span>

  * <span data-ttu-id="a12ee-111">Skupinu, která byla synchronizované z místní</span><span class="sxs-lookup"><span data-stu-id="a12ee-111">A group that was synchronized from on-premises</span></span>

  * <span data-ttu-id="a12ee-112">Skupiny zabezpečení statické vytvořeny v cloudu</span><span class="sxs-lookup"><span data-stu-id="a12ee-112">A static security group created in the cloud</span></span>

  * <span data-ttu-id="a12ee-113">A [skupiny dynamické zabezpečení](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) vytvořeny v cloudu</span><span class="sxs-lookup"><span data-stu-id="a12ee-113">A [dynamic security group](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) created in the cloud</span></span>

  * <span data-ttu-id="a12ee-114">Skupiny Office 365 v cloudu</span><span class="sxs-lookup"><span data-stu-id="a12ee-114">An Office 365 group created in the cloud</span></span>

  * <span data-ttu-id="a12ee-115">[Všichni uživatelé](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) skupiny</span><span class="sxs-lookup"><span data-stu-id="a12ee-115">The [All Users](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) group</span></span>

3.  <span data-ttu-id="a12ee-116">Správce povolí [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) umožňující uživateli přidat aplikace pomocí [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **přidat aplikaci** funkce **bez obchodní schválení**</span><span class="sxs-lookup"><span data-stu-id="a12ee-116">An administrator enables [Self-service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) to allow a user to add an application using the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Add App** feature **without business approval**</span></span>

4.  <span data-ttu-id="a12ee-117">Správce povolí [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) umožňující uživateli přidat aplikace pomocí [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **přidat aplikaci** funkce, ale pouze w**i-té předchozí schválení od vybraná sada business schvalovatelů**</span><span class="sxs-lookup"><span data-stu-id="a12ee-117">An administrator enables [Self-service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) to allow a user to add an application using the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Add App** feature, but only w**ith prior approval from a selected set of business approvers**</span></span>

5.  <span data-ttu-id="a12ee-118">Správce povolí [Samoobslužná správa skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) uživatele o připojení ke skupině přiřazený aplikace **bez obchodní schválení**</span><span class="sxs-lookup"><span data-stu-id="a12ee-118">An administrator enables [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) to allow a user to join a group that an application is assigned to **without business approval**</span></span>

6.  <span data-ttu-id="a12ee-119">Správce povolí [Samoobslužná správa skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) uživatele o připojení ke skupině, která aplikace je přiřazena k, ale pouze **s předchozí schválení od vybraná sada business schvalovatelů**</span><span class="sxs-lookup"><span data-stu-id="a12ee-119">An administrator enables [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) to allow a user to join a group that an application is assigned to, but only **with prior approval from a selected set of business approvers**</span></span>

7.  <span data-ttu-id="a12ee-120">Správce přiřadí licence pro uživatele přímo pro první strany aplikace, jako je třeba [Microsoft Office 365](http://products.office.com/)</span><span class="sxs-lookup"><span data-stu-id="a12ee-120">An administrator assigns a license to a user directly for a first party application, like [Microsoft Office 365](http://products.office.com/)</span></span>

8.  <span data-ttu-id="a12ee-121">Správce přiřadí licence pro skupinu, uživatel je členem k první aplikaci strany, jako je třeba [Microsoft Office 365](http://products.office.com/)</span><span class="sxs-lookup"><span data-stu-id="a12ee-121">An administrator assigns a license to a group that the user is a member of to a first party application, like [Microsoft Office 365](http://products.office.com/)</span></span>

9.  <span data-ttu-id="a12ee-122">[Správce souhlasí k aplikaci](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) má být používána všichni uživatelé a pak uživatel přihlásí k aplikaci</span><span class="sxs-lookup"><span data-stu-id="a12ee-122">An [administrator consents to an application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) to be used by all users and then a user signs in to the application</span></span>

10. <span data-ttu-id="a12ee-123">Uživatel [souhlasí k aplikaci](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) sami po přihlášení k aplikaci</span><span class="sxs-lookup"><span data-stu-id="a12ee-123">A user [consents to an application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) themselves by signing in to the application</span></span>

## <a name="next-steps"></a><span data-ttu-id="a12ee-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a12ee-124">Next steps</span></span>
[<span data-ttu-id="a12ee-125">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a12ee-125">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
