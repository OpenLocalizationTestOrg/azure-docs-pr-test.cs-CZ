---
title: "Postup konfigurace zřizování uživatelů k aplikaci Galerie Azure AD | Microsoft Docs"
description: "Jak můžete snadno konfigurovat bohaté uživatelský účet zajišťování a rušení zajištění aplikací již uveden v galerii aplikací Azure AD"
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
ms.openlocfilehash: 2e38fcb30ea7632339a3ba8815a536872cfcc69e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-user-provisioning-to-an-azure-ad-gallery-application"></a><span data-ttu-id="28df9-103">Postup konfigurace zřizování uživatelů k aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="28df9-103">How to configure user provisioning to an Azure AD Gallery application</span></span>

<span data-ttu-id="28df9-104">*Zřizování účtu uživatele* je úkon, vytváření, aktualizaci nebo zakázání záznamů účet uživatele v úložišti profil místního uživatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="28df9-104">*User account provisioning* is the act of creating, updating, and/or disabling user account records in an application’s local user profile store.</span></span> <span data-ttu-id="28df9-105">Většina cloudu a aplikace SaaS úložiště rolí uživatelů a oprávnění v profilu úložiště vlastní místní uživatele a přítomnost takové uživatelský záznam v jejich místní úložiště je *požadované* pro jednotné přihlašování a přístup k práci.</span><span class="sxs-lookup"><span data-stu-id="28df9-105">Most cloud and SaaS applications store the users role and permissions in their own local user profile store, and presence of such a user record in their local store is *required* for single sign-on and access to work.</span></span>

<span data-ttu-id="28df9-106">Na portálu Azure **zřizování** klikněte v levém navigačním podokně pro podnikové aplikace zobrazuje, jaké zřizování režimy jsou podporovány pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="28df9-106">In the Azure portal, the **Provisioning** tab in the left navigation pane for an Enterprise App displays what provisioning modes are supported for that app.</span></span> <span data-ttu-id="28df9-107">To může být jednu ze dvou hodnot:</span><span class="sxs-lookup"><span data-stu-id="28df9-107">This can be one of two values:</span></span>

## <a name="configuring-an-application-for-manual-provisioning"></a><span data-ttu-id="28df9-108">Konfigurace aplikace pro ruční zřizování</span><span class="sxs-lookup"><span data-stu-id="28df9-108">Configuring an application for Manual Provisioning</span></span>

<span data-ttu-id="28df9-109">*Ruční* zřizování znamená, že uživatelské účty musí být vytvořen ručně pomocí metody poskytované tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="28df9-109">*Manual* provisioning means that user accounts must be created manually using the methods provided by that app.</span></span> <span data-ttu-id="28df9-110">To může znamenat přihlašování portálu pro správu pro tuto aplikaci a přidání uživatelů pomocí webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="28df9-110">This could mean logging into an administrative portal for that app and adding users using a web-based user interface.</span></span> <span data-ttu-id="28df9-111">Nebo může být odesílání tabulku s podrobnostmi účet uživatele, používá mechanismus poskytované tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="28df9-111">Or it could be uploading a spreadsheet with user account detail, using a mechanism provided by that application.</span></span> <span data-ttu-id="28df9-112">Vyhledejte v dokumentaci aplikace, nebo se obraťte na vývojáři aplikace k určení, že wat mechanismy jsou dostupné.</span><span class="sxs-lookup"><span data-stu-id="28df9-112">Consult the documentation provided by the app, or contact the app developer to determine wat mechanisms are available.</span></span>

<span data-ttu-id="28df9-113">Pokud ručně režimu jen pro zobrazí pro danou aplikaci, znamená to, že žádné automatické Azure AD zřizování konektor zatím nebyla vytvořena pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="28df9-113">If Manual is the only mode shown for a given application, it means that no automatic Azure AD provisioning connector has been created for the app yet.</span></span> <span data-ttu-id="28df9-114">Nebo znamená to, že aplikace nepodporuje rozhraní API správy požadovaného uživatele, na kterém k vytvoření konektoru služby Automatické zřizování.</span><span class="sxs-lookup"><span data-stu-id="28df9-114">Or it means the app does not support the pre-requisite user management API upon which to build an automated provisioning connector.</span></span>

