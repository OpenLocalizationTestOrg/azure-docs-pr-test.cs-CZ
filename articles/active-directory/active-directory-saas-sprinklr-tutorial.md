---
title: 'Kurz: Azure Active Directory integrace s Sprinklr | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Sprinklr."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 6e1622cd55e3b0e8063604ac9dc0cb0673fa9753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a><span data-ttu-id="abf7c-103">Kurz: Azure Active Directory integrace s Sprinklr</span><span class="sxs-lookup"><span data-stu-id="abf7c-103">Tutorial: Azure Active Directory integration with Sprinklr</span></span>

<span data-ttu-id="abf7c-104">V tomto kurzu zjistěte, jak integrovat Sprinklr s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="abf7c-104">In this tutorial, you learn how to integrate Sprinklr with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="abf7c-105">Integrace Sprinklr s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="abf7c-105">Integrating Sprinklr with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="abf7c-106">Můžete řídit ve službě Azure AD, který má přístup k Sprinklr</span><span class="sxs-lookup"><span data-stu-id="abf7c-106">You can control in Azure AD who has access to Sprinklr</span></span>
- <span data-ttu-id="abf7c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Sprinklr (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="abf7c-107">You can enable your users to automatically get signed-on to Sprinklr (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="abf7c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="abf7c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="abf7c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="abf7c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abf7c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="abf7c-110">Prerequisites</span></span>

<span data-ttu-id="abf7c-111">Konfigurace integrace Azure AD s Sprinklr, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="abf7c-111">To configure Azure AD integration with Sprinklr, you need the following items:</span></span>

- <span data-ttu-id="abf7c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="abf7c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="abf7c-113">Sprinklr jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="abf7c-113">A Sprinklr single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="abf7c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="abf7c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="abf7c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="abf7c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="abf7c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="abf7c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="abf7c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="abf7c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="abf7c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="abf7c-118">Scenario description</span></span>
<span data-ttu-id="abf7c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="abf7c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="abf7c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="abf7c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="abf7c-121">Přidání Sprinklr z Galerie</span><span class="sxs-lookup"><span data-stu-id="abf7c-121">Adding Sprinklr from the gallery</span></span>
2. <span data-ttu-id="abf7c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="abf7c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sprinklr-from-the-gallery"></a><span data-ttu-id="abf7c-123">Přidání Sprinklr z Galerie</span><span class="sxs-lookup"><span data-stu-id="abf7c-123">Adding Sprinklr from the gallery</span></span>
<span data-ttu-id="abf7c-124">Při konfiguraci integrace Sprinklr do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Sprinklr z galerie.</span><span class="sxs-lookup"><span data-stu-id="abf7c-124">To configure the integration of Sprinklr into Azure AD, you need to add Sprinklr from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="abf7c-125">**Pokud chcete přidat Sprinklr z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="abf7c-125">**To add Sprinklr from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="abf7c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="abf7c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="abf7c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="abf7c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="abf7c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="abf7c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="abf7c-133">Do vyhledávacího pole zadejte **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-133">In the search box, type **Sprinklr**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. <span data-ttu-id="abf7c-135">Na panelu výsledků vyberte **Sprinklr**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="abf7c-135">In the results panel, select **Sprinklr**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="abf7c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="abf7c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="abf7c-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sprinklr podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="abf7c-138">In this section, you configure and test Azure AD single sign-on with Sprinklr based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="abf7c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Sprinklr je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="abf7c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Sprinklr is to a user in Azure AD.</span></span> <span data-ttu-id="abf7c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Sprinklr musí navázat.</span><span class="sxs-lookup"><span data-stu-id="abf7c-140">In other words, a link relationship between an Azure AD user and the related user in Sprinklr needs to be established.</span></span>

<span data-ttu-id="abf7c-141">V Sprinklr, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="abf7c-141">In Sprinklr, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="abf7c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sprinklr, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="abf7c-142">To configure and test Azure AD single sign-on with Sprinklr, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="abf7c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="abf7c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="abf7c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="abf7c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="abf7c-145">**[Vytvoření zkušebního uživatele Sprinklr](#creating-a-sprinklr-test-user)**  – Pokud chcete mít protějšek Britta Simon v Sprinklr propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="abf7c-145">**[Creating a Sprinklr test user](#creating-a-sprinklr-test-user)** - to have a counterpart of Britta Simon in Sprinklr that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="abf7c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="abf7c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="abf7c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="abf7c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="abf7c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="abf7c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="abf7c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Sprinklr.</span><span class="sxs-lookup"><span data-stu-id="abf7c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sprinklr application.</span></span>

<span data-ttu-id="abf7c-150">**Ke konfiguraci Azure AD jednotné přihlašování s Sprinklr, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="abf7c-150">**To configure Azure AD single sign-on with Sprinklr, perform the following steps:**</span></span>

