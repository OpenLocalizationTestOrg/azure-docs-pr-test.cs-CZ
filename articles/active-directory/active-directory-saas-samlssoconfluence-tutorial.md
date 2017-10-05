---
title: "Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML pro soutoku podle řešení GmbH | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování SAML pro soutoku podle řešení GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 9a36d686ba39b5168860a20e8c4db357888df6a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a><span data-ttu-id="ab8c4-103">Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML pro soutoku podle řešení GmbH</span><span class="sxs-lookup"><span data-stu-id="ab8c4-103">Tutorial: Azure Active Directory integration with SAML SSO for Confluence by resolution GmbH</span></span>

<span data-ttu-id="ab8c4-104">V tomto kurzu zjistěte, jak integrovat jednotné přihlašování SAML pro soutoku podle řešení GmbH službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ab8c4-104">In this tutorial, you learn how to integrate SAML SSO for Confluence by resolution GmbH with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ab8c4-105">Integrace jednotné přihlašování SAML pro soutoku podle řešení GmbH s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-105">Integrating SAML SSO for Confluence by resolution GmbH with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ab8c4-106">Můžete řídit ve službě Azure AD, kdo má přístup k jednotné přihlašování SAML pro soutoku podle řešení GmbH</span><span class="sxs-lookup"><span data-stu-id="ab8c4-106">You can control in Azure AD who has access to SAML SSO for Confluence by resolution GmbH</span></span>
- <span data-ttu-id="ab8c4-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k jednotné přihlašování SAML pro soutoku podle řešení GmbH (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ab8c4-107">You can enable your users to automatically get signed-on to SAML SSO for Confluence by resolution GmbH (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ab8c4-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ab8c4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ab8c4-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ab8c4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab8c4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ab8c4-110">Prerequisites</span></span>

<span data-ttu-id="ab8c4-111">Konfigurace integrace Azure AD pomocí jednotného přihlašování SAML pro soutoku pomocí řešení GmbH, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-111">To configure Azure AD integration with SAML SSO for Confluence by resolution GmbH, you need the following items:</span></span>

- <span data-ttu-id="ab8c4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ab8c4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ab8c4-113">Jednotné přihlašování SAML pro soutoku podle řešení GmbH jednotné přihlašování v předplatném povolené</span><span class="sxs-lookup"><span data-stu-id="ab8c4-113">A SAML SSO for Confluence by resolution GmbH single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ab8c4-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ab8c4-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ab8c4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ab8c4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ab8c4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ab8c4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ab8c4-118">Scenario description</span></span>
<span data-ttu-id="ab8c4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ab8c4-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ab8c4-121">Přidání jednotné přihlašování SAML pro soutoku podle řešení GmbH z Galerie</span><span class="sxs-lookup"><span data-stu-id="ab8c4-121">Adding SAML SSO for Confluence by resolution GmbH from the gallery</span></span>
2. <span data-ttu-id="ab8c4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ab8c4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-the-gallery"></a><span data-ttu-id="ab8c4-123">Přidání jednotné přihlašování SAML pro soutoku podle řešení GmbH z Galerie</span><span class="sxs-lookup"><span data-stu-id="ab8c4-123">Adding SAML SSO for Confluence by resolution GmbH from the gallery</span></span>

<span data-ttu-id="ab8c4-124">Při konfiguraci integrace jednotné přihlašování SAML pro soutoku podle řešení GmbH do služby Azure AD, musíte přidat jednotné přihlašování SAML pro soutoku řešení GmbH z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-124">To configure the integration of SAML SSO for Confluence by resolution GmbH into Azure AD, you need to add SAML SSO for Confluence by resolution GmbH from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ab8c4-125">**Přidání jednotné přihlašování SAML pro soutoku řešení GmbH z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ab8c4-125">**To add SAML SSO for Confluence by resolution GmbH from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ab8c4-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ab8c4-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ab8c4-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ab8c4-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ab8c4-133">Do vyhledávacího pole zadejte **jednotné přihlašování SAML pro soutoku podle řešení GmbH**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-133">In the search box, type **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

5. <span data-ttu-id="ab8c4-135">Na panelu výsledků vyberte **jednotné přihlašování SAML pro soutoku podle řešení GmbH**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-135">In the results panel, select **SAML SSO for Confluence by resolution GmbH**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ab8c4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ab8c4-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="ab8c4-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML pro soutoku pomocí řešení, které GmbH podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="ab8c4-138">In this section, you configure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ab8c4-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, jaké příslušného uživatele v jednotné přihlašování SAML pro soutoku pomocí řešení GmbH je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SAML SSO for Confluence by resolution GmbH is to a user in Azure AD.</span></span> <span data-ttu-id="ab8c4-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatele v jednotné přihlašování SAML pro soutoku pomocí řešení GmbH musí být vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-140">In other words, a link relationship between an Azure AD user and the related user in SAML SSO for Confluence by resolution GmbH needs to be established.</span></span>

<span data-ttu-id="ab8c4-141">V jednotné přihlašování SAML pro soutoku podle řešení GmbH, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-141">In SAML SSO for Confluence by resolution GmbH, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ab8c4-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML pro soutoku podle řešení GmbH, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-142">To configure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ab8c4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ab8c4-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ab8c4-145">**[Vytváření jednotné přihlašování SAML pro soutoku uživatelem testovací GmbH řešení](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  – Pokud chcete mít protějšek Britta Simon v jednotné přihlašování SAML pro soutoku podle řešení GmbH propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-145">**[Creating a SAML SSO for Confluence by resolution GmbH test user](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)** - to have a counterpart of Britta Simon in SAML SSO for Confluence by resolution GmbH that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ab8c4-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ab8c4-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ab8c4-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ab8c4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ab8c4-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší jednotné přihlašování SAML pro soutoku podle řešení GmbH aplikace.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAML SSO for Confluence by resolution GmbH application.</span></span>

<span data-ttu-id="ab8c4-150">**Konfigurace Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML pro soutoku pomocí řešení GmbH, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ab8c4-150">**To configure Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, perform the following steps:**</span></span>

1. <span data-ttu-id="ab8c4-151">Na portálu Azure na **jednotné přihlašování SAML pro soutoku podle řešení GmbH** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-151">In the Azure portal, on the **SAML SSO for Confluence by resolution GmbH** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ab8c4-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

3. <span data-ttu-id="ab8c4-155">Na **jednotné přihlašování SAML soutoku podle řešení GmbH domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-155">On the **SAML SSO for Confluence by resolution GmbH Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    <span data-ttu-id="ab8c4-157">a.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-157">a.</span></span> <span data-ttu-id="ab8c4-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="ab8c4-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

    <span data-ttu-id="ab8c4-159">b.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-159">b.</span></span> <span data-ttu-id="ab8c4-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="ab8c4-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

4. <span data-ttu-id="ab8c4-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="ab8c4-162">Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    <span data-ttu-id="ab8c4-164">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="ab8c4-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="ab8c4-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-165">These values are not real.</span></span> <span data-ttu-id="ab8c4-166">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-166">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="ab8c4-167">Obraťte se na [tým podpory jednotné přihlašování SAML pro soutoku podle řešení GmbH klienta](https://www.resolution.de/go/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-167">Contact [SAML SSO for Confluence by resolution GmbH Client support team](https://www.resolution.de/go/support) to get these values.</span></span> 

5. <span data-ttu-id="ab8c4-168">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

6. <span data-ttu-id="ab8c4-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_400.png)  
    
7. <span data-ttu-id="ab8c4-172">V okně prohlížeče jiný web, přihlaste se k vaší **jednotné přihlašování SAML pro soutoku pomocí portálu pro správu GmbH řešení** jako správce.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-172">In a different web browser window, log in to your **SAML SSO for Confluence by resolution GmbH admin portal** as an administrator.</span></span>

8. <span data-ttu-id="ab8c4-173">Pozastavte ukazatel myši na ikonu a klikněte na **doplňky**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-173">Hover on cog and click the **Add-ons**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon1.png)

9. <span data-ttu-id="ab8c4-175">Budete přesměrováni na stránku přístup správce.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-175">You are redirected to Administrator Access page.</span></span> <span data-ttu-id="ab8c4-176">Zadejte heslo a klikněte na tlačítko **potvrdit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-176">Enter the password and click **Confirm** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon2.png)

10. <span data-ttu-id="ab8c4-178">V části **ATLASSIAN MARKETPLACE** , klikněte na **najít nové rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-178">Under **ATLASSIAN MARKETPLACE** tab, click **Find new add-ons**.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon.png)

11. <span data-ttu-id="ab8c4-180">Hledání **SAML jednotné přihlašování na (SSO) pro soutoku** a klikněte na tlačítko **nainstalovat** tlačítko k instalaci nové zásuvný modul SAML.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-180">Search **SAML Single Sign On (SSO) for Confluence** and click **Install** button to install the new SAML plugin.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon7.png)

12. <span data-ttu-id="ab8c4-182">Spustí se instalace modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-182">The plugin installation will start.</span></span> <span data-ttu-id="ab8c4-183">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-183">Click **Close**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon8.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon9.png)

