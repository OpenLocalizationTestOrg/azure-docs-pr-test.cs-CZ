---
title: 'Kurz: Azure Active Directory integrace s eDigitalResearch | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a eDigitalResearch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c6b66ea0-16ba-45b4-b550-e81c56262b1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: f877a1dd844c40c913f3121e5288952653c312cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-edigitalresearch"></a><span data-ttu-id="40da2-103">Kurz: Azure Active Directory integrace s eDigitalResearch</span><span class="sxs-lookup"><span data-stu-id="40da2-103">Tutorial: Azure Active Directory integration with eDigitalResearch</span></span>

<span data-ttu-id="40da2-104">V tomto kurzu zjistěte, jak integrovat eDigitalResearch s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="40da2-104">In this tutorial, you learn how to integrate eDigitalResearch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="40da2-105">Integrace eDigitalResearch s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="40da2-105">Integrating eDigitalResearch with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="40da2-106">Můžete ovládat ve službě Azure AD, který má přístup k eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="40da2-106">You can control in Azure AD who has access to eDigitalResearch.</span></span>
- <span data-ttu-id="40da2-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k eDigitalResearch (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="40da2-107">You can enable your users to automatically get signed-on to eDigitalResearch (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="40da2-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="40da2-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="40da2-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="40da2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40da2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="40da2-110">Prerequisites</span></span>

<span data-ttu-id="40da2-111">Konfigurace integrace Azure AD s eDigitalResearch, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="40da2-111">To configure Azure AD integration with eDigitalResearch, you need the following items:</span></span>

- <span data-ttu-id="40da2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="40da2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="40da2-113">EDigitalResearch jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="40da2-113">A eDigitalResearch single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="40da2-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="40da2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="40da2-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="40da2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="40da2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="40da2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="40da2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="40da2-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="40da2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="40da2-118">Scenario description</span></span>
<span data-ttu-id="40da2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="40da2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="40da2-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="40da2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="40da2-121">Přidání eDigitalResearch z Galerie</span><span class="sxs-lookup"><span data-stu-id="40da2-121">Adding eDigitalResearch from the gallery</span></span>
2. <span data-ttu-id="40da2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="40da2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-edigitalresearch-from-the-gallery"></a><span data-ttu-id="40da2-123">Přidání eDigitalResearch z Galerie</span><span class="sxs-lookup"><span data-stu-id="40da2-123">Adding eDigitalResearch from the gallery</span></span>
<span data-ttu-id="40da2-124">Při konfiguraci integrace eDigitalResearch do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS eDigitalResearch z galerie.</span><span class="sxs-lookup"><span data-stu-id="40da2-124">To configure the integration of eDigitalResearch into Azure AD, you need to add eDigitalResearch from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="40da2-125">**Pokud chcete přidat eDigitalResearch z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="40da2-125">**To add eDigitalResearch from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="40da2-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="40da2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="40da2-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="40da2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="40da2-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="40da2-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="40da2-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40da2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="40da2-133">Do vyhledávacího pole zadejte **eDigitalResearch**, vyberte **eDigitalResearch** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="40da2-133">In the search box, type **eDigitalResearch**, select **eDigitalResearch** from result panel then click **Add** button to add the application.</span></span>

    ![eDigitalResearch v seznamu výsledků](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="40da2-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="40da2-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="40da2-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s eDigitalResearch podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="40da2-136">In this section, you configure and test Azure AD single sign-on with eDigitalResearch based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="40da2-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v eDigitalResearch je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="40da2-137">For single sign-on to work, Azure AD needs to know what the counterpart user in eDigitalResearch is to a user in Azure AD.</span></span> <span data-ttu-id="40da2-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v eDigitalResearch musí navázat.</span><span class="sxs-lookup"><span data-stu-id="40da2-138">In other words, a link relationship between an Azure AD user and the related user in eDigitalResearch needs to be established.</span></span>

<span data-ttu-id="40da2-139">V eDigitalResearch, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="40da2-139">In eDigitalResearch, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="40da2-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s eDigitalResearch, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="40da2-140">To configure and test Azure AD single sign-on with eDigitalResearch, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="40da2-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="40da2-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="40da2-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="40da2-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="40da2-143">**[Vytvoření zkušebního uživatele eDigitalResearch](#create-a-edigitalresearch-test-user)**  – Pokud chcete mít protějšek Britta Simon v eDigitalResearch propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="40da2-143">**[Create a eDigitalResearch test user](#create-a-edigitalresearch-test-user)** - to have a counterpart of Britta Simon in eDigitalResearch that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="40da2-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="40da2-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="40da2-145">**[Test jednotného přihlašování](#test-single-sign-on)**  ověřit, zda funguje konfigurace.</span><span class="sxs-lookup"><span data-stu-id="40da2-145">**[Test single sign-on](#test-single-sign-on)**  to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="40da2-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="40da2-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="40da2-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="40da2-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your eDigitalResearch application.</span></span>

<span data-ttu-id="40da2-148">**Ke konfiguraci Azure AD jednotné přihlašování s eDigitalResearch, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="40da2-148">**To configure Azure AD single sign-on with eDigitalResearch, perform the following steps:**</span></span>

1. <span data-ttu-id="40da2-149">Na portálu Azure na **eDigitalResearch** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="40da2-149">In the Azure portal, on the **eDigitalResearch** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="40da2-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="40da2-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_samlbase.png)

