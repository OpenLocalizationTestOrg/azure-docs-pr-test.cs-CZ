---
title: 'Kurz: Azure Active Directory integrace s Citrix GoToMeeting | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7d7897f6-b88e-4dd5-8f3a-e612337b1413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: c1ac144c4fa43312ec26fce03cd0ee1bfcf73d4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a><span data-ttu-id="e2ace-103">Kurz: Azure Active Directory integrace s Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="e2ace-103">Tutorial: Azure Active Directory integration with Citrix GoToMeeting</span></span>

<span data-ttu-id="e2ace-104">V tomto kurzu zjistěte, jak integrovat Citrix GoToMeeting s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e2ace-104">In this tutorial, you learn how to integrate Citrix GoToMeeting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e2ace-105">Integrace Citrix GoToMeeting s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e2ace-105">Integrating Citrix GoToMeeting with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e2ace-106">Můžete řídit ve službě Azure AD, který má přístup k Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="e2ace-106">You can control in Azure AD who has access to Citrix GoToMeeting</span></span>
- <span data-ttu-id="e2ace-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k systému Citrix GoToMeeting (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2ace-107">You can enable your users to automatically get signed-on to Citrix GoToMeeting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e2ace-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e2ace-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e2ace-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e2ace-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2ace-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e2ace-110">Prerequisites</span></span>

<span data-ttu-id="e2ace-111">Konfigurace integrace Azure AD s Citrix GoToMeeting, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e2ace-111">To configure Azure AD integration with Citrix GoToMeeting, you need the following items:</span></span>

- <span data-ttu-id="e2ace-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2ace-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e2ace-113">Citrix GoToMeeting jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e2ace-113">A Citrix GoToMeeting single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e2ace-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e2ace-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e2ace-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e2ace-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e2ace-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e2ace-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e2ace-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e2ace-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e2ace-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e2ace-118">Scenario description</span></span>
<span data-ttu-id="e2ace-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e2ace-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e2ace-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e2ace-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e2ace-121">Přidání Citrix GoToMeeting z Galerie</span><span class="sxs-lookup"><span data-stu-id="e2ace-121">Adding Citrix GoToMeeting from the gallery</span></span>
2. <span data-ttu-id="e2ace-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e2ace-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-citrix-gotomeeting-from-the-gallery"></a><span data-ttu-id="e2ace-123">Přidání Citrix GoToMeeting z Galerie</span><span class="sxs-lookup"><span data-stu-id="e2ace-123">Adding Citrix GoToMeeting from the gallery</span></span>
<span data-ttu-id="e2ace-124">Při konfiguraci integrace Citrix GoToMeeting do služby Azure AD potřebujete přidat Citrix GoToMeeting z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e2ace-124">To configure the integration of Citrix GoToMeeting into Azure AD, you need to add Citrix GoToMeeting from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e2ace-125">**Pokud chcete přidat Citrix GoToMeeting z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e2ace-125">**To add Citrix GoToMeeting from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e2ace-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e2ace-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e2ace-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e2ace-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e2ace-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e2ace-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e2ace-133">Do vyhledávacího pole zadejte **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-133">In the search box, type **Citrix GoToMeeting**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_search.png)

5. <span data-ttu-id="e2ace-135">Na panelu výsledků vyberte **Citrix GoToMeeting**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2ace-135">In the results panel, select **Citrix GoToMeeting**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e2ace-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e2ace-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e2ace-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s Citrix GoToMeeting podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="e2ace-138">In this section, you configure and test Azure AD single sign-on with Citrix GoToMeeting based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e2ace-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Citrix GoToMeeting je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e2ace-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Citrix GoToMeeting is to a user in Azure AD.</span></span> <span data-ttu-id="e2ace-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Citrix GoToMeeting musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e2ace-140">In other words, a link relationship between an Azure AD user and the related user in Citrix GoToMeeting needs to be established.</span></span>

<span data-ttu-id="e2ace-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="e2ace-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Citrix GoToMeeting.</span></span>

