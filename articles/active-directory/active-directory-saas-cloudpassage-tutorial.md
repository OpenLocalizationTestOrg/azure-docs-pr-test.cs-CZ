---
title: 'Kurz: Azure Active Directory integrace s CloudPassage | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a CloudPassage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 094740e20570665e975dec1a591989e411f90c16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a><span data-ttu-id="e4df8-103">Kurz: Azure Active Directory integrace s CloudPassage</span><span class="sxs-lookup"><span data-stu-id="e4df8-103">Tutorial: Azure Active Directory integration with CloudPassage</span></span>

<span data-ttu-id="e4df8-104">V tomto kurzu zjistěte, jak integrovat CloudPassage s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e4df8-104">In this tutorial, you learn how to integrate CloudPassage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e4df8-105">Integrace CloudPassage s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e4df8-105">Integrating CloudPassage with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e4df8-106">Můžete řídit ve službě Azure AD, který má přístup k CloudPassage</span><span class="sxs-lookup"><span data-stu-id="e4df8-106">You can control in Azure AD who has access to CloudPassage</span></span>
- <span data-ttu-id="e4df8-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k CloudPassage (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4df8-107">You can enable your users to automatically get signed-on to CloudPassage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e4df8-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e4df8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e4df8-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e4df8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4df8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e4df8-110">Prerequisites</span></span>

<span data-ttu-id="e4df8-111">Konfigurace integrace Azure AD s CloudPassage, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e4df8-111">To configure Azure AD integration with CloudPassage, you need the following items:</span></span>

- <span data-ttu-id="e4df8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4df8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e4df8-113">CloudPassage jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e4df8-113">A CloudPassage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e4df8-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e4df8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e4df8-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e4df8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e4df8-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e4df8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e4df8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e4df8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e4df8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e4df8-118">Scenario description</span></span>
<span data-ttu-id="e4df8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e4df8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e4df8-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e4df8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e4df8-121">Přidání CloudPassage z Galerie</span><span class="sxs-lookup"><span data-stu-id="e4df8-121">Adding CloudPassage from the gallery</span></span>
2. <span data-ttu-id="e4df8-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e4df8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloudpassage-from-the-gallery"></a><span data-ttu-id="e4df8-123">Přidání CloudPassage z Galerie</span><span class="sxs-lookup"><span data-stu-id="e4df8-123">Adding CloudPassage from the gallery</span></span>
<span data-ttu-id="e4df8-124">Při konfiguraci integrace CloudPassage do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS CloudPassage z galerie.</span><span class="sxs-lookup"><span data-stu-id="e4df8-124">To configure the integration of CloudPassage into Azure AD, you need to add CloudPassage from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e4df8-125">**Pokud chcete přidat CloudPassage z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e4df8-125">**To add CloudPassage from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e4df8-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e4df8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e4df8-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e4df8-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e4df8-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4df8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e4df8-133">Do vyhledávacího pole zadejte **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-133">In the search box, type **CloudPassage**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_search.png)

5. <span data-ttu-id="e4df8-135">Na panelu výsledků vyberte **CloudPassage**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e4df8-135">In the results panel, select **CloudPassage**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e4df8-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e4df8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e4df8-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s CloudPassage podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="e4df8-138">In this section, you configure and test Azure AD single sign-on with CloudPassage based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e4df8-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v CloudPassage je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e4df8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CloudPassage is to a user in Azure AD.</span></span> <span data-ttu-id="e4df8-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v CloudPassage musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e4df8-140">In other words, a link relationship between an Azure AD user and the related user in CloudPassage needs to be established.</span></span>

