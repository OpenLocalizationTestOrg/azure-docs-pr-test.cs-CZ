---
title: 'Kurz: Azure Active Directory integrace s Convercent | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Convercent."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9c9d290-0e13-490b-b559-0be772d6a690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: 7af33cae7448a212734d327570b5082d0278fa0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-convercent"></a><span data-ttu-id="bf182-103">Kurz: Azure Active Directory integrace s Convercent</span><span class="sxs-lookup"><span data-stu-id="bf182-103">Tutorial: Azure Active Directory integration with Convercent</span></span>

<span data-ttu-id="bf182-104">V tomto kurzu zjistěte, jak integrovat Convercent s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bf182-104">In this tutorial, you learn how to integrate Convercent with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bf182-105">Integrace Convercent s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="bf182-105">Integrating Convercent with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bf182-106">Můžete řídit ve službě Azure AD, který má přístup k Convercent</span><span class="sxs-lookup"><span data-stu-id="bf182-106">You can control in Azure AD who has access to Convercent</span></span>
- <span data-ttu-id="bf182-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Convercent (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="bf182-107">You can enable your users to automatically get signed-on to Convercent (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bf182-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bf182-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bf182-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bf182-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf182-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bf182-110">Prerequisites</span></span>

<span data-ttu-id="bf182-111">Konfigurace integrace Azure AD s Convercent, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="bf182-111">To configure Azure AD integration with Convercent, you need the following items:</span></span>

- <span data-ttu-id="bf182-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bf182-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bf182-113">Convercent jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="bf182-113">A Convercent single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bf182-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bf182-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bf182-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="bf182-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bf182-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="bf182-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bf182-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bf182-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bf182-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="bf182-118">Scenario description</span></span>
<span data-ttu-id="bf182-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bf182-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bf182-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="bf182-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bf182-121">Přidání Convercent z Galerie</span><span class="sxs-lookup"><span data-stu-id="bf182-121">Adding Convercent from the gallery</span></span>
2. <span data-ttu-id="bf182-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bf182-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-convercent-from-the-gallery"></a><span data-ttu-id="bf182-123">Přidání Convercent z Galerie</span><span class="sxs-lookup"><span data-stu-id="bf182-123">Adding Convercent from the gallery</span></span>
<span data-ttu-id="bf182-124">Při konfiguraci integrace Convercent do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Convercent z galerie.</span><span class="sxs-lookup"><span data-stu-id="bf182-124">To configure the integration of Convercent into Azure AD, you need to add Convercent from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bf182-125">**Pokud chcete přidat Convercent z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bf182-125">**To add Convercent from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bf182-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bf182-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bf182-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="bf182-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bf182-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bf182-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="bf182-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bf182-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="bf182-133">Do vyhledávacího pole zadejte **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="bf182-133">In the search box, type **Convercent**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_search.png)

5. <span data-ttu-id="bf182-135">Na panelu výsledků vyberte **Convercent**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bf182-135">In the results panel, select **Convercent**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bf182-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bf182-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bf182-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Convercent podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="bf182-138">In this section, you configure and test Azure AD single sign-on with Convercent based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bf182-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Convercent je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bf182-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Convercent is to a user in Azure AD.</span></span> <span data-ttu-id="bf182-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Convercent musí navázat.</span><span class="sxs-lookup"><span data-stu-id="bf182-140">In other words, a link relationship between an Azure AD user and the related user in Convercent needs to be established.</span></span>

<span data-ttu-id="bf182-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Convercent.</span><span class="sxs-lookup"><span data-stu-id="bf182-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Convercent.</span></span>

<span data-ttu-id="bf182-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Convercent, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="bf182-142">To configure and test Azure AD single sign-on with Convercent, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bf182-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="bf182-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bf182-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bf182-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bf182-145">**[Vytvoření zkušebního uživatele Convercent](#creating-a-convercent-test-user)**  – Pokud chcete mít protějšek Britta Simon v Convercent propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="bf182-145">**[Creating a Convercent test user](#creating-a-convercent-test-user)** - to have a counterpart of Britta Simon in Convercent that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bf182-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bf182-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bf182-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bf182-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bf182-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bf182-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bf182-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Convercent.</span><span class="sxs-lookup"><span data-stu-id="bf182-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Convercent application.</span></span>

<span data-ttu-id="bf182-150">**Ke konfiguraci Azure AD jednotné přihlašování s Convercent, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bf182-150">**To configure Azure AD single sign-on with Convercent, perform the following steps:**</span></span>

1. <span data-ttu-id="bf182-151">Na portálu Azure na **Convercent** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bf182-151">In the Azure portal, on the **Convercent** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="bf182-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bf182-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_samlbase.png)

