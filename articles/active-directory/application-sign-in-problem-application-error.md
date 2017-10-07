---
title: "na stránce aplikace po přihlášení aaaError | Microsoft Docs"
description: "Jak tooresolve problémy s Azure AD přihlášení během vlastní aplikace hello vysílá chybu"
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
ms.openlocfilehash: 317b6f8e6417520ead80ae4e26c591ba6b134683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a><span data-ttu-id="3c808-103">Chyby na stránce aplikace po přihlášení</span><span class="sxs-lookup"><span data-stu-id="3c808-103">Error on an application's page after signing in</span></span>

<span data-ttu-id="3c808-104">V tomto scénáři podepsané hello uživatele v Azure AD, ale aplikace hello zobrazuje chybu neumožňuje hello uživatele toosuccessfully dokončit hello přihlašovací v toku.</span><span class="sxs-lookup"><span data-stu-id="3c808-104">In this scenario, Azure AD has signed hello user in, but hello application is displaying an error not allowing hello user toosuccessfully finish hello sign in flow.</span></span> <span data-ttu-id="3c808-105">V tomto scénáři aplikace hello nepřijímá hello odpovědi problém službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3c808-105">In this scenario, hello application is not accepting hello response issue by Azure AD.</span></span>

<span data-ttu-id="3c808-106">Existují některé možné důvody, proč nebylo aplikace hello přijímat hello odpověď z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3c808-106">There are some possible reasons why hello application didn’t accept hello response from Azure AD.</span></span> <span data-ttu-id="3c808-107">Pokud hello Chyba v aplikaci hello není dostatečně zrušte tooknow toho, co chybí v hello odpověď, a pak:</span><span class="sxs-lookup"><span data-stu-id="3c808-107">If hello error in hello application is not clear enough tooknow what is missing in hello response, then:</span></span>

