---
title: 'Kurz: Azure Active Directory integrace s BenefitHub | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a BenefitHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4069fe32-a452-463f-973e-7aa0baa4c2fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 8df9c0d8443d6685253207ed1915c780275014fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefithub"></a><span data-ttu-id="772cd-103">Kurz: Azure Active Directory integrace s BenefitHub</span><span class="sxs-lookup"><span data-stu-id="772cd-103">Tutorial: Azure Active Directory integration with BenefitHub</span></span>

<span data-ttu-id="772cd-104">V tomto kurzu zjistěte, jak integrovat BenefitHub s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="772cd-104">In this tutorial, you learn how to integrate BenefitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="772cd-105">Integrace BenefitHub s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="772cd-105">Integrating BenefitHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="772cd-106">Můžete řídit ve službě Azure AD, který má přístup k BenefitHub</span><span class="sxs-lookup"><span data-stu-id="772cd-106">You can control in Azure AD who has access to BenefitHub</span></span>
- <span data-ttu-id="772cd-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k BenefitHub (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="772cd-107">You can enable your users to automatically get signed-on to BenefitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="772cd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="772cd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="772cd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="772cd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="772cd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="772cd-110">Prerequisites</span></span>

<span data-ttu-id="772cd-111">Konfigurace integrace Azure AD s BenefitHub, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="772cd-111">To configure Azure AD integration with BenefitHub, you need the following items:</span></span>

- <span data-ttu-id="772cd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="772cd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="772cd-113">BenefitHub jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="772cd-113">A BenefitHub single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="772cd-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="772cd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="772cd-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="772cd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="772cd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="772cd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="772cd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="772cd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="772cd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="772cd-118">Scenario description</span></span>
<span data-ttu-id="772cd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="772cd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="772cd-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="772cd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="772cd-121">Přidání BenefitHub z Galerie</span><span class="sxs-lookup"><span data-stu-id="772cd-121">Adding BenefitHub from the gallery</span></span>
2. <span data-ttu-id="772cd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="772cd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benefithub-from-the-gallery"></a><span data-ttu-id="772cd-123">Přidání BenefitHub z Galerie</span><span class="sxs-lookup"><span data-stu-id="772cd-123">Adding BenefitHub from the gallery</span></span>
<span data-ttu-id="772cd-124">Při konfiguraci integrace BenefitHub do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS BenefitHub z galerie.</span><span class="sxs-lookup"><span data-stu-id="772cd-124">To configure the integration of BenefitHub into Azure AD, you need to add BenefitHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="772cd-125">**Pokud chcete přidat BenefitHub z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="772cd-125">**To add BenefitHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="772cd-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="772cd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="772cd-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="772cd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="772cd-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="772cd-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="772cd-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="772cd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="772cd-133">Do vyhledávacího pole zadejte **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="772cd-133">In the search box, type **BenefitHub**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_search.png)

5. <span data-ttu-id="772cd-135">Na panelu výsledků vyberte **BenefitHub**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="772cd-135">In the results panel, select **BenefitHub**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="772cd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="772cd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="772cd-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BenefitHub podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="772cd-138">In this section, you configure and test Azure AD single sign-on with BenefitHub based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="772cd-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v BenefitHub je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="772cd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BenefitHub is to a user in Azure AD.</span></span> <span data-ttu-id="772cd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v BenefitHub musí navázat.</span><span class="sxs-lookup"><span data-stu-id="772cd-140">In other words, a link relationship between an Azure AD user and the related user in BenefitHub needs to be established.</span></span>

<span data-ttu-id="772cd-141">V BenefitHub, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="772cd-141">In BenefitHub, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="772cd-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s BenefitHub, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="772cd-142">To configure and test Azure AD single sign-on with BenefitHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="772cd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="772cd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="772cd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="772cd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="772cd-145">**[Vytvoření zkušebního uživatele BenefitHub](#creating-a-benefithub-test-user)**  – Pokud chcete mít protějšek Britta Simon v BenefitHub propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="772cd-145">**[Creating a BenefitHub test user](#creating-a-benefithub-test-user)** - to have a counterpart of Britta Simon in BenefitHub that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="772cd-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="772cd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="772cd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="772cd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="772cd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="772cd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="772cd-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci BenefitHub.</span><span class="sxs-lookup"><span data-stu-id="772cd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BenefitHub application.</span></span>

