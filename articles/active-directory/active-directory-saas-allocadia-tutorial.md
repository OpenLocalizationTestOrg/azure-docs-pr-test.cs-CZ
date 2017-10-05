---
title: 'Kurz: Azure Active Directory integrace s Allocadia | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Allocadia."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c415fc55-6dc1-49f2-a8a2-2fc6e3790d65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: jeedes
ms.openlocfilehash: 8e97c365383ecdb72cc1cd449b522b75875fc1db
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a><span data-ttu-id="9c315-103">Kurz: Azure Active Directory integrace s Allocadia</span><span class="sxs-lookup"><span data-stu-id="9c315-103">Tutorial: Azure Active Directory integration with Allocadia</span></span>

<span data-ttu-id="9c315-104">V tomto kurzu zjistěte, jak integrovat Allocadia s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9c315-104">In this tutorial, you learn how to integrate Allocadia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9c315-105">Integrace Allocadia s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9c315-105">Integrating Allocadia with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9c315-106">Můžete řídit ve službě Azure AD, který má přístup k Allocadia</span><span class="sxs-lookup"><span data-stu-id="9c315-106">You can control in Azure AD who has access to Allocadia</span></span>
- <span data-ttu-id="9c315-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Allocadia (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c315-107">You can enable your users to automatically get signed-on to Allocadia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9c315-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9c315-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9c315-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9c315-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c315-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9c315-110">Prerequisites</span></span>

<span data-ttu-id="9c315-111">Konfigurace integrace Azure AD s Allocadia, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9c315-111">To configure Azure AD integration with Allocadia, you need the following items:</span></span>

- <span data-ttu-id="9c315-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c315-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9c315-113">Allocadia jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9c315-113">An Allocadia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9c315-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c315-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9c315-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9c315-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9c315-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9c315-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9c315-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9c315-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9c315-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9c315-118">Scenario description</span></span>
<span data-ttu-id="9c315-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c315-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9c315-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9c315-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9c315-121">Přidání Allocadia z Galerie</span><span class="sxs-lookup"><span data-stu-id="9c315-121">Adding Allocadia from the gallery</span></span>
2. <span data-ttu-id="9c315-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c315-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-allocadia-from-the-gallery"></a><span data-ttu-id="9c315-123">Přidání Allocadia z Galerie</span><span class="sxs-lookup"><span data-stu-id="9c315-123">Adding Allocadia from the gallery</span></span>
<span data-ttu-id="9c315-124">Při konfiguraci integrace Allocadia do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Allocadia z galerie.</span><span class="sxs-lookup"><span data-stu-id="9c315-124">To configure the integration of Allocadia into Azure AD, you need to add Allocadia from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9c315-125">**Pokud chcete přidat Allocadia z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c315-125">**To add Allocadia from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9c315-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9c315-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9c315-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9c315-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9c315-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9c315-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9c315-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c315-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9c315-133">Do vyhledávacího pole zadejte **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="9c315-133">In the search box, type **Allocadia**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_search.png)

5. <span data-ttu-id="9c315-135">Na panelu výsledků vyberte **Allocadia**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c315-135">In the results panel, select **Allocadia**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9c315-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c315-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9c315-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Allocadia podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9c315-138">In this section, you configure and test Azure AD single sign-on with Allocadia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9c315-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Allocadia je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c315-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Allocadia is to a user in Azure AD.</span></span> <span data-ttu-id="9c315-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Allocadia musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9c315-140">In other words, a link relationship between an Azure AD user and the related user in Allocadia needs to be established.</span></span>

