---
title: "Kurz: Azure Active Directory integrace s centrální Cerner | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Cerner střed."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2bc549d-d286-4679-854e-bb67c62b0475
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 77b5fb94cdfa5722081198aabc59fbf86229c2b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cerner-central"></a><span data-ttu-id="d68c7-103">Kurz: Azure Active Directory integrace s Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="d68c7-103">Tutorial: Azure Active Directory integration with Cerner Central</span></span>

<span data-ttu-id="d68c7-104">V tomto kurzu zjistěte, jak integrovat střed Cerner s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d68c7-104">In this tutorial, you learn how to integrate Cerner Central with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d68c7-105">Integrace střed Cerner s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d68c7-105">Integrating Cerner Central with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d68c7-106">Můžete řídit ve službě Azure AD, který má přístup k Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="d68c7-106">You can control in Azure AD who has access to Cerner Central</span></span>
- <span data-ttu-id="d68c7-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Cerner střed (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d68c7-107">You can enable your users to automatically get signed-on to Cerner Central (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d68c7-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d68c7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d68c7-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d68c7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d68c7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d68c7-110">Prerequisites</span></span>

<span data-ttu-id="d68c7-111">Konfigurace integrace Azure AD s centrální Cerner, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d68c7-111">To configure Azure AD integration with Cerner Central, you need the following items:</span></span>

- <span data-ttu-id="d68c7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d68c7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d68c7-113">Schválené Cerner centrální systému účtu</span><span class="sxs-lookup"><span data-stu-id="d68c7-113">An approved Cerner Central System Account</span></span>

> [!NOTE]
> <span data-ttu-id="d68c7-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d68c7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d68c7-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d68c7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d68c7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d68c7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d68c7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d68c7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d68c7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d68c7-118">Scenario description</span></span>
<span data-ttu-id="d68c7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d68c7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d68c7-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d68c7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d68c7-121">Přidání Cerner střed z Galerie</span><span class="sxs-lookup"><span data-stu-id="d68c7-121">Adding Cerner Central from the gallery</span></span>
2. <span data-ttu-id="d68c7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d68c7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cerner-central-from-the-gallery"></a><span data-ttu-id="d68c7-123">Přidání Cerner střed z Galerie</span><span class="sxs-lookup"><span data-stu-id="d68c7-123">Adding Cerner Central from the gallery</span></span>
<span data-ttu-id="d68c7-124">Při konfiguraci integrace Cerner střední do služby Azure AD, potřebujete přidat Cerner střed z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d68c7-124">To configure the integration of Cerner Central into Azure AD, you need to add Cerner Central from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d68c7-125">**Chcete-li přidat Cerner střed z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d68c7-125">**To add Cerner Central from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d68c7-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d68c7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d68c7-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d68c7-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d68c7-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítka na dialogu.</span><span class="sxs-lookup"><span data-stu-id="d68c7-131">To add new application, click **New application** button on top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d68c7-133">Do vyhledávacího pole zadejte **Cerner střed**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-133">In the search box, type **Cerner Central**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_search.png)

5. <span data-ttu-id="d68c7-135">Na panelu výsledků vyberte **Cerner střed**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d68c7-135">In the results panel, select **Cerner Central**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d68c7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d68c7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d68c7-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s centrální Cerner podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="d68c7-138">In this section, you configure and test Azure AD single sign-on with Cerner Central based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d68c7-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšku in – střed Cerner je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d68c7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cerner Central is to a user in Azure AD.</span></span> <span data-ttu-id="d68c7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské in – střed Cerner musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d68c7-140">In other words, a link relationship between an Azure AD user and the related user in Cerner Central needs to be established.</span></span>

<span data-ttu-id="d68c7-141">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s centrální Cerner, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d68c7-141">To configure and test Azure AD single sign-on with Cerner Central, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d68c7-142">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d68c7-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d68c7-143">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d68c7-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d68c7-144">**[Vytvoření zkušebního uživatele střed Cerner](#creating-a-cerner-central-test-user)**  – Pokud chcete mít protějšek Britta Simon in – střed Cerner propojeném s Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="d68c7-144">**[Creating a Cerner Central test user](#creating-a-cerner-central-test-user)** - to have a counterpart of Britta Simon in Cerner Central that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="d68c7-145">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d68c7-145">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d68c7-146">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d68c7-146">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d68c7-147">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d68c7-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d68c7-148">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Cerner střed.</span><span class="sxs-lookup"><span data-stu-id="d68c7-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cerner Central application.</span></span>

<span data-ttu-id="d68c7-149">**Ke konfiguraci Azure AD jednotné přihlašování s centrální Cerner, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d68c7-149">**To configure Azure AD single sign-on with Cerner Central, perform the following steps:**</span></span>

1. <span data-ttu-id="d68c7-150">Na portálu Azure na **Cerner střed** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-150">In the Azure portal, on the **Cerner Central** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d68c7-152">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d68c7-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_samlbase.png)

3. <span data-ttu-id="d68c7-154">Na **Cerner centrální domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d68c7-154">On the **Cerner Central Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_url.png)

    <span data-ttu-id="d68c7-156">a.</span><span class="sxs-lookup"><span data-stu-id="d68c7-156">a.</span></span> <span data-ttu-id="d68c7-157">V **identifikátor** textovému poli, zadejte hodnotu pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="d68c7-157">In the **Identifier** textbox, type the value using the following patterns:</span></span>
    
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcerner.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |

    <span data-ttu-id="d68c7-158">b.</span><span class="sxs-lookup"><span data-stu-id="d68c7-158">b.</span></span> <span data-ttu-id="d68c7-159">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="d68c7-159">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/sso` |
    | `https://cernercentral.com/<instasncename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://<subdomain>.sandboxcernercentral.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="d68c7-160">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="d68c7-160">These values are not the real.</span></span> <span data-ttu-id="d68c7-161">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d68c7-161">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="d68c7-162">Obraťte se na [tým podpory Cerner střed](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="d68c7-162">Contact [Cerner Central support team](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) to get these values.</span></span>
 
4. <span data-ttu-id="d68c7-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d68c7-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="d68c7-165">Ke generování **Metadata** adresu url, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d68c7-165">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="d68c7-166">a.</span><span class="sxs-lookup"><span data-stu-id="d68c7-166">a.</span></span> <span data-ttu-id="d68c7-167">Klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-167">Click **App registrations**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appregistrations.png)
   
    <span data-ttu-id="d68c7-169">b.</span><span class="sxs-lookup"><span data-stu-id="d68c7-169">b.</span></span> <span data-ttu-id="d68c7-170">Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d68c7-170">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpointicon.png)

    <span data-ttu-id="d68c7-172">c.</span><span class="sxs-lookup"><span data-stu-id="d68c7-172">c.</span></span> <span data-ttu-id="d68c7-173">Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="d68c7-173">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpoint.png)
     
    <span data-ttu-id="d68c7-175">d.</span><span class="sxs-lookup"><span data-stu-id="d68c7-175">d.</span></span> <span data-ttu-id="d68c7-176">Nyní přejděte na stránku vlastností **Cerner střed** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="d68c7-176">Now go to the property page of **Cerner Central** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appid.png)

    <span data-ttu-id="d68c7-178">e.</span><span class="sxs-lookup"><span data-stu-id="d68c7-178">e.</span></span> <span data-ttu-id="d68c7-179">Vygenerovat **adresu URL metadat** pomocí následujícího vzorce:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="d68c7-179">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