<span data-ttu-id="e4df8-141">V CloudPassage, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="e4df8-141">In CloudPassage, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e4df8-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s CloudPassage, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e4df8-142">To configure and test Azure AD single sign-on with CloudPassage, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e4df8-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e4df8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e4df8-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4df8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e4df8-145">**[Vytvoření zkušebního uživatele CloudPassage](#creating-a-cloudpassage-test-user)**  – Pokud chcete mít protějšek Britta Simon v CloudPassage propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e4df8-145">**[Creating a CloudPassage test user](#creating-a-cloudpassage-test-user)** - to have a counterpart of Britta Simon in CloudPassage that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e4df8-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e4df8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e4df8-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e4df8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e4df8-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e4df8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e4df8-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci CloudPassage.</span><span class="sxs-lookup"><span data-stu-id="e4df8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CloudPassage application.</span></span>

<span data-ttu-id="e4df8-150">**Ke konfiguraci Azure AD jednotné přihlašování s CloudPassage, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e4df8-150">**To configure Azure AD single sign-on with CloudPassage, perform the following steps:**</span></span>

1. <span data-ttu-id="e4df8-151">Na portálu Azure na **CloudPassage** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-151">In the Azure portal, on the **CloudPassage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e4df8-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e4df8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

3. <span data-ttu-id="e4df8-155">Na **CloudPassage domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e4df8-155">On the **CloudPassage Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    <span data-ttu-id="e4df8-157">a.</span><span class="sxs-lookup"><span data-stu-id="e4df8-157">a.</span></span> <span data-ttu-id="e4df8-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://portal.cloudpassage.com/saml/init/accountid`</span><span class="sxs-lookup"><span data-stu-id="e4df8-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://portal.cloudpassage.com/saml/init/accountid`</span></span>

    <span data-ttu-id="e4df8-159">b.</span><span class="sxs-lookup"><span data-stu-id="e4df8-159">b.</span></span> <span data-ttu-id="e4df8-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce: `https://portal.cloudpassage.com/saml/consume/accountid`.</span><span class="sxs-lookup"><span data-stu-id="e4df8-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://portal.cloudpassage.com/saml/consume/accountid`.</span></span> <span data-ttu-id="e4df8-161">Hodnota tohoto atributu můžete získat kliknutím **nastavení jednotného přihlašování k dokumentaci** v **nastavení jednotného přihlašování** části portálu CloudPassage.</span><span class="sxs-lookup"><span data-stu-id="e4df8-161">You can get your value for this attribute by clicking **SSO Setup documentation** in the **Single Sign-on Settings** section of your CloudPassage portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > <span data-ttu-id="e4df8-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e4df8-163">These values are not real.</span></span> <span data-ttu-id="e4df8-164">Tyto hodnoty aktualizujte s skutečná adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="e4df8-164">Update these values with the actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="e4df8-165">Obraťte se na [tým podpory CloudPassage klienta](https://www.cloudpassage.com/company/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="e4df8-165">Contact [CloudPassage Client support team](https://www.cloudpassage.com/company/contact/) to get these values.</span></span> 

4. <span data-ttu-id="e4df8-166">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="e4df8-166">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

5. <span data-ttu-id="e4df8-168">Aplikace CloudPassage očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="e4df8-168">Your CloudPassage application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="e4df8-169">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="e4df8-169">The following screenshot shows an example for this.</span></span>
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

6. <span data-ttu-id="e4df8-171">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e4df8-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>

    | <span data-ttu-id="e4df8-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e4df8-172">Attribute Name</span></span> | <span data-ttu-id="e4df8-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="e4df8-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="e4df8-174">FirstName</span><span class="sxs-lookup"><span data-stu-id="e4df8-174">firstname</span></span> |<span data-ttu-id="e4df8-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="e4df8-175">user.givenname</span></span> |
    | <span data-ttu-id="e4df8-176">Příjmení</span><span class="sxs-lookup"><span data-stu-id="e4df8-176">lastname</span></span> |<span data-ttu-id="e4df8-177">User.Surname</span><span class="sxs-lookup"><span data-stu-id="e4df8-177">user.surname</span></span> |
    | <span data-ttu-id="e4df8-178">E-mailu</span><span class="sxs-lookup"><span data-stu-id="e4df8-178">email</span></span> |<span data-ttu-id="e4df8-179">User.Mail</span><span class="sxs-lookup"><span data-stu-id="e4df8-179">user.mail</span></span> |
    
    <span data-ttu-id="e4df8-180">a.</span><span class="sxs-lookup"><span data-stu-id="e4df8-180">a.</span></span> <span data-ttu-id="e4df8-181">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4df8-181">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="e4df8-184">b.</span><span class="sxs-lookup"><span data-stu-id="e4df8-184">b.</span></span> <span data-ttu-id="e4df8-185">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="e4df8-185">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="e4df8-186">c.</span><span class="sxs-lookup"><span data-stu-id="e4df8-186">c.</span></span> <span data-ttu-id="e4df8-187">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="e4df8-187">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="e4df8-188">d.</span><span class="sxs-lookup"><span data-stu-id="e4df8-188">d.</span></span> <span data-ttu-id="e4df8-189">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-189">Click **Ok**.</span></span>

7. <span data-ttu-id="e4df8-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e4df8-190">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="e4df8-192">Na **CloudPassage konfigurace** klikněte na tlačítko **konfigurace CloudPassage** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e4df8-192">On the **CloudPassage Configuration** section, click **Configure CloudPassage** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e4df8-193">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e4df8-193">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

9. <span data-ttu-id="e4df8-195">V okně jiný prohlížeč přihlašování k webu společnosti CloudPassage jako správce.</span><span class="sxs-lookup"><span data-stu-id="e4df8-195">In a different browser window, sign-on to your CloudPassage company site as administrator.</span></span>

10. <span data-ttu-id="e4df8-196">V nabídce v horní části, klikněte na tlačítko **nastavení**a potom klikněte na **Správa webu**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-196">In the menu on the top, click **Settings**, and then click **Site Administration**.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][12]

11. <span data-ttu-id="e4df8-198">Klikněte **nastavení ověřování** kartě.</span><span class="sxs-lookup"><span data-stu-id="e4df8-198">Click the **Authentication Settings** tab.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][13]

