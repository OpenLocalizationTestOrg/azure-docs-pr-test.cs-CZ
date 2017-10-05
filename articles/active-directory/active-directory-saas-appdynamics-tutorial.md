---
title: 'Kurz: Azure Active Directory integrace s AppDynamics | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a AppDynamics."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 25fd1df0-411c-4f55-8be3-4273b543100f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 634e68bdb937eba68b27b824dc62fe2677e24ffe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appdynamics"></a><span data-ttu-id="5b98b-103">Kurz: Azure Active Directory integrace s AppDynamics</span><span class="sxs-lookup"><span data-stu-id="5b98b-103">Tutorial: Azure Active Directory integration with AppDynamics</span></span>

<span data-ttu-id="5b98b-104">V tomto kurzu zjistěte, jak integrovat AppDynamics s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5b98b-104">In this tutorial, you learn how to integrate AppDynamics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5b98b-105">Integrace AppDynamics s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5b98b-105">Integrating AppDynamics with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5b98b-106">Můžete řídit ve službě Azure AD, který má přístup k AppDynamics</span><span class="sxs-lookup"><span data-stu-id="5b98b-106">You can control in Azure AD who has access to AppDynamics</span></span>
- <span data-ttu-id="5b98b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k AppDynamics (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5b98b-107">You can enable your users to automatically get signed-on to AppDynamics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5b98b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5b98b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5b98b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5b98b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b98b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5b98b-110">Prerequisites</span></span>

<span data-ttu-id="5b98b-111">Konfigurace integrace Azure AD s AppDynamics, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5b98b-111">To configure Azure AD integration with AppDynamics, you need the following items:</span></span>

- <span data-ttu-id="5b98b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5b98b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5b98b-113">AppDynamics jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5b98b-113">An AppDynamics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5b98b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b98b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5b98b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5b98b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5b98b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5b98b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5b98b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5b98b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5b98b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5b98b-118">Scenario description</span></span>
<span data-ttu-id="5b98b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b98b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5b98b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5b98b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5b98b-121">Přidání AppDynamics z Galerie</span><span class="sxs-lookup"><span data-stu-id="5b98b-121">Adding AppDynamics from the gallery</span></span>
2. <span data-ttu-id="5b98b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5b98b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appdynamics-from-the-gallery"></a><span data-ttu-id="5b98b-123">Přidání AppDynamics z Galerie</span><span class="sxs-lookup"><span data-stu-id="5b98b-123">Adding AppDynamics from the gallery</span></span>
<span data-ttu-id="5b98b-124">Při konfiguraci integrace AppDynamics do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS AppDynamics z galerie.</span><span class="sxs-lookup"><span data-stu-id="5b98b-124">To configure the integration of AppDynamics into Azure AD, you need to add AppDynamics from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5b98b-125">**Pokud chcete přidat AppDynamics z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5b98b-125">**To add AppDynamics from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5b98b-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5b98b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5b98b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5b98b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5b98b-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5b98b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5b98b-133">Do vyhledávacího pole zadejte **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-133">In the search box, type **AppDynamics**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_search.png)

5. <span data-ttu-id="5b98b-135">Na panelu výsledků vyberte **AppDynamics**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5b98b-135">In the results panel, select **AppDynamics**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5b98b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5b98b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5b98b-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s AppDynamics podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5b98b-138">In this section, you configure and test Azure AD single sign-on with AppDynamics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5b98b-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v AppDynamics je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5b98b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AppDynamics is to a user in Azure AD.</span></span> <span data-ttu-id="5b98b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v AppDynamics musí navázat.</span><span class="sxs-lookup"><span data-stu-id="5b98b-140">In other words, a link relationship between an Azure AD user and the related user in AppDynamics needs to be established.</span></span>

