---
title: 'Kurz: Azure Active Directory integrace s Nexonia | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Nexonia."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a93b771a-9bc3-444a-bdc0-457f8bb7e780
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/1/2017
ms.author: jeedes
ms.openlocfilehash: 1370fa64c2ddc25d3121c567ceea4828b1e50921
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nexonia"></a><span data-ttu-id="045b6-103">Kurz: Azure Active Directory integrace s Nexonia</span><span class="sxs-lookup"><span data-stu-id="045b6-103">Tutorial: Azure Active Directory integration with Nexonia</span></span>

<span data-ttu-id="045b6-104">V tomto kurzu zjistěte, jak integrovat Nexonia s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="045b6-104">In this tutorial, you learn how to integrate Nexonia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="045b6-105">Integrace Nexonia s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="045b6-105">Integrating Nexonia with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="045b6-106">Můžete řídit ve službě Azure AD, který má přístup k Nexonia</span><span class="sxs-lookup"><span data-stu-id="045b6-106">You can control in Azure AD who has access to Nexonia</span></span>
- <span data-ttu-id="045b6-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Nexonia (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="045b6-107">You can enable your users to automatically get signed-on to Nexonia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="045b6-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="045b6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="045b6-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma.</span><span class="sxs-lookup"><span data-stu-id="045b6-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="045b6-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="045b6-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="045b6-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="045b6-111">Prerequisites</span></span>

<span data-ttu-id="045b6-112">Konfigurace integrace Azure AD s Nexonia, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="045b6-112">To configure Azure AD integration with Nexonia, you need the following items:</span></span>

- <span data-ttu-id="045b6-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="045b6-113">An Azure AD subscription</span></span>
- <span data-ttu-id="045b6-114">Nexonia jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="045b6-114">A Nexonia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="045b6-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="045b6-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="045b6-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="045b6-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="045b6-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="045b6-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="045b6-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="045b6-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="045b6-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="045b6-119">Scenario description</span></span>
<span data-ttu-id="045b6-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="045b6-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="045b6-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="045b6-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="045b6-122">Přidání Nexonia z Galerie</span><span class="sxs-lookup"><span data-stu-id="045b6-122">Adding Nexonia from the gallery</span></span>
2. <span data-ttu-id="045b6-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="045b6-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-nexonia-from-the-gallery"></a><span data-ttu-id="045b6-124">Přidání Nexonia z Galerie</span><span class="sxs-lookup"><span data-stu-id="045b6-124">Adding Nexonia from the gallery</span></span>
<span data-ttu-id="045b6-125">Při konfiguraci integrace Nexonia do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Nexonia z galerie.</span><span class="sxs-lookup"><span data-stu-id="045b6-125">To configure the integration of Nexonia into Azure AD, you need to add Nexonia from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="045b6-126">**Pokud chcete přidat Nexonia z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="045b6-126">**To add Nexonia from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="045b6-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="045b6-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="045b6-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="045b6-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="045b6-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="045b6-130">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="045b6-132">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="045b6-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="045b6-134">Do vyhledávacího pole zadejte **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="045b6-134">In the search box, type **Nexonia**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_search.png)

5. <span data-ttu-id="045b6-136">Na panelu výsledků vyberte **Nexonia**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="045b6-136">In the results panel, select **Nexonia**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="045b6-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="045b6-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="045b6-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Nexonia podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="045b6-139">In this section, you configure and test Azure AD single sign-on with Nexonia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="045b6-140">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Nexonia je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="045b6-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Nexonia is to a user in Azure AD.</span></span> <span data-ttu-id="045b6-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Nexonia musí navázat.</span><span class="sxs-lookup"><span data-stu-id="045b6-141">In other words, a link relationship between an Azure AD user and the related user in Nexonia needs to be established.</span></span>

<span data-ttu-id="045b6-142">V Nexonia, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="045b6-142">In Nexonia, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="045b6-143">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Nexonia, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="045b6-143">To configure and test Azure AD single sign-on with Nexonia, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="045b6-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="045b6-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="045b6-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="045b6-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="045b6-146">**[Vytvoření zkušebního uživatele Nexonia](#creating-a-nexonia-test-user)**  – Pokud chcete mít protějšek Britta Simon v Nexonia propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="045b6-146">**[Creating a Nexonia test user](#creating-a-nexonia-test-user)** - to have a counterpart of Britta Simon in Nexonia that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="045b6-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="045b6-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="045b6-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="045b6-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="045b6-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="045b6-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="045b6-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Nexonia.</span><span class="sxs-lookup"><span data-stu-id="045b6-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Nexonia application.</span></span>

>[!Note]
><span data-ttu-id="045b6-151">Pokud máte problémy v integraci a odkazovat to [odkaz](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) pro Průvodce odstraňováním potíží s.</span><span class="sxs-lookup"><span data-stu-id="045b6-151">If you have issues in the integration then refer this [link](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) for troubleshooting guide.</span></span> <span data-ttu-id="045b6-152">Pokud ještě nebyly nalezeny řešení, pak vyvolat žádost o podporu z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="045b6-152">If you still have not found the solution, then raise the support request from Azure portal.</span></span>

<span data-ttu-id="045b6-153">**Ke konfiguraci Azure AD jednotné přihlašování s Nexonia, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="045b6-153">**To configure Azure AD single sign-on with Nexonia, perform the following steps:**</span></span>

1. <span data-ttu-id="045b6-154">Na portálu Azure na **Nexonia** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="045b6-154">In the Azure portal, on the **Nexonia** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="045b6-156">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="045b6-156">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_samlbase.png)

