---
title: "Kurz: Azure Active Directory integrace s konkrétně | Microsoft Docs"
description: "Naučte se konfigurovat jednotné přihlašování mezi Azure Active Directory a konkrétně."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 1d7e8fbcfc757853ab909bbb05522f3dc387715d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a><span data-ttu-id="8715e-103">Kurz: Azure Active Directory integrace s konkrétně</span><span class="sxs-lookup"><span data-stu-id="8715e-103">Tutorial: Azure Active Directory integration with Namely</span></span>

<span data-ttu-id="8715e-104">V tomto kurzu zjistěte, jak integrovat konkrétně s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8715e-104">In this tutorial, you learn how to integrate Namely with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8715e-105">Konkrétně integraci s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8715e-105">Integrating Namely with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8715e-106">Můžete řídit ve službě Azure AD, kdo má přístup k konkrétně</span><span class="sxs-lookup"><span data-stu-id="8715e-106">You can control in Azure AD who has access to Namely</span></span>
- <span data-ttu-id="8715e-107">Můžete povolit uživatelům získat automaticky přihlášení k konkrétně (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8715e-107">You can enable your users to automatically get signed-on to Namely (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8715e-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8715e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8715e-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8715e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8715e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8715e-110">Prerequisites</span></span>

<span data-ttu-id="8715e-111">Ke konfiguraci Azure AD integrace s konkrétně, budete potřebovat následující položky:</span><span class="sxs-lookup"><span data-stu-id="8715e-111">To configure Azure AD integration with Namely, you need the following items:</span></span>

- <span data-ttu-id="8715e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8715e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8715e-113">Konkrétně jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8715e-113">A Namely single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8715e-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8715e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8715e-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8715e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8715e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8715e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8715e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8715e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8715e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8715e-118">Scenario description</span></span>
<span data-ttu-id="8715e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8715e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8715e-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8715e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8715e-121">Přidání konkrétně z Galerie</span><span class="sxs-lookup"><span data-stu-id="8715e-121">Adding Namely from the gallery</span></span>
2. <span data-ttu-id="8715e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8715e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-namely-from-the-gallery"></a><span data-ttu-id="8715e-123">Přidání konkrétně z Galerie</span><span class="sxs-lookup"><span data-stu-id="8715e-123">Adding Namely from the gallery</span></span>
<span data-ttu-id="8715e-124">Chcete-li nakonfigurovat integraci konkrétně do služby Azure AD, přidejte konkrétně z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8715e-124">To configure the integration of Namely into Azure AD, you need to add Namely from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8715e-125">**Pokud chcete přidat a to z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8715e-125">**To add Namely from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8715e-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8715e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8715e-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8715e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8715e-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8715e-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8715e-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8715e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8715e-133">Do vyhledávacího pole zadejte **konkrétně**.</span><span class="sxs-lookup"><span data-stu-id="8715e-133">In the search box, type **Namely**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_search.png)

5. <span data-ttu-id="8715e-135">Na panelu výsledků vyberte **konkrétně**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8715e-135">In the results panel, select **Namely**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8715e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8715e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8715e-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s, a to podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="8715e-138">In this section, you configure and test Azure AD single sign-on with Namely based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8715e-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v konkrétně je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8715e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Namely is to a user in Azure AD.</span></span> <span data-ttu-id="8715e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v konkrétně je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="8715e-140">In other words, a link relationship between an Azure AD user and the related user in Namely needs to be established.</span></span>

