---
title: "Problém konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD | Microsoft Docs"
description: "Některé běžné problémy, kterým může dojít při konfiguraci federovaný jeden adres přihlášení pomocí SAML pro aplikace, které jsou uvedeny v galerii aplikací Azure AD"
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
ms.openlocfilehash: 290ca66048281de5e031b0404919bed84ab19ffa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="cfb20-103">Problém konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfb20-103">Problem configuring federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="cfb20-104">Pokud dojde k potížím při konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="cfb20-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="cfb20-105">Ověřte, že jste provedli všechny kroky v tomto kurzu pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cfb20-105">Verify you have followed all the steps in the tutorial for the application.</span></span> <span data-ttu-id="cfb20-106">V konfiguraci aplikace máte vložené dokumentaci o konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="cfb20-106">In the application’s configuration, you have inline documentation on how to configure the application.</span></span> <span data-ttu-id="cfb20-107">Navíc se dostanete [seznam kurzů k integraci aplikací SaaS v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobností podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="cfb20-107">Also, you can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

## <a name="cant-add-another-instance-of-the-application"></a><span data-ttu-id="cfb20-108">Nelze přidat jiná instance aplikace</span><span class="sxs-lookup"><span data-stu-id="cfb20-108">Can’t add another instance of the application</span></span>

<span data-ttu-id="cfb20-109">Přidat druhou instanci aplikace, musíte být schopni:</span><span class="sxs-lookup"><span data-stu-id="cfb20-109">To add a second instance of an application, you need to be able to:</span></span>

-   <span data-ttu-id="cfb20-110">Jedinečný identifikátor pro druhou instanci nakonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="cfb20-110">Configure a unique identifier for the second instance.</span></span> <span data-ttu-id="cfb20-111">Nebudete moci konfigurovat stejný identifikátor použitý pro první instanci.</span><span class="sxs-lookup"><span data-stu-id="cfb20-111">You won’t be able to configure the same identifier used for the first instance.</span></span>

-   <span data-ttu-id="cfb20-112">Nakonfigurujte jiný certifikát než ten, který používá pro první instanci.</span><span class="sxs-lookup"><span data-stu-id="cfb20-112">Configure a different certificate than the one used for the first instance.</span></span>

<span data-ttu-id="cfb20-113">Pokud aplikace nepodporuje žádné z výše.</span><span class="sxs-lookup"><span data-stu-id="cfb20-113">If the application doesn’t support any of the above.</span></span> <span data-ttu-id="cfb20-114">Potom nebudete moci konfigurovat druhou instanci.</span><span class="sxs-lookup"><span data-stu-id="cfb20-114">Then, you won’t be able to configure a second instance.</span></span>

## <a name="cant-add-the-identifier-or-the-reply-url"></a><span data-ttu-id="cfb20-115">Nelze přidat identifikátor nebo adresa URL odpovědi</span><span class="sxs-lookup"><span data-stu-id="cfb20-115">Can’t add the Identifier or the Reply URL</span></span>

<span data-ttu-id="cfb20-116">Pokud si nejste moci konfigurovat identifikátor nebo adresa URL odpovědi, potvrzení hodnoty identifikátor a adresa URL odpovědi vzory předem nakonfigurované pro danou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cfb20-116">If you’re not able to configure the Identifier or the Reply URL, confirm the Identifier and Reply URL values match the patterns pre-configured for the application.</span></span>

<span data-ttu-id="cfb20-117">Vzory vědět předem nakonfigurované pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="cfb20-117">To know the patterns pre-configured for the application:</span></span>

1.  <span data-ttu-id="cfb20-118">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="cfb20-118">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span> <span data-ttu-id="cfb20-119">Přejděte ke kroku 7.</span><span class="sxs-lookup"><span data-stu-id="cfb20-119">Go to step 7.</span></span> <span data-ttu-id="cfb20-120">Pokud jste už v okně Konfigurace aplikace na Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfb20-120">If you are already in the application configuration blade on Azure AD.</span></span>

2.  <span data-ttu-id="cfb20-121">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="cfb20-121">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="cfb20-122">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="cfb20-122">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="cfb20-123">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cfb20-123">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="cfb20-124">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="cfb20-124">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="cfb20-125">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="cfb20-125">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="cfb20-126">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cfb20-126">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="cfb20-127">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="cfb20-127">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="cfb20-128">Vyberte **na základě SAML přihlašování** z **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="cfb20-128">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="cfb20-129">Přejděte na **identifikátor** nebo **adresa URL odpovědi** textové pole, v části **oddílu domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="cfb20-129">Go to the **Identifier** or **Reply URL** textbox, under the **Domain and URLs section.**</span></span>

