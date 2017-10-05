---
title: "Kurz: Azure Active Directory integrace s SensoScientific bezdrátové teploty monitorování systému | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SensoScientific bezdrátové teploty monitorování systému."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: fa6242cf7f9559ca394ffde2e5e734cb935b03dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a><span data-ttu-id="53124-103">Kurz: Azure Active Directory integrace s SensoScientific bezdrátové teploty monitorování systému</span><span class="sxs-lookup"><span data-stu-id="53124-103">Tutorial: Azure Active Directory integration with SensoScientific Wireless Temperature Monitoring System</span></span>

<span data-ttu-id="53124-104">V tomto kurzu zjistěte, jak integrovat SensoScientific bezdrátové teploty monitorování systému s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="53124-104">In this tutorial, you learn how to integrate SensoScientific Wireless Temperature Monitoring System with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="53124-105">Integrace s Azure AD SensoScientific bezdrátové teploty monitorování systému poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="53124-105">Integrating SensoScientific Wireless Temperature Monitoring System with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="53124-106">Můžete řídit ve službě Azure AD, který má přístup k SensoScientific bezdrátové teploty monitorování systému</span><span class="sxs-lookup"><span data-stu-id="53124-106">You can control in Azure AD who has access to SensoScientific Wireless Temperature Monitoring System</span></span>
- <span data-ttu-id="53124-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k SensoScientific bezdrátové teploty monitorování systému (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="53124-107">You can enable your users to automatically get signed-on to SensoScientific Wireless Temperature Monitoring System (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="53124-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="53124-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="53124-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="53124-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53124-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="53124-110">Prerequisites</span></span>

<span data-ttu-id="53124-111">Konfigurace integrace Azure AD s SensoScientific bezdrátové teploty monitorování systému, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="53124-111">To configure Azure AD integration with SensoScientific Wireless Temperature Monitoring System, you need the following items:</span></span>

- <span data-ttu-id="53124-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="53124-112">An Azure AD subscription</span></span>
- <span data-ttu-id="53124-113">SensoScientific bezdrátové teploty monitorování systému jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="53124-113">A SensoScientific Wireless Temperature Monitoring System single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="53124-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="53124-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="53124-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="53124-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="53124-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="53124-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="53124-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="53124-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="53124-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="53124-118">Scenario description</span></span>
<span data-ttu-id="53124-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="53124-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="53124-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="53124-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="53124-121">Přidání SensoScientific bezdrátové teploty monitorování systému z Galerie</span><span class="sxs-lookup"><span data-stu-id="53124-121">Adding SensoScientific Wireless Temperature Monitoring System from the gallery</span></span>
2. <span data-ttu-id="53124-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="53124-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a><span data-ttu-id="53124-123">Přidání SensoScientific bezdrátové teploty monitorování systému z Galerie</span><span class="sxs-lookup"><span data-stu-id="53124-123">Adding SensoScientific Wireless Temperature Monitoring System from the gallery</span></span>
<span data-ttu-id="53124-124">Při konfiguraci integrace SensoScientific bezdrátové teploty monitorování systému do služby Azure AD, potřebujete přidat SensoScientific bezdrátové teploty monitorování systému z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="53124-124">To configure the integration of SensoScientific Wireless Temperature Monitoring System into Azure AD, you need to add SensoScientific Wireless Temperature Monitoring System from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="53124-125">**Chcete-li přidat SensoScientific bezdrátové teploty monitorování systému z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="53124-125">**To add SensoScientific Wireless Temperature Monitoring System from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="53124-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="53124-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="53124-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="53124-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="53124-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="53124-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="53124-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="53124-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="53124-133">Do vyhledávacího pole zadejte **SensoScientific bezdrátové teploty monitorování systému**.</span><span class="sxs-lookup"><span data-stu-id="53124-133">In the search box, type **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

5. <span data-ttu-id="53124-135">Na panelu výsledků vyberte **SensoScientific bezdrátové teploty monitorování systému**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="53124-135">In the results panel, select **SensoScientific Wireless Temperature Monitoring System**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="53124-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="53124-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="53124-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SensoScientific bezdrátové teploty monitorování systému podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="53124-138">In this section, you configure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="53124-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SensoScientific bezdrátové teploty monitorování systému je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="53124-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SensoScientific Wireless Temperature Monitoring System is to a user in Azure AD.</span></span> <span data-ttu-id="53124-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SensoScientific bezdrátové teploty monitorování systému, je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="53124-140">In other words, a link relationship between an Azure AD user and the related user in SensoScientific Wireless Temperature Monitoring System needs to be established.</span></span>

<span data-ttu-id="53124-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="53124-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SensoScientific Wireless Temperature Monitoring System.</span></span>

