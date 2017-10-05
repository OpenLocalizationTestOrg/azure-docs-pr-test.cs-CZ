---
title: 'Kurz: Azure Active Directory integrace s Lynda.com | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Lynda.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6c92789-8b64-4049-bac9-8cb928398433
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 84ed2adcc2d49ddbb6bd2e9cc3b93b967ebed063
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lyndacom"></a><span data-ttu-id="494a0-103">Kurz: Azure Active Directory integrace s Lynda.com</span><span class="sxs-lookup"><span data-stu-id="494a0-103">Tutorial: Azure Active Directory integration with Lynda.com</span></span>

<span data-ttu-id="494a0-104">V tomto kurzu zjistěte, jak integrovat Lynda.com s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="494a0-104">In this tutorial, you learn how to integrate Lynda.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="494a0-105">Integrace Lynda.com s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="494a0-105">Integrating Lynda.com with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="494a0-106">Můžete řídit ve službě Azure AD, který má přístup k Lynda.com</span><span class="sxs-lookup"><span data-stu-id="494a0-106">You can control in Azure AD who has access to Lynda.com</span></span>
- <span data-ttu-id="494a0-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Lynda.com (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="494a0-107">You can enable your users to automatically get signed-on to Lynda.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="494a0-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="494a0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="494a0-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="494a0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="494a0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="494a0-110">Prerequisites</span></span>

<span data-ttu-id="494a0-111">Konfigurace integrace Azure AD s Lynda.com, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="494a0-111">To configure Azure AD integration with Lynda.com, you need the following items:</span></span>

- <span data-ttu-id="494a0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="494a0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="494a0-113">Lynda.com jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="494a0-113">A Lynda.com single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="494a0-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="494a0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="494a0-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="494a0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="494a0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="494a0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="494a0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="494a0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="494a0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="494a0-118">Scenario description</span></span>
<span data-ttu-id="494a0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="494a0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="494a0-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="494a0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="494a0-121">Přidání Lynda.com z Galerie</span><span class="sxs-lookup"><span data-stu-id="494a0-121">Adding Lynda.com from the gallery</span></span>
2. <span data-ttu-id="494a0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="494a0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lyndacom-from-the-gallery"></a><span data-ttu-id="494a0-123">Přidání Lynda.com z Galerie</span><span class="sxs-lookup"><span data-stu-id="494a0-123">Adding Lynda.com from the gallery</span></span>
<span data-ttu-id="494a0-124">Při konfiguraci integrace Lynda.com do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Lynda.com z galerie.</span><span class="sxs-lookup"><span data-stu-id="494a0-124">To configure the integration of Lynda.com into Azure AD, you need to add Lynda.com from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="494a0-125">**Pokud chcete přidat Lynda.com z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="494a0-125">**To add Lynda.com from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="494a0-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="494a0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="494a0-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="494a0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="494a0-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="494a0-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="494a0-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="494a0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="494a0-133">Do vyhledávacího pole zadejte **Lynda.com**.</span><span class="sxs-lookup"><span data-stu-id="494a0-133">In the search box, type **Lynda.com**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_search.png)

5. <span data-ttu-id="494a0-135">Na panelu výsledků vyberte **Lynda.com**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="494a0-135">In the results panel, select **Lynda.com**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="494a0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="494a0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="494a0-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lynda.com podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="494a0-138">In this section, you configure and test Azure AD single sign-on with Lynda.com based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="494a0-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Lynda.com je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="494a0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lynda.com is to a user in Azure AD.</span></span> <span data-ttu-id="494a0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Lynda.com musí navázat.</span><span class="sxs-lookup"><span data-stu-id="494a0-140">In other words, a link relationship between an Azure AD user and the related user in Lynda.com needs to be established.</span></span>

<span data-ttu-id="494a0-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="494a0-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Lynda.com.</span></span>

<span data-ttu-id="494a0-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lynda.com, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="494a0-142">To configure and test Azure AD single sign-on with Lynda.com, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="494a0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="494a0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="494a0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="494a0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="494a0-145">**[Vytvoření zkušebního uživatele Lynda.com](#creating-a-lyndacom-test-user)**  – Pokud chcete mít protějšek Britta Simon v Lynda.com propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="494a0-145">**[Creating a Lynda.com test user](#creating-a-lyndacom-test-user)** - to have a counterpart of Britta Simon in Lynda.com that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="494a0-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="494a0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="494a0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="494a0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="494a0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="494a0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="494a0-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="494a0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lynda.com application.</span></span>

<span data-ttu-id="494a0-150">**Ke konfiguraci Azure AD jednotné přihlašování s Lynda.com, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="494a0-150">**To configure Azure AD single sign-on with Lynda.com, perform the following steps:**</span></span>

1. <span data-ttu-id="494a0-151">Na portálu Azure na **Lynda.com** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="494a0-151">In the Azure portal, on the **Lynda.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="494a0-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="494a0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_samlbase.png)

