---
title: "Chyby na stránce aplikace po přihlášení | Microsoft Docs"
description: "Postup řešení potíží s Azure AD přihlášení při vlastní aplikace vysílá chybu"
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
ms.openlocfilehash: a8cd93256f79ece268ec3411dfbdf590f4b24447
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a><span data-ttu-id="902bb-103">Chyby na stránce aplikace po přihlášení</span><span class="sxs-lookup"><span data-stu-id="902bb-103">Error on an application's page after signing in</span></span>

<span data-ttu-id="902bb-104">V tomto scénáři podepsané uživatele v Azure AD, ale aplikace zobrazuje chybu neumožňuje uživateli úspěšně dokončit přihlášení toku.</span><span class="sxs-lookup"><span data-stu-id="902bb-104">In this scenario, Azure AD has signed the user in, but the application is displaying an error not allowing the user to successfully finish the sign in flow.</span></span> <span data-ttu-id="902bb-105">V tomto scénáři aplikace nepřijímá problém odpovědi službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="902bb-105">In this scenario, the application is not accepting the response issue by Azure AD.</span></span>

<span data-ttu-id="902bb-106">Existují některé možné důvody, proč nebylo aplikace přijmout odpověď z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="902bb-106">There are some possible reasons why the application didn’t accept the response from Azure AD.</span></span> <span data-ttu-id="902bb-107">Pokud chyba v aplikaci není dostatečně zrušte vědět, co je v odpovědi chybí pak:</span><span class="sxs-lookup"><span data-stu-id="902bb-107">If the error in the application is not clear enough to know what is missing in the response, then:</span></span>

