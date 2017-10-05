---
title: "Přiřazená aplikace se nezobrazují na přístupovém panelu | Microsoft Docs"
description: "Poradce při potížích se aplikace nejsou zobrazeny na přístupovém panelu"
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
ms.reviwer: japere
ms.openlocfilehash: 9ea5744d77b90929598ea5feb80c7bbdff3772fc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="an-assigned-application-is-not-appearing-on-the-access-panel"></a><span data-ttu-id="3be46-103">Přiřazená aplikace se nezobrazují přístupového panelu</span><span class="sxs-lookup"><span data-stu-id="3be46-103">An assigned application is not appearing on the access panel</span></span>

<span data-ttu-id="3be46-104">Přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) k zobrazení a spustit cloudové aplikace, aby správce Azure AD má je přístup povolen.</span><span class="sxs-lookup"><span data-stu-id="3be46-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="3be46-105">Tyto aplikace jsou konfigurovány jménem uživatele na portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3be46-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="3be46-106">Aplikace musí být správně nakonfigurovaná a přiřadit uživatele nebo skupiny, kterých je uživatel členem skupiny chcete vidět aplikaci na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="3be46-106">The application must be configured properly and assigned to the user or a group the user is a member of to see the application in the Access Panel.</span></span>

<span data-ttu-id="3be46-107">Typ aplikace, které zobrazuje uživatel možná spadají do následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="3be46-107">The type of apps a user may be seeing fall in the following categories:</span></span>

-   <span data-ttu-id="3be46-108">Aplikace Office 365</span><span class="sxs-lookup"><span data-stu-id="3be46-108">Office 365 Applications</span></span>

-   <span data-ttu-id="3be46-109">Společnost Microsoft a třetích stran aplikace, které jsou nakonfigurované pomocí jednotného přihlašování na základě federation</span><span class="sxs-lookup"><span data-stu-id="3be46-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="3be46-110">Aplikace založené na heslech jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3be46-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="3be46-111">Aplikace s stávajících řešení pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3be46-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="3be46-112">Běžné problémy a proveďte nejprve kontrolu</span><span class="sxs-lookup"><span data-stu-id="3be46-112">General issues to check first</span></span>

-   <span data-ttu-id="3be46-113">Pokud aplikace byl právě přidali na uživatele, zkuste se přihlásit a odhlašování znovu do přístupového panelu uživatele po několika minutách ověřte, zda je aplikace přidána.</span><span class="sxs-lookup"><span data-stu-id="3be46-113">If an application was just added to a user, try to sign in and out again into the user’s Access Panel after a few minutes to see if the application is added.</span></span>

-   <span data-ttu-id="3be46-114">Pokud licence byla právě odebrána ze uživatele nebo skupinu je uživatel že členem skupiny to může trvat dlouhou dobu, v závislosti na velikost a složitost skupiny změny má být provedeno.</span><span class="sxs-lookup"><span data-stu-id="3be46-114">If a license was just removed from a user or group the user is a member of this may take a long time, depending on the size and complexity of the group for changes to be made.</span></span> <span data-ttu-id="3be46-115">Povolit další dobu před přihlášení k přístupovému panelu.</span><span class="sxs-lookup"><span data-stu-id="3be46-115">Allow for extra time before signing into the Access Panel.</span></span>

## <a name="problems-related-to-application-configuration"></a><span data-ttu-id="3be46-116">Problémy související s konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="3be46-116">Problems related to application configuration</span></span>

<span data-ttu-id="3be46-117">Aplikace nemusí zobrazovaných v Panel přístupu uživatele, protože není správně nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="3be46-117">An application may not be appearing in a user’s Access Panel because it is not configured properly.</span></span> <span data-ttu-id="3be46-118">Tady jsou některé způsoby, jak můžete potíže související s konfigurací aplikace:</span><span class="sxs-lookup"><span data-stu-id="3be46-118">Below are some ways you can troubleshoot issues related to application configuration:</span></span>

