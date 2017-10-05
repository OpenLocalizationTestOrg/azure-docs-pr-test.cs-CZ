---
title: 'Kurz: Azure Active Directory integrace s Jitbit Helpdesk | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Jitbit technickou podporu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 6523ee3179dafd79528093b856b0ec10fafb4f7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a><span data-ttu-id="514dc-103">Kurz: Azure Active Directory integrace s Jitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="514dc-103">Tutorial: Azure Active Directory integration with Jitbit Helpdesk</span></span>

<span data-ttu-id="514dc-104">V tomto kurzu zjistěte, jak integrovat Jitbit pracovník s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="514dc-104">In this tutorial, you learn how to integrate Jitbit Helpdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="514dc-105">Integrace Jitbit pracovník s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="514dc-105">Integrating Jitbit Helpdesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="514dc-106">Můžete řídit ve službě Azure AD, který má přístup na technickou podporu Jitbit</span><span class="sxs-lookup"><span data-stu-id="514dc-106">You can control in Azure AD who has access to Jitbit Helpdesk</span></span>
- <span data-ttu-id="514dc-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného na technickou podporu Jitbit (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="514dc-107">You can enable your users to automatically get signed-on to Jitbit Helpdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="514dc-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="514dc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="514dc-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="514dc-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="514dc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="514dc-110">Prerequisites</span></span>

<span data-ttu-id="514dc-111">Konfigurace integrace Azure AD s Jitbit Helpdesk, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="514dc-111">To configure Azure AD integration with Jitbit Helpdesk, you need the following items:</span></span>

- <span data-ttu-id="514dc-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="514dc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="514dc-113">Jitbit Helpdesk jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="514dc-113">A Jitbit Helpdesk single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="514dc-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="514dc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="514dc-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="514dc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="514dc-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="514dc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="514dc-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="514dc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="514dc-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="514dc-118">Scenario description</span></span>
<span data-ttu-id="514dc-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="514dc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="514dc-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="514dc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="514dc-121">Přidání Jitbit Helpdesk z Galerie</span><span class="sxs-lookup"><span data-stu-id="514dc-121">Adding Jitbit Helpdesk from the gallery</span></span>
2. <span data-ttu-id="514dc-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="514dc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a><span data-ttu-id="514dc-123">Přidání Jitbit Helpdesk z Galerie</span><span class="sxs-lookup"><span data-stu-id="514dc-123">Adding Jitbit Helpdesk from the gallery</span></span>
<span data-ttu-id="514dc-124">Při konfiguraci integrace Jitbit Helpdesk do služby Azure AD potřebujete přidat Jitbit Helpdesk z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="514dc-124">To configure the integration of Jitbit Helpdesk into Azure AD, you need to add Jitbit Helpdesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="514dc-125">**Chcete-li přidat Jitbit Helpdesk z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="514dc-125">**To add Jitbit Helpdesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="514dc-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="514dc-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="514dc-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="514dc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="514dc-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="514dc-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="514dc-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="514dc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="514dc-133">Do vyhledávacího pole zadejte **Jitbit Helpdesk**.</span><span class="sxs-lookup"><span data-stu-id="514dc-133">In the search box, type **Jitbit Helpdesk**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. <span data-ttu-id="514dc-135">Na panelu výsledků vyberte **Jitbit Helpdesk**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="514dc-135">In the results panel, select **Jitbit Helpdesk**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="514dc-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="514dc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="514dc-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Jitbit Helpdesk podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="514dc-138">In this section, you configure and test Azure AD single sign-on with Jitbit Helpdesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="514dc-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Jitbit Helpdesk je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="514dc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jitbit Helpdesk is to a user in Azure AD.</span></span> <span data-ttu-id="514dc-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Jitbit Helpdesk je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="514dc-140">In other words, a link relationship between an Azure AD user and the related user in Jitbit Helpdesk needs to be established.</span></span>

