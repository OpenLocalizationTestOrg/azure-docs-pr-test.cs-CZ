---
title: 'Kurz: Azure Active Directory integrace s OpsGenie | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a OpsGenie."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: ce63726d2406d2f1415d29786f0ef92ca95b9b90
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a><span data-ttu-id="1e799-103">Kurz: Azure Active Directory integrace s OpsGenie</span><span class="sxs-lookup"><span data-stu-id="1e799-103">Tutorial: Azure Active Directory integration with OpsGenie</span></span>

<span data-ttu-id="1e799-104">V tomto kurzu zjistěte, jak integrovat OpsGenie s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1e799-104">In this tutorial, you learn how to integrate OpsGenie with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1e799-105">Integrace OpsGenie s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1e799-105">Integrating OpsGenie with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1e799-106">Můžete řídit ve službě Azure AD, který má přístup k OpsGenie</span><span class="sxs-lookup"><span data-stu-id="1e799-106">You can control in Azure AD who has access to OpsGenie</span></span>
- <span data-ttu-id="1e799-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k OpsGenie (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="1e799-107">You can enable your users to automatically get signed-on to OpsGenie (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1e799-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1e799-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1e799-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1e799-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e799-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1e799-110">Prerequisites</span></span>

<span data-ttu-id="1e799-111">Konfigurace integrace Azure AD s OpsGenie, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="1e799-111">To configure Azure AD integration with OpsGenie, you need the following items:</span></span>

- <span data-ttu-id="1e799-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1e799-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1e799-113">OpsGenie jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1e799-113">A OpsGenie single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1e799-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1e799-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1e799-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1e799-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1e799-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="1e799-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1e799-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1e799-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1e799-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1e799-118">Scenario description</span></span>
<span data-ttu-id="1e799-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1e799-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1e799-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1e799-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1e799-121">Přidání OpsGenie z Galerie</span><span class="sxs-lookup"><span data-stu-id="1e799-121">Adding OpsGenie from the gallery</span></span>
2. <span data-ttu-id="1e799-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1e799-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-opsgenie-from-the-gallery"></a><span data-ttu-id="1e799-123">Přidání OpsGenie z Galerie</span><span class="sxs-lookup"><span data-stu-id="1e799-123">Adding OpsGenie from the gallery</span></span>
<span data-ttu-id="1e799-124">Při konfiguraci integrace OpsGenie do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS OpsGenie z galerie.</span><span class="sxs-lookup"><span data-stu-id="1e799-124">To configure the integration of OpsGenie into Azure AD, you need to add OpsGenie from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1e799-125">**Pokud chcete přidat OpsGenie z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1e799-125">**To add OpsGenie from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1e799-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1e799-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1e799-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1e799-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1e799-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1e799-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="1e799-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1e799-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="1e799-133">Do vyhledávacího pole zadejte **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="1e799-133">In the search box, type **OpsGenie**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_search.png)

5. <span data-ttu-id="1e799-135">Na panelu výsledků vyberte **OpsGenie**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1e799-135">In the results panel, select **OpsGenie**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1e799-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1e799-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1e799-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s OpsGenie podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="1e799-138">In this section, you configure and test Azure AD single sign-on with OpsGenie based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1e799-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v OpsGenie je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1e799-139">For single sign-on to work, Azure AD needs to know what the counterpart user in OpsGenie is to a user in Azure AD.</span></span> <span data-ttu-id="1e799-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v OpsGenie musí navázat.</span><span class="sxs-lookup"><span data-stu-id="1e799-140">In other words, a link relationship between an Azure AD user and the related user in OpsGenie needs to be established.</span></span>

