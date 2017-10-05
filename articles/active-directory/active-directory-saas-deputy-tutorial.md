---
title: "Kurz: Azure Active Directory integrace s zástupce | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a zástupce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 51aed908208b7a40ea2ab710dffe84370b573991
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a><span data-ttu-id="c2886-103">Kurz: Azure Active Directory integrace s zástupce</span><span class="sxs-lookup"><span data-stu-id="c2886-103">Tutorial: Azure Active Directory integration with Deputy</span></span>

<span data-ttu-id="c2886-104">V tomto kurzu zjistěte, jak integrovat zástupce s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c2886-104">In this tutorial, you learn how to integrate Deputy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c2886-105">Integrace zástupce s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c2886-105">Integrating Deputy with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c2886-106">Můžete řídit ve službě Azure AD, který má přístup k zástupce</span><span class="sxs-lookup"><span data-stu-id="c2886-106">You can control in Azure AD who has access to Deputy</span></span>
- <span data-ttu-id="c2886-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k zástupce (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2886-107">You can enable your users to automatically get signed-on to Deputy (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c2886-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c2886-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c2886-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c2886-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2886-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c2886-110">Prerequisites</span></span>

<span data-ttu-id="c2886-111">Konfigurace integrace Azure AD s zástupce, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c2886-111">To configure Azure AD integration with Deputy, you need the following items:</span></span>

- <span data-ttu-id="c2886-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2886-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c2886-113">Zástupce jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c2886-113">A Deputy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c2886-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2886-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c2886-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c2886-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c2886-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c2886-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c2886-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c2886-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c2886-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c2886-118">Scenario description</span></span>
<span data-ttu-id="c2886-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2886-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c2886-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c2886-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c2886-121">Přidání zástupce z Galerie</span><span class="sxs-lookup"><span data-stu-id="c2886-121">Adding Deputy from the gallery</span></span>
2. <span data-ttu-id="c2886-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c2886-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-deputy-from-the-gallery"></a><span data-ttu-id="c2886-123">Přidání zástupce z Galerie</span><span class="sxs-lookup"><span data-stu-id="c2886-123">Adding Deputy from the gallery</span></span>
<span data-ttu-id="c2886-124">Konfigurace integrace zástupce do Azure AD, musíte přidat zástupce z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c2886-124">To configure the integration of Deputy into Azure AD, you need to add Deputy from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c2886-125">**Pokud chcete přidat zástupce z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c2886-125">**To add Deputy from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c2886-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c2886-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c2886-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c2886-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c2886-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c2886-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c2886-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c2886-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c2886-133">Do vyhledávacího pole zadejte **zástupce**.</span><span class="sxs-lookup"><span data-stu-id="c2886-133">In the search box, type **Deputy**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_search.png)

5. <span data-ttu-id="c2886-135">Na panelu výsledků vyberte **zástupce**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c2886-135">In the results panel, select **Deputy**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c2886-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c2886-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c2886-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s zástupce podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="c2886-138">In this section, you configure and test Azure AD single sign-on with Deputy based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c2886-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v zástupce je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c2886-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Deputy is to a user in Azure AD.</span></span> <span data-ttu-id="c2886-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v zástupce musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c2886-140">In other words, a link relationship between an Azure AD user and the related user in Deputy needs to be established.</span></span>

