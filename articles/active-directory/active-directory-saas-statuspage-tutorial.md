---
title: "Kurz: Azure Active Directory integrace s stránka se stavem | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a stránka se stavem."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: fa16cdec7b89404c140435fe57d5aa4b08cfa985
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a><span data-ttu-id="e1757-103">Kurz: Azure Active Directory integrace s stránka se stavem</span><span class="sxs-lookup"><span data-stu-id="e1757-103">Tutorial: Azure Active Directory integration with StatusPage</span></span>

<span data-ttu-id="e1757-104">V tomto kurzu zjistěte, jak integrovat stránka se stavem s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e1757-104">In this tutorial, you learn how to integrate StatusPage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e1757-105">Stránka se stavem integraci s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e1757-105">Integrating StatusPage with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e1757-106">Můžete řídit ve službě Azure AD, který má přístup k stránka se stavem</span><span class="sxs-lookup"><span data-stu-id="e1757-106">You can control in Azure AD who has access to StatusPage</span></span>
- <span data-ttu-id="e1757-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k stránka se stavem (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1757-107">You can enable your users to automatically get signed-on to StatusPage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e1757-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e1757-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e1757-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e1757-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1757-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e1757-110">Prerequisites</span></span>

<span data-ttu-id="e1757-111">Konfigurace integrace Azure AD s stránka se stavem, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e1757-111">To configure Azure AD integration with StatusPage, you need the following items:</span></span>

- <span data-ttu-id="e1757-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1757-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e1757-113">Stránka se stavem jednoho přihlášení povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e1757-113">A StatusPage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e1757-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e1757-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e1757-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e1757-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e1757-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e1757-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e1757-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e1757-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e1757-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e1757-118">Scenario description</span></span>
<span data-ttu-id="e1757-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e1757-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e1757-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e1757-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e1757-121">Přidání stránka se stavem z Galerie</span><span class="sxs-lookup"><span data-stu-id="e1757-121">Adding StatusPage from the gallery</span></span>
2. <span data-ttu-id="e1757-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e1757-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-statuspage-from-the-gallery"></a><span data-ttu-id="e1757-123">Přidání stránka se stavem z Galerie</span><span class="sxs-lookup"><span data-stu-id="e1757-123">Adding StatusPage from the gallery</span></span>
<span data-ttu-id="e1757-124">Při konfiguraci integrace stránka se stavem do služby Azure AD potřebujete přidat stránka se stavem z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e1757-124">To configure the integration of StatusPage into Azure AD, you need to add StatusPage from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e1757-125">**Pokud chcete přidat stránka se stavem z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e1757-125">**To add StatusPage from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e1757-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e1757-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e1757-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e1757-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e1757-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e1757-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e1757-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e1757-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e1757-133">Do vyhledávacího pole zadejte **stránka se stavem**.</span><span class="sxs-lookup"><span data-stu-id="e1757-133">In the search box, type **StatusPage**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_search.png)

5. <span data-ttu-id="e1757-135">Na panelu výsledků vyberte **stránka se stavem**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1757-135">In the results panel, select **StatusPage**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e1757-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e1757-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e1757-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s stránka se stavem podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e1757-138">In this section, you configure and test Azure AD single sign-on with StatusPage based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e1757-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v stránka se stavem je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e1757-139">For single sign-on to work, Azure AD needs to know what the counterpart user in StatusPage is to a user in Azure AD.</span></span> <span data-ttu-id="e1757-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v stránka se stavem musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e1757-140">In other words, a link relationship between an Azure AD user and the related user in StatusPage needs to be established.</span></span>