12. <span data-ttu-id="e4df8-200">V **nastavení jednotného přihlašování** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e4df8-200">In the **Single Sign-on Settings** section, perform the following steps:</span></span> 
   
    ![Konfigurovat jednotné přihlašování][14]

    <span data-ttu-id="e4df8-202">a.</span><span class="sxs-lookup"><span data-stu-id="e4df8-202">a.</span></span> <span data-ttu-id="e4df8-203">Vyberte **povolit jeden sign-on(SSO) (dokumentace instalace jednotné přihlašování)** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="e4df8-203">Select **Enable Single sign-on(SSO)(SSO Setup Documentation)** checkbox.</span></span>
    
    <span data-ttu-id="e4df8-204">b.</span><span class="sxs-lookup"><span data-stu-id="e4df8-204">b.</span></span> <span data-ttu-id="e4df8-205">Vložení **SAML Entity ID** do **URL vystavitele SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e4df8-205">Paste **SAML Entity ID** into the **SAML issuer URL** textbox.</span></span>
  
    <span data-ttu-id="e4df8-206">c.</span><span class="sxs-lookup"><span data-stu-id="e4df8-206">c.</span></span> <span data-ttu-id="e4df8-207">Vložení **SAML jeden přihlašování adresa URL služby** do **adresu URL koncového bodu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e4df8-207">Paste **SAML Single Sign-On Service URL** into the **SAML endpoint URL** textbox.</span></span>
  
    <span data-ttu-id="e4df8-208">d.</span><span class="sxs-lookup"><span data-stu-id="e4df8-208">d.</span></span> <span data-ttu-id="e4df8-209">Vložení **Sign-Out URL** do **odhlášení cílová stránka** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e4df8-209">Paste **Sign-Out URL** into the **Logout landing page** textbox.</span></span>
  
    <span data-ttu-id="e4df8-210">e.</span><span class="sxs-lookup"><span data-stu-id="e4df8-210">e.</span></span> <span data-ttu-id="e4df8-211">V poznámkovém bloku otevřete stažený certifikát, zkopírujte obsah stažený certifikát do schránky a vložte ji do **x 509 certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e4df8-211">Open your downloaded certificate in notepad, copy the content of downloaded certificate into your clipboard, and then paste it into the **x 509 certificate** textbox.</span></span>
  
    <span data-ttu-id="e4df8-212">f.</span><span class="sxs-lookup"><span data-stu-id="e4df8-212">f.</span></span> <span data-ttu-id="e4df8-213">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="e4df8-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e4df8-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e4df8-215">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e4df8-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e4df8-216">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e4df8-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e4df8-217">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4df8-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="e4df8-218">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4df8-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e4df8-220">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e4df8-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e4df8-221">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e4df8-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e4df8-223">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e4df8-225">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4df8-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e4df8-227">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e4df8-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e4df8-229">a.</span><span class="sxs-lookup"><span data-stu-id="e4df8-229">a.</span></span> <span data-ttu-id="e4df8-230">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e4df8-231">b.</span><span class="sxs-lookup"><span data-stu-id="e4df8-231">b.</span></span> <span data-ttu-id="e4df8-232">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e4df8-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e4df8-233">c.</span><span class="sxs-lookup"><span data-stu-id="e4df8-233">c.</span></span> <span data-ttu-id="e4df8-234">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e4df8-235">d.</span><span class="sxs-lookup"><span data-stu-id="e4df8-235">d.</span></span> <span data-ttu-id="e4df8-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-236">Click **Create**.</span></span>
 
### <a name="creating-a-cloudpassage-test-user"></a><span data-ttu-id="e4df8-237">Vytvoření zkušebního uživatele CloudPassage</span><span class="sxs-lookup"><span data-stu-id="e4df8-237">Creating a CloudPassage test user</span></span>

