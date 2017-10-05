---
title: 'Kurz: Azure Active Directory integrace s Pantheon | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Pantheon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 3f4ac1db2ee83d9f9fcb375d0fb7c40ad21c4688
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a><span data-ttu-id="39077-103">Kurz: Azure Active Directory integrace s Pantheon</span><span class="sxs-lookup"><span data-stu-id="39077-103">Tutorial: Azure Active Directory integration with Pantheon</span></span>

<span data-ttu-id="39077-104">V tomto kurzu zjistěte, jak integrovat Pantheon s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="39077-104">In this tutorial, you learn how to integrate Pantheon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="39077-105">Integrace Pantheon s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="39077-105">Integrating Pantheon with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="39077-106">Můžete řídit ve službě Azure AD, který má přístup k Pantheon</span><span class="sxs-lookup"><span data-stu-id="39077-106">You can control in Azure AD who has access to Pantheon</span></span>
- <span data-ttu-id="39077-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Pantheon (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="39077-107">You can enable your users to automatically get signed-on to Pantheon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="39077-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="39077-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="39077-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="39077-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39077-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="39077-110">Prerequisites</span></span>

<span data-ttu-id="39077-111">Konfigurace integrace Azure AD s Pantheon, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="39077-111">To configure Azure AD integration with Pantheon, you need the following items:</span></span>

- <span data-ttu-id="39077-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="39077-112">An Azure AD subscription</span></span>
- <span data-ttu-id="39077-113">Pantheon jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="39077-113">A Pantheon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="39077-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="39077-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="39077-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="39077-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="39077-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="39077-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="39077-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="39077-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="39077-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="39077-118">Scenario description</span></span>
<span data-ttu-id="39077-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="39077-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="39077-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="39077-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="39077-121">Přidání Pantheon z Galerie</span><span class="sxs-lookup"><span data-stu-id="39077-121">Adding Pantheon from the gallery</span></span>
2. <span data-ttu-id="39077-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="39077-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pantheon-from-the-gallery"></a><span data-ttu-id="39077-123">Přidání Pantheon z Galerie</span><span class="sxs-lookup"><span data-stu-id="39077-123">Adding Pantheon from the gallery</span></span>
<span data-ttu-id="39077-124">Při konfiguraci integrace Pantheon do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Pantheon z galerie.</span><span class="sxs-lookup"><span data-stu-id="39077-124">To configure the integration of Pantheon into Azure AD, you need to add Pantheon from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="39077-125">**Pokud chcete přidat Pantheon z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="39077-125">**To add Pantheon from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="39077-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="39077-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="39077-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="39077-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="39077-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="39077-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="39077-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="39077-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="39077-133">Do vyhledávacího pole zadejte **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="39077-133">In the search box, type **Pantheon**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_search.png)

5. <span data-ttu-id="39077-135">Na panelu výsledků vyberte **Pantheon**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="39077-135">In the results panel, select **Pantheon**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="39077-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="39077-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="39077-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pantheon podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="39077-138">In this section, you configure and test Azure AD single sign-on with Pantheon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="39077-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Pantheon je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="39077-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pantheon is to a user in Azure AD.</span></span> <span data-ttu-id="39077-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Pantheon musí navázat.</span><span class="sxs-lookup"><span data-stu-id="39077-140">In other words, a link relationship between an Azure AD user and the related user in Pantheon needs to be established.</span></span>

<span data-ttu-id="39077-141">V Pantheon, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="39077-141">In Pantheon, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="39077-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pantheon, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="39077-142">To configure and test Azure AD single sign-on with Pantheon, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="39077-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="39077-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="39077-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="39077-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="39077-145">**[Vytvoření zkušebního uživatele Pantheon](#creating-a-pantheon-test-user)**  – Pokud chcete mít protějšek Britta Simon v Pantheon propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="39077-145">**[Creating a Pantheon test user](#creating-a-pantheon-test-user)** - to have a counterpart of Britta Simon in Pantheon that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="39077-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="39077-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="39077-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="39077-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="39077-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="39077-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="39077-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Pantheon.</span><span class="sxs-lookup"><span data-stu-id="39077-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Pantheon application.</span></span>

