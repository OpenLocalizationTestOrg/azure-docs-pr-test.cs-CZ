---
title: 'Kurz: Azure Active Directory integrace s InTime | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a InTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d4e2c6e1-ae5d-4d2c-8ffc-1b24534d376a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: jeedes
ms.openlocfilehash: 4bb22c92ad7f6963be6ca15073f7f01da99ba2bb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intime"></a><span data-ttu-id="66e7d-103">Kurz: Azure Active Directory integrace s InTime</span><span class="sxs-lookup"><span data-stu-id="66e7d-103">Tutorial: Azure Active Directory integration with InTime</span></span>

<span data-ttu-id="66e7d-104">V tomto kurzu zjistěte, jak integrovat InTime s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="66e7d-104">In this tutorial, you learn how to integrate InTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="66e7d-105">Integrace InTime s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="66e7d-105">Integrating InTime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="66e7d-106">Můžete ovládat ve službě Azure AD, který má přístup k InTime.</span><span class="sxs-lookup"><span data-stu-id="66e7d-106">You can control in Azure AD who has access to InTime.</span></span>
- <span data-ttu-id="66e7d-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k InTime (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="66e7d-107">You can enable your users to automatically get signed-on to InTime (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="66e7d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="66e7d-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="66e7d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="66e7d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66e7d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="66e7d-110">Prerequisites</span></span>

<span data-ttu-id="66e7d-111">Konfigurace integrace Azure AD s InTime, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="66e7d-111">To configure Azure AD integration with InTime, you need the following items:</span></span>

- <span data-ttu-id="66e7d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="66e7d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="66e7d-113">InTime jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="66e7d-113">A InTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="66e7d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="66e7d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="66e7d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="66e7d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="66e7d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="66e7d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="66e7d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="66e7d-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="66e7d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="66e7d-118">Scenario description</span></span>
<span data-ttu-id="66e7d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="66e7d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="66e7d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="66e7d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="66e7d-121">Přidání InTime z Galerie</span><span class="sxs-lookup"><span data-stu-id="66e7d-121">Adding InTime from the gallery</span></span>
2. <span data-ttu-id="66e7d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="66e7d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intime-from-the-gallery"></a><span data-ttu-id="66e7d-123">Přidání InTime z Galerie</span><span class="sxs-lookup"><span data-stu-id="66e7d-123">Adding InTime from the gallery</span></span>
<span data-ttu-id="66e7d-124">Při konfiguraci integrace InTime do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS InTime z galerie.</span><span class="sxs-lookup"><span data-stu-id="66e7d-124">To configure the integration of InTime into Azure AD, you need to add InTime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="66e7d-125">**Pokud chcete přidat InTime z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="66e7d-125">**To add InTime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="66e7d-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="66e7d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="66e7d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="66e7d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="66e7d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="66e7d-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="66e7d-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66e7d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="66e7d-133">Do vyhledávacího pole zadejte **InTime**, vyberte **InTime** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="66e7d-133">In the search box, type **InTime**, select **InTime** from result panel then click **Add** button to add the application.</span></span>

    ![InTime v seznamu výsledků](./media/active-directory-saas-intime-tutorial/tutorial_intime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="66e7d-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="66e7d-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="66e7d-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s InTime podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="66e7d-136">In this section, you configure and test Azure AD single sign-on with InTime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="66e7d-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v InTime je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="66e7d-137">For single sign-on to work, Azure AD needs to know what the counterpart user in InTime is to a user in Azure AD.</span></span> <span data-ttu-id="66e7d-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v InTime musí navázat.</span><span class="sxs-lookup"><span data-stu-id="66e7d-138">In other words, a link relationship between an Azure AD user and the related user in InTime needs to be established.</span></span>

<span data-ttu-id="66e7d-139">V InTime, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="66e7d-139">In InTime, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="66e7d-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s InTime, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="66e7d-140">To configure and test Azure AD single sign-on with InTime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="66e7d-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="66e7d-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="66e7d-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66e7d-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="66e7d-143">**[Vytvoření InTime zkušebního uživatele](#create-a-intime-test-user)**  – Pokud chcete mít protějšek Britta Simon v InTime propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="66e7d-143">**[Create a InTime test user](#create-a-intime-test-user)** - to have a counterpart of Britta Simon in InTime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="66e7d-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="66e7d-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="66e7d-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="66e7d-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="66e7d-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="66e7d-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="66e7d-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci InTime.</span><span class="sxs-lookup"><span data-stu-id="66e7d-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your InTime application.</span></span>

<span data-ttu-id="66e7d-148">**Ke konfiguraci Azure AD jednotné přihlašování s InTime, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="66e7d-148">**To configure Azure AD single sign-on with InTime, perform the following steps:**</span></span>

1. <span data-ttu-id="66e7d-149">Na portálu Azure na **InTime** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="66e7d-149">In the Azure portal, on the **InTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="66e7d-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="66e7d-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-intime-tutorial/tutorial_intime_samlbase.png)

3. <span data-ttu-id="66e7d-153">Na **InTime domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="66e7d-153">On the **InTime Domain and URLs** section, perform the following steps:</span></span>

    ![InTime domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-intime-tutorial/tutorial_intime_url.png)

    <span data-ttu-id="66e7d-155">a.</span><span class="sxs-lookup"><span data-stu-id="66e7d-155">a.</span></span> <span data-ttu-id="66e7d-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL:`https://intime6.intimesoft.com/mytime/login/login.xhtml`</span><span class="sxs-lookup"><span data-stu-id="66e7d-156">In the **Sign-on URL** textbox, type the URL: `https://intime6.intimesoft.com/mytime/login/login.xhtml`</span></span>

    <span data-ttu-id="66e7d-157">b.</span><span class="sxs-lookup"><span data-stu-id="66e7d-157">b.</span></span> <span data-ttu-id="66e7d-158">V **identifikátor** textovému poli, zadejte adresu URL:`https://auth.intimesoft.com/auth/realms/master`</span><span class="sxs-lookup"><span data-stu-id="66e7d-158">In the **Identifier** textbox, type the URL: `https://auth.intimesoft.com/auth/realms/master`</span></span>

4. <span data-ttu-id="66e7d-159">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="66e7d-159">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-intime-tutorial/tutorial_intime_certificate.png) 

