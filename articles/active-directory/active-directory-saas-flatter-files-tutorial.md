---
title: "Kurz: Azure Active Directory integrace s plošší soubory | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a plošší soubory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: e02150cb27768d7b403bdca191bc1f189821def4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a><span data-ttu-id="328f6-103">Kurz: Azure Active Directory integrace s plošší soubory</span><span class="sxs-lookup"><span data-stu-id="328f6-103">Tutorial: Azure Active Directory integration with Flatter Files</span></span>

<span data-ttu-id="328f6-104">V tomto kurzu zjistěte, jak integrovat plošší soubory s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="328f6-104">In this tutorial, you learn how to integrate Flatter Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="328f6-105">Integrace plošší soubory s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="328f6-105">Integrating Flatter Files with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="328f6-106">Můžete řídit ve službě Azure AD, kdo má přístup k souborům plošší</span><span class="sxs-lookup"><span data-stu-id="328f6-106">You can control in Azure AD who has access to Flatter Files</span></span>
- <span data-ttu-id="328f6-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k plošší soubory (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="328f6-107">You can enable your users to automatically get signed-on to Flatter Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="328f6-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="328f6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="328f6-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="328f6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="328f6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="328f6-110">Prerequisites</span></span>

<span data-ttu-id="328f6-111">Konfigurace integrace Azure AD s plošší soubory, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="328f6-111">To configure Azure AD integration with Flatter Files, you need the following items:</span></span>

- <span data-ttu-id="328f6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="328f6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="328f6-113">Plošší soubory jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="328f6-113">A Flatter Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="328f6-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="328f6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="328f6-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="328f6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="328f6-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="328f6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="328f6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="328f6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="328f6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="328f6-118">Scenario description</span></span>
<span data-ttu-id="328f6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="328f6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="328f6-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="328f6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="328f6-121">Přidávání plošší souborů z Galerie</span><span class="sxs-lookup"><span data-stu-id="328f6-121">Adding Flatter Files from the gallery</span></span>
2. <span data-ttu-id="328f6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="328f6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-flatter-files-from-the-gallery"></a><span data-ttu-id="328f6-123">Přidávání plošší souborů z Galerie</span><span class="sxs-lookup"><span data-stu-id="328f6-123">Adding Flatter Files from the gallery</span></span>
<span data-ttu-id="328f6-124">Při konfiguraci integrace plošší souborů do služby Azure AD, musíte přidat plošší soubory z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="328f6-124">To configure the integration of Flatter Files into Azure AD, you need to add Flatter Files from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="328f6-125">**Pokud chcete přidat plošší soubory z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="328f6-125">**To add Flatter Files from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="328f6-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="328f6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="328f6-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="328f6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="328f6-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="328f6-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="328f6-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="328f6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="328f6-133">Do vyhledávacího pole zadejte **plošší soubory**.</span><span class="sxs-lookup"><span data-stu-id="328f6-133">In the search box, type **Flatter Files**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_search.png)

5. <span data-ttu-id="328f6-135">Na panelu výsledků vyberte **plošší soubory**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="328f6-135">In the results panel, select **Flatter Files**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="328f6-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="328f6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="328f6-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s plošší soubory podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="328f6-138">In this section, you configure and test Azure AD single sign-on with Flatter Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="328f6-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v souborech plošší je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="328f6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Flatter Files is to a user in Azure AD.</span></span> <span data-ttu-id="328f6-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v souborech plošší musí navázat.</span><span class="sxs-lookup"><span data-stu-id="328f6-140">In other words, a link relationship between an Azure AD user and the related user in Flatter Files needs to be established.</span></span>

<span data-ttu-id="328f6-141">V souborech plošší, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="328f6-141">In Flatter Files, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="328f6-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování se plošší soubory, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="328f6-142">To configure and test Azure AD single sign-on with Flatter Files, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="328f6-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="328f6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="328f6-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="328f6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="328f6-145">**[Vytvoření zkušebního uživatele plošší soubory](#creating-a-flatter-files-test-user)**  – Pokud chcete mít protějšek Britta Simon v plošší soubory propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="328f6-145">**[Creating a Flatter Files test user](#creating-a-flatter-files-test-user)** - to have a counterpart of Britta Simon in Flatter Files that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="328f6-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="328f6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="328f6-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="328f6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="328f6-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="328f6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="328f6-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci plošší soubory.</span><span class="sxs-lookup"><span data-stu-id="328f6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Flatter Files application.</span></span>

<span data-ttu-id="328f6-150">**Ke konfiguraci Azure AD jednotné přihlašování se plošší soubory, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="328f6-150">**To configure Azure AD single sign-on with Flatter Files, perform the following steps:**</span></span>

