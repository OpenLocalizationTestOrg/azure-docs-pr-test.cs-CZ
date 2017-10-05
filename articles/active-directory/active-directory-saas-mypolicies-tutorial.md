---
title: 'Kurz: Azure Active Directory integrace s myPolicies | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a myPolicies."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bf79e858-1dfb-4ab3-a6df-74b2d5a878d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: jeedes
ms.openlocfilehash: fcb403041cb3a8bd20ff7b34aa5cc4b7bf0c0a16
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mypolicies"></a><span data-ttu-id="79492-103">Kurz: Azure Active Directory integrace s myPolicies</span><span class="sxs-lookup"><span data-stu-id="79492-103">Tutorial: Azure Active Directory integration with myPolicies</span></span>

<span data-ttu-id="79492-104">V tomto kurzu zjistěte, jak integrovat myPolicies s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="79492-104">In this tutorial, you learn how to integrate myPolicies with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="79492-105">Integrace myPolicies s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="79492-105">Integrating myPolicies with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="79492-106">Můžete řídit ve službě Azure AD, který má přístup k myPolicies</span><span class="sxs-lookup"><span data-stu-id="79492-106">You can control in Azure AD who has access to myPolicies</span></span>
- <span data-ttu-id="79492-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k myPolicies (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="79492-107">You can enable your users to automatically get signed-on to myPolicies (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="79492-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="79492-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="79492-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="79492-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79492-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="79492-110">Prerequisites</span></span>

<span data-ttu-id="79492-111">Konfigurace integrace Azure AD s myPolicies, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="79492-111">To configure Azure AD integration with myPolicies, you need the following items:</span></span>

- <span data-ttu-id="79492-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="79492-112">An Azure AD subscription</span></span>
- <span data-ttu-id="79492-113">MyPolicies jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="79492-113">A myPolicies single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="79492-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="79492-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="79492-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="79492-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="79492-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="79492-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="79492-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="79492-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="79492-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="79492-118">Scenario description</span></span>
<span data-ttu-id="79492-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="79492-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="79492-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="79492-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="79492-121">Přidání myPolicies z Galerie</span><span class="sxs-lookup"><span data-stu-id="79492-121">Adding myPolicies from the gallery</span></span>
2. <span data-ttu-id="79492-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="79492-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mypolicies-from-the-gallery"></a><span data-ttu-id="79492-123">Přidání myPolicies z Galerie</span><span class="sxs-lookup"><span data-stu-id="79492-123">Adding myPolicies from the gallery</span></span>
<span data-ttu-id="79492-124">Při konfiguraci integrace myPolicies do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS myPolicies z galerie.</span><span class="sxs-lookup"><span data-stu-id="79492-124">To configure the integration of myPolicies into Azure AD, you need to add myPolicies from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="79492-125">**Pokud chcete přidat myPolicies z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="79492-125">**To add myPolicies from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="79492-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="79492-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="79492-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="79492-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="79492-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="79492-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="79492-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79492-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="79492-133">Do vyhledávacího pole zadejte **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="79492-133">In the search box, type **myPolicies**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_search.png)

5. <span data-ttu-id="79492-135">Na panelu výsledků vyberte **myPolicies**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="79492-135">In the results panel, select **myPolicies**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="79492-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="79492-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="79492-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s myPolicies podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="79492-138">In this section, you configure and test Azure AD single sign-on with myPolicies based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="79492-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v myPolicies je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79492-139">For single sign-on to work, Azure AD needs to know what the counterpart user in myPolicies is to a user in Azure AD.</span></span> <span data-ttu-id="79492-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v myPolicies musí navázat.</span><span class="sxs-lookup"><span data-stu-id="79492-140">In other words, a link relationship between an Azure AD user and the related user in myPolicies needs to be established.</span></span>

