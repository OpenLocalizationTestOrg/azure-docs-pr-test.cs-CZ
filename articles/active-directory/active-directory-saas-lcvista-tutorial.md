---
title: 'Kurz: Azure Active Directory integrace s LCVista | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a LCVista."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8db80d6e-3275-419f-aa39-6115a7bc9800
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: c19f81da495eb7116b62797d1755d312a23f3805
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lcvista"></a><span data-ttu-id="b7902-103">Kurz: Azure Active Directory integrace s LCVista</span><span class="sxs-lookup"><span data-stu-id="b7902-103">Tutorial: Azure Active Directory integration with LCVista</span></span>

<span data-ttu-id="b7902-104">V tomto kurzu zjistěte, jak integrovat LCVista s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b7902-104">In this tutorial, you learn how to integrate LCVista with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b7902-105">Integrace LCVista s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b7902-105">Integrating LCVista with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b7902-106">Můžete řídit ve službě Azure AD, který má přístup k LCVista</span><span class="sxs-lookup"><span data-stu-id="b7902-106">You can control in Azure AD who has access to LCVista</span></span>
- <span data-ttu-id="b7902-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k LCVista (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7902-107">You can enable your users to automatically get signed-on to LCVista (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b7902-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b7902-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b7902-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b7902-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7902-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b7902-110">Prerequisites</span></span>

<span data-ttu-id="b7902-111">Konfigurace integrace Azure AD s LCVista, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b7902-111">To configure Azure AD integration with LCVista, you need the following items:</span></span>

- <span data-ttu-id="b7902-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7902-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b7902-113">LCVista jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b7902-113">A LCVista single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b7902-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b7902-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b7902-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b7902-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b7902-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b7902-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b7902-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b7902-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b7902-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b7902-118">Scenario description</span></span>
<span data-ttu-id="b7902-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b7902-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b7902-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b7902-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b7902-121">Přidání LCVista z Galerie</span><span class="sxs-lookup"><span data-stu-id="b7902-121">Adding LCVista from the gallery</span></span>
2. <span data-ttu-id="b7902-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b7902-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lcvista-from-the-gallery"></a><span data-ttu-id="b7902-123">Přidání LCVista z Galerie</span><span class="sxs-lookup"><span data-stu-id="b7902-123">Adding LCVista from the gallery</span></span>
<span data-ttu-id="b7902-124">Při konfiguraci integrace LCVista do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS LCVista z galerie.</span><span class="sxs-lookup"><span data-stu-id="b7902-124">To configure the integration of LCVista into Azure AD, you need to add LCVista from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b7902-125">**Pokud chcete přidat LCVista z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b7902-125">**To add LCVista from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b7902-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b7902-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b7902-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b7902-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b7902-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b7902-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b7902-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b7902-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b7902-133">Do vyhledávacího pole zadejte **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="b7902-133">In the search box, type **LCVista**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_search.png)

5. <span data-ttu-id="b7902-135">Na panelu výsledků vyberte **LCVista**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b7902-135">In the results panel, select **LCVista**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b7902-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b7902-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b7902-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s LCVista podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b7902-138">In this section, you configure and test Azure AD single sign-on with LCVista based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b7902-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v LCVista je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7902-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LCVista is to a user in Azure AD.</span></span> <span data-ttu-id="b7902-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v LCVista musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b7902-140">In other words, a link relationship between an Azure AD user and the related user in LCVista needs to be established.</span></span>

<span data-ttu-id="b7902-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v LCVista.</span><span class="sxs-lookup"><span data-stu-id="b7902-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LCVista.</span></span>

<span data-ttu-id="b7902-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s LCVista, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b7902-142">To configure and test Azure AD single sign-on with LCVista, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b7902-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b7902-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b7902-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7902-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b7902-145">**[Vytvoření zkušebního uživatele LCVista](#creating-a-lcvista-test-user)**  – Pokud chcete mít protějšek Britta Simon v LCVista propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b7902-145">**[Creating a LCVista test user](#creating-a-lcvista-test-user)** - to have a counterpart of Britta Simon in LCVista that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b7902-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b7902-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b7902-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b7902-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b7902-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b7902-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b7902-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci LCVista.</span><span class="sxs-lookup"><span data-stu-id="b7902-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LCVista application.</span></span>

<span data-ttu-id="b7902-150">**Ke konfiguraci Azure AD jednotné přihlašování s LCVista, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b7902-150">**To configure Azure AD single sign-on with LCVista, perform the following steps:**</span></span>

1. <span data-ttu-id="b7902-151">Na portálu Azure na **LCVista** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b7902-151">In the Azure portal, on the **LCVista** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b7902-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b7902-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_samlbase.png)

3. <span data-ttu-id="b7902-155">Na **LCVista domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b7902-155">On the **LCVista Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_url.png)

    <span data-ttu-id="b7902-157">a.</span><span class="sxs-lookup"><span data-stu-id="b7902-157">a.</span></span> <span data-ttu-id="b7902-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.lcvista.com/rainier/login`</span><span class="sxs-lookup"><span data-stu-id="b7902-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.lcvista.com/rainier/login`</span></span>

    <span data-ttu-id="b7902-159">b.</span><span class="sxs-lookup"><span data-stu-id="b7902-159">b.</span></span> <span data-ttu-id="b7902-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.lcvista.com`</span><span class="sxs-lookup"><span data-stu-id="b7902-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.lcvista.com`</span></span> 
     
    > [!NOTE] 
    > <span data-ttu-id="b7902-161">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="b7902-161">These values are not the real.</span></span> <span data-ttu-id="b7902-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="b7902-162">Update these values with the actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="b7902-163">Obraťte se na [tým podpory LCVista klienta](https://lcvista.com/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="b7902-163">Contact [LCVista Client support team](https://lcvista.com/contact) to get these values.</span></span> 

4. <span data-ttu-id="b7902-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b7902-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_certificate.png) 

5. <span data-ttu-id="b7902-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b7902-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="b7902-168">Na **LCVista konfigurace** klikněte na tlačítko **konfigurace LCVista** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b7902-168">On the **LCVista Configuration** section, click **Configure LCVista** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b7902-169">Kopírování **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b7902-169">Copy the **SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_configure.png) 

7.  <span data-ttu-id="b7902-171">Přihlásit se k vaší aplikaci LCVista jako správce.</span><span class="sxs-lookup"><span data-stu-id="b7902-171">Sign on to your LCVista application as an administrator.</span></span>

8. <span data-ttu-id="b7902-172">V **konfigurace SAML** část, zkontrolujte **povolit SAML přihlášení** a zadejte podrobnosti, jak je uvedeno v následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="b7902-172">In the **SAML Config** section, check the **Enable SAML login** and enter the details as mentioned in below image.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_config.png)

    <span data-ttu-id="b7902-174">a.</span><span class="sxs-lookup"><span data-stu-id="b7902-174">a.</span></span> <span data-ttu-id="b7902-175">Vložení **URL vystavitele** který jste zkopírovali z Azure AD v **Entity ID** části.</span><span class="sxs-lookup"><span data-stu-id="b7902-175">Paste the **Issuer URL** which you have copied from Azure AD in the **Entity ID** section.</span></span> 

    <span data-ttu-id="b7902-176">b.</span><span class="sxs-lookup"><span data-stu-id="b7902-176">b.</span></span> <span data-ttu-id="b7902-177">Vložení **jeden přihlašování adresa URL služby** který jste zkopírovali z Azure AD v **URL** části.</span><span class="sxs-lookup"><span data-stu-id="b7902-177">Paste the **Single Sign-On Service URL** which you have copied from Azure AD in the **URL** section.</span></span>

    <span data-ttu-id="b7902-178">c.</span><span class="sxs-lookup"><span data-stu-id="b7902-178">c.</span></span> <span data-ttu-id="b7902-179">Z metadat (XML), který jste si stáhli z portálu Azure, zkopírujte hodnotu **certifikátu x 509** a vložte jej do **x509 certifikátu** části.</span><span class="sxs-lookup"><span data-stu-id="b7902-179">From Metadata (XML) which you have downloaded from Azure portal, copy the value **X509Certificate** and paste it in the **x509 Certificate** section.</span></span>

    <span data-ttu-id="b7902-180">d.</span><span class="sxs-lookup"><span data-stu-id="b7902-180">d.</span></span> <span data-ttu-id="b7902-181">V **křestní jméno atribut** textovému poli, vložit hodnotu `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="b7902-181">In the **First name attribute** textbox, paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>

    <span data-ttu-id="b7902-182">e.</span><span class="sxs-lookup"><span data-stu-id="b7902-182">e.</span></span> <span data-ttu-id="b7902-183">V **poslední atribut name** textovému poli, vložit hodnotu `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="b7902-183">In the **Last name attribute** textbox, paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>

    <span data-ttu-id="b7902-184">f.</span><span class="sxs-lookup"><span data-stu-id="b7902-184">f.</span></span> <span data-ttu-id="b7902-185">V **atribut e-mailu** textovému poli, vložit hodnotu `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="b7902-185">In the **Email attribute** textbox, paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="b7902-186">g.</span><span class="sxs-lookup"><span data-stu-id="b7902-186">g.</span></span> <span data-ttu-id="b7902-187">V **uživatelské jméno atribut** textovému poli, vložit hodnotu `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="b7902-187">In the **Username attribute** textbox, paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>

    <span data-ttu-id="b7902-188">e.</span><span class="sxs-lookup"><span data-stu-id="b7902-188">e.</span></span> <span data-ttu-id="b7902-189">Klikněte na tlačítko **Uložit** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="b7902-189">Click **Save** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="b7902-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b7902-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b7902-191">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b7902-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b7902-192">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b7902-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b7902-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7902-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="b7902-194">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7902-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b7902-196">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b7902-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b7902-197">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b7902-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b7902-199">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b7902-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b7902-201">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b7902-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b7902-203">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b7902-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b7902-205">a.</span><span class="sxs-lookup"><span data-stu-id="b7902-205">a.</span></span> <span data-ttu-id="b7902-206">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b7902-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b7902-207">b.</span><span class="sxs-lookup"><span data-stu-id="b7902-207">b.</span></span> <span data-ttu-id="b7902-208">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b7902-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b7902-209">c.</span><span class="sxs-lookup"><span data-stu-id="b7902-209">c.</span></span> <span data-ttu-id="b7902-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b7902-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b7902-211">d.</span><span class="sxs-lookup"><span data-stu-id="b7902-211">d.</span></span> <span data-ttu-id="b7902-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b7902-212">Click **Create**.</span></span>
 
