---
title: "aaaHow tooassign uživatelů a skupin tooan aplikace | Microsoft Docs"
description: "Přiřazení uživatelů toohello aplikace toogrant přístup"
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
ms.openlocfilehash: e039a26e4b8f88ad747354859f1071b8f74b6789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-and-groups-tooan-application"></a><span data-ttu-id="f919b-103">Jak aplikace tooan tooassign uživatelů a skupin</span><span class="sxs-lookup"><span data-stu-id="f919b-103">How tooassign users and groups tooan application</span></span>

<span data-ttu-id="f919b-104">Než mohou uživatelé provádět žádné hello pod pro konkrétní aplikaci, musíte toofirst **přiřaďte jim toohello aplikace** toogrant je přístup:</span><span class="sxs-lookup"><span data-stu-id="f919b-104">Before your users can do any of hello below for a specific application, you need toofirst **assign them toohello application** toogrant them access:</span></span>

-   <span data-ttu-id="f919b-105">Přístup k aplikaci pomocí **přejdete přímo adresu URL aplikace toohello** (také označované jako spouštěná SP přihlášení).</span><span class="sxs-lookup"><span data-stu-id="f919b-105">Access an application by **navigating toohello application’s URL directly** (also known as SP-initiated sign-on).</span></span>

-   <span data-ttu-id="f919b-106">Přístup k aplikaci pomocí hello **adresu URL pro přístup uživatelů** na aplikace **vlastnosti** stránky (také označované jako spouštěná IDP přihlašování).</span><span class="sxs-lookup"><span data-stu-id="f919b-106">Access an application by using hello **User Access URL** on an application’s **Properties** page (also known as IDP-initiated sign on).</span></span>