<span data-ttu-id="53124-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SensoScientific bezdrátové teploty monitorování systému, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="53124-142">To configure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="53124-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="53124-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="53124-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="53124-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="53124-145">**[Vytvoření zkušebního uživatele SensoScientific bezdrátové teploty monitorování systému](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  – Pokud chcete mít protějšek Britta Simon v SensoScientific bezdrátové teploty monitorování systému, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="53124-145">**[Creating a SensoScientific Wireless Temperature Monitoring System test user](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)** - to have a counterpart of Britta Simon in SensoScientific Wireless Temperature Monitoring System that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="53124-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="53124-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="53124-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="53124-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="53124-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="53124-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="53124-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="53124-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SensoScientific Wireless Temperature Monitoring System application.</span></span>

<span data-ttu-id="53124-150">**Ke konfiguraci Azure AD jednotné přihlašování s SensoScientific bezdrátové teploty monitorování systému, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="53124-150">**To configure Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, perform the following steps:**</span></span>

1. <span data-ttu-id="53124-151">Na portálu Azure na **SensoScientific bezdrátové teploty monitorování systému** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="53124-151">In the Azure portal, on the **SensoScientific Wireless Temperature Monitoring System** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="53124-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="53124-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

3. <span data-ttu-id="53124-155">Na **SensoScientific bezdrátové teploty monitorování systému domény a adresy URL** část, není nutné provádět žádné kroky, protože aplikace je již předem integrované se službou Azure:</span><span class="sxs-lookup"><span data-stu-id="53124-155">On the **SensoScientific Wireless Temperature Monitoring System Domain and URLs** section, no need to perform any steps as the app is already pre-integrated with Azure:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

4. <span data-ttu-id="53124-157">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="53124-157">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

5. <span data-ttu-id="53124-159">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="53124-159">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="53124-161">Na **SensoScientific bezdrátové teploty monitorování System Configuration** klikněte na tlačítko **nakonfigurujte systém monitorování SensoScientific bezdrátové teploty** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="53124-161">On the **SensoScientific Wireless Temperature Monitoring System Configuration** section, click **Configure SensoScientific Wireless Temperature Monitoring System** to open **Configure sign-on** window.</span></span> <span data-ttu-id="53124-162">Kopírování **Sign-Out adresu URL, SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="53124-162">Copy the **Sign-Out URL, SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

7. <span data-ttu-id="53124-164">Přihlásit se k vaší aplikaci SensoScientific bezdrátové teploty monitorování systému jako správce.</span><span class="sxs-lookup"><span data-stu-id="53124-164">Sign on to your SensoScientific Wireless Temperature Monitoring System application as an administrator.</span></span>

8. <span data-ttu-id="53124-165">V navigační nabídce v horní části, klikněte na tlačítko **konfigurace** a goto **konfigurace** pod **jednotné přihlašování** otevřete jeden znak na nastavení.</span><span class="sxs-lookup"><span data-stu-id="53124-165">In the navigation menu on the top, click **Configuration** and goto **Configure** under **Single Sign On** to open the Single Sign On Settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

9. <span data-ttu-id="53124-167">V **jeden znak v nastavení** formuláři proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="53124-167">In **Single Sign On Settings** form perform the following steps:</span></span>
 
    <span data-ttu-id="53124-168">a.</span><span class="sxs-lookup"><span data-stu-id="53124-168">a.</span></span> <span data-ttu-id="53124-169">Vyberte **název vystavitele** jako Azure AD.</span><span class="sxs-lookup"><span data-stu-id="53124-169">Select **Issuer Name** as Azure AD.</span></span>
    
    <span data-ttu-id="53124-170">b.</span><span class="sxs-lookup"><span data-stu-id="53124-170">b.</span></span> <span data-ttu-id="53124-171">Vložení **SAML Entity ID** který jste zkopírovali z portálu Azure do pole Adresa URL vystavitele.</span><span class="sxs-lookup"><span data-stu-id="53124-171">Paste the **SAML Entity ID** which you have copied from Azure portal into Issuer URL textbox.</span></span>
    
    <span data-ttu-id="53124-172">c.</span><span class="sxs-lookup"><span data-stu-id="53124-172">c.</span></span> <span data-ttu-id="53124-173">Vložení **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure do jednoho přihlášení adresa URL služby textového pole.</span><span class="sxs-lookup"><span data-stu-id="53124-173">Paste the **SAML Single Sign-On Service URL** which you have copied from Azure portal into Single Sign-On Service URL textbox.</span></span>

    <span data-ttu-id="53124-174">d.</span><span class="sxs-lookup"><span data-stu-id="53124-174">d.</span></span> <span data-ttu-id="53124-175">Vložení **Sign-Out URL** který jste zkopírovali z portálu Azure do jedné adresy URL služby Sign-Out textové pole.</span><span class="sxs-lookup"><span data-stu-id="53124-175">Paste the **Sign-Out URL** which you have copied from Azure portal into Single Sign-Out Service URL textbox.</span></span>

    <span data-ttu-id="53124-176">e.</span><span class="sxs-lookup"><span data-stu-id="53124-176">e.</span></span> <span data-ttu-id="53124-177">Vyhledejte certifikát, který jste si stáhli z portálu Azure a sem odeslání.</span><span class="sxs-lookup"><span data-stu-id="53124-177">Browse the certificate which you have downloaded from Azure portal and upload here.</span></span>
    
    <span data-ttu-id="53124-178">f.</span><span class="sxs-lookup"><span data-stu-id="53124-178">f.</span></span> <span data-ttu-id="53124-179">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="53124-179">Click **Save**.</span></span>
  
