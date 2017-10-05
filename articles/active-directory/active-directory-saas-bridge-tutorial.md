---
title: 'Kurz: Azure Active Directory integrace s most | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a most."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dbb6499-80c1-4d00-a0b4-e0ad5522cf0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d2b7fd6f73f1b782cfe6337d13f9f22f9c70d389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bridge"></a><span data-ttu-id="97210-103">Kurz: Azure Active Directory integrace s most</span><span class="sxs-lookup"><span data-stu-id="97210-103">Tutorial: Azure Active Directory integration with Bridge</span></span>

<span data-ttu-id="97210-104">V tomto kurzu zjistěte, jak integrovat most s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="97210-104">In this tutorial, you learn how to integrate Bridge with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="97210-105">Integrace s Azure AD most? víc informací nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="97210-105">Integrating Bridge with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="97210-106">Můžete řídit ve službě Azure AD, který má přístup k most</span><span class="sxs-lookup"><span data-stu-id="97210-106">You can control in Azure AD who has access to Bridge</span></span>
- <span data-ttu-id="97210-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k most (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="97210-107">You can enable your users to automatically get signed-on to Bridge (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="97210-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="97210-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="97210-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="97210-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97210-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="97210-110">Prerequisites</span></span>

<span data-ttu-id="97210-111">Konfigurace integrace Azure AD s most, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="97210-111">To configure Azure AD integration with Bridge, you need the following items:</span></span>

- <span data-ttu-id="97210-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="97210-112">An Azure AD subscription</span></span>
- <span data-ttu-id="97210-113">Most jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="97210-113">A Bridge single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="97210-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="97210-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="97210-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="97210-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="97210-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="97210-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="97210-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="97210-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="97210-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="97210-118">Scenario description</span></span>
<span data-ttu-id="97210-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="97210-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="97210-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="97210-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="97210-121">Přidání most z Galerie</span><span class="sxs-lookup"><span data-stu-id="97210-121">Adding Bridge from the gallery</span></span>
2. <span data-ttu-id="97210-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="97210-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bridge-from-the-gallery"></a><span data-ttu-id="97210-123">Přidání most z Galerie</span><span class="sxs-lookup"><span data-stu-id="97210-123">Adding Bridge from the gallery</span></span>
<span data-ttu-id="97210-124">Při konfiguraci integrace most do služby Azure AD potřebujete přidat most z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="97210-124">To configure the integration of Bridge into Azure AD, you need to add Bridge from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="97210-125">**Pokud chcete přidat most z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97210-125">**To add Bridge from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="97210-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="97210-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="97210-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="97210-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="97210-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="97210-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="97210-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97210-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="97210-133">Do vyhledávacího pole zadejte **most**.</span><span class="sxs-lookup"><span data-stu-id="97210-133">In the search box, type **Bridge**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_search.png)

5. <span data-ttu-id="97210-135">Na panelu výsledků vyberte **most**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97210-135">In the results panel, select **Bridge**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="97210-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="97210-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="97210-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s most podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="97210-138">In this section, you configure and test Azure AD single sign-on with Bridge based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="97210-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v most je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="97210-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Bridge is to a user in Azure AD.</span></span> <span data-ttu-id="97210-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v most musí navázat.</span><span class="sxs-lookup"><span data-stu-id="97210-140">In other words, a link relationship between an Azure AD user and the related user in Bridge needs to be established.</span></span>

<span data-ttu-id="97210-141">V mostu, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="97210-141">In Bridge, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="97210-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s most, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="97210-142">To configure and test Azure AD single sign-on with Bridge, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="97210-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="97210-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="97210-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97210-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="97210-145">**[Vytvoření zkušebního uživatele most](#creating-a-bridge-test-user)**  – Pokud chcete mít protějšek Britta Simon v mostu, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="97210-145">**[Creating a Bridge test user](#creating-a-bridge-test-user)** - to have a counterpart of Britta Simon in Bridge that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="97210-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="97210-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="97210-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="97210-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="97210-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="97210-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="97210-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci most.</span><span class="sxs-lookup"><span data-stu-id="97210-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Bridge application.</span></span>

<span data-ttu-id="97210-150">**Ke konfiguraci Azure AD jednotné přihlašování s most, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97210-150">**To configure Azure AD single sign-on with Bridge, perform the following steps:**</span></span>

1. <span data-ttu-id="97210-151">Na portálu Azure na **most** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="97210-151">In the Azure portal, on the **Bridge** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="97210-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="97210-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_samlbase.png)

