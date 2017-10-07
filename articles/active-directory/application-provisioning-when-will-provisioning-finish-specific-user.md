---
title: "aaaFind se kdy konkrétního uživatele budou mít tooaccess aplikace | Microsoft Docs"
description: "Jak toofind se při zásadní význam uživatel být schopný tooaccess aplikaci jste nakonfigurovali pro zřizování uživatelů s Azure AD"
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
ms.openlocfilehash: bb9520499dcc8bbbe6fae05c5238c8852815ea0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-tooaccess-an-application"></a><span data-ttu-id="518f1-103">Zjistit, kdy konkrétní uživatel nebude moct tooaccess aplikace</span><span class="sxs-lookup"><span data-stu-id="518f1-103">Find out when a specific user will be able tooaccess an application</span></span>
<span data-ttu-id="518f1-104">Při použití zřizování automatické uživatelů s aplikací, Azure AD automaticky zřizovat a aktualizace uživatelské účty v aplikaci na základě třeba [přiřazení uživatelů a skupin](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) v pravidelném plánovaném časovém intervalu, obvykle každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="518f1-104">When using automatic user provisioning with an application, Azure AD automatically provision and update user accounts in an app based on things like [user and group assignment](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) at a regularly scheduled time interval, typically every 10 minutes.</span></span>

## <a name="how-long-does-it-take"></a><span data-ttu-id="518f1-105">Jak dlouho to trvá?</span><span class="sxs-lookup"><span data-stu-id="518f1-105">How long does it take?</span></span>

<span data-ttu-id="518f1-106">Hello doba potřebná pro daného uživatele toobe, zřízený závisí hlavně na tom, jestli počáteční synchronizaci "úplná" již došlo k.</span><span class="sxs-lookup"><span data-stu-id="518f1-106">hello time it takes for a given user toobe provisioned depends mainly on whether or not an initial “full” sync has already occurred.</span></span>

<span data-ttu-id="518f1-107">Hello první synchronizace mezi službou Azure AD a aplikace, může trvat od 20 minut tooseveral hodin, v závislosti na velikosti hello directory hello Azure AD a hello počet uživatelů v oboru pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="518f1-107">hello first sync between Azure AD and an app can take anywhere from 20 minutes tooseveral hours, depending on hello size of hello Azure AD directory and hello number of users in scope for provisioning.</span></span> 

<span data-ttu-id="518f1-108">Následné synchronizace po počáteční synchronizaci hello být rychlejší (např. do 10 minut), jako hello zřizování služby ukládá vodoznaky, které představují hello stav obou systémů po počáteční synchronizaci hello, zvýšení výkonu následné synchronizace.</span><span class="sxs-lookup"><span data-stu-id="518f1-108">Subsequent syncs after hello initial sync be faster (e.g. within 10 minutes), as hello provisioning service stores watermarks that represent hello state of both systems after hello initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-toocheck-hello-status-of-a-user"></a><span data-ttu-id="518f1-109">Jak toocheck hello stav uživatele</span><span class="sxs-lookup"><span data-stu-id="518f1-109">How toocheck hello status of a user</span></span>

<span data-ttu-id="518f1-110">Stav zřizování toosee hello pro vybraného uživatele, poraďte se se hello protokoly auditu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="518f1-110">toosee hello provisioning status for a selected user, consult hello Audit logs in Azure AD.</span></span>

<span data-ttu-id="518f1-111">Hello zřizování protokoly auditu je přístupná v hello portál Azure, v hello **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt; protokolech auditování**kartě. Filtr hello přihlásí hello **zřizování účtu** kategorie tooonly najdete v části hello zřizování události pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="518f1-111">hello provisioning audit logs can be accessed in hello Azure portal, in hello **Azure Active Directory &gt; Enterprise Apps &gt; \[Application Name\] &gt; Audit Logs** tab. Filter hello logs on hello **Account Provisioning** category tooonly see hello provisioning events for that app.</span></span> <span data-ttu-id="518f1-112">Můžete vyhledat uživatele podle hello "odpovídající ID" nakonfigurovaný pro ně v mapování atributů hello.</span><span class="sxs-lookup"><span data-stu-id="518f1-112">You can search for users based on hello “matching ID” that was configured for them in hello attribute mappings.</span></span> 

<span data-ttu-id="518f1-113">Například, pokud jste nakonfigurovali hello "hlavní název uživatele" nebo "e-mailovou adresu" jako hello odpovídající atribut na straně hello Azure AD, a tím zřizování uživatelů hello má hodnotu "audrey@contoso.com", pak protokoly auditu hello vyhledávání pro"audrey@contoso.com" a pak zkontrolujte Vrátí položky.</span><span class="sxs-lookup"><span data-stu-id="518f1-113">For example, if you configured hello “user principal name” or “email address” as hello matching attribute on hello Azure AD side, and hello user not being provisioning has a value of “audrey@contoso.com”, then search hello audit logs for “audrey@contoso.com” and review then entries returned.</span></span>

<span data-ttu-id="518f1-114">Hello zřizování auditu protokoly záznam všech hello operace provedené hello zřizování služby, včetně:</span><span class="sxs-lookup"><span data-stu-id="518f1-114">hello provisioning audit logs record all hello operations performed by hello provisioning service, including:</span></span>

* <span data-ttu-id="518f1-115">Dotazování Azure AD pro přiřazené uživatele, které jsou v oboru pro zřizování</span><span class="sxs-lookup"><span data-stu-id="518f1-115">Querying Azure AD for assigned users that are in scope for provisioning</span></span>
* <span data-ttu-id="518f1-116">Dotazování hello cílové aplikace hello existence tyto uživatele</span><span class="sxs-lookup"><span data-stu-id="518f1-116">Querying hello target app for hello existence of those users</span></span>
* <span data-ttu-id="518f1-117">Porovnávání objektů uživatele hello mezi hello systému</span><span class="sxs-lookup"><span data-stu-id="518f1-117">Comparing hello user objects between hello system</span></span>
* <span data-ttu-id="518f1-118">Přidání, aktualizace nebo deaktivace hello uživatelský účet v cílovém systému hello na základě porovnání hello</span><span class="sxs-lookup"><span data-stu-id="518f1-118">Adding, updating, or disabling hello user account in hello target system based on hello comparison</span></span>

## <a name="next-steps"></a><span data-ttu-id="518f1-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="518f1-119">Next steps</span></span>
<span data-ttu-id="518f1-120">[Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning):</span><span class="sxs-lookup"><span data-stu-id="518f1-120">[Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''</span></span>