13. <span data-ttu-id="ab8c4-186">Klikněte na **Manage** (Spravovat).</span><span class="sxs-lookup"><span data-stu-id="ab8c4-186">Click **Manage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon10.png)
    
14. <span data-ttu-id="ab8c4-188">Klikněte na tlačítko **konfigurace** konfigurace nového modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-188">Click **Configure** to configure the new plugin.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon11.png)

15. <span data-ttu-id="ab8c4-190">Tento nový modul plug-in můžete také najít v části **uživatelů a zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-190">This new plugin can also be found under **USERS & SECURITY** tab.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon3.png)
    
16. <span data-ttu-id="ab8c4-192">Na **konfigurace modulu plug-in SingleSignOn SAML** klikněte na tlačítko **přidat další zprostředkovatele Identity** tlačítko ke konfiguraci nastavení zprostředkovatele Identity.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-192">On **SAML SingleSignOn Plugin Configuration** page, click **Add additional Identity Provider** button to configure the settings of Identity Provider.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon4.png)

17. <span data-ttu-id="ab8c4-194">Proveďte následující kroky na této stránce:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-194">Perform following steps on this page:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon5.png)
 
    <span data-ttu-id="ab8c4-196">a.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-196">a.</span></span> <span data-ttu-id="ab8c4-197">Přidat **název** zprostředkovatele Identity (např. Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ab8c4-197">Add **Name** of the Identity Provider (e.g Azure AD).</span></span>
    
    <span data-ttu-id="ab8c4-198">b.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-198">b.</span></span> <span data-ttu-id="ab8c4-199">Přidat **popis** zprostředkovatele Identity (např. Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ab8c4-199">Add **Description** of the Identity Provider (e.g Azure AD).</span></span>

    <span data-ttu-id="ab8c4-200">c.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-200">c.</span></span> <span data-ttu-id="ab8c4-201">Klikněte na tlačítko **XML** a vyberte **Metadata** soubor, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-201">Click **XML** and select the **Metadata** file that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="ab8c4-202">d.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-202">d.</span></span> <span data-ttu-id="ab8c4-203">Klikněte na tlačítko **zatížení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-203">Click **Load** button.</span></span>

    <span data-ttu-id="ab8c4-204">e.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-204">e.</span></span> <span data-ttu-id="ab8c4-205">Přečte IdP metadata a naplní pole, jak je znázorněno na snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-205">It reads the IdP metadata and populates the fields as highlighted in the screenshot.</span></span> 