<span data-ttu-id="1e799-141">V OpsGenie, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="1e799-141">In OpsGenie, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1e799-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s OpsGenie, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="1e799-142">To configure and test Azure AD single sign-on with OpsGenie, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1e799-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="1e799-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1e799-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1e799-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1e799-145">**[Vytvoření zkušebního uživatele OpsGenie](#creating-a-opsgenie-test-user)**  – Pokud chcete mít protějšek Britta Simon v OpsGenie propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="1e799-145">**[Creating a OpsGenie test user](#creating-a-opsgenie-test-user)** - to have a counterpart of Britta Simon in OpsGenie that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1e799-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1e799-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1e799-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1e799-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1e799-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1e799-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1e799-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="1e799-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your OpsGenie application.</span></span>

<span data-ttu-id="1e799-150">**Ke konfiguraci Azure AD jednotné přihlašování s OpsGenie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1e799-150">**To configure Azure AD single sign-on with OpsGenie, perform the following steps:**</span></span>

1. <span data-ttu-id="1e799-151">Na portálu Azure na **OpsGenie** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1e799-151">In the Azure portal, on the **OpsGenie** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="1e799-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1e799-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

3. <span data-ttu-id="1e799-155">Na **OpsGenie domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1e799-155">On the **OpsGenie Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_url.png)

    <span data-ttu-id="1e799-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL:`https://app.opsgenie.com/auth/login`</span><span class="sxs-lookup"><span data-stu-id="1e799-157">In the **Sign-on URL** textbox, type the URL: `https://app.opsgenie.com/auth/login`</span></span>

4. <span data-ttu-id="1e799-158">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="1e799-158">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_certificate.png) 

5. <span data-ttu-id="1e799-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1e799-160">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1e799-162">Na **OpsGenie konfigurace** klikněte na tlačítko **konfigurace OpsGenie** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="1e799-162">On the **OpsGenie Configuration** section, click **Configure OpsGenie** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1e799-163">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="1e799-163">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_configure.png) 

7. <span data-ttu-id="1e799-165">Otevřete jiná instance prohlížeče a pak přihlásit k OpsGenie jako správce.</span><span class="sxs-lookup"><span data-stu-id="1e799-165">Open another browser instance, and then log-in to OpsGenie as an administrator.</span></span>

8. <span data-ttu-id="1e799-166">Klikněte na tlačítko **nastavení**a klikněte **jednotné přihlašování** kartě.</span><span class="sxs-lookup"><span data-stu-id="1e799-166">Click **Settings**, and then click the **Single Sign On** tab.</span></span>
   
    ![OpsGenie jednotné přihlášení](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png)

9. <span data-ttu-id="1e799-168">Chcete-li povolit jednotné přihlašování, vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="1e799-168">To enable SSO, select **Enabled**.</span></span>
   
    ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 

10. <span data-ttu-id="1e799-170">V **zprostředkovatele** klikněte na položku **Azure Active Directory** kartě.</span><span class="sxs-lookup"><span data-stu-id="1e799-170">In the **Provider** section, click the **Azure Active Directory** tab.</span></span>
   
    ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 

11. <span data-ttu-id="1e799-172">Na stránce dialogové okno služby Azure Active Directory proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1e799-172">On the Azure Active Directory dialog page, perform the following steps:</span></span>
   
    ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    <span data-ttu-id="1e799-174">a.</span><span class="sxs-lookup"><span data-stu-id="1e799-174">a.</span></span> <span data-ttu-id="1e799-175">Vložení **jeden znak na adresu URL služby**, který jste zkopírovali z portálu Azure do **SAML 2.0 Endpoint** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1e799-175">Paste **Single Sign On Service URL**, which you have copied from the Azure portal into the **SAML 2.0 Endpoint** textbox.</span></span>
    
    <span data-ttu-id="1e799-176">b.</span><span class="sxs-lookup"><span data-stu-id="1e799-176">b.</span></span> <span data-ttu-id="1e799-177">V poznámkovém bloku otevřete stažené kódovaného certifikátu kódování base-64, zkopírujte obsah ho do schránky a vložte ji do **X.500 certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1e799-177">Open your downloaded base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it into the **X.500 Certificate** textbox.</span></span>
    
    <span data-ttu-id="1e799-178">c.</span><span class="sxs-lookup"><span data-stu-id="1e799-178">c.</span></span> <span data-ttu-id="1e799-179">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="1e799-179">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="1e799-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="1e799-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1e799-181">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="1e799-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1e799-182">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1e799-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1e799-183">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1e799-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="1e799-184">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1e799-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="1e799-186">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1e799-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1e799-187">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1e799-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1e799-189">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1e799-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1e799-191">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1e799-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1e799-193">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1e799-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1e799-195">a.</span><span class="sxs-lookup"><span data-stu-id="1e799-195">a.</span></span> <span data-ttu-id="1e799-196">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1e799-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1e799-197">b.</span><span class="sxs-lookup"><span data-stu-id="1e799-197">b.</span></span> <span data-ttu-id="1e799-198">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1e799-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1e799-199">c.</span><span class="sxs-lookup"><span data-stu-id="1e799-199">c.</span></span> <span data-ttu-id="1e799-200">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="1e799-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1e799-201">d.</span><span class="sxs-lookup"><span data-stu-id="1e799-201">d.</span></span> <span data-ttu-id="1e799-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1e799-202">Click **Create**.</span></span>
 
