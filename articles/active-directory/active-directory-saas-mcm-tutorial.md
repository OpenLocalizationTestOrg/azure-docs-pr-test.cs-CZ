---
title: 'Kurz: Azure Active Directory integrace s MCM | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a MCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7f00799d-e3e9-4ba9-ae4a-fbca843ac5db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: cbbb0f54b7954c0ec7326fb62bb427155527cc84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mcm"></a><span data-ttu-id="254d4-103">Kurz: Azure Active Directory integrace s MCM</span><span class="sxs-lookup"><span data-stu-id="254d4-103">Tutorial: Azure Active Directory integration with MCM</span></span>

<span data-ttu-id="254d4-104">V tomto kurzu zjistěte, jak integrovat MCM s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="254d4-104">In this tutorial, you learn how to integrate MCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="254d4-105">Integrace MCM s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="254d4-105">Integrating MCM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="254d4-106">Můžete řídit ve službě Azure AD, který má přístup k MCM</span><span class="sxs-lookup"><span data-stu-id="254d4-106">You can control in Azure AD who has access to MCM</span></span>
- <span data-ttu-id="254d4-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k MCM (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="254d4-107">You can enable your users to automatically get signed-on to MCM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="254d4-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="254d4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="254d4-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="254d4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="254d4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="254d4-110">Prerequisites</span></span>

<span data-ttu-id="254d4-111">Konfigurace integrace Azure AD s MCM, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="254d4-111">To configure Azure AD integration with MCM, you need the following items:</span></span>

- <span data-ttu-id="254d4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="254d4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="254d4-113">MCM jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="254d4-113">A MCM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="254d4-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="254d4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="254d4-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="254d4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="254d4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="254d4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="254d4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="254d4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="254d4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="254d4-118">Scenario description</span></span>
<span data-ttu-id="254d4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="254d4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="254d4-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="254d4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="254d4-121">Přidání MCM z Galerie</span><span class="sxs-lookup"><span data-stu-id="254d4-121">Adding MCM from the gallery</span></span>
2. <span data-ttu-id="254d4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="254d4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mcm-from-the-gallery"></a><span data-ttu-id="254d4-123">Přidání MCM z Galerie</span><span class="sxs-lookup"><span data-stu-id="254d4-123">Adding MCM from the gallery</span></span>
<span data-ttu-id="254d4-124">Při konfiguraci integrace MCM do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS MCM z galerie.</span><span class="sxs-lookup"><span data-stu-id="254d4-124">To configure the integration of MCM into Azure AD, you need to add MCM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="254d4-125">**Pokud chcete přidat MCM z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="254d4-125">**To add MCM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="254d4-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="254d4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="254d4-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="254d4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="254d4-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="254d4-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="254d4-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="254d4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="254d4-133">Do vyhledávacího pole zadejte **MCM**.</span><span class="sxs-lookup"><span data-stu-id="254d4-133">In the search box, type **MCM**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_search.png)

5. <span data-ttu-id="254d4-135">Na panelu výsledků vyberte **MCM**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="254d4-135">In the results panel, select **MCM**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="254d4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="254d4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="254d4-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s MCM podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="254d4-138">In this section, you configure and test Azure AD single sign-on with MCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="254d4-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v MCM je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="254d4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in MCM is to a user in Azure AD.</span></span> <span data-ttu-id="254d4-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v MCM musí navázat.</span><span class="sxs-lookup"><span data-stu-id="254d4-140">In other words, a link relationship between an Azure AD user and the related user in MCM needs to be established.</span></span>

<span data-ttu-id="254d4-141">V MCM, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="254d4-141">In MCM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="254d4-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s MCM, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="254d4-142">To configure and test Azure AD single sign-on with MCM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="254d4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="254d4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="254d4-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="254d4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="254d4-145">**[Vytváření MCM testovací uživatel](#creating-a-mcm-test-user)**  – Pokud chcete mít protějšek Britta Simon v MCM propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="254d4-145">**[Creating a MCM test user](#creating-a-mcm-test-user)** - to have a counterpart of Britta Simon in MCM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="254d4-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="254d4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="254d4-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="254d4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="254d4-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="254d4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="254d4-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci MCM.</span><span class="sxs-lookup"><span data-stu-id="254d4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your MCM application.</span></span>

<span data-ttu-id="254d4-150">**Ke konfiguraci Azure AD jednotné přihlašování s MCM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="254d4-150">**To configure Azure AD single sign-on with MCM, perform the following steps:**</span></span>

1. <span data-ttu-id="254d4-151">Na portálu Azure na **MCM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="254d4-151">In the Azure portal, on the **MCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="254d4-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="254d4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_samlbase.png)