<span data-ttu-id="e4df8-238">Cílem této části je vytvoření uživatele v CloudPassage nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4df8-238">The objective of this section is to create a user called Britta Simon in CloudPassage.</span></span>

<span data-ttu-id="e4df8-239">**Vytvoření uživatele v CloudPassage nazývá Britta Simon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e4df8-239">**To create a user called Britta Simon in CloudPassage, perform the following steps:**</span></span>

1. <span data-ttu-id="e4df8-240">Přihlášení k vaší **CloudPassage** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="e4df8-240">Sign-on to your **CloudPassage** company site as an administrator.</span></span> 

2. <span data-ttu-id="e4df8-241">Na panelu nástrojů v horní části klikněte na tlačítko **nastavení**a potom klikněte na **Správa webu**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-241">In the toolbar on the top, click **Settings**, and then click **Site Administration**.</span></span> 
   
   ![Vytvoření zkušebního uživatele CloudPassage][22] 

3. <span data-ttu-id="e4df8-243">Klikněte **uživatelé** a pak klikněte **přidat nové uživatele**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-243">Click the **Users** tab, and then click **Add New User**.</span></span> 
   
   ![Vytvoření zkušebního uživatele CloudPassage][23]

4. <span data-ttu-id="e4df8-245">V **přidat nové uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e4df8-245">In the **Add New User** section, perform the following steps:</span></span> 
   
   ![Vytvoření zkušebního uživatele CloudPassage][24]
    
    <span data-ttu-id="e4df8-247">a.</span><span class="sxs-lookup"><span data-stu-id="e4df8-247">a.</span></span> <span data-ttu-id="e4df8-248">V **křestní jméno** textovému poli, zadejte Britta.</span><span class="sxs-lookup"><span data-stu-id="e4df8-248">In the **First Name** textbox, type Britta.</span></span> 
  
    <span data-ttu-id="e4df8-249">b.</span><span class="sxs-lookup"><span data-stu-id="e4df8-249">b.</span></span> <span data-ttu-id="e4df8-250">V **příjmení** textovému poli, zadejte Simon.</span><span class="sxs-lookup"><span data-stu-id="e4df8-250">In the **Last Name** textbox, type Simon.</span></span>
  
    <span data-ttu-id="e4df8-251">c.</span><span class="sxs-lookup"><span data-stu-id="e4df8-251">c.</span></span> <span data-ttu-id="e4df8-252">V **uživatelské jméno** textovému poli, **e-mailu** textové pole a **znovu zadejte e-mailu** textovému poli, zadejte uživatelské jméno je Britta ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e4df8-252">In the **Username** textbox, the **Email** textbox and the **Retype Email** textbox, type Britta's user name in Azure AD.</span></span>
  
    <span data-ttu-id="e4df8-253">d.</span><span class="sxs-lookup"><span data-stu-id="e4df8-253">d.</span></span> <span data-ttu-id="e4df8-254">Jako **typ přístupu**, vyberte **povolit přístup z portálu bylo**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-254">As **Access Type**, select **Enable Halo Portal Access**.</span></span>
  
    <span data-ttu-id="e4df8-255">e.</span><span class="sxs-lookup"><span data-stu-id="e4df8-255">e.</span></span> <span data-ttu-id="e4df8-256">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-256">Click **Add**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e4df8-257">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4df8-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e4df8-258">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu CloudPassage.</span><span class="sxs-lookup"><span data-stu-id="e4df8-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CloudPassage.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e4df8-260">**Pokud chcete přiřadit Britta Simon CloudPassage, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e4df8-260">**To assign Britta Simon to CloudPassage, perform the following steps:**</span></span>

1. <span data-ttu-id="e4df8-261">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e4df8-263">V seznamu aplikací vyberte **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-263">In the applications list, select **CloudPassage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

3. <span data-ttu-id="e4df8-265">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e4df8-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e4df8-267">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e4df8-267">Click **Add** button.</span></span> <span data-ttu-id="e4df8-268">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4df8-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e4df8-270">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e4df8-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e4df8-271">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4df8-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e4df8-272">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4df8-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e4df8-273">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e4df8-273">Testing single sign-on</span></span>

<span data-ttu-id="e4df8-274">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="e4df8-274">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="e4df8-275">Když kliknete na dlaždici CloudPassage na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci CloudPassage.</span><span class="sxs-lookup"><span data-stu-id="e4df8-275">When you click the CloudPassage tile in the Access Panel, you should get automatically signed-on to your CloudPassage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4df8-276">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e4df8-276">Additional resources</span></span>

* [<span data-ttu-id="e4df8-277">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e4df8-277">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e4df8-278">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e4df8-278">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_203.png

