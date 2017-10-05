---
title: "Kurz: Azure Active Directory integrace s ZABEZPEČENÉ DORUČOVÁNÍ | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ZABEZPEČENÉ DORUČOVÁNÍ."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fccd5668-fe6f-4e6d-a9ce-ba4f321c33d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: f6e5b1e34893f6b8fe14e238e24086bb47d009a5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a><span data-ttu-id="64ef2-103">Kurz: Azure Active Directory integrace s ZABEZPEČENÉ DORUČOVÁNÍ</span><span class="sxs-lookup"><span data-stu-id="64ef2-103">Tutorial: Azure Active Directory integration with SECURE DELIVER</span></span>

<span data-ttu-id="64ef2-104">V tomto kurzu zjistěte, jak integrovat ZABEZPEČENÉ DORUČOVÁNÍ s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="64ef2-104">In this tutorial, you learn how to integrate SECURE DELIVER with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="64ef2-105">Integrace ZABEZPEČENÉ DORUČOVÁNÍ s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="64ef2-105">Integrating SECURE DELIVER with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="64ef2-106">Můžete řídit ve službě Azure AD, který má přístup do ZABEZPEČENÉ DORUČOVÁNÍ</span><span class="sxs-lookup"><span data-stu-id="64ef2-106">You can control in Azure AD who has access to SECURE DELIVER</span></span>
- <span data-ttu-id="64ef2-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ZABEZPEČENÉ DORUČOVÁNÍ (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="64ef2-107">You can enable your users to automatically get signed-on to SECURE DELIVER (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="64ef2-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="64ef2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="64ef2-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="64ef2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64ef2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="64ef2-110">Prerequisites</span></span>

<span data-ttu-id="64ef2-111">Konfigurace integrace Azure AD s ZABEZPEČENÉ DORUČOVÁNÍ, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="64ef2-111">To configure Azure AD integration with SECURE DELIVER, you need the following items:</span></span>

- <span data-ttu-id="64ef2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="64ef2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="64ef2-113">ZABEZPEČENÍ poskytovat jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="64ef2-113">A SECURE DELIVER single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="64ef2-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="64ef2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="64ef2-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="64ef2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="64ef2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="64ef2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="64ef2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="64ef2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="64ef2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="64ef2-118">Scenario description</span></span>
<span data-ttu-id="64ef2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="64ef2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="64ef2-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="64ef2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="64ef2-121">Přidání ZABEZPEČENÉ DORUČOVÁNÍ z Galerie</span><span class="sxs-lookup"><span data-stu-id="64ef2-121">Adding SECURE DELIVER from the gallery</span></span>
2. <span data-ttu-id="64ef2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="64ef2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-secure-deliver-from-the-gallery"></a><span data-ttu-id="64ef2-123">Přidání ZABEZPEČENÉ DORUČOVÁNÍ z Galerie</span><span class="sxs-lookup"><span data-stu-id="64ef2-123">Adding SECURE DELIVER from the gallery</span></span>
<span data-ttu-id="64ef2-124">Chcete-li nakonfigurovat integraci zabezpečení DORUČIT do služby Azure AD, přidejte ZABEZPEČENÉ DORUČOVÁNÍ z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="64ef2-124">To configure the integration of SECURE DELIVER into Azure AD, you need to add SECURE DELIVER from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="64ef2-125">**Pokud chcete přidat ZABEZPEČENÉ DORUČOVÁNÍ z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="64ef2-125">**To add SECURE DELIVER from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="64ef2-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="64ef2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="64ef2-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="64ef2-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="64ef2-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64ef2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="64ef2-133">Do vyhledávacího pole zadejte **ZABEZPEČENÉ DORUČOVÁNÍ**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-133">In the search box, type **SECURE DELIVER**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_search.png)