3. <span data-ttu-id="494a0-155">Na **Lynda.com domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="494a0-155">On the **Lynda.com Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_url.png)

    <span data-ttu-id="494a0-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.lynda.com/Shibboleth.sso/InCommon?providerId=<url>&target=<url> `</span><span class="sxs-lookup"><span data-stu-id="494a0-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.lynda.com/Shibboleth.sso/InCommon?providerId=<url>&target=<url> `</span></span>

    > [!NOTE] 
    > <span data-ttu-id="494a0-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="494a0-158">This value is not real.</span></span> <span data-ttu-id="494a0-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="494a0-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="494a0-160">Obraťte se na [tým podpory Lynda.com klienta](https://www.linkedin.com/help/lynda/ask) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="494a0-160">Contact [Lynda.com Client support team](https://www.linkedin.com/help/lynda/ask) to get these values.</span></span> 
 
4. <span data-ttu-id="494a0-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="494a0-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_certificate.png) 

5. <span data-ttu-id="494a0-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="494a0-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="494a0-165">Konfigurace jednotného přihlašování na **Lynda.com** straně, budete muset odeslat stažené **soubor XML s metadaty** [Lynda.com podporu](https://www.linkedin.com/help/lynda/ask).</span><span class="sxs-lookup"><span data-stu-id="494a0-165">To configure single sign-on on **Lynda.com** side, you need to send the downloaded **Metadata XML** [Lynda.com support](https://www.linkedin.com/help/lynda/ask).</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="494a0-166">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="494a0-166">Creating an Azure AD test user</span></span>
<span data-ttu-id="494a0-167">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="494a0-167">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="494a0-169">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="494a0-169">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="494a0-170">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="494a0-170">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="494a0-172">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="494a0-172">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="494a0-174">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="494a0-174">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="494a0-176">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="494a0-176">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="494a0-178">a.</span><span class="sxs-lookup"><span data-stu-id="494a0-178">a.</span></span> <span data-ttu-id="494a0-179">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="494a0-179">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="494a0-180">b.</span><span class="sxs-lookup"><span data-stu-id="494a0-180">b.</span></span> <span data-ttu-id="494a0-181">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="494a0-181">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="494a0-182">c.</span><span class="sxs-lookup"><span data-stu-id="494a0-182">c.</span></span> <span data-ttu-id="494a0-183">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="494a0-183">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="494a0-184">d.</span><span class="sxs-lookup"><span data-stu-id="494a0-184">d.</span></span> <span data-ttu-id="494a0-185">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="494a0-185">Click **Create**.</span></span>
 
### <a name="creating-a-lyndacom-test-user"></a><span data-ttu-id="494a0-186">Vytvoření zkušebního uživatele Lynda.com</span><span class="sxs-lookup"><span data-stu-id="494a0-186">Creating a Lynda.com test user</span></span>

<span data-ttu-id="494a0-187">Neexistuje žádná položka akce můžete nakonfigurovat na Lynda.com zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="494a0-187">There is no action item for you to configure user provisioning to Lynda.com.</span></span>  
<span data-ttu-id="494a0-188">Když přiřazený uživatel se pokusí přihlásit k Lynda.com pomocí přístupového panelu, Lynda.com ověří, zda uživatel existuje.</span><span class="sxs-lookup"><span data-stu-id="494a0-188">When an assigned user tries to log in to Lynda.com using the access panel, Lynda.com checks whether the user exists.</span></span>  

<span data-ttu-id="494a0-189">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="494a0-189">If there is no user account available yet, it is automatically created by Lynda.com.</span></span>

>[!NOTE]
><span data-ttu-id="494a0-190">Můžete použít všechny ostatní Lynda.com uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Lynda.com zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="494a0-190">You can use any other Lynda.com user account creation tools or APIs provided by Lynda.com to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="494a0-191">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="494a0-191">Assigning the Azure AD test user</span></span>

<span data-ttu-id="494a0-192">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="494a0-192">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lynda.com.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="494a0-194">**Pokud chcete přiřadit Britta Simon Lynda.com, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="494a0-194">**To assign Britta Simon to Lynda.com, perform the following steps:**</span></span>

1. <span data-ttu-id="494a0-195">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="494a0-195">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="494a0-197">V seznamu aplikací vyberte **Lynda.com**.</span><span class="sxs-lookup"><span data-stu-id="494a0-197">In the applications list, select **Lynda.com**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_app.png) 

3. <span data-ttu-id="494a0-199">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="494a0-199">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="494a0-201">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="494a0-201">Click **Add** button.</span></span> <span data-ttu-id="494a0-202">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="494a0-202">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="494a0-204">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="494a0-204">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="494a0-205">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="494a0-205">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="494a0-206">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="494a0-206">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="494a0-207">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="494a0-207">Testing single sign-on</span></span>

<span data-ttu-id="494a0-208">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="494a0-208">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="494a0-209">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="494a0-209">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="494a0-210">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="494a0-210">Additional resources</span></span>

* [<span data-ttu-id="494a0-211">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="494a0-211">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="494a0-212">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="494a0-212">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_203.png

