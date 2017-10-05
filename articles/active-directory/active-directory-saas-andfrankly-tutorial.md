---
title: 'Kurz: Azure Active Directory integrace s & frankly | Microsoft Docs'
description: "Naučte se konfigurovat jednotné přihlašování mezi Azure Active Directory a & frankly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d702060-1b89-4e9d-9f01-ede4f1171c73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: ea18a9f9bff258337a3de6d7703b4c548efa37df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-frankly"></a><span data-ttu-id="11823-103">Kurz: Azure Active Directory integrace s & frankly</span><span class="sxs-lookup"><span data-stu-id="11823-103">Tutorial: Azure Active Directory integration with &frankly</span></span>

<span data-ttu-id="11823-104">V tomto kurzu zjistíte, jak integrovat & frankly s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="11823-104">In this tutorial, you learn how to integrate &frankly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="11823-105">Integrace & frankly s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="11823-105">Integrating &frankly with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="11823-106">Můžete řídit ve službě Azure AD, který má přístup k & frankly</span><span class="sxs-lookup"><span data-stu-id="11823-106">You can control in Azure AD who has access to &frankly</span></span>
- <span data-ttu-id="11823-107">Můžete povolit uživatelům automaticky získat přihlášeného k & frankly (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="11823-107">You can enable your users to automatically get signed-on to &frankly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="11823-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="11823-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="11823-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="11823-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11823-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="11823-110">Prerequisites</span></span>

<span data-ttu-id="11823-111">Ke konfiguraci Azure AD integrace s & frankly, budete potřebovat následující položky:</span><span class="sxs-lookup"><span data-stu-id="11823-111">To configure Azure AD integration with &frankly, you need the following items:</span></span>

- <span data-ttu-id="11823-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="11823-112">An Azure AD subscription</span></span>
- <span data-ttu-id="11823-113">A & frankly jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="11823-113">A &frankly single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="11823-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="11823-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="11823-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="11823-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="11823-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="11823-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="11823-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="11823-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="11823-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="11823-118">Scenario description</span></span>
<span data-ttu-id="11823-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="11823-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="11823-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="11823-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="11823-121">Přidání & frankly z Galerie</span><span class="sxs-lookup"><span data-stu-id="11823-121">Adding &frankly from the gallery</span></span>
2. <span data-ttu-id="11823-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="11823-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-frankly-from-the-gallery"></a><span data-ttu-id="11823-123">Přidání & frankly z Galerie</span><span class="sxs-lookup"><span data-stu-id="11823-123">Adding &frankly from the gallery</span></span>
<span data-ttu-id="11823-124">Při konfiguraci integrace služby & frankly do služby Azure AD, je nutné přidat & frankly z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="11823-124">To configure the integration of &frankly into Azure AD, you need to add &frankly from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="11823-125">**Chcete-li přidat & frankly z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="11823-125">**To add &frankly from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="11823-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="11823-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="11823-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="11823-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="11823-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="11823-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="11823-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="11823-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="11823-133">Do vyhledávacího pole zadejte **& frankly**.</span><span class="sxs-lookup"><span data-stu-id="11823-133">In the search box, type **&frankly**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_search.png)

5. <span data-ttu-id="11823-135">Na panelu výsledků vyberte **& frankly**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="11823-135">In the results panel, select **&frankly**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="11823-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="11823-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="11823-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s & frankly podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="11823-138">In this section, you configure and test Azure AD single sign-on with &frankly based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="11823-139">Azure AD pro jednotné přihlašování pro práci, je potřeba vědět, jaké příslušného uživatele v & frankly je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11823-139">For single sign-on to work, Azure AD needs to know what the counterpart user in &frankly is to a user in Azure AD.</span></span> <span data-ttu-id="11823-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v & frankly musí být vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="11823-140">In other words, a link relationship between an Azure AD user and the related user in &frankly needs to be established.</span></span>

<span data-ttu-id="11823-141">V & frankly, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="11823-141">In &frankly, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="11823-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s & frankly, které potřebujete k dokončení následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="11823-142">To configure and test Azure AD single sign-on with &frankly, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="11823-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="11823-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="11823-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="11823-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="11823-145">**[Vytváření & frankly testovacího uživatele](#creating-a-frankly-test-user)**  – Pokud chcete mít protějšek Britta Simon v & frankly který je spojen s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="11823-145">**[Creating a &frankly test user](#creating-a-frankly-test-user)** - to have a counterpart of Britta Simon in &frankly that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="11823-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="11823-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="11823-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="11823-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="11823-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="11823-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="11823-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší & frankly aplikací.</span><span class="sxs-lookup"><span data-stu-id="11823-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your &frankly application.</span></span>

<span data-ttu-id="11823-150">**Ke konfiguraci Azure AD jedním přihlašování pomocí & frankly, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="11823-150">**To configure Azure AD single sign-on with &frankly, perform the following steps:**</span></span>

1. <span data-ttu-id="11823-151">Na portálu Azure na **& frankly** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="11823-151">In the Azure portal, on the **&frankly** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="11823-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="11823-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_samlbase.png)

