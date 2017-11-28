---
title: 'Kurz: Azure Active Directory integrace s Gigya | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Gigya."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: b65a33989a045a3e0b57fda522a9bc3b0770c7f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a><span data-ttu-id="c68e7-103">Kurz: Azure Active Directory integrace s Gigya</span><span class="sxs-lookup"><span data-stu-id="c68e7-103">Tutorial: Azure Active Directory integration with Gigya</span></span>

<span data-ttu-id="c68e7-104">V tomto kurzu zjistěte, jak integrovat Gigya s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c68e7-104">In this tutorial, you learn how to integrate Gigya with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c68e7-105">Integrace Gigya s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c68e7-105">Integrating Gigya with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c68e7-106">Můžete řídit ve službě Azure AD, který má přístup k Gigya</span><span class="sxs-lookup"><span data-stu-id="c68e7-106">You can control in Azure AD who has access to Gigya</span></span>
- <span data-ttu-id="c68e7-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Gigya (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c68e7-107">You can enable your users to automatically get signed-on to Gigya (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c68e7-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c68e7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c68e7-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c68e7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c68e7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c68e7-110">Prerequisites</span></span>

<span data-ttu-id="c68e7-111">Konfigurace integrace Azure AD s Gigya, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c68e7-111">To configure Azure AD integration with Gigya, you need the following items:</span></span>

- <span data-ttu-id="c68e7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c68e7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c68e7-113">Gigya jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c68e7-113">A Gigya single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c68e7-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c68e7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c68e7-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c68e7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c68e7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c68e7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c68e7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c68e7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c68e7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c68e7-118">Scenario description</span></span>
<span data-ttu-id="c68e7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c68e7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c68e7-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c68e7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c68e7-121">Přidání Gigya z Galerie</span><span class="sxs-lookup"><span data-stu-id="c68e7-121">Adding Gigya from the gallery</span></span>
2. <span data-ttu-id="c68e7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c68e7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gigya-from-the-gallery"></a><span data-ttu-id="c68e7-123">Přidání Gigya z Galerie</span><span class="sxs-lookup"><span data-stu-id="c68e7-123">Adding Gigya from the gallery</span></span>
<span data-ttu-id="c68e7-124">Při konfiguraci integrace Gigya do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Gigya z galerie.</span><span class="sxs-lookup"><span data-stu-id="c68e7-124">To configure the integration of Gigya into Azure AD, you need to add Gigya from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c68e7-125">**Pokud chcete přidat Gigya z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c68e7-125">**To add Gigya from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c68e7-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c68e7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c68e7-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c68e7-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c68e7-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c68e7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c68e7-133">Do vyhledávacího pole zadejte **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-133">In the search box, type **Gigya**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_search.png)

5. <span data-ttu-id="c68e7-135">Na panelu výsledků vyberte **Gigya**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c68e7-135">In the results panel, select **Gigya**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c68e7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c68e7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c68e7-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Gigya podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c68e7-138">In this section, you configure and test Azure AD single sign-on with Gigya based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c68e7-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Gigya je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c68e7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Gigya is to a user in Azure AD.</span></span> <span data-ttu-id="c68e7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Gigya musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c68e7-140">In other words, a link relationship between an Azure AD user and the related user in Gigya needs to be established.</span></span>

<span data-ttu-id="c68e7-141">V Gigya, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="c68e7-141">In Gigya, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c68e7-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Gigya, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c68e7-142">To configure and test Azure AD single sign-on with Gigya, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c68e7-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c68e7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c68e7-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c68e7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c68e7-145">**[Vytvoření zkušebního uživatele Gigya](#creating-a-gigya-test-user)**  – Pokud chcete mít protějšek Britta Simon v Gigya propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c68e7-145">**[Creating a Gigya test user](#creating-a-gigya-test-user)** - to have a counterpart of Britta Simon in Gigya that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c68e7-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c68e7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c68e7-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c68e7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c68e7-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c68e7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c68e7-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Gigya.</span><span class="sxs-lookup"><span data-stu-id="c68e7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Gigya application.</span></span>

<span data-ttu-id="c68e7-150">**Ke konfiguraci Azure AD jednotné přihlašování s Gigya, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c68e7-150">**To configure Azure AD single sign-on with Gigya, perform the following steps:**</span></span>

1. <span data-ttu-id="c68e7-151">Na portálu Azure na **Gigya** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-151">In the Azure portal, on the **Gigya** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c68e7-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c68e7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_samlbase.png)

3. <span data-ttu-id="c68e7-155">Na **Gigya domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c68e7-155">On the **Gigya Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_url.png)

    <span data-ttu-id="c68e7-157">a.</span><span class="sxs-lookup"><span data-stu-id="c68e7-157">a.</span></span> <span data-ttu-id="c68e7-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://<companyname>.gigya.com`</span><span class="sxs-lookup"><span data-stu-id="c68e7-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<companyname>.gigya.com`</span></span>

    <span data-ttu-id="c68e7-159">b.</span><span class="sxs-lookup"><span data-stu-id="c68e7-159">b.</span></span> <span data-ttu-id="c68e7-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://fidm.gigya.com/saml/v2.0/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="c68e7-160">In the **Identifier** textbox, type a URL using the following pattern: `https://fidm.gigya.com/saml/v2.0/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c68e7-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c68e7-161">These values are not real.</span></span> <span data-ttu-id="c68e7-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c68e7-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c68e7-163">Obraťte se na [tým podpory Gigya klienta](https://www.gigya.com/support-policy/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="c68e7-163">Contact [Gigya Client support team](https://www.gigya.com/support-policy/) to get these values.</span></span> 
 
4. <span data-ttu-id="c68e7-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="c68e7-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_certificate.png) 