3. <span data-ttu-id="40da2-153">Na **eDigitalResearch domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="40da2-153">On the **eDigitalResearch Domain and URLs** section, perform the following steps:</span></span>

    ![eDigitalResearch domény a adresy URL jeden přihlašování informace](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_url.png)

    <span data-ttu-id="40da2-155">a.</span><span class="sxs-lookup"><span data-stu-id="40da2-155">a.</span></span> <span data-ttu-id="40da2-156">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company-name>.edigitalresearch.com`</span><span class="sxs-lookup"><span data-stu-id="40da2-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<company-name>.edigitalresearch.com`</span></span>

    <span data-ttu-id="40da2-157">b.</span><span class="sxs-lookup"><span data-stu-id="40da2-157">b.</span></span> <span data-ttu-id="40da2-158">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company-name>.edigitalresearch.com/login/consume`</span><span class="sxs-lookup"><span data-stu-id="40da2-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company-name>.edigitalresearch.com/login/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="40da2-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="40da2-159">These values are not real.</span></span> <span data-ttu-id="40da2-160">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="40da2-160">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="40da2-161">Obraťte se na [tým podpory eDigitalResearch](http://www.maruedr.com/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="40da2-161">Contact [eDigitalResearch support team](http://www.maruedr.com/contact) to get these values.</span></span>
 


4. <span data-ttu-id="40da2-162">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát Base(64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="40da2-162">On the **SAML Signing Certificate** section, click **Certificate Base(64)** and then save the certificate file on your computer.</span></span>

    <span data-ttu-id="40da2-163">!</span><span class="sxs-lookup"><span data-stu-id="40da2-163">!</span></span>![Odkaz ke stažení certifikátu](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_certificate.png) 

5. <span data-ttu-id="40da2-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="40da2-165">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="40da2-167">Na **eDigitalResearch konfigurace** klikněte na tlačítko **konfigurace eDigitalResearch** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="40da2-167">On the **eDigitalResearch Configuration** section, click **Configure eDigitalResearch** to open **Configure sign-on** window.</span></span> <span data-ttu-id="40da2-168">Kopírování **Sign-Out adresu URL, SAML Entity ID** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="40da2-168">Copy the **Sign-Out URL, SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![eDigitalResearch konfigurace](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_configure.png) 

7. <span data-ttu-id="40da2-170">Konfigurace jednotného přihlašování na **eDigitalResearch** straně, budete muset odeslat stažené **soubor certifikátu (Base64)**, **SAML Entity ID**, a **Sign-Out URL** k [tým podpory eDigitalResearch](http://www.maruedr.com/contact).</span><span class="sxs-lookup"><span data-stu-id="40da2-170">To configure single sign-on on **eDigitalResearch** side, you need to send the downloaded **Certificate (Base64) File**, **SAML Entity ID**, and **Sign-Out URL** to [eDigitalResearch support team](http://www.maruedr.com/contact).</span></span> <span data-ttu-id="40da2-171">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="40da2-171">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="40da2-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="40da2-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="40da2-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="40da2-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="40da2-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="40da2-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="40da2-175">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="40da2-175">Create an Azure AD test user</span></span>

<span data-ttu-id="40da2-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="40da2-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="40da2-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="40da2-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="40da2-179">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="40da2-179">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="40da2-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="40da2-181">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="40da2-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40da2-183">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="40da2-185">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="40da2-185">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_04.png)

    <span data-ttu-id="40da2-187">a.</span><span class="sxs-lookup"><span data-stu-id="40da2-187">a.</span></span> <span data-ttu-id="40da2-188">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="40da2-188">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="40da2-189">b.</span><span class="sxs-lookup"><span data-stu-id="40da2-189">b.</span></span> <span data-ttu-id="40da2-190">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="40da2-190">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="40da2-191">c.</span><span class="sxs-lookup"><span data-stu-id="40da2-191">c.</span></span> <span data-ttu-id="40da2-192">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="40da2-192">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="40da2-193">d.</span><span class="sxs-lookup"><span data-stu-id="40da2-193">d.</span></span> <span data-ttu-id="40da2-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="40da2-194">Click **Create**.</span></span>
  
### <a name="create-a-edigitalresearch-test-user"></a><span data-ttu-id="40da2-195">Vytvoření zkušebního uživatele eDigitalResearch</span><span class="sxs-lookup"><span data-stu-id="40da2-195">Create a eDigitalResearch test user</span></span>

<span data-ttu-id="40da2-196">Cílem této části je vytvoření uživatele v eDigitalResearch nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="40da2-196">The objective of this section is to create a user called Britta Simon in eDigitalResearch.</span></span> 

<span data-ttu-id="40da2-197">Pracovat [tým podpory eDigitalResearch](http://www.maruedr.com/contact) získat uživatelé vytvoření.</span><span class="sxs-lookup"><span data-stu-id="40da2-197">Work with the [eDigitalResearch support team](http://www.maruedr.com/contact) to get users created.</span></span>     
    
 > [!NOTE]
 > <span data-ttu-id="40da2-198">Držitel účtu Azure Active Directory obdrží e-mailu a dodržuje odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="40da2-198">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="40da2-199">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="40da2-199">Assign the Azure AD test user</span></span>

<span data-ttu-id="40da2-200">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="40da2-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to eDigitalResearch.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="40da2-202">**Pokud chcete přiřadit Britta Simon eDigitalResearch, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="40da2-202">**To assign Britta Simon to eDigitalResearch, perform the following steps:**</span></span>

1. <span data-ttu-id="40da2-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="40da2-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="40da2-205">V seznamu aplikací vyberte **eDigitalResearch**.</span><span class="sxs-lookup"><span data-stu-id="40da2-205">In the applications list, select **eDigitalResearch**.</span></span>

    ![V seznamu aplikací na eDigitalResearch odkaz](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_app.png)  

3. <span data-ttu-id="40da2-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="40da2-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="40da2-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="40da2-209">Click **Add** button.</span></span> <span data-ttu-id="40da2-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40da2-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="40da2-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="40da2-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="40da2-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40da2-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="40da2-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="40da2-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="40da2-215">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="40da2-215">Test single sign-on</span></span>

<span data-ttu-id="40da2-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="40da2-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="40da2-217">Když kliknete na dlaždici eDigitalResearch na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="40da2-217">When you click the eDigitalResearch tile in the Access Panel, you should get automatically signed-on to your eDigitalResearch application.</span></span>
<span data-ttu-id="40da2-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="40da2-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="40da2-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="40da2-219">Additional resources</span></span>

* [<span data-ttu-id="40da2-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="40da2-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="40da2-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="40da2-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_203.png