<span data-ttu-id="514dc-141">V Jitbit Helpdesk přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="514dc-141">In Jitbit Helpdesk, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="514dc-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jitbit Helpdesk, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="514dc-142">To configure and test Azure AD single sign-on with Jitbit Helpdesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="514dc-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="514dc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="514dc-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="514dc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="514dc-145">**[Vytvoření zkušebního uživatele Jitbit Helpdesk](#creating-a-jitbit-helpdesk-test-user)**  – Pokud chcete mít protějšek Britta Simon v Jitbit Helpdesk, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="514dc-145">**[Creating a Jitbit Helpdesk test user](#creating-a-jitbit-helpdesk-test-user)** - to have a counterpart of Britta Simon in Jitbit Helpdesk that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="514dc-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="514dc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="514dc-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="514dc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="514dc-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="514dc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="514dc-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Jitbit technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="514dc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jitbit Helpdesk application.</span></span>

<span data-ttu-id="514dc-150">**Ke konfiguraci Azure AD jednotné přihlašování s Jitbit Helpdesk, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="514dc-150">**To configure Azure AD single sign-on with Jitbit Helpdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="514dc-151">Na portálu Azure na **Jitbit Helpdesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="514dc-151">In the Azure portal, on the **Jitbit Helpdesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="514dc-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="514dc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. <span data-ttu-id="514dc-155">Na **Jitbit Helpdesk domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="514dc-155">On the **Jitbit Helpdesk Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    <span data-ttu-id="514dc-157">a.</span><span class="sxs-lookup"><span data-stu-id="514dc-157">a.</span></span> <span data-ttu-id="514dc-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="514dc-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > <span data-ttu-id="514dc-159">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="514dc-159">This value is not real.</span></span> <span data-ttu-id="514dc-160">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="514dc-160">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="514dc-161">Obraťte se na [tým podpory Jitbit Helpdesk klienta](https://www.jitbit.com/support/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="514dc-161">Contact [Jitbit Helpdesk Client support team](https://www.jitbit.com/support/) to get this value.</span></span> 
    
    <span data-ttu-id="514dc-162">b.</span><span class="sxs-lookup"><span data-stu-id="514dc-162">b.</span></span>  <span data-ttu-id="514dc-163">V **identifikátor** textovému poli, zadejte adresu URL jako následující:`https://www.jitbit.com/web-helpdesk/`</span><span class="sxs-lookup"><span data-stu-id="514dc-163">In the **Identifier** textbox, type a URL as following: `https://www.jitbit.com/web-helpdesk/`</span></span>

    
 


4. <span data-ttu-id="514dc-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="514dc-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. <span data-ttu-id="514dc-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="514dc-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="514dc-168">Na **Jitbit Helpdesk konfigurace** klikněte na tlačítko **konfigurace Jitbit Helpdesk** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="514dc-168">On the **Jitbit Helpdesk Configuration** section, click **Configure Jitbit Helpdesk** to open **Configure sign-on** window.</span></span> <span data-ttu-id="514dc-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="514dc-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. <span data-ttu-id="514dc-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Jitbit Helpdesk.</span><span class="sxs-lookup"><span data-stu-id="514dc-171">In a different web browser window, log into your Jitbit Helpdesk company site as an administrator.</span></span>

8. <span data-ttu-id="514dc-172">Na panelu nástrojů v horní části klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="514dc-172">In the toolbar on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="514dc-173">![Správa](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "správy")</span><span class="sxs-lookup"><span data-stu-id="514dc-173">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

9. <span data-ttu-id="514dc-174">Klikněte na tlačítko **obecné nastavení**.</span><span class="sxs-lookup"><span data-stu-id="514dc-174">Click **General settings**.</span></span>
   
    <span data-ttu-id="514dc-175">![Uživatelé, společností a oprávnění](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "uživatelů, společností a oprávnění")</span><span class="sxs-lookup"><span data-stu-id="514dc-175">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "Users, companies, and permissions")</span></span>

