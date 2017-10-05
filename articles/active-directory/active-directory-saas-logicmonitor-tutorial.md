---
title: 'Kurz: Azure Active Directory integrace s LogicMonitor | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a LogicMonitor."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: e49960cac868f80af3e9165a9f75e49be87515f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a><span data-ttu-id="e3007-103">Kurz: Azure Active Directory integrace s LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="e3007-103">Tutorial: Azure Active Directory integration with LogicMonitor</span></span>

<span data-ttu-id="e3007-104">V tomto kurzu zjistěte, jak integrovat LogicMonitor s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e3007-104">In this tutorial, you learn how to integrate LogicMonitor with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e3007-105">Integrace LogicMonitor s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e3007-105">Integrating LogicMonitor with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e3007-106">Můžete řídit ve službě Azure AD, který má přístup k LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="e3007-106">You can control in Azure AD who has access to LogicMonitor</span></span>
- <span data-ttu-id="e3007-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k LogicMonitor (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e3007-107">You can enable your users to automatically get signed-on to LogicMonitor (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e3007-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e3007-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e3007-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e3007-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3007-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e3007-110">Prerequisites</span></span>

<span data-ttu-id="e3007-111">Konfigurace integrace Azure AD s LogicMonitor, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e3007-111">To configure Azure AD integration with LogicMonitor, you need the following items:</span></span>

- <span data-ttu-id="e3007-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e3007-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e3007-113">LogicMonitor jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e3007-113">A LogicMonitor single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e3007-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e3007-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e3007-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e3007-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e3007-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e3007-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e3007-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e3007-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e3007-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e3007-118">Scenario description</span></span>
<span data-ttu-id="e3007-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e3007-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e3007-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e3007-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e3007-121">Přidání LogicMonitor z Galerie</span><span class="sxs-lookup"><span data-stu-id="e3007-121">Adding LogicMonitor from the gallery</span></span>
2. <span data-ttu-id="e3007-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e3007-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-logicmonitor-from-the-gallery"></a><span data-ttu-id="e3007-123">Přidání LogicMonitor z Galerie</span><span class="sxs-lookup"><span data-stu-id="e3007-123">Adding LogicMonitor from the gallery</span></span>
<span data-ttu-id="e3007-124">Při konfiguraci integrace LogicMonitor do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS LogicMonitor z galerie.</span><span class="sxs-lookup"><span data-stu-id="e3007-124">To configure the integration of LogicMonitor into Azure AD, you need to add LogicMonitor from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e3007-125">**Pokud chcete přidat LogicMonitor z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e3007-125">**To add LogicMonitor from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e3007-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e3007-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e3007-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e3007-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e3007-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e3007-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e3007-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e3007-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e3007-133">Do vyhledávacího pole zadejte **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="e3007-133">In the search box, type **LogicMonitor**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_search.png)

5. <span data-ttu-id="e3007-135">Na panelu výsledků vyberte **LogicMonitor**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e3007-135">In the results panel, select **LogicMonitor**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e3007-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e3007-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e3007-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LogicMonitor podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e3007-138">In this section, you configure and test Azure AD single sign-on with LogicMonitor based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e3007-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v LogicMonitor je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e3007-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LogicMonitor is to a user in Azure AD.</span></span> <span data-ttu-id="e3007-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v LogicMonitor musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e3007-140">In other words, a link relationship between an Azure AD user and the related user in LogicMonitor needs to be established.</span></span>

<span data-ttu-id="e3007-141">V LogicMonitor, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="e3007-141">In LogicMonitor, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e3007-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s LogicMonitor, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e3007-142">To configure and test Azure AD single sign-on with LogicMonitor, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e3007-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e3007-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e3007-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e3007-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e3007-145">**[Vytvoření zkušebního uživatele LogicMonitor](#creating-a-logicmonitor-test-user)**  – Pokud chcete mít protějšek Britta Simon v LogicMonitor propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e3007-145">**[Creating a LogicMonitor test user](#creating-a-logicmonitor-test-user)** - to have a counterpart of Britta Simon in LogicMonitor that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e3007-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e3007-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e3007-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e3007-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e3007-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e3007-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e3007-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci LogicMonitor.</span><span class="sxs-lookup"><span data-stu-id="e3007-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LogicMonitor application.</span></span>

<span data-ttu-id="e3007-150">**Ke konfiguraci Azure AD jednotné přihlašování s LogicMonitor, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e3007-150">**To configure Azure AD single sign-on with LogicMonitor, perform the following steps:**</span></span>

1. <span data-ttu-id="e3007-151">Na portálu Azure na **LogicMonitor** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e3007-151">In the Azure portal, on the **LogicMonitor** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e3007-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e3007-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_samlbase.png)

3. <span data-ttu-id="e3007-155">Na **LogicMonitor domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e3007-155">On the **LogicMonitor Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_url.png)

    <span data-ttu-id="e3007-157">a.</span><span class="sxs-lookup"><span data-stu-id="e3007-157">a.</span></span> <span data-ttu-id="e3007-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="e3007-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    <span data-ttu-id="e3007-159">b.</span><span class="sxs-lookup"><span data-stu-id="e3007-159">b.</span></span> <span data-ttu-id="e3007-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="e3007-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e3007-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e3007-161">These values are not real.</span></span> <span data-ttu-id="e3007-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e3007-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e3007-163">Obraťte se na [tým podpory LogicMonitor klienta](https://www.logicmonitor.com/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="e3007-163">Contact [LogicMonitor Client support team](https://www.logicmonitor.com/contact/) to get these values.</span></span> 
 


4. <span data-ttu-id="e3007-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e3007-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_certificate.png) 

5. <span data-ttu-id="e3007-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e3007-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e3007-168">Přihlaste se k vaší **LogicMonitor** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="e3007-168">Log in to your **LogicMonitor** company site as an administrator.</span></span>

7. <span data-ttu-id="e3007-169">V nabídce v horní části, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="e3007-169">In the menu on the top, click **Settings**.</span></span>
   
   <span data-ttu-id="e3007-170">![Nastavení](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="e3007-170">![Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "Settings")</span></span>

8. <span data-ttu-id="e3007-171">V navigačním bat na levé straně, klikněte na tlačítko **jednotné přihlašování**</span><span class="sxs-lookup"><span data-stu-id="e3007-171">In the navigation bat on the left side, click **Single Sign On**</span></span>
   
   <span data-ttu-id="e3007-172">![Jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e3007-172">![Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "Single Sign-On")</span></span>

9. <span data-ttu-id="e3007-173">V **jednotné přihlašování (SSO) nastavení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e3007-173">In the **Single Sign-on (SSO) settings** section, perform the following steps:</span></span>
   
   <span data-ttu-id="e3007-174">![Jednotné přihlašování v nastavení](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="e3007-174">![Single Sign-On Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="e3007-175">a.</span><span class="sxs-lookup"><span data-stu-id="e3007-175">a.</span></span> <span data-ttu-id="e3007-176">Vyberte **povolit jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e3007-176">Select **Enable Single Sign-on**.</span></span>

   <span data-ttu-id="e3007-177">b.</span><span class="sxs-lookup"><span data-stu-id="e3007-177">b.</span></span> <span data-ttu-id="e3007-178">Jako **výchozí přiřazení Role**, vyberte **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="e3007-178">As **Default Role Assignment**, select **readonly**.</span></span>
   
   <span data-ttu-id="e3007-179">c.</span><span class="sxs-lookup"><span data-stu-id="e3007-179">c.</span></span> <span data-ttu-id="e3007-180">V poznámkovém bloku otevřete soubor stažený metadat a vložte obsah souboru do **metadat zprostředkovatelů Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e3007-180">Open the downloaded metadata file in notepad, and then paste content of the file into the **Identity Provider Metadata** textbox.</span></span>
   
   <span data-ttu-id="e3007-181">d.</span><span class="sxs-lookup"><span data-stu-id="e3007-181">d.</span></span> <span data-ttu-id="e3007-182">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="e3007-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="e3007-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e3007-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e3007-184">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e3007-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e3007-185">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e3007-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e3007-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e3007-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="e3007-187">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e3007-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e3007-189">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e3007-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e3007-190">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e3007-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e3007-192">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e3007-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e3007-194">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e3007-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e3007-196">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e3007-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e3007-198">a.</span><span class="sxs-lookup"><span data-stu-id="e3007-198">a.</span></span> <span data-ttu-id="e3007-199">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e3007-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e3007-200">b.</span><span class="sxs-lookup"><span data-stu-id="e3007-200">b.</span></span> <span data-ttu-id="e3007-201">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e3007-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e3007-202">c.</span><span class="sxs-lookup"><span data-stu-id="e3007-202">c.</span></span> <span data-ttu-id="e3007-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e3007-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e3007-204">d.</span><span class="sxs-lookup"><span data-stu-id="e3007-204">d.</span></span> <span data-ttu-id="e3007-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e3007-205">Click **Create**.</span></span>
 
### <a name="creating-a-logicmonitor-test-user"></a><span data-ttu-id="e3007-206">Vytvoření zkušebního uživatele LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="e3007-206">Creating a LogicMonitor test user</span></span>

<span data-ttu-id="e3007-207">AAD uživatelé moci přihlásit musí být zřízená do LogicMonitor aplikace pomocí jejich názvy uživatele Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e3007-207">For AAD users to be able to sign in, they must be provisioned to the LogicMonitor application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="e3007-208">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e3007-208">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="e3007-209">Přihlaste se k serveru vaší společnosti LogicMonitor jako správce.</span><span class="sxs-lookup"><span data-stu-id="e3007-209">Log in to your LogicMonitor company site as an administrator.</span></span>

2. <span data-ttu-id="e3007-210">V nabídce v horní části, klikněte na tlačítko **nastavení**a potom klikněte na **role a uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e3007-210">In the menu on the top, click **Settings**, and then click **Roles and Users**.</span></span>
   
   <span data-ttu-id="e3007-211">![Role a uživatelé](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "rolí a uživatelů")</span><span class="sxs-lookup"><span data-stu-id="e3007-211">![Roles and Users](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "Roles and Users")</span></span>

3. <span data-ttu-id="e3007-212">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="e3007-212">Click **Add**.</span></span>

4. <span data-ttu-id="e3007-213">V **přidat účet** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e3007-213">In the **Add an account** section, perform the following steps:</span></span>
   
   <span data-ttu-id="e3007-214">![Přidat účet](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "přidat účet")</span><span class="sxs-lookup"><span data-stu-id="e3007-214">![Add an account](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "Add an account")</span></span>
   
   <span data-ttu-id="e3007-215">a.</span><span class="sxs-lookup"><span data-stu-id="e3007-215">a.</span></span> <span data-ttu-id="e3007-216">Typ **uživatelské jméno**, **e-mailu**, **heslo**, a **znovu zadejte heslo** hodnoty uživatele Azure Active Directory, které chcete zřídit do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="e3007-216">Type the **Username**, **Email**, **Password**, and **Retype password** values of the Azure Active Directory user you want to provision into the related textboxes.</span></span>
   
   <span data-ttu-id="e3007-217">b.</span><span class="sxs-lookup"><span data-stu-id="e3007-217">b.</span></span> <span data-ttu-id="e3007-218">Vyberte **role**, **zobrazit oprávnění**a **stav**.</span><span class="sxs-lookup"><span data-stu-id="e3007-218">Select **Roles**, **View Permissions**, and the **Status**.</span></span>
   
   <span data-ttu-id="e3007-219">c.</span><span class="sxs-lookup"><span data-stu-id="e3007-219">c.</span></span> <span data-ttu-id="e3007-220">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="e3007-220">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="e3007-221">Můžete použít všechny ostatní LogicMonitor uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované LogicMonitor zřídit služby Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="e3007-221">You can use any other LogicMonitor user account creation tools or APIs provided by LogicMonitor to provision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e3007-222">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e3007-222">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e3007-223">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu LogicMonitor.</span><span class="sxs-lookup"><span data-stu-id="e3007-223">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LogicMonitor.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e3007-225">**Pokud chcete přiřadit Britta Simon LogicMonitor, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e3007-225">**To assign Britta Simon to LogicMonitor, perform the following steps:**</span></span>

1. <span data-ttu-id="e3007-226">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e3007-226">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e3007-228">V seznamu aplikací vyberte **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="e3007-228">In the applications list, select **LogicMonitor**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_app.png) 

3. <span data-ttu-id="e3007-230">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e3007-230">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e3007-232">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e3007-232">Click **Add** button.</span></span> <span data-ttu-id="e3007-233">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e3007-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e3007-235">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e3007-235">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e3007-236">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e3007-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e3007-237">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e3007-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e3007-238">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e3007-238">Testing single sign-on</span></span>

<span data-ttu-id="e3007-239">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e3007-239">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="e3007-240">Když kliknete na dlaždici LogicMonitor na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci LogicMonitor.</span><span class="sxs-lookup"><span data-stu-id="e3007-240">When you click the LogicMonitor tile in the Access Panel, you should get automatically signed-on to your LogicMonitor application.</span></span>
<span data-ttu-id="e3007-241">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e3007-241">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e3007-242">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e3007-242">Additional resources</span></span>

* [<span data-ttu-id="e3007-243">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e3007-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e3007-244">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e3007-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_203.png

