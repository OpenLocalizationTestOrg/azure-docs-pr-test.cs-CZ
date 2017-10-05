---
title: 'Kurz: Azure Active Directory integrace s Concur | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Concur."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1eee0a5d-24fa-4986-9aef-3c543cfe3296
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 0b44437b3dcf69dae3587529da7d12e7809b9f55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-concur"></a><span data-ttu-id="d47ef-103">Kurz: Azure Active Directory integrace s Concur</span><span class="sxs-lookup"><span data-stu-id="d47ef-103">Tutorial: Azure Active Directory integration with Concur</span></span>

<span data-ttu-id="d47ef-104">V tomto kurzu zjistěte, jak integrovat Concur s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d47ef-104">In this tutorial, you learn how to integrate Concur with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d47ef-105">Integrace Concur s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d47ef-105">Integrating Concur with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d47ef-106">Můžete řídit ve službě Azure AD, který má přístup k Concur</span><span class="sxs-lookup"><span data-stu-id="d47ef-106">You can control in Azure AD who has access to Concur</span></span>
- <span data-ttu-id="d47ef-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Concur (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d47ef-107">You can enable your users to automatically get signed-on to Concur (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d47ef-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d47ef-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d47ef-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d47ef-109">If you want to know more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d47ef-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d47ef-110">Prerequisites</span></span>

<span data-ttu-id="d47ef-111">Konfigurace integrace Azure AD s Concur, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d47ef-111">To configure Azure AD integration with Concur, you need the following items:</span></span>

- <span data-ttu-id="d47ef-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d47ef-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d47ef-113">Concur jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d47ef-113">A Concur single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d47ef-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d47ef-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d47ef-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d47ef-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d47ef-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d47ef-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d47ef-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d47ef-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d47ef-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d47ef-118">Scenario description</span></span>
<span data-ttu-id="d47ef-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d47ef-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d47ef-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d47ef-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d47ef-121">Přidání Concur z Galerie</span><span class="sxs-lookup"><span data-stu-id="d47ef-121">Adding Concur from the gallery</span></span>
2. <span data-ttu-id="d47ef-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d47ef-122">Configuring and testing Azure AD single sign-on</span></span>

>[!NOTE]
><span data-ttu-id="d47ef-123">Konfigurace odběru Concur pro federované jednotné přihlašování pomocí SAML je samostatná úloha, která bude nutné se obrátit [tým podpory vyústit klienta](https://www.concur.co.in/contact) k provedení.</span><span class="sxs-lookup"><span data-stu-id="d47ef-123">The configuration of your Concur subscription for federated SSO via SAML is a separate task, which you must contact [Concur Client support team](https://www.concur.co.in/contact) to perform.</span></span> 

## <a name="adding-concur-from-the-gallery"></a><span data-ttu-id="d47ef-124">Přidání Concur z Galerie</span><span class="sxs-lookup"><span data-stu-id="d47ef-124">Adding Concur from the gallery</span></span>
<span data-ttu-id="d47ef-125">Při konfiguraci integrace Concur do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Concur z galerie.</span><span class="sxs-lookup"><span data-stu-id="d47ef-125">To configure the integration of Concur into Azure AD, you need to add Concur from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d47ef-126">**Pokud chcete přidat Concur z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d47ef-126">**To add Concur from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d47ef-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d47ef-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d47ef-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d47ef-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-130">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d47ef-132">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d47ef-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d47ef-134">Do vyhledávacího pole zadejte **Concur**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-134">In the search box, type **Concur**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/tutorial_concur_search.png)

5. <span data-ttu-id="d47ef-136">Na panelu výsledků vyberte **Concur**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d47ef-136">In the results panel, select **Concur**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/tutorial_concur_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d47ef-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d47ef-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d47ef-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Concur podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="d47ef-139">In this section, you configure and test Azure AD single sign-on with Concur based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d47ef-140">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Concur je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d47ef-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Concur is to a user in Azure AD.</span></span> <span data-ttu-id="d47ef-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Concur musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d47ef-141">In other words, a link relationship between an Azure AD user and the related user in Concur needs to be established.</span></span>

<span data-ttu-id="d47ef-142">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Concur.</span><span class="sxs-lookup"><span data-stu-id="d47ef-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Concur.</span></span>

<span data-ttu-id="d47ef-143">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Concur, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d47ef-143">To configure and test Azure AD single sign-on with Concur, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d47ef-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d47ef-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d47ef-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d47ef-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d47ef-146">**[Vytvoření zkušebního uživatele Concur](#creating-a-concur-test-user)**  – Pokud chcete mít protějšek Britta Simon v Concur propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d47ef-146">**[Creating a Concur test user](#creating-a-concur-test-user)** - to have a counterpart of Britta Simon in Concur that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d47ef-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d47ef-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d47ef-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d47ef-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d47ef-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d47ef-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d47ef-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Concur.</span><span class="sxs-lookup"><span data-stu-id="d47ef-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Concur application.</span></span>

<span data-ttu-id="d47ef-151">**Ke konfiguraci Azure AD jednotné přihlašování s Concur, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d47ef-151">**To configure Azure AD single sign-on with Concur, perform the following steps:**</span></span>

1. <span data-ttu-id="d47ef-152">Na portálu Azure na **Concur** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-152">In the Azure portal, on the **Concur** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d47ef-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d47ef-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_concur_samlbase.png)

3. <span data-ttu-id="d47ef-156">Na **vyústit domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d47ef-156">On the **Concur Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_concur_url.png)

    <span data-ttu-id="d47ef-158">a.</span><span class="sxs-lookup"><span data-stu-id="d47ef-158">a.</span></span> <span data-ttu-id="d47ef-159">V **přihlásit na adrese URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://www.concursolutions.com/UI/SSO/<OrganizationId>`</span><span class="sxs-lookup"><span data-stu-id="d47ef-159">In the **Sign on URL** textbox, type the value using the following pattern: `https://www.concursolutions.com/UI/SSO/<OrganizationId>`</span></span>

    <span data-ttu-id="d47ef-160">b.</span><span class="sxs-lookup"><span data-stu-id="d47ef-160">b.</span></span> <span data-ttu-id="d47ef-161">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<customer-domain>.concursolutions.com`</span><span class="sxs-lookup"><span data-stu-id="d47ef-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<customer-domain>.concursolutions.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d47ef-162">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="d47ef-162">These values are not the real.</span></span> <span data-ttu-id="d47ef-163">Tyto hodnoty aktualizujte skutečné znakem na adresu URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="d47ef-163">Update these values with the actual Sign on URL and Identifier.</span></span> <span data-ttu-id="d47ef-164">Obraťte se na [tým podpory vyústit klienta](https://www.concur.co.in/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="d47ef-164">Contact [Concur Client support team](https://www.concur.co.in/contact) to get these values.</span></span> 

4. <span data-ttu-id="d47ef-165">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d47ef-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_concur_certificate.png) 

5. <span data-ttu-id="d47ef-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d47ef-167">Click **Save** button.</span></span>

    <span data-ttu-id="d47ef-168">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="d47ef-168">![Configure Single Sign-On](./media/active-directory-saas-concur-tutorial/tutorial_general_400.png)
<CS></span></span>

6. <span data-ttu-id="d47ef-169">Konfigurace jednotného přihlašování na **Concur** straně, budete muset odeslat stažené **soubor XML s metadaty** Concur podpory.</span><span class="sxs-lookup"><span data-stu-id="d47ef-169">To configure single sign-on on **Concur** side, you need to send the downloaded **Metadata XML** to Concur support.</span></span> <span data-ttu-id="d47ef-170">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="d47ef-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

  >[!NOTE]
  ><span data-ttu-id="d47ef-171">Konfigurace odběru Concur pro federované jednotné přihlašování pomocí SAML je samostatná úloha, která bude nutné se obrátit [tým podpory vyústit klienta](https://www.concur.co.in/contact) k provedení.</span><span class="sxs-lookup"><span data-stu-id="d47ef-171">The configuration of your Concur subscription for federated SSO via SAML is a separate task, which you must contact [Concur Client support team](https://www.concur.co.in/contact) to perform.</span></span> 
  
<CE>

> [!TIP]
> <span data-ttu-id="d47ef-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d47ef-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d47ef-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d47ef-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d47ef-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d47ef-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d47ef-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d47ef-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="d47ef-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d47ef-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d47ef-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d47ef-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d47ef-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d47ef-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d47ef-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d47ef-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d47ef-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d47ef-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d47ef-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d47ef-187">a.</span><span class="sxs-lookup"><span data-stu-id="d47ef-187">a.</span></span> <span data-ttu-id="d47ef-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d47ef-189">b.</span><span class="sxs-lookup"><span data-stu-id="d47ef-189">b.</span></span> <span data-ttu-id="d47ef-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d47ef-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d47ef-191">c.</span><span class="sxs-lookup"><span data-stu-id="d47ef-191">c.</span></span> <span data-ttu-id="d47ef-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d47ef-193">d.</span><span class="sxs-lookup"><span data-stu-id="d47ef-193">d.</span></span> <span data-ttu-id="d47ef-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-194">Click **Create**.</span></span>
 
### <a name="creating-a-concur-test-user"></a><span data-ttu-id="d47ef-195">Vytvoření zkušebního uživatele Concur</span><span class="sxs-lookup"><span data-stu-id="d47ef-195">Creating a Concur test user</span></span>

<span data-ttu-id="d47ef-196">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatelé jsou automaticky vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d47ef-196">Application supports the Just in time user provisioning and after authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d47ef-197">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d47ef-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d47ef-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Concur.</span><span class="sxs-lookup"><span data-stu-id="d47ef-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Concur.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d47ef-200">**Pokud chcete přiřadit Britta Simon Concur, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d47ef-200">**To assign Britta Simon to Concur, perform the following steps:**</span></span>

1. <span data-ttu-id="d47ef-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d47ef-203">V seznamu aplikací vyberte **Concur**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-203">In the applications list, select **Concur**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_concur_app.png) 

3. <span data-ttu-id="d47ef-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d47ef-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d47ef-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d47ef-207">Click **Add** button.</span></span> <span data-ttu-id="d47ef-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d47ef-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d47ef-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d47ef-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d47ef-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d47ef-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d47ef-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d47ef-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d47ef-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d47ef-213">Testing single sign-on</span></span>

<span data-ttu-id="d47ef-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d47ef-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d47ef-215">Když kliknete na dlaždici Concur na přístupovém panelu, měli byste obdržet přihlašovací stránku Concur aplikace.</span><span class="sxs-lookup"><span data-stu-id="d47ef-215">When you click the Concur tile in the Access Panel, you should get login page of Concur application.</span></span>
<span data-ttu-id="d47ef-216">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d47ef-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d47ef-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d47ef-217">Additional resources</span></span>

* [<span data-ttu-id="d47ef-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d47ef-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d47ef-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d47ef-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="d47ef-220">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="d47ef-220">Configure User Provisioning</span></span>](active-directory-saas-concur-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-concur-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-concur-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-concur-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-concur-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-concur-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-concur-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-concur-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-concur-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-concur-tutorial/tutorial_general_203.png

