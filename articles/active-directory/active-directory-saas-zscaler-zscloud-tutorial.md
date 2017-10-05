---
title: 'Kurz: Azure Active Directory integrace s Zscaler ZSCloud | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Zscaler ZSCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 2b6eb113e5725260bc04f50e9218939bf28b1ff0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a><span data-ttu-id="e09a2-103">Kurz: Azure Active Directory integrace s Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="e09a2-103">Tutorial: Azure Active Directory integration with Zscaler ZSCloud</span></span>

<span data-ttu-id="e09a2-104">V tomto kurzu zjistěte, jak integrovat Zscaler ZSCloud s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e09a2-104">In this tutorial, you learn how to integrate Zscaler ZSCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e09a2-105">Integrace Zscaler ZSCloud s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e09a2-105">Integrating Zscaler ZSCloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e09a2-106">Můžete řídit ve službě Azure AD, který má přístup k Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="e09a2-106">You can control in Azure AD who has access to Zscaler ZSCloud</span></span>
- <span data-ttu-id="e09a2-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Zscaler ZSCloud (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e09a2-107">You can enable your users to automatically get signed-on to Zscaler ZSCloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e09a2-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e09a2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e09a2-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e09a2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e09a2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e09a2-110">Prerequisites</span></span>

<span data-ttu-id="e09a2-111">Konfigurace integrace Azure AD s Zscaler ZSCloud, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e09a2-111">To configure Azure AD integration with Zscaler ZSCloud, you need the following items:</span></span>

- <span data-ttu-id="e09a2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e09a2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e09a2-113">Zscaler ZSCloud jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e09a2-113">A Zscaler ZSCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e09a2-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e09a2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e09a2-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e09a2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e09a2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e09a2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e09a2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e09a2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e09a2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e09a2-118">Scenario description</span></span>
<span data-ttu-id="e09a2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e09a2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e09a2-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e09a2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e09a2-121">Přidání Zscaler ZSCloud z Galerie</span><span class="sxs-lookup"><span data-stu-id="e09a2-121">Adding Zscaler ZSCloud from the gallery</span></span>
2. <span data-ttu-id="e09a2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e09a2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-zscloud-from-the-gallery"></a><span data-ttu-id="e09a2-123">Přidání Zscaler ZSCloud z Galerie</span><span class="sxs-lookup"><span data-stu-id="e09a2-123">Adding Zscaler ZSCloud from the gallery</span></span>
<span data-ttu-id="e09a2-124">Při konfiguraci integrace Zscaler ZSCloud do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Zscaler ZSCloud z galerie.</span><span class="sxs-lookup"><span data-stu-id="e09a2-124">To configure the integration of Zscaler ZSCloud into Azure AD, you need to add Zscaler ZSCloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e09a2-125">**Pokud chcete přidat Zscaler ZSCloud z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e09a2-125">**To add Zscaler ZSCloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e09a2-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e09a2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e09a2-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e09a2-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e09a2-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e09a2-133">Do vyhledávacího pole zadejte **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-133">In the search box, type **Zscaler ZSCloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. <span data-ttu-id="e09a2-135">Na panelu výsledků vyberte **Zscaler ZSCloud**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e09a2-135">In the results panel, select **Zscaler ZSCloud**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e09a2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e09a2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e09a2-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s Zscaler ZSCloud podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="e09a2-138">In this section, you configure and test Azure AD single sign-on with Zscaler ZSCloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e09a2-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Zscaler ZSCloud je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e09a2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler ZSCloud is to a user in Azure AD.</span></span> <span data-ttu-id="e09a2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Zscaler ZSCloud musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e09a2-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler ZSCloud needs to be established.</span></span>

<span data-ttu-id="e09a2-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="e09a2-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Zscaler ZSCloud.</span></span>