<span data-ttu-id="9c315-141">V Allocadia, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="9c315-141">In Allocadia, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9c315-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Allocadia, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9c315-142">To configure and test Azure AD single sign-on with Allocadia, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9c315-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9c315-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9c315-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c315-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9c315-145">**[Vytváření testovacího uživatele Allocadia](#creating-an-allocadia-test-user)**  – Pokud chcete mít protějšek Britta Simon v Allocadia propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c315-145">**[Creating an Allocadia test user](#creating-an-allocadia-test-user)** - to have a counterpart of Britta Simon in Allocadia that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9c315-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9c315-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9c315-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9c315-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9c315-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c315-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9c315-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Allocadia.</span><span class="sxs-lookup"><span data-stu-id="9c315-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Allocadia application.</span></span>

<span data-ttu-id="9c315-150">**Ke konfiguraci Azure AD jednotné přihlašování s Allocadia, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c315-150">**To configure Azure AD single sign-on with Allocadia, perform the following steps:**</span></span>

1. <span data-ttu-id="9c315-151">Na portálu Azure na **Allocadia** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9c315-151">In the Azure portal, on the **Allocadia** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9c315-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9c315-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_samlbase.png)

3. <span data-ttu-id="9c315-155">Na **Allocadia domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c315-155">On the **Allocadia Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_url.png)

    <span data-ttu-id="9c315-157">a.</span><span class="sxs-lookup"><span data-stu-id="9c315-157">a.</span></span> <span data-ttu-id="9c315-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="9c315-158">In the **Identifier** textbox, type a URL using the following patterns:</span></span> 
       
     <span data-ttu-id="9c315-159">Pro testovací prostředí-`https://na2standby.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="9c315-159">For test environment -  `https://na2standby.allocadia.com`</span></span>
    
     <span data-ttu-id="9c315-160">Pro produkční prostředí-`https://na2.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="9c315-160">For production environment - `https://na2.allocadia.com`</span></span>

    <span data-ttu-id="9c315-161">b.</span><span class="sxs-lookup"><span data-stu-id="9c315-161">b.</span></span> <span data-ttu-id="9c315-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="9c315-162">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 
    
     <span data-ttu-id="9c315-163">Pro testovací prostředí-`https://na2standby.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="9c315-163">For test environment - `https://na2standby.allocadia.com/allocadia/saml/SSO`</span></span>
    
     <span data-ttu-id="9c315-164">Pro produkční prostředí-`https://na2.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="9c315-164">For production environment - `https://na2.allocadia.com/allocadia/saml/SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9c315-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9c315-165">These values are not real.</span></span> <span data-ttu-id="9c315-166">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9c315-166">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="9c315-167">Obraťte se na [tým podpory Allocadia](mailTo:support@allocadia.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="9c315-167">Contact [Allocadia support team](mailTo:support@allocadia.com) to get these values.</span></span>

4. <span data-ttu-id="9c315-168">Aplikace Allocadia očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="9c315-168">Allocadia application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="9c315-169">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c315-169">Configure the following claims for this application.</span></span> <span data-ttu-id="9c315-170">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c315-170">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="9c315-171">Následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="9c315-171">The following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_attributes.png)
    
5. <span data-ttu-id="9c315-173">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c315-173">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="9c315-174">Název atributu</span><span class="sxs-lookup"><span data-stu-id="9c315-174">Attribute Name</span></span> | <span data-ttu-id="9c315-175">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="9c315-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="9c315-176">FirstName</span><span class="sxs-lookup"><span data-stu-id="9c315-176">firstname</span></span> | <span data-ttu-id="9c315-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="9c315-177">user.givenname</span></span> |
    | <span data-ttu-id="9c315-178">Příjmení</span><span class="sxs-lookup"><span data-stu-id="9c315-178">lastname</span></span> | <span data-ttu-id="9c315-179">User.Surname</span><span class="sxs-lookup"><span data-stu-id="9c315-179">user.surname</span></span> |
    | <span data-ttu-id="9c315-180">E-mailu</span><span class="sxs-lookup"><span data-stu-id="9c315-180">email</span></span> | <span data-ttu-id="9c315-181">User.Mail</span><span class="sxs-lookup"><span data-stu-id="9c315-181">user.mail</span></span> |
    
    <span data-ttu-id="9c315-182">a.</span><span class="sxs-lookup"><span data-stu-id="9c315-182">a.</span></span> <span data-ttu-id="9c315-183">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c315-183">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="9c315-185">b.</span><span class="sxs-lookup"><span data-stu-id="9c315-185">b.</span></span> <span data-ttu-id="9c315-186">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="9c315-186">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="9c315-188">c.</span><span class="sxs-lookup"><span data-stu-id="9c315-188">c.</span></span> <span data-ttu-id="9c315-189">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="9c315-189">From the **Value** list, type the attribute value shown for that row.</span></span>
 
    <span data-ttu-id="9c315-190">d.</span><span class="sxs-lookup"><span data-stu-id="9c315-190">d.</span></span> <span data-ttu-id="9c315-191">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c315-191">Click **Ok**.</span></span>