<span data-ttu-id="e2ace-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Citrix GoToMeeting, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e2ace-142">To configure and test Azure AD single sign-on with Citrix GoToMeeting, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e2ace-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e2ace-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e2ace-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e2ace-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e2ace-145">**[Vytvoření zkušebního uživatele Citrix GoToMeeting](#creating-a-citrix-gotomeeting-test-user)**  – Pokud chcete mít protějšek Britta Simon v Citrix GoToMeeting, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e2ace-145">**[Creating a Citrix GoToMeeting test user](#creating-a-citrix-gotomeeting-test-user)** - to have a counterpart of Britta Simon in Citrix GoToMeeting that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e2ace-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e2ace-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e2ace-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e2ace-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e2ace-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e2ace-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e2ace-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="e2ace-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Citrix GoToMeeting application.</span></span>

<span data-ttu-id="e2ace-150">**Ke konfiguraci Azure AD jednotné přihlašování s Citrix GoToMeeting, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e2ace-150">**To configure Azure AD single sign-on with Citrix GoToMeeting, perform the following steps:**</span></span>

1. <span data-ttu-id="e2ace-151">Na portálu Azure na **Citrix GoToMeeting** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-151">In the Azure portal, on the **Citrix GoToMeeting** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e2ace-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e2ace-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_samlbase.png)

3. <span data-ttu-id="e2ace-155">Na **Citrix GoToMeeting domény a adresy URL** část, není nutné provádět žádné kroky.</span><span class="sxs-lookup"><span data-stu-id="e2ace-155">On the **Citrix GoToMeeting Domain and URLs** section, no need to perform any steps.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_url.png)


3. <span data-ttu-id="e2ace-157">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="e2ace-157">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_certificate.png) 

4. <span data-ttu-id="e2ace-159">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e2ace-159">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_400.png)
    
5. <span data-ttu-id="e2ace-161">V části Konfigurace SAML GoToMeeting Citrix klikněte na tlačítko Konfigurovat Citrix GoToMeeting SAML konfigurovat přihlašování okno otevřete.</span><span class="sxs-lookup"><span data-stu-id="e2ace-161">On the Citrix GoToMeeting SAML Configuration section, click Configure Citrix GoToMeeting SAML to open Configure sign-on window.</span></span> <span data-ttu-id="e2ace-162">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e2ace-162">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

