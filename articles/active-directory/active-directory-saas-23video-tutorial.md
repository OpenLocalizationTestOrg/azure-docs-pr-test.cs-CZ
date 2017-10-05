---
title: 'Kurz: Azure Active Directory integrace s 23 Video | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a 23 Video."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e73dd1d-3995-4a73-b9cf-1b2318d49cb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: ffcd665506c21e25c84367af5b6a3afb8e319af7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-23-video"></a><span data-ttu-id="5cdaa-103">Kurz: Azure Active Directory integrace s 23 Video</span><span class="sxs-lookup"><span data-stu-id="5cdaa-103">Tutorial: Azure Active Directory integration with 23 Video</span></span>

<span data-ttu-id="5cdaa-104">V tomto kurzu zjistěte, jak integrovat 23 Video s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5cdaa-104">In this tutorial, you learn how to integrate 23 Video with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5cdaa-105">Integrace 23 Video s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5cdaa-105">Integrating 23 Video with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5cdaa-106">Můžete řídit ve službě Azure AD, který má přístup k 23 Video</span><span class="sxs-lookup"><span data-stu-id="5cdaa-106">You can control in Azure AD who has access to 23 Video</span></span>
- <span data-ttu-id="5cdaa-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k 23 Video (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cdaa-107">You can enable your users to automatically get signed-on to 23 Video (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5cdaa-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5cdaa-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5cdaa-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5cdaa-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5cdaa-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5cdaa-110">Prerequisites</span></span>

<span data-ttu-id="5cdaa-111">Konfigurace integrace Azure AD s 23 Video, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5cdaa-111">To configure Azure AD integration with 23 Video, you need the following items:</span></span>

- <span data-ttu-id="5cdaa-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cdaa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5cdaa-113">Předplatné povolené 23 Video jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cdaa-113">A 23 Video single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5cdaa-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5cdaa-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5cdaa-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5cdaa-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5cdaa-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5cdaa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5cdaa-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5cdaa-118">Scenario description</span></span>
<span data-ttu-id="5cdaa-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5cdaa-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5cdaa-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5cdaa-121">Přidání 23 Video z Galerie</span><span class="sxs-lookup"><span data-stu-id="5cdaa-121">Adding 23 Video from the gallery</span></span>
2. <span data-ttu-id="5cdaa-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cdaa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-23-video-from-the-gallery"></a><span data-ttu-id="5cdaa-123">Přidání 23 Video z Galerie</span><span class="sxs-lookup"><span data-stu-id="5cdaa-123">Adding 23 Video from the gallery</span></span>
<span data-ttu-id="5cdaa-124">Při konfiguraci integrace 23 videa do služby Azure AD, potřebujete přidat 23 Video z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-124">To configure the integration of 23 Video into Azure AD, you need to add 23 Video from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5cdaa-125">**Pokud chcete přidat 23 Video z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5cdaa-125">**To add 23 Video from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5cdaa-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5cdaa-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5cdaa-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5cdaa-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5cdaa-133">Do vyhledávacího pole zadejte **23 Video**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-133">In the search box, type **23 Video**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/tutorial_23video_search.png)

5. <span data-ttu-id="5cdaa-135">Na panelu výsledků vyberte **23 Video**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-135">In the results panel, select **23 Video**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/tutorial_23video_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5cdaa-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cdaa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5cdaa-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 23 Video podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5cdaa-138">In this section, you configure and test Azure AD single sign-on with 23 Video based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5cdaa-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v 23 Video je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 23 Video is to a user in Azure AD.</span></span> <span data-ttu-id="5cdaa-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské ve 23 Video musí navázat.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-140">In other words, a link relationship between an Azure AD user and the related user in 23 Video needs to be established.</span></span>

<span data-ttu-id="5cdaa-141">V 23 Video přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-141">In 23 Video, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5cdaa-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s 23 Video, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5cdaa-142">To configure and test Azure AD single sign-on with 23 Video, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5cdaa-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5cdaa-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5cdaa-145">**[Vytvoření zkušebního uživatele 23 Video](#creating-a-23-video-test-user)**  – Pokud chcete mít protějšek Britta Simon ve 23 Video, které je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-145">**[Creating a 23 Video test user](#creating-a-23-video-test-user)** - to have a counterpart of Britta Simon in 23 Video that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5cdaa-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5cdaa-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5cdaa-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cdaa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5cdaa-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci 23 Video.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 23 Video application.</span></span>

<span data-ttu-id="5cdaa-150">**Ke konfiguraci Azure AD jednotné přihlašování s 23 Video, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5cdaa-150">**To configure Azure AD single sign-on with 23 Video, perform the following steps:**</span></span>

1. <span data-ttu-id="5cdaa-151">Na portálu Azure na **23 Video** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-151">In the Azure portal, on the **23 Video** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5cdaa-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_samlbase.png)

3. <span data-ttu-id="5cdaa-155">Na **23 Video domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5cdaa-155">On the **23 Video Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_url.png)

    <span data-ttu-id="5cdaa-157">a.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-157">a.</span></span> <span data-ttu-id="5cdaa-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.23video.com`</span><span class="sxs-lookup"><span data-stu-id="5cdaa-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.23video.com`</span></span>

    <span data-ttu-id="5cdaa-159">b.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-159">b.</span></span> <span data-ttu-id="5cdaa-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.23video.com/saml/trust/<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="5cdaa-160">In the **Identifier** textbox, type a URL using the following pattern: `https://www.23video.com/saml/trust/<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5cdaa-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-161">These values are not real.</span></span> <span data-ttu-id="5cdaa-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5cdaa-163">Obraťte se na [23 tým podpory Video klienta](mailto:support@23company.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-163">Contact [23 Video Client support team](mailto:support@23company.com) to get these values.</span></span> 
 
4. <span data-ttu-id="5cdaa-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_certificate.png) 

5. <span data-ttu-id="5cdaa-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5cdaa-168">Na **23 konfigurace Video** klikněte na tlačítko **konfigurace Video 23** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-168">On the **23 Video Configuration** section, click **Configure 23 Video** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5cdaa-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5cdaa-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_configure.png) 

7. <span data-ttu-id="5cdaa-171">Konfigurace jednotného přihlašování na **23 Video** straně, budete muset odeslat stažené **certifikátu (Base64)**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [23 tým podpory Video](mailto:support@23company.com).</span><span class="sxs-lookup"><span data-stu-id="5cdaa-171">To configure single sign-on on **23 Video** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [23 Video support team](mailto:support@23company.com).</span></span> 


> [!TIP]
> <span data-ttu-id="5cdaa-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5cdaa-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5cdaa-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5cdaa-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5cdaa-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5cdaa-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cdaa-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="5cdaa-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5cdaa-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5cdaa-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5cdaa-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5cdaa-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5cdaa-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5cdaa-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5cdaa-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5cdaa-187">a.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-187">a.</span></span> <span data-ttu-id="5cdaa-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5cdaa-189">b.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-189">b.</span></span> <span data-ttu-id="5cdaa-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5cdaa-191">c.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-191">c.</span></span> <span data-ttu-id="5cdaa-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5cdaa-193">d.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-193">d.</span></span> <span data-ttu-id="5cdaa-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-194">Click **Create**.</span></span>
 
### <a name="creating-a-23-video-test-user"></a><span data-ttu-id="5cdaa-195">Vytvoření zkušebního uživatele 23 Video</span><span class="sxs-lookup"><span data-stu-id="5cdaa-195">Creating a 23 Video test user</span></span>

<span data-ttu-id="5cdaa-196">Cílem této části je vytvoření uživatele volal Britta Simon v 23 Video.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-196">The objective of this section is to create a user called Britta Simon in 23 Video.</span></span>

<span data-ttu-id="5cdaa-197">**Pokud chcete vytvořit uživateli volat Britta Simon ve 23 Video, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5cdaa-197">**To create a user called Britta Simon in 23 Video, perform the following steps:**</span></span>

1. <span data-ttu-id="5cdaa-198">Přihlásit se k webu společnosti 23 Video jako správce.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-198">Sign on to your 23 Video company site as administrator.</span></span>

2. <span data-ttu-id="5cdaa-199">Přejděte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-199">Go to **Settings**.</span></span>
 
3. <span data-ttu-id="5cdaa-200">V **uživatelé** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-200">In **Users** section, click **Configure**.</span></span>
   
    ![Přiřadit uživatele][400]

4. <span data-ttu-id="5cdaa-202">Klikněte na tlačítko **přidat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-202">Click **Add a new user**.</span></span> 
   
    ![Přiřadit uživatele][401]

5. <span data-ttu-id="5cdaa-204">V **žádost o připojení k této lokalitě** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5cdaa-204">In the **Invite someone to join this site** section, perform the following steps:</span></span>
   
    ![Přiřadit uživatele][402]

    <span data-ttu-id="5cdaa-206">a.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-206">a.</span></span> <span data-ttu-id="5cdaa-207">V **e-mailové adresy** textovému poli, zadejte Britta Simon e-mailovou adresu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-207">In the **E-mail addresses** textbox, type Britta Simon's email address in Azure AD.</span></span>  
 
    <span data-ttu-id="5cdaa-208">b.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-208">b.</span></span> <span data-ttu-id="5cdaa-209">Klikněte na tlačítko **přidejte uživatele**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-209">Click **Add the user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5cdaa-210">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cdaa-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5cdaa-211">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu 23 Video.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 23 Video.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5cdaa-213">**Pokud chcete přiřadit Britta Simon 23 Video, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5cdaa-213">**To assign Britta Simon to 23 Video, perform the following steps:**</span></span>

1. <span data-ttu-id="5cdaa-214">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5cdaa-216">V seznamu aplikací vyberte **23 Video**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-216">In the applications list, select **23 Video**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_app.png) 

3. <span data-ttu-id="5cdaa-218">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5cdaa-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-220">Click **Add** button.</span></span> <span data-ttu-id="5cdaa-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5cdaa-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5cdaa-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5cdaa-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5cdaa-226">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cdaa-226">Testing single sign-on</span></span>

<span data-ttu-id="5cdaa-227">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="5cdaa-228">Když kliknete na dlaždici 23 Video na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci 23 Video.</span><span class="sxs-lookup"><span data-stu-id="5cdaa-228">When you click the 23 Video tile in the Access Panel, you should get automatically signed-on to your 23 Video application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5cdaa-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5cdaa-229">Additional resources</span></span>

* [<span data-ttu-id="5cdaa-230">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5cdaa-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5cdaa-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5cdaa-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png
