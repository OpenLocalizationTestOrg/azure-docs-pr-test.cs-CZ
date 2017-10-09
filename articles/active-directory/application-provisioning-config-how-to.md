---
title: "Zřizování aplikace Galerie tooan Azure AD uživatelů tooconfigure aaaHow | Microsoft Docs"
description: "Jak můžete snadno konfigurovat bohaté uživatelský účet zajišťování a rušení zajištění tooapplications již uveden v hello Azure AD Application Gallery"
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
ms.openlocfilehash: 2c28e59a3ac8f221ed93b2f6b0b1221f7604af23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-user-provisioning-tooan-azure-ad-gallery-application"></a><span data-ttu-id="2e976-103">Jak uživatel tooconfigure zřizování tooan Azure AD application Gallery</span><span class="sxs-lookup"><span data-stu-id="2e976-103">How tooconfigure user provisioning tooan Azure AD Gallery application</span></span>

<span data-ttu-id="2e976-104">*Zřizování účtu uživatele* spočívá hello vytváření, aktualizaci nebo zakázání záznamů účet uživatele v úložišti profil místního uživatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e976-104">*User account provisioning* is hello act of creating, updating, and/or disabling user account records in an application’s local user profile store.</span></span> <span data-ttu-id="2e976-105">Většina cloudu a aplikace SaaS ukládání hello role uživatele a oprávnění v profilu úložiště vlastní místní uživatele a přítomnost takové uživatelský záznam v jejich místní úložiště je *požadované* pro jeden toowork přihlašování a přístupu.</span><span class="sxs-lookup"><span data-stu-id="2e976-105">Most cloud and SaaS applications store hello users role and permissions in their own local user profile store, and presence of such a user record in their local store is *required* for single sign-on and access toowork.</span></span>

<span data-ttu-id="2e976-106">V hello portálu Azure, hello **zřizování** klikněte v levém navigačním podokně hello pro podnikové aplikace zobrazuje, jaké zřizování režimy jsou podporovány pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e976-106">In hello Azure portal, hello **Provisioning** tab in hello left navigation pane for an Enterprise App displays what provisioning modes are supported for that app.</span></span> <span data-ttu-id="2e976-107">To může být jednu ze dvou hodnot:</span><span class="sxs-lookup"><span data-stu-id="2e976-107">This can be one of two values:</span></span>

## <a name="configuring-an-application-for-manual-provisioning"></a><span data-ttu-id="2e976-108">Konfigurace aplikace pro ruční zřizování</span><span class="sxs-lookup"><span data-stu-id="2e976-108">Configuring an application for Manual Provisioning</span></span>

<span data-ttu-id="2e976-109">*Ruční* zřizování znamená, že uživatelské účty musí být vytvořen ručně pomocí metody hello poskytované tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e976-109">*Manual* provisioning means that user accounts must be created manually using hello methods provided by that app.</span></span> <span data-ttu-id="2e976-110">To může znamenat přihlašování portálu pro správu pro tuto aplikaci a přidání uživatelů pomocí webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2e976-110">This could mean logging into an administrative portal for that app and adding users using a web-based user interface.</span></span> <span data-ttu-id="2e976-111">Nebo může být odesílání tabulku s podrobnostmi účet uživatele, používá mechanismus poskytované tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e976-111">Or it could be uploading a spreadsheet with user account detail, using a mechanism provided by that application.</span></span> <span data-ttu-id="2e976-112">Zadaný hello aplikací nebo aplikace kontaktní hello vývojáře toodetermine wat mechanismy jsou dostupné dokumentaci hello.</span><span class="sxs-lookup"><span data-stu-id="2e976-112">Consult hello documentation provided by hello app, or contact hello app developer toodetermine wat mechanisms are available.</span></span>

<span data-ttu-id="2e976-113">Pokud ruční režim pouze hello zobrazí se pro danou aplikaci, znamená to, že žádné automatické Azure AD zřizování konektor zatím nebyla vytvořena pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="2e976-113">If Manual is hello only mode shown for a given application, it means that no automatic Azure AD provisioning connector has been created for hello app yet.</span></span> <span data-ttu-id="2e976-114">Nebo znamená to, že aplikace hello nemá podpora hello předběžné uživatelské rozhraní API pro správu při které toobuild konektoru automatické zřizování.</span><span class="sxs-lookup"><span data-stu-id="2e976-114">Or it means hello app does not support hello pre-requisite user management API upon which toobuild an automated provisioning connector.</span></span>