3. <span data-ttu-id="bf182-155">Na **Convercent domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP iniciované režimu**, provést následující krok:</span><span class="sxs-lookup"><span data-stu-id="bf182-155">On the **Convercent Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**, perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url.png)

    <span data-ttu-id="bf182-157">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="bf182-157">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.convercent.com/`</span></span>
 
4. <span data-ttu-id="bf182-158">Pokud chcete nakonfigurovat aplikace **SP iniciované režimu**na **Convercent domény a adresy URL** části provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bf182-158">If you wish to configure the application in **SP initiated mode**, on the **Convercent Domain and URLs** section perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url1.png)

     <span data-ttu-id="bf182-160">a.</span><span class="sxs-lookup"><span data-stu-id="bf182-160">a.</span></span> <span data-ttu-id="bf182-161">Klikněte na tlačítko **"Zobrazit rozšířené nastavení adresy URL."**</span><span class="sxs-lookup"><span data-stu-id="bf182-161">Click **"Show advanced URL settings."**</span></span> 

     <span data-ttu-id="bf182-162">b.</span><span class="sxs-lookup"><span data-stu-id="bf182-162">b.</span></span> <span data-ttu-id="bf182-163">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="bf182-163">In the **Sign On URL** textbox, type the value using the following pattern: `https://<instancename>.convercent.com/`</span></span>

     <span data-ttu-id="bf182-164">c.</span><span class="sxs-lookup"><span data-stu-id="bf182-164">c.</span></span> <span data-ttu-id="bf182-165">V **předávání stavu** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="bf182-165">In the **Relay State** textbox, type the value using the following pattern: `https://<instancename>.convercent.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bf182-166">Tyto hodnoty nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bf182-166">These values are not the real values.</span></span> <span data-ttu-id="bf182-167">Tyto hodnoty aktualizujte skutečné identifikátor, přihlašovací adresa URL a předávací stavu.</span><span class="sxs-lookup"><span data-stu-id="bf182-167">Update these values with the actual Identifier, Sign On URL and Relay State.</span></span> <span data-ttu-id="bf182-168">Obraťte se na [tým podpory Convercent klienta](http://support.convercent.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="bf182-168">Contact [Convercent Client support team](http://support.convercent.com) to get these values.</span></span>

5. <span data-ttu-id="bf182-169">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="bf182-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_certificate.png) 

6. <span data-ttu-id="bf182-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bf182-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="bf182-173">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [tým podpory Convercent](mailto:support@convercent.com) a jim poskytnout stažený **soubor XML s metadaty**.</span><span class="sxs-lookup"><span data-stu-id="bf182-173">To get SSO configured for your application, contact [Convercent support team](mailto:support@convercent.com) and provide them with the downloaded **Metadata XML**.</span></span>

> [!TIP]
> <span data-ttu-id="bf182-174">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="bf182-174">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bf182-175">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="bf182-175">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bf182-176">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bf182-176">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bf182-177">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bf182-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="bf182-178">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bf182-178">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="bf182-180">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bf182-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bf182-181">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bf182-181">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bf182-183">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="bf182-183">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bf182-185">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bf182-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bf182-187">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bf182-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bf182-189">a.</span><span class="sxs-lookup"><span data-stu-id="bf182-189">a.</span></span> <span data-ttu-id="bf182-190">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bf182-190">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bf182-191">b.</span><span class="sxs-lookup"><span data-stu-id="bf182-191">b.</span></span> <span data-ttu-id="bf182-192">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bf182-192">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bf182-193">c.</span><span class="sxs-lookup"><span data-stu-id="bf182-193">c.</span></span> <span data-ttu-id="bf182-194">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="bf182-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bf182-195">d.</span><span class="sxs-lookup"><span data-stu-id="bf182-195">d.</span></span> <span data-ttu-id="bf182-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bf182-196">Click **Create**.</span></span>
 
### <a name="creating-a-convercent-test-user"></a><span data-ttu-id="bf182-197">Vytvoření zkušebního uživatele Convercent</span><span class="sxs-lookup"><span data-stu-id="bf182-197">Creating a Convercent test user</span></span>

<span data-ttu-id="bf182-198">Práce s [tým podpory Convercent](mailto:support@convercent.com) přidat uživatele do Convercent platformy.</span><span class="sxs-lookup"><span data-stu-id="bf182-198">Work with [Convercent support team](mailto:support@convercent.com) to add the users in the Convercent platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bf182-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bf182-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bf182-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Convercent.</span><span class="sxs-lookup"><span data-stu-id="bf182-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Convercent.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="bf182-202">**Pokud chcete přiřadit Britta Simon Convercent, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bf182-202">**To assign Britta Simon to Convercent, perform the following steps:**</span></span>

1. <span data-ttu-id="bf182-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bf182-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="bf182-205">V seznamu aplikací vyberte **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="bf182-205">In the applications list, select **Convercent**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_app.png) 

3. <span data-ttu-id="bf182-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="bf182-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="bf182-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bf182-209">Click **Add** button.</span></span> <span data-ttu-id="bf182-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bf182-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="bf182-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bf182-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bf182-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bf182-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bf182-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bf182-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bf182-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bf182-215">Testing single sign-on</span></span>

<span data-ttu-id="bf182-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bf182-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="bf182-217">Když kliknete na dlaždici Convercent na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Convercent.</span><span class="sxs-lookup"><span data-stu-id="bf182-217">When you click the Convercent tile in the Access Panel, you should get automatically signed-on to your Convercent application.</span></span>
<span data-ttu-id="bf182-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bf182-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bf182-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bf182-219">Additional resources</span></span>

* [<span data-ttu-id="bf182-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bf182-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bf182-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bf182-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_203.png