### <a name="creating-a-lcvista-test-user"></a><span data-ttu-id="b7902-213">Vytvoření zkušebního uživatele LCVista</span><span class="sxs-lookup"><span data-stu-id="b7902-213">Creating a LCVista test user</span></span>

<span data-ttu-id="b7902-214">V této části vytvoříte volal Britta Simon v LCVista uživatele.</span><span class="sxs-lookup"><span data-stu-id="b7902-214">In this section, you create a user called Britta Simon in LCVista.</span></span> <span data-ttu-id="b7902-215">Budete muset kontaktovat [tým podpory LCVista klienta](https://lcvista.com/contact) přidejte uživatele v aplikaci LCVista.</span><span class="sxs-lookup"><span data-stu-id="b7902-215">You need to contact [LCVista Client support team](https://lcvista.com/contact) to add the users in the LCVista application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b7902-216">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7902-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b7902-217">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu LCVista.</span><span class="sxs-lookup"><span data-stu-id="b7902-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LCVista.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b7902-219">**Pokud chcete přiřadit Britta Simon LCVista, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b7902-219">**To assign Britta Simon to LCVista, perform the following steps:**</span></span>

1. <span data-ttu-id="b7902-220">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b7902-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b7902-222">V seznamu aplikací vyberte **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="b7902-222">In the applications list, select **LCVista**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_app.png) 

3. <span data-ttu-id="b7902-224">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b7902-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b7902-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b7902-226">Click **Add** button.</span></span> <span data-ttu-id="b7902-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b7902-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b7902-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b7902-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b7902-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b7902-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b7902-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b7902-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b7902-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b7902-232">Testing single sign-on</span></span>

<span data-ttu-id="b7902-233">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b7902-233">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> <span data-ttu-id="b7902-234">Klikněte na dlaždici LCVista na přístupovém panelu, budete přesměrováni na organizaci přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="b7902-234">Click the LCVista tile in the Access Panel, you will be redirected to Organization sign on page.</span></span> <span data-ttu-id="b7902-235">Po úspěšném přihlášení můžete se být přihlášení k aplikaci LCVista.</span><span class="sxs-lookup"><span data-stu-id="b7902-235">After successful login, you will be signed-on to your LCVista application.</span></span> <span data-ttu-id="b7902-236">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="b7902-236">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7902-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b7902-237">Additional resources</span></span>

* [<span data-ttu-id="b7902-238">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b7902-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7902-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b7902-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_203.png