<span data-ttu-id="772cd-150">**Ke konfiguraci Azure AD jednotné přihlašování s BenefitHub, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="772cd-150">**To configure Azure AD single sign-on with BenefitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="772cd-151">Na portálu Azure na **BenefitHub** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="772cd-151">In the Azure portal, on the **BenefitHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="772cd-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="772cd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_samlbase.png)

3. <span data-ttu-id="772cd-155">Na **BenefitHub domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="772cd-155">On the **BenefitHub Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_url1.png)
  
    <span data-ttu-id="772cd-157">a.</span><span class="sxs-lookup"><span data-stu-id="772cd-157">a.</span></span> <span data-ttu-id="772cd-158">V **identifikátor** textovému poli, typ:`urn:benefithub:passport`</span><span class="sxs-lookup"><span data-stu-id="772cd-158">In the **Identifier** textbox, type: `urn:benefithub:passport`</span></span>
    
    <span data-ttu-id="772cd-159">b.</span><span class="sxs-lookup"><span data-stu-id="772cd-159">b.</span></span> <span data-ttu-id="772cd-160">V **adresa URL odpovědi** textovému poli, typ:`https://passport.benefithub.info/saml/post/ac`</span><span class="sxs-lookup"><span data-stu-id="772cd-160">In the **Reply URL** textbox, type: `https://passport.benefithub.info/saml/post/ac`</span></span>

4. <span data-ttu-id="772cd-161">Aplikace BenefitHub očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="772cd-161">The BenefitHub application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="772cd-162">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="772cd-162">Configure the following claims for this application.</span></span> <span data-ttu-id="772cd-163">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="772cd-163">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_attribute.png)

