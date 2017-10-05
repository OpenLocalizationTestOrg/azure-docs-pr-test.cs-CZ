---
title: "Kurz: Azure Active Directory integrace s Menlo zabezpečení | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Menlo zabezpečení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: 75366abafa551d21630b0edddb65db23b9ea9d42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a><span data-ttu-id="3aa54-103">Kurz: Azure Active Directory integrace s Menlo zabezpečení</span><span class="sxs-lookup"><span data-stu-id="3aa54-103">Tutorial: Azure Active Directory integration with Menlo Security</span></span>

<span data-ttu-id="3aa54-104">V tomto kurzu zjistěte, jak integrovat Menlo zabezpečení v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3aa54-104">In this tutorial, you learn how to integrate Menlo Security with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3aa54-105">Integrace Menlo zabezpečení s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3aa54-105">Integrating Menlo Security with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3aa54-106">Můžete řídit ve službě Azure AD, který má přístup k Menlo zabezpečení</span><span class="sxs-lookup"><span data-stu-id="3aa54-106">You can control in Azure AD who has access to Menlo Security</span></span>
- <span data-ttu-id="3aa54-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k zabezpečení Menlo (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3aa54-107">You can enable your users to automatically get signed-on to Menlo Security (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3aa54-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3aa54-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3aa54-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma.</span><span class="sxs-lookup"><span data-stu-id="3aa54-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="3aa54-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3aa54-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3aa54-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3aa54-111">Prerequisites</span></span>

<span data-ttu-id="3aa54-112">Konfigurace integrace Azure AD s Menlo zabezpečení, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3aa54-112">To configure Azure AD integration with Menlo Security, you need the following items:</span></span>

- <span data-ttu-id="3aa54-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3aa54-113">An Azure AD subscription</span></span>
- <span data-ttu-id="3aa54-114">Zabezpečení Menlo jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3aa54-114">A Menlo Security single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3aa54-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3aa54-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3aa54-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3aa54-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3aa54-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3aa54-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3aa54-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3aa54-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3aa54-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3aa54-119">Scenario description</span></span>
<span data-ttu-id="3aa54-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3aa54-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3aa54-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3aa54-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3aa54-122">Přidání Menlo zabezpečení z Galerie</span><span class="sxs-lookup"><span data-stu-id="3aa54-122">Adding Menlo Security from the gallery</span></span>
2. <span data-ttu-id="3aa54-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3aa54-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-menlo-security-from-the-gallery"></a><span data-ttu-id="3aa54-124">Přidání Menlo zabezpečení z Galerie</span><span class="sxs-lookup"><span data-stu-id="3aa54-124">Adding Menlo Security from the gallery</span></span>
<span data-ttu-id="3aa54-125">Při konfiguraci integrace Menlo zabezpečení do služby Azure AD, musíte zvýšit zabezpečení Menlo z Galerie váš seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3aa54-125">To configure the integration of Menlo Security into Azure AD, you need to add Menlo Security from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3aa54-126">**Pokud chcete přidat Menlo zabezpečení z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3aa54-126">**To add Menlo Security from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3aa54-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3aa54-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3aa54-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3aa54-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-130">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3aa54-132">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aa54-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3aa54-134">Do vyhledávacího pole zadejte **Menlo zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-134">In the search box, type **Menlo Security**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_search.png)

5. <span data-ttu-id="3aa54-136">Na panelu výsledků vyberte **Menlo zabezpečení**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3aa54-136">In the results panel, select **Menlo Security**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3aa54-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3aa54-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3aa54-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Menlo zabezpečení založené na testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3aa54-139">In this section, you configure and test Azure AD single sign-on with Menlo Security based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3aa54-140">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Menlo zabezpečení je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3aa54-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Menlo Security is to a user in Azure AD.</span></span> <span data-ttu-id="3aa54-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Menlo zabezpečení je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="3aa54-141">In other words, a link relationship between an Azure AD user and the related user in Menlo Security needs to be established.</span></span>

<span data-ttu-id="3aa54-142">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Menlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3aa54-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Menlo Security.</span></span>

<span data-ttu-id="3aa54-143">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Menlo zabezpečení, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3aa54-143">To configure and test Azure AD single sign-on with Menlo Security, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3aa54-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3aa54-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3aa54-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3aa54-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3aa54-146">**[Vytvoření zkušebního uživatele Menlo zabezpečení](#creating-a-menlo-security-test-user)**  – Pokud chcete mít protějšek Britta Simon v Menlo zabezpečení, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3aa54-146">**[Creating a Menlo Security test user](#creating-a-menlo-security-test-user)** - to have a counterpart of Britta Simon in Menlo Security that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3aa54-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3aa54-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3aa54-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3aa54-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3aa54-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3aa54-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3aa54-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Menlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3aa54-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Menlo Security application.</span></span>

<span data-ttu-id="3aa54-151">**Ke konfiguraci Azure AD jednotné přihlašování s Menlo zabezpečení, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3aa54-151">**To configure Azure AD single sign-on with Menlo Security, perform the following steps:**</span></span>

1. <span data-ttu-id="3aa54-152">Na portálu Azure na **Menlo zabezpečení** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-152">In the Azure portal, on the **Menlo Security** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3aa54-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3aa54-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

3. <span data-ttu-id="3aa54-156">Na **Menlo zabezpečení domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3aa54-156">On the **Menlo Security Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    <span data-ttu-id="3aa54-158">a.</span><span class="sxs-lookup"><span data-stu-id="3aa54-158">a.</span></span> <span data-ttu-id="3aa54-159">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.menlosecurity.com/account/login`</span><span class="sxs-lookup"><span data-stu-id="3aa54-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.menlosecurity.com/account/login`</span></span>

    <span data-ttu-id="3aa54-160">b.</span><span class="sxs-lookup"><span data-stu-id="3aa54-160">b.</span></span> <span data-ttu-id="3aa54-161">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="3aa54-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3aa54-162">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="3aa54-162">These values are not the real.</span></span> <span data-ttu-id="3aa54-163">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="3aa54-163">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3aa54-164">Obraťte se na [tým podpory zabezpečení klienta Menlo](https://www.menlosecurity.com/menlo-contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3aa54-164">Contact [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) to get these values.</span></span> 
 
4. <span data-ttu-id="3aa54-165">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="3aa54-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the Certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

5. <span data-ttu-id="3aa54-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3aa54-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3aa54-169">Na **konfigurace zabezpečení Menlo** klikněte na tlačítko **konfigurace zabezpečení Menlo** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3aa54-169">On the **Menlo Security Configuration** section, click **Configure Menlo Security** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3aa54-170">Kopírování **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3aa54-170">Copy the **SAML Entity ID**, and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

7. <span data-ttu-id="3aa54-172">Konfigurace jednotného přihlašování na **Menlo zabezpečení** straně, přihlášení, které **Menlo zabezpečení** webu jako správce.</span><span class="sxs-lookup"><span data-stu-id="3aa54-172">To configure single sign-on on **Menlo Security** side, login to the **Menlo Security** website as an administrator.</span></span>

8. <span data-ttu-id="3aa54-173">V části **nastavení** přejděte na **ověřování** a proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="3aa54-173">Under **Settings** go to **Authentication** and perform following actions:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/menlo_user_setup.png)

    <span data-ttu-id="3aa54-175">a.</span><span class="sxs-lookup"><span data-stu-id="3aa54-175">a.</span></span> <span data-ttu-id="3aa54-176">Zaškrtněte políčka **povolení ověřování uživatele pomocí SAML**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-176">Tick the checkbox **Enable user authentication using SAML**.</span></span>

    <span data-ttu-id="3aa54-177">b.</span><span class="sxs-lookup"><span data-stu-id="3aa54-177">b.</span></span> <span data-ttu-id="3aa54-178">Vyberte **povolit externí přístup** k **Ano**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-178">Select **Allow External Access** to **Yes**.</span></span>

    <span data-ttu-id="3aa54-179">c.</span><span class="sxs-lookup"><span data-stu-id="3aa54-179">c.</span></span> <span data-ttu-id="3aa54-180">V části **SAML zprostředkovatele**, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-180">Under **SAML Provider**, select **Azure Active Directory**.</span></span>

    <span data-ttu-id="3aa54-181">d.</span><span class="sxs-lookup"><span data-stu-id="3aa54-181">d.</span></span> <span data-ttu-id="3aa54-182">**Koncový bod SAML 2.0** : vložení **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3aa54-182">**SAML 2.0 Endpoint** : Paste the **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="3aa54-183">e.</span><span class="sxs-lookup"><span data-stu-id="3aa54-183">e.</span></span> <span data-ttu-id="3aa54-184">**Identifikátor služby (Vystavitel)** : vložení **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3aa54-184">**Service Identifier (Issuer)** : Paste the **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="3aa54-185">f.</span><span class="sxs-lookup"><span data-stu-id="3aa54-185">f.</span></span> <span data-ttu-id="3aa54-186">**Certifikát X.509** : Otevřete **certifikátu (Base64)** stáhli z portálu Azure v programu Poznámkový blok a vložte ho v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="3aa54-186">**X.509 Certificate** : Open the **Certificate (Base64)** downloaded from the Azure Portal in notepad and paste it in this box.</span></span>

    <span data-ttu-id="3aa54-187">g.</span><span class="sxs-lookup"><span data-stu-id="3aa54-187">g.</span></span> <span data-ttu-id="3aa54-188">Klikněte na tlačítko **Uložit** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="3aa54-188">Click **Save** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="3aa54-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3aa54-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3aa54-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3aa54-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3aa54-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3aa54-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3aa54-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3aa54-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="3aa54-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3aa54-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3aa54-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3aa54-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3aa54-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3aa54-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3aa54-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3aa54-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aa54-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3aa54-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3aa54-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3aa54-204">a.</span><span class="sxs-lookup"><span data-stu-id="3aa54-204">a.</span></span> <span data-ttu-id="3aa54-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3aa54-206">b.</span><span class="sxs-lookup"><span data-stu-id="3aa54-206">b.</span></span> <span data-ttu-id="3aa54-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3aa54-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3aa54-208">c.</span><span class="sxs-lookup"><span data-stu-id="3aa54-208">c.</span></span> <span data-ttu-id="3aa54-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3aa54-210">d.</span><span class="sxs-lookup"><span data-stu-id="3aa54-210">d.</span></span> <span data-ttu-id="3aa54-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-211">Click **Create**.</span></span>
 
### <a name="creating-a-menlo-security-test-user"></a><span data-ttu-id="3aa54-212">Vytvoření zkušebního uživatele Menlo zabezpečení</span><span class="sxs-lookup"><span data-stu-id="3aa54-212">Creating a Menlo Security test user</span></span>
 
<span data-ttu-id="3aa54-213">V této části vytvoříte volal Britta Simon v Menlo zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="3aa54-213">In this section, you create a user called Britta Simon in Menlo Security.</span></span> <span data-ttu-id="3aa54-214">Práce s [tým podpory Menlo zabezpečení klienta](https://www.menlosecurity.com/menlo-contact) přidat uživatele do platformy Menlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3aa54-214">Work with [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) to add the users in the Menlo Security platform.</span></span> <span data-ttu-id="3aa54-215">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3aa54-215">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3aa54-216">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3aa54-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3aa54-217">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Menlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3aa54-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Menlo Security.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3aa54-219">**Pokud chcete přiřadit Britta Simon Menlo zabezpečení, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3aa54-219">**To assign Britta Simon to Menlo Security, perform the following steps:**</span></span>

1. <span data-ttu-id="3aa54-220">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3aa54-222">V seznamu aplikací vyberte **Menlo zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-222">In the applications list, select **Menlo Security**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

3. <span data-ttu-id="3aa54-224">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3aa54-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3aa54-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3aa54-226">Click **Add** button.</span></span> <span data-ttu-id="3aa54-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aa54-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3aa54-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3aa54-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3aa54-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aa54-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3aa54-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aa54-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3aa54-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3aa54-232">Testing single sign-on</span></span>

<span data-ttu-id="3aa54-233">V této části můžete vyzkoušet Azure AD jeden přihlašování v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="3aa54-233">In this section, you test your Azure AD single sign-on configuration.</span></span>

<span data-ttu-id="3aa54-234">Otevřete okno prohlížeče v režimu "InPrivate" nebo "Incognito" k aktivaci nové ověřování.</span><span class="sxs-lookup"><span data-stu-id="3aa54-234">Open a browser window in an "InPrivate" or "Incognito" mode to trigger a new authentication.</span></span>  <span data-ttu-id="3aa54-235">V aplikaci Internet Explorer použijte kombinaci kláves Ctrl + Shift + P.</span><span class="sxs-lookup"><span data-stu-id="3aa54-235">In Internet Explorer, use Ctrl+Shift+P.</span></span>  <span data-ttu-id="3aa54-236">V prohlížeči Chrome použijte kombinaci kláves Ctrl + Shift + N.</span><span class="sxs-lookup"><span data-stu-id="3aa54-236">In Chrome, use Ctrl+Shift+N.</span></span>  <span data-ttu-id="3aa54-237">V okně privátní procházení přejděte k chráněnému prostředku a provést přihlášení Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3aa54-237">In the private browsing window, browse to a protected resource and perform an Azure AD login.</span></span>  <span data-ttu-id="3aa54-238">Po úspěšném přihlášení ověření budete přesměrováni na požadovaný server v relaci izolace.</span><span class="sxs-lookup"><span data-stu-id="3aa54-238">Upon successful login, you will be taken to the requested site in an isolation session.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3aa54-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3aa54-239">Additional resources</span></span>

* [<span data-ttu-id="3aa54-240">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3aa54-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3aa54-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3aa54-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_203.png

