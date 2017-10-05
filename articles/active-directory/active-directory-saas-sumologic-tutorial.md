---
title: 'Kurz: Azure Active Directory integrace s SumoLogic | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SumoLogic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e739106472ccf930b2942eb810dd844f2b1ade7c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a><span data-ttu-id="a6205-103">Kurz: Azure Active Directory integrace s SumoLogic</span><span class="sxs-lookup"><span data-stu-id="a6205-103">Tutorial: Azure Active Directory integration with SumoLogic</span></span>

<span data-ttu-id="a6205-104">V tomto kurzu zjistěte, jak integrovat SumoLogic s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a6205-104">In this tutorial, you learn how to integrate SumoLogic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a6205-105">Integrace SumoLogic s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a6205-105">Integrating SumoLogic with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a6205-106">Můžete řídit ve službě Azure AD, který má přístup k SumoLogic</span><span class="sxs-lookup"><span data-stu-id="a6205-106">You can control in Azure AD who has access to SumoLogic</span></span>
- <span data-ttu-id="a6205-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k SumoLogic (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a6205-107">You can enable your users to automatically get signed-on to SumoLogic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a6205-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a6205-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a6205-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a6205-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6205-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a6205-110">Prerequisites</span></span>

<span data-ttu-id="a6205-111">Konfigurace integrace Azure AD s SumoLogic, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a6205-111">To configure Azure AD integration with SumoLogic, you need the following items:</span></span>

- <span data-ttu-id="a6205-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a6205-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a6205-113">SumoLogic jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a6205-113">A SumoLogic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a6205-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6205-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a6205-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a6205-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a6205-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a6205-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a6205-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a6205-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a6205-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a6205-118">Scenario description</span></span>
<span data-ttu-id="a6205-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6205-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a6205-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a6205-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a6205-121">Přidání SumoLogic z Galerie</span><span class="sxs-lookup"><span data-stu-id="a6205-121">Adding SumoLogic from the gallery</span></span>
2. <span data-ttu-id="a6205-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a6205-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sumologic-from-the-gallery"></a><span data-ttu-id="a6205-123">Přidání SumoLogic z Galerie</span><span class="sxs-lookup"><span data-stu-id="a6205-123">Adding SumoLogic from the gallery</span></span>
<span data-ttu-id="a6205-124">Při konfiguraci integrace SumoLogic do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS SumoLogic z galerie.</span><span class="sxs-lookup"><span data-stu-id="a6205-124">To configure the integration of SumoLogic into Azure AD, you need to add SumoLogic from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a6205-125">**Pokud chcete přidat SumoLogic z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a6205-125">**To add SumoLogic from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a6205-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a6205-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a6205-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a6205-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a6205-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a6205-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a6205-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a6205-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a6205-133">Do vyhledávacího pole zadejte **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="a6205-133">In the search box, type **SumoLogic**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_search.png)

5. <span data-ttu-id="a6205-135">Na panelu výsledků vyberte **SumoLogic**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a6205-135">In the results panel, select **SumoLogic**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a6205-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a6205-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a6205-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s SumoLogic podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a6205-138">In this section, you configure and test Azure AD single sign-on with SumoLogic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a6205-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SumoLogic je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a6205-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SumoLogic is to a user in Azure AD.</span></span> <span data-ttu-id="a6205-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SumoLogic musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a6205-140">In other words, a link relationship between an Azure AD user and the related user in SumoLogic needs to be established.</span></span>

