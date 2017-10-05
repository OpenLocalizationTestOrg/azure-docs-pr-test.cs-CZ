---
title: 'Kurz: Azure Active Directory integrace s Netsuite | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 4a19ab310212b93a53495a6fc6c25c77dfb82e79
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a><span data-ttu-id="0a9eb-103">Kurz: Azure Active Directory integrace s Netsuite</span><span class="sxs-lookup"><span data-stu-id="0a9eb-103">Tutorial: Azure Active Directory integration with Netsuite</span></span>

<span data-ttu-id="0a9eb-104">V tomto kurzu zjistěte, jak integrovat Netsuite s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0a9eb-104">In this tutorial, you learn how to integrate Netsuite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0a9eb-105">Integrace Netsuite s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0a9eb-105">Integrating Netsuite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0a9eb-106">Můžete řídit ve službě Azure AD, který má přístup k Netsuite</span><span class="sxs-lookup"><span data-stu-id="0a9eb-106">You can control in Azure AD who has access to Netsuite</span></span>
- <span data-ttu-id="0a9eb-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Netsuite (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a9eb-107">You can enable your users to automatically get signed-on to Netsuite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0a9eb-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0a9eb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0a9eb-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0a9eb-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a9eb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0a9eb-110">Prerequisites</span></span>

<span data-ttu-id="0a9eb-111">Konfigurace integrace Azure AD s Netsuite, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="0a9eb-111">To configure Azure AD integration with Netsuite, you need the following items:</span></span>

- <span data-ttu-id="0a9eb-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a9eb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0a9eb-113">Netsuite jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0a9eb-113">A Netsuite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0a9eb-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0a9eb-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0a9eb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0a9eb-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0a9eb-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0a9eb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0a9eb-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0a9eb-118">Scenario description</span></span>
<span data-ttu-id="0a9eb-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0a9eb-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0a9eb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0a9eb-121">Přidání Netsuite z Galerie</span><span class="sxs-lookup"><span data-stu-id="0a9eb-121">Adding Netsuite from the gallery</span></span>
2. <span data-ttu-id="0a9eb-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0a9eb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netsuite-from-the-gallery"></a><span data-ttu-id="0a9eb-123">Přidání Netsuite z Galerie</span><span class="sxs-lookup"><span data-stu-id="0a9eb-123">Adding Netsuite from the gallery</span></span>
<span data-ttu-id="0a9eb-124">Při konfiguraci integrace Netsuite do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Netsuite z galerie.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-124">To configure the integration of Netsuite into Azure AD, you need to add Netsuite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0a9eb-125">**Pokud chcete přidat Netsuite z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0a9eb-125">**To add Netsuite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0a9eb-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0a9eb-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0a9eb-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0a9eb-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0a9eb-133">Do vyhledávacího pole zadejte **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-133">In the search box, type **Netsuite**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. <span data-ttu-id="0a9eb-135">Na panelu výsledků vyberte **Netsuite**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-135">In the results panel, select **Netsuite**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0a9eb-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0a9eb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0a9eb-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Netsuite podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0a9eb-138">In this section, you configure and test Azure AD single sign-on with Netsuite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0a9eb-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Netsuite je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Netsuite is to a user in Azure AD.</span></span> <span data-ttu-id="0a9eb-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Netsuite musí navázat.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-140">In other words, a link relationship between an Azure AD user and the related user in Netsuite needs to be established.</span></span>

<span data-ttu-id="0a9eb-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Netsuite.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Netsuite.</span></span>

