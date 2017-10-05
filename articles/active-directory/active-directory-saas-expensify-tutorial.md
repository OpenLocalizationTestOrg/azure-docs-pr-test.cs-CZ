---
title: 'Kurz: Azure Active Directory integrace s Expensify | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Expensify."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1e761484-7a2f-4321-91f4-6d5d0b69344e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: jeedes
ms.openlocfilehash: e45576fd92706881121469ccd82150b3d48059cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-expensify"></a><span data-ttu-id="5cfca-103">Kurz: Azure Active Directory integrace s Expensify</span><span class="sxs-lookup"><span data-stu-id="5cfca-103">Tutorial: Azure Active Directory integration with Expensify</span></span>

<span data-ttu-id="5cfca-104">V tomto kurzu zjistěte, jak integrovat Expensify s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5cfca-104">In this tutorial, you learn how to integrate Expensify with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5cfca-105">Integrace Expensify s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5cfca-105">Integrating Expensify with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5cfca-106">Můžete řídit ve službě Azure AD, který má přístup k Expensify</span><span class="sxs-lookup"><span data-stu-id="5cfca-106">You can control in Azure AD who has access to Expensify</span></span>
- <span data-ttu-id="5cfca-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Expensify (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cfca-107">You can enable your users to automatically get signed-on to Expensify (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5cfca-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5cfca-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5cfca-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5cfca-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5cfca-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5cfca-110">Prerequisites</span></span>

<span data-ttu-id="5cfca-111">Konfigurace integrace Azure AD s Expensify, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5cfca-111">To configure Azure AD integration with Expensify, you need the following items:</span></span>

- <span data-ttu-id="5cfca-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cfca-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5cfca-113">Expensify jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5cfca-113">An Expensify single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5cfca-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5cfca-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5cfca-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5cfca-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5cfca-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5cfca-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5cfca-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5cfca-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5cfca-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5cfca-118">Scenario description</span></span>
<span data-ttu-id="5cfca-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5cfca-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5cfca-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5cfca-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5cfca-121">Přidání Expensify z Galerie</span><span class="sxs-lookup"><span data-stu-id="5cfca-121">Adding Expensify from the gallery</span></span>
2. <span data-ttu-id="5cfca-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cfca-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-expensify-from-the-gallery"></a><span data-ttu-id="5cfca-123">Přidání Expensify z Galerie</span><span class="sxs-lookup"><span data-stu-id="5cfca-123">Adding Expensify from the gallery</span></span>
<span data-ttu-id="5cfca-124">Při konfiguraci integrace Expensify do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Expensify z galerie.</span><span class="sxs-lookup"><span data-stu-id="5cfca-124">To configure the integration of Expensify into Azure AD, you need to add Expensify from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5cfca-125">**Pokud chcete přidat Expensify z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5cfca-125">**To add Expensify from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5cfca-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5cfca-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5cfca-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5cfca-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5cfca-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cfca-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5cfca-133">Do vyhledávacího pole zadejte **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-133">In the search box, type **Expensify**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_search.png)

5. <span data-ttu-id="5cfca-135">Na panelu výsledků vyberte **Expensify**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5cfca-135">In the results panel, select **Expensify**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5cfca-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cfca-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5cfca-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Expensify podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5cfca-138">In this section, you configure and test Azure AD single sign-on with Expensify based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5cfca-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Expensify je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cfca-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Expensify is to a user in Azure AD.</span></span> <span data-ttu-id="5cfca-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Expensify musí navázat.</span><span class="sxs-lookup"><span data-stu-id="5cfca-140">In other words, a link relationship between an Azure AD user and the related user in Expensify needs to be established.</span></span>