-   <span data-ttu-id="3c808-108">Pokud aplikace hello Galerie hello Azure AD, ověřte všechny kroky hello v článku hello [jak toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span><span class="sxs-lookup"><span data-stu-id="3c808-108">If hello application is hello Azure AD Gallery, verify you have followed all hello steps in hello article [How toodebug SAML-based single sign-on tooapplications in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span></span>

-   <span data-ttu-id="3c808-109">Pomocí některého nástroje, například [Fiddler](http://www.telerik.com/fiddler) toocapture SAML požadavků, odpověď SAML a tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="3c808-109">Use a tool like [Fiddler](http://www.telerik.com/fiddler) toocapture SAML request, SAML response and SAML token.</span></span>

-   <span data-ttu-id="3c808-110">Sdílet odpověď SAML hello s tooknow dodavatele aplikace hello toho, co chybí.</span><span class="sxs-lookup"><span data-stu-id="3c808-110">Share hello SAML response with hello application vendor tooknow what is missing.</span></span>

## <a name="missing-attributes-in-hello-saml-response"></a><span data-ttu-id="3c808-111">Chybějící atributů v hello odpověď SAML</span><span class="sxs-lookup"><span data-stu-id="3c808-111">Missing attributes in hello SAML response</span></span>

<span data-ttu-id="3c808-112">tooadd atribut v toobe konfigurace Azure AD hello odeslaný v odpovědi hello Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="3c808-112">tooadd an attribute in hello Azure AD configuration toobe sent in hello Azure AD response, follow hello steps below:</span></span>

1.  <span data-ttu-id="3c808-113">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3c808-113">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3c808-114">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-114">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3c808-115">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3c808-115">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3c808-116">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="3c808-116">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3c808-117">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3c808-117">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="3c808-118">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3c808-118">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="3c808-119">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c808-119">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="3c808-120">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="3c808-120">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3c808-121">Klikněte na tlačítko **prohlížení a úpravy atributy všechny ostatní uživatele v části** hello **uživatelské atributy** části tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c808-121">click **View and edit all other user attributes under** hello **User Attributes** section tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="3c808-122">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="3c808-122">tooadd an attribute:</span></span>

   * <span data-ttu-id="3c808-123">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="3c808-123">click **Add attribute**.</span></span> <span data-ttu-id="3c808-124">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-124">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   * <span data-ttu-id="3c808-125">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="3c808-125">Click **Save.**</span></span> <span data-ttu-id="3c808-126">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-126">You see hello new attribute in hello table.</span></span>

9.  <span data-ttu-id="3c808-127">Uložte konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-127">Save hello configuration.</span></span>

<span data-ttu-id="3c808-128">Při příštím přihlášení uživatele hello toohello aplikace Azure AD odeslat hello nový atribut v hello odpověď SAML.</span><span class="sxs-lookup"><span data-stu-id="3c808-128">Next time hello user signs in toohello application, Azure AD send hello new attribute in hello SAML response.</span></span>

## <a name="hello-application-expects-a-different-user-identifier-value-or-format"></a><span data-ttu-id="3c808-129">aplikace Hello očekává formát nebo jinou hodnotu identifikátor uživatele</span><span class="sxs-lookup"><span data-stu-id="3c808-129">hello application expects a different User Identifier value or format</span></span>

<span data-ttu-id="3c808-130">Hello přihlášení toohello aplikace selhává protože hello odpověď SAML chybí atributy, jako je například role nebo aplikace hello je očekáván jiný formát hello EntityID atribut.</span><span class="sxs-lookup"><span data-stu-id="3c808-130">hello sign-in toohello application is failing because hello SAML response is missing attributes such as roles or because hello application is expecting a different format for hello EntityID attribute.</span></span>

## <a name="add-an-attribute-in-hello-azure-ad-application-configuration"></a><span data-ttu-id="3c808-131">Přidáte atribut v konfiguraci aplikace hello Azure AD:</span><span class="sxs-lookup"><span data-stu-id="3c808-131">Add an attribute in hello Azure AD application configuration:</span></span>

<span data-ttu-id="3c808-132">toochange hello hodnota identifikátoru uživatele, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="3c808-132">toochange hello User Identifier value, follow hello steps below:</span></span>

1.  <span data-ttu-id="3c808-133">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3c808-133">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3c808-134">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-134">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3c808-135">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3c808-135">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3c808-136">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="3c808-136">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3c808-137">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3c808-137">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="3c808-138">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3c808-138">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="3c808-139">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c808-139">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="3c808-140">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="3c808-140">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3c808-141">V části hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3c808-141">Under hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

## <a name="change-entityid-user-identifier-format"></a><span data-ttu-id="3c808-142">Změna formátu EntityID (identifikátor uživatele)</span><span class="sxs-lookup"><span data-stu-id="3c808-142">Change EntityID (User Identifier) format</span></span>

<span data-ttu-id="3c808-143">Pokud aplikace hello očekává jiného formátu pro atribut EntityID hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-143">If hello application expects another format for hello EntityID attribute.</span></span> <span data-ttu-id="3c808-144">Potom nebudete moct tooselect hello EntityID (identifikátor uživatele) formát, Azure AD odešle toohello aplikace v odpovědi hello po ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c808-144">Then, you won’t be able tooselect hello EntityID (User Identifier) format that Azure AD sends toohello application in hello response after user authentication.</span></span>

<span data-ttu-id="3c808-145">Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="3c808-145">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="3c808-146">Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="3c808-146">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>

## <a name="hello-application-expects-a-different-signature-method-for-hello-saml-response"></a><span data-ttu-id="3c808-147">aplikace Hello očekává jiný podpis metody pro hello odpověď SAML</span><span class="sxs-lookup"><span data-stu-id="3c808-147">hello application expects a different signature method for hello SAML response</span></span>

<span data-ttu-id="3c808-148">toochange, které části tokenu SAML hello jsou digitálně podepsané službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3c808-148">toochange what parts of hello SAML token are digitally signed by Azure Active Directory.</span></span> <span data-ttu-id="3c808-149">Postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="3c808-149">Follow hello steps below:</span></span>

1.  <span data-ttu-id="3c808-150">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3c808-150">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3c808-151">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-151">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3c808-152">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3c808-152">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3c808-153">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="3c808-153">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3c808-154">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3c808-154">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="3c808-155">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3c808-155">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="3c808-156">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c808-156">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="3c808-157">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="3c808-157">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3c808-158">Klikněte na tlačítko **zobrazit upřesňující nastavení podpisový certifikát** pod hello **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="3c808-158">click **Show advanced certificate signing settings** under hello **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="3c808-159">Vyberte odpovídající hello **podepisování možnost** očekávanou hello aplikace:</span><span class="sxs-lookup"><span data-stu-id="3c808-159">Select hello appropriate **Signing Option** expected by hello application:</span></span>

  * <span data-ttu-id="3c808-160">Přihlašování SAML odpovědi</span><span class="sxs-lookup"><span data-stu-id="3c808-160">Sign SAML response</span></span>

  * <span data-ttu-id="3c808-161">Přihlašování SAML odpovědi a kontrolní výraz</span><span class="sxs-lookup"><span data-stu-id="3c808-161">Sign SAML response and assertion</span></span>

  * <span data-ttu-id="3c808-162">Přihlašovací kontrolního výrazu SAML</span><span class="sxs-lookup"><span data-stu-id="3c808-162">Sign SAML assertion</span></span>

<span data-ttu-id="3c808-163">Při příštím přihlášení uživatele hello toohello aplikace Azure AD přihlášení hello součástí vybrané odpověď SAML hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-163">Next time hello user signs in toohello application, Azure AD sign hello part of hello SAML response selected.</span></span>

## <a name="hello-application-expects-hello-signing-algorithm-toobe-sha-1"></a><span data-ttu-id="3c808-164">aplikace Hello očekává hello podepisování toobe algoritmus SHA-1</span><span class="sxs-lookup"><span data-stu-id="3c808-164">hello application expects hello signing algorithm toobe SHA-1</span></span>

<span data-ttu-id="3c808-165">Ve výchozím nastavení Azure AD podepisuje tokeny SAML hello pomocí hello většina algoritmus zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3c808-165">By default, Azure AD signs hello SAML token using hello most security algorithm.</span></span> <span data-ttu-id="3c808-166">Změna hello přihlašovací algoritmus tooSHA-1 se nedoporučuje, pokud se vyžaduje aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-166">Changing hello sign algorithm tooSHA-1 is not recommended unless required by hello application.</span></span>

<span data-ttu-id="3c808-167">toochange hello podpisový algoritmus, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="3c808-167">toochange hello signing algorithm, follow hello steps below:</span></span>

1.  <span data-ttu-id="3c808-168">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3c808-168">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3c808-169">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="3c808-169">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3c808-170">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3c808-170">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3c808-171">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="3c808-171">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3c808-172">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3c808-172">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="3c808-173">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3c808-173">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="3c808-174">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c808-174">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="3c808-175">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="3c808-175">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3c808-176">Klikněte na tlačítko **zobrazit upřesňující nastavení podpisový certifikát** pod hello **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="3c808-176">click **Show advanced certificate signing settings** under hello **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="3c808-177">Vyberte algoritmus SHA-1, v hello **podpisový algoritmus**.</span><span class="sxs-lookup"><span data-stu-id="3c808-177">Select SHA-1, in hello **Signing Algorithm**.</span></span>

<span data-ttu-id="3c808-178">Uživatel se přihlásí v aplikaci toohello hello příště, Azure AD přihlášení hello tokenu SAML pomocí algoritmu SHA-1.</span><span class="sxs-lookup"><span data-stu-id="3c808-178">Next time hello user signs in toohello application, Azure AD sign hello SAML token using SHA-1 algorithm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c808-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c808-179">Next steps</span></span>
[<span data-ttu-id="3c808-180">Jak toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3c808-180">How toodebug SAML-based single sign-on tooapplications in Azure Active Directory</span></span>](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
