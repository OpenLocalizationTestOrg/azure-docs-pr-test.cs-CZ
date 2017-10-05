---
title: 'Kurz: Azure Active Directory integrace s Jive | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Jive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9fc5659a-c116-4a1b-a601-333325a26b46
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 6d2d4b777d7afd74598d2eba4a7e3571a8a18d6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jive"></a><span data-ttu-id="bbf89-103">Kurz: Azure Active Directory integrace s Jive</span><span class="sxs-lookup"><span data-stu-id="bbf89-103">Tutorial: Azure Active Directory integration with Jive</span></span>

<span data-ttu-id="bbf89-104">V tomto kurzu zjistěte, jak integrovat Jive s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bbf89-104">In this tutorial, you learn how to integrate Jive with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bbf89-105">Integrace Jive s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="bbf89-105">Integrating Jive with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bbf89-106">Můžete řídit ve službě Azure AD, který má přístup k Jive</span><span class="sxs-lookup"><span data-stu-id="bbf89-106">You can control in Azure AD who has access to Jive</span></span>
- <span data-ttu-id="bbf89-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Jive (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="bbf89-107">You can enable your users to automatically get signed-on to Jive (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bbf89-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bbf89-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bbf89-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bbf89-109">If you want to know more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bbf89-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bbf89-110">Prerequisites</span></span>

<span data-ttu-id="bbf89-111">Konfigurace integrace Azure AD s Jive, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="bbf89-111">To configure Azure AD integration with Jive, you need the following items:</span></span>

- <span data-ttu-id="bbf89-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bbf89-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bbf89-113">Jive jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="bbf89-113">A Jive single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bbf89-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bbf89-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bbf89-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="bbf89-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bbf89-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="bbf89-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bbf89-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bbf89-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bbf89-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="bbf89-118">Scenario description</span></span>
<span data-ttu-id="bbf89-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bbf89-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bbf89-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="bbf89-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bbf89-121">Přidání Jive z Galerie</span><span class="sxs-lookup"><span data-stu-id="bbf89-121">Adding Jive from the gallery</span></span>
2. <span data-ttu-id="bbf89-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bbf89-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jive-from-the-gallery"></a><span data-ttu-id="bbf89-123">Přidání Jive z Galerie</span><span class="sxs-lookup"><span data-stu-id="bbf89-123">Adding Jive from the gallery</span></span>
<span data-ttu-id="bbf89-124">Chcete-li nakonfigurovat integraci Jive do služby Azure AD, přidejte Jive z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="bbf89-124">To configure the integration of Jive into Azure AD, you need to add Jive from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bbf89-125">**Pokud chcete přidat Jive z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bbf89-125">**To add Jive from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bbf89-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bbf89-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bbf89-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bbf89-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="bbf89-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bbf89-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="bbf89-133">Do vyhledávacího pole zadejte **Jive**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-133">In the search box, type **Jive**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_search.png)

5. <span data-ttu-id="bbf89-135">Na panelu výsledků vyberte **Jive**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bbf89-135">In the results panel, select **Jive**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bbf89-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bbf89-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bbf89-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jive podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="bbf89-138">In this section, you configure and test Azure AD single sign-on with Jive based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bbf89-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Jive je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bbf89-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jive is to a user in Azure AD.</span></span> <span data-ttu-id="bbf89-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Jive musí navázat.</span><span class="sxs-lookup"><span data-stu-id="bbf89-140">In other words, a link relationship between an Azure AD user and the related user in Jive needs to be established.</span></span>

<span data-ttu-id="bbf89-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Jive.</span><span class="sxs-lookup"><span data-stu-id="bbf89-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Jive.</span></span>

<span data-ttu-id="bbf89-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jive, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="bbf89-142">To configure and test Azure AD single sign-on with Jive, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bbf89-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="bbf89-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bbf89-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bbf89-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bbf89-145">**[Vytvoření zkušebního uživatele Jive](#creating-a-jive-test-user)**  – Pokud chcete mít protějšek Britta Simon v Jive propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="bbf89-145">**[Creating a Jive test user](#creating-a-jive-test-user)** - to have a counterpart of Britta Simon in Jive that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bbf89-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bbf89-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bbf89-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bbf89-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bbf89-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bbf89-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bbf89-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Jive.</span><span class="sxs-lookup"><span data-stu-id="bbf89-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jive application.</span></span>

