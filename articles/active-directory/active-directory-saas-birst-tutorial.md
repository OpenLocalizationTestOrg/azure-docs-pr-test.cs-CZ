---
title: "Kurz: Azure Active Directory integrace s Birst agilní obchodní analýza | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Birst agilní obchodní analýzy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 677183b1-5348-4302-88cc-5c8ab63a3c6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 779f9e0a57ffb2274ea22a90ed9759734ab6916d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-birst-agile-business-analytics"></a><span data-ttu-id="93cbd-103">Kurz: Azure Active Directory integrace s Birst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="93cbd-103">Tutorial: Azure Active Directory integration with Birst Agile Business Analytics</span></span>

<span data-ttu-id="93cbd-104">V tomto kurzu zjistěte, jak integrovat Birst agilní obchodní analýza s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="93cbd-104">In this tutorial, you learn how to integrate Birst Agile Business Analytics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="93cbd-105">Obchodní analýza agilní Birst integrace s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="93cbd-105">Integrating Birst Agile Business Analytics with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="93cbd-106">Můžete řídit ve službě Azure AD, který má přístup k Birst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="93cbd-106">You can control in Azure AD who has access to Birst Agile Business Analytics</span></span>
- <span data-ttu-id="93cbd-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Birst agilní obchodní analýza (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="93cbd-107">You can enable your users to automatically get signed-on to Birst Agile Business Analytics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="93cbd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="93cbd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="93cbd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="93cbd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93cbd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="93cbd-110">Prerequisites</span></span>

<span data-ttu-id="93cbd-111">Konfigurace integrace Azure AD s Birst agilní obchodní analýza, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="93cbd-111">To configure Azure AD integration with Birst Agile Business Analytics, you need the following items:</span></span>

- <span data-ttu-id="93cbd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="93cbd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="93cbd-113">Birst agilní obchodní analýza jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="93cbd-113">A Birst Agile Business Analytics single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="93cbd-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="93cbd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="93cbd-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="93cbd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="93cbd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="93cbd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="93cbd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="93cbd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="93cbd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="93cbd-118">Scenario description</span></span>
<span data-ttu-id="93cbd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="93cbd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="93cbd-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="93cbd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="93cbd-121">Přidání z Galerie Birst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="93cbd-121">Adding Birst Agile Business Analytics from the gallery</span></span>
2. <span data-ttu-id="93cbd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="93cbd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-birst-agile-business-analytics-from-the-gallery"></a><span data-ttu-id="93cbd-123">Přidání z Galerie Birst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="93cbd-123">Adding Birst Agile Business Analytics from the gallery</span></span>
<span data-ttu-id="93cbd-124">Při konfiguraci integrace Birst agilní obchodních analýz do služby Azure AD, potřebujete přidat Birst agilní obchodní analýza z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="93cbd-124">To configure the integration of Birst Agile Business Analytics into Azure AD, you need to add Birst Agile Business Analytics from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="93cbd-125">**Pokud chcete přidat Birst agilní obchodní analýza z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="93cbd-125">**To add Birst Agile Business Analytics from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="93cbd-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="93cbd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="93cbd-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="93cbd-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="93cbd-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="93cbd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="93cbd-133">Do vyhledávacího pole zadejte **Birst Agile obchodní analýza**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-133">In the search box, type **Birst Agile Business Analytics**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/tutorial_birst_search.png)

5. <span data-ttu-id="93cbd-135">Na panelu výsledků vyberte **Birst Agile obchodní analýza**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93cbd-135">In the results panel, select **Birst Agile Business Analytics**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/tutorial_birst_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="93cbd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="93cbd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="93cbd-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Birst agilní obchodní analýza podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="93cbd-138">In this section, you configure and test Azure AD single sign-on with Birst Agile Business Analytics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="93cbd-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Birst agilní obchodní analýza je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="93cbd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Birst Agile Business Analytics is to a user in Azure AD.</span></span> <span data-ttu-id="93cbd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Birst agilní obchodní analýza musí navázat.</span><span class="sxs-lookup"><span data-stu-id="93cbd-140">In other words, a link relationship between an Azure AD user and the related user in Birst Agile Business Analytics needs to be established.</span></span>

