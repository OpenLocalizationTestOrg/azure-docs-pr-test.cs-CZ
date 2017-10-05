---
title: 'Kurz: Azure Active Directory integrace s Kiteworks | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Kiteworks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7984aaf-ab1f-4a85-9646-a9523f5275d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 2fd9b346cb6d838069ef94ee9c2a8d113f22779c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kiteworks"></a><span data-ttu-id="c545d-103">Kurz: Azure Active Directory integrace s Kiteworks</span><span class="sxs-lookup"><span data-stu-id="c545d-103">Tutorial: Azure Active Directory integration with Kiteworks</span></span>

<span data-ttu-id="c545d-104">V tomto kurzu zjistěte, jak integrovat Kiteworks s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c545d-104">In this tutorial, you learn how to integrate Kiteworks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c545d-105">Integrace Kiteworks s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c545d-105">Integrating Kiteworks with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c545d-106">Můžete řídit ve službě Azure AD, který má přístup k Kiteworks</span><span class="sxs-lookup"><span data-stu-id="c545d-106">You can control in Azure AD who has access to Kiteworks</span></span>
- <span data-ttu-id="c545d-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Kiteworks (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c545d-107">You can enable your users to automatically get signed-on to Kiteworks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c545d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c545d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c545d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c545d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c545d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c545d-110">Prerequisites</span></span>

<span data-ttu-id="c545d-111">Konfigurace integrace Azure AD s Kiteworks, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c545d-111">To configure Azure AD integration with Kiteworks, you need the following items:</span></span>

- <span data-ttu-id="c545d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c545d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c545d-113">Kiteworks jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c545d-113">A Kiteworks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c545d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c545d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c545d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c545d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c545d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c545d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c545d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c545d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c545d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c545d-118">Scenario description</span></span>
<span data-ttu-id="c545d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c545d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c545d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c545d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c545d-121">Přidání Kiteworks z Galerie</span><span class="sxs-lookup"><span data-stu-id="c545d-121">Adding Kiteworks from the gallery</span></span>
2. <span data-ttu-id="c545d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c545d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kiteworks-from-the-gallery"></a><span data-ttu-id="c545d-123">Přidání Kiteworks z Galerie</span><span class="sxs-lookup"><span data-stu-id="c545d-123">Adding Kiteworks from the gallery</span></span>
<span data-ttu-id="c545d-124">Při konfiguraci integrace Kiteworks do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Kiteworks z galerie.</span><span class="sxs-lookup"><span data-stu-id="c545d-124">To configure the integration of Kiteworks into Azure AD, you need to add Kiteworks from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c545d-125">**Pokud chcete přidat Kiteworks z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c545d-125">**To add Kiteworks from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c545d-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c545d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c545d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c545d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c545d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c545d-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c545d-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c545d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c545d-133">Do vyhledávacího pole zadejte **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="c545d-133">In the search box, type **Kiteworks**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_search.png)

5. <span data-ttu-id="c545d-135">Na panelu výsledků vyberte **Kiteworks**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c545d-135">In the results panel, select **Kiteworks**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c545d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c545d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c545d-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kiteworks podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c545d-138">In this section, you configure and test Azure AD single sign-on with Kiteworks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c545d-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Kiteworks je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c545d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kiteworks is to a user in Azure AD.</span></span> <span data-ttu-id="c545d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Kiteworks musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c545d-140">In other words, a link relationship between an Azure AD user and the related user in Kiteworks needs to be established.</span></span>