6. <span data-ttu-id="9c315-192">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9c315-192">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_certificate.png) 


7. <span data-ttu-id="9c315-194">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c315-194">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9c315-196">Konfigurace jednotného přihlašování na **Allocadia** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Allocadia](mailTo:support@allocadia.com).</span><span class="sxs-lookup"><span data-stu-id="9c315-196">To configure single sign-on on **Allocadia** side, you need to send the downloaded **Metadata XML** to [Allocadia support team](mailTo:support@allocadia.com).</span></span> <span data-ttu-id="9c315-197">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="9c315-197">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="9c315-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9c315-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9c315-199">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9c315-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9c315-200">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9c315-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9c315-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c315-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="9c315-202">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c315-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9c315-204">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c315-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9c315-205">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9c315-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9c315-207">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c315-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9c315-209">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c315-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9c315-211">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c315-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9c315-213">a.</span><span class="sxs-lookup"><span data-stu-id="9c315-213">a.</span></span> <span data-ttu-id="9c315-214">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9c315-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9c315-215">b.</span><span class="sxs-lookup"><span data-stu-id="9c315-215">b.</span></span> <span data-ttu-id="9c315-216">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9c315-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9c315-217">c.</span><span class="sxs-lookup"><span data-stu-id="9c315-217">c.</span></span> <span data-ttu-id="9c315-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9c315-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9c315-219">d.</span><span class="sxs-lookup"><span data-stu-id="9c315-219">d.</span></span> <span data-ttu-id="9c315-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9c315-220">Click **Create**.</span></span>
 
### <a name="creating-an-allocadia-test-user"></a><span data-ttu-id="9c315-221">Vytváření testovacího uživatele Allocadia</span><span class="sxs-lookup"><span data-stu-id="9c315-221">Creating an Allocadia test user</span></span>

<span data-ttu-id="9c315-222">Aplikace podporuje pouze v době zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9c315-222">Application supports Just in time user provisioning.</span></span> <span data-ttu-id="9c315-223">Po ověření uživatelé jsou automaticky vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c315-223">After authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9c315-224">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c315-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9c315-225">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Allocadia.</span><span class="sxs-lookup"><span data-stu-id="9c315-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Allocadia.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9c315-227">**Pokud chcete přiřadit Britta Simon Allocadia, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c315-227">**To assign Britta Simon to Allocadia, perform the following steps:**</span></span>

1. <span data-ttu-id="9c315-228">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9c315-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9c315-230">V seznamu aplikací vyberte **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="9c315-230">In the applications list, select **Allocadia**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_app.png) 

3. <span data-ttu-id="9c315-232">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9c315-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9c315-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c315-234">Click **Add** button.</span></span> <span data-ttu-id="9c315-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c315-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9c315-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9c315-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9c315-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c315-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9c315-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c315-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9c315-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c315-240">Testing single sign-on</span></span>

<span data-ttu-id="9c315-241">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9c315-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9c315-242">Když kliknete na dlaždici Allocadia na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Allocadia.</span><span class="sxs-lookup"><span data-stu-id="9c315-242">When you click the Allocadia tile in the Access Panel, you should get automatically signed-on to your Allocadia application.</span></span>
<span data-ttu-id="9c315-243">Další informace o na přístupovém panelu najdete v tématu [Úvod do přístupového panelu](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="9c315-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c315-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9c315-244">Additional resources</span></span>

* [<span data-ttu-id="9c315-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c315-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9c315-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9c315-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png