<span data-ttu-id="93cbd-141">V obchodní analýza agilní Birst přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="93cbd-141">In Birst Agile Business Analytics, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="93cbd-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Birst agilní obchodní analýza, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="93cbd-142">To configure and test Azure AD single sign-on with Birst Agile Business Analytics, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="93cbd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="93cbd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="93cbd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="93cbd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="93cbd-145">**[Vytvoření zkušebního uživatele Birst agilní obchodní analýza](#creating-a-birst-agile-business-analytics-test-user)**  – Pokud chcete mít protějšek Britta Simon v Birst agilní obchodní analýza propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="93cbd-145">**[Creating a Birst Agile Business Analytics test user](#creating-a-birst-agile-business-analytics-test-user)** - to have a counterpart of Britta Simon in Birst Agile Business Analytics that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="93cbd-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="93cbd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="93cbd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="93cbd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="93cbd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="93cbd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="93cbd-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Birst agilní obchodní analýzy.</span><span class="sxs-lookup"><span data-stu-id="93cbd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Birst Agile Business Analytics application.</span></span>

<span data-ttu-id="93cbd-150">**Ke konfiguraci Azure AD jednotné přihlašování s Birst agilní obchodní analýza, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="93cbd-150">**To configure Azure AD single sign-on with Birst Agile Business Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="93cbd-151">Na portálu Azure na **Birst Agile obchodní analýza** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-151">In the Azure portal, on the **Birst Agile Business Analytics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="93cbd-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="93cbd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_samlbase.png)

3. <span data-ttu-id="93cbd-155">Na **Birst Agile obchodní analýzy domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="93cbd-155">On the **Birst Agile Business Analytics Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_url.png)

     <span data-ttu-id="93cbd-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="93cbd-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

     <span data-ttu-id="93cbd-158">Adresa URL, závisí na datovém centru nachází účtu Birst:</span><span class="sxs-lookup"><span data-stu-id="93cbd-158">The URL depends on the datacenter that your Birst account is located:</span></span> 

     * <span data-ttu-id="93cbd-159">Pro USA datacenter postupujte podle následujícího vzoru:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="93cbd-159">For US datacenter use following the pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span> 

     * <span data-ttu-id="93cbd-160">Pro Evropu datacenter používají následující vzorec:`https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="93cbd-160">For Europe datacenter use the following pattern: `https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="93cbd-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="93cbd-161">This value is not real.</span></span> <span data-ttu-id="93cbd-162">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="93cbd-162">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="93cbd-163">Obraťte se na [tým podpory Birst agilní obchodní analýzy klienta](mailto:info@birst.com) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="93cbd-163">Contact [Birst Agile Business Analytics Client support team](mailto:info@birst.com) to get the value.</span></span> 
 
4. <span data-ttu-id="93cbd-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="93cbd-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_certificate.png) 

5. <span data-ttu-id="93cbd-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="93cbd-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="93cbd-168">Na **Birst agilní konfigurace obchodní analýzy** klikněte na tlačítko **konfigurace Birst agilní obchodní analýza** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="93cbd-168">On the **Birst Agile Business Analytics Configuration** section, click **Configure Birst Agile Business Analytics** to open **Configure sign-on** window.</span></span> <span data-ttu-id="93cbd-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="93cbd-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_configure.png) 

7. <span data-ttu-id="93cbd-171">Konfigurace jednotného přihlašování na **Birst Agile obchodní analýza** straně, budete muset odeslat stažené **certifikátu (Base64)**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory Birst agilní obchodní analýza](mailto:info@birst.com).</span><span class="sxs-lookup"><span data-stu-id="93cbd-171">To configure single sign-on on **Birst Agile Business Analytics** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Birst Agile Business Analytics support team](mailto:info@birst.com).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="93cbd-172">Birst týmu zmínili, že tato integrace potřebuje algoritmus SHA256 (SHA1 nebude podporuje), aby se můžete nastavit jednotné přihlašování na příslušný server jako **app2101** atd.</span><span class="sxs-lookup"><span data-stu-id="93cbd-172">Mention to Birst team that this integration needs SHA256 Algorithm (SHA1 will not be supported) so that they can set the SSO on the appropriate server like **app2101** etc.</span></span>
  