1. <span data-ttu-id="abf7c-151">Na portálu Azure na **Sprinklr** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-151">In the Azure portal, on the **Sprinklr** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="abf7c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="abf7c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. <span data-ttu-id="abf7c-155">Na **Sprinklr domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="abf7c-155">On the **Sprinklr Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    <span data-ttu-id="abf7c-157">a.</span><span class="sxs-lookup"><span data-stu-id="abf7c-157">a.</span></span> <span data-ttu-id="abf7c-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="abf7c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    <span data-ttu-id="abf7c-159">b.</span><span class="sxs-lookup"><span data-stu-id="abf7c-159">b.</span></span> <span data-ttu-id="abf7c-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="abf7c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="abf7c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="abf7c-161">These values are not real.</span></span> <span data-ttu-id="abf7c-162">Aktualizujte hodnotu s skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="abf7c-162">Update the value with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="abf7c-163">Obraťte se na [tým podpory Sprinklr klienta](https://www.sprinklr.com/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="abf7c-163">Contact [Sprinklr Client support team](https://www.sprinklr.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="abf7c-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="abf7c-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. <span data-ttu-id="abf7c-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="abf7c-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="abf7c-168">Na **Sprinklr konfigurace** klikněte na tlačítko **konfigurace Sprinklr** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="abf7c-168">On the **Sprinklr Configuration** section, click **Configure Sprinklr** to open **Configure sign-on** window.</span></span> <span data-ttu-id="abf7c-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="abf7c-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

7. <span data-ttu-id="abf7c-170">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti Sprinklr jako správce.</span><span class="sxs-lookup"><span data-stu-id="abf7c-170">In a different web browser window, log in to your Sprinklr company site as an administrator.</span></span>

8. <span data-ttu-id="abf7c-171">Přejděte na **správy \> nastavení**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-171">Go to **Administration \> Settings**.</span></span>
   
    <span data-ttu-id="abf7c-172">![Správa](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "správy")</span><span class="sxs-lookup"><span data-stu-id="abf7c-172">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

9. <span data-ttu-id="abf7c-173">Přejděte na **spravovat partnera \> jednotné přihlašování** na v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="abf7c-173">Go to **Manage Partner \> Single Sign** on from the left pane.</span></span>
   
    <span data-ttu-id="abf7c-174">![Správa partnera](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "spravovat partnera")</span><span class="sxs-lookup"><span data-stu-id="abf7c-174">![Manage Partner](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Manage Partner")</span></span>

10. <span data-ttu-id="abf7c-175">Klikněte na tlačítko **+ jednotné přihlašování, doplňky**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-175">Click **+Add Single Sign Ons**.</span></span>
   
    <span data-ttu-id="abf7c-176">![Jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="abf7c-176">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "Single Sign-Ons")</span></span>

11. <span data-ttu-id="abf7c-177">Na **jednotného přihlašování** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="abf7c-177">On the **Single Sign on** page, perform the following steps:</span></span>
   
    <span data-ttu-id="abf7c-178">![Jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="abf7c-178">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "Single Sign-Ons")</span></span>

    <span data-ttu-id="abf7c-179">a.</span><span class="sxs-lookup"><span data-stu-id="abf7c-179">a.</span></span> <span data-ttu-id="abf7c-180">V **název** textovému poli, zadejte název pro svou konfiguraci (například: *WAADSSOTest*).</span><span class="sxs-lookup"><span data-stu-id="abf7c-180">In the **Name** textbox, type a name for your configuration (for example: *WAADSSOTest*).</span></span>

    <span data-ttu-id="abf7c-181">b.</span><span class="sxs-lookup"><span data-stu-id="abf7c-181">b.</span></span> <span data-ttu-id="abf7c-182">Vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-182">Select **Enabled**.</span></span>

    <span data-ttu-id="abf7c-183">c.</span><span class="sxs-lookup"><span data-stu-id="abf7c-183">c.</span></span> <span data-ttu-id="abf7c-184">Vyberte **používat nový certifikát jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-184">Select **Use new SSO Certificate**.</span></span>
             
    <span data-ttu-id="abf7c-185">e.</span><span class="sxs-lookup"><span data-stu-id="abf7c-185">e.</span></span> <span data-ttu-id="abf7c-186">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte jej do **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="abf7c-186">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="abf7c-187">f.</span><span class="sxs-lookup"><span data-stu-id="abf7c-187">f.</span></span> <span data-ttu-id="abf7c-188">Vložení **SAML Entity ID** hodnotu, která jste zkopírovali z portálu Azure do **Entity Id** textové pole.</span><span class="sxs-lookup"><span data-stu-id="abf7c-188">Paste the **SAML Entity ID** value which you have copied from Azure Portal into the **Entity Id** textbox.</span></span>

    <span data-ttu-id="abf7c-189">g.</span><span class="sxs-lookup"><span data-stu-id="abf7c-189">g.</span></span> <span data-ttu-id="abf7c-190">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, která jste zkopírovali z portálu Azure do **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="abf7c-190">Paste the **SAML Single Sign-On Service URL** value which you have copied from Azure Portal into the **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="abf7c-191">h.</span><span class="sxs-lookup"><span data-stu-id="abf7c-191">h.</span></span> <span data-ttu-id="abf7c-192">Vložení **Sign-Out URL** hodnotu, která jste zkopírovali z portálu Azure do **adresa URL odhlašovací zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="abf7c-192">Paste the **Sign-Out URL** value which you have copied from Azure Portal into the **Identity Provider Logout URL** textbox.</span></span>
     
    <span data-ttu-id="abf7c-193">i.</span><span class="sxs-lookup"><span data-stu-id="abf7c-193">i.</span></span> <span data-ttu-id="abf7c-194">Jako **typ ID uživatele SAML**, vyberte **kontrolní výraz obsahuje uživatele "uživatelské jméno s sprinklr.com**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-194">As **SAML User ID Type**, select **Assertion contains User”s sprinklr.com username**.</span></span>

    <span data-ttu-id="abf7c-195">j.</span><span class="sxs-lookup"><span data-stu-id="abf7c-195">j.</span></span> <span data-ttu-id="abf7c-196">Jako **umístění ID uživatele SAML**, vyberte **ID uživatele je v elementu identifikátor název subjektu příkaz**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-196">As **SAML User ID Location**, select **User ID is in the Name Identifier element of the Subject statement**.</span></span>

    <span data-ttu-id="abf7c-197">kB.</span><span class="sxs-lookup"><span data-stu-id="abf7c-197">k.</span></span> <span data-ttu-id="abf7c-198">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-198">Click **Save**.</span></span>
       
    <span data-ttu-id="abf7c-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="abf7c-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span></span>

> [!TIP]
> <span data-ttu-id="abf7c-200">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="abf7c-200">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="abf7c-201">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="abf7c-201">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="abf7c-202">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="abf7c-202">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="abf7c-203">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="abf7c-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="abf7c-204">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="abf7c-204">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="abf7c-206">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="abf7c-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="abf7c-207">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="abf7c-207">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="abf7c-209">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-209">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="abf7c-211">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="abf7c-211">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="abf7c-213">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="abf7c-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="abf7c-215">a.</span><span class="sxs-lookup"><span data-stu-id="abf7c-215">a.</span></span> <span data-ttu-id="abf7c-216">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="abf7c-217">b.</span><span class="sxs-lookup"><span data-stu-id="abf7c-217">b.</span></span> <span data-ttu-id="abf7c-218">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="abf7c-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="abf7c-219">c.</span><span class="sxs-lookup"><span data-stu-id="abf7c-219">c.</span></span> <span data-ttu-id="abf7c-220">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="abf7c-221">d.</span><span class="sxs-lookup"><span data-stu-id="abf7c-221">d.</span></span> <span data-ttu-id="abf7c-222">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-222">Click **Create**.</span></span>
 
### <a name="creating-a-sprinklr-test-user"></a><span data-ttu-id="abf7c-223">Vytvoření zkušebního uživatele Sprinklr</span><span class="sxs-lookup"><span data-stu-id="abf7c-223">Creating a Sprinklr test user</span></span>

1. <span data-ttu-id="abf7c-224">Přihlaste se k serveru vaší společnosti Sprinklr jako správce.</span><span class="sxs-lookup"><span data-stu-id="abf7c-224">Log in to your Sprinklr company site as an administrator.</span></span>

2. <span data-ttu-id="abf7c-225">Přejděte na **správy \> nastavení**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-225">Go to **Administration \> Settings**.</span></span>
   
    <span data-ttu-id="abf7c-226">![Správa](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "správy")</span><span class="sxs-lookup"><span data-stu-id="abf7c-226">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

3. <span data-ttu-id="abf7c-227">Přejděte na **spravovat klienta \> uživatelé** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="abf7c-227">Go to **Manage Client \> Users** from the left pane.</span></span>
   
    <span data-ttu-id="abf7c-228">![Nastavení](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="abf7c-228">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "Settings")</span></span>

4. <span data-ttu-id="abf7c-229">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-229">Click **Add User**.</span></span>
   
    <span data-ttu-id="abf7c-230">![Nastavení](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="abf7c-230">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "Settings")</span></span>

5. <span data-ttu-id="abf7c-231">Na **upravit uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="abf7c-231">On the **Edit user** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="abf7c-232">![Úprava uživatele](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "úpravy uživatele")</span><span class="sxs-lookup"><span data-stu-id="abf7c-232">![Edit user](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Edit user")</span></span> 

    <span data-ttu-id="abf7c-233">a.</span><span class="sxs-lookup"><span data-stu-id="abf7c-233">a.</span></span> <span data-ttu-id="abf7c-234">V **e-mailu**, **křestní jméno** a **příjmení** textová pole, zadejte informace o účtu uživatele Azure AD, chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="abf7c-234">In the **Email**, **First Name** and **Last Name** textboxes, type the information of an Azure AD user account you want to provision.</span></span>

    <span data-ttu-id="abf7c-235">b.</span><span class="sxs-lookup"><span data-stu-id="abf7c-235">b.</span></span> <span data-ttu-id="abf7c-236">Vyberte **heslo zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-236">Select **Password Disabled**.</span></span>

    <span data-ttu-id="abf7c-237">c.</span><span class="sxs-lookup"><span data-stu-id="abf7c-237">c.</span></span> <span data-ttu-id="abf7c-238">Vyberte **jazyk**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-238">Select **Language**.</span></span>

    <span data-ttu-id="abf7c-239">d.</span><span class="sxs-lookup"><span data-stu-id="abf7c-239">d.</span></span> <span data-ttu-id="abf7c-240">Vyberte **typ uživatele**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-240">Select **User Type**.</span></span>

    <span data-ttu-id="abf7c-241">e.</span><span class="sxs-lookup"><span data-stu-id="abf7c-241">e.</span></span> <span data-ttu-id="abf7c-242">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-242">Click **Update**.</span></span>
   
     >[!IMPORTANT]
     ><span data-ttu-id="abf7c-243">**Heslo zakázáno** musí být vybrán tak, aby uživateli přihlásit se pomocí zprostředkovatele Identity.</span><span class="sxs-lookup"><span data-stu-id="abf7c-243">**Password Disabled** must be selected to enable a user to log in via an Identity provider.</span></span> 
     
6. <span data-ttu-id="abf7c-244">Přejděte na **Role**a poté proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="abf7c-244">Go to **Role**, and then perform the following steps:</span></span>
   
    <span data-ttu-id="abf7c-245">![Partner role](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner role")</span><span class="sxs-lookup"><span data-stu-id="abf7c-245">![Partner Roles](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner Roles")</span></span>

    <span data-ttu-id="abf7c-246">a.</span><span class="sxs-lookup"><span data-stu-id="abf7c-246">a.</span></span> <span data-ttu-id="abf7c-247">Z **globální** seznamu, vyberte **všechny\_oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-247">From the **Global** list, select **ALL\_Permissions**.</span></span>  

    <span data-ttu-id="abf7c-248">b.</span><span class="sxs-lookup"><span data-stu-id="abf7c-248">b.</span></span> <span data-ttu-id="abf7c-249">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-249">Click **Update**.</span></span>

>[!NOTE]
><span data-ttu-id="abf7c-250">Můžete použít všechny ostatní Sprinklr uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Sprinklr ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="abf7c-250">You can use any other Sprinklr user account creation tools or APIs provided by Sprinklr to provision Azure AD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="abf7c-251">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="abf7c-251">Assigning the Azure AD test user</span></span>

<span data-ttu-id="abf7c-252">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Sprinklr.</span><span class="sxs-lookup"><span data-stu-id="abf7c-252">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sprinklr.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="abf7c-254">**Pokud chcete přiřadit Britta Simon Sprinklr, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="abf7c-254">**To assign Britta Simon to Sprinklr, perform the following steps:**</span></span>

1. <span data-ttu-id="abf7c-255">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-255">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="abf7c-257">V seznamu aplikací vyberte **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-257">In the applications list, select **Sprinklr**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. <span data-ttu-id="abf7c-259">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="abf7c-259">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="abf7c-261">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="abf7c-261">Click **Add** button.</span></span> <span data-ttu-id="abf7c-262">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="abf7c-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="abf7c-264">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="abf7c-264">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="abf7c-265">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="abf7c-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="abf7c-266">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="abf7c-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="abf7c-267">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="abf7c-267">Testing single sign-on</span></span>

<span data-ttu-id="abf7c-268">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="abf7c-268">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="abf7c-269">Když kliknete na dlaždici Sprinklr na přístupovém panelu, měli byste obdržet automaticky přihlášení k aplikaci Sprinklr naleznete další informace o přístupový Panel [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="abf7c-269">When you click the Sprinklr tile in the Access Panel, you should get automatically signed-on to your Sprinklr application For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="abf7c-270">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="abf7c-270">Additional resources</span></span>

* [<span data-ttu-id="abf7c-271">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="abf7c-271">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="abf7c-272">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="abf7c-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