<span data-ttu-id="5b98b-141">V AppDynamics, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5b98b-141">In AppDynamics, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5b98b-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s AppDynamics, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5b98b-142">To configure and test Azure AD single sign-on with AppDynamics, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5b98b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5b98b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5b98b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5b98b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5b98b-145">**[Vytváření testovacího uživatele AppDynamics](#creating-an-appdynamics-test-user)**  – Pokud chcete mít protějšek Britta Simon v AppDynamics propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5b98b-145">**[Creating an AppDynamics test user](#creating-an-appdynamics-test-user)** - to have a counterpart of Britta Simon in AppDynamics that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5b98b-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5b98b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5b98b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b98b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5b98b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5b98b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5b98b-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="5b98b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AppDynamics application.</span></span>

<span data-ttu-id="5b98b-150">**Ke konfiguraci Azure AD jednotné přihlašování s AppDynamics, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5b98b-150">**To configure Azure AD single sign-on with AppDynamics, perform the following steps:**</span></span>

1. <span data-ttu-id="5b98b-151">Na portálu Azure na **AppDynamics** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-151">In the Azure portal, on the **AppDynamics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5b98b-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5b98b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_samlbase.png)

3. <span data-ttu-id="5b98b-155">Na **AppDynamics domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5b98b-155">On the **AppDynamics Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_url.png)

    <span data-ttu-id="5b98b-157">a.</span><span class="sxs-lookup"><span data-stu-id="5b98b-157">a.</span></span> <span data-ttu-id="5b98b-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.saas.appdynamics.com`</span><span class="sxs-lookup"><span data-stu-id="5b98b-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.saas.appdynamics.com`</span></span>

    <span data-ttu-id="5b98b-159">b.</span><span class="sxs-lookup"><span data-stu-id="5b98b-159">b.</span></span> <span data-ttu-id="5b98b-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.saas.appdynamics.com/controller`</span><span class="sxs-lookup"><span data-stu-id="5b98b-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.saas.appdynamics.com/controller`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5b98b-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5b98b-161">These values are not real.</span></span> <span data-ttu-id="5b98b-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5b98b-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5b98b-163">Obraťte se na [tým podpory AppDynamics klienta](https://www.appdynamics.com/support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5b98b-163">Contact [AppDynamics Client support team](https://www.appdynamics.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="5b98b-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="5b98b-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_certificate.png) 

5. <span data-ttu-id="5b98b-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5b98b-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5b98b-168">Na **AppDynamics konfigurace** klikněte na tlačítko **konfigurace AppDynamics** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5b98b-168">On the **AppDynamics Configuration** section, click **Configure AppDynamics** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5b98b-169">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5b98b-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_configure.png) 

7. <span data-ttu-id="5b98b-171">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti AppDynamics jako správce.</span><span class="sxs-lookup"><span data-stu-id="5b98b-171">In a different web browser window, log in to your AppDynamics company site as an administrator.</span></span>

8. <span data-ttu-id="5b98b-172">Na panelu nástrojů v horní části klikněte na tlačítko **nastavení**a potom klikněte na **správy**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-172">In the toolbar on the top, click **Settings**, and then click **Administration**.</span></span>
   
    <span data-ttu-id="5b98b-173">![Správa](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "správy")</span><span class="sxs-lookup"><span data-stu-id="5b98b-173">![Administration](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "Administration")</span></span>

9. <span data-ttu-id="5b98b-174">Klikněte **zprostředkovatele ověřování** kartě.</span><span class="sxs-lookup"><span data-stu-id="5b98b-174">Click the **Authentication Provider** tab.</span></span>
   
    <span data-ttu-id="5b98b-175">![Zprostředkovatel ověřování](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "zprostředkovatele ověřování")</span><span class="sxs-lookup"><span data-stu-id="5b98b-175">![Authentication Provider](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "Authentication Provider")</span></span>

10. <span data-ttu-id="5b98b-176">V **zprostředkovatele ověřování** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5b98b-176">In the **Authentication Provider** section, perform the following steps:</span></span>
   
    <span data-ttu-id="5b98b-177">![Konfigurace SAML](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="5b98b-177">![SAML Configuration](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML Configuration")</span></span>   

    <span data-ttu-id="5b98b-178">a.</span><span class="sxs-lookup"><span data-stu-id="5b98b-178">a.</span></span> <span data-ttu-id="5b98b-179">Jako **zprostředkovatele ověřování**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-179">As **Authentication Provider**, select **SAML**.</span></span>

    <span data-ttu-id="5b98b-180">b.</span><span class="sxs-lookup"><span data-stu-id="5b98b-180">b.</span></span> <span data-ttu-id="5b98b-181">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5b98b-181">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="5b98b-182">c.</span><span class="sxs-lookup"><span data-stu-id="5b98b-182">c.</span></span> <span data-ttu-id="5b98b-183">V **adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5b98b-183">In the **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="5b98b-184">d.</span><span class="sxs-lookup"><span data-stu-id="5b98b-184">d.</span></span> <span data-ttu-id="5b98b-185">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte jej do **certifikát** textbox</span><span class="sxs-lookup"><span data-stu-id="5b98b-185">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Certificate** textbox</span></span>

    <span data-ttu-id="5b98b-186">e.</span><span class="sxs-lookup"><span data-stu-id="5b98b-186">e.</span></span> <span data-ttu-id="5b98b-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-187">Click **Save**.</span></span>

     <span data-ttu-id="5b98b-188">![Uložit](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "uložit")</span><span class="sxs-lookup"><span data-stu-id="5b98b-188">![Save](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "Save")</span></span>

> [!TIP]
> <span data-ttu-id="5b98b-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5b98b-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5b98b-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5b98b-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5b98b-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5b98b-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5b98b-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5b98b-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="5b98b-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5b98b-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5b98b-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5b98b-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5b98b-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5b98b-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5b98b-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5b98b-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5b98b-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5b98b-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5b98b-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5b98b-204">a.</span><span class="sxs-lookup"><span data-stu-id="5b98b-204">a.</span></span> <span data-ttu-id="5b98b-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5b98b-206">b.</span><span class="sxs-lookup"><span data-stu-id="5b98b-206">b.</span></span> <span data-ttu-id="5b98b-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5b98b-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5b98b-208">c.</span><span class="sxs-lookup"><span data-stu-id="5b98b-208">c.</span></span> <span data-ttu-id="5b98b-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5b98b-210">d.</span><span class="sxs-lookup"><span data-stu-id="5b98b-210">d.</span></span> <span data-ttu-id="5b98b-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-211">Click **Create**.</span></span>
 
### <a name="creating-an-appdynamics-test-user"></a><span data-ttu-id="5b98b-212">Vytváření testovacího uživatele AppDynamics</span><span class="sxs-lookup"><span data-stu-id="5b98b-212">Creating an AppDynamics test user</span></span>

<span data-ttu-id="5b98b-213">Pokud chcete povolit uživatelům Azure AD přihlášení k AppDynamics, musí být zřízená do AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="5b98b-213">To enable Azure AD users to log in to AppDynamics, they must be provisioned into AppDynamics.</span></span> <span data-ttu-id="5b98b-214">V případě AppDynamics zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="5b98b-214">In the case of AppDynamics, provisioning is a manual task.</span></span>

<span data-ttu-id="5b98b-215">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5b98b-215">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="5b98b-216">Přihlaste se k serveru vaší společnosti AppDynamics jako správce.</span><span class="sxs-lookup"><span data-stu-id="5b98b-216">Log in to your AppDynamics company site as an administrator.</span></span>

2. <span data-ttu-id="5b98b-217">Přejděte na **uživatelé**a potom klikněte na  **+**  otevřete **vytvořit uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5b98b-217">Go to **Users**, and then click **+** to open the **Create User** dialog.</span></span>
   
    <span data-ttu-id="5b98b-218">![Uživatelé](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="5b98b-218">![Users](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "Users")</span></span>

3. <span data-ttu-id="5b98b-219">V **vytvořit uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5b98b-219">In the **Create User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="5b98b-220">![Vytvoření uživatele](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="5b98b-220">![Create User](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "Create User")</span></span>
   
    <span data-ttu-id="5b98b-221">a.</span><span class="sxs-lookup"><span data-stu-id="5b98b-221">a.</span></span> <span data-ttu-id="5b98b-222">Typ **uživatelské jméno**, **název**, **e-mailu**, **nové heslo**, **opakujte nové heslo** platný aad účet, který chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="5b98b-222">Type the **Username**, **Name**, **Email**, **New Password**, **Repeat New Password** of a valid AAD account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="5b98b-223">b.</span><span class="sxs-lookup"><span data-stu-id="5b98b-223">b.</span></span> <span data-ttu-id="5b98b-224">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-224">Click **Save**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="5b98b-225">Můžete použít všechny ostatní AppDynamics uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované AppDynamics ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5b98b-225">You can use any other AppDynamics user account creation tools or APIs provided by AppDynamics to provision Azure AD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5b98b-226">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5b98b-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5b98b-227">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="5b98b-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AppDynamics.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5b98b-229">**Pokud chcete přiřadit Britta Simon AppDynamics, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5b98b-229">**To assign Britta Simon to AppDynamics, perform the following steps:**</span></span>

1. <span data-ttu-id="5b98b-230">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5b98b-232">V seznamu aplikací vyberte **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-232">In the applications list, select **AppDynamics**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_app.png) 

3. <span data-ttu-id="5b98b-234">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5b98b-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5b98b-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5b98b-236">Click **Add** button.</span></span> <span data-ttu-id="5b98b-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5b98b-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5b98b-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5b98b-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5b98b-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5b98b-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5b98b-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5b98b-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5b98b-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5b98b-242">Testing single sign-on</span></span>

<span data-ttu-id="5b98b-243">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5b98b-243">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5b98b-244">Když kliknete na dlaždici AppDynamics na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="5b98b-244">When you click the AppDynamics tile in the Access Panel, you should get automatically signed-on to your AppDynamics application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5b98b-245">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5b98b-245">Additional resources</span></span>

* [<span data-ttu-id="5b98b-246">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5b98b-246">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5b98b-247">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5b98b-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_203.png

