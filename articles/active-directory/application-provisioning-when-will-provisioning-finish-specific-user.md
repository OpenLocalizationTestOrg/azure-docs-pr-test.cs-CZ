---
title: "Podívejte se, kdy budou mít přístup k aplikaci konkrétního uživatele | Microsoft Docs"
description: "Jak zjistit, kdy je zásadní význam uživatel bude moci získat přístup k aplikaci, kterou jste nakonfigurovali pro zřizování uživatelů s Azure AD"
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
ms.openlocfilehash: fcefb31904cfb77022db0358e9feee6a0479db81
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-to-access-an-application"></a><span data-ttu-id="38d11-103">Zjistit, kdy konkrétní uživatel bude mít přístup k aplikaci</span><span class="sxs-lookup"><span data-stu-id="38d11-103">Find out when a specific user will be able to access an application</span></span>
<span data-ttu-id="38d11-104">Při použití zřizování automatické uživatelů s aplikací, Azure AD automaticky zřizovat a aktualizace uživatelské účty v aplikaci na základě třeba [přiřazení uživatelů a skupin](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) v pravidelném plánovaném časovém intervalu, obvykle každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="38d11-104">When using automatic user provisioning with an application, Azure AD automatically provision and update user accounts in an app based on things like [user and group assignment](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) at a regularly scheduled time interval, typically every 10 minutes.</span></span>

## <a name="how-long-does-it-take"></a><span data-ttu-id="38d11-105">Jak dlouho to trvá?</span><span class="sxs-lookup"><span data-stu-id="38d11-105">How long does it take?</span></span>

<span data-ttu-id="38d11-106">Čas potřebný pro daného uživatele zřídit závisí hlavně na tom, jestli počáteční synchronizaci "úplná" již došlo k.</span><span class="sxs-lookup"><span data-stu-id="38d11-106">The time it takes for a given user to be provisioned depends mainly on whether or not an initial “full” sync has already occurred.</span></span>

<span data-ttu-id="38d11-107">První synchronizace mezi službou Azure AD a aplikace může trvat od 20 minut několik hodin v závislosti na velikosti adresáře služby Azure AD a počet uživatelů v oboru pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="38d11-107">The first sync between Azure AD and an app can take anywhere from 20 minutes to several hours, depending on the size of the Azure AD directory and the number of users in scope for provisioning.</span></span> 

<span data-ttu-id="38d11-108">Následné synchronizace po počáteční synchronizace být rychlejší (např. do 10 minut), jako službu zřizování ukládá vodoznaky, které představují stav obou systémů po počáteční synchronizaci, zvýšení výkonu následné synchronizace.</span><span class="sxs-lookup"><span data-stu-id="38d11-108">Subsequent syncs after the initial sync be faster (e.g. within 10 minutes), as the provisioning service stores watermarks that represent the state of both systems after the initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-to-check-the-status-of-a-user"></a><span data-ttu-id="38d11-109">Jak zkontrolovat stav uživatele</span><span class="sxs-lookup"><span data-stu-id="38d11-109">How to check the status of a user</span></span>

<span data-ttu-id="38d11-110">A zjistit stav zřizování pro vybraného uživatele, najdete v protokolech auditu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38d11-110">To see the provisioning status for a selected user, consult the Audit logs in Azure AD.</span></span>

<span data-ttu-id="38d11-111">Zřizování protokolů auditu na portálu Azure v přístupné **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt; protokolech auditování** kartě.</span><span class="sxs-lookup"><span data-stu-id="38d11-111">The provisioning audit logs can be accessed in the Azure portal, in the **Azure Active Directory &gt; Enterprise Apps &gt; \[Application Name\] &gt; Audit Logs** tab.</span></span> <span data-ttu-id="38d11-112">Filtrovat protokoly **zřizování účtu** kategorii zobrazíte jen zřizování události pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="38d11-112">Filter the logs on the **Account Provisioning** category to only see the provisioning events for that app.</span></span> <span data-ttu-id="38d11-113">Můžete vyhledat uživatele podle "Odpovídající ID" nakonfigurovaný pro ně v mapování atributů.</span><span class="sxs-lookup"><span data-stu-id="38d11-113">You can search for users based on the “matching ID” that was configured for them in the attribute mappings.</span></span> 

<span data-ttu-id="38d11-114">Například, pokud jste nakonfigurovali "hlavní název uživatele" nebo "e-mailovou adresu" jako odpovídající atribut na straně Azure AD a uživatel není zřizování má hodnotu "audrey@contoso.com", pak vyhledejte v protokolech auditu "audrey@contoso.com" a kontrola a vrácena žádná položka.</span><span class="sxs-lookup"><span data-stu-id="38d11-114">For example, if you configured the “user principal name” or “email address” as the matching attribute on the Azure AD side, and the user not being provisioning has a value of “audrey@contoso.com”, then search the audit logs for “audrey@contoso.com” and review then entries returned.</span></span>

<span data-ttu-id="38d11-115">Zřizování protokoly auditu zaznamenejte všechny operace, které provádí službu zřizování, včetně:</span><span class="sxs-lookup"><span data-stu-id="38d11-115">The provisioning audit logs record all the operations performed by the provisioning service, including:</span></span>

* <span data-ttu-id="38d11-116">Dotazování Azure AD pro přiřazené uživatele, které jsou v oboru pro zřizování</span><span class="sxs-lookup"><span data-stu-id="38d11-116">Querying Azure AD for assigned users that are in scope for provisioning</span></span>
* <span data-ttu-id="38d11-117">Dotaz na cílové aplikace existenci těchto uživatelů</span><span class="sxs-lookup"><span data-stu-id="38d11-117">Querying the target app for the existence of those users</span></span>
* <span data-ttu-id="38d11-118">Porovnávání uživatelských objektů mezi systémem</span><span class="sxs-lookup"><span data-stu-id="38d11-118">Comparing the user objects between the system</span></span>
* <span data-ttu-id="38d11-119">Přidání, aktualizace nebo deaktivace uživatelský účet v cílovém systému podle porovnání</span><span class="sxs-lookup"><span data-stu-id="38d11-119">Adding, updating, or disabling the user account in the target system based on the comparison</span></span>

## <a name="next-steps"></a><span data-ttu-id="38d11-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38d11-120">Next steps</span></span>
<span data-ttu-id="38d11-121">[Automatizovat uživatele zajišťování a rušení zajištění pro aplikace SaaS ve službě Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning):</span><span class="sxs-lookup"><span data-stu-id="38d11-121">[Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''</span></span>