10. <span data-ttu-id="cfb20-130">Existují tři způsoby, jak vědět podporované vzory pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="cfb20-130">There are three ways to know the supported patterns for the application:</span></span>

   * <span data-ttu-id="cfb20-131">Do textového pole, jako zástupný znak zobrazí podporované nalezeny *příklad:* <https://contoso.com>.</span><span class="sxs-lookup"><span data-stu-id="cfb20-131">In the textbox, you see the supported pattern(s) as a placeholder *Example:* <https://contoso.com>.</span></span>

   * <span data-ttu-id="cfb20-132">Pokud vzor není podporován, zobrazí se při pokusu o do textového pole zadejte hodnotu červený vykřičník.</span><span class="sxs-lookup"><span data-stu-id="cfb20-132">if the pattern is not supported, you see a red exclamation mark when you try to enter the value in the textbox.</span></span> <span data-ttu-id="cfb20-133">Umístěte ukazatel myši nad červený vykřičník, uvidíte podporované vzory.</span><span class="sxs-lookup"><span data-stu-id="cfb20-133">If you hover your mouse over the red exclamation mark, you see the supported patterns.</span></span>

   * <span data-ttu-id="cfb20-134">V tomto kurzu pro aplikaci můžete také získat informace o podporovaných vzory.</span><span class="sxs-lookup"><span data-stu-id="cfb20-134">In the tutorial for the application, you can also get information about the supported patterns.</span></span> <span data-ttu-id="cfb20-135">V části **konfigurovat Azure AD jednotné přihlašování** části.</span><span class="sxs-lookup"><span data-stu-id="cfb20-135">Under the **Configure Azure AD single sign-on** section.</span></span> <span data-ttu-id="cfb20-136">Přejděte na krok pro hodnoty v části nakonfigurována **domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="cfb20-136">Go to the step for configured the values under the **Domain and URLs** section.</span></span>

<span data-ttu-id="cfb20-137">Pokud se hodnoty neshodují s vzory předem nakonfigurované na Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfb20-137">If the values don’t match with the patterns pre-configured on Azure AD.</span></span> <span data-ttu-id="cfb20-138">Můžete:</span><span class="sxs-lookup"><span data-stu-id="cfb20-138">You can:</span></span>

-   <span data-ttu-id="cfb20-139">Spolupracujte s dodavatelem aplikace získat hodnoty, které se shodují se vzorem předem nakonfigurované na Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfb20-139">Work with the application vendor to get values that match the pattern pre-configured on Azure AD</span></span>

-   <span data-ttu-id="cfb20-140">Nebo se obrátit na tým Azure AD v < aadapprequest@microsoft.com > nebo komentář v kurzu k vyžádání aktualizace podporované vzory pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="cfb20-140">Or, you can contact Azure AD team at <aadapprequest@microsoft.com> or leave a comment in the tutorial to request the update of the supported patterns for the application</span></span>

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a><span data-ttu-id="cfb20-141">Kde lze nastavit formát EntityID (identifikátor uživatele)</span><span class="sxs-lookup"><span data-stu-id="cfb20-141">Where do I set the EntityID (User Identifier) format</span></span>

<span data-ttu-id="cfb20-142">Nebudete moci vybrat formát EntityID (identifikátor uživatele), který odešle aplikaci v odpovědi po ověření uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfb20-142">You won’t be able to select the EntityID (User Identifier) format that Azure AD sends to the application in the response after user authentication.</span></span>

<span data-ttu-id="cfb20-143">Azure AD vyberte formát pro atribut NameID (identifikátor uživatele) na základě hodnoty vybraných nebo formát požadovanou aplikaci v SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="cfb20-143">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="cfb20-144">Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="cfb20-144">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy,</span></span>

## <a name="cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application"></a><span data-ttu-id="cfb20-145">Nelze najít metadata, Azure AD k dokončení konfigurace s aplikací</span><span class="sxs-lookup"><span data-stu-id="cfb20-145">Can’t find the Azure AD metadata to complete the configuration with the application</span></span>

<span data-ttu-id="cfb20-146">Chcete-li stáhnout metadata aplikace nebo certifikát z Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="cfb20-146">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="cfb20-147">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="cfb20-147">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="cfb20-148">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="cfb20-148">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="cfb20-149">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="cfb20-149">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="cfb20-150">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cfb20-150">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="cfb20-151">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="cfb20-151">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="cfb20-152">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="cfb20-152">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="cfb20-153">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cfb20-153">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="cfb20-154">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="cfb20-154">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="cfb20-155">Přejděte na **SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="cfb20-155">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="cfb20-156">V závislosti na tom, jaké aplikace vyžaduje konfiguraci jednotné přihlašování uvidíte buď možnost Stáhnout soubor XML s metadaty nebo certifikát.</span><span class="sxs-lookup"><span data-stu-id="cfb20-156">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="cfb20-157">Azure AD neposkytuje adresu URL se získat metadata.</span><span class="sxs-lookup"><span data-stu-id="cfb20-157">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="cfb20-158">Metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="cfb20-158">The metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a><span data-ttu-id="cfb20-159">Nemusíte vědět, jak přizpůsobit SAML deklarace identity odeslat do aplikace</span><span class="sxs-lookup"><span data-stu-id="cfb20-159">Don't know how to customize SAML claims sent to an application</span></span>

<span data-ttu-id="cfb20-160">Naučte se přizpůsobovat deklarací identity atributu SAML odeslaných do vaší aplikace, najdete v tématu [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cfb20-160">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cfb20-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cfb20-161">Next steps</span></span>
[<span data-ttu-id="cfb20-162">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cfb20-162">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
