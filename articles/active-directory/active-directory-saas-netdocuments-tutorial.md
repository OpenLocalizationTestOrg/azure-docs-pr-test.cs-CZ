---
title: 'Kurz: Azure Active Directory integrace s NetDocuments | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a NetDocuments."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 87c3338d611daa837aa5f079c4b68e0e6fc58455
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a><span data-ttu-id="8aa86-103">Kurz: Azure Active Directory integrace s NetDocuments</span><span class="sxs-lookup"><span data-stu-id="8aa86-103">Tutorial: Azure Active Directory integration with NetDocuments</span></span>

<span data-ttu-id="8aa86-104">V tomto kurzu zjistěte, jak integrovat NetDocuments s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8aa86-104">In this tutorial, you learn how to integrate NetDocuments with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8aa86-105">Integrace NetDocuments s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8aa86-105">Integrating NetDocuments with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8aa86-106">Můžete řídit ve službě Azure AD, který má přístup k NetDocuments</span><span class="sxs-lookup"><span data-stu-id="8aa86-106">You can control in Azure AD who has access to NetDocuments</span></span>
- <span data-ttu-id="8aa86-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k NetDocuments (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8aa86-107">You can enable your users to automatically get signed-on to NetDocuments (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8aa86-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8aa86-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8aa86-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8aa86-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8aa86-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8aa86-110">Prerequisites</span></span>

<span data-ttu-id="8aa86-111">Konfigurace integrace Azure AD s NetDocuments, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="8aa86-111">To configure Azure AD integration with NetDocuments, you need the following items:</span></span>

- <span data-ttu-id="8aa86-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8aa86-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8aa86-113">NetDocuments jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8aa86-113">A NetDocuments single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8aa86-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8aa86-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8aa86-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8aa86-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8aa86-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8aa86-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8aa86-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8aa86-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8aa86-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8aa86-118">Scenario description</span></span>
<span data-ttu-id="8aa86-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8aa86-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8aa86-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8aa86-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8aa86-121">Přidání NetDocuments z Galerie</span><span class="sxs-lookup"><span data-stu-id="8aa86-121">Adding NetDocuments from the gallery</span></span>
2. <span data-ttu-id="8aa86-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8aa86-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netdocuments-from-the-gallery"></a><span data-ttu-id="8aa86-123">Přidání NetDocuments z Galerie</span><span class="sxs-lookup"><span data-stu-id="8aa86-123">Adding NetDocuments from the gallery</span></span>
<span data-ttu-id="8aa86-124">Při konfiguraci integrace NetDocuments do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS NetDocuments z galerie.</span><span class="sxs-lookup"><span data-stu-id="8aa86-124">To configure the integration of NetDocuments into Azure AD, you need to add NetDocuments from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8aa86-125">**Pokud chcete přidat NetDocuments z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8aa86-125">**To add NetDocuments from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8aa86-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8aa86-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8aa86-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8aa86-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8aa86-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8aa86-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8aa86-133">Do vyhledávacího pole zadejte **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-133">In the search box, type **NetDocuments**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_search.png)

5. <span data-ttu-id="8aa86-135">Na panelu výsledků vyberte **NetDocuments**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8aa86-135">In the results panel, select **NetDocuments**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8aa86-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8aa86-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8aa86-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s NetDocuments podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="8aa86-138">In this section, you configure and test Azure AD single sign-on with NetDocuments based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8aa86-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v NetDocuments je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8aa86-139">For single sign-on to work, Azure AD needs to know what the counterpart user in NetDocuments is to a user in Azure AD.</span></span> <span data-ttu-id="8aa86-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v NetDocuments musí navázat.</span><span class="sxs-lookup"><span data-stu-id="8aa86-140">In other words, a link relationship between an Azure AD user and the related user in NetDocuments needs to be established.</span></span>