<span data-ttu-id="bbf89-150">**Ke konfiguraci Azure AD jednotné přihlašování s Jive, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bbf89-150">**To configure Azure AD single sign-on with Jive, perform the following steps:**</span></span>

1. <span data-ttu-id="bbf89-151">Na portálu Azure na **Jive** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-151">In the Azure portal, on the **Jive** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="bbf89-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bbf89-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_samlbase.png)

3. <span data-ttu-id="bbf89-155">Na **Jive domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bbf89-155">On the **Jive Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_url.png)

    <span data-ttu-id="bbf89-157">a.</span><span class="sxs-lookup"><span data-stu-id="bbf89-157">a.</span></span> <span data-ttu-id="bbf89-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instance name>.jivecustom.com`</span><span class="sxs-lookup"><span data-stu-id="bbf89-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instance name>.jivecustom.com`</span></span>

    <span data-ttu-id="bbf89-159">b.</span><span class="sxs-lookup"><span data-stu-id="bbf89-159">b.</span></span> <span data-ttu-id="bbf89-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instance name>.jiveon.com`</span><span class="sxs-lookup"><span data-stu-id="bbf89-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instance name>.jiveon.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bbf89-161">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="bbf89-161">These values are not the real.</span></span> <span data-ttu-id="bbf89-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="bbf89-162">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="bbf89-163">Obraťte se na [tým podpory Jive klienta](https://www.jivesoftware.com/services-support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="bbf89-163">Contact [Jive Client support team](https://www.jivesoftware.com/services-support/) to get these values.</span></span> 
 
4. <span data-ttu-id="bbf89-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="bbf89-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_certificate.png) 

5. <span data-ttu-id="bbf89-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bbf89-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bbf89-168">Konfigurace jednotného přihlašování na **Jive** straně přihlašování ke klientovi Jive jako správce.</span><span class="sxs-lookup"><span data-stu-id="bbf89-168">To configure single sign-on on **Jive** side, sign-on to your Jive tenant as an administrator.</span></span>

7. <span data-ttu-id="bbf89-169">V nabídce v horní části, klikněte na tlačítko "**Saml**."</span><span class="sxs-lookup"><span data-stu-id="bbf89-169">In the menu on the top, Click "**Saml**."</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    <span data-ttu-id="bbf89-171">a.</span><span class="sxs-lookup"><span data-stu-id="bbf89-171">a.</span></span> <span data-ttu-id="bbf89-172">Vyberte **povoleno** pod **Obecné** kartě.</span><span class="sxs-lookup"><span data-stu-id="bbf89-172">Select **Enabled** under the **General** tab.</span></span>   
    <span data-ttu-id="bbf89-173">b.</span><span class="sxs-lookup"><span data-stu-id="bbf89-173">b.</span></span> <span data-ttu-id="bbf89-174">Klikněte "**uložit všechna nastavení saml**" tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bbf89-174">Click the "**Save all saml settings**" button.</span></span>

8. <span data-ttu-id="bbf89-175">Přejděte na "**Idp Metadata**" kartě.</span><span class="sxs-lookup"><span data-stu-id="bbf89-175">Navigate to the "**Idp Metadata**" tab.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)
   
    <span data-ttu-id="bbf89-177">a.</span><span class="sxs-lookup"><span data-stu-id="bbf89-177">a.</span></span> <span data-ttu-id="bbf89-178">Kopírovat obsah stažený metadata souboru XML a vložte ji do **zprostředkovatele Identity (IDP) Metadata** textové pole.</span><span class="sxs-lookup"><span data-stu-id="bbf89-178">Copy the content of the downloaded metadata XML file, and then paste it into the **Identity Provider (IDP) Metadata** textbox.</span></span>
    
    <span data-ttu-id="bbf89-179">b.</span><span class="sxs-lookup"><span data-stu-id="bbf89-179">b.</span></span> <span data-ttu-id="bbf89-180">Klikněte "**uložit všechna nastavení saml**" tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bbf89-180">Click the "**Save all saml settings**" button.</span></span> 