<span data-ttu-id="28df9-115">Pokud chcete požádat o podporu pro automatické zřizování pro danou aplikaci, můžete vyplnit požadavek na <http://aka.ms/aadapprequest>.</span><span class="sxs-lookup"><span data-stu-id="28df9-115">If you would like to request support for automatic provisioning for a given app, you can fill out a request at <http://aka.ms/aadapprequest>.</span></span>

## <a name="configuring-an-application-for-automatic-provisioning"></a><span data-ttu-id="28df9-116">Konfigurace aplikace pro automatické zřizování</span><span class="sxs-lookup"><span data-stu-id="28df9-116">Configuring an application for Automatic Provisioning</span></span>

<span data-ttu-id="28df9-117">*Automatické* znamená, že byla vyvinuta Azure AD zřizování konektor pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="28df9-117">*Automatic* means that an Azure AD provisioning connector has been developed for this application.</span></span> <span data-ttu-id="28df9-118">Další informace o Azure AD zřizování služby a jak to funguje, najdete v části [automatizace zřizování uživatelů a jeho rušení pro aplikace SaaS ve službě Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span><span class="sxs-lookup"><span data-stu-id="28df9-118">For more information on the Azure AD provisioning service and how it works, see [Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span></span>

<span data-ttu-id="28df9-119">Další informace o tom, jak zřídit konkrétních uživatelů a skupin k aplikaci najdete v tématu [Správa zřizování účtu uživatele pro podnikové aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span><span class="sxs-lookup"><span data-stu-id="28df9-119">For more information on how to provision specific users and groups to an application, see [Managing user account provisioning for enterprise apps](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span></span>

<span data-ttu-id="28df9-120">Skutečné kroky nutné k povolení a konfigurace automatické zřizování se liší v závislosti na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="28df9-120">The actual steps required to enable and configure automatic provisioning vary depending on the application.</span></span>

>[!NOTE]
><span data-ttu-id="28df9-121">Měli byste začít tak, že instalační program kurzu najdete konkrétní nastavení zřizování pro aplikace a následující ty kroky při konfiguraci aplikace a Azure AD k vytvoření zřizování připojení.</span><span class="sxs-lookup"><span data-stu-id="28df9-121">You should start by finding the setup tutorial specific to setting up provisioning for your application, and following those steps to configure both the app and Azure AD to create the provisioning connection.</span></span> 
>
>

<span data-ttu-id="28df9-122">Kurzy aplikace naleznete na adrese [seznamu kurzy o tom, jak integrovat SaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span><span class="sxs-lookup"><span data-stu-id="28df9-122">App tutorials can be found at [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span></span>

<span data-ttu-id="28df9-123">Důležité vzít v úvahu při nastavování zřizování být zkontrolujte a nakonfigurujte mapování atributů a pracovních postupů, které definují, které uživatele (nebo skupiny) vlastnosti toku z Azure AD k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="28df9-123">An important thing to consider when setting up provisioning be to review and configure the attribute mappings and workflows that define which user (or group) properties flow from Azure AD to the application.</span></span> <span data-ttu-id="28df9-124">To zahrnuje nastavení "odpovídající vlastnost", které použít k jednoznačné identifikaci a odpovídají uživatele nebo skupiny mezi těmito dvěma systémy.</span><span class="sxs-lookup"><span data-stu-id="28df9-124">This includes setting the “matching property” that be used to uniquely identify and match users/groups between the two systems.</span></span> <span data-ttu-id="28df9-125">Další informace o tomto důležité procesu.</span><span class="sxs-lookup"><span data-stu-id="28df9-125">For more information on this important process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28df9-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28df9-126">Next steps</span></span>
[<span data-ttu-id="28df9-127">Přizpůsobení mapování atributů zřizování pro aplikace SaaS ve službě Azure Active Directory uživatelů</span><span class="sxs-lookup"><span data-stu-id="28df9-127">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

