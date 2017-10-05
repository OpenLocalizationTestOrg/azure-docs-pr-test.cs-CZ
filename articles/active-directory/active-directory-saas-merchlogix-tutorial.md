---
title: 'Kurz: Azure Active Directory integrace s Merchlogix | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Merchlogix."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a1f49bb8-6b17-433d-8f25-9d26fb390e77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: jeedes
ms.openlocfilehash: 44fc8226480cafc130720fbe78aa85ee95caec6c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-merchlogix"></a><span data-ttu-id="a8821-103">Kurz: Azure Active Directory integrace s Merchlogix</span><span class="sxs-lookup"><span data-stu-id="a8821-103">Tutorial: Azure Active Directory integration with Merchlogix</span></span>

<span data-ttu-id="a8821-104">V tomto kurzu zjistěte, jak integrovat Merchlogix s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a8821-104">In this tutorial, you learn how to integrate Merchlogix with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a8821-105">Integrace Merchlogix s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a8821-105">Integrating Merchlogix with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a8821-106">Můžete ovládat ve službě Azure AD, který má přístup k Merchlogix.</span><span class="sxs-lookup"><span data-stu-id="a8821-106">You can control in Azure AD who has access to Merchlogix.</span></span>
- <span data-ttu-id="a8821-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Merchlogix (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8821-107">You can enable your users to automatically get signed-on to Merchlogix (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="a8821-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a8821-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="a8821-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a8821-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8821-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a8821-110">Prerequisites</span></span>

<span data-ttu-id="a8821-111">Konfigurace integrace Azure AD s Merchlogix, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a8821-111">To configure Azure AD integration with Merchlogix, you need the following items:</span></span>

- <span data-ttu-id="a8821-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8821-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a8821-113">Merchlogix jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a8821-113">A Merchlogix single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a8821-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a8821-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a8821-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a8821-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a8821-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a8821-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a8821-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8821-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a8821-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a8821-118">Scenario description</span></span>
<span data-ttu-id="a8821-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a8821-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a8821-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a8821-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a8821-121">Přidání Merchlogix z Galerie</span><span class="sxs-lookup"><span data-stu-id="a8821-121">Adding Merchlogix from the gallery</span></span>
2. <span data-ttu-id="a8821-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a8821-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-merchlogix-from-the-gallery"></a><span data-ttu-id="a8821-123">Přidání Merchlogix z Galerie</span><span class="sxs-lookup"><span data-stu-id="a8821-123">Adding Merchlogix from the gallery</span></span>
<span data-ttu-id="a8821-124">Při konfiguraci integrace Merchlogix do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Merchlogix z galerie.</span><span class="sxs-lookup"><span data-stu-id="a8821-124">To configure the integration of Merchlogix into Azure AD, you need to add Merchlogix from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a8821-125">**Pokud chcete přidat Merchlogix z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a8821-125">**To add Merchlogix from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a8821-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a8821-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="a8821-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a8821-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a8821-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a8821-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="a8821-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8821-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="a8821-133">Do vyhledávacího pole zadejte **Merchlogix**, vyberte **Merchlogix** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a8821-133">In the search box, type **Merchlogix**, select **Merchlogix** from result panel then click **Add** button to add the application.</span></span>

    ![Merchlogix v seznamu výsledků](./media/active-directory-saas-merchlogix-tutorial/tutorial_merchlogix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a8821-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a8821-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="a8821-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Merchlogix podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a8821-136">In this section, you configure and test Azure AD single sign-on with Merchlogix based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a8821-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Merchlogix je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8821-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Merchlogix is to a user in Azure AD.</span></span> <span data-ttu-id="a8821-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Merchlogix musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a8821-138">In other words, a link relationship between an Azure AD user and the related user in Merchlogix needs to be established.</span></span>

<span data-ttu-id="a8821-139">V Merchlogix, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a8821-139">In Merchlogix, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a8821-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Merchlogix, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a8821-140">To configure and test Azure AD single sign-on with Merchlogix, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a8821-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a8821-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a8821-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8821-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a8821-143">**[Vytvoření zkušebního uživatele Merchlogix](#create-a-merchlogix-test-user)**  – Pokud chcete mít protějšek Britta Simon v Merchlogix propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a8821-143">**[Create a Merchlogix test user](#create-a-merchlogix-test-user)** - to have a counterpart of Britta Simon in Merchlogix that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a8821-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a8821-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a8821-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a8821-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a8821-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a8821-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a8821-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Merchlogix.</span><span class="sxs-lookup"><span data-stu-id="a8821-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Merchlogix application.</span></span>

<span data-ttu-id="a8821-148">**Ke konfiguraci Azure AD jednotné přihlašování s Merchlogix, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a8821-148">**To configure Azure AD single sign-on with Merchlogix, perform the following steps:**</span></span>

1. <span data-ttu-id="a8821-149">Na portálu Azure na **Merchlogix** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a8821-149">In the Azure portal, on the **Merchlogix** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="a8821-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a8821-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-merchlogix-tutorial/tutorial_merchlogix_samlbase.png)

3. <span data-ttu-id="a8821-153">Na **Merchlogix domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a8821-153">On the **Merchlogix Domain and URLs** section, perform the following steps:</span></span>

    ![Merchlogix domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-merchlogix-tutorial/tutorial_merchlogix_url.png)

    <span data-ttu-id="a8821-155">a.</span><span class="sxs-lookup"><span data-stu-id="a8821-155">a.</span></span> <span data-ttu-id="a8821-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<DOMAIN>/login.php?saml=true`</span><span class="sxs-lookup"><span data-stu-id="a8821-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<DOMAIN>/login.php?saml=true`</span></span>

    <span data-ttu-id="a8821-157">b.</span><span class="sxs-lookup"><span data-stu-id="a8821-157">b.</span></span> <span data-ttu-id="a8821-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<DOMAIN>/simplesaml/module.php/saml/sp/metadata.php/<SAML_NAME>`</span><span class="sxs-lookup"><span data-stu-id="a8821-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<DOMAIN>/simplesaml/module.php/saml/sp/metadata.php/<SAML_NAME>`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="a8821-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a8821-159">These values are not real.</span></span> <span data-ttu-id="a8821-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a8821-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a8821-161">Obraťte se na [tým podpory Merchlogix](http://www.merchlogix.com/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a8821-161">Contact [Merchlogix support team](http://www.merchlogix.com/contact/) to get these values.</span></span>

4. <span data-ttu-id="a8821-162">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="a8821-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-merchlogix-tutorial/tutorial_merchlogix_certificate.png) 

5. <span data-ttu-id="a8821-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a8821-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-merchlogix-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a8821-166">Na **Merchlogix konfigurace** klikněte na tlačítko **konfigurace Merchlogix** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a8821-166">On the **Merchlogix Configuration** section, click **Configure Merchlogix** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a8821-167">Kopírování **Sign-Out adresu URL, SAML Entity ID,** a **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a8821-167">Copy the **Sign-Out URL, SAML Entity ID,** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Merchlogix](./media/active-directory-saas-merchlogix-tutorial/tutorial_merchlogix_configure.png) 

7. <span data-ttu-id="a8821-169">Konfigurace jednotného přihlašování na **Merchlogix** straně, budete muset odeslat stažené **certifikátu (Base64)**, **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby**k [tým podpory Merchlogix](http://www.merchlogix.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="a8821-169">To configure single sign-on on **Merchlogix** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Merchlogix support team](http://www.merchlogix.com/contact/).</span></span> <span data-ttu-id="a8821-170">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="a8821-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="a8821-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a8821-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a8821-172">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a8821-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a8821-173">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a8821-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a8821-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8821-174">Create an Azure AD test user</span></span>

<span data-ttu-id="a8821-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8821-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="a8821-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a8821-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a8821-178">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a8821-178">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-merchlogix-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="a8821-180">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a8821-180">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-merchlogix-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="a8821-182">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8821-182">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-merchlogix-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="a8821-184">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a8821-184">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-merchlogix-tutorial/create_aaduser_04.png)

    <span data-ttu-id="a8821-186">a.</span><span class="sxs-lookup"><span data-stu-id="a8821-186">a.</span></span> <span data-ttu-id="a8821-187">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a8821-187">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a8821-188">b.</span><span class="sxs-lookup"><span data-stu-id="a8821-188">b.</span></span> <span data-ttu-id="a8821-189">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8821-189">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="a8821-190">c.</span><span class="sxs-lookup"><span data-stu-id="a8821-190">c.</span></span> <span data-ttu-id="a8821-191">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="a8821-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="a8821-192">d.</span><span class="sxs-lookup"><span data-stu-id="a8821-192">d.</span></span> <span data-ttu-id="a8821-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a8821-193">Click **Create**.</span></span>
 
### <a name="create-a-merchlogix-test-user"></a><span data-ttu-id="a8821-194">Vytvoření zkušebního uživatele Merchlogix</span><span class="sxs-lookup"><span data-stu-id="a8821-194">Create a Merchlogix test user</span></span>

<span data-ttu-id="a8821-195">V této části vytvoříte volal Britta Simon v Merchlogix uživatele.</span><span class="sxs-lookup"><span data-stu-id="a8821-195">In this section, you create a user called Britta Simon in Merchlogix.</span></span> <span data-ttu-id="a8821-196">Práce s [tým podpory Merchlogix](http://www.merchlogix.com/contact/) přidat uživatele do Merchlogix platformy.</span><span class="sxs-lookup"><span data-stu-id="a8821-196">Work with [Merchlogix support team](http://www.merchlogix.com/contact/) to add the users in the Merchlogix platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="a8821-197">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8821-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="a8821-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Merchlogix.</span><span class="sxs-lookup"><span data-stu-id="a8821-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Merchlogix.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="a8821-200">**Pokud chcete přiřadit Britta Simon Merchlogix, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a8821-200">**To assign Britta Simon to Merchlogix, perform the following steps:**</span></span>

1. <span data-ttu-id="a8821-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a8821-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a8821-203">V seznamu aplikací vyberte **Merchlogix**.</span><span class="sxs-lookup"><span data-stu-id="a8821-203">In the applications list, select **Merchlogix**.</span></span>

    ![V seznamu aplikací na Merchlogix odkaz](./media/active-directory-saas-merchlogix-tutorial/tutorial_merchlogix_app.png)  

3. <span data-ttu-id="a8821-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a8821-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="a8821-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a8821-207">Click **Add** button.</span></span> <span data-ttu-id="a8821-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8821-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="a8821-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a8821-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a8821-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8821-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a8821-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8821-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a8821-213">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a8821-213">Test single sign-on</span></span>

<span data-ttu-id="a8821-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a8821-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a8821-215">Když kliknete na dlaždici Merchlogix na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Merchlogix.</span><span class="sxs-lookup"><span data-stu-id="a8821-215">When you click the Merchlogix tile in the Access Panel, you should get automatically signed-on to your Merchlogix application.</span></span>
<span data-ttu-id="a8821-216">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a8821-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a8821-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a8821-217">Additional resources</span></span>

* [<span data-ttu-id="a8821-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8821-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a8821-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a8821-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-merchlogix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-merchlogix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-merchlogix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-merchlogix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-merchlogix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-merchlogix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-merchlogix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-merchlogix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-merchlogix-tutorial/tutorial_general_203.png

