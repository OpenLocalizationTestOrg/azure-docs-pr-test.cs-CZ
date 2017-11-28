---
title: "aaaSetting služby Azure AD Join pro vaše uživatele | Microsoft Docs"
description: "Vysvětluje, jak mohou správci nastavit službu Azure AD Join pro místní adresář a registraci zařízení."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: bfc5d415-c918-4d8b-afee-b3f41cc28469
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 60a5aeb11292cb6057ab1065c3ab77e5981d0cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-azure-ad-join-in-your-organization"></a><span data-ttu-id="8c008-103">Nastavení služby Azure AD Join v organizaci</span><span class="sxs-lookup"><span data-stu-id="8c008-103">Setting up Azure AD Join in your organization</span></span>
<span data-ttu-id="8c008-104">Před nastavením Azure Active Directory Join (Azure AD Join), je nutné tooeither synchronizace registrace vašeho místního adresáře uživatelů toohello cloudu nebo ručně vytvořit spravované účty v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c008-104">Before you set up Azure Active Directory Join (Azure AD Join), you need tooeither sync up your on-premises directory of users toohello cloud or manually create managed accounts in Azure AD.</span></span>

<span data-ttu-id="8c008-105">Podrobné pokyny pro synchronizaci tooAzure vaše místní uživatele AD, najdete v článku [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="8c008-105">Detailed instructions for syncing your on-premises users tooAzure AD is covered in [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="8c008-106">toomanually vytváření a Správa uživatelů ve službě Azure AD, najdete v příliš[Správa uživatelů ve službě Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c008-106">toomanually create and manage users in Azure AD, refer too[User management in Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).</span></span>

## <a name="set-up-device-registration"></a><span data-ttu-id="8c008-107">Nastavení registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="8c008-107">Set up device registration</span></span>
1. <span data-ttu-id="8c008-108">Přihlaste se na toohello portálu Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="8c008-108">Log on toohello Azure portal as an administrator.</span></span>
2. <span data-ttu-id="8c008-109">V levém podokně hello vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8c008-109">On hello left pane, select **Active Directory**.</span></span>
3. <span data-ttu-id="8c008-110">Na hello **Directory** vyberte adresáře.</span><span class="sxs-lookup"><span data-stu-id="8c008-110">On hello **Directory** tab, select your directory.</span></span>
4. <span data-ttu-id="8c008-111">Vyberte hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="8c008-111">Select hello **Configure** tab.</span></span>
5. <span data-ttu-id="8c008-112">Přejděte toohello **zařízení** části.</span><span class="sxs-lookup"><span data-stu-id="8c008-112">Go toohello **Devices** section.</span></span>
6. <span data-ttu-id="8c008-113">Na hello **zařízení** nastavte hello následující:</span><span class="sxs-lookup"><span data-stu-id="8c008-113">On hello **devices** tab, set hello following:</span></span>  
   * <span data-ttu-id="8c008-114">**MAXIMÁLNÍ počet zařízení na uživatele**: Vyberte hello maximální počet zařízení, která uživatel může mít ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c008-114">**MAXIMUM NUMBER OF DEVICES PER USER**: Select hello maximum number of devices that a user can have in Azure AD.</span></span>  <span data-ttu-id="8c008-115">Pokud uživatel dosáhne této kvóty, nebude možné tooadd další zařízení, dokud nejméně jeden z jejich stávajících zařízení se odeberou.</span><span class="sxs-lookup"><span data-stu-id="8c008-115">If a user reaches this quota, they will not be able tooadd additional devices until one or more of their existing devices are removed.</span></span>
   * <span data-ttu-id="8c008-116">**VYŽADOVAT Multi-FACTOR AUTH tooJOIN zařízení**: nastavte, zda uživatelé jsou požadované tooprovide druhé ověření zohlednit toojoin jejich zařízení tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="8c008-116">**REQUIRE MULTI-FACTOR AUTH tooJOIN DEVICES**: Set whether users are required tooprovide a second authentication factor toojoin their device tooAzure AD.</span></span> <span data-ttu-id="8c008-117">Další informace o ověřování Azure Multi-Factor Authentication, naleznete v části [Začínáme s Azure Multi-Factor Authentication v cloudu hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="8c008-117">For more information on Azure Multi-Factor Authentication, see [Getting started with Azure Multi-Factor Authentication in hello cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>
   * <span data-ttu-id="8c008-118">**Uživatelé, kteří mohou zařízení připojit k AZURE AD**: Vyberte hello uživatelů a skupin, které jsou povoleny toojoin zařízení tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="8c008-118">**USERS MAY AZURE AD JOIN DEVICES**: Select hello users and groups that are allowed toojoin devices tooAzure AD.</span></span>
   * <span data-ttu-id="8c008-119">**ZAŘÍZENÍ připojená k další správci ve službě AZURE AD**: U Azure AD Premium nebo hello Enterprise Mobility Suite (EMS), je možné, kteří uživatelé budou mít oprávnění místního správce toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="8c008-119">**ADDITIONAL ADMINISTRATORS ON AZURE AD JOINED DEVICES**: With Azure AD Premium or hello Enterprise Mobility Suite (EMS), you can choose which users are granted local administrator rights toohello device.</span></span> <span data-ttu-id="8c008-120">Globální správci a vlastníci zařízení mají oprávnění místního správce automaticky.</span><span class="sxs-lookup"><span data-stu-id="8c008-120">Global administrators and device owners are granted local administrator rights by default.</span></span>

<span data-ttu-id="8c008-121"><center>![Nastavení registrace zařízení](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center></span><span class="sxs-lookup"><span data-stu-id="8c008-121"><center>![Set up device regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center></span></span>

<span data-ttu-id="8c008-122">Po nastavení služby Azure AD Join pro vaši uživatelé mohli připojit tooAzure AD prostřednictvím jejich podnikové nebo osobní zařízení.</span><span class="sxs-lookup"><span data-stu-id="8c008-122">After you set up Azure AD Join for your users, they can connect tooAzure AD through their corporate or personal devices.</span></span>

<span data-ttu-id="8c008-123">Tady jsou tři scénáře hello můžete použít tooenable tooset vaši uživatelé služby Azure AD Join:</span><span class="sxs-lookup"><span data-stu-id="8c008-123">Following are hello three scenarios you can use tooenable your users tooset up Azure AD Join:</span></span>

* <span data-ttu-id="8c008-124">Uživatelé připojení k zařízení ve vlastnictví společnosti přímo tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="8c008-124">Users join a company-owned device directly tooAzure AD.</span></span>
* <span data-ttu-id="8c008-125">Uživatelé připojení k doméně vlastněných společností zařízení toohello místní služby Active Directory a potom rozšíří hello zařízení tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="8c008-125">Users domain-join a company-owned device toohello on-premises Active Directory and then extend hello device tooAzure AD.</span></span>
* <span data-ttu-id="8c008-126">Uživatelé přidat pracovní nebo školní účty tooWindows na osobním zařízení</span><span class="sxs-lookup"><span data-stu-id="8c008-126">Users add work or school accounts tooWindows on a personal device</span></span>

## <a name="additional-information"></a><span data-ttu-id="8c008-127">Další informace</span><span class="sxs-lookup"><span data-stu-id="8c008-127">Additional information</span></span>
* [<span data-ttu-id="8c008-128">Windows 10 pro podnik hello: způsoby toouse zařízení pro práci</span><span class="sxs-lookup"><span data-stu-id="8c008-128">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="8c008-129">Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="8c008-129">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="8c008-130">Další informace o scénářích použití pro službu Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="8c008-130">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="8c008-131">Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10</span><span class="sxs-lookup"><span data-stu-id="8c008-131">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="8c008-132">Nastavení služby Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="8c008-132">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

