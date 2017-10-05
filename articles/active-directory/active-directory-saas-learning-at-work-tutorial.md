---
title: "Kurz: Azure Active Directory integrace s učení v práci | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a učení v práci."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d607174-bea1-4f40-8233-54cabe02c66a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: 941832740689c583a8e857d706c35f3076fa754f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a><span data-ttu-id="5109c-103">Kurz: Azure Active Directory integrace s učení v práci</span><span class="sxs-lookup"><span data-stu-id="5109c-103">Tutorial: Azure Active Directory integration with Learning at Work</span></span>

<span data-ttu-id="5109c-104">V tomto kurzu zjistěte, jak integrovat učení v práci s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5109c-104">In this tutorial, you learn how to integrate Learning at Work with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5109c-105">Integrace učení v práci s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5109c-105">Integrating Learning at Work with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5109c-106">Můžete řídit ve službě Azure AD, který má přístup k učení v práci</span><span class="sxs-lookup"><span data-stu-id="5109c-106">You can control in Azure AD who has access to Learning at Work</span></span>
- <span data-ttu-id="5109c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k učení v práci (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5109c-107">You can enable your users to automatically get signed-on to Learning at Work (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5109c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5109c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5109c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5109c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5109c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5109c-110">Prerequisites</span></span>

<span data-ttu-id="5109c-111">Konfigurace integrace Azure AD s učení v práci, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5109c-111">To configure Azure AD integration with Learning at Work, you need the following items:</span></span>

- <span data-ttu-id="5109c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5109c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5109c-113">Učení v pracovní jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5109c-113">A Learning at Work single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5109c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5109c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5109c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5109c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5109c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5109c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5109c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5109c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5109c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5109c-118">Scenario description</span></span>
<span data-ttu-id="5109c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5109c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5109c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5109c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5109c-121">Přidání učení v práci z Galerie</span><span class="sxs-lookup"><span data-stu-id="5109c-121">Adding Learning at Work from the gallery</span></span>
2. <span data-ttu-id="5109c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5109c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learning-at-work-from-the-gallery"></a><span data-ttu-id="5109c-123">Přidání učení v práci z Galerie</span><span class="sxs-lookup"><span data-stu-id="5109c-123">Adding Learning at Work from the gallery</span></span>
<span data-ttu-id="5109c-124">Při konfiguraci integrace učení v práci do služby Azure AD, potřebujete přidat učení v práci z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5109c-124">To configure the integration of Learning at Work into Azure AD, you need to add Learning at Work from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5109c-125">**Pokud chcete přidat učení v práci z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5109c-125">**To add Learning at Work from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5109c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5109c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5109c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5109c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5109c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5109c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5109c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5109c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5109c-133">Do vyhledávacího pole zadejte **učení v práci**.</span><span class="sxs-lookup"><span data-stu-id="5109c-133">In the search box, type **Learning at Work**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_search.png)

5. <span data-ttu-id="5109c-135">Na panelu výsledků vyberte **učení v práci**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5109c-135">In the results panel, select **Learning at Work**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5109c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5109c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5109c-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s učení v práci na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5109c-138">In this section, you configure and test Azure AD single sign-on with Learning at Work based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5109c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v učení v práci je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5109c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Learning at Work is to a user in Azure AD.</span></span> <span data-ttu-id="5109c-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v učení v práci.</span><span class="sxs-lookup"><span data-stu-id="5109c-140">In other words, a link relationship between an Azure AD user and the related user in Learning at Work needs to be established.</span></span>

<span data-ttu-id="5109c-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v učení v práci.</span><span class="sxs-lookup"><span data-stu-id="5109c-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Learning at Work.</span></span>

<span data-ttu-id="5109c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s učení v práci, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5109c-142">To configure and test Azure AD single sign-on with Learning at Work, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5109c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5109c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5109c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5109c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5109c-145">**[Vytváření Learning na pracovní testovací uživatele](#creating-a-learning-at-work-test-user)**  – Pokud chcete mít protějšek Britta Simon v učení v práci, kterou je propojena k reprezentaci Azure AD uživatele.</span><span class="sxs-lookup"><span data-stu-id="5109c-145">**[Creating a Learning at Work test user](#creating-a-learning-at-work-test-user)** - to have a counterpart of Britta Simon in Learning at Work that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5109c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5109c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5109c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5109c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5109c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5109c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5109c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování ve svém studiu na pracovní aplikace.</span><span class="sxs-lookup"><span data-stu-id="5109c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Learning at Work application.</span></span>

<span data-ttu-id="5109c-150">**Ke konfiguraci Azure AD jednotné přihlašování s učení v práci, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5109c-150">**To configure Azure AD single sign-on with Learning at Work, perform the following steps:**</span></span>

1. <span data-ttu-id="5109c-151">Na portálu Azure na **učení v práci** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5109c-151">In the Azure portal, on the **Learning at Work** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5109c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5109c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_samlbase.png)

