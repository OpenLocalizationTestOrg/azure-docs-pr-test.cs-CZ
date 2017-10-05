---
title: 'Kurz: Azure Active Directory integrace s Velpic SAML | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Velpic SAML."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 5325f3cca00167e6b7b687509ce43435447ad2f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a><span data-ttu-id="44838-103">Kurz: Azure Active Directory integrace s Velpic SAML</span><span class="sxs-lookup"><span data-stu-id="44838-103">Tutorial: Azure Active Directory integration with Velpic SAML</span></span>

<span data-ttu-id="44838-104">V tomto kurzu zjistěte, jak integrovat Velpic SAML s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="44838-104">In this tutorial, you learn how to integrate Velpic SAML with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="44838-105">Integrace Velpic SAML s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="44838-105">Integrating Velpic SAML with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="44838-106">Můžete řídit ve službě Azure AD, který má přístup k Velpic SAML</span><span class="sxs-lookup"><span data-stu-id="44838-106">You can control in Azure AD who has access to Velpic SAML</span></span>
- <span data-ttu-id="44838-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Velpic SAML (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="44838-107">You can enable your users to automatically get signed-on to Velpic SAML (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="44838-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="44838-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="44838-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="44838-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44838-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="44838-110">Prerequisites</span></span>

<span data-ttu-id="44838-111">Konfigurace integrace Azure AD s Velpic SAML, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="44838-111">To configure Azure AD integration with Velpic SAML, you need the following items:</span></span>

- <span data-ttu-id="44838-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="44838-112">An Azure AD subscription</span></span>
- <span data-ttu-id="44838-113">Velpic SAML jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="44838-113">A Velpic SAML single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="44838-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="44838-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="44838-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="44838-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="44838-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="44838-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="44838-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="44838-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="44838-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="44838-118">Scenario description</span></span>
<span data-ttu-id="44838-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="44838-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="44838-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="44838-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="44838-121">Přidání Velpic SAML z Galerie</span><span class="sxs-lookup"><span data-stu-id="44838-121">Adding Velpic SAML from the gallery</span></span>
2. <span data-ttu-id="44838-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="44838-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-velpic-saml-from-the-gallery"></a><span data-ttu-id="44838-123">Přidání Velpic SAML z Galerie</span><span class="sxs-lookup"><span data-stu-id="44838-123">Adding Velpic SAML from the gallery</span></span>
<span data-ttu-id="44838-124">Při konfiguraci integrace Velpic SAML do služby Azure AD potřebujete přidat Velpic SAML z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="44838-124">To configure the integration of Velpic SAML into Azure AD, you need to add Velpic SAML from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="44838-125">**Pokud chcete přidat Velpic SAML z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="44838-125">**To add Velpic SAML from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="44838-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="44838-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="44838-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="44838-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="44838-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="44838-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="44838-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44838-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="44838-133">Do vyhledávacího pole zadejte **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="44838-133">In the search box, type **Velpic SAML**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. <span data-ttu-id="44838-135">Na panelu výsledků vyberte **Velpic SAML**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="44838-135">In the results panel, select **Velpic SAML**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="44838-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="44838-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="44838-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Velpic SAML podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="44838-138">In this section, you configure and test Azure AD single sign-on with Velpic SAML based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="44838-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Velpic SAML je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="44838-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Velpic SAML is to a user in Azure AD.</span></span> <span data-ttu-id="44838-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Velpic SAML musí navázat.</span><span class="sxs-lookup"><span data-stu-id="44838-140">In other words, a link relationship between an Azure AD user and the related user in Velpic SAML needs to be established.</span></span>

<span data-ttu-id="44838-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="44838-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Velpic SAML.</span></span>

