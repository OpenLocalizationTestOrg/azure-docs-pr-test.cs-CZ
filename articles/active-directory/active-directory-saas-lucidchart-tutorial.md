---
title: 'Kurz: Azure Active Directory integrace s Lucidchart | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Lucidchart."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2dea669f03c893632c08d30feeb3173efc2d8243
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a><span data-ttu-id="fc20f-103">Kurz: Azure Active Directory integrace s Lucidchart</span><span class="sxs-lookup"><span data-stu-id="fc20f-103">Tutorial: Azure Active Directory integration with Lucidchart</span></span>

<span data-ttu-id="fc20f-104">V tomto kurzu zjistěte, jak integrovat Lucidchart s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fc20f-104">In this tutorial, you learn how to integrate Lucidchart with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fc20f-105">Integrace Lucidchart s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="fc20f-105">Integrating Lucidchart with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fc20f-106">Můžete řídit ve službě Azure AD, který má přístup k Lucidchart</span><span class="sxs-lookup"><span data-stu-id="fc20f-106">You can control in Azure AD who has access to Lucidchart</span></span>
- <span data-ttu-id="fc20f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Lucidchart (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="fc20f-107">You can enable your users to automatically get signed-on to Lucidchart (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fc20f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fc20f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fc20f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fc20f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc20f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fc20f-110">Prerequisites</span></span>

<span data-ttu-id="fc20f-111">Konfigurace integrace Azure AD s Lucidchart, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="fc20f-111">To configure Azure AD integration with Lucidchart, you need the following items:</span></span>

- <span data-ttu-id="fc20f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="fc20f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fc20f-113">Lucidchart jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="fc20f-113">A Lucidchart single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fc20f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fc20f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fc20f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="fc20f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fc20f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="fc20f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fc20f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fc20f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fc20f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="fc20f-118">Scenario description</span></span>
<span data-ttu-id="fc20f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fc20f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fc20f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="fc20f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fc20f-121">Přidání Lucidchart z Galerie</span><span class="sxs-lookup"><span data-stu-id="fc20f-121">Adding Lucidchart from the gallery</span></span>
2. <span data-ttu-id="fc20f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fc20f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lucidchart-from-the-gallery"></a><span data-ttu-id="fc20f-123">Přidání Lucidchart z Galerie</span><span class="sxs-lookup"><span data-stu-id="fc20f-123">Adding Lucidchart from the gallery</span></span>
<span data-ttu-id="fc20f-124">Při konfiguraci integrace Lucidchart do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Lucidchart z galerie.</span><span class="sxs-lookup"><span data-stu-id="fc20f-124">To configure the integration of Lucidchart into Azure AD, you need to add Lucidchart from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fc20f-125">**Pokud chcete přidat Lucidchart z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fc20f-125">**To add Lucidchart from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fc20f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fc20f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fc20f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fc20f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="fc20f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fc20f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="fc20f-133">Do vyhledávacího pole zadejte **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-133">In the search box, type **Lucidchart**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_search.png)

5. <span data-ttu-id="fc20f-135">Na panelu výsledků vyberte **Lucidchart**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fc20f-135">In the results panel, select **Lucidchart**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fc20f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fc20f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fc20f-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lucidchart podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="fc20f-138">In this section, you configure and test Azure AD single sign-on with Lucidchart based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fc20f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Lucidchart je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc20f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lucidchart is to a user in Azure AD.</span></span> <span data-ttu-id="fc20f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Lucidchart musí navázat.</span><span class="sxs-lookup"><span data-stu-id="fc20f-140">In other words, a link relationship between an Azure AD user and the related user in Lucidchart needs to be established.</span></span>

