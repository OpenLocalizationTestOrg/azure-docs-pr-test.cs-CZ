---
title: 'Kurz: Azure Active Directory integrace s Zscaler | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Zscaler."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68c453f7-aff1-4614-92d3-9b86f3ad99dc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 73c81691b68ee820e1d905a17b4f2ab6b6ceb5fd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler"></a><span data-ttu-id="5d2b4-103">Kurz: Azure Active Directory integrace s Zscaler</span><span class="sxs-lookup"><span data-stu-id="5d2b4-103">Tutorial: Azure Active Directory integration with Zscaler</span></span>

<span data-ttu-id="5d2b4-104">V tomto kurzu zjistěte, jak integrovat Zscaler s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5d2b4-104">In this tutorial, you learn how to integrate Zscaler with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5d2b4-105">Integrace Zscaler s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-105">Integrating Zscaler with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5d2b4-106">Můžete řídit ve službě Azure AD, který má přístup k Zscaler</span><span class="sxs-lookup"><span data-stu-id="5d2b4-106">You can control in Azure AD who has access to Zscaler</span></span>
- <span data-ttu-id="5d2b4-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Zscaler (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5d2b4-107">You can enable your users to automatically get signed-on to Zscaler (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5d2b4-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5d2b4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5d2b4-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5d2b4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d2b4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5d2b4-110">Prerequisites</span></span>

<span data-ttu-id="5d2b4-111">Konfigurace integrace Azure AD s Zscaler, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-111">To configure Azure AD integration with Zscaler, you need the following items:</span></span>

- <span data-ttu-id="5d2b4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5d2b4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5d2b4-113">Zscaler jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5d2b4-113">A Zscaler single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5d2b4-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5d2b4-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5d2b4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5d2b4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5d2b4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5d2b4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5d2b4-118">Scenario description</span></span>
<span data-ttu-id="5d2b4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5d2b4-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5d2b4-121">Přidání Zscaler z Galerie</span><span class="sxs-lookup"><span data-stu-id="5d2b4-121">Adding Zscaler from the gallery</span></span>
2. <span data-ttu-id="5d2b4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5d2b4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-from-the-gallery"></a><span data-ttu-id="5d2b4-123">Přidání Zscaler z Galerie</span><span class="sxs-lookup"><span data-stu-id="5d2b4-123">Adding Zscaler from the gallery</span></span>
<span data-ttu-id="5d2b4-124">Při konfiguraci integrace Zscaler do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Zscaler z galerie.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-124">To configure the integration of Zscaler into Azure AD, you need to add Zscaler from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5d2b4-125">**Pokud chcete přidat Zscaler z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5d2b4-125">**To add Zscaler from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5d2b4-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5d2b4-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5d2b4-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5d2b4-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5d2b4-133">Do vyhledávacího pole zadejte **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-133">In the search box, type **Zscaler**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_search.png)

5. <span data-ttu-id="5d2b4-135">Na panelu výsledků vyberte **Zscaler**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-135">In the results panel, select **Zscaler**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5d2b4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5d2b4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5d2b4-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5d2b4-138">In this section, you configure and test Azure AD single sign-on with Zscaler based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5d2b4-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Zscaler je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler is to a user in Azure AD.</span></span> <span data-ttu-id="5d2b4-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Zscaler musí navázat.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler needs to be established.</span></span>

<span data-ttu-id="5d2b4-141">V Zscaler, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-141">In Zscaler, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5d2b4-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-142">To configure and test Azure AD single sign-on with Zscaler, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5d2b4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5d2b4-144">**[Konfigurace nastavení proxy serveru](#configuring-proxy-settings)**  – Pokud chcete nakonfigurovat nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="5d2b4-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="5d2b4-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="5d2b4-146">**[Vytvoření zkušebního uživatele Zscaler](#creating-a-zscaler-test-user)**  – Pokud chcete mít protějšek Britta Simon v Zscaler propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-146">**[Creating a Zscaler test user](#creating-a-zscaler-test-user)** - to have a counterpart of Britta Simon in Zscaler that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="5d2b4-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="5d2b4-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5d2b4-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5d2b4-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5d2b4-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Zscaler.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler application.</span></span>

<span data-ttu-id="5d2b4-151">**Ke konfiguraci Azure AD jednotné přihlašování s Zscaler, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5d2b4-151">**To configure Azure AD single sign-on with Zscaler, perform the following steps:**</span></span>

1. <span data-ttu-id="5d2b4-152">Na portálu Azure na **Zscaler** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-152">In the Azure portal, on the **Zscaler** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5d2b4-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_samlbase.png)

3. <span data-ttu-id="5d2b4-156">Na **Zscaler domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-156">On the **Zscaler Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_url.png)

    <span data-ttu-id="5d2b4-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.zsclaer.net`</span><span class="sxs-lookup"><span data-stu-id="5d2b4-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.zsclaer.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5d2b4-159">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-159">This value is not real.</span></span> <span data-ttu-id="5d2b4-160">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-160">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="5d2b4-161">Obraťte se na [tým podpory Zscaler klienta](https://www.zscaler.com/company/contact) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-161">Contact [Zscaler Client support team](https://www.zscaler.com/company/contact) to get this value.</span></span> 

4. <span data-ttu-id="5d2b4-162">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_certificate.png) 

5. <span data-ttu-id="5d2b4-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-164">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5d2b4-166">Na **Zscaler konfigurace** klikněte na tlačítko **konfigurace Zscaler** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-166">On the **Zscaler Configuration** section, click **Configure Zscaler** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5d2b4-167">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5d2b4-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_configure.png) 

7. <span data-ttu-id="5d2b4-169">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti ZScaler jako správce.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-169">In a different web browser window, log in to your ZScaler company site as an administrator.</span></span>

8. <span data-ttu-id="5d2b4-170">V nabídce v horní části, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-170">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="5d2b4-171">![Správa](./media/active-directory-saas-zscaler-tutorial/ic800206.png "správy")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-171">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="5d2b4-172">V části **spravovat správci & role**, klikněte na tlačítko **Správa uživatelů a ověřování**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-172">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="5d2b4-173">![Správa uživatelů a ověřování](./media/active-directory-saas-zscaler-tutorial/ic800207.png "správu uživatelů a ověřování")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-173">![Manage Users & Authentication](./media/active-directory-saas-zscaler-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="5d2b4-174">V **zvolte možnosti ověřování pro vaši organizaci** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-174">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="5d2b4-175">![Ověřování](./media/active-directory-saas-zscaler-tutorial/ic800208.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-175">![Authentication](./media/active-directory-saas-zscaler-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="5d2b4-176">a.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-176">a.</span></span> <span data-ttu-id="5d2b4-177">Vyberte **ověření pomocí SAML Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-177">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="5d2b4-178">b.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-178">b.</span></span> <span data-ttu-id="5d2b4-179">Klikněte na tlačítko **konfigurace SAML jeden přihlašování parametrů**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-179">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="5d2b4-180">Na **konfigurace SAML jeden přihlašování parametry** dialogové okno stránky, proveďte následující kroky a pak klikněte na **provést**</span><span class="sxs-lookup"><span data-stu-id="5d2b4-180">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="5d2b4-181">![Jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/ic800209.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-181">![Single Sign-On](./media/active-directory-saas-zscaler-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="5d2b4-182">a.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-182">a.</span></span> <span data-ttu-id="5d2b4-183">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure do **URL SAML portálu, ke které se odesílají uživatele pro ověřování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-183">Paste the **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="5d2b4-184">b.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-184">b.</span></span> <span data-ttu-id="5d2b4-185">V **atribut obsahující přihlašovací jméno** textovému poli, typ **NameID**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-185">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="5d2b4-186">c.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-186">c.</span></span> <span data-ttu-id="5d2b4-187">Chcete-li nahrát stažený certifikát, klikněte na tlačítko **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-187">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="5d2b4-188">d.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-188">d.</span></span> <span data-ttu-id="5d2b4-189">Vyberte **zapnout automatické zřizování SAML**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-189">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="5d2b4-190">Na **konfigurace ověřování uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-190">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="5d2b4-191">![Správa](./media/active-directory-saas-zscaler-tutorial/ic800210.png "správy")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-191">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="5d2b4-192">a.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-192">a.</span></span> <span data-ttu-id="5d2b4-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-193">Click **Save**.</span></span>

    <span data-ttu-id="5d2b4-194">b.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-194">b.</span></span> <span data-ttu-id="5d2b4-195">Klikněte na tlačítko **aktivovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-195">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="5d2b4-196">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="5d2b4-196">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="5d2b4-197">Konfigurace nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="5d2b4-197">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="5d2b4-198">Spustit **aplikace Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-198">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="5d2b4-199">Vyberte **Možnosti Internetu** z **nástroje** nabídku pro otevření **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-199">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="5d2b4-200">![Možnosti Internetu](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Možnosti Internetu")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-200">![Internet Options](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="5d2b4-201">Klikněte **připojení** kartě.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-201">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="5d2b4-202">![Připojení](./media/active-directory-saas-zscaler-tutorial/ic769493.png "připojení")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-202">![Connections](./media/active-directory-saas-zscaler-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="5d2b4-203">Klikněte na tlačítko **nastavení místní sítě** otevřete **nastavení místní sítě** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-203">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="5d2b4-204">V části Proxy server proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-204">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="5d2b4-205">![Proxy server](./media/active-directory-saas-zscaler-tutorial/ic769494.png "Proxy serveru")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-205">![Proxy server](./media/active-directory-saas-zscaler-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="5d2b4-206">a.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-206">a.</span></span> <span data-ttu-id="5d2b4-207">Vyberte **použít proxy server pro síť LAN**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-207">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="5d2b4-208">b.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-208">b.</span></span> <span data-ttu-id="5d2b4-209">Do textového pole adresu zadejte **gateway.zscaler.net**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-209">In the Address textbox, type **gateway.zscaler.net**.</span></span>

    <span data-ttu-id="5d2b4-210">c.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-210">c.</span></span> <span data-ttu-id="5d2b4-211">Do textového pole Port zadejte **80**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-211">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="5d2b4-212">d.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-212">d.</span></span> <span data-ttu-id="5d2b4-213">Vyberte **Nepoužívat proxy server pro místní adresy**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-213">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="5d2b4-214">e.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-214">e.</span></span> <span data-ttu-id="5d2b4-215">Klikněte na tlačítko **OK** zavřete **nastavení místní sítě (LAN)** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-215">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="5d2b4-216">Klikněte na tlačítko **OK** zavřete **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-216">Click **OK** to close the **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="5d2b4-217">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5d2b4-217">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5d2b4-218">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-218">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5d2b4-219">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5d2b4-219">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5d2b4-220">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5d2b4-220">Creating an Azure AD test user</span></span>
<span data-ttu-id="5d2b4-221">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-221">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5d2b4-223">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5d2b4-223">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5d2b4-224">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-224">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5d2b4-226">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-226">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5d2b4-228">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-228">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5d2b4-230">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-230">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5d2b4-232">a.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-232">a.</span></span> <span data-ttu-id="5d2b4-233">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-233">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5d2b4-234">b.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-234">b.</span></span> <span data-ttu-id="5d2b4-235">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-235">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5d2b4-236">c.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-236">c.</span></span> <span data-ttu-id="5d2b4-237">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-237">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5d2b4-238">d.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-238">d.</span></span> <span data-ttu-id="5d2b4-239">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-239">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-test-user"></a><span data-ttu-id="5d2b4-240">Vytvoření zkušebního uživatele Zscaler</span><span class="sxs-lookup"><span data-stu-id="5d2b4-240">Creating a Zscaler test user</span></span>

<span data-ttu-id="5d2b4-241">Pokud chcete povolit uživatelům Azure AD přihlášení k ZScaler, musí být zřízená k ZScaler.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-241">To enable Azure AD users to log in to ZScaler, they must be provisioned to ZScaler.</span></span>  
<span data-ttu-id="5d2b4-242">V případě ZScaler zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-242">In the case of ZScaler, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="5d2b4-243">Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-243">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="5d2b4-244">Přihlaste se k vaší **Zscaler** klienta.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-244">Log in to your **Zscaler** tenant.</span></span>

2. <span data-ttu-id="5d2b4-245">Klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-245">Click **Administration**.</span></span>   
   
    <span data-ttu-id="5d2b4-246">![Správa](./media/active-directory-saas-zscaler-tutorial/ic781035.png "správy")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-246">![Administration](./media/active-directory-saas-zscaler-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="5d2b4-247">Klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-247">Click **User Management**.</span></span>   
        
     <span data-ttu-id="5d2b4-248">![Přidat](./media/active-directory-saas-zscaler-tutorial/ic781036.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-248">![Add](./media/active-directory-saas-zscaler-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="5d2b4-249">V **uživatelé** , klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-249">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="5d2b4-250">![Přidat](./media/active-directory-saas-zscaler-tutorial/ic781037.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-250">![Add](./media/active-directory-saas-zscaler-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="5d2b4-251">V části přidat uživatele proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5d2b4-251">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="5d2b4-252">![Přidat uživatele](./media/active-directory-saas-zscaler-tutorial/ic781038.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="5d2b4-252">![Add User](./media/active-directory-saas-zscaler-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="5d2b4-253">a.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-253">a.</span></span> <span data-ttu-id="5d2b4-254">Typ **UserID**, **zobrazované uživatelské jméno**, **heslo**, **Potvrdit heslo**a potom vyberte **skupiny** a **oddělení** platného účtu AAD chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-254">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid AAD account you want to provision.</span></span>

    <span data-ttu-id="5d2b4-255">b.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-255">b.</span></span> <span data-ttu-id="5d2b4-256">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-256">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="5d2b4-257">Můžete použít všechny ostatní ZScaler uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ZScaler zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-257">You can use any other ZScaler user account creation tools or APIs provided by ZScaler to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5d2b4-258">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5d2b4-258">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5d2b4-259">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Zscaler.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-259">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5d2b4-261">**Pokud chcete přiřadit Britta Simon Zscaler, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5d2b4-261">**To assign Britta Simon to Zscaler, perform the following steps:**</span></span>

1. <span data-ttu-id="5d2b4-262">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-262">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5d2b4-264">V seznamu aplikací vyberte **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-264">In the applications list, select **Zscaler**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_app.png) 

3. <span data-ttu-id="5d2b4-266">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-266">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5d2b4-268">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-268">Click **Add** button.</span></span> <span data-ttu-id="5d2b4-269">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-269">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5d2b4-271">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-271">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5d2b4-272">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-272">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5d2b4-273">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-273">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5d2b4-274">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5d2b4-274">Testing single sign-on</span></span>

<span data-ttu-id="5d2b4-275">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-275">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5d2b4-276">Když kliknete na dlaždici Zscaler na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Zscaler.</span><span class="sxs-lookup"><span data-stu-id="5d2b4-276">When you click the Zscaler tile in the Access Panel, you should get automatically signed-on to your Zscaler application.</span></span>
<span data-ttu-id="5d2b4-277">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5d2b4-277">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d2b4-278">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5d2b4-278">Additional resources</span></span>

* [<span data-ttu-id="5d2b4-279">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d2b4-279">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5d2b4-280">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5d2b4-280">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_203.png

