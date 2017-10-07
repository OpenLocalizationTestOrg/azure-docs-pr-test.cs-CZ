---
title: "aaaNext kroků pro správu přístupu pomocí skupin | Microsoft Docs"
description: "Jak rozšířené-pro správu skupin zabezpečení a jak toouse tyto skupiny toomanage přístup tooa prostředků."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 44350a3c-8ea1-4da1-aaac-7fc53933dd21
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4fd55f893290fac3551a130f29bd12709cf551e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-owners-for-a-group"></a><span data-ttu-id="04c7f-103">Správa vlastníků pro skupinu</span><span class="sxs-lookup"><span data-stu-id="04c7f-103">Managing owners for a group</span></span>
<span data-ttu-id="04c7f-104">Jakmile vlastník prostředku přiřazena přístup tooa prostředků tooan Azure AD skupiny, hello členství ve skupině hello spravuje vlastník skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="04c7f-104">Once a resource owner has assigned access tooa resource tooan Azure AD group, hello membership of hello group is managed by hello group owner.</span></span> <span data-ttu-id="04c7f-105">Hello vlastník prostředku deleguje efektivně hello oprávnění tooassign uživatelé toohello prostředků toohello vlastník skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="04c7f-105">hello resource owner effectively delegates hello permission tooassign users toohello resource toohello owner of hello group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="04c7f-106">Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="04c7f-106">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> 

## <a name="assigning-group-ownership"></a><span data-ttu-id="04c7f-107">Přiřazení vlastnictví skupin</span><span class="sxs-lookup"><span data-stu-id="04c7f-107">Assigning group ownership</span></span>
<span data-ttu-id="04c7f-108">**tooadd skupinu tooa vlastníka**</span><span class="sxs-lookup"><span data-stu-id="04c7f-108">**tooadd an owner tooa group**</span></span>

1. <span data-ttu-id="04c7f-109">V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak otevřete adresáři vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="04c7f-109">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="04c7f-110">Vyberte hello **skupiny** kartu a pak otevřete hello skupinou, kterou chcete tooadd vlastníkům.</span><span class="sxs-lookup"><span data-stu-id="04c7f-110">Select hello **Groups** tab, and then open hello group that you want tooadd owners to.</span></span>
3. <span data-ttu-id="04c7f-111">Vyberte **přidat vlastníky**.</span><span class="sxs-lookup"><span data-stu-id="04c7f-111">Select **Add Owners**.</span></span>
4. <span data-ttu-id="04c7f-112">Na hello **přidat vlastníky** stránky, vyberte hello uživatele chcete tooadd jako hello vlastník této skupiny a zajistěte, aby toto jméno přidat i toohello **vybrané** podokně.</span><span class="sxs-lookup"><span data-stu-id="04c7f-112">On hello **Add owners** page, select hello user that you want tooadd as hello owner of this group, and make sure this name is added toohello **Selected** pane.</span></span>

<span data-ttu-id="04c7f-113">**tooremove vlastníka ze skupiny**</span><span class="sxs-lookup"><span data-stu-id="04c7f-113">**tooremove an owner from a group**</span></span>

1. <span data-ttu-id="04c7f-114">V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak otevřete adresáři vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="04c7f-114">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="04c7f-115">Vyberte hello **skupiny** kartu a pak otevřete hello skupiny, které chcete tooremove vlastníka z.</span><span class="sxs-lookup"><span data-stu-id="04c7f-115">Select hello **Groups** tab, and then open hello group that you want tooremove an owner from.</span></span>
3. <span data-ttu-id="04c7f-116">Vyberte hello **vlastníky** kartě.</span><span class="sxs-lookup"><span data-stu-id="04c7f-116">Select hello **Owners** tab.</span></span>
4. <span data-ttu-id="04c7f-117">Vyberte hello vlastníka má tooremove z této skupiny a pak vyberte **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="04c7f-117">Select hello owner that you want tooremove from this group, and then select **Remove**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="04c7f-118">Další informace</span><span class="sxs-lookup"><span data-stu-id="04c7f-118">Additional information</span></span>
<span data-ttu-id="04c7f-119">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="04c7f-119">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="04c7f-120">Správa přístupu tooresources pomocí skupin Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="04c7f-120">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="04c7f-121">Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="04c7f-121">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="04c7f-122">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="04c7f-122">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="04c7f-123">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="04c7f-123">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="04c7f-124">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="04c7f-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