<span data-ttu-id="fc20f-141">V Lucidchart, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="fc20f-141">In Lucidchart, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fc20f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lucidchart, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="fc20f-142">To configure and test Azure AD single sign-on with Lucidchart, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fc20f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="fc20f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fc20f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fc20f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fc20f-145">**[Vytvoření zkušebního uživatele Lucidchart](#creating-a-lucidchart-test-user)**  – Pokud chcete mít protějšek Britta Simon v Lucidchart propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="fc20f-145">**[Creating a Lucidchart test user](#creating-a-lucidchart-test-user)** - to have a counterpart of Britta Simon in Lucidchart that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fc20f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fc20f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fc20f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fc20f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fc20f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fc20f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fc20f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="fc20f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lucidchart application.</span></span>

<span data-ttu-id="fc20f-150">**Ke konfiguraci Azure AD jednotné přihlašování s Lucidchart, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fc20f-150">**To configure Azure AD single sign-on with Lucidchart, perform the following steps:**</span></span>

1. <span data-ttu-id="fc20f-151">Na portálu Azure na **Lucidchart** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-151">In the Azure portal, on the **Lucidchart** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="fc20f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fc20f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_samlbase.png)

3. <span data-ttu-id="fc20f-155">Na **Lucidchart domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fc20f-155">On the **Lucidchart Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_url.png)

    <span data-ttu-id="fc20f-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://chart2.office.lucidchart.com/saml/sso/azure`</span><span class="sxs-lookup"><span data-stu-id="fc20f-157">In the **Sign-on URL** textbox, type a URL as: `https://chart2.office.lucidchart.com/saml/sso/azure`</span></span>

4. <span data-ttu-id="fc20f-158">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="fc20f-158">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_certificate.png) 

5. <span data-ttu-id="fc20f-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fc20f-160">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fc20f-162">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="fc20f-162">In a different web browser window, log into your Lucidchart company site as an administrator.</span></span>

