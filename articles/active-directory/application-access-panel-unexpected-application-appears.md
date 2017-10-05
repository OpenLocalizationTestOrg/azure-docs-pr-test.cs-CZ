---
title: "Jak aplikace se objeví na panel přístupu | Microsoft Docs"
description: "Poradce při potížích se aplikace se zobrazuje na přístupovém panelu"
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
ms.reviewr: japere
ms.openlocfilehash: f8ccf2cf66b49940bc7f2b9f4764020efc04838e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-applications-appear-on-the-access-panel"></a><span data-ttu-id="99b30-103">Jak aplikace se objeví na panel přístupu</span><span class="sxs-lookup"><span data-stu-id="99b30-103">How applications appear on the access panel</span></span>

<span data-ttu-id="99b30-104">Přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) k zobrazení a spustit cloudové aplikace, aby správce Azure AD má je přístup povolen.</span><span class="sxs-lookup"><span data-stu-id="99b30-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="99b30-105">Tyto aplikace jsou konfigurovány jménem uživatele na portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99b30-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="99b30-106">Správce můžete zřídit aplikaci na tohoto uživatele přímo nebo do skupiny uživatel je součástí výsledkem aplikace, které jsou uvedeny na Panel přístupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="99b30-106">The admin can provision the application to the user directly or to a group a user is part of resulting in the application appearing on the user’s Access Panel.</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="99b30-107">Běžné problémy a proveďte nejprve kontrolu</span><span class="sxs-lookup"><span data-stu-id="99b30-107">General issues to check first</span></span>

-   <span data-ttu-id="99b30-108">Pokud aplikace byla právě odebrána ze uživatele nebo skupiny, kterých je uživatel členem, zkuste pro přihlášení a odhlášení znovu do přístupového panelu uživatele po několika minutách zda je aplikace odebrat.</span><span class="sxs-lookup"><span data-stu-id="99b30-108">If an application was just removed from a user or group the user is a member of, try to sign in and out again into the user’s Access Panel after a few minutes to see if the application is removed.</span></span>

-   <span data-ttu-id="99b30-109">Pokud licence byla právě odebrána ze uživatele nebo skupinu je uživatel že členem skupiny to může trvat dlouhou dobu, v závislosti na velikost a složitost skupiny změny má být provedeno.</span><span class="sxs-lookup"><span data-stu-id="99b30-109">If a license was just removed from a user or group the user is a member of this may take a long time, depending on the size and complexity of the group for changes to be made.</span></span> <span data-ttu-id="99b30-110">Povolit další dobu před přihlášení k přístupovému panelu.</span><span class="sxs-lookup"><span data-stu-id="99b30-110">Allow for extra time before signing into the Access Panel.</span></span>

## <a name="problems-related-to-assigning-applications-to-users"></a><span data-ttu-id="99b30-111">Problémy související s přiřazení aplikací pro uživatele</span><span class="sxs-lookup"><span data-stu-id="99b30-111">Problems related to assigning applications to users</span></span>

<span data-ttu-id="99b30-112">Uživatel může být zobrazuje aplikace na jejich přístupový Panel měl byly dřív přiřazené k němu.</span><span class="sxs-lookup"><span data-stu-id="99b30-112">A user may be seeing an application on their Access Panel because they had been previously assigned to it.</span></span> <span data-ttu-id="99b30-113">Tady jsou některé způsoby, jak zkontrolovat:</span><span class="sxs-lookup"><span data-stu-id="99b30-113">Below are some ways to check:</span></span>