<span data-ttu-id="c2886-141">V zástupce, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="c2886-141">In Deputy, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c2886-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s zástupce, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c2886-142">To configure and test Azure AD single sign-on with Deputy, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c2886-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c2886-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c2886-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2886-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c2886-145">**[Vytvoření zkušebního uživatele zástupce](#creating-a-deputy-test-user)**  – Pokud chcete mít protějšek Britta Simon v zástupce propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c2886-145">**[Creating a Deputy test user](#creating-a-deputy-test-user)** - to have a counterpart of Britta Simon in Deputy that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c2886-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c2886-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c2886-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c2886-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c2886-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c2886-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c2886-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci zástupce.</span><span class="sxs-lookup"><span data-stu-id="c2886-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Deputy application.</span></span>

<span data-ttu-id="c2886-150">**Ke konfiguraci Azure AD jednotné přihlašování s zástupce, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c2886-150">**To configure Azure AD single sign-on with Deputy, perform the following steps:**</span></span>

1. <span data-ttu-id="c2886-151">Na portálu Azure na **zástupce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c2886-151">In the Azure portal, on the **Deputy** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c2886-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c2886-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_samlbase.png)

3. <span data-ttu-id="c2886-155">Na **zástupce domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="c2886-155">On the **Deputy Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url1.png)

    <span data-ttu-id="c2886-157">a.</span><span class="sxs-lookup"><span data-stu-id="c2886-157">a.</span></span> <span data-ttu-id="c2886-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="c2886-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    <span data-ttu-id="c2886-159">b.</span><span class="sxs-lookup"><span data-stu-id="c2886-159">b.</span></span> <span data-ttu-id="c2886-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="c2886-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs.` |

4. <span data-ttu-id="c2886-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="c2886-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="c2886-162">Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="c2886-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url2.png)

    <span data-ttu-id="c2886-164">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<your-subdomain>.<region>.deputy.com`</span><span class="sxs-lookup"><span data-stu-id="c2886-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<your-subdomain>.<region>.deputy.com`</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="c2886-165">Zástupce oblast přípona je volitelné, nebo se musí používat jednu z těchto: au | Ná | Evropa | jako | la | af | | ent au | ent na | ent Evropa | ent-jako | trola la | trola af | trola – element</span><span class="sxs-lookup"><span data-stu-id="c2886-165">Deputy region suffix is optional, or it should use one of these: au | na | eu |as |la |af |an |ent-au |ent-na |ent-eu |ent-as | ent-la | ent-af | ent-an</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c2886-166">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c2886-166">These values are not real.</span></span> <span data-ttu-id="c2886-167">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="c2886-167">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="c2886-168">Obraťte se na [tým podpory zástupce](https://www.deputy.com/call-centers-customer-support-scheduling-software) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="c2886-168">Contact [Deputy support team](https://www.deputy.com/call-centers-customer-support-scheduling-software) to get these values.</span></span> 

5. <span data-ttu-id="c2886-169">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="c2886-169">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_certificate.png) 

6. <span data-ttu-id="c2886-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c2886-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="c2886-173">Na **zástupce konfigurace** klikněte na tlačítko **konfigurace zástupce** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c2886-173">On the **Deputy Configuration** section, click **Configure Deputy** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c2886-174">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c2886-174">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_configure.png) 

8. <span data-ttu-id="c2886-176">Přejděte na následující adresu URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span><span class="sxs-lookup"><span data-stu-id="c2886-176">Navigate to the following URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span></span> <span data-ttu-id="c2886-177">Přejděte na **nastavení zabezpečení** a klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="c2886-177">Go to **Security Settings** and click **Edit**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

