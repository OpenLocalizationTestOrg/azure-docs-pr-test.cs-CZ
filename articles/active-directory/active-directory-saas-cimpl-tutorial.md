---
title: 'Kurz: Azure Active Directory integrace s Cimpl | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Cimpl."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58ee5481-ae40-4e4a-a3c9-86343851fc9a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 24a0c89966c83e1b32367d4519ead98d76f5ac6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cimpl"></a><span data-ttu-id="c11af-103">Kurz: Azure Active Directory integrace s Cimpl</span><span class="sxs-lookup"><span data-stu-id="c11af-103">Tutorial: Azure Active Directory integration with Cimpl</span></span>

<span data-ttu-id="c11af-104">V tomto kurzu zjistěte, jak integrovat Cimpl s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c11af-104">In this tutorial, you learn how to integrate Cimpl with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c11af-105">Integrace Cimpl s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c11af-105">Integrating Cimpl with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c11af-106">Můžete řídit ve službě Azure AD, který má přístup k Cimpl</span><span class="sxs-lookup"><span data-stu-id="c11af-106">You can control in Azure AD who has access to Cimpl</span></span>
- <span data-ttu-id="c11af-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Cimpl (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c11af-107">You can enable your users to automatically get signed-on to Cimpl (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c11af-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c11af-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c11af-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c11af-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c11af-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c11af-110">Prerequisites</span></span>

<span data-ttu-id="c11af-111">Konfigurace integrace Azure AD s Cimpl, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c11af-111">To configure Azure AD integration with Cimpl, you need the following items:</span></span>

- <span data-ttu-id="c11af-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c11af-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c11af-113">Cimpl jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c11af-113">A Cimpl single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c11af-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c11af-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c11af-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c11af-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c11af-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c11af-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c11af-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c11af-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c11af-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c11af-118">Scenario description</span></span>
<span data-ttu-id="c11af-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c11af-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c11af-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c11af-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c11af-121">Přidání Cimpl z Galerie</span><span class="sxs-lookup"><span data-stu-id="c11af-121">Adding Cimpl from the gallery</span></span>
2. <span data-ttu-id="c11af-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c11af-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cimpl-from-the-gallery"></a><span data-ttu-id="c11af-123">Přidání Cimpl z Galerie</span><span class="sxs-lookup"><span data-stu-id="c11af-123">Adding Cimpl from the gallery</span></span>
<span data-ttu-id="c11af-124">Při konfiguraci integrace Cimpl do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Cimpl z galerie.</span><span class="sxs-lookup"><span data-stu-id="c11af-124">To configure the integration of Cimpl into Azure AD, you need to add Cimpl from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c11af-125">**Pokud chcete přidat Cimpl z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c11af-125">**To add Cimpl from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c11af-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c11af-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c11af-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c11af-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c11af-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c11af-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c11af-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c11af-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c11af-133">Do vyhledávacího pole zadejte **Cimpl**.</span><span class="sxs-lookup"><span data-stu-id="c11af-133">In the search box, type **Cimpl**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_search.png)

5. <span data-ttu-id="c11af-135">Na panelu výsledků vyberte **Cimpl**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c11af-135">In the results panel, select **Cimpl**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c11af-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c11af-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c11af-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Cimpl podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c11af-138">In this section, you configure and test Azure AD single sign-on with Cimpl based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c11af-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Cimpl je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c11af-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cimpl is to a user in Azure AD.</span></span> <span data-ttu-id="c11af-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Cimpl musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c11af-140">In other words, a link relationship between an Azure AD user and the related user in Cimpl needs to be established.</span></span>

<span data-ttu-id="c11af-141">V Cimpl, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="c11af-141">In Cimpl, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c11af-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Cimpl, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c11af-142">To configure and test Azure AD single sign-on with Cimpl, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c11af-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c11af-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c11af-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c11af-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c11af-145">**[Vytvoření zkušebního uživatele Cimpl](#creating-a-cimpl-test-user)**  – Pokud chcete mít protějšek Britta Simon v Cimpl propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c11af-145">**[Creating a Cimpl test user](#creating-a-cimpl-test-user)** - to have a counterpart of Britta Simon in Cimpl that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c11af-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c11af-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c11af-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c11af-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c11af-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c11af-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c11af-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Cimpl.</span><span class="sxs-lookup"><span data-stu-id="c11af-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cimpl application.</span></span>

<span data-ttu-id="c11af-150">**Ke konfiguraci Azure AD jednotné přihlašování s Cimpl, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c11af-150">**To configure Azure AD single sign-on with Cimpl, perform the following steps:**</span></span>

1. <span data-ttu-id="c11af-151">Na portálu Azure na **Cimpl** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c11af-151">In the Azure portal, on the **Cimpl** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c11af-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c11af-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_samlbase.png)

3. <span data-ttu-id="c11af-155">Na **Cimpl domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c11af-155">On the **Cimpl Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_url.png)

    <span data-ttu-id="c11af-157">a.</span><span class="sxs-lookup"><span data-stu-id="c11af-157">a.</span></span> <span data-ttu-id="c11af-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://sso.etelesolv.com/<TENANTNAME>`</span><span class="sxs-lookup"><span data-stu-id="c11af-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://sso.etelesolv.com/<TENANTNAME>`</span></span>

    <span data-ttu-id="c11af-159">b.</span><span class="sxs-lookup"><span data-stu-id="c11af-159">b.</span></span> <span data-ttu-id="c11af-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://sso.etelesolv.com/<TENANTNAME>`</span><span class="sxs-lookup"><span data-stu-id="c11af-160">In the **Identifier** textbox, type a URL using the following pattern: `https://sso.etelesolv.com/<TENANTNAME>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c11af-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c11af-161">These values are not real.</span></span> <span data-ttu-id="c11af-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c11af-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c11af-163">Obraťte se na tým Cimpl v **+1 866-982-8250** k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="c11af-163">Contact Cimpl team at **+1 866-982-8250** to get these values.</span></span> 
 
4. <span data-ttu-id="c11af-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="c11af-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_certificate.png) 

5. <span data-ttu-id="c11af-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c11af-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cimpl-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c11af-168">Na **Cimpl konfigurace** klikněte na tlačítko **konfigurace Cimpl** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c11af-168">On the **Cimpl Configuration** section, click **Configure Cimpl** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c11af-169">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c11af-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_configure.png) 

