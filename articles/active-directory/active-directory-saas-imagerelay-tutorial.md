---
title: "Kurz: Azure Active Directory integrace s Image předávání | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a předávací bitové kopie."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 0bfbbaee7a74df6508584b7c8846fd07c2dc15c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a><span data-ttu-id="ebee3-103">Kurz: Azure Active Directory integrace s předávání bitové kopie</span><span class="sxs-lookup"><span data-stu-id="ebee3-103">Tutorial: Azure Active Directory integration with Image Relay</span></span>

<span data-ttu-id="ebee3-104">V tomto kurzu zjistěte, jak integrovat předávání bitové kopie s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ebee3-104">In this tutorial, you learn how to integrate Image Relay with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ebee3-105">Integrace předávání bitové kopie s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ebee3-105">Integrating Image Relay with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ebee3-106">Můžete řídit ve službě Azure AD, který má přístup k předávání bitové kopie</span><span class="sxs-lookup"><span data-stu-id="ebee3-106">You can control in Azure AD who has access to Image Relay</span></span>
- <span data-ttu-id="ebee3-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k předávání bitové kopie (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ebee3-107">You can enable your users to automatically get signed-on to Image Relay (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ebee3-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ebee3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ebee3-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ebee3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ebee3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ebee3-110">Prerequisites</span></span>

<span data-ttu-id="ebee3-111">Konfigurace integrace Azure AD s předávání bitové kopie, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ebee3-111">To configure Azure AD integration with Image Relay, you need the following items:</span></span>

- <span data-ttu-id="ebee3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ebee3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ebee3-113">Předávání Image jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ebee3-113">An Image Relay single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ebee3-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ebee3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ebee3-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ebee3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ebee3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ebee3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ebee3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ebee3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ebee3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ebee3-118">Scenario description</span></span>
<span data-ttu-id="ebee3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ebee3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ebee3-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ebee3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ebee3-121">Přidání bitové kopie předávání z Galerie</span><span class="sxs-lookup"><span data-stu-id="ebee3-121">Adding Image Relay from the gallery</span></span>
2. <span data-ttu-id="ebee3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ebee3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-image-relay-from-the-gallery"></a><span data-ttu-id="ebee3-123">Přidání bitové kopie předávání z Galerie</span><span class="sxs-lookup"><span data-stu-id="ebee3-123">Adding Image Relay from the gallery</span></span>
<span data-ttu-id="ebee3-124">Při konfiguraci integrace předávání bitové kopie do služby Azure AD potřebujete přidat bitovou kopii předávání z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ebee3-124">To configure the integration of Image Relay into Azure AD, you need to add Image Relay from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ebee3-125">**Chcete-li přidat bitovou kopii předávání z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ebee3-125">**To add Image Relay from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ebee3-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ebee3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ebee3-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ebee3-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ebee3-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ebee3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ebee3-133">Do vyhledávacího pole zadejte **Image předávání**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-133">In the search box, type **Image Relay**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_search.png)

5. <span data-ttu-id="ebee3-135">Na panelu výsledků vyberte **Image předávání**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ebee3-135">In the results panel, select **Image Relay**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ebee3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ebee3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ebee3-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s předávání bitové kopie založené na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ebee3-138">In this section, you configure and test Azure AD single sign-on with Image Relay based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ebee3-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšku Relay bitové kopie je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ebee3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Image Relay is to a user in Azure AD.</span></span> <span data-ttu-id="ebee3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské Image Relay musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ebee3-140">In other words, a link relationship between an Azure AD user and the related user in Image Relay needs to be established.</span></span>

<span data-ttu-id="ebee3-141">Obrázek Relay přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ebee3-141">In Image Relay, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ebee3-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s předávání bitové kopie, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ebee3-142">To configure and test Azure AD single sign-on with Image Relay, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ebee3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ebee3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ebee3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ebee3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ebee3-145">**[Vytváření testovacího uživatele předávání Image](#creating-an-image-relay-test-user)**  – Pokud chcete mít protějšek Britta Simon Relay bitové kopie, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ebee3-145">**[Creating an Image Relay test user](#creating-an-image-relay-test-user)** - to have a counterpart of Britta Simon in Image Relay that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ebee3-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ebee3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ebee3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ebee3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ebee3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ebee3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ebee3-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci předávání bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ebee3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Image Relay application.</span></span>

<span data-ttu-id="ebee3-150">**Ke konfiguraci Azure AD jednotné přihlašování s předávání bitové kopie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ebee3-150">**To configure Azure AD single sign-on with Image Relay, perform the following steps:**</span></span>

1. <span data-ttu-id="ebee3-151">Na portálu Azure na **Image předávání** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-151">In the Azure portal, on the **Image Relay** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ebee3-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ebee3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

3. <span data-ttu-id="ebee3-155">Na **Image předávání domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ebee3-155">On the **Image Relay Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_url.png)

    <span data-ttu-id="ebee3-157">a.</span><span class="sxs-lookup"><span data-stu-id="ebee3-157">a.</span></span> <span data-ttu-id="ebee3-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.imagerelay.com/`</span><span class="sxs-lookup"><span data-stu-id="ebee3-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.imagerelay.com/`</span></span>

    <span data-ttu-id="ebee3-159">b.</span><span class="sxs-lookup"><span data-stu-id="ebee3-159">b.</span></span> <span data-ttu-id="ebee3-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.imagerelay.com/sso/metadata`</span><span class="sxs-lookup"><span data-stu-id="ebee3-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.imagerelay.com/sso/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ebee3-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ebee3-161">These values are not real.</span></span> <span data-ttu-id="ebee3-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ebee3-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ebee3-163">Obraťte se na [tým podpory Image předávání klienta](http://support.imagerelay.com/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="ebee3-163">Contact [Image Relay Client support team](http://support.imagerelay.com/) to get these values.</span></span> 
 


4. <span data-ttu-id="ebee3-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="ebee3-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

5. <span data-ttu-id="ebee3-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ebee3-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ebee3-168">Na **konfigurace přenosového Image** klikněte na tlačítko **konfigurace přenosového bitové kopie** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ebee3-168">On the **Image Relay Configuration** section, click **Configure Image Relay** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ebee3-169">Kopírování **Sign-Out adresa URL služby a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ebee3-169">Copy the **Sign-Out Service URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_configure.png) 

7. <span data-ttu-id="ebee3-171">V jiném okně prohlížeče Přihlaste se k serveru vaší společnosti předávání bitové kopie jako správce.</span><span class="sxs-lookup"><span data-stu-id="ebee3-171">In another browser window, sign in to your Image Relay company site as an administrator.</span></span>

8. <span data-ttu-id="ebee3-172">Na panelu nástrojů v horní části klikněte **uživatelé a oprávnění** zatížení.</span><span class="sxs-lookup"><span data-stu-id="ebee3-172">In the toolbar on the top, click the **Users & Permissions** workload.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

9. <span data-ttu-id="ebee3-174">Klikněte na tlačítko **vytvořit nová položka oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-174">Click **Create New Permission**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

10. <span data-ttu-id="ebee3-176">V **jeden znak v nastavení** úlohy, vyberte **tato skupina může pouze přihlášení přes jednotné přihlašování** zaškrtněte políčko a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-176">In the **Single Sign On Settings** workload, select the **This Group can only sign-in via Single Sign On** check box, and then click **Save**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

11. <span data-ttu-id="ebee3-178">Přejděte na **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-178">Go to **Account Settings**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

12. <span data-ttu-id="ebee3-180">Přejděte na **jeden znak v nastavení** zatížení.</span><span class="sxs-lookup"><span data-stu-id="ebee3-180">Go to the **Single Sign On Settings** workload.</span></span>
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

13. <span data-ttu-id="ebee3-182">Na **SAML nastavení** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ebee3-182">On the **SAML Settings** dialog, perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    <span data-ttu-id="ebee3-184">a.</span><span class="sxs-lookup"><span data-stu-id="ebee3-184">a.</span></span> <span data-ttu-id="ebee3-185">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ebee3-185">In **Login URL** textbox, paste the value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="ebee3-186">b.</span><span class="sxs-lookup"><span data-stu-id="ebee3-186">b.</span></span> <span data-ttu-id="ebee3-187">V **adresy URL odhlašovací** textovému poli, vložte hodnotu **jednu adresu URL služby Sign-Out** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ebee3-187">In **Logout URL**  textbox, paste the value of **Single Sign-Out Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="ebee3-188">c.</span><span class="sxs-lookup"><span data-stu-id="ebee3-188">c.</span></span> <span data-ttu-id="ebee3-189">Jako **formát Id názvu**, vyberte **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-189">As **Name Id Format**, select **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="ebee3-190">d.</span><span class="sxs-lookup"><span data-stu-id="ebee3-190">d.</span></span> <span data-ttu-id="ebee3-191">Jako **vazby možnosti pro žádosti od poskytovatele služeb (Image relé)**, vyberte **POST vazby**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-191">As **Binding Options for Requests from the Service Provider (Image Relay)**, select **POST Binding**.</span></span>

    <span data-ttu-id="ebee3-192">e.</span><span class="sxs-lookup"><span data-stu-id="ebee3-192">e.</span></span> <span data-ttu-id="ebee3-193">V části **x.509 Certificate**, klikněte na tlačítko **aktualizace certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-193">Under **x.509 Certificate**, click **Update Certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    <span data-ttu-id="ebee3-195">f.</span><span class="sxs-lookup"><span data-stu-id="ebee3-195">f.</span></span> <span data-ttu-id="ebee3-196">V poznámkovém bloku otevřete stažený certifikát, kopírovat obsah a pak ji vložit do textového pole certifikátu x.509.</span><span class="sxs-lookup"><span data-stu-id="ebee3-196">Open the downloaded certificate in notepad, copy the content, and then paste it into the x.509 Certificate textbox.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    <span data-ttu-id="ebee3-198">g.</span><span class="sxs-lookup"><span data-stu-id="ebee3-198">g.</span></span> <span data-ttu-id="ebee3-199">V **zřizování uživatelů JIT** vyberte **povolit zřizování uživatelů JIT**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-199">In **Just-In-Time User Provisioning** section, select the **Enable Just-In-Time User Provisioning**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    <span data-ttu-id="ebee3-201">h.</span><span class="sxs-lookup"><span data-stu-id="ebee3-201">h.</span></span> <span data-ttu-id="ebee3-202">Vyberte skupinu oprávnění (například **jednotného přihlašování k základní**) povolený se přihlásit pouze pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ebee3-202">Select the permission group (for example, **SSO Basic**) which is allowed to sign in only through single sign-on.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    <span data-ttu-id="ebee3-204">i.</span><span class="sxs-lookup"><span data-stu-id="ebee3-204">i.</span></span> <span data-ttu-id="ebee3-205">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-205">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="ebee3-206">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ebee3-206">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ebee3-207">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ebee3-207">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ebee3-208">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ebee3-208">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ebee3-209">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ebee3-209">Creating an Azure AD test user</span></span>
<span data-ttu-id="ebee3-210">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ebee3-210">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ebee3-212">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ebee3-212">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ebee3-213">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ebee3-213">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ebee3-215">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-215">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ebee3-217">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ebee3-217">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ebee3-219">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ebee3-219">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ebee3-221">a.</span><span class="sxs-lookup"><span data-stu-id="ebee3-221">a.</span></span> <span data-ttu-id="ebee3-222">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-222">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ebee3-223">b.</span><span class="sxs-lookup"><span data-stu-id="ebee3-223">b.</span></span> <span data-ttu-id="ebee3-224">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ebee3-224">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ebee3-225">c.</span><span class="sxs-lookup"><span data-stu-id="ebee3-225">c.</span></span> <span data-ttu-id="ebee3-226">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-226">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ebee3-227">d.</span><span class="sxs-lookup"><span data-stu-id="ebee3-227">d.</span></span> <span data-ttu-id="ebee3-228">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-228">Click **Create**.</span></span>
 
### <a name="creating-an-image-relay-test-user"></a><span data-ttu-id="ebee3-229">Vytváření testovacího uživatele předávání bitové kopie</span><span class="sxs-lookup"><span data-stu-id="ebee3-229">Creating an Image Relay test user</span></span>

<span data-ttu-id="ebee3-230">Cílem této části je vytvoření uživatele volat Britta Simon Relay bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ebee3-230">The objective of this section is to create a user called Britta Simon in Image Relay.</span></span>

<span data-ttu-id="ebee3-231">**Pokud chcete vytvořit uživateli volat Britta Simon Relay bitové kopie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ebee3-231">**To create a user called Britta Simon in Image Relay, perform the following steps:**</span></span>

1. <span data-ttu-id="ebee3-232">Přihlášení k serveru vaší společnosti předávání bitové kopie jako správce.</span><span class="sxs-lookup"><span data-stu-id="ebee3-232">Sign-on to your Image Relay company site as an administrator.</span></span>

2. <span data-ttu-id="ebee3-233">Přejděte na **uživatelé a oprávnění** a vyberte **vytvořit jednotné přihlašování uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-233">Go to **Users & Permissions**     and select **Create SSO User**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. <span data-ttu-id="ebee3-235">Zadejte **e-mailu**, **křestní jméno**, **příjmení**, a **společnosti** chcete zřídit a vyberte skupinu oprávnění (pro uživatele Příklad: základní jednotné přihlašování) kterého je skupina, která může přihlásit pouze pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ebee3-235">Enter the **Email**, **First Name**, **Last Name**, and **Company** of the user you want to provision and select the permission group (for example, SSO Basic) which is the group that can sign in only through single sign-on.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

4. <span data-ttu-id="ebee3-237">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-237">Click **Create**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ebee3-238">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ebee3-238">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ebee3-239">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k předávání bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ebee3-239">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Image Relay.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ebee3-241">**Přiřadit Britta Simon Relay bitové kopie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ebee3-241">**To assign Britta Simon to Image Relay, perform the following steps:**</span></span>

1. <span data-ttu-id="ebee3-242">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ebee3-244">V seznamu aplikací vyberte **Image předávání**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-244">In the applications list, select **Image Relay**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_app.png) 

3. <span data-ttu-id="ebee3-246">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ebee3-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ebee3-248">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ebee3-248">Click **Add** button.</span></span> <span data-ttu-id="ebee3-249">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ebee3-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ebee3-251">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ebee3-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ebee3-252">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ebee3-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ebee3-253">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ebee3-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ebee3-254">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ebee3-254">Testing single sign-on</span></span>

<span data-ttu-id="ebee3-255">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ebee3-255">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>    

<span data-ttu-id="ebee3-256">Když kliknete na dlaždici předávání bitové kopie na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci předávání bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ebee3-256">When you click the Image Relay tile in the Access Panel, you should get automatically signed-on to your Image Relay application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="ebee3-257">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ebee3-257">Additional resources</span></span>

* [<span data-ttu-id="ebee3-258">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ebee3-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ebee3-259">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ebee3-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png