3. <span data-ttu-id="97210-155">Na **most domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="97210-155">On the **Bridge Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_url.png)

    <span data-ttu-id="97210-157">a.</span><span class="sxs-lookup"><span data-stu-id="97210-157">a.</span></span> <span data-ttu-id="97210-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.bridgeapp.com`</span><span class="sxs-lookup"><span data-stu-id="97210-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.bridgeapp.com`</span></span>

    <span data-ttu-id="97210-159">b.</span><span class="sxs-lookup"><span data-stu-id="97210-159">b.</span></span> <span data-ttu-id="97210-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.bridgeapp.com`</span><span class="sxs-lookup"><span data-stu-id="97210-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.bridgeapp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="97210-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="97210-161">These values are not real.</span></span> <span data-ttu-id="97210-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="97210-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="97210-163">Obraťte se na [tým podpory most klienta](https://community.bridgeapp.com/community/help) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="97210-163">Contact [Bridge Client support team](https://community.bridgeapp.com/community/help) to get these values.</span></span> 
 
4. <span data-ttu-id="97210-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="97210-164">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_certificate.png) 

5. <span data-ttu-id="97210-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97210-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="97210-168">Na **konfigurace mostu** klikněte na tlačítko **konfigurace mostu** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="97210-168">On the **Bridge Configuration** section, click **Configure Bridge** to open **Configure sign-on** window.</span></span> <span data-ttu-id="97210-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="97210-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_configure.png) 

7. <span data-ttu-id="97210-171">Konfigurace jednotného přihlašování na **most** straně, budete muset odeslat stažené **Certificate(Raw)** a **SAML Entity ID, SAML jeden přihlašování adresa URL služby a adresa URL Sign-Out** k [tým podpory most](https://community.bridgeapp.com/community/help).</span><span class="sxs-lookup"><span data-stu-id="97210-171">To configure single sign-on on **Bridge** side, you need to send the downloaded **Certificate(Raw)** and **SAML Entity ID, SAML Single Sign-On Service URL, and Sign-Out URL** to [Bridge support team](https://community.bridgeapp.com/community/help).</span></span> 

> [!TIP]
> <span data-ttu-id="97210-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="97210-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="97210-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="97210-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="97210-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="97210-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="97210-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="97210-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="97210-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97210-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="97210-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97210-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="97210-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="97210-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="97210-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="97210-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="97210-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97210-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="97210-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="97210-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="97210-187">a.</span><span class="sxs-lookup"><span data-stu-id="97210-187">a.</span></span> <span data-ttu-id="97210-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="97210-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="97210-189">b.</span><span class="sxs-lookup"><span data-stu-id="97210-189">b.</span></span> <span data-ttu-id="97210-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="97210-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="97210-191">c.</span><span class="sxs-lookup"><span data-stu-id="97210-191">c.</span></span> <span data-ttu-id="97210-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="97210-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="97210-193">d.</span><span class="sxs-lookup"><span data-stu-id="97210-193">d.</span></span> <span data-ttu-id="97210-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="97210-194">Click **Create**.</span></span>
 
### <a name="creating-a-bridge-test-user"></a><span data-ttu-id="97210-195">Vytvoření zkušebního uživatele most</span><span class="sxs-lookup"><span data-stu-id="97210-195">Creating a Bridge test user</span></span>

<span data-ttu-id="97210-196">V této části vytvoříte uživatele volal Britta Simon v most.</span><span class="sxs-lookup"><span data-stu-id="97210-196">In this section, you create a user called Britta Simon in Bridge.</span></span> <span data-ttu-id="97210-197">Práce s [tým podpory most klienta](https://community.bridgeapp.com/community/help) vytvoření uživatele v platformu.</span><span class="sxs-lookup"><span data-stu-id="97210-197">Work with [Bridge Client support team](https://community.bridgeapp.com/community/help) to create a user in the platform.</span></span> <span data-ttu-id="97210-198">Můžete zvýšit lístku podpory s most z <a href="https://community.bridgeapp.com/community/help">sem</a> přidat uživatele do mostu platformy.</span><span class="sxs-lookup"><span data-stu-id="97210-198">You can raise the support ticket with Bridge from <a href="https://community.bridgeapp.com/community/help">here</a> to add the users in the Bridge platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="97210-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="97210-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="97210-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu most.</span><span class="sxs-lookup"><span data-stu-id="97210-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Bridge.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="97210-202">**Pokud chcete přiřadit Britta Simon most, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97210-202">**To assign Britta Simon to Bridge, perform the following steps:**</span></span>

1. <span data-ttu-id="97210-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="97210-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="97210-205">V seznamu aplikací vyberte **most**.</span><span class="sxs-lookup"><span data-stu-id="97210-205">In the applications list, select **Bridge**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_app.png) 

3. <span data-ttu-id="97210-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="97210-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="97210-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97210-209">Click **Add** button.</span></span> <span data-ttu-id="97210-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97210-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="97210-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="97210-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="97210-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97210-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="97210-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97210-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="97210-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="97210-215">Testing single sign-on</span></span>

<span data-ttu-id="97210-216">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="97210-216">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="97210-217">Když kliknete na dlaždici mostu na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci most.</span><span class="sxs-lookup"><span data-stu-id="97210-217">When you click the Bridge tile in the Access Panel, you should get automatically signed-on to your Bridge application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97210-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="97210-218">Additional resources</span></span>

* [<span data-ttu-id="97210-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="97210-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="97210-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="97210-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_203.png