<span data-ttu-id="39077-150">**Ke konfiguraci Azure AD jednotné přihlašování s Pantheon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="39077-150">**To configure Azure AD single sign-on with Pantheon, perform the following steps:**</span></span>

1. <span data-ttu-id="39077-151">Na portálu Azure na **Pantheon** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="39077-151">In the Azure portal, on the **Pantheon** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="39077-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="39077-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_samlbase.png)

3. <span data-ttu-id="39077-155">Na **Pantheon domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="39077-155">On the **Pantheon Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_url.png)

    <span data-ttu-id="39077-157">a.</span><span class="sxs-lookup"><span data-stu-id="39077-157">a.</span></span> <span data-ttu-id="39077-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`urn:auth0:pantheon:<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="39077-158">In the **Identifier** textbox, type a URL using the following pattern: `urn:auth0:pantheon:<orgname>-SSO`</span></span>

    <span data-ttu-id="39077-159">b.</span><span class="sxs-lookup"><span data-stu-id="39077-159">b.</span></span> <span data-ttu-id="39077-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="39077-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="39077-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="39077-161">These values are not real.</span></span> <span data-ttu-id="39077-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="39077-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="39077-163">Obraťte se na [tým podpory Pantheon](https://pantheon.io/docs/getting-support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="39077-163">Contact [Pantheon support team](https://pantheon.io/docs/getting-support/) to get these values.</span></span>

4. <span data-ttu-id="39077-164">Aplikace pantheon očekává kontrolního výrazu SAML v určitém formátu, který vyžaduje, abyste hodnotu atributu UserIdentifier s e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="39077-164">Pantheon application expects the SAML assertion in specific format, which requires you to set the UserIdentifier attribute value with the user’s email address.</span></span> <span data-ttu-id="39077-165">Ve výchozím nastavení používá Azure AD UserPrincipalName UserIdentifier atributu.</span><span class="sxs-lookup"><span data-stu-id="39077-165">By default Azure AD uses the UserPrincipalName for UserIdentifier attribute.</span></span> <span data-ttu-id="39077-166">Ale pro úspěšné integrace, je potřeba upravit tuto hodnotu tak, aby odpovídaly s e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="39077-166">But for successful integration you need to adjust this value to match with user’s email address.</span></span> <span data-ttu-id="39077-167">Integrace budou fungovat jenom po provedení správné mapování.</span><span class="sxs-lookup"><span data-stu-id="39077-167">The integration will only work after doing the correct mapping.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_attribute.png)  


5. <span data-ttu-id="39077-169">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="39077-169">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_certificate.png)

6. <span data-ttu-id="39077-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="39077-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="39077-173">Na **Pantheon konfigurace** klikněte na tlačítko **konfigurace Pantheon** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="39077-173">On the **Pantheon Configuration** section, click **Configure Pantheon** to open **Configure sign-on** window.</span></span> <span data-ttu-id="39077-174">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="39077-174">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_configure.png) 