3. <span data-ttu-id="045b6-158">Na **Nexonia domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="045b6-158">On the **Nexonia Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_url.png)

    <span data-ttu-id="045b6-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span><span class="sxs-lookup"><span data-stu-id="045b6-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="045b6-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="045b6-161">This value is not real.</span></span> <span data-ttu-id="045b6-162">Aktualizujte tuto hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="045b6-162">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="045b6-163">Obraťte se na [tým podpory Nexonia](https://nexonia.zendesk.com/hc/requests/new) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="045b6-163">Contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) to get this value.</span></span> 


4. <span data-ttu-id="045b6-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="045b6-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_certificate.png) 

5. <span data-ttu-id="045b6-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="045b6-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="045b6-168">Na **Nexonia konfigurace** klikněte na tlačítko **konfigurace Nexonia** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="045b6-168">On the **Nexonia Configuration** section, click **Configure Nexonia** to open **Configure sign-on** window.</span></span> <span data-ttu-id="045b6-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="045b6-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_configure.png) 

7. <span data-ttu-id="045b6-171">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [tým podpory Nexonia](https://nexonia.zendesk.com/hc/requests/new) a poskytněte jim následující:</span><span class="sxs-lookup"><span data-stu-id="045b6-171">To get SSO configured for your application, contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) and provide them with the following:</span></span>

    <span data-ttu-id="045b6-172">• Stažené **certifikátu**</span><span class="sxs-lookup"><span data-stu-id="045b6-172">• The downloaded **certificate**</span></span>

    <span data-ttu-id="045b6-173">• **SAML Entity ID**</span><span class="sxs-lookup"><span data-stu-id="045b6-173">• The **SAML Entity ID**</span></span>

    <span data-ttu-id="045b6-174">• **Adresa URL služby jednotného přihlašování SAML**</span><span class="sxs-lookup"><span data-stu-id="045b6-174">• The **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="045b6-175">• **Odhlášení adresy URL**</span><span class="sxs-lookup"><span data-stu-id="045b6-175">• The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="045b6-176">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="045b6-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="045b6-177">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="045b6-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="045b6-178">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="045b6-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="045b6-179">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="045b6-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="045b6-180">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="045b6-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="045b6-182">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="045b6-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="045b6-183">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="045b6-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="045b6-185">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="045b6-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="045b6-187">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="045b6-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="045b6-189">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="045b6-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="045b6-191">a.</span><span class="sxs-lookup"><span data-stu-id="045b6-191">a.</span></span> <span data-ttu-id="045b6-192">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="045b6-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="045b6-193">b.</span><span class="sxs-lookup"><span data-stu-id="045b6-193">b.</span></span> <span data-ttu-id="045b6-194">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="045b6-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="045b6-195">c.</span><span class="sxs-lookup"><span data-stu-id="045b6-195">c.</span></span> <span data-ttu-id="045b6-196">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="045b6-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="045b6-197">d.</span><span class="sxs-lookup"><span data-stu-id="045b6-197">d.</span></span> <span data-ttu-id="045b6-198">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="045b6-198">Click **Create**.</span></span>
 
### <a name="creating-a-nexonia-test-user"></a><span data-ttu-id="045b6-199">Vytvoření zkušebního uživatele Nexonia</span><span class="sxs-lookup"><span data-stu-id="045b6-199">Creating a Nexonia test user</span></span>

<span data-ttu-id="045b6-200">V této části vytvoříte volal Britta Simon v Nexonia uživatele.</span><span class="sxs-lookup"><span data-stu-id="045b6-200">In this section, you create a user called Britta Simon in Nexonia.</span></span> <span data-ttu-id="045b6-201">Práce s [tým podpory Nexonia](https://nexonia.zendesk.com/hc/requests/new) přidat uživatele do Nexonia platformy.</span><span class="sxs-lookup"><span data-stu-id="045b6-201">Work with [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) to add the users in the Nexonia platform.</span></span> <span data-ttu-id="045b6-202">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="045b6-202">Users must be created and activated before you use single sign-on.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="045b6-203">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="045b6-203">Assigning the Azure AD test user</span></span>

<span data-ttu-id="045b6-204">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Nexonia.</span><span class="sxs-lookup"><span data-stu-id="045b6-204">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Nexonia.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="045b6-206">**Pokud chcete přiřadit Britta Simon Nexonia, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="045b6-206">**To assign Britta Simon to Nexonia, perform the following steps:**</span></span>

1. <span data-ttu-id="045b6-207">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="045b6-207">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="045b6-209">V seznamu aplikací vyberte **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="045b6-209">In the applications list, select **Nexonia**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_app.png) 

3. <span data-ttu-id="045b6-211">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="045b6-211">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="045b6-213">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="045b6-213">Click **Add** button.</span></span> <span data-ttu-id="045b6-214">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="045b6-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="045b6-216">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="045b6-216">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="045b6-217">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="045b6-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="045b6-218">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="045b6-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="045b6-219">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="045b6-219">Testing single sign-on</span></span>

<span data-ttu-id="045b6-220">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="045b6-220">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="045b6-221">Když kliknete na dlaždici Nexonia na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Nexonia.</span><span class="sxs-lookup"><span data-stu-id="045b6-221">When you click the Nexonia tile in the Access Panel, you should get automatically signed-on to your Nexonia application.</span></span>
<span data-ttu-id="045b6-222">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="045b6-222">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="045b6-223">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="045b6-223">Additional resources</span></span>

* [<span data-ttu-id="045b6-224">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="045b6-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="045b6-225">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="045b6-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_203.png