<span data-ttu-id="e09a2-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler ZSCloud, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e09a2-142">To configure and test Azure AD single sign-on with Zscaler ZSCloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e09a2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e09a2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e09a2-144">**[Konfigurace nastavení proxy serveru](#configuring-proxy-settings)**  – Pokud chcete nakonfigurovat nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="e09a2-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
2. <span data-ttu-id="e09a2-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e09a2-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)**  - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e09a2-146">**[Vytvoření zkušebního uživatele Zscaler ZSCloud](#creating-a-zscaler-zscloud-test-user)**  – Pokud chcete mít protějšek Britta Simon v Zscaler ZSCloud propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e09a2-146">**[Creating a Zscaler ZSCloud test user](#creating-a-zscaler-zscloud-test-user)** - to have a counterpart of Britta Simon in Zscaler ZSCloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e09a2-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e09a2-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e09a2-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e09a2-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e09a2-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e09a2-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e09a2-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="e09a2-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler ZSCloud application.</span></span>

<span data-ttu-id="e09a2-151">**Ke konfiguraci Azure AD jednotné přihlašování s Zscaler ZSCloud, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e09a2-151">**To configure Azure AD single sign-on with Zscaler ZSCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="e09a2-152">Na portálu Azure na **Zscaler ZSCloud** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-152">In the Azure portal, on the **Zscaler ZSCloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e09a2-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e09a2-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. <span data-ttu-id="e09a2-156">Na **Zscaler ZSCloud domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e09a2-156">On the **Zscaler ZSCloud Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     <span data-ttu-id="e09a2-158">V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlášení do aplikace ZScaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="e09a2-158">In the **Sign-on URL** textbox, type the URL used by your users to sign-on to your ZScaler ZSCloud application.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="e09a2-159">Budete muset aktualizovat tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e09a2-159">You have to update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="e09a2-160">Obraťte se na [tým podpory Zscaler ZSCloud klienta](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e09a2-160">Contact [Zscaler ZSCloud Client support team](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) to get this value.</span></span> 
 
4. <span data-ttu-id="e09a2-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="e09a2-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. <span data-ttu-id="e09a2-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e09a2-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e09a2-165">Na **Zscaler ZSCloud konfigurace** klikněte na tlačítko **konfigurace Zscaler ZSCloud** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-165">On the **Zscaler ZSCloud Configuration** section, click **Configure Zscaler ZSCloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e09a2-166">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e09a2-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. <span data-ttu-id="e09a2-168">V okně prohlížeče jiný web Přihlaste se na váš web společnosti ZScaler ZSCloud jako správce.</span><span class="sxs-lookup"><span data-stu-id="e09a2-168">In a different web browser window, log in to your ZScaler ZSCloud company site as an administrator.</span></span>

8. <span data-ttu-id="e09a2-169">V nabídce v horní části, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-169">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="e09a2-170">![Správa](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "správy")</span><span class="sxs-lookup"><span data-stu-id="e09a2-170">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="e09a2-171">V části **spravovat správci & role**, klikněte na tlačítko **Správa uživatelů a ověřování**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="e09a2-172">![Správa uživatelů a ověřování](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "správu uživatelů a ověřování")</span><span class="sxs-lookup"><span data-stu-id="e09a2-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="e09a2-173">V **zvolte možnosti ověřování pro vaši organizaci** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e09a2-173">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="e09a2-174">![Ověřování](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="e09a2-174">![Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="e09a2-175">a.</span><span class="sxs-lookup"><span data-stu-id="e09a2-175">a.</span></span> <span data-ttu-id="e09a2-176">Vyberte **ověření pomocí SAML Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="e09a2-177">b.</span><span class="sxs-lookup"><span data-stu-id="e09a2-177">b.</span></span> <span data-ttu-id="e09a2-178">Klikněte na tlačítko **konfigurace SAML jeden přihlašování parametrů**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="e09a2-179">Na **konfigurace SAML jeden přihlašování parametry** dialogové okno stránky, proveďte následující kroky a pak klikněte na **provést**</span><span class="sxs-lookup"><span data-stu-id="e09a2-179">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="e09a2-180">![Jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e09a2-180">![Single Sign-On](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="e09a2-181">a.</span><span class="sxs-lookup"><span data-stu-id="e09a2-181">a.</span></span> <span data-ttu-id="e09a2-182">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu do **URL SAML portálu, ke které se odesílají uživatele pro ověřování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e09a2-182">Paste the **SAML Single Sign-On Service URL** value into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="e09a2-183">b.</span><span class="sxs-lookup"><span data-stu-id="e09a2-183">b.</span></span> <span data-ttu-id="e09a2-184">V **atribut obsahující přihlašovací jméno** textovému poli, typ **NameID**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-184">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="e09a2-185">c.</span><span class="sxs-lookup"><span data-stu-id="e09a2-185">c.</span></span> <span data-ttu-id="e09a2-186">Chcete-li nahrát stažený certifikát, klikněte na tlačítko **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-186">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="e09a2-187">d.</span><span class="sxs-lookup"><span data-stu-id="e09a2-187">d.</span></span> <span data-ttu-id="e09a2-188">Vyberte **zapnout automatické zřizování SAML**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="e09a2-189">Na **konfigurace ověřování uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e09a2-189">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="e09a2-190">![Správa](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "správy")</span><span class="sxs-lookup"><span data-stu-id="e09a2-190">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="e09a2-191">a.</span><span class="sxs-lookup"><span data-stu-id="e09a2-191">a.</span></span> <span data-ttu-id="e09a2-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-192">Click **Save**.</span></span>

    <span data-ttu-id="e09a2-193">b.</span><span class="sxs-lookup"><span data-stu-id="e09a2-193">b.</span></span> <span data-ttu-id="e09a2-194">Klikněte na tlačítko **aktivovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="e09a2-195">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="e09a2-195">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="e09a2-196">Konfigurace nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="e09a2-196">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="e09a2-197">Spustit **aplikace Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="e09a2-198">Vyberte **Možnosti Internetu** z **nástroje** nabídku pro otevření **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-198">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="e09a2-199">![Možnosti Internetu](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Možnosti Internetu")</span><span class="sxs-lookup"><span data-stu-id="e09a2-199">![Internet Options](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="e09a2-200">Klikněte **připojení** kartě.</span><span class="sxs-lookup"><span data-stu-id="e09a2-200">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="e09a2-201">![Připojení](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "připojení")</span><span class="sxs-lookup"><span data-stu-id="e09a2-201">![Connections](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="e09a2-202">Klikněte na tlačítko **nastavení místní sítě** otevřete **nastavení místní sítě** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-202">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="e09a2-203">V části Proxy server proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e09a2-203">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="e09a2-204">![Proxy server](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "Proxy serveru")</span><span class="sxs-lookup"><span data-stu-id="e09a2-204">![Proxy server](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="e09a2-205">a.</span><span class="sxs-lookup"><span data-stu-id="e09a2-205">a.</span></span> <span data-ttu-id="e09a2-206">Vyberte **použít proxy server pro síť LAN**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="e09a2-207">b.</span><span class="sxs-lookup"><span data-stu-id="e09a2-207">b.</span></span> <span data-ttu-id="e09a2-208">Do textového pole adresu zadejte **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-208">In the Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="e09a2-209">c.</span><span class="sxs-lookup"><span data-stu-id="e09a2-209">c.</span></span> <span data-ttu-id="e09a2-210">Do textového pole Port zadejte **80**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-210">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="e09a2-211">d.</span><span class="sxs-lookup"><span data-stu-id="e09a2-211">d.</span></span> <span data-ttu-id="e09a2-212">Vyberte **Nepoužívat proxy server pro místní adresy**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="e09a2-213">e.</span><span class="sxs-lookup"><span data-stu-id="e09a2-213">e.</span></span> <span data-ttu-id="e09a2-214">Klikněte na tlačítko **OK** zavřete **nastavení místní sítě (LAN)** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-214">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="e09a2-215">Klikněte na tlačítko **OK** zavřete **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-215">Click **OK** to close the **Internet Options** dialog.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e09a2-216">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e09a2-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="e09a2-217">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e09a2-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e09a2-219">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e09a2-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e09a2-220">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e09a2-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e09a2-222">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-222">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e09a2-224">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-224">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e09a2-226">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e09a2-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e09a2-228">a.</span><span class="sxs-lookup"><span data-stu-id="e09a2-228">a.</span></span> <span data-ttu-id="e09a2-229">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e09a2-230">b.</span><span class="sxs-lookup"><span data-stu-id="e09a2-230">b.</span></span> <span data-ttu-id="e09a2-231">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e09a2-231">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e09a2-232">c.</span><span class="sxs-lookup"><span data-stu-id="e09a2-232">c.</span></span> <span data-ttu-id="e09a2-233">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e09a2-234">d.</span><span class="sxs-lookup"><span data-stu-id="e09a2-234">d.</span></span> <span data-ttu-id="e09a2-235">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-235">Click **Create**.</span></span>

### <a name="creating-a-zscaler-zscloud-test-user"></a><span data-ttu-id="e09a2-236">Vytvoření zkušebního uživatele Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="e09a2-236">Creating a Zscaler ZSCloud test user</span></span>

<span data-ttu-id="e09a2-237">Pokud chcete povolit uživatelům Azure AD přihlášení k ZScaler ZSCloud, musí být zřízená na ZScaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="e09a2-237">To enable Azure AD users to log in to ZScaler ZSCloud, they must be provisioned to ZScaler ZSCloud.</span></span>  
<span data-ttu-id="e09a2-238">V případě ZScaler ZSCloud zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="e09a2-238">In the case of ZScaler ZSCloud, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="e09a2-239">Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e09a2-239">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="e09a2-240">Přihlaste se k vaší **Zscaler** klienta.</span><span class="sxs-lookup"><span data-stu-id="e09a2-240">Log in to your **Zscaler** tenant.</span></span>

2. <span data-ttu-id="e09a2-241">Klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-241">Click **Administration**.</span></span>   
   
    <span data-ttu-id="e09a2-242">![Správa](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "správy")</span><span class="sxs-lookup"><span data-stu-id="e09a2-242">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="e09a2-243">Klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-243">Click **User Management**.</span></span>   
        
     <span data-ttu-id="e09a2-244">![Přidat](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="e09a2-244">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

4. <span data-ttu-id="e09a2-245">V **uživatelé** , klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-245">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="e09a2-246">![Přidat](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="e09a2-246">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="e09a2-247">V části přidat uživatele proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e09a2-247">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="e09a2-248">![Přidat uživatele](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="e09a2-248">![Add User](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="e09a2-249">a.</span><span class="sxs-lookup"><span data-stu-id="e09a2-249">a.</span></span> <span data-ttu-id="e09a2-250">Typ **UserID**, **zobrazované uživatelské jméno**, **heslo**, **Potvrdit heslo**a potom vyberte **skupiny** a **oddělení** platného účtu AAD chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="e09a2-250">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid AAD account you want to provision.</span></span>

    <span data-ttu-id="e09a2-251">b.</span><span class="sxs-lookup"><span data-stu-id="e09a2-251">b.</span></span> <span data-ttu-id="e09a2-252">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-252">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="e09a2-253">Můžete použít všechny ostatní ZScaler ZSCloud uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ZScaler ZSCloud zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="e09a2-253">You can use any other ZScaler ZSCloud user account creation tools or APIs provided by ZScaler ZSCloud to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e09a2-254">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e09a2-254">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e09a2-255">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="e09a2-255">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler ZSCloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e09a2-257">**Pokud chcete přiřadit Britta Simon Zscaler ZSCloud, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e09a2-257">**To assign Britta Simon to Zscaler ZSCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="e09a2-258">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-258">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e09a2-260">V seznamu aplikací vyberte **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-260">In the applications list, select **Zscaler ZSCloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. <span data-ttu-id="e09a2-262">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e09a2-262">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e09a2-264">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e09a2-264">Click **Add** button.</span></span> <span data-ttu-id="e09a2-265">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e09a2-267">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e09a2-267">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e09a2-268">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e09a2-269">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e09a2-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e09a2-270">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e09a2-270">Testing single sign-on</span></span>

<span data-ttu-id="e09a2-271">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="e09a2-271">If you want to test your single sign-on settings, open the Access Panel.</span></span>

<span data-ttu-id="e09a2-272">Když kliknete na dlaždici Zscaler ZSCloud na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="e09a2-272">When you click the Zscaler ZSCloud tile in the Access Panel, you should get automatically signed-on to your Zscaler ZSCloud application.</span></span>

<span data-ttu-id="e09a2-273">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e09a2-273">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e09a2-274">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e09a2-274">Additional resources</span></span>

* [<span data-ttu-id="e09a2-275">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e09a2-275">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e09a2-276">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e09a2-276">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_203.png

