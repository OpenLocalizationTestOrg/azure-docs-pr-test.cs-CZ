---
title: 'Kurz: Azure Active Directory integrace s TeamSeer | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TeamSeer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 2a5e8f6d1443681c43db95da5cef0b7f2ef92291
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a><span data-ttu-id="3e759-103">Kurz: Azure Active Directory integrace s TeamSeer</span><span class="sxs-lookup"><span data-stu-id="3e759-103">Tutorial: Azure Active Directory integration with TeamSeer</span></span>

<span data-ttu-id="3e759-104">V tomto kurzu zjistěte, jak integrovat TeamSeer s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3e759-104">In this tutorial, you learn how to integrate TeamSeer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3e759-105">Integrace TeamSeer s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3e759-105">Integrating TeamSeer with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3e759-106">Můžete řídit ve službě Azure AD, který má přístup k TeamSeer</span><span class="sxs-lookup"><span data-stu-id="3e759-106">You can control in Azure AD who has access to TeamSeer</span></span>
- <span data-ttu-id="3e759-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k TeamSeer (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e759-107">You can enable your users to automatically get signed-on to TeamSeer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3e759-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3e759-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3e759-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3e759-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e759-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3e759-110">Prerequisites</span></span>

<span data-ttu-id="3e759-111">Konfigurace integrace Azure AD s TeamSeer, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3e759-111">To configure Azure AD integration with TeamSeer, you need the following items:</span></span>

- <span data-ttu-id="3e759-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e759-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3e759-113">TeamSeer jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3e759-113">A TeamSeer single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3e759-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e759-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3e759-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3e759-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3e759-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3e759-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3e759-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3e759-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3e759-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3e759-118">Scenario description</span></span>
<span data-ttu-id="3e759-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e759-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3e759-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3e759-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3e759-121">Přidání TeamSeer z Galerie</span><span class="sxs-lookup"><span data-stu-id="3e759-121">Adding TeamSeer from the gallery</span></span>
2. <span data-ttu-id="3e759-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e759-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamseer-from-the-gallery"></a><span data-ttu-id="3e759-123">Přidání TeamSeer z Galerie</span><span class="sxs-lookup"><span data-stu-id="3e759-123">Adding TeamSeer from the gallery</span></span>
<span data-ttu-id="3e759-124">Chcete-li nakonfigurovat integraci TeamSeer v do Azure AD, přidejte TeamSeer z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3e759-124">To configure the integration of TeamSeer in to Azure AD, you need to add TeamSeer from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3e759-125">**Pokud chcete přidat TeamSeer z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e759-125">**To add TeamSeer from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3e759-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3e759-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3e759-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3e759-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3e759-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3e759-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3e759-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e759-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3e759-133">Do vyhledávacího pole zadejte **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="3e759-133">In the search box, type **TeamSeer**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. <span data-ttu-id="3e759-135">Na panelu výsledků vyberte **TeamSeer**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3e759-135">In the results panel, select **TeamSeer**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3e759-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e759-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3e759-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s TeamSeer podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3e759-138">In this section, you configure and test Azure AD single sign-on with TeamSeer based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3e759-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v TeamSeer je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3e759-139">For single sign-on to work, Azure AD needs to know what the counterpart user in TeamSeer is to a user in Azure AD.</span></span> <span data-ttu-id="3e759-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v TeamSeer musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3e759-140">In other words, a link relationship between an Azure AD user and the related user in TeamSeer needs to be established.</span></span>