-   <span data-ttu-id="902bb-108">Pokud je aplikace galerii Azure AD, ověřte jste postupovali podle pokynů v článku [ladění na základě SAML jednotného přihlašování k aplikacím v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span><span class="sxs-lookup"><span data-stu-id="902bb-108">If the application is the Azure AD Gallery, verify you have followed all the steps in the article [How to debug SAML-based single sign-on to applications in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span></span>

-   <span data-ttu-id="902bb-109">Pomocí některého nástroje, například [Fiddler](http://www.telerik.com/fiddler) zaznamenat žádost SAML, odpověď SAML a tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="902bb-109">Use a tool like [Fiddler](http://www.telerik.com/fiddler) to capture SAML request, SAML response and SAML token.</span></span>

-   <span data-ttu-id="902bb-110">Odpověď SAML sdílet se na dodavatele aplikace potřebujete vědět, co chybí.</span><span class="sxs-lookup"><span data-stu-id="902bb-110">Share the SAML response with the application vendor to know what is missing.</span></span>

## <a name="missing-attributes-in-the-saml-response"></a><span data-ttu-id="902bb-111">Chybějící atributy v odpovědi SAML</span><span class="sxs-lookup"><span data-stu-id="902bb-111">Missing attributes in the SAML response</span></span>

<span data-ttu-id="902bb-112">Pokud chcete přidat atribut v konfiguraci služby Azure AD k odeslání v odpovědi Azure AD, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="902bb-112">To add an attribute in the Azure AD configuration to be sent in the Azure AD response, follow the steps below:</span></span>

1.  <span data-ttu-id="902bb-113">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="902bb-113">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="902bb-114">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="902bb-114">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="902bb-115">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="902bb-115">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="902bb-116">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="902bb-116">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="902bb-117">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="902bb-117">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="902bb-118">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="902bb-118">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="902bb-119">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="902bb-119">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="902bb-120">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="902bb-120">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="902bb-121">Klikněte na tlačítko **prohlížení a úpravy atributy všechny ostatní uživatele v části** **uživatelské atributy** části, chcete-li upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="902bb-121">click **View and edit all other user attributes under** the **User Attributes** section to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="902bb-122">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="902bb-122">To add an attribute:</span></span>

   * <span data-ttu-id="902bb-123">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="902bb-123">click **Add attribute**.</span></span> <span data-ttu-id="902bb-124">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="902bb-124">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   * <span data-ttu-id="902bb-125">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="902bb-125">Click **Save.**</span></span> <span data-ttu-id="902bb-126">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="902bb-126">You see the new attribute in the table.</span></span>

9.  <span data-ttu-id="902bb-127">Uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="902bb-127">Save the configuration.</span></span>

<span data-ttu-id="902bb-128">Při příštím přihlášení uživatele k aplikaci, Azure AD odeslat nový atribut v odpovědi SAML.</span><span class="sxs-lookup"><span data-stu-id="902bb-128">Next time the user signs in to the application, Azure AD send the new attribute in the SAML response.</span></span>

## <a name="the-application-expects-a-different-user-identifier-value-or-format"></a><span data-ttu-id="902bb-129">Aplikace se očekává formát nebo jinou hodnotu identifikátor uživatele</span><span class="sxs-lookup"><span data-stu-id="902bb-129">The application expects a different User Identifier value or format</span></span>

<span data-ttu-id="902bb-130">Přihlášení k aplikaci se nedaří protože odpověď SAML chybí atributy, jako je například role nebo protože aplikace je očekáván jiný formát atribut EntityID.</span><span class="sxs-lookup"><span data-stu-id="902bb-130">The sign-in to the application is failing because the SAML response is missing attributes such as roles or because the application is expecting a different format for the EntityID attribute.</span></span>

## <a name="add-an-attribute-in-the-azure-ad-application-configuration"></a><span data-ttu-id="902bb-131">Přidáte atribut v konfiguraci aplikace Azure AD:</span><span class="sxs-lookup"><span data-stu-id="902bb-131">Add an attribute in the Azure AD application configuration:</span></span>

<span data-ttu-id="902bb-132">Chcete-li změnit hodnotu identifikátoru uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="902bb-132">To change the User Identifier value, follow the steps below:</span></span>

1.  <span data-ttu-id="902bb-133">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="902bb-133">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="902bb-134">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="902bb-134">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="902bb-135">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="902bb-135">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="902bb-136">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="902bb-136">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="902bb-137">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="902bb-137">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="902bb-138">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="902bb-138">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="902bb-139">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="902bb-139">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="902bb-140">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="902bb-140">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="902bb-141">V části **uživatelské atributy**, vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="902bb-141">Under the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

## <a name="change-entityid-user-identifier-format"></a><span data-ttu-id="902bb-142">Změna formátu EntityID (identifikátor uživatele)</span><span class="sxs-lookup"><span data-stu-id="902bb-142">Change EntityID (User Identifier) format</span></span>

<span data-ttu-id="902bb-143">Pokud aplikace očekává jiného formátu pro atribut EntityID.</span><span class="sxs-lookup"><span data-stu-id="902bb-143">If the application expects another format for the EntityID attribute.</span></span> <span data-ttu-id="902bb-144">Potom nebudete moci vybrat formát EntityID (identifikátor uživatele), který odešle aplikaci v odpovědi po ověření uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="902bb-144">Then, you won’t be able to select the EntityID (User Identifier) format that Azure AD sends to the application in the response after user authentication.</span></span>

<span data-ttu-id="902bb-145">Azure AD vyberte formát pro atribut NameID (identifikátor uživatele) na základě hodnoty vybraných nebo formát požadovanou aplikaci v SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="902bb-145">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="902bb-146">Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="902bb-146">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>

## <a name="the-application-expects-a-different-signature-method-for-the-saml-response"></a><span data-ttu-id="902bb-147">Aplikace očekává jiný podpis metody pro odpověď SAML</span><span class="sxs-lookup"><span data-stu-id="902bb-147">The application expects a different signature method for the SAML response</span></span>

<span data-ttu-id="902bb-148">Chcete-li změnit, které části tokenu SAML jsou digitálně podepsané službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="902bb-148">To change what parts of the SAML token are digitally signed by Azure Active Directory.</span></span> <span data-ttu-id="902bb-149">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="902bb-149">Follow the steps below:</span></span>

1.  <span data-ttu-id="902bb-150">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="902bb-150">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="902bb-151">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="902bb-151">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="902bb-152">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="902bb-152">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="902bb-153">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="902bb-153">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="902bb-154">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="902bb-154">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="902bb-155">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="902bb-155">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="902bb-156">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="902bb-156">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="902bb-157">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="902bb-157">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="902bb-158">Klikněte na tlačítko **zobrazit upřesňující nastavení podpisový certifikát** pod **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="902bb-158">click **Show advanced certificate signing settings** under the **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="902bb-159">Vyberte odpovídající **podepisování možnost** očekává aplikací:</span><span class="sxs-lookup"><span data-stu-id="902bb-159">Select the appropriate **Signing Option** expected by the application:</span></span>

  * <span data-ttu-id="902bb-160">Přihlašování SAML odpovědi</span><span class="sxs-lookup"><span data-stu-id="902bb-160">Sign SAML response</span></span>

  * <span data-ttu-id="902bb-161">Přihlašování SAML odpovědi a kontrolní výraz</span><span class="sxs-lookup"><span data-stu-id="902bb-161">Sign SAML response and assertion</span></span>

  * <span data-ttu-id="902bb-162">Přihlašovací kontrolního výrazu SAML</span><span class="sxs-lookup"><span data-stu-id="902bb-162">Sign SAML assertion</span></span>

<span data-ttu-id="902bb-163">Při příštím přihlášení uživatele k aplikaci, Azure AD přihlášení součástí vybrané odpověď SAML.</span><span class="sxs-lookup"><span data-stu-id="902bb-163">Next time the user signs in to the application, Azure AD sign the part of the SAML response selected.</span></span>

## <a name="the-application-expects-the-signing-algorithm-to-be-sha-1"></a><span data-ttu-id="902bb-164">Aplikace očekává podpisový algoritmus SHA-1</span><span class="sxs-lookup"><span data-stu-id="902bb-164">The application expects the signing algorithm to be SHA-1</span></span>

<span data-ttu-id="902bb-165">Ve výchozím nastavení Azure AD přihlášení pomocí algoritmu většina zabezpečení tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="902bb-165">By default, Azure AD signs the SAML token using the most security algorithm.</span></span> <span data-ttu-id="902bb-166">Změna přihlašovací algoritmus SHA-1 se nedoporučuje, pokud požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="902bb-166">Changing the sign algorithm to SHA-1 is not recommended unless required by the application.</span></span>

<span data-ttu-id="902bb-167">Chcete-li změnit podpisový algoritmus, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="902bb-167">To change the signing algorithm, follow the steps below:</span></span>

1.  <span data-ttu-id="902bb-168">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="902bb-168">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="902bb-169">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="902bb-169">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="902bb-170">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="902bb-170">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="902bb-171">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="902bb-171">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="902bb-172">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="902bb-172">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="902bb-173">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="902bb-173">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="902bb-174">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="902bb-174">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="902bb-175">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="902bb-175">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="902bb-176">Klikněte na tlačítko **zobrazit upřesňující nastavení podpisový certifikát** pod **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="902bb-176">click **Show advanced certificate signing settings** under the **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="902bb-177">Vyberte algoritmus SHA-1, v **podpisový algoritmus**.</span><span class="sxs-lookup"><span data-stu-id="902bb-177">Select SHA-1, in the **Signing Algorithm**.</span></span>

<span data-ttu-id="902bb-178">Při příštím přihlášení uživatele k aplikaci, Azure AD přihlášení tokenu SAML pomocí algoritmu SHA-1.</span><span class="sxs-lookup"><span data-stu-id="902bb-178">Next time the user signs in to the application, Azure AD sign the SAML token using SHA-1 algorithm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="902bb-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="902bb-179">Next steps</span></span>
[<span data-ttu-id="902bb-180">Postup ladění na základě SAML jednotného přihlašování k aplikacím v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="902bb-180">How to debug SAML-based single sign-on to applications in Azure Active Directory</span></span>](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