<span data-ttu-id="8715e-141">V konkrétně, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="8715e-141">In Namely, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8715e-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s konkrétně, je potřeba provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="8715e-142">To configure and test Azure AD single sign-on with Namely, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8715e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="8715e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8715e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8715e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8715e-145">**[Vytváření konkrétně testovací uživatel](#creating-a-namely-test-user)**  – Pokud chcete mít protějšek Britta Simon v konkrétně připojený k Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="8715e-145">**[Creating a Namely test user](#creating-a-namely-test-user)** - to have a counterpart of Britta Simon in Namely that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8715e-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8715e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8715e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8715e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8715e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8715e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8715e-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v konkrétně aplikace.</span><span class="sxs-lookup"><span data-stu-id="8715e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Namely application.</span></span>

<span data-ttu-id="8715e-150">**Ke konfiguraci Azure AD jednotné přihlašování s konkrétně, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8715e-150">**To configure Azure AD single sign-on with Namely, perform the following steps:**</span></span>

1. <span data-ttu-id="8715e-151">Na portálu Azure na **konkrétně** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8715e-151">In the Azure portal, on the **Namely** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8715e-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8715e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_samlbase.png)

3. <span data-ttu-id="8715e-155">Na **konkrétně domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8715e-155">On the **Namely Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_url.png)

    <span data-ttu-id="8715e-157">a.</span><span class="sxs-lookup"><span data-stu-id="8715e-157">a.</span></span> <span data-ttu-id="8715e-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.namely.com`</span><span class="sxs-lookup"><span data-stu-id="8715e-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.namely.com`</span></span>

    <span data-ttu-id="8715e-159">b.</span><span class="sxs-lookup"><span data-stu-id="8715e-159">b.</span></span> <span data-ttu-id="8715e-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.namely.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="8715e-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.namely.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8715e-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="8715e-161">These values are not real.</span></span> <span data-ttu-id="8715e-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8715e-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8715e-163">Obraťte se na [tým podpory konkrétně klienta](https://www.namely.com/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="8715e-163">Contact [Namely Client support team](https://www.namely.com/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="8715e-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="8715e-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_certificate.png) 

5. <span data-ttu-id="8715e-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8715e-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8715e-168">Na **konkrétně konfigurace** klikněte na tlačítko **konfigurace konkrétně** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="8715e-168">On the **Namely Configuration** section, click **Configure Namely** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8715e-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="8715e-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_configure.png) 

7. <span data-ttu-id="8715e-171">V jiném okně prohlížeče, přihlaste se k vaší konkrétně společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="8715e-171">In another browser window, sign on to your Namely company site as an administrator.</span></span>

8. <span data-ttu-id="8715e-172">Na panelu nástrojů v horní části klikněte na tlačítko **společnosti**.</span><span class="sxs-lookup"><span data-stu-id="8715e-172">In the toolbar on the top, click **Company**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

9. <span data-ttu-id="8715e-174">Klikněte na kartu **Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="8715e-174">Click the **Settings** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 

10. <span data-ttu-id="8715e-176">Klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="8715e-176">Click **SAML**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 

11. <span data-ttu-id="8715e-178">Na **SAML nastavení** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8715e-178">On the **SAML Settings** page, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png)
 
    <span data-ttu-id="8715e-180">a.</span><span class="sxs-lookup"><span data-stu-id="8715e-180">a.</span></span> <span data-ttu-id="8715e-181">Klikněte na tlačítko **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="8715e-181">Click **Enable SAML**.</span></span> 

    <span data-ttu-id="8715e-182">b.</span><span class="sxs-lookup"><span data-stu-id="8715e-182">b.</span></span> <span data-ttu-id="8715e-183">V **adresa url jednotné přihlašování zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8715e-183">In the **Identity provider SSO url** textbox,  paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="8715e-184">c.</span><span class="sxs-lookup"><span data-stu-id="8715e-184">c.</span></span> <span data-ttu-id="8715e-185">V poznámkovém bloku otevřete stažený certifikát, kopírovat obsah a vložte ji do **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8715e-185">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **Identity provider certificate** textbox.</span></span>
     
    <span data-ttu-id="8715e-186">d.</span><span class="sxs-lookup"><span data-stu-id="8715e-186">d.</span></span> <span data-ttu-id="8715e-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8715e-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="8715e-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="8715e-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8715e-189">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="8715e-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8715e-190">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8715e-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8715e-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8715e-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="8715e-192">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8715e-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8715e-194">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8715e-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8715e-195">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8715e-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8715e-197">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8715e-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8715e-199">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8715e-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8715e-201">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8715e-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8715e-203">a.</span><span class="sxs-lookup"><span data-stu-id="8715e-203">a.</span></span> <span data-ttu-id="8715e-204">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8715e-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8715e-205">b.</span><span class="sxs-lookup"><span data-stu-id="8715e-205">b.</span></span> <span data-ttu-id="8715e-206">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8715e-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8715e-207">c.</span><span class="sxs-lookup"><span data-stu-id="8715e-207">c.</span></span> <span data-ttu-id="8715e-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8715e-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8715e-209">d.</span><span class="sxs-lookup"><span data-stu-id="8715e-209">d.</span></span> <span data-ttu-id="8715e-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8715e-210">Click **Create**.</span></span>
 
### <a name="creating-a-namely-test-user"></a><span data-ttu-id="8715e-211">Vytváření konkrétně testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="8715e-211">Creating a Namely test user</span></span>

<span data-ttu-id="8715e-212">Cílem této části je vytvoření uživatele volal Britta Simon konkrétně v.</span><span class="sxs-lookup"><span data-stu-id="8715e-212">The objective of this section is to create a user called Britta Simon in Namely.</span></span>

<span data-ttu-id="8715e-213">**Vytvoření uživatele volal Britta Simon v konkrétně, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8715e-213">**To create a user called Britta Simon in Namely, perform the following steps:**</span></span>

1. <span data-ttu-id="8715e-214">Přihlášení k vaší konkrétně společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="8715e-214">Sign-on to your Namely company site as an administrator.</span></span>

2. <span data-ttu-id="8715e-215">Na panelu nástrojů v horní části klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="8715e-215">In the toolbar on the top, click **People**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

3. <span data-ttu-id="8715e-217">Klikněte **Directory** kartě.</span><span class="sxs-lookup"><span data-stu-id="8715e-217">Click the **Directory** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

4. <span data-ttu-id="8715e-219">Klikněte na tlačítko **přidat nové osobě**.</span><span class="sxs-lookup"><span data-stu-id="8715e-219">Click **Add New Person**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_12.png)

5. <span data-ttu-id="8715e-221">Na **přidat nové osobě** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8715e-221">On the **Add New Person** dialog, perform the following steps:</span></span>

    <span data-ttu-id="8715e-222">a.</span><span class="sxs-lookup"><span data-stu-id="8715e-222">a.</span></span> <span data-ttu-id="8715e-223">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="8715e-223">In the **First name** textbox, type **Britta**.</span></span>

    <span data-ttu-id="8715e-224">b.</span><span class="sxs-lookup"><span data-stu-id="8715e-224">b.</span></span> <span data-ttu-id="8715e-225">V **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="8715e-225">In the **Last name** textbox, type **Simon**.</span></span>

    <span data-ttu-id="8715e-226">c.</span><span class="sxs-lookup"><span data-stu-id="8715e-226">c.</span></span> <span data-ttu-id="8715e-227">V **e-mailu** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8715e-227">In the **Email** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8715e-228">d.</span><span class="sxs-lookup"><span data-stu-id="8715e-228">d.</span></span> <span data-ttu-id="8715e-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8715e-229">Click **Save**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8715e-230">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8715e-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8715e-231">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k konkrétně.</span><span class="sxs-lookup"><span data-stu-id="8715e-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Namely.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8715e-233">**Přiřazení Britta Simon na konkrétně, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8715e-233">**To assign Britta Simon to Namely, perform the following steps:**</span></span>

1. <span data-ttu-id="8715e-234">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8715e-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8715e-236">V seznamu aplikací vyberte **konkrétně**.</span><span class="sxs-lookup"><span data-stu-id="8715e-236">In the applications list, select **Namely**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_app.png) 

3. <span data-ttu-id="8715e-238">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8715e-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8715e-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8715e-240">Click **Add** button.</span></span> <span data-ttu-id="8715e-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8715e-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8715e-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8715e-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8715e-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8715e-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8715e-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8715e-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8715e-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8715e-246">Testing single sign-on</span></span>

<span data-ttu-id="8715e-247">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="8715e-247">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="8715e-248">Když kliknete dlaždici konkrétně na přístupovém panelu, jste měli získat automaticky přihlášení k konkrétně aplikace</span><span class="sxs-lookup"><span data-stu-id="8715e-248">When you click the Namely tile in the Access Panel, you should get automatically signed-on to your Namely application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8715e-249">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8715e-249">Additional resources</span></span>

* [<span data-ttu-id="8715e-250">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8715e-250">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8715e-251">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8715e-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-namely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png

