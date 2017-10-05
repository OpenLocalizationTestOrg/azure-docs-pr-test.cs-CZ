---
title: 'Kurz: Azure Active Directory integrace se serverem Tableau | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Tableau serveru."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 6b35609d88fbbf649e15863901d521886db2a4d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a><span data-ttu-id="8a0d1-103">Kurz: Azure Active Directory integrace se serverem Tableau</span><span class="sxs-lookup"><span data-stu-id="8a0d1-103">Tutorial: Azure Active Directory integration with Tableau Server</span></span>

<span data-ttu-id="8a0d1-104">V tomto kurzu zjistěte, jak integrovat Tableau serveru se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8a0d1-104">In this tutorial, you learn how to integrate Tableau Server with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8a0d1-105">Integrace serveru Tableau s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8a0d1-105">Integrating Tableau Server with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8a0d1-106">Můžete řídit ve službě Azure AD, který má přístup k serveru Tableau</span><span class="sxs-lookup"><span data-stu-id="8a0d1-106">You can control in Azure AD who has access to Tableau Server</span></span>
- <span data-ttu-id="8a0d1-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k serveru Tableau (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a0d1-107">You can enable your users to automatically get signed-on to Tableau Server (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8a0d1-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8a0d1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8a0d1-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8a0d1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a0d1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8a0d1-110">Prerequisites</span></span>

<span data-ttu-id="8a0d1-111">Ke konfiguraci integrace služby Azure AD se serverem Tableau, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="8a0d1-111">To configure Azure AD integration with Tableau Server, you need the following items:</span></span>

- <span data-ttu-id="8a0d1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a0d1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8a0d1-113">Serveru Tableau jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8a0d1-113">A Tableau Server single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8a0d1-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8a0d1-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8a0d1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8a0d1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8a0d1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8a0d1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8a0d1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8a0d1-118">Scenario description</span></span>
<span data-ttu-id="8a0d1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8a0d1-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8a0d1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8a0d1-121">Přidání serveru Tableau z Galerie</span><span class="sxs-lookup"><span data-stu-id="8a0d1-121">Adding Tableau Server from the gallery</span></span>
2. <span data-ttu-id="8a0d1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a0d1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-server-from-the-gallery"></a><span data-ttu-id="8a0d1-123">Přidání serveru Tableau z Galerie</span><span class="sxs-lookup"><span data-stu-id="8a0d1-123">Adding Tableau Server from the gallery</span></span>
<span data-ttu-id="8a0d1-124">Při konfiguraci integrace Tableau serveru do Azure AD, potřebujete přidat Tableau Server z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-124">To configure the integration of Tableau Server into Azure AD, you need to add Tableau Server from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8a0d1-125">**Chcete-li přidat Tableau Server z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8a0d1-125">**To add Tableau Server from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8a0d1-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8a0d1-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8a0d1-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8a0d1-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8a0d1-133">Do vyhledávacího pole zadejte **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-133">In the search box, type **Tableau Server**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. <span data-ttu-id="8a0d1-135">Na panelu výsledků vyberte **Tableau Server**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-135">In the results panel, select **Tableau Server**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8a0d1-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a0d1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8a0d1-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tableau serveru podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="8a0d1-138">In this section, you configure and test Azure AD single sign-on with Tableau Server based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8a0d1-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšku Tableau serveru je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tableau Server is to a user in Azure AD.</span></span> <span data-ttu-id="8a0d1-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské Tableau serveru je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-140">In other words, a link relationship between an Azure AD user and the related user in Tableau Server needs to be established.</span></span>

<span data-ttu-id="8a0d1-141">V serveru Tableau přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-141">In Tableau Server, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8a0d1-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tableau serveru, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="8a0d1-142">To configure and test Azure AD single sign-on with Tableau Server, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8a0d1-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8a0d1-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8a0d1-145">**[Vytvoření zkušebního uživatele serveru Tableau](#creating-a-tableau-server-test-user)**  – Pokud chcete mít protějšek Britta Simon v Tableau Server, který je připojen k reprezentaci Azure AD uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-145">**[Creating a Tableau Server test user](#creating-a-tableau-server-test-user)** - to have a counterpart of Britta Simon in Tableau Server that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8a0d1-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8a0d1-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8a0d1-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a0d1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8a0d1-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Tableau serveru.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tableau Server application.</span></span>

<span data-ttu-id="8a0d1-150">**Ke konfiguraci Azure AD jednotné přihlašování k serveru Tableau, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8a0d1-150">**To configure Azure AD single sign-on with Tableau Server, perform the following steps:**</span></span>

1. <span data-ttu-id="8a0d1-151">Na portálu Azure na **Tableau Server** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-151">In the Azure portal, on the **Tableau Server** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8a0d1-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. <span data-ttu-id="8a0d1-155">Na **Tableau Server domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8a0d1-155">On the **Tableau Server Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    <span data-ttu-id="8a0d1-157">a.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-157">a.</span></span> <span data-ttu-id="8a0d1-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="8a0d1-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://azure.<domain name>.link`</span></span>
    
    <span data-ttu-id="8a0d1-159">b.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-159">b.</span></span> <span data-ttu-id="8a0d1-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="8a0d1-160">In the **Identifier** textbox, type a URL using the following pattern: `https://azure.<domain name>.link`</span></span>

    <span data-ttu-id="8a0d1-161">c.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-161">c.</span></span> <span data-ttu-id="8a0d1-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://azure.<domain name>.link/wg/saml/SSO/index.html`</span><span class="sxs-lookup"><span data-stu-id="8a0d1-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://azure.<domain name>.link/wg/saml/SSO/index.html`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="8a0d1-163">Předchozí hodnoty nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-163">The preceding values are not real values.</span></span> <span data-ttu-id="8a0d1-164">Později aktualizujte hodnoty s skutečná adresa URL a identifikátor ze stránky konfigurace serveru Tableau.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-164">Later, you update the values with the actual URL and identifier from the Tableau Server configuration page.</span></span> 

4. <span data-ttu-id="8a0d1-165">Aplikace serveru tableau očekává, že SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-165">Tableau Server application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="8a0d1-166">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-166">Configure the following claims for this application.</span></span> <span data-ttu-id="8a0d1-167">Můžete spravovat hodnoty těchto atributů z **"Atributy uživatele"** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-167">You can manage the values of these attributes from the **"User Attributes"** section on application integration page.</span></span> <span data-ttu-id="8a0d1-168">Následující snímek obrazovky ukazuje příklad pro stejné.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-168">The following screenshot shows an example for the same.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. <span data-ttu-id="8a0d1-170">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8a0d1-170">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="8a0d1-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="8a0d1-171">Attribute Name</span></span> | <span data-ttu-id="8a0d1-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="8a0d1-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="8a0d1-173">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="8a0d1-173">username</span></span> | <span data-ttu-id="8a0d1-174">*User.DisplayName*</span><span class="sxs-lookup"><span data-stu-id="8a0d1-174">*user.displayname*</span></span> |

    <span data-ttu-id="8a0d1-175">a.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-175">a.</span></span> <span data-ttu-id="8a0d1-176">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    <span data-ttu-id="8a0d1-179">b.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-179">b.</span></span> <span data-ttu-id="8a0d1-180">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="8a0d1-181">c.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-181">c.</span></span> <span data-ttu-id="8a0d1-182">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-182">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="8a0d1-183">d.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-183">d.</span></span> <span data-ttu-id="8a0d1-184">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="8a0d1-184">Click **Ok**</span></span>


6. <span data-ttu-id="8a0d1-185">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-185">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. <span data-ttu-id="8a0d1-187">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-187">Click **Save** button.</span></span>

    <span data-ttu-id="8a0d1-188">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="8a0d1-188">![Configure Single Sign-On](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span></span>
8. <span data-ttu-id="8a0d1-189">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte k přihlašování ke klientovi Tableau serveru jako správce.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-189">To get SSO configured for your application, you need to sign-on to your Tableau Server tenant as an administrator.</span></span>
   
   <span data-ttu-id="8a0d1-190">a.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-190">a.</span></span> <span data-ttu-id="8a0d1-191">V konfiguraci serveru Tableau, klikněte na **SAML** kartě.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-191">In the Tableau Server configuration, click the **SAML** tab.</span></span>
  
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   <span data-ttu-id="8a0d1-193">b.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-193">b.</span></span> <span data-ttu-id="8a0d1-194">Zaškrtněte políčko z **pomocí SAML pro jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-194">Select the checkbox of **Use SAML for single sign-on**.</span></span>
   
   <span data-ttu-id="8a0d1-195">c.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-195">c.</span></span> <span data-ttu-id="8a0d1-196">Adresa URL odpovědi ze serveru tableau – adresa URL, který Tableau Server uživatelé budou například http://tableau_server.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-196">Tableau Server return URL—The URL that Tableau Server users will be accessing, such as http://tableau_server.</span></span> <span data-ttu-id="8a0d1-197">Se nedoporučuje používat http://localhost.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-197">Using http://localhost is not recommended.</span></span> <span data-ttu-id="8a0d1-198">Použijete adresu URL s koncové lomítko (například http://tableau_server/) není podporována.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-198">Using a URL with a trailing slash (for example, http://tableau_server/) is not supported.</span></span> <span data-ttu-id="8a0d1-199">Kopírování **Tableau Server návratovou adresu URL** a vložte jej do služby Azure AD **přihlašovací adresa URL** textového pole v **Tableau Server domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-199">Copy **Tableau Server return URL** and paste it to Azure AD **Sign On URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="8a0d1-200">d.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-200">d.</span></span> <span data-ttu-id="8a0d1-201">SAML entity ID – entity ID jednoznačně identifikuje instalaci serveru Tableau do rozšíření IdP.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-201">SAML entity ID—The entity ID uniquely identifies your Tableau Server installation to the IdP.</span></span> <span data-ttu-id="8a0d1-202">Můžete zadat vaše adresa URL serveru Tableau znovu sem, pokud chcete, ale nemusí být adresa URL serveru Tableau.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-202">You can enter your Tableau Server URL again here, if you like, but it does not have to be your Tableau Server URL.</span></span> <span data-ttu-id="8a0d1-203">Kopírování **SAML entity ID** a vložte jej do služby Azure AD **identifikátor** textového pole v **Tableau Server domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-203">Copy **SAML entity ID** and paste it to Azure AD **Identifier** textbox in **Tableau Server Domain and URLs** section.</span></span>
     
   <span data-ttu-id="8a0d1-204">e.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-204">e.</span></span> <span data-ttu-id="8a0d1-205">Klikněte **exportovat soubor metadat** a otevře ji v aplikaci textový editor.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-205">Click the **Export Metadata File** and open it in the text editor application.</span></span> <span data-ttu-id="8a0d1-206">Vyhledejte adresu URL služby příjemce kontrolní výraz s Http Post a Index 0 a zkopírujte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-206">Locate Assertion Consumer Service URL with Http Post and Index 0 and copy the URL.</span></span> <span data-ttu-id="8a0d1-207">Nyní vložte jej do služby Azure AD **adresa URL odpovědi** textového pole v **Tableau Server domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-207">Now paste it to Azure AD **Reply URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="8a0d1-208">f.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-208">f.</span></span> <span data-ttu-id="8a0d1-209">Vyhledejte soubor federačních metadat stáhli z portálu Azure a nahrajte ji v **soubor metadat SAML Idp**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-209">Locate your Federation Metadata file downloaded from Azure portal, and then upload it in the **SAML Idp metadata file**.</span></span>
   
   <span data-ttu-id="8a0d1-210">g.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-210">g.</span></span> <span data-ttu-id="8a0d1-211">Klikněte **OK** tlačítka na stránce konfigurace serveru Tableau.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-211">Click the **OK** button in the Tableau Server Configuration page.</span></span>
   
    >[!NOTE] 
    ><span data-ttu-id="8a0d1-212">Zákazník muset nahrát některý z certifikátů v konfiguraci jednotné přihlašování SAML Tableau serveru a bude získat ignorován v toku jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-212">Customer have to upload any certificate in the Tableau Server SAML SSO configuration and it will get ignored in the SSO flow.</span></span>
    ><span data-ttu-id="8a0d1-213">Pokud jste třeba pomoci konfigurace SAML na serveru Tableau pak naleznete v tomto článku [konfigurace SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span><span class="sxs-lookup"><span data-stu-id="8a0d1-213">If you need help configuring SAML on Tableau Server then please refer to this article [Configure SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span></span>
    >
<CE>

> [!TIP]
> <span data-ttu-id="8a0d1-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="8a0d1-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8a0d1-215">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8a0d1-216">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8a0d1-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8a0d1-217">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a0d1-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="8a0d1-218">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8a0d1-220">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8a0d1-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8a0d1-221">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8a0d1-223">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8a0d1-225">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8a0d1-227">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8a0d1-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8a0d1-229">a.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-229">a.</span></span> <span data-ttu-id="8a0d1-230">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8a0d1-231">b.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-231">b.</span></span> <span data-ttu-id="8a0d1-232">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8a0d1-233">c.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-233">c.</span></span> <span data-ttu-id="8a0d1-234">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8a0d1-235">d.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-235">d.</span></span> <span data-ttu-id="8a0d1-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-236">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-server-test-user"></a><span data-ttu-id="8a0d1-237">Vytvoření zkušebního uživatele Tableau serveru</span><span class="sxs-lookup"><span data-stu-id="8a0d1-237">Creating a Tableau Server test user</span></span>

<span data-ttu-id="8a0d1-238">Cílem této části je vytvoření uživatele volat Britta Simon Tableau serveru.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-238">The objective of this section is to create a user called Britta Simon in Tableau Server.</span></span> <span data-ttu-id="8a0d1-239">Budete muset zřídit všechny uživatele v serveru Tableau.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-239">You need to provision all the users in the Tableau server.</span></span> 

<span data-ttu-id="8a0d1-240">Shodovat s tímto uživatelským jménem uživatele, hodnotu, která jste nakonfigurovali v Azure AD vlastní atribut **uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-240">That username of the user should match the value which you have configured in the Azure AD custom attribute of **username**.</span></span> <span data-ttu-id="8a0d1-241">Pomocí správné mapování by měla fungovat integrace [konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="8a0d1-241">With the correct mapping the integration should work [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="8a0d1-242">Pokud je potřeba ručně vytvořit uživateli, budete muset obrátit na správce serveru Tableau ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-242">If you need to create a user manually, you need to contact the Tableau Server administrator in your organization.</span></span>
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8a0d1-243">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a0d1-243">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8a0d1-244">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k serveru Tableau.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tableau Server.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8a0d1-246">**Britta Simon přiřadit k serveru Tableau, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8a0d1-246">**To assign Britta Simon to Tableau Server, perform the following steps:**</span></span>

1. <span data-ttu-id="8a0d1-247">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8a0d1-249">V seznamu aplikací vyberte **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-249">In the applications list, select **Tableau Server**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. <span data-ttu-id="8a0d1-251">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8a0d1-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-253">Click **Add** button.</span></span> <span data-ttu-id="8a0d1-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8a0d1-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8a0d1-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8a0d1-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8a0d1-259">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a0d1-259">Testing single sign-on</span></span>

<span data-ttu-id="8a0d1-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8a0d1-261">Když kliknete na dlaždici Tableau serveru na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Tableau serveru.</span><span class="sxs-lookup"><span data-stu-id="8a0d1-261">When you click the Tableau Server tile in the Access Panel, you should get automatically signed-on to your Tableau Server application.</span></span>
<span data-ttu-id="8a0d1-262">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="8a0d1-262">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8a0d1-263">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8a0d1-263">Additional resources</span></span>

* [<span data-ttu-id="8a0d1-264">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8a0d1-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8a0d1-265">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8a0d1-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