-   [<span data-ttu-id="99b30-114">Zkontrolujte, pokud uživatel je přiřazená k aplikaci</span><span class="sxs-lookup"><span data-stu-id="99b30-114">Check if a user is assigned to the application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="99b30-115">Zkontrolujte, jestli je uživatel v rámci licence týkající se aplikace</span><span class="sxs-lookup"><span data-stu-id="99b30-115">Check if a user is under a license related to the application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a><span data-ttu-id="99b30-116">Zkontrolujte, pokud uživatel je přiřazená k aplikaci</span><span class="sxs-lookup"><span data-stu-id="99b30-116">Check if a user is assigned to the application</span></span>

<span data-ttu-id="99b30-117">Pokud chcete zkontrolovat, pokud má uživatel přiřazeno k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="99b30-117">To check if a user is assigned to the application, follow the steps below:</span></span>

1.  <span data-ttu-id="99b30-118">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="99b30-118">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="99b30-119">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="99b30-119">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="99b30-120">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="99b30-120">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="99b30-121">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="99b30-121">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="99b30-122">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="99b30-122">click **All Applications** to view a list of all your applications.</span></span>

6.  <span data-ttu-id="99b30-123">**Hledání** pro název dané žádosti.</span><span class="sxs-lookup"><span data-stu-id="99b30-123">**Search** for the name of the application in question.</span></span>

7.  <span data-ttu-id="99b30-124">Klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="99b30-124">click **Users and groups**.</span></span>

8.  <span data-ttu-id="99b30-125">Zkontrolujte, pokud uživatel je přiřazena k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="99b30-125">Check to see if your user is assigned to the application.</span></span>

  * <span data-ttu-id="99b30-126">Pokud chcete odebrat uživatele z aplikace, **klikněte na řádek** uživatele a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="99b30-126">If you want to remove the user from the application, **click the row** of the user and select **delete**.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a><span data-ttu-id="99b30-127">Zkontrolujte, jestli je uživatel v rámci licence týkající se aplikace</span><span class="sxs-lookup"><span data-stu-id="99b30-127">Check if a user is under a license related to the application</span></span>

<span data-ttu-id="99b30-128">Pokud chcete zkontrolovat uživatele přiřazené licence, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="99b30-128">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="99b30-129">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="99b30-129">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="99b30-130">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="99b30-130">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="99b30-131">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="99b30-131">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="99b30-132">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="99b30-132">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="99b30-133">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="99b30-133">click **All users**.</span></span>

6.  <span data-ttu-id="99b30-134">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="99b30-134">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="99b30-135">Klikněte na tlačítko **licence** zobrazíte, které uživatel aktuálně licence jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="99b30-135">click **Licenses** to see which licenses the user currently has assigned.</span></span>

   * <span data-ttu-id="99b30-136">Pokud uživatel je přiřazen k Office licence touto aplikací povolit první strany Office se objevily na Panel přístupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="99b30-136">If the user is assigned to an Office license this enable First Party Office applications to appear on the user’s Access Panel.</span></span>

## <a name="problems-related-to-assigning-applications-to-groups"></a><span data-ttu-id="99b30-137">Problémy související s přiřazování aplikací do skupin</span><span class="sxs-lookup"><span data-stu-id="99b30-137">Problems related to assigning applications to groups</span></span>

<span data-ttu-id="99b30-138">Uživatele může být zobrazuje aplikace na jejich přístupového panelu jsou součástí skupiny, která byla přiřazena aplikace.</span><span class="sxs-lookup"><span data-stu-id="99b30-138">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned the application.</span></span> <span data-ttu-id="99b30-139">Tady jsou některé způsoby, jak zkontrolovat:</span><span class="sxs-lookup"><span data-stu-id="99b30-139">Below are some ways to check:</span></span>

-   [<span data-ttu-id="99b30-140">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="99b30-140">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="99b30-141">Zkontrolujte, jestli je uživatel členem skupiny přiřazené k licenci</span><span class="sxs-lookup"><span data-stu-id="99b30-141">Check if a user is a member of a group assigned to a license</span></span>](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="99b30-142">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="99b30-142">Check a user’s group memberships</span></span>

<span data-ttu-id="99b30-143">Pokud chcete zkontrolovat členství ve skupině, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="99b30-143">To check a group’s membership, follow the steps below:</span></span>

1.  <span data-ttu-id="99b30-144">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="99b30-144">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="99b30-145">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="99b30-145">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="99b30-146">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="99b30-146">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="99b30-147">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="99b30-147">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="99b30-148">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="99b30-148">click **All users**.</span></span>

6.  <span data-ttu-id="99b30-149">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="99b30-149">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="99b30-150">Klikněte na tlačítko **skupiny.**</span><span class="sxs-lookup"><span data-stu-id="99b30-150">click **Groups.**</span></span>

8.  <span data-ttu-id="99b30-151">Zkontrolujte, zda uživatel je součástí skupiny přiřazené k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="99b30-151">Check to see if your user is part of a Group assigned to the application.</span></span>

   * <span data-ttu-id="99b30-152">Pokud chcete odebrat uživatele ze skupiny, **klikněte na řádek** tuto skupinu a vyberte možnost odstranit.</span><span class="sxs-lookup"><span data-stu-id="99b30-152">If you want to remove the user from the group, **click the row** of the group and select delete.</span></span>

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a><span data-ttu-id="99b30-153">Zkontrolujte, jestli je uživatel členem skupiny přiřazené k licenci</span><span class="sxs-lookup"><span data-stu-id="99b30-153">Check if a user is a member of a group assigned to a license</span></span>

1.  <span data-ttu-id="99b30-154">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="99b30-154">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="99b30-155">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="99b30-155">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="99b30-156">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="99b30-156">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="99b30-157">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="99b30-157">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="99b30-158">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="99b30-158">click **All users**.</span></span>

6.  <span data-ttu-id="99b30-159">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="99b30-159">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="99b30-160">Klikněte na tlačítko **skupiny.**</span><span class="sxs-lookup"><span data-stu-id="99b30-160">click **Groups.**</span></span>

8.  <span data-ttu-id="99b30-161">Klikněte na řádek určité skupiny.</span><span class="sxs-lookup"><span data-stu-id="99b30-161">click the row of a specific group.</span></span>

9.  <span data-ttu-id="99b30-162">Klikněte na tlačítko **licence** zobrazíte, které skupiny licencí má přiřazen.</span><span class="sxs-lookup"><span data-stu-id="99b30-162">click **Licenses** to see which licenses the group has assigned to it.</span></span>

  * <span data-ttu-id="99b30-163">Pokud tato skupina je přiřazená k licenci Office to může povolit konkrétní aplikace Office první strany se objevily na Panel přístupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="99b30-163">If the group is assigned to an Office license this may enable certain First Party Office applications to appear on the user’s Access Panel.</span></span>


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a><span data-ttu-id="99b30-164">Pokud tyto kroky řešení potíží se není vyřešit problém</span><span class="sxs-lookup"><span data-stu-id="99b30-164">If these troubleshooting steps do not the resolve the issue</span></span>

<span data-ttu-id="99b30-165">Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="99b30-165">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="99b30-166">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="99b30-166">Correlation error ID</span></span>

-   <span data-ttu-id="99b30-167">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="99b30-167">UPN (user email address)</span></span>

-   <span data-ttu-id="99b30-168">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="99b30-168">Tenant ID</span></span>

-   <span data-ttu-id="99b30-169">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="99b30-169">Browser type</span></span>

-   <span data-ttu-id="99b30-170">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="99b30-170">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="99b30-171">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="99b30-171">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="99b30-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99b30-172">Next steps</span></span>
[<span data-ttu-id="99b30-173">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99b30-173">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