> [!TIP]
> <span data-ttu-id="53124-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="53124-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="53124-181">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="53124-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="53124-182">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="53124-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="53124-183">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="53124-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="53124-184">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="53124-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="53124-186">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="53124-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="53124-187">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="53124-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="53124-189">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="53124-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="53124-191">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="53124-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="53124-193">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="53124-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="53124-195">a.</span><span class="sxs-lookup"><span data-stu-id="53124-195">a.</span></span> <span data-ttu-id="53124-196">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="53124-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="53124-197">b.</span><span class="sxs-lookup"><span data-stu-id="53124-197">b.</span></span> <span data-ttu-id="53124-198">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="53124-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="53124-199">c.</span><span class="sxs-lookup"><span data-stu-id="53124-199">c.</span></span> <span data-ttu-id="53124-200">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="53124-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="53124-201">d.</span><span class="sxs-lookup"><span data-stu-id="53124-201">d.</span></span> <span data-ttu-id="53124-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="53124-202">Click **Create**.</span></span>
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a><span data-ttu-id="53124-203">Vytvoření zkušebního uživatele SensoScientific bezdrátové teploty monitorování systému</span><span class="sxs-lookup"><span data-stu-id="53124-203">Creating a SensoScientific Wireless Temperature Monitoring System test user</span></span>

<span data-ttu-id="53124-204">Pokud chcete povolit uživatelům Azure AD přihlášení k SensoScientific bezdrátové teploty monitorování systému, musí být zřízená do SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="53124-204">To enable Azure AD users to log in to SensoScientific Wireless Temperature Monitoring System, they must be provisioned into SensoScientific Wireless Temperature Monitoring System.</span></span> <span data-ttu-id="53124-205">Práce s [tým podpory SensoScientific bezdrátové teploty monitorování systému](https://www.sensoscientific.com/contact-us/) přidat uživatele do platformy SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="53124-205">Work with [SensoScientific Wireless Temperature Monitoring System support team](https://www.sensoscientific.com/contact-us/) to add the users in the SensoScientific Wireless Temperature Monitoring System platform.</span></span> <span data-ttu-id="53124-206">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="53124-206">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="53124-207">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="53124-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="53124-208">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="53124-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SensoScientific Wireless Temperature Monitoring System.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="53124-210">**Pokud chcete přiřadit Britta Simon SensoScientific bezdrátové teploty monitorování systému, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="53124-210">**To assign Britta Simon to SensoScientific Wireless Temperature Monitoring System, perform the following steps:**</span></span>

1. <span data-ttu-id="53124-211">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="53124-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="53124-213">V seznamu aplikací vyberte **SensoScientific bezdrátové teploty monitorování systému**.</span><span class="sxs-lookup"><span data-stu-id="53124-213">In the applications list, select **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

3. <span data-ttu-id="53124-215">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="53124-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="53124-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="53124-217">Click **Add** button.</span></span> <span data-ttu-id="53124-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="53124-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="53124-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="53124-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="53124-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="53124-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="53124-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="53124-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="53124-223">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="53124-223">Testing single sign-on</span></span>

<span data-ttu-id="53124-224">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="53124-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> <span data-ttu-id="53124-225">Klikněte na dlaždici SensoScientific bezdrátové teploty monitorování systému na přístupovém panelu, můžete se být automaticky přihlášení k aplikaci SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="53124-225">Click the SensoScientific Wireless Temperature Monitoring System tile in the Access Panel, you will be automatically signed-on to your SensoScientific Wireless Temperature Monitoring System application.</span></span> <span data-ttu-id="53124-226">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="53124-226">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53124-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="53124-227">Additional resources</span></span>

* [<span data-ttu-id="53124-228">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="53124-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="53124-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="53124-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_203.png

