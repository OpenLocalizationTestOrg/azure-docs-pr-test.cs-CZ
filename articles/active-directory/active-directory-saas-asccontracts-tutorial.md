---
title: 'Kurz: Azure Active Directory integrace se smlouvami ASC | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování v Azure Active Directory a jiné smlouvy ASC."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7f54202-1581-4e55-a97e-02633ff9382d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: jeedes
ms.openlocfilehash: 87ea3cc55f9683e7d5b9912a87d675575cea0347
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asc-contracts"></a><span data-ttu-id="23692-103">Kurz: Azure Active Directory integrace se smlouvami ASC</span><span class="sxs-lookup"><span data-stu-id="23692-103">Tutorial: Azure Active Directory integration with ASC Contracts</span></span>

<span data-ttu-id="23692-104">V tomto kurzu zjistěte, jak integrovat ASC kontrakty s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="23692-104">In this tutorial, you learn how to integrate ASC Contracts with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="23692-105">Integrace ASC kontrakty s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="23692-105">Integrating ASC Contracts with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="23692-106">Můžete řídit ve službě Azure AD, který má přístup k kontrakty ASC</span><span class="sxs-lookup"><span data-stu-id="23692-106">You can control in Azure AD who has access to ASC Contracts</span></span>
- <span data-ttu-id="23692-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k kontrakty ASC (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="23692-107">You can enable your users to automatically get signed-on to ASC Contracts (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="23692-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="23692-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="23692-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="23692-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="23692-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="23692-110">Prerequisites</span></span>

<span data-ttu-id="23692-111">Konfigurace integrace Azure AD s ASC kontrakty, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="23692-111">To configure Azure AD integration with ASC Contracts, you need the following items:</span></span>

- <span data-ttu-id="23692-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="23692-112">An Azure AD subscription</span></span>
- <span data-ttu-id="23692-113">ASC kontrakty jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="23692-113">An ASC Contracts single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="23692-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="23692-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="23692-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="23692-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="23692-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="23692-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="23692-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="23692-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="23692-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="23692-118">Scenario description</span></span>
<span data-ttu-id="23692-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="23692-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="23692-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="23692-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="23692-121">Přidání smluv ASC z Galerie</span><span class="sxs-lookup"><span data-stu-id="23692-121">Adding ASC Contracts from the gallery</span></span>
2. <span data-ttu-id="23692-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="23692-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asc-contracts-from-the-gallery"></a><span data-ttu-id="23692-123">Přidání smluv ASC z Galerie</span><span class="sxs-lookup"><span data-stu-id="23692-123">Adding ASC Contracts from the gallery</span></span>
<span data-ttu-id="23692-124">Při konfiguraci integrace ASC smluv do služby Azure AD, potřebujete přidat ASC kontrakty z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="23692-124">To configure the integration of ASC Contracts into Azure AD, you need to add ASC Contracts from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="23692-125">**Pokud chcete přidat ASC kontrakty z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="23692-125">**To add ASC Contracts from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="23692-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="23692-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="23692-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="23692-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="23692-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="23692-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="23692-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23692-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="23692-133">Do vyhledávacího pole zadejte **ASC kontrakty**.</span><span class="sxs-lookup"><span data-stu-id="23692-133">In the search box, type **ASC Contracts**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_search.png)

5. <span data-ttu-id="23692-135">Na panelu výsledků vyberte **ASC kontrakty**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23692-135">In the results panel, select **ASC Contracts**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="23692-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="23692-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="23692-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s kontrakty ASC podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="23692-138">In this section, you configure and test Azure AD single sign-on with ASC Contracts based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="23692-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v kontraktech ASC je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="23692-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ASC Contracts is to a user in Azure AD.</span></span> <span data-ttu-id="23692-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v kontraktech ASC musí navázat.</span><span class="sxs-lookup"><span data-stu-id="23692-140">In other words, a link relationship between an Azure AD user and the related user in ASC Contracts needs to be established.</span></span>

<span data-ttu-id="23692-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v kontraktech ASC.</span><span class="sxs-lookup"><span data-stu-id="23692-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ASC Contracts.</span></span>

<span data-ttu-id="23692-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ASC kontrakty, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="23692-142">To configure and test Azure AD single sign-on with ASC Contracts, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="23692-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="23692-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="23692-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="23692-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="23692-145">**[Vytváření testovacího uživatele ASC kontrakty](#creating-an-asc-contracts-test-user)**  – Pokud chcete mít protějšek Britta Simon v kontraktech ASC, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="23692-145">**[Creating an ASC Contracts test user](#creating-an-asc-contracts-test-user)** - to have a counterpart of Britta Simon in ASC Contracts that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="23692-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23692-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="23692-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="23692-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="23692-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23692-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="23692-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci kontrakty ASC.</span><span class="sxs-lookup"><span data-stu-id="23692-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ASC Contracts application.</span></span>

<span data-ttu-id="23692-150">**Ke konfiguraci Azure AD jednotné přihlašování s ASC kontrakty, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="23692-150">**To configure Azure AD single sign-on with ASC Contracts, perform the following steps:**</span></span>

