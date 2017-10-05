---
title: "Kurz: Azure Active Directory integrace s klientem Gorilla krajině | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a krajině Gorilla."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: jeedes
ms.openlocfilehash: 744c420aa0298c59c44e645b95a716ad876752de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-land-gorilla-client"></a><span data-ttu-id="20587-103">Kurz: Azure Active Directory integrace s pevnou Gorilla klienta</span><span class="sxs-lookup"><span data-stu-id="20587-103">Tutorial: Azure Active Directory integration with Land Gorilla Client</span></span>

<span data-ttu-id="20587-104">V tomto kurzu zjistíte integrace krajině Gorilla klienta služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="20587-104">In this tutorial, you learn how to integrate Land Gorilla Client with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="20587-105">Integrace klienta Gorilla krajině s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="20587-105">Integrating Land Gorilla Client with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="20587-106">Můžete řídit ve službě Azure AD, který má přístup k krajině Gorilla klienta</span><span class="sxs-lookup"><span data-stu-id="20587-106">You can control in Azure AD who has access to Land Gorilla Client</span></span>
- <span data-ttu-id="20587-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k krajině Gorilla klienta (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="20587-107">You can enable your users to automatically get signed-on to Land Gorilla Client (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="20587-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="20587-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="20587-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="20587-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="20587-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="20587-110">Prerequisites</span></span>

<span data-ttu-id="20587-111">Konfigurace integrace Azure AD s pevnou Gorilla klienta, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="20587-111">To configure Azure AD integration with Land Gorilla Client, you need the following items:</span></span>

- <span data-ttu-id="20587-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="20587-112">An Azure AD subscription</span></span>
- <span data-ttu-id="20587-113">Klient Gorilla krajině jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="20587-113">A Land Gorilla Client single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="20587-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="20587-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="20587-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="20587-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="20587-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="20587-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="20587-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="20587-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="20587-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="20587-118">Scenario description</span></span>
<span data-ttu-id="20587-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="20587-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="20587-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="20587-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="20587-121">Přidání klienta Gorilla krajině z Galerie</span><span class="sxs-lookup"><span data-stu-id="20587-121">Adding Land Gorilla Client from the gallery</span></span>
2. <span data-ttu-id="20587-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="20587-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-land-gorilla-client-from-the-gallery"></a><span data-ttu-id="20587-123">Přidání klienta Gorilla krajině z Galerie</span><span class="sxs-lookup"><span data-stu-id="20587-123">Adding Land Gorilla Client from the gallery</span></span>
<span data-ttu-id="20587-124">Při konfiguraci integrace krajině Gorilla klienta do služby Azure AD, potřebujete přidat krajině Gorilla klienta z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="20587-124">To configure the integration of Land Gorilla Client into Azure AD, you need to add Land Gorilla Client from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="20587-125">**Postup přidání klienta Gorilla krajině z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="20587-125">**To add Land Gorilla Client from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="20587-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="20587-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="20587-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="20587-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="20587-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="20587-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="20587-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20587-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="20587-133">Do vyhledávacího pole zadejte **krajině Gorilla klienta**.</span><span class="sxs-lookup"><span data-stu-id="20587-133">In the search box, type **Land Gorilla Client**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_search.png)

5. <span data-ttu-id="20587-135">Na panelu výsledků vyberte **krajině Gorilla klienta**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="20587-135">In the results panel, select **Land Gorilla Client**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="20587-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="20587-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="20587-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s pevnou Gorilla klienta na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="20587-138">In this section, you configure and test Azure AD single sign-on with Land Gorilla Client based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="20587-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v klientovi Gorilla krajině je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20587-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Land Gorilla Client is to a user in Azure AD.</span></span> <span data-ttu-id="20587-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v klientovi Gorilla krajině musí navázat.</span><span class="sxs-lookup"><span data-stu-id="20587-140">In other words, a link relationship between an Azure AD user and the related user in Land Gorilla Client needs to be established.</span></span>

<span data-ttu-id="20587-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v krajině Gorilla klienta.</span><span class="sxs-lookup"><span data-stu-id="20587-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Land Gorilla Client.</span></span>

<span data-ttu-id="20587-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s pevnou Gorilla klienta, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="20587-142">To configure and test Azure AD single sign-on with Land Gorilla Client, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="20587-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="20587-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="20587-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s omezenou skupinu.</span><span class="sxs-lookup"><span data-stu-id="20587-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with limited group.</span></span>
3. <span data-ttu-id="20587-145">**[Vytvoření zkušebního uživatele krajině Gorilla](#creating-a-land-gorilla-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="20587-145">**[Creating a Land Gorilla test user](#creating-a-land-gorilla-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="20587-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="20587-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="20587-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="20587-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="20587-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="20587-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="20587-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v krajině Gorilla klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="20587-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Land Gorilla Client application.</span></span>

<span data-ttu-id="20587-150">**Ke konfiguraci Azure AD jednotné přihlašování s pevnou Gorilla klienta, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="20587-150">**To configure Azure AD single sign-on with Land Gorilla Client, perform the following steps:**</span></span>

1. <span data-ttu-id="20587-151">Na portálu Azure Management portal na **krajině Gorilla klienta** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="20587-151">In the Azure Management portal, on the **Land Gorilla Client** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="20587-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="20587-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_samlbase.png)