9. <span data-ttu-id="bbf89-181">Přejděte na "**mapování atributů uživatele**" kartě.</span><span class="sxs-lookup"><span data-stu-id="bbf89-181">Go to the "**User Attribute Mapping**" tab.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)
   
    <span data-ttu-id="bbf89-183">a.</span><span class="sxs-lookup"><span data-stu-id="bbf89-183">a.</span></span> <span data-ttu-id="bbf89-184">V **e-mailu** textovému poli, zkopírujte a vložte název atributu pro **e-mailu** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bbf89-184">In the **Email** textbox, copy and paste the attribute name of **mail** value.</span></span>
   
    <span data-ttu-id="bbf89-185">b.</span><span class="sxs-lookup"><span data-stu-id="bbf89-185">b.</span></span> <span data-ttu-id="bbf89-186">V **křestní jméno** textovému poli, zkopírujte a vložte název atributu pro **givenname** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bbf89-186">In the **First Name** textbox, copy and paste the attribute name of **givenname** value.</span></span>
   
    <span data-ttu-id="bbf89-187">c.</span><span class="sxs-lookup"><span data-stu-id="bbf89-187">c.</span></span> <span data-ttu-id="bbf89-188">V **příjmení** textovému poli, zkopírujte a vložte název atributu pro **Přezdívka** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bbf89-188">In the **Last Name** textbox, copy and paste the attribute name of **surname** value.</span></span>

> [!TIP]
> <span data-ttu-id="bbf89-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="bbf89-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bbf89-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="bbf89-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bbf89-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bbf89-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bbf89-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bbf89-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="bbf89-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bbf89-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="bbf89-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bbf89-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bbf89-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bbf89-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bbf89-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bbf89-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bbf89-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bbf89-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bbf89-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bbf89-204">a.</span><span class="sxs-lookup"><span data-stu-id="bbf89-204">a.</span></span> <span data-ttu-id="bbf89-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bbf89-206">b.</span><span class="sxs-lookup"><span data-stu-id="bbf89-206">b.</span></span> <span data-ttu-id="bbf89-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bbf89-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bbf89-208">c.</span><span class="sxs-lookup"><span data-stu-id="bbf89-208">c.</span></span> <span data-ttu-id="bbf89-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bbf89-210">d.</span><span class="sxs-lookup"><span data-stu-id="bbf89-210">d.</span></span> <span data-ttu-id="bbf89-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-211">Click **Create**.</span></span>
 
### <a name="creating-a-jive-test-user"></a><span data-ttu-id="bbf89-212">Vytvoření zkušebního uživatele Jive</span><span class="sxs-lookup"><span data-stu-id="bbf89-212">Creating a Jive test user</span></span>

<span data-ttu-id="bbf89-213">Práce s [tým podpory Jive klienta](https://www.jivesoftware.com/services-support/) přidat uživatele do Jive platformy.</span><span class="sxs-lookup"><span data-stu-id="bbf89-213">Work with [Jive Client support team](https://www.jivesoftware.com/services-support/) to add the users in the Jive platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bbf89-214">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bbf89-214">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bbf89-215">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Jive.</span><span class="sxs-lookup"><span data-stu-id="bbf89-215">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jive.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="bbf89-217">**Pokud chcete přiřadit Britta Simon Jive, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bbf89-217">**To assign Britta Simon to Jive, perform the following steps:**</span></span>

1. <span data-ttu-id="bbf89-218">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-218">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="bbf89-220">V seznamu aplikací vyberte **Jive**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-220">In the applications list, select **Jive**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_app.png) 

3. <span data-ttu-id="bbf89-222">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="bbf89-222">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="bbf89-224">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bbf89-224">Click **Add** button.</span></span> <span data-ttu-id="bbf89-225">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bbf89-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="bbf89-227">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bbf89-227">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bbf89-228">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bbf89-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bbf89-229">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bbf89-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bbf89-230">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bbf89-230">Testing single sign-on</span></span>

<span data-ttu-id="bbf89-231">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bbf89-231">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="bbf89-232">Když kliknete na dlaždici Jive na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Jive.</span><span class="sxs-lookup"><span data-stu-id="bbf89-232">When you click the Jive tile in the Access Panel, you should get automatically signed-on to your Jive application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bbf89-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bbf89-233">Additional resources</span></span>

* [<span data-ttu-id="bbf89-234">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bbf89-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bbf89-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bbf89-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="bbf89-236">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="bbf89-236">Configure User Provisioning</span></span>](active-directory-saas-jive-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jive-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png