5. <span data-ttu-id="772cd-165">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je vidět na předchozím obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="772cd-165">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the preceding image and perform the following steps:</span></span>
    
    | <span data-ttu-id="772cd-166">Název atributu</span><span class="sxs-lookup"><span data-stu-id="772cd-166">Attribute Name</span></span> | <span data-ttu-id="772cd-167">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="772cd-167">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="772cd-168">kódu organizace</span><span class="sxs-lookup"><span data-stu-id="772cd-168">organizationid</span></span> | <span data-ttu-id="772cd-169">< kódu organizace ></span><span class="sxs-lookup"><span data-stu-id="772cd-169">< organizationid ></span></span> |

    > [!NOTE]
    > <span data-ttu-id="772cd-170">Hodnota tohoto atributu není skutečné.</span><span class="sxs-lookup"><span data-stu-id="772cd-170">This attribute value is not real.</span></span> <span data-ttu-id="772cd-171">Aktualizujte tuto hodnotu s skutečné kódu organizace.</span><span class="sxs-lookup"><span data-stu-id="772cd-171">Update this value with actual organizationid.</span></span> <span data-ttu-id="772cd-172">Obraťte se na [tým podpory BenefitHub](https://www.benefithub.com/Home/ContactUs) Chcete-li získat skutečný kódu organizace.</span><span class="sxs-lookup"><span data-stu-id="772cd-172">Contact [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) to get the actual organizationid.</span></span>
    
    <span data-ttu-id="772cd-173">a.</span><span class="sxs-lookup"><span data-stu-id="772cd-173">a.</span></span> <span data-ttu-id="772cd-174">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="772cd-174">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="772cd-177">b.</span><span class="sxs-lookup"><span data-stu-id="772cd-177">b.</span></span> <span data-ttu-id="772cd-178">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="772cd-178">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="772cd-179">c.</span><span class="sxs-lookup"><span data-stu-id="772cd-179">c.</span></span> <span data-ttu-id="772cd-180">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="772cd-180">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="772cd-181">d.</span><span class="sxs-lookup"><span data-stu-id="772cd-181">d.</span></span> <span data-ttu-id="772cd-182">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="772cd-182">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="772cd-183">Před konfigurací kontrolního výrazu SAML, budete muset kontaktovat vaše [BenefitHub podporu](https://www.benefithub.com/Home/ContactUs) a požadovat hodnotu atributu jedinečný identifikátor pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="772cd-183">Before you can configure the SAML assertion, you need to contact your [BenefitHub support](https://www.benefithub.com/Home/ContactUs) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="772cd-184">Je nutné tuto hodnotu ke konfiguraci vlastních deklarací identity pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="772cd-184">You need this value to configure the custom claim for your application.</span></span>

6. <span data-ttu-id="772cd-185">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="772cd-185">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_certificate.png) 

7. <span data-ttu-id="772cd-187">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="772cd-187">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefithub-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="772cd-189">Konfigurace jednotného přihlašování na **BenefitHub** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory BenefitHub](https://www.benefithub.com/Home/ContactUs).</span><span class="sxs-lookup"><span data-stu-id="772cd-189">To configure single sign-on on **BenefitHub** side, you need to send the downloaded **Metadata XML** to [BenefitHub support team](https://www.benefithub.com/Home/ContactUs).</span></span> <span data-ttu-id="772cd-190">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="772cd-190">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="772cd-191">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="772cd-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="772cd-192">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="772cd-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="772cd-193">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="772cd-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="772cd-194">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="772cd-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="772cd-195">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="772cd-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="772cd-197">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="772cd-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="772cd-198">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="772cd-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="772cd-200">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="772cd-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="772cd-202">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="772cd-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="772cd-204">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="772cd-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benefithub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="772cd-206">a.</span><span class="sxs-lookup"><span data-stu-id="772cd-206">a.</span></span> <span data-ttu-id="772cd-207">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="772cd-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="772cd-208">b.</span><span class="sxs-lookup"><span data-stu-id="772cd-208">b.</span></span> <span data-ttu-id="772cd-209">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="772cd-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="772cd-210">c.</span><span class="sxs-lookup"><span data-stu-id="772cd-210">c.</span></span> <span data-ttu-id="772cd-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="772cd-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="772cd-212">d.</span><span class="sxs-lookup"><span data-stu-id="772cd-212">d.</span></span> <span data-ttu-id="772cd-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="772cd-213">Click **Create**.</span></span>
 
### <a name="creating-a-benefithub-test-user"></a><span data-ttu-id="772cd-214">Vytvoření zkušebního uživatele BenefitHub</span><span class="sxs-lookup"><span data-stu-id="772cd-214">Creating a BenefitHub test user</span></span>

<span data-ttu-id="772cd-215">V této části vytvoříte volal Britta Simon v BenefitHub uživatele.</span><span class="sxs-lookup"><span data-stu-id="772cd-215">In this section, you create a user called Britta Simon in BenefitHub.</span></span> <span data-ttu-id="772cd-216">Práce s [tým podpory BenefitHub](https://www.benefithub.com/Home/ContactUs) přidat uživatele do BenefitHub platformy.</span><span class="sxs-lookup"><span data-stu-id="772cd-216">Work with [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) to add the users in the BenefitHub platform.</span></span> <span data-ttu-id="772cd-217">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="772cd-217">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="772cd-218">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="772cd-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="772cd-219">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu BenefitHub.</span><span class="sxs-lookup"><span data-stu-id="772cd-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BenefitHub.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="772cd-221">**Pokud chcete přiřadit Britta Simon BenefitHub, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="772cd-221">**To assign Britta Simon to BenefitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="772cd-222">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="772cd-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="772cd-224">V seznamu aplikací vyberte **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="772cd-224">In the applications list, select **BenefitHub**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_app.png) 

3. <span data-ttu-id="772cd-226">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="772cd-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="772cd-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="772cd-228">Click **Add** button.</span></span> <span data-ttu-id="772cd-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="772cd-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="772cd-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="772cd-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="772cd-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="772cd-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="772cd-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="772cd-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="772cd-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="772cd-234">Testing single sign-on</span></span>

<span data-ttu-id="772cd-235">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="772cd-235">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="772cd-236">Když kliknete na dlaždici BenefitHub na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci BenefitHub.</span><span class="sxs-lookup"><span data-stu-id="772cd-236">When you click the BenefitHub tile in the Access Panel, you should get automatically signed-on to your BenefitHub application.</span></span>
<span data-ttu-id="772cd-237">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="772cd-237">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="772cd-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="772cd-238">Additional resources</span></span>

* [<span data-ttu-id="772cd-239">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="772cd-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="772cd-240">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="772cd-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_203.png