5. <span data-ttu-id="64ef2-135">Na panelu výsledků vyberte **ZABEZPEČENÉ DORUČOVÁNÍ**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="64ef2-135">In the results panel, select **SECURE DELIVER**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="64ef2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="64ef2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="64ef2-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ZABEZPEČENÉ DORUČOVÁNÍ podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="64ef2-138">In this section, you configure and test Azure AD single sign-on with SECURE DELIVER based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="64ef2-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ZABEZPEČENÉ DORUČOVÁNÍ je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64ef2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SECURE DELIVER is to a user in Azure AD.</span></span> <span data-ttu-id="64ef2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ZABEZPEČENÉ DORUČOVÁNÍ musí navázat.</span><span class="sxs-lookup"><span data-stu-id="64ef2-140">In other words, a link relationship between an Azure AD user and the related user in SECURE DELIVER needs to be established.</span></span>

<span data-ttu-id="64ef2-141">V ZABEZPEČENÉ DORUČOVÁNÍ, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="64ef2-141">In SECURE DELIVER, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="64ef2-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ZABEZPEČENÉ DORUČOVÁNÍ, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="64ef2-142">To configure and test Azure AD single sign-on with SECURE DELIVER, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="64ef2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="64ef2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="64ef2-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64ef2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="64ef2-145">**[Vytvoření zkušebního uživatele ZABEZPEČENÉ DORUČOVÁNÍ](#creating-a-secure-deliver-test-user)**  – Pokud chcete mít protějšek Britta Simon v ZABEZPEČENÉ DORUČOVÁNÍ propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="64ef2-145">**[Creating a SECURE DELIVER test user](#creating-a-secure-deliver-test-user)** - to have a counterpart of Britta Simon in SECURE DELIVER that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="64ef2-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="64ef2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="64ef2-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="64ef2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="64ef2-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="64ef2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="64ef2-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ZABEZPEČENÉ DORUČOVÁNÍ.</span><span class="sxs-lookup"><span data-stu-id="64ef2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SECURE DELIVER application.</span></span>

<span data-ttu-id="64ef2-150">**Ke konfiguraci Azure AD jednotné přihlašování s ZABEZPEČENÉ DORUČOVÁNÍ, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="64ef2-150">**To configure Azure AD single sign-on with SECURE DELIVER, perform the following steps:**</span></span>

1. <span data-ttu-id="64ef2-151">Na portálu Azure na **ZABEZPEČENÉ DORUČOVÁNÍ** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-151">In the Azure portal, on the **SECURE DELIVER** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="64ef2-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="64ef2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_samlbase.png)

3. <span data-ttu-id="64ef2-155">Na **ZABEZPEČENÉ DORUČOVÁNÍ domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64ef2-155">On the **SECURE DELIVER Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_url.png)

    <span data-ttu-id="64ef2-157">a.</span><span class="sxs-lookup"><span data-stu-id="64ef2-157">a.</span></span> <span data-ttu-id="64ef2-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span><span class="sxs-lookup"><span data-stu-id="64ef2-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span></span>

    <span data-ttu-id="64ef2-159">b.</span><span class="sxs-lookup"><span data-stu-id="64ef2-159">b.</span></span> <span data-ttu-id="64ef2-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span><span class="sxs-lookup"><span data-stu-id="64ef2-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="64ef2-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="64ef2-161">These values are not real.</span></span> <span data-ttu-id="64ef2-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="64ef2-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="64ef2-163">Obraťte se na [tým podpory zabezpečení klienta poskytovat](mailto:iw-sd-support@fujifilm.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="64ef2-163">Contact [SECURE DELIVER Client support team](mailto:iw-sd-support@fujifilm.com) to get these values.</span></span> 
 
4. <span data-ttu-id="64ef2-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="64ef2-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_certificate.png) 

5. <span data-ttu-id="64ef2-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="64ef2-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="64ef2-168">Na **ZABEZPEČENÉ DORUČOVÁNÍ konfigurace** klikněte na tlačítko **nakonfigurovat ZABEZPEČENÉ DORUČOVÁNÍ** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="64ef2-168">On the **SECURE DELIVER Configuration** section, click **Configure SECURE DELIVER** to open **Configure sign-on** window.</span></span> <span data-ttu-id="64ef2-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="64ef2-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_configure.png) 

