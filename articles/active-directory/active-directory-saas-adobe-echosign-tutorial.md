---
title: "Kurz: Azure Active Directory integrace s Adobe přihlašovací | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Adobe přihlášení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: b413772de1af1fbb128d29b81e5831cfd6a39ab4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a><span data-ttu-id="b0e18-103">Kurz: Azure Active Directory integrace s Adobe přihlášení</span><span class="sxs-lookup"><span data-stu-id="b0e18-103">Tutorial: Azure Active Directory integration with Adobe Sign</span></span>

<span data-ttu-id="b0e18-104">V tomto kurzu zjistěte, jak integrovat Adobe přihlašovací službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b0e18-104">In this tutorial, you learn how to integrate Adobe Sign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b0e18-105">Integrace Adobe přihlášení s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b0e18-105">Integrating Adobe Sign with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b0e18-106">Můžete řídit ve službě Azure AD, který má přístup na Adobe přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0e18-106">You can control in Azure AD who has access to Adobe Sign</span></span>
- <span data-ttu-id="b0e18-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k přihlášení Adobe (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0e18-107">You can enable your users to automatically get signed-on to Adobe Sign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b0e18-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b0e18-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b0e18-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b0e18-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0e18-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b0e18-110">Prerequisites</span></span>

<span data-ttu-id="b0e18-111">Ke konfiguraci integrace služby Azure AD znakem Adobe, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b0e18-111">To configure Azure AD integration with Adobe Sign, you need the following items:</span></span>

- <span data-ttu-id="b0e18-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0e18-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b0e18-113">Přihlášení Adobe jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b0e18-113">An Adobe Sign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b0e18-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0e18-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b0e18-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b0e18-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b0e18-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b0e18-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b0e18-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b0e18-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b0e18-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b0e18-118">Scenario description</span></span>
<span data-ttu-id="b0e18-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0e18-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b0e18-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b0e18-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b0e18-121">Přidání Adobe přihlášení z Galerie</span><span class="sxs-lookup"><span data-stu-id="b0e18-121">Adding Adobe Sign from the gallery</span></span>
2. <span data-ttu-id="b0e18-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0e18-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-sign-from-the-gallery"></a><span data-ttu-id="b0e18-123">Přidání Adobe přihlášení z Galerie</span><span class="sxs-lookup"><span data-stu-id="b0e18-123">Adding Adobe Sign from the gallery</span></span>
<span data-ttu-id="b0e18-124">Při konfiguraci Integrace Adobe registrace do služby Azure AD, potřebujete přidat přihlašovací Adobe z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b0e18-124">To configure the integration of Adobe Sign into Azure AD, you need to add Adobe Sign from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b0e18-125">**Pokud chcete přidat přihlašovací Adobe z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b0e18-125">**To add Adobe Sign from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b0e18-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b0e18-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b0e18-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b0e18-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b0e18-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b0e18-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b0e18-133">Do vyhledávacího pole zadejte **Adobe přihlašovací**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-133">In the search box, type **Adobe Sign**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. <span data-ttu-id="b0e18-135">Na panelu výsledků vyberte **Adobe přihlašovací**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b0e18-135">In the results panel, select **Adobe Sign**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b0e18-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0e18-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b0e18-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování znakem Adobe podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b0e18-138">In this section, you configure and test Azure AD single sign-on with Adobe Sign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b0e18-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v Adobe přihlašování je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b0e18-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Adobe Sign is to a user in Azure AD.</span></span> <span data-ttu-id="b0e18-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Adobe přihlašovací musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b0e18-140">In other words, a link relationship between an Azure AD user and the related user in Adobe Sign needs to be established.</span></span>

