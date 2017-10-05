---
title: 'Kurz: Azure Active Directory integrace s Igloo softwarem | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Igloo softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ab3891e11eb33b4d233e4fc967a40c7df06e4f4e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a><span data-ttu-id="22807-103">Kurz: Azure Active Directory integrace s Igloo softwaru</span><span class="sxs-lookup"><span data-stu-id="22807-103">Tutorial: Azure Active Directory integration with Igloo Software</span></span>

<span data-ttu-id="22807-104">V tomto kurzu zjistěte, jak integrovat Igloo softwaru s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="22807-104">In this tutorial, you learn how to integrate Igloo Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="22807-105">Integrace Igloo softwaru s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="22807-105">Integrating Igloo Software with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="22807-106">Můžete řídit ve službě Azure AD, který má přístup k softwaru Igloo</span><span class="sxs-lookup"><span data-stu-id="22807-106">You can control in Azure AD who has access to Igloo Software</span></span>
- <span data-ttu-id="22807-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k softwaru Igloo (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="22807-107">You can enable your users to automatically get signed-on to Igloo Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="22807-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="22807-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="22807-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="22807-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22807-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="22807-110">Prerequisites</span></span>

<span data-ttu-id="22807-111">Konfigurace integrace Azure AD s Igloo softwaru, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="22807-111">To configure Azure AD integration with Igloo Software, you need the following items:</span></span>

- <span data-ttu-id="22807-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="22807-112">An Azure AD subscription</span></span>
- <span data-ttu-id="22807-113">Softwaru Igloo jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="22807-113">An Igloo Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="22807-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="22807-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="22807-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="22807-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="22807-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="22807-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="22807-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="22807-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="22807-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="22807-118">Scenario description</span></span>
<span data-ttu-id="22807-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="22807-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="22807-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="22807-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="22807-121">Přidání softwaru Igloo z Galerie</span><span class="sxs-lookup"><span data-stu-id="22807-121">Adding Igloo Software from the gallery</span></span>
2. <span data-ttu-id="22807-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="22807-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-igloo-software-from-the-gallery"></a><span data-ttu-id="22807-123">Přidání softwaru Igloo z Galerie</span><span class="sxs-lookup"><span data-stu-id="22807-123">Adding Igloo Software from the gallery</span></span>
<span data-ttu-id="22807-124">Při konfiguraci integrace Igloo softwaru do služby Azure AD, musíte přidat Igloo Software z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="22807-124">To configure the integration of Igloo Software into Azure AD, you need to add Igloo Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="22807-125">**Pokud chcete přidat Igloo softwaru z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="22807-125">**To add Igloo Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="22807-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="22807-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="22807-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="22807-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="22807-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="22807-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="22807-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22807-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="22807-133">Do vyhledávacího pole zadejte **Igloo softwaru**.</span><span class="sxs-lookup"><span data-stu-id="22807-133">In the search box, type **Igloo Software**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. <span data-ttu-id="22807-135">Na panelu výsledků vyberte **Igloo softwaru**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="22807-135">In the results panel, select **Igloo Software**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="22807-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="22807-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="22807-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Igloo Software založený na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="22807-138">In this section, you configure and test Azure AD single sign-on with Igloo Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="22807-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Igloo softwaru je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22807-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Igloo Software is to a user in Azure AD.</span></span> <span data-ttu-id="22807-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Igloo softwaru je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="22807-140">In other words, a link relationship between an Azure AD user and the related user in Igloo Software needs to be established.</span></span>