9. <span data-ttu-id="c2886-179">V tomto **nastavení zabezpečení** proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="c2886-179">On this **Security Settings** page, perform below steps.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)
    
    <span data-ttu-id="c2886-181">a.</span><span class="sxs-lookup"><span data-stu-id="c2886-181">a.</span></span> <span data-ttu-id="c2886-182">Povolit **přihlášení prostřednictvím sociální sítě**.</span><span class="sxs-lookup"><span data-stu-id="c2886-182">Enable **Social Login**.</span></span>
   
    <span data-ttu-id="c2886-183">b.</span><span class="sxs-lookup"><span data-stu-id="c2886-183">b.</span></span> <span data-ttu-id="c2886-184">Otevřete váš certifikát kódovaný v Base64 stáhli z portálu Azure v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **OpenSSL certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c2886-184">Open your Base64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **OpenSSL Certificate** textbox.</span></span>
   
    <span data-ttu-id="c2886-185">c.</span><span class="sxs-lookup"><span data-stu-id="c2886-185">c.</span></span> <span data-ttu-id="c2886-186">Do textového pole adresy URL jednotné přihlašování SAML zadejte`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span><span class="sxs-lookup"><span data-stu-id="c2886-186">In the SAML SSO URL textbox, type `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span></span>
    
    <span data-ttu-id="c2886-187">d.</span><span class="sxs-lookup"><span data-stu-id="c2886-187">d.</span></span> <span data-ttu-id="c2886-188">Do textového pole adresy URL jednotné přihlašování SAML, nahraďte `<your subdomain>` s vaší subdomény.</span><span class="sxs-lookup"><span data-stu-id="c2886-188">In the SAML SSO URL textbox, replace `<your subdomain>` with your subdomain.</span></span>
   
    <span data-ttu-id="c2886-189">e.</span><span class="sxs-lookup"><span data-stu-id="c2886-189">e.</span></span> <span data-ttu-id="c2886-190">Do textového pole adresy URL jednotné přihlašování SAML, nahraďte `<saml sso url>` s **SAML jeden přihlašování adresa URL služby** jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c2886-190">In the SAML SSO URL textbox, replace `<saml sso url>` with the **SAML Single Sign-On Service URL** you have copied from the Azure portal.</span></span>
   
    <span data-ttu-id="c2886-191">f.</span><span class="sxs-lookup"><span data-stu-id="c2886-191">f.</span></span> <span data-ttu-id="c2886-192">Klikněte na tlačítko **uložit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c2886-192">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="c2886-193">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c2886-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c2886-194">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c2886-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c2886-195">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c2886-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c2886-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2886-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="c2886-197">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2886-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c2886-199">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c2886-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c2886-200">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c2886-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c2886-202">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c2886-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c2886-204">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c2886-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c2886-206">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c2886-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c2886-208">a.</span><span class="sxs-lookup"><span data-stu-id="c2886-208">a.</span></span> <span data-ttu-id="c2886-209">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c2886-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c2886-210">b.</span><span class="sxs-lookup"><span data-stu-id="c2886-210">b.</span></span> <span data-ttu-id="c2886-211">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c2886-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c2886-212">c.</span><span class="sxs-lookup"><span data-stu-id="c2886-212">c.</span></span> <span data-ttu-id="c2886-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c2886-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c2886-214">d.</span><span class="sxs-lookup"><span data-stu-id="c2886-214">d.</span></span> <span data-ttu-id="c2886-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c2886-215">Click **Create**.</span></span>
 
### <a name="creating-a-deputy-test-user"></a><span data-ttu-id="c2886-216">Vytvoření zástupce zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="c2886-216">Creating a Deputy test user</span></span>

<span data-ttu-id="c2886-217">Pokud chcete povolit uživatelům Azure AD přihlášení na zástupce, musí být zřízená do zástupce.</span><span class="sxs-lookup"><span data-stu-id="c2886-217">To enable Azure AD users to log in to Deputy, they must be provisioned into Deputy.</span></span> <span data-ttu-id="c2886-218">V případě zástupce zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="c2886-218">In case of Deputy, provisioning is a manual task.</span></span>

#### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="c2886-219">K poskytnutí uživatelského účtu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c2886-219">To provision a user account, perform the following steps:</span></span>
1. <span data-ttu-id="c2886-220">Přihlaste se k serveru vaší společnosti zástupce jako správce.</span><span class="sxs-lookup"><span data-stu-id="c2886-220">Log in to your Deputy company site as an administrator.</span></span>