6. <span data-ttu-id="e2ace-163">V okně jiný prohlížeč, přihlaste se k vaší [Citrix organizace Center](https://account.citrixonline.com/organization/administration/).</span><span class="sxs-lookup"><span data-stu-id="e2ace-163">In a different browser window, log in to your [Citrix Organization Center](https://account.citrixonline.com/organization/administration/).</span></span>

7. <span data-ttu-id="e2ace-164">Klikněte **zprostředkovatele Identity** kartě a pak proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e2ace-164">Click the **Identity Provider** tab, and then perform the following steps:</span></span>  
   
    <span data-ttu-id="e2ace-165">![Instalační program SAML](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "instalace SAML")</span><span class="sxs-lookup"><span data-stu-id="e2ace-165">![SAML setup](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML setup")</span></span>
   
    <span data-ttu-id="e2ace-166">a.</span><span class="sxs-lookup"><span data-stu-id="e2ace-166">a.</span></span> <span data-ttu-id="e2ace-167">Vyberte **ruční**</span><span class="sxs-lookup"><span data-stu-id="e2ace-167">Select **Manual**</span></span>

    <span data-ttu-id="e2ace-168">b.</span><span class="sxs-lookup"><span data-stu-id="e2ace-168">b.</span></span> <span data-ttu-id="e2ace-169">Na portálu Azure na **nakonfigurovat jednotné přihlašování v Citrix GoToMeeting** dialogové okno stránky, kopie **SAML jeden přihlašování adresa URL služby** hodnotu a vložte ji do **přihlašovací adresa URL stránky** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e2ace-169">In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialog page, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **Sign-in page URL** textbox.</span></span> 

    <span data-ttu-id="e2ace-170">c.</span><span class="sxs-lookup"><span data-stu-id="e2ace-170">c.</span></span> <span data-ttu-id="e2ace-171">Na portálu Azure na **nakonfigurovat jednotné přihlašování v Citrix GoToMeeting** dialogové okno stránky, kopie **Sign-Out URL** hodnotu a vložte ji do **adresy URL odhlašovací stránky** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e2ace-171">In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialog page, copy the **Sign-Out URL** value, and then paste it into the **Sign-out page URL** textbox.</span></span>

    <span data-ttu-id="e2ace-172">d.</span><span class="sxs-lookup"><span data-stu-id="e2ace-172">d.</span></span> <span data-ttu-id="e2ace-173">Na portálu Azure na **nakonfigurovat jednotné přihlašování v Citrix GoToMeeting** dialogové okno stránky, kopie **SAML Entity ID** hodnotu a vložte ji do **ID Entity zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e2ace-173">In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialog page, copy the **SAML Entity ID** value, and then paste it into the **Identity Provider Entity ID** textbox.</span></span>

    <span data-ttu-id="e2ace-174">e.</span><span class="sxs-lookup"><span data-stu-id="e2ace-174">e.</span></span> <span data-ttu-id="e2ace-175">Chcete-li nahrát stažený certifikát, klikněte na tlačítko **nahrát certifikát**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-175">To upload your downloaded certificate, click **Upload Certificate**.</span></span>

    <span data-ttu-id="e2ace-176">f.</span><span class="sxs-lookup"><span data-stu-id="e2ace-176">f.</span></span> <span data-ttu-id="e2ace-177">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="e2ace-178">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e2ace-178">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e2ace-179">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e2ace-179">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e2ace-180">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e2ace-180">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e2ace-181">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2ace-181">Creating an Azure AD test user</span></span>
<span data-ttu-id="e2ace-182">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e2ace-182">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e2ace-184">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e2ace-184">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e2ace-185">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e2ace-185">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e2ace-187">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-187">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e2ace-189">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e2ace-189">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e2ace-191">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e2ace-191">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e2ace-193">a.</span><span class="sxs-lookup"><span data-stu-id="e2ace-193">a.</span></span> <span data-ttu-id="e2ace-194">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-194">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e2ace-195">b.</span><span class="sxs-lookup"><span data-stu-id="e2ace-195">b.</span></span> <span data-ttu-id="e2ace-196">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e2ace-196">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e2ace-197">c.</span><span class="sxs-lookup"><span data-stu-id="e2ace-197">c.</span></span> <span data-ttu-id="e2ace-198">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-198">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e2ace-199">d.</span><span class="sxs-lookup"><span data-stu-id="e2ace-199">d.</span></span> <span data-ttu-id="e2ace-200">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-200">Click **Create**.</span></span>
 
### <a name="creating-a-citrix-gotomeeting-test-user"></a><span data-ttu-id="e2ace-201">Vytvoření zkušebního uživatele Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="e2ace-201">Creating a Citrix GoToMeeting test user</span></span>

<span data-ttu-id="e2ace-202">V této části se uživatel volá Britta Simon vytvoří v Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="e2ace-202">In this section, a user called Britta Simon is created in Citrix GoToMeeting.</span></span> <span data-ttu-id="e2ace-203">Citrix GoToMeeting podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="e2ace-203">Citrix GoToMeeting supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="e2ace-204">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="e2ace-204">There is no action item for you in this section.</span></span> <span data-ttu-id="e2ace-205">Pokud uživatel v Citrix GoToMeeting ještě neexistuje, vytvoří se nový při pokusu o přístup k Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="e2ace-205">If a user doesn't already exist in Citrix GoToMeeting, a new one is created when you attempt to access Citrix GoToMeeting.</span></span>

>[!Note]
><span data-ttu-id="e2ace-206">Pokud je potřeba ručně vytvořit uživatele, obraťte se na [tým podpory Citrix GoToMeeting](https://care.citrixonline.com/gotomeeting)</span><span class="sxs-lookup"><span data-stu-id="e2ace-206">If you need to create a user manually, Contact [Citrix GoToMeeting support team](https://care.citrixonline.com/gotomeeting)</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e2ace-207">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2ace-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e2ace-208">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="e2ace-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Citrix GoToMeeting.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e2ace-210">**Pokud chcete přiřadit Britta Simon Citrix GoToMeeting, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e2ace-210">**To assign Britta Simon to Citrix GoToMeeting, perform the following steps:**</span></span>

1. <span data-ttu-id="e2ace-211">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e2ace-213">V seznamu aplikací vyberte **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-213">In the applications list, select **Citrix GoToMeeting**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_app.png) 

3. <span data-ttu-id="e2ace-215">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e2ace-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e2ace-217">Click **Add** button.</span></span> <span data-ttu-id="e2ace-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e2ace-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e2ace-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e2ace-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e2ace-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e2ace-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e2ace-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e2ace-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e2ace-223">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e2ace-223">Testing single sign-on</span></span>

<span data-ttu-id="e2ace-224">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e2ace-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e2ace-225">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="e2ace-225">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="e2ace-226">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e2ace-226">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2ace-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e2ace-227">Additional resources</span></span>

* [<span data-ttu-id="e2ace-228">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e2ace-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e2ace-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e2ace-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="e2ace-230">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="e2ace-230">Configure User Provisioning</span></span>](active-directory-saas-citrixgotomeeting-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_203.png

