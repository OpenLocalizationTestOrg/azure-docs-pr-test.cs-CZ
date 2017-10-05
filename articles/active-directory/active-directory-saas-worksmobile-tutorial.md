---
title: 'Kurz: Azure Active Directory integrace s MOBILE FUNGUJE | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a FUNGUJE MOBILE."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 139a1968a59424eae278de3e7fa227ad340a1eb8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-works-mobile"></a><span data-ttu-id="af5bc-103">Kurz: Azure Active Directory integrace s MOBILE FUNGUJE</span><span class="sxs-lookup"><span data-stu-id="af5bc-103">Tutorial: Azure Active Directory integration with WORKS MOBILE</span></span>

<span data-ttu-id="af5bc-104">V tomto kurzu zjistěte, jak integrovat MOBILE FUNGUJE s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="af5bc-104">In this tutorial, you learn how to integrate WORKS MOBILE with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="af5bc-105">Integrace MOBILE FUNGUJE s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="af5bc-105">Integrating WORKS MOBILE with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="af5bc-106">Můžete řídit ve službě Azure AD, který má přístup k MOBILE FUNGUJE</span><span class="sxs-lookup"><span data-stu-id="af5bc-106">You can control in Azure AD who has access to WORKS MOBILE</span></span>
- <span data-ttu-id="af5bc-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k MOBILE FUNGUJE (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="af5bc-107">You can enable your users to automatically get signed-on to WORKS MOBILE (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="af5bc-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="af5bc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="af5bc-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="af5bc-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af5bc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="af5bc-110">Prerequisites</span></span>

<span data-ttu-id="af5bc-111">Konfigurace integrace Azure AD s MOBILE FUNGUJE, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="af5bc-111">To configure Azure AD integration with WORKS MOBILE, you need the following items:</span></span>

- <span data-ttu-id="af5bc-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="af5bc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="af5bc-113">MOBILE FUNGUJE jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="af5bc-113">A WORKS MOBILE single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="af5bc-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="af5bc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="af5bc-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="af5bc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="af5bc-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="af5bc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="af5bc-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="af5bc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="af5bc-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="af5bc-118">Scenario description</span></span>
<span data-ttu-id="af5bc-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="af5bc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="af5bc-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="af5bc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="af5bc-121">Přidání MOBILE FUNGUJE z Galerie</span><span class="sxs-lookup"><span data-stu-id="af5bc-121">Adding WORKS MOBILE from the gallery</span></span>
2. <span data-ttu-id="af5bc-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="af5bc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-works-mobile-from-the-gallery"></a><span data-ttu-id="af5bc-123">Přidání MOBILE FUNGUJE z Galerie</span><span class="sxs-lookup"><span data-stu-id="af5bc-123">Adding WORKS MOBILE from the gallery</span></span>
<span data-ttu-id="af5bc-124">Při konfiguraci integrace MOBILE FUNGUJE do služby Azure AD potřebujete přidat mobilní FUNGUJE z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="af5bc-124">To configure the integration of WORKS MOBILE into Azure AD, you need to add WORKS MOBILE from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="af5bc-125">**Pokud chcete přidat mobilní FUNGUJE z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="af5bc-125">**To add WORKS MOBILE from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="af5bc-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="af5bc-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="af5bc-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="af5bc-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="af5bc-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af5bc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="af5bc-133">Do vyhledávacího pole zadejte **MOBILE FUNGUJE**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-133">In the search box, type **WORKS MOBILE**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_search.png)

5. <span data-ttu-id="af5bc-135">Na panelu výsledků vyberte **MOBILE FUNGUJE**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af5bc-135">In the results panel, select **WORKS MOBILE**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="af5bc-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="af5bc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="af5bc-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s MOBILE FUNGUJE podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="af5bc-138">In this section, you configure and test Azure AD single sign-on with WORKS MOBILE based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="af5bc-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v MOBILE FUNGUJE je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="af5bc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in WORKS MOBILE is to a user in Azure AD.</span></span> <span data-ttu-id="af5bc-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v MOBILE FUNGUJE musí navázat.</span><span class="sxs-lookup"><span data-stu-id="af5bc-140">In other words, a link relationship between an Azure AD user and the related user in WORKS MOBILE needs to be established.</span></span>

