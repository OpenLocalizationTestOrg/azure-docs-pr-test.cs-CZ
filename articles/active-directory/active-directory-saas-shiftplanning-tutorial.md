---
title: 'Kurz: Azure Active Directory integrace s lidskosti | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a lidskosti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 327cc1e3d0836e79376e0a3cd5a4422a967f5943
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a><span data-ttu-id="9c53c-103">Kurz: Azure Active Directory integrace s lidskosti</span><span class="sxs-lookup"><span data-stu-id="9c53c-103">Tutorial: Azure Active Directory integration with Humanity</span></span>

<span data-ttu-id="9c53c-104">V tomto kurzu zjistěte, jak integrovat lidskosti s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9c53c-104">In this tutorial, you learn how to integrate Humanity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9c53c-105">Integrace lidskosti s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9c53c-105">Integrating Humanity with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9c53c-106">Můžete řídit ve službě Azure AD, který má přístup k lidskosti</span><span class="sxs-lookup"><span data-stu-id="9c53c-106">You can control in Azure AD who has access to Humanity</span></span>
- <span data-ttu-id="9c53c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k lidskosti (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c53c-107">You can enable your users to automatically get signed-on to Humanity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9c53c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9c53c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9c53c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9c53c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c53c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9c53c-110">Prerequisites</span></span>

<span data-ttu-id="9c53c-111">Konfigurace integrace Azure AD s lidskosti, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9c53c-111">To configure Azure AD integration with Humanity, you need the following items:</span></span>

- <span data-ttu-id="9c53c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c53c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9c53c-113">Lidskosti jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9c53c-113">A Humanity single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9c53c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c53c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9c53c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9c53c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9c53c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9c53c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9c53c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9c53c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9c53c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9c53c-118">Scenario description</span></span>
<span data-ttu-id="9c53c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c53c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9c53c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9c53c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9c53c-121">Přidání lidskosti z Galerie</span><span class="sxs-lookup"><span data-stu-id="9c53c-121">Adding Humanity from the gallery</span></span>
2. <span data-ttu-id="9c53c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c53c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-humanity-from-the-gallery"></a><span data-ttu-id="9c53c-123">Přidání lidskosti z Galerie</span><span class="sxs-lookup"><span data-stu-id="9c53c-123">Adding Humanity from the gallery</span></span>
<span data-ttu-id="9c53c-124">Chcete-li nakonfigurovat integraci lidskosti do služby Azure AD, přidejte lidskosti z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9c53c-124">To configure the integration of Humanity into Azure AD, you need to add Humanity from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9c53c-125">**Pokud chcete přidat lidskosti z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c53c-125">**To add Humanity from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9c53c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9c53c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9c53c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9c53c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9c53c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c53c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9c53c-133">Do vyhledávacího pole zadejte **lidskosti**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-133">In the search box, type **Humanity**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_search.png)

5. <span data-ttu-id="9c53c-135">Na panelu výsledků vyberte **lidskosti**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c53c-135">In the results panel, select **Humanity**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9c53c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c53c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9c53c-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s lidskosti podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9c53c-138">In this section, you configure and test Azure AD single sign-on with Humanity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9c53c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v lidskosti je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c53c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Humanity is to a user in Azure AD.</span></span> <span data-ttu-id="9c53c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v lidskosti musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9c53c-140">In other words, a link relationship between an Azure AD user and the related user in Humanity needs to be established.</span></span>