18. <span data-ttu-id="ab8c4-206">Klikněte na tlačítko **uložit nastavení** tlačítko pro uložení nastavení.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-206">Click **Save settings** button to save the settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon6.png)

> [!TIP]
> <span data-ttu-id="ab8c4-208">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ab8c4-208">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ab8c4-209">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-209">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ab8c4-210">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ab8c4-210">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ab8c4-211">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ab8c4-211">Creating an Azure AD test user</span></span>
<span data-ttu-id="ab8c4-212">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-212">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ab8c4-214">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ab8c4-214">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ab8c4-215">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-215">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ab8c4-217">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-217">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ab8c4-219">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-219">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ab8c4-221">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-221">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ab8c4-223">a.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-223">a.</span></span> <span data-ttu-id="ab8c4-224">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-224">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ab8c4-225">b.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-225">b.</span></span> <span data-ttu-id="ab8c4-226">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-226">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ab8c4-227">c.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-227">c.</span></span> <span data-ttu-id="ab8c4-228">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-228">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ab8c4-229">d.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-229">d.</span></span> <span data-ttu-id="ab8c4-230">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-230">Click **Create**.</span></span>
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a><span data-ttu-id="ab8c4-231">Vytváření jednotné přihlašování SAML pro soutoku řešení GmbH testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="ab8c4-231">Creating a SAML SSO for Confluence by resolution GmbH test user</span></span>

<span data-ttu-id="ab8c4-232">Pokud chcete povolit uživatelům Azure AD přihlášení pro jednotné přihlašování SAML pro soutoku řešení GmbH, se musí být zřízená do jednotné přihlašování SAML pro soutoku podle řešení GmbH.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-232">To enable Azure AD users to log in to SAML SSO for Confluence by resolution GmbH, they must be provisioned into SAML SSO for Confluence by resolution GmbH.</span></span>  
<span data-ttu-id="ab8c4-233">V jednotné přihlašování SAML pro soutoku podle řešení GmbH zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-233">In SAML SSO for Confluence by resolution GmbH, provisioning is a manual task.</span></span>