3. <span data-ttu-id="5109c-155">Na **učení v pracovní domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5109c-155">On the **Learning at Work Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_url.png)

    <span data-ttu-id="5109c-157">a.</span><span class="sxs-lookup"><span data-stu-id="5109c-157">a.</span></span> <span data-ttu-id="5109c-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span><span class="sxs-lookup"><span data-stu-id="5109c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span></span>

    <span data-ttu-id="5109c-159">b.</span><span class="sxs-lookup"><span data-stu-id="5109c-159">b.</span></span> <span data-ttu-id="5109c-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span><span class="sxs-lookup"><span data-stu-id="5109c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5109c-161">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="5109c-161">These values are not the real.</span></span> <span data-ttu-id="5109c-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5109c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5109c-163">Obraťte se na [učení na tým podpory pracovní klienta](https://www.learninga-z.com/site/contact/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5109c-163">Contact [Learning at Work Client support team](https://www.learninga-z.com/site/contact/support) to get these values.</span></span> 
 
4. <span data-ttu-id="5109c-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5109c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_certificate.png) 

5. <span data-ttu-id="5109c-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5109c-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5109c-168">Na **učení v konfiguraci pracovní** klikněte na tlačítko **konfigurace učení v práci** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5109c-168">On the **Learning at Work Configuration** section, click **Configure Learning at Work** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5109c-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5109c-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_configure.png) 

7. <span data-ttu-id="5109c-171">Konfigurace jednotného přihlašování na **učení v práci** straně, budete muset odeslat stažené **soubor XML s metadaty**, **SAML Entity ID**, **SAML-služby přihlášení Adresa URL**, a **Sign-Out URL** k [učení na podporu pracovních](https://www.learninga-z.com/site/contact/support).</span><span class="sxs-lookup"><span data-stu-id="5109c-171">To configure single sign-on on **Learning at Work** side, you need to send the downloaded **Metadata XML**, **SAML Entity ID**, **SAML Single Sign-On Service URL**, and **Sign-Out URL** to [Learning at Work support](https://www.learninga-z.com/site/contact/support).</span></span>

> [!TIP]
> <span data-ttu-id="5109c-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5109c-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5109c-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5109c-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5109c-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5109c-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5109c-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5109c-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="5109c-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5109c-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5109c-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5109c-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5109c-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5109c-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5109c-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5109c-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5109c-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5109c-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5109c-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5109c-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5109c-187">a.</span><span class="sxs-lookup"><span data-stu-id="5109c-187">a.</span></span> <span data-ttu-id="5109c-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5109c-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5109c-189">b.</span><span class="sxs-lookup"><span data-stu-id="5109c-189">b.</span></span> <span data-ttu-id="5109c-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5109c-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5109c-191">c.</span><span class="sxs-lookup"><span data-stu-id="5109c-191">c.</span></span> <span data-ttu-id="5109c-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5109c-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5109c-193">d.</span><span class="sxs-lookup"><span data-stu-id="5109c-193">d.</span></span> <span data-ttu-id="5109c-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5109c-194">Click **Create**.</span></span>
 
### <a name="creating-a-learning-at-work-test-user"></a><span data-ttu-id="5109c-195">Vytváření Learning na pracovní testovací uživatele</span><span class="sxs-lookup"><span data-stu-id="5109c-195">Creating a Learning at Work test user</span></span>

<span data-ttu-id="5109c-196">V této části vytvoříte uživatele volal Britta Simon v učení v práci.</span><span class="sxs-lookup"><span data-stu-id="5109c-196">In this section, you create a user called Britta Simon in Learning at Work.</span></span> <span data-ttu-id="5109c-197">Práce s [učení na podporu pracovních](https://www.learninga-z.com/site/contact/support) přidat uživatele do učení na platformě pracovní.</span><span class="sxs-lookup"><span data-stu-id="5109c-197">Work with [Learning at Work support](https://www.learninga-z.com/site/contact/support) to add the users in the Learning at Work platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5109c-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5109c-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5109c-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu učení v práci.</span><span class="sxs-lookup"><span data-stu-id="5109c-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Learning at Work.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5109c-201">**Pokud chcete přiřadit Britta Simon učení v práci, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5109c-201">**To assign Britta Simon to Learning at Work, perform the following steps:**</span></span>

1. <span data-ttu-id="5109c-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5109c-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5109c-204">V seznamu aplikací vyberte **učení v práci**.</span><span class="sxs-lookup"><span data-stu-id="5109c-204">In the applications list, select **Learning at Work**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_app.png) 

3. <span data-ttu-id="5109c-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5109c-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5109c-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5109c-208">Click **Add** button.</span></span> <span data-ttu-id="5109c-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5109c-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5109c-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5109c-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5109c-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5109c-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5109c-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5109c-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5109c-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5109c-214">Testing single sign-on</span></span>

<span data-ttu-id="5109c-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5109c-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5109c-216">Po kliknutí na tlačítko učení v dlaždici pracovní na přístupovém panelu, jste měli získat automaticky přihlášení k vaší učení v pracovní aplikace.</span><span class="sxs-lookup"><span data-stu-id="5109c-216">When you click the Learning at Work tile in the Access Panel, you should get automatically signed-on to your Learning at Work application.</span></span>
<span data-ttu-id="5109c-217">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5109c-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5109c-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5109c-218">Additional resources</span></span>

* [<span data-ttu-id="5109c-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5109c-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5109c-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5109c-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_203.png

