---
title: "Účet zřizování oznámení | Microsoft Docs"
description: "Zjistěte, jak zajistit, že budete upozorněni problémy související s zřizování uživatelů, které vyžadují vaši pozornost povolením účet zřizování oznámení."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: b99037fc28eca1a3ebffefb9e99991e74f52c9a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="account-provisioning-notifications"></a><span data-ttu-id="39505-103">Oznámení o zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="39505-103">Account Provisioning Notifications</span></span>
<span data-ttu-id="39505-104">Zřizování uživatelů, můžete automatizovat proces správy uživatele v aplikacích třetích stran SaaS.</span><span class="sxs-lookup"><span data-stu-id="39505-104">With user provisioning, you can automate the process of managing users in third party SaaS applications.</span></span> <br>
<span data-ttu-id="39505-105">Přestože se automatizovaného procesu, je čas od času interakce se tento proces vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="39505-105">While this is an automated process, your interaction with this process is from time to time required.</span></span> <br>
<span data-ttu-id="39505-106">To je, například v případě, pokud heslo účtu, že jste nakonfigurovali k výměně dat s třetí strany SaaS aplikace vypršela.</span><span class="sxs-lookup"><span data-stu-id="39505-106">This is, for example the case, when the password of the account you have configured to exchange data with a third party SaaS application has expired.</span></span> 

<span data-ttu-id="39505-107">Povolením účet zřizování oznámení můžete zajistit, že budete upozorněni problémy související s zřizování uživatelů, které vyžadují vaši pozornost.</span><span class="sxs-lookup"><span data-stu-id="39505-107">By enabling account provisioning notifications, you can ensure that you are notified of issues related to user provisioning that require your attention.</span></span>

<span data-ttu-id="39505-108">Aktivace nebo deaktivace účet zřizování oznámení jako součást konfigurace pro třetí strany aplikace SaaS zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="39505-108">You activate or deactivate account provisioning notifications as part of your user provisioning configuration for a third party SaaS application.</span></span>

![Zřizování uživatelů][1] 

<span data-ttu-id="39505-110">Pokud chcete aktivovat účet zřizování oznámení, zaškrtněte políčko související na **potvrzení** dialogové okno stránky a pak zadejte e-mailový alias příjemce.</span><span class="sxs-lookup"><span data-stu-id="39505-110">To activate account provisioning notifications, select the related checkbox on the **Confirmation** dialog page, and then type the email alias of the recipient.</span></span>

![Oznámení o zřizování účtů][2]

<span data-ttu-id="39505-112">Distribučního seznamu můžete zadat jako recipient; je ale důležité si uvědomit, že e-mailové oznámení obsahuje odkazy na sestavy, které jsou přístupné správci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="39505-112">You can enter a distribution list as recipient; however, it is important to note that the notification email contains links to reports that are only accessible by the Azure AD administrators.</span></span>

<span data-ttu-id="39505-113">Pokud máte účet zřizování oznámení povolena, obdržíte e-mailů o zásadních problémů, které se vztahují k zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="39505-113">If you have account provisioning notifications enabled, you will receive emails about critical issues that are related to user provisioning.</span></span> <span data-ttu-id="39505-114">Ale abyste se vyhnuli přetížení e-mailu, jenom obdržíte jeden oznámení e-mailu na každý den pro každou aplikaci SaaS oznámení e-mailu je povolený pro.</span><span class="sxs-lookup"><span data-stu-id="39505-114">However, to avoid an email overload, you will only receive one notification email per day for each SaaS application the notification email is enabled for.</span></span>

## <a name="related-articles"></a><span data-ttu-id="39505-115">Související články</span><span class="sxs-lookup"><span data-stu-id="39505-115">Related Articles</span></span>
* [<span data-ttu-id="39505-116">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="39505-116">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="39505-117">Automatizovat uživatele zřízení nebo zrušení zřízení k aplikacím SaaS</span><span class="sxs-lookup"><span data-stu-id="39505-117">Automate User Provisioning/Deprovisioning to SaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="39505-118">Přizpůsobení mapování atributů pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="39505-118">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="39505-119">Zapisují se výrazy pro mapování atributů</span><span class="sxs-lookup"><span data-stu-id="39505-119">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="39505-120">Filtry pro zřizování uživatelů oborů</span><span class="sxs-lookup"><span data-stu-id="39505-120">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="39505-121">Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM</span><span class="sxs-lookup"><span data-stu-id="39505-121">Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="39505-122">Seznam kurzů k integraci aplikací SaaS</span><span class="sxs-lookup"><span data-stu-id="39505-122">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
