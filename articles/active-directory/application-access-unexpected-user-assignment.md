---
title: "aaaHow tooAssign uživatelé tooapplications | Microsoft Docs"
description: "Pochopit, jak uživatelé přiřazovány tooan aplikace ve vašem klientovi"
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
ms.openlocfilehash: 4df60c7d723140d0d1bbd6ba8b34aa4e762d1138
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-tooapplications"></a><span data-ttu-id="b658a-103">Jak uživatelé tooapplications tooassign</span><span class="sxs-lookup"><span data-stu-id="b658a-103">How tooassign users tooapplications</span></span>

<span data-ttu-id="b658a-104">Tento článek vám pomůže toounderstand jak uživatelé přiřazovány tooan aplikace ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="b658a-104">This article help you toounderstand how users get assigned tooan application in your tenant.</span></span>

## <a name="how-do-users-get-assigned-tooan-application-in-azure-ad"></a><span data-ttu-id="b658a-105">Jak uživatelé přiřazovány tooan aplikace ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="b658a-105">How do users get assigned tooan application in Azure AD?</span></span>

<span data-ttu-id="b658a-106">Pro uživatele tooaccess aplikace se musí být přiřazena tooit nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="b658a-106">For a user tooaccess an application, they must first be assigned tooit in some way.</span></span> <span data-ttu-id="b658a-107">Přiřazení můžete provést pomocí správce, delegovaného obchodní nebo v některých případech hello uživatele sami.</span><span class="sxs-lookup"><span data-stu-id="b658a-107">Assignment can be performed by an administrator, a business delegate, or sometimes, hello user themselves.</span></span> <span data-ttu-id="b658a-108">Níže jsou popsány hello způsoby, které uživatelé mohou přiřadit tooapplications:</span><span class="sxs-lookup"><span data-stu-id="b658a-108">Below describes hello ways users can get assigned tooapplications:</span></span>

1.  <span data-ttu-id="b658a-109">Správce [přiřadí uživatele](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) přímo toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="b658a-109">An administrator [assigns a user](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) toohello application directly</span></span>

2.  <span data-ttu-id="b658a-110">Správce [přiřadí skupinu](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) tento hello uživatel je členem toohello aplikace, včetně:</span><span class="sxs-lookup"><span data-stu-id="b658a-110">An administrator [assigns a group](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) that hello user is a member of toohello application, including:</span></span>

  * <span data-ttu-id="b658a-111">Skupinu, která byla synchronizované z místní</span><span class="sxs-lookup"><span data-stu-id="b658a-111">A group that was synchronized from on-premises</span></span>

  * <span data-ttu-id="b658a-112">Skupiny zabezpečení statické vytvořeny v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="b658a-112">A static security group created in hello cloud</span></span>

  * <span data-ttu-id="b658a-113">A [skupiny dynamické zabezpečení](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) vytvořeny v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="b658a-113">A [dynamic security group](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) created in hello cloud</span></span>

  * <span data-ttu-id="b658a-114">Skupiny Office 365 v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="b658a-114">An Office 365 group created in hello cloud</span></span>

  * <span data-ttu-id="b658a-115">Hello [všichni uživatelé](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) skupiny</span><span class="sxs-lookup"><span data-stu-id="b658a-115">hello [All Users](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) group</span></span>

3.  <span data-ttu-id="b658a-116">Správce povolí [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow tooadd uživatele aplikace pomocí hello [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **přidat aplikaci** funkce **bez obchodní schválení**</span><span class="sxs-lookup"><span data-stu-id="b658a-116">An administrator enables [Self-service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow a user tooadd an application using hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Add App** feature **without business approval**</span></span>

4.  <span data-ttu-id="b658a-117">Správce povolí [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow tooadd uživatele aplikace pomocí hello [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **přidat aplikaci** funkce, ale pouze w **i-té předchozí schválení od vybraná sada business schvalovatelů**</span><span class="sxs-lookup"><span data-stu-id="b658a-117">An administrator enables [Self-service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow a user tooadd an application using hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Add App** feature, but only w**ith prior approval from a selected set of business approvers**</span></span>

5.  <span data-ttu-id="b658a-118">Správce povolí [Samoobslužná správa skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow toojoin uživatele skupiny přiřazené aplikace je příliš**bez obchodní schválení**</span><span class="sxs-lookup"><span data-stu-id="b658a-118">An administrator enables [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow a user toojoin a group that an application is assigned too**without business approval**</span></span>

6.  <span data-ttu-id="b658a-119">Správce povolí [Samoobslužná správa skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow toojoin uživatelské skupiny, která aplikace je přiřazena k, ale pouze **s předchozí schválení od vybraná sada business schvalovatelů**</span><span class="sxs-lookup"><span data-stu-id="b658a-119">An administrator enables [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow a user toojoin a group that an application is assigned to, but only **with prior approval from a selected set of business approvers**</span></span>

7.  <span data-ttu-id="b658a-120">Správce přiřadí licence tooa uživatele přímo pro první strany aplikace, jako je třeba [Microsoft Office 365](http://products.office.com/)</span><span class="sxs-lookup"><span data-stu-id="b658a-120">An administrator assigns a license tooa user directly for a first party application, like [Microsoft Office 365](http://products.office.com/)</span></span>

8.  <span data-ttu-id="b658a-121">Správce přiřadí tooa skupinu licencí, které hello uživatel je členem tooa první strany aplikace, jako je [Microsoft Office 365](http://products.office.com/)</span><span class="sxs-lookup"><span data-stu-id="b658a-121">An administrator assigns a license tooa group that hello user is a member of tooa first party application, like [Microsoft Office 365](http://products.office.com/)</span></span>

9.  <span data-ttu-id="b658a-122">[Správce souhlasí tooan aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toobe používány všemi uživateli a potom se uživatel přihlásí v aplikaci toohello</span><span class="sxs-lookup"><span data-stu-id="b658a-122">An [administrator consents tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toobe used by all users and then a user signs in toohello application</span></span>

10. <span data-ttu-id="b658a-123">Uživatel [souhlasí tooan aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) sami přihlášením toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="b658a-123">A user [consents tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) themselves by signing in toohello application</span></span>

## <a name="next-steps"></a><span data-ttu-id="b658a-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b658a-124">Next steps</span></span>
[<span data-ttu-id="b658a-125">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b658a-125">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