1. <span data-ttu-id="328f6-151">Na portálu Azure na **plošší soubory** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="328f6-151">In the Azure portal, on the **Flatter Files** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="328f6-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="328f6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

3. <span data-ttu-id="328f6-155">Na **plošší soubory domény a adresy URL** části uživatel nemusí provádět žádné kroky, protože aplikace je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="328f6-155">On the **Flatter Files Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
4. <span data-ttu-id="328f6-157">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="328f6-157">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

5. <span data-ttu-id="328f6-159">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="328f6-159">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="328f6-161">Na **plošší soubory konfigurace** klikněte na tlačítko **konfigurace plošší soubory** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="328f6-161">On the **Flatter Files Configuration** section, click **Configure Flatter Files** to open **Configure sign-on** window.</span></span> <span data-ttu-id="328f6-162">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="328f6-162">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

7. <span data-ttu-id="328f6-164">Přihlášení do aplikace plošší soubory jako správce.</span><span class="sxs-lookup"><span data-stu-id="328f6-164">Sign-on to your Flatter Files application as an administrator.</span></span>

8. <span data-ttu-id="328f6-165">Klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="328f6-165">Click **DASHBOARD**.</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  

9. <span data-ttu-id="328f6-167">Klikněte na tlačítko **nastavení**a poté proveďte následující kroky na **společnosti** karty:</span><span class="sxs-lookup"><span data-stu-id="328f6-167">Click **Settings**, and then perform the following steps on the **Company** tab:</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    <span data-ttu-id="328f6-169">a.</span><span class="sxs-lookup"><span data-stu-id="328f6-169">a.</span></span> <span data-ttu-id="328f6-170">Vyberte **pomocí SAML 2.0 pro ověřování**.</span><span class="sxs-lookup"><span data-stu-id="328f6-170">Select **Use SAML 2.0 for Authentication**.</span></span>
    
    <span data-ttu-id="328f6-171">b.</span><span class="sxs-lookup"><span data-stu-id="328f6-171">b.</span></span> <span data-ttu-id="328f6-172">Klikněte na tlačítko **konfigurace SAML**.</span><span class="sxs-lookup"><span data-stu-id="328f6-172">Click **Configure SAML**.</span></span>

8. <span data-ttu-id="328f6-173">Na **konfigurace SAML** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="328f6-173">On the **SAML Configuration** dialog, perform the following steps:</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    <span data-ttu-id="328f6-175">a.</span><span class="sxs-lookup"><span data-stu-id="328f6-175">a.</span></span> <span data-ttu-id="328f6-176">V **domény** textovému poli, zadejte registrované domény.</span><span class="sxs-lookup"><span data-stu-id="328f6-176">In the **Domain** textbox, type your registered domain.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="328f6-177">Pokud nemáte registrované domény ještě nebyla, obraťte se na podporu plošší soubory team prostřednictvím [ support@flatterfiles.com ](mailto:support@flatterfiles.com).</span><span class="sxs-lookup"><span data-stu-id="328f6-177">If you don't have a registered domain yet, contact your Flatter Files support team via [support@flatterfiles.com](mailto:support@flatterfiles.com).</span></span> 
    
    <span data-ttu-id="328f6-178">b.</span><span class="sxs-lookup"><span data-stu-id="328f6-178">b.</span></span> <span data-ttu-id="328f6-179">V **adresa URL poskytovatele Identity** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali formuláři portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="328f6-179">In **Identity Provider URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied form Azure portal.</span></span>
   
    <span data-ttu-id="328f6-180">c.</span><span class="sxs-lookup"><span data-stu-id="328f6-180">c.</span></span>  <span data-ttu-id="328f6-181">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte jej do **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="328f6-181">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="328f6-182">d.</span><span class="sxs-lookup"><span data-stu-id="328f6-182">d.</span></span> <span data-ttu-id="328f6-183">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="328f6-183">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="328f6-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="328f6-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="328f6-185">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="328f6-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="328f6-186">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="328f6-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="328f6-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="328f6-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="328f6-188">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="328f6-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="328f6-190">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="328f6-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="328f6-191">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="328f6-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="328f6-193">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="328f6-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="328f6-195">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="328f6-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="328f6-197">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="328f6-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="328f6-199">a.</span><span class="sxs-lookup"><span data-stu-id="328f6-199">a.</span></span> <span data-ttu-id="328f6-200">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="328f6-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="328f6-201">b.</span><span class="sxs-lookup"><span data-stu-id="328f6-201">b.</span></span> <span data-ttu-id="328f6-202">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="328f6-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="328f6-203">c.</span><span class="sxs-lookup"><span data-stu-id="328f6-203">c.</span></span> <span data-ttu-id="328f6-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="328f6-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="328f6-205">d.</span><span class="sxs-lookup"><span data-stu-id="328f6-205">d.</span></span> <span data-ttu-id="328f6-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="328f6-206">Click **Create**.</span></span>
 