<span data-ttu-id="5cfca-141">V Expensify, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5cfca-141">In Expensify, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5cfca-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Expensify, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5cfca-142">To configure and test Azure AD single sign-on with Expensify, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5cfca-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5cfca-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5cfca-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5cfca-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5cfca-145">**[Vytváření testovacího uživatele Expensify](#creating-an-expensify-test-user)**  – Pokud chcete mít protějšek Britta Simon v Expensify propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5cfca-145">**[Creating an Expensify test user](#creating-an-expensify-test-user)** - to have a counterpart of Britta Simon in Expensify that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5cfca-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cfca-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5cfca-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5cfca-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5cfca-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cfca-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5cfca-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Expensify.</span><span class="sxs-lookup"><span data-stu-id="5cfca-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Expensify application.</span></span>

<span data-ttu-id="5cfca-150">**Ke konfiguraci Azure AD jednotné přihlašování s Expensify, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5cfca-150">**To configure Azure AD single sign-on with Expensify, perform the following steps:**</span></span>

1. <span data-ttu-id="5cfca-151">Na portálu Azure na **Expensify** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-151">In the Azure portal, on the **Expensify** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5cfca-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cfca-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_samlbase.png)

3. <span data-ttu-id="5cfca-155">Na **Expensify domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5cfca-155">On the **Expensify Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_url.png)

    <span data-ttu-id="5cfca-157">a.</span><span class="sxs-lookup"><span data-stu-id="5cfca-157">a.</span></span> <span data-ttu-id="5cfca-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.expensify.com/authentication/saml/login`</span><span class="sxs-lookup"><span data-stu-id="5cfca-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.expensify.com/authentication/saml/login`</span></span>

    <span data-ttu-id="5cfca-159">b.</span><span class="sxs-lookup"><span data-stu-id="5cfca-159">b.</span></span> <span data-ttu-id="5cfca-160">V **identifikátoru adresy URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.<companyname>.expensify.com/`</span><span class="sxs-lookup"><span data-stu-id="5cfca-160">In the **Identifier URL** textbox, type a URL using the following pattern: `https://www.<companyname>.expensify.com/`</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="5cfca-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5cfca-161">These values are not real.</span></span> <span data-ttu-id="5cfca-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátoru adresy URL.</span><span class="sxs-lookup"><span data-stu-id="5cfca-162">Update these values with the actual Sign-On URL and Identifier URL.</span></span> <span data-ttu-id="5cfca-163">Obraťte se na [tým podpory Expensify klienta](mailto:help@expensify.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5cfca-163">Contact [Expensify Client support team](mailto:help@expensify.com) to get these values.</span></span> 
 
4. <span data-ttu-id="5cfca-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5cfca-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_certificate.png) 

5. <span data-ttu-id="5cfca-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cfca-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5cfca-168">Pokud chcete povolit jednotné přihlašování v Expensify, musíte nejprve povolit **domény řízení** v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5cfca-168">To enable SSO in Expensify, you first need to enable **Domain Control** in the application.</span></span> <span data-ttu-id="5cfca-169">Ovládací prvek domény můžete povolit v aplikaci pomocí kroků uvedených v tomto [zde](http://help.expensify.com/domain-control).</span><span class="sxs-lookup"><span data-stu-id="5cfca-169">You can enable Domain Control in the application through the steps listed [here](http://help.expensify.com/domain-control).</span></span> <span data-ttu-id="5cfca-170">Pro další podporu, pracovat s [tým podpory Expensify klienta](mailto:help@expensify.com).</span><span class="sxs-lookup"><span data-stu-id="5cfca-170">For additional support, work with [Expensify Client support team](mailto:help@expensify.com).</span></span> <span data-ttu-id="5cfca-171">Až budete mít povoleno řízení domény, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5cfca-171">Once you have Domain Control enabled, follow these steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_51.png)
    
    <span data-ttu-id="5cfca-173">a.</span><span class="sxs-lookup"><span data-stu-id="5cfca-173">a.</span></span> <span data-ttu-id="5cfca-174">Přihlásit se k vaší aplikaci Expensify.</span><span class="sxs-lookup"><span data-stu-id="5cfca-174">Sign on to your Expensify application.</span></span>
    
    <span data-ttu-id="5cfca-175">b.</span><span class="sxs-lookup"><span data-stu-id="5cfca-175">b.</span></span> <span data-ttu-id="5cfca-176">Na panelu nástrojů v horní části klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-176">In the toolbar on the top, click **Admin**.</span></span>
    
    <span data-ttu-id="5cfca-177">c.</span><span class="sxs-lookup"><span data-stu-id="5cfca-177">c.</span></span> <span data-ttu-id="5cfca-178">V levém panelu klikněte na **domény**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-178">In the left panel, click **Domain**.</span></span>
    
    <span data-ttu-id="5cfca-179">d.</span><span class="sxs-lookup"><span data-stu-id="5cfca-179">d.</span></span> <span data-ttu-id="5cfca-180">Klikněte na název ověřené domény.</span><span class="sxs-lookup"><span data-stu-id="5cfca-180">Click your verified domain name.</span></span>
    
    <span data-ttu-id="5cfca-181">e.</span><span class="sxs-lookup"><span data-stu-id="5cfca-181">e.</span></span> <span data-ttu-id="5cfca-182">V levém panelu klikněte na **SAML**a potom vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-182">In the left panel, click **SAML**, and then select **Enabled**.</span></span>
    
    <span data-ttu-id="5cfca-183">f.</span><span class="sxs-lookup"><span data-stu-id="5cfca-183">f.</span></span> <span data-ttu-id="5cfca-184">Z Azure AD v poznámkovém bloku otevřete stažené federačních metadat, kopírovat obsah a vložte ji do **metadat zprostředkovatelů Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5cfca-184">Open the downloaded Federation Metadata from Azure AD in notepad, copy the content, and then paste it into the **Identity Provider Metadata** textbox.</span></span>