5. <span data-ttu-id="66e7d-161">Aplikace InTime očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="66e7d-161">Your InTime application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="66e7d-162">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="66e7d-162">The following screenshot shows an example for this.</span></span> <span data-ttu-id="66e7d-163">Výchozí hodnota **uživatelský identifikátor** je **user.userprincipalname** ale InTime očekává to nejde mapovat pomocí e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="66e7d-163">The default value of **User Identifier** is **user.userprincipalname** but InTime expects this to be mapped with the user's email address.</span></span> <span data-ttu-id="66e7d-164">K tomu můžete použít **user.mail** atribut ze seznamu, nebo použijte hodnotu odpovídajícího atributu na základě vaší organizace konfigurace</span><span class="sxs-lookup"><span data-stu-id="66e7d-164">For that you can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration</span></span> 

    ![Konfigurace atributů](./media/active-directory-saas-intime-tutorial/tutorial_intime_attribute.png)

6. <span data-ttu-id="66e7d-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="66e7d-166">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-intime-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="66e7d-168">Na **InTime konfigurace** klikněte na tlačítko **konfigurace InTime** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="66e7d-168">On the **InTime Configuration** section, click **Configure InTime** to open **Configure sign-on** window.</span></span> <span data-ttu-id="66e7d-169">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="66e7d-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![InTime konfigurace](./media/active-directory-saas-intime-tutorial/tutorial_intime_configure.png) 

8. <span data-ttu-id="66e7d-171">Konfigurace jednotného přihlašování na **InTime** straně, budete muset odeslat stažené **soubor XML s metadaty**, **Sign-Out adresu URL a SAML jeden přihlašování služby URL** k [tým podpory InTime](mailto:hdollard@intimesoft.com).</span><span class="sxs-lookup"><span data-stu-id="66e7d-171">To configure single sign-on on **InTime** side, you need to send the downloaded **Metadata XML**, **Sign-Out URL, and SAML Single Sign-On Service URL** to [InTime support team](mailto:hdollard@intimesoft.com).</span></span> <span data-ttu-id="66e7d-172">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="66e7d-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="66e7d-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="66e7d-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="66e7d-174">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="66e7d-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="66e7d-175">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="66e7d-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="66e7d-176">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="66e7d-176">Create an Azure AD test user</span></span>