3. <span data-ttu-id="254d4-155">Na **MCM domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="254d4-155">On the **MCM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_url.png)

    <span data-ttu-id="254d4-157">a.</span><span class="sxs-lookup"><span data-stu-id="254d4-157">a.</span></span> <span data-ttu-id="254d4-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://myaba.co.uk/client-access/<companyname>/saml.php`</span><span class="sxs-lookup"><span data-stu-id="254d4-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://myaba.co.uk/client-access/<companyname>/saml.php`</span></span>

    <span data-ttu-id="254d4-159">b.</span><span class="sxs-lookup"><span data-stu-id="254d4-159">b.</span></span> <span data-ttu-id="254d4-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://myaba.co.uk/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="254d4-160">In the **Identifier** textbox, type a URL using the following pattern: `https://myaba.co.uk/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="254d4-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="254d4-161">These values are not real.</span></span> <span data-ttu-id="254d4-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="254d4-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="254d4-163">Obraťte se na [tým podpory MCM klienta](http://mcmtechnology.com/support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="254d4-163">Contact [MCM Client support team](http://mcmtechnology.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="254d4-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="254d4-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_certificate.png) 

5. <span data-ttu-id="254d4-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="254d4-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="254d4-168">Konfigurace jednotného přihlašování na **MCM** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory MCM](http://mcmtechnology.com/support/).</span><span class="sxs-lookup"><span data-stu-id="254d4-168">To configure single sign-on on **MCM** side, you need to send the downloaded **Metadata XML** to [MCM support team](http://mcmtechnology.com/support/).</span></span> <span data-ttu-id="254d4-169">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="254d4-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="254d4-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="254d4-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="254d4-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="254d4-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="254d4-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="254d4-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="254d4-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="254d4-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="254d4-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="254d4-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="254d4-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="254d4-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="254d4-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="254d4-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="254d4-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="254d4-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="254d4-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="254d4-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="254d4-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="254d4-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="254d4-185">a.</span><span class="sxs-lookup"><span data-stu-id="254d4-185">a.</span></span> <span data-ttu-id="254d4-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="254d4-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="254d4-187">b.</span><span class="sxs-lookup"><span data-stu-id="254d4-187">b.</span></span> <span data-ttu-id="254d4-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="254d4-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="254d4-189">c.</span><span class="sxs-lookup"><span data-stu-id="254d4-189">c.</span></span> <span data-ttu-id="254d4-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="254d4-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="254d4-191">d.</span><span class="sxs-lookup"><span data-stu-id="254d4-191">d.</span></span> <span data-ttu-id="254d4-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="254d4-192">Click **Create**.</span></span>
 
### <a name="creating-a-mcm-test-user"></a><span data-ttu-id="254d4-193">Vytvoření zkušebního uživatele MCM</span><span class="sxs-lookup"><span data-stu-id="254d4-193">Creating a MCM test user</span></span>

<span data-ttu-id="254d4-194">V této části vytvoříte volal Britta Simon v MCM uživatele.</span><span class="sxs-lookup"><span data-stu-id="254d4-194">In this section, you create a user called Britta Simon in MCM.</span></span> <span data-ttu-id="254d4-195">Práce s [tým podpory MCM](http://mcmtechnology.com/support/) přidat uživatele do MCM platformy.</span><span class="sxs-lookup"><span data-stu-id="254d4-195">Work with [MCM support team](http://mcmtechnology.com/support/) to add the users in the MCM platform.</span></span>

> [!NOTE]
> <span data-ttu-id="254d4-196">Můžete použít všechny ostatní MCM uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované MCM zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="254d4-196">You can use any other MCM user account creation tools or APIs provided by MCM to provision AAD user accounts.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="254d4-197">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="254d4-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="254d4-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu MCM.</span><span class="sxs-lookup"><span data-stu-id="254d4-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to MCM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="254d4-200">**Pokud chcete přiřadit Britta Simon MCM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="254d4-200">**To assign Britta Simon to MCM, perform the following steps:**</span></span>

1. <span data-ttu-id="254d4-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="254d4-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="254d4-203">V seznamu aplikací vyberte **MCM**.</span><span class="sxs-lookup"><span data-stu-id="254d4-203">In the applications list, select **MCM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_app.png) 

3. <span data-ttu-id="254d4-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="254d4-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="254d4-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="254d4-207">Click **Add** button.</span></span> <span data-ttu-id="254d4-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="254d4-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="254d4-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="254d4-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="254d4-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="254d4-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="254d4-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="254d4-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="254d4-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="254d4-213">Testing single sign-on</span></span>

<span data-ttu-id="254d4-214">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="254d4-214">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="254d4-215">Když kliknete na dlaždici MCM na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci MCM.</span><span class="sxs-lookup"><span data-stu-id="254d4-215">When you click the MCM tile in the Access Panel, you should get automatically signed-on to your MCM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="254d4-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="254d4-216">Additional resources</span></span>

* [<span data-ttu-id="254d4-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="254d4-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="254d4-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="254d4-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_203.png

