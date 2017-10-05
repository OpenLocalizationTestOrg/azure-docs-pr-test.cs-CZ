---
title: 'Kurz: Azure Active Directory integrace s HireVue | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a HireVue."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aadfc342-14db-4d74-a83d-f0c76f0cf63c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 682d88d3010f5781948a9adde0e1351471608a5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hirevue"></a><span data-ttu-id="313d8-103">Kurz: Azure Active Directory integrace s HireVue</span><span class="sxs-lookup"><span data-stu-id="313d8-103">Tutorial: Azure Active Directory integration with HireVue</span></span>

<span data-ttu-id="313d8-104">V tomto kurzu zjistěte, jak integrovat HireVue s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="313d8-104">In this tutorial, you learn how to integrate HireVue with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="313d8-105">Integrace HireVue s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="313d8-105">Integrating HireVue with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="313d8-106">Můžete řídit ve službě Azure AD, který má přístup k HireVue</span><span class="sxs-lookup"><span data-stu-id="313d8-106">You can control in Azure AD who has access to HireVue</span></span>
- <span data-ttu-id="313d8-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k HireVue (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="313d8-107">You can enable your users to automatically get signed-on to HireVue (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="313d8-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="313d8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="313d8-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="313d8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="313d8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="313d8-110">Prerequisites</span></span>

<span data-ttu-id="313d8-111">Konfigurace integrace Azure AD s HireVue, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="313d8-111">To configure Azure AD integration with HireVue, you need the following items:</span></span>

- <span data-ttu-id="313d8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="313d8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="313d8-113">HireVue jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="313d8-113">A HireVue single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="313d8-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="313d8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="313d8-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="313d8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="313d8-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="313d8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="313d8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="313d8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="313d8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="313d8-118">Scenario description</span></span>
<span data-ttu-id="313d8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="313d8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="313d8-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="313d8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="313d8-121">Přidání HireVue z Galerie</span><span class="sxs-lookup"><span data-stu-id="313d8-121">Adding HireVue from the gallery</span></span>
2. <span data-ttu-id="313d8-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="313d8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hirevue-from-the-gallery"></a><span data-ttu-id="313d8-123">Přidání HireVue z Galerie</span><span class="sxs-lookup"><span data-stu-id="313d8-123">Adding HireVue from the gallery</span></span>
<span data-ttu-id="313d8-124">Při konfiguraci integrace HireVue do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS HireVue z galerie.</span><span class="sxs-lookup"><span data-stu-id="313d8-124">To configure the integration of HireVue into Azure AD, you need to add HireVue from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="313d8-125">**Pokud chcete přidat HireVue z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="313d8-125">**To add HireVue from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="313d8-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="313d8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="313d8-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="313d8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="313d8-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="313d8-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="313d8-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="313d8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="313d8-133">Do vyhledávacího pole zadejte **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="313d8-133">In the search box, type **HireVue**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_search.png)

5. <span data-ttu-id="313d8-135">Na panelu výsledků vyberte **HireVue**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="313d8-135">In the results panel, select **HireVue**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="313d8-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="313d8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="313d8-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s HireVue podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="313d8-138">In this section, you configure and test Azure AD single sign-on with HireVue based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="313d8-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v HireVue je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="313d8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HireVue is to a user in Azure AD.</span></span> <span data-ttu-id="313d8-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v HireVue musí navázat.</span><span class="sxs-lookup"><span data-stu-id="313d8-140">In other words, a link relationship between an Azure AD user and the related user in HireVue needs to be established.</span></span>

<span data-ttu-id="313d8-141">V HireVue, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="313d8-141">In HireVue, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="313d8-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s HireVue, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="313d8-142">To configure and test Azure AD single sign-on with HireVue, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="313d8-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="313d8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="313d8-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="313d8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="313d8-145">**[Vytvoření zkušebního uživatele HireVue](#creating-a-hirevue-test-user)**  – Pokud chcete mít protějšek Britta Simon v HireVue propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="313d8-145">**[Creating a HireVue test user](#creating-a-hirevue-test-user)** - to have a counterpart of Britta Simon in HireVue that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="313d8-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="313d8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="313d8-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="313d8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="313d8-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="313d8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="313d8-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci HireVue.</span><span class="sxs-lookup"><span data-stu-id="313d8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HireVue application.</span></span>

<span data-ttu-id="313d8-150">**Ke konfiguraci Azure AD jednotné přihlašování s HireVue, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="313d8-150">**To configure Azure AD single sign-on with HireVue, perform the following steps:**</span></span>

1. <span data-ttu-id="313d8-151">Na portálu Azure na **HireVue** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="313d8-151">In the Azure portal, on the **HireVue** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="313d8-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="313d8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_samlbase.png)

