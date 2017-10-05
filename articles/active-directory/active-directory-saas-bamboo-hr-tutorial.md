---
title: 'Kurz: Azure Active Directory integrace s BambooHR | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a BambooHR."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: jeedes
ms.openlocfilehash: 06bf91b0e598fd3d8e644378efdb753611ee1ebc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a><span data-ttu-id="fb99b-103">Kurz: Azure Active Directory integrace s BambooHR</span><span class="sxs-lookup"><span data-stu-id="fb99b-103">Tutorial: Azure Active Directory integration with BambooHR</span></span>

<span data-ttu-id="fb99b-104">V tomto kurzu zjistěte, jak integrovat BambooHR s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fb99b-104">In this tutorial, you learn how to integrate BambooHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fb99b-105">Integrace BambooHR s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="fb99b-105">Integrating BambooHR with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fb99b-106">Můžete řídit ve službě Azure AD, který má přístup k BambooHR</span><span class="sxs-lookup"><span data-stu-id="fb99b-106">You can control in Azure AD who has access to BambooHR</span></span>
- <span data-ttu-id="fb99b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k BambooHR (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb99b-107">You can enable your users to automatically get signed-on to BambooHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fb99b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fb99b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fb99b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fb99b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb99b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fb99b-110">Prerequisites</span></span>

<span data-ttu-id="fb99b-111">Konfigurace integrace Azure AD s BambooHR, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="fb99b-111">To configure Azure AD integration with BambooHR, you need the following items:</span></span>

- <span data-ttu-id="fb99b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb99b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fb99b-113">BambooHR jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="fb99b-113">A BambooHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fb99b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fb99b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fb99b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="fb99b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fb99b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="fb99b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fb99b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fb99b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fb99b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="fb99b-118">Scenario description</span></span>
<span data-ttu-id="fb99b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fb99b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fb99b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="fb99b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fb99b-121">Přidání BambooHR z Galerie</span><span class="sxs-lookup"><span data-stu-id="fb99b-121">Adding BambooHR from the gallery</span></span>
2. <span data-ttu-id="fb99b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fb99b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bamboohr-from-the-gallery"></a><span data-ttu-id="fb99b-123">Přidání BambooHR z Galerie</span><span class="sxs-lookup"><span data-stu-id="fb99b-123">Adding BambooHR from the gallery</span></span>
<span data-ttu-id="fb99b-124">Při konfiguraci integrace BambooHR do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS BambooHR z galerie.</span><span class="sxs-lookup"><span data-stu-id="fb99b-124">To configure the integration of BambooHR into Azure AD, you need to add BambooHR from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fb99b-125">**Pokud chcete přidat BambooHR z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fb99b-125">**To add BambooHR from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fb99b-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fb99b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fb99b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fb99b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="fb99b-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fb99b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="fb99b-133">Do vyhledávacího pole zadejte **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-133">In the search box, type **BambooHR**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_search.png)

5. <span data-ttu-id="fb99b-135">Na panelu výsledků vyberte **BambooHR**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fb99b-135">In the results panel, select **BambooHR**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fb99b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fb99b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fb99b-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BambooHR podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="fb99b-138">In this section, you configure and test Azure AD single sign-on with BambooHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="fb99b-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v BambooHR je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fb99b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BambooHR is to a user in Azure AD.</span></span> <span data-ttu-id="fb99b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v BambooHR musí navázat.</span><span class="sxs-lookup"><span data-stu-id="fb99b-140">In other words, a link relationship between an Azure AD user and the related user in BambooHR needs to be established.</span></span>