<span data-ttu-id="3e759-141">V TeamSeer, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3e759-141">In TeamSeer, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3e759-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TeamSeer, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3e759-142">To configure and test Azure AD single sign-on with TeamSeer, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3e759-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3e759-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3e759-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3e759-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3e759-145">**[Vytvoření zkušebního uživatele TeamSeer](#creating-a-teamseer-test-user)**  – Pokud chcete mít protějšek Britta Simon v TeamSeer propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e759-145">**[Creating a TeamSeer test user](#creating-a-teamseer-test-user)** - to have a counterpart of Britta Simon in TeamSeer that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3e759-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3e759-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3e759-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3e759-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3e759-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e759-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3e759-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci TeamSeer.</span><span class="sxs-lookup"><span data-stu-id="3e759-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TeamSeer application.</span></span>

<span data-ttu-id="3e759-150">**Ke konfiguraci Azure AD jednotné přihlašování s TeamSeer, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e759-150">**To configure Azure AD single sign-on with TeamSeer, perform the following steps:**</span></span>

1. <span data-ttu-id="3e759-151">Na portálu Azure na **TeamSeer** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3e759-151">In the Azure portal, on the **TeamSeer** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3e759-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3e759-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. <span data-ttu-id="3e759-155">Na **TeamSeer domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e759-155">On the **TeamSeer Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     <span data-ttu-id="3e759-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.teamseer.com/<companyid>`</span><span class="sxs-lookup"><span data-stu-id="3e759-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.teamseer.com/<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3e759-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="3e759-158">The value is not real.</span></span> <span data-ttu-id="3e759-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3e759-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="3e759-160">Obraťte se na [tým podpory TeamSeer klienta](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3e759-160">Contact [TeamSeer Client support team](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) to get the value.</span></span> 
 
4. <span data-ttu-id="3e759-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="3e759-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. <span data-ttu-id="3e759-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3e759-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3e759-165">Na **TeamSeer konfigurace** klikněte na tlačítko **konfigurace TeamSeer** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3e759-165">On the **TeamSeer Configuration** section, click **Configure TeamSeer** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3e759-166">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3e759-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. <span data-ttu-id="3e759-168">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti TeamSeer jako správce.</span><span class="sxs-lookup"><span data-stu-id="3e759-168">In a different web browser window, log in to your TeamSeer company site as an administrator.</span></span>

8. <span data-ttu-id="3e759-169">Přejděte na **HR správce**.</span><span class="sxs-lookup"><span data-stu-id="3e759-169">Go to **HR Admin**.</span></span>
   
    <span data-ttu-id="3e759-170">![Správce HR](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR správce")</span><span class="sxs-lookup"><span data-stu-id="3e759-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR Admin")</span></span>

9. <span data-ttu-id="3e759-171">Klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="3e759-171">Click **Setup**.</span></span>
   
    <span data-ttu-id="3e759-172">![Instalační program](./media/active-directory-saas-teamseer-tutorial/ic789635.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="3e759-172">![Setup](./media/active-directory-saas-teamseer-tutorial/ic789635.png "Setup")</span></span>

10. <span data-ttu-id="3e759-173">Klikněte na tlačítko **nastavit podrobné informace o poskytovateli SAML**.</span><span class="sxs-lookup"><span data-stu-id="3e759-173">Click **Set up SAML provider details**.</span></span>
   
    <span data-ttu-id="3e759-174">![Nastavení SAML](./media/active-directory-saas-teamseer-tutorial/ic789636.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="3e759-174">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML Settings")</span></span>

11. <span data-ttu-id="3e759-175">V oddílu Podrobnosti zprostředkovatele SAML proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e759-175">In the SAML provider details section, perform the following steps:</span></span>
   
    <span data-ttu-id="3e759-176">![Nastavení SAML](./media/active-directory-saas-teamseer-tutorial/ic789637.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="3e759-176">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML Settings")</span></span>   

    <span data-ttu-id="3e759-177">a.</span><span class="sxs-lookup"><span data-stu-id="3e759-177">a.</span></span> <span data-ttu-id="3e759-178">Vložení **jeden přihlašování adresa URL služby** hodnotu v **URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3e759-178">Paste the **Single Sign-On Service URL** value in to the **URL** textbox.</span></span>
          
    <span data-ttu-id="3e759-179">b.</span><span class="sxs-lookup"><span data-stu-id="3e759-179">b.</span></span> <span data-ttu-id="3e759-180">V poznámkovém bloku otevřete kódovaného certifikátu vaší kódování base-64, obsah ho zkopírujte do schránky a vložte jej do **veřejný certifikát IdP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3e759-180">Open your base-64 encoded certificate in notepad, copy the content of it in to your clipboard, and then paste it to the **IdP Public Certificate** textbox.</span></span>

12. <span data-ttu-id="3e759-181">K dokončení konfigurace zprostředkovatele SAML, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e759-181">To complete the SAML provider configuration, perform the following steps:</span></span>
    
    <span data-ttu-id="3e759-182">![Nastavení SAML](./media/active-directory-saas-teamseer-tutorial/ic789638.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="3e759-182">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML Settings")</span></span> 

    <span data-ttu-id="3e759-183">a.</span><span class="sxs-lookup"><span data-stu-id="3e759-183">a.</span></span> <span data-ttu-id="3e759-184">V **testovací e-mailové adresy**, zadejte testovacího uživatele e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="3e759-184">In the **Test Email Addresses**, type the test user’s email address.</span></span> 
  
    <span data-ttu-id="3e759-185">b.</span><span class="sxs-lookup"><span data-stu-id="3e759-185">b.</span></span> <span data-ttu-id="3e759-186">V **vystavitele** textovému poli, zadejte adresu URL vystavitele poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="3e759-186">In the **Issuer** textbox, type the Issuer URL of the service provider.</span></span> 
  
    <span data-ttu-id="3e759-187">c.</span><span class="sxs-lookup"><span data-stu-id="3e759-187">c.</span></span> <span data-ttu-id="3e759-188">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3e759-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3e759-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3e759-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3e759-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3e759-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3e759-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3e759-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3e759-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e759-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="3e759-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3e759-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3e759-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e759-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3e759-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3e759-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3e759-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3e759-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3e759-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e759-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3e759-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e759-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3e759-204">a.</span><span class="sxs-lookup"><span data-stu-id="3e759-204">a.</span></span> <span data-ttu-id="3e759-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3e759-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3e759-206">b.</span><span class="sxs-lookup"><span data-stu-id="3e759-206">b.</span></span> <span data-ttu-id="3e759-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3e759-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3e759-208">c.</span><span class="sxs-lookup"><span data-stu-id="3e759-208">c.</span></span> <span data-ttu-id="3e759-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3e759-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3e759-210">d.</span><span class="sxs-lookup"><span data-stu-id="3e759-210">d.</span></span> <span data-ttu-id="3e759-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3e759-211">Click **Create**.</span></span>
 
### <a name="creating-a-teamseer-test-user"></a><span data-ttu-id="3e759-212">Vytvoření zkušebního uživatele TeamSeer</span><span class="sxs-lookup"><span data-stu-id="3e759-212">Creating a TeamSeer test user</span></span>

<span data-ttu-id="3e759-213">Pokud chcete povolit uživatelům Azure AD přihlášení k TeamSeer, se musí být zřízená v k ShiftPlanning.</span><span class="sxs-lookup"><span data-stu-id="3e759-213">To enable Azure AD users to log in to TeamSeer, they must be provisioned in to ShiftPlanning.</span></span> <span data-ttu-id="3e759-214">V případě TeamSeer zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="3e759-214">In the case of TeamSeer, provisioning is a manual task.</span></span>

<span data-ttu-id="3e759-215">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e759-215">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="3e759-216">Přihlaste se k vaší **TeamSeer** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="3e759-216">Log in to your **TeamSeer** company site as an administrator.</span></span>

2. <span data-ttu-id="3e759-217">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e759-217">Perform the following steps:</span></span>
   
    <span data-ttu-id="3e759-218">![Správce HR](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR správce")</span><span class="sxs-lookup"><span data-stu-id="3e759-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR Admin")</span></span>  
 
    <span data-ttu-id="3e759-219">a.</span><span class="sxs-lookup"><span data-stu-id="3e759-219">a.</span></span> <span data-ttu-id="3e759-220">Přejděte na **HR správce \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3e759-220">Go to **HR Admin \> Users**.</span></span>
  
    <span data-ttu-id="3e759-221">b.</span><span class="sxs-lookup"><span data-stu-id="3e759-221">b.</span></span> <span data-ttu-id="3e759-222">Klikněte na tlačítko **spustit Průvodce novým uživatelem**.</span><span class="sxs-lookup"><span data-stu-id="3e759-222">Click **Run the New User wizard**.</span></span>

3. <span data-ttu-id="3e759-223">V **podrobné informace o uživateli** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e759-223">In the **User Details** section, perform the following steps:</span></span>
   
    <span data-ttu-id="3e759-224">![Podrobné informace o uživateli](./media/active-directory-saas-teamseer-tutorial/ic789641.png "podrobné informace o uživateli")</span><span class="sxs-lookup"><span data-stu-id="3e759-224">![User Details](./media/active-directory-saas-teamseer-tutorial/ic789641.png "User Details")</span></span>

    <span data-ttu-id="3e759-225">a.</span><span class="sxs-lookup"><span data-stu-id="3e759-225">a.</span></span> <span data-ttu-id="3e759-226">Typ **křestní jméno**, **Přezdívka**, **uživatelské jméno (e-mailovou adresu)** platného účtu AAD chcete zřídit v k související textových polí.</span><span class="sxs-lookup"><span data-stu-id="3e759-226">Type the **First Name**, **Surname**, **User name (Email address)** of a valid AAD account you want to provision in to the related textboxes.</span></span>
  
    <span data-ttu-id="3e759-227">b.</span><span class="sxs-lookup"><span data-stu-id="3e759-227">b.</span></span> <span data-ttu-id="3e759-228">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e759-228">Click **Next**.</span></span>

4. <span data-ttu-id="3e759-229">Postupujte podle na obrazovce pokyny pro přidání nového uživatele a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="3e759-229">Follow the on-screen instructions for adding a new user, and click **Finish**.</span></span>

>[!NOTE]
><span data-ttu-id="3e759-230">Můžete použít všechny ostatní TeamSeer uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované TeamSeer ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3e759-230">You can use any other TeamSeer user account creation tools or APIs provided by TeamSeer to provision Azure AD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3e759-231">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e759-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3e759-232">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu TeamSeer.</span><span class="sxs-lookup"><span data-stu-id="3e759-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TeamSeer.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3e759-234">**Pokud chcete přiřadit Britta Simon TeamSeer, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e759-234">**To assign Britta Simon to TeamSeer, perform the following steps:**</span></span>

1. <span data-ttu-id="3e759-235">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3e759-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3e759-237">V seznamu aplikací vyberte **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="3e759-237">In the applications list, select **TeamSeer**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. <span data-ttu-id="3e759-239">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3e759-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3e759-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3e759-241">Click **Add** button.</span></span> <span data-ttu-id="3e759-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e759-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3e759-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3e759-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3e759-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e759-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3e759-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e759-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3e759-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e759-247">Testing single sign-on</span></span>

<span data-ttu-id="3e759-248">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="3e759-248">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="3e759-249">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3e759-249">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3e759-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3e759-250">Additional resources</span></span>

* [<span data-ttu-id="3e759-251">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e759-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3e759-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3e759-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

