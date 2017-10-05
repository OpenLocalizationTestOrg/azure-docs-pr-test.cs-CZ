---
title: 'Kurz: Azure Active Directory integrace s Boomi | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Boomi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 1121d22beddf73fd2109a4b410422f76dd37478e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a><span data-ttu-id="a78f1-103">Kurz: Azure Active Directory integrace s Boomi</span><span class="sxs-lookup"><span data-stu-id="a78f1-103">Tutorial: Azure Active Directory integration with Boomi</span></span>

<span data-ttu-id="a78f1-104">V tomto kurzu zjistěte, jak integrovat Boomi s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a78f1-104">In this tutorial, you learn how to integrate Boomi with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a78f1-105">Integrace Boomi s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a78f1-105">Integrating Boomi with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a78f1-106">Můžete řídit ve službě Azure AD, který má přístup k Boomi</span><span class="sxs-lookup"><span data-stu-id="a78f1-106">You can control in Azure AD who has access to Boomi</span></span>
- <span data-ttu-id="a78f1-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Boomi (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a78f1-107">You can enable your users to automatically get signed-on to Boomi (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a78f1-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a78f1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a78f1-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a78f1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a78f1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a78f1-110">Prerequisites</span></span>

<span data-ttu-id="a78f1-111">Konfigurace integrace Azure AD s Boomi, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a78f1-111">To configure Azure AD integration with Boomi, you need the following items:</span></span>

- <span data-ttu-id="a78f1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a78f1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a78f1-113">Boomi jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a78f1-113">A Boomi single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a78f1-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a78f1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a78f1-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a78f1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a78f1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a78f1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a78f1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a78f1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a78f1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a78f1-118">Scenario description</span></span>
<span data-ttu-id="a78f1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a78f1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a78f1-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a78f1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a78f1-121">Přidání Boomi z Galerie</span><span class="sxs-lookup"><span data-stu-id="a78f1-121">Adding Boomi from the gallery</span></span>
2. <span data-ttu-id="a78f1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a78f1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-boomi-from-the-gallery"></a><span data-ttu-id="a78f1-123">Přidání Boomi z Galerie</span><span class="sxs-lookup"><span data-stu-id="a78f1-123">Adding Boomi from the gallery</span></span>
<span data-ttu-id="a78f1-124">Při konfiguraci integrace Boomi do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Boomi z galerie.</span><span class="sxs-lookup"><span data-stu-id="a78f1-124">To configure the integration of Boomi into Azure AD, you need to add Boomi from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a78f1-125">**Pokud chcete přidat Boomi z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a78f1-125">**To add Boomi from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a78f1-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a78f1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a78f1-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a78f1-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a78f1-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a78f1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a78f1-133">Do vyhledávacího pole zadejte **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-133">In the search box, type **Boomi**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. <span data-ttu-id="a78f1-135">Na panelu výsledků vyberte **Boomi**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a78f1-135">In the results panel, select **Boomi**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a78f1-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a78f1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a78f1-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Boomi podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a78f1-138">In this section, you configure and test Azure AD single sign-on with Boomi based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a78f1-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Boomi je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a78f1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Boomi is to a user in Azure AD.</span></span> <span data-ttu-id="a78f1-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Boomi musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a78f1-140">In other words, a link relationship between an Azure AD user and the related user in Boomi needs to be established.</span></span>

<span data-ttu-id="a78f1-141">V Boomi, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a78f1-141">In Boomi, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a78f1-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Boomi, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a78f1-142">To configure and test Azure AD single sign-on with Boomi, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a78f1-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a78f1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a78f1-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a78f1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a78f1-145">**[Vytvoření zkušebního uživatele Boomi](#creating-a-boomi-test-user)**  – Pokud chcete mít protějšek Britta Simon v Boomi propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a78f1-145">**[Creating a Boomi test user](#creating-a-boomi-test-user)** - to have a counterpart of Britta Simon in Boomi that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a78f1-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a78f1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a78f1-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a78f1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a78f1-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a78f1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a78f1-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Boomi.</span><span class="sxs-lookup"><span data-stu-id="a78f1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Boomi application.</span></span>

<span data-ttu-id="a78f1-150">**Ke konfiguraci Azure AD jednotné přihlašování s Boomi, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a78f1-150">**To configure Azure AD single sign-on with Boomi, perform the following steps:**</span></span>

1. <span data-ttu-id="a78f1-151">Na portálu Azure na **Boomi** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-151">In the Azure portal, on the **Boomi** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a78f1-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a78f1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. <span data-ttu-id="a78f1-155">Na **Boomi domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a78f1-155">On the **Boomi Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    <span data-ttu-id="a78f1-157">a.</span><span class="sxs-lookup"><span data-stu-id="a78f1-157">a.</span></span> <span data-ttu-id="a78f1-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="a78f1-158">In the **Identifier** textbox, type a URL using the following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    <span data-ttu-id="a78f1-159">b.</span><span class="sxs-lookup"><span data-stu-id="a78f1-159">b.</span></span> <span data-ttu-id="a78f1-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="a78f1-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a78f1-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a78f1-161">These values are not real.</span></span> <span data-ttu-id="a78f1-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a78f1-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="a78f1-163">Obraťte se na [tým podpory Boomi](https://boomi.com/company/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a78f1-163">Contact [Boomi support team](https://boomi.com/company/contact/) to get these values.</span></span>

4. <span data-ttu-id="a78f1-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="a78f1-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. <span data-ttu-id="a78f1-166">Aplikace Boomi očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="a78f1-166">Boomi application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="a78f1-167">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a78f1-167">Please configure the following claims for this application.</span></span> <span data-ttu-id="a78f1-168">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="a78f1-168">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="a78f1-169">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="a78f1-169">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="a78f1-171">V **uživatelské atributy** části na **jednotného přihlašování** dialogu pro každý řádek v tabulce níže, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a78f1-171">In the **User Attributes** section on the **Single sign-on** dialog, for each row shown in the table below, perform the following steps:</span></span>

    | <span data-ttu-id="a78f1-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="a78f1-172">Attribute Name</span></span> | <span data-ttu-id="a78f1-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="a78f1-173">Attribute Value</span></span> |
    | -------------- | --------------- |
    | <span data-ttu-id="a78f1-174">FEDERATION_ID</span><span class="sxs-lookup"><span data-stu-id="a78f1-174">FEDERATION_ID</span></span> | <span data-ttu-id="a78f1-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="a78f1-175">user.mail</span></span> |
    
    <span data-ttu-id="a78f1-176">a.</span><span class="sxs-lookup"><span data-stu-id="a78f1-176">a.</span></span> <span data-ttu-id="a78f1-177">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a78f1-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="a78f1-180">b.</span><span class="sxs-lookup"><span data-stu-id="a78f1-180">b.</span></span> <span data-ttu-id="a78f1-181">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="a78f1-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="a78f1-182">c.</span><span class="sxs-lookup"><span data-stu-id="a78f1-182">c.</span></span> <span data-ttu-id="a78f1-183">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="a78f1-183">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="a78f1-184">d.</span><span class="sxs-lookup"><span data-stu-id="a78f1-184">d.</span></span> <span data-ttu-id="a78f1-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-185">Click **Ok**.</span></span>

6. <span data-ttu-id="a78f1-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a78f1-186">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="a78f1-188">Na **Boomi konfigurace** klikněte na tlačítko **konfigurace Boomi** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a78f1-188">On the **Boomi Configuration** section, click **Configure Boomi** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a78f1-189">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a78f1-189">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. <span data-ttu-id="a78f1-191">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Boomi.</span><span class="sxs-lookup"><span data-stu-id="a78f1-191">In a different web browser window, log into your Boomi company site as an administrator.</span></span> 

9. <span data-ttu-id="a78f1-192">Přejděte na **název společnosti** a přejděte na **nastavit**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-192">Navigate to **Company Name** and go to **Set up**.</span></span>

10. <span data-ttu-id="a78f1-193">Klikněte **možnosti jednotného přihlašování k** kartě a proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="a78f1-193">Click the **SSO Options** tab and perform below steps.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    <span data-ttu-id="a78f1-195">a.</span><span class="sxs-lookup"><span data-stu-id="a78f1-195">a.</span></span> <span data-ttu-id="a78f1-196">Zkontrolujte **povolit SAML jednotné přihlašování** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="a78f1-196">Check **Enable SAML Single Sign-On** checkbox.</span></span>

    <span data-ttu-id="a78f1-197">b.</span><span class="sxs-lookup"><span data-stu-id="a78f1-197">b.</span></span> <span data-ttu-id="a78f1-198">Klikněte na tlačítko **Import** na kterou odešlete certifikát stažený z Azure AD **certifikát zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-198">Click **Import** to upload the downloaded certificate from Azure AD to **Identity Provider Certificate**.</span></span>
    
    <span data-ttu-id="a78f1-199">c.</span><span class="sxs-lookup"><span data-stu-id="a78f1-199">c.</span></span> <span data-ttu-id="a78f1-200">V **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a78f1-200">In the **Identity Provider Login URL** textbox, put the value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="a78f1-201">d.</span><span class="sxs-lookup"><span data-stu-id="a78f1-201">d.</span></span> <span data-ttu-id="a78f1-202">Jako **Federation Id umístění**, vyberte **federace je v elementu FEDERATION_ID atribut** přepínač.</span><span class="sxs-lookup"><span data-stu-id="a78f1-202">As **Federation Id Location**, select **Federation Id is in FEDERATION_ID Attribute element** radio button.</span></span> 

    <span data-ttu-id="a78f1-203">e.</span><span class="sxs-lookup"><span data-stu-id="a78f1-203">e.</span></span> <span data-ttu-id="a78f1-204">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a78f1-204">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="a78f1-205">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a78f1-205">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a78f1-206">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a78f1-206">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a78f1-207">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a78f1-207">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a78f1-208">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a78f1-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="a78f1-209">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a78f1-209">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a78f1-211">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a78f1-211">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a78f1-212">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a78f1-212">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a78f1-214">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-214">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a78f1-216">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a78f1-216">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a78f1-218">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a78f1-218">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a78f1-220">a.</span><span class="sxs-lookup"><span data-stu-id="a78f1-220">a.</span></span> <span data-ttu-id="a78f1-221">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-221">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a78f1-222">b.</span><span class="sxs-lookup"><span data-stu-id="a78f1-222">b.</span></span> <span data-ttu-id="a78f1-223">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a78f1-223">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a78f1-224">c.</span><span class="sxs-lookup"><span data-stu-id="a78f1-224">c.</span></span> <span data-ttu-id="a78f1-225">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-225">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a78f1-226">d.</span><span class="sxs-lookup"><span data-stu-id="a78f1-226">d.</span></span> <span data-ttu-id="a78f1-227">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-227">Click **Create**.</span></span>
 
### <a name="creating-a-boomi-test-user"></a><span data-ttu-id="a78f1-228">Vytvoření zkušebního uživatele Boomi</span><span class="sxs-lookup"><span data-stu-id="a78f1-228">Creating a Boomi test user</span></span>

<span data-ttu-id="a78f1-229">Pokud chcete povolit uživatelům Azure AD přihlášení k Boomi, musí být zřízená do Boomi.</span><span class="sxs-lookup"><span data-stu-id="a78f1-229">In order to enable Azure AD users to log in to Boomi, they must be provisioned into Boomi.</span></span> <span data-ttu-id="a78f1-230">V případě Boomi zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a78f1-230">In the case of Boomi, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="a78f1-231">K poskytnutí uživatelského účtu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a78f1-231">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="a78f1-232">Přihlaste se k serveru vaší společnosti Boomi jako správce.</span><span class="sxs-lookup"><span data-stu-id="a78f1-232">Log in to your Boomi company site as an administrator.</span></span>

2. <span data-ttu-id="a78f1-233">Po přihlášení, přejděte na **Správa uživatelů** a přejděte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-233">After logging in, navigate to **User Management** and go to **Users**.</span></span>

    <span data-ttu-id="a78f1-234">![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a78f1-234">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "Users")</span></span>

3. <span data-ttu-id="a78f1-235">Klikněte na tlačítko  **+**  ikonu a **role uživatele přidat/zachování** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a78f1-235">Click **+**  icon and the **Add/Maintain User Roles** dialog opens.</span></span>

    <span data-ttu-id="a78f1-236">![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a78f1-236">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "Users")</span></span>

    <span data-ttu-id="a78f1-237">![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a78f1-237">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "Users")</span></span>

    <span data-ttu-id="a78f1-238">a.</span><span class="sxs-lookup"><span data-stu-id="a78f1-238">a.</span></span> <span data-ttu-id="a78f1-239">V **e-mailová adresa uživatele** jako typ e-mailu uživatele k textovému poli, BrittaSimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="a78f1-239">In the **User e-mail address** textbox, type the email of user like BrittaSimon@contoso.com.</span></span>
    
    <span data-ttu-id="a78f1-240">b.</span><span class="sxs-lookup"><span data-stu-id="a78f1-240">b.</span></span> <span data-ttu-id="a78f1-241">V **křestní jméno** textovému poli, název typu první uživatele jako Britta.</span><span class="sxs-lookup"><span data-stu-id="a78f1-241">In the **First name** textbox, type the First name of user like Britta.</span></span>

    <span data-ttu-id="a78f1-242">c.</span><span class="sxs-lookup"><span data-stu-id="a78f1-242">c.</span></span> <span data-ttu-id="a78f1-243">V **příjmení** textovému poli, zadejte příjmení uživatele jako Simon.</span><span class="sxs-lookup"><span data-stu-id="a78f1-243">In the **Last name** textbox, type the Last name of user like Simon.</span></span>
    
    <span data-ttu-id="a78f1-244">d.</span><span class="sxs-lookup"><span data-stu-id="a78f1-244">d.</span></span> <span data-ttu-id="a78f1-245">Zadejte uživatele **federace**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-245">Enter the user's **Federation ID**.</span></span> <span data-ttu-id="a78f1-246">Každý uživatel musí mít ID federace, která jednoznačně identifikuje uživatele v rámci účtu.</span><span class="sxs-lookup"><span data-stu-id="a78f1-246">Each user must have a Federation ID that uniquely identifies the user within the account.</span></span>
    
    <span data-ttu-id="a78f1-247">e.</span><span class="sxs-lookup"><span data-stu-id="a78f1-247">e.</span></span> <span data-ttu-id="a78f1-248">Přiřazení **standardní uživatel** role pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="a78f1-248">Assign the **Standard User** role to the user.</span></span> <span data-ttu-id="a78f1-249">Vzhledem k tomu, který by mu udělit normální prostředí přístup, jakož i přístup přihlašování není přiřadit role správce.</span><span class="sxs-lookup"><span data-stu-id="a78f1-249">Do not assign the Administrator role because that would give him normal Atmosphere access as well as single sign-on access.</span></span>
    
    <span data-ttu-id="a78f1-250">f.</span><span class="sxs-lookup"><span data-stu-id="a78f1-250">f.</span></span> <span data-ttu-id="a78f1-251">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-251">Click **OK**.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="a78f1-252">Uživatel nebude přijímat úvodní oznamovací e-mail obsahující heslo, které lze použít k přihlášení k účtu AtomSphere, protože heslo je spravován pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="a78f1-252">The user will not receive a welcome notification email containing a password that can be used to log in to the AtomSphere account because his password is managed through the identity provider.</span></span> <span data-ttu-id="a78f1-253">Může použít jakékoli jiné Boomi uživatele účtu vytvoření nástroje nebo rozhraní API poskytované Boomi zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="a78f1-253">You may use any other Boomi user account creation tools or APIs provided by Boomi to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a78f1-254">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a78f1-254">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a78f1-255">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Boomi.</span><span class="sxs-lookup"><span data-stu-id="a78f1-255">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Boomi.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a78f1-257">**Pokud chcete přiřadit Britta Simon Boomi, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a78f1-257">**To assign Britta Simon to Boomi, perform the following steps:**</span></span>

1. <span data-ttu-id="a78f1-258">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-258">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a78f1-260">V seznamu aplikací vyberte **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-260">In the applications list, select **Boomi**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. <span data-ttu-id="a78f1-262">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a78f1-262">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a78f1-264">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a78f1-264">Click **Add** button.</span></span> <span data-ttu-id="a78f1-265">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a78f1-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a78f1-267">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a78f1-267">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a78f1-268">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a78f1-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a78f1-269">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a78f1-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a78f1-270">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a78f1-270">Testing single sign-on</span></span>

<span data-ttu-id="a78f1-271">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a78f1-271">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a78f1-272">Když kliknete na dlaždici Boomi na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Boomi.</span><span class="sxs-lookup"><span data-stu-id="a78f1-272">When you click the Boomi tile in the Access Panel, you should get automatically signed-on to your Boomi application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a78f1-273">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a78f1-273">Additional resources</span></span>

* [<span data-ttu-id="a78f1-274">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a78f1-274">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a78f1-275">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a78f1-275">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