7. <span data-ttu-id="64ef2-171">Konfigurace jednotného přihlašování na **ZABEZPEČENÉ DORUČOVÁNÍ** straně, budete muset odeslat stažené **certifikátu (Base64)**, **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [ZABEZPEČENÉ DORUČOVÁNÍ tým podpory](mailto:iw-sd-support@fujifilm.com).</span><span class="sxs-lookup"><span data-stu-id="64ef2-171">To configure single sign-on on **SECURE DELIVER** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com).</span></span> <span data-ttu-id="64ef2-172">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="64ef2-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="64ef2-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="64ef2-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="64ef2-174">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="64ef2-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="64ef2-175">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="64ef2-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="64ef2-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="64ef2-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="64ef2-177">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64ef2-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="64ef2-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="64ef2-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="64ef2-180">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="64ef2-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="64ef2-182">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="64ef2-184">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64ef2-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="64ef2-186">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64ef2-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="64ef2-188">a.</span><span class="sxs-lookup"><span data-stu-id="64ef2-188">a.</span></span> <span data-ttu-id="64ef2-189">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="64ef2-190">b.</span><span class="sxs-lookup"><span data-stu-id="64ef2-190">b.</span></span> <span data-ttu-id="64ef2-191">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="64ef2-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="64ef2-192">c.</span><span class="sxs-lookup"><span data-stu-id="64ef2-192">c.</span></span> <span data-ttu-id="64ef2-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="64ef2-194">d.</span><span class="sxs-lookup"><span data-stu-id="64ef2-194">d.</span></span> <span data-ttu-id="64ef2-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-195">Click **Create**.</span></span>
 
### <a name="creating-a-secure-deliver-test-user"></a><span data-ttu-id="64ef2-196">Vytvoření zkušebního uživatele ZABEZPEČENÉ DORUČOVÁNÍ</span><span class="sxs-lookup"><span data-stu-id="64ef2-196">Creating a SECURE DELIVER test user</span></span>

<span data-ttu-id="64ef2-197">Cílem této části je vytvoření uživatele volal Britta Simon v ZABEZPEČENÉ DORUČOVÁNÍ.</span><span class="sxs-lookup"><span data-stu-id="64ef2-197">The objective of this section is to create a user called Britta Simon in SECURE DELIVER.</span></span> <span data-ttu-id="64ef2-198">Práce s [ZABEZPEČENÉ DORUČOVÁNÍ tým podpory](mailto:iw-sd-support@fujifilm.com) přidat uživatele do účtu ZABEZPEČENÉ DORUČOVÁNÍ.</span><span class="sxs-lookup"><span data-stu-id="64ef2-198">Work with [SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com) to add the users in the SECURE DELIVER account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="64ef2-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="64ef2-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="64ef2-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k poskytování zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="64ef2-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SECURE DELIVER.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="64ef2-202">**Pokud chcete přiřadit Britta Simon ZABEZPEČENÉ DORUČOVÁNÍ, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="64ef2-202">**To assign Britta Simon to SECURE DELIVER, perform the following steps:**</span></span>

1. <span data-ttu-id="64ef2-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="64ef2-205">V seznamu aplikací vyberte **ZABEZPEČENÉ DORUČOVÁNÍ**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-205">In the applications list, select **SECURE DELIVER**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_app.png) 

3. <span data-ttu-id="64ef2-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="64ef2-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="64ef2-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="64ef2-209">Click **Add** button.</span></span> <span data-ttu-id="64ef2-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64ef2-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="64ef2-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="64ef2-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="64ef2-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64ef2-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="64ef2-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64ef2-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="64ef2-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="64ef2-215">Testing single sign-on</span></span>

<span data-ttu-id="64ef2-216">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="64ef2-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="64ef2-217">Když kliknete na dlaždici ZABEZPEČENÉ DORUČOVÁNÍ na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ZABEZPEČENÉ DORUČOVÁNÍ.</span><span class="sxs-lookup"><span data-stu-id="64ef2-217">When you click the SECURE DELIVER tile in the Access Panel, you should get automatically signed-on to your SECURE DELIVER application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64ef2-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="64ef2-218">Additional resources</span></span>

* [<span data-ttu-id="64ef2-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="64ef2-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="64ef2-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="64ef2-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png

