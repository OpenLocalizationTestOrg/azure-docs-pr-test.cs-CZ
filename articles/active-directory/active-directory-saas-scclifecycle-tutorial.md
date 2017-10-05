---
title: "Kurz: Azure Active Directory integrace s životního cyklu SCC | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SCC životního cyklu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 0f8f9d03e8c35109b74088350ef1d68f6b823e8b
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a><span data-ttu-id="549df-103">Kurz: Azure Active Directory integrace s SCC životního cyklu</span><span class="sxs-lookup"><span data-stu-id="549df-103">Tutorial: Azure Active Directory integration with SCC LifeCycle</span></span>

<span data-ttu-id="549df-104">V tomto kurzu zjistěte, jak integrovat životního cyklu SCC s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="549df-104">In this tutorial, you learn how to integrate SCC LifeCycle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="549df-105">Životní cyklus SCC integraci s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="549df-105">Integrating SCC LifeCycle with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="549df-106">Můžete řídit ve službě Azure AD, který má přístup k SCC životního cyklu</span><span class="sxs-lookup"><span data-stu-id="549df-106">You can control in Azure AD who has access to SCC LifeCycle</span></span>
- <span data-ttu-id="549df-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k životního cyklu SCC (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="549df-107">You can enable your users to automatically get signed-on to SCC LifeCycle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="549df-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="549df-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="549df-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="549df-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="549df-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="549df-110">Prerequisites</span></span>

<span data-ttu-id="549df-111">Konfigurace integrace Azure AD s SCC životního cyklu, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="549df-111">To configure Azure AD integration with SCC LifeCycle, you need the following items:</span></span>

- <span data-ttu-id="549df-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="549df-112">An Azure AD subscription</span></span>
- <span data-ttu-id="549df-113">Životního cyklu SCC jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="549df-113">An SCC LifeCycle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="549df-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="549df-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="549df-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="549df-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="549df-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="549df-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="549df-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="549df-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="549df-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="549df-118">Scenario description</span></span>
<span data-ttu-id="549df-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="549df-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="549df-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="549df-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="549df-121">Přidání životního cyklu SCC z Galerie</span><span class="sxs-lookup"><span data-stu-id="549df-121">Adding SCC LifeCycle from the gallery</span></span>
2. <span data-ttu-id="549df-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="549df-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-scc-lifecycle-from-the-gallery"></a><span data-ttu-id="549df-123">Přidání životního cyklu SCC z Galerie</span><span class="sxs-lookup"><span data-stu-id="549df-123">Adding SCC LifeCycle from the gallery</span></span>
<span data-ttu-id="549df-124">Při konfiguraci integrace životního cyklu SCC do služby Azure AD, potřebujete přidat životního cyklu SCC z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="549df-124">To configure the integration of SCC LifeCycle into Azure AD, you need to add SCC LifeCycle from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="549df-125">**Chcete-li přidat životního cyklu SCC z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="549df-125">**To add SCC LifeCycle from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="549df-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="549df-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="549df-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="549df-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="549df-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="549df-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="549df-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="549df-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="549df-133">Do vyhledávacího pole zadejte **životního cyklu SCC**.</span><span class="sxs-lookup"><span data-stu-id="549df-133">In the search box, type **SCC LifeCycle**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_search.png)

5. <span data-ttu-id="549df-135">Na panelu výsledků vyberte **životního cyklu SCC**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="549df-135">In the results panel, select **SCC LifeCycle**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="549df-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="549df-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="549df-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s životního cyklu SCC podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="549df-138">In this section, you configure and test Azure AD single sign-on with SCC LifeCycle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="549df-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v průběhu životního cyklu SCC je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="549df-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SCC LifeCycle is to a user in Azure AD.</span></span> <span data-ttu-id="549df-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatele v průběhu životního cyklu SCC musí navázat.</span><span class="sxs-lookup"><span data-stu-id="549df-140">In other words, a link relationship between an Azure AD user and the related user in SCC LifeCycle needs to be established.</span></span>

<span data-ttu-id="549df-141">V průběhu životního cyklu SCC, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="549df-141">In SCC LifeCycle, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="549df-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SCC životního cyklu, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="549df-142">To configure and test Azure AD single sign-on with SCC LifeCycle, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="549df-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="549df-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="549df-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="549df-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="549df-145">**[Vytváření testovacího uživatele životního cyklu SCC](#creating-an-scc-lifecycle-test-user)**  – Pokud chcete mít protějšek Britta Simon v průběhu životního cyklu SCC propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="549df-145">**[Creating an SCC LifeCycle test user](#creating-an-scc-lifecycle-test-user)** - to have a counterpart of Britta Simon in SCC LifeCycle that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="549df-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="549df-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="549df-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="549df-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="549df-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="549df-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="549df-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="549df-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SCC LifeCycle application.</span></span>

<span data-ttu-id="549df-150">**Ke konfiguraci Azure AD jednotné přihlašování s SCC životního cyklu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="549df-150">**To configure Azure AD single sign-on with SCC LifeCycle, perform the following steps:**</span></span>

1. <span data-ttu-id="549df-151">Na portálu Azure na **životního cyklu SCC** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="549df-151">In the Azure portal, on the **SCC LifeCycle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="549df-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="549df-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_samlbase.png)