7. <span data-ttu-id="fc20f-163">V nabídce v horní části, klikněte na tlačítko **Team**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-163">In the menu on the top, click **Team**.</span></span>
   
    <span data-ttu-id="fc20f-164">![Tým](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span><span class="sxs-lookup"><span data-stu-id="fc20f-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span></span>

8. <span data-ttu-id="fc20f-165">Klikněte na tlačítko **aplikace \> spravovat SAML**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-165">Click **Applications \> Manage SAML**.</span></span>
   
    <span data-ttu-id="fc20f-166">![Spravovat SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "spravovat SAML")</span><span class="sxs-lookup"><span data-stu-id="fc20f-166">![Manage SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "Manage SAML")</span></span>

9. <span data-ttu-id="fc20f-167">Na **nastavení ověřování SAML** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fc20f-167">On the **SAML Authentication Settings** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="fc20f-168">a.</span><span class="sxs-lookup"><span data-stu-id="fc20f-168">a.</span></span> <span data-ttu-id="fc20f-169">Vyberte **povolit ověřování SAML**a potom klikněte na **volitelné**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-169">Select **Enable SAML Authentication**, and then click **Optional**.</span></span>

    <span data-ttu-id="fc20f-170">![Nastavení ověřování SAML](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "nastavení ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="fc20f-170">![SAML Authentication Settings](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "SAML Authentication Settings")</span></span>
 
    <span data-ttu-id="fc20f-171">b.</span><span class="sxs-lookup"><span data-stu-id="fc20f-171">b.</span></span> <span data-ttu-id="fc20f-172">V **domény** textovému poli, zadejte doménu a pak klikněte na tlačítko **změnit certifikát**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-172">In the **Domain** textbox, type your domain, and then click **Change Certificate**.</span></span>

    <span data-ttu-id="fc20f-173">![Změna certifikátu](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "změna certifikátu")</span><span class="sxs-lookup"><span data-stu-id="fc20f-173">![Change Certificate](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "Change Certificate")</span></span>
 
    <span data-ttu-id="fc20f-174">c.</span><span class="sxs-lookup"><span data-stu-id="fc20f-174">c.</span></span> <span data-ttu-id="fc20f-175">Otevřete váš soubor stažený metadata, kopírovat obsah a vložte ji do **nahrát Metadata** textové pole.</span><span class="sxs-lookup"><span data-stu-id="fc20f-175">Open your downloaded metadata file, copy the content, and then paste it into the **Upload Metadata** textbox.</span></span>

    <span data-ttu-id="fc20f-176">![Nahrát Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "nahrát metadat")</span><span class="sxs-lookup"><span data-stu-id="fc20f-176">![Upload Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "Upload Metadata")</span></span>
 
    <span data-ttu-id="fc20f-177">d.</span><span class="sxs-lookup"><span data-stu-id="fc20f-177">d.</span></span> <span data-ttu-id="fc20f-178">Vyberte **automaticky přidat nové uživatele týmu**a potom klikněte na **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-178">Select **Automatically Add new users to the team**, and then click **Save changes**.</span></span>

    <span data-ttu-id="fc20f-179">![Uložit změny](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "uložit změny")</span><span class="sxs-lookup"><span data-stu-id="fc20f-179">![Save Changes](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "Save Changes")</span></span>

> [!TIP]
> <span data-ttu-id="fc20f-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="fc20f-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fc20f-181">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="fc20f-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fc20f-182">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fc20f-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fc20f-183">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fc20f-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="fc20f-184">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fc20f-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="fc20f-186">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fc20f-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fc20f-187">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fc20f-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fc20f-189">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fc20f-191">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fc20f-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fc20f-193">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fc20f-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fc20f-195">a.</span><span class="sxs-lookup"><span data-stu-id="fc20f-195">a.</span></span> <span data-ttu-id="fc20f-196">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fc20f-197">b.</span><span class="sxs-lookup"><span data-stu-id="fc20f-197">b.</span></span> <span data-ttu-id="fc20f-198">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fc20f-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fc20f-199">c.</span><span class="sxs-lookup"><span data-stu-id="fc20f-199">c.</span></span> <span data-ttu-id="fc20f-200">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fc20f-201">d.</span><span class="sxs-lookup"><span data-stu-id="fc20f-201">d.</span></span> <span data-ttu-id="fc20f-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-202">Click **Create**.</span></span>
 
### <a name="creating-a-lucidchart-test-user"></a><span data-ttu-id="fc20f-203">Vytvoření zkušebního uživatele Lucidchart</span><span class="sxs-lookup"><span data-stu-id="fc20f-203">Creating a Lucidchart test user</span></span>

<span data-ttu-id="fc20f-204">Neexistuje žádná položka akce můžete nakonfigurovat na Lucidchart zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="fc20f-204">There is no action item for you to configure user provisioning to Lucidchart.</span></span>  <span data-ttu-id="fc20f-205">Když přiřazený uživatel se pokusí přihlásit pomocí přístupového panelu Lucidchart, Lucidchart ověří, zda uživatel existuje.</span><span class="sxs-lookup"><span data-stu-id="fc20f-205">When an assigned user tries to log into Lucidchart using the access panel, Lucidchart checks whether the user exists.</span></span>  

<span data-ttu-id="fc20f-206">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="fc20f-206">If there is no user account available yet, it is automatically created by Lucidchart.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fc20f-207">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fc20f-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fc20f-208">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="fc20f-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lucidchart.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="fc20f-210">**Pokud chcete přiřadit Britta Simon Lucidchart, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fc20f-210">**To assign Britta Simon to Lucidchart, perform the following steps:**</span></span>

1. <span data-ttu-id="fc20f-211">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="fc20f-213">V seznamu aplikací vyberte **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-213">In the applications list, select **Lucidchart**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_app.png) 

3. <span data-ttu-id="fc20f-215">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="fc20f-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="fc20f-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fc20f-217">Click **Add** button.</span></span> <span data-ttu-id="fc20f-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fc20f-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="fc20f-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="fc20f-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fc20f-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fc20f-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fc20f-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fc20f-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fc20f-223">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fc20f-223">Testing single sign-on</span></span>

<span data-ttu-id="fc20f-224">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="fc20f-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fc20f-225">Když kliknete na dlaždici Lucidchart na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="fc20f-225">When you click the Lucidchart tile in the Access Panel, you should get automatically signed-on to your Lucidchart application.</span></span>
<span data-ttu-id="fc20f-226">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fc20f-226">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fc20f-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fc20f-227">Additional resources</span></span>

* [<span data-ttu-id="fc20f-228">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fc20f-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fc20f-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fc20f-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_203.png