<span data-ttu-id="e1757-141">V stránka se stavem, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="e1757-141">In StatusPage, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e1757-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s stránka se stavem, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e1757-142">To configure and test Azure AD single sign-on with StatusPage, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e1757-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e1757-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e1757-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e1757-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e1757-145">**[Vytvoření zkušebního uživatele stránka se stavem](#creating-a-statuspage-test-user)**  – Pokud chcete mít protějšek Britta Simon v stránka se stavem, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e1757-145">**[Creating a StatusPage test user](#creating-a-statuspage-test-user)** - to have a counterpart of Britta Simon in StatusPage that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e1757-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e1757-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e1757-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e1757-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e1757-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e1757-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e1757-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci stránka se stavem.</span><span class="sxs-lookup"><span data-stu-id="e1757-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your StatusPage application.</span></span>

<span data-ttu-id="e1757-150">**Ke konfiguraci Azure AD jednotné přihlašování s stránka se stavem, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e1757-150">**To configure Azure AD single sign-on with StatusPage, perform the following steps:**</span></span>

1. <span data-ttu-id="e1757-151">Na portálu Azure na **stránka se stavem** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e1757-151">In the Azure portal, on the **StatusPage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e1757-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e1757-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_samlbase.png)

3. <span data-ttu-id="e1757-155">Na **stránka se stavem domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e1757-155">On the **StatusPage Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_url.png)

    <span data-ttu-id="e1757-157">a.</span><span class="sxs-lookup"><span data-stu-id="e1757-157">a.</span></span> <span data-ttu-id="e1757-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="e1757-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    <span data-ttu-id="e1757-159">b.</span><span class="sxs-lookup"><span data-stu-id="e1757-159">b.</span></span> <span data-ttu-id="e1757-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="e1757-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span> 
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

    > [!NOTE]
    > <span data-ttu-id="e1757-161">Obraťte se na tým podpory stránka se stavem v [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)požádat o metadat nutné nakonfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e1757-161">Contact the StatusPage support team at [SupportTeam@statuspage.io](mailto:SupportTeam@statuspage.io)to request metadata necessary to configure single sign-on.</span></span> 
    >
    ><span data-ttu-id="e1757-162">a.</span><span class="sxs-lookup"><span data-stu-id="e1757-162">a.</span></span> <span data-ttu-id="e1757-163">Z metadat, zkopírujte hodnotu vystavitele a vložte ji do **identifikátor** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e1757-163">From the metadata, copy the Issuer value, and then paste it into the **Identifier** textbox.</span></span>
    >
    ><span data-ttu-id="e1757-164">b.</span><span class="sxs-lookup"><span data-stu-id="e1757-164">b.</span></span> <span data-ttu-id="e1757-165">Z metadat, adresa URL odpovědi zkopírujte a vložte ji do **adresa URL odpovědi** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e1757-165">From the metadata, copy the Reply URL, and then paste it into the **Reply URL** textbox.</span></span>

4. <span data-ttu-id="e1757-166">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="e1757-166">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_certificate.png) 

5. <span data-ttu-id="e1757-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e1757-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e1757-170">Na **stránka se stavem konfigurace** klikněte na tlačítko **konfigurace stránka se stavem** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e1757-170">On the **StatusPage Configuration** section, click **Configure StatusPage** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e1757-171">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e1757-171">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_configure.png) 

7. <span data-ttu-id="e1757-173">V jiném okně prohlížeče Přihlaste se k serveru vaší společnosti stránka se stavem jako správce.</span><span class="sxs-lookup"><span data-stu-id="e1757-173">In another browser window, sign on to your StatusPage company site as an administrator.</span></span>

8. <span data-ttu-id="e1757-174">Na hlavním panelu nástrojů klikněte na tlačítko **spravovat účet**.</span><span class="sxs-lookup"><span data-stu-id="e1757-174">In the main toolbar, click **Manage Account**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png) 

10. <span data-ttu-id="e1757-176">Klikněte **jednotné přihlašování** kartě.</span><span class="sxs-lookup"><span data-stu-id="e1757-176">Click the **Single Sign-on** tab.</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png) 

11. <span data-ttu-id="e1757-178">Na stránce nastavení jednotného přihlašování proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e1757-178">On the SSO Setup page, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_09.png) 
 
    <span data-ttu-id="e1757-181">a.</span><span class="sxs-lookup"><span data-stu-id="e1757-181">a.</span></span> <span data-ttu-id="e1757-182">V **jednotné přihlašování cílová adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e1757-182">In the **SSO Target URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="e1757-183">b.</span><span class="sxs-lookup"><span data-stu-id="e1757-183">b.</span></span> <span data-ttu-id="e1757-184">V poznámkovém bloku otevřete stažený certifikát, kopírovat obsah a vložte ji do **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e1757-184">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **Certificate** textbox.</span></span> 

    <span data-ttu-id="e1757-185">c.</span><span class="sxs-lookup"><span data-stu-id="e1757-185">c.</span></span> <span data-ttu-id="e1757-186">Klikněte na tlačítko **uložení konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="e1757-186">Click **SAVE CONFIGURATION**.</span></span>

