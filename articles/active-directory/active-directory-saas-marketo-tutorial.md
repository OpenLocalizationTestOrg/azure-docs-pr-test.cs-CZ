---
title: 'Kurz: Azure Active Directory integrace s Marketo | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Marketo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: e146fd5a8075bc9c7ba049b25e5f301fc2645ed9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a><span data-ttu-id="eb4ee-103">Kurz: Azure Active Directory integrace s Marketo</span><span class="sxs-lookup"><span data-stu-id="eb4ee-103">Tutorial: Azure Active Directory integration with Marketo</span></span>

<span data-ttu-id="eb4ee-104">V tomto kurzu zjistěte, jak integrovat Marketo s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="eb4ee-104">In this tutorial, you learn how to integrate Marketo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eb4ee-105">Integrace služby Marketo s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-105">Integrating Marketo with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="eb4ee-106">Můžete řídit ve službě Azure AD, který má přístup ke službě marketo získáte</span><span class="sxs-lookup"><span data-stu-id="eb4ee-106">You can control in Azure AD who has access to Marketo</span></span>
- <span data-ttu-id="eb4ee-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Marketo (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="eb4ee-107">You can enable your users to automatically get signed-on to Marketo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="eb4ee-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="eb4ee-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="eb4ee-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eb4ee-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb4ee-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eb4ee-110">Prerequisites</span></span>

<span data-ttu-id="eb4ee-111">Konfigurace integrace Azure AD s Marketo, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-111">To configure Azure AD integration with Marketo, you need the following items:</span></span>

- <span data-ttu-id="eb4ee-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="eb4ee-112">An Azure AD subscription</span></span>
- <span data-ttu-id="eb4ee-113">Marketo jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="eb4ee-113">A Marketo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eb4ee-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="eb4ee-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="eb4ee-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="eb4ee-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eb4ee-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eb4ee-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="eb4ee-118">Scenario description</span></span>
<span data-ttu-id="eb4ee-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="eb4ee-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eb4ee-121">Přidání Marketo z Galerie</span><span class="sxs-lookup"><span data-stu-id="eb4ee-121">Adding Marketo from the gallery</span></span>
2. <span data-ttu-id="eb4ee-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="eb4ee-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-marketo-from-the-gallery"></a><span data-ttu-id="eb4ee-123">Přidání Marketo z Galerie</span><span class="sxs-lookup"><span data-stu-id="eb4ee-123">Adding Marketo from the gallery</span></span>
<span data-ttu-id="eb4ee-124">Při konfiguraci integrace služby Marketo do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Marketo z galerie.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-124">To configure the integration of Marketo into Azure AD, you need to add Marketo from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="eb4ee-125">**Pokud chcete přidat Marketo z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eb4ee-125">**To add Marketo from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="eb4ee-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="eb4ee-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="eb4ee-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="eb4ee-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="eb4ee-133">Do vyhledávacího pole zadejte **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-133">In the search box, type **Marketo**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. <span data-ttu-id="eb4ee-135">Na panelu výsledků vyberte **Marketo**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-135">In the results panel, select **Marketo**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="eb4ee-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="eb4ee-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="eb4ee-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Marketo podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="eb4ee-138">In this section, you configure and test Azure AD single sign-on with Marketo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="eb4ee-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Marketo je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Marketo is to a user in Azure AD.</span></span> <span data-ttu-id="eb4ee-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Marketo musí navázat.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-140">In other words, a link relationship between an Azure AD user and the related user in Marketo needs to be established.</span></span>