2. <span data-ttu-id="c2886-221">V horním navigačním podokně klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="c2886-221">On the top navigation pane, click **People**.</span></span>
   
   <span data-ttu-id="c2886-222">![Lidé](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="c2886-222">![People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "People")</span></span>

3. <span data-ttu-id="c2886-223">Klikněte **přidat osoby** tlačítko a klikněte na tlačítko **přidat jedna osoba**.</span><span class="sxs-lookup"><span data-stu-id="c2886-223">Click the **Add People** button and click **Add a single person**.</span></span>
   
   <span data-ttu-id="c2886-224">![Přidat osoby](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="c2886-224">![Add People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Add People")</span></span>

4. <span data-ttu-id="c2886-225">Proveďte následující kroky a klikněte na tlačítko **Uložit & Pozvat**.</span><span class="sxs-lookup"><span data-stu-id="c2886-225">Perform the following steps and click **Save & Invite**.</span></span>
   
   <span data-ttu-id="c2886-226">![Nový uživatel](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="c2886-226">![New User](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "New User")</span></span>

   <span data-ttu-id="c2886-227">a.</span><span class="sxs-lookup"><span data-stu-id="c2886-227">a.</span></span> <span data-ttu-id="c2886-228">V **název** textovému poli, název typu uživatele jako **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c2886-228">In the **Name** textbox, type name of the user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="c2886-229">b.</span><span class="sxs-lookup"><span data-stu-id="c2886-229">b.</span></span> <span data-ttu-id="c2886-230">V **e-mailu** textovému poli, zadejte e-mailovou adresu účtu služby Azure AD, které chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="c2886-230">In the **Email** textbox, type the email address of an Azure AD account you want to provision.</span></span>
   
   <span data-ttu-id="c2886-231">c.</span><span class="sxs-lookup"><span data-stu-id="c2886-231">c.</span></span> <span data-ttu-id="c2886-232">V **fungují na** textovému poli, zadejte název firmy.</span><span class="sxs-lookup"><span data-stu-id="c2886-232">In the **Work at** textbox, type the business name.</span></span>
   
   <span data-ttu-id="c2886-233">d.</span><span class="sxs-lookup"><span data-stu-id="c2886-233">d.</span></span> <span data-ttu-id="c2886-234">Klikněte na tlačítko **Uložit & Pozvat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c2886-234">Click **Save & Invite** button.</span></span>

5. <span data-ttu-id="c2886-235">Držitel účtu AAD obdrží e-mailu a dodržuje odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="c2886-235">The AAD account holder receives an email and follows a link to confirm their account before it becomes active.</span></span> <span data-ttu-id="c2886-236">Můžete použít všechny ostatní zástupce uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované zástupce zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="c2886-236">You can use any other Deputy user account creation tools or APIs provided by Deputy to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c2886-237">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2886-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c2886-238">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu zástupce.</span><span class="sxs-lookup"><span data-stu-id="c2886-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Deputy.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c2886-240">**Pokud chcete přiřadit Britta Simon zástupce, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c2886-240">**To assign Britta Simon to Deputy, perform the following steps:**</span></span>

1. <span data-ttu-id="c2886-241">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c2886-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c2886-243">V seznamu aplikací vyberte **zástupce**.</span><span class="sxs-lookup"><span data-stu-id="c2886-243">In the applications list, select **Deputy**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_app.png) 

3. <span data-ttu-id="c2886-245">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c2886-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c2886-247">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c2886-247">Click **Add** button.</span></span> <span data-ttu-id="c2886-248">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c2886-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c2886-250">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c2886-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c2886-251">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c2886-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c2886-252">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c2886-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c2886-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c2886-253">Testing single sign-on</span></span>

<span data-ttu-id="c2886-254">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="c2886-254">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="c2886-255">Když kliknete na dlaždici zástupce na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci zástupce.</span><span class="sxs-lookup"><span data-stu-id="c2886-255">When you click the Deputy tile in the Access Panel, you should get automatically signed-on to your Deputy application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2886-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c2886-256">Additional resources</span></span>

* [<span data-ttu-id="c2886-257">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c2886-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c2886-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c2886-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png