7. <span data-ttu-id="c11af-171">Konfigurace jednotného přihlašování na **Cimpl** straně, budete muset odeslat stažené **certifikátu (Base64)**, **SAML Entity ID a SAML jeden přihlašování adresa URL služby** Cimpl podpory v **+1 866-982-8250**.</span><span class="sxs-lookup"><span data-stu-id="c11af-171">To configure single sign-on on **Cimpl** side, you need to send the downloaded **Certificate (Base64)**, **SAML Entity ID, and SAML Single Sign-On Service URL** to Cimpl support at **+1 866-982-8250**.</span></span>


> [!TIP]
> <span data-ttu-id="c11af-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c11af-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c11af-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c11af-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c11af-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c11af-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c11af-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c11af-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="c11af-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c11af-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c11af-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c11af-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c11af-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c11af-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cimpl-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c11af-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c11af-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cimpl-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c11af-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c11af-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cimpl-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c11af-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c11af-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cimpl-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c11af-187">a.</span><span class="sxs-lookup"><span data-stu-id="c11af-187">a.</span></span> <span data-ttu-id="c11af-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c11af-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c11af-189">b.</span><span class="sxs-lookup"><span data-stu-id="c11af-189">b.</span></span> <span data-ttu-id="c11af-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c11af-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c11af-191">c.</span><span class="sxs-lookup"><span data-stu-id="c11af-191">c.</span></span> <span data-ttu-id="c11af-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c11af-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c11af-193">d.</span><span class="sxs-lookup"><span data-stu-id="c11af-193">d.</span></span> <span data-ttu-id="c11af-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c11af-194">Click **Create**.</span></span>
 
### <a name="creating-a-cimpl-test-user"></a><span data-ttu-id="c11af-195">Vytvoření zkušebního uživatele Cimpl</span><span class="sxs-lookup"><span data-stu-id="c11af-195">Creating a Cimpl test user</span></span>

<span data-ttu-id="c11af-196">Cílem této části je vytvoření uživatele v Cimpl nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c11af-196">The objective of this section is to create a user called Britta Simon in Cimpl.</span></span> <span data-ttu-id="c11af-197">Práce s Cimpl podporu v **+1 866-982-8250** přidat uživatele do Cimpl účtu.</span><span class="sxs-lookup"><span data-stu-id="c11af-197">Work with Cimpl support at **+1 866-982-8250** to add the users in the Cimpl account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c11af-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c11af-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c11af-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Cimpl.</span><span class="sxs-lookup"><span data-stu-id="c11af-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cimpl.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c11af-201">**Pokud chcete přiřadit Britta Simon Cimpl, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c11af-201">**To assign Britta Simon to Cimpl, perform the following steps:**</span></span>

1. <span data-ttu-id="c11af-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c11af-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c11af-204">V seznamu aplikací vyberte **Cimpl**.</span><span class="sxs-lookup"><span data-stu-id="c11af-204">In the applications list, select **Cimpl**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_app.png) 

3. <span data-ttu-id="c11af-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c11af-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c11af-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c11af-208">Click **Add** button.</span></span> <span data-ttu-id="c11af-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c11af-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c11af-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c11af-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c11af-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c11af-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c11af-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c11af-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c11af-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c11af-214">Testing single sign-on</span></span>

<span data-ttu-id="c11af-215">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="c11af-215">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  <span data-ttu-id="c11af-216">Když kliknete na dlaždici Cimpl na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Cimpl.</span><span class="sxs-lookup"><span data-stu-id="c11af-216">When you click the Cimpl tile in the Access Panel, you should get automatically signed-on to your Cimpl application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c11af-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c11af-217">Additional resources</span></span>

* [<span data-ttu-id="c11af-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c11af-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c11af-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c11af-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_203.png

