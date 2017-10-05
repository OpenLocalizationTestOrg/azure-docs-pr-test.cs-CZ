---
title: 'Kurz: Azure Active Directory integrace s Halosys | Microsoft Docs'
description: "Další informace o použití Halosys s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18c5cd8eb4ca211f8ae2b8dd994c0e8c48625a2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a><span data-ttu-id="2d6e9-103">Kurz: Azure Active Directory integrace s Halosys</span><span class="sxs-lookup"><span data-stu-id="2d6e9-103">Tutorial: Azure Active Directory integration with Halosys</span></span>

<span data-ttu-id="2d6e9-104">V tomto kurzu zjistěte, jak integrovat Halosys s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2d6e9-104">In this tutorial, you learn how to integrate Halosys with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2d6e9-105">Integrace Halosys s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2d6e9-105">Integrating Halosys with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2d6e9-106">Můžete řídit ve službě Azure AD, který má přístup k Halosys</span><span class="sxs-lookup"><span data-stu-id="2d6e9-106">You can control in Azure AD who has access to Halosys</span></span>
- <span data-ttu-id="2d6e9-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Halosys (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d6e9-107">You can enable your users to automatically get signed-on to Halosys (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2d6e9-108">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="2d6e9-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="2d6e9-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2d6e9-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d6e9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2d6e9-110">Prerequisites</span></span>

<span data-ttu-id="2d6e9-111">Konfigurace integrace Azure AD s Halosys, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="2d6e9-111">To configure Azure AD integration with Halosys, you need the following items:</span></span>

- <span data-ttu-id="2d6e9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d6e9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2d6e9-113">Halosys jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2d6e9-113">A Halosys single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="2d6e9-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="2d6e9-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2d6e9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2d6e9-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="2d6e9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2d6e9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="2d6e9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2d6e9-118">Scenario description</span></span>
<span data-ttu-id="2d6e9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="2d6e9-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2d6e9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2d6e9-121">Přidání Halosys z Galerie</span><span class="sxs-lookup"><span data-stu-id="2d6e9-121">Adding Halosys from the gallery</span></span>
2. <span data-ttu-id="2d6e9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d6e9-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-halosys-from-the-gallery"></a><span data-ttu-id="2d6e9-123">Přidání Halosys z Galerie</span><span class="sxs-lookup"><span data-stu-id="2d6e9-123">Adding Halosys from the gallery</span></span>
<span data-ttu-id="2d6e9-124">Při konfiguraci integrace Halosys do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Halosys z galerie.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-124">To configure the integration of Halosys into Azure AD, you need to add Halosys from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2d6e9-125">**Pokud chcete přidat Halosys z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2d6e9-125">**To add Halosys from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2d6e9-126">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]
2. <span data-ttu-id="2d6e9-128">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="2d6e9-129">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Aplikace][2]

4. <span data-ttu-id="2d6e9-131">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-131">Click **Add** at the bottom of the page.</span></span>

    ![Aplikace][3]

5. <span data-ttu-id="2d6e9-133">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>

    ![Aplikace][4]

6. <span data-ttu-id="2d6e9-135">Do vyhledávacího pole zadejte **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-135">In the search box, type **Halosys**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. <span data-ttu-id="2d6e9-137">V podokně výsledků vyberte **Halosys**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-137">In the results pane, select **Halosys**, and then click **Complete** to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2d6e9-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d6e9-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2d6e9-140">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Halosys podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2d6e9-140">In this section, you configure and test Azure AD single sign-on with Halosys based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2d6e9-141">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Halosys je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-141">For single sign-on to work, Azure AD needs to know what the counterpart user in Halosys is to a user in Azure AD.</span></span> <span data-ttu-id="2d6e9-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Halosys musí navázat.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-142">In other words, a link relationship between an Azure AD user and the related user in Halosys needs to be established.</span></span>

<span data-ttu-id="2d6e9-143">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Halosys.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Halosys.</span></span>