<span data-ttu-id="9c53c-141">V lidskosti, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="9c53c-141">In Humanity, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9c53c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s lidskosti, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9c53c-142">To configure and test Azure AD single sign-on with Humanity, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9c53c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9c53c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9c53c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c53c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9c53c-145">**[Vytvoření zkušebního uživatele lidskosti](#creating-a-humanity-test-user)**  – Pokud chcete mít protějšek Britta Simon v lidskosti propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c53c-145">**[Creating a Humanity test user](#creating-a-humanity-test-user)** - to have a counterpart of Britta Simon in Humanity that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9c53c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9c53c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9c53c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9c53c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9c53c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c53c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9c53c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci lidskosti.</span><span class="sxs-lookup"><span data-stu-id="9c53c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Humanity application.</span></span>

<span data-ttu-id="9c53c-150">**Ke konfiguraci Azure AD jednotné přihlašování s lidskosti, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c53c-150">**To configure Azure AD single sign-on with Humanity, perform the following steps:**</span></span>

1. <span data-ttu-id="9c53c-151">Na portálu Azure na **lidskosti** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-151">In the Azure portal, on the **Humanity** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9c53c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9c53c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_samlbase.png)

3. <span data-ttu-id="9c53c-155">Na **lidskosti domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c53c-155">On the **Humanity Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_url.png)

    <span data-ttu-id="9c53c-157">a.</span><span class="sxs-lookup"><span data-stu-id="9c53c-157">a.</span></span> <span data-ttu-id="9c53c-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://company.humanity.com/includes/saml/`</span><span class="sxs-lookup"><span data-stu-id="9c53c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://company.humanity.com/includes/saml/`</span></span>

    <span data-ttu-id="9c53c-159">b.</span><span class="sxs-lookup"><span data-stu-id="9c53c-159">b.</span></span> <span data-ttu-id="9c53c-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://company.humanity.com/app/`</span><span class="sxs-lookup"><span data-stu-id="9c53c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://company.humanity.com/app/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9c53c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9c53c-161">These values are not real.</span></span> <span data-ttu-id="9c53c-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9c53c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9c53c-163">Obraťte se na [tým podpory lidskosti klienta](https://www.humanity.com/support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="9c53c-163">Contact [Humanity Client support team](https://www.humanity.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="9c53c-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="9c53c-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_certificate.png) 

5. <span data-ttu-id="9c53c-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c53c-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9c53c-168">Na **lidskosti konfigurace** klikněte na tlačítko **konfigurace lidskosti** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9c53c-168">On the **Humanity Configuration** section, click **Configure Humanity** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9c53c-169">Kopírování **SAML jeden přihlašování adresa URL služby a adresu URL Sign-Out** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9c53c-169">Copy the **SAML Single Sign-On Service URL, and Sign-Out URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_configure.png) 

7. <span data-ttu-id="9c53c-171">V okně prohlížeče jiný web, přihlaste se k vaší **lidskosti** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="9c53c-171">In a different web browser window, log in to your **Humanity** company site as an administrator.</span></span>

8. <span data-ttu-id="9c53c-172">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-172">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="9c53c-173">![Správce](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "správce")</span><span class="sxs-lookup"><span data-stu-id="9c53c-173">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

9. <span data-ttu-id="9c53c-174">V části **integrace**, klikněte na tlačítko **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-174">Under **Integration**, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="9c53c-175">![Jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9c53c-175">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "Single Sign-On")</span></span>