10. <span data-ttu-id="514dc-176">V **nastavení ověřování** konfigurace části, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="514dc-176">In the **Authentication settings** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="514dc-177">![Nastavení ověřování](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "nastavení ověřování")</span><span class="sxs-lookup"><span data-stu-id="514dc-177">![Authentication settings](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "Authentication settings")</span></span>
    
    <span data-ttu-id="514dc-178">a.</span><span class="sxs-lookup"><span data-stu-id="514dc-178">a.</span></span> <span data-ttu-id="514dc-179">Vyberte **povolit SAML 2.0 jeden přihlásit**, se přihlásit pomocí jednotné přihlašování (SSO), s **OneLogin**.</span><span class="sxs-lookup"><span data-stu-id="514dc-179">Select **Enable SAML 2.0 single sign on**, to sign in using Single Sign-On (SSO), with **OneLogin**.</span></span>

    <span data-ttu-id="514dc-180">b.</span><span class="sxs-lookup"><span data-stu-id="514dc-180">b.</span></span> <span data-ttu-id="514dc-181">V **adresu URL koncového bodu** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="514dc-181">In the **EndPoint URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="514dc-182">c.</span><span class="sxs-lookup"><span data-stu-id="514dc-182">c.</span></span> <span data-ttu-id="514dc-183">Otevřete váš **kódování base-64** kódování certifikátu v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **certifikát X.509** textové pole</span><span class="sxs-lookup"><span data-stu-id="514dc-183">Open your **base-64** encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox</span></span>

    <span data-ttu-id="514dc-184">d.</span><span class="sxs-lookup"><span data-stu-id="514dc-184">d.</span></span> <span data-ttu-id="514dc-185">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="514dc-185">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="514dc-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="514dc-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="514dc-187">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="514dc-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="514dc-188">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="514dc-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="514dc-189">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="514dc-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="514dc-190">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="514dc-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="514dc-192">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="514dc-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="514dc-193">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="514dc-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="514dc-195">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="514dc-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="514dc-197">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="514dc-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="514dc-199">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="514dc-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="514dc-201">a.</span><span class="sxs-lookup"><span data-stu-id="514dc-201">a.</span></span> <span data-ttu-id="514dc-202">V **název** textovému poli, název typu jako **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="514dc-202">In the **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="514dc-203">b.</span><span class="sxs-lookup"><span data-stu-id="514dc-203">b.</span></span> <span data-ttu-id="514dc-204">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="514dc-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="514dc-205">c.</span><span class="sxs-lookup"><span data-stu-id="514dc-205">c.</span></span> <span data-ttu-id="514dc-206">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="514dc-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="514dc-207">d.</span><span class="sxs-lookup"><span data-stu-id="514dc-207">d.</span></span> <span data-ttu-id="514dc-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="514dc-208">Click **Create**.</span></span>
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a><span data-ttu-id="514dc-209">Vytvoření zkušebního uživatele Jitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="514dc-209">Creating a Jitbit Helpdesk test user</span></span>

<span data-ttu-id="514dc-210">Pokud chcete povolit uživatelům Azure AD přihlášení do Jitbit Helpdesk, musí být zřízená do Jitbit technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="514dc-210">In order to enable Azure AD users to log into Jitbit Helpdesk, they must be provisioned into Jitbit Helpdesk.</span></span>  <span data-ttu-id="514dc-211">V případě Jitbit Helpdesk zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="514dc-211">In the case of Jitbit Helpdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="514dc-212">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="514dc-212">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="514dc-213">Přihlaste se k vaší **Jitbit Helpdesk** klienta.</span><span class="sxs-lookup"><span data-stu-id="514dc-213">Log in to your **Jitbit Helpdesk** tenant.</span></span>

2. <span data-ttu-id="514dc-214">V nabídce v horní části, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="514dc-214">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="514dc-215">![Správa](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "správy")</span><span class="sxs-lookup"><span data-stu-id="514dc-215">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

3. <span data-ttu-id="514dc-216">Klikněte na tlačítko **uživatelů, společností a oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="514dc-216">Click **Users, companies and permissions**.</span></span>
   
    <span data-ttu-id="514dc-217">![Uživatelé, společností a oprávnění](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "uživatelů, společností a oprávnění")</span><span class="sxs-lookup"><span data-stu-id="514dc-217">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "Users, companies, and permissions")</span></span>