<span data-ttu-id="b0e18-141">V Adobe přihlašovací přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="b0e18-141">In Adobe Sign, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b0e18-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Adobe přihlášení, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b0e18-142">To configure and test Azure AD single sign-on with Adobe Sign, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b0e18-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b0e18-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b0e18-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0e18-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b0e18-145">**[Vytváření testovacího uživatele přihlášení Adobe](#creating-an-adobe-sign-test-user)**  – Pokud chcete mít protějšek Britta Simon v Adobe znak, který je propojený s Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b0e18-145">**[Creating an Adobe Sign test user](#creating-an-adobe-sign-test-user)** - to have a counterpart of Britta Simon in Adobe Sign that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b0e18-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b0e18-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b0e18-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b0e18-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b0e18-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0e18-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b0e18-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Adobe přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b0e18-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Adobe Sign application.</span></span>

<span data-ttu-id="b0e18-150">**Ke konfiguraci Azure AD jednotné přihlašování znakem Adobe, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b0e18-150">**To configure Azure AD single sign-on with Adobe Sign, perform the following steps:**</span></span>

1. <span data-ttu-id="b0e18-151">Na portálu Azure na **Adobe přihlašovací** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-151">In the Azure portal, on the **Adobe Sign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b0e18-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b0e18-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. <span data-ttu-id="b0e18-155">Na **Adobe přihlašovací domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b0e18-155">On the **Adobe Sign Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    <span data-ttu-id="b0e18-157">a.</span><span class="sxs-lookup"><span data-stu-id="b0e18-157">a.</span></span> <span data-ttu-id="b0e18-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.echosign.com/`</span><span class="sxs-lookup"><span data-stu-id="b0e18-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.echosign.com/`</span></span>

    <span data-ttu-id="b0e18-159">b.</span><span class="sxs-lookup"><span data-stu-id="b0e18-159">b.</span></span> <span data-ttu-id="b0e18-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.echosign.com`</span><span class="sxs-lookup"><span data-stu-id="b0e18-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.echosign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b0e18-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b0e18-161">These values are not real.</span></span> <span data-ttu-id="b0e18-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b0e18-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b0e18-163">Obraťte se na [tým podpory Adobe přihlašovací klienta](https://helpx.adobe.com/in/contact/support.html) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="b0e18-163">Contact [Adobe Sign Client support team](https://helpx.adobe.com/in/contact/support.html) to get these values.</span></span> 
 
4. <span data-ttu-id="b0e18-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="b0e18-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. <span data-ttu-id="b0e18-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b0e18-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b0e18-168">Na **Adobe přihlašovací konfigurace** klikněte na tlačítko **nakonfigurovat přihlašovací Adobe** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b0e18-168">On the **Adobe Sign Configuration** section, click **Configure Adobe Sign** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b0e18-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b0e18-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 


7. <span data-ttu-id="b0e18-171">V okně prohlížeče jiný web Přihlaste se na váš web společnosti Adobe přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="b0e18-171">In a different web browser window, log in to your Adobe Sign company site as an administrator.</span></span>

8. <span data-ttu-id="b0e18-172">V nabídce v horní části, klikněte na tlačítko **účet**a klikněte v navigačním podokně na levé straně, **SAML nastavení** pod **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-172">In the menu on the top, click **Account**, and then, in the navigation pane on the left side, click **SAML Settings** under **Account Settings**.</span></span>
   
   <span data-ttu-id="b0e18-173">![Účet](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "účtu")</span><span class="sxs-lookup"><span data-stu-id="b0e18-173">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "Account")</span></span>

9. <span data-ttu-id="b0e18-174">V části Nastavení SAML proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b0e18-174">In the SAML Settings section, perform the following steps:</span></span>
   
   <span data-ttu-id="b0e18-175">![Nastavení SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="b0e18-175">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML Settings")</span></span>
   
   <span data-ttu-id="b0e18-176">a.</span><span class="sxs-lookup"><span data-stu-id="b0e18-176">a.</span></span> <span data-ttu-id="b0e18-177">Jako **SAML režimu**, vyberte **SAML povinné**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-177">As **SAML Mode**, select **SAML Mandatory**.</span></span>
   
   <span data-ttu-id="b0e18-178">b.</span><span class="sxs-lookup"><span data-stu-id="b0e18-178">b.</span></span> <span data-ttu-id="b0e18-179">Vyberte **povolit správci účtu EchoSign přihlásit pomocí svých přihlašovacích údajů EchoSign**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-179">Select **Allow EchoSign Account Administrators to log in using their EchoSign Credentials**.</span></span>
   
   <span data-ttu-id="b0e18-180">c.</span><span class="sxs-lookup"><span data-stu-id="b0e18-180">c.</span></span> <span data-ttu-id="b0e18-181">Jako **vytvoření uživatele**, vyberte **automaticky přidat uživatele ověřeného pomocí SAML**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-181">As **User Creation**, select **Automatically add users authenticated through SAML**.</span></span>

10. <span data-ttu-id="b0e18-182">Přesunete na následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="b0e18-182">Move on, performing the following steps:</span></span>

       <span data-ttu-id="b0e18-183">![Nastavení SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="b0e18-183">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML Settings")</span></span>

    <span data-ttu-id="b0e18-184">a.</span><span class="sxs-lookup"><span data-stu-id="b0e18-184">a.</span></span> <span data-ttu-id="b0e18-185">Vložení **SAML Entity ID**, který jste zkopírovali z portálu Azure do **IdP Entity ID** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b0e18-185">Paste **SAML Entity ID**, which you have copied from Azure portal into the **IdP Entity ID** textbox.</span></span>
    
    <span data-ttu-id="b0e18-186">b.</span><span class="sxs-lookup"><span data-stu-id="b0e18-186">b.</span></span> <span data-ttu-id="b0e18-187">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure do **IdP přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b0e18-187">Paste **SAML Single Sign-On Service URL**, which you have copied from Azure portal into the **IdP Login URL** textbox.</span></span>
   
    <span data-ttu-id="b0e18-188">c.</span><span class="sxs-lookup"><span data-stu-id="b0e18-188">c.</span></span> <span data-ttu-id="b0e18-189">Vložení **Sign-Out URL**, který jste zkopírovali z portálu Azure do **adresy URL odhlašovací IdP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b0e18-189">Paste **Sign-Out URL**, which you have copied from Azure portal into the **IdP Logout URL** textbox.</span></span>

    <span data-ttu-id="b0e18-190">d.</span><span class="sxs-lookup"><span data-stu-id="b0e18-190">d.</span></span> <span data-ttu-id="b0e18-191">Otevřete váš stažené **Certificate(Base64)** souboru v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **IdP certifikát** textbox</span><span class="sxs-lookup"><span data-stu-id="b0e18-191">Open your downloaded **Certificate(Base64)** file in notepad, copy the content of it into your clipboard, and then paste it to the **IdP Certificate** textbox</span></span>

    <span data-ttu-id="b0e18-192">e.</span><span class="sxs-lookup"><span data-stu-id="b0e18-192">e.</span></span> <span data-ttu-id="b0e18-193">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-193">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="b0e18-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b0e18-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b0e18-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b0e18-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b0e18-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b0e18-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b0e18-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0e18-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="b0e18-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0e18-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b0e18-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b0e18-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b0e18-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b0e18-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b0e18-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b0e18-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b0e18-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b0e18-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b0e18-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b0e18-209">a.</span><span class="sxs-lookup"><span data-stu-id="b0e18-209">a.</span></span> <span data-ttu-id="b0e18-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b0e18-211">b.</span><span class="sxs-lookup"><span data-stu-id="b0e18-211">b.</span></span> <span data-ttu-id="b0e18-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b0e18-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b0e18-213">c.</span><span class="sxs-lookup"><span data-stu-id="b0e18-213">c.</span></span> <span data-ttu-id="b0e18-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b0e18-215">d.</span><span class="sxs-lookup"><span data-stu-id="b0e18-215">d.</span></span> <span data-ttu-id="b0e18-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-216">Click **Create**.</span></span>
 
### <a name="creating-an-adobe-sign-test-user"></a><span data-ttu-id="b0e18-217">Vytváření testovacího uživatele přihlášení Adobe</span><span class="sxs-lookup"><span data-stu-id="b0e18-217">Creating an Adobe Sign test user</span></span>

<span data-ttu-id="b0e18-218">Pokud chcete povolit uživatelům Azure AD přihlášení na Adobe přihlašování, musí být zřízená do Adobe přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b0e18-218">To enable Azure AD users to log in to Adobe Sign, they must be provisioned into Adobe Sign.</span></span> <span data-ttu-id="b0e18-219">V případě Adobe přihlašovací zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b0e18-219">In the case of Adobe Sign, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="b0e18-220">Můžete použít jakékoli jiné Adobe přihlášení uživatele účtu vytvoření nástroje nebo rozhraní API poskytované Adobe přihlašovací zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b0e18-220">You can use any other Adobe Sign user account creation tools or APIs provided by Adobe Sign to provision AAD user accounts.</span></span> 

<span data-ttu-id="b0e18-221">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b0e18-221">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="b0e18-222">Přihlaste se k vaší **Adobe přihlašovací** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b0e18-222">Log in to your **Adobe Sign** company site as administrator.</span></span>

2. <span data-ttu-id="b0e18-223">V nabídce v horní části, klikněte na tlačítko **účet**a klikněte v navigačním podokně na levé straně, **uživatelé a skupiny**a potom klikněte na **vytvořte nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-223">In the menu on the top, click **Account**, and then, in the navigation pane on the left side, click **Users & Groups**, and then, click **Create a new user**.</span></span>
   
   <span data-ttu-id="b0e18-224">![Účet](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "účtu")</span><span class="sxs-lookup"><span data-stu-id="b0e18-224">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "Account")</span></span>
   
3. <span data-ttu-id="b0e18-225">V **vytvořit nového uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b0e18-225">In the **Create New User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="b0e18-226">![Vytvoření uživatele](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="b0e18-226">![Create User](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "Create User")</span></span>
   
   <span data-ttu-id="b0e18-227">a.</span><span class="sxs-lookup"><span data-stu-id="b0e18-227">a.</span></span> <span data-ttu-id="b0e18-228">Typ **e-mailovou adresu**, **křestní jméno**, a **příjmení** platného účtu AAD chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="b0e18-228">Type the **Email Address**, **First Name**, and **Last Name** of a valid AAD account you want to provision into the related textboxes.</span></span>
   
   <span data-ttu-id="b0e18-229">b.</span><span class="sxs-lookup"><span data-stu-id="b0e18-229">b.</span></span> <span data-ttu-id="b0e18-230">Klikněte na tlačítko **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-230">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="b0e18-231">Držitel účtu Azure Active Directory obdrží e-mail, který obsahuje odkaz pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="b0e18-231">The Azure Active Directory account holder receives an email that includes a link to confirm the account before it becomes active.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b0e18-232">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0e18-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b0e18-233">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Adobe přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b0e18-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Adobe Sign.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b0e18-235">**Přiřadit Britta Simon na Adobe přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b0e18-235">**To assign Britta Simon to Adobe Sign, perform the following steps:**</span></span>

1. <span data-ttu-id="b0e18-236">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b0e18-238">V seznamu aplikací vyberte **Adobe přihlašovací**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-238">In the applications list, select **Adobe Sign**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. <span data-ttu-id="b0e18-240">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b0e18-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b0e18-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b0e18-242">Click **Add** button.</span></span> <span data-ttu-id="b0e18-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b0e18-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b0e18-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b0e18-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b0e18-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b0e18-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b0e18-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b0e18-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b0e18-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0e18-248">Testing single sign-on</span></span>

<span data-ttu-id="b0e18-249">Když kliknete na dlaždici Adobe přihlašovací na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Adobe přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b0e18-249">When you click the Adobe Sign tile in the Access Panel, you should get automatically signed-on to your Adobe Sign application.</span></span>
<span data-ttu-id="b0e18-250">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b0e18-250">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0e18-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b0e18-251">Additional resources</span></span>

* [<span data-ttu-id="b0e18-252">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b0e18-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b0e18-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b0e18-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