<span data-ttu-id="8aa86-141">V NetDocuments, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="8aa86-141">In NetDocuments, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8aa86-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s NetDocuments, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="8aa86-142">To configure and test Azure AD single sign-on with NetDocuments, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8aa86-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="8aa86-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8aa86-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8aa86-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8aa86-145">**[Vytvoření zkušebního uživatele NetDocuments](#creating-a-netdocuments-test-user)**  – Pokud chcete mít protějšek Britta Simon v NetDocuments propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="8aa86-145">**[Creating a NetDocuments test user](#creating-a-netdocuments-test-user)** - to have a counterpart of Britta Simon in NetDocuments that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8aa86-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8aa86-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8aa86-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8aa86-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8aa86-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8aa86-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8aa86-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="8aa86-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your NetDocuments application.</span></span>

<span data-ttu-id="8aa86-150">**Ke konfiguraci Azure AD jednotné přihlašování s NetDocuments, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8aa86-150">**To configure Azure AD single sign-on with NetDocuments, perform the following steps:**</span></span>

1. <span data-ttu-id="8aa86-151">Na portálu Azure na **NetDocuments** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-151">In the Azure portal, on the **NetDocuments** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8aa86-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8aa86-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

3. <span data-ttu-id="8aa86-155">Na **NetDocuments domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8aa86-155">On the **NetDocuments Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_url.png)

    <span data-ttu-id="8aa86-157">a.</span><span class="sxs-lookup"><span data-stu-id="8aa86-157">a.</span></span> <span data-ttu-id="8aa86-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="8aa86-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    <span data-ttu-id="8aa86-159">b.</span><span class="sxs-lookup"><span data-stu-id="8aa86-159">b.</span></span> <span data-ttu-id="8aa86-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="8aa86-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8aa86-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="8aa86-161">These values are not real.</span></span> <span data-ttu-id="8aa86-162">Tyto hodnoty aktualizujte skutečná adresa URL přihlašování a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8aa86-162">Update these values with the actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="8aa86-163">Obraťte se na [tým podpory NetDocuments](https://support.netdocuments.com/hc/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="8aa86-163">Contact [NetDocuments support team](https://support.netdocuments.com/hc/) to get these values.</span></span>
 
4. <span data-ttu-id="8aa86-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8aa86-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

5. <span data-ttu-id="8aa86-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8aa86-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8aa86-168">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="8aa86-168">In a different web browser window, log into your NetDocuments company site as an administrator.</span></span>

7. <span data-ttu-id="8aa86-169">Přejděte na **správce**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-169">Go to **Admin**.</span></span>

8. <span data-ttu-id="8aa86-170">Klikněte na tlačítko **přidat a odebrat uživatele a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-170">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="8aa86-171">![Úložiště](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "úložiště")</span><span class="sxs-lookup"><span data-stu-id="8aa86-171">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

9. <span data-ttu-id="8aa86-172">Klikněte na tlačítko **konfigurovat pokročilé možnosti ověřování**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-172">Click **Configure advanced authentication options**.</span></span>
    
    <span data-ttu-id="8aa86-173">![Nakonfigurujte rozšířené možnosti ověřování](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "konfigurovat pokročilé možnosti ověřování")</span><span class="sxs-lookup"><span data-stu-id="8aa86-173">![Configure advanced authentication options](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "Configure advanced authentication options")</span></span>

10. <span data-ttu-id="8aa86-174">Na **federované Identity** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8aa86-174">On the **Federated Identity** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="8aa86-175">![Federované Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "federovaný Identitty")</span><span class="sxs-lookup"><span data-stu-id="8aa86-175">![Federated Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "Federated Identitty")</span></span>
   
    <span data-ttu-id="8aa86-176">a.</span><span class="sxs-lookup"><span data-stu-id="8aa86-176">a.</span></span> <span data-ttu-id="8aa86-177">Jako **federované identity serveru typ**, vyberte **Active Directory Federation Services**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-177">As **Federated identity server type**, select **Active Directory Federation Services**.</span></span>
   
    <span data-ttu-id="8aa86-178">b.</span><span class="sxs-lookup"><span data-stu-id="8aa86-178">b.</span></span> <span data-ttu-id="8aa86-179">Klikněte na tlačítko **zvolte soubor**, nahrát soubor stažený metadata, která jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8aa86-179">Click **Choose file**, to upload the downloaded metadata file which you have downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="8aa86-180">c.</span><span class="sxs-lookup"><span data-stu-id="8aa86-180">c.</span></span> <span data-ttu-id="8aa86-181">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-181">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="8aa86-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="8aa86-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8aa86-183">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="8aa86-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8aa86-184">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8aa86-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8aa86-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8aa86-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="8aa86-186">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8aa86-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8aa86-188">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8aa86-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8aa86-189">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8aa86-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8aa86-191">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8aa86-193">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8aa86-193">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8aa86-195">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8aa86-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8aa86-197">a.</span><span class="sxs-lookup"><span data-stu-id="8aa86-197">a.</span></span> <span data-ttu-id="8aa86-198">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8aa86-199">b.</span><span class="sxs-lookup"><span data-stu-id="8aa86-199">b.</span></span> <span data-ttu-id="8aa86-200">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8aa86-200">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8aa86-201">c.</span><span class="sxs-lookup"><span data-stu-id="8aa86-201">c.</span></span> <span data-ttu-id="8aa86-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8aa86-203">d.</span><span class="sxs-lookup"><span data-stu-id="8aa86-203">d.</span></span> <span data-ttu-id="8aa86-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-204">Click **Create**.</span></span>
 
### <a name="creating-a-netdocuments-test-user"></a><span data-ttu-id="8aa86-205">Vytvoření zkušebního uživatele NetDocuments</span><span class="sxs-lookup"><span data-stu-id="8aa86-205">Creating a NetDocuments test user</span></span>

<span data-ttu-id="8aa86-206">Pokud chcete povolit uživatelům Azure AD přihlášení k NetDocuments, musí být zřízená do NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="8aa86-206">To enable Azure AD users to log in to NetDocuments, they must be provisioned into NetDocuments.</span></span>  
<span data-ttu-id="8aa86-207">V případě NetDocuments zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="8aa86-207">In the case of NetDocuments, provisioning is a manual task.</span></span>

<span data-ttu-id="8aa86-208">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8aa86-208">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="8aa86-209">SING na vaše **NetDocuments** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="8aa86-209">Sing on to your **NetDocuments** company site as administrator.</span></span>

2. <span data-ttu-id="8aa86-210">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-210">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="8aa86-211">![Správce](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "správce")</span><span class="sxs-lookup"><span data-stu-id="8aa86-211">![Admin](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "Admin")</span></span>

3. <span data-ttu-id="8aa86-212">Klikněte na tlačítko **přidat a odebrat uživatele a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-212">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="8aa86-213">![Úložiště](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "úložiště")</span><span class="sxs-lookup"><span data-stu-id="8aa86-213">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

4. <span data-ttu-id="8aa86-214">V **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu platný účet služby Azure Active Directory určené ke zřízení a potom klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-214">In the **Email Address** textbox, type the email address of a valid Azure Active Directory account you want to provision, and then click **Add User**.</span></span>
   
    <span data-ttu-id="8aa86-215">![E-mailová adresa](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "e-mailová adresa")</span><span class="sxs-lookup"><span data-stu-id="8aa86-215">![Email Address](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "Email Address")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="8aa86-216">Držitel účtu Azure Active Directory dostane e-mail, který obsahuje odkaz pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="8aa86-216">The Azure Active Directory account holder will get an email that includes a link to confirm the account before it becomes active.</span></span> <span data-ttu-id="8aa86-217">Můžete použít všechny ostatní NetDocuments uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované NetDocuments zřídit služby Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="8aa86-217">You can use any other NetDocuments user account creation tools or APIs provided by NetDocuments to provision Azure Active Directory user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8aa86-218">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8aa86-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8aa86-219">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="8aa86-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to NetDocuments.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8aa86-221">**Pokud chcete přiřadit Britta Simon NetDocuments, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8aa86-221">**To assign Britta Simon to NetDocuments, perform the following steps:**</span></span>

1. <span data-ttu-id="8aa86-222">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8aa86-224">V seznamu aplikací vyberte **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-224">In the applications list, select **NetDocuments**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_app.png) 

3. <span data-ttu-id="8aa86-226">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8aa86-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8aa86-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8aa86-228">Click **Add** button.</span></span> <span data-ttu-id="8aa86-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8aa86-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8aa86-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8aa86-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8aa86-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8aa86-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8aa86-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8aa86-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8aa86-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8aa86-234">Testing single sign-on</span></span>

<span data-ttu-id="8aa86-235">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8aa86-235">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8aa86-236">Když kliknete na dlaždici NetDocuments na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="8aa86-236">When you click the NetDocuments tile in the Access Panel, you should get automatically signed-on to your NetDocuments application.</span></span>
<span data-ttu-id="8aa86-237">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8aa86-237">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8aa86-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8aa86-238">Additional resources</span></span>

* [<span data-ttu-id="8aa86-239">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8aa86-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8aa86-240">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8aa86-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_203.png