3. <span data-ttu-id="549df-155">Na **SCC životního cyklu domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="549df-155">On the **SCC LifeCycle Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_url.png)

    <span data-ttu-id="549df-157">a.</span><span class="sxs-lookup"><span data-stu-id="549df-157">a.</span></span> <span data-ttu-id="549df-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<sub-domain>.scc.com/ic7/welcome/customer/PICTtest.aspx`</span><span class="sxs-lookup"><span data-stu-id="549df-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<sub-domain>.scc.com/ic7/welcome/customer/PICTtest.aspx`</span></span>

    <span data-ttu-id="549df-159">b.</span><span class="sxs-lookup"><span data-stu-id="549df-159">b.</span></span> <span data-ttu-id="549df-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="549df-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|--|
    | `https://bs1.scc.com/<entity>`|
    | `https://lifecycle.scc.com/<entity>`|
    
    > [!NOTE] 
    > <span data-ttu-id="549df-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="549df-161">These values are not real.</span></span> <span data-ttu-id="549df-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="549df-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="549df-163">Obraťte se na [tým podpory SCC životního cyklu klienta](mailto:lifecycle.support@scc.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="549df-163">Contact [SCC LifeCycle Client support team](mailto:lifecycle.support@scc.com) to get these values.</span></span> 
 
4. <span data-ttu-id="549df-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="549df-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_certificate.png) 

5. <span data-ttu-id="549df-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="549df-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="549df-168">Konfigurace jednotného přihlašování na **životního cyklu SCC** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory životního cyklu SCC](mailto:lifecycle.support@scc.com).</span><span class="sxs-lookup"><span data-stu-id="549df-168">To configure single sign-on on **SCC LifeCycle** side, you need to send the downloaded **Metadata XML** to [SCC LifeCycle support team](mailto:lifecycle.support@scc.com).</span></span> <span data-ttu-id="549df-169">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="549df-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

     >[!NOTE]
   ><span data-ttu-id="549df-170">Jednotné přihlašování musí být povoleno tým podpory SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="549df-170">Single sign-on has to be enabled by the SCC LifeCycle support team.</span></span>
   > 
   > 

> [!TIP]
> <span data-ttu-id="549df-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="549df-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="549df-172">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="549df-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="549df-173">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="549df-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="549df-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="549df-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="549df-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="549df-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="549df-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="549df-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="549df-178">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="549df-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="549df-180">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="549df-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="549df-182">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="549df-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="549df-184">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="549df-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="549df-186">a.</span><span class="sxs-lookup"><span data-stu-id="549df-186">a.</span></span> <span data-ttu-id="549df-187">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="549df-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="549df-188">b.</span><span class="sxs-lookup"><span data-stu-id="549df-188">b.</span></span> <span data-ttu-id="549df-189">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="549df-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="549df-190">c.</span><span class="sxs-lookup"><span data-stu-id="549df-190">c.</span></span> <span data-ttu-id="549df-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="549df-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="549df-192">d.</span><span class="sxs-lookup"><span data-stu-id="549df-192">d.</span></span> <span data-ttu-id="549df-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="549df-193">Click **Create**.</span></span>
 
### <a name="creating-an-scc-lifecycle-test-user"></a><span data-ttu-id="549df-194">Vytvoření životního cyklu SCC testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="549df-194">Creating an SCC LifeCycle test user</span></span>

<span data-ttu-id="549df-195">Pokud chcete povolit uživatelům Azure AD přihlášení do SCC životního cyklu, musí být zřízená do SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="549df-195">In order to enable Azure AD users to log into SCC LifeCycle, they must be provisioned into SCC LifeCycle.</span></span> <span data-ttu-id="549df-196">Neexistuje žádná položka akce pro konfiguraci zřizování uživatelů k SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="549df-196">There is no action item for you to configure user provisioning to SCC LifeCycle.</span></span>

<span data-ttu-id="549df-197">Když přiřazený uživatel se pokusí přihlásit SCC životního cyklu, se automaticky vytvoří účet SCC životního cyklu, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="549df-197">When an assigned user tries to log into SCC LifeCycle, an SCC LifeCycle account is automatically created if necessary.</span></span>

> [!NOTE]
    > <span data-ttu-id="549df-198">Držitel účtu Azure Active Directory obdrží e-mailu a dodržuje odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="549df-198">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="549df-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="549df-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="549df-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="549df-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SCC LifeCycle.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="549df-202">**Pokud chcete přiřadit Britta Simon SCC životního cyklu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="549df-202">**To assign Britta Simon to SCC LifeCycle, perform the following steps:**</span></span>

1. <span data-ttu-id="549df-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="549df-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications.**</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="549df-205">V seznamu aplikací vyberte **životního cyklu SCC**.</span><span class="sxs-lookup"><span data-stu-id="549df-205">In the applications list, select **SCC LifeCycle**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_app.png) 

3. <span data-ttu-id="549df-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="549df-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="549df-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="549df-209">Click **Add** button.</span></span> <span data-ttu-id="549df-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="549df-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="549df-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="549df-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="549df-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="549df-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="549df-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="549df-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="549df-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="549df-215">Testing single sign-on</span></span>

<span data-ttu-id="549df-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="549df-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="549df-217">Když kliknete na dlaždici životního cyklu SCC na přístupovém panelu, jste měli získat automaticky přihlášení k SCC životního cyklu aplikace.</span><span class="sxs-lookup"><span data-stu-id="549df-217">When you click the SCC LifeCycle tile in the Access Panel, you should get automatically signed-on to SCC LifeCycle application.</span></span>
<span data-ttu-id="549df-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="549df-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="549df-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="549df-219">Additional resources</span></span>

* [<span data-ttu-id="549df-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="549df-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="549df-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="549df-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scclifecycle-tutorial/tutorial_general_203.png