<span data-ttu-id="ab8c4-234">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ab8c4-234">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="ab8c4-235">Přihlaste se k vaší SAML jednotného přihlašování pro soutoku řešení GmbH společnosti lokalita jako správce.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-235">Log in to your SAML SSO for Confluence by resolution GmbH company site as an administrator.</span></span>

2. <span data-ttu-id="ab8c4-236">Pozastavte ukazatel myši na ikonu a klikněte na **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-236">Hover on cog and click the **User management**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-samlssoconfluence-tutorial/user1.png) 

3. <span data-ttu-id="ab8c4-238">V části uživatelé klikněte na tlačítko **přidat uživatele** kartě.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-238">Under Users section, click **Add users** tab.</span></span> <span data-ttu-id="ab8c4-239">Na **"Přidat uživatele"** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ab8c4-239">On the **“Add a User”** dialog page, perform the following steps:</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-samlssoconfluence-tutorial/user2.png) 

    <span data-ttu-id="ab8c4-241">a.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-241">a.</span></span> <span data-ttu-id="ab8c4-242">V **uživatelské jméno** textovému poli, zadejte e-mailu uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-242">In the **Username** textbox, type the email of user like Britta Simon.</span></span>

    <span data-ttu-id="ab8c4-243">b.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-243">b.</span></span> <span data-ttu-id="ab8c4-244">V **úplný název** textovému poli, zadejte úplný název tohoto uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-244">In the **Full Name** textbox, type the full name of user like Britta Simon.</span></span>

    <span data-ttu-id="ab8c4-245">c.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-245">c.</span></span> <span data-ttu-id="ab8c4-246">V **e-mailu** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-246">In the **Email** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="ab8c4-247">d.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-247">d.</span></span> <span data-ttu-id="ab8c4-248">V **heslo** textovému poli, zadejte heslo pro Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-248">In the **Password** textbox, type the password for Britta Simon.</span></span>

    <span data-ttu-id="ab8c4-249">e.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-249">e.</span></span> <span data-ttu-id="ab8c4-250">Klikněte na tlačítko **Potvrdit heslo** znovu zadat heslo.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-250">Click **Confirm Password** reenter the password.</span></span>
    
    <span data-ttu-id="ab8c4-251">f.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-251">f.</span></span> <span data-ttu-id="ab8c4-252">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-252">Click **Add** button.</span></span>    

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ab8c4-253">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ab8c4-253">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ab8c4-254">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí jednotného přihlašování SAML pro soutoku udělení přístupu podle řešení GmbH.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-254">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAML SSO for Confluence by resolution GmbH.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ab8c4-256">**Pokud chcete přiřadit Britta Simon jednotné přihlašování SAML pro soutoku podle řešení GmbH, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ab8c4-256">**To assign Britta Simon to SAML SSO for Confluence by resolution GmbH, perform the following steps:**</span></span>

1. <span data-ttu-id="ab8c4-257">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-257">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ab8c4-259">V seznamu aplikací vyberte **jednotné přihlašování SAML pro soutoku podle řešení GmbH**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-259">In the applications list, select **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

3. <span data-ttu-id="ab8c4-261">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-261">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ab8c4-263">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-263">Click **Add** button.</span></span> <span data-ttu-id="ab8c4-264">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-264">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ab8c4-266">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-266">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ab8c4-267">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-267">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ab8c4-268">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-268">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ab8c4-269">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ab8c4-269">Testing single sign-on</span></span>

<span data-ttu-id="ab8c4-270">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-270">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ab8c4-271">Po kliknutí na tlačítko jednotné přihlašování SAML pro soutoku pomocí dlaždice GmbH řešení na přístupovém panelu jste měli získat automaticky přihlášení k vaší jednotné přihlašování SAML pro soutoku podle řešení GmbH aplikace.</span><span class="sxs-lookup"><span data-stu-id="ab8c4-271">When you click the SAML SSO for Confluence by resolution GmbH tile in the Access Panel, you should get automatically signed-on to your SAML SSO for Confluence by resolution GmbH application.</span></span>
<span data-ttu-id="ab8c4-272">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ab8c4-272">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ab8c4-273">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ab8c4-273">Additional resources</span></span>

* [<span data-ttu-id="ab8c4-274">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ab8c4-274">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ab8c4-275">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ab8c4-275">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_203.png