10. <span data-ttu-id="9c53c-176">V **jednotné přihlašování** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c53c-176">In the **Single Sign-On** section, perform the following steps:</span></span>
   
    <span data-ttu-id="9c53c-177">![Jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9c53c-177">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="9c53c-178">a.</span><span class="sxs-lookup"><span data-stu-id="9c53c-178">a.</span></span> <span data-ttu-id="9c53c-179">Vyberte **povoleno SAML**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-179">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="9c53c-180">b.</span><span class="sxs-lookup"><span data-stu-id="9c53c-180">b.</span></span> <span data-ttu-id="9c53c-181">Vyberte **povolit přihlášení heslo**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-181">Select **Allow Password Login**.</span></span>

    <span data-ttu-id="9c53c-182">c.</span><span class="sxs-lookup"><span data-stu-id="9c53c-182">c.</span></span> <span data-ttu-id="9c53c-183">Vložení **SAML jeden přihlašování adresa URL služby** hodnoty do **URL vystavitele SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9c53c-183">Paste the **SAML Single Sign-On Service URL** value into the **SAML Issuer URL** textbox.</span></span>

    <span data-ttu-id="9c53c-184">d.</span><span class="sxs-lookup"><span data-stu-id="9c53c-184">d.</span></span> <span data-ttu-id="9c53c-185">Vložení **Sign-Out URL** hodnotu do **vzdálené adresy URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9c53c-185">Paste the **Sign-Out URL** value into the **Remote Logout URL** textbox.</span></span>
   
    <span data-ttu-id="9c53c-186">e.</span><span class="sxs-lookup"><span data-stu-id="9c53c-186">e.</span></span> <span data-ttu-id="9c53c-187">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte jej do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9c53c-187">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>

11. <span data-ttu-id="9c53c-188">Klikněte na tlačítko **uložit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-188">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="9c53c-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9c53c-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9c53c-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9c53c-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9c53c-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9c53c-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9c53c-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c53c-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="9c53c-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c53c-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9c53c-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c53c-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9c53c-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9c53c-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9c53c-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9c53c-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c53c-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9c53c-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c53c-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9c53c-204">a.</span><span class="sxs-lookup"><span data-stu-id="9c53c-204">a.</span></span> <span data-ttu-id="9c53c-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9c53c-206">b.</span><span class="sxs-lookup"><span data-stu-id="9c53c-206">b.</span></span> <span data-ttu-id="9c53c-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9c53c-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9c53c-208">c.</span><span class="sxs-lookup"><span data-stu-id="9c53c-208">c.</span></span> <span data-ttu-id="9c53c-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9c53c-210">d.</span><span class="sxs-lookup"><span data-stu-id="9c53c-210">d.</span></span> <span data-ttu-id="9c53c-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-211">Click **Create**.</span></span>
 
### <a name="creating-a-humanity-test-user"></a><span data-ttu-id="9c53c-212">Vytvoření zkušebního uživatele lidskosti</span><span class="sxs-lookup"><span data-stu-id="9c53c-212">Creating a Humanity test user</span></span>

<span data-ttu-id="9c53c-213">Pokud chcete povolit uživatelům Azure AD přihlášení do lidskosti, musí být zřízená do lidskosti.</span><span class="sxs-lookup"><span data-stu-id="9c53c-213">In order to enable Azure AD users to log in to Humanity, they must be provisioned into Humanity.</span></span> <span data-ttu-id="9c53c-214">V případě lidskosti zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="9c53c-214">In the case of Humanity, provisioning is a manual task.</span></span>

<span data-ttu-id="9c53c-215">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c53c-215">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="9c53c-216">Přihlaste se k vaší **lidskosti** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="9c53c-216">Log in to your **Humanity** company site as an administrator.</span></span>

2. <span data-ttu-id="9c53c-217">Klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-217">Click **Admin**.</span></span>
   
    <span data-ttu-id="9c53c-218">![Správce](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "správce")</span><span class="sxs-lookup"><span data-stu-id="9c53c-218">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

3. <span data-ttu-id="9c53c-219">Klikněte na tlačítko **zaměstnanci**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-219">Click **Staff**.</span></span>
   
    <span data-ttu-id="9c53c-220">![Zaměstnanci](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "zaměstnanci")</span><span class="sxs-lookup"><span data-stu-id="9c53c-220">![Staff](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "Staff")</span></span>

4. <span data-ttu-id="9c53c-221">V části **akce**, klikněte na tlačítko **přidat zaměstnanci**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-221">Under **Actions**, click **Add Employees**.</span></span>
   
    <span data-ttu-id="9c53c-222">![Přidat zaměstnanci](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "přidat zaměstnanci")</span><span class="sxs-lookup"><span data-stu-id="9c53c-222">![Add Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "Add Employees")</span></span>

5. <span data-ttu-id="9c53c-223">V **přidat zaměstnanci** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c53c-223">In the **Add Employees** section, perform the following steps:</span></span>
   
    <span data-ttu-id="9c53c-224">![Uložit zaměstnanci](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "uložit zaměstnanci")</span><span class="sxs-lookup"><span data-stu-id="9c53c-224">![Save Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "Save Employees")</span></span>
   
    <span data-ttu-id="9c53c-225">a.</span><span class="sxs-lookup"><span data-stu-id="9c53c-225">a.</span></span> <span data-ttu-id="9c53c-226">Typ **křestní jméno**, **příjmení**, a **e-mailu** platného účtu AAD chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="9c53c-226">Type the **First Name**, **Last Name**, and **Email** of a valid AAD account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="9c53c-227">b.</span><span class="sxs-lookup"><span data-stu-id="9c53c-227">b.</span></span> <span data-ttu-id="9c53c-228">Klikněte na tlačítko **uložit zaměstnanci**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-228">Click **Save Employees**.</span></span>

>[!NOTE]
><span data-ttu-id="9c53c-229">Můžete použít všechny ostatní lidskosti uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované lidskosti zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="9c53c-229">You can use any other Humanity user account creation tools or APIs provided by Humanity to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9c53c-230">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c53c-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9c53c-231">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu lidskosti.</span><span class="sxs-lookup"><span data-stu-id="9c53c-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Humanity.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9c53c-233">**Pokud chcete přiřadit Britta Simon lidskosti, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c53c-233">**To assign Britta Simon to Humanity, perform the following steps:**</span></span>

1. <span data-ttu-id="9c53c-234">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9c53c-236">V seznamu aplikací vyberte **lidskosti**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-236">In the applications list, select **Humanity**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_app.png) 

3. <span data-ttu-id="9c53c-238">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9c53c-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9c53c-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c53c-240">Click **Add** button.</span></span> <span data-ttu-id="9c53c-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c53c-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9c53c-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9c53c-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9c53c-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c53c-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9c53c-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c53c-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9c53c-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c53c-246">Testing single sign-on</span></span>

<span data-ttu-id="9c53c-247">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9c53c-247">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9c53c-248">Když kliknete na dlaždici lidskosti na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci lidskosti.</span><span class="sxs-lookup"><span data-stu-id="9c53c-248">When you click the Humanity tile in the Access Panel, you should get automatically signed-on to your Humanity application.</span></span>
<span data-ttu-id="9c53c-249">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9c53c-249">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c53c-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9c53c-250">Additional resources</span></span>

* [<span data-ttu-id="9c53c-251">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c53c-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9c53c-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9c53c-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_203.png

