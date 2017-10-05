---
title: 'Kurz: Azure Active Directory integrace s PostBeyond | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PostBeyond."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 09992f08-ec50-4472-997f-ccbe719039e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 80b920dc99619795cbc86e8cb2e3ac2a78a71932
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-postbeyond"></a><span data-ttu-id="ae411-103">Kurz: Azure Active Directory integrace s PostBeyond</span><span class="sxs-lookup"><span data-stu-id="ae411-103">Tutorial: Azure Active Directory integration with PostBeyond</span></span>

<span data-ttu-id="ae411-104">V tomto kurzu zjistěte, jak integrovat PostBeyond s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ae411-104">In this tutorial, you learn how to integrate PostBeyond with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ae411-105">Integrace PostBeyond s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ae411-105">Integrating PostBeyond with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ae411-106">Můžete řídit ve službě Azure AD, který má přístup k PostBeyond</span><span class="sxs-lookup"><span data-stu-id="ae411-106">You can control in Azure AD who has access to PostBeyond</span></span>
- <span data-ttu-id="ae411-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k PostBeyond (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ae411-107">You can enable your users to automatically get signed-on to PostBeyond (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ae411-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ae411-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ae411-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ae411-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae411-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ae411-110">Prerequisites</span></span>

<span data-ttu-id="ae411-111">Konfigurace integrace Azure AD s PostBeyond, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ae411-111">To configure Azure AD integration with PostBeyond, you need the following items:</span></span>

- <span data-ttu-id="ae411-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ae411-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ae411-113">PostBeyond jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ae411-113">A PostBeyond single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ae411-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ae411-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ae411-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ae411-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ae411-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ae411-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ae411-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ae411-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ae411-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ae411-118">Scenario description</span></span>
<span data-ttu-id="ae411-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ae411-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ae411-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ae411-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ae411-121">Přidání PostBeyond z Galerie</span><span class="sxs-lookup"><span data-stu-id="ae411-121">Adding PostBeyond from the gallery</span></span>
2. <span data-ttu-id="ae411-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ae411-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-postbeyond-from-the-gallery"></a><span data-ttu-id="ae411-123">Přidání PostBeyond z Galerie</span><span class="sxs-lookup"><span data-stu-id="ae411-123">Adding PostBeyond from the gallery</span></span>
<span data-ttu-id="ae411-124">Chcete-li nakonfigurovat integraci PostBeyond do služby Azure AD, přidejte PostBeyond z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ae411-124">To configure the integration of PostBeyond into Azure AD, you need to add PostBeyond from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ae411-125">**Pokud chcete přidat PostBeyond z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ae411-125">**To add PostBeyond from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ae411-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ae411-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ae411-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ae411-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ae411-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ae411-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ae411-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ae411-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ae411-133">Do vyhledávacího pole zadejte **PostBeyond**.</span><span class="sxs-lookup"><span data-stu-id="ae411-133">In the search box, type **PostBeyond**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_search.png)

5. <span data-ttu-id="ae411-135">Na panelu výsledků vyberte **PostBeyond**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae411-135">In the results panel, select **PostBeyond**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ae411-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ae411-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ae411-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PostBeyond podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ae411-138">In this section, you configure and test Azure AD single sign-on with PostBeyond based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ae411-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v PostBeyond je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae411-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PostBeyond is to a user in Azure AD.</span></span> <span data-ttu-id="ae411-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v PostBeyond musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ae411-140">In other words, a link relationship between an Azure AD user and the related user in PostBeyond needs to be established.</span></span>