> [!TIP]
> <span data-ttu-id="93cbd-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="93cbd-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="93cbd-174">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="93cbd-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="93cbd-175">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="93cbd-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="93cbd-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="93cbd-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="93cbd-177">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="93cbd-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="93cbd-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="93cbd-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="93cbd-180">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="93cbd-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="93cbd-182">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="93cbd-184">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="93cbd-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="93cbd-186">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="93cbd-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="93cbd-188">a.</span><span class="sxs-lookup"><span data-stu-id="93cbd-188">a.</span></span> <span data-ttu-id="93cbd-189">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="93cbd-190">b.</span><span class="sxs-lookup"><span data-stu-id="93cbd-190">b.</span></span> <span data-ttu-id="93cbd-191">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="93cbd-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="93cbd-192">c.</span><span class="sxs-lookup"><span data-stu-id="93cbd-192">c.</span></span> <span data-ttu-id="93cbd-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="93cbd-194">d.</span><span class="sxs-lookup"><span data-stu-id="93cbd-194">d.</span></span> <span data-ttu-id="93cbd-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-195">Click **Create**.</span></span>
 
### <a name="creating-a-birst-agile-business-analytics-test-user"></a><span data-ttu-id="93cbd-196">Vytvoření zkušebního uživatele Birst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="93cbd-196">Creating a Birst Agile Business Analytics test user</span></span>

<span data-ttu-id="93cbd-197">Cílem této části je vytvoření uživatele volal Britta Simon v Birst agilní obchodní analýza.</span><span class="sxs-lookup"><span data-stu-id="93cbd-197">The objective of this section is to create a user called Britta Simon in Birst Agile Business Analytics.</span></span> <span data-ttu-id="93cbd-198">Práce s [tým podpory Birst agilní obchodní analýza](mailto:info@birst.com) přidat uživatele do Birst účtu.</span><span class="sxs-lookup"><span data-stu-id="93cbd-198">Work with [Birst Agile Business Analytics support team](mailto:info@birst.com) to add the users in the Birst account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="93cbd-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="93cbd-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="93cbd-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Birst agilní obchodní analýzy.</span><span class="sxs-lookup"><span data-stu-id="93cbd-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Birst Agile Business Analytics.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="93cbd-202">**Pokud chcete přiřadit Britta Simon Birst agilní obchodní analýza, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="93cbd-202">**To assign Britta Simon to Birst Agile Business Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="93cbd-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="93cbd-205">V seznamu aplikací vyberte **Birst Agile obchodní analýza**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-205">In the applications list, select **Birst Agile Business Analytics**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_app.png) 

3. <span data-ttu-id="93cbd-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="93cbd-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="93cbd-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="93cbd-209">Click **Add** button.</span></span> <span data-ttu-id="93cbd-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="93cbd-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="93cbd-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="93cbd-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="93cbd-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="93cbd-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="93cbd-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="93cbd-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="93cbd-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="93cbd-215">Testing single sign-on</span></span>

<span data-ttu-id="93cbd-216">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="93cbd-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="93cbd-217">Když kliknete na dlaždici Birst agilní obchodní analýza na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Birst agilní obchodní analýzy.</span><span class="sxs-lookup"><span data-stu-id="93cbd-217">When you click the Birst Agile Business Analytics tile in the Access Panel, you should get automatically signed-on to your Birst Agile Business Analytics application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="93cbd-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="93cbd-218">Additional resources</span></span>

* [<span data-ttu-id="93cbd-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="93cbd-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="93cbd-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="93cbd-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-birst-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-birst-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-birst-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-birst-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-birst-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-birst-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-birst-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-birst-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-birst-tutorial/tutorial_general_203.png