<span data-ttu-id="fb99b-141">V BambooHR, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="fb99b-141">In BambooHR, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fb99b-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s BambooHR, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="fb99b-142">To configure and test Azure AD single sign-on with BambooHR, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fb99b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="fb99b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fb99b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fb99b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fb99b-145">**[Vytvoření zkušebního uživatele BambooHR](#creating-a-bamboohr-test-user)**  – Pokud chcete mít protějšek Britta Simon v BambooHR propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="fb99b-145">**[Creating a BambooHR test user](#creating-a-bamboohr-test-user)** - to have a counterpart of Britta Simon in BambooHR that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fb99b-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fb99b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fb99b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fb99b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fb99b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fb99b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fb99b-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci BambooHR.</span><span class="sxs-lookup"><span data-stu-id="fb99b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BambooHR application.</span></span>

<span data-ttu-id="fb99b-150">**Ke konfiguraci Azure AD jednotné přihlašování s BambooHR, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fb99b-150">**To configure Azure AD single sign-on with BambooHR, perform the following steps:**</span></span>

1. <span data-ttu-id="fb99b-151">Na portálu Azure na **BambooHR** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-151">In the Azure portal, on the **BambooHR** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="fb99b-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fb99b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. <span data-ttu-id="fb99b-155">Na **BambooHR domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fb99b-155">On the **BambooHR Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    <span data-ttu-id="fb99b-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company>.bamboohr.com`</span><span class="sxs-lookup"><span data-stu-id="fb99b-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.bamboohr.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="fb99b-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="fb99b-158">This value is not real.</span></span> <span data-ttu-id="fb99b-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fb99b-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="fb99b-160">Obraťte se na [tým podpory BambooHR klienta](https://www.bamboohr.com/contact.php) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fb99b-160">Contact [BambooHR Client support team](https://www.bamboohr.com/contact.php) to get this value.</span></span> 
 
4. <span data-ttu-id="fb99b-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="fb99b-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. <span data-ttu-id="fb99b-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fb99b-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fb99b-165">Na **BambooHR konfigurace** klikněte na tlačítko **konfigurace BambooHR** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="fb99b-165">On the **BambooHR Configuration** section, click **Configure BambooHR** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fb99b-166">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="fb99b-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

6. <span data-ttu-id="fb99b-168">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti BambooHR.</span><span class="sxs-lookup"><span data-stu-id="fb99b-168">In a different web browser window, log into your BambooHR company site as an administrator.</span></span>

7. <span data-ttu-id="fb99b-169">Na domovské stránce proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fb99b-169">On the homepage, perform the following steps:</span></span>
   
    <span data-ttu-id="fb99b-170">![Jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="fb99b-170">![Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "Single Sign-On")</span></span>   

    <span data-ttu-id="fb99b-171">a.</span><span class="sxs-lookup"><span data-stu-id="fb99b-171">a.</span></span> <span data-ttu-id="fb99b-172">Klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-172">Click **Apps**.</span></span>
   
    <span data-ttu-id="fb99b-173">b.</span><span class="sxs-lookup"><span data-stu-id="fb99b-173">b.</span></span> <span data-ttu-id="fb99b-174">V nabídce aplikace na levé straně klikněte na tlačítko **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-174">In the apps menu on the left, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="fb99b-175">c.</span><span class="sxs-lookup"><span data-stu-id="fb99b-175">c.</span></span> <span data-ttu-id="fb99b-176">Klikněte na tlačítko **SAML Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-176">Click **SAML Single Sign-On**.</span></span>

8. <span data-ttu-id="fb99b-177">V **SAML Single Sign-On** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fb99b-177">In the **SAML Single Sign-On** section, perform the following steps:</span></span>
   
    <span data-ttu-id="fb99b-178">![SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="fb99b-178">![SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML Single Sign-On")</span></span>
   
    <span data-ttu-id="fb99b-179">a.</span><span class="sxs-lookup"><span data-stu-id="fb99b-179">a.</span></span> <span data-ttu-id="fb99b-180">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu do **adresu Url pro přihlášení SSO** textové pole.</span><span class="sxs-lookup"><span data-stu-id="fb99b-180">Paste the **SAML Single Sign-On Service URL** value into the **SSO Login Url** textbox.</span></span>
      
    <span data-ttu-id="fb99b-181">b.</span><span class="sxs-lookup"><span data-stu-id="fb99b-181">b.</span></span> <span data-ttu-id="fb99b-182">Otevřete kódování base-64 kódovaného certifikátu si stáhli z portálu Azure v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **certifikát X.509** textbox</span><span class="sxs-lookup"><span data-stu-id="fb99b-182">Open base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox</span></span>
   
    <span data-ttu-id="fb99b-183">c.</span><span class="sxs-lookup"><span data-stu-id="fb99b-183">c.</span></span> <span data-ttu-id="fb99b-184">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="fb99b-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="fb99b-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fb99b-186">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="fb99b-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fb99b-187">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fb99b-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fb99b-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb99b-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="fb99b-189">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fb99b-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="fb99b-191">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fb99b-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fb99b-192">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fb99b-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fb99b-194">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fb99b-196">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fb99b-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fb99b-198">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fb99b-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fb99b-200">a.</span><span class="sxs-lookup"><span data-stu-id="fb99b-200">a.</span></span> <span data-ttu-id="fb99b-201">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fb99b-202">b.</span><span class="sxs-lookup"><span data-stu-id="fb99b-202">b.</span></span> <span data-ttu-id="fb99b-203">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fb99b-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fb99b-204">c.</span><span class="sxs-lookup"><span data-stu-id="fb99b-204">c.</span></span> <span data-ttu-id="fb99b-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fb99b-206">d.</span><span class="sxs-lookup"><span data-stu-id="fb99b-206">d.</span></span> <span data-ttu-id="fb99b-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-207">Click **Create**.</span></span>
 
### <a name="creating-a-bamboohr-test-user"></a><span data-ttu-id="fb99b-208">Vytvoření zkušebního uživatele BambooHR</span><span class="sxs-lookup"><span data-stu-id="fb99b-208">Creating a BambooHR test user</span></span>

<span data-ttu-id="fb99b-209">Pokud chcete povolit uživatelům Azure AD přihlášení k BambooHR, musí být zřízená do BambooHR.</span><span class="sxs-lookup"><span data-stu-id="fb99b-209">To enable Azure AD users to log in to BambooHR, they must be provisioned into BambooHR.</span></span>  

<span data-ttu-id="fb99b-210">V případě BambooHR zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="fb99b-210">In the case of BambooHR, provisioning is a manual task.</span></span>

<span data-ttu-id="fb99b-211">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fb99b-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="fb99b-212">Přihlaste se k vaší **BambooHR** lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="fb99b-212">Log in to your **BambooHR** site as administrator.</span></span>

2. <span data-ttu-id="fb99b-213">Na panelu nástrojů v horní části klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-213">In the toolbar on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="fb99b-214">![Nastavení](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="fb99b-214">![Setting](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "Setting")</span></span>

3. <span data-ttu-id="fb99b-215">Klikněte na tlačítko **přehled**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-215">Click **Overview**.</span></span>

4. <span data-ttu-id="fb99b-216">V levém navigačním podokně, přejděte do **zabezpečení \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-216">In the left navigation pane, go to **Security \> Users**.</span></span>

5. <span data-ttu-id="fb99b-217">Zadejte uživatelské jméno, heslo a e-mailovou adresu platného účtu AAD, který má být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="fb99b-217">Type the user name, password, and email address of a valid AAD account you want to provision into the related textboxes.</span></span>

6. <span data-ttu-id="fb99b-218">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-218">Click **Save**.</span></span>
        
>[!NOTE]
><span data-ttu-id="fb99b-219">Můžete použít všechny ostatní BambooHR uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované BambooHR zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="fb99b-219">You can use any other BambooHR user account creation tools or APIs provided by BambooHR to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fb99b-220">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb99b-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fb99b-221">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu BambooHR.</span><span class="sxs-lookup"><span data-stu-id="fb99b-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BambooHR.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="fb99b-223">**Pokud chcete přiřadit Britta Simon BambooHR, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fb99b-223">**To assign Britta Simon to BambooHR, perform the following steps:**</span></span>

1. <span data-ttu-id="fb99b-224">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="fb99b-226">V seznamu aplikací vyberte **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-226">In the applications list, select **BambooHR**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png) 

3. <span data-ttu-id="fb99b-228">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="fb99b-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="fb99b-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fb99b-230">Click **Add** button.</span></span> <span data-ttu-id="fb99b-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fb99b-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="fb99b-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="fb99b-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fb99b-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fb99b-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fb99b-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fb99b-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fb99b-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fb99b-236">Testing single sign-on</span></span>

<span data-ttu-id="fb99b-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="fb99b-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fb99b-238">Když kliknete na dlaždici BambooHR na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci BambooHR.</span><span class="sxs-lookup"><span data-stu-id="fb99b-238">When you click the BambooHR tile in the Access Panel, you should get automatically signed-on to your BambooHR application.</span></span>
<span data-ttu-id="fb99b-239">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fb99b-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fb99b-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fb99b-240">Additional resources</span></span>

* [<span data-ttu-id="fb99b-241">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fb99b-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fb99b-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fb99b-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