<span data-ttu-id="66e7d-177">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66e7d-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="66e7d-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="66e7d-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="66e7d-180">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="66e7d-180">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-intime-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="66e7d-182">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="66e7d-182">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-intime-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="66e7d-184">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66e7d-184">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-intime-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="66e7d-186">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="66e7d-186">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-intime-tutorial/create_aaduser_04.png)

    <span data-ttu-id="66e7d-188">a.</span><span class="sxs-lookup"><span data-stu-id="66e7d-188">a.</span></span> <span data-ttu-id="66e7d-189">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="66e7d-189">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="66e7d-190">b.</span><span class="sxs-lookup"><span data-stu-id="66e7d-190">b.</span></span> <span data-ttu-id="66e7d-191">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66e7d-191">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="66e7d-192">c.</span><span class="sxs-lookup"><span data-stu-id="66e7d-192">c.</span></span> <span data-ttu-id="66e7d-193">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="66e7d-193">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="66e7d-194">d.</span><span class="sxs-lookup"><span data-stu-id="66e7d-194">d.</span></span> <span data-ttu-id="66e7d-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="66e7d-195">Click **Create**.</span></span>
 
### <a name="create-a-intime-test-user"></a><span data-ttu-id="66e7d-196">Vytvoření InTime zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="66e7d-196">Create a InTime test user</span></span>

<span data-ttu-id="66e7d-197">V této části vytvoříte volal Britta Simon v InTime uživatele.</span><span class="sxs-lookup"><span data-stu-id="66e7d-197">In this section, you create a user called Britta Simon in InTime.</span></span> <span data-ttu-id="66e7d-198">Práce s [tým podpory InTime](mailto:hdollard@intimesoft.com) přidat uživatele do InTime platformy.</span><span class="sxs-lookup"><span data-stu-id="66e7d-198">Work with [InTime support team](mailto:hdollard@intimesoft.com) to add the users in the InTime platform.</span></span> <span data-ttu-id="66e7d-199">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="66e7d-199">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="66e7d-200">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="66e7d-200">Assign the Azure AD test user</span></span>

<span data-ttu-id="66e7d-201">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu InTime.</span><span class="sxs-lookup"><span data-stu-id="66e7d-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to InTime.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="66e7d-203">**Pokud chcete přiřadit Britta Simon InTime, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="66e7d-203">**To assign Britta Simon to InTime, perform the following steps:**</span></span>

1. <span data-ttu-id="66e7d-204">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="66e7d-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="66e7d-206">V seznamu aplikací vyberte **InTime**.</span><span class="sxs-lookup"><span data-stu-id="66e7d-206">In the applications list, select **InTime**.</span></span>

    ![Odkaz InTime v seznamu aplikací](./media/active-directory-saas-intime-tutorial/tutorial_intime_app.png)  

3. <span data-ttu-id="66e7d-208">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="66e7d-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="66e7d-210">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="66e7d-210">Click **Add** button.</span></span> <span data-ttu-id="66e7d-211">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66e7d-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="66e7d-213">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="66e7d-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="66e7d-214">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66e7d-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="66e7d-215">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66e7d-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="66e7d-216">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="66e7d-216">Test single sign-on</span></span>

<span data-ttu-id="66e7d-217">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="66e7d-217">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="66e7d-218">Když kliknete na dlaždici InTime na přístupovém panelu, měli byste obdržet přihlašovací stránku vaší InTime aplikace.</span><span class="sxs-lookup"><span data-stu-id="66e7d-218">When you click the InTime tile in the Access Panel, you should get the login page of your InTime application.</span></span> <span data-ttu-id="66e7d-219">Klikněte **přihlášení** tlačítko, pak řadu IdPs se zobrazí v seznamu tlačítek.</span><span class="sxs-lookup"><span data-stu-id="66e7d-219">Click the **Login** button, then a series of IdPs will be displayed on a list of buttons.</span></span> <span data-ttu-id="66e7d-220">Klikněte na tlačítko **IDP název** dané podle [tým podpory InTime](mailto:hdollard@intimesoft.com) k přihlášení do aplikace InTime.</span><span class="sxs-lookup"><span data-stu-id="66e7d-220">click **IDP name** given by [InTime support team](mailto:hdollard@intimesoft.com) to login into your InTime application.</span></span> <span data-ttu-id="66e7d-221">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="66e7d-221">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="66e7d-222">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="66e7d-222">Additional resources</span></span>

* [<span data-ttu-id="66e7d-223">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="66e7d-223">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="66e7d-224">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="66e7d-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-intime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intime-tutorial/tutorial_general_203.png

