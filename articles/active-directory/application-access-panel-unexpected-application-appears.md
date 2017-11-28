---
title: "aaaHow aplikace se objeví na panel přístupu hello | Microsoft Docs"
description: "Poradce při potížích se aplikace se zobrazuje v hello přístupového panelu"
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
ms.openlocfilehash: 14ee732c4ed5260cba878e949cf9d90877aee67e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-applications-appear-on-hello-access-panel"></a><span data-ttu-id="ed1cb-103">Jak aplikace se objeví na panel přístupu hello</span><span class="sxs-lookup"><span data-stu-id="ed1cb-103">How applications appear on hello access panel</span></span>

<span data-ttu-id="ed1cb-104">Hello přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spusťte cloudové aplikace této hello správce Azure AD udělil je přístup k.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="ed1cb-105">Tyto aplikace jsou konfigurovány jménem uživatele hello na portálu Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="ed1cb-106">Dobrý den, správce můžete zřídit hello aplikace toohello uživatel přímo nebo tooa skupinu, kterou uživatel je součástí výsledkem aplikace hello zobrazovaných na Panel přístupu hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-106">hello admin can provision hello application toohello user directly or tooa group a user is part of resulting in hello application appearing on hello user’s Access Panel.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="ed1cb-107">Obecné problémy toocheck nejprve</span><span class="sxs-lookup"><span data-stu-id="ed1cb-107">General issues toocheck first</span></span>

-   <span data-ttu-id="ed1cb-108">Pokud aplikace právě odebral od uživatele nebo skupiny hello uživatel je členem skupiny, opakujte toosign a odhlašování do přístupového panelu hello uživatele po několika minutách toosee Pokud odebrání aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-108">If an application was just removed from a user or group hello user is a member of, try toosign in and out again into hello user’s Access Panel after a few minutes toosee if hello application is removed.</span></span>

-   <span data-ttu-id="ed1cb-109">Pokud licence byla právě odebrána ze uživatele nebo skupiny hello uživatel je že členem skupiny to může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny pro toobe změny provedené.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-109">If a license was just removed from a user or group hello user is a member of this may take a long time, depending on hello size and complexity of hello group for changes toobe made.</span></span> <span data-ttu-id="ed1cb-110">Povolit další dobu před přihlášením hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-110">Allow for extra time before signing into hello Access Panel.</span></span>

## <a name="problems-related-tooassigning-applications-toousers"></a><span data-ttu-id="ed1cb-111">Problémy související tooassigning aplikace toousers</span><span class="sxs-lookup"><span data-stu-id="ed1cb-111">Problems related tooassigning applications toousers</span></span>

<span data-ttu-id="ed1cb-112">Uživatel může být zobrazuje aplikace na jejich přístupového panelu jejich měl byly dřív přiřazené tooit.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-112">A user may be seeing an application on their Access Panel because they had been previously assigned tooit.</span></span> <span data-ttu-id="ed1cb-113">Tady jsou některé toocheck způsoby:</span><span class="sxs-lookup"><span data-stu-id="ed1cb-113">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="ed1cb-114">Zkontrolujte, pokud uživatel je přiřazená toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="ed1cb-114">Check if a user is assigned toohello application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="ed1cb-115">Zkontrolujte, jestli je uživatel v rámci licence související toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="ed1cb-115">Check if a user is under a license related toohello application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-toohello-application"></a><span data-ttu-id="ed1cb-116">Zkontrolujte, pokud uživatel je přiřazená toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="ed1cb-116">Check if a user is assigned toohello application</span></span>

<span data-ttu-id="ed1cb-117">toocheck Pokud uživatel přiřazený toohello aplikace, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="ed1cb-117">toocheck if a user is assigned toohello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="ed1cb-118">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="ed1cb-118">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="ed1cb-119">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-119">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="ed1cb-120">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-120">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="ed1cb-121">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-121">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="ed1cb-122">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-122">click **All Applications** tooview a list of all your applications.</span></span>

6.  <span data-ttu-id="ed1cb-123">**Hledání** pro název hello aplikace hello nejistá.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-123">**Search** for hello name of hello application in question.</span></span>

7.  <span data-ttu-id="ed1cb-124">Klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-124">click **Users and groups**.</span></span>

8.  <span data-ttu-id="ed1cb-125">Toosee zkontrolujte, zda váš uživatel toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-125">Check toosee if your user is assigned toohello application.</span></span>

  * <span data-ttu-id="ed1cb-126">Pokud chcete, aby uživatel hello tooremove z aplikace hello **klikněte na řádek hello** hello uživatele a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-126">If you want tooremove hello user from hello application, **click hello row** of hello user and select **delete**.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a><span data-ttu-id="ed1cb-127">Zkontrolujte, jestli je uživatel v rámci licence související toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="ed1cb-127">Check if a user is under a license related toohello application</span></span>

<span data-ttu-id="ed1cb-128">toocheck uživatele je přiřazena licence, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="ed1cb-128">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="ed1cb-129">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="ed1cb-129">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="ed1cb-130">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-130">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="ed1cb-131">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-131">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="ed1cb-132">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-132">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="ed1cb-133">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-133">click **All users**.</span></span>