8. <span data-ttu-id="39077-176">Konfigurace jednotného přihlašování na **Pantheon** straně, budete muset odeslat stažené **certifikát** a **SAML jeden přihlašování adresa URL služby** k [Pantheon tým podpory](https://pantheon.io/docs/getting-support/).</span><span class="sxs-lookup"><span data-stu-id="39077-176">To configure single sign-on on **Pantheon** side, you need to send the downloaded **Certificate** and **SAML Single Sign-On Service URL** to [Pantheon support team](https://pantheon.io/docs/getting-support/).</span></span>

     > [!Note]
     > <span data-ttu-id="39077-177">Musíte také poskytují informace o doménách e-mailu a datum a čas, pokud chcete povolit toto připojení.</span><span class="sxs-lookup"><span data-stu-id="39077-177">You also need to provide the Email Domain(s) information and Date Time when you want to enable this connection.</span></span> <span data-ttu-id="39077-178">Můžete najít další podrobnosti o z [sem](https://pantheon.io/docs/sso-organizations/)</span><span class="sxs-lookup"><span data-stu-id="39077-178">You can find more details about it from [here](https://pantheon.io/docs/sso-organizations/)</span></span>

> [!TIP]
> <span data-ttu-id="39077-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="39077-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="39077-180">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="39077-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="39077-181">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="39077-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="39077-182">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="39077-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="39077-183">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="39077-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="39077-185">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="39077-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="39077-186">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="39077-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="39077-188">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="39077-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="39077-190">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="39077-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="39077-192">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="39077-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="39077-194">a.</span><span class="sxs-lookup"><span data-stu-id="39077-194">a.</span></span> <span data-ttu-id="39077-195">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="39077-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="39077-196">b.</span><span class="sxs-lookup"><span data-stu-id="39077-196">b.</span></span> <span data-ttu-id="39077-197">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="39077-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="39077-198">c.</span><span class="sxs-lookup"><span data-stu-id="39077-198">c.</span></span> <span data-ttu-id="39077-199">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="39077-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="39077-200">d.</span><span class="sxs-lookup"><span data-stu-id="39077-200">d.</span></span> <span data-ttu-id="39077-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="39077-201">Click **Create**.</span></span>
 
### <a name="creating-a-pantheon-test-user"></a><span data-ttu-id="39077-202">Vytvoření zkušebního uživatele Pantheon</span><span class="sxs-lookup"><span data-stu-id="39077-202">Creating a Pantheon test user</span></span>

<span data-ttu-id="39077-203">V této části vytvoříte volal Britta Simon v Pantheon uživatele.</span><span class="sxs-lookup"><span data-stu-id="39077-203">In this section, you create a user called Britta Simon in Pantheon.</span></span> <span data-ttu-id="39077-204">Postupujte níže uvedených pokynů přidejte uživatele ve Pantheon.</span><span class="sxs-lookup"><span data-stu-id="39077-204">Please follow the below steps to add the user in Pantheon.</span></span> 

>[!NOTE] 
><span data-ttu-id="39077-205">Pro jednotné přihlašování pro práci uživatelů musí být nejprve vytvořen v Pantheon.</span><span class="sxs-lookup"><span data-stu-id="39077-205">For SSO to work user needs to be created first in Pantheon.</span></span>

1. <span data-ttu-id="39077-206">Přihlášení k Pantheon pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="39077-206">Login to Pantheon with admin credentials.</span></span>

2. <span data-ttu-id="39077-207">Přejděte na **organizace** stránka řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="39077-207">Navigate to **Organization** dashboard page.</span></span>
 
3. <span data-ttu-id="39077-208">Klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="39077-208">Click **People**.</span></span>

4. <span data-ttu-id="39077-209">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="39077-209">Click **Add user**.</span></span>

5. <span data-ttu-id="39077-210">Zadejte e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="39077-210">Enter the user's email address.</span></span>

6. <span data-ttu-id="39077-211">Vyberte roli uživatele.</span><span class="sxs-lookup"><span data-stu-id="39077-211">Choose the user's role.</span></span>

7. <span data-ttu-id="39077-212">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="39077-212">Click **Add user**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="39077-213">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="39077-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="39077-214">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Pantheon.</span><span class="sxs-lookup"><span data-stu-id="39077-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Pantheon.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="39077-216">**Pokud chcete přiřadit Britta Simon Pantheon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="39077-216">**To assign Britta Simon to Pantheon, perform the following steps:**</span></span>

1. <span data-ttu-id="39077-217">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="39077-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="39077-219">V seznamu aplikací vyberte **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="39077-219">In the applications list, select **Pantheon**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_app.png) 

3. <span data-ttu-id="39077-221">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="39077-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="39077-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="39077-223">Click **Add** button.</span></span> <span data-ttu-id="39077-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="39077-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="39077-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="39077-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="39077-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="39077-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="39077-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="39077-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="39077-229">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="39077-229">Testing single sign-on</span></span>

<span data-ttu-id="39077-230">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="39077-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="39077-231">Když kliknete na dlaždici Pantheon na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Pantheon.</span><span class="sxs-lookup"><span data-stu-id="39077-231">When you click the Pantheon tile in the Access Panel, you should get automatically signed-on to your Pantheon application.</span></span>
<span data-ttu-id="39077-232">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="39077-232">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="39077-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="39077-233">Additional resources</span></span>

* [<span data-ttu-id="39077-234">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="39077-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="39077-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="39077-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_203.png