<span data-ttu-id="af5bc-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v MOBILE FUNGUJE.</span><span class="sxs-lookup"><span data-stu-id="af5bc-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in WORKS MOBILE.</span></span>

<span data-ttu-id="af5bc-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s MOBILE FUNGUJE, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="af5bc-142">To configure and test Azure AD single sign-on with WORKS MOBILE, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="af5bc-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="af5bc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="af5bc-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="af5bc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="af5bc-145">**[Vytvoření zkušebního uživatele MOBILE FUNGUJE](#creating-a-works-mobile-test-user)**  – Pokud chcete mít protějšek Britta Simon v FUNGUJE MOBILE, která je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="af5bc-145">**[Creating a WORKS MOBILE test user](#creating-a-works-mobile-test-user)** - to have a counterpart of Britta Simon in WORKS MOBILE that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="af5bc-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="af5bc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="af5bc-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="af5bc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="af5bc-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="af5bc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="af5bc-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci MOBILE FUNGUJE.</span><span class="sxs-lookup"><span data-stu-id="af5bc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your WORKS MOBILE application.</span></span>

<span data-ttu-id="af5bc-150">**Ke konfiguraci Azure AD jednotné přihlašování s MOBILE FUNGUJE, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="af5bc-150">**To configure Azure AD single sign-on with WORKS MOBILE, perform the following steps:**</span></span>

1. <span data-ttu-id="af5bc-151">Na portálu Azure na **MOBILE FUNGUJE** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-151">In the Azure portal, on the **WORKS MOBILE** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="af5bc-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="af5bc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_samlbase.png)

3. <span data-ttu-id="af5bc-155">Na **FUNGUJE MOBILE domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="af5bc-155">On the **WORKS MOBILE Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_url.png)

    <span data-ttu-id="af5bc-157">a.</span><span class="sxs-lookup"><span data-stu-id="af5bc-157">a.</span></span> <span data-ttu-id="af5bc-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.worksmobile.com/jp/myservice`</span><span class="sxs-lookup"><span data-stu-id="af5bc-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.worksmobile.com/jp/myservice`</span></span>

    <span data-ttu-id="af5bc-159">b.</span><span class="sxs-lookup"><span data-stu-id="af5bc-159">b.</span></span> <span data-ttu-id="af5bc-160">V **identifikátor** textovému poli, zadejte hodnotu jako`worksmobile.com`</span><span class="sxs-lookup"><span data-stu-id="af5bc-160">In the **Identifier** textbox, type the value as `worksmobile.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="af5bc-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="af5bc-161">This value is not real.</span></span> <span data-ttu-id="af5bc-162">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="af5bc-162">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="af5bc-163">Obraťte se na [tým podpory FUNGUJE MOBILNÍHO klienta](mailto:dl_ssoinfo@worksmobile.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="af5bc-163">Contact [WORKS MOBILE Client support team](mailto:dl_ssoinfo@worksmobile.com) to get this value.</span></span> 
 
4. <span data-ttu-id="af5bc-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="af5bc-164">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_certificate.png) 

5. <span data-ttu-id="af5bc-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="af5bc-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="af5bc-168">Na **Konfigurace mobilních FUNGUJE** klikněte na tlačítko **nakonfigurovat mobilní FUNGUJE** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="af5bc-168">On the **WORKS MOBILE Configuration** section, click **Configure WORKS MOBILE** to open **Configure sign-on** window.</span></span> <span data-ttu-id="af5bc-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="af5bc-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_configure.png) 

