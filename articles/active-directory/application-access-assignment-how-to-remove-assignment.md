---
title: "Postup odebrání přístupu uživatele k aplikaci | Microsoft Docs"
description: "Pochopit, jak k odebrání přístupu uživatele k aplikaci"
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
ms.openlocfilehash: 497429e7bf62f7e1d67ea429d6b858725f843688
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-remove-a-users-access-to-an-application"></a><span data-ttu-id="8ea87-103">Postup odebrání přístupu uživatele k aplikaci</span><span class="sxs-lookup"><span data-stu-id="8ea87-103">How to remove a user's access to an application</span></span>

<span data-ttu-id="8ea87-104">Tento článek vám pomůže lépe porozumět odebrání přístupu uživatele k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8ea87-104">This article help you to understand how to remove a user's access to an application.</span></span>

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a><span data-ttu-id="8ea87-105">Chcete odstranit přiřazení konkrétního uživatele nebo skupiny do aplikace</span><span class="sxs-lookup"><span data-stu-id="8ea87-105">I want to remove a specific user’s or group’s assignment to an application</span></span>

<span data-ttu-id="8ea87-106">Chcete-li odebrat uživatele nebo skupiny přiřazení k aplikaci, postupujte podle kroků uvedených v [odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) článku.</span><span class="sxs-lookup"><span data-stu-id="8ea87-106">To remove a user or group assignment to an application, follow the steps listed in the [Remove a user or group assignment from an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) article.</span></span>

<span data-ttu-id="8ea87-107">. ## Chcete zakázat veškerý přístup k aplikaci pro každého uživatele</span><span class="sxs-lookup"><span data-stu-id="8ea87-107">.## I want to disable all access to an application for every user</span></span>

<span data-ttu-id="8ea87-108">Pokud chcete zakázat všechna uživatelská přihlášení k aplikaci, postupujte podle kroků uvedených v [zakázat uživatelská přihlášení pro podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) článku.</span><span class="sxs-lookup"><span data-stu-id="8ea87-108">To disable all user sign-ins to an application, follow the steps listed in the [Disable user sign-ins for an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) article.</span></span>

## <a name="i-want-to-delete-an-application-entirely"></a><span data-ttu-id="8ea87-109">Chcete odstranit aplikaci zcela</span><span class="sxs-lookup"><span data-stu-id="8ea87-109">I want to delete an application entirely</span></span>

<span data-ttu-id="8ea87-110">K **odstranit aplikaci**, postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="8ea87-110">To **delete an application**, follow the instructions below:</span></span>

1.  <span data-ttu-id="8ea87-111">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="8ea87-111">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="8ea87-112">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="8ea87-112">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="8ea87-113">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="8ea87-113">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="8ea87-114">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8ea87-114">Click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="8ea87-115">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="8ea87-115">Click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="8ea87-116">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="8ea87-116">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="8ea87-117">Vyberte aplikaci, kterou chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="8ea87-117">Select the application you want to delete.</span></span>

7.  <span data-ttu-id="8ea87-118">Po načtení aplikace, klikněte na **odstranit** ikonu z hlavní aplikace **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="8ea87-118">Once the application loads, click **Delete** icon from the top application’s **Overview** blade.</span></span>

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a><span data-ttu-id="8ea87-119">Chcete zakázat všechny operace souhlasu budoucí uživatele do jakékoli aplikace</span><span class="sxs-lookup"><span data-stu-id="8ea87-119">I want to disable all future user consent operations to any application</span></span>

<span data-ttu-id="8ea87-120">Zakázání souhlas uživatele pro souhlas pro žádnou aplikaci koncovým uživatelům zabránit celý adresář.</span><span class="sxs-lookup"><span data-stu-id="8ea87-120">Disabling user consent for your entire directory prevent end users from consenting to any application.</span></span> <span data-ttu-id="8ea87-121">Správci mohou stále souhlas na behalves uživatele.</span><span class="sxs-lookup"><span data-stu-id="8ea87-121">Administrators can still consent on user’s behalves.</span></span> <span data-ttu-id="8ea87-122">Další informace o aplikaci souhlas, a proto může nebo nemusí Chcete to provést, číst [Principy uživatelů a správce souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span><span class="sxs-lookup"><span data-stu-id="8ea87-122">To learn more about application consent, and why you may or may not wish to do this, read [Understanding user and admin consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span></span>

<span data-ttu-id="8ea87-123">K **zakažte všechny operace souhlasu budoucí uživatele v adresáři celý**, postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="8ea87-123">To **disable all future user consent operations in your entire directory**, follow the instructions below:</span></span>

1.  <span data-ttu-id="8ea87-124">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="8ea87-124">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="8ea87-125">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="8ea87-125">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="8ea87-126">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="8ea87-126">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="8ea87-127">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="8ea87-127">Click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="8ea87-128">Klikněte na tlačítko **uživatelská nastavení**.</span><span class="sxs-lookup"><span data-stu-id="8ea87-128">Click **User settings**.</span></span>

6.  <span data-ttu-id="8ea87-129">Zakažte všechny budoucí uživatele souhlasu operace nastavením **uživatelé můžou aplikace přistupovat ke svým datům** přepnutím **ne** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ea87-129">Disable all future user consent operations by setting the **Users can allow apps to access their data** toggle to **No** and click the **Save** button.</span></span>


# <a name="next-steps"></a><span data-ttu-id="8ea87-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8ea87-130">Next steps</span></span>
[<span data-ttu-id="8ea87-131">Správa přístupu k aplikacím</span><span class="sxs-lookup"><span data-stu-id="8ea87-131">Managing access to apps</span></span>](active-directory-managing-access-to-apps.md)