-   [<span data-ttu-id="3be46-119">Postup konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="3be46-119">How to configure federated single sign-on for an Azure AD gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [<span data-ttu-id="3be46-120">Postup konfigurace federované jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="3be46-120">How to configure federated single sign-on for a non-gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="3be46-121">Postup konfigurace hesel jedné přihlášení aplikace pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="3be46-121">How to configure a password single sign-on application for an Azure AD gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="3be46-122">Postup konfigurace hesel jedné přihlášení aplikace pro aplikaci není Galerie</span><span class="sxs-lookup"><span data-stu-id="3be46-122">How to configure a password single sign-on application for a non-gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="3be46-123">Postup konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="3be46-123">How to configure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="3be46-124">Všechny aplikace v galerii Azure AD povolit funkce Enterprise Single Sign-On obsahuje podrobný kurz k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3be46-124">All applications in the Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="3be46-125">Dostanete [seznam kurzů k integraci aplikací SaaS v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobností podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="3be46-125">You can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="3be46-126">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3be46-126">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="3be46-127">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="3be46-127">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="3be46-128">Nakonfigurujte hodnoty metadata aplikace ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="3be46-128">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="3be46-129">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="3be46-129">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="3be46-130">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="3be46-130">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="3be46-131">Konfigurovat Azure AD metadata hodnoty v aplikaci (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="3be46-131">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="3be46-132">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="3be46-132">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="3be46-133">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-133">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-134">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="3be46-134">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="3be46-135">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-135">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-136">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-136">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-137">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-137">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-138">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="3be46-138">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="3be46-139">V **zadejte název** textbox z **přidat z Galerie** části, zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-139">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="3be46-140">Vyberte aplikaci, kterou chcete nakonfigurovat pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3be46-140">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="3be46-141">Před přidáním aplikace, můžete změnit její název ze **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3be46-141">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="3be46-142">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-142">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="3be46-143">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-143">After a short period, you be able to see the application’s configuration blade.</span></span>

#### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="3be46-144">Konfigurovat jednotné přihlašování pro aplikace z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="3be46-144">Configure single sign-on for an application from the Azure AD gallery</span></span>

<span data-ttu-id="3be46-145">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3be46-145">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-146">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-146">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3be46-147">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-147">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-148">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-148">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-149">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-149">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-150">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-150">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="3be46-151">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-151">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3be46-152">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3be46-152">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="3be46-153">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-153">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3be46-154">Vyberte **na základě SAML přihlašování** z **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-154">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="3be46-155">Zadejte požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="3be46-155">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="3be46-156">Tyto hodnoty by měly získat od dodavatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-156">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="3be46-157">Chcete-li nakonfigurovat aplikaci jako spouštěná SP jednotného přihlašování adresa URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="3be46-157">To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span> <span data-ttu-id="3be46-158">Některé aplikace je identifikátor také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="3be46-158">For some applications, the Identifier is also a required value.</span></span>

   2. <span data-ttu-id="3be46-159">Chcete-li nakonfigurovat aplikaci jako spouštěná IdP SSO adresa URL odpovědi je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="3be46-159">To configure the application as IdP-initiated SSO, the Reply URL it’s a required value.</span></span> <span data-ttu-id="3be46-160">Některé aplikace je identifikátor také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="3be46-160">For some applications, the Identifier is also a required value.</span></span>

10. <span data-ttu-id="3be46-161">**Volitelné:** klikněte na tlačítko **zobrazit upřesňující nastavení adresy URL** Pokud budete chtít prohlédnout-požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3be46-161">**Optional:** click **Show advanced URL settings** if you want to see the non-required values.</span></span>

11. <span data-ttu-id="3be46-162">V **uživatelské atributy**, vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-162">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="3be46-163">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="3be46-163">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="3be46-164">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="3be46-164">To add an attribute:</span></span>

   1. <span data-ttu-id="3be46-165">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="3be46-165">click **Add attribute**.</span></span> <span data-ttu-id="3be46-166">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-166">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="3be46-167">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="3be46-167">click **Save.**</span></span> <span data-ttu-id="3be46-168">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="3be46-168">You see the new attribute in the table.</span></span>

13. <span data-ttu-id="3be46-169">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  dokumentace přístupu na tom, jak nakonfigurovat jednotné přihlašování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-169">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="3be46-170">Navíc má adres URL metadat a certifikát nutný k nastavení jednotného přihlašování k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-170">Also, you has the metadata URLs and certificate required to setup SSO with the application.</span></span>

14. <span data-ttu-id="3be46-171">Klikněte na tlačítko **Uložit** konfiguraci uložíte.</span><span class="sxs-lookup"><span data-stu-id="3be46-171">click **Save** to save the configuration.</span></span>

15. <span data-ttu-id="3be46-172">Přiřazení uživatelů k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-172">Assign users to the application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="3be46-173">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="3be46-173">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="3be46-174">K výběru identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-174">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-175">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-175">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3be46-176">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-176">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-177">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-177">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-178">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-178">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-179">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-179">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="3be46-180">Pokud nevidíte aplikaci chcete zde zobrazí, použijte **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-180">If you do not see the application you want to show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3be46-181">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3be46-181">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="3be46-182">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-182">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3be46-183">V části **uživatelské atributy** vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-183">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="3be46-184">Vybraná možnost musí odpovídat očekávaná hodnota v aplikaci k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="3be46-184">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="3be46-185">Azure AD vyberte formát pro atribut NameID (identifikátor uživatele) na základě hodnoty vybraných nebo formát požadovanou aplikaci v SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="3be46-185">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="3be46-186">Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="3be46-186">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="3be46-187">Chcete-li přidat atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="3be46-187">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="3be46-188">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="3be46-188">To add an attribute:</span></span>

   1. <span data-ttu-id="3be46-189">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="3be46-189">click **Add attribute**.</span></span> <span data-ttu-id="3be46-190">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-190">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="3be46-191">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="3be46-191">click **Save.**</span></span> <span data-ttu-id="3be46-192">Zobrazí se nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="3be46-192">You will see the new attribute in the table.</span></span>

#### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="3be46-193">Stáhnout Azure AD metadata nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="3be46-193">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="3be46-194">Chcete-li stáhnout metadata aplikace nebo certifikát z Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-194">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-195">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-195">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3be46-196">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-196">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-197">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-197">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-198">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-198">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-199">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-199">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="3be46-200">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-200">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3be46-201">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3be46-201">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="3be46-202">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-202">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3be46-203">Přejděte na **SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="3be46-203">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="3be46-204">V závislosti na tom, jaké aplikace vyžaduje konfiguraci jednotné přihlašování uvidíte buď možnost Stáhnout soubor XML s metadaty nebo certifikát.</span><span class="sxs-lookup"><span data-stu-id="3be46-204">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

    <span data-ttu-id="3be46-205">Azure AD neposkytuje adresu URL se získat metadata.</span><span class="sxs-lookup"><span data-stu-id="3be46-205">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="3be46-206">Metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="3be46-206">The metadata can only be retrieved as a XML file.</span></span>

### <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="3be46-207">Postup konfigurace federované jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="3be46-207">How to configure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="3be46-208">Konfigurace aplikace bez galerie, potřebujete Azure AD premium a aplikace podporuje SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="3be46-208">To configure a non-gallery application, you need to have Azure AD premium and the application supports SAML 2.0.</span></span> <span data-ttu-id="3be46-209">Další informace o verzích služby Azure AD, najdete v článku [ceny služby Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="3be46-209">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="3be46-210">Nakonfigurujte hodnoty metadata aplikace ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="3be46-210">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="3be46-211">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="3be46-211">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="3be46-212">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="3be46-212">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="3be46-213">Konfigurovat Azure AD metadata hodnoty v aplikaci (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="3be46-213">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

#### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="3be46-214">Nakonfigurujte hodnoty metadata aplikace ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="3be46-214">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="3be46-215">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, která není v galerii Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-215">To configure single sign-on for an application that is not in the Azure AD gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-216">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-216">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3be46-217">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-217">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-218">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-218">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-219">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-219">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-220">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="3be46-220">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="3be46-221">Klikněte na tlačítko **aplikace bez Galerie** v **přidat vlastní aplikaci** části.</span><span class="sxs-lookup"><span data-stu-id="3be46-221">click **Non-gallery application** in the **Add your own app** section.</span></span>

7.  <span data-ttu-id="3be46-222">Zadejte název aplikace **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3be46-222">Enter the name of the application in the **Name** textbox.</span></span>

8.  <span data-ttu-id="3be46-223">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-223">Click **Add** button, to add the application.</span></span>

9.  <span data-ttu-id="3be46-224">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-224">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="3be46-225">Vyberte **na základě SAML přihlašování** v **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-225">Select **SAML-based Sign-on** in the **Mode** dropdown.</span></span>

11. <span data-ttu-id="3be46-226">Zadejte požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="3be46-226">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="3be46-227">Tyto hodnoty by měly získat od dodavatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-227">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="3be46-228">Chcete-li nakonfigurovat aplikaci jako jednotné přihlašování iniciované deklarací identity, zadejte adresa URL odpovědi a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="3be46-228">To configure the application as IdP-initiated SSO, enter the Reply URL and the Identifier.</span></span>

   2.  <span data-ttu-id="3be46-229">**Volitelné:** konfigurace aplikace jako spouštěná SP jednotného přihlašování adresa URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="3be46-229">**Optional:** To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="3be46-230">V **uživatelské atributy**, vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-230">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="3be46-231">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="3be46-231">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="3be46-232">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="3be46-232">To add an attribute:</span></span>

   1. <span data-ttu-id="3be46-233">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="3be46-233">click **Add attribute**.</span></span> <span data-ttu-id="3be46-234">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-234">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="3be46-235">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="3be46-235">Click **Save.**</span></span> <span data-ttu-id="3be46-236">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="3be46-236">You see the new attribute in the table.</span></span>

14. <span data-ttu-id="3be46-237">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  dokumentace přístupu na tom, jak nakonfigurovat jednotné přihlašování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-237">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="3be46-238">Navíc má Azure AD adresy URL a certifikát nutný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-238">Also, you has Azure AD URLs and certificate required for the application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="3be46-239">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="3be46-239">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="3be46-240">K výběru identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-240">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-241">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-241">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3be46-242">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-242">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-243">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-243">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-244">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-244">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-245">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-245">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="3be46-246">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-246">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3be46-247">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3be46-247">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="3be46-248">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-248">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3be46-249">V části **uživatelské atributy** vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-249">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="3be46-250">Vybraná možnost musí odpovídat očekávaná hodnota v aplikaci k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="3be46-250">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="3be46-251">Azure AD vyberte formát pro atribut NameID (identifikátor uživatele) na základě hodnoty vybraných nebo formát požadovanou aplikaci v SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="3be46-251">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="3be46-252">Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="3be46-252">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="3be46-253">Chcete-li přidat atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="3be46-253">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="3be46-254">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="3be46-254">To add an attribute:</span></span>

   1. <span data-ttu-id="3be46-255">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="3be46-255">click **Add attribute**.</span></span> <span data-ttu-id="3be46-256">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-256">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="3be46-257">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="3be46-257">Click **Save.**</span></span> <span data-ttu-id="3be46-258">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="3be46-258">You see the new attribute in the table.</span></span>

#### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="3be46-259">Stáhnout Azure AD metadata nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="3be46-259">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="3be46-260">Chcete-li stáhnout metadata aplikace nebo certifikát z Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-260">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-261">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-261">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3be46-262">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-262">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-263">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-263">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-264">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-264">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-265">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-265">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="3be46-266">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-266">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3be46-267">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3be46-267">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="3be46-268">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-268">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3be46-269">Přejděte na **SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="3be46-269">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="3be46-270">V závislosti na tom, jaké aplikace vyžaduje konfiguraci jednotné přihlašování uvidíte buď možnost Stáhnout soubor XML s metadaty nebo certifikát.</span><span class="sxs-lookup"><span data-stu-id="3be46-270">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="3be46-271">Azure AD neposkytuje adresu URL se získat metadata.</span><span class="sxs-lookup"><span data-stu-id="3be46-271">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="3be46-272">Metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="3be46-272">The metadata can only be retrieved as a XML file.</span></span>

### <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="3be46-273">Postup konfigurace hesel jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="3be46-273">How to configure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="3be46-274">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3be46-274">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="3be46-275">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="3be46-275">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="3be46-276">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3be46-276">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="3be46-277">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="3be46-277">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="3be46-278">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-278">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-279">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="3be46-279">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="3be46-280">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-280">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-281">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-281">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-282">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-282">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-283">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="3be46-283">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="3be46-284">V **zadejte název** textbox z **přidat z Galerie** části, zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-284">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="3be46-285">Vyberte aplikaci, kterou chcete nakonfigurovat pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3be46-285">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="3be46-286">Před přidáním aplikace, můžete změnit její název ze **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3be46-286">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="3be46-287">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-287">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="3be46-288">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-288">After a short period, you be able to see the application’s configuration blade.</span></span>

#### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="3be46-289">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3be46-289">Configure the application for password single sign-on</span></span>

<span data-ttu-id="3be46-290">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3be46-290">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-291">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-291">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3be46-292">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-292">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-293">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-293">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-294">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-294">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-295">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-295">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="3be46-296">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-296">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3be46-297">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3be46-297">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="3be46-298">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-298">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3be46-299">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="3be46-299">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="3be46-300">[Přiřazení uživatelů k aplikaci](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="3be46-300">[Assign users to the application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="3be46-301">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="3be46-301">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="3be46-302">Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="3be46-302">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

### <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="3be46-303">Postup konfigurace hesel jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="3be46-303">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="3be46-304">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3be46-304">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="3be46-305">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="3be46-305">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="3be46-306">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3be46-306">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a><span data-ttu-id="3be46-307">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="3be46-307">Add a non-gallery application</span></span>

<span data-ttu-id="3be46-308">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-308">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-309">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.</span><span class="sxs-lookup"><span data-stu-id="3be46-309">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="3be46-310">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-310">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-311">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-311">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-312">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-312">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-313">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="3be46-313">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="3be46-314">Klikněte na tlačítko **Galerie jiná aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-314">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="3be46-315">Zadejte název vaší aplikace v **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3be46-315">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="3be46-316">Vyberte **přidat.**</span><span class="sxs-lookup"><span data-stu-id="3be46-316">Select **Add.**</span></span>

<span data-ttu-id="3be46-317">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-317">After a short period, you be able to see the application’s configuration blade.</span></span>

#### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="3be46-318">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3be46-318">Configure the application for password single sign-on</span></span>

<span data-ttu-id="3be46-319">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3be46-319">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-320">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-320">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3be46-321">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-321">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-322">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-322">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-323">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-323">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-324">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-324">click **All Applications** to view a list of all your applications.</span></span>

    1.  <span data-ttu-id="3be46-325">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-325">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3be46-326">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3be46-326">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="3be46-327">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-327">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3be46-328">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="3be46-328">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="3be46-329">Zadejte **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="3be46-329">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="3be46-330">Toto je adresa URL, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-330">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="3be46-331">Zkontrolujte, zda že přihlašovací pole jsou viditelné na adrese URL.</span><span class="sxs-lookup"><span data-stu-id="3be46-331">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="3be46-332">[Přiřazení uživatelů k aplikaci](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="3be46-332">[Assign users to the application](#how-to-assign-a-user-to-an-application-directly).</span></span>

11. <span data-ttu-id="3be46-333">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="3be46-333">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="3be46-334">Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="3be46-334">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="problems-related-to-assigning-applications-to-users"></a><span data-ttu-id="3be46-335">Problémy související s přiřazení aplikací pro uživatele</span><span class="sxs-lookup"><span data-stu-id="3be46-335">Problems related to assigning applications to users</span></span>

<span data-ttu-id="3be46-336">Uživatel nemusí být zobrazuje aplikace na jejich přístupového panelu nejsou přiřazeny k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-336">A user may not be seeing an application on their Access Panel because they are not assigned to the application.</span></span> <span data-ttu-id="3be46-337">Tady jsou některé způsoby, jak zkontrolovat:</span><span class="sxs-lookup"><span data-stu-id="3be46-337">Below are some ways to check:</span></span>

-   [<span data-ttu-id="3be46-338">Zkontrolujte, pokud uživatel je přiřazená k aplikaci</span><span class="sxs-lookup"><span data-stu-id="3be46-338">Check if a user is assigned to the application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="3be46-339">Postup přiřazení uživatele k aplikaci přímo</span><span class="sxs-lookup"><span data-stu-id="3be46-339">How to assign a user to an application directly</span></span>](#how-to-assign-a-user-to-an-application-directly)

-   [<span data-ttu-id="3be46-340">Zkontrolujte, pokud uživatel je přiřazená k licenci týkající se aplikace</span><span class="sxs-lookup"><span data-stu-id="3be46-340">Check if a user is assigned to a license related to the application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [<span data-ttu-id="3be46-341">Přiřazení licence pro uživatele</span><span class="sxs-lookup"><span data-stu-id="3be46-341">How to assign a license to a user</span></span>](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-to-the-application"></a><span data-ttu-id="3be46-342">Zkontrolujte, pokud uživatel je přiřazená k aplikaci</span><span class="sxs-lookup"><span data-stu-id="3be46-342">Check if a user is assigned to the application</span></span>

<span data-ttu-id="3be46-343">Pokud chcete zkontrolovat, pokud má uživatel přiřazeno k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-343">To check if a user is assigned to the application, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-344">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-344">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="3be46-345">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-345">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-346">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-346">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-347">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-347">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-348">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-348">click **All Applications** to view a list of all your applications.</span></span>

6.  <span data-ttu-id="3be46-349">**Hledání** pro název dané žádosti.</span><span class="sxs-lookup"><span data-stu-id="3be46-349">**Search** for the name of the application in question.</span></span>

7.  <span data-ttu-id="3be46-350">Klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3be46-350">click **Users and groups**.</span></span>

8.  <span data-ttu-id="3be46-351">Zkontrolujte, pokud uživatel je přiřazena k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-351">Check to see if your user is assigned to the application.</span></span>

   * <span data-ttu-id="3be46-352">Pokud ne, postupujte podle kroků v "Jak přiřazení uživatele k aplikaci přímo" Uděláte to tak.</span><span class="sxs-lookup"><span data-stu-id="3be46-352">If not follow the steps in “How to assign a user to an application directly” to do so.</span></span>

### <a name="how-to-assign-a-user-to-an-application-directly"></a><span data-ttu-id="3be46-353">Postup přiřazení uživatele k aplikaci přímo</span><span class="sxs-lookup"><span data-stu-id="3be46-353">How to assign a user to an application directly</span></span>

<span data-ttu-id="3be46-354">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-354">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-355">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce**.</span><span class="sxs-lookup"><span data-stu-id="3be46-355">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="3be46-356">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-356">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-357">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-357">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-358">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-358">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-359">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-359">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="3be46-360">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-360">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3be46-361">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-361">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="3be46-362">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-362">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3be46-363">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="3be46-363">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="3be46-364">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="3be46-364">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="3be46-365">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="3be46-365">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="3be46-366">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="3be46-366">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="3be46-367">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-367">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="3be46-368">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-368">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="3be46-369">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-369">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="3be46-370">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="3be46-370">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="3be46-371">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3be46-371">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="3be46-372">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="3be46-372">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a><span data-ttu-id="3be46-373">Zkontrolujte, jestli je uživatel v rámci licence týkající se aplikace</span><span class="sxs-lookup"><span data-stu-id="3be46-373">Check if a user is under a license related to the application</span></span>

<span data-ttu-id="3be46-374">Pokud chcete zkontrolovat uživatele přiřazené licence, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-374">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-375">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-375">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="3be46-376">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-376">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-377">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-377">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-378">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="3be46-378">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="3be46-379">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3be46-379">click **All users**.</span></span>

6.  <span data-ttu-id="3be46-380">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="3be46-380">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="3be46-381">Klikněte na tlačítko **licence** zobrazíte, které uživatel aktuálně licence jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="3be46-381">click **Licenses** to see which licenses the user currently has assigned.</span></span>

  * <span data-ttu-id="3be46-382">Pokud uživatel je přiřazen k Office licence touto aplikací povolit první strany Office se objevily na Panel přístupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="3be46-382">If the user is assigned to an Office license this enable First Party Office applications to appear on the user’s Access Panel.</span></span>

### <a name="how-to-assign-a-user-a-license"></a><span data-ttu-id="3be46-383">Postup přiřazení licence uživatele</span><span class="sxs-lookup"><span data-stu-id="3be46-383">How to assign a user a license</span></span> 

<span data-ttu-id="3be46-384">Chcete-li přiřadit licenci pro uživatele, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3be46-384">To assign a license to a user, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-385">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-385">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="3be46-386">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-386">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-387">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-387">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-388">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="3be46-388">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="3be46-389">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3be46-389">click **All users**.</span></span>

6.  <span data-ttu-id="3be46-390">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="3be46-390">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="3be46-391">Klikněte na tlačítko **licence** zobrazíte, které uživatel aktuálně licence jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="3be46-391">click **Licenses** to see which licenses the user currently has assigned.</span></span>

8.  <span data-ttu-id="3be46-392">klikněte **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3be46-392">click the **Assign** button.</span></span>

9.  <span data-ttu-id="3be46-393">Vyberte **jeden nebo více produktů** ze seznamu dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="3be46-393">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="3be46-394">**Volitelné** klikněte na tlačítko **přiřazení možností** položky granularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="3be46-394">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="3be46-395">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="3be46-395">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="3be46-396">Klikněte **přiřadit** tlačítko tyto licence přiřadit tomuto uživateli.</span><span class="sxs-lookup"><span data-stu-id="3be46-396">Click the **Assign** button to assign these licenses to this user.</span></span>

## <a name="problems-related-to-assigning-applications-to-groups"></a><span data-ttu-id="3be46-397">Problémy související s přiřazování aplikací do skupin</span><span class="sxs-lookup"><span data-stu-id="3be46-397">Problems related to assigning applications to groups</span></span>

<span data-ttu-id="3be46-398">Uživatele může být zobrazuje aplikace na jejich přístupového panelu jsou součástí skupiny, která byla přiřazena aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-398">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned the application.</span></span> <span data-ttu-id="3be46-399">Tady jsou některé způsoby, jak zkontrolovat:</span><span class="sxs-lookup"><span data-stu-id="3be46-399">Below are some ways to check:</span></span>

-   [<span data-ttu-id="3be46-400">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="3be46-400">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="3be46-401">Tom, jak přiřadit aplikaci pro skupinu přímo</span><span class="sxs-lookup"><span data-stu-id="3be46-401">How to assign an application to a group directly</span></span>](#how-to-assign-an-application-to-a-group-directly)

-   [<span data-ttu-id="3be46-402">Zkontrolujte, zda uživatel je součástí skupiny přiřazené k licenci</span><span class="sxs-lookup"><span data-stu-id="3be46-402">Check if a user is part of group assigned to a license</span></span>](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [<span data-ttu-id="3be46-403">Postup přiřazení licence do skupiny</span><span class="sxs-lookup"><span data-stu-id="3be46-403">How to assign a license to a group</span></span>](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="3be46-404">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="3be46-404">Check a user’s group memberships</span></span>

<span data-ttu-id="3be46-405">Pokud chcete zkontrolovat členství ve skupině, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-405">To check a group’s membership, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-406">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-406">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="3be46-407">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-407">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-408">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-408">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-409">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="3be46-409">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="3be46-410">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3be46-410">click **All users**.</span></span>

6.  <span data-ttu-id="3be46-411">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="3be46-411">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="3be46-412">Klikněte na tlačítko **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="3be46-412">click **Groups**.</span></span>

8.  <span data-ttu-id="3be46-413">Zkontrolujte, zda uživatel je součástí skupiny přiřazené k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-413">Check to see if your user is part of a Group assigned to the application.</span></span>

  * <span data-ttu-id="3be46-414">Pokud chcete odebrat uživatele ze skupiny, **klikněte na řádek** tuto skupinu a vyberte možnost odstranit.</span><span class="sxs-lookup"><span data-stu-id="3be46-414">If you want to remove the user from the group, **click the row** of the group and select delete.</span></span>

### <a name="how-to-assign-an-application-to-a-group-directly"></a><span data-ttu-id="3be46-415">Tom, jak přiřadit aplikaci pro skupinu přímo</span><span class="sxs-lookup"><span data-stu-id="3be46-415">How to assign an application to a group directly</span></span>

<span data-ttu-id="3be46-416">Jeden nebo více skupin přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-416">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-417">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce**.</span><span class="sxs-lookup"><span data-stu-id="3be46-417">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="3be46-418">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-418">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-419">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-419">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-420">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3be46-420">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3be46-421">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="3be46-421">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="3be46-422">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="3be46-422">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="3be46-423">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-423">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="3be46-424">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="3be46-424">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3be46-425">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="3be46-425">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="3be46-426">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="3be46-426">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="3be46-427">Zadejte **název úplné skupiny** vás zajímá přiřazení do skupiny **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="3be46-427">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="3be46-428">Najeďte myší **skupiny** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="3be46-428">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="3be46-429">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-429">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="3be46-430">**Volitelné:** Pokud byste chtěli **přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko k přidání do této skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="3be46-430">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="3be46-431">Po dokončení výběru skupiny klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3be46-431">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="3be46-432">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit ke skupinám, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="3be46-432">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="3be46-433">Klikněte **přiřadit** tlačítko přiřazení aplikace k vybraným skupinám.</span><span class="sxs-lookup"><span data-stu-id="3be46-433">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="3be46-434">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="3be46-434">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

### <a name="check-if-a-user-is-part-of-group-assigned-to-a-license"></a><span data-ttu-id="3be46-435">Zkontrolujte, zda uživatel je součástí skupiny přiřazené k licenci</span><span class="sxs-lookup"><span data-stu-id="3be46-435">Check if a user is part of group assigned to a license</span></span>

1.  <span data-ttu-id="3be46-436">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-436">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="3be46-437">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-437">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-438">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-438">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-439">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="3be46-439">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="3be46-440">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3be46-440">click **All users**.</span></span>

6.  <span data-ttu-id="3be46-441">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="3be46-441">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="3be46-442">Klikněte na tlačítko **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="3be46-442">click **Groups**.</span></span>

8.  <span data-ttu-id="3be46-443">Klikněte na řádek určité skupiny.</span><span class="sxs-lookup"><span data-stu-id="3be46-443">click the row of a specific group.</span></span>

9.  <span data-ttu-id="3be46-444">Klikněte na tlačítko **licence** zobrazíte, které skupiny licencí má přiřazen.</span><span class="sxs-lookup"><span data-stu-id="3be46-444">click **Licenses** to see which licenses the group has assigned to it.</span></span>

   * <span data-ttu-id="3be46-445">Pokud tato skupina je přiřazená k licenci Office to může povolit konkrétní aplikace Office první strany se objevily na Panel přístupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="3be46-445">If the group is assigned to an Office license this may enable certain First Party Office applications to appear on the user’s Access Panel.</span></span>

### <a name="how-to-assign-a-license-to-a-group"></a><span data-ttu-id="3be46-446">Postup přiřazení licence do skupiny</span><span class="sxs-lookup"><span data-stu-id="3be46-446">How to assign a license to a group</span></span>

<span data-ttu-id="3be46-447">Přiřadit licenci pro skupinu, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="3be46-447">To assign a license to a group, follow the steps below:</span></span>

1.  <span data-ttu-id="3be46-448">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="3be46-448">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="3be46-449">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="3be46-449">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3be46-450">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="3be46-450">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3be46-451">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="3be46-451">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="3be46-452">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="3be46-452">click **All groups**.</span></span>

6.  <span data-ttu-id="3be46-453">**Hledání** pro skupinu vás zajímá a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="3be46-453">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="3be46-454">Klikněte na tlačítko **licence** zobrazíte, které skupiny licencí aktuálně jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="3be46-454">click **Licenses** to see which licenses the group currently has assigned.</span></span>

8.  <span data-ttu-id="3be46-455">klikněte **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3be46-455">click the **Assign** button.</span></span>

9.  <span data-ttu-id="3be46-456">Vyberte **jeden nebo více produktů** ze seznamu dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="3be46-456">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="3be46-457">**Volitelné** klikněte na tlačítko **přiřazení možností** položky granularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="3be46-457">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="3be46-458">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="3be46-458">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="3be46-459">Klikněte **přiřadit** tlačítko přiřadit tyto licence do této skupiny.</span><span class="sxs-lookup"><span data-stu-id="3be46-459">Click the **Assign** button to assign these licenses to this group.</span></span> <span data-ttu-id="3be46-460">To může trvat dlouhou dobu, v závislosti na velikost a složitost skupiny.</span><span class="sxs-lookup"><span data-stu-id="3be46-460">This may take a long time, depending on the size and complexity of the group.</span></span>

>[!NOTE]
><span data-ttu-id="3be46-461">Uděláte to tak rychleji, vezměte v úvahu dočasně přiřazení licence uživatele přímo.</span><span class="sxs-lookup"><span data-stu-id="3be46-461">To do this faster, consider temporarily assigning a license to the user directly.</span></span> 
>
>

## <a name="next-steps"></a><span data-ttu-id="3be46-462">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3be46-462">Next steps</span></span>
[<span data-ttu-id="3be46-463">Přidání nových uživatelů do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3be46-463">Add new users to Azure Active Directory</span></span>](active-directory-users-create-azure-portal.md)

