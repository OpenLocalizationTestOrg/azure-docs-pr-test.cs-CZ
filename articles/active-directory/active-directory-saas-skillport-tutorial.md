---
title: 'Kurz: Azure Active Directory integrace s Skillport | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Skillport."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4df349b2-a73f-4b88-a077-ec0fbfc26527
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 668fc5ae4f964bd776904c3a9dbc2b203689d50c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skillport"></a><span data-ttu-id="c9c03-103">Kurz: Azure Active Directory integrace s Skillport</span><span class="sxs-lookup"><span data-stu-id="c9c03-103">Tutorial: Azure Active Directory integration with Skillport</span></span>

<span data-ttu-id="c9c03-104">V tomto kurzu zjistěte, jak integrovat Skillport s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c9c03-104">In this tutorial, you learn how to integrate Skillport with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c9c03-105">Integrace Skillport s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c9c03-105">Integrating Skillport with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c9c03-106">Můžete řídit ve službě Azure AD, který má přístup k Skillport</span><span class="sxs-lookup"><span data-stu-id="c9c03-106">You can control in Azure AD who has access to Skillport</span></span>
- <span data-ttu-id="c9c03-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Skillport (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9c03-107">You can enable your users to automatically get signed-on to Skillport (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c9c03-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c9c03-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c9c03-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c9c03-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9c03-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c9c03-110">Prerequisites</span></span>

<span data-ttu-id="c9c03-111">Konfigurace integrace Azure AD s Skillport, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c9c03-111">To configure Azure AD integration with Skillport, you need the following items:</span></span>

- <span data-ttu-id="c9c03-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9c03-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c9c03-113">Skillport jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c9c03-113">A Skillport single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c9c03-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c9c03-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c9c03-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c9c03-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c9c03-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c9c03-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c9c03-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9c03-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c9c03-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c9c03-118">Scenario description</span></span>
<span data-ttu-id="c9c03-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c9c03-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c9c03-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c9c03-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c9c03-121">Přidání Skillport z Galerie</span><span class="sxs-lookup"><span data-stu-id="c9c03-121">Adding Skillport from the gallery</span></span>
2. <span data-ttu-id="c9c03-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9c03-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skillport-from-the-gallery"></a><span data-ttu-id="c9c03-123">Přidání Skillport z Galerie</span><span class="sxs-lookup"><span data-stu-id="c9c03-123">Adding Skillport from the gallery</span></span>
<span data-ttu-id="c9c03-124">Při konfiguraci integrace Skillport do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Skillport z galerie.</span><span class="sxs-lookup"><span data-stu-id="c9c03-124">To configure the integration of Skillport into Azure AD, you need to add Skillport from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c9c03-125">**Pokud chcete přidat Skillport z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c9c03-125">**To add Skillport from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c9c03-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c9c03-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c9c03-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c9c03-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c9c03-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9c03-131">Click **New Application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c9c03-133">Do vyhledávacího pole zadejte **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-133">In the search box, type **Skillport**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_search.png)

5. <span data-ttu-id="c9c03-135">Na panelu výsledků vyberte **Skillport**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c9c03-135">In the results panel, select **Skillport**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c9c03-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9c03-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c9c03-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Skillport podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c9c03-138">In this section, you configure and test Azure AD single sign-on with Skillport based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c9c03-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Skillport je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9c03-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Skillport is to a user in Azure AD.</span></span> <span data-ttu-id="c9c03-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Skillport musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c9c03-140">In other words, a link relationship between an Azure AD user and the related user in Skillport needs to be established.</span></span>

<span data-ttu-id="c9c03-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Skillport.</span><span class="sxs-lookup"><span data-stu-id="c9c03-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Skillport.</span></span>

<span data-ttu-id="c9c03-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Skillport, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c9c03-142">To configure and test Azure AD single sign-on with Skillport, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c9c03-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c9c03-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c9c03-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9c03-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c9c03-145">**[Vytvoření zkušebního uživatele Skillport](#creating-a-skillport-test-user)**  – Pokud chcete mít protějšek Britta Simon v Skillport propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c9c03-145">**[Creating a Skillport test user](#creating-a-skillport-test-user)** - to have a counterpart of Britta Simon in Skillport that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c9c03-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c9c03-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c9c03-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c9c03-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c9c03-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9c03-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c9c03-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Skillport.</span><span class="sxs-lookup"><span data-stu-id="c9c03-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Skillport application.</span></span>

<span data-ttu-id="c9c03-150">**Ke konfiguraci Azure AD jednotné přihlašování s Skillport, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c9c03-150">**To configure Azure AD single sign-on with Skillport, perform the following steps:**</span></span>

1. <span data-ttu-id="c9c03-151">Na portálu Azure na **Skillport** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-151">In the Azure  portal, on the **Skillport** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c9c03-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c9c03-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_samlbase.png)

3. <span data-ttu-id="c9c03-155">Na **Skillport domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c9c03-155">On the **Skillport Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_url.png)

    <span data-ttu-id="c9c03-157">a.</span><span class="sxs-lookup"><span data-stu-id="c9c03-157">a.</span></span> <span data-ttu-id="c9c03-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="c9c03-158">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span>
      
      <span data-ttu-id="c9c03-159">Evropa Datacenter:`https://<subdomain>.skillport.eu`</span><span class="sxs-lookup"><span data-stu-id="c9c03-159">EU Datacenter: `https://<subdomain>.skillport.eu`</span></span>
   
      <span data-ttu-id="c9c03-160">USA – Datacenter:`https://<subdomain>.skillport.com`</span><span class="sxs-lookup"><span data-stu-id="c9c03-160">US Datacenter: `https://<subdomain>.skillport.com`</span></span>
   
    <span data-ttu-id="c9c03-161">b.</span><span class="sxs-lookup"><span data-stu-id="c9c03-161">b.</span></span> <span data-ttu-id="c9c03-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="c9c03-162">In the **Reply URL** textbox, type a URL using the following patterns:</span></span>
    
      <span data-ttu-id="c9c03-163">Evropa Datacenter:`https://<subdomain>.skillport.eu/adfs/ls/`</span><span class="sxs-lookup"><span data-stu-id="c9c03-163">EU Datacenter: `https://<subdomain>.skillport.eu/adfs/ls/`</span></span>
    
      <span data-ttu-id="c9c03-164">USA – Datacenter:`https://<subdomain>.skillport.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="c9c03-164">US Datacenter: `https://<subdomain>.skillport.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c9c03-165">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="c9c03-165">These values are not the real.</span></span> <span data-ttu-id="c9c03-166">Tyto hodnoty aktualizujte skutečná adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="c9c03-166">Update these values with the actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="c9c03-167">Obraťte se na [tým podpory Skillport klienta](https://www.skillsoft.com/contact.asp) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="c9c03-167">Contact [Skillport Client support team](https://www.skillsoft.com/contact.asp) to get these values.</span></span>
 
4. <span data-ttu-id="c9c03-168">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c9c03-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_certificate.png) 

5. <span data-ttu-id="c9c03-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c9c03-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c9c03-172">Konfigurace jednotného přihlašování na **Skillport** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Skillport](https://www.skillsoft.com/contact.asp).</span><span class="sxs-lookup"><span data-stu-id="c9c03-172">To configure single sign-on on **Skillport** side, you need to send the downloaded **Metadata XML** to [Skillport support team](https://www.skillsoft.com/contact.asp).</span></span> <span data-ttu-id="c9c03-173">Budou se nastavit tak, aby připojení jednotné přihlašování SAML správně nastavené na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="c9c03-173">They will set it up to have the SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c9c03-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9c03-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="c9c03-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9c03-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c9c03-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c9c03-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c9c03-178">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c9c03-178">In the **Azure  portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c9c03-180">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c9c03-180">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c9c03-182">V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9c03-182">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c9c03-184">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c9c03-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c9c03-186">a.</span><span class="sxs-lookup"><span data-stu-id="c9c03-186">a.</span></span> <span data-ttu-id="c9c03-187">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c9c03-188">b.</span><span class="sxs-lookup"><span data-stu-id="c9c03-188">b.</span></span> <span data-ttu-id="c9c03-189">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c9c03-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c9c03-190">c.</span><span class="sxs-lookup"><span data-stu-id="c9c03-190">c.</span></span> <span data-ttu-id="c9c03-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c9c03-192">d.</span><span class="sxs-lookup"><span data-stu-id="c9c03-192">d.</span></span> <span data-ttu-id="c9c03-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-193">Click **Create**.</span></span>
 
### <a name="creating-a-skillport-test-user"></a><span data-ttu-id="c9c03-194">Vytvoření zkušebního uživatele Skillport</span><span class="sxs-lookup"><span data-stu-id="c9c03-194">Creating a Skillport test user</span></span>

<span data-ttu-id="c9c03-195">Chcete-li vytvořit Skillport testovacího uživatele, budete muset kontaktovat [tým podpory Skillport](https://www.skillsoft.com/contact.asp) mají více obchodní scénáře podle požadovaného koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="c9c03-195">In order to create Skillport test user, you need to contact [Skillport support team](https://www.skillsoft.com/contact.asp) as they have multiple business scenarios according to the requirement of end user.</span></span> <span data-ttu-id="c9c03-196">Nakonfiguruje ho po diskuse s uživateli.</span><span class="sxs-lookup"><span data-stu-id="c9c03-196">They will configure it after discussion with the users.</span></span>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c9c03-197">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9c03-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c9c03-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Skillport.</span><span class="sxs-lookup"><span data-stu-id="c9c03-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Skillport.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c9c03-200">**Pokud chcete přiřadit Britta Simon Skillport, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c9c03-200">**To assign Britta Simon to Skillport, perform the following steps:**</span></span>

1. <span data-ttu-id="c9c03-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c9c03-203">V seznamu aplikací vyberte **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-203">In the applications list, select **Skillport**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_app.png) 

3. <span data-ttu-id="c9c03-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c9c03-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c9c03-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c9c03-207">Click **Add** button.</span></span> <span data-ttu-id="c9c03-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9c03-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c9c03-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c9c03-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c9c03-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9c03-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c9c03-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9c03-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c9c03-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9c03-213">Testing single sign-on</span></span>

<span data-ttu-id="c9c03-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c9c03-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c9c03-215">Když kliknete na dlaždici Skillport na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Skillport.</span><span class="sxs-lookup"><span data-stu-id="c9c03-215">When you click the Skillport tile in the Access Panel, you should get automatically signed-on to your Skillport application.</span></span>
<span data-ttu-id="c9c03-216">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="c9c03-216">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c9c03-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c9c03-217">Additional resources</span></span>

* [<span data-ttu-id="c9c03-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c9c03-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9c03-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c9c03-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_203.png