<span data-ttu-id="c545d-141">V Kiteworks, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="c545d-141">In Kiteworks, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c545d-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kiteworks, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c545d-142">To configure and test Azure AD single sign-on with Kiteworks, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c545d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c545d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c545d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c545d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c545d-145">**[Vytvoření zkušebního uživatele Kiteworks](#creating-a-kiteworks-test-user)**  – Pokud chcete mít protějšek Britta Simon v Kiteworks propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c545d-145">**[Creating a Kiteworks test user](#creating-a-kiteworks-test-user)** - to have a counterpart of Britta Simon in Kiteworks that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c545d-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c545d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c545d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c545d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c545d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c545d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c545d-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Kiteworks.</span><span class="sxs-lookup"><span data-stu-id="c545d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kiteworks application.</span></span>

<span data-ttu-id="c545d-150">**Ke konfiguraci Azure AD jednotné přihlašování s Kiteworks, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c545d-150">**To configure Azure AD single sign-on with Kiteworks, perform the following steps:**</span></span>

1. <span data-ttu-id="c545d-151">Na portálu Azure na **Kiteworks** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c545d-151">In the Azure portal, on the **Kiteworks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c545d-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c545d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_samlbase.png)

3. <span data-ttu-id="c545d-155">Na **Kiteworks domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c545d-155">On the **Kiteworks Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_url.png)

    <span data-ttu-id="c545d-157">a.</span><span class="sxs-lookup"><span data-stu-id="c545d-157">a.</span></span> <span data-ttu-id="c545d-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.kiteworks.com`</span><span class="sxs-lookup"><span data-stu-id="c545d-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.kiteworks.com`</span></span>

    <span data-ttu-id="c545d-159">b.</span><span class="sxs-lookup"><span data-stu-id="c545d-159">b.</span></span> <span data-ttu-id="c545d-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span><span class="sxs-lookup"><span data-stu-id="c545d-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c545d-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c545d-161">These values are not real.</span></span> <span data-ttu-id="c545d-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c545d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c545d-163">Obraťte se na [tým podpory Kiteworks klienta](http://accellion.com/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="c545d-163">Contact [Kiteworks Client support team](http://accellion.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="c545d-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="c545d-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_certificate.png) 

5. <span data-ttu-id="c545d-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c545d-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kiteworks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c545d-168">Na **Kiteworks konfigurace** klikněte na tlačítko **konfigurace Kiteworks** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c545d-168">On the **Kiteworks Configuration** section, click **Configure Kiteworks** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c545d-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c545d-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_configure.png) 

7. <span data-ttu-id="c545d-171">Přihlásit se k serveru vaší společnosti Kiteworks jako správce.</span><span class="sxs-lookup"><span data-stu-id="c545d-171">Sign on to your Kiteworks company site as an administrator.</span></span>

8. <span data-ttu-id="c545d-172">Na panelu nástrojů v horní části klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c545d-172">In the toolbar on the top, click **Settings**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_06.png) 

9. <span data-ttu-id="c545d-174">V **ověřování a autorizace** klikněte na tlačítko **jednotného přihlašování k instalaci**.</span><span class="sxs-lookup"><span data-stu-id="c545d-174">In the **Authentication and Authorization** section, click **SSO Setup**.</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_07.png)
 
10. <span data-ttu-id="c545d-176">Na stránce nastavení jednotného přihlašování proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c545d-176">On the SSO Setup page, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_09.png)   

    <span data-ttu-id="c545d-178">a.</span><span class="sxs-lookup"><span data-stu-id="c545d-178">a.</span></span> <span data-ttu-id="c545d-179">Vyberte **ověřování prostřednictvím jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="c545d-179">Select **Authenticate via SSO**.</span></span>

    <span data-ttu-id="c545d-180">b.</span><span class="sxs-lookup"><span data-stu-id="c545d-180">b.</span></span> <span data-ttu-id="c545d-181">Vyberte **zahájit AuthnRequest**.</span><span class="sxs-lookup"><span data-stu-id="c545d-181">Select **Initiate AuthnRequest**.</span></span>

    <span data-ttu-id="c545d-182">c.</span><span class="sxs-lookup"><span data-stu-id="c545d-182">c.</span></span> <span data-ttu-id="c545d-183">V **IDP Entity ID** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c545d-183">In the **IDP Entity ID** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="c545d-184">d.</span><span class="sxs-lookup"><span data-stu-id="c545d-184">d.</span></span> <span data-ttu-id="c545d-185">V **jeden přihlašování adresa URL služby** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c545d-185">In the **Single Sign-On Service URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="c545d-186">e.</span><span class="sxs-lookup"><span data-stu-id="c545d-186">e.</span></span> <span data-ttu-id="c545d-187">V **jedné adresy URL odhlašovací služby** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c545d-187">In the **Single Logout Service URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="c545d-188">f.</span><span class="sxs-lookup"><span data-stu-id="c545d-188">f.</span></span> <span data-ttu-id="c545d-189">V poznámkovém bloku otevřete stažený certifikát, kopírovat obsah a vložte ji do **certifikátu veřejného klíče RSA** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c545d-189">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **RSA Public Key Certificate** textbox.</span></span>
 
    <span data-ttu-id="c545d-190">g.</span><span class="sxs-lookup"><span data-stu-id="c545d-190">g.</span></span> <span data-ttu-id="c545d-191">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c545d-191">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="c545d-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c545d-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c545d-193">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c545d-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c545d-194">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c545d-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c545d-195">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c545d-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="c545d-196">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c545d-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c545d-198">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c545d-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c545d-199">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c545d-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c545d-201">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c545d-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c545d-203">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c545d-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c545d-205">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c545d-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c545d-207">a.</span><span class="sxs-lookup"><span data-stu-id="c545d-207">a.</span></span> <span data-ttu-id="c545d-208">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c545d-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c545d-209">b.</span><span class="sxs-lookup"><span data-stu-id="c545d-209">b.</span></span> <span data-ttu-id="c545d-210">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c545d-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c545d-211">c.</span><span class="sxs-lookup"><span data-stu-id="c545d-211">c.</span></span> <span data-ttu-id="c545d-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c545d-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c545d-213">d.</span><span class="sxs-lookup"><span data-stu-id="c545d-213">d.</span></span> <span data-ttu-id="c545d-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c545d-214">Click **Create**.</span></span>
 
### <a name="creating-a-kiteworks-test-user"></a><span data-ttu-id="c545d-215">Vytvoření zkušebního uživatele Kiteworks</span><span class="sxs-lookup"><span data-stu-id="c545d-215">Creating a Kiteworks test user</span></span>

<span data-ttu-id="c545d-216">Cílem této části je vytvoření uživatele v Kiteworks nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c545d-216">The objective of this section is to create a user called Britta Simon in Kiteworks.</span></span>

<span data-ttu-id="c545d-217">Kiteworks podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="c545d-217">Kiteworks supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="c545d-218">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="c545d-218">There is no action item for you in this section.</span></span> <span data-ttu-id="c545d-219">Nový uživatel se vytvoří během pokusu o přístup k Kitewors, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c545d-219">A new user is created during an attempt to access Kitewors if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="c545d-220">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory Kiteworks](http://accellion.com/support).</span><span class="sxs-lookup"><span data-stu-id="c545d-220">If you need to create a user manually, you need to contact the [Kiteworks support team](http://accellion.com/support).</span></span>
 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c545d-221">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c545d-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c545d-222">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Kiteworks.</span><span class="sxs-lookup"><span data-stu-id="c545d-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kiteworks.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c545d-224">**Pokud chcete přiřadit Britta Simon Kiteworks, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c545d-224">**To assign Britta Simon to Kiteworks, perform the following steps:**</span></span>

1. <span data-ttu-id="c545d-225">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c545d-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c545d-227">V seznamu aplikací vyberte **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="c545d-227">In the applications list, select **Kiteworks**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_app.png) 

3. <span data-ttu-id="c545d-229">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c545d-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c545d-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c545d-231">Click **Add** button.</span></span> <span data-ttu-id="c545d-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c545d-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c545d-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c545d-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c545d-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c545d-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c545d-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c545d-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c545d-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c545d-237">Testing single sign-on</span></span>

<span data-ttu-id="c545d-238">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="c545d-238">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="c545d-239">Když kliknete na dlaždici Kiteworks na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Kiteworks.</span><span class="sxs-lookup"><span data-stu-id="c545d-239">When you click the Kiteworks tile in the Access Panel, you should get automatically signed-on to your Kiteworks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c545d-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c545d-240">Additional resources</span></span>

* [<span data-ttu-id="c545d-241">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c545d-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c545d-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c545d-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_203.png