<span data-ttu-id="a6205-141">V SumoLogic, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a6205-141">In SumoLogic, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a6205-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SumoLogic, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a6205-142">To configure and test Azure AD single sign-on with SumoLogic, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a6205-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a6205-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a6205-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a6205-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a6205-145">**[Vytvoření zkušebního uživatele SumoLogic](#creating-a-sumologic-test-user)**  – Pokud chcete mít protějšek Britta Simon v SumoLogic propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a6205-145">**[Creating a SumoLogic test user](#creating-a-sumologic-test-user)** - to have a counterpart of Britta Simon in SumoLogic that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a6205-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a6205-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a6205-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a6205-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a6205-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a6205-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a6205-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="a6205-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SumoLogic application.</span></span>

<span data-ttu-id="a6205-150">**Ke konfiguraci Azure AD jednotné přihlašování s SumoLogic, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a6205-150">**To configure Azure AD single sign-on with SumoLogic, perform the following steps:**</span></span>

1. <span data-ttu-id="a6205-151">Na portálu Azure na **SumoLogic** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a6205-151">In the Azure portal, on the **SumoLogic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a6205-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a6205-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_samlbase.png)

3. <span data-ttu-id="a6205-155">Na **SumoLogic domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a6205-155">On the **SumoLogic Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_url.png)

    <span data-ttu-id="a6205-157">a.</span><span class="sxs-lookup"><span data-stu-id="a6205-157">a.</span></span> <span data-ttu-id="a6205-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.SumoLogic.com`</span><span class="sxs-lookup"><span data-stu-id="a6205-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.SumoLogic.com`</span></span>

    <span data-ttu-id="a6205-159">b.</span><span class="sxs-lookup"><span data-stu-id="a6205-159">b.</span></span> <span data-ttu-id="a6205-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="a6205-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > <span data-ttu-id="a6205-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a6205-161">These values are not real.</span></span> <span data-ttu-id="a6205-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a6205-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a6205-163">Obraťte se na [tým podpory SumoLogic klienta](https://www.sumologic.com/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a6205-163">Contact [SumoLogic Client support team](https://www.sumologic.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="a6205-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="a6205-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_certificate.png) 

5. <span data-ttu-id="a6205-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a6205-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a6205-168">Na **SumoLogic konfigurace** klikněte na tlačítko **konfigurace SumoLogic** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a6205-168">On the **SumoLogic Configuration** section, click **Configure SumoLogic** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a6205-169">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a6205-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_configure.png) 

7. <span data-ttu-id="a6205-171">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti SumoLogic jako správce.</span><span class="sxs-lookup"><span data-stu-id="a6205-171">In a different web browser window, log in to your SumoLogic company site as an administrator.</span></span>

8. <span data-ttu-id="a6205-172">Přejděte na **spravovat \> zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="a6205-172">Go to **Manage \> Security**.</span></span>
   
    <span data-ttu-id="a6205-173">![Spravovat](./media/active-directory-saas-sumologic-tutorial/ic778556.png "spravovat")</span><span class="sxs-lookup"><span data-stu-id="a6205-173">![Manage](./media/active-directory-saas-sumologic-tutorial/ic778556.png "Manage")</span></span>

9. <span data-ttu-id="a6205-174">Klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="a6205-174">Click **SAML**.</span></span>
   
    <span data-ttu-id="a6205-175">![Nastavení zabezpečení globální](./media/active-directory-saas-sumologic-tutorial/ic778557.png "nastavení globálního zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="a6205-175">![Global security settings](./media/active-directory-saas-sumologic-tutorial/ic778557.png "Global security settings")</span></span>

10. <span data-ttu-id="a6205-176">Z **vyberte konfiguraci, nebo vytvořte novou** vyberte **Azure AD**a potom klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="a6205-176">From the **Select a configuration or create a new one** list, select **Azure AD**, and then click **Configure**.</span></span>
   
    <span data-ttu-id="a6205-177">![Konfigurace SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "konfigurace SAML 2.0")</span><span class="sxs-lookup"><span data-stu-id="a6205-177">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "Configure SAML 2.0")</span></span>

11. <span data-ttu-id="a6205-178">Na **konfigurace SAML 2.0** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a6205-178">On the **Configure SAML 2.0** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="a6205-179">![Konfigurace SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "konfigurace SAML 2.0")</span><span class="sxs-lookup"><span data-stu-id="a6205-179">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "Configure SAML 2.0")</span></span>
   
    <span data-ttu-id="a6205-180">a.</span><span class="sxs-lookup"><span data-stu-id="a6205-180">a.</span></span> <span data-ttu-id="a6205-181">V **název konfigurace** textovému poli, typ **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="a6205-181">In the **Configuration Name** textbox, type **Azure AD**.</span></span> 

    <span data-ttu-id="a6205-182">b.</span><span class="sxs-lookup"><span data-stu-id="a6205-182">b.</span></span> <span data-ttu-id="a6205-183">Vyberte **režim ladění**.</span><span class="sxs-lookup"><span data-stu-id="a6205-183">Select **Debug Mode**.</span></span>

    <span data-ttu-id="a6205-184">c.</span><span class="sxs-lookup"><span data-stu-id="a6205-184">c.</span></span> <span data-ttu-id="a6205-185">V **vystavitele** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a6205-185">In the **Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="a6205-186">d.</span><span class="sxs-lookup"><span data-stu-id="a6205-186">d.</span></span> <span data-ttu-id="a6205-187">V **ověřovacího požadavku URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a6205-187">In the **Authn Request URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a6205-188">e.</span><span class="sxs-lookup"><span data-stu-id="a6205-188">e.</span></span> <span data-ttu-id="a6205-189">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte celý certifikát do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a6205-189">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **X.509 Certificate** textbox.</span></span>

    <span data-ttu-id="a6205-190">f.</span><span class="sxs-lookup"><span data-stu-id="a6205-190">f.</span></span> <span data-ttu-id="a6205-191">Jako **atribut e-mailu**, vyberte **pomocí SAML subjektu**.</span><span class="sxs-lookup"><span data-stu-id="a6205-191">As **Email Attribute**, select **Use SAML subject**.</span></span>  

    <span data-ttu-id="a6205-192">g.</span><span class="sxs-lookup"><span data-stu-id="a6205-192">g.</span></span> <span data-ttu-id="a6205-193">Vyberte **SP iniciované konfigurace přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="a6205-193">Select **SP initiated Login Configuration**.</span></span>

    <span data-ttu-id="a6205-194">h.</span><span class="sxs-lookup"><span data-stu-id="a6205-194">h.</span></span> <span data-ttu-id="a6205-195">V **přihlašovací cestu** textovému poli, typ **Azure** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a6205-195">In the **Login Path** textbox, type **Azure** and click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="a6205-196">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a6205-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a6205-197">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a6205-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a6205-198">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a6205-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a6205-199">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a6205-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="a6205-200">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a6205-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a6205-202">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a6205-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a6205-203">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a6205-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a6205-205">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a6205-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a6205-207">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a6205-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a6205-209">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a6205-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a6205-211">a.</span><span class="sxs-lookup"><span data-stu-id="a6205-211">a.</span></span> <span data-ttu-id="a6205-212">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a6205-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a6205-213">b.</span><span class="sxs-lookup"><span data-stu-id="a6205-213">b.</span></span> <span data-ttu-id="a6205-214">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a6205-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a6205-215">c.</span><span class="sxs-lookup"><span data-stu-id="a6205-215">c.</span></span> <span data-ttu-id="a6205-216">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a6205-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a6205-217">d.</span><span class="sxs-lookup"><span data-stu-id="a6205-217">d.</span></span> <span data-ttu-id="a6205-218">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a6205-218">Click **Create**.</span></span>
 
### <a name="creating-a-sumologic-test-user"></a><span data-ttu-id="a6205-219">Vytvoření zkušebního uživatele SumoLogic</span><span class="sxs-lookup"><span data-stu-id="a6205-219">Creating a SumoLogic test user</span></span>

<span data-ttu-id="a6205-220">Pokud chcete povolit uživatelům Azure AD přihlášení k SumoLogic, musí být zřízená k SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="a6205-220">In order to enable Azure AD users to log in to SumoLogic, they must be provisioned to SumoLogic.</span></span>  

* <span data-ttu-id="a6205-221">V případě SumoLogic zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a6205-221">In the case of SumoLogic, provisioning is a manual task.</span></span>

<span data-ttu-id="a6205-222">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a6205-222">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="a6205-223">Přihlaste se k vaší **SumoLogic** klienta.</span><span class="sxs-lookup"><span data-stu-id="a6205-223">Log in to your **SumoLogic** tenant.</span></span>

2. <span data-ttu-id="a6205-224">Přejděte na **spravovat \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a6205-224">Go to **Manage \> Users**.</span></span>
   
    <span data-ttu-id="a6205-225">![Uživatelé](./media/active-directory-saas-sumologic-tutorial/ic778561.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a6205-225">![Users](./media/active-directory-saas-sumologic-tutorial/ic778561.png "Users")</span></span>

3. <span data-ttu-id="a6205-226">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="a6205-226">Click **Add**.</span></span>
   
    <span data-ttu-id="a6205-227">![Uživatelé](./media/active-directory-saas-sumologic-tutorial/ic778562.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a6205-227">![Users](./media/active-directory-saas-sumologic-tutorial/ic778562.png "Users")</span></span>

4. <span data-ttu-id="a6205-228">Na **nového uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a6205-228">On the **New User** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="a6205-229">![Nový uživatel](./media/active-directory-saas-sumologic-tutorial/ic778563.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="a6205-229">![New User](./media/active-directory-saas-sumologic-tutorial/ic778563.png "New User")</span></span> 
 
    <span data-ttu-id="a6205-230">a.</span><span class="sxs-lookup"><span data-stu-id="a6205-230">a.</span></span> <span data-ttu-id="a6205-231">Zadejte související informace, které chcete zřídit do účtu Azure AD **křestní jméno**, **příjmení**, a **e-mailu** textových polí.</span><span class="sxs-lookup"><span data-stu-id="a6205-231">Type the related information of the Azure AD account you want to provision into the **First Name**, **Last Name**, and **Email** textboxes.</span></span>
  
    <span data-ttu-id="a6205-232">b.</span><span class="sxs-lookup"><span data-stu-id="a6205-232">b.</span></span> <span data-ttu-id="a6205-233">Vyberte roli.</span><span class="sxs-lookup"><span data-stu-id="a6205-233">Select a role.</span></span>
  
    <span data-ttu-id="a6205-234">c.</span><span class="sxs-lookup"><span data-stu-id="a6205-234">c.</span></span> <span data-ttu-id="a6205-235">Jako **stav**, vyberte **Active**.</span><span class="sxs-lookup"><span data-stu-id="a6205-235">As **Status**, select **Active**.</span></span>
  
    <span data-ttu-id="a6205-236">d.</span><span class="sxs-lookup"><span data-stu-id="a6205-236">d.</span></span> <span data-ttu-id="a6205-237">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a6205-237">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="a6205-238">Můžete použít všechny ostatní SumoLogic uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované SumoLogic zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="a6205-238">You can use any other SumoLogic user account creation tools or APIs provided by SumoLogic to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a6205-239">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a6205-239">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a6205-240">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="a6205-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SumoLogic.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a6205-242">**Pokud chcete přiřadit Britta Simon SumoLogic, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a6205-242">**To assign Britta Simon to SumoLogic, perform the following steps:**</span></span>

1. <span data-ttu-id="a6205-243">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a6205-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a6205-245">V seznamu aplikací vyberte **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="a6205-245">In the applications list, select **SumoLogic**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_app.png) 

3. <span data-ttu-id="a6205-247">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a6205-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a6205-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a6205-249">Click **Add** button.</span></span> <span data-ttu-id="a6205-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a6205-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a6205-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a6205-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a6205-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a6205-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a6205-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a6205-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a6205-255">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a6205-255">Testing single sign-on</span></span>

<span data-ttu-id="a6205-256">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a6205-256">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a6205-257">Když kliknete na dlaždici SumoLogic na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="a6205-257">When you click the SumoLogic tile in the Access Panel, you should get automatically signed-on to your SumoLogic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6205-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a6205-258">Additional resources</span></span>

* [<span data-ttu-id="a6205-259">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a6205-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a6205-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a6205-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_203.png