> [!TIP]
> <span data-ttu-id="5cfca-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5cfca-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5cfca-186">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5cfca-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5cfca-187">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5cfca-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5cfca-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cfca-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="5cfca-189">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5cfca-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5cfca-191">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5cfca-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5cfca-192">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5cfca-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5cfca-194">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5cfca-196">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cfca-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5cfca-198">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5cfca-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5cfca-200">a.</span><span class="sxs-lookup"><span data-stu-id="5cfca-200">a.</span></span> <span data-ttu-id="5cfca-201">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5cfca-202">b.</span><span class="sxs-lookup"><span data-stu-id="5cfca-202">b.</span></span> <span data-ttu-id="5cfca-203">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5cfca-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5cfca-204">c.</span><span class="sxs-lookup"><span data-stu-id="5cfca-204">c.</span></span> <span data-ttu-id="5cfca-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5cfca-206">d.</span><span class="sxs-lookup"><span data-stu-id="5cfca-206">d.</span></span> <span data-ttu-id="5cfca-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-207">Click **Create**.</span></span>
 
### <a name="creating-an-expensify-test-user"></a><span data-ttu-id="5cfca-208">Vytváření testovacího uživatele Expensify</span><span class="sxs-lookup"><span data-stu-id="5cfca-208">Creating an Expensify test user</span></span>

<span data-ttu-id="5cfca-209">V této části vytvoříte volal Britta Simon v Expensify uživatele.</span><span class="sxs-lookup"><span data-stu-id="5cfca-209">In this section, you create a user called Britta Simon in Expensify.</span></span> <span data-ttu-id="5cfca-210">Práce s [tým podpory Expensify klienta](mailto:help@expensify.com) přidat uživatele do Expensify platformy.</span><span class="sxs-lookup"><span data-stu-id="5cfca-210">Work with [Expensify Client support team](mailto:help@expensify.com) to add the users in the Expensify platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5cfca-211">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cfca-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5cfca-212">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Expensify.</span><span class="sxs-lookup"><span data-stu-id="5cfca-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Expensify.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5cfca-214">**Pokud chcete přiřadit Britta Simon Expensify, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5cfca-214">**To assign Britta Simon to Expensify, perform the following steps:**</span></span>

1. <span data-ttu-id="5cfca-215">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5cfca-217">V seznamu aplikací vyberte **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-217">In the applications list, select **Expensify**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_app.png) 

3. <span data-ttu-id="5cfca-219">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5cfca-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5cfca-221">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cfca-221">Click **Add** button.</span></span> <span data-ttu-id="5cfca-222">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cfca-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5cfca-224">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5cfca-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5cfca-225">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cfca-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5cfca-226">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cfca-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5cfca-227">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cfca-227">Testing single sign-on</span></span>

<span data-ttu-id="5cfca-228">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5cfca-228">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="5cfca-229">Když kliknete na dlaždici Expensify na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Expensify.</span><span class="sxs-lookup"><span data-stu-id="5cfca-229">When you click the Expensify tile in the Access Panel, you should get automatically signed-on to your Expensify application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5cfca-230">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5cfca-230">Additional resources</span></span>

* [<span data-ttu-id="5cfca-231">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5cfca-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5cfca-232">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5cfca-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_203.png