<span data-ttu-id="44838-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Velpic SAML, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="44838-142">To configure and test Azure AD single sign-on with Velpic SAML, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="44838-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="44838-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="44838-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44838-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="44838-145">**[Vytvoření zkušebního uživatele Velpic SAML](#creating-a-velpic-saml-test-user)**  – Pokud chcete mít protějšek Britta Simon v Velpic SAML, propojené služby Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="44838-145">**[Creating a Velpic SAML test user](#creating-a-velpic-saml-test-user)** - to have a counterpart of Britta Simon in Velpic SAML that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="44838-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="44838-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="44838-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="44838-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="44838-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="44838-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="44838-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="44838-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Velpic SAML application.</span></span>

<span data-ttu-id="44838-150">**Ke konfiguraci Azure AD jednotné přihlašování s Velpic SAML, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="44838-150">**To configure Azure AD single sign-on with Velpic SAML, perform the following steps:**</span></span>

1. <span data-ttu-id="44838-151">Na portálu Azure Management portal na **Velpic SAML** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="44838-151">In the Azure Management portal, on the **Velpic SAML** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="44838-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="44838-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. <span data-ttu-id="44838-155">Zadejte podrobnosti v **domény Velpic SAML a adres URL** části -</span><span class="sxs-lookup"><span data-stu-id="44838-155">Enter the details in the **Velpic SAML Domain and URLs** section-</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    <span data-ttu-id="44838-157">a.</span><span class="sxs-lookup"><span data-stu-id="44838-157">a.</span></span> <span data-ttu-id="44838-158">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu jako:`https://<sub-domain>.velpicsaml.net`</span><span class="sxs-lookup"><span data-stu-id="44838-158">In the **Sign-on URL** textbox, type the value as: `https://<sub-domain>.velpicsaml.net`</span></span>

    <span data-ttu-id="44838-159">b.</span><span class="sxs-lookup"><span data-stu-id="44838-159">b.</span></span> <span data-ttu-id="44838-160">V **identifikátor** textovému poli, Vložit **'jednotné přihlašování na adresu URL,** hodnota`https://auth.velpic.com/saml/v2/<entity-id>/login`</span><span class="sxs-lookup"><span data-stu-id="44838-160">In the **Identifier** textbox, paste the **‘Single sign on URL’** value `https://auth.velpic.com/saml/v2/<entity-id>/login`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="44838-161">Upozorňujeme, že adresa URL přihlašování budou poskytovat týmem Velpic SAML a hodnotu identifikátoru bude k dispozici, když konfigurujete jednotného přihlašování k modulu plug-in na straně Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="44838-161">Please note that the Sign on URL will be provided by the Velpic SAML team and Identifier value will be available when you configure the SSO Plugin on Velpic SAML side.</span></span> <span data-ttu-id="44838-162">Budete muset zkopírujte tuto hodnotu ze stránky aplikace Velpic SAML a vložte ho sem.</span><span class="sxs-lookup"><span data-stu-id="44838-162">You need to copy that value from Velpic SAML application  page and paste it here.</span></span>

4. <span data-ttu-id="44838-163">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="44838-163">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. <span data-ttu-id="44838-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="44838-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="44838-167">V části Konfigurace SAML Velpic klikněte na tlačítko Konfigurovat Velpic SAML otevřete konfigurovat přihlašování okno.</span><span class="sxs-lookup"><span data-stu-id="44838-167">On the Velpic SAML Configuration section, click Configure Velpic SAML to open Configure sign-on window.</span></span> <span data-ttu-id="44838-168">Zkopírujte SAML Entity ID z části Stručná referenční příručka.</span><span class="sxs-lookup"><span data-stu-id="44838-168">Copy the SAML Entity ID from the Quick Reference section.</span></span>

7. <span data-ttu-id="44838-169">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="44838-169">In a different web browser window, log into your Velpic SAML company site as an administrator.</span></span>

8. <span data-ttu-id="44838-170">Klikněte na **spravovat** kartě a přejděte na **integrace** část, kde je potřeba klikněte na **modulů plug-in** tlačítko pro vytvoření nového modulu plug-in pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="44838-170">Click on **Manage** tab and go to **Integration** section where you need to click on **Plugins** button to create new plugin for Sign-In.</span></span>

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. <span data-ttu-id="44838-172">Klikněte na **'Přidání modulu plug-in'** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="44838-172">Click on the **‘Add plugin’** button.</span></span>
    
    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. <span data-ttu-id="44838-174">Klikněte na **SAML** dlaždici na stránce Přidat modul plug-in.</span><span class="sxs-lookup"><span data-stu-id="44838-174">Click on the **SAML** tile in the Add Plugin page.</span></span>
    
    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. <span data-ttu-id="44838-176">Zadejte název nové zásuvný modul SAML a klikněte na **"Přidat"** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="44838-176">Enter the name of the new SAML plugin and click the **‘Add’** button.</span></span>

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. <span data-ttu-id="44838-178">Zadejte podrobnosti následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="44838-178">Enter the details as follows:</span></span>

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    <span data-ttu-id="44838-180">a.</span><span class="sxs-lookup"><span data-stu-id="44838-180">a.</span></span> <span data-ttu-id="44838-181">V **název** textovému poli, zadejte název zásuvný modul SAML.</span><span class="sxs-lookup"><span data-stu-id="44838-181">In the **Name** textbox, type the name of SAML plugin.</span></span>

    <span data-ttu-id="44838-182">b.</span><span class="sxs-lookup"><span data-stu-id="44838-182">b.</span></span> <span data-ttu-id="44838-183">V **URL vystavitele** textovému poli, Vložit **SAML Entity ID** jste zkopírovali z **konfigurovat přihlášení** okno portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44838-183">In the **Issuer URL** textbox, paste the **SAML Entity ID** you copied from the **Configure sign-on** window of the Azure portal.</span></span>

    <span data-ttu-id="44838-184">c.</span><span class="sxs-lookup"><span data-stu-id="44838-184">c.</span></span> <span data-ttu-id="44838-185">V **zprostředkovatele metadat konfigurace** nahrát soubor XML s metadaty soubor, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44838-185">In the **Provider Metadata Config** upload the Metadata XML file which you downloaded from Azure portal.</span></span>

    <span data-ttu-id="44838-186">d.</span><span class="sxs-lookup"><span data-stu-id="44838-186">d.</span></span> <span data-ttu-id="44838-187">Můžete také povolit SAML jenom při zřizování čas povolením **, automaticky vytvoří nové uživatele'** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="44838-187">You can also choose to enable SAML just in time provisioning by enabling the **‘Auto create new users’** checkbox.</span></span> <span data-ttu-id="44838-188">Pokud uživatel neexistuje v Velpic a tento příznak není povolen, přihlašovací údaje z Azure se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="44838-188">If a user doesn’t exist in Velpic and this flag is not enabled, the login from Azure will fail.</span></span> <span data-ttu-id="44838-189">Pokud je povoleno příznak uživatele se automaticky zřídí do Velpic při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="44838-189">If the flag is enabled the user will automatically be provisioned into Velpic at the time of login.</span></span> 

    <span data-ttu-id="44838-190">e.</span><span class="sxs-lookup"><span data-stu-id="44838-190">e.</span></span> <span data-ttu-id="44838-191">Kopírování **jednotné přihlašování na adrese URL** z textu pole a vložte ji na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44838-191">Copy the **Single sign on URL** from the text box and paste it in the Azure portal.</span></span>
    
    <span data-ttu-id="44838-192">f.</span><span class="sxs-lookup"><span data-stu-id="44838-192">f.</span></span> <span data-ttu-id="44838-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="44838-193">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="44838-194">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="44838-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="44838-195">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44838-195">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="44838-197">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="44838-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="44838-198">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="44838-198">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="44838-200">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="44838-200">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="44838-202">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44838-202">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="44838-204">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="44838-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="44838-206">a.</span><span class="sxs-lookup"><span data-stu-id="44838-206">a.</span></span> <span data-ttu-id="44838-207">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="44838-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="44838-208">b.</span><span class="sxs-lookup"><span data-stu-id="44838-208">b.</span></span> <span data-ttu-id="44838-209">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="44838-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="44838-210">c.</span><span class="sxs-lookup"><span data-stu-id="44838-210">c.</span></span> <span data-ttu-id="44838-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="44838-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="44838-212">d.</span><span class="sxs-lookup"><span data-stu-id="44838-212">d.</span></span> <span data-ttu-id="44838-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="44838-213">Click **Create**.</span></span>
 
### <a name="creating-a-velpic-saml-test-user"></a><span data-ttu-id="44838-214">Vytvoření zkušebního uživatele Velpic SAML</span><span class="sxs-lookup"><span data-stu-id="44838-214">Creating a Velpic SAML test user</span></span>

<span data-ttu-id="44838-215">Tento krok se obvykle nevyžaduje jako aplikace podporuje jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="44838-215">This step is usually not required as the application supports just in time user provisioning.</span></span> <span data-ttu-id="44838-216">Pokud uživatel automatické zřizování není povolena můžete vytvoření ruční uživatele provádí, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="44838-216">If the automatic user provisioning is not enabled then manual user creation can be done as described below.</span></span>

<span data-ttu-id="44838-217">Přihlaste se k serveru vaší společnosti Velpic SAML jako správce a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="44838-217">Log into your Velpic SAML company site as an administrator and perform following steps:</span></span>
    
1. <span data-ttu-id="44838-218">Klikněte na kartu Správa a přejděte k části uživatelů a potom klikněte na tlačítko Nový přidat uživatele.</span><span class="sxs-lookup"><span data-stu-id="44838-218">Click on Manage tab and go to Users section, then click on New button to add users.</span></span>

    ![Přidat uživatele](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. <span data-ttu-id="44838-220">Na **"Vytvořit nový uživatel"** dialogové okno proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="44838-220">On the **“Create New User”** dialog page, perform the following steps.</span></span>

    ![Uživatel](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    <span data-ttu-id="44838-222">a.</span><span class="sxs-lookup"><span data-stu-id="44838-222">a.</span></span> <span data-ttu-id="44838-223">V **křestní jméno** textovému poli, název typu první Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44838-223">In the **First Name** textbox, type the first name of Britta Simon.</span></span>

    <span data-ttu-id="44838-224">b.</span><span class="sxs-lookup"><span data-stu-id="44838-224">b.</span></span> <span data-ttu-id="44838-225">V **příjmení** textovému poli, zadejte příjmení Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44838-225">In the **Last Name** textbox, type the last name of Britta Simon.</span></span>

    <span data-ttu-id="44838-226">c.</span><span class="sxs-lookup"><span data-stu-id="44838-226">c.</span></span> <span data-ttu-id="44838-227">V **uživatelské jméno** textovému poli, zadejte uživatelské jméno Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44838-227">In the **User Name** textbox, type the user name of Britta Simon.</span></span>

    <span data-ttu-id="44838-228">d.</span><span class="sxs-lookup"><span data-stu-id="44838-228">d.</span></span> <span data-ttu-id="44838-229">V **e-mailu** textovému poli, zadejte e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44838-229">In the **Email** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="44838-230">e.</span><span class="sxs-lookup"><span data-stu-id="44838-230">e.</span></span> <span data-ttu-id="44838-231">Zbývající informace je volitelné, můžete vyplnit, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="44838-231">Rest of the information is optional, you can fill it if needed.</span></span>
    
    <span data-ttu-id="44838-232">f.</span><span class="sxs-lookup"><span data-stu-id="44838-232">f.</span></span> <span data-ttu-id="44838-233">Klikněte na **ULOŽIT**.</span><span class="sxs-lookup"><span data-stu-id="44838-233">Click **SAVE**.</span></span>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="44838-234">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="44838-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="44838-235">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="44838-235">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Velpic SAML.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="44838-237">**Pokud chcete přiřadit Britta Simon Velpic SAML, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="44838-237">**To assign Britta Simon to Velpic SAML, perform the following steps:**</span></span>

1. <span data-ttu-id="44838-238">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="44838-238">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="44838-240">V seznamu aplikací vyberte **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="44838-240">In the applications list, select **Velpic SAML**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. <span data-ttu-id="44838-242">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="44838-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="44838-244">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="44838-244">Click **Add** button.</span></span> <span data-ttu-id="44838-245">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44838-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="44838-247">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="44838-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="44838-248">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44838-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="44838-249">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44838-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="44838-250">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="44838-250">Testing single sign-on</span></span>

<span data-ttu-id="44838-251">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="44838-251">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

1. <span data-ttu-id="44838-252">Když kliknete na dlaždici Velpic SAML na přístupovém panelu, měli byste obdržet přihlašovací stránku aplikace Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="44838-252">When you click the Velpic SAML tile in the Access Panel, you should get login page of Velpic SAML application.</span></span> <span data-ttu-id="44838-253">Měli byste vidět **"přihlásit se přes Azure AD,** tlačítko na přihlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="44838-253">You should see the **‘Log In With Azure AD’** button on the sign in page.</span></span>

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. <span data-ttu-id="44838-255">Klikněte na **"přihlásit se přes Azure AD,** tlačítko pro přihlášení do Velpic pomocí účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="44838-255">Click on the **‘Log In With Azure AD’** button to log in to Velpic using your Azure AD account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="44838-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="44838-256">Additional resources</span></span>

* [<span data-ttu-id="44838-257">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="44838-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="44838-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="44838-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png