3. <span data-ttu-id="20587-155">Na **domény krajině Gorilla klienta a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20587-155">On the **Land Gorilla Client Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_url_02.png)

    <span data-ttu-id="20587-157">a.</span><span class="sxs-lookup"><span data-stu-id="20587-157">a.</span></span> <span data-ttu-id="20587-158">V **identifikátor** textovému poli, zadejte hodnotu pomocí jedné z následujících vzoru:</span><span class="sxs-lookup"><span data-stu-id="20587-158">In the **Identifier** textbox, type the value using one of the following pattern:</span></span> 
    
    `https://<customer domain>.landgorilla.com/` 
    
    `https://www.<customer domain>.landgorilla.com`

    <span data-ttu-id="20587-159">b.</span><span class="sxs-lookup"><span data-stu-id="20587-159">b.</span></span> <span data-ttu-id="20587-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí jedné z následujících vzoru:</span><span class="sxs-lookup"><span data-stu-id="20587-160">In the **Reply URL** textbox, type a URL using one of the following pattern:</span></span>

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`
    
    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`

    > [!NOTE] 
    > <span data-ttu-id="20587-161">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="20587-161">Please note that these are not the real values.</span></span> <span data-ttu-id="20587-162">Budete muset aktualizovat tyto hodnoty se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="20587-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="20587-163">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="20587-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="20587-164">Obraťte se na [tým krajině Gorilla klientů](https://www.landgorilla.com/support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="20587-164">Contact [Land Gorilla Client team](https://www.landgorilla.com/support/) to get these values.</span></span> 

4. <span data-ttu-id="20587-165">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="20587-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_certificate.png) 

5. <span data-ttu-id="20587-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20587-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-landgorilla-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="20587-169">Chcete-li získat dokončení konfigurace jednotného přihlašování pro vaši aplikaci na konci krajině Gorilla, obraťte se na [tým podpory krajině Gorilla klienta](https://www.landgorilla.com/support/) a jim poskytnout stažený **"soubor XML s metadaty** souboru.</span><span class="sxs-lookup"><span data-stu-id="20587-169">To get SSO configuration complete for your application at Land Gorilla end, Contact [Land Gorilla Client support team](https://www.landgorilla.com/support/) and provide them with the downloaded **“Metadata XML** file.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="20587-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="20587-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="20587-171">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="20587-171">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="20587-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="20587-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="20587-174">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="20587-174">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="20587-176">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="20587-176">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="20587-178">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20587-178">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="20587-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20587-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="20587-182">a.</span><span class="sxs-lookup"><span data-stu-id="20587-182">a.</span></span> <span data-ttu-id="20587-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="20587-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="20587-184">b.</span><span class="sxs-lookup"><span data-stu-id="20587-184">b.</span></span> <span data-ttu-id="20587-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="20587-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="20587-186">c.</span><span class="sxs-lookup"><span data-stu-id="20587-186">c.</span></span> <span data-ttu-id="20587-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="20587-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="20587-188">d.</span><span class="sxs-lookup"><span data-stu-id="20587-188">d.</span></span> <span data-ttu-id="20587-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="20587-189">Click **Create**.</span></span> 

### <a name="creating-a-land-gorilla-test-user"></a><span data-ttu-id="20587-190">Vytvoření zkušebního uživatele krajině Gorilla</span><span class="sxs-lookup"><span data-stu-id="20587-190">Creating a Land Gorilla test user</span></span>

<span data-ttu-id="20587-191">Spojte se s [tým podpory krajině Gorilla](https://www.landgorilla.com/support/) přidat uživatele do krajině Gorilla platformy.</span><span class="sxs-lookup"><span data-stu-id="20587-191">Please work with [Land Gorilla support team](https://www.landgorilla.com/support/) to add the users in the Land Gorilla platform.</span></span>
    
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="20587-192">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="20587-192">Assigning the Azure AD test user</span></span>

<span data-ttu-id="20587-193">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup na pevnou Gorilla klienta.</span><span class="sxs-lookup"><span data-stu-id="20587-193">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Land Gorilla Client.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="20587-195">**Pokud chcete přiřadit Britta Simon krajině Gorilla klienta, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="20587-195">**To assign Britta Simon to Land Gorilla Client, perform the following steps:**</span></span>

1. <span data-ttu-id="20587-196">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="20587-196">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="20587-198">V seznamu aplikací vyberte **krajině Gorilla klienta**.</span><span class="sxs-lookup"><span data-stu-id="20587-198">In the applications list, select **Land Gorilla Client**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_app.png) 

3. <span data-ttu-id="20587-200">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="20587-200">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="20587-202">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20587-202">Click **Add** button.</span></span> <span data-ttu-id="20587-203">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20587-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="20587-205">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="20587-205">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="20587-206">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20587-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="20587-207">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20587-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="20587-208">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="20587-208">Testing single sign-on</span></span>

<span data-ttu-id="20587-209">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="20587-209">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="20587-210">Když kliknete na dlaždici krajině Gorilla klienta na přístupovém panelu, jste měli získat automaticky přihlášení k krajině Gorilla klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="20587-210">When you click the Land Gorilla Client tile in the Access Panel, you should get automatically signed-on to your Land Gorilla Client application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="20587-211">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="20587-211">Additional resources</span></span>

* [<span data-ttu-id="20587-212">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="20587-212">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="20587-213">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="20587-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_203.png