5. <span data-ttu-id="c68e7-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c68e7-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c68e7-168">Na **Gigya konfigurace** klikněte na tlačítko **konfigurace Gigya** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c68e7-168">On the **Gigya Configuration** section, click **Configure Gigya** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c68e7-169">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c68e7-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_configure.png) 

7. <span data-ttu-id="c68e7-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Gigya.</span><span class="sxs-lookup"><span data-stu-id="c68e7-171">In a different web browser window, log into your Gigya company site as an administrator.</span></span>

8. <span data-ttu-id="c68e7-172">Přejděte na **nastavení \> SAML přihlášení**a klikněte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c68e7-172">Go to **Settings \> SAML Login**, and then click the **Add** button.</span></span>
   
    <span data-ttu-id="c68e7-173">![Přihlašování SAML](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML přihlášení")</span><span class="sxs-lookup"><span data-stu-id="c68e7-173">![SAML Login](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML Login")</span></span>

9. <span data-ttu-id="c68e7-174">V **SAML přihlášení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c68e7-174">In the **SAML Login** section, perform the following steps:</span></span>
   
    <span data-ttu-id="c68e7-175">![Konfigurace SAML](./media/active-directory-saas-gigya-tutorial/ic789533.png "konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="c68e7-175">![SAML Configuration](./media/active-directory-saas-gigya-tutorial/ic789533.png "SAML Configuration")</span></span>
   
    <span data-ttu-id="c68e7-176">a.</span><span class="sxs-lookup"><span data-stu-id="c68e7-176">a.</span></span> <span data-ttu-id="c68e7-177">V **název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c68e7-177">In the **Name** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="c68e7-178">b.</span><span class="sxs-lookup"><span data-stu-id="c68e7-178">b.</span></span> <span data-ttu-id="c68e7-179">V **vystavitele** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c68e7-179">In **Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure Portal.</span></span> 
   
    <span data-ttu-id="c68e7-180">c.</span><span class="sxs-lookup"><span data-stu-id="c68e7-180">c.</span></span> <span data-ttu-id="c68e7-181">V **jeden přihlašování adresa URL služby** textovému poli, vložte hodnotu **jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c68e7-181">In **Single Sign-On Service URL** textbox, paste the value of **Single Sign-On Service URL** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="c68e7-182">d.</span><span class="sxs-lookup"><span data-stu-id="c68e7-182">d.</span></span> <span data-ttu-id="c68e7-183">V **formát ID názvu** textovému poli, vložte hodnotu **formát názvu identifikátor** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c68e7-183">In **Name ID Format** textbox, paste the value of **Name Identifier Format** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="c68e7-184">e.</span><span class="sxs-lookup"><span data-stu-id="c68e7-184">e.</span></span> <span data-ttu-id="c68e7-185">Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku stáhli z portálu Azure, zkopírujte obsah ho do schránky a vložte jej do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c68e7-185">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="c68e7-186">f.</span><span class="sxs-lookup"><span data-stu-id="c68e7-186">f.</span></span> <span data-ttu-id="c68e7-187">Klikněte na tlačítko **uložit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-187">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="c68e7-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c68e7-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c68e7-189">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c68e7-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c68e7-190">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c68e7-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c68e7-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c68e7-191">Creating an Azure AD test user</span></span>

<span data-ttu-id="c68e7-192">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c68e7-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c68e7-194">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c68e7-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c68e7-195">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c68e7-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c68e7-197">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c68e7-199">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c68e7-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c68e7-201">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c68e7-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c68e7-203">a.</span><span class="sxs-lookup"><span data-stu-id="c68e7-203">a.</span></span> <span data-ttu-id="c68e7-204">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c68e7-205">b.</span><span class="sxs-lookup"><span data-stu-id="c68e7-205">b.</span></span> <span data-ttu-id="c68e7-206">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c68e7-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c68e7-207">c.</span><span class="sxs-lookup"><span data-stu-id="c68e7-207">c.</span></span> <span data-ttu-id="c68e7-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c68e7-209">d.</span><span class="sxs-lookup"><span data-stu-id="c68e7-209">d.</span></span> <span data-ttu-id="c68e7-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-210">Click **Create**.</span></span>
 
### <a name="creating-a-gigya-test-user"></a><span data-ttu-id="c68e7-211">Vytvoření zkušebního uživatele Gigya</span><span class="sxs-lookup"><span data-stu-id="c68e7-211">Creating a Gigya test user</span></span>

<span data-ttu-id="c68e7-212">Pokud chcete povolit uživatelům Azure AD přihlášení do Gigya, musí být zřízená do Gigya.</span><span class="sxs-lookup"><span data-stu-id="c68e7-212">In order to enable Azure AD users to log into Gigya, they must be provisioned into Gigya.</span></span>  
<span data-ttu-id="c68e7-213">V případě Gigya zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="c68e7-213">In the case of Gigya, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="c68e7-214">Ke zřízení uživatelských účtů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c68e7-214">To provision a user accounts, perform the following steps:</span></span>

1. <span data-ttu-id="c68e7-215">Přihlaste se k vaší **Gigya** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="c68e7-215">Log in to your **Gigya** company site as an administrator.</span></span>

2. <span data-ttu-id="c68e7-216">Přejděte na **správce \> spravovat uživatele**a potom klikněte na **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-216">Go to **Admin \> Manage Users**, and then click **Invite Users**.</span></span>
   
    <span data-ttu-id="c68e7-217">![Správa uživatelů](./media/active-directory-saas-gigya-tutorial/ic789535.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="c68e7-217">![Manage Users](./media/active-directory-saas-gigya-tutorial/ic789535.png "Manage Users")</span></span>

3. <span data-ttu-id="c68e7-218">V dialogovém okně pozvat uživatele proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c68e7-218">On the Invite Users dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="c68e7-219">![Uživatele pozvat](./media/active-directory-saas-gigya-tutorial/ic789536.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="c68e7-219">![Invite Users](./media/active-directory-saas-gigya-tutorial/ic789536.png "Invite Users")</span></span>
   
    <span data-ttu-id="c68e7-220">a.</span><span class="sxs-lookup"><span data-stu-id="c68e7-220">a.</span></span> <span data-ttu-id="c68e7-221">V **e-mailu** textovému poli, zadejte e-mailový alias chcete zřídit platný účet služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c68e7-221">In the **Email** textbox, type the email alias of a valid Azure Active Directory account you want to provision.</span></span>
    
    <span data-ttu-id="c68e7-222">b.</span><span class="sxs-lookup"><span data-stu-id="c68e7-222">b.</span></span> <span data-ttu-id="c68e7-223">Klikněte na tlačítko **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-223">Click **Invite User**.</span></span>
      
    > [!NOTE]
    > <span data-ttu-id="c68e7-224">Držitel účtu Azure Active Directory obdrží e-mail, který obsahuje odkaz pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="c68e7-224">The Azure Active Directory account holder will receive an email that includes a link to confirm the account before it becomes active.</span></span>
    > 
    

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c68e7-225">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c68e7-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c68e7-226">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Gigya.</span><span class="sxs-lookup"><span data-stu-id="c68e7-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Gigya.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c68e7-228">**Pokud chcete přiřadit Britta Simon Gigya, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c68e7-228">**To assign Britta Simon to Gigya, perform the following steps:**</span></span>

1. <span data-ttu-id="c68e7-229">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c68e7-231">V seznamu aplikací vyberte **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-231">In the applications list, select **Gigya**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_app.png) 

3. <span data-ttu-id="c68e7-233">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c68e7-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c68e7-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c68e7-235">Click **Add** button.</span></span> <span data-ttu-id="c68e7-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c68e7-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c68e7-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c68e7-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c68e7-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c68e7-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c68e7-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c68e7-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c68e7-241">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c68e7-241">Testing single sign-on</span></span>

<span data-ttu-id="c68e7-242">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="c68e7-242">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="c68e7-243">Když kliknete na dlaždici Gigya na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Gigya.</span><span class="sxs-lookup"><span data-stu-id="c68e7-243">When you click the Gigya tile in the Access Panel, you should get automatically signed-on to your Gigya application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c68e7-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c68e7-244">Additional resources</span></span>

* [<span data-ttu-id="c68e7-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c68e7-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c68e7-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c68e7-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_203.png