### <a name="creating-a-flatter-files-test-user"></a><span data-ttu-id="328f6-207">Vytvoření zkušebního uživatele plošší soubory</span><span class="sxs-lookup"><span data-stu-id="328f6-207">Creating a Flatter Files test user</span></span>

<span data-ttu-id="328f6-208">Cílem této části je vytvoření uživatele volal Britta Simon v plošší soubory.</span><span class="sxs-lookup"><span data-stu-id="328f6-208">The objective of this section is to create a user called Britta Simon in Flatter Files.</span></span>

<span data-ttu-id="328f6-209">**Vytvoření uživatele volal Britta Simon v plošší soubory, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="328f6-209">**To create a user called Britta Simon in Flatter Files, perform the following steps:**</span></span>

1. <span data-ttu-id="328f6-210">Přihlaste se k vaší **plošší soubory** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="328f6-210">Sign on to your **Flatter Files** company site as administrator.</span></span>

2. <span data-ttu-id="328f6-211">V navigačním podokně na levé straně klikněte na **nastavení**a klikněte **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="328f6-211">In the navigation pane on the left, click **Settings**, and then click the **Users** tab.</span></span>
   
    ![Vytvoření plošší soubory uživatele](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. <span data-ttu-id="328f6-213">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="328f6-213">Click **Add User**.</span></span> 

4. <span data-ttu-id="328f6-214">Na **přidat uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="328f6-214">On the **Add User** dialog, perform the following steps:</span></span>
   
    ![Vytvoření plošší soubory uživatele](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    <span data-ttu-id="328f6-216">a.</span><span class="sxs-lookup"><span data-stu-id="328f6-216">a.</span></span> <span data-ttu-id="328f6-217">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="328f6-217">In the **First Name** textbox, type **Britta**.</span></span>
   
    <span data-ttu-id="328f6-218">b.</span><span class="sxs-lookup"><span data-stu-id="328f6-218">b.</span></span> <span data-ttu-id="328f6-219">V **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="328f6-219">In the **Last Name** textbox, type **Simon**.</span></span> 
   
    <span data-ttu-id="328f6-220">c.</span><span class="sxs-lookup"><span data-stu-id="328f6-220">c.</span></span> <span data-ttu-id="328f6-221">V **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu Britta na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="328f6-221">In the **Email Address** textbox, type Britta's email address in the Azure portal.</span></span>
   
    <span data-ttu-id="328f6-222">d.</span><span class="sxs-lookup"><span data-stu-id="328f6-222">d.</span></span> <span data-ttu-id="328f6-223">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="328f6-223">Click **Submit**.</span></span>   


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="328f6-224">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="328f6-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="328f6-225">V této části povolíte Britta Simon používat tak, že udělíte přístup k souborům plošší Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="328f6-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Flatter Files.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="328f6-227">**Pokud chcete přiřadit Britta Simon plošší soubory, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="328f6-227">**To assign Britta Simon to Flatter Files, perform the following steps:**</span></span>

1. <span data-ttu-id="328f6-228">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="328f6-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="328f6-230">V seznamu aplikací vyberte **plošší soubory**.</span><span class="sxs-lookup"><span data-stu-id="328f6-230">In the applications list, select **Flatter Files**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_app.png) 

3. <span data-ttu-id="328f6-232">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="328f6-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="328f6-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="328f6-234">Click **Add** button.</span></span> <span data-ttu-id="328f6-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="328f6-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="328f6-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="328f6-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="328f6-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="328f6-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="328f6-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="328f6-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="328f6-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="328f6-240">Testing single sign-on</span></span>

<span data-ttu-id="328f6-241">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="328f6-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="328f6-242">Když kliknete na dlaždici plošší soubory na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci plošší soubory.</span><span class="sxs-lookup"><span data-stu-id="328f6-242">When you click the Flatter Files tile in the Access Panel, you should get automatically signed-on to your Flatter Files application.</span></span>
<span data-ttu-id="328f6-243">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="328f6-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="328f6-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="328f6-244">Additional resources</span></span>

* [<span data-ttu-id="328f6-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="328f6-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="328f6-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="328f6-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png