<span data-ttu-id="2d6e9-144">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Halosys, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="2d6e9-144">To configure and test Azure AD single sign-on with Halosys, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2d6e9-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2d6e9-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2d6e9-147">**[Vytvoření zkušebního uživatele Halosys](#creating-a-halosys-test-user)**  – Pokud chcete mít protějšek Britta Simon v Halosys propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-147">**[Creating a Halosys test user](#creating-a-halosys-test-user)** - to have a counterpart of Britta Simon in Halosys that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="2d6e9-148">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2d6e9-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2d6e9-150">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d6e9-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2d6e9-151">V této části můžete povolit Azure AD jednotného přihlašování na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci Halosys.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your Halosys application.</span></span>


<span data-ttu-id="2d6e9-152">**Ke konfiguraci Azure AD jednotné přihlašování s Halosys, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2d6e9-152">**To configure Azure AD single sign-on with Halosys, perform the following steps:**</span></span>

1. <span data-ttu-id="2d6e9-153">Na portálu classic na **Halosys** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-153">In the classic portal, on the **Halosys** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
     
    ![Konfigurovat jednotné přihlašování][6] 

2. <span data-ttu-id="2d6e9-155">Na **jak chcete uživatelům se přihlásit Halosys** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-155">On the **How would you like users to sign on to Halosys** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. <span data-ttu-id="2d6e9-157">Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2d6e9-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    <span data-ttu-id="2d6e9-159">a.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-159">a.</span></span> <span data-ttu-id="2d6e9-160">V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlášení do aplikace Halosys pomocí následujícího vzorce: `https://<company-name>.Halosys.com/client-api/api`.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-160">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your Halosys application using the following pattern: `https://<company-name>.Halosys.com/client-api/api`.</span></span>

    <span data-ttu-id="2d6e9-161">b.In **identifikátoru adresy URL** textovému poli, zadejte adresu URL v následujících vzoru: `https://<company-name>.Halosys.com`.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-161">b.In the **Identifier URL** textbox, type the URL in the following pattern: `https://<company-name>.Halosys.com`.</span></span>   
         
4. <span data-ttu-id="2d6e9-162">Na **nakonfigurovat jednotné přihlašování v Halosys** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor v počítači:</span><span class="sxs-lookup"><span data-stu-id="2d6e9-162">On the **Configure single sign-on at Halosys** page, click **Download metadata**, and then save the file on your computer:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. <span data-ttu-id="2d6e9-164">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Halosys a poskytněte s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="2d6e9-164">To get SSO configured for your application, contact Halosys support team and provide them with the following:</span></span>

    <span data-ttu-id="2d6e9-165">• Stažené **soubor metadat**</span><span class="sxs-lookup"><span data-stu-id="2d6e9-165">• The downloaded **metadata file**</span></span>
    
    <span data-ttu-id="2d6e9-166">• **URL jednotné přihlašování SAML**</span><span class="sxs-lookup"><span data-stu-id="2d6e9-166">• The **SAML SSO URL**</span></span>
    

6. <span data-ttu-id="2d6e9-167">Klasický portál, vyberte potvrzení konfigurace přihlášení a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-167">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Azure AD jednotné přihlášení][10]

7. <span data-ttu-id="2d6e9-169">Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-169">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2d6e9-171">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d6e9-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="2d6e9-172">V této části vytvoříte testovacího uživatele na portálu classic názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-172">In this section, you create a test user in the classic portal called Britta Simon.</span></span>


![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="2d6e9-174">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2d6e9-174">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2d6e9-175">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-175">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="2d6e9-177">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-177">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="2d6e9-178">Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-178">To display the list of users, in the menu on the top, click **Users**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2d6e9-180">Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-180">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="2d6e9-182">Na **Povězte nám o tohoto uživatele** dialogové okno stránky, proveďte následující kroky: ![vytváření testovací uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span><span class="sxs-lookup"><span data-stu-id="2d6e9-182">On the **Tell us about this user** dialog page, perform the following steps:  ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span></span> 

    <span data-ttu-id="2d6e9-183">a.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-183">a.</span></span> <span data-ttu-id="2d6e9-184">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-184">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="2d6e9-185">b.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-185">b.</span></span> <span data-ttu-id="2d6e9-186">V uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-186">In the User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2d6e9-187">c.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-187">c.</span></span> <span data-ttu-id="2d6e9-188">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-188">Click **Next**.</span></span>

6.  <span data-ttu-id="2d6e9-189">Na **profil uživatele** dialogové okno stránky, proveďte následující kroky: ![vytváření testovací uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span><span class="sxs-lookup"><span data-stu-id="2d6e9-189">On the **User Profile** dialog page, perform the following steps: ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span></span> 

    <span data-ttu-id="2d6e9-190">a.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-190">a.</span></span> <span data-ttu-id="2d6e9-191">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-191">In the **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="2d6e9-192">b.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-192">b.</span></span> <span data-ttu-id="2d6e9-193">V **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-193">In the **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="2d6e9-194">c.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-194">c.</span></span> <span data-ttu-id="2d6e9-195">V **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-195">In the **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="2d6e9-196">d.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-196">d.</span></span> <span data-ttu-id="2d6e9-197">V **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-197">In the **Role** list, select **User**.</span></span>

    <span data-ttu-id="2d6e9-198">e.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-198">e.</span></span> <span data-ttu-id="2d6e9-199">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-199">Click **Next**.</span></span>

7. <span data-ttu-id="2d6e9-200">Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-200">On the **Get temporary password** dialog page, click **create**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="2d6e9-202">Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2d6e9-202">On the **Get temporary password** dialog page, perform the following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="2d6e9-204">a.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-204">a.</span></span> <span data-ttu-id="2d6e9-205">Poznamenejte si hodnotu **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-205">Write down the value of the **New Password**.</span></span>

    <span data-ttu-id="2d6e9-206">b.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-206">b.</span></span> <span data-ttu-id="2d6e9-207">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-207">Click **Complete**.</span></span>   



### <a name="creating-a-halosys-test-user"></a><span data-ttu-id="2d6e9-208">Vytvoření zkušebního uživatele Halosys</span><span class="sxs-lookup"><span data-stu-id="2d6e9-208">Creating a Halosys test user</span></span>

<span data-ttu-id="2d6e9-209">V této části vytvoříte volal Britta Simon v Halosys uživatele.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-209">In this section, you create a user called Britta Simon in Halosys.</span></span> <span data-ttu-id="2d6e9-210">Spojte se s Halosys tým podpory pro přidání uživatele v platformě Halosys.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-210">Please work with Halosys support team to add the users in the Halosys platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2d6e9-211">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d6e9-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2d6e9-212">V této části povolíte Britta Simon používat tak, že udělíte přístup k Halosys Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-212">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Halosys.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2d6e9-214">**Pokud chcete přiřadit Britta Simon Halosys, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2d6e9-214">**To assign Britta Simon to Halosys, perform the following steps:**</span></span>

1. <span data-ttu-id="2d6e9-215">Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-215">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2d6e9-217">V seznamu aplikací vyberte **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-217">In the applications list, select **Halosys**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. <span data-ttu-id="2d6e9-219">V nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-219">In the menu on the top, click **Users**.</span></span>

    ![Přiřadit uživatele][203]

4. <span data-ttu-id="2d6e9-221">V seznamu uživatelů vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-221">In the Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="2d6e9-222">Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-222">In the toolbar on the bottom, click **Assign**.</span></span>

    ![Přiřadit uživatele][205]


### <a name="testing-single-sign-on"></a><span data-ttu-id="2d6e9-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d6e9-224">Testing single sign-on</span></span>

<span data-ttu-id="2d6e9-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2d6e9-226">Když kliknete na dlaždici Halosys na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Halosys.</span><span class="sxs-lookup"><span data-stu-id="2d6e9-226">When you click the Halosys tile in the Access Panel, you should get automatically signed-on to your Halosys application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="2d6e9-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2d6e9-227">Additional resources</span></span>

* [<span data-ttu-id="2d6e9-228">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2d6e9-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2d6e9-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2d6e9-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