### <a name="creating-a-opsgenie-test-user"></a><span data-ttu-id="1e799-203">Vytvoření zkušebního uživatele OpsGenie</span><span class="sxs-lookup"><span data-stu-id="1e799-203">Creating a OpsGenie test user</span></span>

<span data-ttu-id="1e799-204">Cílem této části je vytvoření uživatele v OpsGenie nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1e799-204">The objective of this section is to create a user called Britta Simon in OpsGenie.</span></span> 

1. <span data-ttu-id="1e799-205">V okně webového prohlížeče Přihlaste se ke klientovi OpsGenie jako správce.</span><span class="sxs-lookup"><span data-stu-id="1e799-205">In a web browser window, log into your OpsGenie tenant as an administrator.</span></span>

2. <span data-ttu-id="1e799-206">Přejděte do seznamu uživatelů kliknutím **uživatele** v levém panelu.</span><span class="sxs-lookup"><span data-stu-id="1e799-206">Navigate to Users list by clicking **User** in left panel.</span></span>
   
   ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. <span data-ttu-id="1e799-208">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="1e799-208">Click **Add User**.</span></span>

4. <span data-ttu-id="1e799-209">Na **přidat uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1e799-209">On the **Add User** dialog, perform the following steps:</span></span>
   
   ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   <span data-ttu-id="1e799-211">a.</span><span class="sxs-lookup"><span data-stu-id="1e799-211">a.</span></span> <span data-ttu-id="1e799-212">V **e-mailu** textovému poli, typ e-mailovou adresu BrittaSimon řešit v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1e799-212">In the **Email** textbox, type the email address of BrittaSimon addressed in Azure Active Directory.</span></span>
   
   <span data-ttu-id="1e799-213">b.</span><span class="sxs-lookup"><span data-stu-id="1e799-213">b.</span></span> <span data-ttu-id="1e799-214">V **úplný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="1e799-214">In the **Full Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="1e799-215">c.</span><span class="sxs-lookup"><span data-stu-id="1e799-215">c.</span></span> <span data-ttu-id="1e799-216">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1e799-216">Click **Save**.</span></span> 

>[!NOTE]
><span data-ttu-id="1e799-217">Britta získá e-mail s pokyny k nastavení svůj profil.</span><span class="sxs-lookup"><span data-stu-id="1e799-217">Britta gets an email with instructions for setting up her profile.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1e799-218">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1e799-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1e799-219">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="1e799-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to OpsGenie.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="1e799-221">**Pokud chcete přiřadit Britta Simon OpsGenie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1e799-221">**To assign Britta Simon to OpsGenie, perform the following steps:**</span></span>

1. <span data-ttu-id="1e799-222">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1e799-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1e799-224">V seznamu aplikací vyberte **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="1e799-224">In the applications list, select **OpsGenie**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_app.png) 

3. <span data-ttu-id="1e799-226">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1e799-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="1e799-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1e799-228">Click **Add** button.</span></span> <span data-ttu-id="1e799-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1e799-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="1e799-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1e799-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1e799-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1e799-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1e799-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1e799-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1e799-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1e799-234">Testing single sign-on</span></span>

<span data-ttu-id="1e799-235">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="1e799-235">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="1e799-236">Když kliknete na dlaždici OpsGenie na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="1e799-236">When you click the OpsGenie tile in the Access Panel, you should get automatically signed-on to your OpsGenie application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e799-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1e799-237">Additional resources</span></span>

* [<span data-ttu-id="1e799-238">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1e799-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1e799-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1e799-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png