6.  <span data-ttu-id="ed1cb-134">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-134">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="ed1cb-135">Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-135">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

   * <span data-ttu-id="ed1cb-136">Pokud uživatel hello přiřazené tooan Office licence tato povolí tooappear aplikace první strany Office na Panel přístupu hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-136">If hello user is assigned tooan Office license this enable First Party Office applications tooappear on hello user’s Access Panel.</span></span>

## <a name="problems-related-tooassigning-applications-toogroups"></a><span data-ttu-id="ed1cb-137">Problémy související tooassigning aplikace toogroups</span><span class="sxs-lookup"><span data-stu-id="ed1cb-137">Problems related tooassigning applications toogroups</span></span>

<span data-ttu-id="ed1cb-138">Uživatele může být zobrazuje aplikace na jejich přístupového panelu jsou součástí skupiny, která byla přiřazena aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-138">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned hello application.</span></span> <span data-ttu-id="ed1cb-139">Tady jsou některé toocheck způsoby:</span><span class="sxs-lookup"><span data-stu-id="ed1cb-139">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="ed1cb-140">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="ed1cb-140">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="ed1cb-141">Zkontrolujte, zda uživatel je členem skupiny přiřazené licence tooa</span><span class="sxs-lookup"><span data-stu-id="ed1cb-141">Check if a user is a member of a group assigned tooa license</span></span>](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="ed1cb-142">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="ed1cb-142">Check a user’s group memberships</span></span>

<span data-ttu-id="ed1cb-143">toocheck členství ve skupině, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="ed1cb-143">toocheck a group’s membership, follow hello steps below:</span></span>

1.  <span data-ttu-id="ed1cb-144">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="ed1cb-144">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="ed1cb-145">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-145">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="ed1cb-146">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-146">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="ed1cb-147">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-147">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="ed1cb-148">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-148">click **All users**.</span></span>

6.  <span data-ttu-id="ed1cb-149">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-149">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="ed1cb-150">Klikněte na tlačítko **skupiny.**</span><span class="sxs-lookup"><span data-stu-id="ed1cb-150">click **Groups.**</span></span>

8.  <span data-ttu-id="ed1cb-151">Zkontrolujte toosee, pokud uživatel je součástí aplikace toohello skupiny přiřazené.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-151">Check toosee if your user is part of a Group assigned toohello application.</span></span>

   * <span data-ttu-id="ed1cb-152">Pokud chcete, aby tooremove hello uživatele ze skupiny hello **klikněte na řádek hello** hello skupiny a vyberte možnost odstranit.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-152">If you want tooremove hello user from hello group, **click hello row** of hello group and select delete.</span></span>

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-tooa-license"></a><span data-ttu-id="ed1cb-153">Zkontrolujte, zda uživatel je členem skupiny přiřazené licence tooa</span><span class="sxs-lookup"><span data-stu-id="ed1cb-153">Check if a user is a member of a group assigned tooa license</span></span>

1.  <span data-ttu-id="ed1cb-154">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="ed1cb-154">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="ed1cb-155">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-155">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="ed1cb-156">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-156">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="ed1cb-157">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-157">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="ed1cb-158">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-158">click **All users**.</span></span>

6.  <span data-ttu-id="ed1cb-159">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-159">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="ed1cb-160">Klikněte na tlačítko **skupiny.**</span><span class="sxs-lookup"><span data-stu-id="ed1cb-160">click **Groups.**</span></span>

8.  <span data-ttu-id="ed1cb-161">Klikněte na řádek hello určité skupiny.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-161">click hello row of a specific group.</span></span>

9.  <span data-ttu-id="ed1cb-162">Klikněte na tlačítko **licence** toosee, které skupiny licencí hello přiřazenému tooit.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-162">click **Licenses** toosee which licenses hello group has assigned tooit.</span></span>

  * <span data-ttu-id="ed1cb-163">Pokud skupina hello přiřazené tooan Office licence to může povolit určité aplikace tooappear první strany Office na Panel přístupu hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ed1cb-163">If hello group is assigned tooan Office license this may enable certain First Party Office applications tooappear on hello user’s Access Panel.</span></span>


## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="ed1cb-164">Pokud tento postup řešení není hello hello problém vyřešte</span><span class="sxs-lookup"><span data-stu-id="ed1cb-164">If these troubleshooting steps do not hello resolve hello issue</span></span>

<span data-ttu-id="ed1cb-165">Otevřete lístek podpory se hello, pokud je k dispozici následující informace:</span><span class="sxs-lookup"><span data-stu-id="ed1cb-165">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="ed1cb-166">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="ed1cb-166">Correlation error ID</span></span>

-   <span data-ttu-id="ed1cb-167">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="ed1cb-167">UPN (user email address)</span></span>

-   <span data-ttu-id="ed1cb-168">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="ed1cb-168">Tenant ID</span></span>

-   <span data-ttu-id="ed1cb-169">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="ed1cb-169">Browser type</span></span>

-   <span data-ttu-id="ed1cb-170">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="ed1cb-170">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="ed1cb-171">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="ed1cb-171">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed1cb-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed1cb-172">Next steps</span></span>
[<span data-ttu-id="ed1cb-173">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ed1cb-173">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