4. <span data-ttu-id="514dc-218">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="514dc-218">Click **Add user**.</span></span>
   
    <span data-ttu-id="514dc-219">![Přidat uživatele](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="514dc-219">![Add user](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Add user")</span></span>
   
5. <span data-ttu-id="514dc-220">V části Vytvoření typů data účtu Azure AD, které chcete zřídit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="514dc-220">In the Create section, type the data of the Azure AD account you want to provision as follows:</span></span>

    <span data-ttu-id="514dc-221">![Vytvoření](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="514dc-221">![Create](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "Create")</span></span>
   
   <span data-ttu-id="514dc-222">a.</span><span class="sxs-lookup"><span data-stu-id="514dc-222">a.</span></span> <span data-ttu-id="514dc-223">V **uživatelské jméno** textovému poli, typ **BrittaSimon**, uživatelské jméno jako v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="514dc-223">In the **Username** textbox, type **BrittaSimon**, the user name as in the Azure portal.</span></span>

   <span data-ttu-id="514dc-224">b.</span><span class="sxs-lookup"><span data-stu-id="514dc-224">b.</span></span> <span data-ttu-id="514dc-225">V **e-mailu** jako typ e-mail uživatele k textovému poli,  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="514dc-225">In the **Email** textbox, type email of the user like **BrittaSimon@contoso.com**.</span></span>

   <span data-ttu-id="514dc-226">c.</span><span class="sxs-lookup"><span data-stu-id="514dc-226">c.</span></span> <span data-ttu-id="514dc-227">V **křestní jméno** textovému poli, zadejte jméno uživatele, jako je **Britta**.</span><span class="sxs-lookup"><span data-stu-id="514dc-227">In the **First Name** textbox, type first name of the user like **Britta**.</span></span>

   <span data-ttu-id="514dc-228">d.</span><span class="sxs-lookup"><span data-stu-id="514dc-228">d.</span></span> <span data-ttu-id="514dc-229">V **příjmení** textovému poli, zadejte příjmení uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="514dc-229">In the **Last Name** textbox, type last name of the user like **Simon**.</span></span>
   
   <span data-ttu-id="514dc-230">e.</span><span class="sxs-lookup"><span data-stu-id="514dc-230">e.</span></span> <span data-ttu-id="514dc-231">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="514dc-231">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="514dc-232">Další nástroje pro tvorbu účet uživatele Jitbit Helpdesk nebo rozhraní API poskytovaných Jitbit Helpdesk můžete použít ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="514dc-232">You can use any other Jitbit Helpdesk user account creation tools or APIs provided by Jitbit Helpdesk to provision Azure AD user accounts.</span></span>
> 
        

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="514dc-233">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="514dc-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="514dc-234">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu na Jitbit technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="514dc-234">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jitbit Helpdesk.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="514dc-236">**Přiřadit Britta Simon na technickou podporu Jitbit, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="514dc-236">**To assign Britta Simon to Jitbit Helpdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="514dc-237">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="514dc-237">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="514dc-239">V seznamu aplikací vyberte **Jitbit Helpdesk**.</span><span class="sxs-lookup"><span data-stu-id="514dc-239">In the applications list, select **Jitbit Helpdesk**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. <span data-ttu-id="514dc-241">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="514dc-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="514dc-243">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="514dc-243">Click **Add** button.</span></span> <span data-ttu-id="514dc-244">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="514dc-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="514dc-246">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="514dc-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="514dc-247">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="514dc-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="514dc-248">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="514dc-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="514dc-249">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="514dc-249">Testing single sign-on</span></span>

<span data-ttu-id="514dc-250">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="514dc-250">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="514dc-251">Když kliknete na dlaždici Jitbit technickou podporu na přístupovém panelu, měli byste obdržet přihlašovací stránku aplikace Jitbit technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="514dc-251">When you click the Jitbit Helpdesk tile in the Access Panel, you should get login page of Jitbit Helpdesk application.</span></span>
<span data-ttu-id="514dc-252">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="514dc-252">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="514dc-253">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="514dc-253">Additional resources</span></span>

* [<span data-ttu-id="514dc-254">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="514dc-254">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="514dc-255">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="514dc-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_203.png