<span data-ttu-id="2e976-115">Pokud chcete toorequest podporu pro automatické zřizování pro danou aplikaci, můžete vyplnit požadavek na <http://aka.ms/aadapprequest>.</span><span class="sxs-lookup"><span data-stu-id="2e976-115">If you would like toorequest support for automatic provisioning for a given app, you can fill out a request at <http://aka.ms/aadapprequest>.</span></span>

## <a name="configuring-an-application-for-automatic-provisioning"></a><span data-ttu-id="2e976-116">Konfigurace aplikace pro automatické zřizování</span><span class="sxs-lookup"><span data-stu-id="2e976-116">Configuring an application for Automatic Provisioning</span></span>

<span data-ttu-id="2e976-117">*Automatické* znamená, že byla vyvinuta Azure AD zřizování konektor pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e976-117">*Automatic* means that an Azure AD provisioning connector has been developed for this application.</span></span> <span data-ttu-id="2e976-118">Další informace o hello Azure AD zřizování služby a jak to funguje, najdete v části [automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span><span class="sxs-lookup"><span data-stu-id="2e976-118">For more information on hello Azure AD provisioning service and how it works, see [Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span></span>

<span data-ttu-id="2e976-119">Další informace o tom, jak tooprovision konkrétní uživatele a skupiny tooan aplikace, najdete v části [Správa zřizování účtu uživatele pro podnikové aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span><span class="sxs-lookup"><span data-stu-id="2e976-119">For more information on how tooprovision specific users and groups tooan application, see [Managing user account provisioning for enterprise apps](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span></span>

<span data-ttu-id="2e976-120">Hello skutečné kroky požadované tooenable a nakonfigurovat automatické zřizování lišit v závislosti na aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="2e976-120">hello actual steps required tooenable and configure automatic provisioning vary depending on hello application.</span></span>

>[!NOTE]
><span data-ttu-id="2e976-121">Byste měli začít hledání hello kurz konkrétní toosetting instalace si zřizování pro vaši aplikaci a pomocí těchto kroků tooconfigure aplikace hello i Azure AD toocreate hello zřizování připojení.</span><span class="sxs-lookup"><span data-stu-id="2e976-121">You should start by finding hello setup tutorial specific toosetting up provisioning for your application, and following those steps tooconfigure both hello app and Azure AD toocreate hello provisioning connection.</span></span> 
>
>

<span data-ttu-id="2e976-122">Kurzy aplikace naleznete na adrese [seznamu kurzy o tooIntegrate SaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span><span class="sxs-lookup"><span data-stu-id="2e976-122">App tutorials can be found at [List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span></span>

<span data-ttu-id="2e976-123">Tooconsider důležité při nastavování zřizování být tooreview a konfigurace mapování atributů hello a pracovní postupy, které definují vlastnosti toku z Azure AD toohello aplikace které uživatele (nebo skupiny).</span><span class="sxs-lookup"><span data-stu-id="2e976-123">An important thing tooconsider when setting up provisioning be tooreview and configure hello attribute mappings and workflows that define which user (or group) properties flow from Azure AD toohello application.</span></span> <span data-ttu-id="2e976-124">To zahrnuje nastavení hello "odpovídající vlastnost", který se použité toouniquely identifikovat a odpovídají uživatele nebo skupiny mezi hello dvěma systémy.</span><span class="sxs-lookup"><span data-stu-id="2e976-124">This includes setting hello “matching property” that be used toouniquely identify and match users/groups between hello two systems.</span></span> <span data-ttu-id="2e976-125">Další informace o tomto důležité procesu.</span><span class="sxs-lookup"><span data-stu-id="2e976-125">For more information on this important process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e976-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e976-126">Next steps</span></span>
[<span data-ttu-id="2e976-127">Přizpůsobení mapování atributů zřizování pro aplikace SaaS ve službě Azure Active Directory uživatelů</span><span class="sxs-lookup"><span data-stu-id="2e976-127">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

