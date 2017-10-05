---
title: "Pomocí skupiny pro správu přístupu k aplikacím SaaS | Microsoft Docs"
description: "Jak používat skupin v Azure Active Directory Premium nebo Basic přiřazení přístupu k aplikacím SaaS, které jsou integrované s Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro;oldportal
ms.openlocfilehash: d350011ee9fc5ced9ddb16993f68d3c840a645a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a><span data-ttu-id="50e70-103">Použití skupiny ke správě přístupu k aplikacím SaaS</span><span class="sxs-lookup"><span data-stu-id="50e70-103">Using a group to manage access to SaaS applications</span></span>
<span data-ttu-id="50e70-104">Pomocí Azure Active Directory (Azure AD) s licencí Azure AD Premium nebo Azure AD Basic, můžete použít skupiny přiřazení přístupu k aplikaci SaaS, která je integrovaná s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="50e70-104">Using Azure Active Directory (Azure AD) with an Azure AD Premium or Azure AD Basic license, you can use groups to assign access to a SaaS application that's integrated with Azure AD.</span></span> <span data-ttu-id="50e70-105">Například pokud chcete přiřadit přístup pro marketingové oddělení používat pět různých aplikace SaaS, je můžete vytvořit skupinu, která obsahuje uživatelé v marketingovém oddělení a potom přidělit této skupině pro těchto pět aplikace SaaS, které jsou vyžadovány z marketingového oddělení.</span><span class="sxs-lookup"><span data-stu-id="50e70-105">For example, if you want to assign access for the marketing department to use five different SaaS applications, you can create a group that contains the users in the marketing department, and then assign that group to these five SaaS applications that are needed by the marketing department.</span></span> <span data-ttu-id="50e70-106">Tímto způsobem můžete ušetřit čas tím Správa členství z marketingového oddělení na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="50e70-106">This way you can save time by managing the membership of the marketing department in one place.</span></span> <span data-ttu-id="50e70-107">Uživatelé pak přiřazené k aplikaci při jsou přidány jako členové skupiny marketing a jejich přiřazení odebrali z aplikace, když jsou odebrány ze skupiny marketing.</span><span class="sxs-lookup"><span data-stu-id="50e70-107">Users then are assigned to the application when they are added as members of the marketing group, and have their assignments removed from the application when they are removed from the marketing group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="50e70-108">Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.</span><span class="sxs-lookup"><span data-stu-id="50e70-108">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> 

<span data-ttu-id="50e70-109">Tato funkce slouží se stovkami aplikací, které můžete přidat z v galerii aplikací Azure AD.</span><span class="sxs-lookup"><span data-stu-id="50e70-109">This capability can be used with hundreds of applications that you can add from within the Azure AD Application Gallery.</span></span>

<span data-ttu-id="50e70-110">**Přiřadit přístup pro skupinu k aplikaci SaaS**</span><span class="sxs-lookup"><span data-stu-id="50e70-110">**To assign access for a group to a SaaS application**</span></span>

1. <span data-ttu-id="50e70-111">V [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory** na navigačním panelu na levé straně.</span><span class="sxs-lookup"><span data-stu-id="50e70-111">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory** on the navigation bar on the left hand side.</span></span>
2. <span data-ttu-id="50e70-112">Vyberte **Directory** kartě a pak otevřete adresář, ve které chcete přiřadit přístup pro skupinu k aplikaci SaaS.</span><span class="sxs-lookup"><span data-stu-id="50e70-112">Select the **Directory** tab, and then open the directory in which you want to assign access for a group to a SaaS application.</span></span>
3. <span data-ttu-id="50e70-113">Vyberte **aplikace** kartě. Vyberte aplikaci, která jste přidali v galerii aplikací a klikněte **uživatelů a skupin** kartě.</span><span class="sxs-lookup"><span data-stu-id="50e70-113">Select the **Applications** tab. Select an application that you added from the Application Gallery, and then click  the **Users and Groups** tab.</span></span>
4. <span data-ttu-id="50e70-114">Na **uživatelů a skupin** ve **počínaje** pole, zadejte název skupiny, ke kterému chcete přiřadit přístup a potom vyberte zaškrtnutí v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="50e70-114">On the **Users and Groups** tab, in the **Starting with** field, enter the name of the group to which you want to assign access, and then select the check mark in the upper right.</span></span> <span data-ttu-id="50e70-115">Stačí zadejte první část název skupiny.</span><span class="sxs-lookup"><span data-stu-id="50e70-115">You only need to type the first part of a group's name.</span></span>
5. <span data-ttu-id="50e70-116">Vyberte skupinu, a poté vyberte položku **přiřadit přístup** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="50e70-116">Select the group, then then select the **Assign Access** button.</span></span> <span data-ttu-id="50e70-117">Vyberte **Ano** až se zobrazí zpráva o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="50e70-117">Select **Yes** when you see the confirmation message.</span></span> <span data-ttu-id="50e70-118">Vnořené členství ve skupinách momentálně není podporované v případě přiřazování k aplikacím podle skupiny.</span><span class="sxs-lookup"><span data-stu-id="50e70-118">Nested group memberships are not supported for group-based assignment to applications at this time.</span></span>
6. <span data-ttu-id="50e70-119">Uvidíte taky, kteří uživatelé jsou přiřazeny k aplikaci, buď přímo, nebo členství ve skupině.</span><span class="sxs-lookup"><span data-stu-id="50e70-119">You can also see which users are assigned to the application, either directly or by membership in a group.</span></span> <span data-ttu-id="50e70-120">Chcete-li to provést, změňte **zobrazit rozevírací z "Skupinami"** k **'Všechny uživatele'**.</span><span class="sxs-lookup"><span data-stu-id="50e70-120">To do this, change the **Show dropdown from 'Groups'** to **'All Users'**.</span></span> <span data-ttu-id="50e70-121">V seznamu jsou uživatelé v adresáři a jestli je každý uživatel přiřazené k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="50e70-121">The list shows users in the directory and whether or not each user is assigned to the application.</span></span> <span data-ttu-id="50e70-122">V seznamu také ukazuje, jestli přiřazené uživatele přiřazené k aplikaci přímo (zobrazené jako "Direct, typ přiřazení) nebo na základě členství ve skupině (typ přiřazení zobrazí jako 'Zděděné.')</span><span class="sxs-lookup"><span data-stu-id="50e70-122">The list also shows whether the assigned users are assigned to the application directly (assignment type shown as 'Direct'), or by virtue of group membership (assignment type shown as 'Inherited.')</span></span>

> [!NOTE]
> <span data-ttu-id="50e70-123">Na kartě uživatelů a skupin najdete v až poté, co jste povolili Azure AD Premium nebo Azure AD Basic.</span><span class="sxs-lookup"><span data-stu-id="50e70-123">You can see the Users and Groups tab only after you have enabled Azure AD Premium or Azure AD Basic.</span></span>
>
>

### <a name="next-steps"></a><span data-ttu-id="50e70-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50e70-124">Next steps</span></span>
<span data-ttu-id="50e70-125">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="50e70-125">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="50e70-126">Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="50e70-126">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="50e70-127">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="50e70-127">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="50e70-128">Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="50e70-128">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="50e70-129">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="50e70-129">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="50e70-130">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="50e70-130">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