> [!TIP]
> <span data-ttu-id="e1757-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e1757-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e1757-188">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e1757-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e1757-189">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e1757-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e1757-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1757-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="e1757-191">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e1757-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e1757-193">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e1757-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e1757-194">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e1757-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e1757-196">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e1757-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e1757-198">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e1757-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e1757-200">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e1757-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e1757-202">a.</span><span class="sxs-lookup"><span data-stu-id="e1757-202">a.</span></span> <span data-ttu-id="e1757-203">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e1757-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e1757-204">b.</span><span class="sxs-lookup"><span data-stu-id="e1757-204">b.</span></span> <span data-ttu-id="e1757-205">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e1757-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e1757-206">c.</span><span class="sxs-lookup"><span data-stu-id="e1757-206">c.</span></span> <span data-ttu-id="e1757-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e1757-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e1757-208">d.</span><span class="sxs-lookup"><span data-stu-id="e1757-208">d.</span></span> <span data-ttu-id="e1757-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e1757-209">Click **Create**.</span></span>
 
### <a name="creating-a-statuspage-test-user"></a><span data-ttu-id="e1757-210">Vytvoření zkušebního uživatele stránka se stavem</span><span class="sxs-lookup"><span data-stu-id="e1757-210">Creating a StatusPage test user</span></span>

<span data-ttu-id="e1757-211">Cílem této části je vytvoření uživatele volal Britta Simon v stránka se stavem.</span><span class="sxs-lookup"><span data-stu-id="e1757-211">The objective of this section is to create a user called Britta Simon in StatusPage.</span></span>

<span data-ttu-id="e1757-212">Stránka se stavem podporuje zřizování za běhu.</span><span class="sxs-lookup"><span data-stu-id="e1757-212">StatusPage supports just-in-time provisioning.</span></span> <span data-ttu-id="e1757-213">Již je v povolíte [konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="e1757-213">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="e1757-214">**Pokud chcete vytvořit uživateli volat Britta Simon v stránka se stavem, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e1757-214">**To create a user called Britta Simon in StatusPage, perform the following steps:**</span></span>

1. <span data-ttu-id="e1757-215">Přihlašování k webu společnosti stránka se stavem jako správce.</span><span class="sxs-lookup"><span data-stu-id="e1757-215">Sign-on to your StatusPage company site as an administrator.</span></span>

2. <span data-ttu-id="e1757-216">V nabídce v horní části, klikněte na tlačítko **spravovat účet**.</span><span class="sxs-lookup"><span data-stu-id="e1757-216">In the menu on the top, click **Manage Account**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png)

3. <span data-ttu-id="e1757-218">Klikněte **členové týmu** kartě.</span><span class="sxs-lookup"><span data-stu-id="e1757-218">Click the **Team Members** tab.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png) 

4. <span data-ttu-id="e1757-220">Klikněte na tlačítko **člen týmu přidat**.</span><span class="sxs-lookup"><span data-stu-id="e1757-220">Click **ADD TEAM MEMBER**.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png) 

5. <span data-ttu-id="e1757-222">Typ **e-mailovou adresu**, **křestní jméno**, a **Sur název** platný uživatele, který chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="e1757-222">Type the **Email Address**, **First Name**, and **Sur Name** of a valid user you want to provision into the related textboxes.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png) 

6. <span data-ttu-id="e1757-224">Jako **Role**, zvolte **správce klienta**.</span><span class="sxs-lookup"><span data-stu-id="e1757-224">As **Role**, choose **Client Administrator**.</span></span>

7. <span data-ttu-id="e1757-225">Klikněte na tlačítko **vytvořit účet**.</span><span class="sxs-lookup"><span data-stu-id="e1757-225">Click **CREATE ACCOUNT**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e1757-226">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1757-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e1757-227">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu stránka se stavem.</span><span class="sxs-lookup"><span data-stu-id="e1757-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to StatusPage.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e1757-229">**Pokud chcete přiřadit Britta Simon stránka se stavem, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e1757-229">**To assign Britta Simon to StatusPage, perform the following steps:**</span></span>

1. <span data-ttu-id="e1757-230">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e1757-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e1757-232">V seznamu aplikací vyberte **stránka se stavem**.</span><span class="sxs-lookup"><span data-stu-id="e1757-232">In the applications list, select **StatusPage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_app.png) 

3. <span data-ttu-id="e1757-234">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e1757-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e1757-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e1757-236">Click **Add** button.</span></span> <span data-ttu-id="e1757-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e1757-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e1757-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e1757-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e1757-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e1757-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e1757-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e1757-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e1757-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e1757-242">Testing single sign-on</span></span>

<span data-ttu-id="e1757-243">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e1757-243">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e1757-244">Když kliknete na dlaždici stránka se stavem na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci stránka se stavem.</span><span class="sxs-lookup"><span data-stu-id="e1757-244">When you click the StatusPage tile in the Access Panel, you should get automatically signed-on to your StatusPage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1757-245">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e1757-245">Additional resources</span></span>

* [<span data-ttu-id="e1757-246">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e1757-246">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e1757-247">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e1757-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png