<span data-ttu-id="ae411-141">V PostBeyond, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ae411-141">In PostBeyond, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ae411-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s PostBeyond, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ae411-142">To configure and test Azure AD single sign-on with PostBeyond, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ae411-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ae411-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ae411-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ae411-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ae411-145">**[Vytváření PostBeyond testovacího uživatele](#creating-a-postbeyond-test-user)**  – Pokud chcete mít protějšek Britta Simon v PostBeyond propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ae411-145">**[Creating a PostBeyond test user](#creating-a-postbeyond-test-user)** - to have a counterpart of Britta Simon in PostBeyond that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ae411-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ae411-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ae411-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ae411-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ae411-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ae411-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ae411-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci PostBeyond.</span><span class="sxs-lookup"><span data-stu-id="ae411-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PostBeyond application.</span></span>

<span data-ttu-id="ae411-150">**Ke konfiguraci Azure AD jednotné přihlašování s PostBeyond, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ae411-150">**To configure Azure AD single sign-on with PostBeyond, perform the following steps:**</span></span>

1. <span data-ttu-id="ae411-151">Na portálu Azure na **PostBeyond** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ae411-151">In the Azure portal, on the **PostBeyond** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ae411-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ae411-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_samlbase.png)

3. <span data-ttu-id="ae411-155">Na **PostBeyond domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ae411-155">On the **PostBeyond Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_url.png)

    <span data-ttu-id="ae411-157">a.</span><span class="sxs-lookup"><span data-stu-id="ae411-157">a.</span></span> <span data-ttu-id="ae411-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.postbeyond.com`</span><span class="sxs-lookup"><span data-stu-id="ae411-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.postbeyond.com`</span></span>

    <span data-ttu-id="ae411-159">b.</span><span class="sxs-lookup"><span data-stu-id="ae411-159">b.</span></span> <span data-ttu-id="ae411-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.postbeyond.com`</span><span class="sxs-lookup"><span data-stu-id="ae411-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.postbeyond.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ae411-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ae411-161">These values are not real.</span></span> <span data-ttu-id="ae411-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ae411-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ae411-163">Obraťte se na [tým podpory PostBeyond klienta](mailto:sso@postbeyond.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="ae411-163">Contact [PostBeyond Client support team](mailto:sso@postbeyond.com) to get these values.</span></span> 
 
4. <span data-ttu-id="ae411-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="ae411-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_certificate.png) 

5. <span data-ttu-id="ae411-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ae411-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-postbeyond-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ae411-168">Na **PostBeyond konfigurace** klikněte na tlačítko **konfigurace PostBeyond** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ae411-168">On the **PostBeyond Configuration** section, click **Configure PostBeyond** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ae411-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ae411-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_configure.png) 

7. <span data-ttu-id="ae411-171">Konfigurace jednotného přihlašování na **PostBeyond** straně, budete muset odeslat stažené **Certificate(Base64)**, **SAML Entity ID**, **SAML jeden přihlašování adresa URL služby** a **Sign-Out URL** k [tým podpory PostBeyond](mailto:sso@postbeyond.com).</span><span class="sxs-lookup"><span data-stu-id="ae411-171">To configure single sign-on on **PostBeyond** side, you need to send the downloaded **Certificate(Base64)**, **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL** to [PostBeyond support team](mailto:sso@postbeyond.com).</span></span> <span data-ttu-id="ae411-172">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="ae411-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="ae411-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ae411-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ae411-174">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ae411-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ae411-175">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ae411-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ae411-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ae411-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="ae411-177">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ae411-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ae411-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ae411-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ae411-180">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ae411-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ae411-182">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ae411-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ae411-184">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ae411-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ae411-186">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ae411-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ae411-188">a.</span><span class="sxs-lookup"><span data-stu-id="ae411-188">a.</span></span> <span data-ttu-id="ae411-189">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ae411-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ae411-190">b.</span><span class="sxs-lookup"><span data-stu-id="ae411-190">b.</span></span> <span data-ttu-id="ae411-191">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ae411-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ae411-192">c.</span><span class="sxs-lookup"><span data-stu-id="ae411-192">c.</span></span> <span data-ttu-id="ae411-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ae411-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ae411-194">d.</span><span class="sxs-lookup"><span data-stu-id="ae411-194">d.</span></span> <span data-ttu-id="ae411-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ae411-195">Click **Create**.</span></span>
 
### <a name="creating-a-postbeyond-test-user"></a><span data-ttu-id="ae411-196">Vytváření PostBeyond zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="ae411-196">Creating a PostBeyond test user</span></span>

<span data-ttu-id="ae411-197">V této části vytvoříte volal Britta Simon v PostBeyond uživatele.</span><span class="sxs-lookup"><span data-stu-id="ae411-197">In this section, you create a user called Britta Simon in PostBeyond.</span></span> <span data-ttu-id="ae411-198">Pokud nevíte jak přidat Britta Simon v PostBeyond, spojte se s [tým podpory PostBeyond](mailto:sso@postbeyond.com) k přidání testovacího uživatele a povolení jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ae411-198">If you don't know how to add Britta Simon in PostBeyond, please work with [PostBeyond support team](mailto:sso@postbeyond.com) to add the test user and enable SSO.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ae411-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ae411-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ae411-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu PostBeyond.</span><span class="sxs-lookup"><span data-stu-id="ae411-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PostBeyond.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ae411-202">**Pokud chcete přiřadit Britta Simon PostBeyond, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ae411-202">**To assign Britta Simon to PostBeyond, perform the following steps:**</span></span>

1. <span data-ttu-id="ae411-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ae411-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ae411-205">V seznamu aplikací vyberte **PostBeyond**.</span><span class="sxs-lookup"><span data-stu-id="ae411-205">In the applications list, select **PostBeyond**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_app.png) 

3. <span data-ttu-id="ae411-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ae411-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ae411-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ae411-209">Click **Add** button.</span></span> <span data-ttu-id="ae411-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ae411-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ae411-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ae411-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ae411-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ae411-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ae411-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ae411-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ae411-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ae411-215">Testing single sign-on</span></span>

<span data-ttu-id="ae411-216">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="ae411-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="ae411-217">Když kliknete na dlaždici PostBeyond na přístupovém panelu, měli byste obdržet k PostBeyond přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="ae411-217">When you click the PostBeyond tile in the Access Panel, you should get to the PostBeyond sign in page.</span></span> <span data-ttu-id="ae411-218">Klikněte na **přihlásit pomocí Office 365**, zadejte přihlašovací údaje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae411-218">Click on **Sign in with Office 365**, enter your Azure AD credentials.</span></span> <span data-ttu-id="ae411-219">Potom můžete by měl být přihlášení do PostBeyond.</span><span class="sxs-lookup"><span data-stu-id="ae411-219">Then, you should be logged in into PostBeyond.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ae411-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ae411-220">Additional resources</span></span>

* [<span data-ttu-id="ae411-221">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ae411-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ae411-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ae411-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_203.png