<span data-ttu-id="79492-141">V myPolicies, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="79492-141">In myPolicies, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="79492-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s myPolicies, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="79492-142">To configure and test Azure AD single sign-on with myPolicies, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="79492-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="79492-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="79492-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79492-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="79492-145">**[Vytvoření zkušebního uživatele myPolicies](#creating-a-mypolicies-test-user)**  – Pokud chcete mít protějšek Britta Simon v myPolicies propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="79492-145">**[Creating a myPolicies test user](#creating-a-mypolicies-test-user)** - to have a counterpart of Britta Simon in myPolicies that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="79492-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="79492-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="79492-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="79492-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="79492-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="79492-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="79492-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci myPolicies.</span><span class="sxs-lookup"><span data-stu-id="79492-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your myPolicies application.</span></span>

<span data-ttu-id="79492-150">**Ke konfiguraci Azure AD jednotné přihlašování s myPolicies, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="79492-150">**To configure Azure AD single sign-on with myPolicies, perform the following steps:**</span></span>

1. <span data-ttu-id="79492-151">Na portálu Azure na **myPolicies** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="79492-151">In the Azure portal, on the **myPolicies** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="79492-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="79492-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_samlbase.png)

3. <span data-ttu-id="79492-155">Na **myPolicies domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="79492-155">On the **myPolicies Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_url.png)

    <span data-ttu-id="79492-157">a.</span><span class="sxs-lookup"><span data-stu-id="79492-157">a.</span></span> <span data-ttu-id="79492-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.mypolicies.com/`</span><span class="sxs-lookup"><span data-stu-id="79492-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.mypolicies.com/`</span></span>

    <span data-ttu-id="79492-159">b.</span><span class="sxs-lookup"><span data-stu-id="79492-159">b.</span></span> <span data-ttu-id="79492-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="79492-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="79492-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="79492-161">These values are not real.</span></span> <span data-ttu-id="79492-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="79492-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="79492-163">Obraťte se na [tým podpory myPolicies](mailto:support@mypolicies.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="79492-163">Contact [myPolicies support team](mailto:support@mypolicies.com) to get these values.</span></span>

4. <span data-ttu-id="79492-164">Aplikace myPolicies očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="79492-164">The myPolicies application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="79492-165">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="79492-165">Configure the following claims for this application.</span></span> <span data-ttu-id="79492-166">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="79492-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="79492-167">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="79492-167">The following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_attribute.png)

5. <span data-ttu-id="79492-169">Klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** zaškrtnout políčko **uživatelské atributy** rozbalte atributy.</span><span class="sxs-lookup"><span data-stu-id="79492-169">Click **View and edit all other user attributes** checkbox in the **User Attributes** section to expand the attributes.</span></span> <span data-ttu-id="79492-170">Proveďte následující kroky na každém z zobrazených atributů-</span><span class="sxs-lookup"><span data-stu-id="79492-170">Perform the following steps on each of the displayed attributes-</span></span>

    | <span data-ttu-id="79492-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="79492-171">Attribute Name</span></span> | <span data-ttu-id="79492-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="79492-172">Attribute Value</span></span> |
    | ------------------- | ---------- |
    | <span data-ttu-id="79492-173">givenName</span><span class="sxs-lookup"><span data-stu-id="79492-173">givenname</span></span> | <span data-ttu-id="79492-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="79492-174">user.givenname</span></span> |
    | <span data-ttu-id="79492-175">Příjmení</span><span class="sxs-lookup"><span data-stu-id="79492-175">surname</span></span> | <span data-ttu-id="79492-176">User.Surname</span><span class="sxs-lookup"><span data-stu-id="79492-176">user.surname</span></span> |
    | <span data-ttu-id="79492-177">EmailAddress</span><span class="sxs-lookup"><span data-stu-id="79492-177">emailaddress</span></span> | <span data-ttu-id="79492-178">User.Mail</span><span class="sxs-lookup"><span data-stu-id="79492-178">user.mail</span></span> |
    | <span data-ttu-id="79492-179">jméno</span><span class="sxs-lookup"><span data-stu-id="79492-179">name</span></span> | <span data-ttu-id="79492-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="79492-180">user.userprincipalname</span></span> |
    
    <span data-ttu-id="79492-181">a.</span><span class="sxs-lookup"><span data-stu-id="79492-181">a.</span></span> <span data-ttu-id="79492-182">Klikněte na atribut, který se otevře **Upravit atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79492-182">Click on the attribute to open the **Edit Attribute** dialog.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="79492-184">b.</span><span class="sxs-lookup"><span data-stu-id="79492-184">b.</span></span> <span data-ttu-id="79492-185">Odstranit hodnotu adresy URL **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="79492-185">Delete the URL value from the **Namespace**.</span></span>
    
    <span data-ttu-id="79492-186">c.</span><span class="sxs-lookup"><span data-stu-id="79492-186">c.</span></span> <span data-ttu-id="79492-187">Klikněte na tlačítko **Ok** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="79492-187">Click **Ok** to save the setting.</span></span>
    
6. <span data-ttu-id="79492-188">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="79492-188">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_certificate.png) 

7. <span data-ttu-id="79492-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="79492-190">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="79492-192">Na **myPolicies konfigurace** klikněte na tlačítko **konfigurace myPolicies** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="79492-192">On the **myPolicies Configuration** section, click **Configure myPolicies** to open **Configure sign-on** window.</span></span> <span data-ttu-id="79492-193">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="79492-193">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_configure.png) 

9. <span data-ttu-id="79492-195">Konfigurace jednotného přihlašování na **myPolicies** straně, budete muset odeslat stažené **Certificate(Base64)** a **SAML jeden přihlašování adresa URL služby** k [ tým podpory myPolicies](mailto:support@mypolicies.com).</span><span class="sxs-lookup"><span data-stu-id="79492-195">To configure single sign-on on **myPolicies** side, you need to send the downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** to [myPolicies support team](mailto:support@mypolicies.com).</span></span> 

> [!TIP]
> <span data-ttu-id="79492-196">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="79492-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="79492-197">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="79492-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="79492-198">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="79492-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="79492-199">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="79492-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="79492-200">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79492-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="79492-202">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="79492-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="79492-203">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="79492-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="79492-205">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="79492-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="79492-207">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79492-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="79492-209">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="79492-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="79492-211">a.</span><span class="sxs-lookup"><span data-stu-id="79492-211">a.</span></span> <span data-ttu-id="79492-212">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="79492-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="79492-213">b.</span><span class="sxs-lookup"><span data-stu-id="79492-213">b.</span></span> <span data-ttu-id="79492-214">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="79492-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="79492-215">c.</span><span class="sxs-lookup"><span data-stu-id="79492-215">c.</span></span> <span data-ttu-id="79492-216">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="79492-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="79492-217">d.</span><span class="sxs-lookup"><span data-stu-id="79492-217">d.</span></span> <span data-ttu-id="79492-218">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="79492-218">Click **Create**.</span></span>
 
### <a name="creating-a-mypolicies-test-user"></a><span data-ttu-id="79492-219">Vytvoření zkušebního uživatele myPolicies</span><span class="sxs-lookup"><span data-stu-id="79492-219">Creating a myPolicies test user</span></span>

<span data-ttu-id="79492-220">V této části vytvoříte volal Britta Simon v myPolicies uživatele.</span><span class="sxs-lookup"><span data-stu-id="79492-220">In this section, you create a user called Britta Simon in myPolicies.</span></span> <span data-ttu-id="79492-221">Práce s [tým podpory myPolicies](mailto:support@mypolicies.com) přidat uživatele do myPolicies platformy.</span><span class="sxs-lookup"><span data-stu-id="79492-221">Work with [myPolicies support team](mailto:support@mypolicies.com) to add the users in the myPolicies platform.</span></span> <span data-ttu-id="79492-222">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="79492-222">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="79492-223">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="79492-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="79492-224">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k myPolicies.</span><span class="sxs-lookup"><span data-stu-id="79492-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to myPolicies.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="79492-226">**Pokud chcete přiřadit Britta Simon myPolicies, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="79492-226">**To assign Britta Simon to myPolicies, perform the following steps:**</span></span>

1. <span data-ttu-id="79492-227">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="79492-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="79492-229">V seznamu aplikací vyberte **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="79492-229">In the applications list, select **myPolicies**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_app.png) 

3. <span data-ttu-id="79492-231">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="79492-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="79492-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="79492-233">Click **Add** button.</span></span> <span data-ttu-id="79492-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79492-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="79492-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="79492-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="79492-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79492-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="79492-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79492-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="79492-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="79492-239">Testing single sign-on</span></span>

<span data-ttu-id="79492-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="79492-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="79492-241">Když kliknete na dlaždici myPolicies na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci myPolicies.</span><span class="sxs-lookup"><span data-stu-id="79492-241">When you click the myPolicies tile in the Access Panel, you should get automatically signed-on to your myPolicies application.</span></span>
<span data-ttu-id="79492-242">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="79492-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79492-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="79492-243">Additional resources</span></span>

* [<span data-ttu-id="79492-244">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="79492-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="79492-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="79492-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_203.png