3. <span data-ttu-id="11823-155">Na **& frankly domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="11823-155">On the **&frankly Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_url.png)

    <span data-ttu-id="11823-157">a.</span><span class="sxs-lookup"><span data-stu-id="11823-157">a.</span></span> <span data-ttu-id="11823-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="11823-158">In the **Identifier** textbox, type a URL using the following pattern: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`</span></span>

    <span data-ttu-id="11823-159">b.</span><span class="sxs-lookup"><span data-stu-id="11823-159">b.</span></span> <span data-ttu-id="11823-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="11823-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`</span></span>

4. <span data-ttu-id="11823-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="11823-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="11823-162">Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="11823-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_url1.png)

    <span data-ttu-id="11823-164">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="11823-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`</span></span>
    > [!NOTE] 
    > <span data-ttu-id="11823-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="11823-165">These values are not real.</span></span> <span data-ttu-id="11823-166">Tyto hodnoty aktualizovat skutečným identifikátorem přihlášení a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="11823-166">Update these values with the actual Identifier, Sign-on, and Reply URL.</span></span> <span data-ttu-id="11823-167">Obraťte se na [tým podpory andfrankly](mailto:help@andfrankly.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="11823-167">Contact [andfrankly support team](mailto:help@andfrankly.com) to get these values.</span></span>

5. <span data-ttu-id="11823-168">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="11823-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_certificate.png) 

6. <span data-ttu-id="11823-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="11823-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-andfrankly-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="11823-172">Konfigurace jednotného přihlašování na **& frankly** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory andfrankly](mailto:help@andfrankly.com).</span><span class="sxs-lookup"><span data-stu-id="11823-172">To configure single sign-on on **&frankly** side, you need to send the downloaded **Metadata XML** to [andfrankly support team](mailto:help@andfrankly.com).</span></span> 

> [!TIP]
> <span data-ttu-id="11823-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="11823-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="11823-174">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="11823-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="11823-175">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="11823-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="11823-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="11823-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="11823-177">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="11823-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="11823-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="11823-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="11823-180">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="11823-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="11823-182">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="11823-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="11823-184">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="11823-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="11823-186">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="11823-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="11823-188">a.</span><span class="sxs-lookup"><span data-stu-id="11823-188">a.</span></span> <span data-ttu-id="11823-189">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="11823-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="11823-190">b.</span><span class="sxs-lookup"><span data-stu-id="11823-190">b.</span></span> <span data-ttu-id="11823-191">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="11823-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="11823-192">c.</span><span class="sxs-lookup"><span data-stu-id="11823-192">c.</span></span> <span data-ttu-id="11823-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="11823-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="11823-194">d.</span><span class="sxs-lookup"><span data-stu-id="11823-194">d.</span></span> <span data-ttu-id="11823-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="11823-195">Click **Create**.</span></span>
 
### <a name="creating-a-frankly-test-user"></a><span data-ttu-id="11823-196">Vytváření & frankly testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="11823-196">Creating a &frankly test user</span></span>

<span data-ttu-id="11823-197">V této části vytvoříte názvem Britta Simon v & frankly uživatele.</span><span class="sxs-lookup"><span data-stu-id="11823-197">In this section, you create a user called Britta Simon in &frankly.</span></span> <span data-ttu-id="11823-198">Práce s [tým podpory andfrankly](mailto:help@andfrankly.com) přidat uživatele v vý & frankly platformy.</span><span class="sxs-lookup"><span data-stu-id="11823-198">Work with  [andfrankly support team](mailto:help@andfrankly.com) to add the users in the &frankly platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="11823-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="11823-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="11823-200">V této části povolíte Britta Simon chcete udělit přístup k & frankly použít Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="11823-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to &frankly.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="11823-202">**Přiřadit Britta Simon do & frankly, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="11823-202">**To assign Britta Simon to &frankly, perform the following steps:**</span></span>

1. <span data-ttu-id="11823-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="11823-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="11823-205">V seznamu aplikací vyberte **& frankly**.</span><span class="sxs-lookup"><span data-stu-id="11823-205">In the applications list, select **&frankly**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_app.png) 

3. <span data-ttu-id="11823-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="11823-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="11823-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="11823-209">Click **Add** button.</span></span> <span data-ttu-id="11823-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="11823-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="11823-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="11823-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="11823-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="11823-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="11823-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="11823-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="11823-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="11823-215">Testing single sign-on</span></span>

<span data-ttu-id="11823-216">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="11823-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="11823-217">Po kliknutí & frankly dlaždici na přístupovém panelu, měli byste obdržet automaticky přihlášení k vaší & frankly aplikací</span><span class="sxs-lookup"><span data-stu-id="11823-217">When you click the &frankly tile in the Access Panel, you should get automatically signed-on to your &frankly application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11823-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="11823-218">Additional resources</span></span>

* [<span data-ttu-id="11823-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="11823-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="11823-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="11823-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png