3. <span data-ttu-id="313d8-155">Na **HireVue domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="313d8-155">On the **HireVue Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_url.png)

    <span data-ttu-id="313d8-157">a.</span><span class="sxs-lookup"><span data-stu-id="313d8-157">a.</span></span> <span data-ttu-id="313d8-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="313d8-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>

    | <span data-ttu-id="313d8-159">Prostředí</span><span class="sxs-lookup"><span data-stu-id="313d8-159">Environment</span></span> | <span data-ttu-id="313d8-160">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="313d8-160">URL</span></span> |
    |-------------|---|
    | <span data-ttu-id="313d8-161">Produkční</span><span class="sxs-lookup"><span data-stu-id="313d8-161">Production</span></span> | `https://<companyname>.hirevue.com` |
    | <span data-ttu-id="313d8-162">Pracovní</span><span class="sxs-lookup"><span data-stu-id="313d8-162">Staging</span></span>    | `https://<companyname>.stghv.com` |
    
    <span data-ttu-id="313d8-163">b.</span><span class="sxs-lookup"><span data-stu-id="313d8-163">b.</span></span> <span data-ttu-id="313d8-164">V **identifikátor** textovému poli, zadejte adresu URL jako:</span><span class="sxs-lookup"><span data-stu-id="313d8-164">In the **Identifier** textbox, type a URL as:</span></span>
    
    | <span data-ttu-id="313d8-165">Prostředí</span><span class="sxs-lookup"><span data-stu-id="313d8-165">Environment</span></span> | <span data-ttu-id="313d8-166">NÁZEV URN</span><span class="sxs-lookup"><span data-stu-id="313d8-166">URN</span></span> |
    |-------------|-----|
    | <span data-ttu-id="313d8-167">Produkční</span><span class="sxs-lookup"><span data-stu-id="313d8-167">Production</span></span> |`urn:federation:hirevue.com:saml:sp:prod` |
    | <span data-ttu-id="313d8-168">Pracovní</span><span class="sxs-lookup"><span data-stu-id="313d8-168">Staging</span></span>    | `urn:federation:hirevue.com:saml:sp:staging`|
    
    > [!NOTE] 
    > <span data-ttu-id="313d8-169">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="313d8-169">These values are not real.</span></span> <span data-ttu-id="313d8-170">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="313d8-170">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="313d8-171">Obraťte se na [tým podpory HireVue klienta](mailto:samlsupport@hirevue.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="313d8-171">Contact [HireVue Client support team](mailto:samlsupport@hirevue.com) to get these values.</span></span> 
 
4. <span data-ttu-id="313d8-172">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="313d8-172">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_certificate.png) 

5. <span data-ttu-id="313d8-174">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="313d8-174">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="313d8-176">Na **HireVue konfigurace** klikněte na tlačítko **konfigurace HireVue** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="313d8-176">On the **HireVue Configuration** section, click **Configure HireVue** to open **Configure sign-on** window.</span></span> <span data-ttu-id="313d8-177">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="313d8-177">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_configure.png) 

7. <span data-ttu-id="313d8-179">Konfigurace jednotného přihlašování na **HireVue** straně, budete muset odeslat stažené **Certificate(Base64)** a **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory HireVue](mailto:samlsupport@hirevue.com).</span><span class="sxs-lookup"><span data-stu-id="313d8-179">To configure single sign-on on **HireVue** side, you need to send the downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [HireVue support team](mailto:samlsupport@hirevue.com).</span></span> <span data-ttu-id="313d8-180">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="313d8-180">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="313d8-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="313d8-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="313d8-182">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="313d8-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="313d8-183">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="313d8-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="313d8-184">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="313d8-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="313d8-185">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="313d8-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="313d8-187">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="313d8-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="313d8-188">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="313d8-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="313d8-190">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="313d8-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="313d8-192">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="313d8-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="313d8-194">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="313d8-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="313d8-196">a.</span><span class="sxs-lookup"><span data-stu-id="313d8-196">a.</span></span> <span data-ttu-id="313d8-197">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="313d8-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="313d8-198">b.</span><span class="sxs-lookup"><span data-stu-id="313d8-198">b.</span></span> <span data-ttu-id="313d8-199">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="313d8-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="313d8-200">c.</span><span class="sxs-lookup"><span data-stu-id="313d8-200">c.</span></span> <span data-ttu-id="313d8-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="313d8-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="313d8-202">d.</span><span class="sxs-lookup"><span data-stu-id="313d8-202">d.</span></span> <span data-ttu-id="313d8-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="313d8-203">Click **Create**.</span></span>
 
### <a name="creating-a-hirevue-test-user"></a><span data-ttu-id="313d8-204">Vytvoření zkušebního uživatele HireVue</span><span class="sxs-lookup"><span data-stu-id="313d8-204">Creating a HireVue test user</span></span>

<span data-ttu-id="313d8-205">V této části vytvoříte volal Britta Simon v HireVue uživatele.</span><span class="sxs-lookup"><span data-stu-id="313d8-205">In this section, you create a user called Britta Simon in HireVue.</span></span> <span data-ttu-id="313d8-206">Spojte se s [tým podpory HireVue](mailto:samlsupport@hirevue.com) přidat uživatele do HireVue platformy.</span><span class="sxs-lookup"><span data-stu-id="313d8-206">Please work with [HireVue support team](mailto:samlsupport@hirevue.com) to add the users in the HireVue platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="313d8-207">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="313d8-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="313d8-208">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu HireVue.</span><span class="sxs-lookup"><span data-stu-id="313d8-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HireVue.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="313d8-210">**Pokud chcete přiřadit Britta Simon HireVue, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="313d8-210">**To assign Britta Simon to HireVue, perform the following steps:**</span></span>

1. <span data-ttu-id="313d8-211">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="313d8-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="313d8-213">V seznamu aplikací vyberte **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="313d8-213">In the applications list, select **HireVue**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_app.png) 

3. <span data-ttu-id="313d8-215">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="313d8-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="313d8-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="313d8-217">Click **Add** button.</span></span> <span data-ttu-id="313d8-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="313d8-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="313d8-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="313d8-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="313d8-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="313d8-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="313d8-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="313d8-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="313d8-223">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="313d8-223">Testing single sign-on</span></span>

<span data-ttu-id="313d8-224">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="313d8-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="313d8-225">Když kliknete na dlaždici HireVue na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci HireVue.</span><span class="sxs-lookup"><span data-stu-id="313d8-225">When you click the HireVue tile in the Access Panel, you should get automatically signed-on to your HireVue application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="313d8-226">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="313d8-226">Additional resources</span></span>

* [<span data-ttu-id="313d8-227">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="313d8-227">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="313d8-228">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="313d8-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_203.png

