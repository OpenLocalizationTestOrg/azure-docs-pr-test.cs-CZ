---
title: "aaaUsing tooSaaS přístup toomanage skupiny aplikací | Microsoft Docs"
description: "Jak toouse skupin v Azure Active Directory Premium nebo Basic tooassign přistupovat k tooSaaS aplikace, které jsou integrované s Azure Active Directory."
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
ms.openlocfilehash: f45ea4472b3d88e8ea514af3db31b4cc9ea68d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-group-toomanage-access-toosaas-applications"></a><span data-ttu-id="37839-103">Pomocí skupiny toomanage přístup k tooSaaS aplikacím</span><span class="sxs-lookup"><span data-stu-id="37839-103">Using a group toomanage access tooSaaS applications</span></span>
<span data-ttu-id="37839-104">Pomocí Azure Active Directory (Azure AD) s licencí Azure AD Premium nebo Azure AD Basic, můžete použít skupiny tooassign přístup tooa SaaS aplikace, která je integrovaná se službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="37839-104">Using Azure Active Directory (Azure AD) with an Azure AD Premium or Azure AD Basic license, you can use groups tooassign access tooa SaaS application that's integrated with Azure AD.</span></span> <span data-ttu-id="37839-105">Například pokud chcete přístup tooassign pro hello marketingového oddělení toouse pět různých aplikací SaaS, můžete vytvořit skupinu, která obsahuje hello uživatelé v marketingovém oddělení hello a pak mu přiřaďte tuto skupinu toothese pět SaaS aplikace, které jsou potřebuje hello marketingového oddělení.</span><span class="sxs-lookup"><span data-stu-id="37839-105">For example, if you want tooassign access for hello marketing department toouse five different SaaS applications, you can create a group that contains hello users in hello marketing department, and then assign that group toothese five SaaS applications that are needed by hello marketing department.</span></span> <span data-ttu-id="37839-106">Tímto způsobem můžete ušetřit čas tím Správa členství hello hello marketingové oddělení na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="37839-106">This way you can save time by managing hello membership of hello marketing department in one place.</span></span> <span data-ttu-id="37839-107">Uživatelé pak jsou přiřazeni toohello aplikace když budou přidány jako členové skupiny marketing hello, a jejich přiřazení odebrali z aplikace hello, když jsou odebrány z hello marketingová skupina.</span><span class="sxs-lookup"><span data-stu-id="37839-107">Users then are assigned toohello application when they are added as members of hello marketing group, and have their assignments removed from hello application when they are removed from hello marketing group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="37839-108">Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="37839-108">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> 

<span data-ttu-id="37839-109">Tato funkce slouží se stovkami aplikací, které můžete přidat z v rámci hello galerii aplikací Azure AD.</span><span class="sxs-lookup"><span data-stu-id="37839-109">This capability can be used with hundreds of applications that you can add from within hello Azure AD Application Gallery.</span></span>

<span data-ttu-id="37839-110">**tooassign přístup pro skupiny tooa aplikace SaaS**</span><span class="sxs-lookup"><span data-stu-id="37839-110">**tooassign access for a group tooa SaaS application**</span></span>

1. <span data-ttu-id="37839-111">V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory** na hello navigačním panelu na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="37839-111">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory** on hello navigation bar on hello left hand side.</span></span>
2. <span data-ttu-id="37839-112">Vyberte hello **Directory** kartu a pak otevřete hello adresář, ve kterém má tooassign přístup pro skupiny tooa aplikaci SaaS.</span><span class="sxs-lookup"><span data-stu-id="37839-112">Select hello **Directory** tab, and then open hello directory in which you want tooassign access for a group tooa SaaS application.</span></span>
3. <span data-ttu-id="37839-113">Vyberte hello **aplikace** kartě. Vyberte aplikaci, která jste přidali z hello galerii aplikací a pak klikněte na hello **uživatelů a skupin** kartě.</span><span class="sxs-lookup"><span data-stu-id="37839-113">Select hello **Applications** tab. Select an application that you added from hello Application Gallery, and then click  hello **Users and Groups** tab.</span></span>
4. <span data-ttu-id="37839-114">Na hello **uživatelů a skupin** na kartě hello **počínaje** pole, zadejte název hello toowhich hello skupiny, které má tooassign přístup, a pak vyberte hello zaškrtnutí v pravé horní části hello.</span><span class="sxs-lookup"><span data-stu-id="37839-114">On hello **Users and Groups** tab, in hello **Starting with** field, enter hello name of hello group toowhich you want tooassign access, and then select hello check mark in hello upper right.</span></span> <span data-ttu-id="37839-115">Potřebujete jenom tootype hello první část název skupiny.</span><span class="sxs-lookup"><span data-stu-id="37839-115">You only need tootype hello first part of a group's name.</span></span>
5. <span data-ttu-id="37839-116">Vyberte skupiny hello a pak vyberte hello **přiřadit přístup** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="37839-116">Select hello group, then then select hello **Assign Access** button.</span></span> <span data-ttu-id="37839-117">Vyberte **Ano** až se zobrazí potvrzovací zpráva hello.</span><span class="sxs-lookup"><span data-stu-id="37839-117">Select **Yes** when you see hello confirmation message.</span></span> <span data-ttu-id="37839-118">Vnořené členství ve skupinách nejsou podporovány pro přiřazení na základě skupiny tooapplications v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="37839-118">Nested group memberships are not supported for group-based assignment tooapplications at this time.</span></span>
6. <span data-ttu-id="37839-119">Můžete také zjistit, uživatelů, kteří jsou přiřazeny toohello aplikace, buď přímo, nebo členství ve skupině.</span><span class="sxs-lookup"><span data-stu-id="37839-119">You can also see which users are assigned toohello application, either directly or by membership in a group.</span></span> <span data-ttu-id="37839-120">toodo se změna hello **zobrazit rozevírací z "Skupinami"** příliš**'Všechny uživatele'**.</span><span class="sxs-lookup"><span data-stu-id="37839-120">toodo this, change hello **Show dropdown from 'Groups'** too**'All Users'**.</span></span> <span data-ttu-id="37839-121">Hello zobrazuje uživatele v adresáři hello a jestli je každý uživatel přiřazen toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37839-121">hello list shows users in hello directory and whether or not each user is assigned toohello application.</span></span> <span data-ttu-id="37839-122">seznam Hello také ukazuje, jestli uživatelé hello přiřazené přiřazené přímo toohello aplikaci (zobrazené jako "Direct, typ přiřazení), nebo na základě členství ve skupině (typ přiřazení zobrazí jako 'Zděděné.')</span><span class="sxs-lookup"><span data-stu-id="37839-122">hello list also shows whether hello assigned users are assigned toohello application directly (assignment type shown as 'Direct'), or by virtue of group membership (assignment type shown as 'Inherited.')</span></span>

> [!NOTE]
> <span data-ttu-id="37839-123">Až poté, co jste povolili Azure AD Premium nebo Azure AD Basic, můžete zobrazit hello uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="37839-123">You can see hello Users and Groups tab only after you have enabled Azure AD Premium or Azure AD Basic.</span></span>
>
>

### <a name="next-steps"></a><span data-ttu-id="37839-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37839-124">Next steps</span></span>
<span data-ttu-id="37839-125">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="37839-125">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="37839-126">Správa přístupu tooresources pomocí skupin Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="37839-126">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="37839-127">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="37839-127">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="37839-128">Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="37839-128">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="37839-129">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="37839-129">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="37839-130">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="37839-130">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