1. <span data-ttu-id="23692-151">Na portálu Azure na **ASC kontrakty** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="23692-151">In the Azure portal, on the **ASC Contracts** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="23692-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23692-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_samlbase.png)

3. <span data-ttu-id="23692-155">Na **ASC kontrakty domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="23692-155">On the **ASC Contracts Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_url.png)

    <span data-ttu-id="23692-157">a.</span><span class="sxs-lookup"><span data-stu-id="23692-157">a.</span></span> <span data-ttu-id="23692-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.asccontracts.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="23692-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.asccontracts.com/shibboleth`</span></span>

    <span data-ttu-id="23692-159">b.</span><span class="sxs-lookup"><span data-stu-id="23692-159">b.</span></span> <span data-ttu-id="23692-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span><span class="sxs-lookup"><span data-stu-id="23692-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="23692-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="23692-161">These values are not real.</span></span> <span data-ttu-id="23692-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="23692-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="23692-163">Obraťte se na tým ASC sítě Inc. (ASC) na **613.599.6178** k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="23692-163">Contact ASC Networks Inc. (ASC) team at **613.599.6178** to get these values.</span></span>

4. <span data-ttu-id="23692-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="23692-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_certificate.png) 

5. <span data-ttu-id="23692-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="23692-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="23692-168">Konfigurace jednotného přihlašování na **ASC kontrakty** straně, obraťte se na podporu ASC sítě Inc. (ASC) na **613.599.6178** a jim poskytnout stažené **soubor XML s metadaty**.</span><span class="sxs-lookup"><span data-stu-id="23692-168">To configure single sign-on on **ASC Contracts** side, call ASC Networks Inc. (ASC) support at **613.599.6178** and provide them with the downloaded **Metadata XML**.</span></span> <span data-ttu-id="23692-169">Nastavené tuto aplikaci tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="23692-169">They set this application up to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="23692-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="23692-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="23692-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="23692-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="23692-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="23692-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="23692-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="23692-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="23692-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="23692-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="23692-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="23692-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="23692-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="23692-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="23692-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="23692-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="23692-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23692-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="23692-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="23692-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="23692-185">a.</span><span class="sxs-lookup"><span data-stu-id="23692-185">a.</span></span> <span data-ttu-id="23692-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="23692-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="23692-187">b.</span><span class="sxs-lookup"><span data-stu-id="23692-187">b.</span></span> <span data-ttu-id="23692-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="23692-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="23692-189">c.</span><span class="sxs-lookup"><span data-stu-id="23692-189">c.</span></span> <span data-ttu-id="23692-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="23692-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="23692-191">d.</span><span class="sxs-lookup"><span data-stu-id="23692-191">d.</span></span> <span data-ttu-id="23692-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="23692-192">Click **Create**.</span></span>
 
### <a name="creating-an-asc-contracts-test-user"></a><span data-ttu-id="23692-193">Vytváření testovacího uživatele kontrakty ASC</span><span class="sxs-lookup"><span data-stu-id="23692-193">Creating an ASC Contracts test user</span></span>

<span data-ttu-id="23692-194">Práce s tým podpory ASC sítě Inc. (ASC) na **613.599.6178** získat přidané v platformě ASC kontrakty uživatele.</span><span class="sxs-lookup"><span data-stu-id="23692-194">Work with ASC Networks Inc. (ASC) support team at **613.599.6178** to get the users added in the ASC Contracts platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="23692-195">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="23692-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="23692-196">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu kontrakty ASC.</span><span class="sxs-lookup"><span data-stu-id="23692-196">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ASC Contracts.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="23692-198">**Pokud chcete přiřadit Britta Simon ASC kontrakty, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="23692-198">**To assign Britta Simon to ASC Contracts, perform the following steps:**</span></span>

1. <span data-ttu-id="23692-199">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="23692-199">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="23692-201">V seznamu aplikací vyberte **ASC kontrakty**.</span><span class="sxs-lookup"><span data-stu-id="23692-201">In the applications list, select **ASC Contracts**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_app.png) 

3. <span data-ttu-id="23692-203">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="23692-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="23692-205">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="23692-205">Click **Add** button.</span></span> <span data-ttu-id="23692-206">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23692-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="23692-208">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="23692-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="23692-209">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23692-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="23692-210">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23692-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="23692-211">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23692-211">Testing single sign-on</span></span>

<span data-ttu-id="23692-212">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="23692-212">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="23692-213">Když kliknete na dlaždici ASC kontrakty na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci kontrakty ASC.</span><span class="sxs-lookup"><span data-stu-id="23692-213">When you click the ASC Contracts tile in the Access Panel, you should get automatically signed-on to your ASC Contracts application.</span></span> <span data-ttu-id="23692-214">Další informace o na přístupovém panelu najdete v tématu.</span><span class="sxs-lookup"><span data-stu-id="23692-214">For more information about the Access Panel, see.</span></span> <span data-ttu-id="23692-215">[Úvod do přístupového panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="23692-215">[Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="23692-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="23692-216">Additional resources</span></span>

* [<span data-ttu-id="23692-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="23692-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="23692-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="23692-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_203.png