-   <span data-ttu-id="f919b-107">V tématu aplikace se zobrazují v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/) nebo mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="f919b-107">See an application appear on their [Application Access Panel](https://myapps.microsoft.com/) or mobile application.</span></span>

-   <span data-ttu-id="f919b-108">V tématu aplikace se zobrazují v jejich [Spouštěč aplikace Office 365](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span><span class="sxs-lookup"><span data-stu-id="f919b-108">See an application appear on their [Office 365 Application Launcher](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span></span>

## <a name="methods-tooassign-applications-with-azure-active-directory"></a><span data-ttu-id="f919b-109">Metody tooassign aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f919b-109">Methods tooassign applications with Azure Active Directory</span></span> 

<span data-ttu-id="f919b-110">Aplikace s Azure Active Directory můžete přiřadit 3 způsoby:</span><span class="sxs-lookup"><span data-stu-id="f919b-110">There are 3 ways you can assign applications with Azure Active Directory:</span></span>

-   [<span data-ttu-id="f919b-111">Přiřazení uživatele přímo tooan aplikace jako správce</span><span class="sxs-lookup"><span data-stu-id="f919b-111">Assign a user directly tooan application as an administrator</span></span>](#assign-a-user-directly-as-an-administrator)

-   [<span data-ttu-id="f919b-112">Přiřadit ke skupině přímo tooan aplikace jako správce</span><span class="sxs-lookup"><span data-stu-id="f919b-112">Assign a group directly tooan application as an administrator</span></span>](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [<span data-ttu-id="f919b-113">Povolit aplikaci Samoobslužné služby přístup tooallow uživatelé toofind svých vlastních aplikacích</span><span class="sxs-lookup"><span data-stu-id="f919b-113">Enable self-service application access tooallow users toofind their own applications</span></span>](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a><span data-ttu-id="f919b-114">Přiřazení uživatele přímo jako správce</span><span class="sxs-lookup"><span data-stu-id="f919b-114">Assign a user directly as an administrator</span></span>

<span data-ttu-id="f919b-115">tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="f919b-115">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="f919b-116">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="f919b-116">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f919b-117">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="f919b-117">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f919b-118">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f919b-118">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f919b-119">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f919b-119">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f919b-120">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="f919b-120">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="f919b-121">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="f919b-121">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="f919b-122">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="f919b-122">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="f919b-123">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f919b-123">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f919b-124">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="f919b-124">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="f919b-125">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="f919b-125">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="f919b-126">Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="f919b-126">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="f919b-127">Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="f919b-127">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="f919b-128">Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f919b-128">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="f919b-129">**Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f919b-129">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="f919b-130">Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f919b-130">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="f919b-131">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="f919b-131">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="f919b-132">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f919b-132">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="f919b-133">Po krátké době být hello uživatelů, které jste vybrali možnost toolaunch hello tyto aplikace pomocí metody popsané v části popis řešení hello.</span><span class="sxs-lookup"><span data-stu-id="f919b-133">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="assign-a-group-directly-tooan-application-as-an-administrator"></a><span data-ttu-id="f919b-134">Přiřadit ke skupině přímo tooan aplikace jako správce</span><span class="sxs-lookup"><span data-stu-id="f919b-134">Assign a group directly tooan application as an administrator</span></span>

<span data-ttu-id="f919b-135">tooassign jeden nebo více skupin tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="f919b-135">tooassign one or more groups tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="f919b-136">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="f919b-136">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f919b-137">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="f919b-137">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f919b-138">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f919b-138">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f919b-139">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f919b-139">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f919b-140">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="f919b-140">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="f919b-141">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="f919b-141">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="f919b-142">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="f919b-142">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="f919b-143">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f919b-143">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f919b-144">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="f919b-144">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="f919b-145">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="f919b-145">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="f919b-146">Typ v hello **název úplné skupiny** hello skupiny, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="f919b-146">Type in hello **full group name** of hello group you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="f919b-147">Pozastavte ukazatel myši nad hello **skupiny** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="f919b-147">Hover over hello **group** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="f919b-148">Klikněte na tlačítko hello políčko další toohello skupiny profilu fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f919b-148">Click hello checkbox next toohello group’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="f919b-149">**Volitelné:** Pokud byste chtěli příliš**přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole a klikněte na zaškrtávací políčko tooadd hello této skupiny toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f919b-149">**Optional:** If you would like too**add more than one group**, type in another **full group name** into hello **Search by name or email address** search box, and click hello checkbox tooadd this group toohello **Selected** list.</span></span>

13. <span data-ttu-id="f919b-150">Po dokončení výběru skupiny klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f919b-150">When you are finished selecting groups, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="f919b-151">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect toohello tooassign role skupinách vybrali.</span><span class="sxs-lookup"><span data-stu-id="f919b-151">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello groups you have selected.</span></span>

15. <span data-ttu-id="f919b-152">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybrané skupiny.</span><span class="sxs-lookup"><span data-stu-id="f919b-152">Click hello **Assign** button tooassign hello application toohello selected groups.</span></span>

<span data-ttu-id="f919b-153">Po krátké době být hello uživatelů v rámci hello skupin, které jste vybrali možnost toolaunch hello tyto aplikace pomocí metody popsané v části popis řešení hello.</span><span class="sxs-lookup"><span data-stu-id="f919b-153">After a short period of time, hello users within hello groups you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span> <span data-ttu-id="f919b-154">Pokud jsou dynamické skupiny, může být zpoždění některé další zpracování v těchto přiřazení zobrazování pro uživatele v rámci těchto přiřazeny skupiny.</span><span class="sxs-lookup"><span data-stu-id="f919b-154">If these are dynamic groups, there may be some additional processing delay in these assignments appearing for users within these assigned groups.</span></span>

## <a name="enable-self-service-application-access-tooallow-users-toofind-their-own-applications"></a><span data-ttu-id="f919b-155">Povolit aplikaci Samoobslužné služby přístup tooallow uživatelé toofind svých vlastních aplikacích</span><span class="sxs-lookup"><span data-stu-id="f919b-155">Enable self-service application access tooallow users toofind their own applications</span></span>

<span data-ttu-id="f919b-156">Přístup k aplikaci Samoobslužné služby je skvělý způsob tooallow uživatelé tooself-zjišťovat aplikace, můžete také povolit hello firmy skupiny tooapprove přístup toothose aplikace.</span><span class="sxs-lookup"><span data-stu-id="f919b-156">Self-service application access is a great way tooallow users tooself-discover applications, optionally allow hello business group tooapprove access toothose applications.</span></span> <span data-ttu-id="f919b-157">Můžete povolit, že pověření hello hello firmy skupiny toomanage přiřadili toothose uživatelů pro heslo jednotné přihlašování v aplikacích přímo z jejich panelů přístup.</span><span class="sxs-lookup"><span data-stu-id="f919b-157">You can allow hello business group toomanage hello credentials assigned toothose users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="f919b-158">tooenable aplikace Samoobslužné služby přístup tooan aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="f919b-158">tooenable self-service application access tooan application, follow hello steps below:</span></span>

1.  <span data-ttu-id="f919b-159">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="f919b-159">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f919b-160">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="f919b-160">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f919b-161">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f919b-161">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f919b-162">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f919b-162">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f919b-163">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="f919b-163">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="f919b-164">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="f919b-164">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="f919b-165">Vyberte aplikaci hello chcete tooenable samoobslužné služby přístup toofrom hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="f919b-165">Select hello application you want tooenable Self-service access toofrom hello list.</span></span>

7.  <span data-ttu-id="f919b-166">Po načtení hello aplikace, klikněte na **samoobslužné služby** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f919b-166">Once hello application loads, click **Self-service** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f919b-167">tooenable přístup k aplikaci Samoobslužné služby pro tuto aplikaci, aktivujte hello **povolit uživatelům toorequest přístup toothis aplikace?** přepnutí příliš**Ano.**</span><span class="sxs-lookup"><span data-stu-id="f919b-167">tooenable Self-service application access for this application, turn hello **Allow users toorequest access toothis application?** toggle too**Yes.**</span></span>

9.  <span data-ttu-id="f919b-168">V dalším kroku tooselect hello skupiny toowhich uživatelů, kteří požadují přístup toothis aplikace by měla být přidány, klikněte na tlačítko hello selektor další toohello popisek **toowhich skupina by měla přiřazená byli přidáni uživatelé?** a vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="f919b-168">Next, tooselect hello group toowhich users who request access toothis application should be added, click hello selector next toohello label **toowhich group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="f919b-169">**Volitelné:** nechcete-li toorequire obchodní schválení předtím, než mohou uživatelé přístup, nastavte hello **vyžadovat schválení před udělením přístupu toothis aplikace?** přepnutí příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="f919b-169">**Optional:** If you wish toorequire a business approval before users are allowed access, set hello **Require approval before granting access toothis application?** toggle too**Yes**.</span></span>

11. <span data-ttu-id="f919b-170">**Volitelné: pro aplikace pomocí hesla jednotné přihlašování na pouze** Pokud chcete tyto firmy schvalovatelů toospecify hello hesel, která se posílají toothis žádost o schválení uživatelé tooallow, nastavte hello **tooset schvalovatelů povolit hesla uživatele pro tuto aplikaci?**  přepnutí příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="f919b-170">**Optional: For applications using password single-sign on only,** if you wish tooallow those business approvers toospecify hello passwords that are sent toothis application for approved users, set hello **Allow approvers tooset user’s passwords for this application?** toggle too**Yes**.</span></span>

12. <span data-ttu-id="f919b-171">**Volitelné:** toospecify hello firmy schvalovatelů, kteří mají povoleno tooapprove přístup toothis aplikace, klikněte na tlačítko hello selektor další toohello popisek **kdo je povolená tooapprove přístup toothis aplikace?** tooselect nahoru too10 jednotlivé obchodní schvalovatelů.</span><span class="sxs-lookup"><span data-stu-id="f919b-171">**Optional:** toospecify hello business approvers who are allowed tooapprove access toothis application, click hello selector next toohello label **Who is allowed tooapprove access toothis application?** tooselect up too10 individual business approvers.</span></span>

  >[!NOTE]
  ><span data-ttu-id="f919b-172">Skupiny nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="f919b-172">Groups are not supported.</span></span>
  >
  >

13. <span data-ttu-id="f919b-173">**Volitelné:** **pro aplikace, které zveřejňují role**, pokud chcete, aby role tooa tooassign schválené uživatelé samoobslužné služby, klikněte na tlačítko Další toohello hello selektor **toowhich role lze přiřadit uživatelům v této aplikace?**  tooselect hello role toowhich by měla být přiřazená tyto uživatele.</span><span class="sxs-lookup"><span data-stu-id="f919b-173">**Optional:** **For applications which expose roles**, if you wish tooassign self-service approved users tooa role, click hello selector next toohello **toowhich role should users be assigned in this application?** tooselect hello role toowhich these users should be assigned.</span></span>

14. <span data-ttu-id="f919b-174">Klikněte na tlačítko hello **Uložit** tlačítko hello horní části okna toofinish hello.</span><span class="sxs-lookup"><span data-stu-id="f919b-174">Click hello **Save** button at hello top of hello blade toofinish.</span></span>

<span data-ttu-id="f919b-175">Po dokončení konfigurace samoobslužné služby aplikace, uživatelé mohou přejít tootheir [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko hello **+ přidat** tlačítko toofind hello aplikace toowhich jste povolili Samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="f919b-175">Once you complete Self-service application configuration, users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled Self-service access.</span></span> <span data-ttu-id="f919b-176">Obchodní schvalovatelů také zobrazit oznámení v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="f919b-176">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="f919b-177">Můžete povolit e-mail s upozorněním, když uživatel požaduje přístup tooan aplikace, která vyžaduje schválení.</span><span class="sxs-lookup"><span data-stu-id="f919b-177">You can enable an email notifying them when a user has requested access tooan application that requires their approval.</span></span> 

<span data-ttu-id="f919b-178">Tato schválení podporovat pouze jeden schválení pracovních, tj. Pokud zadáte několik schvalovatelů, jeden schvalovatel může schvalovatel přístup toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f919b-178">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access toohello application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f919b-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f919b-179">Next steps</span></span>
[<span data-ttu-id="f919b-180">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="f919b-180">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