6. <span data-ttu-id="d68c7-180">Konfigurace jednotného přihlašování na **Cerner střed** straně, budete muset odeslat **adresu URL metadat** k [Cerner střed podporu](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span><span class="sxs-lookup"><span data-stu-id="d68c7-180">To configure single sign-on on **Cerner Central** side, you need to send the **Metadata URL** to [Cerner Central support](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span></span> <span data-ttu-id="d68c7-181">Na straně aplikace k dokončení integrace je potřeba nakonfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d68c7-181">They configure the SSO on application side to complete the integration.</span></span>

> [!TIP]
> <span data-ttu-id="d68c7-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d68c7-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d68c7-183">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d68c7-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d68c7-184">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d68c7-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d68c7-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d68c7-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="d68c7-186">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d68c7-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span> 

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d68c7-188">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d68c7-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d68c7-189">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d68c7-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d68c7-191">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d68c7-193">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-193">To open the **User** dialog, click **Add**.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d68c7-195">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d68c7-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d68c7-197">a.</span><span class="sxs-lookup"><span data-stu-id="d68c7-197">a.</span></span> <span data-ttu-id="d68c7-198">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d68c7-199">b.</span><span class="sxs-lookup"><span data-stu-id="d68c7-199">b.</span></span> <span data-ttu-id="d68c7-200">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d68c7-200">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="d68c7-201">c.</span><span class="sxs-lookup"><span data-stu-id="d68c7-201">c.</span></span> <span data-ttu-id="d68c7-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d68c7-203">d.</span><span class="sxs-lookup"><span data-stu-id="d68c7-203">d.</span></span> <span data-ttu-id="d68c7-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-204">Click **Create**.</span></span>
 
### <a name="creating-a-cerner-central-test-user"></a><span data-ttu-id="d68c7-205">Vytvoření zkušebního uživatele Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="d68c7-205">Creating a Cerner Central test user</span></span>

<span data-ttu-id="d68c7-206">**Střed Cerner** aplikace umožňuje ověřování z kteréhokoli zprostředkovatele federovaných identit.</span><span class="sxs-lookup"><span data-stu-id="d68c7-206">**Cerner Central** application allows authentication from any federated identity provider.</span></span> <span data-ttu-id="d68c7-207">Pokud je uživatel moct přihlásit k domovské stránce aplikace, jsou federovaný a mít pro jakékoli ruční zřizování není nutné.</span><span class="sxs-lookup"><span data-stu-id="d68c7-207">If a user is able to log in to the application home page, they are federated and have no need for any manual provisioning.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d68c7-208">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d68c7-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d68c7-209">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Cerner střed.</span><span class="sxs-lookup"><span data-stu-id="d68c7-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cerner Central.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d68c7-211">**Pokud chcete přiřadit Britta Simon Cerner – střed, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d68c7-211">**To assign Britta Simon to Cerner Central, perform the following steps:**</span></span>

1. <span data-ttu-id="d68c7-212">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d68c7-214">V seznamu aplikací vyberte **Cerner střed**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-214">In the applications list, select **Cerner Central**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_app.png) 

3. <span data-ttu-id="d68c7-216">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d68c7-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d68c7-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d68c7-218">Click **Add** button.</span></span> <span data-ttu-id="d68c7-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d68c7-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d68c7-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d68c7-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d68c7-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d68c7-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d68c7-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d68c7-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d68c7-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d68c7-224">Testing single sign-on</span></span>

<span data-ttu-id="d68c7-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d68c7-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d68c7-226">Když kliknete na dlaždici Cerner střed na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Cerner střed.</span><span class="sxs-lookup"><span data-stu-id="d68c7-226">When you click the Cerner Central tile in the Access Panel, you should get automatically signed-on to your Cerner Central application.</span></span> <span data-ttu-id="d68c7-227">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d68c7-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d68c7-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d68c7-228">Additional resources</span></span>

* [<span data-ttu-id="d68c7-229">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d68c7-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d68c7-230">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d68c7-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_203.png

