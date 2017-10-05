---
title: 'Kurz: Azure Active Directory integrace s CompetencyIQ | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a CompetencyIQ."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e262bf7e-cc7d-4d0e-aea7-861f00d8837d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: ad3cec3de9034ddab2161952620d31540ae51978
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-competencyiq"></a><span data-ttu-id="ce4fc-103">Kurz: Azure Active Directory integrace s CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="ce4fc-103">Tutorial: Azure Active Directory integration with CompetencyIQ</span></span>

<span data-ttu-id="ce4fc-104">V tomto kurzu zjistěte, jak integrovat CompetencyIQ s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ce4fc-104">In this tutorial, you learn how to integrate CompetencyIQ with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ce4fc-105">Integrace CompetencyIQ s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ce4fc-105">Integrating CompetencyIQ with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ce4fc-106">Můžete řídit ve službě Azure AD, který má přístup k CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="ce4fc-106">You can control in Azure AD who has access to CompetencyIQ</span></span>
- <span data-ttu-id="ce4fc-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k CompetencyIQ (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce4fc-107">You can enable your users to automatically get signed-on to CompetencyIQ (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ce4fc-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ce4fc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ce4fc-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ce4fc-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ce4fc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ce4fc-110">Prerequisites</span></span>

<span data-ttu-id="ce4fc-111">Konfigurace integrace Azure AD s CompetencyIQ, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ce4fc-111">To configure Azure AD integration with CompetencyIQ, you need the following items:</span></span>

- <span data-ttu-id="ce4fc-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce4fc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ce4fc-113">CompetencyIQ jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ce4fc-113">A CompetencyIQ single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ce4fc-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ce4fc-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ce4fc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ce4fc-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ce4fc-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ce4fc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ce4fc-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ce4fc-118">Scenario description</span></span>
<span data-ttu-id="ce4fc-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ce4fc-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ce4fc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ce4fc-121">Přidání CompetencyIQ z Galerie</span><span class="sxs-lookup"><span data-stu-id="ce4fc-121">Adding CompetencyIQ from the gallery</span></span>
2. <span data-ttu-id="ce4fc-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce4fc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-competencyiq-from-the-gallery"></a><span data-ttu-id="ce4fc-123">Přidání CompetencyIQ z Galerie</span><span class="sxs-lookup"><span data-stu-id="ce4fc-123">Adding CompetencyIQ from the gallery</span></span>
<span data-ttu-id="ce4fc-124">Při konfiguraci integrace CompetencyIQ do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS CompetencyIQ z galerie.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-124">To configure the integration of CompetencyIQ into Azure AD, you need to add CompetencyIQ from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ce4fc-125">**Pokud chcete přidat CompetencyIQ z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce4fc-125">**To add CompetencyIQ from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ce4fc-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ce4fc-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ce4fc-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ce4fc-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ce4fc-133">Do vyhledávacího pole zadejte **CompetencyIQ**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-133">In the search box, type **CompetencyIQ**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_search.png)

5. <span data-ttu-id="ce4fc-135">Na panelu výsledků vyberte **CompetencyIQ**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-135">In the results panel, select **CompetencyIQ**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ce4fc-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce4fc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ce4fc-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s CompetencyIQ podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="ce4fc-138">In this section, you configure and test Azure AD single sign-on with CompetencyIQ based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ce4fc-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v CompetencyIQ je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CompetencyIQ is to a user in Azure AD.</span></span> <span data-ttu-id="ce4fc-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v CompetencyIQ musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-140">In other words, a link relationship between an Azure AD user and the related user in CompetencyIQ needs to be established.</span></span>

<span data-ttu-id="ce4fc-141">V CompetencyIQ, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-141">In CompetencyIQ, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ce4fc-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s CompetencyIQ, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ce4fc-142">To configure and test Azure AD single sign-on with CompetencyIQ, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ce4fc-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ce4fc-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ce4fc-145">**[Vytvoření zkušebního uživatele CompetencyIQ](#creating-a-competencyiq-test-user)**  – Pokud chcete mít protějšek Britta Simon v CompetencyIQ propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-145">**[Creating a CompetencyIQ test user](#creating-a-competencyiq-test-user)** - to have a counterpart of Britta Simon in CompetencyIQ that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ce4fc-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ce4fc-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ce4fc-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce4fc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ce4fc-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci CompetencyIQ.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CompetencyIQ application.</span></span>

<span data-ttu-id="ce4fc-150">**Ke konfiguraci Azure AD jednotné přihlašování s CompetencyIQ, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce4fc-150">**To configure Azure AD single sign-on with CompetencyIQ, perform the following steps:**</span></span>

1. <span data-ttu-id="ce4fc-151">Na portálu Azure na **CompetencyIQ** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-151">In the Azure portal, on the **CompetencyIQ** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ce4fc-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_samlbase.png)

3. <span data-ttu-id="ce4fc-155">Na **CompetencyIQ domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce4fc-155">On the **CompetencyIQ Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_url1.png)

    <span data-ttu-id="ce4fc-157">a.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-157">a.</span></span> <span data-ttu-id="ce4fc-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<customer>.competencyiq.com/`</span><span class="sxs-lookup"><span data-stu-id="ce4fc-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<customer>.competencyiq.com/`</span></span>
    
    <span data-ttu-id="ce4fc-159">b.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-159">b.</span></span> <span data-ttu-id="ce4fc-160">V **identifikátor** textovému poli, zadejte adresu URL:`https://www.competencyiq.com/`</span><span class="sxs-lookup"><span data-stu-id="ce4fc-160">In the **Identifier** textbox, type the URL: `https://www.competencyiq.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ce4fc-161">Hodnota přihlašovací adresa URL není skutečně tak aktualizovat s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-161">The Sign-on URL value is not real so update this with actual Sign-On URL.</span></span> <span data-ttu-id="ce4fc-162">Obraťte se na [tým podpory CompetencyIQ klienta](https://www.competencyiq.com/) se získat to.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-162">Contact [CompetencyIQ Client support team](https://www.competencyiq.com/) to get this.</span></span> 
 
4. <span data-ttu-id="ce4fc-163">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-163">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_certificate.png) 

5. <span data-ttu-id="ce4fc-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-competencyiq-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ce4fc-167">Na **CompetencyIQ konfigurace** klikněte na tlačítko **konfigurace CompetencyIQ** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-167">On the **CompetencyIQ Configuration** section, click **Configure CompetencyIQ** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ce4fc-168">Kopírování **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ce4fc-168">Copy the **SAML Entity ID**, and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_configure.png) 

7. <span data-ttu-id="ce4fc-170">Konfigurace jednotného přihlašování na **CompetencyIQ** straně, budete muset odeslat stažené **soubor XML s metadaty**, **SAML Entity ID** a **SAML-služby přihlášení Adresa URL** k [tým podpory CompetencyIQ](https://www.competencyiq.com/).</span><span class="sxs-lookup"><span data-stu-id="ce4fc-170">To configure single sign-on on **CompetencyIQ** side, you need to send the downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** to [CompetencyIQ support team](https://www.competencyiq.com/).</span></span> <span data-ttu-id="ce4fc-171">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-171">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="ce4fc-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ce4fc-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ce4fc-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ce4fc-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ce4fc-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ce4fc-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce4fc-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="ce4fc-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ce4fc-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce4fc-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ce4fc-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ce4fc-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ce4fc-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ce4fc-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce4fc-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ce4fc-187">a.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-187">a.</span></span> <span data-ttu-id="ce4fc-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ce4fc-189">b.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-189">b.</span></span> <span data-ttu-id="ce4fc-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ce4fc-191">c.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-191">c.</span></span> <span data-ttu-id="ce4fc-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ce4fc-193">d.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-193">d.</span></span> <span data-ttu-id="ce4fc-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-194">Click **Create**.</span></span>
 
### <a name="creating-a-competencyiq-test-user"></a><span data-ttu-id="ce4fc-195">Vytvoření zkušebního uživatele CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="ce4fc-195">Creating a CompetencyIQ test user</span></span>

<span data-ttu-id="ce4fc-196">Pokud chcete povolit uživatelům Azure AD přihlášení k CompetencyIQ, musí být zřízená do CompetencyIQ.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-196">To enable Azure AD users to log in to CompetencyIQ, they must be provisioned into CompetencyIQ.</span></span> <span data-ttu-id="ce4fc-197">Obraťte se na [tým podpory CompetencyIQ](https://www.competencyiq.com/) pro vytváření uživatelů v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-197">Contact [CompetencyIQ support team](https://www.competencyiq.com/) to create users in the application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ce4fc-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce4fc-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ce4fc-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu CompetencyIQ.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CompetencyIQ.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ce4fc-201">**Pokud chcete přiřadit Britta Simon CompetencyIQ, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce4fc-201">**To assign Britta Simon to CompetencyIQ, perform the following steps:**</span></span>

1. <span data-ttu-id="ce4fc-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ce4fc-204">V seznamu aplikací vyberte **CompetencyIQ**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-204">In the applications list, select **CompetencyIQ**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_app.png) 

3. <span data-ttu-id="ce4fc-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ce4fc-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-208">Click **Add** button.</span></span> <span data-ttu-id="ce4fc-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ce4fc-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ce4fc-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ce4fc-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ce4fc-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce4fc-214">Testing single sign-on</span></span>

<span data-ttu-id="ce4fc-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ce4fc-216">Když kliknete na dlaždici CompetencyIQ na přístupovém panelu, by měl získat automaticky přihlášeni aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce4fc-216">When you click the CompetencyIQ tile in the Access Panel, you should get automatically logged into the application.</span></span>
<span data-ttu-id="ce4fc-217">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ce4fc-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ce4fc-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ce4fc-218">Additional resources</span></span>

* [<span data-ttu-id="ce4fc-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ce4fc-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ce4fc-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ce4fc-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_203.png

