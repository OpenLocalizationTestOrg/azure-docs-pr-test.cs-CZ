---
title: 'Kurz: Azure Active Directory integrace s Wingspan eTMF | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Wingspan eTMF."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ace320d3-521c-449c-992f-feabe7538de7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: 8c76fb64229abcad0cabb910e7c170979a79d839
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wingspan-etmf"></a><span data-ttu-id="c284b-103">Kurz: Azure Active Directory integrace s Wingspan eTMF</span><span class="sxs-lookup"><span data-stu-id="c284b-103">Tutorial: Azure Active Directory integration with Wingspan eTMF</span></span>

<span data-ttu-id="c284b-104">V tomto kurzu zjistěte, jak integrovat Wingspan eTMF s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c284b-104">In this tutorial, you learn how to integrate Wingspan eTMF with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c284b-105">Integrace Wingspan eTMF s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c284b-105">Integrating Wingspan eTMF with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c284b-106">Můžete řídit ve službě Azure AD, který má přístup k Wingspan eTMF</span><span class="sxs-lookup"><span data-stu-id="c284b-106">You can control in Azure AD who has access to Wingspan eTMF</span></span>
- <span data-ttu-id="c284b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Wingspan eTMF (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c284b-107">You can enable your users to automatically get signed-on to Wingspan eTMF (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c284b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c284b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c284b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c284b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c284b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c284b-110">Prerequisites</span></span>

<span data-ttu-id="c284b-111">Konfigurace integrace Azure AD s Wingspan eTMF, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c284b-111">To configure Azure AD integration with Wingspan eTMF, you need the following items:</span></span>

- <span data-ttu-id="c284b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c284b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c284b-113">Wingspan eTMF jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c284b-113">A Wingspan eTMF single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c284b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c284b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c284b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c284b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c284b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c284b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c284b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c284b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c284b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c284b-118">Scenario description</span></span>
<span data-ttu-id="c284b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c284b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c284b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c284b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c284b-121">Přidání Wingspan eTMF z Galerie</span><span class="sxs-lookup"><span data-stu-id="c284b-121">Adding Wingspan eTMF from the gallery</span></span>
2. <span data-ttu-id="c284b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c284b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wingspan-etmf-from-the-gallery"></a><span data-ttu-id="c284b-123">Přidání Wingspan eTMF z Galerie</span><span class="sxs-lookup"><span data-stu-id="c284b-123">Adding Wingspan eTMF from the gallery</span></span>
<span data-ttu-id="c284b-124">Při konfiguraci integrace Wingspan eTMF do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Wingspan eTMF z galerie.</span><span class="sxs-lookup"><span data-stu-id="c284b-124">To configure the integration of Wingspan eTMF into Azure AD, you need to add Wingspan eTMF from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c284b-125">**Pokud chcete přidat Wingspan eTMF z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c284b-125">**To add Wingspan eTMF from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c284b-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c284b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c284b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c284b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c284b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c284b-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c284b-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c284b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c284b-133">Do vyhledávacího pole zadejte **Wingspan eTMF**.</span><span class="sxs-lookup"><span data-stu-id="c284b-133">In the search box, type **Wingspan eTMF**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_search.png)

5. <span data-ttu-id="c284b-135">Na panelu výsledků vyberte **Wingspan eTMF**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c284b-135">In the results panel, select **Wingspan eTMF**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c284b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c284b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c284b-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Wingspan eTMF podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="c284b-138">In this section, you configure and test Azure AD single sign-on with Wingspan eTMF based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c284b-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Wingspan eTMF je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c284b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Wingspan eTMF is to a user in Azure AD.</span></span> <span data-ttu-id="c284b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Wingspan eTMF musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c284b-140">In other words, a link relationship between an Azure AD user and the related user in Wingspan eTMF needs to be established.</span></span>

<span data-ttu-id="c284b-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Wingspan eTMF.</span><span class="sxs-lookup"><span data-stu-id="c284b-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Wingspan eTMF.</span></span>

<span data-ttu-id="c284b-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Wingspan eTMF, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c284b-142">To configure and test Azure AD single sign-on with Wingspan eTMF, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c284b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c284b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c284b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c284b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c284b-145">**[Vytvoření zkušebního uživatele eTMF Wingspan](#creating-a-wingspan-etmf-test-user)**  – Pokud chcete mít protějšek Britta Simon v Wingspan eTMF propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c284b-145">**[Creating a Wingspan eTMF test user](#creating-a-wingspan-etmf-test-user)** - to have a counterpart of Britta Simon in Wingspan eTMF that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c284b-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c284b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c284b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c284b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c284b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c284b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c284b-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci eTMF Wingspan.</span><span class="sxs-lookup"><span data-stu-id="c284b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Wingspan eTMF application.</span></span>

<span data-ttu-id="c284b-150">**Ke konfiguraci Azure AD jednotné přihlašování s Wingspan eTMF, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c284b-150">**To configure Azure AD single sign-on with Wingspan eTMF, perform the following steps:**</span></span>

1. <span data-ttu-id="c284b-151">Na portálu Azure na **Wingspan eTMF** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c284b-151">In the Azure portal, on the **Wingspan eTMF** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c284b-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c284b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_samlbase.png)

3. <span data-ttu-id="c284b-155">Na **Wingspan eTMF domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c284b-155">On the **Wingspan eTMF Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_url11.png)

    <span data-ttu-id="c284b-157">a.</span><span class="sxs-lookup"><span data-stu-id="c284b-157">a.</span></span> <span data-ttu-id="c284b-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<customer name>.<instance name>.mywingspan.com/saml`</span><span class="sxs-lookup"><span data-stu-id="c284b-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<customer name>.<instance name>.mywingspan.com/saml`</span></span>

    <span data-ttu-id="c284b-159">b.</span><span class="sxs-lookup"><span data-stu-id="c284b-159">b.</span></span> <span data-ttu-id="c284b-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://saml.<instance name>.wingspan.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="c284b-160">In the **Identifier** textbox, type a URL using the following pattern: `http://saml.<instance name>.wingspan.com/shibboleth`</span></span>

    <span data-ttu-id="c284b-161">c.</span><span class="sxs-lookup"><span data-stu-id="c284b-161">c.</span></span> <span data-ttu-id="c284b-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<customer name>.<instance name>.mywingspan.com/`</span><span class="sxs-lookup"><span data-stu-id="c284b-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<customer name>.<instance name>.mywingspan.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="c284b-163">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="c284b-163">These values are not the real.</span></span> <span data-ttu-id="c284b-164">Tyto hodnoty aktualizujte skutečná adresa URL přihlašování, identifikátor a adresa URL odpovědi včetně zákazníka skutečný název a název instance.</span><span class="sxs-lookup"><span data-stu-id="c284b-164">Update these values with the actual Sign-On URL, Identifier and Reply URL including the actual customer name and instance name.</span></span> <span data-ttu-id="c284b-165">Obraťte se na [tým podpory klienta eTMF Wingspan](http://www.wingspan.com/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="c284b-165">Contact [Wingspan eTMF Client support team](http://www.wingspan.com/contact-us/) to get these values.</span></span> 

4. <span data-ttu-id="c284b-166">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c284b-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_certificate.png) 

5. <span data-ttu-id="c284b-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c284b-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c284b-170">Konfigurace jednotného přihlašování na **Wingspan eTMF** straně, budete muset odeslat stažené **soubor XML s metadaty** k [Wingspan eTMF podporu](http://www.wingspan.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="c284b-170">To configure single sign-on on **Wingspan eTMF** side, you need to send the downloaded **Metadata XML** to [Wingspan eTMF support](http://www.wingspan.com/contact-us/).</span></span> <span data-ttu-id="c284b-171">Se toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="c284b-171">They set this up to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="c284b-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c284b-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c284b-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c284b-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c284b-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c284b-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c284b-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c284b-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="c284b-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c284b-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c284b-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c284b-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c284b-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c284b-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c284b-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c284b-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c284b-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c284b-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c284b-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c284b-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c284b-187">a.</span><span class="sxs-lookup"><span data-stu-id="c284b-187">a.</span></span> <span data-ttu-id="c284b-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c284b-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c284b-189">b.</span><span class="sxs-lookup"><span data-stu-id="c284b-189">b.</span></span> <span data-ttu-id="c284b-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c284b-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c284b-191">c.</span><span class="sxs-lookup"><span data-stu-id="c284b-191">c.</span></span> <span data-ttu-id="c284b-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c284b-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c284b-193">d.</span><span class="sxs-lookup"><span data-stu-id="c284b-193">d.</span></span> <span data-ttu-id="c284b-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c284b-194">Click **Create**.</span></span>
 
### <a name="creating-a-wingspan-etmf-test-user"></a><span data-ttu-id="c284b-195">Vytvoření zkušebního uživatele eTMF Wingspan</span><span class="sxs-lookup"><span data-stu-id="c284b-195">Creating a Wingspan eTMF test user</span></span>

<span data-ttu-id="c284b-196">V této části vytvoříte volal Britta Simon v Wingspan eTMF uživatele.</span><span class="sxs-lookup"><span data-stu-id="c284b-196">In this section, you create a user called Britta Simon in Wingspan eTMF.</span></span> <span data-ttu-id="c284b-197">Práce s [Wingspan eTMF podporu](http://www.wingspan.com/contact-us/) přidejte uživatele v aplikaci eTMF Wingspan.</span><span class="sxs-lookup"><span data-stu-id="c284b-197">Work with [Wingspan eTMF support](http://www.wingspan.com/contact-us/) to add the users in the Wingspan eTMF application.</span></span> <span data-ttu-id="c284b-198">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c284b-198">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c284b-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c284b-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c284b-200">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k Wingspan eTMF.</span><span class="sxs-lookup"><span data-stu-id="c284b-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Wingspan eTMF.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c284b-202">**Pokud chcete přiřadit Britta Simon Wingspan eTMF, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c284b-202">**To assign Britta Simon to Wingspan eTMF, perform the following steps:**</span></span>

1. <span data-ttu-id="c284b-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c284b-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c284b-205">V seznamu aplikací vyberte **Wingspan eTMF**.</span><span class="sxs-lookup"><span data-stu-id="c284b-205">In the applications list, select **Wingspan eTMF**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_app.png) 

3. <span data-ttu-id="c284b-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c284b-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c284b-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c284b-209">Click **Add** button.</span></span> <span data-ttu-id="c284b-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c284b-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c284b-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c284b-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c284b-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c284b-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c284b-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c284b-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c284b-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c284b-215">Testing single sign-on</span></span>

<span data-ttu-id="c284b-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c284b-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> 

<span data-ttu-id="c284b-217">Klikněte na dlaždici Wingspan eTMF na přístupovém panelu, budete přesměrováni na organizaci přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="c284b-217">Click the Wingspan eTMF tile in the Access Panel, you will be redirected to Organization sign on page.</span></span> <span data-ttu-id="c284b-218">Po úspěšném přihlášení můžete se být přihlášení k aplikaci eTMF Wingspan.</span><span class="sxs-lookup"><span data-stu-id="c284b-218">After successful login, you will be signed-on to your Wingspan eTMF application.</span></span> <span data-ttu-id="c284b-219">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="c284b-219">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c284b-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c284b-220">Additional resources</span></span>

* [<span data-ttu-id="c284b-221">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c284b-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c284b-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c284b-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_203.png