7. <span data-ttu-id="af5bc-171">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [tým podpory FUNGUJE MOBILE](mailto:dl_ssoinfo@worksmobile.com) a uveďte následující informace:</span><span class="sxs-lookup"><span data-stu-id="af5bc-171">To get SSO configured for your application, contact [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) and provide them with the following information:</span></span> 

    <span data-ttu-id="af5bc-172">• Stažené **soubor certifikátu**</span><span class="sxs-lookup"><span data-stu-id="af5bc-172">• The downloaded **Certificate file**</span></span>

    <span data-ttu-id="af5bc-173">• **Adresa URL služby jednotného přihlašování SAML**</span><span class="sxs-lookup"><span data-stu-id="af5bc-173">• The **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="af5bc-174">• **SAML Entity ID**</span><span class="sxs-lookup"><span data-stu-id="af5bc-174">• The **SAML Entity ID**</span></span>

    <span data-ttu-id="af5bc-175">• **Odhlášení adresy URL**</span><span class="sxs-lookup"><span data-stu-id="af5bc-175">• The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="af5bc-176">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="af5bc-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="af5bc-177">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="af5bc-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="af5bc-178">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="af5bc-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="af5bc-179">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="af5bc-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="af5bc-180">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="af5bc-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="af5bc-182">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="af5bc-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="af5bc-183">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="af5bc-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="af5bc-185">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="af5bc-187">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af5bc-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="af5bc-189">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="af5bc-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="af5bc-191">a.</span><span class="sxs-lookup"><span data-stu-id="af5bc-191">a.</span></span> <span data-ttu-id="af5bc-192">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="af5bc-193">b.</span><span class="sxs-lookup"><span data-stu-id="af5bc-193">b.</span></span> <span data-ttu-id="af5bc-194">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="af5bc-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="af5bc-195">c.</span><span class="sxs-lookup"><span data-stu-id="af5bc-195">c.</span></span> <span data-ttu-id="af5bc-196">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="af5bc-197">d.</span><span class="sxs-lookup"><span data-stu-id="af5bc-197">d.</span></span> <span data-ttu-id="af5bc-198">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-198">Click **Create**.</span></span>
 
### <a name="creating-a-works-mobile-test-user"></a><span data-ttu-id="af5bc-199">Vytvoření zkušebního uživatele MOBILE FUNGUJE</span><span class="sxs-lookup"><span data-stu-id="af5bc-199">Creating a WORKS MOBILE test user</span></span>

 <span data-ttu-id="af5bc-200">V této části vytvoříte uživatele v MOBILE FUNGUJE jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="af5bc-200">In this section, you create a user called Britta Simon in WORKS MOBILE.</span></span> <span data-ttu-id="af5bc-201">Spojte se s [tým podpory FUNGUJE MOBILE](mailto:dl_ssoinfo@worksmobile.com) přidat uživatele do FUNGUJE mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="af5bc-201">Please work with [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) to add the users in the WORKS MOBILE platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="af5bc-202">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="af5bc-202">Assigning the Azure AD test user</span></span>

<span data-ttu-id="af5bc-203">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k MOBILE FUNGUJE.</span><span class="sxs-lookup"><span data-stu-id="af5bc-203">In this section, you enable Britta Simon to use Azure single sign-on by granting access to WORKS MOBILE.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="af5bc-205">**Pokud chcete přiřadit Britta Simon MOBILE FUNGUJE, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="af5bc-205">**To assign Britta Simon to WORKS MOBILE, perform the following steps:**</span></span>

1. <span data-ttu-id="af5bc-206">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-206">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="af5bc-208">V seznamu aplikací vyberte **MOBILE FUNGUJE**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-208">In the applications list, select **WORKS MOBILE**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_app.png) 

3. <span data-ttu-id="af5bc-210">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="af5bc-210">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="af5bc-212">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="af5bc-212">Click **Add** button.</span></span> <span data-ttu-id="af5bc-213">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af5bc-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="af5bc-215">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="af5bc-215">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="af5bc-216">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af5bc-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="af5bc-217">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af5bc-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="af5bc-218">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="af5bc-218">Testing single sign-on</span></span>

<span data-ttu-id="af5bc-219">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="af5bc-219">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="af5bc-220">Když kliknete na dlaždici MOBILE FUNGUJE na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci MOBILE FUNGUJE.</span><span class="sxs-lookup"><span data-stu-id="af5bc-220">When you click the WORKS MOBILE tile in the Access Panel, you should get automatically signed-on to your WORKS MOBILE application.</span></span>
<span data-ttu-id="af5bc-221">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="af5bc-221">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="af5bc-222">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="af5bc-222">Additional resources</span></span>

* [<span data-ttu-id="af5bc-223">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="af5bc-223">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="af5bc-224">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="af5bc-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_203.png