<span data-ttu-id="22807-141">V softwaru Igloo přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="22807-141">In Igloo Software, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="22807-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Igloo softwaru, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="22807-142">To configure and test Azure AD single sign-on with Igloo Software, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="22807-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="22807-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="22807-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22807-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="22807-145">**[Vytváření testovacího uživatele softwaru Igloo](#creating-an-igloo-software-test-user)**  – Pokud chcete mít protějšek Britta Simon v Igloo Software, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="22807-145">**[Creating an Igloo Software test user](#creating-an-igloo-software-test-user)** - to have a counterpart of Britta Simon in Igloo Software that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="22807-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="22807-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="22807-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="22807-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="22807-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="22807-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="22807-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Igloo softwaru.</span><span class="sxs-lookup"><span data-stu-id="22807-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Igloo Software application.</span></span>

<span data-ttu-id="22807-150">**Ke konfiguraci Azure AD jednotné přihlašování s Igloo softwarem, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="22807-150">**To configure Azure AD single sign-on with Igloo Software, perform the following steps:**</span></span>

1. <span data-ttu-id="22807-151">Na portálu Azure na **Igloo softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="22807-151">In the Azure portal, on the **Igloo Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="22807-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="22807-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. <span data-ttu-id="22807-155">Na **Igloo softwaru domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="22807-155">On the **Igloo Software Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    <span data-ttu-id="22807-157">a.</span><span class="sxs-lookup"><span data-stu-id="22807-157">a.</span></span> <span data-ttu-id="22807-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.igloocommmunities.com`</span><span class="sxs-lookup"><span data-stu-id="22807-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.igloocommmunities.com`</span></span>

    <span data-ttu-id="22807-159">b.</span><span class="sxs-lookup"><span data-stu-id="22807-159">b.</span></span> <span data-ttu-id="22807-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="22807-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    <span data-ttu-id="22807-161">c.</span><span class="sxs-lookup"><span data-stu-id="22807-161">c.</span></span> <span data-ttu-id="22807-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="22807-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="22807-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="22807-163">These values are not real.</span></span> <span data-ttu-id="22807-164">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="22807-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="22807-165">Obraťte se na [tým podpory klientský Software Igloo](https://www.igloosoftware.com/services/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="22807-165">Contact [Igloo Software Client support team](https://www.igloosoftware.com/services/support) to get these values.</span></span> 

4. <span data-ttu-id="22807-166">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="22807-166">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. <span data-ttu-id="22807-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22807-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="22807-170">Na **Igloo softwarové konfigurace** klikněte na tlačítko **konfigurace softwaru Igloo** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="22807-170">On the **Igloo Software Configuration** section, click **Configure Igloo Software** to open **Configure sign-on** window.</span></span> <span data-ttu-id="22807-171">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="22807-171">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. <span data-ttu-id="22807-173">V okně prohlížeče jiný web Přihlaste se na váš web společnosti Igloo softwaru jako správce.</span><span class="sxs-lookup"><span data-stu-id="22807-173">In a different web browser window, log in to your Igloo Software company site as an administrator.</span></span>

8. <span data-ttu-id="22807-174">Přejděte na **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="22807-174">Go to the **Control Panel**.</span></span>
   
     <span data-ttu-id="22807-175">![Ovládací panely](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "ovládací panely")</span><span class="sxs-lookup"><span data-stu-id="22807-175">![Control Panel](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Control Panel")</span></span>

9. <span data-ttu-id="22807-176">V **členství** , klikněte na **přihlášení v nastavení**.</span><span class="sxs-lookup"><span data-stu-id="22807-176">In the **Membership** tab, click **Sign In Settings**.</span></span>
   
    <span data-ttu-id="22807-177">![Přihlaste se nastavení](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "přihlášení nastavení")</span><span class="sxs-lookup"><span data-stu-id="22807-177">![Sign in Settings](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "Sign in Settings")</span></span>

10. <span data-ttu-id="22807-178">V části Konfigurace SAML, klikněte na tlačítko **konfigurace ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="22807-178">In the SAML Configuration section, click **Configure SAML Authentication**.</span></span>
   
    <span data-ttu-id="22807-179">![Konfigurace SAML](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="22807-179">![SAML Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML Configuration")</span></span>
   
11. <span data-ttu-id="22807-180">V **obecné konfigurace** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="22807-180">In the **General Configuration** section, perform the following steps:</span></span>
   
    <span data-ttu-id="22807-181">![Obecná konfigurace](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "obecné konfigurace")</span><span class="sxs-lookup"><span data-stu-id="22807-181">![General Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "General Configuration")</span></span>

    <span data-ttu-id="22807-182">a.</span><span class="sxs-lookup"><span data-stu-id="22807-182">a.</span></span> <span data-ttu-id="22807-183">V **název připojení** textovému poli, napište vlastní název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="22807-183">In the **Connection Name** textbox, type a custom name for your configuration.</span></span>
   
    <span data-ttu-id="22807-184">b.</span><span class="sxs-lookup"><span data-stu-id="22807-184">b.</span></span> <span data-ttu-id="22807-185">V **IdP přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22807-185">In the **IdP Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="22807-186">c.</span><span class="sxs-lookup"><span data-stu-id="22807-186">c.</span></span> <span data-ttu-id="22807-187">V **adresy URL odhlašovací IdP** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22807-187">In the **IdP Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="22807-188">d.</span><span class="sxs-lookup"><span data-stu-id="22807-188">d.</span></span> <span data-ttu-id="22807-189">Vyberte **odhlášení odpovědi a typ požadavku HTTP** jako **POST**.</span><span class="sxs-lookup"><span data-stu-id="22807-189">Select **Logout Response and Request HTTP Type** as **POST**.</span></span>
   
    <span data-ttu-id="22807-190">e.</span><span class="sxs-lookup"><span data-stu-id="22807-190">e.</span></span> <span data-ttu-id="22807-191">Otevřete váš **kódování base-64** kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, zkopírujte obsah ho do schránky a vložte jej do **veřejný certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="22807-191">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **Public Certificate** textbox.</span></span>
    
12. <span data-ttu-id="22807-192">V **odpovědi a konfiguraci ověřování**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="22807-192">In the **Response and Authentication Configuration**, perform the following steps:</span></span>
    
    <span data-ttu-id="22807-193">![Odpověď a konfiguraci ověřování](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "odpovědi a konfiguraci ověřování")</span><span class="sxs-lookup"><span data-stu-id="22807-193">![Response and Authentication Configuration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Response and Authentication Configuration")</span></span>
  
      <span data-ttu-id="22807-194">a.</span><span class="sxs-lookup"><span data-stu-id="22807-194">a.</span></span> <span data-ttu-id="22807-195">Jako **zprostředkovatele Identity**, vyberte **Microsoft ADFS**.</span><span class="sxs-lookup"><span data-stu-id="22807-195">As **Identity Provider**, select **Microsoft ADFS**.</span></span>
      
      <span data-ttu-id="22807-196">b.</span><span class="sxs-lookup"><span data-stu-id="22807-196">b.</span></span> <span data-ttu-id="22807-197">Jako **typ identifikátoru**, vyberte **e-mailovou adresu**.</span><span class="sxs-lookup"><span data-stu-id="22807-197">As **Identifier Type**, select **Email Address**.</span></span> 

      <span data-ttu-id="22807-198">c.</span><span class="sxs-lookup"><span data-stu-id="22807-198">c.</span></span> <span data-ttu-id="22807-199">V **atribut e-mailu** textovému poli, typ **emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="22807-199">In the **Email Attribute** textbox, type **emailaddress**.</span></span>

      <span data-ttu-id="22807-200">d.</span><span class="sxs-lookup"><span data-stu-id="22807-200">d.</span></span> <span data-ttu-id="22807-201">V **křestní jméno atribut** textovému poli, typ **givenname**.</span><span class="sxs-lookup"><span data-stu-id="22807-201">In the **First Name Attribute** textbox, type **givenname**.</span></span>

      <span data-ttu-id="22807-202">e.</span><span class="sxs-lookup"><span data-stu-id="22807-202">e.</span></span> <span data-ttu-id="22807-203">V **poslední atribut Name** textovému poli, typ **Přezdívka**.</span><span class="sxs-lookup"><span data-stu-id="22807-203">In the **Last Name Attribute** textbox, type **surname**.</span></span>

13. <span data-ttu-id="22807-204">Proveďte následující kroky k dokončení konfigurace:</span><span class="sxs-lookup"><span data-stu-id="22807-204">Perform the following steps to complete the configuration:</span></span>
    
    <span data-ttu-id="22807-205">![Vytvoření uživatele na přihlašovací](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "vytvoření uživatele na přihlášení")</span><span class="sxs-lookup"><span data-stu-id="22807-205">![User creation on Sign in](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "User creation on Sign in")</span></span> 

     <span data-ttu-id="22807-206">a.</span><span class="sxs-lookup"><span data-stu-id="22807-206">a.</span></span> <span data-ttu-id="22807-207">Jako **vytvoření uživatele na přihlašovací**, vyberte **vytvořte nového uživatele ve vaší lokalitě při přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="22807-207">As **User creation on Sign in**, select **Create a new user in your site when they sign in**.</span></span>

     <span data-ttu-id="22807-208">b.</span><span class="sxs-lookup"><span data-stu-id="22807-208">b.</span></span> <span data-ttu-id="22807-209">Jako **přihlášení nastavení**, vyberte **SAML použijte tlačítko na obrazovce "Přihlásit"**.</span><span class="sxs-lookup"><span data-stu-id="22807-209">As **Sign in Settings**, select **Use SAML button on “Sign in” screen**.</span></span>

     <span data-ttu-id="22807-210">c.</span><span class="sxs-lookup"><span data-stu-id="22807-210">c.</span></span> <span data-ttu-id="22807-211">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="22807-211">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="22807-212">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="22807-212">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="22807-213">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="22807-213">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="22807-214">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="22807-214">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="22807-215">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="22807-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="22807-216">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22807-216">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="22807-218">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="22807-218">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="22807-219">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="22807-219">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="22807-221">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="22807-221">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="22807-223">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22807-223">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="22807-225">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="22807-225">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="22807-227">a.</span><span class="sxs-lookup"><span data-stu-id="22807-227">a.</span></span> <span data-ttu-id="22807-228">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="22807-228">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="22807-229">b.</span><span class="sxs-lookup"><span data-stu-id="22807-229">b.</span></span> <span data-ttu-id="22807-230">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="22807-230">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="22807-231">c.</span><span class="sxs-lookup"><span data-stu-id="22807-231">c.</span></span> <span data-ttu-id="22807-232">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="22807-232">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="22807-233">d.</span><span class="sxs-lookup"><span data-stu-id="22807-233">d.</span></span> <span data-ttu-id="22807-234">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="22807-234">Click **Create**.</span></span>
 
### <a name="creating-an-igloo-software-test-user"></a><span data-ttu-id="22807-235">Vytváření testovacího uživatele Igloo softwaru</span><span class="sxs-lookup"><span data-stu-id="22807-235">Creating an Igloo Software test user</span></span>

<span data-ttu-id="22807-236">Neexistuje žádná položka akce můžete nakonfigurovat software Igloo zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="22807-236">There is no action item for you to configure user provisioning to Igloo Software.</span></span>  

<span data-ttu-id="22807-237">Když přiřazený uživatel se pokusí přihlásit k Igloo Software pomocí přístupového panelu, Igloo softwaru ověří, zda uživatel existuje.</span><span class="sxs-lookup"><span data-stu-id="22807-237">When an assigned user tries to log in to Igloo Software using the access panel, Igloo Software checks whether the user exists.</span></span>  <span data-ttu-id="22807-238">Pokud neexistuje žádný účet k dispozici dosud, se automaticky vytvoří Igloo softwarem.</span><span class="sxs-lookup"><span data-stu-id="22807-238">If there is no user account available yet, it is automatically created by Igloo Software.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="22807-239">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="22807-239">Assigning the Azure AD test user</span></span>

<span data-ttu-id="22807-240">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Igloo softwaru.</span><span class="sxs-lookup"><span data-stu-id="22807-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Igloo Software.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="22807-242">**Pokud chcete přiřadit Britta Simon Igloo softwaru, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="22807-242">**To assign Britta Simon to Igloo Software, perform the following steps:**</span></span>

1. <span data-ttu-id="22807-243">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="22807-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="22807-245">V seznamu aplikací vyberte **Igloo softwaru**.</span><span class="sxs-lookup"><span data-stu-id="22807-245">In the applications list, select **Igloo Software**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. <span data-ttu-id="22807-247">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="22807-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="22807-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22807-249">Click **Add** button.</span></span> <span data-ttu-id="22807-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22807-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="22807-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="22807-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="22807-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22807-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="22807-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22807-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="22807-255">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="22807-255">Testing single sign-on</span></span>

<span data-ttu-id="22807-256">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="22807-256">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="22807-257">Když kliknete na dlaždici Igloo softwaru na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Igloo softwaru.</span><span class="sxs-lookup"><span data-stu-id="22807-257">When you click the Igloo Software tile in the Access Panel, you should get automatically signed-on to your Igloo Software application.</span></span>
<span data-ttu-id="22807-258">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="22807-258">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="22807-259">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="22807-259">Additional resources</span></span>

* [<span data-ttu-id="22807-260">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="22807-260">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="22807-261">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="22807-261">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