<span data-ttu-id="0a9eb-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Netsuite, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="0a9eb-142">To configure and test Azure AD single sign-on with Netsuite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0a9eb-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0a9eb-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0a9eb-145">**[Vytvoření zkušebního uživatele Netsuite](#creating-a-netsuite-test-user)**  – Pokud chcete mít protějšek Britta Simon v Netsuite propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-145">**[Creating a Netsuite test user](#creating-a-netsuite-test-user)** - to have a counterpart of Britta Simon in Netsuite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0a9eb-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0a9eb-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0a9eb-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0a9eb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0a9eb-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Netsuite.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Netsuite application.</span></span>

<span data-ttu-id="0a9eb-150">**Ke konfiguraci Azure AD jednotné přihlašování s Netsuite, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0a9eb-150">**To configure Azure AD single sign-on with Netsuite, perform the following steps:**</span></span>

1. <span data-ttu-id="0a9eb-151">Na portálu Azure na **Netsuite** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-151">In the Azure portal, on the **Netsuite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0a9eb-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. <span data-ttu-id="0a9eb-155">Na **Netsuite domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0a9eb-155">On the **Netsuite Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    <span data-ttu-id="0a9eb-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span><span class="sxs-lookup"><span data-stu-id="0a9eb-157">In the **Reply URL** textbox, type a URL using the following pattern:   `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0a9eb-158">Tato hodnota není skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-158">This value is not real value.</span></span> <span data-ttu-id="0a9eb-159">Aktualizujte hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-159">Update the value with the actual Reply URL.</span></span> <span data-ttu-id="0a9eb-160">Obraťte se na [tým podpory Netsuite](http://www.netsuite.com/portal/services/support.shtml) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-160">Contact [Netsuite support team](http://www.netsuite.com/portal/services/support.shtml) to get this value.</span></span>
 
4. <span data-ttu-id="0a9eb-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. <span data-ttu-id="0a9eb-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0a9eb-165">Na **Netsuite konfigurace** klikněte na tlačítko **konfigurace Netsuite** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-165">On the **Netsuite Configuration** section, click **Configure Netsuite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0a9eb-166">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0a9eb-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. <span data-ttu-id="0a9eb-168">V prohlížeči otevřete novou kartu a přihlaste se k serveru vaší společnosti Netsuite jako správce.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-168">Open a new tab in your browser, and sign into your Netsuite company site as an administrator.</span></span>

8. <span data-ttu-id="0a9eb-169">Na panelu nástrojů v horní části stránky klikněte na tlačítko **instalace**, pak klikněte na tlačítko **Správce instalace**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-169">In the toolbar at the top of the page, click **Setup**, then click **Setup Manager**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. <span data-ttu-id="0a9eb-171">Z **úlohy nastavení** seznamu, vyberte **integrace**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-171">From the **Setup Tasks** list, select **Integration**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. <span data-ttu-id="0a9eb-173">V **spravovat ověřování** klikněte na tlačítko **SAML jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-173">In the **Manage Authentication** section, click **SAML Single Sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. <span data-ttu-id="0a9eb-175">Na **SAML instalace** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0a9eb-175">On the **SAML Setup** page, perform the following steps:</span></span>
   
    <span data-ttu-id="0a9eb-176">a.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-176">a.</span></span> <span data-ttu-id="0a9eb-177">Kopírování **SAML jeden přihlašování adresa URL služby** z hodnoty **Stručná referenční příručka** části **konfigurovat přihlášení** a vložte ji do **stránka přihlášení zprostředkovatele Identity** pole Netsuite.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-177">Copy the **SAML Single Sign-On Service URL** value from **Quick Reference** section of **Configure sign-on** and paste it into the **Identity Provider Login Page** field in Netsuite.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    <span data-ttu-id="0a9eb-179">b.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-179">b.</span></span> <span data-ttu-id="0a9eb-180">V Netsuite, vyberte **primární metoda ověřování**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-180">In Netsuite, select **Primary Authentication Method**.</span></span>

    <span data-ttu-id="0a9eb-181">c.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-181">c.</span></span> <span data-ttu-id="0a9eb-182">Pro pole s názvem bez přípony **metadat zprostředkovatelů Identity SAMLV2**, vyberte **nahrát soubor metadat IDP**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-182">For the field labeled **SAMLV2 Identity Provider Metadata**, select **Upload IDP Metadata File**.</span></span> <span data-ttu-id="0a9eb-183">Pak klikněte na tlačítko **Procházet** nahrát soubor metadat, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-183">Then click **Browse** to upload the metadata file that you downloaded from Azure portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    <span data-ttu-id="0a9eb-185">d.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-185">d.</span></span> <span data-ttu-id="0a9eb-186">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-186">Click **Submit**.</span></span>

12. <span data-ttu-id="0a9eb-187">Ve službě Azure AD, klikněte na **zobrazit a upravit všechny ostatní atributy uživatele** zaškrtávací políčko a přidání atributu.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-187">In Azure AD, Click on **View and edit all other user attributes** check-box and add attribute.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. <span data-ttu-id="0a9eb-189">Pro **název atributu** pole, zadejte v `account`.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-189">For the **Attribute Name** field, type in `account`.</span></span> <span data-ttu-id="0a9eb-190">Pro **hodnota atributu** pole, zadejte v ID Netsuite účtu. Tato hodnota je konstanta a změňte pomocí účtu.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-190">For the **Attribute Value** field, type in your Netsuite account ID.This value is constant and change with account.</span></span> <span data-ttu-id="0a9eb-191">Pokyny o tom, jak najít ID vašeho účtu jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="0a9eb-191">Instructions on how to find your account ID are included below:</span></span>

      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    <span data-ttu-id="0a9eb-193">a.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-193">a.</span></span> <span data-ttu-id="0a9eb-194">V Netsuite, klikněte na **instalační program** v horním navigačním panelu nabídce.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-194">In Netsuite, click **Setup** from the top navigation menu.</span></span>

    <span data-ttu-id="0a9eb-195">b.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-195">b.</span></span> <span data-ttu-id="0a9eb-196">Klikněte v části **instalační úlohy, které** levé navigační nabídky, vyberte **integrace** a klikněte na **webové služby Předvolby**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-196">Then click under the **Setup Tasks** section of the left navigation menu, select the **Integration** section, and click **Web Services Preferences**.</span></span>

    <span data-ttu-id="0a9eb-197">c.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-197">c.</span></span> <span data-ttu-id="0a9eb-198">Vaše ID pro účet Netsuite zkopírujte a vložte ji do **hodnota atributu** pole ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-198">Copy your Netsuite Account ID and paste it into the **Attribute Value** field in Azure AD.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. <span data-ttu-id="0a9eb-200">Předtím, než mohou uživatelé provádět jednotné přihlašování do Netsuite, je třeba nejprve je přiřadit v Netsuite příslušná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-200">Before users can perform single sign-on into Netsuite, they must first be assigned the appropriate permissions in Netsuite.</span></span> <span data-ttu-id="0a9eb-201">Postupujte podle pokynů pro toto přiřazení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-201">Follow the instructions below to assign these permissions.</span></span>

    <span data-ttu-id="0a9eb-202">a.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-202">a.</span></span> <span data-ttu-id="0a9eb-203">V nabídce horním navigačním panelu klikněte na tlačítko **instalace**, pak klikněte na tlačítko **Správce instalace**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-203">On the top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="0a9eb-205">b.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-205">b.</span></span> <span data-ttu-id="0a9eb-206">V levém navigačním nabídce vyberte **uživatele/role**, pak klikněte na tlačítko **spravovat role**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-206">On the left navigation menu, select **Users/Roles**, then click **Manage Roles**.</span></span>
      
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    <span data-ttu-id="0a9eb-208">c.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-208">c.</span></span> <span data-ttu-id="0a9eb-209">Klikněte na tlačítko **novou roli**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-209">Click **New Role**.</span></span>

    <span data-ttu-id="0a9eb-210">d.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-210">d.</span></span> <span data-ttu-id="0a9eb-211">Zadejte **název** pro novou roli a vyberte **jednoho přihlášení pouze** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-211">Type in a **Name** for your new role, and select the **Single Sign-On Only** checkbox.</span></span>
      
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    <span data-ttu-id="0a9eb-213">e.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-213">e.</span></span> <span data-ttu-id="0a9eb-214">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-214">Click **Save**.</span></span>

    <span data-ttu-id="0a9eb-215">f.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-215">f.</span></span> <span data-ttu-id="0a9eb-216">V nabídce v horní části, klikněte na tlačítko **oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-216">In the menu on the top, click **Permissions**.</span></span> <span data-ttu-id="0a9eb-217">Pak klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-217">Then click **Setup**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    <span data-ttu-id="0a9eb-219">g.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-219">g.</span></span> <span data-ttu-id="0a9eb-220">Vyberte **nastavit až SAM jednotné přihlašování**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-220">Select **Set Up SAM Single Sign-on**, and then click **Add**.</span></span>

    <span data-ttu-id="0a9eb-221">h.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-221">h.</span></span> <span data-ttu-id="0a9eb-222">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-222">Click **Save**.</span></span>

    <span data-ttu-id="0a9eb-223">i.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-223">i.</span></span> <span data-ttu-id="0a9eb-224">V nabídce horním navigačním panelu klikněte na tlačítko **instalace**, pak klikněte na tlačítko **Správce instalace**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-224">On the top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="0a9eb-226">j.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-226">j.</span></span> <span data-ttu-id="0a9eb-227">V levém navigačním nabídce vyberte **uživatele/role**, pak klikněte na tlačítko **spravovat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-227">On the left navigation menu, select **Users/Roles**, then click **Manage Users**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    <span data-ttu-id="0a9eb-229">kB.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-229">k.</span></span> <span data-ttu-id="0a9eb-230">Vyberte testovací uživatele.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-230">Select a test user.</span></span> <span data-ttu-id="0a9eb-231">Pak klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-231">Then click **Edit**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    <span data-ttu-id="0a9eb-233">l.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-233">l.</span></span> <span data-ttu-id="0a9eb-234">V dialogovém okně rolí vyberte roli, který jste vytvořili a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-234">On the Roles dialog, select the role that you have created and click **Add**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    <span data-ttu-id="0a9eb-236">m.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-236">m.</span></span> <span data-ttu-id="0a9eb-237">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-237">Click **Save**.</span></span>
    
> [!TIP]
> <span data-ttu-id="0a9eb-238">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="0a9eb-238">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0a9eb-239">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-239">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0a9eb-240">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0a9eb-240">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0a9eb-241">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a9eb-241">Creating an Azure AD test user</span></span>
<span data-ttu-id="0a9eb-242">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-242">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0a9eb-244">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0a9eb-244">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0a9eb-245">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-245">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="0a9eb-247">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-247">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0a9eb-249">V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-249">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0a9eb-251">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0a9eb-251">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0a9eb-253">a.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-253">a.</span></span> <span data-ttu-id="0a9eb-254">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-254">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0a9eb-255">b.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-255">b.</span></span> <span data-ttu-id="0a9eb-256">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-256">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0a9eb-257">c.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-257">c.</span></span> <span data-ttu-id="0a9eb-258">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-258">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0a9eb-259">d.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-259">d.</span></span> <span data-ttu-id="0a9eb-260">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-260">Click **Create**.</span></span> 

### <a name="creating-a-netsuite-test-user"></a><span data-ttu-id="0a9eb-261">Vytvoření zkušebního uživatele Netsuite</span><span class="sxs-lookup"><span data-stu-id="0a9eb-261">Creating a Netsuite test user</span></span>

<span data-ttu-id="0a9eb-262">V této části se uživatel volá Britta Simon vytvoří v Netsuite.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-262">In this section, a user called Britta Simon is created in Netsuite.</span></span> <span data-ttu-id="0a9eb-263">Netsuite podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-263">Netsuite supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="0a9eb-264">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-264">There is no action item for you in this section.</span></span> <span data-ttu-id="0a9eb-265">Pokud uživatel v Netsuite ještě neexistuje, vytvoří se při pokusu o přístup k Netsuite novou.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-265">If a user doesn't already exist in Netsuite, a new one is created when you attempt to access Netsuite.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0a9eb-266">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a9eb-266">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0a9eb-267">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Netsuite.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-267">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Netsuite.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0a9eb-269">**Pokud chcete přiřadit Britta Simon Netsuite, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0a9eb-269">**To assign Britta Simon to Netsuite, perform the following steps:**</span></span>

1. <span data-ttu-id="0a9eb-270">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-270">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0a9eb-272">V seznamu aplikací vyberte **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-272">In the applications list, select **Netsuite**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. <span data-ttu-id="0a9eb-274">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-274">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0a9eb-276">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-276">Click **Add** button.</span></span> <span data-ttu-id="0a9eb-277">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0a9eb-279">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-279">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0a9eb-280">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0a9eb-281">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0a9eb-282">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0a9eb-282">Testing single sign-on</span></span>

<span data-ttu-id="0a9eb-283">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-283">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0a9eb-284">K testování vaše nastavení jednotného přihlašování, otevřete Panel přístupu na [https://myapps.microsoft.com](https://myapps.microsoft.com/), přihlaste se k testovací účet a klikněte na tlačítko **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="0a9eb-284">To test your single sign-on settings, open the Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), sign into the test account, and click **Netsuite**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a9eb-285">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0a9eb-285">Additional resources</span></span>

* [<span data-ttu-id="0a9eb-286">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0a9eb-286">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0a9eb-287">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0a9eb-287">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="0a9eb-288">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="0a9eb-288">Configure User Provisioning</span></span>](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