<span data-ttu-id="eb4ee-141">V Marketo, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-141">In Marketo, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="eb4ee-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Marketo, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-142">To configure and test Azure AD single sign-on with Marketo, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="eb4ee-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="eb4ee-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eb4ee-145">**[Vytvoření zkušebního uživatele Marketo](#creating-a-marketo-test-user)**  – Pokud chcete mít protějšek Britta Simon v Marketo propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-145">**[Creating a Marketo test user](#creating-a-marketo-test-user)** - to have a counterpart of Britta Simon in Marketo that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="eb4ee-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eb4ee-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="eb4ee-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eb4ee-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="eb4ee-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Marketo.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Marketo application.</span></span>

<span data-ttu-id="eb4ee-150">**Ke konfiguraci Azure AD jednotné přihlašování s Marketo, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eb4ee-150">**To configure Azure AD single sign-on with Marketo, perform the following steps:**</span></span>

1. <span data-ttu-id="eb4ee-151">Na portálu Azure na **Marketo** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-151">In the Azure portal, on the **Marketo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="eb4ee-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. <span data-ttu-id="eb4ee-155">Na **Marketo domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-155">On the **Marketo Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    <span data-ttu-id="eb4ee-157">a.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-157">a.</span></span> <span data-ttu-id="eb4ee-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://saml.marketo.com/sp`</span><span class="sxs-lookup"><span data-stu-id="eb4ee-158">In the **Identifier** textbox, type a URL using the following pattern: `https://saml.marketo.com/sp`</span></span>

    <span data-ttu-id="eb4ee-159">b.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-159">b.</span></span> <span data-ttu-id="eb4ee-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://login.marketo.com/saml/assertion/\<munchkinid\>`</span><span class="sxs-lookup"><span data-stu-id="eb4ee-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://login.marketo.com/saml/assertion/\<munchkinid\>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="eb4ee-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-161">These values are not real.</span></span> <span data-ttu-id="eb4ee-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="eb4ee-163">Obraťte se na [tým podpory Marketo](http://investors.marketo.com/contactus.cfm) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-163">Contact [Marketo support team](http://investors.marketo.com/contactus.cfm) to get these values.</span></span>
 
4. <span data-ttu-id="eb4ee-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. <span data-ttu-id="eb4ee-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="eb4ee-168">Na **Marketo konfigurace** klikněte na tlačítko **konfigurace Marketo** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-168">On the **Marketo Configuration** section, click **Configure Marketo** to open **Configure sign-on** window.</span></span> <span data-ttu-id="eb4ee-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="eb4ee-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. <span data-ttu-id="eb4ee-171">Pokud chcete získat Id Munchkin vaší aplikace, přihlaste se ke službě marketo získáte pomocí přihlašovacích údajů správce a proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-171">To get Munchkin Id of your application, log in to Marketo using admin credentials and perform following actions:</span></span>
   
    <span data-ttu-id="eb4ee-172">a.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-172">a.</span></span> <span data-ttu-id="eb4ee-173">Přihlaste se k aplikaci Marketo pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-173">Log in to Marketo app using admin credentials.</span></span>
   
    <span data-ttu-id="eb4ee-174">b.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-174">b.</span></span> <span data-ttu-id="eb4ee-175">Klikněte **správce** tlačítko v horním navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-175">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="eb4ee-177">c.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-177">c.</span></span> <span data-ttu-id="eb4ee-178">Přejděte do nabídky integrace a klikněte na **Munchkin odkaz**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-178">Navigate to the Integration menu and click the **Munchkin link**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    <span data-ttu-id="eb4ee-180">d.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-180">d.</span></span> <span data-ttu-id="eb4ee-181">Zkopírujte Id Munchkin uvedené na obrazovce a dokončete vaše adresa URL odpovědi v Průvodci konfigurací služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-181">Copy the Munchkin Id shown on the screen and complete your Reply URL in the Azure AD configuration wizard.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. <span data-ttu-id="eb4ee-183">Chcete-li nakonfigurovat jednotné přihlašování v aplikaci, postupujte následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-183">To configure the SSO in the application, follow the below steps:</span></span>
   
    <span data-ttu-id="eb4ee-184">a.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-184">a.</span></span> <span data-ttu-id="eb4ee-185">Přihlaste se k aplikaci Marketo pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-185">Log in to Marketo app using admin credentials.</span></span>
   
    <span data-ttu-id="eb4ee-186">b.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-186">b.</span></span> <span data-ttu-id="eb4ee-187">Klikněte **správce** tlačítko v horním navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-187">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="eb4ee-189">c.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-189">c.</span></span> <span data-ttu-id="eb4ee-190">Přejděte do nabídky integrace a klikněte na tlačítko **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-190">Navigate to the Integration menu and click **Single Sign On**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    <span data-ttu-id="eb4ee-192">d.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-192">d.</span></span> <span data-ttu-id="eb4ee-193">Chcete-li povolit SAML nastavení, klikněte na tlačítko **upravit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-193">To enable the SAML Settings, click **Edit** button.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    <span data-ttu-id="eb4ee-195">e.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-195">e.</span></span> <span data-ttu-id="eb4ee-196">**Povolit** nastavení jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-196">**Enabled** Single Sign-On settings.</span></span>
   
    <span data-ttu-id="eb4ee-197">f.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-197">f.</span></span> <span data-ttu-id="eb4ee-198">Vložení **SAML Entity ID**v **ID vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-198">Paste the **SAML Entity ID**, in the **Issuer ID** textbox.</span></span>
   
    <span data-ttu-id="eb4ee-199">g.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-199">g.</span></span> <span data-ttu-id="eb4ee-200">V **Entity ID** textovému poli, zadejte adresu URL jako `http://saml.marketo.com/sp`.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-200">In the **Entity ID** textbox, enter the URL as `http://saml.marketo.com/sp`.</span></span>
   
    <span data-ttu-id="eb4ee-201">h.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-201">h.</span></span> <span data-ttu-id="eb4ee-202">Vyberte umístění ID uživatele jako **název identifikátor elementu**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-202">Select the User ID Location as **Name Identifier element**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > <span data-ttu-id="eb4ee-204">Pokud není uživatelský identifikátor hodnota UPN a změnit hodnotu na kartě atribut.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-204">If your User Identifier is not UPN value then change the value in the Attribute tab.</span></span>
   
    <span data-ttu-id="eb4ee-205">i.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-205">i.</span></span> <span data-ttu-id="eb4ee-206">Nahrajte certifikát, který jste si stáhli z Průvodce konfigurací služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-206">Upload the certificate, which you have downloaded from Azure AD configuration wizard.</span></span> <span data-ttu-id="eb4ee-207">**Uložit** nastavení.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-207">**Save** the settings.</span></span>
   
    <span data-ttu-id="eb4ee-208">j.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-208">j.</span></span> <span data-ttu-id="eb4ee-209">Upravte nastavení přesměrování stránky.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-209">Edit the Redirect Pages settings.</span></span>
   
    <span data-ttu-id="eb4ee-210">kB.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-210">k.</span></span> <span data-ttu-id="eb4ee-211">Vložení **SAML jeden přihlašování adresa URL služby** v **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-211">Paste the **SAML Single Sign-On Service URL** in the **Login URL** textbox.</span></span>
   
    <span data-ttu-id="eb4ee-212">l.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-212">l.</span></span> <span data-ttu-id="eb4ee-213">Vložení **Sign-Out URL** v **adresy URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-213">Paste the **Sign-Out URL** in the **Logout URL** textbox.</span></span>
   
    <span data-ttu-id="eb4ee-214">m.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-214">m.</span></span> <span data-ttu-id="eb4ee-215">V **chyba URL**, kopie vaší **adresu URL instance Marketo** a klikněte na tlačítko **Uložit** tlačítko pro uložení nastavení.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-215">In the **Error URL**, copy your **Marketo instance URL** and click **Save** button to save settings.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. <span data-ttu-id="eb4ee-217">Chcete-li povolit jednotné přihlašování pro uživatele, proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-217">To enable the SSO for users, complete the following actions:</span></span>
   
    <span data-ttu-id="eb4ee-218">a.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-218">a.</span></span> <span data-ttu-id="eb4ee-219">Přihlaste se k aplikaci Marketo pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-219">Log in to Marketo app using admin credentials.</span></span>
   
    <span data-ttu-id="eb4ee-220">b.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-220">b.</span></span> <span data-ttu-id="eb4ee-221">Klikněte **správce** tlačítko v horním navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-221">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="eb4ee-223">c.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-223">c.</span></span> <span data-ttu-id="eb4ee-224">Přejděte na **zabezpečení** nabídky a klikněte na tlačítko **nastavení přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-224">Navigate to the **Security** menu and click **Login Settings**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    <span data-ttu-id="eb4ee-226">d.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-226">d.</span></span> <span data-ttu-id="eb4ee-227">Zkontrolujte **vyžadovat jednotné přihlašování** možnost a **Uložit** nastavení.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-227">Check the **Require SSO** option and **Save** the settings.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> <span data-ttu-id="eb4ee-229">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="eb4ee-229">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="eb4ee-230">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-230">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="eb4ee-231">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="eb4ee-231">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="eb4ee-232">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="eb4ee-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="eb4ee-233">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-233">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="eb4ee-235">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eb4ee-235">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="eb4ee-236">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-236">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="eb4ee-238">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-238">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="eb4ee-240">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-240">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="eb4ee-242">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eb4ee-242">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="eb4ee-244">a.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-244">a.</span></span> <span data-ttu-id="eb4ee-245">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-245">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="eb4ee-246">b.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-246">b.</span></span> <span data-ttu-id="eb4ee-247">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-247">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="eb4ee-248">c.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-248">c.</span></span> <span data-ttu-id="eb4ee-249">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-249">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="eb4ee-250">d.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-250">d.</span></span> <span data-ttu-id="eb4ee-251">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-251">Click **Create**.</span></span>
 
### <a name="creating-a-marketo-test-user"></a><span data-ttu-id="eb4ee-252">Vytvoření zkušebního uživatele Marketo</span><span class="sxs-lookup"><span data-stu-id="eb4ee-252">Creating a Marketo test user</span></span>

<span data-ttu-id="eb4ee-253">V této části vytvoříte uživatele volal Britta Simon v Marketo.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-253">In this section, you create a user called Britta Simon in Marketo.</span></span> <span data-ttu-id="eb4ee-254">postupujte podle těchto kroků můžete vytvořit uživateli v platformě Marketo.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-254">follow these steps to create a user in Marketo platform.</span></span>

1. <span data-ttu-id="eb4ee-255">Přihlaste se k aplikaci Marketo pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-255">Log in to Marketo app using admin credentials.</span></span>

2. <span data-ttu-id="eb4ee-256">Klikněte **správce** tlačítko v horním navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-256">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. <span data-ttu-id="eb4ee-258">Přejděte na **zabezpečení** nabídky a klikněte na tlačítko **uživatelé a role**</span><span class="sxs-lookup"><span data-stu-id="eb4ee-258">Navigate to the **Security** menu and click **Users & Roles**</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. <span data-ttu-id="eb4ee-260">Klikněte **pozvat nového uživatele** odkaz na kartě Uživatelé</span><span class="sxs-lookup"><span data-stu-id="eb4ee-260">Click the **Invite New User** link on the Users tab</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. <span data-ttu-id="eb4ee-262">V Průvodci vytvořením nového uživatele pozvat, zadejte následující informace</span><span class="sxs-lookup"><span data-stu-id="eb4ee-262">In the Invite New User wizard fill the following information</span></span>
   
    <span data-ttu-id="eb4ee-263">a.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-263">a.</span></span> <span data-ttu-id="eb4ee-264">Zadejte uživatele **e-mailu** adresu do textového pole</span><span class="sxs-lookup"><span data-stu-id="eb4ee-264">Enter the user **Email** address in the textbox</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    <span data-ttu-id="eb4ee-266">b.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-266">b.</span></span> <span data-ttu-id="eb4ee-267">Zadejte **křestní jméno** do textového pole</span><span class="sxs-lookup"><span data-stu-id="eb4ee-267">Enter the **First Name** in the textbox</span></span>
   
    <span data-ttu-id="eb4ee-268">c.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-268">c.</span></span> <span data-ttu-id="eb4ee-269">Zadejte **příjmení** do textového pole</span><span class="sxs-lookup"><span data-stu-id="eb4ee-269">Enter the **Last Name**  in the textbox</span></span>
   
    <span data-ttu-id="eb4ee-270">d.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-270">d.</span></span> <span data-ttu-id="eb4ee-271">Klikněte na **Další**</span><span class="sxs-lookup"><span data-stu-id="eb4ee-271">Click **Next**</span></span>

6. <span data-ttu-id="eb4ee-272">V **oprávnění** vyberte **userRoles** a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="eb4ee-272">In the **Permissions** tab, select the **userRoles** and click **Next**</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. <span data-ttu-id="eb4ee-274">Klikněte **odeslat** tlačítko Poslat pozvánku uživatele</span><span class="sxs-lookup"><span data-stu-id="eb4ee-274">Click the **Send** button to send the user invitation</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. <span data-ttu-id="eb4ee-276">Uživatel obdrží e-mailových oznámení a klikněte na odkaz a změnit heslo, aby účet.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-276">User receives the email notification and has to click the link and change the password to activate the account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="eb4ee-277">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="eb4ee-277">Assigning the Azure AD test user</span></span>

<span data-ttu-id="eb4ee-278">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Marketo.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-278">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Marketo.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="eb4ee-280">**Pokud chcete přiřadit Britta Simon Marketo, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eb4ee-280">**To assign Britta Simon to Marketo, perform the following steps:**</span></span>

1. <span data-ttu-id="eb4ee-281">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-281">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="eb4ee-283">V seznamu aplikací vyberte **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-283">In the applications list, select **Marketo**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. <span data-ttu-id="eb4ee-285">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-285">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="eb4ee-287">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-287">Click **Add** button.</span></span> <span data-ttu-id="eb4ee-288">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-288">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="eb4ee-290">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-290">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="eb4ee-291">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-291">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="eb4ee-292">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-292">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="eb4ee-293">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eb4ee-293">Testing single sign-on</span></span>

<span data-ttu-id="eb4ee-294">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-294">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="eb4ee-295">Když kliknete na dlaždici Marketo na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Marketo.</span><span class="sxs-lookup"><span data-stu-id="eb4ee-295">When you click the Marketo tile in the Access Panel, you should get automatically signed-on to your Marketo application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eb4ee-296">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="eb4ee-296">Additional resources</span></span>

* [<span data-ttu-id="eb4ee-297">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eb4ee-297">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eb4ee-298">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="eb4ee-298">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

